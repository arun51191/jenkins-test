def myVar() {
       def result_str = sh(script: './script.sh', returnStdout: true).trim()
       if(!result_str?.trim()) {
           result_list = null
       }
       else {
           println "In else condtion"
           result_list = result_str.split()
       }
       println result_list
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
        stage("Validate") {
               steps {
                      script {                             
                             if(null == null) {
                                   println "Exiting the build as no lambda function is updated"
                                   currentBuild.result = 'SUCCESS'
                                   println "${currentBuild.currentResult}" 
                             } 
                             else {
                                   println myList
                                   println "Exiting the build even the condition failed"
                                   currentBuild.result = 'SUCCESS'
                                   println "${currentBuild.currentResult}"
                             
                             }
                                                                       
                      }
               }
        }   
           
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
