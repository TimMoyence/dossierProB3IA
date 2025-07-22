### 1. Contexte et enjeux du projet

#### 1.1 Contexte sanitaire et besoin de prédiction

Depuis la pandémie de COVID-19, la capacité à anticiper l’évolution d’une crise sanitaire s’est imposée comme un enjeu stratégique majeur pour les institutions de santé publique. Qu’il s’agisse d’allocations de ressources hospitalières, de campagnes de vaccination ou de mesures préventives, les acteurs publics doivent pouvoir s’appuyer sur des outils de prévision fiables, transparents et accessibles.

Dans ce contexte, les données ouvertes (Open Data) ont joué un rôle crucial, mais restent souvent sous-exploitées en raison de leur complexité ou de leur hétérogénéité. La valorisation de ces données via des modèles prédictifs peut apporter une réponse concrète, à condition de combiner rigueur technique, transparence des résultats et facilité de déploiement.

#### 1.2 Objectifs pédagogiques et techniques du projet

Ce projet s’inscrit dans le cadre du module MSPR TPRE502, dans une logique de mise en œuvre complète de la chaîne de valeur d’un projet IA : depuis la collecte et la préparation des données jusqu’à la mise à disposition d’un service prédictif via une API.

L’objectif était double :

- **Construire une plateforme de prédiction multi-cibles épidémiologiques**, capable de prédire plusieurs indicateurs (nouveaux cas, décès, récupérations, etc.) en fonction de la géolocalisation et du temps.
- **Déployer une solution modulaire, conteneurisée et documentée**, permettant une intégration rapide dans un système d’information de santé publique ou dans un outil de data visualisation.

L’ensemble du projet a été mené de manière autonome, dans une logique DevOps et orientée utilisateur final, avec une attention particulière portée à la traçabilité des résultats, à l’automatisation des traitements, et à la simplicité d’utilisation.

#### 1.3 Positionnement dans la chaîne de valeur Data / IA

Ce projet se positionne sur l’ensemble des étapes du cycle de vie d’un produit Data :

- **Collecte** : extraction des données COVID et MPOX depuis une base PostgreSQL intégrant des sources Open Data,
- **Transformation** : modélisation sémantique avec DBT et structuration étoile orientée analytique,
- **Entraînement IA** : prédiction automatique avec AutoGluon en multi-cibles,
- **Déploiement** : exposition des prédictions via une API REST documentée avec FastAPI,
- **Exploitation** : visualisation via une interface frontend en React, avec intégration directe de l’API.

Ce projet m’a permis de mobiliser à la fois mes compétences en traitement de données, en Machine Learning, en développement backend et en architecture logicielle. Il s’intègre parfaitement dans mon parcours de Développeur IA & Data Science, en cohérence avec les attendus du titre RNCP 36581.

### 2. Cahier des charges et exigences fonctionnelles

#### 2.1 Enjeux fonctionnels

Le projet vise à répondre à un besoin fondamental : disposer d’une **plateforme simple et accessible permettant de prédire des indicateurs épidémiologiques** à partir de données contextuelles (date, pays, population, etc.).

Les utilisateurs cibles sont :

- des analystes de la santé publique souhaitant anticiper les évolutions d’une pandémie,
- des développeurs souhaitant intégrer les prédictions à une plateforme existante,
- des décideurs ayant besoin d’indicateurs synthétiques et prédictifs.

L’objectif est donc de proposer un système :

- **fiable**, basé sur des modèles de Machine Learning entraînés automatiquement sur données vérifiées,
- **modulaire**, chaque cible de prédiction (nouveaux cas, décès, récupérations, etc.) disposant de son propre modèle,
- **accessible**, via une API REST simple et un frontend épuré,
- **automatisé**, avec génération dynamique des jeux de données et mise à jour facile des modèles.

#### 2.2 Fonctionnalités attendues

Le cahier des charges a été défini autour des fonctionnalités suivantes :

##### 🔍 Collecte et traitement des données

- Collecte de données depuis une base PostgreSQL.
- Utilisation de DBT pour la structuration des données en trois couches : bronze, silver, gold.
- Nettoyage, transformation, enrichissement (moyennes mobiles, différences J/J-7, etc.).

##### 🧠 Entraînement et gestion des modèles

- Entraînement automatique des modèles avec AutoGluon pour chaque cible définie.
- Optimisation par stacking multi-niveaux et évaluation RMSE/MAE/R².
- Stockage des scores par groupe (pays, continent, année) dans des fichiers CSV versionnés.

##### 📦 Mise à disposition via API

