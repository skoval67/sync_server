def returnBackupList() {
    // sh '''
    //     set -x
    //     ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 "ls /tmp/*.tar.gz"
    // '''
    ret_value = ['nginx_backup_2023_08_28_09_16_47', 'nginx_backup_2023_08_28_09_17_56']
    //return "ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 'ls /tmp/*.tar.gz'".execute().text
    return ret_value
}

def backup() {
    sh '''
        set -x
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "sudo tar cvzf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz /etc/nginx"
        scp -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr:/tmp/nginx_backup_${CURRENT_TIME}.tar.gz admin@10.128.0.3:/tmp
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "sudo rm -rf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz"
    '''
}

pipeline {
    agent any
    // options {
    //     buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
    //     timeout(time: 20, unit: 'MINUTES')
    //     disableConcurrentBuilds()
    // }
    parameters {
        choice(name: "update_servers", choices: ['no', 'yes'])
        choice(name: 'restore_to', choices: returnBackupList(), description: 'дата для отката изменений')
    }
    
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