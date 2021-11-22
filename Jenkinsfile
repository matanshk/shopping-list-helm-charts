pipeline {

    options {
        timestamps()
        timeout(time:15, unit:'MINUTES')
        buildDiscarder(logRotator(
            numToKeepStr: '8',
            daysToKeepStr: '10',
            artifactNumToKeepStr: '30'))
    }
    
    agent any

    stages {
        stage('Change image ArgoCD image tag') {
            steps {
                script {
                    sshagent(['jenkins-ssh']) {
                    sh """
                    echo "############### Change image ArgoCD image tag to $NEW_VERSION ################"
                    """
                    OLD_TAG = sh(script: 'cat ./shopping-list/values.yaml | grep imageTag | cut -d " " -f 4', returnStdout: true).trim() 
                    sh """
                    sed -i "s/$OLD_TAG/$NEW_VERSION/g" ./shopping-list/values.yaml
                    echo "###############################################################"
                    echo "Old TAG: $OLD_TAG "
                    echo "New TAG: $NEW_VERSION "
                    """
                    }                
                }            
            }
        }

        stage('Push') {
            steps {
                sh """
                    git add .
                    git commit -m "Updated app+nginx image tag to $NEW_VERSION"
                    git push -u origin master
                """
            }
        }   
    }
}