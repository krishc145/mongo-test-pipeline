@Library('shared-lib-mongo-connector') _

pipeline {
    agent any
    stages {
        stage('MongoDB Example') {
            steps {
                script {
                    mongoConnector.connect("mongodb://localhost:27017")
                    mongoOperations.createDB("testdb")
                }
            }
        }
    }
}
