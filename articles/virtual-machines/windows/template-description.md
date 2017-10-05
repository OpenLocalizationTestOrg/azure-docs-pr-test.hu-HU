---
title: "Az Azure Resource Manager-sablonokban a virtuális gépek |} A Microsoft Azure"
description: "További tudnivalók hogyan Azure Resource Manager-sablonokban a virtuális gép erőforrás van definiálva."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f63ab5cc-45b8-43aa-a4e7-69dc42adbb99
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: davidmu
ms.openlocfilehash: d9b9121bc5e38396ba4def6c17f9b373c2b48056
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="3d88e-103">Az Azure Resource Manager-sablonokban a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="3d88e-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="3d88e-104">Ez a cikk ismerteti az Azure Resource Manager-sablonokban a virtuális gép aspektusait.</span><span class="sxs-lookup"><span data-stu-id="3d88e-104">This article describes aspects of an Azure Resource Manager template that apply to virtual machines.</span></span> <span data-ttu-id="3d88e-105">Ez a cikk nem alkalmazható a teljes sablont hoz létre virtuális gépet; az adott erőforrás-definíciókban storage-fiókok, a hálózati adapterek, a nyilvános IP-címek és a virtuális hálózatok kell.</span><span class="sxs-lookup"><span data-stu-id="3d88e-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="3d88e-106">Hogyan ezekkel az erőforrásokkal együtt definiálható kapcsolatos további információkért tekintse meg a [Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3d88e-106">For more information about how these resources can be defined together, see the [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="3d88e-107">Nincsenek a sok [a tárban lévő sablonok](https://azure.microsoft.com/documentation/templates/?term=VM) , amelyek tartalmazzák a virtuális gép erőforrásához.</span><span class="sxs-lookup"><span data-stu-id="3d88e-107">There are many [templates in the gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include the VM resource.</span></span> <span data-ttu-id="3d88e-108">Nem minden sablonként szereplő összetevőit itt.</span><span class="sxs-lookup"><span data-stu-id="3d88e-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="3d88e-109">Ez a példa bemutatja a jellemző erőforrás szakaszában megadott számú virtuális gépek létrehozására szolgáló sablont:</span><span class="sxs-lookup"><span data-stu-id="3d88e-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

```json
"resources": [
  { 
    "apiVersion": "2016-04-30-preview", 
    "type": "Microsoft.Compute/virtualMachines", 
    "name": "[concat('myVM', copyindex())]", 
    "location": "[resourceGroup().location]",
    "copy": {
      "name": "virtualMachineLoop", 
      "count": "[parameters('numberOfInstances')]"
    },
    "dependsOn": [
      "[concat('Microsoft.Network/networkInterfaces/myNIC', copyindex())]" 
    ], 
    "properties": { 
      "hardwareProfile": { 
        "vmSize": "Standard_DS1" 
      }, 
      "osProfile": { 
        "computername": "[concat('myVM', copyindex())]", 
        "adminUsername": "[parameters('adminUsername')]", 
        "adminPassword": "[parameters('adminPassword')]" 
      }, 
      "storageProfile": { 
        "imageReference": { 
          "publisher": "MicrosoftWindowsServer", 
          "offer": "WindowsServer", 
          "sku": "2012-R2-Datacenter", 
          "version": "latest" 
        }, 
        "osDisk": { 
          "name": "[concat('myOSDisk', copyindex())]",
          "caching": "ReadWrite", 
          "createOption": "FromImage" 
        },
        "dataDisks": [
          {
            "name": "[concat('myDataDisk', copyindex())]",
            "diskSizeGB": "100",
            "lun": 0,
            "createOption": "Empty"
          }
        ] 
      }, 
      "networkProfile": { 
        "networkInterfaces": [ 
          { 
            "id": "[resourceId('Microsoft.Network/networkInterfaces',
              concat('myNIC', copyindex()))]" 
          } 
        ] 
      },
      "diagnosticsProfile": {
        "bootDiagnostics": {
          "enabled": "true",
          "storageUri": "[concat('https://', variables('storageName'), '.blob.core.windows.net')]"
        }
      } 
    },
    "resources": [ 
      { 
        "name": "Microsoft.Insights.VMDiagnosticsSettings", 
        "type": "extensions", 
        "location": "[resourceGroup().location]", 
        "apiVersion": "2016-03-30", 
        "dependsOn": [ 
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
        ], 
        "properties": { 
          "publisher": "Microsoft.Azure.Diagnostics", 
          "type": "IaaSDiagnostics", 
          "typeHandlerVersion": "1.5", 
          "autoUpgradeMinorVersion": true, 
          "settings": { 
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
            variables('wadmetricsresourceid'), 
            concat('myVM', copyindex()),
            variables('wadcfgxend')))]", 
            "storageAccount": "[variables('storageName')]" 
          }, 
          "protectedSettings": { 
            "storageAccountName": "[variables('storageName')]", 
            "storageAccountKey": "[listkeys(variables('accountid'), 
              '2015-06-15').key1]", 
            "storageAccountEndPoint": "https://core.windows.net" 
          } 
        } 
      },
      {
        "name": "MyCustomScriptExtension",
        "type": "extensions",
        "apiVersion": "2016-03-30",
        "location": "[resourceGroup().location]",
        "dependsOn": [
          "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
        ],
        "properties": {
          "publisher": "Microsoft.Compute",
          "type": "CustomScriptExtension",
          "typeHandlerVersion": "1.7",
          "autoUpgradeMinorVersion": true,
          "settings": {
            "fileUris": [
              "[concat('https://', variables('storageName'),
                '.blob.core.windows.net/customscripts/start.ps1')]" 
            ],
            "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
          }
        }
      } 
    ]
  } 
]
``` 

> [!NOTE] 
><span data-ttu-id="3d88e-110">Ez a példa egy korábban létrehozott tárfiókot támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="3d88e-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="3d88e-111">Ehhez a sablon alapján létrehozhatja a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="3d88e-111">You could create the storage account by deploying it from the template.</span></span> <span data-ttu-id="3d88e-112">A példa is egy hálózati adapter és a tőle függő erőforrások, akkor lehet a sablonban definiált támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="3d88e-112">The example also relies on a network interface and its dependent resources that would be defined in the template.</span></span> <span data-ttu-id="3d88e-113">A példa nem mutatja be ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3d88e-113">These resources are not shown in the example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="3d88e-114">API-verzió</span><span class="sxs-lookup"><span data-stu-id="3d88e-114">API Version</span></span>

<span data-ttu-id="3d88e-115">Sablon használatával erőforrások telepítésekor akkor adja meg a használandó API verzióját.</span><span class="sxs-lookup"><span data-stu-id="3d88e-115">When you deploy resources using a template, you have to specify a version of the API to use.</span></span> <span data-ttu-id="3d88e-116">A példa bemutatja a virtuálisgép-erőforrás a apiVersion elem használatával:</span><span class="sxs-lookup"><span data-stu-id="3d88e-116">The example shows the virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="3d88e-117">Az API-t ad meg a sablon a verziószáma mely tulajdonságok meghatározhatja a sablonban.</span><span class="sxs-lookup"><span data-stu-id="3d88e-117">The version of the API you specify in your template affects which properties you can define in the template.</span></span> <span data-ttu-id="3d88e-118">A legújabb API verziót általában ki kell választania sablonok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="3d88e-118">In general, you should select the most recent API version when creating templates.</span></span> <span data-ttu-id="3d88e-119">A meglévő sablonok eldöntheti, hogy továbbra is az korábbi API-verziót használja, vagy a sablont az új szolgáltatások előnyeinek legújabb verziójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="3d88e-119">For existing templates, you can decide whether you want to continue using an earlier API version, or update your template for the latest version to take advantage of new features.</span></span>

<span data-ttu-id="3d88e-120">Ezek a lehetőségek lekérhesse a legújabb API-verziók használata:</span><span class="sxs-lookup"><span data-stu-id="3d88e-120">Use these opportunities for getting the latest API versions:</span></span>

- <span data-ttu-id="3d88e-121">REST API - [összes erőforrás-szolgáltatók felsorolása](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="3d88e-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="3d88e-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="3d88e-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="3d88e-123">Az Azure CLI 2.0 - [az szolgáltató megjelenítése](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="3d88e-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="3d88e-124">Paraméterek és változók</span><span class="sxs-lookup"><span data-stu-id="3d88e-124">Parameters and variables</span></span>

<span data-ttu-id="3d88e-125">[Paraméterek](../../resource-group-authoring-templates.md) megkönnyíti, hogy a sablon értékeket megadni, ha.</span><span class="sxs-lookup"><span data-stu-id="3d88e-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you to specify values for the template when you run it.</span></span> <span data-ttu-id="3d88e-126">A Paraméterek szakaszban szerepel a példában:</span><span class="sxs-lookup"><span data-stu-id="3d88e-126">This parameters section is used in the example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="3d88e-127">A példa sablon telepítésekor adhatja értékek a nevet és jelszót a rendszergazdai fiók létrehozása minden virtuális gép és a virtuális gépek számát.</span><span class="sxs-lookup"><span data-stu-id="3d88e-127">When you deploy the example template, you enter values for the name and password of the administrator account on each VM and the number of VMs to create.</span></span> <span data-ttu-id="3d88e-128">Lehetősége van a paraméterértékek meghatározásáról sablonnal felügyelt külön fájlban, vagy megadása az értékek, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="3d88e-128">You have the option of specifying parameter values in a separate file that's managed with the template, or providing values when prompted.</span></span>

<span data-ttu-id="3d88e-129">[Változók](../../resource-group-authoring-templates.md) könnyen használt ismételten egész, vagy idővel megváltozhat, hogy a sablonban szereplő értékek beállítása.</span><span class="sxs-lookup"><span data-stu-id="3d88e-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you to set up values in the template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="3d88e-130">A változók szakaszban szerepel a példában:</span><span class="sxs-lookup"><span data-stu-id="3d88e-130">This variables section is used in the example:</span></span>

```
"variables": { 
  "storageName": "mystore1",
  "accountid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name,
  '/providers/','Microsoft.Storage/storageAccounts/', variables('storageName'))]", 
  "wadlogs": "<WadCfg> 
    <DiagnosticMonitorConfiguration overallQuotaInMB=\"4096\" xmlns=\"http://schemas.microsoft.com/ServiceHosting/2010/10/DiagnosticsConfiguration\"> 
      <DiagnosticInfrastructureLogs scheduledTransferLogLevelFilter=\"Error\"/> 
      <WindowsEventLog scheduledTransferPeriod=\"PT1M\" > 
        <DataSource name=\"Application!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"Security!*[System[(Level = 1 or Level = 2)]]\" /> 
        <DataSource name=\"System!*[System[(Level = 1 or Level = 2)]]\" />
      </WindowsEventLog>", 
  "wadperfcounters": "<PerformanceCounters scheduledTransferPeriod=\"PT1M\">
      <PerformanceCounterConfiguration counterSpecifier=\"\\Process(_Total)\\Thread Count\" sampleRate=\"PT15S\" unit=\"Count\">
        <annotation displayName=\"Threads\" locale=\"en-us\"/>
      </PerformanceCounterConfiguration>
    </PerformanceCounters>", 
  "wadcfgxstart": "[concat(variables('wadlogs'), variables('wadperfcounters'), 
    '<Metrics resourceId=\"')]", 
  "wadmetricsresourceid": "[concat('/subscriptions/', subscription().subscriptionId, 
    '/resourceGroups/', resourceGroup().name , 
    '/providers/', 'Microsoft.Compute/virtualMachines/')]", 
  "wadcfgxend": "\"><MetricAggregation scheduledTransferPeriod=\"PT1H\"/>
    <MetricAggregation scheduledTransferPeriod=\"PT1M\"/>
    </Metrics></DiagnosticMonitorConfiguration>
    </WadCfg>"
}, 
```

<span data-ttu-id="3d88e-131">A példa sablon telepítésekor változó a neve és azonosítója a korábban létrehozott tárfiókot használ.</span><span class="sxs-lookup"><span data-stu-id="3d88e-131">When you deploy the example template, variable values are used for the name and identifier of the previously created storage account.</span></span> <span data-ttu-id="3d88e-132">Adja meg a beállításokat a diagnosztikai bővítmény változók is használhatók.</span><span class="sxs-lookup"><span data-stu-id="3d88e-132">Variables are also used to provide the settings for the diagnostic extension.</span></span> <span data-ttu-id="3d88e-133">Használja a [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../../resource-manager-template-best-practices.md) segítségével eldöntheti, hogy a paraméterek és változók alkalmazása a sablon felépítésének módját.</span><span class="sxs-lookup"><span data-stu-id="3d88e-133">Use the [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) to help you decide how you want to structure the parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="3d88e-134">Erőforrás hurkok</span><span class="sxs-lookup"><span data-stu-id="3d88e-134">Resource loops</span></span>

<span data-ttu-id="3d88e-135">Ha egynél több virtuális gép van szükség az alkalmazás, egy sablon egy másolás elem is használhatja.</span><span class="sxs-lookup"><span data-stu-id="3d88e-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="3d88e-136">A választható elem végighalad a virtuális gépek paraméterként megadott számát létrehozása:</span><span class="sxs-lookup"><span data-stu-id="3d88e-136">This optional element loops through creating the number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="3d88e-137">A példa is, láthatja, hogy ciklusindex használja, ha néhány értéket az erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3d88e-137">Also, notice in the example that the loop index is used when specifying some of the values for the resource.</span></span> <span data-ttu-id="3d88e-138">Például három példányszám ad meg, ha az operációs rendszer lemezén a következők myOSDisk1, myOSDisk2, és myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="3d88e-138">For example, if you entered an instance count of three, the names of the operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="3d88e-139">A példa felügyelt lemezt a virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="3d88e-139">This example uses managed disks for the virtual machines.</span></span>
>
>

<span data-ttu-id="3d88e-140">Ne feledje, hogy egy erőforrás hurkot létrehozása a sablonban előfordulhat, hogy a hurok létrehozásakor, vagy más erőforrások eléréséhez használja.</span><span class="sxs-lookup"><span data-stu-id="3d88e-140">Keep in mind that creating a loop for one resource in the template may require you to use the loop when creating or accessing other resources.</span></span> <span data-ttu-id="3d88e-141">Például több virtuális gép nem használható hálózati adaptert, ha a sablon végighalad három virtuális gépek létrehozása azt kell is ismétlése három hálózati adapterek létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3d88e-141">For example, multiple VMs can’t use the same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="3d88e-142">A hálózati adaptert egy virtuális géphez hozzárendelésekor ciklusindex alapján határozza meg azt:</span><span class="sxs-lookup"><span data-stu-id="3d88e-142">When assigning a network interface to a VM, the loop index is used to identify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="3d88e-143">Függőségek</span><span class="sxs-lookup"><span data-stu-id="3d88e-143">Dependencies</span></span>

<span data-ttu-id="3d88e-144">A legtöbb erőforrást megfelelő működéséhez más erőforrások függenek.</span><span class="sxs-lookup"><span data-stu-id="3d88e-144">Most resources depend on other resources to work correctly.</span></span> <span data-ttu-id="3d88e-145">Virtuális gépek rendelve, a virtuális hálózat és a teendő, hogy kell-e a hálózati adaptert kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3d88e-145">Virtual machines must be associated with a virtual network and to do that it needs a network interface.</span></span> <span data-ttu-id="3d88e-146">A [dependsOn](../../resource-group-define-dependencies.md) elem segítségével győződjön meg arról, hogy a virtuális gépek létrehozása előtt használható készen áll-e hálózati kapcsolat:</span><span class="sxs-lookup"><span data-stu-id="3d88e-146">The [dependsOn](../../resource-group-define-dependencies.md) element is used to make sure that the network interface is ready to be used before the VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="3d88e-147">Erőforrás-kezelő párhuzamosan telepíti, amelyek nem függenek más erőforrás telepített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3d88e-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="3d88e-148">Ügyeljen arra, hogy függőségek beállításakor, mert a szükségtelen függőségek megadásával véletlenül lelassíthatja a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="3d88e-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="3d88e-149">Függőségek is láncolt több forrásanyagok segítségével.</span><span class="sxs-lookup"><span data-stu-id="3d88e-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="3d88e-150">Például a hálózati illesztő a nyilvános IP-cím és a virtuális hálózati erőforrások függ.</span><span class="sxs-lookup"><span data-stu-id="3d88e-150">For example, the network interface depends on the public IP address and virtual network resources.</span></span>

<span data-ttu-id="3d88e-151">Hogyan tudja, hogy szükség-e egy függőséget?</span><span class="sxs-lookup"><span data-stu-id="3d88e-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="3d88e-152">Tekintse meg a sablonban beállított értéket.</span><span class="sxs-lookup"><span data-stu-id="3d88e-152">Look at the values you set in the template.</span></span> <span data-ttu-id="3d88e-153">Ha a virtuális gép erőforrás definition mutat, amely ugyanazt a sablont telepítve van egy másik erőforrás eleme, egy függőségi kell.</span><span class="sxs-lookup"><span data-stu-id="3d88e-153">If an element in the virtual machine resource definition points to another resource that is deployed in the same template, you need a dependency.</span></span> <span data-ttu-id="3d88e-154">A Példa virtuális gép például egy hálózati profil határozza meg:</span><span class="sxs-lookup"><span data-stu-id="3d88e-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="3d88e-155">Ez a tulajdonság beállításához a hálózati illesztő léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="3d88e-155">To set this property, the network interface must exist.</span></span> <span data-ttu-id="3d88e-156">Ezért függőség van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3d88e-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="3d88e-157">Szükség függőség beállítása, amikor egy erőforrást (gyermek) egy másik erőforrás (szülő) van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="3d88e-157">You also need to set a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="3d88e-158">Például a diagnosztikai beállításokat és egyéni parancsfájl-kiterjesztés is definiálhatók a virtuális gép gyermek erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="3d88e-158">For example, the diagnostic settings and custom script extensions are both defined as child resources of the virtual machine.</span></span> <span data-ttu-id="3d88e-159">Amíg nem létezik a virtuális gép nem hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="3d88e-159">They cannot be created until the virtual machine exists.</span></span> <span data-ttu-id="3d88e-160">Ezért mindkét erőforrás meg van jelölve, a virtuális gép függ.</span><span class="sxs-lookup"><span data-stu-id="3d88e-160">Therefore, both resources are marked as dependent on the virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="3d88e-161">Profilok</span><span class="sxs-lookup"><span data-stu-id="3d88e-161">Profiles</span></span>

<span data-ttu-id="3d88e-162">Több profil elemek használt virtuálisgép-erőforrás definiálásakor.</span><span class="sxs-lookup"><span data-stu-id="3d88e-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="3d88e-163">Néhány szükséges és választható.</span><span class="sxs-lookup"><span data-stu-id="3d88e-163">Some are required and some are optional.</span></span> <span data-ttu-id="3d88e-164">Például a hardwareProfile, osProfile, storageProfile és networkProfile elemek szükségesek, de a diagnosticsProfile nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="3d88e-164">For example, the hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but the diagnosticsProfile is optional.</span></span> <span data-ttu-id="3d88e-165">Ezeket a profilokat, mint beállításainak megadása:</span><span class="sxs-lookup"><span data-stu-id="3d88e-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="3d88e-166">méret</span><span class="sxs-lookup"><span data-stu-id="3d88e-166">size</span></span>](sizes.md)
- <span data-ttu-id="3d88e-167">[név](/architecture/best-practices/naming-conventions) és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="3d88e-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="3d88e-168">lemez és [operációsrendszer-beállításokat](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="3d88e-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="3d88e-169">hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="3d88e-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="3d88e-170">Rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="3d88e-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="3d88e-171">A lemezek és lemezképek</span><span class="sxs-lookup"><span data-stu-id="3d88e-171">Disks and images</span></span>
   
<span data-ttu-id="3d88e-172">Az Azure, a vhd-fájlok jelenthet [lemezek vagy képeket](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d88e-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="3d88e-173">Ha egy vhd-fájlt az operációs rendszernek kell lennie egy adott VM kifejezetten, azt nevezzük lemezt.</span><span class="sxs-lookup"><span data-stu-id="3d88e-173">When the operating system in a vhd file is specialized to be a specific VM, it is referred to as a disk.</span></span> <span data-ttu-id="3d88e-174">Ha a vhd-fájlt az operációs rendszer általánosított sok virtuális gép létrehozásához használt, azt nevezzük lemezkép.</span><span class="sxs-lookup"><span data-stu-id="3d88e-174">When the operating system in a vhd file is generalized to be used to create many VMs, it is referred to as an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="3d88e-175">Hozzon létre új virtuális gépek és az új lemezt, a platformlemezkép</span><span class="sxs-lookup"><span data-stu-id="3d88e-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="3d88e-176">Amikor létrehoz egy virtuális Gépet, határozza meg, milyen operációs rendszer használatához.</span><span class="sxs-lookup"><span data-stu-id="3d88e-176">When you create a VM, you must decide what operating system to use.</span></span> <span data-ttu-id="3d88e-177">Az imageReference elem egy új virtuális gép operációs rendszerének azonosítására szolgál.</span><span class="sxs-lookup"><span data-stu-id="3d88e-177">The imageReference element is used to define the operating system of a new VM.</span></span> <span data-ttu-id="3d88e-178">A példa bemutatja, a következő definícióját: a Windows Server operációs rendszert:</span><span class="sxs-lookup"><span data-stu-id="3d88e-178">The example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="3d88e-179">Ha szeretne létrehozni a Linux operációs rendszert, ez a definíció használhatja:</span><span class="sxs-lookup"><span data-stu-id="3d88e-179">If you want to create a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="3d88e-180">Az operációsrendszer-lemez konfigurációs beállításait az osDisk elemhez rendelt.</span><span class="sxs-lookup"><span data-stu-id="3d88e-180">Configuration settings for the operating system disk are assigned with the osDisk element.</span></span> <span data-ttu-id="3d88e-181">A példa egy új felügyelt lemezes meghatározása a gyorsítótár módban **ReadWrite** , és hogy a lemez létrehozása folyamatban van a egy [platformlemezképet](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="3d88e-181">The example defines a new managed disk with the caching mode set to **ReadWrite** and that the disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="3d88e-182">Új virtuális gépek létrehozása a meglévő felügyelt lemezekből</span><span class="sxs-lookup"><span data-stu-id="3d88e-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="3d88e-183">Ha azt szeretné, a meglévő lemezt a virtuális gépek létrehozásához, távolítsa el az imageReference, és a osProfile elemek, és a következő lemez beállítások megadása:</span><span class="sxs-lookup"><span data-stu-id="3d88e-183">If you want to create virtual machines from existing disks, remove the imageReference and the osProfile elements and define these disk settings:</span></span>

```
"osDisk": { 
  "osType": "Windows",
  "managedDisk": { 
    "id": "[resourceId('Microsoft.Compute/disks', [concat('myOSDisk', copyindex())])]" 
  }, 
  "caching": "ReadWrite",
  "createOption": "Attach" 
},
```

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="3d88e-184">Hozzon létre új virtuális gépek egy felügyelt lemezképből</span><span class="sxs-lookup"><span data-stu-id="3d88e-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="3d88e-185">Ha egy felügyelt képre egy virtuális gép létrehozása, módosítsa az imageReference elem, és a következő lemez beállítások megadása:</span><span class="sxs-lookup"><span data-stu-id="3d88e-185">If you want to create a virtual machine from a managed image, change the imageReference element and define these disk settings:</span></span>

```
"storageProfile": { 
  "imageReference": {
    "id": "[resourceId('Microsoft.Compute/images', 'myImage')]"
  },
  "osDisk": { 
    "name": "[concat('myOSDisk', copyindex())]",
    "osType": "Windows",
    "caching": "ReadWrite", 
    "createOption": "FromImage" 
  }
},
```

### <a name="attach-data-disks"></a><span data-ttu-id="3d88e-186">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="3d88e-186">Attach data disks</span></span>

<span data-ttu-id="3d88e-187">Az adatlemezek opcionálisan a virtuális gépeket adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="3d88e-187">You can optionally add data disks to the VMs.</span></span> <span data-ttu-id="3d88e-188">A [lemezek száma](sizes.md) használt operációsrendszer-lemez méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="3d88e-188">The [number of disks](sizes.md) depends on the size of operating system disk that you use.</span></span> <span data-ttu-id="3d88e-189">A virtuális gépek Standard_DS1_v2 beállítása méretű a számukra fel adatlemezek maximális számának két.</span><span class="sxs-lookup"><span data-stu-id="3d88e-189">With the size of the VMs set to Standard_DS1_v2, the maximum number of data disks that could be added to the them is two.</span></span> <span data-ttu-id="3d88e-190">A példában egy felügyelt adatlemezt ad hozzá minden egyes virtuális gép:</span><span class="sxs-lookup"><span data-stu-id="3d88e-190">In the example, one managed data disk is being added to each VM:</span></span>

```
"dataDisks": [
  {
    "name": "[concat('myDataDisk', copyindex())]",
    "diskSizeGB": "100",
    "lun": 0, 
    "caching": "ReadWrite",
    "createOption": "Empty"
  }
],
```

## <a name="extensions"></a><span data-ttu-id="3d88e-191">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="3d88e-191">Extensions</span></span>

<span data-ttu-id="3d88e-192">Bár a [bővítmények](extensions-features.md) külön forrást, szorosan a virtuális gépek vannak társítva.</span><span class="sxs-lookup"><span data-stu-id="3d88e-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied to VMs.</span></span> <span data-ttu-id="3d88e-193">Bővítmények gyermek erőforrásként a virtuális gép vagy egy külön erőforrásként lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="3d88e-193">Extensions can be added as a child resource of the VM or as a separate resource.</span></span> <span data-ttu-id="3d88e-194">A példa azt mutatja meg a [diagnosztika bővítmény](extensions-diagnostics-template.md) kíván hozzáadni a virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="3d88e-194">The example shows the [Diagnostics Extension](extensions-diagnostics-template.md) being added to the VMs:</span></span>

```
{ 
  "name": "Microsoft.Insights.VMDiagnosticsSettings", 
  "type": "extensions", 
  "location": "[resourceGroup().location]", 
  "apiVersion": "2016-03-30", 
  "dependsOn": [ 
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]" 
  ], 
  "properties": { 
    "publisher": "Microsoft.Azure.Diagnostics", 
    "type": "IaaSDiagnostics", 
    "typeHandlerVersion": "1.5", 
    "autoUpgradeMinorVersion": true, 
    "settings": { 
      "xmlCfg": "[base64(concat(variables('wadcfgxstart'), 
      variables('wadmetricsresourceid'), 
      concat('myVM', copyindex()),
      variables('wadcfgxend')))]", 
      "storageAccount": "[variables('storageName')]" 
    }, 
    "protectedSettings": { 
      "storageAccountName": "[variables('storageName')]", 
      "storageAccountKey": "[listkeys(variables('accountid'), 
        '2015-06-15').key1]", 
      "storageAccountEndPoint": "https://core.windows.net" 
    } 
  } 
},
```

<span data-ttu-id="3d88e-195">A bővítmény erőforrás storageName változó és a diagnosztikai változók használatával adjon meg értékeket.</span><span class="sxs-lookup"><span data-stu-id="3d88e-195">This extension resource uses the storageName variable and the diagnostic variables to provide values.</span></span> <span data-ttu-id="3d88e-196">Ha azt szeretné, a bővítmény által gyűjtött adatokat, adhat hozzá további teljesítményszámlálók a wadperfcounters változó.</span><span class="sxs-lookup"><span data-stu-id="3d88e-196">If you want to change the data that is collected by this extension, you can add more performance counters to the wadperfcounters variable.</span></span> <span data-ttu-id="3d88e-197">Sikerült meg szeretne adni a diagnosztika adatokat során eltérő tárfiók, mint a Virtuálisgép-lemezek tárolására.</span><span class="sxs-lookup"><span data-stu-id="3d88e-197">You could also choose to put the diagnostics data into a different storage account than where the VM disks are stored.</span></span>

<span data-ttu-id="3d88e-198">Sok kiterjesztések, a virtuális gép telepíthető, de a leghasznosabb valószínűleg a [egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="3d88e-198">There are many extensions that you can install on a VM, but the most useful is probably the [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="3d88e-199">A példában egy start.ps1 nevű PowerShell-parancsfájl futó minden virtuális gép első indításakor:</span><span class="sxs-lookup"><span data-stu-id="3d88e-199">In the example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

```
{
  "name": "MyCustomScriptExtension",
  "type": "extensions",
  "apiVersion": "2016-03-30",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/myVM', copyindex())]"
  ],
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.7",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "[concat('https://', variables('storageName'),
          '.blob.core.windows.net/customscripts/start.ps1')]" 
      ],
      "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted -File start.ps1"
    }
  }
}
```

<span data-ttu-id="3d88e-200">A start.ps1 parancsfájl számos konfigurációs feladat érhető el.</span><span class="sxs-lookup"><span data-stu-id="3d88e-200">The start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="3d88e-201">A példában a virtuális gépek által hozzáadott adatlemezek például nincs inicializálva; Egyéni parancsfájl segítségével azokat.</span><span class="sxs-lookup"><span data-stu-id="3d88e-201">For example, the data disks that are added to the VMs in the example are not initialized; you can use a custom script to initialize them.</span></span> <span data-ttu-id="3d88e-202">Ha több indítási feladatok elvégzéséhez, a start.ps1 fájl segítségével más PowerShell-parancsfájlok az Azure storage-hívás.</span><span class="sxs-lookup"><span data-stu-id="3d88e-202">If you have multiple startup tasks to do, you can use the start.ps1 file to call other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="3d88e-203">A példában PowerShell, de bármely a parancsfájl-kezelési módszer, amely az Ön által használt operációs rendszeren érhető el.</span><span class="sxs-lookup"><span data-stu-id="3d88e-203">The example uses PowerShell, but you can use any scripting method that is available on the operating system that you are using.</span></span>

<span data-ttu-id="3d88e-204">A telepített bővítmények bővítmények beállításai közül a portálon állapota látható:</span><span class="sxs-lookup"><span data-stu-id="3d88e-204">You can see the status of the installed extensions from the Extensions settings in the portal:</span></span>

![Bővítmény állapotának beolvasása](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="3d88e-206">Is kaphat a sémakiterjesztési adatok használatával a **Get-AzureRmVMExtension** PowerShell-parancsot a **virtuálisgép-bővítmény get** Azure CLI 2.0 parancsot, vagy a **sémakiterjesztésiadatokbeolvasása** REST API-T.</span><span class="sxs-lookup"><span data-stu-id="3d88e-206">You can also get extension information by using the **Get-AzureRmVMExtension** PowerShell command, the **vm extension get** Azure CLI 2.0 command, or the **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="3d88e-207">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="3d88e-207">Deployments</span></span>

<span data-ttu-id="3d88e-208">Amikor telepít egy sablont, az Azure erőforrások csoportként telepíteni, és automatikusan hozzárendel egy nevet a központilag telepített csoportban követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="3d88e-208">When you deploy a template, Azure tracks the resources that you deployed as a group and automatically assigns a name to this deployed group.</span></span> <span data-ttu-id="3d88e-209">A központi telepítés neve megegyezik a sablon nevét.</span><span class="sxs-lookup"><span data-stu-id="3d88e-209">The name of the deployment is the same as the name of the template.</span></span>

<span data-ttu-id="3d88e-210">Ha a központi telepítésben lévő erőforrások állapotával fejezetét, használhatja az erőforráscsoport panelről az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="3d88e-210">If you are curious about the status of resources in the deployment, you can use the Resource Group blade in the Azure portal:</span></span>

![Telepítési információk](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="3d88e-212">Azt nem ugyanazt a sablont használni erőforrásokat létrehozni vagy frissíteni a meglévő erőforrásokat probléma.</span><span class="sxs-lookup"><span data-stu-id="3d88e-212">It’s not a problem to use the same template to create resources or to update existing resources.</span></span> <span data-ttu-id="3d88e-213">Parancsok használatával történő telepítése a sablonok, lehetősége van a tegyük fel például, amely [mód](../../resource-group-template-deploy.md) szeretne használni.</span><span class="sxs-lookup"><span data-stu-id="3d88e-213">When you use commands to deploy templates, you have the opportunity to say which [mode](../../resource-group-template-deploy.md) you want to use.</span></span> <span data-ttu-id="3d88e-214">A módot is megadni **Complete** vagy **növekményes**.</span><span class="sxs-lookup"><span data-stu-id="3d88e-214">The mode can be set to either **Complete** or **Incremental**.</span></span> <span data-ttu-id="3d88e-215">Az alapértelmezett érték a növekményes frissítések végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="3d88e-215">The default is to do incremental updates.</span></span> <span data-ttu-id="3d88e-216">Használata esetén ügyeljen a **Complete** módban, mert előfordulhat, hogy véletlenül törli az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3d88e-216">Be careful when using the **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="3d88e-217">Ha a mód beállítása legyen **Complete**, erőforrás-kezelő törlése az erőforráscsoporthoz tartozik, amelyek nincsenek a sablonban lévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="3d88e-217">When you set the mode to **Complete**, Resource Manager deletes any resources in the resource group that are not in the template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3d88e-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3d88e-218">Next Steps</span></span>

- <span data-ttu-id="3d88e-219">Hozzon létre egy saját sablon használatával [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="3d88e-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="3d88e-220">A létrehozott sablon üzembe helyezése [Windows virtuális gép létrehozása egy Resource Manager sablonnal](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="3d88e-220">Deploy the template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="3d88e-221">Megtudhatja, hogyan kezelheti a virtuális gépek által létrehozott [létrehozása és kezelése Windows virtuális gépek az Azure PowerShell modulra](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3d88e-221">Learn how to manage the VMs that you created by reviewing [Create and manage Windows VMs with the Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
