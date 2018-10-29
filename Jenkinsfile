node {

    stage ('Checkout Repo') {
        deleteDir()
        checkout scm
    }

    stage ('setup environment') {
        sh 'python3 -m venv jenkins_build'
        sh 'jenkins_build/bin/python -m pip install setuptools wheel virtualenv --upgrade'
        sh 'jenkins_build/bin/python -m pip install -r requirements.txt'
        sh 'git clone https://github.com/carlniger/napalm-ansible'
        sh 'cp -r napalm-ansible/napalm_ansible/ jenkins_build/lib/python3.6/site-packages/'
        sh 'jenkins_build/bin/python napalm-ansible/setup.py install'
        sh '''sed -i -e 's/\\/usr\\/local/jenkins_build/g' ansible.cfg'''
        sh '''sed -i -e 's/dist-/site-/g' ansible.cfg'''
        sh '''sed -i -e 's/\\/usr\\/bin/jenkins_build\\/bin/g' ansible.cfg'''
    }

    stage ('validate playbook to render configurations') {
        sh 'ansible-playbook gen_configs.yaml -e "ansible_python_interpreter=jenkins_build/bin/python" --syntax-check'
    }

    stage ('Render Confs') {
        sh 'ansible-playbook gen_configs.yaml -e "ansible_python_interpreter=jenkins_build/bin/python"'
    }

    stage ('Unit testing') {
        sh 'ansible-playbook deploy_configurations.yaml -e "ansible_python_interpreter=jenkins_build/bin/python" --syntax-check'
        sh 'ansible-playbook deploy_configurations.yaml -e "ansible_python_interpreter=jenkins_build/bin/python" --syntax-check'
    }

    stage ('Deploy confs to DEV') {
        sh 'ansible-playbook deploy_configurations.yaml -e "ansible_python_interpreter=jenkins_build/bin/python"'
    }

    stage ('func/inte testing') {
        sh 'ansible-playbook validate_configs.yaml -e "ansible_python_interpreter=jenkins_build/bin/python"'
    }

    stage ('promote configs to PROD') {
        // comment
    }

    stage ('PROD func/inte testing') {
        // comment
    }

}

