node(){
    git 'git@github.com:roachmaster/raspberry-pi-mariadb-deployment.git'

    stage("Create Secret"){
        withCredentials([usernamePassword(credentialsId: '8047ae57-cfa7-4ee1-86aa-be906b124593', passwordVariable: 'credPw', usernameVariable: 'credName')]) {
            k3sSecret name:"mysql-pass", credPw: "${credPw}"
        }
    }

    stage("Create ConfigMap"){
        k3sConfigMap name: "mysql-initdb-config"
    }

    stage("Create PVC"){
        k3sPvc name: "mariadb-pv-claim"
    }

    stage("Create Deployment"){
        k3sDeployment name: "pi-mariadb"
    }

    stage("Create Service"){
        k3sService name: "pi-mariadb"
    }
}
