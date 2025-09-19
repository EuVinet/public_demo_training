
# Atelier Genie AI BI – Databricks

## 1. Création d’un Workspace Databricks gratuit

1. Rendez-vous sur https://www.databricks.com/learn/free-edition.
2. Cliquez sur **S’inscrire**.
3. Inscrivez-vous avec votre adresse e-mail.
4. Choisissez **AWS** (recommandé) ou **Azure** comme cloud.
5. Activez votre compte via le lien de validation reçu dans votre messagerie.
6. Connectez-vous à votre workspace Databricks.

***

## 2. Accéder au dataset « bakehouse »

### Option 1 : accès direct via les "samples" disponibles

- Dans le menu de gauche, cliquez sur **Catalog**.
- Dans la section **Delta Shares**, sélectionnez **samples**.
- Vérifiez que le dataset **bakehouse** (ou **bakehouse_demo**) est disponible.
- S’il y est, passez directement à l’étape suivante.


### Option 2 : installer depuis la Marketplace

1. Si le dataset n’est pas dans « samples » :
    - Cliquez sur **Marketplace** dans le menu de gauche.
    - Recherchez **cookie** dans la barre de recherche.
    - Sélectionnez **Cookies Dataset DAIS 2024**.
    - Cliquez sur **Get Instant Access**.
    - Renommez le dataset en **bakehouse_demo** si besoin.
    - Acceptez les politiques d’utilisation, puis cliquez sur **Install**.
    - Cliquez sur **Open** une fois l’installation terminée.

***

## 3. Explorer les données dans Catalog

- Retournez dans **Catalog**.
- Repérez le dataset **bakehouse** dans **samples** ou dans le dossier issu de la Marketplace.
- Cliquez dessus pour visualiser les tables disponibles.

***

## 4. Créer un espace Genie

1. Ouvrez l’application **Genie**.
2. Cliquez sur **New** pour créer un nouvel espace Genie.
3. Sélectionnez :
    - **samples**
    - puis **bakehouse**
    - puis **toutes les tables** du dataset.
4. Cliquez sur **Créer**.
5. Votre espace Genie est prêt.

***

## 5. Comprendre la donnée à disposition

- Dans votre nouvel espace Genie, cliquez sur **Explain the dataset**.

***

## 6. Ajouter des instructions métier

- Dans le volet **Instructions : Text**, copiez-collez les lignes suivantes en anglais :

```
* totalPrice column is revenue and sales volume
* Always include franchise name with franchise ID
* Use totalPrice column for total sales and performance comparison
* Fiscal year starts in February of each year
* First quarter is February, March, April
* Second quarter is May, June, July
* Third quarter is August, September, October
* Fourth quarter is November, December, January
```


***

## 7. Créer les Jointures

1. Allez dans l’onglet **Instructions : Joins**.
2. Cliquez sur **Joins**.
3. Créez les jointures suivantes en mode **Many to One** :
    - Joignez **sales_transactions** avec **sales_franchises** sur la condition `franchiseID = FranchiseID`.
    - Joignez **media_fold_review_chuncked** avec **sales_franchises** sur la condition `franchiseID = FranchiseID` (toujours en **Many to One**).

***

## 8. Sélectionner les données exposées à l'utilisateur

- Choisissez les tables et colonnes à exposer selon les consignes de la séance.
- On veut eviter de garder les transactions et informations de carte bancaire.

***

## 9. Configurer un exemple de question

1. Allez dans **Configure** puis **Sample Questions**.
2. Ajoutez :
`What was our revenue in May 2024?`

***

## 10. Decouverte des fonctionnalités de Monitoring et de Benchmark 

Voir explications et exemple en séance.

***

## 10. Ajouter une instruction et un exemple pour l'analyse de sentiment

- Ajoutez dans **Instructions : SQL Queries** la question suivante en anglais :

```
What is the sentiment of reviews for franchise Sweetie Pies ?
```

- Et sa requête, qui va permettre à Genie d'utiliser ai_analyze_sentiment.
  On est en train d'utiliser les ai_functions de Databricks, qui sont des fonctions basés sur des modèles d'IA requêtables en SQL.
  [Voir Ici](https://learn.microsoft.com/en-gb/azure/databricks/large-language-models/ai-functions)

```sql
SELECT
  `media_customer_reviews`.`review`,
  ai_analyze_sentiment( `media_customer_reviews`.`review`) AS sentiment
FROM
  `samples`.`bakehouse`.`media_customer_reviews`
    JOIN `samples`.`bakehouse`.`sales_franchises`
      ON `media_customer_reviews`.`franchiseID` = `sales_franchises`.`franchiseID`
WHERE
  `sales_franchises`.`name` = 'Sweetie Pies'
```
Vous pouvez maintenant demander à Genie : 
```
What is the overall sentiment for all reviews of Butter Babies?
```

***
