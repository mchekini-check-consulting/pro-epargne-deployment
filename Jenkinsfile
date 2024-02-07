node("deploy-node-$environment") {

    stage("checkout") {
        checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mchekini-check-consulting/pro-epargne-deployment.git']])
    }

    stage("Deploy Pro-Epargne") {

        def path = ""
        withCredentials([string(credentialsId: 'VAULT_TOKEN', variable: 'VAULT_TOKEN')]) {

            dir("$env.workspace/$environment") {
                def files = findFiles(glob: '**/docker-compose.yml')
                for (file in files) {
                    path = file.path.split(file.name)[0]
                    dir("$env.workspace/$environment/$path") {
                        try {
                            sh "sudo docker-compose down"
                        } catch (Exception e) {
                            println "No Running Container"
                        } finally {
                            sh "echo VAULT_TOKEN=$VAULT_TOKEN > .env"
                            sh "sudo docker-compose up -d"
                            sh "sudo rm .env"
                        }
                    }
                }
            }
        }

    }

}