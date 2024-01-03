def imageName = 'am22d35n'
def registry = 'https://registry-1.docker.io/'


node('workers'){
    stage('Checkout'){
        checkout scm
    }

    stage('Unit Tests'){
        def imageTest= docker.build("${imageName}-test", "-f Dockerfile.test .")
        sh "docker run --rm -v $PWD/reports:/app/reports ${imageName}-test"
    }

    stage('Build'){
        docker.build(imageName)
    }

stage('Push') {
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


def commitID() {
    sh 'git rev-parse HEAD > .git/commitID'
    def commitID = readFile('.git/commitID').trim()
    sh 'rm .git/commitID'
    commitID
}
