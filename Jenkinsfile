def backups_list = []

node('master') {
   stage('prepare backups list') {
       def my_choices = sh script: 'ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 "ls /tmp/*.tar.gz" | sed -nE "s/\\/tmp\\/(.+).tar.gz/\\1/p"', returnStdout:true
       backups_list = my_choices.trim()
   }
}

def backup_config() {
    sh '''
        set -x
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "sudo tar cvzf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz /etc/nginx"
        scp -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr:/tmp/nginx_backup_${CURRENT_TIME}.tar.gz admin@10.128.0.3:/tmp
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@websrvr "sudo rm -rf /tmp/nginx_backup_${CURRENT_TIME}.tar.gz"
        # make sync here
    '''
}

def restore_config(filename) {
    sh returnStdout:true, script: '''
        set -x
        scp -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 "sudo tar xzf /tmp/${filename}.tar.gz /etc"
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
        choice(name: "update_servers", choices: ['yes', 'no'])
        choice(name: "restore_to", choices: backups_list, description: "бекап для отката изменений")
    }
    
    environment {
        CURRENT_TIME = sh(script: "echo \$(date +%Y_%m_%d_%H_%M_%S)", returnStdout: true).trim()
    }

    stages {
        stage("Create backup") {
            when { 
                expression{params.update_servers == 'yes'}
            }            
            steps {
                script {
                    backup_config()
                }
            }
        }

        stage("Make restore") {
            when { 
                expression{params.update_servers == 'no'}
            }
            steps {
                script {
                    sh '''
                        echo "${CURRENT_TIME}"
                        echo $params.update_server
                        echo $params.restore_to
                    '''
                }
                script {
                    restore_config($params.restore_to)
                }
            }
        }
    }
    
    // post {
    //     cleanup {
    //         deleteDir() /* clean up our workspace */
    //     }
    // }
}