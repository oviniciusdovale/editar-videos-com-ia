# Modelo: perfil

O perfil registra a identidade estética e o estilo de edição da pessoa, separado das regras técnicas do fluxo. Fica armazenado em `<pasta de ferramentas>/perfis/<nome>/`.
Se você editar para canais diferentes, formatos diferentes (Reels vs. YouTube) ou clientes distintos, basta criar perfis separados sem duplicar as skills.

---

## Arquivos do Perfil

| Arquivo | O que define | Quem consulta |
|---|---|---|
| `formato.md` | Proporção (9:16 vertical ou 16:9 horizontal), resolução (ex: 1080x1920), FPS real e safe zones de interface (TikTok/Instagram) | Todas as etapas |
| `identidade.md` | Cores da marca (HEX/RGB), tipografia, estilo de legendas dinâmicas, elementos visuais permitidos e proibidos | motion-mapa, motion-render |
| `estilo-motion.md` | Ferramenta preferida (**Remotion** ou **HyperFrames**), curvas de aceleração (ease-out, bezier), presença de overshoot, densidade de elementos | motion-mapa, motion-render |
| `estilo-camera.md` | Escala dos zooms (ex: 115% nos jump-cuts), punch-in em momentos de ênfase, reenquadramento no rosto | motion-mapa, câmera |
| `estilo-cortes.md` | Pacing: tolerância a silêncio (ex: corta pausas > 0.3s em Reels), folga de ar nas bordas de fala, tratamento de hesitações | cortes |
| `estilo-som.md` | Densidade de efeitos sonoros por minuto, volume de SFX relativo à voz (-18dB a -24dB), sons favoritos | sfx |
| `catalogo.md` | Biblioteca de componentes visuais já prontos (cards, callouts, lower-thirds em Remotion ou HTML) | motion-mapa |

---

## Exemplo: `formato.md` (Para Reels e Shorts)

```markdown
# Configurações de Formato - Perfil Reels Dinâmico

- **Proporção:** 9:16 Vertical
- **Resolução:** 1080 x 1920 pixels
- **Taxa de Quadros:** 29.97 fps (NTSC)
- **Editor Principal:** DaVinci Resolve
- **Safe Zones:**
  - Margem superior: manter elementos principais abaixo de Y = 350px (livre da barra de pesquisa e topo da rede).
  - Margem inferior: manter legendas e gráficos acima de Y = 1500px (livre da legenda nativa, áudio e descrição).
  - Margem lateral direita: 120px de respiro (livre dos botões de like, comentário e compartilhamento).
```

---

## Exemplo: `identidade.md` (Com Legendas Dinâmicas)

```markdown
# Identidade Visual e Legendas

- **Cores Principais:** Amarelo `#FFDF00`, Branco `#FFFFFF`, Fundo Escuro `#121214`
- **Fonte Padrão:** Montserrat Black ou Inter Extra Bold
- **Legendas Dinâmicas (Karaokê):**
  - Motor: Remotion (`@remotion/captions`)
  - Caixa alta (Uppercase), 3 a 5 palavras por linha na tela
  - Palavra ativa: destaque em `#FFDF00` com escala de 110%
  - Posição: Centralizado verticalmente em Y = 1200px (dentro da safe zone)
```
