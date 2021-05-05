Docker - Criando containers sem dor de cabeça

Link do curso: https://cursos.alura.com.br/course/docker-e-docker-compose

Repositório: https://github.com/RodrigoFrancoZup/Docker-OrangeTalents

#Resumo em TXT:

Capítulo 01 - Introdução ao Docker
Neste capítulo aprendemos:

⦁	Como era Antigamente para "hostear" as aplicações? Antigamente cada host era feita em servidores (máquinas físicas), ou seja, era uma aplicação para cada servidor, por exemplo: um servidor para o Tomcat, outro servidor para o MySql e assim por diante;

⦁	Cada servidor precisa de um sistema operacional, e como há necessidade de comunicação entre as aplicações vamos precisar de uma rede entre os servidores;

⦁	Se tínhamos uma aplicação por servidor, a maior parte do tempo o uso da máquina ficava ociosa (desperdício de recurso). Os servidores normalmente eram potentes para aguentar dias festivos, logo em dias normais sobrava recursos;

⦁	A primeira melhora foi do problema citado anteriormente foi a Virtualização (Máquinas Virtuais);

⦁	Máquinas Virtuais surgiu graças o Hypervisor, ela permite "criar um computador dentro de outro", com o Hypervisor podiamos dividir os recursos de hardware para cada "Máquina Virtual = Computador Virtual rodando em cima de um Computador físico";

⦁	Desvantagem das Máquinas Virtuais: Cada máquina virtual "virtualização" precisa de um Sistema Operacional e cada Sistema Operacional vai consumir os recursos de Hardware. Outros problema: cada Sistema Operacional precisa de configuração (instalar drivers e outras configurações), tem que atualizar para corrigir bugs e por segurança;

⦁	A alternativa boa das Máquinas Virtuais foi os Containers (Docker faz uso de Container);

⦁	Um Container vai possuir a nossa aplicação, e haverá a divisão do recurso de hardware e de Sistema Operacional entre os Containers. Aqui temos um SO para todos os Containers, logo o Container é mais leve e por isso ele é rápido para subir;

⦁	Mas a dúvida geral é: Por que não instalamos tudo direto no Sistema Operacional? A resposta é: 
	-Aplicações querendo utilizar a mesma porta; 
	-Se um app começar a consumir muito recurso de hardware, ele vai travar as outras;
	-E se cada app precisar de versões diferentes da mesma linguagem?
	-Se a máquina cair, cai todo mundo;

⦁	O que é Docker? Docker é a empresa Docker Inc (Antiga dotCloud, essa era uma empresa de PaaS - Plataform as a Service, igual a Heroku, Microsfot Azure..) e é também o Software, a dotCloud utilizava esse software para economizar e ter lucro ao vender seus serviços;

⦁	Tecnologias Docker:
	- Docker Engine (só Docker) = Tecnologia que gerencia os containers, é o HOST. Ele que faz o intermédio com o Sistema Operacional;
	- Docker Compose: Gerência / Orquestra múltiplos containers;
	- Docker Hub = Repositório de imagens de containers;
	- Docker Swarm = Criar clusters de Docker para trabalharem juntos;
	- Docker Machine: Ferramenta para instalar e configurar host virtual;

⦁	Testando a instalação do Docker:
	- Rodar o comando no terminal docker run hello-world
	Esse comando significa que queremos construir um container com a imagem hello-world, mas nós não temos essa imagem em nosso PC, então o docker vai puxar essa imagem do Docker hub e criar o container com essa imagem.
	Uma Imagem é declarada com o arquivo dockerfile (é uma receita que ensina a criar o container)


Capítulo 02 - Trabalhando com as Imagens
Neste capítulo aprendemos:

⦁	Docker run NomeDaImagem é um comando para criar um container com a imagem NomeDaImagem, essa imagem deve estar no PC, caso não esteja o Docker vai tentar puxar essa imagem no Docker Hub. Uma imagem é uma receita de bolo, que define como o container vai ser. Exemplo usando a imagem ubuntu: docker run ubuntu;

⦁	Criar um container e dar um nome a ele: docker run --name Meu-container-ubuntu ubuntu

⦁	Docker ps é o comando para listar todos containers ativos;

⦁	Docker ps -a é o comando para listar todos os containers (mesmo os que estão no estado de parada).

⦁	Quando criamos um container com uma imagem que não executa nada, igual a imagem do Ubuntu o container será criado e será colocado no estado de parada. A imagem do Ubuntu não executa nada, mas podemos fazer ela executar, executando ela com a imagem: docker run ubuntu echo "Ola mundo". O comando echo será executado dentro do container do ubuntu, depois disso ele será colocado no estado de parada;

