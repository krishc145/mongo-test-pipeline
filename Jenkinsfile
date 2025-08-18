@Library('shared-lib-mongo-connector') _

pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${JAVA_HOME}\\bin;${env.PATH}"
    }

    stages {
        stage('Verify Java Setup') {
            steps {
                bat 'echo JAVA_HOME is %JAVA_HOME%'
                bat 'java -version'
            }
        }

        stage('Run Mongo Setup') {
            steps {
                script {
                    mongoConnector()
                }
            }
        }
    }

    post {
        always {
            echo "Pipeline completed with status: ${currentBuild.result ?: 'SUCCESS'}"
        }
    }
}
