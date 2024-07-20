# CTF_y3ar_0f_th3_f0x
walkthrough yearofthefox
******************************************************************************************
Vou começar enumerando os serviços no host com o Nmap.

![fox0 2](https://github.com/user-attachments/assets/9497c669-b7f2-4bdd-8510-cd86bffa1d11)

******************************************************************************************
Usando o smbclient, existe um compartilhamento chamado yotf, porem precisamos da senha.

![fox0 3](https://github.com/user-attachments/assets/253ebfc0-6749-41c1-83a5-285e52290969)

******************************************************************************************
Vou usar o enum4linux para listar os usuários disponíveis. Encontramos 2 usuários válidos: fox e rascal.

![fox0 4](https://github.com/user-attachments/assets/f865ba47-1778-459e-b986-88271d0551ad)

******************************************************************************************
Se no serviço da web não revelar senhas, precisaremos usar força bruta

![fox0 5](https://github.com/user-attachments/assets/454d2602-828d-4e07-a9e9-e710ece3911a)

sou imediatamente bloqueados, pois todo o caminho é protegido por senha usando uma autenticação básica.

******************************************************************************************
Vou tentar forçar a autenticação com hydra usando rascal como usuário

![fox0 6](https://github.com/user-attachments/assets/075d4faf-9659-4af9-b5e1-d13ebf1e3cc8)

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
![fox1 2](https://github.com/user-attachments/assets/cb664284-5903-4e80-b3c6-d318b4478660)

******************************************************************************************
a requisição com o shell reverso codificado em base64
![fox1 3](https://github.com/user-attachments/assets/250dd357-101b-466f-ac63-e40b1d9e6c26)
