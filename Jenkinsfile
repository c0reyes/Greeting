pipeline {
    agent none
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
                    label 'docker'
                }
            } 
            steps {
                sh 'mvn -B -DskipTests clean package' 
                stash name: "jar", includes: "target/**/*.jar"
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/**/*.jar', fingerprint: true
                }
            }
        }
        stage('Dockerfile') {
            agent { label 'docker' } 
            steps {
                script {
                    unstash 'jar'
                    docker.withRegistry('http://172.17.0.1:5043','registry') {
                        def customImage = docker.build("greeting:${env.BUILD_ID}")
                        customImage.push()
                        def customImageLatest = docker.build("greeting:latest")
                        customImageLatest.push()
                    }
                }
            }
            post{
                cleanup {
                    echo "Clean"
                    cleanWs()
                }
            }
        }
    }
}
