# IDEIA

Produzir um jogo estilo Wordle/Termo com motos vendidas no Brasil, sendo limitadas as motos a venda oficialmente no país, desconsiderando importações.

# FUNCIONALIDADES

## FUNCIONALIDADES BÁSICAS

* O usuário dá palpites digitando marca e modelo da Moto;
* O usuário pode escolher uma marca como "Equipe";

## MODO DIÁRIO
* As motos devem ser classificadas com as seguintes características:

    * Fabricante
    * Modelo
    * Cilindradas
    * Peso em ordem de marcha
    * Estilo [Naked, Street, Trail, Sport, Touring]

* A moto deve ser mudada diariamente;
  
## MODO INFINITO

* Deve seguir as mesmas regras básicas do modo diário;

* Ao acertar a moto, outra moto aleatória é sorteada.

## RANKING

* O ranking deve ser organziado pela menor quantidade de palpites, quanto menos palpites maior o ranking;
    * O desempate é feito pela ordem de resposta, o usuário que respondeu primeiro tem prioridade superior à aquele que teve a mesma quantidade de palpites.

* Deve haver dois rankings:
  * O ranking das equipes:
    * Deve haver um ranking das equipes com maior número de usuários que acertaram;
    * Dentro de cada equipe deve haver um ranking dos usuários da mesma equipe;
  * O ranking Geral:
    * Deve conter o ranking geral dos usuários 
    
## SISTEMA DE DICAS

* Deve haver um sistema de dicas:

    * Devem haver duas dicas para a moto do dia:
        
        * FIPE
        * Velocidade máxima (declarada pela fabricante)
    
    * As dicas afetam o ranking:

        * Usar as dicas diminuem o ranque.

* Ao acertar a moto, encerra o modo.


# ARQUITETURA

## WEB
* Serverless

* Dataset das motos em JSON

## CUSTOS

* Bancos de dados de usuários (Supabase)

* AWS

* Google Ads

***