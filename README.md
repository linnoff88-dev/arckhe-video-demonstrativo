ï»¿# ê°Å¸Å½Â¬ video-demonstrativo

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


Skill do Claude Code que gera **vêÂ­deos de demonstraêÂ§êÂ£o (walkthrough) de uma aplicaêÂ§êÂ£o web a partir do link do app**.

VocêÂª dêÂ¡ a URL. O Claude Code navega o app **de verdade** com um navegador automatizado, captura as **telas reais** passo a passo e monta um vêÂ­deo narrado em PT-BR com **moldura de navegador, cursor animado, destaque/zoom** e a CTA do INEMA.CLUB. Tudo na mêÂ¡quina ê¢â¬â **sem chave de API**.

- ê°Å¸ââ **Guia de uso:** https://inematds.github.io/video-demonstrativo/guia/
- ê°Å¸Å½â **Curso completo** (3 trilhas, 10 mêÂ³dulos): https://inematds.github.io/skill-video-demonstrativo/

Diferente da skill `video-explicativo` (que explica um conceito com motion graphics): esta **mostra um app real sendo usado**.

## PrincêÂ­pio: capturar antes, animar depois

O render do HyperFrames êÂ© determinêÂ­stico (sem rede durante a renderizaêÂ§êÂ£o), entêÂ£o o site nunca êÂ© carregado ao vivo dentro do vêÂ­deo: capturam-se os screenshots reais antes e anima-se por cima. O **viewport fixo da captura** vira o espaêÂ§o de coordenadas onde o cursor mira ê¢â¬â o alvo êÂ© a **bounding box real** (`getBoundingClientRect`), nêÂ£o o olho.

## Stack

`agent-browser` (Playwright) para captura êÂ· **HyperFrames** (HTMLê¢â âMP4) para o render êÂ· **TTS Kokoro** local (voz `pf_dora`, PT-BR) para a narraêÂ§êÂ£o êÂ· FFmpeg/ffprobe êÂ· Node 22+.

## Fluxo (8 etapas)

1. **Roteiro** ê¢â¬â `STEPS.md`: 5ê¢â¬â8 passos + CTA (ê¢â°Ë35ê¢â¬â50s), 1 frase de narraêÂ§êÂ£o por passo.
2. **RevisêÂ£o de texto** ê¢â¬â cada frase em duas formas: *tela* (PT-BR acentuado, botêÂµes do app na grafia original) e *fala* (nêÂºmeros expandidos, inglêÂªs foneticamente).
3. **Captura** ê¢â¬â `capture.mjs` + `actions.json` ê¢â â `assets/shots/*.png` + `steps.json`.
4. **Projeto** ê¢â¬â `cd ~/projetos/output && npx hyperframes init <nome> --example blank --non-interactive`.
5. **NarraêÂ§êÂ£o** ê¢â¬â `narration-template.sh` ê¢â â `assets/txt/sN.txt` + `assets/audio/sN.wav` (Kokoro).
6. **ComposiêÂ§êÂ£o** ê¢â¬â `composition-template.mjs` como `build-demo.mjs` ê¢â â `index.html` (16:9).
7. **Validar** ê¢â¬â `npx hyperframes lint` êÂ· `npx hyperframes inspect --samples 14`.
8. **Render** ê¢â¬â draft para conferir frames, depois `--quality high --fps 30`.

## Estrutura

```
skills/video-demonstrativo/
  SKILL.md                        # a skill: fluxo, regras de ouro, limites
  CHANGELOG.md                    # histêÂ³rico (v1.yy.xxx)
  references/
    pipeline.md                   # pipeline detalhado, etapa a etapa
    house-style.md                # paleta e tipografia (dark premium êÂ¢mbar)
    cursor-and-chrome.md          # como o cursor, a moldura e o zoom funcionam
    revisao-texto.md              # checklist de acentuaêÂ§êÂ£o + lêÂ©xico inglêÂªsê¢â âPT
    gotchas.md                    # armadilhas do HyperFrames
  scripts/
    capture.mjs                   # dirige o agent-browser; emite shots + steps.json
    composition-template.mjs      # lêÂª steps.json, mede WAVs, gera o index.html
    narration-template.sh         # gera os textos e os WAVs no Kokoro
    fetch-fonts.mjs               # baixa as fontes (alternativa ao assets/fonts/)
    actions.example.json          # exemplo de entrada da captura
    steps.example.json            # exemplo da saêÂ­da da captura
  assets/fonts/                   # Sora, Inter, JetBrains Mono (subset latin) + fonts.css
guia/                             # esta landing + guia de uso (GitHub Pages)
capa/                             # capa oficial do catêÂ¡logo
```

A skill êÂ© **auto-contida**: jêÂ¡ traz as fontes e nêÂ£o depende de nenhum outro projeto.

## Instalar

```bash
cp -r skills/video-demonstrativo ~/.claude/skills/
```

PrêÂ©-requisitos: Node 22+, FFmpeg, `npx hyperframes browser ensure`, `pip install kokoro-onnx soundfile`, `agent-browser` no PATH e o app-alvo no ar.

## Limites conhecidos

- Estado dinêÂ¢mico (animaêÂ§êÂµes, vêÂ­deo, dados ao vivo) vira **print estêÂ¡tico**.
- App com login precisa de **credenciais de teste** (ou capture sêÂ³ telas pêÂºblicas).
- Formato natural êÂ© **16:9** ê¢â¬â telas de app sêÂ£o landscape.
- Voz do Kokoro êÂ© boa, mas sem atuaêÂ§êÂ£o; e o agente nêÂ£o escuta o êÂ¡udio ê¢â¬â o usuêÂ¡rio valida.

---

ConteêÂºdo do [INEMA.CLUB](https://inema.club).

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>