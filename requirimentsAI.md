# Documento de Especificação de Requisitos - Jogo de Adivinhação de Motos

## 1. Visão Geral do Produto
Aplicação web responsiva no formato de jogo diário e infinito de adivinhação de motos comercializadas oficialmente no Brasil, comparando atributos técnicos a cada palpite.

---

## 2. Requisitos Funcionais (RF)

### RF01 - Mecânica de Palpites e Comparação
- **RF01.1 - Busca e Autocomplete:** Campo de busca que filtra por Fabricante e Modelo conforme digitação, impedindo palpites inválidos fora da base.
- **RF01.2 - Feedback Visual por Atributos:** A cada palpite, o sistema deve comparar os atributos da moto palpitada com a moto secreta:
  - **Fabricante:** Exato (Verde) / Incorreto (Vermelho).
  - **Estilo:** Exato (Verde) / Incorreto (Vermelho).
  - **Cilindradas (cc):** Exato (Verde) / Maior/Menor com setas indicativas (Amarelo ou Vermelho).
  - **Peso em Ordem de Marcha (kg):** Exato (Verde) / Maior/Menor com setas indicativas.
- **RF01.3 - Condição de Vitória:** O jogo finaliza quando todos os atributos coincidirem.
- **RF01.4 - Limite de Tentativas:** Definir limite padrão (ex: 6 tentativas) ou modo livre com contagem progressiva de palpites.

### RF02 - Modos de Jogo
- **RF02.1 - Modo Diário:**
  - Sorteio determinístico único por data (fuso horário `America/Sao_Paulo`).
  - Uma única tentativa diária por usuário/dispositivo.
  - Bloqueio de jogo após conclusão com contador regressivo para o próximo desafio (às 00:00).
- **RF02.2 - Modo Infinito:**
  - Seleção aleatória contínua de motos do catálogo.
  - Ao acertar, opção de reiniciar imediatamente uma nova rodada independente do ranking diário.

### RF03 - Sistema de Dicas
- **RF03.1 - Dica 1:** Faixa de preço / Valor Médio da Tabela FIPE (bloqueada por padrão).
- **RF03.2 - Dica 2:** Velocidade máxima declarada pela fabricante (bloqueada por padrão).
- **RF03.3 - Penalidade de Pontuação:** Cada dica desbloqueada adiciona penalidade no score do ranking (ex: equivalente a +2 tentativas por dica usada).

### RF04 - Gestão de Usuários e Equipes
- **RF04.1 - Autenticação:** Login social (Google/OAuth) e modo visitante (armazenamento local via `localStorage`).
- **RF04.2 - Escolha de Equipe:** Usuário autenticado seleciona uma marca oficial como sua "Equipe" (ex: Honda, Yamaha, Kawasaki, BMW, Triumph, etc.).
- **RF04.3 - Troca de Equipe:** Permitida com restrições temporais (ex: 1 troca por mês/temporada).

### RF05 - Rankings e Pontuação
- **RF05.1 - Critérios de Classificação Individual:**
  1. Menor número total de palpites (incluindo penalidades de dicas).
  2. Menor tempo de conclusão (desempate por timestamp UTC).
- **RF05.2 - Tipos de Leaderboards:**
  - **Ranking Geral Diário:** Todos os jogadores do dia.
  - **Ranking de Equipes:** Classificação das fabricantes pelo total de acertos acumulados de seus membros.
  - **Ranking Interno da Equipe:** Melhores pontuações entre usuários da mesma marca.

### RF06 - Compartilhamento e Estatísticas
- **RF06.1 - Compartilhamento Sem Spoiler:** Gerador de grid de emojis (🟩 🟥 ⬆️ ⬇️) para área de transferência (WhatsApp, X/Twitter, etc.).
- **RF06.2 - Estatísticas do Usuário:** Gráficos de vitórias, taxa de acerto, sequência de vitórias (*streaks*) e distribuição de palpites.

---

## 3. Requisitos Não Funcionais (RNF)

- **RNF01 - Performance:** Tempo de carregamento inicial (LCP) inferior a 2 segundos; payload estático mínimo.
- **RNF02 - Compatibilidade e Responsividade:** *Mobile-first*, compatível com Chrome, Safari, Edge e Firefox modernos.
- **RNF03 - Segurança:**
  - Resolução do enigma diário validada no backend (para evitar que o usuário descubra a resposta inspecionando o JSON local).
  - Rate limiting nas rotas da API via Edge/Serverless.
- **RNF04 - Disponibilidade:** 99.9% de uptime, suportando picos de tráfego na virada diária de rodada.

---

## 4. Requisitos de Dados (Dataset)

### Estrutura do Objeto Moto (`motos.json` / Tabela SQL):
```json
{
  "id": "uuid-v4",
  "fabricante": "Yamaha",
  "modelo": "MT-07",
  "ano_modelo": 2024,
  "cilindrada": 689,
  "peso_ordem_marcha": 184,
  "estilo": "Naked",
  "dica_fipe": "R\$ 47.990",
  "dica_velocidade_max": "214 km/h",
  "ativa": true
}
