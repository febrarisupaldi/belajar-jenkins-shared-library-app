pipeline {
    agent {
        node{
            label "linux && java17"
        }
    }

    stages {
        stage('Hello') {
            steps {
                echo 'Hello Pipeline'
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
