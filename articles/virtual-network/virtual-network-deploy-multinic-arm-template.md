---
title: "virtuális gép több hálózati adapter - Azure Resource Manager-sablon és aaaCreate |} Microsoft Docs"
description: "Egy Azure Resource Manager-sablon használatával több hálózati adapterrel rendelkező virtuális gép létrehozása."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 486f7dd5-cf2f-434c-85d1-b3e85c427def
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f5d9ffcbd40c72dc18ae6de38e739eb5e45bf669
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-multiple-nics-using-a-template"></a>Egy sablon használatával több hálózati adapterrel rendelkező virtuális gép létrehozása
[!INCLUDE [virtual-network-deploy-multinic-arm-selectors-include.md](../../includes/virtual-network-deploy-multinic-arm-selectors-include.md)]

[!INCLUDE [virtual-network-deploy-multinic-intro-include.md](../../includes/virtual-network-deploy-multinic-intro-include.md)]

> [!NOTE]
> Az Azure két különböző üzembe helyezési modellel rendelkezik az erőforrások létrehozásához és használatához: [Resource Manager és klasszikus](../resource-manager-deployment-model.md).  Ez a cikk ismerteti a használatával a Microsoft azt javasolja, a legtöbb új központi telepítés helyett hello hello Resource Manager üzembe helyezési modellben [klasszikus üzembe helyezési modellel](virtual-network-deploy-multinic-classic-ps.md).
> 

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

hello következő lépések használják nevű erőforráscsoport *IaaSStory* hello webkiszolgálók és az erőforráscsoport neve *IaaSStory-háttérrendszer* hello DB kiszolgálók.

## <a name="prerequisites"></a>Előfeltételek
Mielőtt hello DB kiszolgálók hozhat létre, meg kell-e toocreate hello *IaaSStory* erőforráscsoport összes hello szükséges erőforrások ehhez a forgatókönyvhöz. toocreate ezeket az erőforrásokat, végezze el hello a következő lépéseket:

1. Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Hello sablon lapon jobbra toohello **szülő erőforráscsoport**, kattintson a **tooAzure telepítése**.
3. Szükség esetén módosítsa a hello paraméterértékeket, majd hello lépésekkel hello Azure betekintő portál toodeploy hello erőforráscsoportban.

> [!IMPORTANT]
> Győződjön meg arról, hogy a tárfiókok neve egyedi. Ismétlődő tárfiókok neve nem lehet az Azure-ban.
> 

## <a name="understand-hello-deployment-template"></a>Hello központi telepítési sablont ismertetése
Ebben a dokumentációban megadott hello sablon telepítése előtt győződjön meg arról hogy tudomásul veszi, hogyan kezeli. a lépéseket követve hello hello sablon jó áttekintést biztosítanak:

1. Keresse meg a túl[hello sablonlap](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC).
2. Kattintson a **azuredeploy.json** tooopen hello sablonfájl.
3. Értesítés hello *osType* alábbi paraméter. Ez a paraméter akkor használt tooselect milyen VM-lemezkép toouse hello adatbázis-kiszolgáló, valamint több operációs rendszer kapcsolódó beállításokat.

    ```json
    "osType": {
      "type": "string",
      "defaultValue": "Windows",
      "allowedValues": [
        "Windows",
        "Ubuntu"
      ],
      "metadata": {
      "description": "Type of OS toouse for VMs: Windows or Ubuntu."
      }
    },
    ```

4. Görgessen le a változók toohello listája, és ellenőrizze a hello hello definíciója **dbVMSetting** alább felsorolt változók. Hello tömbelemek hello szereplő egyik kap **dbVMSettings** változó. Ha ismeri a szoftver fejlesztési terminológia, megtekintheti a hello **dbVMSettings** egy kivonattáblát vagy dictionary változó.

    ```json
    "dbVMSetting": "[variables('dbVMSettings')[parameters('osType')]]"
    ```

5. Tegyük fel, toodeploy Windows virtuális gépeken futó SQL hello háttér-mellett dönt. A következő majd hello **osType** lenne *Windows*, és hello **dbVMSetting** változóhoz az alább felsorolt hello elemet, amely jelzi a hello hello első értékét tartalmazná **dbVMSettings** változó.

    ```json
    "Windows": {
      "vmSize": "Standard_DS3",
      "publisher": "MicrosoftSQLServer",
      "offer": "SQL2014SP1-WS2012R2",
      "sku": "Standard",
      "version": "latest",
      "vmName": "DB",
      "osdisk": "osdiskdb",
      "datadisk": "datadiskdb",
      "nicName": "NICDB",
      "ipAddress": "192.168.2.",
      "extensionDeployment": "",
      "avsetName": "ASDB",
      "remotePort": 3389,
      "dbPort": 1433
    },
    ```

6. Értesítés hello **vmSize** hello értéket tartalmaz *Standard_DS3*. Csak egyes Virtuálisgép-méretek hello több hálózati adapter használatát teszik lehetővé. Ellenőrizheti, hogy mely Virtuálisgép-méretek támogatja több hálózati adapter hello olvasásával [Windows Virtuálisgép-méretek](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) és [Linux Virtuálisgép-méretek](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) cikkeket.

