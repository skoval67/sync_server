pipeline {
    agent any
    // options {
    //     buildDiscarder(logRotator(numToKeepStr: '15', artifactNumToKeepStr: '15'))
    //     timeout(time: 20, unit: 'MINUTES')
    //     disableConcurrentBuilds()
    // }
    parameters {
        // choice(name: "update_servers", choices: ['no', 'yes'])
        // choice(name: 'grpr21', choices: returnBackupListPR21(), description: 'дата для отката изменений grpr21')
        // choice(name: 'grpr31', choices: returnBackupListPR31(), description: 'дата для отката изменений grpr31')
        // choice(name: 'grpr32', choices: returnBackupListPR32(), description: 'дата для отката изменений grpr32')
    }
    environment {
    //   AWS_ACCESS_KEY_ID = credentials('fonbet_gr_aws_access_key_id')
    //   AWS_SECRET_ACCESS_KEY = credentials('fonbet_gr_aws_secret_access_key')
    //   AWS_DEFAULT_REGION = "eu-south-1"
    //   S3_URL = "fonbet-gr" 
    //   //S3_BUCKET = "fonbet-gr"
    //   WORK_FOLDER = "/var/www"
    //   //CONFIG_FOLDER = 'filelist'
    //   IP_SERVER = '10.0.2.11'
    //   SERVER_USERNAME = 'jenkins'
    //   SERVER_PROD_IP = '10.253.53.60' // prodtest-deb
    //   SERVER_PROD_FOLDER = "/opt/grpr_prod"
      
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
            // steps {
            //     withCredentials([sshUserPrivateKey(credentialsId: "fonbet", keyFileVariable: 'keyfile')]) {
            //         script {
            //             servers.each{server,ip->
            //                 backup(server, ip)
            //             }
            //         }
            //     }
            // }
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