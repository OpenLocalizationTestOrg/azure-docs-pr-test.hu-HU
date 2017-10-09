---
title: "az Azure Resource Manager sablon aaaVirtual gépek |} A Microsoft Azure"
description: "További tudnivalók hogyan hello virtuálisgép-erőforrást az Azure Resource Manager sablon van definiálva."
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
ms.openlocfilehash: 94adcbe5bf44be72ffc1b920461aed15c4fc025f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a><span data-ttu-id="298e9-103">Az Azure Resource Manager-sablonokban a virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="298e9-103">Virtual machines in an Azure Resource Manager template</span></span>

<span data-ttu-id="298e9-104">Ez a cikk ismerteti az Azure Resource Manager-sablonok, amelyek érvényesek a toovirtual gépek aspektusait.</span><span class="sxs-lookup"><span data-stu-id="298e9-104">This article describes aspects of an Azure Resource Manager template that apply toovirtual machines.</span></span> <span data-ttu-id="298e9-105">Ez a cikk nem alkalmazható a teljes sablont hoz létre virtuális gépet; az adott erőforrás-definíciókban storage-fiókok, a hálózati adapterek, a nyilvános IP-címek és a virtuális hálózatok kell.</span><span class="sxs-lookup"><span data-stu-id="298e9-105">This article doesn’t describe a complete template for creating a virtual machine; for that you need resource definitions for storage accounts, network interfaces, public IP addresses, and virtual networks.</span></span> <span data-ttu-id="298e9-106">Hogyan ezekkel az erőforrásokkal együtt definiálható kapcsolatos további információkért lásd: hello [Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="298e9-106">For more information about how these resources can be defined together, see hello [Resource Manager template walkthrough](../../azure-resource-manager/resource-manager-template-walkthrough.md).</span></span>

<span data-ttu-id="298e9-107">Nincsenek a sok [hello tárban lévő sablonok](https://azure.microsoft.com/documentation/templates/?term=VM) , amelyek tartalmazzák a hello virtuális gép erőforrásához.</span><span class="sxs-lookup"><span data-stu-id="298e9-107">There are many [templates in hello gallery](https://azure.microsoft.com/documentation/templates/?term=VM) that include hello VM resource.</span></span> <span data-ttu-id="298e9-108">Nem minden sablonként szereplő összetevőit itt.</span><span class="sxs-lookup"><span data-stu-id="298e9-108">Not all elements that can be included in a template are described here.</span></span>

<span data-ttu-id="298e9-109">Ez a példa bemutatja a jellemző erőforrás szakaszában megadott számú virtuális gépek létrehozására szolgáló sablont:</span><span class="sxs-lookup"><span data-stu-id="298e9-109">This example shows a typical resource section of a template for creating a specified number of VMs:</span></span>

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
><span data-ttu-id="298e9-110">Ez a példa egy korábban létrehozott tárfiókot támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="298e9-110">This example relies on a storage account that was previously created.</span></span> <span data-ttu-id="298e9-111">Létrehozhat hello tárfiók hello sablonból telepítésével.</span><span class="sxs-lookup"><span data-stu-id="298e9-111">You could create hello storage account by deploying it from hello template.</span></span> <span data-ttu-id="298e9-112">hello példa is egy hálózati adapter és a tőle függő erőforrások, akkor lehet hello sablonban definiált támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="298e9-112">hello example also relies on a network interface and its dependent resources that would be defined in hello template.</span></span> <span data-ttu-id="298e9-113">Hello példa nem mutatja be ezeket az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="298e9-113">These resources are not shown in hello example.</span></span>
>
>

## <a name="api-version"></a><span data-ttu-id="298e9-114">API-verzió</span><span class="sxs-lookup"><span data-stu-id="298e9-114">API Version</span></span>

<span data-ttu-id="298e9-115">Amikor telepít egy sablon használatával erőforrások, toospecify egy verziójával rendelkezik hello API toouse.</span><span class="sxs-lookup"><span data-stu-id="298e9-115">When you deploy resources using a template, you have toospecify a version of hello API toouse.</span></span> <span data-ttu-id="298e9-116">a példában hello hello virtuálisgép-erőforrás a apiVersion elem használatával:</span><span class="sxs-lookup"><span data-stu-id="298e9-116">hello example shows hello virtual machine resource using this apiVersion element:</span></span>

```
"apiVersion": "2016-04-30-preview",
```

<span data-ttu-id="298e9-117">hello verzióját adja meg, ha a sablonban API hello megadására a hello sablon tulajdonságok hatással van.</span><span class="sxs-lookup"><span data-stu-id="298e9-117">hello version of hello API you specify in your template affects which properties you can define in hello template.</span></span> <span data-ttu-id="298e9-118">Általánosságban elmondható választhat hello legújabb API verziót sablonok létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="298e9-118">In general, you should select hello most recent API version when creating templates.</span></span> <span data-ttu-id="298e9-119">A meglévő sablonok eldöntheti, hogy szeretné, hogy egy korábbi verziójával API toocontinue, vagy a hello legújabb verziója tootake új szolgáltatásainak előnyeit a sablon frissítésére.</span><span class="sxs-lookup"><span data-stu-id="298e9-119">For existing templates, you can decide whether you want toocontinue using an earlier API version, or update your template for hello latest version tootake advantage of new features.</span></span>

<span data-ttu-id="298e9-120">Ezek a lehetőségek lekérhesse a legújabb API-verziók hello használata:</span><span class="sxs-lookup"><span data-stu-id="298e9-120">Use these opportunities for getting hello latest API versions:</span></span>

- <span data-ttu-id="298e9-121">REST API - [összes erőforrás-szolgáltatók felsorolása](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span><span class="sxs-lookup"><span data-stu-id="298e9-121">REST API - [List all resource providers](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)</span></span>
- <span data-ttu-id="298e9-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span><span class="sxs-lookup"><span data-stu-id="298e9-122">PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)</span></span>
- <span data-ttu-id="298e9-123">Az Azure CLI 2.0 - [az szolgáltató megjelenítése](https://docs.microsoft.com/cli/azure/provider#show)</span><span class="sxs-lookup"><span data-stu-id="298e9-123">Azure CLI 2.0 - [az provider show](https://docs.microsoft.com/cli/azure/provider#show)</span></span>

## <a name="parameters-and-variables"></a><span data-ttu-id="298e9-124">Paraméterek és változók</span><span class="sxs-lookup"><span data-stu-id="298e9-124">Parameters and variables</span></span>

<span data-ttu-id="298e9-125">[Paraméterek](../../resource-group-authoring-templates.md) könnyítheti meg toospecify értékek hello sablon futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="298e9-125">[Parameters](../../resource-group-authoring-templates.md) make it easy for you toospecify values for hello template when you run it.</span></span> <span data-ttu-id="298e9-126">A Paraméterek szakaszban hello példa szerepel:</span><span class="sxs-lookup"><span data-stu-id="298e9-126">This parameters section is used in hello example:</span></span>

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

<span data-ttu-id="298e9-127">Hello példa sablon telepítésekor adhatja értékek hello nevét és hello rendszergazdai fiók jelszavát minden egyes virtuális gépek toocreate a virtuális gép és hello számára.</span><span class="sxs-lookup"><span data-stu-id="298e9-127">When you deploy hello example template, you enter values for hello name and password of hello administrator account on each VM and hello number of VMs toocreate.</span></span> <span data-ttu-id="298e9-128">Lehetősége van hello paraméterértékek meghatározásáról hello sablonnal felügyelt külön fájlban, vagy biztosító értékek, amikor a rendszer kéri.</span><span class="sxs-lookup"><span data-stu-id="298e9-128">You have hello option of specifying parameter values in a separate file that's managed with hello template, or providing values when prompted.</span></span>

<span data-ttu-id="298e9-129">[Változók](../../resource-group-authoring-templates.md) könnyítheti meg tooset értékeket hello sablonban használt ismételten egész, vagy idővel megváltozhat.</span><span class="sxs-lookup"><span data-stu-id="298e9-129">[Variables](../../resource-group-authoring-templates.md) make it easy for you tooset up values in hello template that are used repeatedly throughout it or that can change over time.</span></span> <span data-ttu-id="298e9-130">A változók szakaszban hello példa szerepel:</span><span class="sxs-lookup"><span data-stu-id="298e9-130">This variables section is used in hello example:</span></span>

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

<span data-ttu-id="298e9-131">Hello példa sablon telepítésekor változók értékeinek hello neve és azonosítója hello korábban létrehozott tárfiókot használ.</span><span class="sxs-lookup"><span data-stu-id="298e9-131">When you deploy hello example template, variable values are used for hello name and identifier of hello previously created storage account.</span></span> <span data-ttu-id="298e9-132">A változókat is használt tooprovide hello beállítások hello diagnosztikai bővítmény.</span><span class="sxs-lookup"><span data-stu-id="298e9-132">Variables are also used tooprovide hello settings for hello diagnostic extension.</span></span> <span data-ttu-id="298e9-133">Használjon hello [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../../resource-manager-template-best-practices.md) toohelp úgy dönt, hogy hogyan toostructure hello paramétereket és változókat a sablonban.</span><span class="sxs-lookup"><span data-stu-id="298e9-133">Use hello [best practices for creating Azure Resource Manager templates](../../resource-manager-template-best-practices.md) toohelp you decide how you want toostructure hello parameters and variables in your template.</span></span>

## <a name="resource-loops"></a><span data-ttu-id="298e9-134">Erőforrás hurkok</span><span class="sxs-lookup"><span data-stu-id="298e9-134">Resource loops</span></span>

<span data-ttu-id="298e9-135">Ha egynél több virtuális gép van szükség az alkalmazás, egy sablon egy másolás elem is használhatja.</span><span class="sxs-lookup"><span data-stu-id="298e9-135">When you need more than one virtual machine for your application, you can use a copy element in a template.</span></span> <span data-ttu-id="298e9-136">A választható elem végighalad a virtuális gépek, paraméterként megadott hello száma létrehozása:</span><span class="sxs-lookup"><span data-stu-id="298e9-136">This optional element loops through creating hello number of VMs that you specified as a parameter:</span></span>

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

<span data-ttu-id="298e9-137">Figyelje meg, amely ciklusindex hello hello példában használt is, hello erőforrás értékek egyes hello megadása esetén.</span><span class="sxs-lookup"><span data-stu-id="298e9-137">Also, notice in hello example that hello loop index is used when specifying some of hello values for hello resource.</span></span> <span data-ttu-id="298e9-138">Például ha példányszámának hello operációs rendszer lemezek három, hello nevek a megadott is myOSDisk1 myOSDisk2 és myOSDisk3:</span><span class="sxs-lookup"><span data-stu-id="298e9-138">For example, if you entered an instance count of three, hello names of hello operating system disks are myOSDisk1, myOSDisk2, and myOSDisk3:</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
><span data-ttu-id="298e9-139">A példa felügyelt lemezek hello virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="298e9-139">This example uses managed disks for hello virtual machines.</span></span>
>
>

<span data-ttu-id="298e9-140">Ne feledje, hogy egy erőforrás hurkot létre a hello sablon előfordulhat, hogy Ön toouse hello hurok létrehozásakor, vagy más erőforrások eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="298e9-140">Keep in mind that creating a loop for one resource in hello template may require you toouse hello loop when creating or accessing other resources.</span></span> <span data-ttu-id="298e9-141">Például a több virtuális gép nem használható hello azonos hálózati adapter, így ha a sablon végighalad végig vezeti a kell is hurok három virtuális gépek létrehozása három hálózati illesztőt.</span><span class="sxs-lookup"><span data-stu-id="298e9-141">For example, multiple VMs can’t use hello same network interface, so if your template loops through creating three VMs it must also loop through creating three network interfaces.</span></span> <span data-ttu-id="298e9-142">A hálózati illesztő tooa VM hozzárendelésekor hello ciklusindex-e a használt tooidentify azt:</span><span class="sxs-lookup"><span data-stu-id="298e9-142">When assigning a network interface tooa VM, hello loop index is used tooidentify it:</span></span>

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a><span data-ttu-id="298e9-143">Függőségek</span><span class="sxs-lookup"><span data-stu-id="298e9-143">Dependencies</span></span>

<span data-ttu-id="298e9-144">A legtöbb erőforrások megfelelően egyéb erőforrások toowork függenek.</span><span class="sxs-lookup"><span data-stu-id="298e9-144">Most resources depend on other resources toowork correctly.</span></span> <span data-ttu-id="298e9-145">Virtuális gépek virtuális hálózati és, hogy kell-e egy adott hálózati csatoló toodo társítva kell lennie.</span><span class="sxs-lookup"><span data-stu-id="298e9-145">Virtual machines must be associated with a virtual network and toodo that it needs a network interface.</span></span> <span data-ttu-id="298e9-146">Hello [dependsOn](../../resource-group-define-dependencies.md) elem használt toomake meg arról, hogy hello hálózati illesztőt készen toobe használt hello virtuális gépek létrehozása előtt:</span><span class="sxs-lookup"><span data-stu-id="298e9-146">hello [dependsOn](../../resource-group-define-dependencies.md) element is used toomake sure that hello network interface is ready toobe used before hello VMs are created:</span></span>

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

<span data-ttu-id="298e9-147">Erőforrás-kezelő párhuzamosan telepíti, amelyek nem függenek más erőforrás telepített erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="298e9-147">Resource Manager deploys in parallel any resources that are not dependent on another resource being deployed.</span></span> <span data-ttu-id="298e9-148">Ügyeljen arra, hogy függőségek beállításakor, mert a szükségtelen függőségek megadásával véletlenül lelassíthatja a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="298e9-148">Be careful when setting dependencies because you can inadvertently slow your deployment by specifying unnecessary dependencies.</span></span> <span data-ttu-id="298e9-149">Függőségek is láncolt több forrásanyagok segítségével.</span><span class="sxs-lookup"><span data-stu-id="298e9-149">Dependencies can chain through multiple resources.</span></span> <span data-ttu-id="298e9-150">Például hello hálózati illesztő hello nyilvános IP-címhez és virtuális hálózati erőforrások függ.</span><span class="sxs-lookup"><span data-stu-id="298e9-150">For example, hello network interface depends on hello public IP address and virtual network resources.</span></span>

<span data-ttu-id="298e9-151">Hogyan tudja, hogy szükség-e egy függőséget?</span><span class="sxs-lookup"><span data-stu-id="298e9-151">How do you know if a dependency is required?</span></span> <span data-ttu-id="298e9-152">Tekintse meg hello értékeket hello sablont.</span><span class="sxs-lookup"><span data-stu-id="298e9-152">Look at hello values you set in hello template.</span></span> <span data-ttu-id="298e9-153">Ha egy elem hello virtuális gépek erőforrás-definícióban hello üzembe helyezett erőforrás tooanother ugyanazt a sablont, függőség van szüksége.</span><span class="sxs-lookup"><span data-stu-id="298e9-153">If an element in hello virtual machine resource definition points tooanother resource that is deployed in hello same template, you need a dependency.</span></span> <span data-ttu-id="298e9-154">A Példa virtuális gép például egy hálózati profil határozza meg:</span><span class="sxs-lookup"><span data-stu-id="298e9-154">For example, your example virtual machine defines a network profile:</span></span>

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

<span data-ttu-id="298e9-155">tooset ezt a tulajdonságot, hello hálózati illesztő léteznie kell.</span><span class="sxs-lookup"><span data-stu-id="298e9-155">tooset this property, hello network interface must exist.</span></span> <span data-ttu-id="298e9-156">Ezért függőség van szüksége.</span><span class="sxs-lookup"><span data-stu-id="298e9-156">Therefore, you need a dependency.</span></span> <span data-ttu-id="298e9-157">Amikor egy erőforrást (gyermek) van meghatározva egy másik erőforrás (szülő) is kell tooset függőséget.</span><span class="sxs-lookup"><span data-stu-id="298e9-157">You also need tooset a dependency when one resource (a child) is defined within another resource (a parent).</span></span> <span data-ttu-id="298e9-158">Például hello diagnosztikai beállítások és egyéni parancsfájl-kiterjesztés is definiálhatók, gyermekszintű erőforrása hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="298e9-158">For example, hello diagnostic settings and custom script extensions are both defined as child resources of hello virtual machine.</span></span> <span data-ttu-id="298e9-159">Ezek nem hozható létre, amíg hello virtuális gép is létezik.</span><span class="sxs-lookup"><span data-stu-id="298e9-159">They cannot be created until hello virtual machine exists.</span></span> <span data-ttu-id="298e9-160">Ezért mindkét erőforrás van megjelölve függő hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="298e9-160">Therefore, both resources are marked as dependent on hello virtual machine.</span></span>

## <a name="profiles"></a><span data-ttu-id="298e9-161">Profilok</span><span class="sxs-lookup"><span data-stu-id="298e9-161">Profiles</span></span>

<span data-ttu-id="298e9-162">Több profil elemek használt virtuálisgép-erőforrás definiálásakor.</span><span class="sxs-lookup"><span data-stu-id="298e9-162">Several profile elements are used when defining a virtual machine resource.</span></span> <span data-ttu-id="298e9-163">Néhány szükséges és választható.</span><span class="sxs-lookup"><span data-stu-id="298e9-163">Some are required and some are optional.</span></span> <span data-ttu-id="298e9-164">Például hello hardwareProfile, osProfile, storageProfile és networkProfile elemek szükségesek, de hello diagnosticsProfile nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="298e9-164">For example, hello hardwareProfile, osProfile, storageProfile, and networkProfile elements are required, but hello diagnosticsProfile is optional.</span></span> <span data-ttu-id="298e9-165">Ezeket a profilokat, mint beállításainak megadása:</span><span class="sxs-lookup"><span data-stu-id="298e9-165">These profiles define settings such as:</span></span>
   
- [<span data-ttu-id="298e9-166">méret</span><span class="sxs-lookup"><span data-stu-id="298e9-166">size</span></span>](sizes.md)
- <span data-ttu-id="298e9-167">[név](/architecture/best-practices/naming-conventions) és hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="298e9-167">[name](/architecture/best-practices/naming-conventions) and credentials</span></span>
- <span data-ttu-id="298e9-168">lemez és [operációsrendszer-beállításokat](cli-ps-findimage.md)</span><span class="sxs-lookup"><span data-stu-id="298e9-168">disk and [operating system settings](cli-ps-findimage.md)</span></span>
- [<span data-ttu-id="298e9-169">hálózati adapter</span><span class="sxs-lookup"><span data-stu-id="298e9-169">network interface</span></span>](../../virtual-network/virtual-networks-multiple-nics.md) 
- <span data-ttu-id="298e9-170">Rendszerindítási diagnosztika</span><span class="sxs-lookup"><span data-stu-id="298e9-170">boot diagnostics</span></span>

## <a name="disks-and-images"></a><span data-ttu-id="298e9-171">A lemezek és lemezképek</span><span class="sxs-lookup"><span data-stu-id="298e9-171">Disks and images</span></span>
   
<span data-ttu-id="298e9-172">Az Azure, a vhd-fájlok jelenthet [lemezek vagy képeket](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="298e9-172">In Azure, vhd files can represent [disks or images](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> <span data-ttu-id="298e9-173">Amikor hello operációs rendszer egy vhd-fájlt a speciális toobe speciális virtuális Gépet, hivatkozott tooas lemez.</span><span class="sxs-lookup"><span data-stu-id="298e9-173">When hello operating system in a vhd file is specialized toobe a specific VM, it is referred tooas a disk.</span></span> <span data-ttu-id="298e9-174">Ha a vhd-fájl hello operációs rendszer általánosított toobe használt toocreate sok virtuális gép, hivatkozott tooas lemezkép.</span><span class="sxs-lookup"><span data-stu-id="298e9-174">When hello operating system in a vhd file is generalized toobe used toocreate many VMs, it is referred tooas an image.</span></span>   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a><span data-ttu-id="298e9-175">Hozzon létre új virtuális gépek és az új lemezt, a platformlemezkép</span><span class="sxs-lookup"><span data-stu-id="298e9-175">Create new virtual machines and new disks from a platform image</span></span>

<span data-ttu-id="298e9-176">Amikor létrehoz egy virtuális Gépet, határozza meg, milyen operációs rendszer toouse.</span><span class="sxs-lookup"><span data-stu-id="298e9-176">When you create a VM, you must decide what operating system toouse.</span></span> <span data-ttu-id="298e9-177">hello imageReference elem használt toodefine hello operációs rendszer egy új virtuális gép állapota.</span><span class="sxs-lookup"><span data-stu-id="298e9-177">hello imageReference element is used toodefine hello operating system of a new VM.</span></span> <span data-ttu-id="298e9-178">hello példa bemutatja, a következő definícióját: a Windows Server operációs rendszert:</span><span class="sxs-lookup"><span data-stu-id="298e9-178">hello example shows a definition for a Windows Server operating system:</span></span>

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

<span data-ttu-id="298e9-179">Ha azt szeretné, hogy a Linux operációs rendszer toocreate, ez a definíció használhatja:</span><span class="sxs-lookup"><span data-stu-id="298e9-179">If you want toocreate a Linux operating system, you might use this definition:</span></span>

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

<span data-ttu-id="298e9-180">Az operációsrendszer-lemez hello konfigurációs beállítások hello osDisk elemmel van hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="298e9-180">Configuration settings for hello operating system disk are assigned with hello osDisk element.</span></span> <span data-ttu-id="298e9-181">hello példa határozza meg egy új felügyelt lemezes rendelkező módú készlet túl gyorsítótárazás hello**ReadWrite** és, hogy hello lemez létrehozása folyamatban van a egy [platformlemezképet](cli-ps-findimage.md):</span><span class="sxs-lookup"><span data-stu-id="298e9-181">hello example defines a new managed disk with hello caching mode set too**ReadWrite** and that hello disk is being created from a [platform image](cli-ps-findimage.md):</span></span>

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a><span data-ttu-id="298e9-182">Új virtuális gépek létrehozása a meglévő felügyelt lemezekből</span><span class="sxs-lookup"><span data-stu-id="298e9-182">Create new virtual machines from existing managed disks</span></span>

<span data-ttu-id="298e9-183">Ha azt szeretné, hogy a meglévő lemezek toocreate virtuális gépeket, hello imageReference és hello osProfile elemek eltávolítása és a következő lemez beállítások megadása:</span><span class="sxs-lookup"><span data-stu-id="298e9-183">If you want toocreate virtual machines from existing disks, remove hello imageReference and hello osProfile elements and define these disk settings:</span></span>

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a><span data-ttu-id="298e9-184">Hozzon létre új virtuális gépek egy felügyelt lemezképből</span><span class="sxs-lookup"><span data-stu-id="298e9-184">Create new virtual machines from a managed image</span></span>

<span data-ttu-id="298e9-185">Ha azt szeretné, hogy a virtuális gépek egy felügyelt képre toocreate, hello imageReference elem módosítása, és a következő lemez beállítások megadása:</span><span class="sxs-lookup"><span data-stu-id="298e9-185">If you want toocreate a virtual machine from a managed image, change hello imageReference element and define these disk settings:</span></span>

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

### <a name="attach-data-disks"></a><span data-ttu-id="298e9-186">Adatlemez csatolása</span><span class="sxs-lookup"><span data-stu-id="298e9-186">Attach data disks</span></span>

<span data-ttu-id="298e9-187">Opcionálisan hozzáadhat adatok lemezek toohello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="298e9-187">You can optionally add data disks toohello VMs.</span></span> <span data-ttu-id="298e9-188">Hello [lemezek száma](sizes.md) hello használt operációsrendszer-lemez méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="298e9-188">hello [number of disks](sizes.md) depends on hello size of operating system disk that you use.</span></span> <span data-ttu-id="298e9-189">A hello hello virtuális gépek méretét állítsa be a tooStandard_DS1_v2, hello fel őket a rendszer két toohello adatlemezek maximális számát.</span><span class="sxs-lookup"><span data-stu-id="298e9-189">With hello size of hello VMs set tooStandard_DS1_v2, hello maximum number of data disks that could be added toohello them is two.</span></span> <span data-ttu-id="298e9-190">Hello példában egy felügyelt adatlemez hozzáadott tooeach VM:</span><span class="sxs-lookup"><span data-stu-id="298e9-190">In hello example, one managed data disk is being added tooeach VM:</span></span>

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

## <a name="extensions"></a><span data-ttu-id="298e9-191">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="298e9-191">Extensions</span></span>

<span data-ttu-id="298e9-192">Bár a [bővítmények](extensions-features.md) külön erőforrás, olyan szorosan kapcsolt tooVMs.</span><span class="sxs-lookup"><span data-stu-id="298e9-192">Although [extensions](extensions-features.md) are a separate resource, they are closely tied tooVMs.</span></span> <span data-ttu-id="298e9-193">Bővítmények hello VM gyermek erőforrása, vagy egy külön erőforrás lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="298e9-193">Extensions can be added as a child resource of hello VM or as a separate resource.</span></span> <span data-ttu-id="298e9-194">hello példában hello [diagnosztika bővítmény](extensions-diagnostics-template.md) hozzáadni kívánt toohello virtuális gépek:</span><span class="sxs-lookup"><span data-stu-id="298e9-194">hello example shows hello [Diagnostics Extension](extensions-diagnostics-template.md) being added toohello VMs:</span></span>

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

<span data-ttu-id="298e9-195">A bővítmény erőforrás hello storageName változó és a hello diagnosztikai változók tooprovide értékeit használja.</span><span class="sxs-lookup"><span data-stu-id="298e9-195">This extension resource uses hello storageName variable and hello diagnostic variables tooprovide values.</span></span> <span data-ttu-id="298e9-196">Ha azt szeretné, hogy a bővítmény által gyűjtött toochange hello adatokhoz, hozzáadhat további teljesítmény számlálók toohello wadperfcounters változó.</span><span class="sxs-lookup"><span data-stu-id="298e9-196">If you want toochange hello data that is collected by this extension, you can add more performance counters toohello wadperfcounters variable.</span></span> <span data-ttu-id="298e9-197">Tooput hello diagnosztikai adatokat egy másik tárolóhelyre figyelembe mint hello Virtuálisgép-lemezek tárolására is kiválaszthatják.</span><span class="sxs-lookup"><span data-stu-id="298e9-197">You could also choose tooput hello diagnostics data into a different storage account than where hello VM disks are stored.</span></span>

<span data-ttu-id="298e9-198">Sok kiterjesztések, a virtuális gép telepíthető, de a leghasznosabb hello valószínűleg hello [egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md).</span><span class="sxs-lookup"><span data-stu-id="298e9-198">There are many extensions that you can install on a VM, but hello most useful is probably hello [Custom Script Extension](extensions-customscript.md).</span></span> <span data-ttu-id="298e9-199">Hello példában start.ps1 nevű PowerShell-parancsfájl futtatása az egyes virtuális gépek első indításakor:</span><span class="sxs-lookup"><span data-stu-id="298e9-199">In hello example, a PowerShell script named start.ps1 runs on each VM when it first starts:</span></span>

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

<span data-ttu-id="298e9-200">hello start.ps1 parancsfájl számos konfigurációs feladat érhető el.</span><span class="sxs-lookup"><span data-stu-id="298e9-200">hello start.ps1 script can accomplish many configuration tasks.</span></span> <span data-ttu-id="298e9-201">Például hello adatlemezek hozzáadott virtuális gépek toohello hello példában nincs inicializálva; egy egyéni parancsfájl tooinitialize is használhatja őket.</span><span class="sxs-lookup"><span data-stu-id="298e9-201">For example, hello data disks that are added toohello VMs in hello example are not initialized; you can use a custom script tooinitialize them.</span></span> <span data-ttu-id="298e9-202">Ha több indítási feladatok toodo, hello start.ps1 fájl toocall más PowerShell parancsfájlokat használhat az Azure-tárfiókba.</span><span class="sxs-lookup"><span data-stu-id="298e9-202">If you have multiple startup tasks toodo, you can use hello start.ps1 file toocall other PowerShell scripts in Azure storage.</span></span> <span data-ttu-id="298e9-203">hello példa PowerShell használ, de bármely Ön által használt hello operációs rendszeren elérhető parancsfájl-kezelési metódus használatával kérheti le.</span><span class="sxs-lookup"><span data-stu-id="298e9-203">hello example uses PowerShell, but you can use any scripting method that is available on hello operating system that you are using.</span></span>

<span data-ttu-id="298e9-204">Hello portál hello bővítmények beállítások extensions telepítve hello hello állapota látható:</span><span class="sxs-lookup"><span data-stu-id="298e9-204">You can see hello status of hello installed extensions from hello Extensions settings in hello portal:</span></span>

![Bővítmény állapotának beolvasása](./media/template-description/virtual-machines-show-extensions.png)

<span data-ttu-id="298e9-206">Is kaphat a sémakiterjesztési adatok hello segítségével **Get-AzureRmVMExtension** PowerShell parancsot, hello **virtuálisgép-bővítmény get** Azure CLI 2.0 parancs vagy hello **sémakiterjesztési adatok beolvasása**  REST API-t.</span><span class="sxs-lookup"><span data-stu-id="298e9-206">You can also get extension information by using hello **Get-AzureRmVMExtension** PowerShell command, hello **vm extension get** Azure CLI 2.0 command, or hello **Get extension information** REST API.</span></span>

## <a name="deployments"></a><span data-ttu-id="298e9-207">Központi telepítés</span><span class="sxs-lookup"><span data-stu-id="298e9-207">Deployments</span></span>

<span data-ttu-id="298e9-208">Egy sablon, egy csoportot, és automatikusan telepített Azure számok hello erőforrások telepítésekor hozzárendel egy név telepített toothis csoport.</span><span class="sxs-lookup"><span data-stu-id="298e9-208">When you deploy a template, Azure tracks hello resources that you deployed as a group and automatically assigns a name toothis deployed group.</span></span> <span data-ttu-id="298e9-209">hello telepítési hello neve van hello ugyanaz, mint a hello sablon hello nevét.</span><span class="sxs-lookup"><span data-stu-id="298e9-209">hello name of hello deployment is hello same as hello name of hello template.</span></span>

<span data-ttu-id="298e9-210">Ha fejezetét hello központi telepítésben lévő erőforrások hello állapotával kapcsolatos, hello Azure-portálon hello erőforráscsoport panel is használhatja:</span><span class="sxs-lookup"><span data-stu-id="298e9-210">If you are curious about hello status of resources in hello deployment, you can use hello Resource Group blade in hello Azure portal:</span></span>

![Telepítési információk](./media/template-description/virtual-machines-deployment-info.png)
    
<span data-ttu-id="298e9-212">Nincs a probléma toouse hello azonos toocreate sablonerőforrás vagy tooupdate meglévő erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="298e9-212">It’s not a problem toouse hello same template toocreate resources or tooupdate existing resources.</span></span> <span data-ttu-id="298e9-213">Parancsok toodeploy sablonok használatakor hello lehetőség toosay rendelkezik amely [mód](../../resource-group-template-deploy.md) toouse szeretné.</span><span class="sxs-lookup"><span data-stu-id="298e9-213">When you use commands toodeploy templates, you have hello opportunity toosay which [mode](../../resource-group-template-deploy.md) you want toouse.</span></span> <span data-ttu-id="298e9-214">hello mód beállítható tooeither **Complete** vagy **növekményes**.</span><span class="sxs-lookup"><span data-stu-id="298e9-214">hello mode can be set tooeither **Complete** or **Incremental**.</span></span> <span data-ttu-id="298e9-215">hello alapértelmezés szerint toodo növekményes frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="298e9-215">hello default is toodo incremental updates.</span></span> <span data-ttu-id="298e9-216">Hello használata esetén ügyeljen **Complete** módban, mert előfordulhat, hogy véletlenül törli az erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="298e9-216">Be careful when using hello **Complete** mode because you may accidentally delete resources.</span></span> <span data-ttu-id="298e9-217">Beállításakor hello mód túl**Complete**, erőforrás-kezelő törli, amelyek nincsenek hello sablonban hello erőforráscsoportban található erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="298e9-217">When you set hello mode too**Complete**, Resource Manager deletes any resources in hello resource group that are not in hello template.</span></span>

## <a name="next-steps"></a><span data-ttu-id="298e9-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="298e9-218">Next Steps</span></span>

- <span data-ttu-id="298e9-219">Hozzon létre egy saját sablon használatával [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="298e9-219">Create your own template using [Authoring Azure Resource Manager templates](../../resource-group-authoring-templates.md).</span></span>
- <span data-ttu-id="298e9-220">Létrehozott hello sablon üzembe helyezése [Windows virtuális gép létrehozása egy Resource Manager sablonnal](ps-template.md).</span><span class="sxs-lookup"><span data-stu-id="298e9-220">Deploy hello template that you created using [Create a Windows virtual machine with a Resource Manager template](ps-template.md).</span></span>
- <span data-ttu-id="298e9-221">Ismerje meg, hogyan toomanage hello alapján létrehozott virtuális gépek [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="298e9-221">Learn how toomanage hello VMs that you created by reviewing [Create and manage Windows VMs with hello Azure PowerShell module](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
