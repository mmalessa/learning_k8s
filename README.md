# Learning k8s (in a local environment)
  
## Install 

### kubectl
https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/
```sh
sudo apt-get update
sudo apt-get install -y ca-certificates curl
curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-archive-keyring.gpg
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl

kubectl version -o json
```


#### krew
https://krew.sigs.k8s.io/docs/user-guide/setup/install/
```sh
(
  set -x; cd "$(mktemp -d)" &&
  OS="$(uname | tr '[:upper:]' '[:lower:]')" &&
  ARCH="$(uname -m | sed -e 's/x86_64/amd64/' -e 's/\(arm\)\(64\)\?.*/\1\2/' -e 's/aarch64$/arm64/')" &&
  KREW="krew-${OS}_${ARCH}" &&
  curl -fsSLO "https://github.com/kubernetes-sigs/krew/releases/latest/download/${KREW}.tar.gz" &&
  tar zxvf "${KREW}.tar.gz" &&
  ./"${KREW}" install krew
)
```
Add the $HOME/.krew/bin directory to your PATH environment variable. To do this, update your .bashrc or .zshrc file and append the following line:
```sh
export PATH="${KREW_ROOT:-$HOME/.krew}/bin:$PATH"
```
and restart your sheel

```sh
kubectl krew version
```

####  ctx and ns
```sh
kubectl krew install ctx
kubectl krew install ns
```

### k3d
```sh
wget -q -O - https://raw.githubusercontent.com/k3d-io/k3d/main/install.sh | bash

k3d version
```

## notes
- https://eskala.io/tutorial/deploying-apps-to-a-local-k3d-cluster/




```sh
k3d registry create test-app-registry --port 5050
```
use `registries.yaml` file
```sh
k3d cluster create mycluster -p "9900:80@loadbalancer" --registry-use k3d-test-app-registry:5050 --registry-config registries.yaml
```

Tag and push local docker image
```sh
docker tag go-web-server:latest localhost:5050/go-web-server:v1.0
docker push localhost:5050/go-web-server:v1.0
```

?? Access to images ??
```sh
docker exec k3d-k3s-default-server-0 sh -c "ctr image list -q | grep registry.gitlab | xargs ctr image rm"
```



