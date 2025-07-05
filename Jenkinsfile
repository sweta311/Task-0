pipeline {
    agent any

    environment {
        DEPLOY_SERVER = ${{ secrets.DEPLOY_SERVER }}
        DEPLOY_USER = ${{ secrets.DEPLOY_USER }}
        DEPLOY_PATH = ${{ vars.DEPLOY_PATH }}      
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'uat', credentialsId: 'Github', url: ${{vars.GITHUB_URL}}'
            }
        } 
        stage('SonarQube Analysis') {
            environment {
                scannerHome = tool 'sonarqube'
            }
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        ${scannerHome}/bin/sonar-scanner \
                        -Dsonar.projectKey= ${{ secrets.PROJECT_KEY }} \
                        -Dsonar.sources=. \
                        -Dsonar.host.url= ${{ secrets.HOST_URL }} \
                        -Dsonar.login= ${{ secrets.LOGIN_TOKEN }}
                    '''
                }
            }
        }        

        stage('Deploy HTML') {
            // when {
            //     changeset "**/frontend/**" 
            // }
            steps {
                sshagent(['ssh-snapcut']){
                    script {
                        echo 'Deploying updated HTML files to the frontend server...'
                        sh """
                            scp -r * $DEPLOY_USER@$DEPLOY_SERVER:$DEPLOY_PATH/
                            ssh $DEPLOY_USER@$DEPLOY_SERVER "sudo systemctl restart nginx"
                        """
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Frontend deployment successful!'
        }
        failure {
            echo 'Frontend deployment failed!'
        }
    }
}
