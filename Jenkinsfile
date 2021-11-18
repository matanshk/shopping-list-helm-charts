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

        stage('Checkout') {
            steps {
                checkout scm
            }
        }


        stage('Change image ArgoCD image tag') {
            // when {
            //     branch 'release/*'
            // }
            steps {
                script {
                    sshagent(['jenkins-ssh']) {
                    echo "############### Change image ArgoCD image tag to $NEW_VERSION ################"
                    // sh "git status"
                    // sh "git branch"      
                    // sh " ./branch.sh ${env.BRANCH_NAME}"
                    // OLD_TAG = sh(script: 'echo $(cat ./ver.txt)', returnStdout: true).trim() 
                    sh """ 
                    
                    echo "############ Changing shopping list app image tag: ############"
                    OLD_TAG=$(cat ./shopping-list/values.yaml | grep imageTag | cut -d " " -f 4)
                    sed -i "s/$OLD_TAG/$NEW_VERSION/g" ./shopping-list/values.yaml
                    echo "###############################################################"


                    echo "################## Changing nginx image tag: ##################"
                    OLD_TAG=$(cat ./nginx/values.yaml | grep imageTag | cut -d " " -f 4)
                    sed -i "s/$OLD_TAG/$NEW_VERSION/g" ./nginx/values.yaml
                    echo "###############################################################"


                    """
                    sh " echo $OLD_TAG "
                    sh " echo $NEW_VERSION "
                    // sh "git clean -f"
                    }                
                }            
            }
        }

        // stage('Push') {
        //     steps {
        //         sh """
        //             git add .
        //             git commit -m "Updated app+nginx image tag to $NEW_VERSION"
        //             git push
        //         """
        //     }
        // }   
    }
}