1. Kernel e Boot loader
	O usuário vagrant está sem permissão para executar comandos root usando sudo. Sua tarefa consiste em reativar a permissão no sudo para esse usuário.
	Dica: lembre-se que você possui acesso "físico" ao host.

		1- quebrar a senha de root da máquina "fisica" 
			- acessar o menu de edição de configuração de boot do CentOs
			- na linha que inicia com 'linux ($root)' alterar as informações 'ro' para 'rw init=sysroot/bin/sh' e pressionar 'CTRL+x'
			- no prompt entrar com os comandos: 'chroot /sysroot/' e depois 'passwd root' para alterar a senha do usuário 'root'
			- executar o comando 'touch /.autorelabel' para habilitar a reclassificação do SELinux.
			- sair com o comando 'exit' e em seguida usar o comando 'reboot' para reiniciar o servidor.
		2- fazer logon com o usuário 'root' e alterar a senha do usuario 'vagrant' usando o comando 'passwd vagrant' alteramos a senha do usuário 'vagrant'
		3- ainda com o usuário root logado, incluir o usuário 'vagrant' no grupo 'wheel' para que ele volte a ter permissão de 'sudo' -> 'usermod -aG wheel vagrant'
		4- identificar o ip do servidor com o comando -> 'ip addr show dev eth1'

	 ![image](https://user-images.githubusercontent.com/109318929/188898147-80f89931-d4d0-4264-9453-268af691af5e.png)

2. Usuários
	2.1 Criação de usuários
	Crie um usuário com as seguintes características:
	     ->	username: getup (UID=1111)
	     ->	grupos: getup (principal, GID=2222) e bin
	     ->	permissão sudo para todos os comandos, sem solicitação de senha.  	             
		     	               	             
		1- criar o usuário 'getup' com o UID 1111 -> 'useradd -u 1111 getup'
		2- incluir o usuario 'getup' no grupo 'wheel' para ter permisssão de 'sudo' -> 'usermod -aG wheel getup'
		3- incluir o usuário 'getup' no grupo bin -> 'usermod -aG bin getup'
		4- modificar o GID do grupo getup para 2222 -> 'groupmod -g 2222 getup'
		5- incluir o usuário 'getup' no grupo getup -> 'usermod -aG getup getup'
		6- usando o comando 'visudo' incluir a informação do usuário 'getup' com os dados 'getup ALL=(ALL) NOPASSWD: ALL'

	![image](https://user-images.githubusercontent.com/109318929/188898632-476f385c-eeb1-43fe-bc94-b8d8bd75ce82.png) ![image](https://user-images.githubusercontent.com/109318929/188898653-07df3e45-2220-4ac1-9ac1-1ca985371cba.png)


3. SSH
	3.1 Autenticação confiável
	O servidor SSH está configurado para aceitar autenticação por senha. No entanto esse método é desencorajado pois apresenta alto nivel de fragilidade.
	Voce deve desativar autenticação por senhas e permitir apenas o uso de par de chaves:
	
		1- remover a permissão de login com senha, editando o arquivo 'sshd_config', no caminho '/etc/ssh' e localizar a entrada 'PasswordAuthentication yes'.
		2- alterar para 'no' e salvar o arquivo. em seguida reiniciar o serviço de ssh -> 'systemctl restart sshd'
		
	![image](https://user-images.githubusercontent.com/109318929/188900047-a8697d9e-1870-4114-891f-10a65a4ac734.png)

	3.2 Criação de chaves
	Crie uma chave SSH do tipo ECDSA (qualquer tamanho) para o usuário vagrant. Em seguida, use essa mesma chave para acessar a VM:
	
		1- no client, criar uma pasta oculta '.ssh' no perfil do usuario e executar o comando 'ssh-keygen -t ecdsa -C "vagrant@192.168.0.84"'
		2- ainda no cliente, usar o comando -> 'ssh-copy-id vagrant@192.168.0.84', informar a senha do usuario 'vagrant'
	
	![image](https://user-images.githubusercontent.com/109318929/188900161-53c15c37-9bc5-4a80-bd9f-ae91fd6c5e10.png)

	3.3 Análise de logs e configurações ssh
	Utilizando a chave do arquivos id_rsa-desafio-devel.gz.b64 deste repositório, acesso a VM com o usuário devel:
	
		1 fiz a instalação do git na maquina do desafio e baixei a chave que foi disponibilizada. 
		Em seguida, decodifiquei e descompactei a chave com o comando 'base64 -d id_rsa-desafio-linux-devel.gz.b64 | gzip -d > id_rsa'.
		Dessa forma o conteúdo do arquivo pode ser copiado para um novo arquivo, resolvendo assim o problema relatado no log. 
		A chave esta com carriage-return e devia estar com a newline.
		2- fiz também a mudança das permissões do arquivo authorized_keys para permissão de leitura e gravação apenas para o dono do arquivo.
		3- Usando o comando 'chmod 600 authorized_keys'.
		4- Em seguida, fiz um reset da senha do usuário devel, para conseguir fazer a cópia da chave para o computador cliente.
		5- Feito isso, usando os mesmos passos dos item 1, foi possível fazer logon com a conta de devel, como podemos ver abaixo
	
	![image](https://user-images.githubusercontent.com/109318929/188900493-e2c4666a-2299-424f-bd62-1712567de897.png)

	

4. Systemd
	Identifique e corrija os erros na inicialização do servico nginx. Em seguida, execute o comando abaixo (exatamente como está) e apresente o resultado. Note que o comando não deve falhar.
	curl http://127.0.0.1
	
		1- para acessar a url http://127.0.0.1, deve-se primeiramente, analisar o arquivo nginx.conf e procurar por erros de configuração. 
		2- Pode-se perceber a falta de um ';' no final da linha 42 'root         /usr/share/nginx/html'.
		3- alterar a porta de conexão para 80 (padrão http), nas linhas 39 e 40 e salvar o arquivo

	![image](https://user-images.githubusercontent.com/109318929/188902229-e83062dc-6c40-480d-95b9-ca82f9208874.png)

		1- reiniciar o serviço do nginx, com o comando -> 'systemctl restart nginx'
		2- colocar o serviço para iniciar automaticamente -> 'systemctl enable nginx'
		3- testar o acesso usando o comando 'curl http://127.0.0.1' a saída será 'Duas palavrinhas pra você: para, béns!'
		
	![image](https://user-images.githubusercontent.com/109318929/188902759-e4f06b10-c1c1-4874-b53f-b377029c7c20.png)



5. SSL
	5.1 Criação de certificados
	Utilizando o comando de sua preferencia (openssl, cfssl, etc...) crie uma autoridade certificadora (CA) para o hostname desafio.local. Em seguida, utilizando esse CA para assinar, crie um certificado de web server para o hostname www.desafio.local.


	5.2 Uso de certificados	
	Utilizando os certificados criados anteriormente, instale-os no serviço nginx de forma que este responda na porta 443 para o endereço www.desafio.local. Certifique-se que o comando abaixo executa com sucesso e responde o mesmo que o desafio 4. Voce pode inserir flags no comando abaixo para utilizar seu CA.
	curl https://www.desafio.local


6. Rede
	6.1 Firewall
	Faço o comando abaixo funcionar:
	ping 8.8.8.8
	
	 	 1- nada precisou ser feito. o comando 'ping' foi executado com sucesso, tendo como saída o resultado abaixo:	
	
	![image](https://user-images.githubusercontent.com/109318929/188902903-b4b898e9-bf33-42af-bc16-9293eb4ef46b.png)	

  6.2 HTTP
	Apresente a resposta completa, com headers, da URL https://httpbin.org/response-headers?hello=world
	
		1- usando o comando ‘curl -i https://httpbin.org/response-headers?hello=world’, foi exibido o conteúdo abaixo:
		
![image](https://user-images.githubusercontent.com/109318929/188903037-275c7baa-e134-4f36-a61d-05e235d1d89d.png)

  6.3 Logs
  Configure o logrotate para rotacionar arquivos do diretório /var/log/nginx

		1- acessar a pasta '/etc/logrotate.d'.
		2- Criar os arquivos com as informações de log de acesso e erro do nginx, 'nginx_acess' e 'nginx_error'.
		3- Configurar para rotacionar diariamente, como mostrado abaixo:
		4- testar as configurações com o comando 'logrotate -d /etc/logrotate.conf'

![image](https://user-images.githubusercontent.com/109318929/188903433-c37cbd77-9bb0-4cf8-a41e-f007fed503f1.png)

![image](https://user-images.githubusercontent.com/109318929/188903487-5012e457-a233-42bf-8b81-8344f5c98639.png)


7. Filesystem
	7.1 Expandir partição LVM
	Aumente a partição LVM sdb1 para 5Gi e expanda o filesystem para o tamanho máximo.
	
		Expandindo o sdb1
		1- usando o comando 'df -hT' para identificar a partição e o tipo de filesystem utilizado, nesse caso: 
		/dev/mapper/data_vg-data_lv e ext4, respectivamente.
		2- usando o comando 'vgdisplay' para identificar o nome do volume group que sofrerá a expansão, nesse caso o 'data_vg'
		3- usando o comando 'fdisk /dev/sdb' para acessar as configurações de partição do disco informado.
		4- depois usamos as opções na ordem: n<nova partição>, p<primaria> com +5000M, t <mudar o tipo de partição para 8e LinuxLVM>, w 
		<gravar tabela de partição>
		5- depois usamos novamente as opções na ordem: n<nova partição>, p<primaria> com +4000M, t <mudar o tipo de partição para 8e LinuxLVM>, 
		w <gravar tabela de partição>
		6- executar o comando 'vgextend data_vg /dev/sdb3' para utilizar o espaço da partição de 4G recém criada.
		7- executar o comando 'lvextend -L +4000M /dev/mapper/data_vg-data_lv' para extender o volume em 4G.
		8- executar o comando 'resize2fs /dev/mapper/data_vg-data_lv'
	
	7.2 Criar partição LVM
	Crie uma partição LVM sdb2 com 5Gi e formate com o filesystem ext4.
	
		9- depois vamos criar a partição sdb2 com LVM, usando os comandos: 'pvcreate /dev/sdb2' e em seguida 'vgcreate data_vg2 /dev/sdb2'
		10- a seguir, executar o comando 'vgextend data_vg2 /dev/sdb2' e em seguida 'lvcreate -L 4.87G -n Dados data_vg2'
		11- formatar a nova partição utilizando ext4, com o comando 'mkfs.ext4 /dev/mapper/data_vg2-Dados' 
		12- criar uma nova pasta com o comando 'mkdir /mnt/Dados'
		13- montar a nova partição usando o comando 'mount /dev/mapper/data_vg2-Dados /mnt/Dados'
		14- para manter a montagem automatica, atualizar o arquivo '/etc/fstab', com as informações: '/dev/mapper/data_vg2-Dados /mnt/Dados ext4 defaults 0 0'

	
	7.3 Criar partição XFS
	Utilizando o disco sdc em sua todalidade (sem particionamento), formate com o filesystem xfs.
	
		15- usando o comand 'fdisk /dev/sdc', vamos criar o disco com apenas 1 partição e com todo o espaço disponível.
		16- depois usamos as opções na ordem: n<nova partição>, p<primaria> "aceitar todas as opções default como padrão, t 
		<mudar o tipo de partição para 8e LinuxLVM>, w <gravar tabela de partição>
		17- como não é padrão do CentOs, deve-se instalar o pacote com as ferramentas para manipulação de xfs, usando o comando 'yum install xfsprogs -y'
		18- usando o comando 'mkfs.xfs /dev/sdc' para formatar o disco com o formato de arquivos XFS.
		19- criar uma nova pasta com o comando 'mkdir /mnt/Dados2'
		20- montar a nova partição usando o comando 'mount /dev/sdc /mnt/Dados2'
		21- para manter a montagem automática, atualizar o arquivo '/etc/fstab', com as informações: '/dev/sdc /mnt/Dados2 xfs defaults 0 0'

![image](https://user-images.githubusercontent.com/109318929/188904675-53d600a8-ff51-4078-9d70-9ef0cd9307ea.png)

