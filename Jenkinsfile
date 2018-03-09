#!/usr/bin/env groovy

/**
 * Sample Jenkinsfile for Jenkins2 Pipeline
 * from https://github.com/hotwilson/jenkins2/edit/master/Jenkinsfile
 * by wilsonmar@gmail.com
 */


try {
    node {
        stage('\u2776 Checkout from GitHub') {
            git credentialsId: 'RodGitHub', url: 'https://github.com/rodrigocmn/elk_arm_puppet.git'
            //checkout scm
            echo "\u2600 BUILD_URL=${env.BUILD_URL}"


            def workspace = pwd()
            echo "\u2600 workspace=${workspace}"
        }


        stage('\u2777 Validate Azure Deployment') {
            azureCLI commands: [[exportVariablesString: '',
                                 script: 'az group deployment validate \\\n' +
                                         '--resource-group IE1-ELK-R1-RG-01 \\\n' +
                                         '--template-file ARM_Templates/elk_sa_vm.json \\\n' +
                                         '--parameters @ARM_Templates/elk_sa_vm_parameters.json']],
                    principalCredentialId: 'GLB-PACKER-NONPROD-SP'
        }
    }
} // try end
catch (exc) {

    echo "Caught: ${exc}"

    String recipient = 'rodrigocmn@gmail.com'

    mail subject: "${env.JOB_NAME} (${env.BUILD_NUMBER}) failed",
            body: "It appears that ${env.BUILD_URL} is failing, somebody should do something about that",
            to: recipient,
            replyTo: recipient,
            from: 'noreply@ci.jenkins.io'

    /* Rethrow to fail the Pipeline properly */
    throw exc
}