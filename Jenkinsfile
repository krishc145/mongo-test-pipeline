@Library('shared-lib-mongo-connector@master') _

pipeline {
    agent any
    stages {
        stage('MongoDB Example') {
            steps {
                script {
                    def client = mongoConnector.connect()
                    def db = mongoOperations.createDatabase(client, "testdb")
                    mongoOperations.createCollection(db, "myCollection")
                    mongoOperations.insertDocument(db, "myCollection", [name: "Krish", role: "Admin"])
                    client.close()
                }
            }
        }
    }
}
