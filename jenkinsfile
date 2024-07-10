pipeline {
    agent any  
    stages {
        stage('Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/ramesh-h24/PR-_test_repo.git'
                }
            }
        }
        stage('Check Merge Conflicts') {
            steps {
                script {
                    def featureBranch = "feature-1"  
                    def mainBranch = "main"
                    sh "git fetch origin ${featureBranch}:${featureBranch}"
                    def result = sh script: "git merge --no-commit --no-ff origin/${featureBranch}", returnStatus: true
                    if (result != 0) {
                        error "Merge conflicts detected. Aborting pipeline."
                    } else {
                        echo "No merge conflicts. Proceeding with merge."
                    }
                }
            }
        }
        stage('Merge to Main') {
            steps {
                script {
                    sh "git commit -m 'Merging ${featureBranch} into ${mainBranch}'"
                    withCredentials([usernamePassword(credentialsId: 'git_repo', passwordVariable: 'GIT_PASSWORD', usernameVariable: 'GIT_USERNAME')]) {
                        sh "git push https://${GIT_USERNAME}:${GIT_PASSWORD}@github.com/weather_app.git ${mainBranch}"
                    }
                }
            }
        }
    }
    post {
        success {
            build job: 'wether_ap'
        }  
        failure {
            echo 'Pipeline failed.'
        }
    }
}
