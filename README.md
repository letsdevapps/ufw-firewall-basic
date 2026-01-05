# UFW Uncomplicated Firewall Basic

## Instalação Ubuntu

O UFW costuma vir em instalações padrão

    sudo ufw status

Caso não tenha vindo

    sudo apt-get install ufw -y

## Habilitar/Desabilitar

    sudo ufw enable
    
    sudo ufw disable

## Adicionar Regras

O tráfego de saída não é bloqueado pelo UFW por padrão. O que você precisaria adicionar regras para bloquear ou permitir são as conexões de entrada, como quando alguém tenta acessar o seu computador ou quando você roda um servidor.

Se você está usando um servidor web (Apache ou Nginx) e quer que ele seja acessível pela internet, você precisará liberar as portas correspondentes no UFW, como a **porta 80** para HTTP e a **porta 443** para HTTPS.

Permitir HTTP (porta 80)

   sudo ufw allow 80/tcp
   
Permitir HTTPS (porta 443)

   sudo ufw allow 443/tcp

Permitir SSH (porta 22) para acesso remoto via terminal

   sudo ufw allow 22/tcp

### Permitir qualquer aplicação já instalada:

O UFW também permite que você **libere aplicativos inteiros** em vez de liberar apenas portas específicas

Permitir o Apache (servidor web)

  sudo ufw allow 'Apache'

Permitir o Nginx

  sudo ufw allow 'Nginx Full'

### Como saber quais aplicativos podem ser permitidos:

Você pode ver uma lista de aplicativos já configurados no UFW com o seguinte comando:

    sudo ufw app list

Esse comando mostra os serviços que você pode permitir de forma mais fácil, sem ter que se preocupar com as portas exatas.

