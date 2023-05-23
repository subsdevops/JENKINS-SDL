pipeline {
    agent any
    tools {
        go 'go-1.19' // Comes from the jenkins global config
    }
    environment {
        ENV = "${env.BRANCH_NAME == 'master' ? 'PROD' : 'DEV'}" // Define the ENV based on the branch name
    }

    stages {
        stage('Build') {
            steps {
                sh 'bash scripts/build.sh' // Run the build
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh' // Run the test
            }
        }
    }
}
