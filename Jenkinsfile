pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'master',
                    credentialsId: 'git-cred',
                    url: 'https://github.com/<YOUR_USERNAME>/<YOUR_REPO>.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('File System Scan') {
            steps {
                sh 'trivy fs --format table -o trivy-fs-report.html .'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=BoardGame \
                        -Dsonar.projectKey=BoardGame \
                        -Dsonar.java.binaries=.
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                waitForQualityGate abortPipeline: true, credentialsId: 'sonar-token'
            }
        }

        stage('Publish to Nexus') {
            steps {
                withMaven(
                    globalMavenSettingsConfig: 'global-settings',
                    jdk: 'jdk17',
                    maven: 'maven3',
                    mavenSettingsConfig: '',
                    traceability: true
                ) {
                    sh "mvn deploy"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker build -t <DOCKERHUB_USERNAME>/<IMAGE_NAME>:<TAG> ."
                    }
                }
            }
        }

        stage('Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-scan-image.html <DOCKERHUB_USERNAME>/<IMAGE_NAME>:<TAG>"
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-cred') {
                        sh "docker push <DOCKERHUB_USERNAME>/<IMAGE_NAME>:<TAG>"
                    }
                }
            }
        }

        stage('Deploy To Kubernetes') {
            steps {
               withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://<K8S_MASTER_IP>:6443') {
                        sh "kubectl apply -f deployment-service.yaml"
                }
            }
        }

        stage('Verify the Deployment') {
            steps {
               withKubeConfig(caCertificate: '', clusterName: 'kubernetes', contextName: '', credentialsId: 'k8-cred', namespace: 'webapps', restrictKubeConfigAccess: false, serverUrl: 'https://<K8S_MASTER_IP>:6443') {
                        sh "kubectl get pods -n webapps"
                        sh "kubectl get svc -n webapps"
                }
            }
        }

        stage('Send Email') {
            steps {
                script {
                    def body = "Build ${env.JOB_NAME} #${env.BUILD_NUMBER} finished with status ${currentBuild.currentResult}"
                    emailext(
                        subject: "${env.JOB_NAME} - Build ${env.BUILD_NUMBER} - ${currentBuild.currentResult}",
                        body: body,
                        to: '<YOUR_EMAIL>',
                        from: 'jenkins@example.com',
                        replyTo: 'jenkins@example.com',
                        mimeType: 'text/html',
                        attachmentsPattern: 'trivy-scan-image.html'
                    )
                }
            }
        }

    }
}
