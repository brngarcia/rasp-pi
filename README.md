# 🧪 Raspberry Pi – Acesso Remoto via Shell Reverso (SSH Tunnel)

Este documento descreve como configurar um **Raspberry Pi** para realizar conexão automática via **túnel reverso SSH**, permitindo acesso remoto mesmo em redes que não possuem redirecionamento de portas. Esse método é ideal para situações em que o dispositivo é deixado em campo (ex: em uma rede de cliente) e não pode ser acessado diretamente.

---

## ✅ Cenário

- O Raspberry Pi está configurado com um sistema Linux (como Raspberry Pi OS ou Kali).
- Você tem uma **VPS pública** com IP acessível externamente.
- O Raspberry será **conectado à rede do cliente** e deve estabelecer automaticamente um túnel reverso para sua VPS.
- Após a conexão, você poderá acessar o Raspberry remotamente via `ssh -p <porta> localhost` na VPS.

---

## 📦 Pré-requisitos

- Raspberry Pi com antena Wi-Fi ou cabo de rede funcionando
- Acesso root ou `sudo` no Raspberry
- Acesso root ou `sudo` na VPS
- Chave SSH configurada entre Raspberry → VPS (sem senha)

---

## ⚙️ Passo a Passo

### 1. Gere e configure as chaves SSH no Raspberry

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id user@IP_DA_VPS
```

> **Importante:** o Raspberry deve conseguir conectar sem senha.

---

### 2. Crie o túnel reverso via SSH

Teste manualmente no Raspberry:

```bash
ssh -N -R 2345:localhost:22 user@IP_DA_VPS
```

- `-N`: não executa comando remoto
- `-R 2345:localhost:22`: encaminha a porta 2345 da VPS para a porta 22 do Raspberry

---

### 3. Configure a conexão automática com `autossh`

Instale no Raspberry:

```bash
sudo apt install autossh
```

Crie um serviço systemd:

```bash
sudo nano /etc/systemd/system/reverse-ssh.service
```

Cole o conteúdo abaixo (ajuste o IP e usuário):

```ini
[Unit]
Description=Reverse SSH tunnel via autossh
After=network.target

[Service]
User=pi
Environment="AUTOSSH_GATETIME=0"
ExecStart=/usr/bin/autossh -N -o "ServerAliveInterval 30" -o "ServerAliveCountMax 3" -R 2345:localhost:22 user@IP_DA_VPS
Restart=always

[Install]
WantedBy=multi-user.target
```

Ative o serviço:

```bash
sudo systemctl daemon-reexec
sudo systemctl enable --now reverse-ssh
```

---

## 🛠️ Acessando o Raspberry a partir da VPS

Na VPS, basta executar:

```bash
ssh pi@localhost -p <port>
```

---

## 📌 Observações

- Certifique-se de que a VPS permite o uso de `GatewayPorts` no `/etc/ssh/sshd_config`:
  ```bash
  GatewayPorts yes
  ```
- Verifique se o túnel está ativo com:
  ```bash
  netstat -tunlp | grep 2222
  ```
- Verifique se a porta esta em uso:
  ```bash
  lsof -i :port
  ```
- Matar os processos se precisar:
  ```bash
  kill -9 IDPROCESS:
  ```
---
