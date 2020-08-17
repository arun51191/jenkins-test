def myVar() {
       println "Executing shell script"
       def result_str = sh(script: './script.sh', returnStdout: true).trim()
       println "Testing the shell script ouput"
       if(!result_str?.trim())
           println "Exiting the script no lambda function has been updated"
           currentBuild.result = 'SUCCESS'
           String[] result_list = result_str.split()
       return result_list
}

pipeline {
    environment { 
        myList = myVar()
    }
    agent any
    options {
        buildDiscarder(logRotator(numToKeepStr:'10'))
        timeout(time: 30, unit: 'MINUTES')
        ansiColor('xterm')
        disableConcurrentBuilds()
    }

    stages {
        stage("Build") {

            agent {
                docker { 
                     image 'python:3.6-stretch' 
                     args '--network host'
                }
            }            
            steps {
                script {
                 sh """#!/bin/bash -xe
                    echo "running build stage"
                    #mkdir temp
                    #cd temp
                    #python3 -m venv ami_cleanup_job
                    #source ami_cleanup_job/bin/activate
                    #pip install  --no-cache-dir --default-timeout=100 -U pip
                    #pip3 install --no-cache-dir --default-timeout=100 -r ../requirements.txt
                    #cp -r ami_cleanup_job/lib/python3.6/site-packages/* .
                    #deactivate                    
                    #cd ..
                 """ 
                }
            }
        }

        stage("Compress") {

            steps {
                script {
                    println myList
                }
            }             
        }
    }
}
