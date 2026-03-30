# KURO Studio — Landing Page PRD v2.0

> **Status:** Pronto para implementação
> **Stack:** HTML + CSS + JS vanilla (single file, zero dependências além de Google Fonts)
> **Objetivo:** Peça #1 do portfólio de landing pages frontend
> **Skill de referência:** `/mnt/skills/user/landing-page-design/SKILL.md` — ler ANTES de começar
> **Referência de design tokens:** `/mnt/skills/user/landing-page-design/references/LANDING_PAGE_PRD.md`

---

## 0. Contexto para o Claude Code

Este documento é o blueprint completo para implementar a landing page do KURO Studio. Leia integralmente antes de escrever qualquer linha de código. Cada seção contém decisões de design já tomadas — não improvise fora dos parâmetros definidos aqui.

**O que é o KURO:** Um estúdio fictício de design e motion digital. A landing page funciona como portfólio — cada seção DEMONSTRA na prática o que o estúdio diz fazer. Animação aqui não é decoração, é conteúdo.

**Entrega:** Um único arquivo HTML contendo todo o CSS em `<style>` e todo o JS em `<script>`. Sem frameworks, sem Tailwind, sem React. Google Fonts via `<link>`. O arquivo deve abrir no browser e funcionar imediatamente.

---

## 1. Conceito & Tom de Voz

**Nome:** KURO (黒, "preto" em japonês)
**Tagline primária:** "We craft digital experiences that move."
**Tagline curta (nav/footer):** "Motion is identity."

**Logotipo:** Wordmark puro, sempre uppercase — **KURO·** (com dot accent). `letter-spacing: 0.12em`, `font-weight: 600`. Sem símbolo separado.

**Tom:** Confiante, contido, preciso. Frases curtas e declarativas. Sem exclamações, sem hype. A confiança vem da economia de palavras.

### Copy exata por seção

| Seção | Copy |
|---|---|
| Hero H1 | "We don't decorate. We choreograph." |
| Manifesto | "Every transition tells a story. Every pause is intentional. We believe motion isn't ornament — it's the language your brand speaks when no one is reading." |
| Showcase label | "Selected work." |
| Processo steps | "Brief → Direction → Choreography → Code" |
| CTA H2 | "Start a conversation." |
| CTA sub | "New projects begin with a single message." |
| CTA botão | "hello@kuro.studio" (link mailto) |
| Footer | "KURO· Tokyo / São Paulo / Remote" |

---

## 2. Design System — Tokens CSS

Definir TUDO em `:root` antes de qualquer HTML. Estes são os únicos valores permitidos na página.

