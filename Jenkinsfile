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
                    sh "scp -o StrictHostKeyChecking=no server-script.sh ec2-user@172.31.36.146:/home/ec2-user"  
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@172.31.36.146 'bash server-script.sh'"  
                    echo "Packaging the code"
                    sh "mvn package"

                }
                }
                
            }

            
        }
    }
}
