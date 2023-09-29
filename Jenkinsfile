pipeline{
    agent {label "jenkins-agent"}
    environment{
        APP_NAME = "devops-project"
    } 
    stages{
        stage("cleanup workspace"){
            steps{
                cleanWs()
            }
        }
        stage("checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/mukeshr-29/gitops-project-7.git'
            }

        }
        stage("update the deployment tags"){
            steps{
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*/${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage("push the changes to git"){
            steps{
                sh """
                    git config --global user.name "mukeshr-29"
                    git config --global user.email "mukeshr2911@gmail.com"
                    git add deployment.yaml
                    git commit -m "auto build commit"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolname: 'Default')]){
                    sh "git push https://github.com/mukeshr-29/gitops-project-7.git main"
                }
            }
        }
    }
}