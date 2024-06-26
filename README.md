# born2beroot

APTITUDE é uma ferramenta de gerenciamento de pacotes com uma interface mais sofisticada e resolução de dependências mais avançada, enquanto APT é mais simples e amplamente utilizado.
SELinux e AppArmor são sistemas de segurança de controle de acesso obrigatório, com SELinux oferecendo um controle mais detalhado e complexo, enquanto AppArmor é mais simples de configurar e usar

---
UTILIZADOR ROOT

HOSTNAME : cde-paiv42

ROOTPASSWORD : 1103Mota?

HOSTNAME : cde-paiv

ROOTPASSWORD : Mota1103?

ENCRYPTION PASSPHRASE : 42school

---

### CONFIGURATION VIRTUAL MACHINE

Instalação do sudo e configuração dos utilizadores e grupos

1 - colocar Su no terminal e introduzir a palavra-passe. Aceder ao utilizador root, colocar o comando apt install sudo para instalar os pacotes necessários.

2 - verificar se instalámos correctamente o sudo, voltar a entrar no utilizador root e introduziremos o comando sudo -V

3 - precisamos criar um novo grupo chamado USER42. Para criar sudo addgroup user42

4 - Com o comando sudo adduser group, iremos incluir o utilizador no grupo. Devemos incluir o utilizador nos grupos sudo e user42.a

---

### INSTALLATION AND CONFIGURATION TO SSH

SSH É o nome de um protocolo e do programa que o implementa, cuja função principal é o acesso remoto a um servidor através de um canal seguro no qual toda a informação é encriptada.

1 - A primeira coisa que faremos é sudo apt update para actualizar os repositórios por nós definidos no ficheiro /etc/apt/sources.list

2 - instalaremos a principal ferramenta de conectividade para o login remoto com o protocolo SSH, esta ferramenta é OpenSSH. Para a instalar, introduza o comando sudo apt install openssh-server. Na mensagem de confirmação coloque Y, depois aguarde que a instalação termine.
Sudo service ssh status para verificar seu funcionamento

3 - foram criados alguns ficheiros que temos de configurar. Para tal, utilizaremos Nano ou se preferir outro editor de texto. O primeiro ficheiro a editar é /etc/ssh/sshd_config
devemos modificar as seguintes linhas:
➤ #Port 22 -> Port 4242  
➤ #PermitRootLogin prohibit-password -> PermitRootLogin no
Temos agora de editar o ficheiro /etc/ssh/ssh_config➤ #Port 22 -> Port 4242
reiniciar o serviço ssh para actualizar as modificações feitas o comando sudo service ssh restart, uma vez reiniciado, vamos olhar para o estado actual com sudo service ssh status e para confirmar que as alterações foram feitas no ouvinte do servidor, a porta 4242 deve aparecer

---

### INSTALLATION AND CONFIGURATION TO UFW

UFW (Uncomplicated Firewall) é uma ferramenta simples para gerenciar regras de firewall em sistemas Linux, facilitando a segurança da rede.

1 - instalar o UFW, para o fazer utilizaremos o comando  sudo apt install ufw, depois escreveremos um y para confirmar que o queremos instalar e esperar que termine.

2 - Uma vez instalada, devemos activá-la, para o fazer devemos colocar o seguinte comando sudo ufw enable e depois devemos indicar que a firewall está activa.

3 ◦ Agora o que precisamos de fazer é permitir que a nossa firewall permita ligações através da porta 4242. Fá-lo-emos com o seguinte comando sudo ufw allow 4242.

### CONFIGURATION PASSWORD STRONG FOR TO SUDO

1 ◦ Vamos criar um ficheiro no caminho /etc/sudoers.d/. Decidi chamar o meu ficheiro sudo_config, pois é aqui que a configuração da senha será armazenada. O comando exacto para criar o ficheiro é touch /etc/sudoers.d/sudo_config.

2 ◦ Devemos criar o directório sudo no caminho /var/log porque cada comando que executamos com o sudo, tanto a entrada como a saída devem ser armazenadas nesse directório. Para o criar utilizaremos o comando mkdir /var/log/sudo. 

3 ◦ Devemos editar o ficheiro criado no passo 1. Como disse antes, pode usar qualquer editor que quiser, mas eu usarei nano. Comando para editar o ficheiro: nano /etc/sudoers.d/sudo_config.
1 Defaults  passwd_tries=3

