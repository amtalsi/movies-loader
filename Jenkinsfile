def imageName = 'mlabouardy/movies-loader'
def registry = 'https://hub.docker.com/repositories/am22d35n'

pipeline{
    agent{
        label 'workers'
    }

    stages{
        stage('Checkout'){
            steps{
                checkout scm
            }
        }

        stage('Unit Tests'){
            steps{
                script {
                    def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
                    imageTest.inside{
                        sh "python test_main.py"
                    }
                }
            }
        }

        stage('Build'){
            steps{
                script {
                    docker.build(imageName)
                }
            }
        }

        stage('Push') {
        steps {
            script { // Ajoutez ce bloc script
                docker.withRegistry(registry, 'registryCredentials') {
                    def builtImage = docker.image(imageName)
                    builtImage.push(commitID())

                    if (env.BRANCH_NAME == 'develop') {
                        builtImage.push('develop')
                    }
                }
            }
        }
    }
}

def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}
