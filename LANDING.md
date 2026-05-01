# LANDING.md — Contexte persistant authentique-landing

Fichier de référence pour toutes les modifications futures de `index.html`.
Lire ce fichier avant chaque modif pour éviter les ré-explorations inutiles.

---

## 1. STRUCTURE HTML — IDs et classes importantes

### Globaux
- `#authGrad` — `<linearGradient>` défini dans un `<svg width="0" height="0">` en haut du body (ligne ~658). Réutilisable partout via `fill="url(#authGrad)"` ou `stroke="url(#authGrad)"`.
- `.arch-icon` — wrapper du logo arche (le « A » géométrique). Utilisé dans nav, hero, philosophy, footer.
- `.section-label` — petite étiquette grise au-dessus de chaque titre de section.
- `.reveal` — éléments qui s'animent au scroll (fade-in via IntersectionObserver).
- `.authentique-gradient-text` — applique le dégradé Authentique au texte (background-clip: text).

### Navigation (`<nav>`, ligne 669)
- `.nav-logo` — lien retour haut (logo arche)
- `.nav-right` — conteneur droite (langues + CTA)
- `.lang-switcher` — bloc des 4 boutons FR/EN/ES/RU
- `.lang-btn` / `.lang-btn.active` — bouton de langue
- `.nav-cta` — bouton « Soutenir sur Kickstarter » (K + texte + flèche)

