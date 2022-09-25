Item 1 
------

    Depois de executados os comandos do AWS CloudFormation foi criado uma instancia conforme mostra a tela abaixo:
   ![image](https://user-images.githubusercontent.com/109318929/192166522-c0e1405e-a5d2-418b-970b-4082b9a17701.png)

Item 2
------
    
    Para que a página web fosse exibida como no exemplo dado, foi necessário adicionar nas regras de entrada 
    da instancia a porta 80 (http), para que as requisições vindas com destino nessa porta, através do Security Group 
    “stack-controle-WebServerSecurityGroup-1WTUBSU3BHD3A”, pudesse abrir a aplicação.
   ![image](https://user-images.githubusercontent.com/109318929/192166699-589078d7-e8c4-4409-8fbd-92f22abac39a.png)
   
    Dessa forma a página WEB pode ser exibida como podemos ver abaixo:
   ![image](https://user-images.githubusercontent.com/109318929/192166893-d064a8bb-7592-4f76-ae86-ab4859a1bf41.png)


Item 3
------
    
    Para essa atividade, foi criado um par de chaves para acesso via SSH a instancia na AWS.
   ![image](https://user-images.githubusercontent.com/109318929/192166945-d3d26262-2b56-41d2-967c-2915a6ad7944.png)
   
    Para acessar a instancia EC2 através de SSH utilizando o par de chaves que foi criado, precisamos desligar 
    a instancia original. Em seguida, criar uma imagem usando como base essa instancia original e, a seguir, 
    associando a chave que foi gerada a uma nova instancia criada a partir dessa imagem.
   ![image](https://user-images.githubusercontent.com/109318929/192166992-94316389-c3bc-4ee5-853b-5d9afdba5951.png)
   ![image](https://user-images.githubusercontent.com/109318929/192167014-6910fa58-c71d-4154-a6eb-87b12bb3753b.png)
   ![image](https://user-images.githubusercontent.com/109318929/192167017-c657ca83-e61a-40ac-ac16-12f8adcabcb6.png)
   
     Nova instancia criada com base na imagem elaborada através da instancia original.
   ![image](https://user-images.githubusercontent.com/109318929/192167054-ef2aa4ad-d94a-463d-a773-b233678bdaaf.png)
   
    Par de chaves associada a nova instancia:
   ![image](https://user-images.githubusercontent.com/109318929/192167085-ec0a440c-f16e-4bf3-9e42-081fa2b486e0.png)
   
    Acessando a nova instancia via SSH usando o par de chaves que foi gerada e associada a nova instancia.
   ![image](https://user-images.githubusercontent.com/109318929/192167092-ba3fe3a9-1388-472e-8900-db95c810afae.png)
   
    Editado o arquivo "/var/www/html/index.html" e adicionado meu nome, conforme instruções, como pode-se ver abaixo:
   <img width="704" alt="image" src="https://user-images.githubusercontent.com/109318929/192167404-524d285f-6ecb-47a7-85d3-e0beade9417b.png">
   
Item 4
------

    Identificado que o serviço do Apache TomCat estava parado. Feito a transição para root, usando o comando “sudo su”. 
    Iniciado o serviço com o comando “systemctl start httpd" e depois colocado o serviço para iniciar automaticamente
    com o comando “systemctl enable httpd”.
   ![image](https://user-images.githubusercontent.com/109318929/192167113-d1375a3f-e388-481f-85ea-1d5720d8ad4a.png)
   
    Identificado o novo IP público da instancia depois de iniciada e aberto no navegador. O resultado foi esse:
   ![image](https://user-images.githubusercontent.com/109318929/192167130-65d13c9b-dae5-4f07-8949-8bb9977545ed.png)
   
Item 5
------

    Feito a criação de um Target Group (tg-aws-challenge) e registrado os hosts que iriam fazem parte dele.
   ![image](https://user-images.githubusercontent.com/109318929/192167461-311af4fd-0b8b-4c0d-acce-51aac3f11387.png)
   
    Criado um “Application Load Balancer” e atribuído o Target Group (tg-aws-challenge) recém-criado.
   ![image](https://user-images.githubusercontent.com/109318929/192167470-67903c51-5492-490b-b9de-6f1d30d2d0b9.png)
   
    Podemos identificar que o “Application Load Balance” funciona ora mandando o trafego para “maquina 1”,
    ora mandando para “maquina 2”
   ![image](https://user-images.githubusercontent.com/109318929/192167480-ebd38985-00ab-423c-be01-d67964dc5769.png)
   ![image](https://user-images.githubusercontent.com/109318929/192167489-3da46f66-75e3-4116-9621-ded46d739cfa.png)
   
    Depois de interromper uma das instancias, podemos observar que a aplicação continua abrindo normalmente pelo
    “Application Load Balancer” e todo o trafego agora é redirecionado apenas para a “maquina 1”.
   ![image](https://user-images.githubusercontent.com/109318929/192167515-e617e521-d99e-4cab-b340-50b7f62689cd.png)
   ![image](https://user-images.githubusercontent.com/109318929/192167525-7d71eb50-12e1-4a96-a168-7bf717c44ea4.png)

Item 6
------

    Feito a liberação da porta 80 apenas para as redes que compõem o Security Group do “Application Load Balancer”.
    Com isso, a aplicação deixou de responder para os Ip’s das instancias e passou a funcionar 
    apenas pelo ip do "Application Load Balancer", como podemos ver abaixo:
   ![image](https://user-images.githubusercontent.com/109318929/192167607-ce099a5c-d17e-425b-a1dc-ad5bf49b0174.png)





   


    




   

