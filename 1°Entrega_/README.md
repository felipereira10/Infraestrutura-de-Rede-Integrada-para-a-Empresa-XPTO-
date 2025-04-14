# Load Balancer com Docker + NGINX + Docker Compose

Este projeto configura um ambiente com **3 containers NGINX** servindo páginas HTML simples, balanceados por um container **NGINX acting as Load Balancer**, utilizando **Docker Compose**.

---

## 🧱 1. Instalação do Docker

```bash
sudo apt update && sudo apt upgrade -y

sudo apt install -y apt-transport-https ca-certificates curl software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

sudo systemctl enable docker
sudo systemctl start docker
```

## 📁 2. Estrutura de Diretórios
```bash
mkdir ~/meu-site
mkdir ~/nginx-lb
```

## 🖥️ 3. Arquivos da Aplicação
~/meu-site/index.html
```bash
<h1>Estamos no Container</h1>
```

~/meu-site/Dockerfile
```bash
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

~/nginx-lb/nginx.conf
```bash
events {}

http {
    upstream meusites {
        server web01:80;
        server web02:80;
        server web03:80;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://meusites;
        }
    }
}
```

## ⚙️ 4. Arquivo docker-compose.yml
Crie este arquivo na raiz do projeto (por exemplo, em ~/):

```bash
version: '3'

services:
  web01:
    build: ./meu-site
    container_name: web01

  web02:
    build: ./meu-site
    container_name: web02

  web03:
    build: ./meu-site
    container_name: web03

  loadbalancer:
    image: nginx
    container_name: loadbalancer
    ports:
      - "80:80"
    volumes:
      - ./nginx-lb/nginx.conf:/etc/nginx/nginx.conf:ro
    depends_on:
      - web01
      - web02
      - web03
```

## 🚀 5. Subir os containers com Docker Compose
Limpar containers antigos (se houver):
```bash
docker rm web01 web02 web03 loadbalancer
```

Subir ambiente:
```bash
docker compose up -d --build
```

Verificar status:
```bash
docker ps
```

## 🖌️ 6. Personalizar mensagem de cada container (Opcional)
```bash
docker exec -it web01 sh
cd /usr/share/nginx/html
echo "<h1>Estamos no Container01!</h1>" > index.html
exit
```
Repita o processo para web02 e web03, mudando o número da mensagem.

## 🌐 7. Acessar pelo Navegador
Use o IP público da sua instância EC2:

```bash
http://<seu_ip_publico>
```

Cada atualização na página alternará entre os containers, demonstrando o funcionamento do balanceamento de carga.

## ✅ Resultado Esperado
3 containers NGINX servindo páginas HTML diferentes

1 container NGINX atuando como Load Balancer

Docker Compose gerenciando tudo automaticamente




