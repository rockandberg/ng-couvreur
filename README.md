# NG Couvreur — site vitrine

Couvreur à Propriano, Corse-du-Sud · 07 44 81 79 45

**Site :** https://ngcouvreur.fr

## Organisation

| Chemin | Rôle |
|---|---|
| `build/build.py` | Générateur. Source unique : navigation, prestations, communes, conseils, SEO, sitemap. |
| `build/site.css` · `build/site.js` | Système graphique et interactions. |
| `build/optimize-images.py` | Fabrique les dérivés web (AVIF/WebP/JPEG) depuis la banque d'images. |
| `build/verify-images.py` | Recoupe chaque image référencée avec le disque. Bloquant en CI. |
| `site/` | Site généré — **publié tel quel**. Ne pas éditer à la main. |
| `docs/` | Bible visuelle, shot list, prompts, manifeste des visuels. |

## Fabriquer localement

```bash
python3 -m pip install Pillow pillow-avif-plugin
python3 build/optimize-images.py   # visuels → site/assets/img/
python3 build/build.py             # pages HTML
python3 build/verify-images.py     # contrôle
```

L'ordre compte : `build.py` préserve `site/assets/img/` et `site/assets/icon/`,
mais efface tout le reste de `site/`.

## Déploiement

`.github/workflows/deploy.yml` publie **le dossier `site/`** sur GitHub Pages à
chaque poussée sur `main`. Les sources de fabrication et le cache des originaux
ne sont donc jamais publiés.

Le domaine est écrit dans `site/CNAME`, dérivé de `NG_SITE_ORIGIN` : une seule
source de vérité.

## Modifier le contenu

| Pour… | Éditer dans `build/build.py` |
|---|---|
| Retirer ou ajouter une prestation | la table `SERVICES` (§3) — répercuté partout |
| Ajouter une commune | la table `COMMUNES` (§4) ; renseigner `page` crée une page locale |
| Ajouter un conseil | la table `CONSEILS` (§4) |
| Changer un texte alternatif | la table `ALT` (§2) |
| Recadrer un visuel | la table `FOCUS` — `object-position` par image |
| Changer la boîte de réception | `FORM_TO` (§13) |
| Activer le captcha FormSubmit | `FORM_CAPTCHA = True` (§13) — si du spam passe |

## Reste à renseigner

1. **Mentions légales** — SIRET, forme juridique, siège, hébergeur, assurance
   décennale et médiateur sont marqués « à compléter ».
2. **Formulaire** — relié à FormSubmit. La première soumission déclenche un
   e-mail d'activation à valider une fois, sans quoi rien n'est transmis.
   Remplacer ensuite `FORM_TO` par l'alias fourni par FormSubmit, pour que
   l'adresse n'apparaisse plus, même encodée.
