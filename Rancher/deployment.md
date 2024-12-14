

Pré-requisitos
Antes de começar, certifique-se de que você tem:

Um cluster Kubernetes configurado.
kubectl instalado e configurado para acessar o seu cluster.
Helm 3 instalado (para facilitar a instalação).
Se não tiver o Helm, instale-o com o seguinte comando:

bash
Copiar código
curl https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
2. Adicionar o repositório do Rancher
Adicione o repositório do Rancher ao Helm:

bash
Copiar código
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo update
3. Criar o namespace para o Rancher
Crie um namespace no Kubernetes onde o Rancher será instalado:

bash
Copiar código
kubectl create namespace cattle-system
4. Instalar o Rancher com Helm
Agora, instale o Rancher usando o Helm:

export RANCHER_HOSTNAME="localhost.localdomain"

bash
Copiar código
helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=<127.0.0.1>
Substitua <RANCHER_HOSTNAME> pelo domínio completo (FQDN) onde o Rancher será acessado. Esse domínio precisa apontar para o IP do seu cluster.

5. Verificar a instalação
Verifique se os pods do Rancher estão rodando corretamente:

bash
Copiar código
kubectl -n cattle-system get pods
Você deve ver algo como rancher-xxxx sendo executado.

Acessar o Rancher
Depois de instalar, o Rancher estará acessível no seu navegador através do hostname configurado. Se você configurou um endereço DNS corretamente, basta acessar https://<RANCHER_HOSTNAME>.

7. Configurar o acesso inicial
Ao acessar a interface web do Rancher pela primeira vez, você será solicitado a definir uma senha para o admin. Após definir a senha, você pode começar a usar o Rancher para gerenciar seus clusters Kubernetes.

8. (Opcional) Configuração de Ingress Controller
Se você não tem um Ingress Controller configurado no seu cluster, instale um (como o NGINX Ingress Controller) para expor o Rancher publicamente:

bash
Copiar código
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/cloud/deploy.yaml
Isso configura um Ingress Controller, permitindo que você acesse o Rancher através de um domínio externo.





----

2301  2024-12-06 16:41:10 mkdir cert-manager
 2302  2024-12-06 16:41:24 cd ..
 2303  2024-12-06 16:41:26 ls -lthra
 2304  2024-12-06 16:41:30 ls -ltrha cert-manager/
 2305  2024-12-06 16:41:44 cd cert-manager/
 2306  2024-12-06 16:41:48 kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.crds.yaml
 2307  2024-12-06 16:41:51 ls -ltrha
 2308  2024-12-06 16:42:19 sudo kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.yaml
 2309  2024-12-06 16:42:24 kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.12.0/cert-manager.yaml
 2310  2024-12-06 16:43:05 kubectl get pods cert-manager
 2311  2024-12-06 16:43:09 kubectl get pods -n cert-manager
 2312  2024-12-06 16:43:23 helm install rancher rancher-latest/rancher --namespace cattle-system --set hostname="localhost.localdomain"
 2313  2024-12-06 16:43:39 kubectl get pods

