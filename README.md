# Ansible Playbooks - Provisionamento e Configuração de Servidores

Este repositório contém uma série de playbooks Ansible para provisionamento básico e configuração de servidores Linux, com foco em automação de tarefas comuns como atualização do sistema, criação de usuários, instalação de ferramentas essenciais e configuração do Zabbix Agent.

## Estrutura dos Playbooks

### 1. `UpdateSystem.yml`

Atualiza os pacotes do sistema operacional para a versão mais recente disponível.

**Tarefas executadas:**

- Atualização da lista de pacotes (`apt update`)
- Atualização dos pacotes instalados (`apt upgrade`)

---

### 2. `AddUsers.yml`

Adiciona usuários ao sistema com permissões específicas e configurações básicas de segurança.

**Tarefas executadas:**

- Criação de usuários
- Inclusão em grupos (ex: `sudo`)
- Configuração de senha ou chave SSH (se aplicável)

---

### 3. `InstallTools.yml`

Instala ferramentas e utilitários essenciais para administração e diagnóstico de sistemas.

**Ferramentas instaladas:**

- `curl`
- `vim`
- `htop`
- `net-tools`
- Outras ferramentas básicas utilizadas em ambientes Linux

---

### 4. `InstallZabbixAgent.yml`

Instala e configura o Zabbix Agent, permitindo o monitoramento do servidor por meio da plataforma Zabbix.

**Tarefas executadas:**

- Download do pacote oficial do repositório Zabbix
- Instalação do repositório
- Instalação do agente Zabbix
- Configuração do arquivo `zabbix_agentd.conf` com o IP ou hostname do servidor Zabbix
- Início e habilitação do serviço `zabbix-agent`

---

## Como utilizar

1. **Configure o inventário Ansible** com os hosts-alvo (ex: `hosts.ini`).
2. **Edite os arquivos de variáveis**, se necessário (por exemplo: lista de usuários ou IP do servidor Zabbix).
3. **Execute os playbooks individualmente**, conforme a necessidade:

```bash
ansible-playbook -i hosts.ini UpdateSystem.yml
ansible-playbook -i hosts.ini addusers.yml
ansible-playbook -i hosts.ini InstallTools.yml
ansible-playbook -i hosts.ini InstallZabbixAgent.yml
```

## Observações

- É recomendado executar o playbook `UpdateSystem.yml` antes dos demais, para garantir que os pacotes estejam atualizados.
- Certifique-se de revisar e personalizar as variáveis, especialmente:
  - Lista de usuários em `addusers.yml`
  - Endereço do servidor Zabbix em `InstallZabbixAgent.yml`
- O playbook `InstallZabbixAgent.yml` depende do acesso ao repositório oficial do Zabbix via HTTPS.
- Verifique regras de firewall e portas (como a `10050/TCP`) se for necessário o monitoramento remoto via Zabbix.

## Requisitos

- Python 3 instalado na máquina de controle
- Instalar Python 3.9 ou superior tratando-se de Oracle Linux
- Ansible 2.9+ instalado localmente
- Acesso SSH funcional aos servidores gerenciados
- Servidores gerenciados baseados em Debian/Ubuntu
- Permissões de sudo nos hosts de destino
- Conexão com a internet para instalação de pacotes
