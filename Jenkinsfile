node {
    // define commands 
    def mvnCmd = tool 'M3'
    // injection of environment variables is not done so set them here...
    def sourceRef = "master"
    def sourceUrl = "https://github.com/alexbarbosa1989/jboss-eap-quickstart-demo"
    def devProject = "cicd"
    def applicationName = "eap-demo"

    stage('Preparation') { // for display purposes
      // Get some code from a GitHub repository
      git branch: sourceRef, url: sourceUrl
    }
    stage('Build') {
      sh "${mvnCmd}/bin/mvn -f pom.xml clean install -DskipTests"

    }
    stage('Test') {
      sh "${mvnCmd}/bin/mvn -f pom.xml test"
    }
    stage('Deploy'){
      sh "rm -rf oc-build && mkdir -p oc-build/deployments"
      sh "cp kitchensink-angularjs/target/kitchensink-angularjs.war oc-build/deployments/ROOT.war"
      // clean up. keep the image stream
      sh "oc project ${devProject}"
      sh "oc delete bc,dc,svc,route -l application=${applicationName} -n ${devProject}"
      // create build. override the exit code since it complains about existing imagestream
      sh "oc new-build --name=${applicationName} --image-stream=wildfly --binary=true --labels=application=${applicationName} -n ${devProject} || true"
      // build image
      sh "oc start-build ${applicationName} --from-dir=oc-build --wait=true -n ${devProject}"
      // deploy image
      //sh "oc new-app ${applicationName}:latest -n ${devProject}"
      //sh "oc expose svc/${applicationName} -n ${devProject}"
    }
}
