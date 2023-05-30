pipeline {
    agent {
        node{
            label "linux && java17"
        }
    }

    stages {
        stage('Build') {
            steps {
                echo 'Start build'
                sh("./mvnw clean compile test-compile")
                echo 'Finish build'
            }
        }

        stage('Test') {
            steps {
                 echo 'Start Test'
                sh("./mvnw test")
                echo 'Finish Test'
            }
        }

        stage('Deploy') {
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
