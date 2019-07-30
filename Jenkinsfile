pipeline {
    agent {
        kubernetes {
            label 'my-pod-template'
            defaultContainer 'curl'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: curl
    image: centos:latest
    command:
    - cat
    tty: true
  - name: busybox
    image: busybox
    command:
    - cat
    tty: true
"""
        }
    }
  stages{
    stage('api call'){
      environment {
        FLOWSERVER = credentials('flow-server-url')
      }
      steps{
        withCredentials([usernamePassword(credentialsId: 'flow-admin-creds', passwordVariable: 'vPassword', usernameVariable: 'vUser')]) {
          sh 'curl -u $vPassword:$vUser --insecure -X GET "${FLOWSERVER}/rest/v1.0/users" -H "accept: application/json"'
        }   
      }
    }
  }
}
