pipeline {

    agent {
        node {
            label 'master'
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        
        stage('Code Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    userRemoteConfigs: [[url: 'https://github.com/spring-projects/spring-petclinic.git']]
                ])
            }
        }

        stage(' Unit Testing') {
            steps {
                sh """
                echo "Running Unit Tests"
                """
            }
        }

        stage('Code Analysis') {
            steps {
                sh """
                echo "Running Code Analysis"
                """
            }
        }

        stage('Build Deploy Code') {
            when {
                branch 'develop'
            }
            steps {
                sh """
                echo "Building Artifact"
                """

                sh """
                echo "Deploying Code"
                """
            }
             post {
                    success {
                      sendStatus("Build Deploy Code","success")
                    }
                  }
        }

    }   
}

void sendStatus(String stage, String status) {
    container('curl') {
        withCredentials([string(credentialsId: 'TOKEN-CREDS-NAME', variable: 'TOKEN')]) {
            sh "curl -u USER-NAME:$TOKEN -X POST 'https://api.github.com/repos/kunchalavikram1427/spring-petclinic/statuses/$BRANCH_NAME' -H 'Accept: application/vnd.github.v3+json' -d '{\"state\": \"$status\",\"context\": \"$stage\", \"description\": \"Jenkins\", \"target_url\": \"$JENKINS_URL/job/$JOB_NAME/$BUILD_NUMBER/console\"}' "
        }
    }
}

