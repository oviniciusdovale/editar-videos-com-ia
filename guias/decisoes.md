# Decisões da entrevista

Siga esta ordem. Para cada decisão: explique as opções em poucas linhas, dê a sua sugestão para o caso da pessoa, registre a escolha no `montagem.md`. Detalhes que mudam com o tempo (preço, versão, recurso novo), pesquise na hora antes de recomendar.

---

## 0. Agente de IA (Claude Code ou OpenAI Codex)
- **Claude Code:** Roda no terminal, executa comandos bash e scripts, e lê skills globais em `~/.claude/skills/<nome>/SKILL.md`. Invocado por barra (`/edit-cortes`).
- **Codex (OpenAI Codex CLI / Agents):** Executa tarefas via terminal com comandos ou scripts locais. Lê instruções na pasta `.codex/skills/`, regras em `AGENTS.md` ou prompts de contexto.
- **Sugestão:** A arquitetura deste fluxo é agnóstica; você pode usar tanto Claude Code quanto Codex. Pergunte qual ambiente a pessoa está usando para gravar os arquivos no local correto.

---

## 1. Onde a pessoa revisa (pergunte primeiro: muda o fluxo inteiro)
- **No DaVinci Resolve (Recomendado):** O agente gera uma timeline em formato FCPXML (ou injeta via API Python do DaVinci) com cortes, animações, câmera e SFX em trilhas separadas (`V1` a `V4`, `A1` a `A3`). A pessoa revisa com fluidez, ajusta tempos e mantém o controle criativo.
- **Em outro editor (Premiere, Final Cut):** Suporte via XML padrão (FCP7 XML ou FCPXML).
- **Sem programa de edição:** O agente entrega o vídeo final pronto renderizado via FFmpeg. A revisão é feita assistindo ao `.mp4` e pedindo ajustes pelo chat com minutagens.
- Detalhes de cada caminho: `entrega-no-editor.md`.

---

## 2. Tipo de vídeo e formato (Reels 9:16 vs Vídeo Longo 16:9)
- **Reels / Shorts / TikTok (Vertical 9:16 - 1080x1920):**
  - Ritmo acelerado, cortes sem pausas mortas (pausas > 0.3s são eliminadas).
  - Respeito obrigatório às **Safe Zones** (área central limpa; evitar os 20% inferiores e laterais onde ficam os botões da rede social).
  - Legendas dinâmicas animadas (palavra por palavra).
- **Vídeos Longos / YouTube (Horizontal 16:9 - 1920x1080 ou 4K):**
  - Ritmo natural e respirado, respeitando pausas reflexivas do apresentador.
  - Animações de tela cheia (slides, diagramas, dados) e lower-thirds informativos.
- **Taxa de quadros (FPS):** Sempre a taxa real da câmera (24, 25, 29.97, 30, 60 fps). Erro de taxa causa descompasso progressivo de áudio no fim do vídeo.

---

## 3. Pasta e organização
- Sugestão padrão: `AAAA-MM-DD_Nome/` contendo:
  - `Brutos/` (arquivos originais de vídeo e áudio da câmera).
  - `Assets/` (imagens, logos, capturas de tela).
  - `Docs/` (roteiro, transcrição, notas).
  - `Renders/` (saídas parciais ou finais).
  - `_WORK/` (pasta de trabalho do Claude Code ou Codex com os arquivos temporários e timelines geradas).
- Pasta de ferramentas compartilhada entre vídeos (ex.: `~/.edicao/`): scripts Python, ambiente virtual, perfil de estilo, biblioteca de SFX e `montagem.md`.

---

## 4. Transcrição (a base de tudo)
- **Critérios cruciais:**
  - **Timestamp preciso por palavra (word-level):** Essencial para cortes cirúrgicos e legendas animadas em Reels.
  - **Manter hesitações ("hum", "ééé", recomeços):** Para que o agente saiba exatamente o que eliminar nos cortes.
  - **Suporte ao Português:** Reconhecimento fonético de termos técnicos e gírias.
- **Opções recomendadas:**
  - *Local:* `WhisperX` ou `mlx-whisper` (ultrarrápido em Macs Apple Silicon M1/M2/M3/M4). Gratuito e offline.
  - *Nuvem:* ElevenLabs Scribe ou OpenAI Whisper API (para quem não tem GPU dedicada).
- **Armadilha:** Whisper pode omitir frases repetidas rapidamente. O script de preparação deve validar a duração do áudio versus a transcrição.

---

## 5. Notas faladas na gravação
- O criador pode falar instruções durante a gravação para o agente: *"nota de edição, corta esse trecho e usa a próxima tomada, fim da nota"*.
- A etapa de preparação identifica as notas, separa-as para execução e descarta o trecho falado da timeline final.

---

## 6. Cortes (Pacing e Decupagem)
- **Reels/Shorts:** Pacing enxuto. Cortes secos em respiradas e micro-pausas. Na dúvida entre cortar ou manter, corta.
- **Vídeo Longo:** Pacing moderado. Mantém respiros entre parágrafos, preserva a cadência do apresentador.
- **Uso de Roteiro:** Quando fornecido, serve como mapa temático dos tópicos, nunca para forçar o texto exato se o apresentador improvisou bem.

---

## 7. Animações (Motion) e Legendas: Remotion ou HyperFrames
- **Remotion (React + TypeScript):**
  - Recomendado se você busca criar **legendas dinâmicas estilo Reels** (`@remotion/captions`), componentes reutilizáveis e gráficos com Tailwind CSS.
  - Renderiza via CLI (`npx remotion render`) gerando arquivos `.mov` ProRes 4444 com canal alfa (transparência).
  - Detalhes em: `guias/remotion.md`.
- **HyperFrames (HTML + CSS + GSAP):**
  - Recomendado para quem prefere templates rápidos em HTML/JS puro sem compilação React.
  - Leve e direto ao ponto. Detalhes em: `guias/hyperframes.md`.

---

## 8. Câmera e Dinâmica Visual
- Zooms alternados nos cortes (jump-cuts disfarçados com corte seco entre enquadramento normal e zoom de 115%).
- Zooms de ênfase (punch-in) em momentos-chave e punch-out de relaxamento.
- Detecção e tracking de rosto (opcional via OpenCV YuNet) para centralizar apresentadores em vídeos 9:16 gravados na horizontal.

---

## 9. Som e Efeitos Sonoros (SFX)
- Biblioteca local organizada por categorias: *whoosh*, *impacto/pop*, *clique*, *glitch*, *riser*.
- Automação de posicionamento: o agente alinha os efeitos sonoros aos frames exatos de entrada/saída das animações e cortes.
- Nível de áudio sugerido: voz em -6dB a -12dB (com compressor/limiter); SFX entre -18dB e -24dB; música de fundo em -26dB a -32dB.

---

## 10. Revisão e Retorno
- Ciclo de trabalho: o agente gera a timeline FCPXML -> DaVinci Resolve importa -> a pessoa assiste e ajusta se desejar -> o agente lê as correções feitas na timeline do DaVinci.

---

## 11. Nome das Skills e Comandos
- Prefixo das skills (ex.: `edit-` gera `/edit-cortes`, `/edit-motion`, etc.).
- Nome da skill mestra (ex.: `/editar` ou `/fluxo`).
- Configuração de pasta:
  - Claude Code: `~/.claude/skills/<prefixo><etapa>/SKILL.md`
  - Codex: `~/.codex/skills/<prefixo><etapa>/SKILL.md` ou regras no workspace.
