@Library('shared-lib-mongo-connector') _

pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
        SHARED_LIB_PATH = 'K:\\1.Rushi technologies - 6AM - May 2025 batch\\shared library\\shared-lib-mongo-connector'
    }

    stages {
        stage('Verify Environment') {
            steps {
                bat 'java -version'
                bat 'groovy --version'
            }
        }

        stage('Run Mongo Connector') {
            steps {
                bat """
                groovy -cp \\"%SHARED_LIB_PATH%\\lib\\mongodb-driver-sync-5.5.0.jar;%SHARED_LIB_PATH%\\scripts\\" \\"%SHARED_LIB_PATH%\\scripts\\mongo-connector.groovy\\"
                """
            }
        }
    }
}
