---
title: "Linux virtuálisgép-méretezési csoport létrehozása Azure-sablonnal | Microsoft Docs"
description: "Ismerje meg, hogyan hozhat létre gyorsan Linux virtuálisgép-méretezési csoportot egy mintaalkalmazást üzembe helyező és automatikus méretezési szabályokat konfiguráló Azure Resource Manager-sablonnal"
services: virtual-machine-scale-sets
documentationcenter: 
author: iainfoulds
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/19/2017
ms.author: iainfou
ms.openlocfilehash: b07bdd0739dabb05ef7012051b7ac28af3aaddaf
ms.sourcegitcommit: 901a3ad293669093e3964ed3e717227946f0af96
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/21/2017
---
# <a name="create-a-linux-virtual-machine-scale-set-with-an-azure-template"></a>Linux virtuálisgép-méretezési csoport létrehozása Azure-sablonnal
A virtuálisgép-méretezési csoportok segítségével azonos, automatikus skálázású virtuális gépek csoportját hozhatja létre és kezelheti. A méretezési csoportban lévő virtuális gépek számát skálázhatja manuálisan, vagy megadhat automatikus skálázási szabályokat is az erőforrás-használat (például processzorhasználat, memóriaigény vagy hálózati forgalom) alapján. Ebben az első lépéseket bemutató cikkben egy Linux virtuálisgép-méretezési csoportot hozunk létre egy Azure Resource Manager-sablon használatával. Méretezési csoportokat az [Azure CLI 2.0](virtual-machine-scale-sets-create-cli.md) és az [Azure PowerShell](virtual-machine-scale-sets-create-powershell.md) használatával, illetve az [Azure Portalon](virtual-machine-scale-sets-create-portal.md) is létrehozhat.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Ha a parancssori felület helyi telepítését és használatát választja, akkor ehhez az oktatóanyaghoz az Azure CLI 2.0.20-as vagy újabb verziójára lesz szükség. A verzió azonosításához futtassa a következőt: `az --version`. Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 


## <a name="define-a-scale-set-in-a-template"></a>Méretezési csoport meghatározása sablonban
Az Azure Resource Manager-sablonok segítségével egymáshoz kapcsolódó erőforráscsoportokat helyezhet üzembe. A sablonok a JavaScript Object Notation (JSON) formátumban vannak megírva, továbbá az alkalmazás teljes Azure-infrastruktúra környezetét meghatározzák. Egyetlen sablonban hozhatja létre a virtuálisgép-méretezési csoportot, telepítheti az alkalmazásokat és konfigurálhatja az automatikus méretezési szabályokat. Különféle változók és paraméterek segítségével a sablon többször is felhasználható meglévő méretezési csoportok frissítésére vagy újabbak létrehozására. A sablonokat az Azure Portal, az Azure CLI 2.0 vagy az Azure PowerShell használatával, illetve folyamatos integrációs (CI) / folyamatos továbbítási (CD) folyamatokon keresztül telepítheti.

További információ a sablonokkal kapcsolatban: [Az Azure Resource Manager áttekintése](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment).

Méretezési csoport sablon alapján való létrehozásához definiálnia kell a megfelelő erőforrásokat. A virtuálisgép-méretezési csoport erőforrástípus alapvető elemei a következők:

| Tulajdonság                     | A tulajdonság leírása                                  | Példa sablonérték                    |
|------------------------------|----------------------------------------------------------|-------------------------------------------|
| type                         | A létrehozandó Azure-erőforrástípus                            | Microsoft.Compute/virtualMachineScaleSets |
| név                         | A méretezési csoport neve                                       | myScaleSet                                |
| location                     | A méretezési csoport létrehozásának helye                     | USA keleti régiója                                   |
| sku.name                     | A méretezési csoport egyes példányainak virtuálisgép-mérete                  | Standard_A1                               |
| sku.capacity                 | Az először létrehozandó virtuálisgép-példányok száma           | 2                                         |
| upgradePolicy.mode           | A virtuálisgép-példányok frissítésének módja módosítás esetén              | Automatikus                                 |
| imageReference               | A virtuálisgép-példányokhoz használandó platform vagy egyéni rendszerkép | Canonical Ubuntu Server 16.04-LTS         |
| osProfile.computerNamePrefix | Az egyes virtuálisgép-példányokhoz tartozó név előtagja                     | myvmss                                    |
| osProfile.adminUsername      | Az egyes virtuálisgép-példányokhoz tartozó felhasználónév                        | azureuser                                 |
| osProfile.adminPassword      | Az egyes virtuálisgép-példányokhoz tartozó jelszó                        | P@ssw0rd!                                 |

 Az alábbi példa az alapvető méretezésicsoport-erőforrás definícióját mutatja be. A méretezési csoport sablonjának testreszabásához módosíthatja a virtuális gépek méretét vagy kezdeti kapacitását, illetve használhat egy másik platformot vagy egy egyedi rendszerképet.