7. Görgessen lefelé, túl**erőforrások** és a hirdetmény hello első eleme. A tárfiók ismerteti. Ezt a tárfiókot az egyes adatbázisok virtuális gép által használt használt toomaintain hello adatlemezek lesz. Ebben az esetben az egyes adatbázisok VM van egy rendszeres storage-ban tárolt operációsrendszer-lemez, és két adatlemezek SSD-re (prémium) tárolja.

    ```json
    {
      "apiVersion": "2015-05-01-preview",
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[parameters('prmStorageName')]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "Storage Account - Premium"
      },
      "properties": {
      "accountType": "[parameters('prmStorageType')]"
      }
    },
    ```

8. Görgessen le a következő erőforrás toohello, az alább felsorolt. Ehhez az erőforráshoz a hálózati adapter által használt minden virtuális gép adatbázis adatbázis-hozzáférési hello jelöli. Értesítés hello használata hello **másolási** függvényt ehhez az erőforráshoz. hello sablon teszi toodeploy, ahányat csak szeretne, a hello alapján sok virtuális gép **dbCount** paraméter. Ezért toocreate van szüksége az adatbázis eléréséhez, az egyes virtuális gépek egyik hálózati adapter olyan mértékű hello.

    ```json
    {
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[concat(variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
    "location": "[variables('location')]",
    "tags": {
      "displayName": "NetworkInterfaces - DB DA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "privateIPAllocationMethod": "Static",
            "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(4))]",
            "subnet": {
              "id": "[variables('backEndSubnetRef')]"
            }
          }
         }
       ] 
     }
    },
    ```

9. Görgessen le a következő erőforrás toohello, az alább felsorolt. Ehhez az erőforráshoz minden adatbázis VM-kezelésre szolgáló hálózati hello jelöli. Ebben az esetben kell ezek a hálózati adapterek közül az egyes adatbázisok virtuális gép. Értesítés hello **hálózati biztonsági csoporthoz tartozik** elem csatolása, amely lehetővé teszi a hozzáférést tooRDP/SSH toothis NIC csak egy NSG.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[concat(variables('dbVMSetting').nicName, '-RA-',copyindex(1))]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "NetworkInterfaces - DB RA"
    },
    "copy": {
      "name": "dbniccount",
      "count": "[parameters('dbCount')]"
    },
    "properties": {
      "ipConfigurations": [
        {
          "name": "ipconfig1",
          "properties": {
            "networkSecurityGroup": {
             "id": "[resourceId('Microsoft.Network/networkSecurityGroups', parameters('remoteAccessNSGName'))]"
             },
             "privateIPAllocationMethod": "Static",
             "privateIPAddress": "[concat(variables('dbVMSetting').ipAddress,copyindex(54))]",
             "subnet": {
              "id": "[variables('backEndSubnetRef')]"
             }
           }
          }
        ]
      }
    },
