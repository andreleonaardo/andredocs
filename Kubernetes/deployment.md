# Instalação do Kubernetes local (k3s)

## Steps:



### Edite o arquivo do serviço:

sudo vim /etc/systemd/system/k3s.service

Localize a linha ExecStart=/usr/local/bin/k3s server.

Adicione o parâmetro --write-kubeconfig-mode 644 ao final da linha.

### Salve o arquivo e recarregue o daemon do systemd:

sudo systemctl daemon-reload

## Reinicie o serviço do k3s:

sudo systemctl restart k3s

---

## Baixar o binário do k3s manualmente

### Baixe o binário mais recente:

curl -Lo k3s https://github.com/k3s-io/k3s/releases/latest/download/k3s

chmod +x k3s

sudo mv k3s /usr/local/bin/

### Verifique a instalação:

k3s --version

## Configurar o k3s como serviço

### Baixe e instale o script de configuração do serviço:

curl -Lo /etc/systemd/system/k3s.service https://raw.githubusercontent.com/k3s-io/k3s/main/k3s.service

### Configure o serviço (necessário colocar as configurações abaixo neste arquivo)

```
[Unit]
Description=Lightweight Kubernetes
Documentation=https://k3s.io
After=network-online.target
Wants=network-online.target

[Service]
Type=notify
EnvironmentFile=-/etc/default/%N
EnvironmentFile=-/etc/sysconfig/%N
EnvironmentFile=-/etc/systemd/system/k3s.service.env
ExecStartPre=/bin/sh -xc '! /usr/bin/systemctl is-enabled --quiet nm-cloud-setup.service 2>/dev/null'
ExecStart=/usr/local/bin/k3s server --write-kubeconfig-mode 644
KillMode=process
Delegate=yes
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNOFILE=1048576
LimitNPROC=infinity
LimitCORE=infinity
TasksMax=infinity
TimeoutStartSec=0
Restart=always
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

### Inicie e habilite o serviço:

sudo systemctl daemon-reload

sudo systemctl enable --now k3s

### Necessário definir as permissões do arquivo abaixo antes de seguir:

sudo chmod 644 /etc/rancher/k3s/k3s.yaml

### Exporte a variável de ambiente para o contexto do Kubernetes

sudo export KUBECONFIG=/etc/rancher/k3s/k3s.yaml

### Para tornar permanete, siga abaixo:

sudo echo "export KUBECONFIG=/etc/rancher/k3s/k3s.yaml" >> ~/.bashrc

sudo source ~/.bashrc