# 🔴 Tokosel - Chatbot Bancaire Intelligent

Un assistant virtuel multicanal pour la Société Générale Sénégal, utilisant RAG (Retrieval-Augmented Generation) et IA générative pour automatiser les interactions clients.

![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)
![FastAPI](https://img.shields.io/badge/FastAPI-0.104+-green.svg)
![License](https://img.shields.io/badge/License-MIT-yellow.svg)

## 🌟 Fonctionnalités Principales

### 💬 Interaction Multicanal
- ✅ **Interface Web** avec chat texte et vocal
- ✅ **WhatsApp** via Twilio (texte + audio)
- ✅ **Support Audio** : Speech-to-Text (Whisper)

### 🤖 Intelligence Artificielle
- ✅ **RAG (Retrieval-Augmented Generation)** avec ChromaDB
- ✅ **LLM** : Google Gemini 2.5 Flash
- ✅ **MCP (Model Context Protocol)** : Web scraping intelligent du site bancaire
- ✅ **Détection d'intention** automatique

### 🏦 Services Bancaires Automatisés

#### 1. **Simulateur de Prêt** 💰
- Calcul de mensualités en temps réel
- Taux personnalisés selon profil (CDI, Fonctionnaire, Indépendant...)
- Vérification d'éligibilité automatique (taux d'endettement < 35%)
- Types : Immobilier, Auto, Consommation, Entreprise

#### 2. **Ouverture de Compte** 💼
- Collecte conversationnelle des informations
- Validation en temps réel (téléphone, email...)
- Types de comptes : Courant, Épargne, Jeune, Entreprise
- Génération de leads qualifiés

#### 3. **Gestion de Réclamations** ⚠️
- Catégorisation automatique (Carte, Virement, Frais...)
- Collecte structurée des détails
- Suivi avec ID unique (CLAIM_XXX)
- Statut en temps réel

### 📊 Analytics & Monitoring
- ✅ **Dashboard Streamlit** temps réel
- ✅ **Métriques** : latence, taux d'escalade, CSAT
- ✅ **Tracking** : intentions, canaux, modes d'interaction
- ✅ **KPIs Business** : prospects, simulations, réclamations

---

## 📋 Architecture Technique

### Stack Backend
- **Framework** : FastAPI (asynchrone)
- **Base de données** : SQLite (prospects, claims, simulations, metrics)
- **Vector Store** : ChromaDB (embeddings HuggingFace)
- **LLM** : Google Gemini 2.5 Flash via LangChain
- **Audio** : Whisper (STT)

### Stack Frontend
- **Web UI** : Html + Tailwind CSS + JavaScript
- **Analytics** : Streamlit

### Intégrations
- **Twilio** : WhatsApp API
- **Google AI Studio** : Gemini + Embeddings
- **Web Scraping** : BeautifulSoup + httpx

---

## 🚀 Installation

### 1️⃣ Prérequis

**Logiciels**
- Python 3.8+
- pip et npm

**Comptes nécessaires**
1. [Google AI Studio](https://aistudio.google.com) (API Gemini - gratuit)
2. [Twilio](https://www.twilio.com) (WhatsApp - 20$ offerts)

### 2️⃣ Cloner le projet

```bash
git clone https://github.com/tokosel/chatbot-bancaire
cd chatbot-bancaire
```

### 3️⃣ Configuration Backend

#### Créer l'environnement virtuel
```bash
python -m venv env

# Windows
env\Scripts\activate

# Linux/Mac
source env/bin/activate
```

#### Installer les dépendances
```bash
pip install -r requirements.txt
```

#### Configurer les variables d'environnement

Créez un fichier `.env` à la racine :

```env
# Google AI (Gemini)
GOOGLE_API_KEY=AIzaSyxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Twilio WhatsApp
TWILIO_ACCOUNT_SID=ACxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_AUTH_TOKEN=xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
TWILIO_WHATSAPP_NUMBER=whatsapp:+14155238886

# Configuration App
PUBLIC_URL=https://votre-domaine.com
BANK_WEBSITE_URL=https://societegenerale.sn/fr/

# MCP (Web Scraping)
MCP_ENABLED=true
MCP_CACHE_TTL=3600
```

#### Créer la base de connaissances

```bash
cd backend
python ingest_data.py
```

Cela va :
- Charger les documents de `data/`
- Créer les embeddings avec HuggingFace
- Stocker dans ChromaDB (`chroma_db/`)

---

## 🏃 Démarrage

### Backend (API FastAPI)

```bash
cd backend
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

API disponible sur : `http://localhost:8000`

**Endpoints principaux** :
- `POST /api/chat` - Chat textuel
- `POST /api/voice-chat` - Chat vocal
- `POST /whatsapp/webhook` - Webhook Twilio

### Frontend (Flask)

```bash
python app.py
```

Interface disponible sur : `http://127.0.0.1:5000`

### Dashboard Analytics

```bash
cd backend
streamlit run dashboard.py
```

Dashboard disponible sur : `http://localhost:8501`

---

## 📱 Configuration WhatsApp

### Étape 1 : Sandbox Twilio

1. Console Twilio → **Messaging** → **Try WhatsApp**
2. Notez le code sandbox (ex: `join knew-depth`)
3. Envoyez ce code au numéro `+1 415 523 8886` depuis WhatsApp

### Étape 2 : Webhook

#### Développement (ngrok)

```bash
# Installer ngrok
npm install -g ngrok

# Exposer l'API
ngrok http 8000

# Notez l'URL : https://abc123.ngrok.io
```

#### Configuration Twilio

1. Console Twilio → **Messaging** → **Settings** → **WhatsApp sandbox**
2. **When a message comes in** : `https://abc123.ngrok.io/whatsapp/webhook`
3. **Method** : `POST`
4. **Save**

### Étape 3 : Test

Envoyez à `+1 415 523 8886` :
```
Bonjour
```

Vous devriez recevoir :
```
Bonjour ! Je suis Tokosel, votre assistant bancaire SG.
Je peux vous aider à :
- Ouvrir un compte
- Simuler un prêt
- Répondre à vos questions

Comment puis-je vous aider ?
```

---

## 💡 Exemples d'Utilisation

### 1. Simulation de Prêt

**Via API** :
```bash
curl -X POST http://localhost:8000/api/chat \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Je veux emprunter 5 millions",
    "session_id": "user123"
  }'
```

**Via WhatsApp** :
```
User: Je veux simuler un prêt immobilier
Bot: Quel montant souhaitez-vous emprunter ?
User: 10 millions
Bot: Sur combien de temps ?
User: 15 ans
Bot: Quels sont vos revenus mensuels ?
User: 800000
Bot: Votre situation professionnelle ?
User: CDI

Bot: 📊 RÉSULTATS DE VOTRE SIMULATION
💰 Prêt Immobilier : 10 000 000 FCFA
Durée : 15 ans (180 mois)
💳 MENSUALITÉ : 88 849 FCFA
Taux : 6.0% / an
✅ Votre dossier est éligible !
```

### 2. Ouverture de Compte

```
User: Je veux ouvrir un compte
Bot: Quel type de compte vous intéresse ?
     [Courant] [Épargne] [Jeune] [Entreprise]
User: Compte Courant
Bot: Quel est votre nom de famille ?
User: DIOP
Bot: Et votre prénom ?
User: Fatou
...
Bot: ✅ Votre demande est enregistrée (PROS_20251210_847)
     Un conseiller vous contactera sous 48h
```

### 3. Réclamation

```
User: J'ai un problème avec ma carte
Bot: De quelle catégorie s'agit-il ?
     [Carte bancaire] [Virement] [Frais] [Service client]
User: Carte bancaire
Bot: Pouvez-vous décrire le problème ?
User: Débit frauduleux de 50 000 FCFA
...
Bot: ✅ Votre réclamation est enregistrée (CLAIM_20251210_328)
     Un conseiller vous contactera sous 24h
```

---

## 📊 Dashboard Analytics

Le dashboard Streamlit affiche en temps réel :

### KPIs Globaux
- 📊 Nombre d'interactions
- ⚡ Latence moyenne (ms)
- 🔄 Taux d'escalade (%)
- 😊 CSAT Score (%)
- 💼 Prospects générés
- 💰 Simulations effectuées
- ⚠️ Réclamations ouvertes

### Graphiques
- Évolution du trafic (quotidien)
- Distribution des intentions
- Canaux d'acquisition (Web/WhatsApp)
- Modes d'interaction (Texte/Audio)
- Taux d'éligibilité prêts
- Types de prêts demandés
- Profils professionnels

### Tables Détaillées
- 10 derniers prospects
- 10 dernières simulations
- 10 dernières réclamations

**Accès** : `http://localhost:8501`

---

## 🗄️ Structure de la Base de Données

### Tables Principales

**prospects**
```sql
prospect_id, session_id, nom, prenom, telephone, 
email, type_compte, status, source, created_at, updated_at
```

**loan_simulations**
```sql
simulation_id, session_id, montant, duree, type_pret,
revenus_mensuels, situation_pro, taux_interet, mensualite,
cout_total, interets_totaux, taux_endettement, eligible,
status, source, created_at, updated_at
```

**claims**
```sql
claim_id, session_id, categorie, description, montant,
date_incident, nom_complet, telephone, email, status,
source, created_at, updated_at
```

**interactions** (Métriques)
```sql
id, timestamp, session_id, source, input_type, intent,
latency_ms, is_fallback, message_length
```

**feedbacks**
```sql
id, timestamp, session_id, score, comment
```

**chat_history**
```sql
id, session_id, role, content, source, timestamp
```

---


## 🚀 Déploiement Production

### Backend (Recommandations)

**Options** :
- **Railway** : `railway up` (gratuit 5$/mois)
- **Render** : Web Service + PostgreSQL
- **Heroku** : Dyno + Heroku Postgres
- **VPS** : DigitalOcean, Linode (5$/mois)

**Configuration** :
```bash
# Installer gunicorn
pip install gunicorn

# Démarrer en production
gunicorn main:app -w 4 -k uvicorn.workers.UvicornWorker
```

**Variables d'environnement** :
- Migrer SQLite → PostgreSQL
- Configurer `PUBLIC_URL` avec votre domaine
- Activer HTTPS (Let's Encrypt)

### Frontend

**Options** :
- **Vercel** : `vercel deploy` (gratuit)
- **Netlify** : Drag & Drop (gratuit)
- **GitHub Pages** : `npm run build` + deploy

### Webhook WhatsApp

Remplacez l'URL ngrok par votre domaine :
```
https://api.votredomaine.com/whatsapp/webhook
```

---

## 💰 Coûts Estimés

### Développement (Gratuit)
- Google AI : Gratuit (60 req/min)
- Twilio Sandbox : Gratuit illimité
- Hosting local : Gratuit

### Production (~ 15-30 $/mois)
- **Backend** : 5-10 $/mois (Railway, Render)
- **Base de données** : 0-5 $/mois (PostgreSQL gratuit < 1GB)
- **Twilio WhatsApp** :
  - Numéro dédié : 1 $/mois
  - Messages : 0.005 $/msg (ex: 1000 msg = 5$)
- **Google AI** : Gratuit sous quota (puis ~0.001$/req)

**Total estimé** : **10-20 $/mois** pour 1000-2000 messages/mois

---

## 🔒 Sécurité

### Recommandations Production

✅ **Variables d'environnement**
- Jamais de secrets dans le code
- Utiliser `.env` + `.gitignore`

✅ **HTTPS obligatoire**
- Let's Encrypt gratuit
- Certificat SSL pour webhook Twilio

✅ **Rate Limiting**
- Limiter les requêtes API (10 req/sec)
- Protection anti-spam WhatsApp

✅ **CORS**
- Configurer `allow_origins` en production
- Bloquer origines inconnues

✅ **Logs & Monitoring**
- Sentry pour erreurs
- LogRocket pour sessions utilisateur

---

## 📚 Documentation Complémentaire

### APIs Utilisées
- [FastAPI Docs](https://fastapi.tiangolo.com/)
- [Twilio WhatsApp API](https://www.twilio.com/docs/whatsapp)
- [Google AI Studio](https://ai.google.dev/docs)
- [LangChain](https://python.langchain.com/docs/)
- [Whisper](https://github.com/openai/whisper)

---

## 📄 Licence

MIT License - Voir [LICENSE](LICENSE)

---

## 👥 Auteurs
- Développement : Abdoulaye SALL
- Encadrement : Pr Fodé CAMARA, M. Mamadou SARR
- Etablissement : CRD - Université Alioune Diop de Bambey, Sénégal

---

**🎉 Tokosel - L'assistant bancaire intelligent qui révolutionne l'expérience client ! 🔴**