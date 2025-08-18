pipeline {
    agent any

    parameters {
        string(name: 'DB_NAME', defaultValue: 'krishna_DB_admin', description: 'Database name')
        string(name: 'COLLECTION_NAME', defaultValue: 'dba_admin_fruits', description: 'Collection name')
        text(name: 'DATA_TO_INSERT', defaultValue: '[{"name": "mango"},{"name": "apple"},{"name": "jackfruit"}]', description: 'JSON array of documents to insert')
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/krishc145/mongo-test-pipeline', branch: 'main'
            }
        }

        stage('Insert Data into Mongo') {
            steps {
                bat """
                    echo db.${params.COLLECTION_NAME}.insertMany(${params.DATA_TO_INSERT}) > insert.js
                    mongosh -u kichu-admin-group -p kichu --authenticationDatabase admin ${params.DB_NAME} insert.js
                """
            }
        }

        stage('Read Data') {
            steps {
                bat """
                    echo db.${params.COLLECTION_NAME}.find().forEach(function(doc) { printjson(doc); }) > read.js
                    mongosh -u kichu-admin-group -p kichu --authenticationDatabase admin ${params.DB_NAME} read.js
                """
            }
        }
    }
}