2 Defaults  badpass_message="password error"

3 Defaults  logfile="/var/log/sudo/sudo_config"

4 Defaults  log_input, log_output

4 Defaults  iolog_dir="/var/log/sudo"

5 Defaults  requiretty

6 Defaults  secure_path="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/snap/bin"

1 Número de tentativas em caso de entrada de uma senha errada

2 Mensagem que será exibida na tela caso a senha inserida esteja incorreta

3 Arquivo no qual todos os comandos sudo serão registrados

4 Para que o comando seja executado com sudo, tanto a entrada quanto a saída fique arquivada no diretório especificado

5 Para ativar o modo TTY

6 Para restringir os diretórios utilizáveis por sudo

---

### STRONG PASSWORD POLICY CONFIGURATION

1 - edição do ficheiro login.defs
Comando: nano /etc/login.defs

2 - Uma vez que estamos a editar o ficheiro, modificaremos os seguintes parâmetros:
PASS_MAX_DAYS 99999 -> PASS_MAX_DAYS 30
PASS_MIN_DAYS 0 -> PASS_MIN_DAYS 2
PASS_MAX_DAYS: Este é o tempo de expiração da palavra-passe. O número corresponde a dias.
PASS_MIN_DAYS: O número mínimo de dias permitido antes de alterar uma palavra-passe.
PASS_WARN_AGE: O utilizador receberá uma mensagem de aviso indicando que o número de dias especificado permanece até a sua senha expirar.

3 ◦ Para continuar com a configuração temos de instalar os seguintes pacotes com este comando sudo apt install libpam-pwquality, depois colocar Y para confirmar a instalação e esperar que termine.

4 ◦ A próxima coisa a fazer é voltar atrás e editar um ficheiro e modificar algumas linhas. Faremos nano /etc/pam.d/comnon-password

5 ◦ Após nova tentativa=3, devemos acrescentar os seguintes comandos:

minlen=10 ➤ O número mínimo de caracteres que a senha deve conter.

ucredit=-1 ➤ Deve conter pelo menos uma letra maiúscula. Colocamos o - como deve conter pelo menos um caracter, se colocarmos + queremos dizer no máximo esses caracteres.

dcredit=-1 ➤ Deve conter pelo menos um dígito.

lcredit=-1 ➤ Deve conter pelo menos uma letra minúscula.

maxrepeat=3 ➤ Não se pode ter o mesmo carácter mais de 3 vezes seguidas.

reject_username ➤ Não pode conter o nome do utilizador.

difok=7 ➤ Deve ter pelo menos 7 caracteres que não façam parte da senha antiga.

enforce_for_root ➤ Iremos implementar esta política para o utilizador de raiz.

### Conectar via SSH

1 ◦ Para ligar via SSH temos de fechar a máquina, abrir a VirtualBox e clicar na configuração. 

2 ◦ Uma vez na configuração devemos clicar na secção network , clicar em advanced para nos mostrar mais opções e clicar port forwarding. 

3 ◦ Clique no seguinte emoticon : negative_squared_cross_mark :para adicionar uma regra de reencaminhamento 

4 ◦ Finalmente, acrescentaremos o porto 4242 ao anfitrião e convidado. Os IP's não são necessários. Clique no botão de aceitação para aplicar as alterações
➤ Para nos podermos ligar à máquina virtual a partir da máquina real, temos de abrir um terminal na máquina real e escrever ssh cde-paiv@  -p 4242, pedir-nos-á a palavra-passe do utilizador e assim que a introduzirmos veremos o login a verde e isso significa que estaremos ligados.

---
---

### Valuation born2beroot

## Funcionamento Básico de uma Máquina Virtual

Uma VM emula um sistema computacional completo, permitindo a execução de múltiplos sistemas operacionais em uma única máquina física. É gerenciada por um hipervisor (software que cria e gerencia VMs).

# Benefícios das Máquinas Virtuais
Isolamento
Melhor Utilização de Recursos
Flexibilidade
Redução de Custos

# Diferenças Básicas entre Rocky e Debian
Rocky Linux: focado em estabilidade e segurança corporativa, usa dnf para gerenciamento de pacotes.

