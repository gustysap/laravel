pipeline {
    agent { label 'master' }
    environment {
        registry = "registry.batumbu.id/laravel"
        registryCredential = 'dockerregistry'
        serviceName= 'laravel'
        dockerImage = ''
        branchname = "master"
    }
    stages {
        stage("Clone Code") {
            steps {
                script {
                checkout scm: [$class: 'GitSCM', userRemoteConfigs: [[url: 'https://github.com/gustysap/laravel.git',
                credentialsId: 'DBIndo']], branches: [[name: "${branchname}" ]]],poll: false
                echo "current build number: ${env.BUILD_NUMBER}"
                env.HASH = sh(script: "echo \$(git rev-parse --short HEAD)",returnStdout: true).trim()
                echo env.HASH
                echo branchname
                env.VERSION = "${env.BUILD_NUMBER}-${branchname}-${env.HASH}"
                echo env.VERSION
                sh "sed -i '14c\\ARG version=$env.VERSION' Dockerfile"
                sh "cat Dockerfile"
                }
            }
        }
        stage("Building Image") {
            steps{
              script {
                  sh "export version=$env.VERSION"
                  echo version
                  dockerImage = docker.build registry + ":$env.VERSION"
              }
	   }
        }
    }
}

