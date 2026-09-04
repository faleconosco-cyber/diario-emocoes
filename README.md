# Diário das Emoções

Recurso terapêutico em página única para o paciente registrar mudanças de humor ao
longo do dia. Cada registro tem cinco passos curtos: carinha, sentimento, o que
acontecia, pensamento e ação.

No ar: **https://diario-emocoes.pages.dev**

## Para que serve na clínica

O paciente registra sempre que o humor muda, não uma vez por dia. Entre as sessões
isso vira material concreto: dá pra ver o que se repete, em que horário, em que
contexto, e qual pensamento aparece junto. Na sessão seguinte ele traz o resumo pelo
botão "Copiar para enviar" ou impresso em PDF.

## Privacidade

Tudo fica no `localStorage` do navegador do próprio paciente, na chave
`rumo-diario-emocoes-v1`. Nada sai do aparelho: não existe servidor, banco nem
requisição de rede. O único jeito de o conteúdo chegar até a psicóloga é o paciente
copiar ou imprimir e enviar por conta própria.

Consequência prática: se ele limpar os dados do navegador, trocar de celular ou abrir
em modo anônimo, os registros somem. Vale avisar isso na entrega do recurso.

## Como está construído

Um arquivo só, `index.html`, com HTML, CSS e JavaScript juntos. Sem framework, sem
build, sem dependência. O único recurso externo são as fontes do Google (Playfair
Display e Montserrat), com fallback pra Georgia e system-ui se não carregarem.

Peças principais do JavaScript, tudo dentro de uma IIFE:

- `FAMILIES`: as sete famílias de emoção (alegria, calma, tristeza, medo, raiva,
  cansaço, surpresa), cada uma com emoji, cor e a lista de palavras do passo 2.
- `seed()`: o registro de exemplo que aparece na primeira abertura, marcado com a
  etiqueta "exemplo", pro paciente entender o que se espera antes de escrever o dele.
- `load()` e `save()`: leem e gravam o array de registros no `localStorage`.
- `render()` e `renderToday()`: desenham o histórico completo e o bloco "Seu dia até
  agora".
- `asText()`: monta o texto que o botão "Copiar para enviar" joga na área de
  transferência. Tem fallback pra `<textarea>` quando a Clipboard API não está
  disponível, que é o caso comum em navegador servido por HTTP sem TLS.

## Identidade visual

Editorial de Consultório, da conta @claudiabotelhopsi. Fundo creme `#FBF2EE`, verde
profundo `#3B503F` como cor de apoio, laranja `#FD745D` só em detalhe. Playfair
Display nos títulos, Montserrat no texto. Tem modo escuro por
`prefers-color-scheme`, com a paleta redefinida inteira.

Cada família de emoção tem cor própria (`--f-alegria`, `--f-calma` e assim por
diante), com valores diferentes em claro e escuro pra manter o contraste.

## Regras de escrita

O texto do app não usa travessão, nem na interface nem no resumo exportado. No texto
que o paciente copia, o separador do cabeçalho do dia é `·` e o marcador de cada
registro é `•`.

## Celular

O `<head>` traz `meta viewport` com `viewport-fit=cover`, e o `<body>` usa
`min-height: 100svh` com padding de `env(safe-area-inset-*)`. Sem o viewport o celular
renderiza numa tela virtual de 980px e encolhe a página inteira, que foi exatamente o
bug corrigido em 04/09/2026.

Os metas `mobile-web-app-capable` e `apple-mobile-web-app-capable` estão lá: se o
paciente usar "Adicionar à tela de início", o app abre sem a barra do navegador.

## Publicação

Cloudflare Pages, projeto `diario-emocoes`, ligado a este repositório no GitHub
(`faleconosco-cyber/diario-emocoes`). Build sem framework, diretório de saída `/`.

Push no `main` dispara o deploy sozinho. Não existe passo manual:

```bash
git add index.html && git commit -m "sua mensagem" && git push origin main
```

Pra conferir depois de subir:

```bash
curl -s https://diario-emocoes.pages.dev/ | head -3
```

## Testar local

O app precisa ser servido por HTTP, não aberto por `file://`, senão a Clipboard API
não funciona e o teste do "Copiar para enviar" engana.

```bash
python -m http.server 5173
```

Vale testar num viewport de 375x812 e conferir que `document.documentElement.scrollWidth`
continua 375, ou seja, sem rolagem lateral.

## Autoria

Cláudia Botelho, psicóloga e orientadora profissional, Instituto Rumo
faleconosco@institutorumo.com · (21) 99062-5330
