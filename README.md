# ðŸŽ¬ video-demonstrativo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


Skill do Claude Code que gera **vÃ­deos de demonstraÃ§Ã£o (walkthrough) de uma aplicaÃ§Ã£o web a partir do link do app**.

VocÃª dÃ¡ a URL. O Claude Code navega o app **de verdade** com um navegador automatizado, captura as **telas reais** passo a passo e monta um vÃ­deo narrado em PT-BR com **moldura de navegador, cursor animado, destaque/zoom** e a CTA do INEMA.CLUB. Tudo na mÃ¡quina â€” **sem chave de API**.

- ðŸ“„ **Guia de uso:** https://inematds.github.io/video-demonstrativo/guia/
- ðŸŽ“ **Curso completo** (3 trilhas, 10 mÃ³dulos): https://inematds.github.io/skill-video-demonstrativo/

Diferente da skill `video-explicativo` (que explica um conceito com motion graphics): esta **mostra um app real sendo usado**.

## PrincÃ­pio: capturar antes, animar depois

O render do HyperFrames Ã© determinÃ­stico (sem rede durante a renderizaÃ§Ã£o), entÃ£o o site nunca Ã© carregado ao vivo dentro do vÃ­deo: capturam-se os screenshots reais antes e anima-se por cima. O **viewport fixo da captura** vira o espaÃ§o de coordenadas onde o cursor mira â€” o alvo Ã© a **bounding box real** (`getBoundingClientRect`), nÃ£o o olho.

## Stack

`agent-browser` (Playwright) para captura Â· **HyperFrames** (HTMLâ†’MP4) para o render Â· **TTS Kokoro** local (voz `pf_dora`, PT-BR) para a narraÃ§Ã£o Â· FFmpeg/ffprobe Â· Node 22+.

## Fluxo (8 etapas)

1. **Roteiro** â€” `STEPS.md`: 5â€“8 passos + CTA (â‰ˆ35â€“50s), 1 frase de narraÃ§Ã£o por passo.
2. **RevisÃ£o de texto** â€” cada frase em duas formas: *tela* (PT-BR acentuado, botÃµes do app na grafia original) e *fala* (nÃºmeros expandidos, inglÃªs foneticamente).
3. **Captura** â€” `capture.mjs` + `actions.json` â†’ `assets/shots/*.png` + `steps.json`.
4. **Projeto** â€” `cd ~/projetos/output && npx hyperframes init <nome> --example blank --non-interactive`.
5. **NarraÃ§Ã£o** â€” `narration-template.sh` â†’ `assets/txt/sN.txt` + `assets/audio/sN.wav` (Kokoro).
6. **ComposiÃ§Ã£o** â€” `composition-template.mjs` como `build-demo.mjs` â†’ `index.html` (16:9).
7. **Validar** â€” `npx hyperframes lint` Â· `npx hyperframes inspect --samples 14`.
8. **Render** â€” draft para conferir frames, depois `--quality high --fps 30`.

## Estrutura

```
skills/video-demonstrativo/
  SKILL.md                        # a skill: fluxo, regras de ouro, limites
  CHANGELOG.md                    # histÃ³rico (v1.yy.xxx)
  references/
    pipeline.md                   # pipeline detalhado, etapa a etapa
    house-style.md                # paleta e tipografia (dark premium Ã¢mbar)
    cursor-and-chrome.md          # como o cursor, a moldura e o zoom funcionam
    revisao-texto.md              # checklist de acentuaÃ§Ã£o + lÃ©xico inglÃªsâ†’PT
    gotchas.md                    # armadilhas do HyperFrames
  scripts/
    capture.mjs                   # dirige o agent-browser; emite shots + steps.json
    composition-template.mjs      # lÃª steps.json, mede WAVs, gera o index.html
    narration-template.sh         # gera os textos e os WAVs no Kokoro
    fetch-fonts.mjs               # baixa as fontes (alternativa ao assets/fonts/)
    actions.example.json          # exemplo de entrada da captura
    steps.example.json            # exemplo da saÃ­da da captura
  assets/fonts/                   # Sora, Inter, JetBrains Mono (subset latin) + fonts.css
guia/                             # esta landing + guia de uso (GitHub Pages)
capa/                             # capa oficial do catÃ¡logo
```

A skill Ã© **auto-contida**: jÃ¡ traz as fontes e nÃ£o depende de nenhum outro projeto.

## Instalar

```bash
cp -r skills/video-demonstrativo ~/.claude/skills/
```

PrÃ©-requisitos: Node 22+, FFmpeg, `npx hyperframes browser ensure`, `pip install kokoro-onnx soundfile`, `agent-browser` no PATH e o app-alvo no ar.

## Limites conhecidos

- Estado dinÃ¢mico (animaÃ§Ãµes, vÃ­deo, dados ao vivo) vira **print estÃ¡tico**.
- App com login precisa de **credenciais de teste** (ou capture sÃ³ telas pÃºblicas).
- Formato natural Ã© **16:9** â€” telas de app sÃ£o landscape.
- Voz do Kokoro Ã© boa, mas sem atuaÃ§Ã£o; e o agente nÃ£o escuta o Ã¡udio â€” o usuÃ¡rio valida.

---

ConteÃºdo do [INEMA.CLUB](https://inema.club).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>