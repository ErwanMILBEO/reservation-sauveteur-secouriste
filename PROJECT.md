# PROJECT.md — reservation-sauveteur-secouriste

> Source de vérité du projet. À lire au début de **chaque** session, avant toute action.
> À tenir à jour à chaque évolution structurante (fonctionnalité, stack, URL, dépôt).

## 1. Objectif

Page web publique unique permettant à Erwan Milbéo — nageur-sauveteur-secouriste
diplômé (BNSSA · PSE1 · PSE2, Finistère / Bretagne) — de :

- **présenter ses disponibilités** sous forme d'un calendrier mois par mois
  (jours *disponibles* vs *déjà réservés*) ;
- **recevoir des demandes de réservation** : le visiteur sélectionne un ou
  plusieurs jours libres, remplit un court formulaire, et la demande part
  **par e-mail** (ouverture de Gmail dans un nouvel onglet + solutions de
  repli `mailto:` et copier-coller). Aucun back-end, aucune base de données.

Public visé : piscines, thalassothérapies, lacs/étangs, centres de loisirs,
campings, compétitions sportives, événements/concerts/festivals cherchant un
remplaçant surveillant de baignade. Contrats salariés ou auto-entrepreneur.

## 2. Stack & architecture

- **Un seul fichier : `index.html`** à la racine. HTML + CSS + JS **inline**,
  aucune dépendance de build, aucun framework.
- Polices via Google Fonts (`Bebas Neue`, `IBM Plex Mono`, `Inter`).
- Calendrier généré en JS à partir du tableau `monthsData` (année, index de
  mois, libellé, liste des jours `booked`). Les jours non-`booked` sont
  cliquables et alimentent un « panier » de dates (`selectedDates`).
- Envoi de la demande : construction d'un `subject`/`body`, ouverture de
  `https://mail.google.com/mail/?view=cm...` + liens `mailto:` de repli.
- **Contact de destination** : constante `CONTACT_EMAIL` en tête de `<script>`
  (actuellement `em.officiel@pm.me`). Coordonnées affichées en pied de page :
  tél. `06 50 87 68 87`, e-mail `em.officiel@pm.me`.

## 3. Déploiement

- **Hébergement : GitHub Pages** (source : branche `main`, dossier racine `/`).
- URL de production : <https://erwanmilbeo.github.io/reservation-sauveteur-secouriste/>
- HTTPS forcé. Pas de domaine personnalisé (CNAME) pour l'instant.
- **Process de déploiement : `git push origin main`** → GitHub Pages
  reconstruit automatiquement. Rien à activer à la main.

## 4. Design system (référence — ne pas modifier sans demande explicite)

Univers visuel « sauvetage aquatique » : bleu marine profond, turquoise piscine,
touches sable/or, drapeaux de plage (vert = ok, rouge = interdit).

**Couleurs** (variables CSS `:root`) :

| Variable          | Valeur    | Usage |
|-------------------|-----------|-------|
| `--marine`        | `#0A2A3B` | fond hero, panier fixe, boutons principaux |
| `--marine-2`      | `#0F3A50` | dégradé hero |
| `--foam`          | `#F5FAF9` | fond de page, texte sur marine |
| `--pool`          | `#147D86` | turquoise d'accent, focus, liens |
| `--pool-light`    | `#DCEEEF` | fonds doux (encadrés, tags) |
| `--flag-green`    | `#2E8B57` | drapeau « disponible » |
| `--flag-red`      | `#C1432E` | drapeau « indisponible » |
| `--sand`          | `#E8C468` | accent doré (titre, CTA panier) |
| `--ink`           | `#16323F` | texte courant |
| `--mist`          | `#7C9399` | texte secondaire / labels |
| `--line`          | `#D7E3E4` | bordures |

**Typographies** :
- `Bebas Neue` → titres (`h1/h2/h3`), condensé, capitales.
- `IBM Plex Mono` → labels, badges, données, boutons (aspect « technique »).
- `Inter` → texte courant et champs de formulaire.

**Composants clés** : hero à dégradé radial + bouée/casque décoratifs ;
badges de diplômes ; légende ; grille calendrier (cellules `aspect-ratio:1/1`,
drapeau + numéro) ; panier fixe bas d'écran (`.sticky-cart`) ; panneau de
réservation (`#booking-panel`) ; bloc de confirmation (`#confirmation-box`).

**Responsive** : point de rupture unique `@media (max-width:560px)` regroupant
tous les ajustements mobiles. Le desktop ne doit jamais être impacté par un
correctif mobile (et inversement).

## 5. État d'avancement

- ✅ Page fonctionnelle en production sur GitHub Pages.
- ✅ Calendrier août 2026 → mars 2027 ; agenda « ouvert » à partir d'avril 2027.
- ✅ Affichage mobile audité et corrigé (360 / 390 / 414 px) — voir HANDOFF.md.

## 6. Points ouverts / à surveiller

- Les jours `booked` sont saisis **manuellement** dans `monthsData` : à mettre
  à jour à chaque nouvelle réservation confirmée.
- `CONTACT_EMAIL` et l'e-mail du pied de page doivent rester cohérents et
  pointer vers une boîte réellement relevée par Erwan.
- Pas de suivi analytique ni de confirmation côté serveur (choix assumé : outil
  léger, sans back-end).
