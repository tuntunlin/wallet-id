# RKE2 cluster with Ansible

### 1. Install Kubectl

```sh
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl

```

### 2. Install Helm

```sh
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod 700 get_helm.sh
./get_helm.sh

```

### 3. Install Ansible

```sh
sudo apt update
sudo apt install software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible
```

### 4. Install rke2 with ansible.

1. find the ip with commend to confirm the ip

```warp-runnable-command
for x in 192.168.0.{101..106}; do ping -W1 -c1 $x > /dev/null && echo "$x : OK" || echo "$x : Not OK"; done;
```

2. install ansible plugin

```warp-runnable-command
ansible-galaxy install lablabs.rke2
```

3. make ssh copy to login without password (Note. This is possible to add user into sudoer and passwordless but I will add sudo password in ansible become user)

```warp-runnable-command
ssh-keygen -t rsa
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-01@192.168.0.101
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-02@192.168.0.102
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-03@192.168.0.103
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-04@192.168.0.104
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-05@192.168.0.105
ssh-copy-id -i ~/.ssh/id_rsa.pub k8s-vm-06@192.168.0.106
```


4. Prepare `inventory.yaml` installation list.

```yaml
[masters]
k8s-vm-01 ansible_host=172.16.10.205 ansible_user=walletnode1 ansible_become=true ansible_become_password=abcd1234 rke2_type=server
k8s-vm-02 ansible_host=172.16.10.206 ansible_user=walletnode2 ansible_become=true ansible_become_password=abcd1234 rke2_type=server
k8s-vm-03 ansible_host=172.16.10.207 ansible_user=walletnode3 ansible_become=true ansible_become_password=abcd1234 rke2_type=server

[workers]
k8s-vm-04 ansible_host=172.16.10.208 ansible_user=walletnode4 ansible_become=true ansible_become_password=abcd1234 rke2_type=agent
k8s-vm-05 ansible_host=172.16.10.209 ansible_user=walletnode5 ansible_become=true ansible_become_password=abcd1234 rke2_type=agent
k8s-vm-06 ansible_host=172.16.10.210 ansible_user=walletnode6 ansible_become=true ansible_become_password=abcd1234 rke2_type=agent

[k8s_cluster:children]
masters
workers
```

5. Prepare rke2 deployment file. (cilium-playbook.yaml)

```yaml
- name: Deploy RKE2
  hosts: k8s_cluster
  become: yes
  vars:
    rke2_version: v1.30.6+rke2r1
    rke2_api_ip: 172.16.10.205
    rke2_download_kubeconf: true
    rke2_loadbalancer_ip_range: {"start": "10.0.0.100", "end": "10.0.0.200"}
    rke2_server_node_taints:
      - "CriticalAddonsOnly=true:NoExecute"

  roles:
    - role: lablabs.rke2
```

7. deploy rke2 with ansible.

```warp-runnable-command
ansible-playbook -i inventory.yaml rke2-playbook.yaml
```
kubectl get nodes -A -o wide
kubectl get svc -A

```
mkdir -p ~/.kube
ls /tmp/rke2.yaml
cat /tmp/rke2.yaml
cp /tmp/rke2.yaml ~/.kube/rke2.yaml
chmod 600 ~/.kube/rke2.yaml
mv ~/.kube/rke2.yaml ~/.kube/config
ls -la .kube/
sudo chown walletvm:walletvm .kube/config
ls -la .kube/
sudo vi .kube/config
kubectl get nodes -A
sudo vi .kube/config (Input node1 IP)
kubectl get nodes -A
kubectl label node k8s-vm-04 node-role.kubernetes.io/worker=worker
kubectl label node k8s-vm-05 node-role.kubernetes.io/worker=worker
kubectl label node k8s-vm-06 node-role.kubernetes.io/worker=worker 
```

### 5. Install rancher with cert-manager.
Install cert-manager first
```sh
helm repo add jetstack https://charts.jetstack.io --force-update

helm repo update

helm install cert-manager jetstack/cert-manager \
              --namespace cert-manager \
              --create-namespace \
              --set crds.enabled=true
```
Then Install rancher.rancher.wallet.mm(Passw0rd!@#$)
```sh
helm repo add rancher-stable https://releases.rancher.com/server-charts/stable

helm repo update

helm install rancher rancher-stable/rancher \
              --namespace cattle-system \
              --set hostname=rancher.my.org \
              --set bootstrapPassword=admin

kubectl edit ing rancher -n cattle-system //hostname edit
kubectl get pods -A
kubectl get ing -A
```


### 6. Install istio

```sh
helm repo add istio https://istio-release.storage.googleapis.com/charts

helm repo update

helm install istio-base istio/base -n istio-system --set defaultRevision=default --create-namespace
helm install istiod istio/istiod -n istio-system --wait
kubectl create namespace istio-ingress
helm install istio-ingress istio/gateway -n istio-ingress --wait
```

### 7. Install prometheus with kiali.

```sh
kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.24/samples/addons/prometheus.yaml

kubectl apply -f https://raw.githubusercontent.com/istio/istio/release-1.24/samples/addons/kiali.yaml
```

### 8. Installing Longhorn
Install Longhorn on any Kubernetes cluster using this command:
```sh
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.8.0/deploy/longhorn.yaml
One way to monitor the progress of the installation is to watch pods being created in the longhorn-system namespace:

kubectl get pods \
--namespace longhorn-system \
--watch
```

### Install demo application
```sh

```
