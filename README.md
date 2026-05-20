# IDOR Lab

## 🃏 Descrição  
Este projeto consiste em explorar a vulnerabilidade **IDOR (Insecure Direct Object Reference)**, em um ambiente de laboratório seguro.

## Objetivo  
- Compreender e explorar falhas relacionadas a **IDOR**.    

## Ferramentas Utilizadas  
- Burp Suite  
- OWASP Juice Shop (docker)
- OWASP BWA (máquina virtual - Oracle VirtualBox)
- Kali Linux  

## Passos do Projeto  
1. Configuração do ambiente de teste
2. Interceptação com Burp Suite  
3. Exploração da vulnerabilidade **IDOR**

## 💻 Evidências  

### PASSO 1 : 
![Máquinas no Oracle VirtualBox](https://github.com/user-attachments/assets/d9798ef2-480e-4888-96a0-9ab906e1ed51)

Para o primeiro exemplo eu utilizei o Sistema Operacional Kali Linux e a OWASP BWA, ambos virtualizados. A OWASP BWA se trata de uma máquina com diversas aplicações vulneráveis, feitas exclusivamente para testes de penetração.


Para que as máquinas pudessem se comunicar, configurei as interfaces de rede de ambas no modo "Internal Network", pois preciso da comunicação somente na rede local para este laboratório.

![Modo da interface de rede](https://github.com/user-attachments/assets/9a416768-fdf2-42e1-afb0-d9c3ab346edf)


E para que ocorra a comunicação, ambas as máquinas precisam receber um endereço ip na mesma faixa. O ip da rede é 192.168.0.0/24 portanto as máquinas devem receber endereços ip dentro da faixa 192.168.0.1 - 192.168.0.254 . 

OWASP BWA (192.168.0.1): 

![Configurando ip OWASP BWA](https://github.com/user-attachments/assets/3bbcff97-8576-420c-887e-bb442caab096)

A seta vermelha indica o comando digitado para setar o ip na interface de rede desejada, e a seta verde mostra que a interface recebeu o ip com sucesso.


KALI LINUX (192.168.0.2):

![Configurando ip Kali Linux](https://github.com/user-attachments/assets/6e3b4797-e705-41cb-bb0a-4c3187eea1fb)

No Kali Linux eu já havia configurado previamente o ip, a seta verde indica o ip já configurado na interface.

### PASSO 2 :

Executei o Burp Suite, abri o navegador e digitei o ip da OWASP BWA.

![Burp Suite](https://github.com/user-attachments/assets/fe79e038-de4b-4387-b942-9f24b1206cdf)

![Acessando a OWASP BWA](https://github.com/user-attachments/assets/16f2c4e0-a6f2-497b-8bb3-f6f7f9ecb6e9)

A aplicação que usei neste exemplo, foi a OWASP Mutillidae II

![Aplicação OWASP BWA](https://github.com/user-attachments/assets/fa2c847e-7978-4f26-ad3a-44e0f93235ad)

Na tela de login coloquei as credenciais de uma conta já criada previamente, e ativei o modo de interceptação do Burp Suite. Com o modo ativado, após eu apertar em "login", o Burp me mostra a requisição HTTP antes de ser enviada, mas neste caso é preciso interceptar a resposta antes de chegar no navegador, pois é nela que recebemos o id de sessão.

![login](https://github.com/user-attachments/assets/bfdede55-dcd3-4358-84b1-4b1fb66f686a)

![response](https://github.com/user-attachments/assets/a8011ece-15d2-4f36-b9b5-db23154f49e5)

### PASSO 3:

![id de sessão](https://github.com/user-attachments/assets/daa52a95-3dce-4605-abe0-c3b3bea73417)

Como pode ser visto na imagem acima, o id do usuário no banco de dados foi atribuído e setado diretamente no cookie de sessão, o meu usuário é "25". Não é uma boa prática setar o uid no cookie, mas o maior erro está em não verificar no backend a relação das requisições vindas daquele uid com a sessão autenticada. Por isso que alterando o uid, um atacante consegue acessar a conta de outro usuário sem precisar se autenticar (Authentication Bypass) via IDOR.
Seguindo esse ponto de vista, provavelmente o uid do admin do sistema seria "1", o primeiro usuário portanto.  

![id admin](https://github.com/user-attachments/assets/8d8daf2f-a175-4a33-a9ce-1c4223542fd5)

Após alterarmos o uid para "1", dou um forward na resposta, permitindo que chegue até meu navegador. 
E voltando para a aplicação novamente, temos acesso a conta do admin.

![admin session hijacking](https://github.com/user-attachments/assets/3521725e-3c19-4e5f-a4b0-9f8f428e3c9f)



## IDOR no carrinho/cesta de compras (Basket ID)

Para este segundo e último exemplo, eu rodei a aplicação OWASP Juice Shop em um container Docker, especifiquei a porta 3000 para o host e para o serviço, após isso acessei a aplicação pelo navegador digitando o meu localhost:porta. 

![OWASP Juice Shop Docker](https://github.com/user-attachments/assets/4d792f7e-e52a-41d5-b09b-06acd011e345)

![Acessando Juice Shop](https://github.com/user-attachments/assets/25982c18-65d6-4a4e-afd0-e6a519596908)

A técnica vai ser interceptar a resposta da requisição e alterar o basket id atribuído ao meu usuário.

Carrinho vazio da conta criada: 

![carrinho](https://github.com/user-attachments/assets/8fb6aa2b-e251-4b3b-9cc4-c92eaa824502)

ID(bid) atribuído ao carrinho desta conta:

![bid carrinho](https://github.com/user-attachments/assets/3b02f836-aa2c-44e8-9b67-e1c04af192fb)

Agora para interceptar a resposta, irei fazer o login novamente e trocarei o id do carrinho para "1". Após isso, caso essa aplicação esteja vulnerável ao IDOR, teremos acesso ao carrinho de outro usuário!

![login Juice Shop](https://github.com/user-attachments/assets/b3fdc497-ed25-4562-b08e-8d4b19e65bc6)

![Interceptando resposta e alterando id](https://github.com/user-attachments/assets/df9c9fb2-2cab-4b70-90db-fb6b65eef94e)

E após dar o "forward" e voltar para a aplicação, foi recuperado com sucesso o objeto (carrinho) de outro usuário.

![carrinho após IDOR](https://github.com/user-attachments/assets/e087470a-54c4-4a41-8598-91559b1f2499)





  

  

