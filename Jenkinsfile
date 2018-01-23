pipeline {
    agent any

	options {
	disableConcurrentBuilds()
	}
			
	environment {

	// Application Specific    
	NEXUS_ARTIFACTID="DemoNunit"
	NEXUS_IQ_STAGE="release"
	ARTIFACT_FILENAME="DemoNunit.zip"
	NEXUS_REPOSITORY="maven-central"
	NEXUS_GROUP="maven-public"
	TARGET_VERSION=''
	VERSION_TAG="v1.42"
	GIT_PROJECT="rahoodroshan/NunitDemo"
	}
	stages 
	{
		stage( 'Checkout Source' ) 
		{
		//Checkout source code from GIT
		    steps{
			git "https://github.com/rahoodroshan/game-of-life.git"
		    }
		}//End Checkout Source   
		stage ( 'complie' ){
		    steps{
		       withMaven( maven : 'localMaven' ){
		       bat 'mvn clean compile'
		    }
		   }
		}
		stage ('testing' ){
		    steps{
		        withMaven( maven : 'localMaven' ){
		        bat 'mvn test'
		       }
		    }
		}
		stage( 'IQ_Scan' ){
		     steps{
			nexusPolicyEvaluation failBuildOnNetworkError: false, 
				iqApplication: 'GameofLife_App', 
				iqScanPatterns: [[scanPattern:  '**/gameoflife-core.jar']], 
				iqStage: 'release', 
				jobCredentialsId: 'NexusIQCedentials'
			     
     			}
     		}
		stage( "Deploy" ){
		      steps{
				nexusArtifactUploader artifacts: [[artifactId: 'gameoflife-core', classifier: '', file: 'gameoflife-core/build/libs/gameoflife-core.jar', type: 'jar']],
				credentialsId: 'NexusRepoCredentials', 
				groupId: 'GameofLifeGroup', 
				nexusUrl: 'localhost:9091', 
				nexusVersion: 'nexus3', 
				protocol: 'http', 
				repository: 'GameofLifeRepo', 
				version: '1.0'
			}
		}
    	
	
	}
}
