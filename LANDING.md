# LANDING.md — Contexte persistant authentique-landing

> **Lis ce fichier EN ENTIER avant toute modification. Ne modifie que ce qui est demandé. Ne réécris jamais tout `index.html`.**

Voir aussi `SECTIONS.md` pour la liste exhaustive des sections.

---

## Instructions de communication

- Réponses courtes et directes
- Pas d'explication longue sauf si explicitement demandée
- Afficher uniquement le code modifié, pas le fichier entier
- Format de réponse :
  1. Ce qui a été fait (1-2 lignes)
  2. Code modifié si pertinent
  3. Commit pushé ✅
- Ne jamais expliquer ce qu'on pourrait faire — juste le faire
- Si diagnostic demandé : afficher le code concerné + la cause en 2-3 lignes max
- Toujours committer et pusher sans demander confirmation
- Ne jamais réécrire `index.html` en entier — modifications chirurgicales uniquement

---

## Règles à ne jamais enfreindre

1. **Ne jamais réécrire `index.html` en entier**. Toujours utiliser des `Edit` ciblés sur les blocs concernés.
2. **Toujours `git add -A && git commit -m "..." && git push` sans demander confirmation** après chaque modif.
3. **Format de commit** : type court en français (`feat:`, `fix:`, `docs:`, `style:`).
4. **Avant toute modif structurelle** : lire le fichier (`Read`) ou faire un `git show <commit>:index.html` pour vérifier l'état réel.
5. **Quand l'utilisateur demande de restaurer un état antérieur** : `git diff HEAD~N` ou `git show <commit>:file` puis appliquer en `Edit` ciblé. Ne jamais modifier ce qui n'est pas demandé.
6. **Toujours mettre à jour les 6 langues** (FR/EN/ES/IT/PT/RU) en même temps quand on touche à `data-i18n`.
7. **Pas d'emojis** dans le code/HTML sauf demande explicite.

---

## 1. STRUCTURE & SECTIONS

Ordre vertical (chacune `100vh` sauf le sticky qui fait 400vh outer + 100vh inner) :

| # | Section | id | Class |
|---|---------|----|----|
| 1 | Hero | — | `.hero` |
| 2 | Le problème | `section-problem` | `.section-problem` |
| 3 | Conçu pour te libérer | `section-features` | `.section-features` |
| 4 | L'app en images (sticky 4 slides) | `screens` | `.section-screens` |
| 5 | Tu contrôles tout. | `control` | `.section-control` |
| 6 | Bien-être numérique + Philosophy fusionnés | `wellbeing` | `.section-wellbeing` |
| 7 | On construit Authentique ensemble. | `cta` | `.section-cta` |
| 8 | FAQ (8 items) | `faq` | `.section-faq` |
| 9 | Footer | — | `<footer>` (sibling de section-faq) |

Détail dans `SECTIONS.md`.

---

## 2. DESIGN SYSTEM

### Couleurs CSS variables
```css
--bg: #f0f0ec        /* fond global page */
--white: #ffffff     /* fond sections claires */
--text: #141414      /* corps de texte sombre */
--text-soft: #555    /* texte secondaire */
--text-mute: #888    /* texte tertiaire */
--border: rgba(20,20,20,0.08)
--green: #19d382     /* vert principal Authentique */
--auth-grad: linear-gradient(138deg, #adf5d8, #38eb98, #19d382, #10c89e, #0aaa8e)
--grad-icon: linear-gradient(138deg, #19d382, #10c89e)
```

### SVG defs partagé
- `<linearGradient id="authGrad">` défini une seule fois dans `<svg width="0" height="0">` en haut du body. Utilisé par les SVG features/pains/ally via `fill="url(#authGrad)"` ou `stroke="url(#authGrad)"`.

### Polices
- **GrosVentre** — police custom embedded base64. Utilisée pour h1/h2/h3 + mot « Authentique ».
- **System UI** — `-apple-system, BlinkMacSystemFont, 'Segoe UI', Helvetica, Arial, sans-serif` pour le corps.

