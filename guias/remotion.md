# Remotion: Animações e Legendas em React

O Remotion permite criar animações de vídeo programadas usando React, Tailwind CSS, TypeScript e o ecossistema web moderno. É uma excelente alternativa ao HyperFrames, especialmente para quem já trabalha com React ou precisa de legendas dinâmicas palavra por palavra em Reels.

---

## Remotion vs. HyperFrames

| Critério | Remotion | HyperFrames |
| :--- | :--- | :--- |
| **Linguagem / Stack** | React, TypeScript, Tailwind | HTML, CSS, JavaScript (GSAP) |
| **Legendas Dinâmicas** | Excelente (`@remotion/captions`) | Manual / mais complexo |
| **Componentização** | Componentes React reutilizáveis | Arquivos HTML individuais |
| **Renderizador** | `@remotion/cli` (via Chromium/Node) | `npx hyperframes` (via Puppeteer) |
| **Curva de Aprendizado** | Exige conhecimento em React | Apenas HTML/CSS/JS básico |

---

## 1. Estrutura recomendada para a pasta de motion

Na sua pasta de ferramentas (`~/.edicao/motion/` ou dentro de cada projeto em `_MOTION/`):

```bash
_MOTION/
├── package.json
├── remotion.config.ts
├── src/
│   ├── index.ts               # Registra as composições
│   ├── Root.tsx               # Define as composições (tamanho, fps, duração)
│   ├── compositions/
│   │   ├── LowerThird.tsx     # Tarjas de identificação
│   │   ├── Callout.tsx        # Destaques visuais, setas, ícones
│   │   ├── DynamicCaption.tsx # Legendas animadas tipo karaokê
│   │   └── FullscreenCard.tsx # Cartões e gráficos de tela cheia
│   └── styles/
│       └── global.css
```

---

## 2. Instalação e Setup Rápido

Para iniciar um projeto Remotion limpo na pasta de trabalho:

```bash
npm create video@latest -- --template blank-typescript
npm install @remotion/captions lucide-react clsx tailwindcss
```

Configuração do `remotion.config.ts` para renderização rápida com transparência:
```typescript
import { Config } from '@remotion/cli/config';

Config.setVideoImageFormat('png');
Config.setPixelFormat('yuva444p10le'); // Suporta transparência alfa
Config.setCodec('prores');             // ProRes 4444 para importação direta no DaVinci Resolve
```

---

## 3. Renderizando Animações com Transparência (Overlay)

Para sobrepor no DaVinci Resolve em `V3` ou `V4`:

```bash
# Renderiza com canal alfa (transparência) em ProRes 4444
npx remotion render src/index.ts NomeDaComposicao out/animacao_01.mov \
  --codec=prores \
  --prores-profile=4444 \
  --pixel-format=yuva444p10le
```

*Dica para arquivos menores:* Se o DaVinci estiver rodando no mesmo computador, renderizar em **ProRes 4444 com Alpha** tem performance de reprodução em tempo real na timeline do Resolve.

---

## 4. Legendas Dinâmicas para Reels (`@remotion/captions`)

Para Reels/Shorts, legendas palavra por palavra com efeito de destaque (karaokê) são fundamentais para retenção.

1. O script de transcrição (WhisperX ou similar) gera um JSON com timestamps por palavra:
   ```json
   [
     { "text": "Bem-vindo", "startMs": 0, "endMs": 350 },
     { "text": "ao", "startMs": 360, "endMs": 480 },
     { "text": "meu", "startMs": 490, "endMs": 650 },
     { "text": "canal", "startMs": 660, "endMs": 1100 }
   ]
   ```

2. No componente Remotion:
   ```tsx
   import { useCurrentFrame, useVideoConfig } from 'remotion';

   export const DynamicCaption = ({ words }) => {
     const frame = useCurrentFrame();
     const { fps } = useVideoConfig();
     const currentTimeMs = (frame / fps) * 1000;

     return (
       <div className="flex flex-wrap justify-center text-center font-black uppercase text-4xl px-8">
         {words.map((w, i) => {
           const isActive = currentTimeMs >= w.startMs && currentTimeMs <= w.endMs;
           return (
             <span
               key={i}
               className={`mx-1.5 transition-all ${
                 isActive ? 'text-yellow-400 scale-110 drop-shadow-md' : 'text-white opacity-90'
               }`}
             >
               {w.text}
             </span>
           );
         })}
       </div>
     );
   };
   ```

---

## 5. Boas Práticas do Remotion com DaVinci Resolve

1. **Mesma Taxa de Quadros (FPS):** Se o seu vídeo no DaVinci está em `29.97` ou `30` fps, o Remotion DEVE renderizar exatamente no mesmo fps: `--fps=29.97`.
2. **Safe Zones de Redes Sociais (9:16):**
   - No formato vertical (1080x1920), mantenha animações e textos entre `Y = 350px` e `Y = 1500px`.
   - Evite os 20% inferiores (ocultos pela descrição e áudio do Reels/TikTok) e os 10% superiores (nome de usuário e busca).
3. **Parâmetros Dinâmicos (`--props`):**
   O Claude Code ou Codex pode disparar renders passando variáveis via linha de comando sem alterar o código React:
   ```bash
   npx remotion render src/index.ts LowerThird out/nome.mov \
     --props='{"nome": "Vinicius do Vale", "cargo": "Desenvolvedor"}' \
     --codec=prores --prores-profile=4444
   ```

