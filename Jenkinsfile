pipeline {
  agent {
    kubernetes {
      inheritFrom 'jenkins-docker'
      defaultContainer 'jnlp'
    }
  }

  environment {
    IMAGE_NAME = 'bharatdasa/hello-world'
    TAG = "${BUILD_NUMBER}"
    NAMESPACE = "project1"
    PROM_URL = "http://monitoring-kube-prometheus-prometheus.monitoring.svc:9090"
    ANSIBLE_HOST_KEY_CHECKING = "False"
  }

  stages {

    // =========================
    stage('Checkout') {
      steps {
        git credentialsId: 'github-ssh',
            url: 'git@github.com:BharatDasa/devops-muti-env.git'
      }
    }

    // =========================
    stage('Build') {
      steps {
        container('maven') {
          sh 'mvn clean package -DskipTests'
        }
      }
    }

    // =========================
    stage('Docker Build & Push') {
      steps {
        container('docker') {
          withCredentials([usernamePassword(
            credentialsId: 'Dockerhub',
            usernameVariable: 'DOCKER_USER',
            passwordVariable: 'DOCKER_PASS'
          )]) {
            sh '''
            docker build -t $IMAGE_NAME:$TAG .
            echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
            docker push $IMAGE_NAME:$TAG
            '''
          }
        }
      }
    }

    // =========================
    stage('Ansible Auto-Fix Infrastructure') {
      steps {
        container('ansible') {
          withCredentials([
            sshUserPrivateKey(
              credentialsId: 'ansible-ssh-key',
              keyFileVariable: 'SSH_KEY'
            )
          ]) {
            sh '''
            chmod 600 $SSH_KEY
            ansible-playbook -i ansible/inventory.ini ansible/auto-fix.yml \
              --private-key=$SSH_KEY -u bharat
            '''
          }
        }
      }
    }

    // =========================
    stage('⚡ Parallel Security Checks') {
      parallel {

        // 🔐 HTML REPORT (FIXED)
        stage('🔐 Trivy HTML Report') {
          steps {
            container('trivy') {
              sh '''
              mkdir -p reports
              mkdir -p /tmp/trivy-cache-html

              # ✅ FIX: download template (important)
              wget -q https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl -O html.tpl

              trivy image \
                --cache-dir /tmp/trivy-cache-html \
                --timeout 15m \
                --scanners vuln \
                --format template \
                --template "@html.tpl" \
                -o reports/trivy-report.html \
                docker.io/$IMAGE_NAME:$TAG || true

              # Debug (optional)
              ls -lh reports/
              '''
            }
          }
        }

        // 🔐 SECURITY GATE
        stage('🔐 Trivy Critical Gate') {
          steps {
            container('trivy') {
              sh '''
              mkdir -p /tmp/trivy-cache-gate

              trivy image \
                --cache-dir /tmp/trivy-cache-gate \
                --timeout 15m \
                --scanners vuln \
                --severity CRITICAL \
                --ignore-unfixed \
                --exit-code 1 \
                docker.io/$IMAGE_NAME:$TAG
              '''
            }
          }
        }

      }
    }

    // =========================
    stage('Ansible Pre-Deployment Validation') {
      steps {
        container('ansible') {
          withCredentials([
            sshUserPrivateKey(
              credentialsId: 'ansible-ssh-key',
              keyFileVariable: 'SSH_KEY'
            )
          ]) {
            sh '''
            chmod 600 $SSH_KEY
            ansible-playbook -i ansible/inventory.ini ansible/precheck.yml \
              --private-key=$SSH_KEY -u bharat
            '''
          }
        }
      }
    }

    // =========================
    stage('📊 Publish Trivy Report') {
      steps {
        publishHTML([
          reportDir: 'reports',
          reportFiles: 'trivy-report.html',
          reportName: 'Trivy Security Report',
          keepAll: true,
          alwaysLinkToLastBuild: true,
          allowMissing: true
        ])
      }
    }

    // =========================
    stage('Deploy') {
      steps {
        container('kubectl') {
          sh '''
          apk add --no-cache curl

          curl -LO "https://storage.googleapis.com/kubernetes-release/release/v1.29.0/bin/linux/amd64/kubectl"
          chmod +x kubectl
          mv kubectl /usr/local/bin/

          kubectl apply -f k8s/namespace.yaml

          sed -i "s|IMAGE_PLACEHOLDER|$IMAGE_NAME:$TAG|g" k8s/deployment.yaml

          kubectl apply -f k8s/deployment.yaml
          kubectl apply -f k8s/service.yaml
          kubectl apply -f k8s/ingress.yaml

          kubectl rollout status deployment/hello-app -n $NAMESPACE --timeout=180s
          '''
        }
      }
    }

    // =========================
    stage('Deploy HPA') {
      steps {
        container('kubectl') {
          sh '''
          kubectl apply -f k8s/hpa.yaml
          kubectl get hpa -n $NAMESPACE
          '''
        }
      }
    }

    // =========================
    stage('Deploy Monitoring') {
      steps {
        container('kubectl') {
          sh '''
          kubectl apply -f k8s/servicemonitor.yaml
          '''
        }
      }
    }

    // =========================
    stage('Verify Monitoring') {
      steps {
        sh '''
        RESPONSE=$(curl -s ${PROM_URL}/api/v1/query?query=up)

        if echo "$RESPONSE" | grep -q '"status":"success"'; then
          echo "✅ Prometheus reachable"
        else
          echo "❌ Prometheus failed"
          exit 1
        fi
        '''
      }
    }

    // =========================
    stage('Health Check & Rollback') {
      steps {
        container('kubectl') {
          sh '''
          STATUS=$(kubectl get pods -n $NAMESPACE -l app=hello-app \
            -o jsonpath='{.items[*].status.phase}')

          if echo "$STATUS" | grep -q "Failed"; then
            kubectl rollout undo deployment/hello-app -n $NAMESPACE
            exit 1
          fi

          CRASH=$(kubectl get pods -n $NAMESPACE | grep CrashLoopBackOff || true)

          if [ ! -z "$CRASH" ]; then
            kubectl rollout undo deployment/hello-app -n $NAMESPACE
            exit 1
          fi

          kubectl get hpa -n $NAMESPACE
          echo "✅ Deployment Healthy"
          '''
        }
      }
    }
  }

  post {
    always {
      archiveArtifacts artifacts: 'reports/*.html', allowEmptyArchive: true
    }

    success {
      echo "✅ SUCCESS: ${IMAGE_NAME}:${TAG} deployed with DevSecOps pipeline"
    }

    failure {
      echo "❌ FAILED: Blocked by security or rolled back"
    }
  }
}
