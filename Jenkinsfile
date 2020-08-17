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
                    echo "${myList}"
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
