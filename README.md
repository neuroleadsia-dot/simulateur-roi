# Simulateur ROI IA — Neuroleads

Simulateur interactif de retour sur investissement pour le déploiement de solutions IA en entreprise.

**Démo live :** https://www.neuroleads.fr/simulateur-roi/

---

## Ce que contient ce repo

| Fichier | Rôle |
|---|---|
| `index.html` | Le simulateur complet, **un seul fichier autonome** (HTML + CSS + JS embarqués). C'est le seul fichier à héberger. |
| `embed.html` | Exemple d'intégration par iframe (copier-coller dans une page existante de neuroleads.fr). |
| `README.md` | Ce guide. |

**Stack :** HTML5, Tailwind CSS (CDN), Chart.js (CDN), vanilla JS. Aucun build, aucune dépendance npm. S'héberge sur n'importe quel hébergement statique (OVH, Netlify, Vercel, GitHub Pages).

**Contenu :** 46 cas d'usage IA répartis sur 10 départements (Transverse, Commercial, Marketing, Finance, RH, Juridique, Supply Chain, IT/DSI, Service Client, Production).

---

## Déploiement sur OVH (recommandé pour neuroleads.fr)

### Option A — Page dédiée (la plus propre)

1. Connectez-vous à votre espace OVH → **Hébergements** → **FTP-SSH**.
2. Créez un dossier `simulateur-roi/` à la racine de votre hébergement (à côté de `index.html` du site principal).
3. Uploadez **uniquement** `index.html` dans ce dossier, en le renommant ou non.
4. Le simulateur est accessible à l'URL **https://www.neuroleads.fr/simulateur-roi/**.
5. Ajoutez un lien vers cette page depuis la navigation de neuroleads.fr.

### Option B — Embed iframe dans une page existante

Si vous voulez afficher le simulateur à l'intérieur d'une page existante de neuroleads.fr (par exemple sur la home entre la section "Problème" et la section "Méthode") :

1. Déployez `index.html` comme dans l'option A.
2. Sur la page existante où vous voulez l'afficher, collez ce snippet à l'emplacement souhaité :

```html
<iframe
  id="neuroleads-simulator"
  src="/simulateur-roi/"
  title="Simulateur ROI IA"
  style="width:100%; border:none; min-height:800px; background:#0A0E27; border-radius:16px;"
  loading="lazy"
  scrolling="no"
></iframe>

<script>
  window.addEventListener('message', function (event) {
    if (event.data && event.data.type === 'neuroleads-simulator-height') {
      document.getElementById('neuroleads-simulator').style.height = event.data.height + 'px';
    }
  });
</script>
```

Le simulateur envoie sa hauteur en continu via `postMessage`, donc l'iframe s'adapte automatiquement sans scrollbar interne.

Voir `embed.html` pour un exemple complet.

### Option C — Déploiement via GitHub Pages (pour preview)

1. Pushez ce repo sur GitHub.
2. **Settings → Pages → Branch: `main` / `root` → Save.**
3. URL de preview : `https://<votre-user>.github.io/<nom-du-repo>/`.
4. Une fois validé, basculez sur OVH selon l'option A.

---

## Workflow Git recommandé

```bash
# 1. Initialiser le repo
cd simulateur-roi/
git init
git add .
git commit -m "Initial commit — simulateur ROI IA Neuroleads"

# 2. Créer le repo sur GitHub puis push
git remote add origin https://github.com/<votre-user>/neuroleads-simulateur-roi.git
git branch -M main
git push -u origin main

# 3. À chaque mise à jour
git add index.html
git commit -m "Ajout de 3 nouveaux cas d'usage Finance"
git push
```

---

## Personnalisation rapide

### Ajouter / modifier un cas d'usage

Ouvrez `index.html`, cherchez `const USECASES = {` (vers la ligne ~370). Chaque cas d'usage est un objet :

```js
{
  id: 'c6',                                    // identifiant unique
  name: 'Nom du cas d\'usage',
  desc: 'Description courte (1 phrase).',
  type: 'agent',                               // 'agent' ou 'auto'
  hoursPerWeek: 5,                             // heures/semaine économisées par collaborateur
  automation: 0.75,                            // taux d'automatisation (0 à 1)
  complexity: 2                                // 1 Quick Win | 2 Standard | 3 Stratégique
}
```

### Modifier le CTA (bouton final)

Cherchez `https://www.neuroleads.fr/#contact` dans `index.html` et remplacez par l'URL de votre choix (formulaire de contact, Cal.com, Calendly, etc.).

### Changer le code couleur

Les couleurs sont dans le `<script>` de config Tailwind en tête de fichier. Par défaut :
- `accent` (indigo) : `#6366F1`
- `mint` (turquoise) : `#14B8A6`
- `gold` : `#F59E0B`
- `bg` (fond) : `#0A0E27`

### Modifier les hypothèses de calcul

Dans `updateResults()`, recherchez `* 46` (nombre de semaines travaillées/an) ou la projection 3 ans `[yearlyMoney * 0.7, * 1.7, * 2.8]` (courbe d'adoption progressive).

---

## Modèle de calcul

Pour chaque cas d'usage sélectionné :

```
heures_économisées_semaine = hoursPerWeek × automation
total_équipe_semaine       = Σ heures_économisées_semaine × nombre_collaborateurs
total_an_heures            = total_équipe_semaine × 46 semaines
total_an_euros             = total_an_heures × coût_horaire
```

La complexité affichée est la moyenne arrondie des complexités des cas sélectionnés, mappée sur :
- 1 → Quick Win (4–8 semaines)
- 2 → Standard (2–4 mois)
- 3 → Stratégique (4–6 mois)

La projection 3 ans applique une courbe d'adoption : 70 % année 1, +100 % année 2, +110 % année 3 (cumulé).

---

## Checklist de mise en ligne sur neuroleads.fr

- [ ] Uploader `index.html` dans `/simulateur-roi/` sur l'hébergement OVH
- [ ] Vérifier l'URL `https://www.neuroleads.fr/simulateur-roi/` (desktop + mobile)
- [ ] Ajouter un lien vers le simulateur dans la navigation principale
- [ ] Vérifier que le CTA final pointe vers le bon formulaire / Cal.com
- [ ] Tester sur Safari, Chrome, Firefox
- [ ] Déclarer la page dans Google Search Console

---

## Licence

Propriétaire — Neuroleads © 2026. Réutilisation interdite sans autorisation.
