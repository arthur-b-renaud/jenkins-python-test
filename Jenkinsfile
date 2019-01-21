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
                sh'''
                export PYINT=python3.6
                export PYVERSION=3.6

                echo CREATE VIRTUAL ENVIRONMENT in $WORKSPACE/_venv
                if [-f $WORKSPACE/_venv]; then mkdir $WORKSPACE/_venv; fi
                echo $WORKSPACE/_venv
                export KEEPPATH=$PATH
                PYEXEC=$WORKSPACE/_venv/bin/python3.6

                export PATH=/usr/bin:$PATH
                "/usr/bin/$PYINT" -c 'from virtualenv import create_environment;create_environment(\"_venv\", site_packages=True)'
                export PATH=$KEEPPATH

                $PYEXEC -m pip install pipenv
                $PYEXEC -m pipenv --python $PYVERSION
                $PYPIPENVEXEC = $PYEXEC -m pipenv --where
                $PYPIPENVEXEC -m pip install pipenv
                $PYPIPENVEXEC -m pipenv install
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh '''
                $PYPIPENVEXEC = $PYEXEC -m pipenv --where
                echo "Sending pytest"
                $PYPIPENVEXEC -m pytest
                '''
                echo 'End Testing'
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