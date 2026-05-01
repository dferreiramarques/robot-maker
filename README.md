# 🤖 Robot Maker

**Jogo de tabuleiro educativo** de worker placement e construção de robot.
Desenvolvido no âmbito do programa InGaming do **CICF — Centro de Inovação Carlos Fiolhais**, Maia.

> Constrói o teu robot. Programa-o. Vence a arena.

---

## 📦 Conteúdo do Repositório

```
robot-maker/
├── app/
│   └── robot-maker-sim.html        # Simulador jogável no browser (4 jogadores)
├── print/
│   └── robot-maker-regras-cartas.html  # Regras + cartas para imprimir
├── docs/
│   └── robot-maker-tutorial-notebooklm.docx  # Guia de tutorial (para NotebookLM)
└── README.md
```

---

## 🎮 O Jogo

**Robot Maker** é um jogo de tabuleiro para **2 a 4 jogadores**, com duração de **45 a 90 minutos**, recomendado a partir dos **8 anos**.

Cada jogador é um engenheiro de robots que compete para construir o robot mais eficiente, adquirindo peças no mercado partilhado, gerindo blocos de código como recurso, e activando circuitos eléctricos que desbloqueiam workers extra.

### Objectivo
Ser o jogador com mais pontos quando alguém completar o robot (6 slots preenchidas + 1 peça L3).

---

## ⚙️ Mecânicas Principais

### O Robot — 6 Slots
| Slot | Função |
|---|---|
| 🧠 Cabeça | Detecção e análise |
| ⚙️ CPU | Amplifica peças adjacentes |
| 🦺 Tronco | Resistência e defesa |
| 💪 Braço | Combate e força |
| 🔧 Interface | Conectividade e hack |
| 🦿 Pernas | Mobilidade |

**Progressão obrigatória:** Slot vazia → L1 → L2 → L3 (sem saltar níveis).

### Recursos
- **⬛ Bloco L1** — recurso base, obtido no Compilador (+3 por worker)
- **🟡 Bloco L2** — recurso avançado, obtido convertendo 2×L1 no Optimizador

### Slots do Tabuleiro
| Slot | Capacidade | Efeito |
|---|---|---|
| 🔵 Compilador | Ilimitado | +3 L1 por worker |
| 🟡 Optimizador | Ilimitado | 2×L1 → 1×L2 |
| 🔴 Forja | Exclusivo | Peça L1 grátis |
| 🟢 Deploy | Exclusivo | +6 pts imediatos |

### CPU — Amplificação
| CPU | L1 | L2 | L3 |
|---|---|---|---|
| 🟢 Bio-Core | Cabeça ×2 | Cabeça ×2.5 | Cabeça+Pernas ×3 |
| 🔴 Combat Core | Braço+Interface ×2 | ×2.5 | Braço+Interface+Tronco ×3 |
| 🟡 Agility Core | Pernas ×2 | ×2.5 | Pernas+Cabeça ×3 |
| 🔵 Shield Core | Tronco ×2 | ×2.5 | Tronco+Braço+Interface ×3 |
| ⚪ Omni Core (L3) | — | — | Todas as zonas ×1.5 |

### Circuitos — Worker Cinzento ⚙
5 circuitos únicos no jogo. Cada carta de circuito começa com 1 worker cinzento ⚙ em cima. Quando completas as 3 slots necessárias, recolhes o ⚙ — passa a ser o teu worker permanente (máx. 3 workers por jogador).

| Circuito | Slots |
|---|---|
| ⚡ Central | Cabeça + Tronco + Pernas |
| ⚡ Combate | Cabeça + Tronco + Braço |
| ⚡ Mobilidade | Tronco + Braço + Pernas |
| ⚡ Armas | Braço + Interface + Tronco |
| ⚡ Processo | Cabeça + Braço + Interface |

### Ordem de Turno
Roda 1 posição por ronda automaticamente — sem cálculos, sem vantagem estrutural.

```
Ronda 1: P1 → P2 → P3 → P4
Ronda 2: P2 → P3 → P4 → P1
Ronda 3: P3 → P4 → P1 → P2
...
```

