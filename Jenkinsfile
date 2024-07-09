 pipeline {
   agent any
   
    environment {
        IMAGE_TAG = "${BUILD_NUMBER}"
        IMG_NAME = "angulardemo"
        // getting stored credentials
        DOCKERHUB_CREDENTIALS = credentials('dockerhub_cred')
    } 
   
   tools {
     maven 'maven397'
     nodejs "nodejs2241"	 
   }
   
   stages {

   
     stage('SCM') {
       steps {
         // git branch: 'main', url: 'https://github.com/abbassizied/angular-cicd-k8s.git'
         checkout scmGit(branches: [
           [name: '*/main']
         ], extensions: [], userRemoteConfigs: [
           [url: 'https://github.com/abbassizied/angular-cicd-k8s']
         ])
         echo 'Git Checkout Completed'
       }
     }


     stage('Build') {
       steps {
         sh 'npm install ./angular-demo'
         // sh 'npm run sonar ./angular-demo'
       }
     }

     stage('Testing Stage') {
       steps {
         sh 'ng test --no-watch --no-progress --browsers=ChromeHeadless ./angular-demo'
         // sh 'npm run sonar-scanner ./angular-demo'
         // sh 'npm run sonar ./angular-demo'
       }
     }

     stage('Sonarqube Analysis - Angular') {
       steps {
         withSonarQubeEnv(installationName: 'sq1') {
           sh 'sonar-scanner ./angular-demo'
         }
       }
     }


    stage('quality gate') {
        steps {
            waitForQualityGate abortPipeline: false, credentialsId: 'sq1'
        }
    }

 
        // ##################################################
        // ### Docker
        // ##################################################	    

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${IMG_NAME} .'
                sh 'docker tag ${IMG_NAME} ${DOCKERHUB_CREDENTIALS_USR}/${IMG_NAME}:${BUILD_NUMBER}'
                echo 'Docker image built successfully!'
            }
        }


        stage('Push Image') {
            steps {
                echo 'Logon in to docker hub'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin docker.io'
                echo 'Login Successfull'
                sh 'docker push ${DOCKERHUB_CREDENTIALS_USR}/${IMG_NAME}:${BUILD_NUMBER}'
                echo 'Docker pushed successfully!'
                sh 'docker logout' 
            }
        }


    // ##################################################################################################  
     stage('What\'s next?') {
       steps {
         echo 'What\'s next?'
       }
     }
   }
 }