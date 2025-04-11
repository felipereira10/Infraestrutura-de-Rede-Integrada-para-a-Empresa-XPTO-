# Infraestrutura-de-Rede-Integrada-para-a-Empresa-XPTO

![image](https://github.com/user-attachments/assets/29ea5c83-39e8-40df-ae30-6c6478c18586)

# 🏗️ Infraestrutura de Rede Integrada - Empresa XPTO

Projeto prático de implementação de uma infraestrutura de TI, segura e escalável para a empresa fictícia XPTO. A proposta tem as seguintes tecnologias: Docker, VPN, balanceamento de carga, segmentação de rede, proxy reverso e monitoramento.

## 📌 Objetivos

- Criar um ambiente seguro, escalável e confiável
- Permitir acesso remoto via VPN
- Distribuir o tráfego com Load Balancer
- Garantir segurança com firewall e autenticação
- Utilizar serviços virtualizados com Docker
- Monitorar desempenho e disponibilidade

## 🖧 Diagrama da Arquitetura

![Diagrama da Rede](./caminho/para/o/diagrama.png)

## 🚀 Tecnologias Utilizadas

- 🔧 **Docker & Docker Compose**
- 🌐 **Nginx** (Proxy Reverso e Load Balancer)
- 🐳 **Containers Web (Web01, Web02, Web03)**
- 🛢️ **Banco de Dados: PostgreSQL (via Docker ou AWS RDS)**
- 🔐 **OpenVPN** (acesso remoto seguro)
- 🔥 **Firewall** com regras específicas
- 📊 **Monitoramento com Prometheus + Grafana**
- 📡 **DHCP e Endereçamento IPv4 com Sub-redes**

## 🧱 Estrutura dos Serviços

```bash
.
├── docker-compose.yml
├── nginx/
│   ├── proxy.conf
│   └── loadbalancer.conf
├── vpn/
│   └── openvpn.conf
├── firewall/
│   └── regras.sh
├── bd/
│   └── init.sql
├── monitoramento/
│   ├── prometheus.yml
│   └── grafana/
└── README.md
```

## 📋 Como Executar o Projeto
Clone o repositório:

```bash
git clone https://github.com/seu-usuario/xpto-infra.git
cd xpto-infra
```

Inicie os serviços:

```bash
docker-compose up -d
```

Acesse o sistema:

Web: http://localhost

VPN: via OpenVPN

Monitoramento: http://localhost:3000

## 🧠 Justificativas Técnicas
PostgreSQL escolhido pelo suporte robusto a dados estruturados e confiabilidade.

Nginx por sua leveza e versatilidade tanto como proxy quanto como balanceador.

Docker para facilitar escalabilidade e replicação do ambiente.

OpenVPN por ser uma solução madura e segura de VPN open-source.

## 🔒 Segurança
Acesso remoto via VPN com autenticação

Regras de firewall personalizadas

Containers isolados

Planejamento de sub-redes e DHCP

Possibilidade de integração com 2FA

## 📈 Monitoramento
Prometheus para coleta de métricas

Grafana para dashboards em tempo real

Alertas configuráveis para disponibilidade de serviços

## 🧮 Exercícios de Endereçamento
Sub-redes da rede 192.168.50.0/24

Detalhamento da rede 172.18.0.0/20

Simulação de configuração de DHCP (rede 10.5.1.0/25)
