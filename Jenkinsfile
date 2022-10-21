pipeline{
    agent{
        docker {
            image 'maven:3.8.1-adoptopenjdk-11'
            args '-v /root/.m2:/root/.m2'
        }
    }
    options {
        skipStagesAfterUnstable()
    }
    stages{
        stage("Build"){
            steps{
                echo "========Suba - executing Build stage========"
                sh 'mvn -B -DskipTests clean package'
            }
        }

        stage("Test"){
            steps{
                echo "====Suba - ++++executing Test++++===="
                sh "mvn test"
            }
            post{
                always{
                    junit "target/surefire-reports/*.xml"
                }
                success{
                    echo "====Suba - ++++Test executed successfully++++===="
                }
                failure{
                    echo "====++++Test execution failed++++===="
                }
        
            }
        }

        stage("Sonarqube Analysis"){
            steps{
                echo "====++++Suba.i am executing sonar executing A++++===="
                withSonarQubeEnv('SonarQube') {
                sh 'mvn sonar:sonar'
            }
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====++++A executed successfully++++===="
                }
                failure{
                    echo "====++++A execution failed++++===="
                }
       
            }
        }

        stage("Deploy"){
            steps{
                echo "====Suba - ++++executing Deploy++++suba===="
                sh './jenkins/scripts/deliver.sh'
            }
            post{
                always{
                    echo "====++++always++++===="
                }
                success{
                    echo "====Suba - ++++Deploy executed successfully++++===="
                }
                failure{
                    echo "====Suba - ++++Deploy execution failed++++===="
                }
        
            }
        }
    }
}