### Pontuação Final (Opção C)
Peças **não** dão pontos ao comprar. Só o `robotScore()` no fim conta:
- Valor base de cada peça instalada
- Multiplicado pelo factor da CPU (se na zona amplificada)
- **+8 pts** se robot completo (6/6 slots)
- **+5 pts** se tem pelo menos 1 peça L3
- **+6 pts** acumulados do Deploy (único ganho durante o jogo)

---

## 🚀 Deploy no Railway

A pasta `app/` é o projecto Node.js pronto a fazer deploy.

**Estrutura da app:**
```
app/
├── public/
│   └── index.html          ← o jogo (servido como página principal)
├── server.js               ← Express server (lê PORT do Railway)
├── package.json
├── railway.json
└── .gitignore
```

**Passos:**
```bash
cd app
npm install
# push para um repo Git e liga ao Railway — auto-detecta Node.js
# ou usa Railway CLI:
railway login
railway init
railway up
```

O Railway define automaticamente a variável `PORT` — o servidor usa-a via `process.env.PORT`.

---

## 🖥️ O Simulador (`app/robot-maker-sim.html`)

Abre directamente no browser — sem instalação, sem servidor.

**Funcionalidades:**
- 1 jogador humano + 3 IAs com estratégias distintas (aggressive, defensive, balanced)
- Mercado dinâmico com stock real (L1×4, L2×2, L3×1 por tipo)
- 5 circuitos únicos esgotáveis globalmente
- CPUs L1/L2/L3 com amplificação correcta (L3 = ×3)
- Ordem de turno por rotação
- Progressão sequencial obrigatória L1→L2→L3
- Pontuação Opção C — só robotScore no final
- Log detalhado de todas as acções com explicação das regras

**Como jogar:** abre o ficheiro `robot-maker-sim.html` no Chrome ou Firefox e clica em **▶ Iniciar**.

---

## 🖨️ Impressão (`print/robot-maker-regras-cartas.html`)

Abre no browser e usa **Ctrl+P** para imprimir. Contém:

- **Pág. 1** — Regras completas em formato A4 (para laminar)
- **Pág. 2** — Peças do Robot: Cabeça, Tronco, Braço, Interface, Pernas (L1/L2/L3)
- **Pág. 3** — CPUs (L1, L2, L3 + Omni Core)
- **Pág. 4** — Circuitos + Tokens de worker + Anatomia do robot

**Recomendação de impressão:** activar "imprimir fundos" nas opções do browser.

---

## 📊 Balanço e Validação

O jogo foi testado com **200 simulações** de partidas completas. Resultados:

| Métrica | Resultado | Veredito |
|---|---|---|
| Vantagem do 1º jogador | +1.5pp vs esperado 25% | ✅ Neutro |
| Spread entre posições de turno | 7pp | ✅ Equilibrado |
| Spread de pontuação (1º vs 4º) | ~124 pts | ✅ Aceitável |
| Rondas médias por partida | ~13 | ✅ Adequado |

---

## 🗂️ Historial de Decisões de Design

| Decisão | Opção escolhida | Razão |
|---|---|---|
| Ordem de turno | Rotação fixa por ronda | Worker placement criava +17pp para pos. #4 |
| Circuitos | 5 únicos no jogo total | 7 era demasiado — sem tensão real |
| Worker extra | Cinzento no circuito desde o setup | Clareza visual — o ⚙ promete o bonus |
| Scoring de peças | Só robotScore no fim (Opção C) | Spread 241→124 pts, sem dupla contagem |
| CPU L3 factor | ×3 | ×2 não incentivava especialização |
| Bónus de conclusão | +8 robot completo / +5 tem L3 | Reduzido de +15/+10 para menor spread |

---

## 👤 Autoria

**David Marques**
Coordenador · CICF — Centro de Inovação Carlos Fiolhais, Maia
Programa InGaming · CDI Portugal · Norte 2030

---

## 📄 Licença

Protótipo educativo — todos os direitos reservados.
Versão 0.2 · Maio 2026
