# SECTIONS.md — Détail section par section

> Référence rapide pour chaque section : id, classe, hauteur, contenu, animations actives. Voir `LANDING.md` pour le contexte global.

---

## 1. Hero — `<section class="hero">`

- **Hauteur** : `min-height: 100vh; padding: 140px 24px 80px`
- **Position** : relative
- **Contenu** :
  - `.hero-arch` : grand logo arche
  - `.hero-title` (h1) : 2 lignes
    - `#hero-line1` : « Authentique. » GrosVentre + `#19d382`
    - `#hero-line2` : sous-titre (« Ton Instagram, tes choix. »)
  - `.hero-platforms-wrap` : « Bientôt disponible sur : » + `.platform-pill` × 2 (iOS + Android)
  - `.hero-actions` : `.btn-primary` Kickstarter
  - `.hero-tagline` : phrase d'accroche
  - `.hero-scroll` : flèche bounce, fade out > 100 px scroll
- **Animations** : `fitHeroTitle()` ajuste line2 ≤ line1, `IntersectionObserver` pour `.reveal`, hero-scroll fade au scroll.

---

## 2. Le problème — `<section class="section-problem" id="section-problem">`

- **Hauteur** : `height: 100vh; padding: 288px 24px 100px; overflow: hidden`
- **Position** : relative
- **Layout** : flex-column ; `.problem-grid` à l'intérieur en grid 2 colonnes (texte gauche / phone mockup droite)
- **Contenu** :
  - `.problem-text` (col gauche) : `.section-label` « Le problème » + h2 « Instagram décide… » + paragraphe + `.problem-pains` (6 cards)
  - `.problem-pains` : grid `repeat(3, 1fr)` × 2 lignes — 6 pain-cards (Posts sponsorisés, Contenus suggérés, Stories sponsorisées, Doom-scrolling, Perte de temps, Anxiété numérique). Icônes SVG via `url(#authGrad)`.
  - `.phone-mockup` (col droite) : `.phone-frame > .phone-screen > .phone-feed > .phone-feed-track` — feed scroll synthétique
  - `.problem-stats > .problem-stat` : bandeau italique « 96 fois par jour » (margin-top négatif + translateY pour repositionner)
  - `.section-scroll` → scroll vers `#section-features`
- **Animations** : feed scroll JS sur `.phone-feed-track` (sponsor + neutres + videos via IIFE `initFeed` sans data-feed-mode).

---

## 3. Conçu pour te libérer — `<section class="section-features" id="section-features">`

- **Hauteur** : `min-height: 100vh; padding: 60px 24px 100px; flex-column; justify-content: center`
- **Contenu** :
  - `.features-header` : section-label « Ce qu'Authentique fait » + h2 « Conçu pour te libérer. » + paragraphe
  - `.features-grid` : grid 3 col × 2 lignes — 6 `.feature-card` (Masquer posts sponsorisés / suggestions / Reels / stories / scroll / Recherche)
    - Chaque card : `.feature-toggle` (input checkbox + `.feature-switch` 48×28 visuel) + `.card-body > .card-content` (icon + h3 + p) + `.card-on` (message vert)
    - CSS via `:has(input:checked)` toggle l'état
  - `.section-scroll` → scroll vers `#screens`
- **Animations** : IIFE toggles ON/OFF en boucle ordre fixe `[0,2,4,1,3,5]`, ON 2.5 s → OFF 1.5 s → suivant. Inputs `pointer-events: none`.

---

## 4. L'app en images (sticky 4 slides) — `<section class="section-screens" id="screens">`

- **Hauteur** : section sans hauteur explicite, `.sticky-outer { height: 400vh }` capture le scroll, `.sticky-inner { position: sticky; top: 0; height: 100vh; flex-column }`
- **Header** : `.screens-header` (label + h2 « Vois la différence par toi-même. ») dans `.sticky-inner`, padding-top 96
- **Slides area** : `.sticky-slides-area { flex: 1; position: relative }` — 4 `.sticky-slide` superposés en absolute, fade opacity 0.4 s entre eux

### Slides
| # | Titre | Mockup gauche | Mockup droit |
|---|-------|---------------|--------------|
| 1 | Zéro publicité dans ton fil | `iphone-screen--demo` + feed `chaos` (sponsor) | `iphone-screen--demo + --counter` + feed `calm` (neutres) + strip « X éléments masqués » verte |
| 2 | Fini les stories publicitaires. | `iphone-screen--stories data-stories-mode="chaos"` (3 sponsor + 1 neutre) | `iphone-screen--stories data-stories-mode="overlay"` (alternance neutre + overlay Authentique app-style) |
| 3 | Les Reels, à ta façon. | `iphone-screen--reels data-reels-mode="chaos"` (5 Reels translateY 1.5s) | `.iphone-screen` vide |
| 4 | L'onglet Recherche épuré. | `iphone-screen--light` vide | `.iphone-screen` vide |

