---
title: Afficher les mesures avec Azure Monitor
titleSuffix: Azure Digital Twins
description: Découvrez comment afficher des métriques Azure Digital Twins dans Azure Monitor.
author: baanders
ms.author: baanders
ms.date: 8/4/2020
ms.topic: troubleshooting
ms.service: digital-twins
ms.openlocfilehash: db4383526874c98f717c5f166b296d814724184a
ms.sourcegitcommit: 2e72661f4853cd42bb4f0b2ded4271b22dc10a52
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/14/2020
ms.locfileid: "92044633"
---
# <a name="troubleshooting-azure-digital-twins-metrics"></a>Dépannage d’Azure Digital Twins : Mesures

Les métriques décrites dans le présent article vous donnent des informations sur l’état des ressources Azure Digital Twins dans votre abonnement Azure. Les métriques Azure Digital Twins vous aident à évaluer l’intégrité globale du service Azure Digital Twins et des ressources qui y sont connectées. Ces statistiques accessibles à l’utilisateur vous permettent d’effectuer le suivi de votre Azure Digital Twins et d’analyser les causes des problèmes sans avoir à contacter le support Azure.

Les métriques sont activées par défaut. Vous pouvez afficher les métriques Azure Digital Twins à partir du [portail Azure](https://portal.azure.com).

## <a name="how-to-view-azure-digital-twins-metrics"></a>Comment afficher les métriques Azure Digital Twins

1. Créez une instance Azure Digital Twins. Vous trouverez des instructions sur la configuration d’une instance Azure Digital Twins dans [*Guide pratique : Configurer une instance et l’authentification*](how-to-set-up-instance-portal.md).

2. Recherchez votre instance Azure Digital Twins dans le [portail Azure](https://portal.azure.com) (vous pouvez ouvrir la page correspondante en tapant son nom dans la barre de recherche du portail). 

    Dans le menu de l’instance, sélectionnez **Métriques**.
   
    :::image type="content" source="media/troubleshoot-metrics/azure-digital-twins-metrics.png" alt-text="Capture d’écran montrant la page des métriques pour Azure Digital Twins":::

    Cette page affiche les métriques de votre instance Azure Digital Twins. Vous pouvez également créer des vues personnalisées de vos métriques en sélectionnant dans la liste celles que vous souhaitez afficher.
    
3. Vous pouvez choisir d’envoyer vos données de métriques à un point de terminaison Event Hubs ou à un compte Stockage Azure en sélectionnant **Paramètres de diagnostic**dans le menu, puis **Ajouter un paramètre de diagnostic**.

    :::image type="content" source="media/troubleshoot-diagnostics/diagnostic-settings.png" alt-text="Capture d’écran montrant la page des métriques pour Azure Digital Twins":::

    Pour plus d’informations sur ce processus, consultez [*Résolution des problèmes : Configurer les diagnostics*](troubleshoot-diagnostics.md).

4. Vous pouvez choisir de configurer des alertes pour vos données de métriques en sélectionnant **Alertes** dans le menu, puis **+ Nouvelle règle d’alerte**.
    :::image type="content" source="media/troubleshoot-alerts/alerts-pre.png" alt-text="Capture d’écran montrant la page des métriques pour Azure Digital Twins":::

    Pour plus d’informations sur ce processus, consultez [*Résolution des problèmes : Configurer des alertes*](troubleshoot-alerts.md).

## <a name="azure-digital-twins-metrics-and-how-to-use-them"></a>Métriques Azure Digital Twins et comment les utiliser

Azure Digital Twins fournit plusieurs métriques pour vous offrir une vue d’ensemble de l’intégrité de votre instance et des ressources qui y sont associées. Vous pouvez également combiner les informations de plusieurs métriques pour obtenir une image plus grande de l’état de votre instance. 

Les tableaux suivants décrivent les métriques suivies par chaque instance Azure Digital Twins, et la façon dont chaque métrique se rattache à l’état global de votre instance.

#### <a name="api-request-metrics"></a>Métriques de demande d’API

Métriques en rapport avec des demandes d’API :

| Métrique | Nom d’affichage de la métrique | Unité | Type d’agrégation| Description | Dimensions |
| --- | --- | --- | --- | --- | --- |
| ApiRequests | Demandes d’API (préversion) | Count | Total | Nombre de demandes d’API effectuées pour des opérations de lecture, d’écriture, de suppression et de requête de Digital Twins. |  Authentification, <br>Opération, <br>Protocole, <br>Code d’état, <br>Classe de code d’état, <br>Texte d’état |
| ApiRequestsFailureRate | Taux d’échec des demandes d’API (préversion) | Pourcentage | Average | Pourcentage de demandes d’API que le service reçoit pour votre instance, qui génèrent un code de réponse d’erreur interne (500) pour une opération de lecture, d’écriture, de suppression ou de requête de Digital Twins. | Authentification, <br>Opération, <br>Protocole, <br>Code d’état, <br>Classe de code d’état, <br>Texte d’état
| ApiRequestsLatency | Latence des demandes d’API (préversion) | Millisecondes | Average | Temps de réponse aux demandes d’API. Il s’agit du temps qui s’écoule entre le moment où Azure Digital Twins reçoit la demande et le moment où le service envoie un résultat de réussite ou d’échec d’une opération de lecture, d’écriture, de suppression ou de requête de Digital Twins. | Authentification, <br>Opération, <br>Protocol |

#### <a name="billing-metrics"></a>Métriques de facturation

Métriques en rapport avec la facturation :

>[!NOTE]
> Pendant la période de préversion, la **facturation est zéro**. Bien que ces métriques apparaissent toujours dans la liste sélectionnable, elles ne s’appliquent pas au cours de la période de préversion et restent à zéro jusqu’à ce que le service sorte de la préversion.

| Métrique | Nom d’affichage de la métrique | Unité | Type d’agrégation| Description | Dimensions |
| --- | --- | --- | --- | --- | --- |
| BillingApiOperations | Opérations de l’API de facturation (préversion) | Count | Total | Métrique de facturation pour le nombre total des demandes d’API adressées au service Azure Digital Twins. | ID du compteur |
| BillingMessagesProcessed | Messages de facturation traités (préversion) | Count | Total | Métrique de facturation pour le nombre de messages envoyés à partir d’Azure Digital Twins vers des points de terminaison externes.<br><br>Pour être considérée comme un message unique à des fins de facturation, une charge utile ne doit pas dépasser 1 Ko. Les charges utiles supérieures à cette taille sont comptabilisées en tant que messages supplémentaires par incréments de 1 Ko (par conséquent, un message d’une taille comprise entre 1 et 2 Ko sera comptabilisé comme 2 messages, d’une taille comprise entre 2 et 3 Ko comme 3 messages, et ainsi de suite).<br>Cette restriction s’applique également aux réponses. Par exemple, un appel retournant 1,5 Ko dans le corps de la réponse est facturé comme s’il s’agissait de 2 opérations. | ID du compteur |
| BillingQueryUnits | Unités de requête de facturation (préversion) | Count | Total | Nombre d’unités de requête, mesure calculée en interne de l’utilisation des ressources des services, consommées pour exécuter des requêtes. Une API d’assistance est également disponible pour mesurer les unités de requête : [Classe QueryChargeHelper](/dotnet/api/azure.digitaltwins.core.querychargehelper?preserve-view=true&view=azure-dotnet-preview) | ID du compteur |

#### <a name="ingress-metrics"></a>Métriques d’entrée

Métriques en rapport avec l’entrée de données :

| Métrique | Nom d’affichage de la métrique | Unité | Type d’agrégation| Description | Dimensions |
| --- | --- | --- | --- | --- | --- |
| IngressEvents | Événements d’entrée (préversion) | Count | Total | Nombre d’événements de télémétrie entrants dans Azure Digital Twins. | Résultats |
| IngressEventsFailureRate | Taux d’échec des demandes des événement d'entrée (préversion) | Pourcentage | Average | Pourcentage d’événements de télémétrie entrants pour lesquels le service retourne un code de réponse d’erreur interne (500). | Résultats |
| IngressEventsLatency | Latence des événement d'entrée (préversion) | Millisecondes | Average | Heure à laquelle un événement arrive lorsqu’il est prêt à être sortie par Azure Digital Twins, auquel cas le service envoie un résultat de réussite/échec. | Résultats |

#### <a name="routing-metrics"></a>Métriques de routage

Métriques en rapport avec le routage :

| Métrique | Nom d’affichage de la métrique | Unité | Type d’agrégation| Description | Dimensions |
| --- | --- | --- | --- | --- | --- |
| MessagesRouted | Messages routés (préversion) | Count | Total | Nombre de messages routés vers un service Azure de point de terminaison, tel que Event Hub, Service Bus ou Event Grid. | Type de point de terminaison, <br>Résultats |
| RoutingFailureRate | Taux d’échec de routage (préversion) | Pourcentage | Average | Pourcentage d’événements qui génèrent une erreur lors de leur routage à partir d’Azure Digital Twins vers un service Azure de point de terminaison, tel que Event Hub, Service Bus ou Event Grid. | Type de point de terminaison, <br>Résultats |
| RoutingLatency | Latence du routage (préversion) | Millisecondes | Average | Temps écoulé entre le routage d’un événement à partir d’Azure Digital Twins et sa publication sur le service Azure de point de terminaison, tel que Event Hub, Service Bus ou Event Grid. | Type de point de terminaison, <br>Résultats |

## <a name="dimensions"></a>Dimensions

Les dimensions permettent d’identifier plus de détails sur les métriques. Certaines des métriques de routage fournissent des informations pour chaque point de terminaison. Le tableau ci-dessous répertorie les valeurs possibles de ces dimensions.

| Dimension | Valeurs |
| --- | --- |
| Authentification | OAuth |
| Opération (pour les demandes d’API) | Microsoft.DigitalTwins/digitaltwins/delete, <br>Microsoft.DigitalTwins/digitaltwins/write, <br>Microsoft.DigitalTwins/digitaltwins/read, <br>Microsoft.DigitalTwins/eventroutes/read, <br>Microsoft.DigitalTwins/eventroutes/write, <br>Microsoft.DigitalTwins/eventroutes/delete, <br>Microsoft.DigitalTwins/models/read, <br>Microsoft.DigitalTwins/models/write, <br>Microsoft.DigitalTwins/models/delete, <br>Microsoft.DigitalTwins/query/action |
| Type de point de terminaison | Event Grid, <br>Event Hub, <br>Service Bus |
| Protocol | HTTPS |
| Résultats | Réussite, <br>Échec |
| Code d’état | 200, 404, 500, etc. |
| Classe de code d’état | 2xx, 4xx, 5xx, etc. |
| Texte d’état | Erreur de serveur interne, Introuvable, etc. |

## <a name="next-steps"></a>Étapes suivantes

Pour en savoir plus sur la gestion des métriques enregistrées pour Azure Digital Twins, consultez [*résolution des problèmes : Configurer les diagnostics*](troubleshoot-diagnostics.md).