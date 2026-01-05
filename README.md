# UFW Uncomplicated Firewall Basic

## Instalação Ubuntu

O UFW costuma vir em instalações padrão

    sudo ufw status

Depois de adicionar as permissões, você pode verificar se as regras foram aplicadas corretamente com

    sudo ufw status verbose
    
    Status: active Logging: on (low) Default: deny (incoming), allow (outgoing), disabled (routed) New profiles: skip

1. Status: active
Isso significa que o UFW está ativo e em funcionamento.
2. Logging: on (low)
O log está ativado, mas com um nível baixo de detalhamento. Isso ajuda a monitorar eventos sem sobrecarregar seu sistema com muitas informações.
3. Default: deny (incoming), allow (outgoing)
+ deny (incoming): Conexões de entrada são bloqueadas por padrão. Ou seja, qualquer pessoa ou sistema tentando se conectar ao seu computador (sem uma regra específica) será bloqueado.
+ allow (outgoing): Conexões de saída são permitidas por padrão. Então, programas como o Firefox podem acessar a internet sem problemas, porque o tráfego de saída não é bloqueado.
4. disabled (routed)
Isso se refere ao tráfego de roteamento. Basicamente, significa que o firewall não está configurado para rotear pacotes entre diferentes interfaces de rede. Na maioria dos casos, isso não afeta você diretamente, a menos que esteja configurando algo como um servidor que precise roteamento entre redes.
5. New profiles: skip
Isso significa que o UFW não está automaticamente aplicando novos perfis de aplicativos. Ou seja, ele não vai automaticamente tentar configurar regras para novos programas ou serviços que você instale. Você terá que adicionar essas regras manualmente se precisar.

Caso não tenha vindo

    sudo apt-get install ufw -y

## Habilitar/Desabilitar

    sudo ufw enable
    
    sudo ufw disable

Se você quiser bloquear o tráfego de entrada, você poderia fazer isso. Bloquear tudo de entrada (porém, isso pode afetar outros serviços):

    sudo ufw default deny incoming

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

## Verificação dos Logs

Para abrir o arquivo de log com o nano e visualizar

    sudo nano /var/log/ufw.log

Se você quiser ver os logs em tempo real enquanto o sistema está gerando novos eventos

    sudo tail -f /var/log/ufw.log

Se o log estiver muito grande ou se você quiser buscar por informações específicas (por exemplo, tentar achar conexões bloqueadas), você pode usar o comando grep para procurar algo dentro do log. Por exemplo, para procurar por entradas que mencionam "DENIED" (bloqueios)

    sudo grep "DENIED" /var/log/ufw.log

### Como habilitar ou desabilitar o log no UFW

Para ativar o log

    sudo ufw logging on

Isso vai habilitar o log no nível "baixo" por padrão (mas você pode ajustar o nível de detalhamento, se necessário).

Nível de log: O log pode ter diferentes níveis, como:
* low: Registra apenas as conexões mais importantes, com o mínimo de detalhes.
* medium: Registra mais detalhes.
* high: Registra praticamente todos os eventos, o que pode gerar muitos logs.

Para especificar o nível de detalhamento ao habilitar o log, você pode fazer assim:

    sudo ufw logging low  # Para um log mais básico
    sudo ufw logging medium  # Para um log mais detalhado
    sudo ufw logging high  # Para um log detalhado com muitos eventos

Desabilitar o log

Se você quiser desabilitar os logs (ou seja, parar de registrar qualquer evento)

    sudo ufw logging off

Isso vai desativar completamente a gravação de logs no sistema.

Para verificar se o log está ativo ou qual nível de log está configurado, você pode usar:

    sudo ufw status verbose

Isso mostrará o nível de log ativo, entre outras informações de status.
