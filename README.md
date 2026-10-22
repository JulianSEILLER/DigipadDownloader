# 📦 Digipad Downloader

Application web locale qui récupère toutes les ressources d'un mur **Digipad** (*La Digitale*) et les télécharge en une seule archive ZIP, rangées par colonne.

Développée en **Python (Flask)** avec **Selenium**, pour sauvegarder en quelques secondes des cours partagés sur Digipad, au lieu d'ouvrir chaque document un par un.

<!-- Ajouter ici une capture de l'interface, par exemple :
![Aperçu de Digipad Downloader](docs/apercu.png)
-->

---

## ✨ Fonctionnalités

- 🔍 **Récupération des vrais documents** : l'application ne s'arrête pas aux vignettes. Elle ouvre chaque bloc du mur pour récupérer le lien du fichier d'origine (PDF, Word, Excel, PowerPoint…).
- 📂 **Structure conservée** : dans l'archive ZIP, les fichiers sont rangés dans un dossier par colonne du Digipad.
- 🔐 **Murs protégés** : prise en charge des Digipads privés, à condition de connaître le code d'accès.
- 💀 **Détection des liens morts** : chaque lien est vérifié avant le téléchargement. Un partage *Nuage* (apps.education.fr) supprimé est signalé au lieu de faire échouer l'archive.
- ⚡ **Téléchargement parallèle** : jusqu'à 10 fichiers téléchargés en même temps (multithreading).
- ✅ **Sélection des fichiers** : l'interface liste toutes les ressources trouvées, et vous choisissez celles à mettre dans l'archive.
- 🌐 **Deux modes d'analyse** :
  - **Par lien** : l'application ouvre le Digipad dans un navigateur sans interface (Selenium) ;
  - **Par code source HTML** : pour les murs auxquels Selenium ne peut pas accéder. Dans ce mode, seuls les fichiers *Nuage* sont téléchargés en entier ; pour les autres documents, seule la vignette est récupérée.

## 🛠️ Stack technique

| Rôle | Technologie |
|---|---|
| Serveur web | Python, Flask |
| Navigation automatisée | Selenium (Chrome sans interface), webdriver-manager |
| Analyse du HTML | BeautifulSoup |
| Téléchargements | requests, `concurrent.futures` (multithreading) |
| Interface | HTML, Tailwind CSS, JavaScript |

## ⚙️ Fonctionnement

1. **Analyse** (`/index-page`) : Selenium ouvre le mur, saisit le code d'accès si besoin, puis exécute un script JavaScript qui parcourt chaque bloc. Ce script clique sur les documents pour faire apparaître leur vrai lien de téléchargement.
2. **Vérification** : les liens trouvés sont testés en parallèle, et les liens morts sont marqués dans l'interface.
3. **Archive** (`/download-zip`) : les fichiers sélectionnés sont téléchargés en parallèle, puis l'archive ZIP est créée en mémoire et envoyée au navigateur, sans fichier temporaire sur le disque.

## 📋 Prérequis

- **Python 3.8** ou plus récent
- **Google Chrome** installé (Selenium l'utilise en arrière-plan ; le pilote ChromeDriver est téléchargé automatiquement)

## 🚀 Installation

```bash
git clone https://github.com/JulianSEILLER/DigipadDownloader.git
cd DigipadDownloader

# Environnement virtuel (recommandé)
python -m venv venv
venv\Scripts\activate        # Windows
source venv/bin/activate     # macOS / Linux

pip install -r requirements.txt
```

## 💻 Utilisation

1. Lancez le serveur :
   ```bash
   python app.py
   ```
2. Ouvrez **http://127.0.0.1:5000** dans votre navigateur.
3. Collez l'adresse du Digipad, ajoutez le code d'accès si le mur est protégé, puis cliquez sur **Analyser**.
4. Cochez les fichiers à garder et cliquez sur **Télécharger la sélection (ZIP)**.

## 🗂️ Structure du projet

```
DigipadDownloader/
├── app.py             # Serveur Flask : analyse du mur, vérification des liens, création du ZIP
├── templates/
│   └── index.html     # Interface (Tailwind CSS)
└── requirements.txt   # Dépendances Python
```

## ⚠️ Usage responsable

Ce projet est destiné à un **usage personnel**, par exemple pour sauvegarder des cours auxquels on a légitimement accès.

- L'extraction automatisée de données peut être contraire aux conditions d'utilisation de *La Digitale* ou des hébergeurs des fichiers.
- **Ne déployez pas cette application sur un serveur public** : un usage massif depuis une même adresse IP chargerait les serveurs de *La Digitale*, un service gratuit et associatif.
- L'auteur décline toute responsabilité en cas de mauvaise utilisation de l'outil.

## 📄 Licence

**Tous droits réservés.** Le code est consultable à des fins de démonstration et d'usage personnel. Sa redistribution ou sa modification publique n'est pas autorisée.

---

Développé par **Julian SEILLER** · [Portfolio](https://julianseiller.vercel.app) · [LinkedIn](https://linkedin.com/in/julianseiller)
