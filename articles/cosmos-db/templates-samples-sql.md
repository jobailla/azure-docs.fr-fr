---
title: Modèles Azure Resource Manager pour Core (API SQL) Azure Cosmos DB
description: Utilisez des modèles Azure Resource Manager pour créer et configurer Azure Cosmos DB.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/14/2020
ms.author: mjbrown
ms.openlocfilehash: 3247f35fec8615b9c5e277b7453c340caaf56ffd
ms.sourcegitcommit: b6f3ccaadf2f7eba4254a402e954adf430a90003
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/20/2020
ms.locfileid: "92283112"
---
# <a name="azure-resource-manager-templates-for-azure-cosmos-db"></a>Modèles Azure Resource Manager pour Azure Cosmos DB

Cet article présente uniquement des exemples de modèles Azure Resource Manager pour des comptes de l’API Core (SQL). Vous trouverez également des exemples de modèles pour les API [Cassandra](templates-samples-cassandra.md), [Gremlin](templates-samples-gremlin.md), [MongoDB](templates-samples-mongodb.md) et [Table](templates-samples-table.md).

## <a name="core-sql-api"></a>API Core (SQL)

|**Modèle**|**Description**|
|---|---|
|[Créer un compte, une base de données et un conteneur Azure Cosmos avec un débit de mise à l’échelle automatique](manage-with-templates.md#create-autoscale) | Ce modèle crée un compte d’API Core (SQL) dans deux régions, une base de données et un conteneur avec un débit de mise à l’échelle automatique. |
|[Créer un compte, une base de données et un conteneur Azure Cosmos avec un magasin analytique](manage-with-templates.md#create-analytical-store) | Ce modèle crée un compte d’API Core (SQL) dans une région avec un conteneur configuré avec une durée de vie Analytical TTL activée et une option d’utilisation de débit manuel ou de mise à l’échelle automatique. |
|[Créer un compte, une base de données et un conteneur Azure Cosmos avec un débit (manuel) standard](manage-with-templates.md#create-manual) | Ce modèle crée un compte d’API Core (SQL) dans deux régions, une base de données et un conteneur avec un débit standard. |
|[Créer un compte Azure Cosmos, une base de données et un conteneur avec une procédure stockée, un déclencheur et une fonction définie par l’utilisateur](manage-with-templates.md#create-sproc) | Ce modèle crée un compte d’API Core (SQL) dans deux régions avec une procédure stockée, un déclencheur et un UDF pour un conteneur. |
|[Créer un point de terminaison privé pour un compte Azure Cosmos](how-to-configure-private-endpoints.md#create-a-private-endpoint-by-using-a-resource-manager-template) |  Ce modèle crée un point de terminaison privé pour un compte d’API Core (SQL) Azure Cosmos existant dans un réseau virtuel existant. |
|[Créer un compte Azure Cosmos gratuit](manage-with-templates.md#free-tier) |  Ce modèle crée un compte d’API Core (SQL) Azure Cosmos DB au niveau de service gratuit. |

Pour consulter la documentation de référence, voir [Référence Azure Resource Manager pour Azure Cosmos DB](/azure/templates/microsoft.documentdb/allversions).