- **Indicateurs** : `.sticky-dots` (4 dots) absolute bottom 78, centrés. Dot actif `#19d382` 10×10, inactifs `#ccc` 7×7.
- **Bouton scroll sticky** : `.sticky-scroll` absolute bottom 32, centré. Click : si `currentIdx < 3` → slide suivant ; sinon → `#control`.
- **Animations** : IIFE `update()` calcule `progress = -outer.rect.top / scrollable` et toggle `.is-active`. IIFE feed (slide 1), IIFE stories (slide 2), IIFE reels (slide 3).

---

## 5. Tu contrôles tout. — `<section class="section-control" id="control">`

- **Hauteur** : `height: 100vh; padding: 48px 24px 100px; flex-column`
- **Contenu** (centré verticalement, gap 56) :
  - `.control-head` : h2 « Tu contrôles tout. » (« tout » en `.authentique-gradient-text`) + `.control-sub` paragraphe
  - `.control-phones` : **un seul** mockup iPhone gauche avec `.iphone-screen--settings` contenant le mockup Settings (titre `Paramètres` + 7 toggles)
    - Toggle ON `#19d382`, OFF `#e0e0e0`, taille 32×19, cercle blanc 15
    - Labels traduits via `data-i18n="settings_label_*"`
  - `.control-continuation` : h3 « Et ce n'est que le **début**. » + p « De nouvelles fonctionnalités arrivent régulièrement. »
  - `.section-scroll` → scroll vers `#wellbeing`
- **Animations** : aucune (Settings statique).

---

## 6. Bien-être numérique — `<section class="section-wellbeing" id="wellbeing">`

Section fusionnée Ally + Philosophy.

- **Hauteur** : `min-height: 100vh; padding: 80px 24px 100px; flex-column; justify-content: center`
- **Contenu** :
  - `.wellbeing-inner > .wellbeing-top` (flex:1) : `.ally-header` (label « Bien-être numérique » + h2 « Ton allié contre l'addiction digitale. ») + `.ally-grid` (3 cards `Moins de stimulation / Moins de temps perdu / Plus de liens vrais` avec icônes SVG en `url(#authGrad)`). `.ally-grid { margin: auto 0 }` la centre dans wellbeing-top.
  - `.philosophy-separator` : `<hr>` 60% width, border-top `#d0d0cc`, margin 32 auto
  - `.philosophy-inner` (flex:1) : logo arche + blockquote « Les réseaux sociaux devraient te connecter… » + paragraphe philosophy_text
  - `.section-scroll` → scroll vers `#cta`
- **Animations** : aucune (statique).

---

## 7. On construit Authentique ensemble. — `<section class="section-cta" id="cta">`

- **Hauteur** : `height: 100vh; padding: 48px 24px 100px; flex-column`
- **Layout** : `.cta-inner > .cta-top-half` (flex:1) + `<hr class="cta-separator">` (au centre exact 50%) + `.cta-bottom-half` (flex:1)
- **Top half** :
  - h2 « On construit `<br>` Authentique ensemble. » (« Authentique » en `.authentique-gradient-text` + GrosVentre, padding-top 64)
  - `.cta-top-group` (centré) : `.cta-thanks` paragraphe + `.cta-action > .btn-primary` Kickstarter
- **Bottom half** (centré, gap 24) :
  - `.cta-prompt` : « Tu as une idée, une question ou une suggestion ? »
  - `.suggest-form` : textarea + bouton Envoyer (avion en papier). Submit JS ouvre mailto.
  - `.cta-email-line` : « ou écris-nous directement à hello@get-authentique.com »
- **Bouton scroll** : `.section-scroll` → scroll vers `#faq`
- **Animations** : aucune.

---

## 8. FAQ — `<section class="section-faq" id="faq">`

- **Hauteur** : `min-height: calc(100vh - 91px)` (laisse place au footer en sibling)
- **Padding** : `120px 24px`
- **Contenu** :
  - `.faq-inner` : section-label « FAQ » + h2 « Questions fréquentes » + `.faq-list` (8 `<details class="faq-item">`)
  - 8 questions/réponses (faq_q1..q8 / faq_a1..a8) traduites 6 langues, mots-clés en `<b>` dans les réponses.
  - Flèche `::after` qui rotate `[open]`.
- **Animations** : `<details>` natif (transition CSS sur la flèche).

---

## 9. Footer — `<footer>` (sibling de section-faq, pas dans une section)

- **Hauteur naturelle** : ~91 px (padding 32 vertical + ~26 contenu + 1 border-top)
- **CSS** : `border-top` léger, `padding: 32px 24px`, `display: flex; justify-content: space-between; flex-wrap: wrap; max-width: 1100px; margin: 0 auto`
- **Contenu** :
  - `.footer-logo` : arche-icon + `.footer-name` « Authentique » (vert)
  - `.footer-links` : Contact (mailto) + Mentions légales (lien `/mentions-legales.html`)
  - `.footer-copy` : « © 2026 Authentique »
- **Fond visible** : body `#f0f0ec` (le footer n'a pas de bg propre)
- **Animations** : aucune.
