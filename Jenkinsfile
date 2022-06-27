pipeline {
    agent any
    stages {
        stage('Pytest') {
            agent "tester"
            steps {
                //
                git branch: 'main', url: 'https://github.com/Crush-Steelpunch/staffflaskapp.git'
                sh '''#/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                python3 -m pytest'''
            }
        }
        stage('Pip install') {
            agent "runner"
            steps {
                //
                sh '''#/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                pip3 install gunicorn'''
            }
        }
        stage('Deploy') {
            agent "runner"
            steps {
                //
                sh '''#/bin/bash
                kill $(cat /tmp/gpidfile)
                source venv/bin/activate
                BUILD_ID=nokill gunicorn -D -w 4 -b 0.0.0.0:5000 -p /tmp/gpidfile'''
            }
        }
    }
}