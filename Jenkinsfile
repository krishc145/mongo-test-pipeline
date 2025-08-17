@Library('shared-lib-mongo-connector@master') _

import com.mongodb.client.MongoClients

pipeline {
    agent any
    stages {
        stage('MongoDB Example') {
            steps {
                script {
                    def client = MongoClients.create("mongodb://localhost:27017")
                    def db = mongoOperations.createDatabase(client, "testdb")
                    mongoOperations.createCollection(db, "myCollection")
                    mongoOperations.insertDocument(db, "myCollection", [name: "Krish", role: "Admin"])
                    client.close()
                }
            }
        }
    }
}
