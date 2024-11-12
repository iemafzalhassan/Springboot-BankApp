pipeline {
    agent {label "slave-2"};
    
    environment {
        // Define a variable for branch-specific settings (optional)
        BRANCH_NAME = "${env.BRANCH_NAME}"
    }
    
    stages {
        stage('Code Clone') {
            steps {
                echo "Code Clone Stage for branch: ${BRANCH_NAME}"
                git url: "https://github.com/iemafzalhassan/Springboot-BankApp.git", branch: "${BRANCH_NAME}"
            }
        }

        stage('Code Build & Test') {
            steps {
                echo "Code Build Stage for branch: ${BRANCH_NAME}"

                // Adjust build command based on the branch
                script {
                    if (BRANCH_NAME == 'prod') {
                        // For prod, maybe skip tests or adjust build args
                        sh 'docker build -t bankapp:prod --build-arg environment=prod .'
                    } else if (BRANCH_NAME == 'DevOps') {
                        // For DevOps, build with DevOps-specific options
                        sh 'docker build -t bankapp:devops --build-arg environment=devops .'
                    } else {
                        // Default build (e.g., for dev branch or any other branch)
                        sh 'docker build -t bankapp .'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploy Stage for branch: ${BRANCH_NAME}"

                script {
                    if (BRANCH_NAME == 'prod') {
                        // For prod, deploy with extra steps or use a different command
                        sh 'docker-compose -f docker-compose.prod.yml down && docker-compose -f docker-compose.prod.yml up -d --build'
                    } else {
                        // Default deployment logic for other branches
                        sh 'docker-compose down && docker-compose up -d --build'
                    }
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline for branch ${BRANCH_NAME} completed successfully!"
        }
        failure {
            echo "Pipeline for branch ${BRANCH_NAME} failed!"
        }
    }
}
