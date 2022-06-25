# Установка инструментов k8s

## Установка kubectl

### загрузим бинарь
curl -LO https://storage.googleapis.com/kubernetes-release/release/`curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt`/bin/linux/amd64/kubectl

### изменним права
chmod +x ./kubectl

### Переместим прогу в директорию из переменной окружения PATH
env 
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin

mv kubectl /usr/local/sbin

### Проверим работу проги
kubectl version

### Настроим алиас и автоподстановку
echo 'alias k=kubectl' >>~/.bashrc

### Настроим автоподстановку
echo 'source <(kubectl completion bash)' >>~/.bashrc или kubectl completion bash >/etc/bash_completion.d/kubectl (путь можно чекнуть командой 
echo 'complete -F __start_kubectl k' >>~/.bashrc

## Установка kubens и kubectx https://github.com/ahmetb/kubectx/releases
```
wget -q https://github.com/ahmetb/kubectx/releases/download/v0.9.4/kubens
wget -q https://github.com/ahmetb/kubectx/releases/download/v0.9.4/kubectx
chmod +x kubens kubectx
mv kubens /usr/local/sbin
mv kubectx /usr/local/sbin
```