⦁	Podemos atrelar o nosso terminal com o terminal do container, basta passar a flag -it, por exemplo: docker run -it ubuntu Com isso o container do ubuntu será criado e o meu terminal agora está atrelado com esse container, tudo que eu digitar no terminal será executado dentro do container do ubuntu. Para eu parar essa vinculação dos terminais basta apertar ctrl + d;

⦁	Se eu criei o container do ubuntu e ele não executou nada ele vai para o estado de parada, como ativá-lo? Pois se eu digito docker run ubuntu eu vou criar outro container. Comando para ativar um container parado: docker start IdDoContainer Para conseguir esse id posso rodar o docker ps -a;

⦁	Para parar um comando, ou seja, colocá-lo em estado de parada: docker stop IdDoContainer. Esse comanda leva 10 segundos, para nao esperar basta passar o -t 0;

⦁	Quero "starta" um container e vincular meu terminal ao dele, comando para isso: docker -a -i IdDoContainer;

⦁	Obs: Não precisamos digitar o IdDoContainer completo, basta o começo do número que o Docker reconhece!

⦁	Obs2: Podemos combinar o comando com o flag --help, por exemplo docker run --help, assim será mostrado ajuda sobre o comando docker run, por exemplo quais flags esse comando aceita;

⦁	Comando para remover um container é docker rm IdDoContainer;

⦁	Comando para remover todos containers parados é docker container prune;

⦁	Comando para listar imagens (as receitas usada para criar o container) é Docker images;

⦁	Comando para remover imagens: Docker rmi NomeDaImagem;

⦁	Toda imagem que baixamos é composta por várias camadas, essa camadas podem ser utilizadas em várias imagens;

⦁	Toda imagem tem as camadas apenas de leitura, ou seja, não podemos escrever nelas, mas como conseguimos criar um arquivo na imagem do ubuntu? Simples, todo container é feito pelas camadas da imagem e é acrescentada uma camada (Layer) de leitura e escrita, e é nessa camada que vamos escrever, e não na imagem em si.

⦁	Imagens oficiais como ubuntu tem nome simples, já imagens não oficiais tem o padrao: NomDoUsuarioDono/NomeDaImagem

⦁	Há imagens que ao serem usadas para construir um container vai travar o terminal, pois ela vai ficar rodando um processo. Teremos que abrir outro terminal;

⦁	Como usar imagens que vão executar algo sem travar o terminal? Basta usar o comando docker run -d NomeDaImagem;

⦁	Se eu quiser me comunicar com o container (com sua execução) eu preciso mapear uma porta do meu PC com a do Container, para fazer isso automaticamente posso passar a flag -P, caso eu queira escolher a porta eu devo usar -p NumeroDaPorta. Exemplo: docker run -d -p 12345:80 dockersamples/static-site aqui mapei a minha porta do PC 12345 para a 80 do container;

⦁	Comando para saber o mapeamento de portas de um container: docker port IdDoContainer;

⦁	Comando para passar um valor para a variável de ambiente: docker run -d -P -e AUTHOR="Rodrigo Franco" dockersamples/static-site aqui AUTHOR é a variavel do container, ela foi declarada na imagem dockersamples/static-site;

⦁	Comando para lista apenas os ID dos containers rodando: docker ps -q;

⦁	Para vários containers de uma vez: docker stop -t0 $(docker ps -q);

⦁	Os estados de um container: Ativo = Running / Parado = Stopped. Quando criamos um container ele começa como ativo, caso ele não execute nada ele vai para parado. 

⦁	Obs: Onde eu uso IdDoContainer eu também posso usar o nome que eu dei ao container pelo flag --name


Capítulo 03 - Usando volumes
Neste capítulo aprendemos:

⦁	Lembra que estamos escrevendo os dados na layer de escrita que todo container cria em cima das layers da imagem. Assim quando apagamos o container os dados também são perdidos. Para não perder os dados nós devemos usar Volumes;

⦁	Volume é um reflexo (um link) entre pasta do container e pasta do meu PC. Vou criar uma pasta (volume) no container, mas tudo que eu colocar nessa pasta vai ser refletida para a pasta do Docker Host (essa fica no nosso PC). Vice-versa também, tudo que eu colocar na pasta do Docker Host vai para o Container;

⦁	Mais detalhes de um container podem ser obtidos com o comando: docker inspect IdDoContainer. Com esse comando poderei ver a seção Mounts e nela vou ver o source = endereço do meu PC onde foi salvo os dados do volume;

