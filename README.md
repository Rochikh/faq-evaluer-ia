# FAQ Évaluer en formation à l'ère de l'IA

App web installable (PWA) pour consulter rapidement la banque d'objections en conférence ou en entretien.

Adossée au livre *Évaluer en formation à l'ère de l'IA générative*, Rochane Kherbouche, Chronique Sociale, 2026.

---

## Déploiement complet (GitHub + Netlify, ~10 minutes)

### Étape 1 — Créer le repo GitHub

1. Va sur https://github.com/new
2. Nom du repo : `faq-evaluer-ia` (ou autre)
3. Public ou privé, peu importe
4. Coche "Add a README file" pour initialiser le repo
5. Clique "Create repository"
6. Sur la page du repo, clique "Add file" → "Upload files"
7. Glisse-dépose tous les fichiers de ce dossier (sauf ce README si tu veux garder le tien)
8. Scrolle en bas, "Commit changes"

### Étape 2 — Connecter Netlify au repo

1. Va sur https://app.netlify.com
2. Connecte-toi avec ton compte GitHub
3. "Add new site" → "Import an existing project" → "Deploy with GitHub"
4. Autorise Netlify à accéder à ton repo
5. Sélectionne le repo `faq-evaluer-ia`
6. **Build settings** : laisse tout vide (c'est un site statique)
   - Branch to deploy : `main`
   - Build command : (vide)
   - Publish directory : (vide ou `./`)
7. "Deploy site"

Netlify déploie en 30 secondes. Tu obtiens une URL du type `https://random-name-12345.netlify.app`.

### Étape 3 — Renommer le site

1. Dans le tableau de bord Netlify, clique sur ton site
2. "Site configuration" → "Change site name"
3. Mets `faq-evaluer-ia` (si dispo) ou autre
4. Ton URL devient `https://faq-evaluer-ia.netlify.app`

### Étape 4 — Installer sur Android

1. Ouvre l'URL dans **Chrome Android** (pas Firefox, pas Samsung Internet pour la première fois)
2. Attends quelques secondes que la PWA soit détectée
3. Menu trois points en haut à droite
4. "Installer l'application" (ou "Ajouter à l'écran d'accueil" si l'installation PWA n'apparaît pas)
5. Valide

L'icône (un "?" italique blanc sur fond rouge brique) apparaît dans le tiroir d'apps et sur l'écran d'accueil. Tap pour lancer en plein écran, sans la barre Chrome.

### Étape 5 — Installer sur ordinateur (optionnel)

Dans Chrome desktop sur la même URL, une icône d'installation apparaît à droite de la barre d'adresse (un petit "+" ou un écran avec flèche). Clique dessus pour installer comme app desktop avec sa propre fenêtre.

---

## Mettre à jour le contenu plus tard

1. Modifie le fichier `index.html` en local (ou directement sur l'interface web GitHub)
2. Pour modifier les questions/réponses : cherche la constante `DATA = [` dans `index.html` et édite les objets
3. Commit/push sur GitHub
4. Netlify redéploie automatiquement en 30 secondes
5. La PWA installée se met à jour au prochain lancement (parfois après deux lancements à cause du cache du service worker)

Pour forcer une mise à jour immédiate, incrémente la constante `CACHE` dans `sw.js` (par exemple `faq-evaluer-ia-v1` → `faq-evaluer-ia-v2`). Le service worker invalidera l'ancien cache.

---

## Structure des fichiers

| Fichier | Rôle |
|---------|------|
| `index.html` | Application complète (HTML + CSS + JS + données) |
| `manifest.json` | Configuration PWA (nom, icônes, couleurs) |
| `sw.js` | Service worker (offline + installation) |
| `icon-192.png` | Icône PWA standard |
| `icon-512.png` | Icône PWA haute résolution |
| `favicon-32.png` | Favicon onglet navigateur |
| `README.md` | Ce fichier |

---

## Ajouter ou modifier une question

Dans `index.html`, cherche le tableau `DATA`. Chaque entrée suit ce format :

```javascript
{
  id: '11.1',                          // Famille.Numéro
  famille: 'Nom complet de la famille',
  famille_short: 'Nom court',          // Apparaît dans les chips
  objection: "La question telle qu'elle est posée en conférence",
  presuppose: "Le présupposé piégé identifié",
  pivot: "La réponse pivot en 2-3 phrases denses",
  donnee: "Donnée chiffrée ou étude",  // Optionnel
  exemple: "Cas concret du manuscrit",  // Optionnel
  relance: "Question retour qui retourne l'objection",  // Optionnel
  mots_cles: ['mot1', 'mot2', 'expression composée']  // Pour la recherche
}
```

Les mots-clés alimentent la recherche fuzzy. Plus ils sont précis et variés (synonymes, fautes courantes, expressions familières), plus la recherche tombe juste sous stress en conférence.

---

## Fonctionnement hors-ligne

Une fois la PWA installée et lancée au moins une fois avec internet, tout le contenu (fichiers + polices Google Fonts) est mis en cache par le service worker. L'app fonctionne ensuite sans connexion, en avion, en sous-sol, dans n'importe quelle salle de conférence mal couverte.

---

## Licence

Contenu de la FAQ : propriété de l'auteur, adossée au livre.
Code de l'app : libre d'usage personnel.
