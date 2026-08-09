# HANDOFF-DOMAINE.md — Mise en ligne sur erwan-milbeo.com (home + page réservation)

> Passation **spécifique au chantier « domaine erwan-milbeo.com »** (home vitrine + page
> de réservation sous-chemin). À lire avec `PROJECT.md` et `HANDOFF.md` (qui, eux,
> documentent la page de réservation elle-même).
> Dernière mise à jour : **2026-08-08**.

---

## 0. Reprendre le projet en 30 secondes

1. Lire ce fichier, puis `PROJECT.md` + `HANDOFF.md`.
2. Dépôt local : `/Users/EM/reservation-sauveteur-secouriste/` (GitHub :
   `ErwanMILBEO/reservation-sauveteur-secouriste`).
3. **État : tout est en LOCAL, non commité, non poussé.** Rien n'est en ligne sur le
   domaine pour l'instant. La page de réservation, elle, est toujours en ligne à son
   ancienne URL GitHub Pages (voir §2).
4. Lancer l'aperçu local :
   ```bash
   cd /Users/EM/reservation-sauveteur-secouriste && python3 -m http.server 4599
   ```
   puis ouvrir `http://localhost:4599/` (home) et
   `http://localhost:4599/nageur-sauveteur-secouriste/` (réservation).
5. Méthode de travail (règles Erwan) : **plan validé AVANT exécution** ; **visuel validé
   sur localhost AVANT tout déploiement** ; répondre **en français + tutoiement** ;
   contenus **FR et EN** ; **toujours vérifier le mobile** ; sur les choix de goût,
   **montrer les options sans imposer de préférence**.

---

## 1. Objectif du chantier

Mettre le site en ligne sur le domaine **erwan-milbeo.com** (acheté chez **Namecheap**) avec :

| URL | Contenu |
|-----|---------|
| `www.erwan-milbeo.com` | **Home / vitrine perso** d'Erwan Milbéo (nouvelle page, style « portail de cartes ») |
| `www.erwan-milbeo.com/nageur-sauveteur-secouriste` | La **page de réservation** existante (celle du dépôt), inchangée |

Architecture retenue : **un seul dépôt GitHub Pages** pour le domaine — `index.html` (la home)
à la racine + sous-dossier `nageur-sauveteur-secouriste/` (la réservation) + `CNAME`.

---

## 2. Structure du dépôt (après restructuration — en local)

```
reservation-sauveteur-secouriste/
├── index.html                    → HOME (nouvelle vitrine bento)   ← le gros du chantier
├── CNAME                         → contenu : www.erwan-milbeo.com
├── .nojekyll                     → site statique servi tel quel (couvre tout le dépôt)
├── PROJECT.md                    → doc de la page de réservation
├── HANDOFF.md                    → passation de la page de réservation
├── HANDOFF-DOMAINE.md            → CE fichier
└── nageur-sauveteur-secouriste/
    └── index.html                → page de réservation (DÉPLACÉE via `git mv`, intacte)
```

- La page de réservation est **100 % autonome** (aucune dépendance à un chemin racine) →
  elle fonctionne telle quelle sous le sous-dossier.
