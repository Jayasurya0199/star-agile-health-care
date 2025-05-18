pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds'
        IMAGE_NAME = 'jayasurya0199/staragilehcarev1'
        REPO_URL = 'https://github.com/Jayasurya0199/star-agile-health-care.git'
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

        stage('Run Tests') {  // Add a step for running tests
            steps {
                script {
                    sh "mvn test"  // Run the tests using Maven
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

        stage('Configure and Run Docker Locally'){
        when{ expression {env.GIT_BRANCH == 'master'}}
            steps{
                script{
                     kubernetesDeploy (configs: 'kubernetesfile.yaml' ,kubeconfigId: 'k8sconfigpwd')
    
                }
            }
        }
    }
}