⦁	O source do comando anterior não foi escolhido por nós, foi automático. Pois criamos o volume com o comando: docker run -v "/var/www" ubuntu Aqui criamos a pasta www dentro do diretório /var e tudo que for colocado lá foi para nosso PC no diretório source;

⦁	Como escolher o caminho do meu PC que vai armazenar os dados do container? Com o comando: docker run -v "C:\rodrigo\documents:/var/www" ubuntu

⦁	Quando criamos um container não temos certeza em qual diretório o terminal dele (do container) encontra-se. Podemos escolher o diretório com a flag -w;

⦁	Vamos complicar um código com container. Vamos pegar um programa escrito em NodeJs. Para isso preciso que o container tenha o nodejs instalado, que ele tenha o código do programa e que execute o comando npm start no mesmo diretório que encontrasse o arquivo package.json. Na minha porta 8080 quero acessar a porta 3000 do container. Não quero que o terminal fique travado na execução O comando vai ficar:
	docker run -d -p 8080:3000 -v "C:\Users\Rodrigo\Desktop\volume-	exemplo:/var/www" -w "/var/www" node npm start

⦁	-d = para não travar o terminal;
⦁	-p = para mapear minha porta 8080 com a 3000 do container;
⦁	-v = para criar volume, tudo que criar no container ficará na minha pasta local, ou vice-versa;
⦁	-w = diretório que o terminal do container vai começar;
⦁	node = nome da imagem;
⦁	npm start = comando que o container vai executar ao ser levantado.
	


Capítulo 04 - Construindo nossas próprias imagens
Neste capítulo aprendemos:

⦁	Imagem é uma "receita de bolo" para construir o Container;

⦁	Uma imagem pode usar outra imagem como base, ou seja, uma receita de bolo que se baseia em outra receita;

⦁	Para criar uma imagem usamos o arquivo Dockerfile. Podemos por um nome, daí sua extensão será dockerfile, por exemplo: Ola.dockerfile. Mas é muito comum esse arquivo não ter nome, daí ele fica sem extensão, dai ele será apenas Dockerfile;

⦁	O dockerfile por si só não é uma imagem, temos que buildar essa imagem. Comando para Buildar: docker build -f Dockerfile -t rodrigofranco/node . Onde: 
	-f não é obrigatório quando o nosso Dockerfile não tem nome;
	-t é para dar nome a imagem e se quisermos subir para dockerhub o 	primeiro nome tem que ser do usuário do dockerhub;
	. indica que o dockerfile está no mesmo diretório do terminal que 	estamos executando esse comando, caso contrário devemos informar 	o diretório correto.

⦁	Após o build nós temos a imagem, podemos criar um container com essa imagem com o comando: docker run -d -p 8080:3000 rodrigofranco/node

⦁	Comando para Logar-se no Docker: docker login;

⦁	Comando para subir a imagem para docker hub: docker push rodrigofrancozup/node


Capítulo 05 - Comunicação entre containers
Neste capítulo aprendemos:

⦁	Se nossa app estiver em um container e o banco de dados em outro container (devemos fazer dessa forma), como eles vão se comunicar?

⦁	Por padrão todo container criado ficará na mesma rede, rede chamada de bridge;

⦁	Cada vez que um container é derrubado/levantado o seu ip é alterado, então se quisermos comunicar com um container o ideal é saber o nome do container, mas mesmo sabendo o nome não podemos comunicar com ele através do nome, isso quando estamos na rede default, se criarmos uma nova rede vamos poder comunicar com os containers através dos nomes;

⦁	Comando parar criar uma rede: docker network create --driver bridge NomeDaRede;

⦁	Comando para listar as redes disponíveis: docker network ls;

⦁	Adicionar um container em uma rede específica: docker run -it --name meu-container-de-ubuntu --network minha-rede ubuntu;

⦁	Comandos do Exercício:

	1.docker pull douglasq/alura-books:cap05;
	2.docker run -d --name meu-mongo --network minha-rede mongo;
	3.docker run -d -p 8080:3000 --network minha-rede douglasq/alura-books:cap05;
	4.Acessar navegador e digitar localhost:8080/seed e depois localhost:8080


Capítulo 06 - Trabalhando com Docker Compose
Neste capítulo aprendemos:

⦁	No exercício anterior subimos 2 containers, logo escrevemos dois comandos e os comandos são cheios de flags, podemos acabar esquecendo algum flag. No dia a dia é comum levantarmos diversos containers e por isso vamos usar o Docker Compose para subirmos múltiplos containers;

