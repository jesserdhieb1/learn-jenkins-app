pipeline {
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
    }
}
