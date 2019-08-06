node{
    def root = tool name: 'Maven1', type: 'java'
    
    ws("${HOME}/agent/jobs/${JOB_NAME}/builds/${BUILD_ID}/") {               

            stage('Initialize') {
               sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }


            withCredentials([$class: 'UsernamePasswordMultiBinding', credentialsId: '7ff51d39-65f2-4ae4-93b0-14505d18750e',
                          usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR', passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW']]) {
                stage('Build') {
                   withEnv(["GIT_COMMIT=${gitCommit}",
                         'GIT_BRANCH=master',
                         "GIT_REPO='GIT_REPO_URL_PLACEHOLDER' {
                    try {
                         sh 'mvn clean install' 

                        // use "publishBuildRecord" method to publish build record
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", duration: 1, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                    }
                    catch (Exception e) {
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", duration : 11, hostName: "local-dash.gravitant.net", serviceName: "Hello"
                    }
                        }  
                    }
            }
            }
}