```json
{
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  "name": "myScaleSet",
  "location": "East US",
  "apiVersion": "2017-12-01",
  "sku": {
    "name": "Standard_A1",
    "capacity": "2"
  },
  "properties": {
    "upgradePolicy": {
      "mode": "Automatic"
    },
    "virtualMachineProfile": {
      "storageProfile": {
        "osDisk": {
          "caching": "ReadWrite",
          "createOption": "FromImage"
        },
        "imageReference":  {
          "publisher": "Canonical",
          "offer": "UbuntuServer",
          "sku": "16.04-LTS",
          "version": "latest"
        }
      },
      "osProfile": {
        "computerNamePrefix": "myvmss",
        "adminUsername": "azureuser",
        "adminPassword": "P@ssw0rd!"
      }
    }
  }
}
```

 A minta rövidségének megőrzése érdekében a virtuális hálózati kártya (NIC) konfigurációját lehagytuk a képről. A további összetevők, például a terheléselosztók szintén nem láthatók. [A cikk végén](#deploy-the-template) egy teljes méretezésicsoport-sablon található.


## <a name="install-an-application"></a>Alkalmazások telepítése
A méretezési csoportok üzembe helyezésekor a virtuálisgép-bővítményekkel biztosíthatóak az üzembe helyezést követő konfigurációs és automatizálási feladatok, például az alkalmazások telepítése. A szkriptek az Azure Storage-ből vagy a GitHubról tölthetők le, illetve megadhatók az Azure Portalon a bővítmény futásidejében. Ha alkalmazni szeretné valamelyik bővítményt a méretezési csoportra, egészítse ki az erőforrás előző példáját az *extensionProfile* szakasszal. A bővítményprofil általában az alábbi tulajdonságokat határozza meg:

- Bővítmény típusa
- Bővítmény kiadója
- Bővítmény verziója
- A konfigurációs vagy telepítési szkriptek helye
- A virtuálisgép-példányokon végrehajtandó parancsok

A [Linux rendszeren futó Python HTTP-kiszolgáló](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sablon az egyéni szkriptbővítmény segítségével telepíti a [Bottle](http://bottlepy.org/docs/dev/) Python webes keretrendszert és egy egyszerű HTTP-kiszolgálót. 

A **fileUris**alatt két szkript van meghatározva - az *installserver.sh* és a *workserver.py*. A rendszer letölti a fájlokat a GitHubról, majd a *commandToExecute* a `bash installserver.sh` fájlt futtatja az alkalmazás letöltéséhez és telepítéséhez:

```json
"extensionProfile": {
  "extensions": [
    {
      "name": "AppInstall",
      "properties": {
        "publisher": "Microsoft.Azure.Extensions",
        "type": "CustomScript",
        "typeHandlerVersion": "2.0",
        "autoUpgradeMinorVersion": true,
        "settings": {
          "fileUris": [
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/installserver.sh",
            "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/workserver.py"
          ],
          "commandToExecute": "bash installserver.sh"
        }
      }
    }
  ]
}
```


## <a name="deploy-the-template"></a>A sablon üzembe helyezése
A [Linux rendszeren futó Python HTTP-kiszolgáló](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale) sablont a következő **Üzembe helyezés az Azure-ban** gombbal helyezheti üzembe. A gomb megnyomására megnyílik az Azure Portal, betöltődik a teljes sablon, és a rendszer néhány paraméter megadását kéri (például a méretezési csomag neve, a példányok száma és a rendszergazdai hitelesítő adatok).

[![Sablon üzembe helyezése az Azure-ban](media/virtual-machine-scale-sets-create-template/deploy-button.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-vmss-bottle-autoscale%2Fazuredeploy.json)

Az Azure CLI 2.0 használatával is telepítheti a Linux rendszeren futó Python HTTP-kiszolgálót az [az group deployment create](/cli/azure/group/deployment#create) paranccsal a következő módon:

```azurecli-interactive
# Create a resource group
az group create --name myResourceGroup --location EastUS

# Deploy template into resource group
az group deployment create \
    --resource-group myResourceGroup \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vmss-bottle-autoscale/azuredeploy.json
```

A kérések megválaszolásával adja meg a méretezési csoport nevét, a példányok számát és a virtuálisgép-példányok rendszergazdai hitelesítő adatait. A méretezési csoport és a támogató erőforrások létrehozása eltarthat néhány percig.


## <a name="test-your-sample-application"></a>A mintaalkalmazás tesztelése
Ha működés közben szeretné megtekinteni az alkalmazást, kérje le a terheléselosztó nyilvános IP-címét az [az network public-ip list](/cli/azure/network/public-ip#show) paranccsal a következők szerint:

```azurecli-interactive
az network public-ip list \
    --resource-group myResourceGroup \
    --query [*].ipAddress -o tsv
```

Adja meg a terheléselosztó nyilvános IP-címét egy webböngészőben a következő formátumban: *http://publicIpAddress:9000/do_work*. A terheléselosztó az egyik virtuálisgép-példányra terjeszti a forgalmat, ahogy az a következő példában látható:

![Alapértelmezett weboldal az NGINX-ben](media/virtual-machine-scale-sets-create-template/running-python-app.png)


## <a name="clean-up-resources"></a>Az erőforrások eltávolítása
Ha már nincs rá szükség, az [az group delete](/cli/azure/group#delete) paranccsal eltávolítható az erőforráscsoport, a méretezési csoport és az összes kapcsolódó erőforrás a következők szerint:

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>További lépések
Ebben az első lépéseket bemutató cikkben egy Linux méretezési csoportot hoztunk létre egy Azure-sablonnal, valamint az egyéni szkriptbővítménnyel egy alapszintű Python-webkiszolgálót telepítettünk a VM-példányokon. A nagyobb méretezhetőség és automatizáció érdekében bővítse méretezési csoportját az alábbi útmutatók segítségével:

- [Alkalmazások üzembe helyezése virtuálisgép-méretezési csoportokon](virtual-machine-scale-sets-deploy-app.md)
- Automatikus méretezés az [Azure CLI](virtual-machine-scale-sets-autoscale-cli.md), az [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md), illetve az [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md) használatával.
- [Az operációs rendszer automatikus frissítéseinek használata a méretezési csoport virtuálisgép-példányain](virtual-machine-scale-sets-automatic-upgrade.md)
