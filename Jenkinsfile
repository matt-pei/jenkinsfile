#!groovy

@Library("jenkinslib") _

String workspace = "/app/.jenkins/workspace"

def tool = new org.devops.tools()

pipeline{
    agent any
    options{
        timeout(time: 5, unit: "MINUTES")
    }

    stages {
        // 拉取代码
        stage('CheckOut'){  // 阶段
            when { environment name: 'test', value: 'aaacc' }
            steps { // 步骤
                timeout{time: 3, unit: 'MINUTES'}
                script{
                    println('拉取代码')
                    println("${test}")

                    input id: 'Test', message: '是否继续?', ok: '继续', parameters: [choice(choices: ['a', 'b'], description: '', name: 'test1')], submitter: 'matt.pei'
                }
            }
        }

        stage('001'){
            // failFast true
            parallel {
                stage('Build'){
                    setps{
                        timeout(time: 2, unit: 'MINUTES'){
                            script{
                                println("应用打包")
                                tools.PrintMes("应用打包",'green')
                                mvnhome = tool "m2"
                                println(mvnhome)

                                sh "${mvnhome}/bin/mvn --version"
                            }
                        }
                    }
                }

                // 代码扫描
                stage('CodeScan'){
                    setps{
                        timeout(time: 2, unit: 'MINUT'){
                            script{
                                println("代码扫描")
                                tools.PrintMes("代码扫描",'green')
                            }
                        }
                    }
                }
            }
        }
    }

    // 构建后操作
    post{
        alway {
            script{
                println("always")
            }
        }
        
        success {
            script{
                currentBuild.description = "\n 构建成功"
            }
        }

        failure {
            script {
                currentBuild.description = "\n 构建失败"
            }
        }

        aborted {
            script {
                currentBuild.description = "\n 构建取消"
            }
        }
    }

}



