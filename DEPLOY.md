# Guide de Déploiement - BioMarket Insights

## 🎯 Problème Résolu

Le site affichait "Failed to fetch" sur GitHub Pages car l'URL de l'API était codée en dur vers `http://localhost:3000`.

## ✅ Solution Implémentée

Le site détecte maintenant automatiquement l'environnement et :
- **En local** : utilise l'API Node.js sur `localhost:3000`
- **En production (GitHub Pages)** : fonctionne en mode démonstration avec des données d'exemple

## 📦 Fichiers Modifiés

1. **`config.js`** (nouveau) : Configuration dynamique qui détecte l'environnement
2. **`resultat.js`** : Utilise la config et génère des données de démo si l'API n'est pas disponible
3. **`results.html`** : Inclut le fichier `config.js`
4. **`resultat.css`** : Style pour le bandeau d'avertissement en mode démo

## 🚀 Déploiement sur GitHub Pages

### Option 1 : Mode Démonstration (Actuel)

Le site fonctionne immédiatement sur GitHub Pages avec des données d'exemple.

**Étapes :**
1. Commitez tous les fichiers modifiés :
   ```bash
   git add .
   git commit -m "Fix: Configuration dynamique pour GitHub Pages"
   git push origin main
   ```

2. Activez GitHub Pages :
   - Allez dans **Settings** → **Pages**
   - Source : `main` branch
   - Dossier : `/ (root)`
   - Cliquez sur **Save**

3. Votre site sera accessible à : `https://mpinguet.github.io/hackacthon_site/`

### Option 2 : Avec API Réelle (Recommandé pour Production)

Pour utiliser l'analyse IA réelle en production, vous devez déployer l'API séparément.

**Services recommandés pour déployer l'API Node.js :**

1. **Render** (Gratuit) :
   - https://render.com
   - Déployez le dossier `AI/` comme Web Service
   - Copiez l'URL fournie (ex: `https://votre-app.onrender.com`)

2. **Railway** (Gratuit) :
   - https://railway.app
   - Connectez votre repo GitHub
   - Sélectionnez le dossier `AI/`
   - Copiez l'URL du déploiement

3. **Heroku** (Payant) :
   - https://heroku.com
   - Créez une nouvelle app
   - Déployez le contenu du dossier `AI/`

**Configuration après déploiement de l'API :**

1. Ouvrez `config.js`
2. Modifiez la ligne `production: ''` avec votre URL d'API :
   ```javascript
   production: 'https://votre-api.onrender.com'  // Remplacez par votre URL
   ```
3. Committez et poussez les changements
4. Le site utilisera automatiquement l'API réelle en production

## 🧪 Test en Local

```bash
# Dans le dossier AI/
cd AI
./start.sh

# Le site sera accessible sur http://localhost:3000/index.html
```

## 📋 Variables d'Environnement pour l'API

Si vous déployez l'API, configurez ces variables d'environnement :

```
PORT=3000
OLLAMA_URL=http://localhost:11434/api/generate  # URL de votre instance Ollama
NODE_ENV=production
```

## 🔧 Configuration CORS

Si vous déployez l'API séparément, assurez-vous que le CORS est configuré pour accepter les requêtes depuis GitHub Pages.

Dans `AI/server.js`, vérifiez que cette ligne existe :
```javascript
app.use(cors({
  origin: ['https://mpinguet.github.io', 'http://localhost:3000'],
  credentials: true
}));
```

## 📱 Tester le Déploiement

1. **Mode Démo** : Visitez votre GitHub Pages → devrait afficher un bandeau jaune "Mode démonstration"
2. **Avec API** : Les rapports seront générés par l'IA, pas de bandeau de démo

## ⚠️ Notes Importantes

- **Mode démo** : Les données sont des exemples statiques, pas de vraie analyse
- **CORS** : L'API doit autoriser les requêtes depuis votre domaine GitHub Pages
- **Performance** : L'analyse IA peut prendre 20-30 secondes
- **Coûts** : Render/Railway ont des tiers gratuits avec limitations

## 🆘 Dépannage

### "Failed to fetch" persiste
- Vérifiez que `config.js` est bien inclus dans `results.html`
- Videz le cache du navigateur (Ctrl+Shift+R)
- Vérifiez la console du navigateur pour les erreurs

### Le bandeau de démo ne s'affiche pas
- Normal si vous avez configuré une URL d'API en production
- Vérifiez `CONFIG.USE_DEMO_MODE` dans la console

### L'API ne répond pas
- Vérifiez que l'API est déployée et accessible
- Testez l'URL directement : `https://votre-api.com/api/health`
- Vérifiez les logs du service de déploiement

## 📞 Support

En cas de problème, vérifiez :
1. Les logs de la console navigateur (F12)
2. Le statut de GitHub Pages dans Settings
3. Les logs de votre service d'API (si déployée)
