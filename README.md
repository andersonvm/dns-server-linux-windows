# dns-server-linux-windows
Como configurar o serviço de DNS no Linux e no Windows

## Debian (usando BIND9)

### Passo 1: Instalação do BIND9

```
sudo apt update
sudo apt install bind9
```

### Passo 2: Configuração do arquivo de zona

Crie um arquivo de zona para o seu domínio, por exemplo, db.example.com em /etc/bind/.

```
sudo vim /etc/bind/db.example.com
```

O conteúdo pode ser semelhante a isto (substitua com suas informações):

```
;
; Arquivo de zona para example.com
;
$TTL 86400
@   IN  SOA     ns1.example.com. admin.example.com. (
        2023010101 ; Serial
        3600       ; Refresh a cada hora
        1800       ; Tentativa de retransmissão a cada meia hora
        604800     ; Válido por uma semana
        86400      ; Cache negativo por um dia
)
@       IN      NS      ns1.example.com.
@       IN      A       192.168.1.10  ; IP do servidor DNS (substitua pelo seu)
ns1     IN      A       192.168.1.10  ; Nome do servidor DNS (substitua pelo seu)
www     IN      CNAME   example.com.  ; Apontamento para www
```

### Passo 3: Configuração do arquivo de configuração do BIND

Edite o arquivo /etc/bind/named.conf.local para incluir sua zona:

```
sudo vim /etc/bind/named.conf.local
```

Adicione algo como:

```
zone "example.com" {
    type master;
    file "/etc/bind/db.example.com"; # Caminho para o seu arquivo de zona
};
```

### Passo 4: Reiniciar o BIND9

```
sudo systemctl restart bind9
```

## CentOS (usando BIND)

### Passo 1: Instalação do BIND

```
sudo yum install bind bind-utils
```

### Passo 2: Configuração do arquivo de zona

Crie um arquivo de zona para o seu domínio, por exemplo, example.com.zone em /var/named/.

```
sudo vim /var/named/example.com.zone
```

### Passo 3: Configuração do arquivo de configuração do BIND

Edite o arquivo /etc/named.conf para incluir sua zona:

```
sudo vim /etc/named.conf
```

O conteúdo pode ser semelhante a isto:

```
;
; Arquivo de zona para example.com
;
$TTL 86400
@   IN  SOA     ns1.example.com. admin.example.com. (
        2023010101 ; Serial
        3600       ; Refresh a cada hora
        1800       ; Tentativa de retransmissão a cada meia hora
        604800     ; Válido por uma semana
        86400      ; Cache negativo por um dia
)
@       IN      NS      ns1.example.com.
@       IN      A       192.168.1.10  ; IP do servidor DNS (substitua pelo seu)
ns1     IN      A       192.168.1.10  ; Nome do servidor DNS (substitua pelo seu)
www     IN      CNAME   example.com.  ; Apontamento para www
```

### Passo 3: Configuração do arquivo de configuração do BIND

Adicione a zona ao arquivo named.conf:

```
sudo vim /etc/named.conf
```

```
zone "example.com" IN {
    type master;
    file "example.com.zone";
};
```

### Passo 4: Reiniciar o BIND

```
sudo systemctl restart named
```

Ambos os sistemas requerem a configuração correta dos arquivos de zona, onde você define os registros DNS (A, CNAME, MX, etc.) para o seu domínio. Lembre-se de ajustar os caminhos e nomes de arquivos conforme necessário.

Depois de configurar o BIND nos sistemas Debian e CentOS, você pode verificar se o servidor DNS está funcionando usando ferramentas como nslookup ou dig. Certifique-se de permitir o tráfego de DNS (porta 53 UDP/TCP) através do firewall, se estiver habilitado.

Estas são diretrizes básicas; a configuração exata pode variar dependendo da sua rede, domínio e requisitos específicos.

## No Windows Server, aqui está um guia básico:
## Passo 1: Instalação do Serviço DNS

Abra o "Gerenciador do servidor": Clique no ícone do Windows e selecione "Gerenciador do servidor".

Selecione "Adicionar funções e recursos": Na janela do Gerenciador do servidor, clique em "Adicionar funções e recursos" no painel de tarefas.

Assistente de Adicionar Funções e Recursos: Siga as instruções do assistente até chegar à página de seleção de funções. Marque a caixa "Serviços de Domínio de Diretório" e, em seguida, selecione "Serviços de DNS".

Conclua a instalação: Continue com o assistente, aceite as configurações padrão ou ajuste conforme necessário e conclua a instalação.

## Passo 2: Configuração do DNS

Abrir o "Gerenciador de DNS": Depois de instalar o serviço DNS, abra o "Gerenciador de DNS". Você pode encontrá-lo no "Ferramentas Administrativas" no menu Iniciar ou no próprio "Gerenciador do servidor".

Criar Zona de Pesquisa Direta: Clique com o botão direito em "Zonas de Pesquisa Direta" e escolha "Nova Zona". Siga o assistente para criar uma nova zona, inserindo o nome do domínio desejado.

Configurar Registros DNS: Dentro da zona criada, você pode adicionar registros DNS necessários, como registros A (para mapear nomes de host para endereços IP), registros MX (para configuração de servidor de e-mail), entre outros, de acordo com suas necessidades.

## Passo 3: Configuração de Zonas Reversas (Opcional)

Criar Zona de Pesquisa Reversa: No "Gerenciador de DNS", clique com o botão direito em "Zonas de Pesquisa Reversa" e selecione "Nova Zona". Siga o assistente e insira o intervalo de endereços IP para a qual a zona reversa se aplica.

Adicionar Registros PTR: Dentro da zona reversa, adicione registros PTR para mapear endereços IP para nomes de host. Isso é útil para resolver nomes a partir de endereços IP.

## Passo 4: Configuração de Encaminhadores (Opcional)

Abra as Propriedades do Servidor DNS: Clique com o botão direito no servidor DNS no "Gerenciador de DNS" e selecione "Propriedades".

Defina os Encaminhadores: Vá para a guia "Encaminhadores" e adicione os endereços IP dos servidores DNS dos provedores ou da rede que você deseja usar para resolver consultas DNS que seu servidor não pode resolver sozinho.

## Passo 5: Teste e Verificação

Verifique as Configurações: Certifique-se de que os registros DNS estejam corretamente configurados, especialmente os registros SOA (Start of Authority) e NS (Name Server).

Teste de Resolução DNS: Use ferramentas como o comando nslookup no prompt de comando para testar a resolução de nomes de domínio em seu servidor DNS.
Lembre-se de que a configuração exata pode variar dependendo da sua infraestrutura e requisitos específicos. Este guia oferece uma visão geral dos passos necessários para configurar o serviço DNS no Windows Server
