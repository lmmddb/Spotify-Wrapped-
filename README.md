# 🎵 Spotify Wrapped 2025 - Analyse & Vidéo Automatisée

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Manim](https://img.shields.io/badge/Manim-Community-orange.svg)](https://www.manim.community/)

> Créez votre propre **Spotify Wrapped** personnalisé avec analyse complète de vos données d'écoute et génération automatique de vidéo !

![Spotify Wrapped Demo](https://img.shields.io/badge/Status-Production%20Ready-brightgreen)

---

## 📋 Table des matières

- [Aperçu](#-aperçu)
- [Fonctionnalités](#-fonctionnalités)
- [Prérequis](#-prérequis)
- [Installation](#-installation)
- [Utilisation](#-utilisation)
- [Structure du projet](#-structure-du-projet)
- [Exemples de résultats](#-exemples-de-résultats)
- [Problèmes courants](#-problèmes-courants)

---

## 🎯 Aperçu

Ce projet vous permet de créer votre propre version de **Spotify Wrapped** avec :

1. **Analyse complète** de vos données Spotify
2. **Enrichissement automatique** via l'API Last.fm (genres, albums)
3. **Génération de vidéo** professionnelle avec Manim
4. **Statistiques temporelles** détaillées (heures, jours, mois préférés)

### 🎬 Résultat final

Une vidéo animée de 1-3 minutes contenant :
- 🏆 Top 5 chansons, artistes, albums, genres
- 📊 Statistiques temporelles (temps total, habitudes d'écoute)
- 📈 Graphiques mensuels et quotidiens
- 🎨 Design inspiré de Spotify

---

## ✨ Fonctionnalités

### 📊 Analyse des données
- ✅ Chargement automatique depuis Google Drive ou upload local
- ✅ Nettoyage intelligent (filtrage des skips < 15s)
- ✅ Détection des sons d'ambiance (pluie, ASMR, etc.)
- ✅ Calcul du nombre d'écoutes réelles

### 🎸 Enrichissement métadonnées
- ✅ Récupération automatique des **genres** via Last.fm
- ✅ Récupération automatique des **albums** via Last.fm
- ✅ Traitement parallèle (multi-threading) pour rapidité
- ✅ Mapping intelligent des genres (normalisation)

### ⏰ Statistiques temporelles
- ✅ Temps total d'écoute
- ✅ Moyenne quotidienne (heures/minutes)
- ✅ Écoutes par mois (graphique)
- ✅ Plages horaires préférées (matin/après-midi/soir)
- ✅ Heure de pointe
- ✅ Jour de la semaine préféré

### 🎬 Génération vidéo (Manim)
- ✅ Animations fluides et professionnelles
- ✅ Design aux couleurs Spotify (#1DB954)
- ✅ Graphiques dynamiques (barres, camembert)
- ✅ Plusieurs qualités disponibles (low, medium, HD, 4K)

---

## 🔧 Prérequis

### Logiciels requis
- **Python 3.8+**
- **Google Colab** (recommandé) ou environnement local
- **Compte Last.fm** (pour API gratuite)

### Données Spotify
Vous devez d'abord **télécharger vos données Spotify** :

1. Allez sur [Spotify Account Privacy](https://www.spotify.com/account/privacy/)
2. Sélectionnez **"Données de compte "**
3. Attendez 5-30 jours de recevoir vos données par email
4. Extrayez les fichiers `StreamingHistory_music_*.json`

---

## 📥 Installation

### Option 1 : Google Colab (Recommandé)

1. **Clonez le repository**
```bash
git clone https://github.com/lmmddb/Spotify-Wrapped-.git
```

2. **Ouvrez le notebook dans Colab**
   - Téléchargez `main_final.py` dans Colab
   - Ou créez un nouveau notebook et copiez le code

3. **Installez Manim** (une seule fois)
```python
# Cellule 1: Installation
!sudo apt update -qq
!sudo apt install libcairo2-dev ffmpeg texlive texlive-latex-extra texlive-fonts-extra texlive-latex-recommended texlive-science tipa libpango1.0-dev -qq
!pip install manim -qq
!pip install "ipython==7.34.0" -qq

# Redémarrer le kernel
import os
os.kill(os.getpid(), 9)
```

### Option 2 : Installation locale

```bash
# 1. Créer un environnement virtuel
python -m venv venv
source venv/bin/activate  # Sur Windows: venv\Scripts\activate

# 2. Installer les dépendances
pip install pandas requests manim ipywidgets

# 3. Installer ffmpeg (selon votre OS)
# Ubuntu/Debian:
sudo apt install ffmpeg

# macOS:
brew install ffmpeg

# Windows: Téléchargez depuis https://ffmpeg.org/
```

---

## 🚀 Utilisation

### Étape 1 : Obtenir votre clé API Last.fm

1. Créez un compte sur [Last.fm](https://www.last.fm/api/account/create)
2. Récupérez votre **API Key**
3. Remplacez dans le code :
```python
LASTFM_API_KEY = 'VOTRE_CLE_ICI'
```

### Étape 2 : Analyse des données

```python
# 1. Charger l'interface
main()

# 2. Charger vos fichiers Spotify via l'interface
# (Sélectionnez Google Drive ou Upload local)

# 3. Nettoyer les données
streaming_clean = clean_data(streaming)
streaming_clean = detect_and_remove_ambient(streaming_clean)

# 4. Enrichir avec genres & albums
songs_with_metadata = fetch_genres_and_albums(streaming_clean)

# 5. Générer les tops
tops = generate_tops(songs_with_metadata)

# 6. Calculer les stats temporelles
temporal_stats = calculate_temporal_stats(streaming_clean)

# 7. Afficher les résultats
display_results(tops)
display_temporal_stats(temporal_stats)

# 8. Sauvegarder pour Manim
json_file = save_results_with_temporal(tops, temporal_stats)
```

### Étape 3 : Générer la vidéo

```python
# Cellule 1: Définir la classe Manim
from manim import *
import json

DATA_FILE = '/content/drive/MyDrive/Spotify/spotify_wrapped_data.json'

class SpotifyWrappedComplete(Scene):
    # ... (copier le code complet)

# Cellule 2: Lancer la génération
%%manim -qm -v WARNING SpotifyWrappedComplete
```

### Options de qualité vidéo

| Option | Qualité | Temps de rendu | Taille fichier |
|--------|---------|----------------|----------------|
| `-ql` | Low (480p) | ~30 sec | ~5 MB |
| `-qm` | Medium (720p) | ~2-3 min | ~20 MB |
| `-qh` | High (1080p) | ~5-7 min | ~50 MB |
| `-qk` | 4K (2160p) | ~15-20 min | ~200 MB |

**Recommandation** : Utilisez `-qm` pour tester, puis `-qh` pour la version finale.

---

## 📁 Structure du projet

```
Spotify-Wrapped-/
│
├── main_final.py              # Script principal (analyse + vidéo)
├── README.md                  # Ce fichier
├── requirements.txt           # Dépendances Python
├── LICENSE                    # Licence MIT
│
├── examples/                  # Exemples de résultats
│   ├── sample_output.json     # Exemple de données générées
│   └── demo_video.mp4         # Vidéo exemple
```

---

## 📊 Exemples de résultats

### Statistiques générées

```
🎵 TOP 10 CHANSONS
═══════════════════════════════════════
1. Song Name          - Artist Name         (120 écoutes)
2. Another Song       - Another Artist      (98 écoutes)
...

📊 STATISTIQUES TEMPORELLES
═══════════════════════════════════════
🎵 TEMPS TOTAL : 487.5 heures
📅 PÉRIODE : 365 jours
⏱️ MOYENNE QUOTIDIENNE : 80.2 minutes

🏆 MOIS LE PLUS ACTIF : Juillet (52.3h)
☀️ MOMENT PRÉFÉRÉ : Afternoon (45.2%)
🕐 HEURE DE POINTE : 15h
📆 JOUR PRÉFÉRÉ : Samedi
```

### Fichier JSON généré

```json
{
  "top_songs": [...],
  "top_artists": [...],
  "top_albums": [...],
  "top_genres": [...],
  "stats": {
    "total_artists": 245,
    "total_songs": 1523,
    "top_genre": "Rap"
  },
  "temporal_stats": {
    "total_hours": 487.5,
    "avg_mins_per_day": 80.2,
    "top_month": {"name": "Juillet", "hours": 52.3},
    ...
  }
}
```

---

## 🐛 Problèmes courants

### ❌ `KeyError: 'SpotifyWrappedComplete'`

**Solution** : Définissez la classe dans une cellule, puis exécutez `%%manim` dans une cellule séparée.

```python
# Cellule 1
class SpotifyWrappedComplete(Scene):
    # ... code ...

# Cellule 2
%%manim -qm -v WARNING SpotifyWrappedComplete
```

### ❌ Erreur de chargement des fichiers

**Solution** : Vérifiez que vos fichiers suivent le format `StreamingHistory_music_*.json`

### ❌ API Last.fm ne répond pas

**Solutions** :
- Vérifiez votre clé API
- Attendez quelques minutes (rate limiting)
- Réduisez `TOP_N_ALBUMS` de 500 à 200

### ❌ Vidéo trop longue à générer

**Solution** : Utilisez `-ql` pour tester rapidement, puis `-qm` ou `-qh` pour la version finale.

---

### Idées de contributions
- [ ] Support Spotify API directe (sans téléchargement manuel)
- [ ] Plus de thèmes visuels (dark mode, néon, etc.)
- [ ] Export en différents formats (GIF, images statiques)
- [ ] Comparaison entre plusieurs années
- [ ] Interface web interactive

---

## 📝 Notes importantes

### Limites de l'API Last.fm
- **Rate limit** : 5 requêtes/seconde
- **Genres** : Parfois incomplets ou génériques
- **Albums** : Peuvent ne pas correspondre exactement

### Temps de traitement
- **Analyse** : 5-15 minutes (selon nombre d'artistes)
- **Vidéo** : 2-20 minutes (selon qualité choisie)

### Données Spotify
- Format : JSON uniquement
- Mise à jour : Tous les trimestres chez Spotify
- Historique max : 1 an d'écoutes

---

## 🙏 Remerciements

- [Spotify](https://www.spotify.com/) pour les données
- [Last.fm](https://www.last.fm/) pour l'API gratuite
- [Manim Community](https://www.manim.community/) pour la bibliothèque d'animation
- Inspiré par le [Spotify Wrapped](https://www.spotify.com/wrapped/) officiel

---

## Auteurs
- BAH Elhadj
- CAMARA M'bemba
- DJAU Mamadou 