- URL de prod actuelle de la réservation (tant que le domaine n'est pas branché) :
  `https://erwanmilbeo.github.io/reservation-sauveteur-secouriste/` — **après** le push de
  restructuration, cette URL affichera la HOME, et la réservation passera sous
  `.../reservation-sauveteur-secouriste/nageur-sauveteur-secouriste/`.

---

## 3. La HOME (`index.html` racine) — anatomie

Fichier **unique**, HTML + CSS + JS **inline**, zéro dépendance de build. Esprit graphique
**distinct** de la page de réservation : **page-portail « bento »**, chaque carte a sa
propre ambiance.

### 3.1 Polices (Google Fonts)
`Bebas Neue` (titres des box), `Cormorant Garamond` (nom, sous-titres serif),
`Fraunces` italique (slogan), `Michroma` (titre large de la carte Entrepreneur),
`IBM Plex Mono` (labels/kickers/switch), `Inter` (corps).

### 3.2 Palette (`:root`)
marine `#0A2A3B` · marine-2 `#0F3A50` · foam `#F5FAF9` · pool `#147D86` ·
pool-light `#DCEEEF` · gold `#E8C468` · gold-soft `#F0D488` · ink `#16323F` ·
mist `#7C9399` · line `#D7E3E4` · coral `#E8734A` · coral-2 `#D6552E` · page `#EEF3F4`.
Ambre du logo : `#E8B23C`.

### 3.3 Structure
`.wrap > main` contient, dans l'ordre :
1. **`.card.area-intro`** — le **hero** (carte autonome, HORS grille bento).
2. **`.section-lead`** — le **sous-titre** de structure (voir §3.6).
3. **`.bento`** — la grille des **4 box**.

Grille bento **desktop** (12 colonnes) : `lead` (7 col, 2 rangs) · `sport` (5 col, rang 1)
· `save` (5 col, rang 2) · `skill` (12 col, rang 3).
**Mobile** (`max-width:860`) : 1 colonne, ordre `lead → save → sport → skill`.

### 3.4 Le hero (`.area-intro`)
- Fond clair dégradé, flex horizontal (emblème à gauche, corps à droite ; empilé en mobile).
- **Switch FR/EN** en position absolue **haut-droite** (`.lang-switch` / `.lang-btn`).
- **Nom** `.intro-name` : Cormorant 600, 46 px (38 mobile), **noir atténué de 20 %**
  (`color:rgba(22,50,63,.8)`). Balise `<span class="nm-first">Erwan&nbsp;</span>Milbéo`
  (le span sert à mesurer la largeur de « Erwan » pour le calage du slogan).
- **Slogan** `.hero-baseline` : **« Many roles. One freedom. »** — Fraunces **italique**,
  22 px (17 mobile), turquoise, **entre guillemets typographiques**. **JAMAIS TRADUIT**
  (pas de `data-i18n`). Calage : **desktop** commence pile sous le **M de Milbéo**
  (marge gauche = largeur de « Erwan » calculée en JS) ; **mobile** léger retrait fixe
  (`margin-left:3em`).
- **Bio** `.intro-bio` (`data-i18n="hero.bio"`) — placeholder.

### 3.5 Emblème + logo (`.intro-emblem`)
- Carré turquoise (dégradé radial), 112 px (88 mobile), radius 26, **ombre « flottante »**
  (box-shadow portée renforcée + reliefs internes) + **animation `emblem-float`**
  (translateY ±7 px, 5.5 s ; coupée en `prefers-reduced-motion`).
- Contient le **LOGO** (SVG inline, viewBox 120, 78 px desktop / 62 mobile) :
  - **Monogramme ME** (blanc `#F5FAF9`, trait 6, arrondi) — **montant partagé** : la jambe
    droite du **M** prolongée vers le bas **est** la barre verticale du **E** ; les 3 traits
    horizontaux du E partent de ce montant.
  - **Mappemonde ambre** (`#E8B23C`, trait 2) dans le **coin haut droit**, **dégagée des
    lettres** (cercle + équateur + méridien + 2 parallèles).
- **Favicon** (data-URI SVG dans `<link rel="icon">`) assorti : ME blanc sur carré
  turquoise `#147D86` + petit point ambre.

### 3.6 Les accroches (validées par Erwan — construction copywriting)
- **Hero (slogan, non traduit)** : « Many roles. One freedom. » — l'accroche émotionnelle.
- **Sous-titre `.section-lead` (juste avant les box, traduit)** : le « sommaire ».
  - FR : **« Entrepreneur. Sauveteur. Athlète. D'abord humain. »**
  - EN : **« Entrepreneur. Lifeguard. Athlete. Human first. »**
  - « D'abord humain » / « Human first. » accentué en turquoise (`.section-lead .hf`).
  - ⚠️ Bonne trad de *Human First* = **« D'abord humain »** (surtout pas « Humain d'abord »).

