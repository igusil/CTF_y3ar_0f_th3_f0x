# CTF_y3ar_0f_th3_f0x
walkthrough yearofthefox
******************************************************************************************
Vamos começar enumerando os serviços no host com o Nmap.

![fox0 2](https://github.com/user-attachments/assets/9497c669-b7f2-4bdd-8510-cd86bffa1d11)

******************************************************************************************
Usando o smbclient, existe um compartilhamento chamado yotf, porem precisamos da senha.

![fox0 3](https://github.com/user-attachments/assets/253ebfc0-6749-41c1-83a5-285e52290969)

******************************************************************************************
Vamos usar o enum4linux para listar os usuários disponíveis. Encontramos 2 usuários válidos: fox e rascal.

![fox0 4](https://github.com/user-attachments/assets/f865ba47-1778-459e-b986-88271d0551ad)
