def backup() {
    sh '''
        set +x
        echo ${CURRENT_TIME} 
        echo $(pwd)
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "sudo tar cvzf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz /etc/nginx"
        scp -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr:/tmp/nginx_backup_${CURRENT_TIME}.tar.gz admin@websrvr2:/tmp
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "rm -rf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz"
        echo $(hostname)
    '''
}

pipeline {
    agent any
    // options {
    //     buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
    //     timeout(time: 20, unit: 'MINUTES')
    //     disableConcurrentBuilds()
    // }
    // parameters {
        // choice(name: "update_servers", choices: ['no', 'yes'])
        // choice(name: 'grpr21', choices: returnBackupListPR21(), description: 'дата для отката изменений grpr21')
        // choice(name: 'grpr31', choices: returnBackupListPR31(), description: 'дата для отката изменений grpr31')
        // choice(name: 'grpr32', choices: returnBackupListPR32(), description: 'дата для отката изменений grpr32')
    // }
    
    environment {
        CURRENT_TIME = sh(script: "echo \$(date +%Y_%m_%d_%H_%M_%S)", returnStdout: true).trim()
    }

    stages {
        stage("Create backup") {
            // when { 
            //     allOf {
            //         expression{params.grpr21 == ''}
            //         expression{params.grpr31 == ''}
            //         expression{params.grpr32 == ''}
            //     }
            // }            
            steps {
                script {
                    backup()
                }
            }
        }

        // stage("Sync") {
        //     when { 
        //         allOf {
        //             expression{params.update_servers == 'yes'}
        //             expression{params.grpr21 == ''}
        //             expression{params.grpr31 == ''}
        //             expression{params.grpr32 == ''}
        //         }
        //     } 
        //     steps {
        //         withCredentials([sshUserPrivateKey(credentialsId: "fonbet", keyFileVariable: 'keyfile')]) {
        //             script {
        //                 servers.each{server,ip->
        //                     syncserver(server, ip)
        //                 }
        //             }
        //         }
        //     }
        // }

        // Переделать в один этап.
    //     stage("restore to date grpr21") {
    //         when { 
    //              expression{params.grpr21 != ''}
    //         }
    //         steps {
    //             withCredentials([sshUserPrivateKey(credentialsId: "fonbet", keyFileVariable: 'keyfile')]) {
    //                 script {
    //                     restorebackup("grpr21", "ip", params.grpr21 ) // IP 10.253.49.2
    //                 }
    //             }
    //         }
    //     }
    

    //     stage("Update filelist") {
    //         steps {
    //             withCredentials([sshUserPrivateKey(credentialsId: "fonbet", keyFileVariable: 'keyfile')]) {
    //                 script {
    //                     servers.each{server, ip->
    //                         getBackupList(server, ip)
    //                     }
    //                 }
    //             }
    //         }
    //     }
    // }
    // post {
    //     cleanup {
    //         deleteDir() /* clean up our workspace */
    //     }
  }
}