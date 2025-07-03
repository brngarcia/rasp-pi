# rasp-pi


Configurar um Shell Reverso Persistente com SSH, de modo que o Raspberry Pi, ao ser ligado, se conecte automaticamente à uma VPS e permita que você entre remotamente a partir dela.

# Visão geral

1 - O Raspberry Pi abre um túnel SSH reverso para sua VPS (que tem IP fixo ou domínio).
2 - Esse túnel expõe a porta SSH do Raspberry através da VPS.
3 - Você, a partir da VPS, poderá se conectar ao Raspberry com ssh usando o túnel.
4 - Tudo isso acontece automaticamente na inicialização do Raspberry.


⚙️ Pré-requisitos

✅ VPS com IP público fixo (ex: Ubuntu ou Debian)
✅ Acesso root ao Raspberry Pi e à VPS
✅ Chave SSH criada no Raspberry (ou login por senha liberado na VPS)
✅ Porta 22 liberada na VPS (ou outra, se preferir)

1. Criari user e chave SSH no Raspy:
ssh-keygen -t ed25519 -f ~/.ssh/id_ed25519 -N ""

Copie a chave pública para a VPS:
ssh-copy-id -i ~/.ssh/id_ed25519.pub usuario@IP_DA_VPS

Teste a conexao:
ssh -i ~/.ssh/id_ed25519 usuario@IP_DA_VPS
