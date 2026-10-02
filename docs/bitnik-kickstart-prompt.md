# Prompt — Bitnik Platform (kickstart + teste do Forge)

Copiar tudo abaixo da linha para a Bitnik Platform.

---

## Projeto: Robot Maker (digital)

**Género:** jogo de tabuleiro digital por turnos, worker placement + construção de robot.
**Jogadores:** 2–4 (1 humano + até 3 IAs; depois multiplayer online). **Duração:** 45–90 min. **Idade:** 8+.
**Contexto:** projeto educativo InGaming, CICF Maia (STEM: engenharia, lógica, recursos). Língua da UI: **português de Portugal**.
**Referência existente:** simulador HTML single-file em https://github.com/ (repo `robot-maker`, `public/index.html`) — usar como fonte de verdade das regras.

### Objetivo
Cada jogador é um engenheiro que constrói o robot mais eficiente. O jogo acaba quando alguém completa o robot; ganha quem tiver mais pontos (`robotScore`).

### Recursos
- **Bloco L1** (base) e **Bloco L2** (avançado). Cada jogador começa com 0.

### Tabuleiro — 4 slots de worker placement
| Slot | Capacidade | Efeito |
|---|---|---|
| 🔵 Compilador | ilimitado | +3 L1 por worker |
| 🟡 Optimizador | ilimitado | 2×L1 → 1×L2 (sem 2×L1 = worker gasto sem efeito) |
| 🔴 Forja | exclusivo | 1 peça L1 grátis à escolha |
| 🟢 Deploy | exclusivo | +6 pts imediatos |

Cada jogador tem 1 worker inicial, máx. 3. Turno: colocar 1 worker **ou** comprar 1 peça no mercado **ou** passar. Ordem de turno roda 1 posição por ronda (sem cálculo de prioridade).

### Robot — 6 slots
Cabeça (head), Tronco (chest), Braço (arm), Interface (port), Pernas (legs), CPU (cpu).
Progressão **obrigatória** por slot: vazio → L1 → L2 → L3 (sem saltar níveis).

### Peças (custo em L1/L2 · pontos base)
| Slot | L1 | L2 | L3 |
|---|---|---|---|
| Cabeça | 2/0 · 3 | 1/1 · 8 | 1/1 · 15 |
| Tronco | 2/0 · 4 | 0/1 · 8 | 1/1 · 12 |
| Braço | 2/0 · 3 | 0/1 · 8 | 1/1 · 12 |
| Interface | 2/0 · 4 | 0/1 · 8 | 1/1 · 12 |
| Pernas | 2/0 · 3 | 1/1 · 8 | 1/1 · 12 |

Nomes: Cabeça Básica/Sensor/IA · Tronco Leve/Blindado/Adaptativo · Braço Simples/Laser/Plasma · Porta Série/Porta Dados/Antena Quântica · Pernas Standard/Turbo/Gravitação.

### CPUs — amplificam a pontuação das zonas indicadas
| CPU | L1 (2/0, 5pts) | L2 (1/1, 9pts) | L3 (1/1, 14pts) |
|---|---|---|---|
| Bio-Core | Cabeça ×2 | Cabeça ×2.5 | Cabeça+Pernas ×3 |
| Combat Core | Braço+Interface ×2 | ×2.5 | Braço+Interface+Tronco ×3 |
| Agility Core | Pernas ×2 | ×2.5 | Pernas+Cabeça ×3 |
| Shield Core | Tronco ×2 | ×2.5 | Tronco+Braço+Interface ×3 |
| Omni Core (só L3, 2/1, 18pts) | — | — | Todas as zonas ×1.5 |

### Circuitos (5 únicos, esgotáveis globalmente)
Cada carta começa com 1 worker cinzento ⚙. Quem preencher as 3 slots indicadas recolhe-o como worker permanente (máx. 3).
Central: Cabeça+Tronco+Pernas · Combate: Cabeça+Tronco+Braço · Mobilidade: Tronco+Braço+Pernas · Armas: Braço+Interface+Tronco · Processo: Cabeça+Braço+Interface.

### Mercado
Stock real partilhado por tipo de peça: L1×4, L2×2, L3×1. Comprar reduz stock.

### Fim de jogo e pontuação
- **Trigger de fim:** um jogador tem as **6 slots preenchidas e uma delas é peça L3** (basta 1 L3 em qualquer slot, CPU incluída). A ronda em curso termina e o jogo acaba.
- **Pontuação final** = Σ (pts da peça × fator da CPU se a zona for amplificada) + pts da CPU + pontos acumulados do Deploy (+6 cada vez que o slot é usado).
- **Bónus de conclusão:** +8 se robot completo (6/6 slots) · +5 se tem pelo menos 1 peça L3.
- Peças não dão pontos ao comprar; só contam no cálculo final (Opção C).

### IAs (3 estratégias)
BETA-7 *aggressive* (nega peças ao adversário), GRIX *defensive*, LUMIS *efficiency* (maximiza ROI de recursos).

### Requisitos técnicos
- Web app responsiva (desktop + tablet + telemóvel), deploy Node/Express (Railway).
- Estado de jogo puro e serializável (`G`), lógica separada da UI → facilita multiplayer e testes.
- Log de ações com explicação da regra aplicada (valor pedagógico).
- Testes automáticos: simulação de 200 partidas IA-vs-IA a validar balanço (1.º jogador ≈25%, ~13 rondas, spread 1.º–4.º ≲125 pts).
- Identidade visual: usar o design system bitnikgames.

### Entregas esperadas do Forge (por ordem)
1. Estrutura do projeto + motor de regras (`rules/` puro, sem UI) com testes unitários dos custos, progressão L1→L2→L3, CPU, circuitos e fim de jogo.
2. UI jogável 1 humano + 3 IAs a partir do motor.
3. Harness de simulação 200 jogos + relatório de balanço.
4. Multiplayer online (salas, 2–4 jogadores) como fase seguinte.

### Pontos a confirmar durante o kickstart
- Nº inicial de workers, ações por turno e limite de rondas não estão definidos no README; o Forge deve propor valores a partir do simulador e documentá-los.
