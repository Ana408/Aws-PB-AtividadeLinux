# Aws-PB-AtividadeLinux
AWS - DevSecOps - 2023 -  Atividade Linux
Ana Karine Nobre Bezerra

Atividade Prática
AWS: 
- Gerei uma chave pública para acesso a máquina;  
- Criei a VPC, sub-rede e gateway da internet e os deixei vinculados ;
- Criei 1 instância EC2 com o sistema operacional Amazon Linux 2 (Família t3.small, 16 GB SSD) e vinculei a VPC;  
- Gerei 1 elastic IP e anexar à instância EC2; 
- Liberei as portas de comunicação para acesso público: (22/TCP, 111/TCP e UDP, 2049/TCP/UDP, 80/TCP, 443/TCP);
- Realizei a conexão via ssh;
- Utilizei o power shell em modo administrador e executei o seguinte código:
- ssh -i “/caminho/para/sua-chave-privada.pem” ec2-user@seu-endereco-ip-publico


Passo 1 - Para instalar um servidor NFS no Amazon Linux 2, seguir os seguintes passos:

1. Para instalar o Servidor NFS:
Utilizei o  pacote `nfs-utils` que é necessário para usar o NFS. Instalei com o seguinte comando:
   sudo yum install nfs-utils -y
   
2. Habilitar e Iniciar o Serviço NFS:
 Para habilitar e iniciar o serviço NFS,  primeiro, tive que habilitar o serviço NFS, com este comando:
   sudo systemctl enable nfs-server
  
3.  Em seguida, iniciei o serviço NFS:   
   sudo systemctl start nfs-server

4. Agora verifique se o NFS está ativo:
    sudo systemctl status nfs-server

5. Atualize o Sistema:   Atualizar os repositórios de pacotes e atualizar o sistema executando o seguinte comando:
   sudo yum update -y

6. Tornar administrador:  comando  que deixa como ‘super usuário’ e evita de repetição nos próximos passos:
   sudo su
   
7. Configurar as Exportações NFS:
     	Configurei as exportações do NFS, que determinam quais diretórios o sistema de arquivos será compartilhado via NFS. Para realizar as exportações utilizei o arquivo  `/etc/exports` com o editor ‘nano’ para editar o arquivo.
   nano /etc/exports

8. Compartilhamento de diretórios do NFS
 Para compartilhar o diretório `/var/nfs_share` com permissão de leitura e gravação para todos os clientes NFS:
     /var/nfs_share *(rw,sync,no_root_squash)

9.Criar um diretório dentro do filesystem do NFS com nome do usuário:
Com o comando mkdir para criar um diretório dentro do filesystem do NFS. 
   mkdir /caminho/do/seu/diretorio/seu_nome

10.Em seguida, configure as exportações do NFS no arquivo /etc/exports.  
 Adicione uma linha para compartilhar o diretório. Após adicionar a linha, pressione CTRL+O e ENTER para salvar e depois CTRL+X para sair do arquivo.
    /caminho/do/seu/diretorio *(rw,sync,no_root_squash

11. Habilitar o Serviço Portmapper:
O serviço Portmapper é para o funcionamento do NFS. No caso, é necessário que ele esteja habilitado e em execução:
    systemctl enable rpcbind
    systemctl start rpcbin

12. Aplicar as Configurações de Exportação:
Após editar o arquivo `/etc/exports`, aplique as novas configurações de exportação executando o seguinte comando:
   exportfs -a

Parte 2 - Para instalar o servidor web Apache 2 no Amazon Linux 2, 

1. Atualize o Sistema:
Execute o seguinte comando para atualizar os repositórios de pacotes e atualizar o sistema:
    yum update -y
  
2. Instale o Apache 2:
Instalei com o nome do pacote do Apache (‘httpd’) da instância. Executei o comando para iniciar a instalação:
    yum install httpd -y

3. Inicie o Serviço Apache:
Após a instalação bem-sucedida, inicie o serviço Apache2:
   systemctl start httpd
 
4. Habilite o Apache para Inicialização Automática:
Para garantir que o Apache seja iniciado automaticamente sempre que a instância for reiniciada, execute o comando:
    systemctl enable httpd 

5. Teste o Apache:
 Abra um navegador da web e insira o endereço IP público da sua instância Amazon Linux 2. Verificar o status do Apache no servidor usando o seguinte comando:
 systemctl status httpd

*****Verifique se o status é "ativo (running)". 

Passo 3. Criação de um script de validação para o serviço Apache:

1. Crie um diretório onde você deseja armazenar o script e os relatórios:
 mkdir -p /caminho/do/seu/diretorio/nfs

2. Criar o arquivo do script  dentro desse diretório que  utilize um editor nano para criar o arquivo “.sh”
    nano check_apache.sh

3. Adicione o seguinte código ao script criado: (arquivo no repositório)

4. Torne o script executável:
    chmod +x /caminho/do/seu/diretorio/nfs/nome do arquivo
   
5. O comando dá permissão para os doc criados para o script ter acesso do documento.
   chmod 777 servico_offline.txt
   chmod 777 servico_online.txt

Passo 4. Preparar a execução do script a cada 5 minutos:

1. Usar o cron para edição:
Abre o editor para consultar os documentos 
   crontab -e
   
2. Adicionar execução no cron
Adicione a seguinte linha ao seu arquivo crontab para executar o script a cada 5 minutos:
*/5 * * * * /bin/sh /home/ec2-user/nomedoarquivo

 3.Finalize o arquivo cron.
	Para digitar no arquivo cron, pressione a tecla A, após terminar a edição, pressione ESC e depois dê : e digite wq e pressione ENTER.
Agora, o script será executado de forma automatizada a cada intervalo de 5 minutos.
Ele fará uma verificação do status do serviço Apache e vai registrar o resultado em dois arquivos: servico_online.txt e servico_offline.txt

Para verificar o status do Apache e se o script está funcionando utilize esses comandos: 
	cat servico_online.txt 
	cat servico_offline.txt

Para mudar o fuso horário da Instância, execute o seguinte comando:
 timedatectl set-timezone America/Fortaleza


