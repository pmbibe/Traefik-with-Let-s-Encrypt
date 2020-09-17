# Traefik-with-Let-s-Encrypt
# Install Traefik with Helm 3  
- The first, you must install Helm 3  
   - curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3  
   - chmod 700 get_helm.sh  
   - ./get_helm.sh  
   Check your Helm version with: helm version  
 - Next, we will install Traefik:  
   - helm repo add traefik https://containous.github.io/traefik-helm-chart  
   - helm repo update  
   - helm install traefik traefik/traefik  
For using Dashboard, you can create IngressRoute:  
apiVersion: traefik.containo.us/v1alpha1  
kind: IngressRoute  
metadata:  
  name: dashboard  
spec:  
  entryPoints:  
    - web  
  routes:  
    - match: Host(`traefik-ui.com`) && (PathPrefix(`/dashboard`) || PathPrefix(`/api`))  
      kind: Rule  
      services:  
        - name: api@internal  
          kind: TraefikService  
In this example, I use Metallb to loadbalance, so I can access dashboard via http://traefik-ui.com/dashboard/
 
