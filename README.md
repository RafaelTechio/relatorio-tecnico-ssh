# Relatório Técnico sobre Acesso SSH no EC2
Rafael Mateus Zimmer Téchio - Engenharia de Software Inteli - Turma 5

## Introdução
O EC2 é um tipo de serviço da AWS que disponibiliza uma VM a partir de uma imagem para a livre utilização. Já o SSH é um protocolo de comunicação extremamente difundido e nos permite ter acesso a o terminal de um dos usuários da máquina virtual.

## Objetivo
O objetivo desse relatório é documentar passo a passo a criação de uma instância EC2 e seu acesso por SSH. Esse conhecimento será útil para a realização do projeto, visto que é uma das formas de acessar um servidor para realizar o deploy da aplicação.

## Materiais
Para a realização dos passos presentes neste relatório, são necessários:

**Navegador Web**: Google Chrome, Microsoft Edge ou similares.

**Conta AWS**: Conta AWS academy com acesso à criação de instâncias ou uma conta padrão AWS com acesso liberado à criação de instâncias.

**Serviço SSH**: Corretamente instalado e configurado, o serviço SSH permitirá a conexão com a instância criada.

## Métodos
A seguir estão os passos para a criação de uma instância EC2 e seu acesso por SSH.

### Criação da instância EC2
Na página inicial, procure o link para o serviço EC2 ou então pesquise pela barra de pesquisa
![1](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/a188a0e5-f353-498d-aee0-43660be9b15b)
Dessa forma o navegador será redirecionado para a página de administração das instâncias EC2. Aqui, serão mostradas várias informações úteis sobre a quantidade de instâncias criadas. 
![2](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/d7a7e783-470b-48bd-8829-cfa239e67915)
Após clicar em "Instâncias (em execução)" aparecerá a lista de instâncias ativas no momento.
![3](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/9dc30ebd-5c30-4a41-8c93-f568c4644c17)
![4](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/ce6a151a-7034-4155-9679-18ddc9f2a512)
Ao clicar em "Executar instância", aparecerá a página para criar uma instância nova de EC2. Comece  preenchendo o nome, no caso "srv-01", para assim manter uma padronização.
![5](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/61086ac4-eada-45c9-b51d-8c468b4a0e4a)
Um pouco mais abaixo, é solicitada a escolha de uma AMI (Amazon Machine Image), que basicamente é uma pré-configuração do servidor com um SO e até mesmo alguns softwares de mais alto nível. Existem as AMIs verificadas da própria AWS e AMIs criadas pela comunidade. No caso, a escolhida foi o Amazon Linux
![6](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/3e2f6496-8796-48d9-a17b-c81058df45c2)
No tipo de instância, escolha a configuração de máquina que se adeque às suas necessidades computacionais. Existem diversas família, gerações e tamanhos de máquina que são representadas pelo seu nome (t1.micro, t2.large, etc) e cada uma é otimizada para alguma área.

![7](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/4dbb1083-18f0-4e6e-b15a-a40494475b20)
Em seguida, clique em criar parte chaves e gere uma nova. Essa chave será necessária para a conexão por SSH. Escolha o formato .pem para ser compatível com o OpenSSH utilizado nesse passo a passo, mas caso necessário, é possível converter a chave .pem para .ppk para o uso do PuTTY.

![8](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/d4c72fbb-d232-4992-a3df-257208c3ec08)
Dessa forma, a chave criada ficará salva.

![9](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/026fa2d9-4f99-48dd-a32e-8c04a5a5ed1f)
Nas configurações de rede, escolha as configurações que se adequem com a finalidade do uso de sua instância. Caso seja uma aplicação web como site ou webservice, libere o tráfego HTTPS e HTTP. Também libere o SSH para poder conectar-se. Há a possibilide de restringir os IPs que podem se conectar à instância.

![10](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/2e816d0b-2d1f-4d65-9004-66ac782a4f51)
Cada instância EC2 pode possuir N volumes além de um volume principal que é obrigatório. Esses volumes podem ser EBS para memória duradoura ou outros tipos que guardam dados apenas enquanto a instância estiver ativa. Escolha o tamanho do volume desejado.
![11](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/9eba2f6a-6f3b-4754-acb4-e451353632ce)
Os detalhes avançados podem ser usados para configurações específicas e ao final é possível criar mais de uma instância com a mesma configuração pela barra lateral. Clique em "Executar instância" e ela será criada.

Agora aparecerá o resumo da instância com as configurações colocadas. Aqui será possível visualizá-las e editá-las.
![12](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/ff817c33-c693-43c8-b148-172ab67b3e94)

Na aba detalhes aparecerá as configurações gerais da instância
![13](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/32e287fc-8402-46a9-ac2e-e39b21ed44dc)

Na aba Segurança aparecerá as regras de firewall definidas
![14](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/b83e25f3-141a-4494-a64f-748e1d97125f)

Na aba rede aparecerá informações gerais de rede como IP, referência da VPC e sub-rede
![15](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/4b13234b-4ebd-4cb5-ae1a-a0e34188aaca)

Na aba armazenamento será mostrada informações dos volumes configurados
![16](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/108cb84f-7cb0-41b4-bfd8-beb919a25709)

Na aba Verificação de status será mostrada informações sobre a o status do sistema e da instância
![17](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/785a29af-a6e1-42b2-b772-1780f2e8ddf5)

Em monitoramento serão mostradas informações de utilização da instância, rede e mais
![18](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/1fd94dac-d40e-4e8e-af9b-f9659a739e0b)

Em tags serão mostradas as tags do sistema. Tags são uma forma de categorizar instâncias de forma livre.
![19](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/4dedd901-2574-41bc-829f-8eb7bfd5d9dd)



### Acesso SSH
![20](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/1478b47a-9048-4142-9f3e-5c212052da89)

![21](https://github.com/RafaelTechio/relatorio-tecnico-ssh/assets/110608373/5f49e65b-cd2e-437b-9035-86c2d80b7a27)

## Resultados
Como resultado dos passos desse relatório, uma instância de IP 18.212.69.93 foi criada e acessada por meio de SSH por um computador local usando a rede da faculdade Inteli.

## Conclusão
Seguindo os passos contidos nesse relatório, é possível criar uma máquina virtual e acessá-la de modo a disponibilizar aplicações para a web.
