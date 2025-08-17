@Library('shared-lib-mongo-connector@master') _

pipeline {
    agent any
    stages {
        stage('MongoDB Example') {
            steps {
                script {
                    def client = mongoConnector.connect()
                    def db = mongoOperations.createDatabase(client, "krish-db")
                    mongoOperations.createCollection(db, "myCollection")
                    mongoOperations.insertDocument(db, "myCollection", [name: "Krishnakumarchinnusamy", role: "Admin"])
                    client.close()
                }
            }
        }
    }
}