```css
:root {
  /* === BACKGROUNDS === */
  --kuro-void: #0A0A0B;      /* fundo primário — preto profundo com micro-tom frio */
  --kuro-surface: #141416;    /* superfície elevada (cards, seções alternadas) */
  --kuro-subtle: #1E1E22;     /* bordas, dividers, hover states */

  /* === TEXTO === */
  --kuro-text: #E8E6E3;       /* off-white quente — texto principal */
  --kuro-muted: #6B6B73;      /* texto secundário, labels, metadata */

  /* === ACCENT === */
  --kuro-accent: #C4C0B6;     /* areia/champagne metálico — pontuação visual cirúrgica */
  --kuro-accent-glow: rgba(196, 192, 182, 0.08); /* glow sutil do accent */

  /* === BORDERS === */
  --kuro-border: rgba(255, 255, 255, 0.06);
  --kuro-border-hover: rgba(255, 255, 255, 0.12);

  /* === TIPOGRAFIA === */
  --font-primary: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;

  /* Display (hero) — TRACKING ABERTO, não negativo. Isso é identidade KURO. */
  --text-display: clamp(48px, 6.5vw, 88px);
  --ls-display: 0.06em;
  --lh-display: 1.08;
  --fw-display: 700;

  /* H2 (seções) — tracking TIGHT, contraste com o display */
  --text-h2: clamp(24px, 3.2vw, 44px);
  --ls-h2: -0.02em;
  --lh-h2: 1.15;
  --fw-h2: 600;

  /* H3 (subtítulos) */
  --text-h3: clamp(15px, 1.8vw, 20px);
  --ls-h3: 0em;
  --lh-h3: 1.3;
  --fw-h3: 500;

  /* Body */
  --text-body: 16px;
  --ls-body: -0.011em;
  --lh-body: 1.65;
  --fw-body: 400;

  /* Small/Label */
  --text-small: 13px;
  --ls-small: 0.04em;
  --lh-small: 1.4;
  --fw-small: 400;

  /* === ESPAÇAMENTO (grid 4pt) === */
  --sp-1: 4px;  --sp-2: 8px;  --sp-3: 12px;  --sp-4: 16px;
  --sp-6: 24px; --sp-8: 32px; --sp-10: 40px; --sp-12: 48px;
  --sp-16: 64px; --sp-20: 80px; --sp-24: 96px; --sp-32: 128px;
  --section-y: clamp(80px, 10vw, 140px);

  /* === LAYOUT === */
  --container-max: 1200px;
  --container-narrow: 800px;
  --container-px: clamp(20px, 5vw, 40px);

  /* === BORDER-RADIUS === */
  --radius-sm: 4px;
  --radius-md: 8px;
  --radius-lg: 12px;

  /* === EASING — o easing canônico do KURO === */
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);

  /* === DURATIONS === */
  --duration-fast: 400ms;
  --duration-normal: 700ms;
  --duration-slow: 1000ms;
  --duration-reveal: 900ms;
}
```

### Regras tipográficas obrigatórias

1. **Display (hero):** tracking ABERTO (`0.06em`). Isso é identidade KURO — diferente de SaaS convencionais que usam tracking negativo. O espaço entre letras reforça a respiração e premium-ness.
2. **H2 e body:** tracking tight/negativo — padrão Linear/Framer. O contraste entre display aberto e body tight é proposital.
3. **`-webkit-font-smoothing: antialiased`** obrigatório no body.
4. **`font-feature-settings: 'cv02','cv03','cv04','cv11'`** obrigatório no body.
5. **Nunca usar `font-family: Arial`**. Nunca usar `color: #000000`. Nunca usar `background: white`.

### Regra de uso do accent

