pipeline {
    agent any

    stages {
        stage('Init') {
            steps {
                sh "env"
                echo 'Initializing..'
                echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"
            }
        }
        stage('Test') {
            steps {
                script {
                    // Print current branch name
                    echo "Current Branch: ${GIT_BRANCH}"
                    
                    // Print last commit hash
                    echo "Last Commit Hash: ${GIT_COMMIT}"
                    
                    // Print repository URL
                    echo "Repository URL: $GIT_URL}"
                    
                    // Print the author of the last commit
                    echo "Last Commit Author: ${GIT_AUTHOR_NAME}"
                    
                    // Print the commit message of the last commit
                    //echo "Last Commit Message: ${git.message}"
                    
                    // Print changeset details (files changed, commit message, etc.)
                    //def changeLog = git.changelog
                    //echo "ChangeLog: ${changeLog}"
                    echo 'Testing..'
                    echo 'Running pytest..'
                }
            }
        }
        stage('Build') {
            steps {
                echo 'Building..'
                echo 'Running docker build -t sntshk/cotu .'
            }
        }
        stage('Publish') {
            steps {
                echo 'Publishing..'
                echo 'Running docker push..'
            }
        }
        stage('Cleanup') {
            steps {
                echo 'Cleaning..'
                echo 'Running docker rmi..'
            }
        }
    }
}