### Classes utilitaires
- `.authentique-gradient-text` — applique le **dégradé** Authentique au texte (background-clip text). Pour usage explicite.
- `.authentique-name` — applique le **vert plein** `#19d382`. Utilisé par l'auto-wrapper `wrapAuthentiqueOccurrences()` sur chaque occurrence du mot « Authentique » (skip SPAN/SCRIPT/STYLE).
- `.reveal` — fade-in + translateY 20→0 au scroll, déclenché par `IntersectionObserver`.
- `.section-label` — petit label gris au-dessus des h2 de section.

### Boutons
- `.btn-primary` — bouton principal vert dégradé (Kickstarter, Envoyer)
- `.section-scroll` / `.sticky-scroll` — flèche `↓` chevron animée bounce, position absolute bottom 32, left 50%, translateX(-50%). Couleur `#19d382`. SVG 40×40 stroke 3.6.
- `.hero-scroll` — variante hero (id`#hero-scroll`), même style + fade out après 100 px de scroll.

---

## 3. SYSTÈME i18n

### Source des traductions
Bloc `<script>` en bas du fichier avec un objet `translations` contenant 6 clés langue (`fr`, `en`, `es`, `it`, `pt`, `ru`).

### Switcher
6 boutons dans `<nav>` : `<button class="lang-btn" data-lang="fr" onclick="setLang('fr')">FR</button>` etc.

### Fonction `setLang(lang)`
1. Met à jour `currentLang` global.
2. Active la `.lang-btn[data-lang=lang].active`.
3. Pour chaque élément `[data-i18n]` : remplace l'`innerHTML` par `translations[lang][key]`.
4. Pour chaque `[data-i18n-placeholder]` : remplace l'attribut `placeholder`.
5. Appelle les hooks live-update :
   - `window.__authUpdateScrollLabels(lang)` — labels du scroll feed
   - `window.__authUpdateHiddenStrip()` — strip « X éléments masqués »
6. Re-run `wrapAuthentiqueOccurrences()` pour wrapper les nouveaux mots « Authentique » du DOM mis à jour.

### Ajouter une clé i18n
1. Ajouter `<element data-i18n="ma_cle">Texte FR par défaut</element>` dans le HTML.
2. Ajouter `ma_cle: "..."` dans **chacun** des 6 blocs de translations (FR/EN/ES/IT/PT/RU).
3. Si l'élément contient des SVG inline, les sortir du `data-i18n` (sinon `setLang()` les wipe via innerHTML). Mettre le `data-i18n` sur un `<span>` enfant.

### Clés HTML (dynamiques, ne pas mettre les SVG dedans)
- Pour les boutons CTA Kickstarter avec K-logo + flèche : structure `[K SVG] [span data-i18n] [arrow SVG]`
- Pour les titres avec gradient ponctuel : `<h2 data-i18n="cta_title">On construit&nbsp;<span class="authentique-gradient-text">Authentique</span> ensemble.</h2>` — la traduction inclut le span dans la chaîne.

---

## 4. ANIMATIONS JS

Fichiers : tout est inline dans `<script>` au fond de `index.html`.

### Globaux
- **`setLang(lang)`** — change la langue (cf. § 3)
- **`wrapAuthentiqueOccurrences()`** — au DOMContentLoaded, parcourt le DOM et wrap les occurrences du mot « Authentique » dans `<span class="authentique-name">`. Skip SPAN/SCRIPT/STYLE.
- **`fitHeroTitle()`** — ajuste dynamiquement la taille de `#hero-line2` pour matcher la largeur de `#hero-line1`.
- **`submitSuggestion(e)`** — handler du form CTA suggestion → ouvre `mailto:hello@get-authentique.com?subject=Suggestion Authentique&body=…`
- **IntersectionObserver `.reveal`** — fade-in au scroll.

### Hero
- **Hero scroll arrow** : IIFE qui écoute le scroll et toggle `.is-hidden` après 100 px. Au clic, scroll smooth vers `#section-problem` (offset = `rect.top + scrollY`).

