pipeline {
  agent {
    docker {
      image 'node:6-alpine'
      args '-p 3001:3001'
    }
    
  }
  stages {
    stage('Sonar Quality') {
      steps {
        echo 'sonar'
      }
    }
    stage('Build Validation') {
      parallel {
        stage('Selective CI Validation') {
          steps {
            echo 'working'
          }
        }
        stage('Review Deployment Checklist') {
          agent none
          steps {
            input(id: 'Proceed1', message: 'Is the Deployment Checklist ok?', parameters: [
                                                                                                      [$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'Please confirm you agree with this'],
                                                                                                      [$class: 'TextParameterDefinition', defaultValue: 'Comments if false', description: 'Environment', name: 'env'],
                                                                                                                     ])
            }
          }
        }
      }
      stage('AutoMerge') {
        steps {
          echo 'merge pull request'
          sleep 4
        }
      }
      stage('Full CI Validation') {
        steps {
          echo 'Full CI'
          sleep 7
        }
      }
      stage('Manual Pre Deployment to QA') {
        agent none
        steps {
          input(id: 'Proceed4', message: 'Are all Manual steps performed?', parameters: [
                                                                                        [$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'Please confirm'],
                                                                                        [$class: 'TextParameterDefinition', defaultValue: 'Comments if false', description: 'Environment', name: 'env'],
                                                                                        ])
          }
        }
        stage('Deploy to QA') {
          agent none
          steps {
            input 'Proceed if the Sandbox is ready'
            sleep 3
          }
        }
        stage('Manual Post Deployment to QA') {
          agent none
          steps {
            input(id: 'Proceed2', message: 'Are all Post Deployment steps performed?', parameters: [
                                                                                    [$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'Please confirm'],
                                                                                    [$class: 'TextParameterDefinition', defaultValue: 'Comments if false', description: 'Environment', name: 'env'],
                                                                                                   ])
            }
          }
          stage('Functional QA Testing Evidence') {
            agent none
            steps {
              input(id: 'Proceed3', message: 'Please provide a link to where the testing evidence is uploaded to Confluence', parameters: [
                                                                                    [$class: 'BooleanParameterDefinition', defaultValue: false, description: '', name: 'Please confirm'],
                                                                                    [$class: 'TextParameterDefinition', defaultValue: 'Location of Testing Evidence', description: 'Environment', name: 'env'],
                                                                                    ])
              }
            }
          }
        }
