---
author: DCtheGeek
ms.service: azure-policy
ms.topic: include
ms.date: 10/20/2020
ms.author: dacoulte
ms.custom: generated
ms.openlocfilehash: 1b06c20e4c8cad1421bc328ff40570b3fa4a7572
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92346984"
---
|Nom<br /><sub>(Portail Azure)</sub> |Description |Effet(s) |Version<br /><sub>(GitHub)</sub> |
|---|---|---|---|
|[La sauvegarde Azure doit être activée pour les machines virtuelles](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F013e242c-8828-4970-87b3-ab247555486d) |Assurez la protection de vos machines virtuelles Azure en activant la sauvegarde Azure. La sauvegarde Azure est une solution de protection des données sécurisée et économique pour Azure. |AuditIfNotExists, Désactivé |[1.0.1](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachines_EnableAzureBackup_Audit.json) |
|[Configurer la sauvegarde sur les machines virtuelles d’un emplacement dans un coffre central existant au même emplacement](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2F09ce66bc-1220-4153-8104-e3f51c936913) |Cette stratégie configure la protection de Sauvegarde Azure sur les machines virtuelles d’un emplacement donné dans un coffre central existant au même emplacement. Elle s’applique uniquement aux machines virtuelles qui ne sont pas déjà configurées pour la sauvegarde. Nous vous recommandons de ne pas attribuer cette stratégie à plus de 200 machines virtuelles. Si la stratégie est attribuée à plus de 200 machines virtuelles, la sauvegarde peut être déclenchée quelques heures après la planification définie. Cette stratégie sera améliorée pour prendre en charge davantage d’images de machine virtuelle. |deployIfNotExists, auditIfNotExists, désactivé |[1.0.0](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/VirtualMachineBackup_Backup_DeployIfNotExists.json) |
|[Déployez les paramètres de diagnostic du coffre Recovery Services sur l’espace de travail Log Analytics pour les catégories propres à une ressource.](https://portal.azure.com/#blade/Microsoft_Azure_Policy/PolicyDetailBlade/definitionId/%2Fproviders%2FMicrosoft.Authorization%2FpolicyDefinitions%2Fc717fb0c-d118-4c43-ab3d-ece30ac81fb3) |Déployez les paramètres de diagnostic du coffre Recovery Services afin de les envoyer vers l’espace de travail Log Analytics pour les catégories propres à une ressource. Si l’une des catégories propres à la ressource n’est pas activée, un nouveau paramètre de diagnostic est créé. |deployIfNotExists |[1.0.1-preview](https://github.com/Azure/azure-policy/blob/master/built-in-policies/policyDefinitions/Backup/EnableRecoveryServiceVaultDiagnosticSetting_Backup_DeployIfNotExist.json) |
