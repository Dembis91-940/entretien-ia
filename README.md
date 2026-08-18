# Entretien IA

**Un entretien structuré, noté sans biais, en 30 minutes.**

Checklist d'entretien d'embauche propulsée par IA pour solopreneur — landing de vente + outil web réel de génération de questions et de notation, 100 % local.

## Offres

| Offre | Prix | Contenu |
|---|---|---|
| **La Checklist** | 19 € | Checklist PDF complète (35 points) + 5 grilles de notation 1-5 à imprimer + méthode STAR + questions interdites |
| **Le Kit Complet** ⭐ Le plus choisi | 39 € | Tout de la Checklist + générateur de questions par poste (outil web, 8 postes) + templates d'emails (convocation, relance, refus) + scripts d'ouverture/clôture |
| **Le Pack RH** | 79 € | Tout du Kit Complet + guide anti-biais (12 biais + protocole) + formation recruteurs (7 modules) + grille de comparaison multi-candidats |

Paiement par virement ou message privé, confirmé par email ; livraison des fichiers sous 24-48 h ouvrées. Garantie 14 jours satisfait ou remboursé.

## Fichiers

| Fichier | Rôle |
|---|---|
| `index.html` | Landing de vente : hero, 3 douleurs, 3 étapes (préparer → interviewer → évaluer), 3 offres, formulaire de commande EmailJS, FAQ (8 questions), footer. Design grenat `#800020` / beige sable `#f0e6d6` / or clair `#c9a227`, Playfair Display, JSON-LD (Product + FAQPage), OG tags, reveal au scroll, chatbot intégré. |
| `outil-entretien.html` | **Outil réel, 100 % local** : 8 postes (commercial, développeur, support, marketing, RH, manager, alternant, data) → 15 questions générées par compétence (savoir-faire, savoir-être, motivation), grille de notation 1-5 colorée, score par groupe + moyenne globale, verdict à 4 niveaux, checklist anti-biais (7 vérifications), minuteur 30 minutes, notes libres par question, export PDF du compte-rendu. Persistance localStorage. Aucune donnée envoyée sur Internet. |
| `checklist-entretien.md` | Le produit livrable (converti en PDF) : checklist de préparation en 12 points, déroulé 30 min minute par minute, méthode STAR, 7 vérifications anti-biais, questions interdites (cadre légal), 5 grilles de notation à photocopier, 15 questions types, 10 règles d'or. |
| `chatbot.js` / `chatbot-config.js` | Widget chatbot (pattern ai-course-builder) : accent grenat `#800020`, welcome et 8 FAQs spécifiques au business, capture de leads EmailJS. |
| `README.md` | Ce fichier. |

## EmailJS (commande réelle — zéro simulateur)

- **Service ID :** `service_cy1ytdb`
- **Template ID :** `template_xpo58cv`
- **Clé publique :** `8Pui4ZEqxW2jRVF7h`
- **Payload :** `{ site: "Entretien IA", name, email, question }` — `question` = récapitulatif complet de la commande (« Commande : Kit Complet (39 €) | Besoin : … »).

Le SDK EmailJS est chargé à la demande dans le handler de soumission (`chargerEmailJS`) : si `window.emailjs` n'existe pas, le CDN `@emailjs/browser@4` est injecté puis `emailjs.init` appelé avant l'envoi. Le formulaire fonctionne donc même si le chatbot n'a pas chargé le SDK. `emailjs.init` au chargement est gardé par `if (window.emailjs)` pour ne pas casser la page hors-ligne.

Le chatbot (chatbot-config.js) branche la même capture de leads : toute question sans réponse prédéfinie déclenche un formulaire nom + email → envoi EmailJS + sauvegarde locale.

## Outil — logique métier

- **8 postes × 15 questions**, chacune rattachée à un groupe (Savoir-faire `sf`, Savoir-être `se`, Motivation `mo`) et à un critère nommé.
- **Notation 1-5** par question (select coloré) ; moyenne par groupe et moyenne globale affichées en direct.
- **Verdict à 4 niveaux** (≥ 4,2 retenir · 3,4-4,1 approfondir · 2,6-3,3 prudent · < 2,6 écarter), affiché seulement quand les 15 critères sont notés ; score partiel signalé sinon.
- **Checklist anti-biais 7/7** : alerte si incomplète, bloquante pour la décision (affichée dans le rapport PDF).
- **Export PDF** : div `#print-report` construit à la volée (`beforeprint` + clic) → `window.print()` ; masque nav, contrôles et chatbot à l'impression.
- **Minuteur 30 min** : compte à rebours avec pause/réinitialisation, bip WebAudio à zéro, réinitialisé à chaque génération d'entretien.
- **Persistance** : `localStorage` clé `entretien-ia-v1` (poste, candidat, notes, cotes, anti-biais) — reprise automatique au rechargement.

## Déploiement

Statique, aucun build : héberger `index.html`, `outil-entretien.html`, `chatbot.js`, `chatbot-config.js` à la racine (GitHub Pages, Netlify, Vercel…). Aucune API serveur requise ; la seule dépendance externe est le CDN EmailJS (chargé à la demande) et Google Fonts (avec fallback serif/sans).

## Vérifications effectuées

- Syntaxe JS des blocs inline validée (Node) ; JSON-LD parsable.
- Test navigateur end-to-end : génération des 15 questions pour les 8 postes, notation complète → score/verdict, checklist anti-biais, rapport PDF.
- Formulaire EmailJS câblé sur les vrais identifiants (aucun envoi de test déclenché — règle « zéro faux lead »).
- Orthographe française relue (aucune faute connue).

*© 2026 Entretien IA — les grilles d'évaluation aident à la décision mais ne se substituent pas à un avis juridique.*
