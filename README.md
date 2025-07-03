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

- Raspberry Pi com conexao externa
- Acesso root ou `sudo` no Raspberry
- Acesso root ou `sudo` na VPS
- Chave SSH configurada entre Raspberry → VPS

---

## ⚙️ Passo a Passo

### 1. Gere e configure as chaves SSH no Raspberry

```bash
ssh-keygen -t rsa -b 4096
ssh-copy-id user@IP_DA_VPS
