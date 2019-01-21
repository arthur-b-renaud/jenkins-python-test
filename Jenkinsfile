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
                export NAME_JENKINS=testjenkins
                export DIST=std
                export PYINT=python3.6
                export VERSION=3.6
                export NAME=UT

                echo AUTOMATEDSETUP
                export current=$WORKSPACE/$NAME_JENKINS

                echo interpreter=/usr/bin/$PYINT

                echo CREATE VIRTUAL ENVIRONMENT in $WORKSPACE/$NAME_JENKINS/_venv
                if [-f $WORKSPACE/$NAME_JENKINS/_venv]; then mkdir "$WORKSPACE/$NAME_JENKINS/_venv"; fi
                export KEEPPATH=$PATH
                export PATH=/usr/bin:$PATH
                "/usr/bin/$PYINT" -c 'from virtualenv import create_environment;create_environment(\"$WORKSPACE/$NAME_JENKINS/_venv\", site_packages=True)'
                export PATH=$KEEPPATH
                if [ $? -ne 0 ]; then exit $?; fi

                echo INSTALL
                export PATH=$WORKSPACE/$NAME_JENKINS/_venv/bin:$PATH
                echo $PATH
                if [ $? -ne 0 ]; then exit $?; fi
                $PYINT -c 'from pip._internal import main;main(\"install -r requirements.txt\".split())'
                if [ $? -ne 0 ]; then exit $?; fi
                $PYINT --version
                if [ $? -ne 0 ]; then exit $?; fi
                $PYINT -c 'from pip._internal import main;main([\"freeze\"])'
                if [ $? -ne 0 ]; then exit $?; fi
                '''
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
                sh '''
                PYENV_HOME=$WORKSPACE/.jenkins_venv_ZERZE/
                . $PYENV_HOME/bin/activate
                echo "Sending pytest"
                pytest
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