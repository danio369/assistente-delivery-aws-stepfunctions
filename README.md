
 # ⚙️Desafio de Projeto: Criando um Assistente de Delivery com AWS Step Functions e Bedrock - Aulas

## Bootcamp [Nexa](https://www.nexaresources.com/) - Engenharia de Prompts na AWS com Claude

---

### Objetivo

Resumir o projeto "Criando um Assistente de Delivery com AWS Step Functions e Bedrock", da [DIO](https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock), ministrado pelo professor [Felipe Aguiar](https://github.com/felipeAguiarCode) 
Um passo a passo para auxiliar quem está iniciando.

---

### 🧠Entendendo a Situação Problema

Usuário abre o aplicativo de delivery de alimentação para fazer um pedido para um jantar romântico. 
Quando realiza o pedido, embora veja apenas uma tela, quando a aplicação está quebrada em microsserviços, ela não é uma aplicação só. Na verdade, são várias aplicações distribuídas, (como ilhas isoladas), chamamos de **sistemas distribuídos**. Um leque de ferramentas que trabalha em conjunto. Existe um fluxo onde uma coisa depende de um gatilho da outra.

Etapas do usuário ao realizar o pedido:

1. Usuário passa por uma **API de User Management** - API que armazena o histórico de pedidos e histórico de utilização de cupons do usuário;

2. **Gerenciador de Push Notifications** - enviar push notifications  do status para o cliente: "Seu pedido foi realizado com sucesso" e para o restaurante: "Você tem um novo pedido";

3. **API Delivery APP** - API de gerenciamento de estado de pedido. Tanto atualiza o restaurante quanto à fila para atender, quanto atualiza o status da fila para o cliente.

### 😵‍💫Problema 

Se em cada um dos passos, ele depende de alguma ação que está acontecendo em outra aplicação, em outro microsserviço, e cada um desses microsserviços isolados só enxerga ele mesmo, como fazer para que um se comunique com o outro?

---

### 💡Solução Possível: 

-  Ferramentas de Serviços de Mensageria 

Precisamos de outra ferramenta, que atue como agente e comunique entre os microsserviços e execute alguma função. Um agente que fale: "Alguma coisa aconteceu no histórico de pedidos, preciso disparar uma notificação e o app de delivery tem que se preparar". **Ferramenta de Serviços de Mensageria** - publica uma mensagem para que os outros serviços "ouçam" as novas mensagens, sempre que alguma coisa acontecer. 

**Publisher** - como um carteiro que leva cartas para haver comunicação. É parte do serviço de mensageria.

Cada microsserviço, cada "ilha isolada", é um **subscriber** (alguém que está inscrito).

Exemplo de ferramentra para criar serviço de mensageria: [RabbitMQ](https://www.rabbitmq.com/)

RabbitMQ - para criar o publisher (carteiro) e fazer as ferramentas se inscreverem (subscriber).

Para que cada etapa execute uma função, depende sempre de um publisher, de uma mensagem, e cada etapa precisa estar inscrita para receber a mensagem e entender o que está acontecendo nas outras etapas.

---

### 🎯Solução Facilitadora: AWS STEP FUNCTIONS

Step = passo

Function = função

Para cada etapa, executar uma função que converse com outro componente.

Se você já usa os serviços da AWS para cada uma dessas etapas, você consegue fazer isso com clica e arrasta. Sem precisar criar serviço de mensageria nem desenhar o subscriber.

### Benefícios da Solução

**AWS STEP FUNCTIONS orquestra todas as camadas do sistema**, só com componentes de clica e arrasta.


"O AWS Step Functions facilita coordenar os componentes de aplicações distribuídas em microsserviços usando fluxos de trabalhos visuais". Fonte: AWS news dec 1, 2016

Ele automatiza processos, orquestra microsserviços e cria pipelines de dados e machine learning (ML).

---

### ⚒️Conhecendo a Ferramenta

Para acessar os tópicos:
- Introdução 
- Benefícios 
- Como funciona
- Casos de uso
- Clientes do Step Functions 
- Artigos para explorar conceitos básicos 
- [Workshop autoguiado](https://catalog.workshops.aws/stepfunctions/pt-BR/introduction) 
- Links para treinamentos do Step Functions

Clique no link: [AWS Step Functions](https://aws.amazon.com/pt/step-functions/) e acesse aos conteúdos oficiais.

---


### ❓Público-alvo

- Qualquer pessoa interessada em aprender sobre o AWS Step Functions.
- Qualquer pessoa que queira aprender técnicas para coordenar e orquestrar fluxos de trabalho de aplicações.

---

### ✅Pré-requisitos

- Ter uma conta na AWS. [Passo a passo para criar uma conta na AWS](https://github.com/digitalinnovationone/aws-cloud-quickstart)
- Conhecimento prévio em outra ferramenta AWS. Para começar, você precisa do AWS Step Functions para automatizar e criar fluxos com outras ferramentas da AWS e explorar o máximo de poder que ele tem para oferecer.

---


### ➕Saber mais: Saga Pattern

Conjunto de 5 ações comuns em serviços de orquestração. É um padrão de desenvolvimento que são estratégias para fazer o gerenciamento entre aplicações e as técnicas mais comuns.

1. Sequenciar tarefas
2. Repetir tarefas com falhas
3. Executar tarefas em paralelo
4. Selecionar tarefas baseado nos dados de saída
5. "Try/catch/finally"

### Step Functions Workflows

- Linguagem ASL (Amazon States Languages) - A Amazon States Language é uma linguagem JSON baseada e estruturada usada para definir sua máquina de estados, uma coleção de estados que pode trabalhar (Taskestados), determinar para quais estados fazer a transição (Choiceestados), interromper uma execução com um erro (Failestados) e assim por diante.
- Definidos como máquinas de estado. **Máquina de estado** é uma aplicação do Step Functions rodando.
- Estados = passos
- Podem orquestrar múltiplos serviços AWS
- Workflow = fluxo de trabalho

### Máquina de estado 

Controla vários momentos de uma aplicação de trabalho, ela coordena os processos de:

1. Obtenção da lista de IDs de chamados a serem processados - **Inicialização**
2. Realiza uma série de etapas até que todos sejam processados (para cada chamado)- **Processamento em Loop**
3. Encerra a execução após o processamento completo - **Finalização** 



### Estados do Step Functions 

| Estados |
| ------- | 
| Choice  | 
| Paraliel| 
| Map     | 
| Pass    | 
| Wait    | 
| Sucess  | 
| Fail    | 

###  Example Tasks

| Example Tasks     |
| ----------------- |
| AWS Lambda Invoke |
| Amazon SNS Publish|
| Amazon ECS RunTask|
| AWS Step Functions StartExecution|
| AWS Glue StartJobRun |

https://catalog.workshops.aws/stepfunctions/pt-BR/introduction/states

[Documentação de estados de fluxo de trabalho](https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/workflow-states.html)

---

### 📄Documentação de entrada e saída

https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/concepts-input-output-filtering.html

---

### 🤔Escolhendo o Tipo de Fluxo de Trabalho em Step Functions

https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/choosing-workflow-type.html

---

### 🤝Integrações com AWS SDK 

https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/supported-services-awssdk.html

---

### 🔨Ferramenta Low Code 

https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/workflow-studio.html

---

### 🔗Padrões de Integração de Serviço

https://docs.aws.amazon.com/pt_br/step-functions/latest/dg/connect-to-resource.html

---

### 👣 **Criação de Projeto** passo a passo

1. Logar na conta AWS
2. Na página inicial, clicar em: serviços>integração de aplicativos>Step Functions>
3. Favoritar com a estrela e clicar nele ou digitar AWS Step Functions no Menu searcch 🔎
4. Criar máquina de estado
5. Escolha um modelo (você poderá editá-lo) ou comece do zero

---

#### **Projeto mais básico** "Hello World" para você se habituar com o Step Functions:

- Na página inicial, ao invés de criar máquina nova, clique em step functions no alto da página. Clique em: comece a usar > Executar Hello World > Iniciar > Próximo. 

- Do lado esquerdo aparecem todas as **Ações** que poderá executar. Pode puxar uma ação dela, clique e arraste para a tela principal. Se clicar em **Fluxo**, consegue fazer tomada de decisões. Em **Padrões** tem os processamentos padrões em blocos de ações, são ações padronizadas.

- Em **design**, você verá tudo de forma visual.

- Clique em **código**, você verá um **JSON** de cada caixinha.

- Faça configurações específicas para esse projeto, vá em **configurações**: permissões para usuário, configurações adicionais, adicionar etiqueta.

- Clique em **criar** para criar e executar seu fluxo.

- Em cada bloco ASL, você pode clicar em **Inspetor** (aba do lado direito) e fazer configurações específicas para esse bloquinho.

- Clique no lápis para **renomear o projeto**.

---

### 👋Criando e executando Hello World

- Criar

- Iniciar execução

- Role para baixo e veja a execução passo a passo (JSONs sendo transformados) em **Event view**. Também pode visualizar o gráfico ou exibir em forma de tabela.

- No canto superior direito, podertá editar máquina de estado. Salve, execute e inicie a execução.

- Poderá fazer isso do nível micro ao macro, com aplicações inteiras.

- Poderá exportar em **ações** no lado superior direito, ao lado 

---

### 💲Precificação e custos

**👀Dica do Felipão** - Importante!!! Não deixe de acessar! https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock/learning/e0def464-a56c-48bb-9610-7c611201b170

Step Functions não tem preço fixo. Você precisa ver os preços dos serviços de forma individual para saber o custo da execução da automação. Entre na página oficial de cada serviço, (Amazon Beddrock, AWS EC2). Cada execução é cobrada. Se fizer 10 exececuções de Amazon Beddrock, as 10 execuções serão cobradas e assim em cada serviço. O Valor varia. 

Gere estimativas de preços usando a ferramenta [Calculadora de preços da AWS](https://calculator.aws/#/)  

Algumas ferramenmtas possuem limites gratuitos, outras têm créditos experimentais. Você precisa checar, **leia as regras de preços** para evitar surpresas de experiências ruins por falta de informações do utilizador. 

A AWS é uma ferramenta muito poderosa, é excelente plataforma com excelentes serviços. Hoje é difícil ver uma empresa que não use AWS, seus serviços são muito solicitados no dia a dia das empresas.

---

### Passo a passo da [Aula Nível 1](https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock/learning/e760a1c8-b9e4-48af-b947-25cb93fe5291?back=/track/engenharia-prompts-aws) e da [Aula Nível 2](https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock/learning/7e5715ca-1581-411e-ba61-3bea269efe46)
- Aula Nível 1 de configuração - Permissão de Perfil 
- Aula Nível 2 de configuração - Disponibilidade de Serviço   

---

## Criando um Assistente de Delivery na Prática 

Siga o passo a passo em:

https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock/learning/7a95372c-c5ae-4f59-a2db-3771a6b250fa?back=/track/engenharia-prompts-aws

---

## Como entregar meu Projeto

https://web.dio.me/lab/criando-um-assistente-de-delivery-com-aws-step-functions-e-bedrock/learning/b17e9f6c-37ec-492b-a5ab-bf8c23028a62?back=/track/engenharia-prompts-aws

- Se você fez o projeto na prática: copie o código JSON e suba no seu GitHub. Entregar Projeto. Cole o link do projeto do GitHub.
- Se você fez o passo a passo da aula. Copie e cole o link do GitHub em Entregar Projeto.

---


⚠️**Atenção**: Os serviços AWS utilizados precisam estar corretamente configurados e as permissões necessárias definidas para acesso aos recursos necessários.

---

## Links Úteis:

- AWS Step Functions: https://aws.amazon.com/pt/step-functions/

- AWS Step Functions Examples: https://github.com/aws-samples/aws-stepfunctions-examples

- Serverless e Amazon Bedrock: https://aws.amazon.com/pt/blogs/aws-brasil/como-criar-um-assistente-virtual-de-baixa-latencia-com-multiplos-modelos-usando-serverless-e-amazon-bedrock/