```

10. Görgessen le a következő erőforrás toohello, az alább felsorolt. Ez az erőforrás egy rendelkezésre állási készlet toobe összes adatbázis virtuális gépek által megosztott jelöli. Ily módon azt garantálja, hogy mindig lesz egy virtuális gép a karbantartás során futó hello.

    ```json
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/availabilitySets",
      "name": "[variables('dbVMSetting').avsetName]",
      "location": "[variables('location')]",
      "tags": {
        "displayName": "AvailabilitySet - DB"
      }
    },
    ```

11. Görgessen le a következő erőforrás toohello. Ehhez az erőforráshoz hello adatbázis virtuális gépeket, jelöli hello látható első néhány sor alábbi. Értesítés hello használata hello **másolási** újra működik, győződjön meg arról, hogy több virtuális gépek jönnek létre alapján hello **dbCount** paraméter. Észrevette hello **dependsOn** gyűjtemény. Felsorolja alatt álló virtuális gép van telepítve, valamint hello rendelkezésre állási csoportot, és az hello tárfiók hello előtt létrehozott szükséges toobe két hálózati adaptert.

    ```json
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[concat(variables('dbVMSetting').vmName,copyindex(1))]",
    "location": "[variables('location')]",
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-DA-', copyindex(1))]",
      "[concat('Microsoft.Network/networkInterfaces/', variables('dbVMSetting').nicName,'-RA-', copyindex(1))]",
      "[concat('Microsoft.Compute/availabilitySets/', variables('dbVMSetting').avsetName)]",
      "[concat('Microsoft.Storage/storageAccounts/', parameters('prmStorageName'))]"
    ],
    "tags": {
      "displayName": "VMs - DB"
    },
    "copy": {
      "name": "dbvmcount",
      "count": "[parameters('dbCount')]"
    },
    ```

12. Görgessen le a hello VM erőforrás toohello **networkProfile** elem, az alább felsorolt. Figyelje meg, hogy vannak-e két hálózati adaptert, az egyes virtuális gépek hivatkozás alatt. Több hálózati adaptert a virtuális gépek létrehozásakor meg kell adni a hello **elsődleges** hello hálózati adapterek közül az egyik tulajdonság túl*igaz*, és a többi túl hello*hamis*.

    ```json
    "networkProfile": {
      "networkInterfaces": [
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-DA-',copyindex(1)))]",
          "properties": { "primary": true }
        },
        {
          "id": "[resourceId('Microsoft.Network/networkInterfaces', concat(variables('dbVMSetting').nicName,'-RA-',copyindex(1)))]",
          "properties": { "primary": false }
        }
      ]
    }
    ```

## <a name="deploy-hello-arm-template-by-using-click-toodeploy"></a>Központi telepítése hello ARM-sablon használatával toodeploy kattintson

> [!IMPORTANT]
> Ellenőrizze, hogy hajtsa végre a hello [szükséges előfeltételek](#Pre-requisites) lépéseket hello az alábbi utasítások követése előtt.
> 

hello mintasablon elérhető hello nyilvános tárházban hello alapértelmezett értékeket használja toogenerate hello forgatókönyv a fent leírt tartalmazó paraméter fájlt használ. toodeploy toodeploy, a sablon használatával kattintson kövesse [Ez a hivatkozás](https://github.com/Azure/azure-quickstart-templates/tree/master/IaaS-Story/11-MultiNIC), toohello jobbra **erőforráscsoport háttér (lásd a dokumentációt)** kattintson **tooAzure telepítése**, cserélje le hello alapértelmezett paraméterértékek, ha szükséges, és hello utasítások hello portálon.

hello az alábbi ábra hello tartalmát hello új erőforráscsoportot, a telepítés utáni.

![Háttér-erőforráscsoport](./media/virtual-network-deploy-multinic-arm-template/Figure2.png)

## <a name="deploy-hello-template-by-using-powershell"></a>Hello sablon üzembe helyezése a PowerShell használatával
a letöltött PowerShell, toodeploy hello sablon PowerShell telepítése és konfigurálása a hello hello lépések elvégzésével [PowerShell telepítése és konfigurálása](/powershell/azure/overview) a következő cikket, és kövesse az alábbi lépésekkel hello:

Futtassa a hello  **`New-AzureRmResourceGroup`**  erőforrás csoport használatával parancsmag toocreate hello sablont.

```powershell
New-AzureRmResourceGroup -Name IaaSStory-Backend -Location uswest `
TemplateFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json' `
-TemplateParameterFile 'https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json'
```

Várt kimenet:

    ResourceGroupName : IaaSStory-Backend
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    Permissions       :
                        Actions  NotActions
                        =======  ==========
                        *
        Resources         :
                        Name                 Type                                 Location
                        ===================  ===================================  ========
                        ASDB                 Microsoft.Compute/availabilitySets   westus  
                        DB1                  Microsoft.Compute/virtualMachines    westus  
                        DB2                  Microsoft.Compute/virtualMachines    westus  
                        NICDB-DA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-DA-2           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-1           Microsoft.Network/networkInterfaces  westus  
                        NICDB-RA-2           Microsoft.Network/networkInterfaces  westus  
                        wtestvnetstorageprm  Microsoft.Storage/storageAccounts    westus  
    ResourceId        : /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend

## <a name="deploy-hello-template-by-using-hello-azure-cli"></a>Hello sablon üzembe helyezése hello Azure parancssori felület használatával
toodeploy hello sablon hello Azure parancssori felület használatával kövesse hello lépéseket.

1. Ha még sosem használta az Azure parancssori felület, lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) hello utasítások mentése toohello pont, ahol ki kell választania az Azure-fiókja és -előfizetést.
2. Futtassa a hello  **`azure config mode`**  tooswitch tooResource Manager üzemmód, alább látható módon.

    ```azurecli
    azure config mode arm
    ```

    hello várható kimenet követi:

        info:    New mode is arm

3. Nyissa meg hello [paraméterfájl](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.parameters.json), válassza ki annak tartalmát, és mentse azokat a számítógép tooa fájlt. Ehhez a példához mentettük hello paraméterfájl túl*parameters.json*.
4. Futtassa a hello  **`azure group deployment create`**  parancsmag toodeploy hello hello sablonnal és paraméterfájlokkal segítségével új virtuális hálózat fájlokat fent letöltött és módosított. hello kimenet után látható hello lista hello paramétereket ismerteti.

    ```azurecli
    azure group create -n IaaSStory-Backend -l westus --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/azuredeploy.json -e parameters.json
    ```

    Várt kimenet:
   
        info:    Executing command group create
        + Getting resource group IaaSStory-Backend
        + Creating resource group IaaSStory-Backend
        info:    Created resource group IaaSStory-Backend
        + Initializing template configurations and parameters
        + Creating a deployment
        info:    Created template deployment "azuredeploy"
        data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/IaaSStory-Backend
        data:    Name:                IaaSStory-Backend
        data:    Location:            westus
        data:    Provisioning State:  Succeeded
        data:    Tags: null
        data:
        info:    group create command OK

