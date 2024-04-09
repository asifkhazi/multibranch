pipeline {
    agent any
    triggers {
        cron '* * * * *'
        pollSCM '* * * * *'
    }
    parameters {
        string(name: 'PERSON_NAME', defaultValue: 'SAM', description: 'Candidate details')
        booleanParam(name: 'AGE', defaultValue: true, description: 'Age is greater than 18 or not')
        choice(name: 'VEG_TYPE', choices: ['VEG', 'NON-VEG', 'MIXED'], description: 'Type of vegitarian')
    }
    environment {
        PERSON_NAME="${params.PERSON_NAME}"
        AGE="${params.AGE}"
        VEG_TYPE="${params.VEG_TYPE}"
        PASSWORD=credentials('PASSWORD')
        SSH_CRED=credentials('SSH_CRED')
        USER_CRED=credentials('USER_CRED')
    }
    stages {
        stage ('stage-1 and Agent-1 ') {
           // agent {label 'Agent-1'}
            steps {
                sh 'echo "Name of the candidate is ${PERSON_NAME}"'
                sh 'echo "Name of the job is ${JOB_NAME}"'
                sh 'echo "Build number of the job is :${BUILD_NUMBER}"'
            }
        }
        stage ('stage-2 and Agent-1') {
            //agent {label 'Agent-1'}
            steps {
              sh 'echo "Age of the candidate is greater than 18 ? :${AGE}"'
              sh 'echo "The URL of the job is : ${JOB_URL}"'
              sh 'echo "PASSWD CRED USER are :${PASSWORD_USR}"'
              sh 'echo "PASSWD CRED PASSWD are :${PASSWORD_PSW}"'
            }
        }
        stage ('stage-3 and Agent-2') {
            //agent {label 'Agent-2'}
            when {
                anyOf {
                    branch 'main'
                    branch 'dev'
                }
               equals expected: 'success', actual: currentBuild.result 
            }
            steps {
                sh 'echo "Candidate vegitarian type is : ${VEG_TYPE}"'
            }
        }
        stage ('stage-4 and agent-2') {
            //agent {label 'Agent-2'}
            when {
                equals expected: 'success', actual: currentBuild.result
            }
            steps {
                sh 'echo "BUILD_NUMBER: ${BUILD_NUMBER}"'
            }
        }
    }
    post {
        success {
            emailext body: 'Hi \nPlease find job details below \nJOB_NAME:${JOB_NAME}, BUILD_NUMBER: ${BUILD_NUMBER} \nJOB_URL: ${JOB_URL} \nBUILD_STATUS: ${BUILD_STATUS} \nThank you \nAsif Khazi', subject: 'Build Status', to: 'asifkhazi248@gmail.com'
        }
    }
}
