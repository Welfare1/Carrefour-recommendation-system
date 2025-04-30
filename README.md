# CARREFOUR DEFI IA 2024-2025

![Slide 1](Assets/image-carrefour-challenge.png)

**[UFHB]-@WELFARE**

**MARIAM DJIRE, ANGELA KONATE, FREDERIC AKADJE**<br>
**Rang compétition: 2è/ 43 équipes**

---

La compétition Kaggle a été organisée par l’Université de Bordeaux en France et s’est déroulée sur une période de quatre mois, réunissant plusieurs universités françaises. Mon équipe et moi avons eu l’opportunité d’y participer grâce au partenariat entre notre établissement et l’Université de Rennes 2 (France). Nous partageons ici l’approche que nous avons développée, laquelle nous a permis d’obtenir la 2ᵉ place du classement.

## Table des matières

* Contexte et problématique [cite: 3]
* Description des données [cite: 3]
* Métrique d'évaluation [cite: 3]
* Modélisation [cite: 3]
    * Etiquette [cite: 3]
    * Disposition du jeu de données [cite: 3]
    * Tendance au sein des données [cite: 3]
    * Caractéristiques [cite: 3]
    * Algorithmes d'apprentissage [cite: 3]
    * Caractéristiques du modèle obtenu [cite: 3]
    * Recommandation d'un produit [cite: 3]

---

## Contexte et problématique [cite: 4]

L'utilisation de la data et le développement des capacités analytiques constituent un axe majeur du plan stratégique du Groupe Carrefour à horizon 2027. L'Analytics Factory de Carrefour France est le pilier de cette transformation analytique, au service des clients et de l'ensemble des équipes métier de Carrefour[cite: 4].

La Compétition Kaggle a été organisé par l'université de bordeau en France. Elle a durée 4 mois et a vu la participation de plusieurs universtités Françaises. Mon équipe et moi avons pu participer par le Biais du partenariat entre notre école et l'université de Rènnes 2 (France). Nous partageons ainsi l'approche dévéloppée, qui nous a valu la 2nde place du classement.

Au sein des équipes de data science de l'Analytics Factory, des approches de machine learning et de recherche opérationnelle sont utilisées pour résoudre des défis commerciaux et opérationnels tels que les ruptures en magasin, la prévision des ventes promotionnelles, l'optimisation de l'assortiment des produits et les recommandations personnalisées sur carrefour.fr[cite: 5].

**L'objectif de ce défi est de prédire le réachat d'un produit par un client sur la plateforme e-commerce**[cite: 6].

Une particularité de cette problématique est la fréquence d'achat potentiellement élevée pour les courses alimentaires par rapport aux produits non alimentaires[cite: 7]. Cette fréquence peut varier considérablement entre les produits (ex: pack de yaourt vs moulin à poivre) et d'un client à l'autre[cite: 8].

Ces prédictions sont cruciales pour[cite: 9]:

* **Amélioration de la satisfaction client :** Recommander des produits pertinents améliore l'expérience d'achat, la rendant plus rapide et fluide[cite: 9]. Cela contribue à la fidélisation des clients[cite: 10].
* **Amélioration des ventes :** Proposer des produits que les clients sont susceptibles de racheter peut augmenter les ventes et les revenus[cite: 10].

---

## Description des données [cite: 11]

### 1. Base de données "Transactions" [cite: 11]

Historique des transactions d'achat de 100 000 clients sur 2 ans (2022 et 2023)[cite: 11].

**Structure :** [cite: 12, 13]

| Colonne             | Description                                                                     |
| :------------------ | :------------------------------------------------------------------------------ |
| `date`              | Date de la transaction                                                          |
| `transaction_id`    | Identifiant de la transaction                                                   |
| `customer_id`       | Identifiant du client                                                           |
| `product_id`        | Produit acheté                                                                  |
| `has_loyality_card` | Indicateur de possession de carte de fidélité                                  |
| `store_id`          | Magasin où l'achat a été effectué                                                |
| `is_promo`          | Indicateur de réduction sur le produit                                          |
| `quantity`          | Quantité achetée du produit                                                     |
| `format`            | Format de l'activité e-commerce (`clcv`, `lex`, `DRIVE`)                         |
| `orderChannelCode`  | Canal de l'activité en ligne (site web ou application mobile) [cite: 14]        |

### 2. Base de données "Products" [cite: 14]

Informations détaillées sur les produits[cite: 14].

**Structure (Exemples de colonnes) :** [cite: 15, 16, 17]

| Colonne             | Description                             |
| :------------------ | :-------------------------------------- |
| `product_id`        | Nom du produit                          |
| `product_description`| Description du produit                  |
| `department_key`    | Clé du département                     |
| `class_key`         | Clé de la classe                        |
| `subclass_key`      | Clé de la sous-classe                   |
| `sector`            | Nom du secteur                          |
| `brand_key`         | Nom de la marque                        |
| `shelf_level1`      | Catégorie de rayon (niveau 1)           |
| `shelf_level2`      | Catégorie de rayon (niveau 2)           |
| `shelf_level3`      | Catégorie de rayon (niveau 3)           |
| `shelf_level4`      | Catégorie de rayon (niveau 4)           |

### 3. Base de données "Test" [cite: 17]

Achats réels des 80 000 premiers clients en 2024 (10 premières transactions par client)[cite: 17].

**Structure :** [cite: 18, 19]

| Colonne          | Description                     |
| :--------------- | :------------------------------ |
| `transaction_id` | Identifiant de la transaction |
| `customer_id`    | Identifiant du client         |
| `product_id`     | Identifiant du produit acheté |

Les transactions de 20 000 clients en 2024 sont masquées. La tâche consiste à créer un modèle qui recommande les 10 meilleurs produits (les plus susceptibles d'être achetés en premier) pour chacun de ces 20 000 clients, en se basant sur l'historique des transactions et les informations produits[cite: 19, 20, 21].

---

## Métrique d'évaluation [cite: 22]

La performance du modèle sera évaluée avec la métrique **Hit Rate@10**[cite: 22]. Cette métrique mesure le pourcentage de produits recommandés (parmi les 10) qui sont effectivement achetés par le client lors de sa première transaction en 2024[cite: 23].

**Formule :** [cite: 24]

$HitRate@K(a, y) = \frac{1}{\min(N, 10)} \sum_{l=1}^{10} 1_{y_l \in (a_1, ..., a_N)}$

Où :
* $a = (a_1, ..., a_{10})$ sont les 10 produits recommandés (distincts).
* $y = (y_1, ..., y_N)$ sont les $N$ produits réellement achetés lors de la première transaction.
* $1_{condition}$ vaut 1 si la condition est vraie, 0 sinon.

La division par $\min(N, 10)$ permet d'obtenir un score parfait de 1 si le client achète moins de 10 produits et que tous ces produits sont correctement prédits[cite: 24]. Il est requis de prédire 10 produits **différents** pour éviter de fausser le score si un même produit est acheté plusieurs fois[cite: 25, 26].

---