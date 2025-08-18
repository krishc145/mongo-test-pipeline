@Library('shared-lib-mongo-connector') _

pipeline {
    agent any

    stages {
        stage('Run Mongo Connector') {
            steps {
                script {
                    mongoConnector()
                }
            }
        }
    }
}