O `--kuro-accent` (#C4C0B6) NUNCA preenche áreas grandes. Existe como pontuação cirúrgica:
- O dot em `KURO·` (circle de 4-6px)
- Underline de hover em links (height: 1px)
- Bordas de hover em cards (1px)
- Label "eyebrow" acima de títulos de seção
- Cursor custom (se implementado)

Se o accent chama mais atenção que o conteúdo, tem accent demais. No máximo 5 pontos na página inteira.

---

## 3. Linguagem de Movimento — Os 4 Princípios

Toda animação na página DEVE seguir um destes princípios. Se não encaixar em nenhum, a animação não deve existir.

### P1 — "Earned entrance"
Nada aparece instantaneamente. Todo elemento ganha seu espaço com:
- `opacity: 0 → 1`
- `translateY(12px) → translateY(0)` (translate CURTO, 8-20px, nunca 40+px)
- `duration: var(--duration-reveal)` (600-900ms)
- `easing: var(--ease-out-expo)`
- Stagger entre elementos irmãos: 80-120ms de delay entre cada um

```css
/* Pattern de referência para o Code */
@keyframes kuro-reveal {
  from {
    opacity: 0;
    transform: translateY(12px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.reveal {
  opacity: 0;
  animation: kuro-reveal var(--duration-reveal) var(--ease-out-expo) forwards;
}
.reveal-d1 { animation-delay: 80ms; }
.reveal-d2 { animation-delay: 160ms; }
.reveal-d3 { animation-delay: 240ms; }
.reveal-d4 { animation-delay: 320ms; }
```

### P2 — "Scroll as narrative"
O scroll é o input primário. Implementar via IntersectionObserver:
- Elementos abaixo do fold começam com `opacity: 0; transform: translateY(12px)`
- Quando entram no viewport (threshold: 0.15), recebem classe `.is-visible`
- A transição usa o mesmo easing e duration do P1
- NO manifesto: texto revela palavra por palavra (ou linha por linha) via scroll progress

```js
/* Pattern de referência para o Code — IntersectionObserver */
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('is-visible');
      observer.unobserve(entry.target); // anima só 1 vez
    }
  });
}, { threshold: 0.15 });

document.querySelectorAll('.scroll-reveal').forEach(el => observer.observe(el));
```

```css
.scroll-reveal {
  opacity: 0;
  transform: translateY(12px);
  transition: opacity var(--duration-reveal) var(--ease-out-expo),
              transform var(--duration-reveal) var(--ease-out-expo);
}
.scroll-reveal.is-visible {
  opacity: 1;
  transform: translateY(0);
}
```

### P3 — "Hover as dialogue"
Todo elemento interativo responde ao cursor. Não é `scale(1.05)` genérico:
- Links: underline reveal de 1px em accent, via `::after` com `scaleX(0) → scaleX(1)`
- Showcase cards: shift sutil de 2px no `translateY`, borda muda de `--kuro-border` para `--kuro-border-hover`, reveal de texto metadata
- Botão CTA: glow sutil via `box-shadow` expandindo

```css
/* Pattern hover para links */
.link-hover {
  position: relative;
  color: var(--kuro-text);
  text-decoration: none;
}
.link-hover::after {
  content: '';
  position: absolute;
  bottom: -2px;
  left: 0;
  width: 100%;
  height: 1px;
  background: var(--kuro-accent);
  transform: scaleX(0);
  transform-origin: right;
  transition: transform var(--duration-fast) var(--ease-out-expo);
}
.link-hover:hover::after {
  transform: scaleX(1);
  transform-origin: left;
}
```

### P4 — "Stillness is design"
Nem tudo se move. Zero idle animations (nada piscando, nada girando em loop). Espaço vazio é proposital. Se uma seção tem pouco conteúdo e muito padding, está correto.

---

## 4. Estrutura da Página — Seção por Seção

### 4.0 Nav — Sticky, transparent, minimal

**Layout:** Logo à esquerda, CTA à direita. Sem links de navegação (é single-page).

```
[KURO·]                                    [hello@kuro.studio]
```

**Comportamento:**
- `position: fixed; top: 0; width: 100%; z-index: 100`
- Background: transparente inicialmente
- Ao scrollar 80px+: `background: rgba(10, 10, 11, 0.85); backdrop-filter: blur(12px); border-bottom: 1px solid var(--kuro-border)`
- Transição suave: `transition: background 400ms var(--ease-out-quart), border-color 400ms var(--ease-out-quart)`
- Height: 64px
- Container com `max-width: var(--container-max)`, centrado

**Logo KURO·:**
- `font-size: 15px; font-weight: 600; letter-spacing: 0.12em; text-transform: uppercase; color: var(--kuro-text)`
- O `·` (dot) usa `color: var(--kuro-accent)`

**CTA nav:**
- `font-size: var(--text-small); letter-spacing: var(--ls-small); color: var(--kuro-muted)`
- Hover: `color: var(--kuro-text)` + underline accent via `::after`

### 4.1 Hero — "Declaração"

**Objetivo:** Impactar nos primeiros 2-3 segundos. O movimento É o hero.

**Layout:** Centrado, verticalmente. Muito espaço negativo acima e abaixo.

```
                    [espaço generoso]

               We don't decorate.
               We choreograph.

                    [espaço]

             "Motion is identity."

                    [espaço generoso]
```

**Especificações:**
- `min-height: 100svh; display: flex; align-items: center; justify-content: center`
- `padding: var(--section-y) var(--container-px)`
- Background: `var(--kuro-void)` puro, sem glow, sem gradiente. A tipografia é o visual.

**H1:**
- Copy: "We don't decorate. We choreograph."
- Duas linhas, cada uma centrada
- `font-size: var(--text-display); font-weight: var(--fw-display); letter-spacing: var(--ls-display); line-height: var(--lh-display); color: var(--kuro-text); text-align: center`

**Animação do H1 — Reveal por palavra:**
Cada palavra do H1 revela individualmente com stagger. Implementar com `<span>` por palavra:

```html
<h1 class="hero-headline">
  <span class="word reveal reveal-d0">We</span>
  <span class="word reveal reveal-d1">don't</span>
  <span class="word reveal reveal-d2">decorate.</span>
  <br>
  <span class="word reveal reveal-d3">We</span>
  <span class="word reveal reveal-d4">choreograph.</span>
</h1>
```

Stagger: 100ms entre palavras. A animação começa 300ms após o page load.

**Tagline abaixo do H1:**
- Copy: "Motion is identity."
- `font-size: var(--text-small); letter-spacing: var(--ls-small); color: var(--kuro-muted); text-transform: uppercase`
- Reveal com delay de 700ms (depois que o H1 termina)
- Margem acima: `var(--sp-12)` (48px)

**Elemento gráfico sutil (opcional mas recomendado):**
Uma única linha horizontal fina (1px) em accent que se expande do centro para as bordas, posicionada entre o H1 e a tagline. Width máximo: 120px. Animação: `scaleX(0) → scaleX(1)` com delay de 600ms.

### 4.2 Manifesto — "Filosofia"

**Objetivo:** Demonstrar scroll-driven text reveal. A seção mais "motion-forward" da página.

**Layout:** Texto centrado, largura restrita. Uma frase curta.

```
Every transition tells a story.
Every pause is intentional.
We believe motion isn't ornament —
it's the language your brand speaks
when no one is reading.
```

**Especificações:**
- `padding: var(--section-y) var(--container-px)`
- `max-width: var(--container-narrow)` (800px), centrado
- Background: `var(--kuro-void)` (mesmo do hero, fluxo contínuo)

**Texto:**
- `font-size: var(--text-h2); font-weight: var(--fw-h2); letter-spacing: var(--ls-h2); line-height: var(--lh-h2); color: var(--kuro-text); text-align: center`

**Animação — Scroll-triggered line reveal:**
Cada linha do manifesto começa com `opacity: 0.15; color: var(--kuro-muted)` e transiciona para `opacity: 1; color: var(--kuro-text)` conforme o scroll avança.

Implementar com IntersectionObserver por `<span class="line">` — cada linha é um span display block. Threshold escalonado: à medida que o usuário scrolla, as linhas vão "acendendo" sequencialmente.

```html
<div class="manifesto">
  <p class="manifesto-text">
    <span class="line scroll-line">Every transition tells a story.</span>
    <span class="line scroll-line">Every pause is intentional.</span>
    <span class="line scroll-line">We believe motion isn't ornament —</span>
    <span class="line scroll-line">it's the language your brand speaks</span>
    <span class="line scroll-line">when no one is reading.</span>
  </p>
</div>
```

```css
.scroll-line {
  display: block;
  opacity: 0.15;
  transition: opacity 600ms var(--ease-out-expo);
}
.scroll-line.is-lit {
  opacity: 1;
}
```

Usar IntersectionObserver com `threshold: [0.0, 0.2, 0.4, 0.6, 0.8, 1.0]` e `rootMargin: '-20% 0px -20% 0px'` para criar o efeito progressivo. A implementação exata deve garantir que scrollar pela seção "acende" as linhas de cima para baixo.

### 4.3 Showcase — "Trabalhos"

**Objetivo:** Grid de projetos fictícios que ganham vida no hover. Micro-interações são o conteúdo.

**Layout:** Grid 2 colunas no desktop, 1 coluna no mobile.

**Dados dos projetos fictícios:**

| # | Nome | Tipo | Ano |
|---|------|------|-----|
| 1 | AURA | Brand Identity & Motion System | 2024 |
| 2 | VANTA | Product Launch Campaign | 2024 |
| 3 | SOLIS | E-commerce Experience | 2023 |
| 4 | NØRD | Digital Editorial | 2023 |

**Especificações:**
- Section label acima: "Selected work." — `font-size: var(--text-small); letter-spacing: var(--ls-small); color: var(--kuro-muted); text-transform: uppercase; margin-bottom: var(--sp-12)`
- Grid: `display: grid; grid-template-columns: repeat(2, 1fr); gap: var(--sp-4)` (gap pequeno, 16px — referência Gladia bento)
- `max-width: var(--container-max)`, centrado

**Cada card:**
- `aspect-ratio: 16/10`
- `background: var(--kuro-surface)`
- `border: 1px solid var(--kuro-border)`
- `border-radius: var(--radius-lg)` (12px)
- `overflow: hidden; position: relative; cursor: pointer`
- Padding interno: `var(--sp-8)` (32px)

**Conteúdo do card:**
- Nome do projeto: `font-size: var(--text-h3); font-weight: var(--fw-h3); color: var(--kuro-text); letter-spacing: var(--ls-h3)`
- Tipo + ano: `font-size: var(--text-small); color: var(--kuro-muted); letter-spacing: var(--ls-small)`
- Posicionados no canto inferior esquerdo do card

**Hover do card (P3 — "Hover as dialogue"):**
- Background: sutil shift para `var(--kuro-subtle)`
- Border: `var(--kuro-border-hover)`
- O card inteiro faz `translateY(-2px)` — micro, não exagerado
- Um indicador de seta ou "→" aparece com `opacity: 0 → 1` no canto inferior direito
- Transição: `transition: transform var(--duration-fast) var(--ease-out-expo), background var(--duration-fast) var(--ease-out-expo), border-color var(--duration-fast) var(--ease-out-expo)`

**Scroll animation:** Cada card usa a classe `.scroll-reveal` com stagger de 80ms entre cards.

### 4.4 Processo — "Como"

**Objetivo:** Mostrar o processo criativo em 4 passos com reveal progressivo.

**Layout:** Horizontal no desktop (4 colunas), vertical no mobile.

**Steps:** Brief → Direction → Choreography → Code

**Especificações:**
- Section label: nenhum label — o contexto é autoevidente
- `display: grid; grid-template-columns: repeat(4, 1fr); gap: var(--sp-6)`
- Padding: `var(--section-y) var(--container-px)`
- `max-width: var(--container-max)`, centrado
- Separador visual entre steps: uma linha horizontal de 1px em `var(--kuro-border)` que conecta os 4 (estilo timeline)

**Cada step:**
- Número: "01", "02", "03", "04" — `font-size: var(--text-small); color: var(--kuro-accent); letter-spacing: var(--ls-small); font-variant-numeric: tabular-nums`
- Título: "Brief", "Direction", "Choreography", "Code" — `font-size: var(--text-h3); font-weight: var(--fw-h3); color: var(--kuro-text); margin-top: var(--sp-3)`

**Animação:** Cada step revela via scroll com stagger de 120ms. A linha timeline "cresce" da esquerda para a direita conforme os steps aparecem — implementar com `scaleX(0) → scaleX(1)` e `transform-origin: left`.

### 4.5 Closing/CTA — "Contato"

**Objetivo:** Minimalista. Uma frase + email. O último detalhe que faz o visitante pensar "isso foi bem feito."

**Layout:** Centrado, muito espaço negativo.

```
              Start a conversation.

        New projects begin with a single message.

              [hello@kuro.studio]
```

**Especificações:**
- `padding: var(--sp-32) var(--container-px)` (128px top/bottom — generoso)
- `max-width: var(--container-narrow)`, centrado
- Background: `var(--kuro-void)` contínuo

**H2:** "Start a conversation."
- `font-size: var(--text-h2); font-weight: var(--fw-h2); letter-spacing: var(--ls-h2); color: var(--kuro-text); text-align: center`

**Sub:** "New projects begin with a single message."
- `font-size: var(--text-body); color: var(--kuro-muted); text-align: center; margin-top: var(--sp-4)`

**Email link (CTA principal):**
- `font-size: var(--text-body); color: var(--kuro-accent); text-decoration: none; letter-spacing: var(--ls-body)`
- `margin-top: var(--sp-10)` (40px)
- Comportamento hover: efeito magnético OU underline accent reveal (escolher um)
- `display: inline-block` para centralização

**Efeito magnético (implementação sugerida):**
O link reage suavemente à posição do cursor quando o mouse está próximo (raio de 100px). O texto "puxa" levemente na direção do cursor. Implementar com JS:

```js
/* Pattern de referência — efeito magnético no CTA */
const magneticEl = document.querySelector('.magnetic-cta');
magneticEl.addEventListener('mousemove', (e) => {
  const rect = magneticEl.getBoundingClientRect();
  const x = e.clientX - rect.left - rect.width / 2;
  const y = e.clientY - rect.top - rect.height / 2;
  magneticEl.style.transform = `translate(${x * 0.15}px, ${y * 0.15}px)`;
});
magneticEl.addEventListener('mouseleave', () => {
  magneticEl.style.transform = 'translate(0, 0)';
  magneticEl.style.transition = `transform 500ms var(--ease-out-expo)`;
});
magneticEl.addEventListener('mouseenter', () => {
  magneticEl.style.transition = 'none';
});
```

### 4.6 Footer

**Layout:** Simples, uma linha.

```
KURO·                                   Tokyo / São Paulo / Remote

─────────────────────────────────────────────────────────────────
                    © 2026 KURO Studio
```

**Especificações:**
- `border-top: 1px solid var(--kuro-border)`
- `padding: var(--sp-8) var(--container-px)`
- `max-width: var(--container-max)`, centrado
- Logo à esquerda, locais à direita (flexbox, `justify-content: space-between`)
- Copyright centrado abaixo, separado por `var(--sp-6)`
- Tudo em `font-size: var(--text-small); color: var(--kuro-muted)`
- Logo mantém `color: var(--kuro-text)` com dot em accent

---

## 5. Responsividade

**Breakpoint principal:** 768px

### Mobile (≤ 768px)

- Hero H1: `font-size: clamp(36px, 9vw, 48px)` — mantém tracking aberto
- Manifesto text: `font-size: clamp(20px, 5vw, 28px)`
- Showcase grid: `grid-template-columns: 1fr` (uma coluna)
- Processo grid: `grid-template-columns: 1fr` (vertical, timeline vira vertical)
- Nav CTA: pode ficar oculto (link no footer é suficiente)
- Touch targets: todos ≥ 44px
- `padding-section` reduz: `clamp(64px, 8vw, 80px)`

### `prefers-reduced-motion`

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
  .scroll-reveal, .scroll-line, .reveal {
    opacity: 1 !important;
    transform: none !important;
  }
}
```

---

## 6. Padrões de Referência Extraídos

### Da Gladia (Referência: bento layout + eyebrow labels)
- **Eyebrow pattern:** pill SVG + texto uppercase + linha horizontal. Adaptação KURO: usar apenas texto uppercase em `--kuro-accent` sem pill, seguido de linha fina. Mais contido.
- **Bento grid gaps:** 16px gap entre cards — tight, premium. Adotar no showcase.
- **Testimonial carousel horizontal:** NÃO usar no KURO. O estúdio é fictício, sem testimonials.

### Do Raycast (Referência: dark premium + hierarchy by luminosity)
- **Hierarquia por luminosidade:** Raycast usa 4+ níveis de cinza para separar conteúdo. KURO adota o mesmo princípio com seus 3 tons (`void`, `surface`, `subtle`).
- **Keyboard section (animação como conteúdo):** O teclado animado da Raycast é equivalente ao hero do KURO — a animação demonstra o produto. No KURO, a tipografia animada do hero faz esse papel.
- **Glassmorphism:** Raycast usa `backdrop-filter: blur()` em cards. KURO usa de forma mais contida — apenas no nav após scroll, não em cards (os cards usam superfície sólida com borda sutil).

### Padrões PROIBIDOS nesta landing
- ❌ Logo carousel / marquee (Gladia tem, KURO não — é estúdio fictício)
- ❌ Announcement pill acima do hero (muito SaaS, quebra o minimalismo)
- ❌ Gradiente no texto do H1 (KURO usa cor sólida off-white)
- ❌ Glow radial no hero (a tipografia pura é o visual)
- ❌ Parallax agressivo (conflita com "stillness is design")
- ❌ Qualquer `transition: all`
- ❌ Qualquer `border-radius: 50px` ou `9999px` (sem pills, exceto no nav dot)
- ❌ Espaçamento que não seja múltiplo de 4
- ❌ Idle animations (nada girando, pulsando ou piscando em loop)

---

## 7. Checklist de Qualidade

Antes de considerar pronto, verificar cada item:

### Identidade
- [ ] Paleta usa EXCLUSIVAMENTE os tokens `--kuro-*` definidos (sem hex avulsos)
- [ ] Accent (`--kuro-accent`) aparece em no máximo 5 pontos da página
- [ ] Tracking ABERTO nos displays, TIGHT no body — contraste proposital
- [ ] Tom de voz: declarativo, sem exclamações, sem hype, sem adjetivos inflados
- [ ] Espaço negativo generoso — se parecer "vazio", provavelmente tá certo

### Movimento
- [ ] Toda animação segue um dos 4 princípios (P1-P4)
- [ ] Nenhuma idle animation sem propósito
- [ ] Easing consistente: `var(--ease-out-expo)` em todas as transições
- [ ] Stagger entre elementos irmãos: 80-120ms
- [ ] IntersectionObserver para todas as animações abaixo do fold
- [ ] `prefers-reduced-motion` implementado e remove todas as animações

### Código
- [ ] `:root` com todos os tokens definidos
- [ ] `clamp()` em todos os font-sizes de headline
- [ ] Zero `transition: all` no código
- [ ] Zero `font-family: Arial`
- [ ] Zero `color: #000` ou `background: #fff`
- [ ] Todos valores de spacing são múltiplos de 4
- [ ] `-webkit-font-smoothing: antialiased` no body
- [ ] `font-feature-settings: 'cv02','cv03','cv04','cv11'` no body
- [ ] Sem JS frameworks — vanilla only

