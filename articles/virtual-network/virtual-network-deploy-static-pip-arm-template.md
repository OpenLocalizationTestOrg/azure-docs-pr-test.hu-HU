---
title: "a virtuális gép statikus nyilvános IP-címmel – Azure Resource Manager sablon aaaCreate |} Microsoft Docs"
description: "Ismerje meg, hogyan toocreate egy virtuális Gépet egy statikus nyilvános IP-cím, egy Azure Resource Manager-sablon használatával."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d551085a-c7ed-4ec6-b4c3-e9e1cebb774c
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6a8640ed4fad06b0e09820e6114fd6789db73847
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-a-static-public-ip-address-using-an-azure-resource-manager-template"></a>Virtuális gép létrehozása az Azure Resource Manager-sablonnal statikus nyilvános IP-cím

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sablon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klasszikus)](virtual-networks-reserved-public-ip.md)

[!INCLUDE [virtual-network-deploy-static-pip-intro-include.md](../../includes/virtual-network-deploy-static-pip-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md). Ez a cikk a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello klasszikus üzembe helyezési modellel hello Resource Manager telepítési modell használatát bemutatja.

[!INCLUDE [virtual-network-deploy-static-pip-scenario-include.md](../../includes/virtual-network-deploy-static-pip-scenario-include.md)]

## <a name="public-ip-address-resources-in-a-template-file"></a>Nyilvános IP-cím erőforrás egy sablonfájlban
Tekintheti meg és töltse le a hello [mintasablon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json).

hello következő szakasz bemutatja hello nyilvános IP-erőforrás, a fenti hello forgatókönyv alapján hello definíciója:

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('webVMSetting').pipName]",
  "location": "[variables('location')]",
  "properties": {
    "publicIPAllocationMethod": "Static"
  },
  "tags": {
    "displayName": "PublicIPAddress - Web"
  }
},
```

Értesítés hello **publicIPAllocationMethod** tulajdonságot, amelynek értéke túl*statikus*. Ez a tulajdonság lehet *dinamikus* (alapértelmezett érték) vagy *statikus*. A beállítás azt toostatic biztosítja, hogy az hello nyilvános IP-cím lesz soha ne módosuljanak.

hello következő szakasz bemutatja egy adott hálózati csatoló hello nyilvános IP-cím hello társítást:

```json
  {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('webVMSetting').nicName]",
    "location": "[variables('location')]",
    "tags": {
    "displayName": "NetworkInterface - Web"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/publicIPAddresses/', variables('webVMSetting').pipName)]",
      "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
    ],
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
          "privateIPAllocationMethod": "Static",
          "privateIPAddress": "[variables('webVMSetting').ipAddress]",
          "publicIPAddress": {
          "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('webVMSetting').pipName)]"
          },
          "subnet": {
            "id": "[variables('frontEndSubnetRef')]"
          }
        }
      }
    ]
  }
},
```

Értesítés hello **publicIPAddress** toohello mutató tulajdonság **azonosító** nevű erőforrás **variables('webVMSetting').pipName**. Ez az hello hello nyilvános IP-erőforrás nevét fent látható.

Végezetül, a fenti hello hálózati illesztő szerepel hello **networkProfile** tulajdonsága hello virtuális gép létrehozása folyamatban.

```json
      "networkProfile": {
        "networkInterfaces": [
          {
            "id": "[resourceId('Microsoft.Network/networkInterfaces', variables('webVMSetting').nicName)]"
          }
        ]
      }
