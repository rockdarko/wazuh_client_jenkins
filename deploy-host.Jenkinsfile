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
        stage ('wazuh') {
            steps {
                sh "ansible-playbook -i ${INVENTORY_CELLARDOOR}/env/${env.ENV}/${env.ENV}.hosts -l ${env.HOST} -e wazuh_server=${env.SERVER} -e wazuh_client_default_group=${env.GROUP} -e wazuh_installer_url=${env.INSTALLER_URL} deploy.yml"
            }
        }

    }
}