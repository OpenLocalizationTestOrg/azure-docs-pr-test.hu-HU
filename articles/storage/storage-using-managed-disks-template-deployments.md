---
title: "aaaUsing által kezelt lemezeken Azure Resource Manager sablonokban |} Microsoft Docs"
description: "Hogyan toouse kezelése az Azure Resource Manager sablonokban misks részletek"
services: storage
documentationcenter: 
author: jboeshart
manager: 
ms.service: storage
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage
ms.date: 06/01/2017
ms.author: jaboes
ms.openlocfilehash: ea83f4ed11acfd8f642dbc8331fa8cf077ef577c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Az Azure Resource Manager-sablonok lemezek felügyelt

Ez a dokumentum végigvezeti a kezelt és nem kezelt lemezeken Azure Resource Manager sablonok tooprovision virtuális gépek használatakor hello különbségei. Ez segítséget nyújt tooupdate, meglévő sablonok nem felügyelt lemezek toomanaged lemezt használ. Referenciaként használunk hello [101-vm-egyszerű-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) útmutatóként sablont. Láthatja, hogy mindkét hello sablon [által kezelt lemezeken](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) egy korábbi verzióját használja, és [lemezek nem felügyelt](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) Ha azt szeretné toodirectly hasonlítsa össze azokat.

## <a name="unmanaged-disks-template-formatting"></a>Nem felügyelt lemezek sablon formázás

toobegin, hogy tekintse meg, hogyan nem felügyelt lemezek vannak telepítve. Nem felügyelt lemezek létrehozásakor a tárolási fiók toohold hello VHD-fájlokat kell. Hozzon létre egy új tárfiókot, vagy használjon már meglévő. Ez a cikk bemutatja, hogyan toocreate egy új tárfiókot. tooaccomplish, a tárolási fiók erőforrás hello erőforrások blokkban kell alább látható módon.

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

Hello virtuális gép objektumban kell hello tárolási fiók tooensure hello virtual machine létrehozva függ. Hello belül `storageProfile` szakaszban, majd meg hello hello VHD helyre, amely hello tárfiók hivatkozik, és az operációs rendszer hello lemez és bármely adatlemezek szükséges teljes URI-címe. 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a>Felügyelt lemezek sablon formázás

A Azure felügyelt lemezek esetében a hello lemez egy legfelső szintű erőforrás lesz, és többé nem kell a tárolási fiók toobe hello felhasználó által létrehozott. Felügyelt lemezek először volt felfedett hello `2016-04-30-preview` API-verzió, akkor az összes azt követő API-verziók és elérhetőek most hello alapértelmezett lemeztípus. hello alábbiakban hello alapértelmezett beállításait ismerteti és részletesen, hogyan toofurther testre a lemezek.

> [!NOTE]
> Ajánlott toouse API verziója újabb, mint `2016-04-30-preview` mert jelentős változásokat közötti `2016-04-30-preview` és `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Alapértelmezett felügyelt lemezes beállításai

toocreate felügyelt lemezeket, hogy már nem rendelkező virtuális toocreate hello tárolási fiók erőforrás kell, és a következőképpen frissítheti a virtuálisgép-erőforrás. Kifejezetten vegye figyelembe, hogy hello `apiVersion` tükrözi `2017-03-30` és hello `osDisk` és `dataDisks` már nem hivatkozik az tooa hello virtuális merevlemez megadott URI-Azonosítóját. Ha telepítése További tulajdonságok megadása nélkül, használja a hello lemez [Standard-LRS tárolási](storage-redundancy.md). Ha nem ad meg nevet, tart hello formátuma `<VMName>_OsDisk_1_<randomstring>` hello operációsrendszer-lemez és `<VMName>_disk<#>_<randomstring>` adatok lemezek. Alapértelmezés szerint az Azure disk encryption le van tiltva; Olvasási/írási gyorsítótárazást az operációs rendszer hello lemez és adatlemezek nincs. Az alábbi példa hello Észreveheti, továbbra is fennáll a tárolási fiók függőségi, bár ez csak a tárolási diagnosztikai és a lemezes tárolás esetén nem szükséges.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a>Egy legfelső szintű felügyelt lemezes erőforrást használja

Egy alternatív toospecifying hello lemezkonfigurációt hello virtuálisgép-objektumot, mint legfelső szintű lemez erőforrás létrehozása, és mellékelje hello virtuális gép létrehozásának részeként. Például azt a lemezerőforrás toouse adatlemez, az alábbi módon hozhat létre.

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

Belül hello Virtuálisgép-objektum a csatlakoztatott lemez objektum toobe majd azt is hivatkozik. A hello létrehozott lemez hello hello erőforrás-Azonosítóját megadó felügyelt `managedDisk` tulajdonság lehetővé teszi, hogy a hello melléklet hello lemez, a virtuális gép kerül létrehozásra hello. Vegye figyelembe, hogy hello `apiVersion` a virtuális gép erőforrásához hello értéke túl`2017-03-30`. Ne feledje, hogy létrehoztunk Önnek egy függőséget meg hello lemez erőforrás tooensure sikeres létrehozása előtt a virtuális gép létrehozása. 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Felügyelt rendelkezésre állási csoportok felügyelt lemezek használata virtuális gépek létrehozása

toocreate felügyelt rendelkezésre állási készletek kezelt lemezeken, virtuális gépek hozzáadása hello `sku` objektum toohello rendelkezésre állási erőforrás és hello `name` tulajdonság túl`Aligned`. Ez biztosítja, hogy az egyes virtuális gépek hello lemezek elég elkülönül egymástól tooavoid egyetlen ponton felmerülő hibákat. Ne feledje, hogy hello `apiVersion` hello rendelkezésre állási csoport erőforrás van állítani túl`2017-03-30`.

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a>További forgatókönyvek és testreszabása

teljes körű információt toofind hello REST API specifikációk, tekintse át a hello [felügyelt lemezes REST API-dokumentáció](/rest/api/manageddisks/disks/disks-create-or-update). További helyzeteket is, valamint az alapértelmezett és az elfogadható értékek, amelyek lehetnek sablon központi telepítések keresztül elküldött toohello API találja. 

## <a name="next-steps"></a>Következő lépések

* Teljes kezelt lemezek használó sablonokat Microsoft hello Azure gyors üzembe helyezés tárház hivatkozásokat követve.
    * [A felügyelt lemezes Windows VM](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [A felügyelt lemezes Linux virtuális gép](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Felügyelt lemezes sablonok teljes listája](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* A Microsoft hello [Azure felügyelt lemezekhez – áttekintés](storage-managed-disks-overview.md) további információt a dokumentum toolearn által kezelt lemezeken.
* Tekintse át a virtuális gép erőforrásai hello sablon referenciadokumentációt tartalmaz hello felkeresésével [Microsoft.Compute/virtualMachines sablonra való hivatkozást](/templates/microsoft.compute/virtualmachines) dokumentum.
* Tekintse át a hello sablon referenciadokumentációt tartalmaz lemezerőforrásokat hello felkeresésével [Microsoft.Compute/disks sablonra való hivatkozást](/templates/microsoft.compute/disks) dokumentum.
 
