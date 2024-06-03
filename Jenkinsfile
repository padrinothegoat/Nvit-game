node{
    checkout scm
    if (env.BRANCH_NAME ==~ /(develop|master|main)/) {
        
        stage('Build image ') {
            ms1 = docker.build("fourth-memento-307608/test","./cicd/Dockerfile")
        }
        
        if (env.BRANCH_NAME ==~ /(develop)/ )  {
            stage('Push image develop') {
                docker.withRegistry('https://eu.gcr.io', 'gcr:google-container-registry') {
                    ms1.push("latest")
                }
            }
        }

        if (env.BRANCH_NAME ==~ /(master)/ )  {
            stage('Push image master') {
                docker.withRegistry('https://eu.gcr.io', 'gcr:google-container-registry') {
                    ms1.push("${env.BUILD_NUMBER}")
                }
            }
        }
        
    }
}