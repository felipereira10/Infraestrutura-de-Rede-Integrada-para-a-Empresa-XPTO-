# Load Balance simples com Docker
Atualizar pacotes:
```bash
sudo apt update && sudo apt upgrade -y
```

Instalar dependências:
```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

Adicionar a chave GPG oficial do Docker:
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Adicionar repositório Docker:
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] \
  https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Instalar Docker:
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```

Habilitar e iniciar o Docker:
```bash
sudo systemctl enable docker
sudo systemctl start docker
```

Criar um diretório com o conteúdo HTML que será servido:
```bash
mkdir ~/meu-site
cd ~/meu-site
```

Criar um HTML Simples dentro do "meu-site":
```bash
echo "<h1>Estamos no Container</h1>" > index.html
```

Posteriomente crie um Dockerfile com:
```bash
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

Para construir a imagem:
```bash
docker build -t meu-site .
```

Para iniciar os containers:
```bash
docker run -d --name web01 meu-site
docker run -d --name web02 meu-site
docker run -d --name web03 meu-site
```

Caso já tenha criado, utilize:
```bash
docker start <nome_ou_id_do_container ou os três primeiro digitos do ID>
```

Pegar o IP dos containers para utilizar no Load Balancer:
```bash
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web01
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web02
docker inspect -f '{{range.NetworkSettings.Networks}}{{.IPAddress}}{{end}}' web03
```

Subir container com NGINX como Load Balancer: realize um cd - ( para sair de do meu-site ou o nome de foi dado anteriormente)
```bash
mkdir ~/nginx-lb
cd ~/nginx-lb
```

Crie o nginx.conf dentro do nginx-lb:
```bash
events {}
http {
    upstream meusites {
        server 172.17.0.2;  # IP do web01
        server 172.17.0.3;  # IP do web02
        server 172.17.0.4;  # IP do web03
    }

    server {
        listen 80;

        location / {
            proxy_pass http://meusites;
        }
    }
}
```

Crie o container com esse NGINX:
```bash
docker run -d --name loadbalancer \
  -v $(pwd)/nginx.conf:/etc/nginx/nginx.conf:ro \
  -p 80:80 \
  nginx
```

Teste agora é só pegar o IP público para entrar no site que foi criado.

## Opicional
Caso queria alterar o HTML dos containers para diferenciá-los, pode utilizar dos seguintes comandos (se containers estejam rodando ainda):

comando exemplo:
```bash
docker exec -it web01 sh                             - comando para entrar no container  
cd /usr/share/nginx/html                             - comando para acessar diretório que está o html
echo '<h1>Estamos no Container01!</h1>' > index.html - a alteração do html
exit                                                 - comando para sair do container
```

Repetir este processo para os outros containers.
