---
title: Sécurité pour les solutions Azure Digital Twins
titleSuffix: Azure Digital Twins
description: Comprendre les meilleures pratiques de sécurité avec Azure Digital Twins.
author: baanders
ms.author: baanders
ms.date: 3/18/2020
ms.topic: conceptual
ms.service: digital-twins
ms.openlocfilehash: 0b99b9034dc382552d292cef95a3790bb27eba89
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331751"
---
# <a name="secure-azure-digital-twins"></a>Sécuriser Azure Digital Twins

Pour assurer la sécurité, Azure Digital Twins permet un contrôle d’accès précis sur des données, ressources et actions spécifiques de votre déploiement. En effet, il utilise une gestion granulaire des rôles et une stratégie de gestion des autorisations appelée **contrôle d’accès en fonction du rôle Azure (Azure RBAC)** . Pour plus d’informations sur les principaux généraux d’Azure RBAC, voir [ici](../role-based-access-control/overview.md).

Azure Digital Twins prend également en charge le chiffrement des données au repos.

## <a name="granting-permissions-with-azure-rbac"></a>Accorder des autorisations avec le contrôle d’accès en fonction du rôle (RBAC) Azure

Azure RBAC est fourni à Azure Digital Twins via l’intégration à [Azure Active Directory](../active-directory/fundamentals/active-directory-whatis.md) (Azure AD).

Vous pouvez utiliser Azure RBAC pour accorder des autorisations à un *principal de sécurité* , qui peut être un utilisateur, un groupe ou un principal de service d’application. Le principal de sécurité est authentifié par Azure AD et reçoit un jeton OAuth 2.0 en retour. Ce jeton peut être utilisé pour autoriser une demande d’accès à une instance Azure Digital Twins.

### <a name="authentication-and-authorization"></a>Authentification et autorisation

Avec Azure AD, l’accès est un processus en deux étapes. Quand un principal de sécurité (un utilisateur, un groupe ou une application) tente d’accéder à une entité Azure Digital Twins, la requête doit être *authentifiée* et *autorisée* . 

1. Pour commencer, l’identité du principal de sécurité est *authentifiée* , et un jeton OAuth 2.0 est renvoyé.
2. Ensuite, ce jeton est transmis dans une requête adressée au service Azure Digital Twins pour *autoriser* l’accès à la ressource spécifiée.

L’étape d’authentification nécessite que toute requête d’application contienne un jeton d’accès OAuth 2.0 au moment de l’exécution. Si une application s’exécute à partir d’une entité Azure telle qu’une application [Azure Functions](../azure-functions/functions-overview.md), elle peut utiliser une **identité managée** pour accéder aux ressources. Vous trouverez plus d'informations sur les identités managées dans la section suivante.

L’étape d’autorisation exige qu’un rôle Azure soit attribué au principal de sécurité. Les rôles qui sont attribués à un principal de sécurité déterminent les autorisations dont disposera le principal. Azure Digital Twins fournit des rôles Azure qui englobent des ensembles d’autorisations pour les ressources Azure Digital Twins. Ces rôles sont décrits plus loin dans cet article.

Pour en savoir plus sur les rôles et les attributions de rôles pris en charge dans Azure, consultez [*Comprendre les différents rôles*](../role-based-access-control/rbac-and-directory-admin-roles.md) dans la documentation RBAC Azure.

#### <a name="authentication-with-managed-identities"></a>Authentification avec des identités managées

La fonctionnalité [Identités managées pour les ressources Azure](../active-directory/managed-identities-azure-resources/overview.md) vous permet de créer une identité sécurisée associée au déploiement où s’exécute le code de votre application. Vous pouvez ensuite associer cette identité à des rôles de contrôle d’accès pour accorder des autorisations personnalisées pour l’accès aux ressources Azure nécessaires à votre application.

Avec les identités managées, la plateforme Azure gère cette identité d’exécution. Vous n’avez pas besoin de stocker et de protéger des clés d’accès dans le code ou la configuration de votre application, que ce soit pour l’identité elle-même ou pour les ressources auxquelles vous devez accéder. Une application cliente Azure Digital Twins en cours d’exécution à l’intérieur d’une application Azure App Service n’a pas besoin de gérer des clés et des règles SAS ou d’autres jetons d’accès. L’application cliente a uniquement besoin de l’adresse de point de terminaison de l’espace de noms Azure Digital Twins. Lorsque l’application se connecte, Azure Digital Twins lie le contexte de l’entité managée au client. Une fois associé à une identité managée, votre client Azure Digital Twins peut effectuer toutes les opérations autorisées. L’autorisation sera ensuite accordée en associant une entité gérée à un rôle Azure dans Azure Digital Twins (décrit ci-dessous).

#### <a name="authorization-azure-roles-for-azure-digital-twins"></a>Autorisation : Rôles Azure pour Azure Digital Twins

