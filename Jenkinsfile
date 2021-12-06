pipeline{
 agent none
 stages{
  stage('SCM'){
   agent { 
                docker { 
                    image 'alpine'
                } 
            }
   steps{
    echo 'Gathering code from version control'
    git branch : '${branch}', url: 'https://github.com/pbuczma/step3.git'
  
   }
  }
  stage('Build'){
   agent { 
       docker { 
           image 'mcr.microsoft.com/dotnet/sdk'
           // label 'maven-label'
       } 
   }
   environment {
      HOME = '/tmp'
   }
   steps{
    try{
      echo 'Building...'
      sh 'dotnet --version'
      sh 'cd ConsoleApp1'
      sh "dotnet build ConsoleApp1"
      echo 'Building New Feature'
      sh 'cd ..'
      releasenotes()
      
    }catch(ex){
      echo  'Something went wrong'
      echo  ex.toString();
      currentBuild.result = 'FAILURE'
   }
    finally{
      //cleanup
    }
   }
  }
  stage('Test'){
   steps{
    echo  'Testing...'
   }
  }
  stage('Deploy'){
   steps{
    echo 'Deploying...'
   }
  }
 }
}