⦁	Extensão de um arquivo de Docker Compose é  .yml ou .yaml;

⦁	O arquivo .yml agora será a receita de bolo para o Docker Compose subir nossos containers;

⦁	O Docker Compose utiliza imagens, logo precisamos ter a imagem oficial ou então criar a nossa imagem (cria-se através do Dockerfile), para assim podermos usar o Docker Compose;

⦁	O comando docker-compose ps lista todos os containers ativos levantados pelo compose;

⦁	O comando docker-compose down vai parar todos containers e removê-los;

⦁	O comando docker-compose restart vai parar todos containers e levantá-los novamente;

⦁	Exercício:
	1.Criar o arquivo docker-compose.yml;
	2.No diretório que temos o arquivo .yml executar o comando: docker-compose build. Esse comando vai buildar as imagens necessárias, ou seja, vai pegar os dockerfile e criar a imagem que vão ser utilizadas para criar os containers;
	3.No diretório que temos o arquivo .yml  executar o comando: docker-compose up (pode usar o -d junto). Esse comando vai subir todos os serviços em seus respectivos containers;
	4.Para testar basta acessar localhost:80 e localhost:80/seed
	5.Digitar ctrl + c, para sair caso o terminal esteja preso;


#Segue a lista com os principais comandos utilizados durante o curso:

⦁	Comandos relacionados às informações
	docker version - exibe a versão do docker que está instalada.
	docker inspect ID_CONTAINER - retorna diversas informações sobre o container.
	docker ps - exibe todos os containers em execução no momento.
	docker ps -a - exibe todos os containers, independentemente de estarem em 	execução ou não.

⦁	Comandos relacionados à execução
	docker run NOME_DA_IMAGEM - cria um container com a respectiva imagem 	passada como parâmetro.
	docker run -it NOME_DA_IMAGEM - conecta o terminal que estamos utilizando com 	o do container.
	docker run -d -P --name NOME dockersamples/static-site - ao executar, dá um 	nome ao container e define uma porta aleatória.
	docker run -d -p 12345:80 dockersamples/static-site - define uma porta específica 	para ser atribuída à porta 80 do container, neste caso 12345.
	docker run -v "CAMINHO_VOLUME" NOME_DA_IMAGEM - cria um volume no 	respectivo caminho do container.
	docker run -it --name NOME_CONTAINER --network NOME_DA_REDE 	NOME_IMAGEM - cria um container especificando seu nome e qual rede deverá ser 	usada.

⦁	Comandos relacionados à inicialização/interrupção
	docker start ID_CONTAINER - inicia o container com id em questão.
	docker start -a -i ID_CONTAINER - inicia o container com id em questão e integra os 	terminais, além de permitir interação entre ambos.
	docker stop ID_CONTAINER - interrompe o container com id em questão.
	Comandos relacionados à remoção
	docker rm ID_CONTAINER - remove o container com id em questão.
	docker container prune - remove todos os containers que estão parados.
	docker rmi NOME_DA_IMAGEM - remove a imagem passada como parâmetro.

⦁	Comandos relacionados à construção de Dockerfile
	docker build -f Dockerfile - cria uma imagem a partir de um Dockerfile.
	docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM - constrói e nomeia 	uma imagem não-oficial.
	docker build -f Dockerfile -t NOME_USUARIO/NOME_IMAGEM 	CAMINHO_DOCKERFILE - constrói e nomeia uma imagem não-oficial informando o 	caminho para o Dockerfile.

⦁	Comandos relacionados ao Docker Hub
	docker login - inicia o processo de login no Docker Hub.
	docker push NOME_USUARIO/NOME_IMAGEM - envia a imagem criada para o 	Docker Hub.
	docker pull NOME_USUARIO/NOME_IMAGEM - baixa a imagem desejada do Docker 	Hub.

⦁	Comandos relacionados à rede

	hostname -i - mostra o ip atribuído ao container pelo docker (funciona apenas 	dentro do container).
	docker network create --driver bridge NOME_DA_REDE - cria uma rede 	especificando o driver desejado.

⦁	Comandos relacionados ao docker-compose

	docker-compose build - Realiza o build dos serviços relacionados ao arquivo docker-	compose.yml, assim como verifica a sua sintaxe.
	docker-compose up - Sobe todos os containers relacionados ao docker-compose, 	desde que o build já tenha sido executado.
	docker-compose down - Para todos os serviços em execução que estejam 	relacionados ao arquivo docker-compose.yml.
