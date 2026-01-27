  45  docker ps
   46  docker ps -A
   47  docker ps -a
   48  docker stop $(docker ps -aq)
   49  docker rm $(docker ps -aq)
   50  docker -a
   51  docker images
   52  docker images -aq
   53  docker rmi -f $(docker images -aq)
   54  docker images
   55  docker ps -a

 214  ls -la .kube
  215  ls /tmp/
  216  ls /tmp/rke2.yaml
  217  cat /tmp/rke2.yaml
  218  cp /tmp/rke2.yaml ~/.kube/rke2.yaml
  219  sudo cp /tmp/rke2.yaml ~/.kube/rke2.yaml
  220  chmod 600 ~/.kube/rke2.yaml
  221  sudo chmod 600 ~/.kube/rke2.yaml
  222  mv ~/.kube/rke2.yaml ~/.kube/config
  223  sudo mv ~/.kube/rke2.yaml ~/.kube/config
  224  clear
  225  kubectl get nodes -A
  226  ls -la .kube/
  227  sudo chown walletjh:walletjh .kube/config
  228  ls -la .kube/
  229  kubectl get nodes -A
  230  ls /tmp/
  231  ls /tmp/rke2.yaml
  232  ls -la /tmp/rke2.yaml
  233  cat /tmp/rke2.yaml
  234  doff
  235  diff
  236  diff /tmp/rke2.yaml .kube/config
  237  clear
  238  cat /tmp/rke2.yaml
  239  diff /tmp/rke2.yaml .kube/config
  240  cp /tmp/rke2.yaml ~/.kube/rke2.yaml
  241  sudo cp /tmp/rke2.yaml ~/.kube/rke2.yaml
  242  clear
  243  ls -la .kube/
  244  sudo chown walletjh:walletjh .kube/rke2.yaml
  245  chmod 600 ~/.kube/rke2.yaml
  246  mv ~/.kube/rke2.yaml ~/.kube/config
  247  sudo mv ~/.kube/rke2.yaml ~/.kube/config
  248  ls -la .kube
  249  kubectl get nodes -A
  250  diff /tmp/rke2.yaml .kube/config
  251  ls -la /tmp
  252  diff /tmp/rke2.yaml .kube/config
  253  sudo cp /tmp/rke2.yaml ~/.kube/rke2.yaml
  254  sudo chown walletjh:walletjh .kube/rke2.yaml
  255  chmod 600 ~/.kube/rke2.yaml
  256  sudo mv ~/.kube/rke2.yaml ~/.kube/config
  257  ls -la .kube/
  258  sudo chown walletjh:walletjh .kube/
  259  ls -la .kube/
  260  kubectl get node
  261  clear
  262  kubectl get pods -A
  263  kubectl get svc -A -o wide
