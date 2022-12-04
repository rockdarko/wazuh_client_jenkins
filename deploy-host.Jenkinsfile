#!/usr/bin/env groovy

pipeline {
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr: '5'))
        disableConcurrentBuilds()
    }

    stages {
        stage ('git checkout') {
            steps {
                checkout scm
            }
        }
        stage ('ansible setup') {
            steps {
	            sh "rm -rf roles"
	            sh "ansible-galaxy install -f -r requirements.yml"        	    
            }
        }
        stage ('chrony') {
            steps {
                sh "ansible-playbook -i ${INVENTORY_CELLARDOOR}/env/${env.ENV}/${env.ENV}.hosts -l ${env.HOST} -e chrony_server=${env.SERVER} deploy.yml"
            }
        }

    }
}