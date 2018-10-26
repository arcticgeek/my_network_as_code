node {

    stage ('Checkout Repo') {
        deleteDir()
        checkout scm
    }

    stage ('Render Confs') {
        sh 'ansible-playbook gen_configs.yaml'
    }

    stage ('Unit testing') {
        // comment
    }

    stage ('Deploy confs to DEV') {
        // comment
    }

    stage ('func/inte testing') {
        // comment
    }

    stage ('promote configs to PROD') {
        // comment
    }

    stage ('PROD func/inte testing') {
        // comment
    }

}

