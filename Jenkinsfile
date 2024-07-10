pipeline {
    agent any

    stages {
        stage('Pull Request Handling') {
            steps {
                script {
                    // Extract branch names from the webhook payload or parameters
                    def featureBranch = "feature"
                    def integrationBranch = "main"
                    
                    // Checkout the feature branch
                    checkout([$class: 'GitSCM', branches: [[name: featureBranch]], userRemoteConfigs: [[url: 'https://github.com/ramesh-h24/weather_app.git']]])

                    // Merge feature branch into integration branch
                    def mergeResult = sh(script: "git merge --allow-unrelated-histories ${featureBranch} --no-ff", returnStatus: true)

                    if (mergeResult == 0) {
                        // Merge successful, push integration branch to another repo
                        sh "git push https://github.com/ramesh-h24/PR-_test_repo.git ${integrationBranch}"
                    } else {
                        error "Merge conflict detected. Pipeline aborted."
                    }
                }
            }
        }

        stage('Gated Tests with Timeout') {
            steps {
                timeout(time: 10, unit: 'SECONDS') {
                    // Perform gated tests, e.g., integration tests, unit tests
                    // Example: sh 'mvn test'
                    echo 'Running gated tests...'
                    sh 'sleep 15' // Placeholder for actual tests
                }
            }
        }
    }

    post {
        success {
            // Success handling after gated tests pass
            echo 'Gated tests passed successfully.'
            // Additional steps if needed
        }

        failure {
            // Failure handling if tests fail or timeout
            echo 'Gated tests failed or timed out.'
            // Additional steps if needed
        }
    }
}
