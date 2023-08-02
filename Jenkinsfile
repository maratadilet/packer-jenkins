template = '''
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: packer
  name: packer
spec:
  containers:
  - command:
    - sleep
    - "3600"
    image: hashicorp/packer
    name: packer
'''


def buildNumber = env.BUILD_NUMBER


podTemplate(cloud: 'kubernetes', label: 'packer', showRawYaml: false, yaml: template){
node("packer"){
container("packer"){
stage("Clone repo"){
git branch: 'main', url: 'https://github.com/maratadilet/packer-jenkins.git'
}


withCredentials([usernamePassword(credentialsId: 'aws-creds', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
stage("Packer version"){
sh "packer build -var 'jenkins_build_number=${buildNumber}' ./packer.pkr.hcl"
}


}
}
}
}
