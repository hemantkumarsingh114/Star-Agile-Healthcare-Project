pipeline {
    agent any
    stages{
        stage('Build Maven'){
            steps{
                git url:'https://github.com/hemantkumarsingh114/Star-Agile-Healthcare-Project.git', branch: "master"
               sh 'mvn clean install'
            }
        }
        stage('Build Docker Image'){
            steps{
                script{
                    sh 'docker build -t hemantkumarsingh114/healthcareproject:1.0 .'
                }
            }
        }
        stage('Docker Hub login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push hemantkumarsingh114/healthcareproject:1.0'
                }
            }
        }
                
        stage('Deploy to Kubernetes') {
            when {
                expression { 
                    // Debug: Print the branch name
                    echo "Branch: ${env.BRANCH_NAME}"
                    return env.BRANCH_NAME == 'master'
                }
            }
            steps {
                script {
                    kubernetesDeploy(configs: 'deploymentservice.yml', kubeconfigId: 'k8sconfigpwd')
                }
            }
        }
    }
}