### Hero (`section.hero`, ligne 698)
- `.hero-arch` — grand logo arche au-dessus du titre
- `.hero-title` — `<h1>` global
- `#hero-line1` — première ligne du titre = mot « Authentique. » (GrosVentre + #19d382)
- `#hero-line2` — deuxième ligne = sous-titre (« Ton Instagram, tes choix. »)
- `.hero-title-line` / `.hero-title-line--regular` — modificateurs des lignes
- `.hero-title-accent` — mot accentué dans la ligne 2 (« choix », « choices », etc.)
- `.hero-platforms-wrap` / `.hero-platforms-caption` / `.hero-platforms` — bloc « Bientôt disponible sur : » + pills
- `.platform-pill` — pill iOS / Android
- `.hero-actions` — conteneur du CTA principal
- `.btn-primary` — bouton vert « Soutenir sur Kickstarter » (K + texte + flèche)
- `.hero-tagline` — phrase d'accroche sous le bouton

### Section Problème (`section.section-problem`, ligne 764)
- `.problem-grid` — grille texte + mockup téléphone
- `.problem-text` — colonne gauche
- `.problem-pains` — grille des 6 pains
- `.pain-card` — carte d'un pain (icône + label)
- `.pain-icon` — wrapper SVG (utilise `url(#authGrad)`)
- `.pain-label` — texte du pain (data-i18n: pain1..pain6)
- `.phone-mockup` / `.phone-frame` / `.phone-notch` / `.phone-screen` — mockup iPhone côté droit
- `.phone-feed` / `.phone-feed-track` — conteneurs du faux feed scrollant (cartes injectées en JS)

### Section Features (`section.section-features`, ligne 839)
- `.features-inner` — wrapper centré
- `.features-header` — header (label + h2)
- `.features-grid` — grille 6 cartes
- `.feature-card` — encadré blanc (les 6 features)
- `.feature-toggle` / `.feature-toggle label` / `.feature-toggle input` / `.feature-switch` — toggle ON/OFF en haut de chaque carte (style identique à l'app : 48×28 px, vert #19d382 ON, gris #e0e0e0 OFF, cercle blanc qui glisse 20 px)
- `.card-body` — wrapper position:relative pour le swap content/on
- `.card-content` — contenu OFF (icon + titre + desc)
- `.card-on` — overlay absolu, message court vert #19d382 affiché quand toggle ON (pure CSS via `:has(input:checked)`)
- `.feature-icon` — wrapper SVG en haut de carte (utilise `url(#authGrad)`)
- Features data-i18n : `f1_title..f6_title`, `f1_desc..f6_desc`
  - f1 = Masquer les posts sponsorisés
  - f2 = Masquer les suggestions de comptes
  - f3 = Section Recherche épurée
  - f4 = Masquer les Reels
  - f5 = Masquer les stories sponsorisées
  - f6 = Bloquer le scroll infini

### Section Screens (`section.section-screens`, ligne 913)
- `.screens-inner` — wrapper centré
- `.screens-header` — header de section
- `.screens-row` / `.screens-row--reverse` / `.screens-row--center` — alternance gauche/droite
- `.screen-col` / `.screen-col--text` / `.screen-col--phones` — colonnes texte / téléphones
- `.iphone` / `.iphone--featured` — mockup iPhone (le 2e est mis en avant)
- `.iphone-frame` / `.iphone-dynamic-island` / `.iphone-screen` — composants du mockup
- Images : `images/feed.jpg`, `feed-light.jpg`, `recherche-light.jpg`, `recherche-dark.jpg`, `reels-dark.jpg`, `stories-light.jpg`, `parametres-light.jpg`, `parametres-dark.jpg`

### Section Philosophie (`section.section-philosophy`, ligne 1007)
- `.philosophy-inner` — wrapper centré
- `.philosophy-arch` — logo arche

### Section CTA finale (`section.section-cta#cta`, ligne ~1003)
- `.cta-inner` — wrapper centré
- `.cta-thanks` — phrase de remerciement
- `.cta-action` — wrapper du bouton Kickstarter
- `.cta-contact` — ligne « Tu as des questions ? hello@... »

### Section FAQ (`section.section-faq#faq`, ligne ~1024)
- `.faq-inner` — wrapper centré (max-width 760px)
- `.faq-list` — liste verticale d'items
- `.faq-item` — `<details>` avec fond blanc + radius 12px + flèche rotative
- `.faq-item summary` — la question (couleur #141414)
- `.faq-item p` — la réponse (couleur #555)
- Flèche : pseudo-element `::after` (border-right + border-bottom rotatés), tourne via `[open]`

### Footer (`<footer>`, ligne 1039)
- `.footer-logo` / `.footer-name` — logo + nom Authentique
- `.footer-links` — liens (CGU, contact, etc.)
- `.footer-copy` — copyright

### Feed mockup (JS, ligne ~1311+)
- `.feed-card` — carte du faux feed
- `.feed-action` — bouton action (like/comment/repost/send)
- Icônes : `ICON_HEART`, `ICON_COMMENT`, `ICON_REPOST`, `ICON_SEND`
- Hook live update labels : `window.__authUpdateScrollLabels`

---

## 2. DESIGN SYSTEM

### Couleurs
- **Vert principal** : `#19d382` (mot Authentique, accents, CTA primaire, bordures)
- **Fond clair** : `#f0f0ec` (fond global de la page)
- **Texte sombre** : `#141414` (titres, points finaux, corps)

### Dégradé Authentique
```css
linear-gradient(138deg, #adf5d8, #38eb98, #19d382, #10c89e, #0aaa8e)
```
Stops SVG correspondants (`#authGrad`) :
- 0% → `#adf5d8`
- 22% → `#38eb98`
- 50% → `#19d382`
- 72% → `#10c89e`
- 100% → `#0aaa8e`

### Polices
- **GrosVentre** — embedded en base64 dans le `<style>` (extraite du logo v5). Utilisée pour : titres `<h1>`/`<h2>`, mot « Authentique » partout.
- **-apple-system, BlinkMacSystemFont, "Segoe UI"...** — corps de texte, paragraphes, UI.

### Classe utilitaire texte dégradé
```css
.authentique-gradient-text {
  background: linear-gradient(138deg, #adf5d8, #38eb98, #19d382, #10c89e, #0aaa8e);
  -webkit-background-clip: text;
  background-clip: text;
  -webkit-text-fill-color: transparent;
  color: transparent;
}
```
Wrapper JS auto sur le mot « Authentique » : `wrapAuthentiqueOccurrences()` (DOMContentLoaded), skip SPAN/SCRIPT/STYLE.

---

## 3. RÈGLES IMPORTANTES

### Visuel
- Le mot **« Authentique »** apparaît toujours en GrosVentre + couleur `#19d382` (sauf cas spécifiques où le dégradé est appliqué via `.authentique-gradient-text`).
- Les **icônes SVG des features et pains** utilisent `fill="url(#authGrad)"` ou `stroke="url(#authGrad)"`.
- Le `<defs>` `#authGrad` est défini **une seule fois** en haut du HTML (ligne ~656). Ne pas dupliquer.
- **Bouton Kickstarter** : `href="#"` (placeholder, pas encore live). Structure obligatoire :
  ```html
  <svg> K logo </svg>
  <span data-i18n="...">Texte traduit</span>
  <svg> flèche </svg>
  ```
  Le `data-i18n` doit être sur le `<span>`, **jamais** sur le `<a>` parent (sinon `setLang()` écrase les SVGs via innerHTML).

### Édition
- **Ne jamais réécrire tout le fichier** — utiliser Edit/str_replace pour cibler les sections.
- **Grep d'abord** : chercher la classe/ID concerné avant Read.
- Les **traductions** vivent dans 6 blocs (FR/EN/ES/IT/PT/RU) du `<script>` en bas du fichier, à mettre à jour ensemble pour toute clé `data-i18n`. Le sélecteur de langue affiche les 6 codes en texte (FR | EN | ES | IT | PT | RU). Le bouton actif est résolu via `data-lang` sur chaque `.lang-btn`.

### Git
- Toujours `git add -A && git commit -m "..." && git push` **sans demander confirmation**.
- Format de commit : type court en français (`feat:`, `fix:`, `docs:`, `style:`).

---

## 4. SECTIONS DE LA PAGE (ordre)

| # | Section | Tag/Class | Ancre |
|---|---------|-----------|-------|
| 1 | Navigation | `<nav>` | — |
| 2 | Hero | `section.hero` | top |
| 3 | Problème | `section.section-problem` | — |
| 4 | Features (« Conçu pour te libérer ») | `section.section-features` | — |
| 5 | Screens (« L'app en images ») | `section.section-screens` | — |
| 6 | Philosophie | `section.section-philosophy` | — |
| 7 | CTA finale | `section.section-cta` | `#cta` |
| 8 | FAQ | `section.section-faq` | `#faq` |
| 9 | Footer | `<footer>` | — |

### Navigation interne
- Lien retour haut : `<a href="#">` (logo nav + footer)
- Lien CTA finale : `<a href="#cta">` (ancre vers la section finale)
- Lien Kickstarter : `href="#"` (placeholder, à remplacer le moment venu)

---

## 5. JS / Hooks utiles

- `setLang(lang)` — change la langue (FR/EN/ES/RU). Met à jour tous les `data-i18n` via innerHTML + appelle `window.__authUpdateScrollLabels()` pour les labels du mockup.
- `fitHeroTitle()` — ajuste dynamiquement la taille de `#hero-line2` pour matcher la largeur de `#hero-line1` (window.load + document.fonts.ready).
- `wrapAuthentiqueOccurrences()` — wrap auto du mot « Authentique » dans `.authentique-gradient-text` au DOMContentLoaded.
- `IntersectionObserver` sur `.reveal` — animation fade-in au scroll.
- Faux feed mockup — boucle scroll saccadé (kick + ease-in + micro-pause), 12-15 cartes, 3 types, dark mode IG-style.

---

## 6. Fichiers du repo

- `index.html` — fichier principal
- `mentions-legales.html` — page CGU + politique de confidentialité + mentions légales
- `questionnaire/index.html` — page questionnaire séparée
- `images/` — screenshots iPhone (feed, recherche, reels, stories, parametres × light/dark)
- `LANDING.md` — ce fichier (contexte persistant)
