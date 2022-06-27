pipeline {
    agent any
    stages {
        stage('Pytest') {
            agent { 
                label 'tester'
            }            
            steps {
                //
                git branch: 'main', url: 'https://github.com/Crush-Steelpunch/staffflaskapp.git'
                sh '''#!/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                pip3 install pytest pytest-cov
                python3 -m pytest --cov=application'''
            }
        }
        stage('Pip install') {
            agent { 
                label 'runner'
            }
            steps {
                //
                git branch: 'main', url: 'https://github.com/Crush-Steelpunch/staffflaskapp.git'
                sh '''#!/bin/bash
                python3 -m venv venv
                source venv/bin/activate
                pip3 install -r requirements.txt
                pip3 install gunicorn'''
            }
        }
        stage('Deploy') {
            agent { 
                label 'runner'
            }            
            steps {
                //
                sh '''#!/bin/bash
                if [ -f  /tmp/gpidfile ]
                  then kill $(cat /tmp/gpidfile)
                fi
                source venv/bin/activate
                BUILD_ID=nokill gunicorn -D -w 4 -b 0.0.0.0:5000 -p /tmp/gpidfile'''
            }
        }
    }
}