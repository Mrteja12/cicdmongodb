pipeline {
  agent any
  stages {

      stage("Clear Workspace"){
        steps {
          sh 'rm -rvf G*'
        }
    }
       stage("Checkout") {
        steps {
        sh 'git clone https://github.com/Mrteja12/cicdmongodb.git'
      }
    }
    stage('GCP Auth') {
        steps {
         withCredentials([usernameColonPassword(credentialsId: 'd5561465-63fa-4631-94f1-b8199d940da6', variable: 'GCP'), file(credentialsId: 'GCP', variable: 'GCP')]){
         sh 'gcloud auth activate-service-account --key-file=$GCP'
        }
      }
    }
    stage("Docker pull") {
      steps {
        withCredentials([usernameColonPassword(credentialsId: '04612ef4-06e8-4cb0-8bde-e5bc930020e6', variable: 'docker')])  {
        sh 'docker login -u charyy -p 123456789'
        sh 'docker pull charyy/tejachary:tag23'
      
       }
     }
    }
    stage("Docker tag") {
      steps {     
         sh 'docker tag charyy/tejachary:tag23 gcr.io/tarak-408506/cassandradb:taga'
      }
    }
    stage("Docker push") { 
        steps {
             sh 'gcloud auth configure-docker'
             sh 'docker push charyy/tejachary:tag23'
           }
        }
     stage("cluster create") {
       steps {
          sh 'gcloud container clusters create cassandradb-cluster --num-nodes 3 --location=asia-east1-a'
       }
     }
    stage("Create & expose deploy") {
       steps {
         sh 'kubectl create deployment mongodb --image=gcr.io/tarak-408506/cassandradb:taga'
         sh 'kubectl expose deployment mongodb --type=LoadBalancer --port 80 --target-port 8080'
       }
    }
  }  
} 