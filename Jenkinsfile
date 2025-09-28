pipeline {
    agent any
    environment {
    WEBSERVER = "Apache"
    }
    
    stages {
        stage('Create web directory')
        {
            input {
              message 'Enter the data'
              parameters {
                    string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment ')
                    string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
                 }
            }
            steps{
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }

        //apache
        stage('Create the Apache httpd container and deploy') {
            steps {
            when {
                environment name: 'WEBSERVER', value: 'Apache'
            }
            echo 'Creating the container...'
            sh 'docker run -dit --name apache1 -p 9000:80  -v /var/lib/jenkins/workspace/Pipe_despliegue/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        //nginx
        stage('Create the Nginx container and deploy') {
            steps {
            when {
                environment name: 'WEBSERVER', value: 'Nginx'
            }
            echo 'Creating the container...'
            sh 'docker run -dit --name apache1 -p 9000:80  -v /var/lib/jenkins/workspace/Pipe_despliegue/web:/usr/share/nginx/html nginx'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9000'
            }
        }
    }

    post {
        always{
            echo 'These steps are always executed'
            
        }
        success {
            echo 'Deployment succesfully'
            archiveArtifacts artifacts: 'web/*.html', followSymlinks: false
            cleanWs()
        }
        failure {
            echo 'CRITICAL ERROR'
        }
    }
}

