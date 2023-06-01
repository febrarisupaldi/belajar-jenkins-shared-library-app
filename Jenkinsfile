pipeline {
    agent none
    environment {
        AUTHOR = "Febrari Supaldi"
        EMAIL = "febrarisupaldi@gmail.com"
        WEB = "https://www.febrarisupaldi.com"
    }
    options{
        disableConcurrentBuilds()
        timeout(time:10, unit:'MINUTES')
    }

    // triggers{
    //     cron("*/5 * * * *")

    //     pollSCM("*/5 * * * *")
    //     upstream(upstreamProjects:'job1,job2', threshold: hudson.model.Result.SUCCESS)
    // }
    

    parameters{
        string(name:'NAME', defaultValue:'Guest', description:'What is your name ?')
        text(name:'DESCRIPTION', defaultValue:'Guest', description:'Tell me about you')
        booleanParam(name:'DEPLOY', defaultValue:false, description:'Need to Deploy?')
        choice(name:'SOCIAL_MEDIA', choices:['Instagram', 'Facebook', 'Twitter'], description:'Which social media')
        password(name:'SECRET', defaultValue:'', description:'Encryot Key')
    }
    
    stages {
        stage("OS Setup"){
            matrix{
                
                axes{
                    axis{
                        name "OS"
                        values "linux", "windows", "mac"
                    }

                    axis{
                        name "ARC"
                        values "32", "64"
                    }
                }
                excludes{
                    exclude{
                        axis{
                            name "OS"
                            values "mac"
                        }
                        axis{
                            name "ARC"
                            values "32"
                        }
                    }
                }
                stages{
                    stage("OS Setup"){
                        agent {
                            node{
                                label "linux && java17"
                            }
                        }

                        steps{
                            echo "Setup ${OS} ${ARC}"
                        }
                    }
                }
            }

            
        }
        stage("Preparation"){
            parallel{
                stage("Prepare Java"){
                    agent {
                        node{
                            label "linux && java17"
                        }
                    }
                    steps{
                        echo "Prepare Java"
                        sleep(5)
                    }
                }

                stage("Prepare Maven"){
                    agent {
                        node{
                            label "linux && java17"
                        }
                    }
                    steps{
                        echo "Prepare Maven"
                        sleep(5)
                    }
                }
            }
        }

        stage('Prepare'){
            environment{
                APP = credentials("paldi_rahasia")
            }
            agent {
                node{
                    label "linux && java17"
                }
            }
            steps{
                echo "Author : ${AUTHOR}"
                echo "Email  : ${EMAIL}"
                echo "WEB : ${WEB}"
                echo "Start Job : ${env.JOB_NAME}"
                echo "Start Build : ${env.BUILD_NUMBER}"
                echo "Branch Name : ${env.BRANCH_NAME}"
                echo "App User  : ${APP_USR}"
                echo "App Password : ${APP_PSW}"
                sh('echo "App Password : $APP_PSW" > "rahasia.txt"')
            }
        }

        stage('Parameter'){
            agent {
                node{
                    label "linux && java17"
                }
            }

            steps{
                echo "Hello ${params.NAME}"
                echo "You are ${params.DESCRIPTION}"
                echo "Need to deploy ${params.DEPLOY}"
                echo "Your social media is ${params.SOCIAL_MEDIA}"
                echo "Your secret ${params.SECRET}"
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
            input{
                message "Can we deploy?"
                ok "Yes, of course"
                submitter "paldi"
                parameters{
                    choice(name:"TARGET_ENV", choices:['DEV', 'QA', 'PROD'], description: "Which Environment ?")
                }
            }

            agent {
                node{
                    label "linux && java17"
                }
            }
            steps {
                echo "deploy to ${TARGET_ENV}"
            }
        }

        stage('Release'){
            
            when {
                expression{
                    return params.DEPLOY
                }
            }

            agent {
                node{
                    label "linux && java17"
                }
            }

            steps{
                withCredentials([
                    usernamePassword(
                        credentialsId:"paldi_rahasia",
                        usernameVariable:"USER",
                        passwordVariable:"PASSWORD"
                    )
                ]){
                    sh('echo "Release it with -u $USER -p $PASSWORD" > "release.txt"')
                }
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
