---
title: 'Démarrage rapide : Créer un cache de niveau Entreprise'
description: Dans ce guide de démarrage rapide, vous allez apprendre à créer une instance Azure Cache pour Redis de niveau Entreprise
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.custom: mvc
ms.topic: quickstart
ms.date: 05/12/2020
ms.openlocfilehash: 3df6cb1afe8a6249eadbbd3f61619e66be2d2478
ms.sourcegitcommit: eb6bef1274b9e6390c7a77ff69bf6a3b94e827fc
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/05/2020
ms.locfileid: "83402489"
---
# <a name="quickstart-create-an-enterprise-tier-cache-preview"></a>Démarrage rapide : Créer un cache de niveau Entreprise (préversion)

Les niveaux Enterprise d’Azure Cache pour Redis offrent des instances [Redis Enterprise](https://redislabs.com/redis-enterprise/) entièrement intégrées et gérées. Ils sont actuellement disponibles en préversion. Il existe deux nouveaux niveaux dans cette préversion :
* Enterprise, qui utilise la mémoire volatile (DRAM) sur une machine virtuelle pour stocker les données
* Enterprise SSD, qui utilise à la fois la mémoire volatile et la mémoire non volatile (NVMe) pour stocker les données.

La participation à la préversion ne coûte rien. Inscrivez-vous via la [Place de marché Azure](https://aka.ms/redispreviewsignup/) **Contactez-moi** si l’offre vous intéresse. Nous avons un nombre très limité de places pour les participants et ne pouvons pas garantir que votre demande de participation à la préversion sera acceptée.

## <a name="prerequisites"></a>Prérequis

Vous aurez besoin d’un abonnement Azure avant de commencer. Si vous n’en avez pas, commencez par [créer un compte gratuit](https://azure.microsoft.com/free/).

## <a name="create-a-cache"></a>Création d'un cache
1. Pour créer un cache, connectez-vous au portail Azure via le lien dans votre invitation à la préversion, puis sélectionnez **Créer une ressource**.

   > [!IMPORTANT] 
   > Ne vous abonnez pas à *Azure Cache pour Redis, niveaux Enterprise* directement dans la place de marché.
   > Cette étape est effectuée par l’interface utilisateur du portail Azure Cache pour Redis automatiquement.
   >
   
1. Dans la page **Nouvelle**, sélectionnez **Bases de données**, puis **Azure Cache pour Redis**.
   
   ![Sélectionner Azure Cache pour Redis](media/cache-create/new-cache-menu.png)
   
1. Dans la page **Nouveau cache Redis**, configurez les paramètres du nouveau cache.
   
   | Paramètre      | Valeur suggérée  | Description |
   | ------------ |  ------- | -------------------------------------------------- |
   | **Abonnement** | Dans la liste déroulante, sélectionnez votre abonnement. | Abonnement sous lequel créer cette nouvelle instance d’Azure Cache pour Redis. | 
   | **Groupe de ressources** | Dans la liste déroulante, sélectionnez un groupe de ressources ou choisissez **Créer nouveau**, puis entrez un nouveau nom de groupe de ressources. | Nom du groupe de ressources dans lequel créer votre cache et d’autres ressources. En plaçant toutes les ressources de votre application dans un seul groupe de ressources, vous pouvez facilement les gérer ou les supprimer ensemble. | 
   | **Nom DNS** | Entrez un nom globalement unique. | Le nom du cache doit être une chaîne de 1 à 63 caractères ne contenant que des chiffres, des lettres et des traits d’union. Le nom doit commencer et se terminer par un chiffre ou une lettre, et ne peut pas contenir de traits d’union consécutifs. Le *nom d’hôte* de votre instance de cache sera *\<DNS name>.<Azure region>.redisenterprise.cache.azure.net*. | 
   | **Lieu** | Dans la liste déroulante, sélectionnez un emplacement. | Les niveaux Enterprise sont disponibles dans les régions USA Ouest, USA Est 2 et Europe Ouest. |
   | **Niveau de cache** | Ouvrez le menu déroulant et sélectionnez un niveau *Enterprise DRAM* ou *Enterprise SSD* et une taille. |  Le niveau détermine la taille, les performances et les fonctionnalités disponibles pour le cache. |
   
   ![Notions de base sur le niveau Enterprise](media/cache-create/enterprise-tier-basics.png) 

1. Sélectionnez **Suivant : Mise en réseau** et ignorez.

   > [!NOTE] 
   > La prise en charge d’un lien privé sera fournie plus tard.
   >

1. Sélectionnez **Suivant : Avancé**.
   
   Vous pouvez conserver les paramètres par défaut ou les modifier en fonction de vos besoins. Lorsque vous activez **Autoriser l’accès uniquement via TLS**, vous devez utiliser TLS pour accéder au nouveau cache à partir de votre application.

   ![Niveau Enterprise avancé](media/cache-create/enterprise-tier-advanced.png) 

   > [!NOTE] 
   > Les modules Redis ne sont pas encore pris en charge dans le niveau Enterprise SSD. Si vous envisagez d’utiliser un module Redis, veillez à choisir un cache de niveau Enterprise.
   >
   
1. Sélectionnez **Suivant : Étiquettes** et ignorez.

1. Sélectionnez **Suivant : Résumé**.

   ![Résumé du niveau Enterprise](media/cache-create/enterprise-tier-summary.png) 

1. Cliquez sur la case à cocher sous **Conditions**, passez en revue les paramètres, puis sélectionnez **Vérifier + créer**.
   
   La création du cache prend un certain temps. Vous pouvez surveiller la progression dans la page **Vue d’ensemble** du Azure Cache pour Redis. Lorsque **État** indique **En cours d’exécution**, le cache est prêt pour utilisation.

   > [!NOTE] 
   > Une fois le cache de niveau Enterprise créé, vous recevrez un e-mail **Action requise** de la place de marché Azure pour configurer *Azure Cache pour Redis, niveaux Enterprise*. Cette action n’est pas nécessaire. Vous pouvez ignorer cet e-mail sans problème.
   >

## <a name="next-steps"></a>Étapes suivantes

Dans ce guide de démarrage rapide, vous avez appris à créer une instance de niveau Entreprise pour Azure Cache pour Redis.

> [!div class="nextstepaction"]
> [Créer une application web ASP.NET qui utilise un cache Azure pour Redis.](./cache-web-app-howto.md)

