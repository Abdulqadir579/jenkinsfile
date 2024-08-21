pipeline {

    agent {
        node {
            label 'dev'
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '15', 
                    numToKeepStr: '10'
            )
    }

    environment {
        APP_NAME = "Jenkins_file"
        APP_ENV  = "dev"
    }

    stages {
        
        stage('Cleanup Workspace') {
            steps {
                cleanWs()
                sh """
                echo "Cleaned Up Workspace for ${APP_NAME}"
                """
            }
        }

        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/Abdulqadir579/jenkinsfile.git']]
                ])
            }
        }

        stage('Environment Analysis') {

            parallel {

                stage('Priting All Global Variables') {
                    steps {
                        sh """
                        env
                        """
                    }
                }

                stage('Execute Shell') {
                    steps {
                        sh 'echo "Hello this is SWAPSPACE testing"'
                    }
                }

                stage('Print ENV variable') {
                    steps {
                        sh "echo ${APP_ENV}"
                    }
 
              }

            
            }
        }

    }   
}
