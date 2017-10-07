---
title: "Virtuálisgép-méretezési kapcsolatos aaaLearn sablonok beállítása |} Microsoft Docs"
description: "Ismerje meg a minimális életképes méretezési toocreate sablon virtuálisgép-méretezési csoportok beállítása"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/01/2017
ms.author: negat
ms.openlocfilehash: b7a1cf6c03b22585e16db9c071d45795c8ae75df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-virtual-machine-scale-set-templates"></a>További tudnivalók a virtuálisgép-méretezési készlet sablonok
[Az Azure Resource Manager-sablonok](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#template-deployment) vannak egy kiváló módja toodeploy kapcsolódó erőforrások csoportja. Az oktatóanyag adatsorozat bemutatja, hogyan toocreate egy minimális életképes méretezési sablon beállítása, és hogyan toomodify a sablon toosuit különböző lehetőségeket. Minden példában ez származhat [GitHub-tárházban](https://github.com/gatneil/mvss). 

Ez a sablon egyszerű tervezett toobe. Skála teljesebb példákat sablonok, olvassa el hello [Azure gyors üzembe helyezési sablonokat GitHub-tárházban](https://github.com/Azure/azure-quickstart-templates) és keresési mappák hello karakterláncot tartalmazó `vmss`.

Ha már ismeri a sablonok létrehozásával, kihagyhatja toohello "További lépések" szakasz toosee hogyan toomodify ezt a sablont.

## <a name="review-hello-template"></a>Felülvizsgálati hello sablonja

A GitHub használatára tooreview a minimális életképes méretezési sablon, [azuredeploy.json](https://raw.githubusercontent.com/gatneil/mvss/minimum-viable-scale-set/azuredeploy.json).

Az oktatóanyag azt vizsgálja meg hello diff (`git diff master minimum-viable-scale-set`) toocreate hello minimális életképes méretezési sablon adat által adat.

## <a name="define-schema-and-contentversion"></a>$Schema és contentVersion megadása
Első lépésként meghatároztuk `$schema` és `contentVersion` hello sablonban. Hello `$schema` elem hello sablon nyelvi hello verziója határozza meg, és a Visual Studio szintaxis kiemelését és hasonló érvényesítési szolgáltatások használható. Hello `contentVersion` elem nem használja az Azure-ban. Ehelyett segít nyomon követjük, hello verziójának.

```json
{
  "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json",
  "contentVersion": "1.0.0.0",
```
## <a name="define-parameters"></a>Paraméterek megadása
A következő két paraméter meghatároztuk `adminUsername` és `adminPassword`. A paraméterek olyan értékek adja meg, ha a központi telepítés hello időpontjában. Hello `adminUsername` paraméter egyszerűen egy `string` típusa, de mivel `adminPassword` van egy titkos kulcsot, amelyben tudatjuk a felhasználókkal az típus `securestring`. Később ezek a paraméterek vannak átadott hello méretezési készlet konfigurációja.

```json
  "parameters": {
    "adminUsername": {
      "type": "string"
    },
    "adminPassword": {
      "type": "securestring"
    }
  },
```
## <a name="define-variables"></a>Változók meghatározása
Resource Manager-sablonok segítségével definiálhatja a változók toobe hello sablon szerepel. A példa változó, nem használ, így a jelenleg üres már hello JSON-objektumból.

```json
  "variables": {},
```

## <a name="define-resources"></a>Adja meg az erőforrások
Ezután van hello sablon hello források szakaszában. Itt adhat meg ténylegesen mit toodeploy. Eltérően `parameters` és `variables` (amelyeket JSON-objektumok), `resources` JSON JSON-objektumok listáját.

```json
   "resources": [
```

Összes erőforrást igényelnek `type`, `name`, `apiVersion`, és `location` tulajdonságok. Ez a példa első erőforrás fájltípusú `Microsft.Network/virtualNetwork`nevű `myVnet`, és apiVersion `2016-03-30`. (toofind hello legújabb API-verzió erőforrástípus, lásd: hello [Azure REST API-dokumentáció](https://docs.microsoft.com/rest/api/).)

```json
     {
       "type": "Microsoft.Network/virtualNetworks",
       "name": "myVnet",
       "apiVersion": "2016-12-01",
```

## <a name="specify-location"></a>Adjon meg helyet
virtuális hálózati hello toospecify hello helyét, használjuk a [Resource Manager sablon függvény](../azure-resource-manager/resource-group-template-functions.md). Ez a funkció ajánlatok és a kapcsos zárójeleket kell tenni: `"[<template-function>]"`. Ebben az esetben használjuk hello `resourceGroup` függvény. Veszi a argumentum nélkül, és a központi telepítés telepítése hello erőforráscsoport kapcsolatos metaadatok egy JSON-objektumot ad vissza. hello erőforráscsoport hello időpontban a központi telepítés hello felhasználó van beállítva. Azt, majd a JSON-objektum az index `.location` tooget hello hely hello JSON-objektumból.

```json
       "location": "[resourceGroup().location]",
```

## <a name="specify-virtual-network-properties"></a>Adja meg a virtuális hálózati tulajdonságok
Minden erőforrás-kezelő erőforrás rendelkezik saját `properties` konfigurációk adott toohello erőforrás szakaszát. Ebben az esetben adtuk meg hello virtuális hálózatban kell hello privát IP-címtartományt egy alhálózatot `10.0.0.0/16`. A méretezési mindig szerepel egy alhálózaton belül. Alhálózatok nem foglal magába.

```json
       "properties": {
         "addressSpace": {
           "addressPrefixes": [
             "10.0.0.0/16"
           ]
         },
         "subnets": [
           {
             "name": "mySubnet",
             "properties": {
               "addressPrefix": "10.0.0.0/16"
             }
           }
         ]
       }
     },
```

## <a name="add-dependson-list"></a>Adja hozzá a dependsOn listája
Ezenkívül toohello szükséges `type`, `name`, `apiVersion`, és `location` tulajdonságok, az egyes erőforrások lehetnek egy nem kötelező `dependsOn` karakterláncok listáját. Ez a lista megadja, hogy más erőforrások, a központi telepítésből Befejezés ehhez az erőforráshoz telepítése előtt.

Ebben az esetben van csak egy elem hello listában hello hello előző példa virtuális hálózaton. A függőség adtuk meg, mert hello méretezési igények hello hálózati tooexist bármely virtuális gépek létrehozása előtt. Ezzel a módszerrel hello méretezési hello IP-címtartomány hello hálózati tulajdonságainak korábban már megadta a biztosíthat a virtuális gépek magánhálózati IP-címét. hello hello dependsOn listán minden karakterlánc formátuma `<type>/<name>`. Használja az azonos hello `type` és `name` korábban használt hello virtuális hálózati erőforrás-definícióban.

```json
     {
       "type": "Microsoft.Compute/virtualMachineScaleSets",
       "name": "myScaleSet",
       "apiVersion": "2016-04-30-preview",
       "location": "[resourceGroup().location]",
       "dependsOn": [
         "Microsoft.Network/virtualNetworks/myVnet"
       ],
```
## <a name="specify-scale-set-properties"></a>Adja meg a skála tulajdonságainak beállítása
Méretezési csoportok hello virtuális gépek hello méretezési csoportban lévő testreszabásához sok tulajdonságokkal rendelkezik. Ezek a tulajdonságok teljes listáját lásd: hello [méretezési REST API-dokumentáció](https://docs.microsoft.com/en-us/rest/api/virtualmachinescalesets/create-or-update-a-set). Ebben az oktatóanyagban helyezünk csak néhány gyakran használt tulajdonságokat.
### <a name="supply-vm-size-and-capacity"></a>Adja meg a Virtuálisgép-méretet és a kapacitás
hello méretezési igények tooknow milyen mértékű VM toocreate ("termékváltozat"), és hány ilyen virtuális gépek toocreate ("termékváltozat-kapacitás"). toosee mely Virtuálisgép-méretek érhetők el, lásd: hello [Virtuálisgép-méretek dokumentáció](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-sizes).

```json
       "sku": {
         "name": "Standard_A1",
         "capacity": 2
       },
```

### <a name="choose-type-of-updates"></a>Válassza ki a kívánt frissítések típusát
hello méretezési kell tooknow toohandle frissítéséről hello méretezési készlet. Jelenleg nincsenek két lehetőség `Manual` és `Automatic`. Hello különbségei hello két további információkért lásd: hello dokumentáció a [hogyan tooupgrade egy méretezési](./virtual-machine-scale-sets-upgrade-scale-set.md).

```json
       "properties": {
         "upgradePolicy": {
           "mode": "Manual"
         },
```

### <a name="choose-vm-operating-system"></a>Válassza ki a virtuális gép operációs rendszer
hello méretezési igények tooknow milyen operációs rendszer tooput hello virtuális gépeken. Itt létrehozhatunk hello virtuális gépek egy teljes mértékben javított Ubuntu 16.04-es lts verzió lemezképpel.

```json
         "virtualMachineProfile": {
           "storageProfile": {
             "imageReference": {
               "publisher": "Canonical",
               "offer": "UbuntuServer",
               "sku": "16.04-LTS",
               "version": "latest"
             }
           },
```

### <a name="specify-computernameprefix"></a>Adja meg a gépnév előtagja
hello méretezési több virtuális gép telepítését. Minden virtuális gép nevének megadása helyett adtuk meg `computerNamePrefix`. hello méretezési hozzáfűz egy index toohello előtagot az egyes virtuális gépek, virtuális gépek nevénél jogosult hello űrlap `<computerNamePrefix>_<auto-generated-index>`.

A következő kódrészletet hello hello tooset hello rendszergazdai jogosultságú felhasználónevet és jelszót előtt származó paraméterek használjuk hello méretezési csoportban lévő virtuális gép esetében. A Microsoft ehhez a hello `parameters` sablonfüggvény. Ez a függvény egy karakterlánc, amely meghatározza, melyik paraméter toorefer tooand kiírja, hogy a paraméter értékét hello időt vesz igénybe.

```json
           "osProfile": {
             "computerNamePrefix": "vm",
             "adminUsername": "[parameters('adminUsername')]",
             "adminPassword": "[parameters('adminPassword')]"
           },
```

### <a name="specify-vm-network-configuration"></a>Adja meg a Virtuálisgép-hálózati konfiguráció
Végül igazolnia kell toospecify hello hálózati konfiguráció hello méretezési csoportban lévő hello virtuális gépekhez. Ebben az esetben csak kell korábban létrehozott hello alhálózati toospecify hello azonosítója. Ez alapján hello méretezési tooput hello hálózati illesztők az alhálózaton.

Hello Azonosítóhoz hello virtuális hálózat hello alhálózati tartalmazó hello használatával juthat `resourceId` sablonfüggvény. Ez a függvény argumentuma hello típus és az erőforrás neve, és vissza hello teljesen minősített, hogy az erőforrás-azonosítója. Ezt az Azonosítót hello űrlap rendelkezik:`/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/<resourceProviderNamespace>/<resourceType>/<resourceName>`

Azonban hello hello virtuális hálózat azonosító nem elegendő. Meg kell adnia, hogy a virtuális gépek kell a méretezési hello hello alhálózatot. toodo, összefűzésére `/subnets/mySubnet` toohello hello virtuális hálózat Azonosítójává. hello eredménye hello alhálózati teljesen minősített hello azonosítója. Hajtsa végre a kapott a hello `concat` funkció, amely a karakterláncok több időt vesz igénybe, és visszaadja a kapott.

```json
           "networkProfile": {
             "networkInterfaceConfigurations": [
               {
                 "name": "myNic",
                 "properties": {
                   "primary": "true",
                   "ipConfigurations": [
                     {
                       "name": "myIpConfig",
                       "properties": {
                         "subnet": {
                           "id": "[concat(resourceId('Microsoft.Network/virtualNetworks', 'myVnet'), '/subnets/mySubnet')]"
                         }
                       }
                     }
                   ]
                 }
               }
             ]
           }
         }
       }
     }
   ]
}

```

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [mvss-next-steps-include](../../includes/mvss-next-steps.md)]
