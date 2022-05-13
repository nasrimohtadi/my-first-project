pipeline{
    agent {
       docker { 
           image 'trion/ng-cli' 
       }     
    }
    environment {
         def BUILDVERSION = sh(script: "echo `date +%F-%T`", returnStdout: true).trim()
    }
    stages {
        stage('Checkout') { 
         agent any 
          steps { 
               deleteDir() 
               checkout scm 
          } 
       } 
        stage('Install Node Module'){
           steps { 
               sh 'npm install'  
           }    
        }
        stage('Test'){
            steps { 
                echo "Testing..."
           } 
        }
        stage('Build'){
            steps { 
               sh 'ng build' 
               sh 'ls' 
               
            } 
        }
         stage('Archive'){
            steps { 
               sh 'tar -cvzf ng_project.tar.gz --strip-components=1 dist' 
               archive 'ng_project.tar.gz'
               
            } 
        }
        
        stage('Nexus') {

            agent none 
                when {
                    expression {
                        currentBuild.result == null || currentBuild.result == 'SUCCESS' 
                    }
                }
            
                steps {     
                     withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'nexus_manven_user',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                        echo "Nexus Connected..."
                        sh 'curl -v -u ${USERNAME}:${PASSWORD} --upload-file ng_project.tar.gz http://artefact.focus.com.tn:8081/repository/webbuild/com/ng_project/manifests/$BUILDVERSION.$BUILD_ID-RELEASE/ng_project.tar.gz' 
                        sh 'curl -v -u ${USERNAME}:${PASSWORD} --upload-file ng_project.tar.gz http://artefact.focus.com.tn:8081/repository/webbuild/com/ng_project/tags/latest/ng_project.tar.gz'         
                     } 
                } 
        }
        
    }
}