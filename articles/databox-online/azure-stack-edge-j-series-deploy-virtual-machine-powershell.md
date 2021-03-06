---
title: Déployer des machines virtuelles sur votre appareil Azure Stack Edge Pro avec GPU via Azure PowerShell
description: Décrit comment créer et gérer des machines virtuelles sur un appareil Azure Stack Edge Pro avec GPU à l’aide d’Azure PowerShell.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: how-to
ms.date: 08/28/2020
ms.author: alkohli
ms.openlocfilehash: aa492acdedc2d131d28c894031de2181e87a2f3e
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 10/09/2020
ms.locfileid: "90890705"
---
# <a name="deploy-vms-on-your-azure-stack-edge-pro-gpu-device-via-azure-powershell"></a>Déployer des machines virtuelles sur votre appareil Azure Stack Edge Pro avec GPU via Azure PowerShell

<!--[!INCLUDE [azure-stack-edge-gateway-deploy-vm-overview](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-overview.md)]-->

Ce tutoriel explique comment créer et gérer une machine virtuelle sur votre appareil Azure Stack Edge Pro à l’aide d’Azure PowerShell.

## <a name="vm-deployment-workflow"></a>Workflow du déploiement de machine virtuelle

Le workflow du déploiement est illustré dans le diagramme suivant.

![Workflow du déploiement de machine virtuelle](media/azure-stack-edge-j-series-deploy-virtual-machine-powershell/vm-workflow_r.svg)

## <a name="prerequisites"></a>Prérequis

[!INCLUDE [azure-stack-edge-gateway-deploy-vm-prerequisites](../../includes/azure-stack-edge-gateway-deploy-virtual-machine-prerequisites.md)]



## <a name="query-for-built-in-subscription-on-the-device"></a>Interroger l’abonnement intégré sur l’appareil

Pour Azure Resource Manager, un seul abonnement fixe visible par l’utilisateur est pris en charge. Cet abonnement est unique par appareil et ce nom ou ID d’abonnement ne peut pas être modifié.

Cet abonnement contient toutes les ressources créées nécessaires à la création de la machine virtuelle. 

> [!IMPORTANT]
> Cet abonnement n’est pas connecté ou lié à votre abonnement Azure et réside localement sur votre appareil.

Cet abonnement sera utilisé pour déployer les machines virtuelles.

1.  Pour lister cet abonnement, tapez :

    ```powershell
    Get-AzureRmSubscription
    ```
    
    Voici un exemple de sortie obtenue.

    ```powershell
    PS C:\windows\system32> Get-AzureRmSubscription
    
    Name                 Id                 TenantId          State
    ----                 --                --------           -----
    Default Provider Subscription A4257FDE-B946-4E01-ADE7-674760B8D1A3 c0257de7-538f-415c-993a-1b87a031879d Enabled
    
    PS C:\windows\system32>
    ```
        
3.  Obtenez la liste des fournisseurs de ressources inscrits qui s’exécutent sur l’appareil. Cette liste comprend généralement Calcul, Réseau et Stockage.

    ```powershell
    Get-AzureRMResourceProvider
    ```

    > [!NOTE]
    > Les fournisseurs de ressources sont préinscrits et ne peuvent pas être modifiés.
    
    Voici un exemple de sortie obtenue :

    ```powershell
    Get-AzureRmResourceProvider
    ProviderNamespace : Microsoft.Compute
    RegistrationState : Registered
    ResourceTypes     : {virtualMachines, virtualMachines/extensions, locations, operations...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Network
    RegistrationState : Registered
    ResourceTypes     : {operations, locations, locations/operations, locations/usages...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Resources
    RegistrationState : Registered
    ResourceTypes     : {tenants, locations, providers, checkresourcename...}
    Locations         : {DBELocal}
    ZoneMappings      :
    
    ProviderNamespace : Microsoft.Storage
    RegistrationState : Registered
    ResourceTypes     : {storageaccounts, storageAccounts/blobServices, storageAccounts/tableServices,
                        storageAccounts/queueServices...}
    Locations         : {DBELocal}
    ZoneMappings      :
    ```
    
