---
title: Fichier Include
description: Fichier Include
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file, devx-track-azurecli
ms.openlocfilehash: 2b2c2a8f471aba36592f76caf46b7d26d7e3bb31
ms.sourcegitcommit: 8c7f47cc301ca07e7901d95b5fb81f08e6577550
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/27/2020
ms.locfileid: "92755585"
---
### <a name="to-modify-local-network-gateway-ip-address-prefixes---no-gateway-connection"></a><a name="noconnection"></a>Pour modifier des préfixes d’adresses IP de passerelle de réseau local - sans connexion de passerelle

Si vous n’avez pas de connexion de passerelle et mais souhaitez ajouter ou supprimer des préfixes d’adresses IP, utilisez la commande qui vous a permis de créer la passerelle de réseau local, [az network local-gateway create](https://docs.microsoft.com/cli/azure/network/local-gateway). Vous pouvez également utiliser cette commande pour mettre à jour l’adresse IP de passerelle du périphérique VPN. Pour remplacer les paramètres actuels, utilisez le nom existant de votre passerelle de réseau local. Si vous utilisez un nom différent, vous créerez une nouvelle passerelle de réseau local au lieu de remplacer la passerelle existante.

Chaque fois que vous apportez une modification, vous devez renseigner l’intégralité des préfixes, et pas uniquement les préfixes que vous souhaitez modifier. Spécifiez uniquement les préfixes que vous souhaitez conserver. Dans ce cas, 10.0.0.0/24 et 20.0.0.0/24

```azurecli
az network local-gateway create --gateway-ip-address 23.99.221.164 --name Site2 -g TestRG1 --local-address-prefixes 10.0.0.0/24 20.0.0.0/24
```

### <a name="to-modify-local-network-gateway-ip-address-prefixes---existing-gateway-connection"></a><a name="withconnection"></a>Pour modifier des préfixes d’adresses IP de passerelle de réseau local - avec une connexion de passerelle existante

Si vous disposez d’une connexion de passerelle et souhaitez ajouter ou supprimer des préfixes d’adresses IP, vous pouvez mettre à jour les préfixes à l’aide de la commande [az network local-gateway update](https://docs.microsoft.com/cli/azure/network/local-gateway). Cela entraînera une interruption de votre connexion VPN. Lorsque vous modifiez les préfixes d’adresse IP, vous n’avez pas besoin de supprimer la passerelle VPN.

Chaque fois que vous apportez une modification, vous devez renseigner l’intégralité des préfixes, et pas uniquement les préfixes que vous souhaitez modifier. Dans cet exemple, 10.0.0.0/24 et 20.0.0.0/24 sont déjà présents. Nous ajoutons les préfixes 30.0.0.0/24 et 40.0.0.0/24, et nous spécifions tous les 4 des préfixes lors de la mise à jour.

```azurecli
az network local-gateway update --local-address-prefixes 10.0.0.0/24 20.0.0.0/24 30.0.0.0/24 40.0.0.0/24 --name VNet1toSite2 -g TestRG1
```
