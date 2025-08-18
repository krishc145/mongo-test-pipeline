pipeline {
    agent any

    stages {
        stage('Insert Data into Mongo') {
            steps {
                bat '''
                    (
                        echo db.dba_admin_fruits.insertMany([
                        echo   {name: "mango"},
                        echo   {name: "apple"},
                        echo   {name: "jackfruit"}
                        echo ])
                    ) > insert.js
                    mongosh krishna_DB_admin insert.js
                '''
            }
        }

        stage('Read Data') {
            steps {
                bat '''
                    (
                        echo db.dba_admin_fruits.find().forEach(function(doc) {
                        echo   printjson(doc)
                        echo })
                    ) > read.js
                    mongosh krishna_DB_admin read.js
                '''
            }
        }
    }
}
