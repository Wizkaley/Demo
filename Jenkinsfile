node{
   // def root = tool name: '3.5.4', type: 'mvn'
     //String jdktool = tool name: "jdk8", type: 'hudson.model.JDK'
    String jdktool = tool name: "Java_8", type: 'hudson.model.JDK'
    def mvnHome = tool name: '3.5.4'

    /* Set JAVA_HOME, and special PATH variables. */
    List javaEnv = [
        "PATH+MVN=${jdktool}/bin:${mvnHome}/bin",
        "M2_HOME=${mvnHome}",
        "JAVA_HOME=${jdktool}"
    ]
    
     withEnv(javaEnv) {
    stage ('Initialize') {
        sh '''
            echo "PATH = ${PATH}"
            echo "M2_HOME = ${M2_HOME}"
        '''
    }
    
    ws("${HOME}/agent/jobs/${JOB_NAME}/builds/${BUILD_ID}/") {               
            
            stage('Initialize') {
               sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        
        stage('Checkout') {
                
                    git(
                        url: 'https://github.com/ShrutiMaske/Demo.git',
                        branch: "master"
                    )
                
            }


            withCredentials([usernamePassword(credentialsId: '9cf91d78-8760-4775-88b6-2185dba39103', 
                passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW', usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR')]) {
                def gitCommit = sh(returnStdout: true, script: "git rev-parse HEAD").trim()
                stage('Build') {
                   withEnv(["GIT_COMMIT=${gitCommit}",
                         'GIT_BRANCH=master',
                         "GIT_REPO=https://github.com/xunrongl/DemoDRA-1"]) {
                    try {
                         sh 'mvn package' 
                           // junit 'target/surefire-reports/**/*.xml'
                        // use "publishBuildRecord" method to publish build record
//publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", duration: 1, hostName: "local-dash.gravitant.net", serviceName: "Serve"
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"SUCCESS", hostName: "local-core-dash.gravitant.net", serviceName: "Serve"
  
                    }
                    catch (Exception e) {
//publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", duration : 11, hostName: "local-dash.gravitant.net", serviceName: "Serve"
publishBuildRecord gitBranch: "${GIT_BRANCH}", gitCommit: "${GIT_COMMIT}", gitRepo: "${GIT_REPO}", result:"FAIL", hostName: "local-core-dash.gravitant.net", serviceName: "Serve"
  
                    }
                    
                    
            }
                    
            }
             
}
        withCredentials([usernamePassword(credentialsId: '897c1b2f-83d8-4dda-86bc-780f7b2fef23', 
                passwordVariable: 'IBM_CLOUD_DEVOPS_CREDS_PSW', usernameVariable: 'IBM_CLOUD_DEVOPS_CREDS_USR')]) {

                    stage('Unit Test and Code Coverage') {
                  
                    // use "publishTestResult" method to publish test result
//publishTestResult type:'unit', fileLocation: '/var/jenkins_home/workspace/Jenkins-Github/simpleTest.json'
                    publishTestResult fileLocation: 'target/surefire-reports/', type: "unit", serviceName: "Serve", hostName: "local-core-dash.gravitant.net", resultType: "junit"
                } 
                }
    }
}
}
