---
title: Utiliser le module Scorer un modèle d’image
titleSuffix: Azure Machine Learning
description: Découvrez comment utiliser le module Score Image Model dans Azure Machine Learning pour générer des prédictions à l’aide d’un modèle d’image entraîné.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 05/26/2020
ms.openlocfilehash: 88ca997e2d22283babf582b10d9b0eeb7de122c0
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90905192"
---
# <a name="score-image-model"></a>Scorer un modèle d’image

Cet article décrit un module dans le concepteur Azure Machine Learning.

Utilisez ce module pour générer des prédictions à l’aide d’un modèle d’image entraîné sur des données d’image d’entrée.

## <a name="how-to-configure-score-image-model"></a>Comment configurer le module Score Image Model

1. Ajoutez le module **Score Image Model** à votre pipeline.

2. Attachez un modèle d’image entraîné et un jeu de données contenant des données d’image d’entrée. 

    Les données doivent être de type ImageDirectory. Consultez le module [Convert to Image Directory](convert-to-image-directory.md) (Convertir en répertoire d’images) pour plus d’informations sur l’obtention d’un répertoire d’images. Le schéma du jeu de données d’entrée doit correspondre aussi généralement au schéma des données utilisées pour effectuer l’apprentissage du modèle.

3. Envoyez le pipeline.

## <a name="results"></a>Résultats

Après avoir généré un ensemble de scores à l’aide du module [Score Image Model](score-image-model.md), si vous souhaitez générer un ensemble de métriques pour évaluer la justesse (le niveau de performance) du modèle, vous pouvez connecter ce module et le jeu de données doté de scores au module [Evaluate Model](evaluate-model.md), 

### <a name="publish-scores-as-a-web-service"></a>Publier des scores en tant que service web

Une utilisation courante du scoring consiste à retourner la sortie en tant que partie d’un service web prédictif. Pour plus d’informations, consultez [ce didacticiel](https://docs.microsoft.com/azure/machine-learning/tutorial-designer-automobile-price-deploy) sur le déploiement d’un point de terminaison en temps réel basé sur un pipeline dans le concepteur Azure Machine Learning.

## <a name="next-steps"></a>Étapes suivantes

Consultez [l’ensemble des modules disponibles](module-reference.md) pour Azure Machine Learning. 
