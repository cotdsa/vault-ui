node ('master'){
  stage 'Checkout'
  checkout scm
  // https://issues.jenkins-ci.org/browse/JENKINS-31924
  //sh('git submodule sync')
  //sh('git submodule update --init')
  sh('git rev-parse HEAD > GIT_COMMIT')
  def git_commit = readFile('GIT_COMMIT')
  def short_commit = git_commit.take(12)

  stage 'Build Docker Image'
  sh "sed -i 's/COMMIT_ID/${short_commit}/g' kubernetes.yml"
  sh 'cat kubernetes.yml'
  sh "docker build -t cgws-private/vault-ui:${short_commit} ."

  stage 'Tag and Deploy Docker Image'
  sh "docker tag cgws-private/vault-ui:${short_commit} cgws-private/vault-ui:latest"
  sh 'kubectl apply -f kubernetes.yml'
 }
