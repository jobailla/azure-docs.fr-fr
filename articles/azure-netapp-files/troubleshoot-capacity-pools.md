---
title: Résoudre les problèmes de pool de capacités pour Azure NetApp Files | Microsoft Docs
description: Décrit les problèmes potentiels que vous pouvez rencontrer lors de la gestion des pools de capacités et fournit des solutions aux problèmes.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 10/13/2020
ms.author: b-juche
ms.openlocfilehash: 54e6f4abd5ca6d15a4cc5a7bc9015abb005296a0
ms.sourcegitcommit: 2c586a0fbec6968205f3dc2af20e89e01f1b74b5
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92013642"
---
# <a name="troubleshoot-capacity-pool-issues"></a>Résoudre les problèmes de pools de capacités

Cet article décrit les résolutions des problèmes que vous pouvez rencontrer lors de la gestion des pools de capacités, notamment l’opération de modification de pool. 

## <a name="issues-managing-a-capacity-pool"></a>Problèmes relatifs à la gestion d’un pool de capacités 

|     État d’erreur    |     Résolution    |
|-|-|
| Problèmes lors de la création d’un pool de capacités |  Assurez-vous que le nombre de pools de capacités ne dépasse pas la limite. Voir [Limites des ressources pour Azure NetApp Files](azure-netapp-files-resource-limits.md).  Si le nombre est inférieur à la limite et que vous rencontrez toujours des problèmes, soumettez un ticket de support et spécifiez le nom du pool de capacités. |
| Problèmes lors de la suppression d’un pool de capacités  |  Veillez à supprimer tous les volumes et instantanés Azure NetApp Files de l’abonnement dans lequel vous tentez de supprimer le pool de capacités. <br> Si vous avez déjà supprimé tous les volumes et instantanés et que vous ne pouvez toujours pas supprimer le pool de capacités, les références aux ressources peuvent encore exister sans apparaître dans le portail. Dans ce cas, soumettez un ticket de support et spécifiez que vous avez effectué les étapes recommandées ci-dessus. |
| Échec de la création ou de la modification du volume avec l’erreur `Requested throughput not available` | Le débit disponible pour un volume est déterminé par la taille et le niveau de service de son pool de capacités. Si vous ne disposez pas d’un débit suffisant, vous devez augmenter la taille du pool ou ajuster le débit du volume existant. | 

## <a name="issues-moving-a-capacity-pool"></a>Problèmes relatifs au déplacement d’un pool de capacités 
|     État d’erreur    |     Résolution    |
|-|-|
| La modification du pool de capacités pour un volume n’est pas autorisée. | Vous n’êtes peut-être pas encore autorisé à utiliser cette fonctionnalité. <br> La fonctionnalité de déplacement d’un volume vers un autre pool de capacité est actuellement disponible en préversion. Si vous utilisez cette fonctionnalité pour la première fois, vous devez commencer par l’inscrire et définir `-FeatureName ANFTierChange`. Consultez les étapes d’inscription dans [Changer dynamiquement le niveau de service d’un volume](dynamic-change-volume-service-level.md). |
| La taille du pool de capacités est trop petite pour la taille totale du volume. |  L’erreur est due au fait que le pool de capacités de destination n’a pas la capacité disponible pour le volume qui est déplacé.  <br> Augmentez la taille du pool de destination ou choisissez un autre pool plus grand.  Voir [Redimensionner un pool de capacités ou un volume](azure-netapp-files-resize-capacity-pools-or-volumes.md).   |
| Impossible de déplacer un volume lorsque le pool de capacités de destination a un type de chiffrement différent de celui du pool de capacités d’origine.  Par exemple, vous passez d’un chiffrement double à un chiffrement unique, ou vice versa.  | Sélectionnez un pool de capacités de destination avec le même type de chiffrement que le pool de capacités source.   |
|  La modification du pool ne peut pas être effectuée, car un volume appelé `'{source pool name}'` existe déjà dans le pool cible `'{target pool name}'`. | Cette erreur se produit parce qu’un volume portant le même nom existe déjà dans le pool de capacités cible.  Sélectionnez un autre pool de capacités qui n’a pas de volume portant le même nom.   | 

## <a name="next-steps"></a>Étapes suivantes  

* [Configurer un pool de capacité](azure-netapp-files-set-up-capacity-pool.md)
* [Gérer un pool de capacités de QoS manuel](manage-manual-qos-capacity-pool.md)
* [Changer dynamiquement le niveau de service d’un volume](dynamic-change-volume-service-level.md)
* [Redimensionner un pool de capacités ou un volume](azure-netapp-files-resize-capacity-pools-or-volumes.md)
