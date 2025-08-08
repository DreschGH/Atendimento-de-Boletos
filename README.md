# Atendimento-de-Boletos
Solução em Power Apps e Power Automate que visa melhorar o controle sobre o processo de atendimento da caixa de e-mails de boletos.

--- 
# Componentes da Solução
Esse projeto trata de dois grupos de soluções:
- Aplicativo em Power Apps para gestão das solicitações e atendimentos
- Automações em Power Automate para gerenciar os contatos com clientes externos

## Aplicativo em Power Apps
### Interface:
O aplicativo conta com uma interface padronizada, com controle de temas (claro/escuro), menu lateral colapsável, indicador de seleção de tela, caixa de texto para pequisa entre telas.
#### Menu Lateral Aberto (Tema Claro)
<img width="1489" height="828" alt="image" src="https://github.com/user-attachments/assets/b56a3aa1-80b8-4773-80b3-a08fb7a85ae2" />

#### Menu Lateral Aberto (Tema Escuro)
<img width="1515" height="844" alt="image" src="https://github.com/user-attachments/assets/8fd02a52-a077-4d67-af53-039c7401e5a4" />


Aplicativo em Power Apps com 3 telas:
### Controle de Boletos:
<img width="1426" height="824" alt="image" src="https://github.com/user-attachments/assets/e2be4c95-d4b3-49e9-9e6a-e5ca3ea8e8c5" />
Painel central do aplicativo onde a atividade principal do processo de atendimento será executada. O painel conta com dois cards indicando o volume de pendências e o volume de atendimentos recentes, uma tabela interativa, um botão para execução de filtragem predefinida e um botão que ativa um painel lateral para filtros adicionais.

#### Painel de Filtros Adicionais:
  
<img width="1417" height="801" alt="image" src="https://github.com/user-attachments/assets/c45c4b25-f8c9-4dca-88db-b6f88f15d0cc" />

#### Tabela Interativa

<img width="880" height="453" alt="image" src="https://github.com/user-attachments/assets/cf990f15-07b4-4aec-a85b-c80c38691575" />

A tabela interativa é um objeto composto por uma série de elementos (Conteiners, Labels, Gallerys e Buttons) que em conjunto funcionam como uma tabela unificada com posibilidade de interação via cliques:

<img width="320" height="616" alt="image" src="https://github.com/user-attachments/assets/2e46bef5-8077-4b77-8050-ae40fb9dbdb3" />

Interagir com a tabela, coleciona o item selecionado via Set() e abre um painel modal que dispõe das informações mais importantes envolvendo a solicitação selecionada pelo usuário:
 - Dados da solicitação
 - Timeline do Atendimento
 - Dados disponíveis do cliente
 - Documentos anexados à solicitação
 - Histórico de e-mail enviados à caixa de boletos
 - Opções de ações para a solicitação

   <img width="718" height="523" alt="image" src="https://github.com/user-attachments/assets/11cd4b63-ac58-4377-a6c6-793df28342dd" />

Selecionar a ação de "Finalizar Atendimento" abrirá uma nova janela, com uma caixa de texto, para o analista registrar a resposta ao atendimento e um controle de anexos, para o analista anexar os documentos necessários. Clicar em "Enviar" finalizará o processo de atendimento e enviará a resposta ao cliente via Outlook, por meio da caixa de e-mail de saída definida.

<img width="651" height="464" alt="image" src="https://github.com/user-attachments/assets/0c886c11-d900-4db6-8f31-16c5339c44f7" />

--- 
## Painel de Controle

É um conjunto de duas telas, na navegação da barra lateral, que funcionam como uma central de observabilidade dos processos envolvidos no aplicativo, diponibilizando a visualização de indicadores e a interação indireta com algumas mecânicas de gestão dos atendentes disponíveis:

<img width="260" height="135" alt="image" src="https://github.com/user-attachments/assets/dd772b72-1921-462c-a515-1563389aabfa" />

### Painel de Controle

A página "Painel de Controle" tem uma característica mais geral, disponibiliza indicadores do processo de atendimento como um todo. Os gráficos utilizam ChartJS, possibilitando uma personalização mais flexível e uma experiência ao usuário mais agradável do que os controles de gráfico padrões do Power Apps.
<img width="1512" height="841" alt="image" src="https://github.com/user-attachments/assets/4a9d1410-feff-4c30-94f7-e6ad7e4db098" />

Os processos de cálculos para apuração dos indicadores utilizam variáveis temporárias via 'With()' para evitar impactos indesejados nos processos de filtragem por conta das delegações resultantes de calculos de agregação como 'AVERAGE()', 'COUNT()', etc. Assim os processos de cálculo e agrupamento executam de maneira mais otimizada e sem imprevistos.

<img width="422" height="740" alt="image" src="https://github.com/user-attachments/assets/56e6d0c3-e5ad-422a-b7e2-febc49b5b267" />

### Gestão de Atendentes

