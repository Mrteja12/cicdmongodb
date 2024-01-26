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
        git branch: 'main', credentialsId: 'f14d13cc-ecb1-46b1-bc38-d63e23ec9823', url: 'https://github.com/Mrteja12/cicdmongodb.git'
      }
    }
    stage('GCP Auth') {
        steps {
          withCredentials([file(credentialsId: 'd688f03f-872d-4900-8c43-d5e5000a20c3', variable: 'gcp_creden')]) {
         sh 'gcloud auth activate-service-account --key-file=$gcp_creden'
        }
      }
    }
    stage("Docker pull") {
      steps {
        withCredentials([usernameColonPassword(credentialsId: 'f483136c-9d83-44d6-92cb-d880f2b80014', variable: 'docker')]){
      
        sh 'docker pull charyy/cassandra:tag'
        }
   
     }
    }
    stage("Docker tag") {
      steps {     
         sh 'docker tag charyy/cassandra:tag gcr.io/teja-410209/cassandra:tag'
      }
    }
    stage("Docker push") { 
        steps {
             sh 'gcloud auth configure-docker'
             sh 'docker push gcr.io/teja-410209/cassandra:tag'
           }
        }
     stage("cluster create") {
       steps {
          sh 'gcloud container clusters create cassandra-cluster --num-nodes 3 --location=asia-east1-a'
          
       }
     }
    stage("Create & expose deploy") {
       steps {
         sh 'kubectl create deployment cassandra --image=gcr.io/cassandra:tag'
         

       }
    }
  }  
} 
