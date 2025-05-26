pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        IMAGE_NAME = 'jayasurya0199/staragilehcarev1'
        REPO_URL = 'https://github.com/Jayasurya0199/star-agile-health-care.git'
        KUBECONFIG_CREDENTIALS = 'k8configpwd'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'master', url: env.REPO_URL
            }
        }

        stage('Build Java Application') {
            steps {
                script {
                    sh "mvn clean package -DskipTests"
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    sh "mvn test"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build(env.IMAGE_NAME)
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                script {
                    docker.withRegistry('', env.DOCKERHUB_CREDENTIALS) {
                        docker.image(env.IMAGE_NAME).push("latest")
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    kubernetesDeploy(
                        configs: 'kubernetesfile.yml',
                        kubeconfigId: env.KUBECONFIG_CREDENTIALS
                    )
                }
            }
        }
    }
}
