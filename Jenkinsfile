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
            //when {
            //    branch 'master'
            //}
            steps{
                withCredentials([usernamePassword('credentialsId':'nodeservercredentials',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sshPublisher(
                        failOnError: true,
                        continueOnError: false,
                        publishers: [
                            sshPublisherDesc(
                                configName: 'staging',
                                sshCredentials: [
                                    username: "$USERNAME",
                                    encryptedPassphrase: "$PASSWORD"
                                ],
                                transfers: [
                                    sshTransfer(
                                        sourceFiles: 'dist/trainSchedule.zip',
                                        removePrefix: 'dist/',
                                        remoteDirectory: '/tmp',
                                    )                                
                                ]
                            )
                        ]
                    )
                }
            }
        }
    }
}