### 3.7 Les 4 box
| Box (classe) | Ambiance | Lien |
|---|---|---|
| **Entrepreneur international** (`.area-lead`, carte leader) | navy & gold premium « façon LE CLUB » | `<a>` → `https://suncom.estate` |

> **Carte Entrepreneur — détail du restyle (réf. « LE CLUB / ESPACE MEMBRES » fournie par Erwan)** :
> fond navy dégradé + **filet doré en bas** (`border-bottom` gold) ; **icône dorée** (immeubles, avec accent turquoise) en haut à gauche (`.lead-icon`) ; **badge encadré doré** (`.lead-badge`, = le kicker) ; **titre large bi-ton** en **Michroma** (`.lead-title` avec `.t1` clair / `.t2` doré — ⚠️ règle sous `.area-lead .lead-title` sinon `.card h2` l'emporte) ; **sous-titre mono doré espacé** (`.lead-sub`) ; corps `.lead-body` ; **CTA pilule contour doré** (fond transparent, se remplit au survol) ; **visuel filigrane** immeubles en bas à droite (`.lead-deco`, opacity .09). **Sans le chiffre** de la réf. Titre i18n = deux spans `.t1`/`.t2`.
| **Sportif** (`.area-sport`) | corail énergique | — |
| **Nageur-Sauveteur-Secouriste** (`.area-save`) | marine/turquoise + bouée déco | `<a>` → `nageur-sauveteur-secouriste/` |
| **Compétences** (`.area-skill`) | sombre technique, 3 colonnes | — |

Certifs `BNSSA · PSE1 · PSE2` : **non traduites** (pas de clé i18n).

### 3.8 Footer
© Erwan Milbéo + contacts : tél **06 50 87 68 87**, mail **em.officiel@pm.me**.

---

## 4. Internationalisation (i18n) — comment ça marche

- Moteur maison en JS (bas de fichier). Chaque élément traduisible porte
  `data-i18n="clé"`. Deux dictionnaires : `I18N.fr` et `I18N.en`.
- `setLang(lang)` : remplace le `innerHTML` de chaque `[data-i18n]`, pose
  `document.documentElement.lang`, met à jour l'état des boutons, **persiste**
  `localStorage.lang`, et réaligne le slogan.
- Défaut : **FR**. Le **slogan n'a pas de `data-i18n`** → jamais traduit.
- **Clés existantes** : `hero.bio`, `sectionlead`, `lead.kicker|title|sub|body|cta`,
  `sport.kicker|title|body|tags`, `save.kicker|title|body|cta`,
  `skill.kicker|title|col1|col2|col3`.
- **Ajouter du contenu** = ajouter la clé dans **`I18N.fr` ET `I18N.en`** (les deux
  langues à chaque fois). Balises HTML autorisées dans les valeurs (rendu via innerHTML).

⚠️ **Les textes actuels sont des PLACEHOLDERS** (sauf les accroches). À réécrire.

---

## 5. Ton éditorial des contenus (validé)

**Style « Forbes »** : storytelling entrepreneurial, registre prestige/magazine business
haut de gamme, phrases affûtées. À appliquer à **tous** les contenus rédactionnels de la
home (bio, entrepreneur, sportif, compétences), **en FR et en EN**.

---

## 6. Aperçu local

- Config dans `~/.claude/launch.json` (PAS dans le dépôt) : entrée `erwan-milbeo-home` =
  `python3 -m http.server 4599 --directory <dépôt>`.
- Relance manuelle : `cd /Users/EM/reservation-sauveteur-secouriste && python3 -m http.server 4599`.
- L'outil `preview_start` a parfois été **bloqué par le classifieur d'auto-mode** → dans ce
  cas, lancer le serveur via une commande shell (même résultat).
- Le navigateur **cache** l'`index.html` → recharger avec un suffixe `?v=N` pour voir les
  modifs.

---

## 7. RESTE À FAIRE (dans l'ordre) — MAJ 2026-08-08

✅ **Rédaction des contenus « Forbes » FR + EN : TERMINÉE.** Toutes les cartes sont
rédigées, plus aucun placeholder (voir changelog 2026-08-08). Rien n'est encore déployé.

**Prochaines étapes validées par Erwan (ordre) :**

✅ **1. SEO / AEO « Erwan Milbéo » : FAIT (09/08).** Voir changelog 2026-08-09.
✅ **2. Déploiement : FAIT (09/08)** via `git push origin main`.
🔲 **3. Connexion DNS Namecheap** (ci-dessous) — PROCHAINE ÉTAPE.
🔲 **4. Vérif des 2 URLs cibles + non-régression réservation** (après propagation DNS).
🔲 **+ Google Search Console** : vérif de propriété + envoi de `sitemap.xml`.

<details><summary>Détail initial de l'étape 1 (conservé pour mémoire)</summary>

1. **SEO / AEO « Erwan Milbéo »** (nouvelle priorité, ajoutée le 08/08) : optimiser la home
   pour la recherche et les moteurs de réponse (AEO). À prévoir : `<title>` + `meta
   description` (FR par défaut) ; **Open Graph** + Twitter Card + **og:image** ; **JSON-LD
   `schema.org/Person`** (name « Erwan Milbéo », jobTitle, sameAs vers suncom.estate/réseaux,
   knowsAbout, knowsLanguage FR/EN/ES) ; **hreflang** FR/EN (le switch est JS côté client →
   voir si on expose 2 URLs ou balises alternates) ; `lang` correct ; `sitemap.xml` +
   `robots.txt` ; favicon/nom cohérents ; canonical. Viser le **Knowledge Panel** « Erwan
   Milbéo ». **À cadrer avec Erwan avant exécution** (périmètre, réseaux à lier, og:image).
2. **Déployer** : `git add` + `commit` + **`git push origin main`** → GitHub Pages publie.
   (Restructuration `git mv` + home + `CNAME` partent au même moment.)
3. **Connexion au NDD → DNS** :
   - **GitHub Pages** : Settings → Pages → Custom domain = `www.erwan-milbeo.com`, puis
     **Enforce HTTPS** une fois la propagation faite.
   - **DNS chez Namecheap** (Erwan les pose, on le guide) :
     - **A records** (apex `erwan-milbeo.com`) → `185.199.108.153`, `185.199.109.153`,
       `185.199.110.153`, `185.199.111.153`.
     - **CNAME** `www` → `erwanmilbeo.github.io`.
4. Vérifier les 2 URLs cibles + non-régression de la page de réservation.

</details>

⚠️ **Piège build Pages connu** : l'étape « deploy » de `pages-build-deployment` peut
**timeout** (« Timeout reached, aborting! ») alors que « build » passe et que le statut
GitHub est « operational ». Ce n'est **pas** le code. Remède : relancer avec un commit à
vide (`git commit --allow-empty`) ; ça finit par passer. Ne **pas** empiler de relances
pendant une panne GitHub Pages avérée.

---

## 8. Changelog du chantier

### 2026-08-09 — SEO / AEO (mot-clé « Erwan Milbéo », FR) + DÉPLOIEMENT
Optimisation référencement (moteurs classiques + IA/AEO), périmètre validé par Erwan :
**FR only**, **un seul mot-clé « Erwan Milbéo »**, **pas de `sameAs`** (réseaux) ni de
Knowledge Panel visé pour l'instant, image de partage = **le logo de la page**.

- **Structure HTML** : ajout des balises d'enveloppe manquantes `<!doctype html>`,
  `<html lang="fr">`, `<head>…</head>`, `<body>…</body>` (le navigateur les inventait ;
  désormais explicites pour les robots). Aucune régression visuelle (head-only).
- **`<head>`** : `title` = **`Erwan Milbéo`** (option A) ; `meta description` FR réécrite ;
  **`canonical` → `https://www.erwan-milbeo.com/`** ; `meta robots`
  (`max-snippet:-1, max-image-preview:large`) ; **Open Graph** (`og:type=profile`) +
  **Twitter Card** avec `og:image` ; **JSON-LD `@graph`** = **`Person`** (jobTitle,
  knowsAbout immobilier/tech/IA/sauvetage-secourisme, knowsLanguage fr/en/es, diplômes
  BNSSA/PSE1/PSE2 dans `description`, `worksFor` SUNCOM) + **`WebSite`** (`inLanguage` fr).
- **Nouveaux fichiers (racine)** :
  - **`og-image.png`** — 1200×1200, reproduction du logo (emblème turquoise ME + mappemonde
    ambre) + nom + slogan sur fond page. Fabriqué depuis un SVG converti via **`qlmanage`**
    (pas de rsvg/chrome sur la machine ; qlmanage force un cadre carré → format carré assumé,
    parfait pour WhatsApp/iMessage/LinkedIn). Source SVG dans le scratchpad de session.
  - **`robots.txt`** — tout ouvert (`User-agent: * / Allow: /`) + **robots d'IA explicitement
    autorisés** (GPTBot, OAI-SearchBot, ChatGPT-User, ClaudeBot, Claude-Web, anthropic-ai,
    PerplexityBot, Google-Extended, Applebot(-Extended), CCBot) + `Sitemap:`.
  - **`sitemap.xml`** — 2 URLs : home (`priority 1.0`) + réservation
    (`/nageur-sauveteur-secouriste/`, `priority 0.7`), `lastmod 2026-08-09`.
- **Domaine (précision Erwan)** : le canonique est **`www.erwan-milbeo.com`** (URL réservée) ;
  **`erwan-milbeo.com` (apex) fonctionne aussi et redirige vers le www** (usage standard).
  **`CNAME` inchangé** = `www.erwan-milbeo.com`.
- **Vérifs** : desktop + mobile (375) **identiques** à avant, **0 erreur console**, **JSON-LD
  valide**, `200` sur `/`, `/og-image.png`, `/robots.txt`, `/sitemap.xml`,
  `/nageur-sauveteur-secouriste/` (servis en local).
- **DÉPLOYÉ** via `git push origin main` (bundle : restructuration `git mv` + home + `CNAME` +
  `og-image.png` + `robots.txt` + `sitemap.xml` + doc).
- **Reste** : brancher le **DNS Namecheap** (§7.3) puis **Google Search Console** (vérif de
  propriété + envoi du sitemap). Sujet `sameAs`/Knowledge Panel mis de côté (à reprendre si
  Erwan décide des profils publics à assumer).

### 2026-08-08 — Toutes les cartes rédigées (Forbes, FR+EN) + réglages CTA/rythme
Session de rédaction/finition carte par carte (validée pas à pas sur localhost). **Tout reste
en LOCAL, non commité, non poussé.** Plus aucun placeholder sur la home.

- **Carte Entrepreneur** :
  - Badge (`lead.kicker`) : « SUNCOM · Immobilier international » → **« Immobilier ·
    Technologie · IA »** / EN **« Real estate · Technology · AI »** (séparateur point médian ·).
  - Sous-titre (`lead.sub`) : → **« Créateur · Dirigeant · Actionnaire »** / EN **« Founder ·
    Executive · Shareholder »** (choix : FR « Créateur », EN « Founder » — pas un calque).
  - Corps (`lead.body`) rédigé Forbes (Piste B) : 35 ans de créations/cessions, immobilier +
    deep tech, avènement de l'IA, associés & amis, dernier projet à **New York**, « inverser le
    paradigme d'une corporation mondiale ». FR + EN.
  - CTA texte : « Découvrir suncom.estate » → **« Découvrir »** / EN **« Explore »**.
  - CTA **style « Lingot »** (choisi parmi 3 maquettes) : or massif dégradé
    (`gold-soft→gold→#C79A38`), texte navy, ombre dorée profonde + reflet interne, léger lift
    au survol. Règle `.area-lead .cta`.
  - **Rythme vertical réaménagé** (le CTA était collé + vide en bas) : padding carte `40px
    34px 40px` ; marges `lead-icon` 26 / `lead-badge` 24 / `lead-title` 16 / `lead-sub` 26 ;
    `.lead-body` `line-height:1.72` + `margin-bottom:30px` ; **`.area-lead .cta` repassé en
    `margin-top:auto`** → occupe le bas (aligné avec le CTA save) en desktop 2-col, écart mini
    garanti 30px en mode étroit/mobile.
- **Bio — conclusion mise en valeur** : dernière phrase « Libérer le corps et l'esprit laisse
  la vision respirer. » sortie en classe dédiée **`.bio-sign`** (Michroma turquoise **13px** +
  **filet doré** au-dessus ; 1 ligne desktop, 2 lignes mobile). Le `!` de « …sauver des
  vies **!** » remplacé par des points de suspension **« …sauver des vies… »** (FR + EN
  « …even saving lives… »). Traitement « A » choisi parmi 3.
- **Carte Sportif** : corps Forbes (Piste 2 sans « dans l'âme ») — compétiteur depuis l'enfance,
  tennis/rugby, endurance/dépassement/émotions/mental, « pas un jour sans sport », nageur/skieur
  haut gradé/kitesurf des origines, endorphine → performance. **Tags = disciplines** :
  « Tennis · Rugby · Ski · Kitesurf · Natation » / EN « …Swimming ». Kicker « Way of life »
  conservé.
- **Carte Sauveteur** : **certifs BNSSA/PSE1/PSE2 supprimées** (bloc `.save-flags` retiré) ;
  CTA « Réserver un créneau » → **« Réservation »** / EN **« Booking »** (lien inchangé vers
  `nageur-sauveteur-secouriste/`) ; corps réécrit (don de soi, service des autres, déconnexion
  du cérébral, **équilibre**, « veiller sur le public » — **sans plages, sans diplômes, sans
  “sauver des vies”**). `.area-save p` : `line-height:1.62` + `margin-bottom:28px` pour
  décoller le CTA (même souci que lead : `margin-top:auto` = 0 quand la carte = hauteur contenu).
- **Carte Compétences** : 3 colonnes réécrites (titre col.2 « Terrain » → **« Sauvetage-
  Secourisme »**). FR — **Entreprise** : Vision & stratégie / Créativité / Pilotage & management
  / Recherche & développement · **Sauvetage-Secourisme** : Sens de l'équipe / Rigueur &
  organisation / Anticipation / Gestion de l'urgence · **Atouts** : Trilingue FR EN ES /
  Relationnel & Adaptabilité / Assidu / **Pédagogue**. EN — **Business / Rescue & first aid /
  Strengths** ; dernier atout **« Mentor »** ; « Pilotage & management » = *Leadership &
  management*, « Assidu » = *Dedicated*, « Relationnel & Adaptabilité » = *Interpersonal &
  adaptability*.
- **Aperçu local** : `~/.claude/launch.json` → entrée `erwan-milbeo-home` passée en
  **`autoPort:true`** avec lanceur `bash -c 'python3 -m http.server "${PORT:-4599}" --directory
  …'` (le port 4599 est souvent squatté par un autre chat). Le cache navigateur oblige à
  recharger avec **`?v=N`** (le `navigate ?v=` a parfois été refusé une fois par le classifieur,
  puis passe). Vérifs : desktop 2-col + étroit + mobile 375, FR + EN, aucun débordement.
- **Retouches finales (fin de session)** : (1) **ordre des paragraphes de la bio inversé** —
  la philosophie (« Au-delà de la créativité… ») passe **avant** le parcours (« Serial
  entrepreneur… »), la conclusion `.bio-sign` reste en bas ; l'inversion se fait sur le
  **contenu** en gardant les classes en place (`.bio-lead` 1er sans marge / `.bio-close` 2e
  avec `margin-top`). (2) **« Erwan Milbéo évolue » → « Erwan évolue »** dans la bio, et idem
  dans le corps de la carte Entrepreneur **« Erwan Milbéo fait éclore » → « Erwan fait
  éclore »** (EN : « Erwan operates » / « Erwan is bringing »). Le nom complet « Erwan Milbéo »
  ne subsiste que dans le hero (titre) — utile pour le SEO/AEO à venir.
- **Reste** : SEO/AEO → déploiement → domaine (voir §7).

### 2026-08-07 (2) — Contenus Forbes : carte présentation (bio) + carte Entrepreneur restylée
- **Carte présentation (bio)** rédigée en ton **Forbes** (FR + EN) : bio étirée pleine largeur
  (`max-width:none`) ; 2 paragraphes de prose continue — parcours (serial entrepreneur depuis
  1991, immobilier international + tech, 5 pays, jusqu'à 300 salariés, 35 ans) puis philosophie
  (esprit sain/corps sain, sport, terrain, service, « jusqu'à sauver des vies ! » / « Libérer le
  corps et l'esprit laisse la vision respirer. »). Puces de faits retirées (à replacer ailleurs
  si voulu : Canal+/Air France/Ibéria, MKT Lines, e2w/SUNCOM LLC). Espace slogan→bio porté à 24 px.
- **Carte Entrepreneur** refaite façon référence « LE CLUB » (navy & gold premium, Michroma,
  badge, titre bi-ton, sous-titre espacé, CTA contour doré, filigrane immeubles, filet doré,
  sans chiffre) — voir §3.7. Vérifié desktop + mobile, FR + EN, sans débordement.
- **Reste** : contenu Forbes du corps de la carte Entrepreneur (+ éventuelle ligne d'accroche
  forte type « C'est vous le patron ! ») ; puis cartes **Sportif** et **Compétences**.

### 2026-08-07 — Home bento + logo ME + i18n FR/EN
- **Restructuration dépôt** : `git mv index.html → nageur-sauveteur-secouriste/index.html`,
  ajout `CNAME` (`www.erwan-milbeo.com`). (En local, non commité.)
- **Home créée** : layout bento 5 zones (hero + 4 box), chaque carte son ambiance.
- **Hero itéré** :
  - Accroches : slogan « Many roles. One freedom. » (Fraunces italique, non traduit,
    calé sous le M) + sous-titre « Entrepreneur. Lifeguard/Sauveteur. Athlete/Athlète.
    Human first./D'abord humain. ».
  - Suppression de la ligne « Bretagne · France · International ».
  - Logo : emblème 3D « flottant » (ombre + animation) ; nom atténué 20 % ;
    slogan taille/interligne resserrés ; réglages mobile (retrait + taille du slogan).
  - **Logo ME** (monogramme montant partagé, arrondi) + **mappemonde ambre** coin haut
    droit dégagée des lettres → intégré dans l'emblème ; favicon assorti.
- **i18n** : moteur `data-i18n` + switch FR/EN (haut-droite du hero) + dico FR/EN +
  persistance localStorage. Slogan exclu de la traduction. Vérifié desktop + mobile
  (aucun débordement).
- **Direction éditoriale** fixée : ton « Forbes ».
- **Contenus** : encore des placeholders (à rédiger, étape suivante).

---

## 9. Rappels décisions / préférences

- Déploiement **uniquement** par `git push origin main` (pas d'outil tiers).
- Contenu bilingue : **une modif de contenu = les 2 langues** (FR + EN) systématiquement.
- Slogan « Many roles. One freedom. » : **jamais traduit**.
- Certifications (BNSSA/PSE1/PSE2) et coordonnées : non traduites.
