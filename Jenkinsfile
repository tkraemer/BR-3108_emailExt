

timestamps {
    ansiColor('xterm') {

        /** keep builds/artifacts for 10 days */
        String keep10 = '10'
        /** keep up to 5 builds/artifacts */
        String keep5 = '10'

        properties([ disableConcurrentBuilds(),
                     pipelineTriggers([[$class: 'GitHubPushTrigger']]),
                     [$class  : 'BuildDiscarderProperty',
                        strategy: [$class: 'LogRotator', 
                        artifactDaysToKeepStr: keep10, 
                        artifactNumToKeepStr: keep5, 
                        daysToKeepStr: keep10, 
                        numToKeepStr: keep5]]])

        String n4jsNodeLabel = "ide"
        node(n4jsNodeLabel) {
            try {
                //get parameters provided in the environment or the local defaults

                String workspace = pwd()

                // change 1 2 3

                stage("PreBuild") {
                    //clean workspace
                 
                    step([$class: 'WsCleanup'])
               

                    //clone
                    timeout(time: 30, unit: 'MINUTES') {
                        checkout scm
                    }

                }

                stage("Fail or not"){
                    // throw new Exception("Breaking")
                    // error("Build failed because of this and that..")
                    currentBuild.result = 'FAILURE'
                }


                sendEmail("${env.JOB_NAME} (${env.BUILD_NUMBER}) succeeded", "${env.BUILD_URL} succeeded - ${env.JOB_NAME} (#${env.BUILD_NUMBER}).")
            } catch (exc) {
                sendEmail( "${env.JOB_NAME} (${env.BUILD_NUMBER}) failed", "${env.BUILD_URL} is failing - ${env.JOB_NAME} (#${env.BUILD_NUMBER}). The following exception was caught : \n ${exc.toString()}")
                //rethrow otherwise job will always be green
                throw exc
            } finally {
            //checkLogsForDownloads();
            }
        }
    }
}


/**
 * Sends email notification about job status based on the provided data.
 *
 * @param subject the subject of the email
 * @param body the body of the email
 */
@NonCPS
void sendEmail(String subject, String body) {
    echo "Figuring out mails"
    echo "   Subject with ${subject}"
    def mailTo = emailextrecipients( recipientProviders: [
                        [$class: 'CulpritsRecipientProvider'],
                        [$class: 'RequesterRecipientProvider']] )
    
    echo "Mails go to: ${mailTo}"
    echo "Number of mails: ${mailTo.size()}"

    
/*        emailext subject: subject,
                body: body,
                recipientProviders: [
                        [$class: 'CulpritsRecipientProvider'],
                        [$class: 'RequesterRecipientProvider']]
*/ 
}


