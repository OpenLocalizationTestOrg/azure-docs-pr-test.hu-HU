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
# <a name="virtual-machines-in-an-azure-resource-manager-template"></a>Az Azure Resource Manager-sablonokban a virtuális gépek

Ez a cikk ismerteti az Azure Resource Manager-sablonok, amelyek érvényesek a toovirtual gépek aspektusait. Ez a cikk nem alkalmazható a teljes sablont hoz létre virtuális gépet; az adott erőforrás-definíciókban storage-fiókok, a hálózati adapterek, a nyilvános IP-címek és a virtuális hálózatok kell. Hogyan ezekkel az erőforrásokkal együtt definiálható kapcsolatos további információkért lásd: hello [Resource Manager sablonokhoz](../../azure-resource-manager/resource-manager-template-walkthrough.md).

Nincsenek a sok [hello tárban lévő sablonok](https://azure.microsoft.com/documentation/templates/?term=VM) , amelyek tartalmazzák a hello virtuális gép erőforrásához. Nem minden sablonként szereplő összetevőit itt.

Ez a példa bemutatja a jellemző erőforrás szakaszában megadott számú virtuális gépek létrehozására szolgáló sablont:

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
>Ez a példa egy korábban létrehozott tárfiókot támaszkodik. Létrehozhat hello tárfiók hello sablonból telepítésével. hello példa is egy hálózati adapter és a tőle függő erőforrások, akkor lehet hello sablonban definiált támaszkodik. Hello példa nem mutatja be ezeket az erőforrásokat.
>
>

## <a name="api-version"></a>API-verzió

Amikor telepít egy sablon használatával erőforrások, toospecify egy verziójával rendelkezik hello API toouse. a példában hello hello virtuálisgép-erőforrás a apiVersion elem használatával:

```
"apiVersion": "2016-04-30-preview",
```

hello verzióját adja meg, ha a sablonban API hello megadására a hello sablon tulajdonságok hatással van. Általánosságban elmondható választhat hello legújabb API verziót sablonok létrehozásakor. A meglévő sablonok eldöntheti, hogy szeretné, hogy egy korábbi verziójával API toocontinue, vagy a hello legújabb verziója tootake új szolgáltatásainak előnyeit a sablon frissítésére.

Ezek a lehetőségek lekérhesse a legújabb API-verziók hello használata:

- REST API - [összes erőforrás-szolgáltatók felsorolása](https://docs.microsoft.com/rest/api/resources/providers#Providers_List)
- PowerShell - [Get-AzureRmResourceProvider](/powershell/module/azurerm.resources/get-azurermresourceprovider)
- Az Azure CLI 2.0 - [az szolgáltató megjelenítése](https://docs.microsoft.com/cli/azure/provider#show)

## <a name="parameters-and-variables"></a>Paraméterek és változók

[Paraméterek](../../resource-group-authoring-templates.md) könnyítheti meg toospecify értékek hello sablon futtatásakor. A Paraméterek szakaszban hello példa szerepel:

```        
"parameters": {
  "adminUsername": { "type": "string" },
  "adminPassword": { "type": "securestring" },
  "numberOfInstances": { "type": "int" }
},
```

Hello példa sablon telepítésekor adhatja értékek hello nevét és hello rendszergazdai fiók jelszavát minden egyes virtuális gépek toocreate a virtuális gép és hello számára. Lehetősége van hello paraméterértékek meghatározásáról hello sablonnal felügyelt külön fájlban, vagy biztosító értékek, amikor a rendszer kéri.

[Változók](../../resource-group-authoring-templates.md) könnyítheti meg tooset értékeket hello sablonban használt ismételten egész, vagy idővel megváltozhat. A változók szakaszban hello példa szerepel:

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

Hello példa sablon telepítésekor változók értékeinek hello neve és azonosítója hello korábban létrehozott tárfiókot használ. A változókat is használt tooprovide hello beállítások hello diagnosztikai bővítmény. Használjon hello [ajánlott eljárások Azure Resource Manager-sablonok létrehozásához](../../resource-manager-template-best-practices.md) toohelp úgy dönt, hogy hogyan toostructure hello paramétereket és változókat a sablonban.

## <a name="resource-loops"></a>Erőforrás hurkok

Ha egynél több virtuális gép van szükség az alkalmazás, egy sablon egy másolás elem is használhatja. A választható elem végighalad a virtuális gépek, paraméterként megadott hello száma létrehozása:

```
"copy": {
  "name": "virtualMachineLoop", 
  "count": "[parameters('numberOfInstances')]"
},
```

Figyelje meg, amely ciklusindex hello hello példában használt is, hello erőforrás értékek egyes hello megadása esetén. Például ha példányszámának hello operációs rendszer lemezek három, hello nevek a megadott is myOSDisk1 myOSDisk2 és myOSDisk3:

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
}
```

> [!NOTE] 
>A példa felügyelt lemezek hello virtuális gépekhez.
>
>

Ne feledje, hogy egy erőforrás hurkot létre a hello sablon előfordulhat, hogy Ön toouse hello hurok létrehozásakor, vagy más erőforrások eléréséhez. Például a több virtuális gép nem használható hello azonos hálózati adapter, így ha a sablon végighalad végig vezeti a kell is hurok három virtuális gépek létrehozása három hálózati illesztőt. A hálózati illesztő tooa VM hozzárendelésekor hello ciklusindex-e a használt tooidentify azt:

```
"networkInterfaces": [ { 
  "id": "[resourceId('Microsoft.Network/networkInterfaces',
    concat('myNIC', copyindex()))]" 
} ]
```

## <a name="dependencies"></a>Függőségek

A legtöbb erőforrások megfelelően egyéb erőforrások toowork függenek. Virtuális gépek virtuális hálózati és, hogy kell-e egy adott hálózati csatoló toodo társítva kell lennie. Hello [dependsOn](../../resource-group-define-dependencies.md) elem használt toomake meg arról, hogy hello hálózati illesztőt készen toobe használt hello virtuális gépek létrehozása előtt:

```
"dependsOn": [
  "[concat('Microsoft.Network/networkInterfaces/', 'myNIC', copyindex())]" 
],
```

Erőforrás-kezelő párhuzamosan telepíti, amelyek nem függenek más erőforrás telepített erőforrásokat. Ügyeljen arra, hogy függőségek beállításakor, mert a szükségtelen függőségek megadásával véletlenül lelassíthatja a központi telepítés. Függőségek is láncolt több forrásanyagok segítségével. Például hello hálózati illesztő hello nyilvános IP-címhez és virtuális hálózati erőforrások függ.

Hogyan tudja, hogy szükség-e egy függőséget? Tekintse meg hello értékeket hello sablont. Ha egy elem hello virtuális gépek erőforrás-definícióban hello üzembe helyezett erőforrás tooanother ugyanazt a sablont, függőség van szüksége. A Példa virtuális gép például egy hálózati profil határozza meg:

```
"networkProfile": { 
  "networkInterfaces": [ { 
    "id": "[resourceId('Microsoft.Network/networkInterfaces',
      concat('myNIC', copyindex())]" 
  } ] 
},
```

tooset ezt a tulajdonságot, hello hálózati illesztő léteznie kell. Ezért függőség van szüksége. Amikor egy erőforrást (gyermek) van meghatározva egy másik erőforrás (szülő) is kell tooset függőséget. Például hello diagnosztikai beállítások és egyéni parancsfájl-kiterjesztés is definiálhatók, gyermekszintű erőforrása hello virtuális gép. Ezek nem hozható létre, amíg hello virtuális gép is létezik. Ezért mindkét erőforrás van megjelölve függő hello virtuális gépen.

## <a name="profiles"></a>Profilok

Több profil elemek használt virtuálisgép-erőforrás definiálásakor. Néhány szükséges és választható. Például hello hardwareProfile, osProfile, storageProfile és networkProfile elemek szükségesek, de hello diagnosticsProfile nem kötelező megadni. Ezeket a profilokat, mint beállításainak megadása:
   
- [méret](sizes.md)
- [név](/architecture/best-practices/naming-conventions) és hitelesítő adatait.
- lemez és [operációsrendszer-beállításokat](cli-ps-findimage.md)
- [hálózati adapter](../../virtual-network/virtual-networks-multiple-nics.md) 
- Rendszerindítási diagnosztika

## <a name="disks-and-images"></a>A lemezek és lemezképek
   
Az Azure, a vhd-fájlok jelenthet [lemezek vagy képeket](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Amikor hello operációs rendszer egy vhd-fájlt a speciális toobe speciális virtuális Gépet, hivatkozott tooas lemez. Ha a vhd-fájl hello operációs rendszer általánosított toobe használt toocreate sok virtuális gép, hivatkozott tooas lemezkép.   
    
### <a name="create-new-virtual-machines-and-new-disks-from-a-platform-image"></a>Hozzon létre új virtuális gépek és az új lemezt, a platformlemezkép

Amikor létrehoz egy virtuális Gépet, határozza meg, milyen operációs rendszer toouse. hello imageReference elem használt toodefine hello operációs rendszer egy új virtuális gép állapota. hello példa bemutatja, a következő definícióját: a Windows Server operációs rendszert:

```
"imageReference": { 
  "publisher": "MicrosoftWindowsServer", 
  "offer": "WindowsServer", 
  "sku": "2012-R2-Datacenter", 
  "version": "latest" 
},
```

Ha azt szeretné, hogy a Linux operációs rendszer toocreate, ez a definíció használhatja:

```
"imageReference": {
  "publisher": "Canonical",
  "offer": "UbuntuServer",
  "sku": "14.04.2-LTS",
  "version": "latest"
},
```

Az operációsrendszer-lemez hello konfigurációs beállítások hello osDisk elemmel van hozzárendelve. hello példa határozza meg egy új felügyelt lemezes rendelkező módú készlet túl gyorsítótárazás hello**ReadWrite** és, hogy hello lemez létrehozása folyamatban van a egy [platformlemezképet](cli-ps-findimage.md):

```
"osDisk": { 
  "name": "[concat('myOSDisk', copyindex())]",
  "caching": "ReadWrite", 
  "createOption": "FromImage" 
},
```

### <a name="create-new-virtual-machines-from-existing-managed-disks"></a>Új virtuális gépek létrehozása a meglévő felügyelt lemezekből

Ha azt szeretné, hogy a meglévő lemezek toocreate virtuális gépeket, hello imageReference és hello osProfile elemek eltávolítása és a következő lemez beállítások megadása:

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

### <a name="create-new-virtual-machines-from-a-managed-image"></a>Hozzon létre új virtuális gépek egy felügyelt lemezképből

Ha azt szeretné, hogy a virtuális gépek egy felügyelt képre toocreate, hello imageReference elem módosítása, és a következő lemez beállítások megadása:

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

### <a name="attach-data-disks"></a>Adatlemez csatolása

Opcionálisan hozzáadhat adatok lemezek toohello virtuális gépeket. Hello [lemezek száma](sizes.md) hello használt operációsrendszer-lemez méretétől függ. A hello hello virtuális gépek méretét állítsa be a tooStandard_DS1_v2, hello fel őket a rendszer két toohello adatlemezek maximális számát. Hello példában egy felügyelt adatlemez hozzáadott tooeach VM:

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

## <a name="extensions"></a>Bővítmények

Bár a [bővítmények](extensions-features.md) külön erőforrás, olyan szorosan kapcsolt tooVMs. Bővítmények hello VM gyermek erőforrása, vagy egy külön erőforrás lehet hozzáadni. hello példában hello [diagnosztika bővítmény](extensions-diagnostics-template.md) hozzáadni kívánt toohello virtuális gépek:

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

A bővítmény erőforrás hello storageName változó és a hello diagnosztikai változók tooprovide értékeit használja. Ha azt szeretné, hogy a bővítmény által gyűjtött toochange hello adatokhoz, hozzáadhat további teljesítmény számlálók toohello wadperfcounters változó. Tooput hello diagnosztikai adatokat egy másik tárolóhelyre figyelembe mint hello Virtuálisgép-lemezek tárolására is kiválaszthatják.

Sok kiterjesztések, a virtuális gép telepíthető, de a leghasznosabb hello valószínűleg hello [egyéni parancsprogramok futtatására szolgáló bővítmény](extensions-customscript.md). Hello példában start.ps1 nevű PowerShell-parancsfájl futtatása az egyes virtuális gépek első indításakor:

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

hello start.ps1 parancsfájl számos konfigurációs feladat érhető el. Például hello adatlemezek hozzáadott virtuális gépek toohello hello példában nincs inicializálva; egy egyéni parancsfájl tooinitialize is használhatja őket. Ha több indítási feladatok toodo, hello start.ps1 fájl toocall más PowerShell parancsfájlokat használhat az Azure-tárfiókba. hello példa PowerShell használ, de bármely Ön által használt hello operációs rendszeren elérhető parancsfájl-kezelési metódus használatával kérheti le.

Hello portál hello bővítmények beállítások extensions telepítve hello hello állapota látható:

![Bővítmény állapotának beolvasása](./media/template-description/virtual-machines-show-extensions.png)

Is kaphat a sémakiterjesztési adatok hello segítségével **Get-AzureRmVMExtension** PowerShell parancsot, hello **virtuálisgép-bővítmény get** Azure CLI 2.0 parancs vagy hello **sémakiterjesztési adatok beolvasása**  REST API-t.

## <a name="deployments"></a>Központi telepítés

Egy sablon, egy csoportot, és automatikusan telepített Azure számok hello erőforrások telepítésekor hozzárendel egy név telepített toothis csoport. hello telepítési hello neve van hello ugyanaz, mint a hello sablon hello nevét.

Ha fejezetét hello központi telepítésben lévő erőforrások hello állapotával kapcsolatos, hello Azure-portálon hello erőforráscsoport panel is használhatja:

![Telepítési információk](./media/template-description/virtual-machines-deployment-info.png)
    
Nincs a probléma toouse hello azonos toocreate sablonerőforrás vagy tooupdate meglévő erőforrásokat. Parancsok toodeploy sablonok használatakor hello lehetőség toosay rendelkezik amely [mód](../../resource-group-template-deploy.md) toouse szeretné. hello mód beállítható tooeither **Complete** vagy **növekményes**. hello alapértelmezés szerint toodo növekményes frissítéseket. Hello használata esetén ügyeljen **Complete** módban, mert előfordulhat, hogy véletlenül törli az erőforrásokat. Beállításakor hello mód túl**Complete**, erőforrás-kezelő törli, amelyek nincsenek hello sablonban hello erőforráscsoportban található erőforrásokat.

## <a name="next-steps"></a>Következő lépések

- Hozzon létre egy saját sablon használatával [Azure Resource Manager-sablonok készítése](../../resource-group-authoring-templates.md).
- Létrehozott hello sablon üzembe helyezése [Windows virtuális gép létrehozása egy Resource Manager sablonnal](ps-template.md).
- Ismerje meg, hogyan toomanage hello alapján létrehozott virtuális gépek [létrehozása és kezelése Windows virtuális gépek hello Azure PowerShell modul](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