- Création d’une API REST sous FastAPI permettant :

  - de prédire une cible en fonction de données d’entrée,
  - d’enregistrer la prédiction en base (table `predicted_pandemic_stats`),
  - de vérifier les modèles disponibles via la route `/models/available`.

##### 💻 Interface utilisateur

- Mise en place d’un frontend React + Vite + Tailwind,
- Composants dynamiques (formulaire, affichage des prédictions),
- Responsive design compatible desktop et mobile.

#### 2.3 Contraintes techniques

- Déploiement conteneurisé avec Docker et Docker Compose,
- Respect des normes de sécurité (isolation des services, gestion des variables d’environnement),
- Compatibilité avec PostgreSQL pour lecture et écriture des données,
- Temps d'entraînement limité à 600 secondes par cible pour maîtriser la charge machine.

#### 2.4 Contraintes organisationnelles

- Travail réalisé dans le cadre d’un sprint pédagogique avec rendu final au **22 juillet 2025**,
- Collaboration en autonomie, avec gestion du backlog sur Trello,
- Suivi des avancements via un diagramme de Gantt produit sur Instagantt,
- Livraison complète attendue sous forme de dépôt Git + rapport documenté + soutenance orale.

## 3. Architecture technique et choix technologiques

### 3.1 Vue d’ensemble de l’architecture

L’architecture technique du projet a été pensée selon un modèle **modulaire, conteneurisé et évolutif**, reposant sur les grands principes du développement moderne : séparation des responsabilités, scalabilité et automatisation.

Elle s’articule autour de 3 couches principales :

1. **Traitement des données** (ETL + modélisation via DBT)
2. **Machine Learning** (entraînement automatique des modèles avec AutoGluon)
3. **Exposition des résultats** (API FastAPI + interface utilisateur React)

L’ensemble est orchestré via Docker, garantissant la reproductibilité, la compatibilité entre environnements, et la facilité de déploiement.

### 3.2 Choix des technologies

#### 🧱 Backend API – FastAPI

- **Motivation** : Rapidité de développement, support natif d’OpenAPI, intégration facile avec Pydantic pour la validation des données.
- **Usage** : Exposition de routes REST pour la prédiction, la consultation des modèles disponibles et l’insertion des résultats en base PostgreSQL.

#### 🤖 Machine Learning – AutoGluon

- **Motivation** : Plateforme AutoML puissante, permettant l’automatisation complète de l’entraînement de modèles.
- **Avantages** :

  - Support multi-modèles (LightGBM, CatBoost, FastAI, NeuralNetTorch, etc.),
  - Stacking automatique sur plusieurs niveaux,
  - Optimisation intégrée sur des métriques de régression (RMSE, MAE, R²),
  - Définition d’une limite de temps par cible (600 secondes) pour équilibrer performance et coût.

- **Résultat** : Un modèle distinct par indicateur épidémiologique, stocké dans un répertoire versionné (`/models/`), avec score par pays, année et continent.

#### 🗃️ Base de données – PostgreSQL

- Stockage des données brutes et retraitées (modèle en étoile avec table de faits et dimensions),
- Stockage des prédictions effectuées par l’API.

#### 🔄 DBT (Data Build Tool)

- **Motivation** : Structuration robuste des données en 3 couches :

  - **Bronze** : injection des données brutes COVID-19 et MPOX,
  - **Silver** : enrichissement géographique et temporel,
  - **Gold** : table de faits prête à l’usage pour le Machine Learning.

- **Avantages** : documentation automatique, versionning des transformations, contrôle qualité intégré.

#### 💻 Frontend – React + Vite + Tailwind

- **Motivation** : Stack moderne, rapide à mettre en place, réactive et ergonomique.
- **Composants clés** :

  - Formulaire d’entrée pour générer des prédictions à partir de données manuelles,
  - Affichage conditionnel des résultats,
  - Icônes via Lucide React,
  - Graphiques via la librairie **Recharts**, permettant une intégration directe dans React sans surcharge inutile.

#### 🐳 Orchestration – Docker & Docker Compose

- Conteneurisation des 4 services : `postgres`, `api`, `trainer`, `frontend`,
- Réseau interne (`backend`, `frontend`) pour isolation,
- Montage du dossier partagé `/models` pour que l’API accède aux modèles générés à chaud par l’entraîneur.

### 3.3 Schéma d’architecture

