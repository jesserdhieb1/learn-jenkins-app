pipeline {

    environment{
        NETLIFY_SITE_ID = '5ce57b2a-6a9d-4faf-89ad-3499c16bf6b3'
        NETLIFY_AUTH_TOKEN = credentials ('Jenkins-netlify')
    }

    agent any

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                // we use npm ci for the dependecies installation to make the pipeline delete the node_modules file (each time) and do the installation from the package-lock.json file only 
                sh'''
                ls -la
                node --version
                npm --version
                npm ci 
                npm run build
                ls -la
                '''
            }
        }
        stage('Test'){
             agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh'''
                echo "testing stage"
                test -f build/index.html
                npm run test
                '''
            }
        }
        stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh'''
                    npm ci 
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "Deploying into production within the site id : $NETLIFY_SITE_ID"
                    echo "Deploying into production within the site id : $NETLIFY_AUTH_TOKEN"
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
