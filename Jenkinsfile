pipeline {
    agent {
        label 'agent01'
    }
    environment {
        NOTIF = credentials('notif-discord')
    }
    stages {
        stage('Clone Repository'){
            steps {
                echo 'Clone repository'
                sh 'rm -rf k8s-manifest'
                sh 'git clone https://github.com/jokoss92/k8s-manifest.git'
            }
        }
        stage('Update Git'){
            steps {
                echo 'Update Git'
                sh 'cat deployment.yml'
                sh "sed -i 's/api-server:/api-server:${BUILD_NUMBER}/g' deployment.yml"
                sh 'cat deployment.yml'
            }
        }
    }
     post {
            always {
                echo 'Jobs triggered'
                discordSend description: "Jenkins Pipeline Build", footer: "Jobs triggered", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$NOTIF"
            }
            success {
                 echo 'Jobs success'
                 discordSend description: "Jenkins Pipeline Build", footer: "Jobs success", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$NOTIF"
            }
            failure {
                echo 'Jobs failure'
                discordSend description: "Jenkins Pipeline Build", footer: "Jobs failure", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$NOTIF"
            }
            aborted {
                echo 'Jobs aborted'
                discordSend description: "Jenkins Pipeline Build", footer: "Jobs aborted", link: env.BUILD_URL, result: currentBuild.currentResult, title: JOB_NAME, webhookURL: "$NOTIF"
            }
        }
}
