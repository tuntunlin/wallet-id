
## 1. စနစ်စစ်ဆေးခြင်း (System Verification)

```bash
# Pod အားလုံးအလုပ်လုပ်နေကြောင်းစစ်ဆေးပါ
kubectl get pods -n mosip-system

# Service များကိုစစ်ဆေးပါ
kubectl get svc -n mosip-system

# Ingress ရှိပါက စစ်ဆေးပါ
kubectl get ingress -n mosip-system
```




# Add repository
```bash
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list
```
# Install kubeadm, kubectl, and kubelet
```bash
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
```
# Hold packages at current version (prevent auto-upgrade)
```bash
sudo apt-mark hold kubelet kubeadm kubectl
```
