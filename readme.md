# Como instalar e usar o Docker Compose no Ubuntu 20.04 + NGINX + WIKI.JS


# Etapa 1 - Instalar o Nginx no Ubuntu 20.04

Como o Nginx está disponível nos repositórios padrão do Ubuntu, é possível instalá-lo a partir desses repositórios usando o sistema de pacotes do apt.

sudo apt update
sudo apt install nginx

# Etapa 2 - Verificando seu Servidor Web
Passo 3 — Verificando seu Servidor Web


# Etapa 3 - Configurando Blocos do Servidor (Recomendado)
Para que o Nginx exiba este conteúdo, é necessário criar um bloco de servidor com as diretivas corretas. Em vez de modificar o arquivo de configuração padrão diretamente, vamos fazer um novo em /etc/nginx/sites-available/example.com:

sudo nano /etc/nginx/sites-available/wiki-dominio-com.conf

Cole no seguinte bloco de configuração, que é similar ao padrão, mas atualizado para nosso novo diretório e nome de domínio:

server {
 
    listen 80;
    listen [::]:80;
    server_name  wiki.dominio.com;


  location / {
    proxy_pass http://127.0.0.1:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
  }
}
Salve e feche o arquivo quando você terminar.

Em seguida, vamos habilitar o arquivo criando um link dele para o diretório sites-enabled, de onde o Nginx lê durante a inicialização:

sudo ln -s /etc/nginx/sites-available/wiki-dominio-com.conf /etc/nginx/sites-enabled/

Em seguida, teste para garantir que não haja erros de sintaxe em qualquer um dos seus arquivos do Nginx:

sudo nginx -t
Se não houver problemas, reinicie o Nginx para habilitar suas alterações:

sudo systemctl restart nginx

O Nginx agora deve estar exibindo seu nome de domínio. Você pode testar isso navegando para http://wiki.dominio.com

# Etapa 4 — Instalando o Docker Compose

Primeiro, confirme a versão mais recente disponível na página de lançamentos (https://github.com/docker/compose/releases). No momento da redação deste artigo, a versão estável mais atual é 1.29.2.

O comando a seguir fará o download da 1.29.2versão e salvará o arquivo executável em /usr/local/bin/docker-compose, o que tornará este software globalmente acessível como docker-compose:

sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$
(uname -m)" -o /usr/local/bin/docker-compose

Em seguida, defina as permissões corretas para que o docker-composecomando seja executável:

sudo chmod +x /usr/local/bin/docker-compose

Para verificar se a instalação foi bem-sucedida, você pode executar:

docker-compose --version

Você verá uma saída semelhante a esta:

Output
docker-compose version 1.29.2, build 5becea4c

# Etapa 5 — Acesse a pasta wiki altere o password

- POSTGRES_PASSWORD: meuwiki
- DB_PASS: meuwiki

# Etapa 6 — Executando o Docker Compose

Com o docker-compose.ymlarquivo no lugar, agora você pode executar o Docker Compose para ativar seu ambiente. O comando a seguir fará o download das imagens do Docker necessárias, criará um contêiner para o webserviço e executará o ambiente em contêiner no modo de segundo plano:

docker-compose up -d

Seu ambiente agora está funcionando em segundo plano. Para verificar se o contêiner está ativo, você pode executar:

docker-compose ps

Depois da intslação é só acessar via http://wiki.dominio.com


