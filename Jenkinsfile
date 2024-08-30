pipeline {
    agent {
        label "jenkins-agent"
    }
    environment {
        APP_NAME = "e2e-cicd-pipline"
    }

    tools {
        git 'default' // Specify the Git tool to use
    }

    stages {
        stage ("Cleanup workspace") {
            steps {
                cleanWs()
            }
        }

        stage ("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/fahadriaz1206/gitops-complete-prodcution-e2e-pipeline.git'
            }
        }

        stage ("Update the Deployment Tags") {
            steps {
                sh """
                    cat deployment.yaml
                    sed -i 's/${APP_NAME}.*${APP_NAME}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }

        }

        stage ("Push the change deployment file to Git") {
            steps {
                sh """
                    git config --global user.name "fahadriaz1206"
                    git config --global user.email "fahadriaz1206@gmail.com"
                    git add deployment.yaml
                    git commit -m "Updated Deployment Menifest"
                """
                //withCredentials([usernamePassword{credentialsId: 'github', gitToolName: 'default'}]) 
                withCredentials([usernamePassword(credentialsId: 'github', usernameVariable: 'GIT_USERNAME', passwordVariable: 'GIT_PASSWORD')]){
                    sh "git push https://github.com/fahadriaz1206/gitops-complete-prodcution-e2e-pipeline.git main"
                }
            }
        }
    }
}