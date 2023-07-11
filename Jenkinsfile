pipeline {
	    agent any
	

	        // Environment Variables
	        environment {
	        MAJOR = '1'
	        MINOR = '0'
	        //Orchestrator Services
	        UIPATH_ORCH_URL = "https://cloud.uipath.com/"
	        UIPATH_ORCH_LOGICAL_NAME = "uipathlucian"
	        UIPATH_ORCH_TENANT_NAME = "DefaultTenant"
	        UIPATH_ORCH_FOLDER_NAME = "Shared"
	    }
	

	    stages {
	

	        // Printing Basic Information
	        stage('Preparing'){
	            steps {
	                echo "Jenkins Home ${env.JENKINS_HOME}"
	                echo "Jenkins URL ${env.JENKINS_URL}"
	                echo "Jenkins JOB Number ${env.BUILD_NUMBER}"
	                echo "Jenkins JOB Name ${env.JOB_NAME}"
	                echo "GitHub BranhName ${env.BRANCH_NAME}"
	                checkout scm
	

	            }
	        }
	

	         // Build Stages
	        stage('Build') {
	            steps {
	                echo "Building..with ${WORKSPACE}"
	                UiPathPack (
	                      outputPath: "Output\\${env.BUILD_NUMBER}",
	                      projectJsonPath: "project.json",
			 credentials: ExternalApp(accountForApp: 'uipathLucian', applicationId: 'a2a58103-8d15-40f2-aa2c-f994c2da7a23', applicationScope: 'OR.Settings.Read OR.Robots.Read OR.Machines.Read OR.Execution OR.Assets OR.Jobs OR.Users.Read OR.Monitoring OR.Folders OR.BackgroundTasks OR.TestSets OR.TestSetExecutions OR.TestSetSchedules', applicationSecret: 'jenk', identityUrl: ''),
	                      version: [$class: 'ManualVersionEntry', version: "${MAJOR}.${MINOR}.${env.BUILD_NUMBER}"],
	                      useOrchestrator: false,
						  traceLevel: 'None'
	        )
	            }
	        }	

	         // Deploy Stages
 stage('Insstalling platform') { 
	 	steps {
                    echo "Installing WIN_22.10.8438.32859"
                    UiPathInstallPlatform(
                    cliVersion: 'WIN_22.10.8438.32859',
			traceLevel: 'None'
		)
	            }
 }
		    
 stage('Deploy to Automation Cloud') {
                steps {
                    echo "Deploying ${BRANCH_NAME} to UAT "
                    UiPathDeploy (
                    packagePath: "Output\\${env.BUILD_NUMBER}",
		    createProcess: true,
                    orchestratorAddress: "${UIPATH_ORCH_URL}",
                    orchestratorTenant: "${UIPATH_ORCH_TENANT_NAME}",
                    folderName: "${UIPATH_ORCH_FOLDER_NAME}",
                    environments: 'Shared',
            credentials: ExternalApp(accountForApp: 'uipathLucian', applicationId: 'a2a58103-8d15-40f2-aa2c-f994c2da7a23', applicationScope: 'OR.Settings.Read OR.Robots.Read OR.Machines.Read OR.Execution OR.Assets OR.Jobs OR.Users.Read OR.Monitoring OR.Folders OR.BackgroundTasks OR.TestSets OR.TestSetExecutions OR.TestSetSchedules', applicationSecret: 'jenk', identityUrl: ''),
                    traceLevel: 'None',
                    entryPointPaths: 'Main.xaml'

	

	        )
	            }
	        }
	

	

	         // Deploy to Production Step

	    }
	

	    // Options
	    options {
	        // Timeout for pipeline
	        timeout(time:80, unit:'MINUTES')
	        skipDefaultCheckout()
	    }
	

	

	    // 
	    post {
	        success {
	            echo 'Deployment has been completed!'
	        }
	        failure {
	          echo "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.JOB_DISPLAY_URL})"
	        }
	        always {
	            /* Clean workspace if success */
	            cleanWs()
	        }
	    }
	

	}
