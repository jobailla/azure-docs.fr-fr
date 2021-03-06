---
title: Utilisation de tableaux et objets dans Azure Cosmos DB
description: Découvrez la syntaxe SQL permettant de créer des tableaux et des objets dans Azure Cosmos DB. Cet article fournit également quelques exemples pour effectuer des opérations sur des objets de tableau
author: timsander1
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/02/2019
ms.author: tisande
ms.openlocfilehash: 2b882e1e39f035d27fc6d09d1a9d0c04691b499c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "89426246"
---
# <a name="working-with-arrays-and-objects-in-azure-cosmos-db"></a>Utilisation de tableaux et objets dans Azure Cosmos DB

Une fonctionnalité essentielle de l’API SQL Azure Cosmos DB est la création de tableau et d’objet.

## <a name="arrays"></a>Tableaux

Vous pouvez construire des tableaux, comme l’illustre l’exemple suivant :

```sql
SELECT [f.address.city, f.address.state] AS CityState
FROM Families f
```

Les résultats sont :

```json
[
  {
    "CityState": [
      "Seattle",
      "WA"
    ]
  },
  {
    "CityState": [
      "NY", 
      "NY"
    ]
  }
]
```

Vous pouvez également utiliser l’[expression ARRAY](sql-query-subquery.md#array-expression) pour construire un tableau à partir des résultats d’une [sous-requête](sql-query-subquery.md). Cette requête obtient tous les noms donnés distincts des enfants dans un tableau.

```sql
SELECT f.id, ARRAY(SELECT DISTINCT VALUE c.givenName FROM c IN f.children) as ChildNames
FROM f
```

## <a name="iteration"></a><a id="Iteration"></a>Itération

L’API SQL prend en charge l’itération sur les tableaux JSON, avec une nouvelle construction ajoutée par le biais du [mot clé IN](sql-query-keywords.md#in) dans la source FROM. Dans l’exemple suivant :

```sql
SELECT *
FROM Families.children
```

Les résultats sont :

```json
[
  [
    {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy"}]
    }
  ], 
  [
    {
        "familyName": "Merriam",
        "givenName": "Jesse",
        "gender": "female",
        "grade": 1
    }, 
    {
        "familyName": "Miller",
        "givenName": "Lisa",
        "gender": "female",
        "grade": 8
    }
  ]
]
```

La requête suivante effectue une itération sur `children` dans le conteneur `Families`. Le tableau obtenu est différent de la requête précédente. Cet exemple fractionne `children` et regroupe les résultats en un seul tableau :  

```sql
SELECT *
FROM c IN Families.children
```

Les résultats sont :

```json
[
  {
      "firstName": "Henriette Thaulow",
      "gender": "female",
      "grade": 5,
      "pets": [{ "givenName": "Fluffy" }]
  },
  {
      "familyName": "Merriam",
      "givenName": "Jesse",
      "gender": "female",
      "grade": 1
  },
  {
      "familyName": "Miller",
      "givenName": "Lisa",
      "gender": "female",
      "grade": 8
  }
]
```

Vous pouvez même filtrer chaque entrée du tableau, comme dans l’exemple suivant :

```sql
SELECT c.givenName
FROM c IN Families.children
WHERE c.grade = 8
```

Les résultats sont :

```json
[{
  "givenName": "Lisa"
}]
```

Vous pouvez également effectuer des agrégations sur le résultat d’une itération de tableau. Par exemple, la requête suivante compte le nombre d’enfants parmi toutes les familles :

```sql
SELECT COUNT(child)
FROM child IN Families.children
```

Les résultats sont :

```json
[
  {
    "$1": 3
  }
]
```

## <a name="next-steps"></a>Étapes suivantes

- [Bien démarrer](sql-query-getting-started.md)
- [Exemples .NET Azure Cosmos DB](https://github.com/Azure/azure-cosmos-dotnet-v3)
- [Joins](sql-query-join.md)
