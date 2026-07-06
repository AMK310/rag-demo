# 📖 Fiche de révision — RAG

À lire **avant** la séance pratique. Objectif : avoir tous les concepts en tête pour coder sereinement.

---

## 1. C'est quoi le RAG ?

**RAG** = *Retrieval-Augmented Generation* (génération augmentée par la recherche).

> Donner au LLM les **bons documents à lire** AVANT qu'il réponde.

**Le problème qu'il résout :** un LLM ne connaît que ses données d'entraînement. Il ne connaît PAS :
- vos documents internes
- les infos récentes (après sa date d'entraînement)
- les données privées de votre entreprise

**La solution RAG :** avant de répondre, on cherche les passages pertinents dans VOS documents, et on les donne au LLM comme contexte.

### La métaphore de l'examen
| | Sans RAG | Avec RAG |
|---|---|---|
| Type d'examen | Livre **fermé** | Livre **ouvert** |
| Comportement | Répond de mémoire → peut inventer | Cherche la bonne page → répond juste |

---

## 2. Les 5 étapes d'un RAG (à connaître par cœur)

```
1. CHARGER      les documents          → Loader
2. DÉCOUPER     en morceaux (chunks)   → Text Splitter
3. VECTORISER   chaque chunk           → Embeddings
4. STOCKER      les vecteurs           → Vector Store (base vectorielle)
5. CHERCHER     + RÉPONDRE             → Retriever + LLM
```

> ⚙️ Étapes **1 à 4** = préparation (une seule fois).
> ⚙️ Étape **5** = à chaque question posée.

---

## 3. Les concepts clés

### 🔹 Chunk (morceau)
Un document découpé en petits morceaux. Pourquoi ?
- Un LLM ne peut pas avaler un document entier (fenêtre de contexte limitée)
- On veut retrouver LE bon paragraphe, pas tout le fichier

**Chunk overlap (chevauchement)** = les chunks se recouvrent un peu pour ne pas couper une info en deux.

```
Sans overlap :  [chunk 1][chunk 2]   ← info coupée = perdue
Avec overlap :  [chunk 1]
                    [chunk 2]          ← zone commune = pas de perte
```

### 🔹 Embedding (vecteur)
Transformer un texte en **liste de nombres** (un vecteur).

> Règle d'or : **deux textes qui parlent de la même chose ont des vecteurs proches.**

```
"budget Japon"        → [0.12, -0.05, 0.88, ...]
"combien coûte Tokyo" → [0.11, -0.04, 0.90, ...]   ← proche ! (même sens)
"recette de couscous" → [-0.7, 0.33, 0.01, ...]    ← loin (autre sujet)
```

### 🔹 Vector Store (base vectorielle)
Une base de données qui stocke les vecteurs et permet de chercher **par le sens** (pas par mots-clés).
- **Chroma** = local, gratuit, parfait pour apprendre (notre choix)
- FAISS = local, très rapide, gros volumes
- Pinecone = cloud, production, payant

### 🔹 Retriever (chercheur)
L'objet qui, pour une question, retourne les chunks les plus pertinents.
`similarity_search("ma question", k=3)` → les 3 meilleurs chunks.

### 🔹 Recherche sémantique vs mots-clés
| Recherche mots-clés (Ctrl+F) | Recherche sémantique (RAG) |
|---|---|
| Cherche les mots exacts | Cherche le **sens** |
| "voiture" ≠ "automobile" | "voiture" ≈ "automobile" ✅ |

---

## 4. Le flux complet (schéma à mémoriser)

```
PRÉPARATION (une fois)
──────────────────────
Documents → Découper → Vectoriser → Stocker dans Chroma


À CHAQUE QUESTION
──────────────────────
Question
   │
   ▼
Vectoriser la question
   │
   ▼
Chercher les chunks proches (Retriever)
   │
   ▼
Injecter ces chunks dans le prompt   ← "Voici le contexte : ..."
   │
   ▼
LLM génère la réponse
   │
   ▼
Réponse fondée sur VOS documents
```

---

## 5. Points de vigilance (les pièges)

| Piège | Ce qu'il faut savoir |
|---|---|
| **Hallucination** | Un LLM peut inventer. Un bon RAG lui dit : « réponds UNIQUEMENT à partir du contexte ». S'il ne sait pas, il doit l'avouer. |
| **Chunks mal découpés** | Trop petits = pas assez de contexte. Trop grands = bruit + coût. ~500 caractères est un bon départ. |
| **Mauvais embeddings** | Pour du français, utiliser un modèle **multilingue**, sinon la recherche est moins précise. |
| **Question hors sujet** | Le RAG doit répondre « je ne sais pas », pas inventer. C'est un signe de qualité. |

---

## 6. RAG vs Fine-tuning (question classique)

| | RAG | Fine-tuning |
|---|---|---|
| Principe | Donner des **docs à lire** | **Ré-entraîner** le modèle |
| Coût | Faible | Élevé |
| Mise à jour | Instantanée (on change les docs) | Il faut ré-entraîner |
| Quand ? | 90% des cas | Cas spécifiques (style, format très précis) |

> 👉 **On commence TOUJOURS par le RAG.**

---

## 7. Vocabulaire express

| Terme | Définition en une ligne |
|---|---|
| **RAG** | Donner les bons documents au LLM avant qu'il réponde |
| **Chunk** | Un morceau de document |
| **Embedding** | Un texte transformé en vecteur de nombres |
| **Vector Store** | Base qui stocke les vecteurs et cherche par le sens |
| **Retriever** | Composant qui trouve les chunks pertinents |
| **Chroma** | Notre base vectorielle locale et gratuite |
| **LangChain** | Le framework qui connecte toutes ces briques |
| **Hallucination** | Quand l'IA invente une réponse fausse |

---

## ✅ Checklist avant la séance

- [ ] Je sais expliquer le RAG en une phrase (« donner les bons docs au LLM avant qu'il réponde »)
- [ ] Je connais les 5 étapes dans l'ordre
- [ ] Je comprends pourquoi on découpe en chunks
- [ ] Je comprends ce qu'est un embedding (texte → nombres, sens proche = vecteurs proches)
- [ ] Je sais ce qu'est une base vectorielle et à quoi sert Chroma
- [ ] Je sais pourquoi un bon RAG doit pouvoir dire « je ne sais pas »

> Si tu coches tout : tu es prêt(e) pour coder ton premier RAG 🚀
