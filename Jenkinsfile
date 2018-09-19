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
        stage ('DeployToStaging') {
            when {
                branch 'master'
            }
            steps {
                withCredentials([usernamePassword(credendialsId: 'webserver_login' , usernameVariable: 'USERNAME',passwordVariable: 'USERPASS')]){
                    sshPublisher (
                        failOnError : true,
                        continueOnError : false,
                        publishers: [
                            sshPublisherDesc (
                                configName: 'staging',
                                sshCredential : [
                                    username : "$USERNAME",
                                    encryptedPasspharse: "$USERPASS" ],
                                transfers: [
                                    sshTransfer (
                                        sourceFiles : 'dist/trainSchedule.zip',
                                        removePrefix : 'dist/',
                                        remoteDirectory : '/tmp',
                                        execCommand : 'sudo /usr/bin/systemctl stop train-Schedule && rm -rf /opt/train-Schedule/* && unzip /tmp/trains-Schedule.zip -d /opt/train-Schedule && sudo /usr/bin/systemctl start train-Schedule'
                                        )
                                    ]
                                )
                            ]
                        )
                 }
            }
 }                              
             stage ('DeployToProduction') {
                                     when {
                branch 'master'
            }
                                    steps {
                                        input 'Does the Stagging env look OK?'
                                        milestone (1)
                                        withCredentials([usernamePassword(credendialsId: 'webserver_login' , usernameVariable: 'USERNAME',passwordVariable: 'USERPASS')]){
                    sshPublisher (
                        failOnError : true,
                        continueOnError : false,
                        publishers: [
                            sshPublisherDesc (
                                configName: 'staging',
                                sshCredential : [
                                    username : "$USERNAME",
                                    encryptedPasspharse: "$USERPASS" ],
                                transfers: [
                                    sshTransfer (
                                        sourceFiles : 'dist/trainSchedule.zip',
                                        removePrefix : 'dist/',
                                        remoteDirectory : '/tmp',
                                        execCommand : 'sudo /usr/bin/systemctl stop train-Schedule && rm -rf /opt/train-Schedule/* && unzip /tmp/trains-Schedule.zip -d /opt/train-Schedule && sudo /usr/bin/systemctl start train-Schedule'
                                        )
                                    ]
                                )
                            ]
                        )
                                        }}  } } 
}
                                    
