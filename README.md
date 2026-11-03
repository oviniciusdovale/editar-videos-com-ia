# Fluxo de Edição com IA (DaVinci Resolve + Reels & Vídeos Longos)

Uma meta-skill inteligente para **Claude Code** e **OpenAI Codex** que te guia, passo a passo, a construir o **seu pipeline profissional de edição automatizada de vídeo**: decupagem cirúrgica, animações programadas (**Remotion** e **HyperFrames**), dinâmica de câmera, legendas palavra por palavra e sonoplastia em camadas.

O fluxo tem como editor de referência o **DaVinci Resolve** e foi desenhado sob medida para **vídeos falados**, com suporte especializado tanto para o ritmo agressivo de **Reels/Shorts/TikTok (9:16)** quanto para a cadência natural de **vídeos longos para o YouTube (16:9)**.

---

## Diferenciais Deste Fluxo

- **Multi-Agente:** Compatível nativamente com **Claude Code** e **OpenAI Codex**.
- **DaVinci Resolve como Protagonista:** Gera timelines em formato FCPXML e integra-se diretamente com o DaVinci (com suporte à API Python nativa do DaVinci Resolve).
- **Dualidade de Formatos:**
  - *Reels / Shorts (9:16):* Respeito às Safe Zones das redes sociais, eliminação de micro-pausas (< 0.3s) e legendas dinâmicas estilo karaokê.
  - *Vídeos Longos (16:9):* Cortes com respiros naturais, motion de tela cheia e lower-thirds informativos.
- **Remotion + HyperFrames:** Escolha entre animações e legendas em React/Tailwind (**Remotion**) ou templates leves em HTML/CSS/GSAP (**HyperFrames**), ambos exportando com transparência (canal alfa) para sobreposição na timeline.
- **Totalmente Personalizável:** O fluxo não impõe um molde visual rígido. Ele analisa as suas referências de estilo e gera skills exclusivas para você.

---

## Pré-requisitos

1. Um agente de IA de terminal:
   - [Claude Code](https://claude.com/claude-code) (com assinatura Pro/Team) OU
   - [OpenAI Codex](https://github.com/openai/codex) / Codex CLI.
2. [DaVinci Resolve](https://www.blackmagicdesign.com/products/davinciresolve) instalado (versão gratuita ou Studio).
3. Dependências de sistema (a própria skill diagnostica o que você já tem e orienta a instalação):
   - Python 3.10+
   - Node.js (v18+) e FFmpeg
   - Transcrição local rápida (`mlx-whisper` no Mac Apple Silicon ou `WhisperX` com GPU NVIDIA) ou API na nuvem.

---

## Como Instalar

### No Claude Code:

Basta abrir o Claude Code e digitar:

```bash
Instala esta skill para mim: https://github.com/oviniciusdovale/editar-videos-com-ia
```

Ou manualmente pelo terminal:

```bash
git clone https://github.com/oviniciusdovale/editar-videos-com-ia ~/.claude/skills/editar-videos-com-ia
```

### No OpenAI Codex (CLI / Agents):

Clone para a sua pasta de skills do Codex ou adicione ao seu workspace:

```bash
git clone https://github.com/oviniciusdovale/editar-videos-com-ia ~/.codex/skills/editar-videos-com-ia
```

---

## Como Usar

No seu terminal com o Claude Code ou Codex ativo, execute:

```bash
/fluxo-de-edicao
```

A skill iniciará uma sessão interativa:

1. **Ambiente:** Analisa o seu hardware (macOS Apple Silicon, GPU, FFmpeg, Python, Node).
2. **Entrevista de Arquitetura:** Define seu agente (Claude Code ou Codex), editor (DaVinci Resolve), formato alvo (Reels 9:16 ou YouTube 16:9) e biblioteca de animação (Remotion ou HyperFrames).
3. **Análise de Referências:** Mapeia canais e vídeos de referência para definir seu perfil visual, ritmo de corte e estilo sonoro.
4. **Plano de Execução:** Estrutura o pipeline modular adaptado ao seu tipo de vídeo.
5. **Geração das Skills:** Cria uma skill para cada etapa (`cortes`, `motion`, `câmera`, `sfx`) além da skill mestra integradora.
6. **Teste em Clipe Real:** Roda um teste com um vídeo curto (1 a 2 min) para validar a importação no DaVinci Resolve.

---

## Estrutura do Repositório

```
fluxo-de-edicao/
├── SKILL.md                 # Roteiro mestre da entrevista e geração do pipeline
├── guias/                   # Base de conhecimento e ofício técnico
│   ├── decisoes.md          # Critérios de escolha (Agente, DaVinci, Formato, Remotion vs HyperFrames)
│   ├── entrega-no-editor.md # Regras técnicas de exportação FCPXML e API Python do DaVinci
│   ├── remotion.md          # Guia de animações e legendas dinâmicas em React
│   ├── hyperframes.md       # Guia de animações em HTML/CSS/GSAP
│   ├── oficio-motion.md     # Princípios de motion design (curvas, antecipação, ritmo)
│   ├── etapa-cortes.md      # Pacing de decupagem para Reels vs Vídeos Longos
│   ├── etapa-sfx.md         # Mixagem e alinhamento de efeitos sonoros
│   ├── etapa-camera.md      # Zooms, punch-ins e dinâmica de enquadramento
│   └── referencias.md       # Como dissecar vídeos de referência e extrair perfis
└── modelos/                 # Esqueletos e templates dos arquivos gerados
    ├── perfil.md            # Template do perfil estético e safe zones
    ├── montagem.md          # Diário de progresso da configuração
    └── skill-etapa.md       # Estrutura modular de cada etapa
```

---

## Créditos

- **Autor:** [Vinicius do Vale](https://github.com/oviniciusdovale)
- **Inspiração:** Conceito original de meta-skill e princípios de edição por Lucas Leite.
