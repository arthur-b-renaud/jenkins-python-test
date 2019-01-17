pipeline {
    agent any

    stages {
        stage ("Code pull"){
            steps{
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh '''PYENV_HOME=$WORKSPACE/.jenkins_venv_ZERZE/
                    if [ -d $PYENV_HOME ]; then
                       rm -rf $PYENV_HOME
                    fi

                    echo "$WORKSPACE/.jenkins_venv_ZERZE/"

                    virtualenv --no-site-packages $PYENV_HOME
                    . $PYENV_HOME/bin/activate
                    pipenv install
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
    }
    post {
        always {
            echo 'This will always run'
        }
        success {
            echo 'This will run only if successful'
        }
        failure {
            echo 'This will run only if failed'
        }
        unstable {
            echo 'This will run only if the run was marked as unstable'
        }
        changed {
            echo 'This will run only if the state of the Pipeline has changed'
            echo 'For example, if the Pipeline was previously failing but is now successful'
        }
    }
}