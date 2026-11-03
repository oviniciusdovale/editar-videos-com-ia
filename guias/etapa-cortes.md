# Etapa 3: Cortes (Decupagem e Ritmo)

O primeiro corte limpo: escolhe os melhores takes, remove falsas largadas, notas de edição e pausas desnecessárias. Sem animação ou efeito ainda. Esta é a espinha dorsal sobre a qual todas as outras etapas serão construídas.

---

## 1. Pacing: Reels (9:16) vs. Vídeo Longo (16:9)

* **Para Reels / Shorts (Vertical):**
  - **Ritmo de Retenção:** Silêncios mortos maiores que `0.25s` a `0.3s` devem ser eliminados sumariamente.
  - **Jump Cuts Naturais:** Em Reels, o corte seco entre frases não é apenas aceito, mas esperado. Use micro-pausas mínimas (~60ms a 100ms de ar antes da próxima palavra).
  - **Gancho nos Primeiros 3 Segundos:** O primeiro take deve começar imediatamente sem respiração antes da primeira sílaba.
* **Para Vídeos Longos / YouTube:**
  - **Ritmo Natural e Respirado:** Manter pausas reflexivas (0.4s a 0.8s) entre blocos de ideias para não soar robótico.
  - **Preservação de Cadência:** Deixar pequenos respiros naturais da fala do apresentador.

---

## 2. Como Decidir os Takes

- **Leia por frase, decida por frase:** A transcrição por frase com timestamps é o guia do agente.
- **Identifique repetições:** Frases recomeçadas, "pera", "deixa eu regravar", "vou voltar dali".
- **Critério de escolha do take:**
  1. Nota falada de edição ("usa esse take");
  2. O último take completo gravado (geralmente é o mais natural);
  3. Se o último engasgou e um anterior está impecável, selecione o anterior.
- **Notas de edição faladas:** O trecho de abertura até o fechamento da nota é completamente removido da timeline.
- **Na dúvida entre dois takes:** Escolha o melhor tecnicamente e adicione um marcador na timeline indicando o take alternativo para a pessoa conferir no DaVinci.

---

## 3. Ajuste de Bordas de Áudio (Snap de Silêncio)

- O agente identifica as palavras de início e fim.
- O script analisa o waveform para não "mutilar" a primeira consoante da palavra (ex: 'P', 'T', 'C') e deixa ~50ms a 80ms de pré-roll.
- **Micro-fades cruzados:** Sempre 1 a 2 frames de crossfade para evitar cliques audíveis nos pontos de emenda.

---

## 4. Marcadores na Linha do Tempo (DaVinci Resolve)

Em vez de gerar relatórios longos de texto no chat, insira marcadores coloridos diretamente na timeline do DaVinci:
- **Cyan:** Marcador informativo do agente (ex.: take alternativo disponível).
- **Amarelo:** Alerta de áudio ou corte duvidoso para o editor humano ouvir.
- **Verde:** Ponto onde foi solicitada uma inserção visual ou animação futura.

---

## 5. Regra de Ouro: O Corte Trava
Após a pessoa validar e aprovar a timeline de cortes no DaVinci Resolve, a duração dos clipes fica travada. Isso garante que as animações do Remotion/HyperFrames, movimentos de câmera e SFX sejam posicionados nos frames exatos sem ficarem defasados.
