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