### Sticky scroll (section L'app en images)
- IIFE calcule `progress = scrolled / (outer.height - 100vh)` et toggle `.is-active` sur le bon `.sticky-slide` (4 slides). Les dots correspondants reçoivent aussi `.is-active`.
- Bouton scroll sticky : si `currentIdx < 3`, scroll vers le slide suivant ; sinon vers `#control`.
- Expose `outer.__getStickyIndex` pour le handler du bouton.

### Toggles features
- IIFE qui pilote les 6 toggles ON/OFF de la section Features. **Ordre fixe** `[0,2,4,1,3,5]` répété en boucle. Cycle : ON 2.5 s → OFF 1.5 s → toggle suivant.
- Les inputs sont en `pointer-events: none` (non cliquables).

### Phone feed (problem section + slide 1 left + slide 1 right)
- IIFE `initFeed(track, opts)` itère sur tous les `.phone-feed-track`. Mode lu via `data-feed-mode` (`chaos` = sponsor + neutres, `calm` = neutres only). Le défaut (sans data-feed-mode) = problem section, avec sponsors.
- Opts : `kickVelocity` (default 18 px/frame, `calm` = 14), `kickDuration` (120), `pauseMs` (150), `neutralOnly` (calm only).
- Cards : sponsor (gradient + label), neutral (#2d2d2d + avatar simulé), video (#2d2d2d + play SVG).
- Animation : kick-velocity decay (1-t)² sur kickDuration frames, micro-pause `pauseMs`, recyclage des cards qui sortent par le haut (DOM move au bottom).
- Hook `window.__authUpdateScrollLabels(lang)` met à jour les labels « Sponsorisé / Pour vous / etc. » en live.

### Counter strip (slide 1 right phone)
- IIFE qui anime un compteur 0 → 47 toutes les 2 s sur la strip `[data-hidden-strip]`. Démarre quand le strip entre dans le viewport (IntersectionObserver, threshold 0.3).
- Format via `translations[lang].hidden_count_format` avec placeholder `{n}`.
- Hook `window.__authUpdateHiddenStrip()` re-render le texte au changement de langue.

### Stories (slide 2)
- 2 mockups : `data-stories-mode="chaos"` (3 sponsor + 1 neutre, 1.2 s/story) et `data-stories-mode="overlay"` (alternance neutre 2.5 s + overlay Authentique 5 s).
- IIFE `initStories(screen, cards, durations)` : barre de progression UNIQUE pilotée par `requestAnimationFrame`. Quand fill atteint 100%, advance card + reset.
- Bar `.stories-progress--green` quand la carte courante est `.story-overlay`. Wrap reset pour sens unique gauche → droite.

### Reels (slide 3 left phone)
- `data-reels-mode="chaos"`. 5 Reels pré-générés avec dégradés de la palette sponsor (alternés via `pickDifferent`).
- Transition translateY 0.4 s ease-in-out, advance toutes les 1.5 s via setInterval.
- Chaque Reel : header (avatar + namebar + Suivre), label rotatif, description 2 barres, barre son, colonne actions ♥ 💬 ↻ ➤ ⋮.

---

## 5. STRUCTURE DE FICHIER `index.html`

Ordre approximatif (~3800 lignes) :
1. `<head>` : meta, CSS inline (font GrosVentre base64, variables, classes utilitaires, sections)
2. `<body>` : SVG `#authGrad` defs, `<nav>`, sections dans l'ordre, `<footer>`, `<script>`
3. `<script>` : `translations` object (6 langues), `setLang`, `wrapAuthentiqueOccurrences`, `fitHeroTitle`, `submitSuggestion`, IIFE hero-scroll, IIFE toggles features, IIFE feed (problem + s1), IIFE stories (slide 2), IIFE reels (slide 3), IIFE counter strip.

---

## 6. FICHIERS DU REPO

- `index.html` — fichier principal (~3800 lignes, ~170 KB avec font base64)
- `mentions-legales.html` — page CGU + politique de confidentialité + mentions légales
- `questionnaire/index.html` — page questionnaire séparée
- `images/` — captures iPhone (legacy, partiellement utilisées)
- `LANDING.md` — ce fichier (contexte persistant)
- `SECTIONS.md` — détail section par section
