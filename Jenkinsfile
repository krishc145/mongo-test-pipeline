pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/krishc145/mongo-test-pipeline', branch: 'main'
            }
        }

        stage('Insert Data into Mongo') {
            steps {
                bat '''
                    echo db.dba_admin_fruits.insertMany([
                      {name: "mango"},
                      {name: "apple"},
                      {name: "jackfruit"}
                    ]) > insert.js

                    mongosh -u kichu-admin-group -p kichu --authenticationDatabase admin krishna_DB_admin insert.js
                '''
            }
        }

        stage('Read Data') {
            steps {
                bat '''
                    echo db.dba_admin_fruits.find().forEach(function(doc) { printjson(doc); }) > read.js

                    mongosh -u kichu-admin-group -p kichu --authenticationDatabase admin krishna_DB_admin read.js
                '''
            }
        }
    }
}
