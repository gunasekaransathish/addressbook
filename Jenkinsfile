pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "mymaven"
    }

    stages {
        stage('Compile') {
            steps {
               echo "compiling teh code"
            }
        }
        stage('UnitTest') { // running on slave1
            //agent {label 'linux_slave'}
            agent any
            steps {
                script{
                    echo "RUNNING THE TC"
                    sh "mvn test"
                }
                }
            
            post{
                always{
                    junit 'target/surefire-reports/*.xml'
                }
            }
            
        }
        stage('Package') { // running on slave2 via ssh-agent
            agent any
            steps {
                script{
                    sshagent(['slave2']) {
                    echo "Packaging the code"
                    sh "mvn package"

                }
                }
                
            }

            
        }
    }
}
