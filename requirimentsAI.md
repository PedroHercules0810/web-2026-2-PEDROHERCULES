# Documento de Requisitos — Jogo de Adivinhação de Motos

## 1. Descrição do Problema

O projeto propõe o desenvolvimento de uma aplicação web de adivinhação de motos comercializadas oficialmente no Brasil, inspirada na mecânica dos jogos Wordle e Termo.

O usuário deverá identificar uma moto secreta por meio de palpites contendo marca e modelo. A cada tentativa, o sistema fornecerá indicações visuais comparando os atributos da moto informada com os atributos da resposta correta.

A aplicação terá dois modos:

- **Modo diário:** todos os usuários recebem a mesma moto secreta em determinado dia.
- **Modo infinito:** o usuário recebe motos aleatórias consecutivamente, sem depender da rodada diária.

Também haverá um sistema de equipes baseado em fabricantes de motos, rankings individuais e por equipe, além de dicas que influenciam negativamente a pontuação do jogador.

---

## 2. Perfis de Usuário

### 2.1. Visitante

Usuário sem autenticação na plataforma.

Funcionalidades permitidas:

- Acessar a página inicial;
- Consultar regras do jogo;
- Jogar partidas, caso o modo visitante seja implementado;
- Consultar rankings públicos;
- Visualizar informações gerais sobre motos.

Restrições:

- Não poderá ter progresso e estatísticas sincronizados entre dispositivos;
- Não poderá participar plenamente de rankings vinculados a uma conta;
- Não poderá definir ou alterar uma equipe de forma persistente.

### 2.2. Jogador Autenticado

Usuário cadastrado e autenticado na aplicação.

Funcionalidades permitidas:

- Jogar nos modos diário e infinito;
- Registrar palpites e resultados;
- Escolher uma fabricante como equipe;
- Usar dicas durante as partidas;
- Consultar seu histórico e estatísticas;
- Participar do ranking geral;
- Participar do ranking da equipe;
- Consultar sua posição dentro da própria equipe;
- Compartilhar o resultado da rodada sem revelar a resposta.

### 2.3. Administrador

Usuário responsável pela administração do sistema e do catálogo de motos.

Funcionalidades permitidas:

- Cadastrar motos;
- Editar dados técnicos das motos;
- Ativar ou desativar motos disponíveis para sorteio;
- Corrigir informações incorretas no catálogo;
- Gerenciar fabricantes disponíveis como equipes;
- Consultar dados administrativos e rankings;
- Moderar usuários, caso necessário;
- Definir ou revisar a moto selecionada para o modo diário;
- Atualizar informações de FIPE e velocidade máxima declarada.

---

## 3. Informações a Serem Armazenadas

### 3.1. Dados dos Usuários

Para jogadores autenticados, deverão ser armazenados:

- Identificador único do usuário;
- Nome de exibição;
- E-mail;
- Método de autenticação;
- Data de cadastro;
- Foto de perfil, se disponibilizada pelo provedor de autenticação;
- Equipe selecionada;
- Data da última alteração de equipe;
- Perfil de acesso: jogador ou administrador;
- Estatísticas gerais do jogador.

### 3.2. Dados das Motos

Cada moto cadastrada deverá conter:

- Identificador único;
- Fabricante;
- Modelo;
- Ano/modelo;
- Cilindrada;
- Peso em ordem de marcha;
- Estilo;
- Valor ou faixa de preço FIPE;
- Velocidade máxima declarada pela fabricante;
- Situação de disponibilidade para sorteio;
- Data de inclusão e atualização do cadastro.

Os estilos permitidos inicialmente serão:

- Naked;
- Street;
- Trail;
- Sport;
- Touring.

### 3.3. Dados das Partidas

Para cada partida iniciada ou concluída, deverão ser registrados:

- Identificador da partida;
- Identificador do usuário;
- Modo de jogo: diário ou infinito;
- Moto secreta da rodada;
- Data e horário de início;
- Data e horário de conclusão;
- Status da partida: em andamento, vencida ou encerrada;
- Quantidade de palpites;
- Histórico dos palpites realizados;
- Dicas utilizadas;
- Pontuação final;
- Tempo total de resolução.

### 3.4. Dados dos Palpites

Cada palpite deverá armazenar:

- Identificador do palpite;
- Identificador da partida;
- Moto selecionada;
- Data e horário do palpite;
- Resultado da comparação de fabricante;
- Resultado da comparação de modelo;
- Resultado da comparação de cilindrada;
- Resultado da comparação de peso;
- Resultado da comparação de estilo.

### 3.5. Dados do Desafio Diário

Para garantir que todos os jogadores recebam a mesma resposta no dia, deverão ser armazenados:

- Data da rodada;
- Moto sorteada;
- Data e horário de publicação;
- Data e horário de encerramento;
- Status da rodada;
- Quantidade de jogadores que concluíram o desafio.

A troca da moto diária deverá respeitar o fuso horário do Brasil, preferencialmente `America/Sao_Paulo`.

### 3.6. Dados de Ranking

Para compor os rankings, deverão ser mantidos:

- Usuário;
- Equipe do usuário;
- Data da rodada;
- Quantidade de palpites;
- Quantidade de dicas utilizadas;
- Pontuação ajustada pelas penalidades;
- Horário de conclusão;
- Posição no ranking geral;
- Posição no ranking da equipe;
- Número de acertos por equipe.

O ranking individual deve obedecer à seguinte ordem:

1. Menor quantidade de palpites;
2. Menor número de dicas utilizadas ou menor penalidade acumulada;
3. Menor horário de conclusão da partida.

### 3.7. Dados de Equipes

Cada equipe deverá conter:

