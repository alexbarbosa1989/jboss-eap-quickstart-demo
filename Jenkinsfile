node('maven') {
  // define commands
  def mvnCmd = "mvn"
  // injection of environment variables is not done so set them here...
  def sourceRef = "master"
  def sourceUrl = "https://github.com/alexbarbosa1989/jboss-eap-quickstart-demo"
  def devProject = "eap-demo"
  def applicationName = "eap-demo"

  stage 'build'
    git branch: sourceRef, url: sourceUrl
    sh "${mvnCmd} clean install -DskipTests=true"
  stage 'test'
    sh "${mvnCmd} test"
  stage 'deployInDev'
    sh "rm -rf oc-build && mkdir -p oc-build/deployments"
    sh "cp target/kitchensink-angularjs.war oc-build/deployments/ROOT.war"
    // clean up. keep the image stream
    sh "oc project ${devProject}"
    sh "oc delete bc,dc,svc,route -l application=${applicationName} -n ${devProject}"
    // create build. override the exit code since it complains about existing imagestream
    sh "oc new-build --name=${applicationName} --image-stream=registry.access.redhat.com/jboss-eap-7/eap70-openshift --binary=true --labels=application=${applicationName} -n ${devProject} || true"
    // build image
    sh "oc start-build ${applicationName} --from-dir=oc-build --wait=true -n ${devProject}"
    // deploy image
    sh "oc new-app ${applicationName}:latest -n ${devProject}"
    sh "oc expose svc/${applicationName} -n ${devProject}"
}