A página de Gestão de Atendentes é relativamente semelhante à página de Painel de Controle, mas com um foco maior em analisar os indicadores de cada atendente, assim como a capacidade de definir se um atendente está, ou não, disponível para receber novas solicitações para atendimento, além de adicionar novos atendentes à lista de atendimento.

<img width="1264" height="712" alt="image" src="https://github.com/user-attachments/assets/8f95bfa6-15ff-4ec3-924d-2b77664c9bcc" />

Nessa tela, é possível filtrar os indicadores para cada um dos atendentes, selecionando o atendente na tabela de atendentes cadastrados:

<img width="1377" height="720" alt="image" src="https://github.com/user-attachments/assets/b309eb28-0cec-465b-903a-86d19d3e4a80" />

Em sua grande maioria, as atividades dentro desse aplicativos ficam registradas por meio de logs, coletados durante as interações do usuário com o aplicativo.

--- 
## Automações

As Automações desse projeto utilizam o Power Automate como uma central de direcionamento de páginas HTML via uma lógica semelhante a de servidores de APIs RESTful, manipulando endpoints, com metodos, paths, queries e parâmetros personalizados. O Power Automate lida com as entradas recebidas de agentes externos, valida comunicações, retorna respostas ao cliente, rastreia interações e registra os resultados para eventuais tratativas internas, de uma maneira em que a experiência do cliente seja otimizada e centralizada, facilitando o processo de solicitação de atendimentos mais personalizados.

### Recebimento do Processamento do RPA e Notificação do Cliente
Após o processo automatizado que extrai os boletos do cliente na nossa plataforma de boletos, um RPA registra a ocorrência em uma lista do Sharepoint. Esse processo inicia um fluxo que obtem essas informações da lista e envia um e-mail cujo o corpo é um HTML personlizado, contendo o resultado do atendimento do RPA e, no final, dois botões com as opções de 'Fui Atendido' e 'Não Fui Atendido'.
<img width="650" height="896" alt="image" src="https://github.com/user-attachments/assets/88e604e5-eeba-4dee-97e1-450e93a37bd3" />


### Recebimento da Resposta Positiva
Caso o cliente selecione a opção de "Fui Atendido" o outlook o redirecionará para um endpoint com método GET que ativa um fluxo no Power Automate. O Endpoint retorna um HTML que informa ao cliente que a resposta foi recebida e agradece a interação com o e-mail.
<img width="678" height="739" alt="image" src="https://github.com/user-attachments/assets/f2ba422a-d1c3-40ff-ba0f-2ccec4f42770" />
Após isso o fluxo ativado pela chamada registra na lista do Sharepoint que aquele item foi atendido pelo processo automatizado de extração de boletos

### Recebimento da Resposta Negativa
Caso o cliente seleciona a opção "Não Fui Atendido" o outlook o redirecionará para um endpoint com método GET que ativa um fluxo no Power Automate. Esse Endpoint retorna um HTML que contém uma caixa de texto, onde o cliente será capaz de detalhar a sua necessidade. Dentro desse HTML, existe um script que envia um POST para outro endpoint, cuja função é registrar a demanda do cliente junto ao devido registro no Sharepoint, esse processo executa uma validação para avaliar se a solicitação está disponível para ser alterada e também se o usuário que está mandando as informações é o mesmo dos registros internos.

<img width="669" height="883" alt="image" src="https://github.com/user-attachments/assets/8778611e-193f-4669-b0d5-942d484a695d" />


### Recebimento dos Detalhes da Necessidade do Cliente
Ao receber os detalhes do cliente, caso validados pelo processo de avaliação de recebimento da solicitação, é iniciado o processo de definição do atendente, que captura o último atendente a receber uma demanda, busca o próximo atendente na lista e define-o como o atendente da demanda recebida, caso não exista um "próximo" atendente, o primeiro atendente da lista de atendentes disponíveis recebe a demanda.

### Envio da Resposta do Analista ao Cliente
Quando o atendente efetua o envio das informações dentro do aplicativo de atendimento, o registro dentro da lista do sharepoint é atualizado e recebe o status de "Atendido", nesse momento, um fluxo é ativado para enviar a notificação ao cliente via Outlook. O parecer do analista é enviado por e-mail, embutido em um HTML que além do resultado da análise, contém um menu de avaliação de atendimento, possibilitando ao cliente dar um feedback sobre a qualidade do atendimento do canal.

<img width="632" height="642" alt="image" src="https://github.com/user-attachments/assets/8569b609-2355-4a59-8b56-f7f9142fa7e9" />


### Recebimento da Avaliação do Atendimento 
Por fim, caso um cliente selecione uma das opções de avaliação do atendimento, o HTML o redirecionará para um endpoint que retornar uma mensagem de agradecimento pelo feedback e, pelo back-end, registra o resultado da avaliação dentro do item no sharepoint
<img width="650" height="982" alt="image" src="https://github.com/user-attachments/assets/4f680365-fc4a-459e-9677-4949e5f5c7d9" />





