pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Deploy to Staging'){
            when {
                branch 'master'
            }
            steps{
                withCredentials([usernamePassword('credentialsId':node_server,usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sshPublisher(
                        failOnError:true,
                        continueOnError: false,
                        publishers: [
                            configName: 'Staging',
                            sshCredentials: [
                                username: "$USERNAME",
                                encryptedPassPhrase: "$PASSSWORD"
                            ],
                            transfers: [
                                sshTransfer(
                                    sourceFiles: 'dist/trainSchedule.zip',
                                    removePrefix: 'dist/',
                                    remoteDirectory: '/tmp',
                                )                                
                            ]
                        ]
                    )
                }
            }
        }
    }
}
