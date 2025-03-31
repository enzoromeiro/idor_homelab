# IDOR & Session Hijacking Lab

## 🃏 Descrição  
Este projeto explora a vulnerabilidade **IDOR (Insecure Direct Object Reference)**, a identificação de **Weak Session ID** e a realização de **Session Hijacking** em um ambiente de laboratório seguro.

## Objetivo  
- Compreender e explorar falhas relacionadas a **IDOR**.  
- Identificar **Weak Session ID** e analisar seus riscos.  
- Realizar **Session Hijacking** para entender os impactos dessa vulnerabilidade.  

## Ferramentas Utilizadas  
- Burp Suite  
- OWASP Juice Shop (docker)
- OWASP BWA (máquina virtual - Oracle VirtualBox)
- Kali Linux  

## Passos do Projeto  
1. Configuração do ambiente de teste
2. Interceptação com Burp Suite  
3. Identificação de **Weak Session ID**
4. Exploração da vulnerabilidade **IDOR**
5. Execução de **Session Hijacking**

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



  

  

