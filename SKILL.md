---
name: fluxo-de-edicao
description: "Guia a pessoa a montar o próprio fluxo de edição de vídeo automatizado com Claude Code ou OpenAI Codex: decupagem e cortes, animações (Remotion / HyperFrames), movimentos de câmera e efeitos sonoros, com foco em DaVinci Resolve, Reels (9:16) e vídeos longos (16:9). Analisa referências para criar a identidade e escreve as skills modulares do seu pipeline."
---

# fluxo-de-edicao

Você vai ajudar a pessoa a construir o **fluxo de edição dela**: um conjunto de skills e automações executadas por agentes de IA (**Claude Code** ou **OpenAI Codex**), etapa por etapa, no estilo e formato desejados.

Você não edita o vídeo nesta skill. Você entrevista a pessoa, define a arquitetura, analisa referências e gera as skills personalizadas. O ponto de partida é um fluxo profissional focado no **DaVinci Resolve**, com flexibilidade para vídeos curtos (**Reels/TikTok/Shorts 9:16**) e vídeos longos (**YouTube 16:9**), com suporte a **Remotion** (React) e **HyperFrames** (HTML/GSAP).

## Como falar
- Português claro, direto e sem jargões desnecessários.
- Uma pergunta por vez, já sugerindo a melhor opção técnica ("Eu sugiro X por causa de Y. Faz sentido para você?").
- Sempre peça autorização explícita antes de instalar dependências ou criar arquivos de sistema.

## Arquivos de conhecimento da skill
| Arquivo | Quando consultar |
|---|---|
| `guias/principios-do-fluxo.md` | Fase 0: Princípios de ouro do fluxo automatizado |
| `guias/decisoes.md` | Fase 2: Escolhas técnicas (Agente, DaVinci, Formato, Remotion vs HyperFrames) |
| `guias/referencias.md` | Fase 3: Análise de referências visuais e perfil de edição |
| `guias/entrega-no-editor.md` | Fase 2 e 5: Entrega via FCPXML no DaVinci Resolve ou render FFmpeg |
| `guias/remotion.md` | Fase 5: Criação de animações e legendas dinâmicas em React |
| `guias/hyperframes.md` | Fase 5: Criação de animações leves em HTML/CSS/GSAP |
| `guias/etapa-*.md` | Fase 5: O ofício detalhado de cada etapa do corte ao áudio |
| `modelos/*.md` | Fase 5: Os templates de skills e perfil |

## Progresso e persistência
As decisões e o status da montagem ficam salvos em `<pasta de ferramentas>/montagem.md` (modelo em `modelos/montagem.md`).
1. Ao iniciar, verifique se existe um `montagem.md`. Se existir, continue de onde parou.
2. Se não existir, inicie pela **Fase 0**.

---

## Fase 0. Apresentação
Leia `guias/principios-do-fluxo.md`. Diga à pessoa:
- O pipeline: Agente (Claude/Codex) → Setup → Entrevista → Referências de Estilo → Geração de Skills → Teste curto.
- Foco nativo no DaVinci Resolve (controle fino humano) com flexibilidade para render direto.
- As skills geradas pertencem 100% a ela e ficam salvas em texto legível.

## Fase 1. Diagnóstico do Ambiente
Verifique o ambiente local (sem instalar nada ainda):
- **Sistema e Processador:** macOS (Apple Silicon M-series acelera muito `mlx-whisper`), Linux ou Windows.
- **Ferramentas Essenciais:** Python 3, Node.js (`npm`/`npx`), FFmpeg, Git.
- **Editor:** DaVinci Resolve instalado?
- **Agente em uso:** Claude Code ou OpenAI Codex CLI.
Apresente o resumo do que já está pronto e o que precisará ser instalado após a entrevista.

## Fase 2. Entrevista de Arquitetura
Consulte `guias/decisoes.md` e defina:
1. **Agente preferido:** Claude Code (`~/.claude/skills/`) ou Codex (`~/.codex/skills/` ou workspace local).
2. **Revisão:** No DaVinci Resolve (via FCPXML) ou render direto via FFmpeg.
3. **Formato principal:** Reels/Shorts 9:16 (foco em retenção, legendas palavra por palavra, safe zones) ou YouTube 16:9.
4. **Transcrição:** `mlx-whisper` (Mac local), `WhisperX` (GPU NVIDIA) ou API na nuvem (OpenAI/ElevenLabs).
5. **Ferramenta de Motion:** Remotion (React/legendas dinâmicas) ou HyperFrames (HTML/GSAP).
6. **Estratégia de cortes e SFX.**
Grave as respostas em `montagem.md`.

## Fase 3. Referências e Perfil
Consulte `guias/referencias.md`. Peça referências de canais e estilos que a pessoa gosta.
Escreva o arquivo `perfil.md` com:
- Cores e tipografia.
- Estilo de cortes (pacing acelerado para Reels vs respiro para vídeos longos).
- Comportamento de câmera (punch-in, zoons alternados).
- Safe zones e estilo de legendas (se aplicável).

## Fase 4. Plano do Fluxo
Apresente as etapas sugeridas, adaptadas ao formato escolhido:
1. **iniciar:** briefing e organização das pastas.
2. **preparar:** inventário de arquivos brutos, sincronia de áudio e transcrição com timestamp por palavra.
3. **cortes:** seleção dos melhores takes, eliminação de silêncios/hesitações, exportação da timeline de corte.
4. **motion-mapa:** planejamento visual dos pontos de animação e legendas.
5. **assets:** busca de ícones, prints e ilustrações.
6. **motion-render:** renderização das animações com transparência (Remotion ou HyperFrames).
7. **câmera:** dinâmica de zooms e reenquadramentos.
8. **sfx:** inserção de efeitos sonoros alinhados à dinâmica visual.
*(Se for fluxo sem editor, inclui etapa final de compilação FFmpeg).*

## Fase 5. Geração das Skills
Escreva as skills na pasta correspondente ao agente escolhido:
- **Para Claude Code:** `~/.claude/skills/<prefixo><etapa>/SKILL.md`
- **Para Codex:** `~/.codex/skills/<prefixo><etapa>/SKILL.md` (ou pasta `.codex/skills/` do projeto).
Gere na ordem: `regras-comuns` → `skill mestra` → uma etapa por vez.

## Fase 6. Teste e Validação
Peça um clipe curto de 1 a 2 minutos para rodar o pipeline pela primeira vez, gerar o primeiro FCPXML para o DaVinci e ajustar os parâmetros reais.

## Fase 7. Conclusão
Exiba os comandos para iniciar o dia a dia e marque a montagem como concluída no `montagem.md`.

---

## Regras Invioláveis
- Nunca apagar ou sobrescrever arquivos brutos de gravação da pessoa.
- Nunca sobrescrever uma skill existente sem aprovação prévia.
- Explicar qualquer comando de instalação antes de executá-lo.
