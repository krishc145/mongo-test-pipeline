@Library('shared-lib-mongo-connector') _

pipeline {
    agent any

    environment {
        JAVA_HOME = 'C:\\Program Files\\Java\\jdk-17'
        PATH = "${env.JAVA_HOME}\\bin;${env.PATH}"
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
                mongoConnector()
            }
        }
    }
}
