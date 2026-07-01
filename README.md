# RAG Demo — Hands-on LangChain + Chroma

Séance pratique : construire un **chatbot RAG** qui répond à partir de vos propres documents.

**Stack :** LangChain · ChromaDB · FastEmbed (embeddings locaux) · Google Gemini

---

## 🎯 Objectif

Comprendre et coder les **5 étapes d'un RAG** :

1. **Charger** des documents (Loader)
2. **Découper** en chunks (Splitter)
3. **Vectoriser** les chunks (Embeddings — en local, sans clé)
4. **Stocker** dans une base vectorielle (Chroma)
5. **Chercher + Répondre** (Retriever + LLM)

---

## 🚀 Installation

```bash
# 1. Cloner le repo
git clone https://github.com/adamazirou/rag-demo.git
cd rag-demo

# 2. Installer les dépendances
pip install -r requirements.txt

# 3. Créer le fichier .env avec votre clé Gemini
cp .env.example .env
# puis éditer .env et coller votre clé (commence par AIza...)
```

> 🔑 **Clé Gemini gratuite** : [aistudio.google.com/app/apikey](https://aistudio.google.com/app/apikey)  
> Elle sert **uniquement** à la génération de la réponse finale. Les étapes 1 à 5 tournent **100% en local**.

---

## 📓 Utilisation

Ouvrir `rag-demo.ipynb` dans Jupyter / VS Code et exécuter les cellules dans l'ordre.

```bash
jupyter notebook rag-demo.ipynb
```

---

## 📁 Structure

```
rag-demo/
├── rag-demo.ipynb        ← Le notebook de la séance
├── data/                 ← Les documents (à enrichir !)
│   ├── maroc.txt
│   ├── japon.txt
│   └── islande.txt
├── requirements.txt
├── .env.example          ← Modèle (copier en .env)
└── README.md
```

---

## ✏️ Exercice

Ajoutez un fichier `data/votre_destination.txt`, relancez le notebook, et posez des questions sur cette nouvelle destination. Testez aussi une question **hors sujet** : le bot doit avouer qu'il ne sait pas (pas d'hallucination).
