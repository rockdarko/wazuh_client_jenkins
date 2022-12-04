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
                sh "ansible-playbook -i /var/lib/jenkins/cellardoor/env/${env.ENV}/${env.ENV}.hosts -e wazuh_server=${env.SERVER} -e wazuh_client_default_group=${env.GROUP} -e wazuh_installer_url=${env.INSTALLER_URL} deploy.yml"
            }
        }

    }
}

