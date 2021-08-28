pipeline {
   agent { label 'GOL' }
   triggers {
       cron('H * * * *')
       pollSCM('* * * * *')
       
   }
    stages {
        stage('SCM') {
            steps {
                mail subject : 'Build started' +env.BUILD_ID, to: 'sreekavya586@gmail.com', from: 'jenkins@build.com'
                git branch : 'master', url : 'https://github.com/sreekavya1098/game-of-life.git'
  
            }
        }
        stage('Build') {
            steps {
                echo env.GIT_URL
                timeout(time:10, unit: 'MINUTES'){
                sh 'mvn clean package'
                }
            }
        } 
    }   
        post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    //junit '**/TEST-*.xml'
                    archive '**/*.war'
                    mail subject : 'Build completed successfully' +env.BUILD_ID, to: 'sreekavya586@gmail.com', from: 'jenkins@build.com'
            }
            failure {
                mail subject : 'Build failed' +env.BUILD_ID + 'URL is '+env.BUILD_URL, to: 'sreekavya586@gmail.com', from: 'jenkins@build.com'
            }
               
        }
}