Debian: Conhecido por sua estabilidade e ampla gama de pacotes, base para várias outras distribuições, usa apt.

Diferença entre aptitude e apt

aptitude: Interface avançada e robusta para gerenciamento de pacotes.

apt: Interface moderna e simplificada para uso diário.

O que é APPArmor

Módulo de segurança do kernel Linux que restringe capacidades de programas individuais com perfis predefinidos, aumentando a segurança do sistema.

comando de avaliação
1 ◦ Verificar se não há nenhuma interface gráfica em uso.
ls /usr/bin/*session

2 ◦ Verificar se o serviço UFW está a ser utilizado.
sudo ufw status
Sudo service ufw status

3 ◦ Verificar se o serviço SSH está a ser utilizado.
sudo service ssh status

4 ◦ Verifique se está a utilizar o sistema operativo Debian ou Centos.
uname -v
5 ◦ Verifique se o seu utilizador está nos grupos "sudo" e "user42".
getent group sudo
getent group user42

6 ◦ Criar um novo utilizador e mostrar que segue a política de palavra-passe que criámos.
sudo adduser name_user
introduzir uma palavra-passe que siga a política.

7 ◦ Criamos um novo grupo chamado "evaluating".
sudo addgroup evaluating

8 ◦ Acrescentar o novo utilizador ao novo grupo.
sudo adduser name_user evaluating

9 ◦ Verificar se o nome da máquina está correcto para o cde-paiv42.  hostname

10 ◦ Modifique o nome de anfitrião para substituir o seu login pelo login do avaliador.
sudo nano /etc/hostname introduzir o nome user do avaliador
sudo nano /etc/hosts e substituir o nosso login pelo novo. 
Reiniciar a máquina.

11 ◦ Verificar se todas as partições estão como indicado no subject.
lsblk

12 ◦ Verificar se o sudo está instalado.
which sudo
Para uma melhor utilização, podemos utlizar o seguinte comando:
dpkg -s sudo

13 ◦ Introduzir o novo utilizador no grupo sudo.
sudo adduser name_user sudo Verificamos se está no grupo.
getent group sudo

14 ◦ Mostra a aplicação das regras impostas ao sudo pelo subject.
nano /etc/sudoers.d/sudo_config

15 ◦ Mostra que o caminho /var/log/sudo/ existe e contém pelo menos um ficheiro, no qual se deve ver um histórico dos comandos utilizados com o sudo.
Cd /var/log/sudo/
Ls
Cat sudo_config
Executar um comando com sudo e verificar se o ficheiro está actualizado.
sudo nano hello42world

16 ◦ Verificar se o programa UFW está instalado na máquina virtual e verificar se está a funcionar correctamente.
dpkg -s ufw  sudo
Verificar se está ativo
service ufw status

17 ◦ Listar as regras activas na UFW, apenas a regra para a porta 4242 deve aparecer.
sudo ufw status numbered

18 ◦ Criar uma nova regra para o porto 8080. Verifique se foi adicionada às regras activas e depois pode apagá-la.
sudo ufw allow 8080 para o criar 
sudo ufw status numbered para verificar
Para eliminar a regra, devemos usar o comando sudo ufw delete num_rule
Verificamos se foi eliminada e vemos o número da regra seguinte a ser eliminada.      
Eliminamos novamente a regra.
Verificamos que só nos restam as regras necessárias no assunto.

19 ◦ Verificar se o serviço ssh está instalado na máquina virtual, se funciona correctamente e se só funciona no porto 4242.
which ssh
para verificar se estar ativo
sudo service ssh status
20 ◦ Utilize ssh para iniciar sessão com o utilizador recém-criado. Certifique-se de que não pode utilizar o ssh com o utilizador de raiz.
Tentamos ligar-nos via ssh com o utilizador raiz, mas não temos permissões.
Ligamo-nos via ssh ao novo utilizador com o comando ssh newuser@10.12.250.109 -p 4242

21 ◦ Modificar o tempo de execução do guia de 10 minutos para 1 minuto.
Executar o seguinte comando para modificar o ficheiro crontab
sudo crontab -u root -e

22 ◦ Finalmente, fazer o guia parar de funcionar quando o servidor tiver começado, mas não modificar o guia.
sudo /etc/init.d/cron stop
Se quisermos que volte a funcionar:
sudo /etc/init.d/cron start

 
