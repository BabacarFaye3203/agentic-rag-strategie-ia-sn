# Agentic RAG - Stratégie IA du Sénégal

Projet de démonstration d’un système **RAG agentique** permettant d’interroger des documents locaux sur les initiatives en intelligence artificielle au Sénégal.

Le projet charge des PDF, découpe leur contenu en fragments, crée une base vectorielle avec ChromaDB, puis utilise un agent LangGraph capable de décider quand rechercher des informations dans cette base.

## Fonctionnement

1. Chargement des PDF depuis le dossier `ressources/`.
2. Découpage du contenu en chunks avec `RecursiveCharacterTextSplitter`.
3. Création d’embeddings avec Ollama.
4. Stockage des vecteurs dans ChromaDB.
5. Exposition du retriever sous forme de tool LangChain.
6. Décision par le modèle Llama 3.2 d’utiliser ou non le tool de recherche.
7. Génération d’une réponse basée uniquement sur les documents retrouvés.

```text
Question utilisateur
        |
        v
Noeud Agent (Llama 3.2)
        |
        +--> Appel du retriever --> Noeud Tools --> Agent
        |
        +--> Reponse finale
```

## Technologies

- Python 3.13
- LangChain
- LangGraph
- Ollama
- ChromaDB
- Jupyter Notebook
- PyPDF

## Structure du projet

```text
agentic_rag/
├── ressources/
│   └── Synthèse de la stratégie IA du Sénégal.pdf
├── src/
│   └── agentic_rag/
│       └── __init__.py
├── sn_ai_strategies.ipynb
├── pyproject.toml
├── uv.lock
└── README.md
```

## Prérequis

- Python 3.13 ou supérieur
- [uv](https://docs.astral.sh/uv/)
- [Ollama](https://ollama.com/)
- Le modèle Ollama `llama3.2`
- Le modèle d’embeddings `nomic-embed-text`

Installer Ollama et télécharger les modèles :

```bash
ollama pull llama3.2
ollama pull EmbeddingGemma
```

Vérifier qu’Ollama est démarré :

```bash
ollama list
```

## Installation

Depuis la racine du projet :

```bash
uv sync
```

Pour enregistrer l’environnement comme kernel Jupyter :

```bash
uv run python -m ipykernel install --user --name agentic_rag
```

## Exécution

Lancer Jupyter :

```bash
uv run jupyter notebook
```

Puis ouvrir `sn_ai_strategies.ipynb` et sélectionner le kernel `agentic_rag`.

Exécuter les cellules dans l’ordre, depuis le chargement du PDF jusqu’au test final. Le notebook construit notamment :

- le retriever vectoriel ;
- le tool `retriever_tool` ;
- le modèle lié aux outils avec `bind_tools(...)` ;
- le graphe LangGraph ;
- les arêtes conditionnelles permettant de passer à l’outil ou de terminer.

## Exemple de question

```text
Quelles sont les initiatives nationales en intelligence artificielle présentées dans la stratégie du Sénégal ?
```