[Schema d l'architeture a a récupérr du dossier word] ()

## 4. Méthodologie de travail et organisation

### 4.1 Approche projet

Ce projet a été mené selon une approche **agile**, combinant planification initiale, itérations courtes, livraisons intermédiaires et ajustements progressifs. Le but était de favoriser la montée en compétence tout en produisant un livrable concret, utile et réutilisable.

Nous avons adopté les principes suivants :

- **Découpage modulaire** : chaque brique technique (ETL, ML, API, Frontend) a été développée indépendamment mais de manière cohérente avec les autres.
- **Tests manuels fréquents** : notamment sur l’API, les données d’entraînement, les scores AutoGluon et les prédictions finales.
- **Documentation progressive** : intégrée dans les scripts DBT, les readme de chaque dossier, et synthétisée dans le livrable final.

### 4.2 Organisation et outils

#### 🛠️ Outils utilisés

- **Trello** pour la gestion des tâches et le suivi d’avancement,
- **Git** et **GitHub** pour le versionnement,
- **Docker Compose** pour la cohérence entre les environnements,
- **Notion** pour la documentation complémentaire,
- **Metabase** pour la visualisation métier en parallèle du projet IA.

#### 📆 Planification

Le projet a officiellement débuté en **février 2025** avec une première phase de cadrage fonctionnel et de prise en main des sources de données.

Un **diagramme de Gantt**, réalisé via Instagantt, a permis de définir les jalons suivants :

| Étape principale                     | Période approximative |
| ------------------------------------ | --------------------- |
| Analyse et cadrage                   | Fév. 2025             |
| Mise en place du modèle de données   | Mars 2025             |
| Développement des modèles DBT        | Avril 2025            |
| Entraînement des modèles IA          | Mai 2025              |
| Développement de l’API & du Frontend | Juin 2025             |
| Finalisation, tests et rendu         | Juillet 2025          |

### 4.3 Répartition des tâches

Le projet a été conçu comme une démonstration complète de mes compétences en développement IA & Data, avec une implication sur l’ensemble des couches techniques :

- **Traitement de la donnée** (DBT, PostgreSQL) – 30 %
- **Machine Learning** (AutoGluon, scripts Python) – 40 %
- **API & Frontend** – 20 %
- **Documentation & coordination projet** – 10 %

Ce découpage a permis une montée en autonomie progressive, avec un rythme soutenu jusqu’à la livraison finale.

## 5. Résultats obtenus et évaluation

### 5.1 Réalisation fonctionnelle

Le projet a abouti à une **plateforme complète de prédiction épidémiologique**, fonctionnelle et déployable localement, composée des éléments suivants :

- 📊 **Un pipeline de traitement de données robuste** via DBT, avec des modèles bronze, argent et une table de faits quotidienne,
- 🧠 **Un moteur de Machine Learning automatique** entraîné sur plusieurs cibles avec AutoGluon, stocké dans une arborescence `models/`,
- 🌐 **Une API FastAPI** exposant les prédictions par cible et la consultation des modèles disponibles,
- 🖥️ **Une interface frontend React** avec formulaire et graphiques, connectée dynamiquement à l’API.

Chaque brique a été testée individuellement puis intégrée dans un environnement cohérent avec Docker Compose, garantissant un fonctionnement bout-en-bout.

### 5.2 Performances des modèles

Les modèles AutoGluon ont été entraînés sur sept indicateurs clés, incluant `new_cases`, `new_deaths`, `active_cases`, etc. Pour chaque cible, un pipeline AutoGluon a sélectionné automatiquement les meilleurs modèles (LightGBM, CatBoost, NeuralNetTorch, etc.) et optimisé le score via stacking.

Un temps maximum d’entraînement de 600 secondes par cible a été imposé pour garantir un bon compromis entre performance et temps de calcul.

Les performances moyennes observées (sur jeu de test) pour les modèles les plus stables :

| Cible          | RMSE (±) | R²    | Modèle dominant     |
| -------------- | -------- | ----- | ------------------- |
| `new_cases`    | ≈ 12.4   | 0.78  | LightGBM + CatBoost |
| `new_deaths`   | ≈ 0.49   | -8.43 | Modèle non stable   |
| `active_cases` | ≈ 34.7   | 0.69  | Ensemble weighted   |

NB : Certaines cibles instables comme `new_recovered` n’ont pas pu être modélisées efficacement, ce qui a été anticipé et géré par des contrôles de données avant l’entraînement.

### 5.3 Évaluation de la solution

#### ✅ Points forts :

- **Automatisation avancée** de bout en bout (DBT → ML → API),
- **Architecture modulaire et maintenable** (chaque composant isolé),
- **Qualité du scoring** pour les cibles pertinentes,
- **Documentation complète** pour la reprise du projet.

#### 🔄 Points de vigilance :

- Certaines **cibles ont peu de données fiables** → amélioration possible via enrichissement ou ingénierie de features.
- **Pas encore de tests automatisés unitaires** (notamment côté frontend), mais les tests manuels ont couvert les cas d’usage principaux.

5. Résultats obtenus et évaluation
   5.1 Choix du framework : AutoGluon

Le projet nécessitait une solution d'apprentissage automatique capable de s'adapter à plusieurs indicateurs cibles, sans multiplier la complexité algorithmique. AutoGluon a été retenu comme framework principal pour l’entraînement des modèles en raison de ses atouts majeurs :

Automatisation du pipeline de machine learning : AutoGluon prend en charge l’ingestion des données, le prétraitement, la sélection de modèles, la validation croisée, l’optimisation d’hyperparamètres et l’assemblage final.
Stacking multi-niveaux : AutoGluon utilise une stratégie d’empilement (stacking) de modèles sur plusieurs couches (L1, L2…), permettant de combiner intelligemment plusieurs algorithmes comme LightGBM, CatBoost, XGBoost, NeuralNetTorch, KNN, etc.
Adaptabilité aux ressources : Un temps d’entraînement maximal de 600 secondes a été imposé pour chaque cible, assurant un bon équilibre entre performance prédictive et coût computationnel.
Évaluation intégrée : Le framework permet un calcul direct de plusieurs métriques clés : RMSE, MAE, R², et Pearson, facilitant la comparaison entre cibles.
5.2 Fonctionnement des modèles AutoGluon

Pour chaque indicateur cible (ex. : new_cases, new_deaths, active_cases, etc.), un modèle indépendant est entraîné à partir de données prétraitées (via DBT). Chaque modèle suit une procédure stricte :

Préparation du dataset :
Nettoyage des valeurs manquantes,
Ajout de features temporelles (jour, semaine, année),
Création de moyennes mobiles sur 7 jours et deltas J-J7 pour capturer les tendances.
Split temporel :
70 % des données pour l'entraînement,
15 % pour la validation,
15 % pour le test,
Une logique de fallback est intégrée si un des jeux est vide, en élargissant dynamiquement l’échantillon.
Sélection automatique de modèles de base :
AutoGluon essaie une centaine de configurations incluant : LightGBM (et LightGBMXT), CatBoost, XGBoost, KNN, Random Forest, Extra Trees, réseaux de neurones torch et FastAI.
Chaque modèle est entraîné et évalué individuellement, puis les meilleurs sont combinés via un WeightedEnsemble_L2.
Évaluation et scoring :
À l’issue de l’entraînement, chaque modèle est évalué sur le jeu de test via les métriques précitées.
Les prédictions sont sauvegardées et des scores par pays et par année sont également produits sous forme de fichiers scores_by_group.csv.
Persistance des modèles :
Chaque modèle est sauvegardé dans un dossier models/{target}/ accessible par l’API FastAPI.
Une API dédiée permet ensuite de charger dynamiquement le bon modèle et de produire des prédictions en ligne à partir de nouvelles données.
5.3 Interprétation des résultats

L'évaluation des modèles a montré des performances contrastées selon les cibles. Les indicateurs de type new_cases ou new_deaths présentent une certaine stabilité grâce aux signaux temporels ajoutés, tandis que new_recovered ou cases_per_million montrent davantage de variabilité, en lien avec les disparités géographiques et les biais de déclaration.

Les scores négatifs de R² observés sur certaines cibles indiquent un fort désalignement entre les prévisions et les données réelles, ce qui a conduit à des actions correctives : nettoyage plus strict, enrichissement de features, ou exclusion de cibles trop bruitées.

5.4 Bilan sur l’IA et mes compétences mobilisées

Ce projet m’a permis d’explorer concrètement la mise en œuvre complète d’un pipeline de machine learning en production :

Préparation et ingénierie des features avancée : intégration de signaux temporels, moyennes mobiles, catégorisation géographique.
Entraînement multi-cibles automatisé : gestion de modèles indépendants, avec monitoring de la qualité.
Utilisation de modèles en ligne : intégration dans une API scalable et connectée à une base PostgreSQL pour tracer les prédictions.
Évaluation fine : sélection de métriques pertinentes pour la régression, et enregistrement structuré des résultats.
Capacité à itérer rapidement pour corriger les erreurs (ex. : cibles trop bruitées, validation vide).
La robustesse d’AutoGluon a été un levier important, mais c’est la maîtrise de son paramétrage, de la gestion des données et de l’exploitation API qui ont permis d’en tirer le plein potentiel.
