pipeline {
    agent none

    stages {
        stage('Prepare'){
            agent {
                node{
                    label "linux && java17"
                }
            }
            steps{
                echo "Start Job : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_NUMBER}"
                echo "Branch Name : ${env.BRANCH_NAME}"
            }
        }

        stage('Build') {
            agent {
                node{
                    label "linux && java17"
                }
            }
            steps {
                script{
                    for (int i = 0; i < 10; i++) {
                        echo("script ${i}")
                    }
                }

                echo 'Start build'
                sh("./mvnw clean compile test-compile")
                echo 'Finish build'
            }
        }

        stage('Test') {
            agent {
                node{
                    label "linux && java17"
                }
            }
            steps {
                script{
                    def data = [
                        "firstName":"Febrari",
                        "lastName":"Supaldi"
                    ]

                    writeJSON(file:"data.json",json: data)
                }

                echo 'Start Test'
                sh("./mvnw test")
                echo 'Finish Test'
            }
        }

        stage('Deploy') {
            agent {
                node{
                    label "linux && java17"
                }
            }
            steps {
                echo 'Hello Deploy 1'
                sleep(5)
                echo 'Hello Deploy 2'
                echo 'Hello Deploy 3'
            }
        }
    }

    post{
        always{
            echo "i will always say Hello again"
        }

        success{
            echo "yay, success"
        }

        failure{
            echo "Oh no, failure"
        }

        cleanup{
            echo "Dont't care success or error"
        }
    }
}
