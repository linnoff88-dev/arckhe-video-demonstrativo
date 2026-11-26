# Changelog — video-demonstrativo

Versionamento: **`v1.yy.xxx`** — `yy` = recurso (feature), `xxx` = correçêo (bug).

## 1.1.0 — Revisêo de texto + pronúncia de inglês
Recurso: nova etapa **antes** de captura e narraçêo, fechando um buraco (o texto ia direto pra tela
e pro TTS sem revisêo de acentuaçêo/ortografia).

- **Passo 2 "Revisêo de texto"** inserido no fluxo (passos renumerados; 7→8). Revisa acentuaçêo PT-BR
  palavra a palavra e a ortografia de todo texto de tela (`caption` no `steps.json` + labels) e da
  forma-fala (`txt/sN.txt`) **antes** dos WAVs e dos screenshots.
- **Contrato de duas formas por frase**: **tela** (PT-BR acentuado + inglês/botões do app na grafia
  original, ex.: `Generate`, `Upload`) vs **fala** (números/siglas expandidos + inglês reescrito
  foneticamente, ex.: `upload`→"âploud", `deploy`→"deplói").
- **Pronúncia de termos em inglês**: como nomes de botêo/menu de app costumam vir em inglês e o Kokoro
  fonemiza pela grafia escrita, a forma-fala troca por grafia fonética. Na dúvida, gerar WAV de teste e
  o usuário ouvir.
- Nova referência [`references/revisao-texto.md`](references/revisao-texto.md) (checklist + léxico
  inglês→PT + como testar). Regra de ouro nova no `SKILL.md`; `pipeline.md` atualizado. Zip reempacotado.

## 1.0.0 — Release inicial
- Walkthrough narrado de aplicaçêo web: captura real com `agent-browser` (Playwright) + HyperFrames
  (HTML→MP4) + TTS local Kokoro (voz `pf_dora`), tudo na máquina, **sem chave de API**.
- Moldura de navegador, **cursor global animado** mirando a **bounding box real** (`getBoundingClientRect`),
  destaque/zoom no resultado e **CTA do ARCKHÉ AI**.
- Princípio "capturar antes, animar depois" (render determinístico); viewport fixo = espaço de coordenadas.
- Pacote `.zip` da skill + curso ARCKHÉ AI (landing + 3 trilhas / 10 módulos, GitHub Pages).
- Ajuste de saída: **sem cauda muda** (`ambientRepeat`) e **saída única** em `~/projetos/output/<nome>/`.