```

## <a name="deploy-hello-template-by-using-click-toodeploy"></a>Hello sablon üzembe helyezése a toodeploy kattintson

hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ. toodeploy toodeploy, a sablon használatával kattintson kattintson **tooAzure telepítése** hello Readme.md fájl hello [statikus PIP rendelkező virtuális gépet](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/03-Static-public-IP) sablont. Cserélje le a hello alapértelmezett paraméterértékek, ha szükséges, és adja meg a hello üres paraméterek.  Kövesse a hello utasításait hello portál toocreate egy virtuális gép egy statikus nyilvános IP-címmel.

## <a name="deploy-hello-template-by-using-powershell"></a>Hello sablon üzembe helyezése a PowerShell használatával

toodeploy hello sablon PowerShell használatával hajtsa végre az alábbi hello lépéseket.

1. Ha még sosem használta az Azure PowerShell, teljes hello hello szükséges lépések [hogyan tooInstall és konfigurálása az Azure PowerShell](/powershell/azure/overview) cikk.
2. PowerShell-konzolban, futtassa a hello `New-AzureRmResourceGroup` parancsmag toocreate egy új erőforráscsoportot, ha szükséges. Ha már létrehozott egy erőforráscsoport, folytassa a toostep 3.

    ```powershell
    New-AzureRmResourceGroup -Name PIPTEST -Location westus
    ```

    Várt kimenet:
   
        ResourceGroupName : PIPTEST
        Location          : westus
        ProvisioningState : Succeeded
        Tags              :
        ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/StaticPublicIP

3. PowerShell-konzolban, futtassa a hello `New-AzureRmResourceGroupDeployment` parancsmag toodeploy hello sablont.

    ```powershell
    New-AzureRmResourceGroupDeployment -Name DeployVM -ResourceGroupName PIPTEST `
        -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json `
        -TemplateParameterUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json
    ```

    Várt kimenet:
   
        DeploymentName    : DeployVM
        ResourceGroupName : PIPTEST
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
                            Uri            : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/mas
                            ter/IaaS-Story/03-Static-public-IP/azuredeploy.json
                            ContentVersion : 1.0.0.0
   
        Parameters        :
                            Name                      Type                       Value     
                            ========================  =========================  ==========
                            vnetName                  String                     WTestVNet
                            vnetPrefix                String                     192.168.0.0/16
                            frontEndSubnetName        String                     FrontEnd  
                            frontEndSubnetPrefix      String                     192.168.1.0/24
                            storageAccountNamePrefix  String                     iaasestd  
                            stdStorageType            String                     Standard_LRS
                            osType                    String                     Windows   
                            adminUsername             String                     adminUser
                            adminPassword             SecureString                         
   
        Outputs           :

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Hello sablon üzembe helyezése hello Azure parancssori felület használatával
toodeploy hello sablon hello Azure CLI, teljes hello lépések segítségével:

1. Ha még sosem használta az Azure parancssori felület, kövesse hello hello [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) tooinstall a következő cikket, és állítsa be.
2. Futtassa a hello `azure config mode` tooswitch tooResource Manager üzemmód, alább látható módon.

    ```azurecli
    azure config mode arm
    ```

    hello várt fenti hello parancs kimenetét:

        info:    New mode is arm

3. Nyissa meg hello [paraméterfájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.parameters.json), válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájlt. Ehhez a példához hello paraméterek nevű tooa fájl mentett *parameters.json*. Hello paraméterértékek hello fájlban szükség esetén módosítsa, de legalább, javasoljuk, hogy megváltoztatja hello hello adminPassword paraméter tooa egyedi, összetett jelszót.
4. Futtassa a hello `azure group deployment create` cmd toodeploy hello hello sablonnal és paraméterfájlokkal segítségével új virtuális hálózat fájlokat fent letöltött és módosított. Az alábbi hello parancs, cserélje le <path> hello elérési úttal rendelkező hello fájlt mentette. 

    ```azurecli
    azure group create -n PIPTEST2 -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/03-Static-public-IP/azuredeploy.json -e <path>\parameters.json
    ```

    Várt kimenet (használt paraméterértékeket ismerteti):

        info:    Executing command group create
        + Getting resource group PIPTEST2
        + Creating resource group PIPTEST2
        info:    Created resource group PIPTEST2
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/[Subscription ID]/resourceGroups/PIPTEST2
        data:    Name:                PIPTEST2
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

