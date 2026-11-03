# Entrega no DaVinci Resolve (ou vídeo pronto)

Como o resultado de cada etapa (cortes, animações, câmera, áudio/SFX) chega à pessoa. O **DaVinci Resolve** é o editor de referência deste fluxo por ser gratuito, profissional e oferecer suporte excelente a FCPXML e automação via API Python.

---

## 1. Regras Fundamentais (Para Qualquer Formato)

* **Teste curto primeiro:** Na primeira execução, gere um teste com 3–4 clipes e abra no DaVinci. Valide sincronia, canais de áudio, markers e enquadramento. Salve a validação na pasta de ferramentas.
* **Apontar para originais:** Sempre utilize caminhos absolutos para os arquivos de mídia brutos.
* **Controle de versões:** Nunca sobrescreva timelines. Gere `<projeto>_cortes_v01`, `_v02`, `<projeto>_final_v01`, etc.
* **Precisão de Frames e Timebase:** Calcule frames rigorosamente:
  * Em 24 fps: `frame = round(segundos * 24)`
  * Em 29.97 fps NTSC: `frame = round(segundos * 30000 / 1001)`
  * Em 30 fps: `frame = round(segundos * 30)`
  * Em 60 fps: `frame = round(segundos * 60000 / 1001)`
* **Organização de Trilhas no DaVinci:**
  * `V1`: Câmera principal (take / apresentador).
  * `V2`: Gravação de tela ou B-Rolls complementares.
  * `V3`: Motion e animações de tela cheia (Remotion ou HyperFrames).
  * `V4`: Overlays, callouts, selos e legendas animadas (com transparência).
  * `A1`: Trilha de voz principal (com clip gain nivelado).
  * `A2`: Efeitos sonoros de transição e impactos (SFX).
  * `A3`: Ambientes e efeitos secundários.
  * `A4`: Trilha de música de fundo (BGM), se aplicável.
* **Micro-fades de áudio:** Sempre aplique crossfades de 1 a 2 frames entre cortes de voz para eliminar estalos e cliques.

---

## 2. Caminho A: DaVinci Resolve via FCPXML (Recomendado)

O DaVinci Resolve possui compatibilidade nativa de alto nível com **FCPXML** (e FCP 7 XML).

### Como importar no DaVinci:
1. Abra o projeto no DaVinci Resolve.
2. Vá em `File -> Import -> Timeline...` (ou `Ctrl+Shift+I` / `Cmd+Shift+I`).
3. Selecione o arquivo `.fcpxml` ou `.xml` gerado pelo Claude Code ou Codex.
4. Desmarque "Automatically set project settings" caso queira garantir a resolução vertical predefinida.

### Detalhes técnicos do XML no DaVinci:
* **Marcadores (Markers):** O DaVinci suporta marcadores de timeline e de clipe com cores (Cyan, Blue, Green, Yellow, Red, Pink, etc.). Use Cyan para anotações do agente e Yellow/Blue para notas manuais da pessoa.
* **Vídeos Verticais (Reels / Shorts):**
  Defina a tag de formato para 1080x1920:
  ```xml
  <format id="r1" frameDuration="1001/30000s" width="1080" height="1920"/>
  ```
* **Áudio Estéreo vs Mono:** No DaVinci, garanta que a track no XML esteja definida com canal estéreo ou o mapeamento correto dos canais embutidos do arquivo de câmera.
* **Câmera e Zooms (Dynamic Zoom / Transform):** Keyframes de escala e posição em FCPXML são interpretados pelo DaVinci na aba *Inspector -> Transform*.

### Ciclo de Retorno (Feedback via DaVinci):
1. A pessoa revisa o corte na timeline do DaVinci.
2. Se fizer ajustes nos cortes, exporta a timeline: `File -> Export -> Timeline...` em formato FCPXML.
3. O agente lê o novo XML, detecta os ajustes feitos pela pessoa e atualiza o estado para as etapas seguintes (motion, câmera e SFX). **Os ajustes humanos sempre têm prioridade absoluta.**

---

## 3. Caminho B: DaVinci Resolve Scripting API (Automação Avançada)

O DaVinci Resolve Studio (e a versão gratuita no macOS/Linux/Windows) expõe uma API Python oficial que permite controlar o editor diretamente caso o programa esteja aberto:

```python
#!/usr/bin/env python3
import sys

# Caminho padrão da API DaVinci no macOS:
# /Library/Application Support/Blackmagic Design/DaVinci Resolve/Developer/Scripting/Modules/
try:
    import DaVinciResolveScript as dvr
except ImportError:
    dvr = None

def get_resolve():
    if dvr:
        return dvr.scriptapp("Resolve")
    return None

def criar_timeline_reels(nome_projeto, nome_timeline):
    resolve = get_resolve()
    if not resolve:
        print("DaVinci Resolve não está aberto com scripting habilitado.")
        return False
    pm = resolve.GetProjectManager()
    project = pm.GetCurrentProject()
    media_pool = project.GetMediaPool()
    
    # Cria timeline vertical
    timeline = media_pool.CreateEmptyTimeline(nome_timeline)
    timeline.SetSetting("useCustomSettings", "1")
    timeline.SetSetting("timelineResolutionWidth", "1080")
    timeline.SetSetting("timelineResolutionHeight", "1920")
    return True
```
> *Nota:* A API Python pode ser usada para montar a timeline sem que a pessoa precise passar pelo diálogo manual de importação de arquivo.

---

## 4. Caminho C: Sem Editor (Render Direto via FFmpeg)

Se a pessoa não quiser usar DaVinci ou outro software:
* O agente compõe todas as camadas via script **FFmpeg**:
  * Take original com cortes e crops aplicados.
  * Overlays de animação (HyperFrames ou Remotion com canal alpha) via filtro `overlay`.
  * Mixagem de voz, SFX e música via filtro `amix` / `adelay`.
* O resultado é exportado como um `.mp4` (H.264 / AAC) pronto para publicação.
* A revisão é feita por minutagem enviada no chat.
