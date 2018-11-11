This project is oriented to academical purposes . Is an adaptation to the EAP quickstart project  that you can follow and contribute in the next URL https://github.com/jboss-developer/jboss-eap-quickstarts.

The present project adapt the pom structure to generate deployable wars in Openshift platform

The following steps could be applied to deploy the project in minishfit. (Tested in minishift v1.26.1+1e20f27 Openshift 3.11)

1- Start minishift
> minishift start

2- Create a project 
> oc new-project eap-demo

3- Create a new application into the project (In this case the deployed project is kitchensink-angularjs):
> oc new-app --name=eap-demo registry.access.redhat.com/jboss-eap-7/eap70-openshift~https://github.com/alexbarbosa1989/jboss-eap-quickstart-demo.git -p SOURCE_REPOSITORY_REF="master" -p CONTEXT_DIR="kitchensink-angularjs"

NOTE: If you want to deploy another module, just replate the name in CONTEXT_DIR parameter and update the modules section in parent POM.

4- Expose the service, in order to get acceded from web 
> oc expose svc eap-demo

5- Wait until finished building proces (This action could take several minutes until download all dependencies and build artifacts in the Source-to-Image process)

6- Enjoy!


Openshitf is very flexible with the methods to deploy an application. If want to know more about Openshift deployment methods, you can find more in the next links:

- https://blog.openshift.com/multiple-deployment-methods-openshift/
- https://docs.openshift.com/online/using_images/s2i_images/java.html
- https://blog.openshift.com/maven-multi-module-projects-and-openshift/


Tools installation guide:
- Minishift: https://docs.okd.io/latest/minishift/getting-started/installing.html
