@Library("belajar-jenkins-shared-library@main") _

import programmerzamannow.jenkins.Output;

pipeline{
    agent any
    stages{
        
        stage("Global Variable"){
            steps{
                script{
                    echo(author())
                    echo(author.name())
                    echo(author.channel())
                }
            }
        }

        stage("Maven Compile"){
            steps{
                script{
                    maven(["clean","compile","test"])
                }
            }
        }

        stage("Hello Person"){
            steps{
                script{
                    hello.person([
                        firstName:"Febrari",
                        lastName:"Supaldi"
                    ])
                }
            }
        }

        stage("Hello Groovy"){
            steps{
                script{
                    Output.hello(this, "Groovy")
                }
            }
        }

        stage("Hello World"){
            steps{
                script{
                    hello.world()
                }
            }
        }
    }
}