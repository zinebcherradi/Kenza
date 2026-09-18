Projet Kenza - Agent Commercial Autonome Hybride

Assistant conversationnel intelligent et trilingue (Francais, Arabe, Darija) dedie aux e-commercants au Maroc (WhatsApp / Instagram), avec automatisation des ventes, gestion des stocks et relance des paniers abandonnes.

Presentation du Projet

Le projet Kenza repond a la surcharge operationnelle des petits commerces en ligne au Maroc face au volume massif de messages et aux paniers abandonnes. L'agent combine une intelligence conversationnelle structuree et des garde-fous stricts connectes a une base de donnees relationnelle pour automatiser la relation client, la recherche de catalogue, le calcul de livraison et la prise de commande sans hallucination.

Architecture Technique (Stack SUT)

L'application repose sur une architecture moderne, modulaire et conteneurisee :

API Backend : Node.js avec Fastify pour la rapidite d'execution.

Intelligence Artificielle et Agents : LangGraph / LangChain pour orchestrer le comportement agentique et les boucles de decision.

Persistence et Donnees : PostgreSQL 16 (catalogue, clients, commandes, persistence des etats).

Cache et Asynchronisme : Redis 7 et BullMQ pour la gestion de la memoire court terme et le traitement en arriere-plan (relances de paniers abandonnes).

Interface Utilisateur : React / Vite (simulateur de chat et tableau de bord commercant).

Structure du Repertoire

Kenza-Hackaton/
├── backend/               # Code source du serveur et des agents
│   └── src/
│       ├── agents/        # Definition du graphe LangGraph (Kenza)
│       ├── config/        # Connexions PostgreSQL et Redis
│       ├── queues/        # Files d'attente BullMQ
│       ├── routes/        # Endpoints API Fastify
│       ├── tools/         # Les 5 Tools SQL securises (Recherche, Stock, etc.)
│       └── workers/       # Workers de fond (ex: relance paniers)
├── data/                  # Jeux de donnees initiaux (CSV, Markdown)
├── docker/                # Fichiers de configuration Docker
├── docker-compose.yml     # Orchestration globale des services
└── .env                   # Variables d'environnement


Les 5 Outils Metier (Tools SQL)

Pour eliminer tout risque d'hallucination, l'agent LangGraph s'appuie exclusivement sur des fonctions strictement typees :

searchProducts : Exploration dynamique du catalogue produits.

checkStock : Verification en temps reel des stocks par article et variante (taille, couleur).

calculateShipping : Evaluation dynamique des frais et delais de livraison selon la ville au Maroc.

createOrder : Enregistrement securise de la commande et mise a jour des stocks.

escalateToHuman : Transfert de la conversation a un operateur humain si necessaire.

Orchestration du Systeme

L'architecture s'articule autour de deux grands orchestrateurs :

L'Orchestrateur Technique (Fastify) : Recoit les requetes HTTP/WebSocket du frontend, dialogue avec PostgreSQL et pilote le courtage de messages avec Redis/BullMQ.

L'Orchestrateur Logique (LangGraph) : Pilote le raisonnement de l'agent Kenza, gere le fil de la discussion et determine l'appel sequentiel des outils metiers.

Lancement Rapide (Docker Compose)

Pour deployer l'ensemble de l'infrastructure (API, PostgreSQL, Redis, Worker) :

# Lancer les conteneurs en arriere-plan
docker-compose up -d
