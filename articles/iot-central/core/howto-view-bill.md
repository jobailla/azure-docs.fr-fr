---
title: Gérer sa facture et passer du plan Gratuit au plan Standard dans l’application Azure IoT Central | Microsoft Docs
description: En tant qu’administrateur, découvrez comment gérer votre facture et passer du plan tarifaire Gratuit au plan Standard dans votre application Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 12/09/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
manager: philmea
ms.openlocfilehash: 515f5f665e947ff9594cce6dbbaea9b5e0c50ebf
ms.sourcegitcommit: 090ea6e8811663941827d1104b4593e29774fa19
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91999708"
---
# <a name="manage-your-bill-in-an-iot-central-application"></a>Gérer sa facture dans une application IoT Central

Cet article explique aux administrateurs comment gérer la facturation Azure IoT Central. Vous pouvez migrer une application du plan tarifaire Gratuit au plan Standard et passer à un niveau tarifaire supérieur ou inférieur.

Pour accéder à la section **Administration**, vous devez disposer du rôle *Administrateur* ou d’un *rôle d’utilisateur personnalisé* permettant de voir la facturation. Si vous créez une application Azure IoT Central, le rôle **Administrateur** vous est automatiquement attribué.

## <a name="move-from-free-to-standard-pricing-plan"></a>Passer du plan tarifaire Gratuit au plan Standard

- Les applications qui utilisent le plan Gratuit sont gratuites pendant sept jours, puis elles expirent. Pour éviter de perdre vos données, vous pouvez les déplacer vers un plan tarifaire Standard avant leur expiration.
- Les applications qui utilisent un plan tarifaire Standard sont facturées par appareil. Cependant, chaque application est gratuite pour les deux premiers appareils qui l’utilisent.

Apprenez-en davantage au sujet de la tarification sur la [page de tarification d’Azure IoT Central](https://azure.microsoft.com/pricing/details/iot-central/).

Dans la section des tarifs, vous pouvez faire passer votre application du plan tarifaire Gratuit au plan Standard.

Pour mener à bien ce processus en libre-service, effectuez ces étapes :

1. Accédez à la page **Tarifs** de la section **Administration**.

    ![État de la version d'évaluation](media/howto-view-bill/freetrialbilling.png)

1. Sélectionnez **Convert to a paid plan** (Passer à un plan payant).

    ![Convertir la version d'évaluation](media/howto-view-bill/convert.png)

1. Sélectionnez le service Azure Active Directory approprié, ainsi que l’abonnement Azure à utiliser pour l’application associée au plan payant.

1. Lorsque vous sélectionnez **Convertir**, votre application devient payante et son utilisation vous est donc facturée.

> [!Note]
> Par défaut, vous passez à un plan tarifaire *Standard 2*.

## <a name="how-to-change-your-application-pricing-plan"></a>Modifier le plan tarifaire de votre application

Les applications qui utilisent un plan tarifaire Standard sont facturées par appareil. Cependant, chaque application est gratuite pour les deux premiers appareils qui l’utilisent.

Dans la section des tarifs, vous pouvez passer à un niveau tarifaire Azure IoT inférieur ou supérieur à celui dont vous disposez actuellement.

1. Accédez à la page **Tarifs** de la section **Administration**.

    ![Mise à niveau du plan tarifaire](media/howto-view-bill/pricing.png)

1. Sélectionnez le **Plan**, puis sélectionnez **Enregistrer** pour passer à un niveau tarifaire supérieur ou inférieur.

## <a name="view-your-bill"></a>Consulter votre facture

1. Sélectionnez le service Azure Active Directory approprié, ainsi que l’abonnement Azure à utiliser pour l’application associée au plan payant.

1. Lorsque vous sélectionnez **Convertir**, votre application devient payante et son utilisation vous est donc facturée.

> [!Note]
> Par défaut, vous passez à un plan tarifaire *Standard 2*.

## <a name="next-steps"></a>Étapes suivantes

Maintenant que vous savez comment gérer votre facture dans l’application Azure IoT Central, nous vous conseillons de découvrir comment [Personnaliser l’interface utilisateur de l’application](howto-customize-ui.md) dans Azure IoT Central.