### Responsividade
- [ ] Hero funciona em 375px (iPhone SE)
- [ ] Touch targets ≥ 44px
- [ ] Grids colapsam em 1 coluna no mobile
- [ ] Texto legível sem zoom (mín 14px mobile)

### Performance
- [ ] Google Fonts com `preconnect` e `display=swap`
- [ ] Sem animações em `width`, `height`, `top`, `left` — usar `transform` e `opacity`
- [ ] Arquivo único, zero dependências externas além de Google Fonts
- [ ] Sem imagens (tudo é tipografia + cor + forma CSS)

---

## 8. Decisões Técnicas Finais

- **Sem imagens:** A landing inteira é construída com tipografia, cor e formas CSS. Os showcase cards não têm thumbnails — são superfícies coloridas com texto.
- **Sem hamburger menu:** O nav é tão mínimo que não precisa de mobile toggle.
- **Sem JavaScript frameworks:** Vanilla JS para IntersectionObserver, nav scroll behavior, e efeito magnético.
- **Sem scroll-hijacking:** O scroll nativo do browser é respeitado. As animações reagem ao scroll, mas não o controlam.
- **Sem noise texture:** O void puro (#0A0A0B) é suficiente. Noise adicionaria ruído visual ao conceito minimalista.

---

*Documento pronto para implementação. O Claude Code deve ler a skill `landing-page-design` e seu PRD de referência antes de começar, mas as decisões deste documento têm prioridade em caso de conflito (ex: tracking aberto no display vs negativo na skill genérica).*
