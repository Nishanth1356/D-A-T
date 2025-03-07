pipeline {
    agent any

    tools{
        maven 'maven'
    }

     stages {
        stage("Pull SRC") {
            steps {
                git branch: 'main', url: 'https://github.com/Nishanth1356/nishanth1.git'
            }
        }
        stage("move the target") {
            steps {
                sh 'mv target/nishanth1.war .'
            }
        }

        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Create image'){
            steps{
                sh 'docker build -t app .'
            }
        }
        stage('Assign tag'){
            steps{
                sh 'docker tag app nishanth321/dock1'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "nishanth321dh" | docker login -u "nishanth321" --password-stdin'
                sh 'docker push nishanth321/dock1'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f nishanth321/dock1'
            }
        }
        stage("run ansible playbook") {
            steps {
                sshPublisher(
                    continueOnError: false, 
                    failOnError: true,
                    publishers: [
                        sshPublisherDesc(
                            configName: "remote",
                            transfers: [
                                sshTransfer(sourceFiles: 'play.yml'),
                                sshTransfer(execCommand: "ansible-playbook play.yml")
                            ],
                            verbose: true
                        )
                    ]
                )
            }
        }
    }
}