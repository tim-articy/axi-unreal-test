pipeline {
    agent any

    parameters {
        string(name: 'BuildPlatforms', description: 'The build platforms to package the plugin', defaultValue: 'Win64')
        string(name: 'UnrealVersions', description: 'The Unreal Engine versions to package the plugin for, separated by space', defaultValue: '5.6 5.7 5.8')
    }

    environment {
        AXI_REPO_URL = "git@github.com:ArticySoftware/ArticyXImporterForUnreal.git"
    }

    stages {
        stage('Checkout') {
            steps {
                sh 'git clone $AXI_REPO_URL ArticyXImporter'
                sh 'cd ArticyXImporter'
                // This determines the latest tag
                sh '$VERSION = git describe --tags --abbrev=0'
                sh 'git checkout $VERSION'
                sh 'cd ..'
            }
        }

        stage('Package') {
            steps {
                sh 'unreal-plugin-pkg --versions ${params.UnrealVersions} --platforms ${params.BuildPlatforms} --out out/ --cleanBinaries ./ArticyXImporter'
                archiveArtifacts artifacts: 'out/*.zip', fingerprint: true, onlyIfSuccessful: true
            }
        }
    }
}
