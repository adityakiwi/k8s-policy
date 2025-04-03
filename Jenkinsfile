pipeline {
    agent any

    environment {
        IMAGE_NAME = 'adidix/jenkins-agent-k8s:arm64'
        MANIFEST = 'manifests/deployment.yaml'
        POLICY = 'manifests/kyverno-policy.yaml'
    }

    stages {
        stage('Trivy Scan') {
            steps {
                script {
                    def scanResult = sh(script: "trivy image --exit-code 1 --severity CRITICAL,HIGH $IMAGE_NAME", returnStatus: true)
                    if (scanResult != 0) {
                        error("Trivy scan failed: High or Critical vulnerabilities found in $IMAGE_NAME")
                    }
                }
            }
        }

        stage('Validate Manifest with Kyverno') {
            steps {
                script {
                    def validateResult = sh(script: "kyverno apply $POLICY --resource $MANIFEST --namespace default", returnStatus: true)
                    if (validateResult != 0) {
                        error("Kyverno policy check failed: Deployment does not comply with policies.")
                    }
                }
            }
        }

        stage('Apply Kubernetes Manifest') {
            steps {
                script {
                    sh "kubectl apply -f $MANIFEST"
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed: Vulnerability or policy violation detected."
        }
        success {
            echo "Deployment successful: Passed security and policy validation."
        }
    }
}
