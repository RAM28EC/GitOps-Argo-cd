pipeline {
    agent any
    environment {
        
        APP_NAME = "gitops-argo-app"
        
    }
    stages {
        stage('clean workspace'){
            steps {
                script{
                    cleanWs()
                }
            }
        }
        stage('git checkout'){
            steps{
                script{
                    git credentials : 'github',
                    url: 'https://github.com/RAM28EC/GitOps-Argo-cd.git',
                    branch: 'main'
                }
            }
        }
        
        stage('updating kubernetes deployment file'){
            steps{
                script{
                    sh """
                    cat deployment.yml
                    sed -i 's/${APP_NAME}/${APP_NAME}.*:${IMAGE_TAG}/g' deployment.yml
                    cat deployment.yml
                    """
                }
            }
        }
        stage('push the changed deployment file to git') {
            steps{
                script{
                    sh """
                    git config --global user.name "RAM28EC"
                    git config --global user.email "ram.28ec@gmail.com"
                    git add deployment.yml
                    git commit -m "updated deployment file"
                    """
                    withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]){
                      sh  "git push https://github.com/RAM28EC/GitOps-Argo-cd.git main"
                    }
                }
            }
        }
    }
}
