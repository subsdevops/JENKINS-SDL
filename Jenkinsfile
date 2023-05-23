pipeline {
    agent any
    tools {
        go 'go-1.20.4' // Comes from the jenkins global config
    }
    environment {
        ENV = "${env.BRANCH_NAME == 'master' ? 'PROD' : 'DEV'}"
        BRANCH = "${env.BRANCH_NAME}" // NEW ADDITION => Needed by the deploy.sh script
    }

    stages {
        stage('Scan for secrets') {
            steps {
                sh '''
                    curl -LO https://github.com/zricethezav/gitleaks/releases/download/v8.9.0/gitleaks_8.9.0_linux_x64.tar.gz
                    tar -xzf gitleaks_8.9.0_linux_x64.tar.gz
                    ./gitleaks protect -v // Scan for commonly leaked secrets
                    rm -rf gitleaks*
                '''
            }
        }    
        stage('Build') {
            steps {
                sh 'bash scripts/build.sh'
            }
        }
        stage('Test') {
            steps {
                sh 'bash scripts/test.sh'
            }
        }
        // NEW DEPLOY STAGE
        stage('Deploy') {
            when {

                /*
                anyOf ensures that this stage will only be triggered when the
                values mentioned match.
                With below config, the deploy stage ONLY triggers if the branch is
                develop or master.
                */
                anyOf {
                    branch 'master';
                    branch 'develop'
                }
            }
            steps {
                // Run the deployment script as mentioned in the task.
                sh 'export JENKINS_NODE_COOKIE=do_not_kill ; bash scripts/deploy.sh'
            }
        }
    }
}