## <a name="create-a-resource-group"></a>Créer un groupe de ressources

Créez un groupe de ressources Azure avec [New-AzureRmResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup). Un groupe de ressources est un conteneur logique dans lequel les ressources Azure comme le compte de stockage, le disque et le disque managé sont déployées et gérées.

> [!IMPORTANT]
> Toutes les ressources sont créées au même emplacement que celui de l’appareil et cet emplacement est défini sur **DBELocal**.

```powershell
New-AzureRmResourceGroup -Name <Resource group name> -Location DBELocal
```

Voici un exemple de sortie obtenue.

```powershell
New-AzureRmResourceGroup -Name rg191113014333 -Location DBELocal 
Successfully created Resource Group:rg191113014333
```

## <a name="create-a-storage-account"></a>Créez un compte de stockage.

Créez un compte de stockage en utilisant le groupe de ressources créé à l’étape précédente. Il s’agit d’un **compte de stockage local** qui sera utilisé pour charger l’image de disque virtuel de la machine virtuelle.

```powershell
New-AzureRmStorageAccount -Name <Storage account name> -ResourceGroupName <Resource group name> -Location DBELocal -SkuName Standard_LRS
```

> [!NOTE]
> Seuls les comptes de stockage locaux comme le stockage localement redondant (Standard_LRS ou Premium_LRS) peuvent être créés par le biais d’Azure Resource Manager. Pour créer des comptes de stockage hiérarchisés, consultez les étapes décrites dans [Ajouter des comptes de stockage à Azure Stack Edge Pro et se connecter à ces comptes](azure-stack-edge-j-series-deploy-add-storage-accounts.md).

Voici un exemple de sortie obtenue.

```powershell
New-AzureRmStorageAccount -Name sa191113014333  -ResourceGroupName rg191113014333 -SkuName Standard_LRS -Location DBELocal

ResourceGroupName      : rg191113014333
StorageAccountName     : sa191113014333
Id                     : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Microsoft.Storage/storageaccounts/sa191113014333
Location               : DBELocal
Sku                    : Microsoft.Azure.Management.Storage.Models.Sku
Kind                   : Storage
Encryption             : Microsoft.Azure.Management.Storage.Models.Encryption
AccessTier             :
CreationTime           : 11/13/2019 9:43:49 PM
CustomDomain           :
Identity               :
LastGeoFailoverTime    :
PrimaryEndpoints       : Microsoft.Azure.Management.Storage.Models.Endpoints
PrimaryLocation        : DBELocal
ProvisioningState      : Succeeded
SecondaryEndpoints     :
SecondaryLocation      :
StatusOfPrimary        : Available
StatusOfSecondary      :
Tags                   :
EnableHttpsTrafficOnly : False
NetworkRuleSet         :
Context                : Microsoft.WindowsAzure.Commands.Common.Storage.LazyAzureStorageContext
ExtendedProperties     : {}
```

Pour obtenir la clé du compte de stockage, exécutez la commande `Get-AzureRmStorageAccountKey`. Vous trouverez ici un exemple de sortie de la commande.

```powershell
PS C:\Users\Administrator> Get-AzureRmStorageAccountKey

cmdlet Get-AzureRmStorageAccountKey at command pipeline position 1
Supply values for the following parameters:
(Type !? for Help.)
ResourceGroupName: my-resource-ase
Name:myasestoracct

KeyName Value
------- -----
key1 /IjVJN+sSf7FMKiiPLlDm8mc9P4wtcmhhbnCa7...
key2 gd34TcaDzDgsY9JtDNMUgLDOItUU0Qur3CBo6Q...
```

## <a name="add-blob-uri-to-hosts-file"></a>Ajouter l’URI d’objet blob au fichier hosts

