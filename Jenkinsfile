pipeline {
    agent none
    stages {
        stage('Building package') {
            agent {
                docker {
                    image 'maven:3-alpine'
                    args '-v /root/.m2:/root/.m2'
                    label 'docker'
                }
            }
            steps {
                sh 'mvn -B clean package'
                stash name: "jar", includes: "target/**/*.jar"
            }
            post {
                success {
                    archiveArtifacts artifacts: 'target/**/*.jar', fingerprint: true
                }
            }
        }
        stage('Building image') {
            agent { label 'docker' }
            steps {
                script {
                    unstash 'jar'
                    docker.withRegistry('https://registry:5043') {
                        def customImageLatest = docker.build("greeting:latest")
                        customImageLatest.push()
                    }
                }
            }
        }
        stage('Deploy image') {
            agent { label 'docker' }
            steps {
                withKubeConfig([credentialsId: 'kubeconfig']) {
                    sh 'kubectl apply -f deployment.yml'
                    sh 'kubectl apply -f service.yml'
                    sh 'kubectl apply -f ingress.yml'
                }
            }
        }
        stage('Remove Unused docker image') {
            agent { label 'docker' }
            steps{
                sh "docker rmi registry:5043/greeting:latest"
            }
        }
    }
}
