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
- For using Dashboard, you can create IngressRoute: kubectl apply -f dashboard-traefik.yaml  
In this example, I use Metallb to loadbalance, so I can access dashboard via http://traefik-ui.com/dashboard/  
# In oder to use Let's Encrypt for creating a cert, we will edit Traefik's deployment
- For editing Traefik's deployment:  
  -  Use kubectl edit deployment traefik: In this config, we will add some line below:  
        - --certificatesresolvers.myresolver.acme.tlschallenge
        - --certificatesresolvers.myresolver.acme.email=ducptm@example.com.vn
        - --certificatesresolvers.myresolver.acme.storage=/data/acme.json
        - --certificatesresolvers.myresolver.acme.caserver=https://acme-staging-v02.api.letsencrypt.org/directory
- Edit config IngressRoute Traefik's Dashboard to use SSL/TLS for it: We can add line - websecure under - web in config before  
Finally, we redeploy Traefik: kubectl rollout restart deployment traefik

  
