# ingress-nginx-test Project
## Purpose
The purpose of the prototype project is to demonstrate the configuration and use of ingress-nginx  within the minikube environment. The tests include http and https terminated at the ingress controller. Cert-manager is
used to manage self-signed certificates for the TLS.

## Projects Included
The projects include the following: 
   a) fruit-deploy - Deploys all components to a minikube environment. 
   b) passionfruit - Deploys a simple pekko-http server with an http ingress. 
   c) papaya       - A simple pekko-http tls server with tls termination on the ingress.

## Project Components
The main components of the prototype project are: 
a) kubernetes via minikube 
b) cert-manager for certificate generation 
c) ingress-nginx for controller reverse proxy and ingress configuration management. 
d) apache pekko-http as an http server

## Deployment Assumptioons
The project deployment presumes the following:
    Clone fruit-deploy, passionfruit and papaya projects to the same directory.
    A running minikube environment - I have been using the following commands to start and restart minikube. 
       a) minikube delete 
       b) minikube start --cpus 4 --memory 12288 --vm-driver kvm2 --disk-size 100g --insecure-registry="192.168.39.0/24" 
       c) minikube addons enable dashboard 
       d) minikube addons enable ingress

    Linux machine. I use Ubuntu 20.04 There are several utility commands within deployKube.sh which are linux specific. These can easily be modified or removed for your operating system environment.

Runtime Instructions:
    Open the fruit-deploy/scripts/deployKube.sh file within a text editor. 
    Open a terminal to process the commands. 
    Modify the $PROTODIR env variable in the terminal to point to your source directory.
    Run the commands within the fruit-deploy/scripts/deployKube.sh separately.

At the end of the deployKube.sh file there is a section for curl commands to invoke various deployed capabilities. A brief summary of the capabilites are:
  a) Both apple and banana support a path based http ingress to an echo server, as well as a host based ingress for invoking the echo server. The configurations for these can be found in 
     1) Configuration - kube/apple-path.yaml Deployment Test - curl -kL http://$ipAddr/apple 
     2) Configuration - kube/apple-host.yaml Deployment Test - curl -kL http://apple.foo.com/apple 
     3) Configuration - kube/banana-path.yaml Deployment Test - curl -kL http://$ipAddr/banana 
     4) Configuration - kube/banana-host.yaml Deployment Test - curl -kL http://banana.foo.com/banana
  b) mango supports an https request with tls passthrough to the echo server and cert-manager certificate generation. 
     Configuration - kube/mango.yaml; tls - mango-tls-cert-issuer.yaml 
     Deployment Test - curl -kL https://mango.foo.com/mango
  c) passion supports an http request to a simple pekko-http server 
     Configuration - passionfruit.yaml 
     Deployment Test - curl -kL http://passion.foo.com/passion

d) papaya supports an https request to a simple pekko-http server with tls termination at the ingress, as well as cert-manager certificate generation. Configuration - papaya.yaml; secret - papaya-auth.yaml; pvc - papaya-pvc.yaml; tls - papaya-tls-cert-issuer.yaml Deployment Test - curl -kL https://papaya.foo.com/papaya