- Identificador único;
- Nome da fabricante;
- Logotipo, se utilizado;
- Status de disponibilidade;
- Quantidade de membros;
- Quantidade acumulada de acertos;
- Posição no ranking de equipes.

---

## 4. Requisitos Funcionais

### RF01 — Palpites

- O usuário deverá informar palpites por meio de busca de fabricante e modelo.
- O sistema deverá aceitar apenas motos existentes e ativas no catálogo.
- O sistema deverá exibir o resultado comparativo de cada atributo do palpite.
- A resposta correta deverá encerrar a partida.

### RF02 — Comparação de Atributos

A aplicação deverá comparar:

- Fabricante;
- Modelo;
- Cilindrada;
- Peso em ordem de marcha;
- Estilo.

A interface deverá informar visualmente:

- Atributo correto;
- Atributo incorreto;
- Quando cilindrada ou peso são maiores ou menores que os valores da moto secreta.

### RF03 — Modo Diário

- O sistema deverá selecionar uma moto por dia.
- Todos os jogadores deverão receber a mesma moto diária.
- Cada jogador autenticado deverá ter uma única partida diária válida.
- O resultado e a posição no ranking deverão ser registrados após a conclusão.
- Após a vitória, o usuário deverá visualizar o tempo restante até a próxima rodada.

### RF04 — Modo Infinito

- O sistema deverá sortear uma moto aleatória do catálogo ativo.
- Ao acertar a resposta, o usuário poderá iniciar imediatamente uma nova rodada.
- Os resultados do modo infinito não deverão interferir no ranking diário, salvo se essa regra for posteriormente definida.

### RF05 — Dicas

- A rodada diária deverá disponibilizar duas dicas:
  - FIPE;
  - Velocidade máxima declarada pela fabricante.
- Cada dica deverá poder ser utilizada apenas uma vez por partida.
- O uso de dicas deverá gerar penalidade na pontuação ou equivalência em tentativas adicionais.
- A regra de penalidade deverá ser configurável pelo administrador.

### RF06 — Equipes

- O jogador autenticado deverá poder escolher uma fabricante como equipe.
- Cada jogador deverá pertencer a apenas uma equipe por vez.
- O sistema deverá manter um ranking de equipes baseado no total de usuários que acertaram a moto diária.
- O sistema deverá apresentar um ranking interno dos usuários de cada equipe.

### RF07 — Ranking Geral

- O sistema deverá exibir um ranking geral dos jogadores.
- O ranking deverá considerar a quantidade de palpites, penalidades por dicas e horário de conclusão.
- O ranking deverá ser atualizado após a conclusão das partidas.

### RF08 — Administração

- O administrador deverá cadastrar, editar, consultar e desativar motos.
- O administrador deverá manter os dados técnicos atualizados.
- O administrador deverá gerenciar os dados utilizados como dicas.
- O administrador deverá poder consultar as rodadas diárias e os rankings.

---

## 5. Requisitos Não Funcionais

### RNF01 — Plataforma

- A aplicação deverá funcionar como sistema web responsivo.
- A interface deverá priorizar dispositivos móveis.
- A aplicação deverá ser compatível com versões atuais de Chrome, Firefox, Edge e Safari.

### RNF02 — Segurança

- A resposta da moto diária não deverá ficar exposta diretamente no código do navegador antes do encerramento da rodada.
- Usuários só poderão consultar e alterar seus próprios dados.
- Operações administrativas deverão exigir perfil de administrador.
- A aplicação deverá possuir proteção contra requisições excessivas e tentativas automatizadas de manipular rankings.

### RNF03 — Desempenho

- O carregamento inicial deverá ser otimizado para conexões móveis.
- Consultas de ranking deverão possuir paginação ou limite de resultados.
- O catálogo de motos deverá ser pesquisável rapidamente por marca e modelo.

### RNF04 — Disponibilidade

- O sistema deverá permanecer disponível durante todo o período das rodadas diárias.
- O processo de troca da moto diária deverá ser automatizado.
- Dados de usuários, partidas e rankings deverão possuir mecanismos de backup.

---

## 6. Serviços AWS Indicados

A estimativa de custos deverá ser montada na [AWS Pricing Calculator](https://calculator.aws), com envio da URL pública para conferência. Os serviços recomendados para compor essa estimativa são:

| Serviço AWS | Finalidade no projeto |
|---|---|
| **Amazon S3** | Hospedagem de arquivos estáticos, imagens, ícones e catálogo público de motos em JSON. |
| **Amazon CloudFront** | Distribuição rápida do frontend e dos arquivos armazenados no S3. |
| **Amazon API Gateway** | Exposição das APIs utilizadas pelo frontend, como palpites, rankings, partidas e dados de motos. |
| **AWS Lambda** | Execução serverless da lógica do jogo, validação de palpites, cálculo de rankings e administração. |
| **Amazon DynamoDB** | Armazenamento serverless dos dados do jogo, usuários, partidas, palpites, rankings e catálogo de motos. |
| **Amazon Cognito** | Cadastro, login, autenticação e controle de sessões dos usuários. |
| **Amazon EventBridge Scheduler** | Agendamento automático da troca da moto diária. |
| **AWS IAM** | Controle de permissões entre usuários, APIs, funções Lambda e bancos de dados. |
| **Amazon CloudWatch** | Monitoramento de erros, logs das funções Lambda, métricas e alertas operacionais. |
| **AWS WAF** | Proteção da API e da aplicação contra tráfego malicioso e excesso de requisições. |
| **AWS Backup** | Políticas de backup e retenção dos dados armazenados. |

A arquitetura utilizará o modelo **serverless**, reduzindo custos em períodos de baixa utilização.