Vous avez déjà ajouté l’URI d’objet blob dans le fichier hosts pour le client que vous utilisez pour vous connecter au Stockage Blob dans la section [Modifier le fichier hosts pour la résolution de noms de point de terminaison](azure-stack-edge-j-series-connect-resource-manager.md#step-5-modify-host-file-for-endpoint-name-resolution). Il s’agissait de l’entrée pour l’URI d’objet blob :

\<Azure consistent network services VIP \> \<storage name\>.blob.\<appliance name\>.\<dnsdomain\>


## <a name="install-certificates"></a>Installer des certificats

Si vous utilisez *https*, vous devez installer les certificats appropriés sur votre appareil. Dans ce cas, installez le certificat de point de terminaison d’objet blob. Pour plus d’informations, consultez le guide pratique pour créer et charger des certificats dans [Gérer des certificats](azure-stack-edge-j-series-manage-certificates.md).

## <a name="upload-a-vhd"></a>Télécharger un disque dur virtuel

Copiez les images de disque à utiliser dans des objets blob de pages dans le compte de stockage local que vous avez créé lors des étapes précédentes. Vous pouvez utiliser un outil comme [AzCopy](../storage/common/storage-use-azcopy-v10.md) pour charger le disque dur virtuel sur le compte de stockage que vous avez créé lors des étapes précédentes. 

Avant d’utiliser l’outil AzCopy, vérifiez qu’il est [correctement configuré](#configure-azcopy) pour être utilisé avec la version de l’API REST du stockage Blob que vous utilisez avec votre appareil Azure Stack Edge Pro.

```powershell
AzCopy /Source:<sourceDirectoryForVHD> /Dest:<blobContainerUri> /DestKey:<storageAccountKey> /Y /S /V /NC:32  /BlobType:page /destType:blob 
```

> [!NOTE]
> Définissez `BlobType` sur page pour créer un disque managé en dehors d’un disque dur virtuel. Définissez `BlobType` sur block lors de l’écriture dans des comptes de stockage hiérarchisés à l’aide d’AzCopy.

Vous pouvez télécharger les images de disque à partir de la Place de marché. Pour obtenir des instructions détaillées, accédez à [Obtenir l’image de disque virtuel à partir de la Place de marché Azure](azure-stack-edge-j-series-create-virtual-machine-image.md).

Vous trouverez ci-dessous un exemple de sortie qui utilise AzCopy 7.3. Pour plus d’informations sur cette commande, accédez à [Charger le fichier VHD sur le compte de stockage avec AzCopy](../devtest-labs/devtest-lab-upload-vhd-using-azcopy.md).


```powershell
AzCopy /Source:\\hcsfs\scratch\vm_vhds\linux\ /Dest:http://sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages /DestKey:gJKoyX2Amg0Zytd1ogA1kQ2xqudMHn7ljcDtkJRHwMZbMK== /Y /S /V /NC:32 /BlobType:page /destType:blob /z:2e7d7d27-c983-410c-b4aa-b0aa668af0c6
```


## <a name="create-managed-disks-from-the-vhd"></a>Créer des disques managés à partir du disque dur virtuel

Créez un disque managé à partir du disque dur virtuel chargé.

```powershell
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import -SourceUri "Source URL for your VHD"
```
Voici un exemple de sortie obtenue : 
<code>
$DiskConfig = New-AzureRmDiskConfig -Location DBELocal -CreateOption Import –SourceUri http://</code><code>sa191113014333.blob.dbe-1dcmhq2.microsoftdatabox.com/vmimages/ubuntu13.vhd</code> 

```powershell
New-AzureRMDisk -ResourceGroupName <Resource group name> -DiskName <Disk name> -Disk $DiskConfig
```

Voici un exemple de sortie obtenue. Pour plus d’informations sur cette applet de commande, accédez à [New-AzureRmDisk](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermdisk?view=azurermps-6.13.0).

```powershell
Tags               :
New-AzureRmDisk -ResourceGroupName rg191113014333 -DiskName ld191113014333 -Disk $DiskConfig

ResourceGroupName  : rg191113014333
ManagedBy          :
Sku                : Microsoft.Azure.Management.Compute.Models.DiskSku
Zones              :
TimeCreated        : 11/13/2019 1:49:07 PM
OsType             :
CreationData       : Microsoft.Azure.Management.Compute.Models.CreationData
DiskSizeGB         : 30
EncryptionSettings :
ProvisioningState  : Succeeded
Id                 : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micros
                     oft.Compute/disks/ld191113014333
Name               : ld191113014333
Type               : Microsoft.Compute/disks
Location           : DBELocal
Tags               : {}
```

## <a name="create-a-vm-image-from-the-image-managed-disk"></a>Créer une image de machine virtuelle à partir de l’image de disque managé

Utilisez la commande suivante pour créer une image de machine virtuelle à partir du disque managé. Remplacez les valeurs \< \> par les noms de votre choix.

```powershell
$imageConfig = New-AzureRmImageConfig -Location DBELocal
$ManagedDiskId = (Get-AzureRmDisk -Name <Disk name> -ResourceGroupName <Resource group name>).Id
Set-AzureRmImageOsDisk -Image $imageConfig -OsType '<OS type>' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId 

The supported OS types are Linux and Windows.

For OS Type=Linux, for example:
Set-AzureRmImageOsDisk -Image $imageConfig -OsType 'Linux' -OsState 'Generalized' -DiskSizeGB <Disk size> -ManagedDiskId $ManagedDiskId
New-AzureRmImage -Image $imageConfig -ImageName <Image name>  -ResourceGroupName <Resource group name>
```

Voici un exemple de sortie obtenue. Pour plus d’informations sur cette applet de commande, accédez à [New-AzureRmImage](https://docs.microsoft.com/powershell/module/azurerm.compute/new-azurermimage?view=azurermps-6.13.0).

```powershell
New-AzureRmImage -Image Microsoft.Azure.Commands.Compute.Automation.Models.PSImage -ImageName ig191113014333  -ResourceGroupName rg191113014333
ResourceGroupName    : rg191113014333
SourceVirtualMachine :
StorageProfile       : Microsoft.Azure.Management.Compute.Models.ImageStorageProfile
ProvisioningState    : Succeeded
Id                   : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg191113014333/providers/Micr
                       osoft.Compute/images/ig191113014333
Name                 : ig191113014333
Type                 : Microsoft.Compute/images
Location             : dbelocal
Tags                 : {}
```

## <a name="create-vm-with-previously-created-resources"></a>Créer une machine virtuelle avec des ressources créées précédemment

Vous devez créer un seul réseau virtuel et associer une interface réseau virtuelle avant de créer et déployer la machine virtuelle.

> [!IMPORTANT]
> Quand vous créez un réseau virtuel et une interface réseau virtuelle, les règles suivantes s’appliquent :
> - Un seul réseau virtuel peut être créé (même dans plusieurs groupes de ressources) et il doit correspondre exactement au réseau logique en termes d’espace d’adressage.
> -   Un seul sous-réseau est autorisé dans le réseau virtuel. Ce sous-réseau doit être exactement le même espace d’adressage que le réseau virtuel.
> -   Seule la méthode d’allocation statique est autorisée lors de la création de la carte réseau virtuelle et l’utilisateur doit fournir une adresse IP privée.

 
**Créer un réseau virtuel**

```powershell
$subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name <Subnet name> -AddressPrefix <Address Prefix>
$aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName <Resource group name> -Name <Vnet name> -Location DBELocal -AddressPrefix <Address prefix> -Subnet $subNetId
```

**Créer une carte réseau virtuelle à l’aide de l’ID du sous-réseau du réseau virtuel**

```powershell
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <IP config Name> -SubnetId $aRmVN.Subnets[0].Id -PrivateIpAddress <Private IP>
$Nic = New-AzureRmNetworkInterface -Name <Nic name> -ResourceGroupName <Resource group name> -Location DBELocal -IpConfiguration $ipConfig
```

Vous trouverez ci-dessous un exemple de sortie de ces commandes :

```powershell
PS C:\Users\Administrator> $subNetId=New-AzureRmVirtualNetworkSubnetConfig -Name my-ase-subnet -AddressPrefix "5.5.0.0/16"

PS C:\Users\Administrator> $aRmVN = New-AzureRmVirtualNetwork -ResourceGroupName Resource-my-ase -Name my-ase-virtualnetwork -Location DBELocal -AddressPrefix "5.5.0.0/16" -Subnet $subNetId
WARNING: The output object type of this cmdlet will be modified in a future release.
PS C:\Users\Administrator> $ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name my-ase-ip -SubnetId $aRmVN.Subnets[0].Id
PS C:\Users\Administrator> $Nic = New-AzureRmNetworkInterface -Name my-ase-nic -ResourceGroupName Resource-my-ase -Location DBELocal -IpConfiguration $ipConfig
WARNING: The output object type of this cmdlet will be modified in a future release.

PS C:\Users\Administrator> $Nic

PS C:\Users\Administrator> (Get-AzureRmNetworkInterface)[0]

Name                        : nic200108020444
ResourceGroupName           : rg200108020444
Location                    : dbelocal
Id                          : /subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Network/networ
                              kInterfaces/nic200108020444
Etag                        : W/"f9d1759d-4d49-42fa-8826-e218e0b1d355"
ResourceGuid                : 3851ae62-c13e-4416-9386-e21d9a2fef0f
ProvisioningState           : Succeeded
Tags                        :
VirtualMachine              : {
                                "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Compu
                              te/virtualMachines/VM200108020444"
                              }
IpConfigurations            : [
                                {
                                  "Name": "ip200108020444",
                                  "Etag": "W/\"f9d1759d-4d49-42fa-8826-e218e0b1d355\"",
                                  "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/rg200108020444/providers/Microsoft.Net
                              work/networkInterfaces/nic200108020444/ipConfigurations/ip200108020444",
                                  "PrivateIpAddress": "5.5.166.65",
                                  "PrivateIpAllocationMethod": "Static",
                                  "Subnet": {
                                    "Id": "/subscriptions/a4257fde-b946-4e01-ade7-674760b8d1a3/resourceGroups/DbeSystemRG/providers/Microsoft.Netw
                              ork/virtualNetworks/vSwitch1/subnets/subnet123",
                                    "ResourceNavigationLinks": [],
                                    "ServiceEndpoints": []
                                  },
                                  "ProvisioningState": "Succeeded",
                                  "PrivateIpAddressVersion": "IPv4",
                                  "LoadBalancerBackendAddressPools": [],
                                  "LoadBalancerInboundNatRules": [],
                                  "Primary": true,
                                  "ApplicationGatewayBackendAddressPools": [],
                                  "ApplicationSecurityGroups": []
                                }
                              ]
DnsSettings                 : {
                                "DnsServers": [],
                                "AppliedDnsServers": []
                              }
EnableIPForwarding          : False
EnableAcceleratedNetworking : False
NetworkSecurityGroup        : null
Primary                     : True
MacAddress                  : 00155D18E432                :
```

Quand vous créez une carte réseau virtuelle pour une machine virtuelle, vous pouvez éventuellement transmettre l’adresse IP publique. Dans ce cas, l’adresse IP publique retourne l’adresse IP privée. 

```powershell
New-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <ResourceGroupName> -AllocationMethod Static -Location DBELocal
$publicIP = (Get-AzureRmPublicIPAddress -Name <Public IP> -ResourceGroupName <Resource group name>).Id
$ipConfig = New-AzureRmNetworkInterfaceIpConfig -Name <ConfigName> -PublicIpAddressId $publicIP -SubnetId $subNetId
```


**Créer une machine virtuelle**

Vous pouvez maintenant utiliser l’image de machine virtuelle pour créer une machine virtuelle et l’attacher au réseau virtuel que vous avez créé précédemment.

```powershell
$pass = ConvertTo-SecureString "<Password>" -AsPlainText -Force;
$cred = New-Object System.Management.Automation.PSCredential("<Enter username>", $pass)

You will use this username, password to login to the VM, once it is created and powered up.

$VirtualMachine = New-AzureRmVMConfig -VMName <VM name> -VMSize "Standard_D1_v2"

$VirtualMachine = Set-AzureRmVMOperatingSystem -VM $VirtualMachine -<OS type> -ComputerName <Your computer Name> -Credential $cred

$VirtualMachine = Set-AzureRmVMOSDisk -VM $VirtualMachine -Name <OS Disk Name> -Caching "ReadWrite" -CreateOption "FromImage" -Linux -StorageAccountType StandardLRS

$nicID = (Get-AzureRmNetworkInterface -Name <nic name> -ResourceGroupName <Resource Group Name>).Id

$VirtualMachine = Add-AzureRmVMNetworkInterface -VM $VirtualMachine -Id $nicID

$image = (Get-AzureRmImage -ResourceGroupName <Resource Group Name> -ImageName $ImageName).Id

$VirtualMachine = Set-AzureRmVMSourceImage -VM $VirtualMachine -Id $image

New-AzureRmVM -ResourceGroupName <Resource Group Name> -Location DBELocal -VM $VirtualMachine -Verbose
```

## <a name="connect-to-a-vm"></a>Se connecter à une machine virtuelle

Selon que vous avez créé une machine virtuelle Windows ou Linux, les étapes de connexion peuvent être différentes.

### <a name="connect-to-linux-vm"></a>Se connecter à une machine virtuelle Linux

Suivez ces étapes pour vous connecter à une machine virtuelle Linux.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-linux.md)]

### <a name="connect-to-windows-vm"></a>Se connecter à une machine virtuelle Windows

Suivez ces étapes pour vous connecter à une machine virtuelle Windows.

[!INCLUDE [azure-stack-edge-gateway-connect-vm](../../includes/azure-stack-edge-gateway-connect-virtual-machine-windows.md)]


<!--Connect to the VM using the private IP that you passed during the VM creation.

Open an SSH session to connect with the IP address.

`ssh -l <username> <ip address>`

When prompted, provide the password that you used when creating the VM.

If you need to provide the SSH key, use this command.

ssh -i c:/users/Administrator/.ssh/id_rsa Administrator@5.5.41.236

If you used a public IP address during VM creation, you can use that IP to connect to the VM. To get the public IP: 

```powershell
$publicIp = Get-AzureRmPublicIpAddress -Name <Public IP> -ResourceGroupName <Resource group name>
```
The public IP in this case will be the same as the private IP that you passed during virtual network interface creation.-->


## <a name="manage-vm"></a>Gérer la machine virtuelle

La section suivante décrit quelques-unes des opérations courantes liées à la machine virtuelle que vous allez créer sur votre appareil Azure Stack Edge Pro.

### <a name="list-vms-running-on-the-device"></a>Lister les machines virtuelles en cours d’exécution sur l’appareil

Pour retourner la liste de toutes les machines virtuelles en cours d’exécution sur votre appareil Azure Stack Edge Pro, exécutez la commande suivante.


`Get-AzureRmVM -ResourceGroupName <String> -Name <String>`
 

### <a name="turn-on-the-vm"></a>Activer la machine virtuelle

Exécutez l’applet de commande suivante pour activer une machine virtuelle en cours d’exécution sur votre appareil :


`Start-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>`


Pour plus d’informations sur cette applet de commande, accédez à [Start-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/start-azurermvm?view=azurermps-6.13.0).

### <a name="suspend-or-shut-down-the-vm"></a>Interrompre ou arrêter la machine virtuelle

Exécutez l’applet de commande suivante pour arrêter une machine virtuelle en cours d’exécution sur votre appareil :


```powershell
Stop-AzureRmVM [-Name] <String> [-StayProvisioned] [-ResourceGroupName] <String>
```


Pour plus d’informations sur cette applet de commande, accédez à [Stop-AzureRmVM](https://docs.microsoft.com/powershell/module/azurerm.compute/stop-azurermvm?view=azurermps-6.13.0).

### <a name="add-a-data-disk"></a>Ajouter un disque de données

Si les exigences en matière de charge de travail qui pèsent sur votre machine virtuelle augmentent, vous devrez peut-être ajouter un disque de données.

```powershell
Add-AzureRmVMDataDisk -VM $VirtualMachine -Name "disk1" -VhdUri "https://contoso.blob.core.windows.net/vhds/diskstandard03.vhd" -LUN 0 -Caching ReadOnly -DiskSizeinGB 1 -CreateOption Empty 
 
Update-AzureRmVM -ResourceGroupName "<Resource Group Name string>" -VM $VirtualMachine
```

### <a name="delete-the-vm"></a>Supprimer la machine virtuelle

Exécutez l’applet de commande suivante pour supprimer une machine virtuelle de votre appareil :

```powershell
Remove-AzureRmVM [-Name] <String> [-ResourceGroupName] <String>
```

Pour plus d’informations sur cette applet de commande, accédez à [Remove-AzureRmVm](https://docs.microsoft.com/powershell/module/azurerm.compute/remove-azurermvm?view=azurermps-6.13.0).


## <a name="supported-vm-sizes"></a>Tailles des machines virtuelles prises en charge

La taille de machine virtuelle détermine la quantité de ressources de calcul, comme le processeur, le processeur graphique (GPU) et la mémoire, qui sont mises à la disposition de la machine virtuelle. Les machines virtuelles doivent être créées avec une taille adaptée à la charge de travail. Même si toutes les machines vont s’exécuter sur le même matériel, leur taille ont des limites différentes pour l’accès au disque, ce qui peut vous aider à gérer l’accès global au disque sur toutes vos machines virtuelles. Si une charge de travail augmente, une machine virtuelle existante peut également être redimensionnée.

La création des machines virtuelles suivantes de la série Standard Dv2 est prise en charge sur un appareil Azure Stack Edge Pro.

### <a name="dv2-series"></a>Série Dv2
|Taille     |Processeurs virtuels     |Mémoire (Gio) | Stockage temporaire (Gio)  | Débit de disque du système d’exploitation max. (E/S par seconde) | Débit de stockage temporaire max. (E/S par seconde) | Disques de données max. / débit (E/S par seconde) | Nombre max de cartes réseau |
|-------------------|----|----|-----|----|------|------------|---------|
|**Standard_D1_v2** |1   |3,5 |50   |500 |3000  |4 / 4 x 500   |2 |
|**Standard_D2_v2** |2   |7   |100  |500 |6000  |8 / 8 x 500   |2 |
|**Standard_D3_v2** |4   |14  |200  |500 |12 000 |16 / 16 x 500 |4 |
|**Standard_D4_v2** |8   |28  |400  |500 |24 000 |32 / 32 x 500 |8 |
|**Standard_D5_v2** |16  |56  |800  |500 |48 000 |64 / 64 x 500 |8 |

### <a name="dsv2-series"></a>Séries DSv2
|Taille     |Processeurs virtuels     |Mémoire (Gio) | Stockage temporaire (Gio)  | Débit de disque du système d’exploitation max. (E/S par seconde) | Débit de stockage temporaire max. (E/S par seconde) | Disques de données max. / débit (E/S par seconde) | Nombre max de cartes réseau |
|--------------------|----|----|----|-----|------|-------------|---------|
|**Standard_DS1_v2** |1   |3,5 |7   |1 000 |4000  |4 / 4x2 300   |2 |
|**Standard_DS2_v2** |2   |7   |14  |1 000 |8000  |8 / 8x2 300   |2 |
|**Standard_DS3_v2** |4   |14  |28  |1 000 |16000 |16 / 16x2 300 |4 |
|**Standard_DS4_v2** |8   |28  |56  |1 000 |32000 |32 / 32x2 300 |8 |
|**Standard_DS5_v2** |16  |56  |112 |1 000 |64 000 |64 / 64x2 300 |8 |

### <a name="dv2-series"></a>Série Dv2
|Taille     |Processeurs virtuels     |Mémoire (Gio) | Stockage temporaire (Gio)  | Débit de disque du système d’exploitation max. (E/S par seconde) | Débit de stockage temporaire max. (E/S par seconde) | Disques de données max. / débit (E/S par seconde) | Nombre max de cartes réseau |
|--------------------|----|----|-----|----|-------|-------------|---------|
|**Standard_D11_v2** |2   |14  |100  |500 |6000   |8 / 8 x 500    |2 |
|**Standard_D12_v2** |4   |28  |200  |500 |12 000  |16 / 16 x 500  |4 |
|**Standard_D13_v2** |8   |56  |400  |500 |24 000  |32 / 32 x 500  |8 |
|**Standard_D14_v2** |16  |112 |800  |500 |48 000  |64 / 64 x 500  |8 |


### <a name="dsv2-series"></a>Séries DSv2
|Taille     |Processeurs virtuels     |Mémoire (Gio) | Stockage temporaire (Gio)  | Débit de disque du système d’exploitation max. (E/S par seconde) | Débit de stockage temporaire max. (E/S par seconde) | Disques de données max. / débit (E/S par seconde) | Nombre max de cartes réseau |
|---------------------|----|----|-----|-----|-------|--------------|---------|
|**Standard_DS11_v2** |2   |14  |28   |1 000 |8000   |4 / 4x2 300    |2 |
|**Standard_DS12_v2** |4   |28  |56   |1 000 |16000  |8 / 8x2 300    |4 |
|**Standard_DS13_v2** |8   |56  |112  |1 000 |32000  |16 / 16x2 300  |8 |
|**Standard_DS14_v2** |16  |112 |224  |1 000 |64 000  |32 / 32x2 300  |8 |

Pour plus d’informations, consultez [Série Dv2 sur des tailles de machine virtuelle à usage général](../virtual-machines/dv2-dsv2-series.md#dv2-series).

## <a name="unsupported-vm-operations-and-cmdlets"></a>Opérations et applets de commande de machine virtuelle non prises en charge

L’extension, les groupes identiques, les groupes à haute disponibilité et les instantanés ne sont pas pris en charge.

## <a name="configure-azcopy"></a>Configurer AzCopy

Quand vous installez la dernière version de l’outil AzCopy, vous devez la configurer pour veiller à ce qu’elle corresponde à la version de l’API REST du stockage Blob de votre appareil Azure Stack Edge Pro.

Sur le client utilisé pour accéder à votre appareil Azure Stack Edge Pro, configurez une variable globale qui correspond à la version de l’API REST du stockage Blob.

### <a name="on-windows-client"></a>Sur un client Windows 

`$Env:AZCOPY_DEFAULT_SERVICE_API_VERSION = "2017-11-09"`

### <a name="on-linux-client"></a>Sur un client Linux

`export AZCOPY_DEFAULT_SERVICE_API_VERSION=2017-11-09`

Pour vérifier si la variable d’environnement a été correctement définie pour AzCopy, effectuez les étapes suivantes :

1. Exécutez « azcopy env ».
2. Recherchez le paramètre `AZCOPY_DEFAULT_SERVICE_API_VERSION`. Celui-ci doit avoir la valeur que vous avez définie lors des étapes précédentes.


## <a name="next-steps"></a>Étapes suivantes

[Applets de commande Azure Resource Manager](https://docs.microsoft.com/powershell/module/azurerm.resources/?view=azurermps-6.13.0)
