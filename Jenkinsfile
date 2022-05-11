pipeline{
    agent {
       docker { 
           image 'trion/ng-cli-karma:1.2.1' 
       }     
    }
    stages {
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
               echo "Build..." 
            } 
        }
        stage('Nexus') {
            agent none 
            steps { 
                withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'nexus_manven_user',usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD']]) {
                     echo "Nexus Connected..."
                    //sh 'curl -v -u ${USERNAME}:${PASSWORD} --upload-file dist.tar.gz http://artefact.focus.com.tn:8081/repository/webbuild/com/focuscorp/release-$BUILDVERSION/dist.tar.gz' 
                } 
            } 
        } 
    }
}