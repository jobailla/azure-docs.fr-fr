---
title: Utiliser JavaScript pour créer une salle de conversation avec Azure Functions et SignalR Service
description: Guide de démarrage rapide pour utiliser Azure SignalR Service et Azure Functions afin de créer une salle de conversation à l’aide de JavaScript.
author: sffamily
ms.service: signalr
ms.devlang: javascript
ms.topic: quickstart
ms.date: 12/14/2019
ms.author: zhshang
ms.custom: devx-track-js
ms.openlocfilehash: c210096c1765015378f12f8c5e01fc9f8f97e10b
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91327678"
---
# <a name="quickstart-use-javascript-to-create-a-chat-room-with-azure-functions-and-signalr-service"></a>Démarrage rapide : Utiliser JavaScript pour créer une salle de conversation avec Azure Functions et SignalR Service

Azure SignalR Service vous permet d’ajouter facilement des fonctionnalités en temps réel à votre application, et Azure Functions est une plateforme serverless qui vous permet d’exécuter votre code sans gérer d’infrastructure. Dans ce démarrage rapide, vous allez utiliser JavaScript pour générer une application serverless de conversation en temps réel en utilisant SignalR Service et Functions.

## <a name="prerequisites"></a>Prérequis

- Un éditeur de code, comme [Visual Studio Code](https://code.visualstudio.com/).
- Compte Azure avec un abonnement actif. [Créez un compte gratuitement](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio).
- [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools#installing), version 2 ou supérieure. Sert à exécuter les applications Azure Functions localement.
- [Node.js](https://nodejs.org/en/download/), version 10.x.

   > [!NOTE]
   > Les exemples devraient fonctionner avec d’autres versions Node.js. Pour plus d’informations, consultez la [documentation des versions du runtime Azure Functions](../azure-functions/functions-versions.md#languages).

> [!NOTE]
> Ce démarrage rapide peut être exécuté sur macOS, Windows ou Linux.

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

## <a name="log-in-to-azure"></a>Connexion à Azure

Connectez-vous au portail Azure sur <https://portal.azure.com/> avec votre compte Azure.

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

[!INCLUDE [Create instance](includes/signalr-quickstart-create-instance.md)]

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

[!INCLUDE [Clone application](includes/signalr-quickstart-clone-application.md)]

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

## <a name="configure-and-run-the-azure-function-app"></a>Configurer et exécuter l’application de fonction Azure

1. Dans le navigateur dans lequel le portail Azure est ouvert, vérifiez que l’instance du service SignalR que vous avez déployée précédemment a bien été créée en recherchant son nom dans la zone de recherche en haut du portail. Sélectionnez l’instance pour l’ouvrir.

    ![Rechercher l’instance du service SignalR](media/signalr-quickstart-azure-functions-csharp/signalr-quickstart-search-instance.png)

1. Sélectionnez **Clés** pour afficher les chaînes de connexion de l’instance du service SignalR.

1. Sélectionnez et copiez la chaîne de connexion principale.

    ![Créer un service SignalR](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-keys.png)

1. Dans votre éditeur de code, ouvrez le dossier *src/chat/javascript* situé dans le référentiel cloné.

1. Renommez *local.settings.sample.json* en *local.settings.json*.

1. Dans le fichier **local.settings.json**, collez la chaîne de connexion dans la valeur du paramètre **AzureSignalRConnectionString**. Enregistrez le fichier .

1. Les fonctions JavaScript sont organisées dans des dossiers. Deux fichiers se trouvent dans chaque dossier : le fichier *function.json* définit les liaisons utilisées dans la fonction et le fichier *index.js* correspond au corps de la fonction. Il existe deux fonctions déclenchées par HTTP dans cette application de fonction :

    - **negotiate** : utilise la liaison d’entrée *SignalRConnectionInfo* pour générer et retourner des informations de connexion valides.
    - **messages** : reçoit un message de conversation dans le corps de la demande et utilise la liaison de sortie *SignalR* pour diffuser le message à toutes les applications clientes connectées.

1. Dans le terminal, vérifiez que vous vous trouvez dans le dossier *src/chat/javascript*. Exécutez l’application de fonction.

    ```bash
    func start
    ```

    ![Créer un service SignalR](media/signalr-quickstart-azure-functions-javascript/signalr-quickstart-run-application.png)

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

[!INCLUDE [Run web application](includes/signalr-quickstart-run-web-application.md)]

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

[!INCLUDE [Cleanup](includes/signalr-quickstart-cleanup.md)]

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)

## <a name="next-steps"></a>Étapes suivantes

Dans ce démarrage rapide, vous avez généré et exécuté une application serverless en temps réel dans VS Code. Apprenez-en plus sur le déploiement d’Azure Functions à partir de VS Code.

> [!div class="nextstepaction"]
> [Déployer Azure Functions avec VS Code](/azure/developer/javascript/tutorial-vscode-serverless-node-01)

[Vous rencontrez des problèmes ? Faites-le nous savoir.](https://aka.ms/asrs/qsjs)
