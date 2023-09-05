def backups_list = []

def update_backups_list() {
    node {
        return sh(script: "ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 ls /tmp/*.tar.gz | sed -nE 's/\\/tmp\\/(.+).tar.gz/\\1/p'", returnStdout: true)
    }
    //output = sh(script: "ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 'ls /tmp/*.tar.gz'", returnStdout: true)
    //return "echo ${output} | sed -nE 's/\\/tmp\\/(.+).tar.gz/\\1/p'".execute().text
    //return "echo ${output}".execute().text
    //return '''ls /tmp/*.tar.gz'''.execute().text
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
    sh """
        ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/secrets/id_ed25519 admin@10.128.0.3 "sudo tar xzvf /tmp/${filename}.tar.gz -C /"
    """
}

// node('master') {
//    stage('prepare backups list') {
//        backups_list = update_backups_list()
//    }
// }

pipeline {
    agent {
        label 'master'
    }
    options {
        buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
        timeout(time: 20, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    environment {
        CURRENT_TIME = sh(script: "echo \$(date +%Y_%m_%d_%H_%M_%S)", returnStdout: true).trim()
        BACKUP_DIR = "/opt/backup"
        SEARCH_DIR = sh(script: "sed -nE 's/\//\\\//g' <<< ${BACKUP_DIR}", returnStdout: true).trim()
    }
    parameters {
        choice(name: "update_config", choices: ['yes', 'no'], description: "yes - будет создана резервная копия текущего конфига nginx и его синхронизация с другого сервера,\n \
no - будет восстановлен конфиг nginx из резервной копии, указанной в параметре restore_to")
        choice(name: "restore_to", choices: update_backups_list(), description: "бекап для отката изменений")
    }
    
    stages {
        stage("Create backup") {
            when { 
                expression{params.update_config == 'yes'}
            }            
            steps {
                script {
                    try {
                        backup_config()
                    }
                    catch (exc) {
                        throw exc
                    }
                }
            }
        }

        stage("Make restore") {
            when { 
                expression{params.update_config == 'no'}
            }
            steps {
                script {
                    try {
                        restore_config(params.restore_to)
                    }
                    catch (exc) {
                        throw exc
                    }
                }
            }
        }
    }

    post {
        cleanup {
            deleteDir() /* clean up our workspace */
        }
    }
}