Azure fournit les rôles intégrés Azure ci-dessous pour autoriser l’accès à une ressource Azure Digital Twins :
* *Propriétaire Azure Digital Twins (préversion)*  : utilisez ce rôle pour accorder un accès total aux ressources Azure Digital Twins.
* *Lecteur Azure Digital Twins (préversion)*  : utilisez ce rôle pour accorder un accès en lecture seule aux ressources Azure Digital Twins.

> [!TIP]
> Le rôle de *lecteur Azure Digital Twins (préversion)* prend désormais également en charge les relations de navigation.

Pour plus d’informations sur la définition des rôles intégrés, consultez [*Comprendre les définitions de rôles*](../role-based-access-control/role-definitions.md) dans la documentation RBAC Azure. Pour plus d’informations sur la création de rôles personnalisés Azure, consultez [*Rôles personnalisés Azure*](../role-based-access-control/custom-roles.md).

Vous pouvez attribuer des rôles de deux manières :
* Via le volet de contrôle d’accès (IAM) pour Azure Digital Twins dans le portail Azure (consultez [*Ajouter ou supprimer des attributions de rôles Azure à l’aide du portail Azure*](../role-based-access-control/role-assignments-portal.md))
* Via les commandes CLI pour ajouter ou supprimer un rôle

Pour plus d’informations sur la procédure à suivre, testez le [*tutoriel Azure Digital Twins : Connecter une solution de bout en bout*](tutorial-end-to-end.md).

### <a name="permission-scopes"></a>Étendues d’autorisation

Avant d’attribuer un rôle Azure à un principal de sécurité, déterminez l’étendue de l’accès dont doit disposer le principal de sécurité. Selon les bonnes pratiques, il est préférable d’accorder la plus petite étendue possible.

La liste suivante décrit les niveaux auxquels vous pouvez étendre l’accès aux ressources Azure Digital Twins.
* Modèles : Les actions de cette ressource dictent le contrôle de [modèles](concepts-models.md) chargés dans Azure Digital Twins.
* Interroger le graphique de jumeaux numériques : Les actions de cette ressource déterminent la capacité à exécuter des [opérations de requête](concepts-query-language.md) sur des jumeaux numériques dans le graphique Azure Digital Twins.
* Jumeau numérique : les actions de cette ressource permettent de contrôler les opérations CRUD sur les [jumeaux numériques](concepts-twins-graph.md) dans le graphique des jumeaux numériques.
* Relation de jumeaux numériques : les actions de cette ressource définissent le contrôle des opérations CRUD sur les opérations relatives aux [relations](concepts-twins-graph.md) entre des jumeaux numériques dans le graphique des jumeaux.
* Itinéraire des événements : les actions de cette ressource déterminent les autorisations de [routage d’événements](concepts-route-events.md) d’Azure Digital Twins vers un service de point de terminaison comme [Event Hub](../event-hubs/event-hubs-about.md), [Event Grid](../event-grid/overview.md)ou [Service Bus](../service-bus-messaging/service-bus-messaging-overview.md).

### <a name="troubleshooting-permissions"></a>Résolution des problèmes d'autorisations

Si un utilisateur tente d’effectuer une action qui n’est pas autorisée par son rôle, il peut recevoir un message d’erreur `403 (Forbidden)` de la demande de service. Si vous souhaitez en savoir plus, également sur les étapes à suivre pour le dépannage, consultez [*Résolution des problèmes : échec de la requête Azure Digital Twins avec l’état : 403 (Interdit)*](troubleshoot-error-403.md).

## <a name="encryption-of-data-at-rest"></a>Chiffrement des données au repos

Azure Digital Twins assure le chiffrement des données au repos et en transit à mesure qu'elles sont écrites dans nos centres de données, et les déchiffre pour vous lorsque vous y accédez. Ce chiffrement s'effectue à l'aide d'une clé de chiffrement gérée par Microsoft.

## <a name="cross-origin-resource-sharing-cors"></a>Partage des ressources cross-origin (CORS)

Azure Digital Twins ne prend pas actuellement en charge **le partage des ressources cross-origin (CORS)** . Par conséquent, si vous appelez une API REST à partir d’une application de navigateur, d’une interface de [Gestion des API (APIM)](../api-management/api-management-key-concepts.md) ou d’un connecteur [Power Apps](https://docs.microsoft.com/powerapps/powerapps-overview), un message d’erreur de stratégie peut s’afficher.

Pour résoudre cette erreur, vous pouvez effectuer l’une des opérations suivantes :
* Supprimez l’en-tête CORS `Access-Control-Allow-Origin` du message. Cet en-tête indique si la réponse peut être partagée. 
* Vous pouvez également créer un proxy CORS et envoyer la requête de l’API REST Azure Digital Twins à travers celui-ci. 

## <a name="next-steps"></a>Étapes suivantes

* Consultez ces concepts en action dans [*Guide pratique : Configurer une instance et l’authentification*](how-to-set-up-instance-portal.md).

* Voyez comment interagir avec ces concepts à partir du code d’application cliente dans [*Guide pratique : Écrire le code d’authentification d’une application*](how-to-authenticate-client.md).

* Découvrez plus d’informations sur [Azure RBAC](../role-based-access-control/overview.md).
