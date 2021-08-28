pipeline {
   agent { label 'GOL' }
   triggers {
       cron('H * * * *')
       pollSCM('* * * * *')
       
   }
    stages {
        stage('SCM') {
            steps {
                //sh 'GIT URL is ${GIT_URL}'
                // Get some code from a GitHub repository
                git branch : 'master', url : 'https://github.com/sreekavya1098/game-of-life.git'
  
            }
        }
        stage('Build') {
            steps {
                echo env.GIT_URLi
                sh 'mvn clean package'
            }
        } 
    }   
        post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    //junit '**/TEST-*.xml'
                    archive '**/*.war'
            }
               
        }
}
