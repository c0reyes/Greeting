node('docker') {
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'maven:3-alpine' 
                    args '-v /root/.m2:/root/.m2' 
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
            agent {
                docker {
                    image 'openjdk:8' 
                }
            } 
            steps {
                script{
                    unstash 'jar'
                    docker.withRegistry('https://registry:5000') {
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
