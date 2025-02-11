pipeline {
    agent { label 'GOL'}
    triggers {
        cron('H * * * *')
        pollSCM('* * * * *')
    }
    parameters {
        string(name: 'BRANCH', defaultValue: 'master', description: 'Branch to build' )
        choice(name: 'GOAL', choices: ['package', 'clean package', 'install'], description: 'maven goals')
    }
    options {
        timeout(time: 1, unit: 'HOURS')
        retry(2)
    }
    environment {
        CI_ENV = 'DEV'
    }
    stages {
        stage('scm') {
            environment {
                DUMMY = 'FUN'
            }
            steps {
                //mail subject: 'BUILD Started '+env.BUILD_ID, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
                git branch: "${params.BRANCH}", url: 'https://github.com/sreekavya1098/game-of-life.git'
                //input message: 'Continue to next stage? ', submitter: 'qtaws,qtazure'
                echo env.CI_ENV
                echo env.DUMMY
            }
        }
        stage('build') {
            steps {
                echo env.GIT_URL
                timeout(time:10, unit: 'MINUTES') {
                    sh "mvn ${params.GOAL}"
                }
                stash includes: '**/gameoflife.war', name: 'golwar'
                
            }
        }
        stage('devserver'){
            agent { label 'UBUNTU'}
            steps {
                unstash name: 'golwar'
            }
        }
    }
    post {
        success {
            archive '**/gameoflife.war'
            junit '**/TEST-*.xml'
           // mail subject: 'BUILD Completed Successfully '+env.BUILD_ID, to: 'devops@qt.com', from: 'jenkins@qt.com', body: 'EMPTY BODY'
        }

        always {
            echo "Finished"
        }
        
    }
}