pipeline {
    agent none // No global agent; dynamically assign agents per stage

    environment {
        DEV_AGENT = 'dev'
        QA_AGENT = 'qa'
    }

    options {
        buildDiscarder logRotator(
            daysToKeepStr: '16',
            numToKeepStr: '10'
        )
    }

    stages {
        stage('Checkout Code') {
            when {
                anyOf {
                    allOf {
                        branch 'develop'
                        expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                    }
                    allOf {
                        branch 'QA'
                        expression { return env.BRANCH_NAME == 'QA' && env.AGENT_LABEL == env.QA_AGENT }
                    }
                }
            }
            agent { label env.AGENT_LABEL }
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: "*/${env.BRANCH_NAME}"]],
                    userRemoteConfigs: [[url: 'https://github.com/Abdulqadir579/jenkinsfile.git']]
                ])
            }
        }

        stage('Maven Build') {
            when {
                anyOf {
                    allOf {
                        branch 'develop'
                        expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                    }
                    allOf {
                        branch 'QA'
                        expression { return env.BRANCH_NAME == 'QA' && env.AGENT_LABEL == env.QA_AGENT }
                    }
                }
            }
            agent { label env.AGENT_LABEL }
            steps {
                sh """
                echo "Building the package in process....."
                """
                sh """
                echo "Building the package is completed....."
                """
            }
        }

        stage('Run Unit Tests') {
            when {
                anyOf {
                    allOf {
                        branch 'develop'
                        expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                    }
                    allOf {
                        branch 'QA'
                        expression { return env.BRANCH_NAME == 'QA' && env.AGENT_LABEL == env.QA_AGENT }
                    }
                }
            }
            agent { label env.AGENT_LABEL }
            steps {
                sh """
                echo "Running unit test....."
                """
                sh """
                echo "Running unit test completed....."
                """
            }
        }

        stage('SonarQube Analysis') {
            when {
                allOf {
                    branch 'develop'
                    expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                }
            }
            agent { label env.DEV_AGENT }
            steps {
                sh """
                echo "SonarQube analysis is in process....."
                """
                sh """
                echo "SonarQube analysis has been completed....."
                """
            }
        }

        stage('Trivy Scan') {
            when {
                allOf {
                    branch 'develop'
                    expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                }
            }
            agent { label env.DEV_AGENT }
            steps {
                sh """
                echo "Trivy file scan is in process....."
                """
                sh """
                echo "Trivy file scan has been completed....."
                """
            }
        }

        stage('Deploy to Server') {
            when {
                anyOf {
                    allOf {
                        branch 'develop'
                        expression { return env.BRANCH_NAME == 'develop' && env.AGENT_LABEL == env.DEV_AGENT }
                    }
                    allOf {
                        branch 'QA'
                        expression { return env.BRANCH_NAME == 'QA' && env.AGENT_LABEL == env.QA_AGENT }
                    }
                }
            }
            agent { label env.AGENT_LABEL }
            steps {
                sh """
                echo "Deploying to ${env.AGENT_LABEL.capitalize()} Server"
                """
                sh """
                echo "Deploying in ${env.AGENT_LABEL.capitalize()} environment successful! CONGRATULATIONS!!!....."
                """
            }
        }

    }

    environment {
        AGENT_LABEL = (env.BRANCH_NAME == 'develop') ? env.DEV_AGENT :
                      (env.BRANCH_NAME == 'QA') ? env.QA_AGENT : 'default'
    }
}

