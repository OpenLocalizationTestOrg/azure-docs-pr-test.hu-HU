---
title: "aaaAvailability és az Azure Resource Manager sablonokban méretezési |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8fcfea79-f017-4658-8c51-74242fcfb7f6
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 6f830ca0a64e6b65859312bdf31dc0af59e2b978
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-linux-vms"></a>Rendelkezésre állásának és méretezésének a Linux virtuális gépek Azure Resource Manager-sablonok

Rendelkezésre állás és méretezhetőség tekintse meg a toouptime és hello képességét toomeet igény szerint. Egy alkalmazás hello idő 99,9 %-át kell lennie, hogy szükséges-e toohave egy architektúra, amely lehetővé teszi, hogy több egyidejű számítási erőforrásokat. Ahelyett, hogy egy webhely, például egy rendelkezésre állási magasabb szintű konfiguráció azonos hely, a terheléselosztás előtti technológia hello több példányát tartalmazza. Ebben a konfigurációban hello alkalmazás egy példánya le lehessen állítani karbantartásra, fennmaradó hello leállítaná toofunction. Skála a hello ugyanakkor tooan alkalmazások képességét tooserve igény szerint hivatkozik. Terhelésű kiegyensúlyozott alkalmazás hozzáadása vagy eltávolítása a példányok hello készlet lehetővé teszi, hogy egy alkalmazás tooscale toomeet igény szerint.

Ez a dokumentum részletesen, hogyan zene tároló üzembe helyezési minta hello beállítása rendelkezésre állása és méretezése. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux).

## <a name="availability-set"></a>Rendelkezésre állási csoport
Rendelkezésre állási csoport logikailag kiterjedésű Azure virtuális gépek fizikai állomások és egyéb infrastrukturális összetevők, például áramforrások és a fizikai hálózati eszközt. Rendelkezésre állási készletek győződjön meg arról, hogy közben karbantartási, az eszköz nem vagy más állásidő, nem az összes virtuális gép történik. Rendelkezésre állási csoport is hozzáadhatók a tooan Azure Resource Manager sablon hello Visual Studio új erőforrás hozzáadása varázsló használatával, vagy érvényes JSON beszúrása egy sablont.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [rendelkezésre állási csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L387).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "avalibility-set"
  },
  "properties": {
    "platformUpdateDomainCount": 5,
    "platformFaultDomainCount": 3
  }
}
```

Rendelkezésre állási csoport egy virtuális gép erőforrásának tulajdonságainál deklarálva van. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [rendelkezésre állási csoport hozzárendelését a virtuális gép](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L313).

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
hello rendelkezésre állási csoportban, amint az Azure-portálon hello látható. Minden virtuális gép és a hello konfigurációs részleteit itt részletes.

![Rendelkezésre állási csoport](./media/dotnet-core-4-availability-scale/aset.png)

A rendelkezésre állási készletek részletes információkért lásd: [virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

## <a name="network-load-balancer"></a>Hálózati terheléselosztó
Mivel a rendelkezésre állási csoportok alkalmazás hibatűrést biztosít, a terheléselosztó elérhetővé hello alkalmazás hány példánya egyetlen hálózati címhez. Egy alkalmazás több példánya lehet üzemeltetni a több virtuális gép, mindegyik kapcsolódó tooa terheléselosztó. Hello alkalmazás érhető el, mert a hello load balancer útvonalak bejövő kérelem hello csatolt hello tagok között. A terheléselosztó hello Visual Studio új erőforrás hozzáadása varázsló használatával adhatók, vagy beszúrva megfelelően formázott JSON-erőforrás hello Azure Resource Manager sablonba.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati terheléselosztó](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-front"
  },
  ........<truncated>
}
```

Mivel hello mintaalkalmazás kitett toohello internet nyilvános IP-címek, ez a cím tartozik hello terheléselosztót. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati terheléselosztó hozzárendelését a nyilvános IP-cím](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L221).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicipaddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

Hello Azure-portálon, a hello hálózati terheléselosztó áttekintése látható hello hozzárendelését a hello nyilvános IP-cím.

![Hálózati terheléselosztó](./media/dotnet-core-4-availability-scale/nlb.png)

## <a name="load-balancer-rule"></a>Terheléselosztási szabály
Ha terheléselosztót használ, a szabályok konfigurálva vannak, hogyan forgalom szánt hello erőforrások között elosztott terhelésű szabályozó. Zeneáruház hello mintaalkalmazást a forgalom érkezik a hello nyilvános IP-cím 80-as porton, és van elosztva a 80-as port az összes virtuális gépet. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [terheléselosztási szabály](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270).

```json
"loadBalancingRules": [
  {
    "name": "[variables('loadBalencerRule')]",
    "properties": {
      "frontendIPConfiguration": {
        "id": "[concat(resourceId('Microsoft.Network/loadBalancers', variables('loadBalancerName')), '/frontendIPConfigurations/LoadBalancerFrontend')]"
      },
      "backendAddressPool": {
        "id": "[variables('lbPoolID')]"
      },
      "protocol": "Tcp",
      "frontendPort": 80,
      "backendPort": 80,
      "enableFloatingIP": false,
      "idleTimeoutInMinutes": 5,
      "probe": {
        "id": "[variables('lbProbeID')]"
      }
    }
  }
]
```

Hello hálózatot terheléselosztó szabályhoz hello portálról betölteni.

![Hálózati terheléselosztási szabály](./media/dotnet-core-4-availability-scale/lbrule.png)

## <a name="load-balancer-probe"></a>Terheléselosztói mintavétel
hello terheléselosztónak is kell toomonitor virtuális gépeken, hogy a kérelmek szolgáltatott csak toorunning rendszerek. Ez a figyelő akkor történik meg által állandó probing egy előre definiált port. hello zeneáruház telepítési konfigurált tooprobe 80-as port az összes szereplő virtuális gépek. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Load Balancer mintavételi](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L257).

```json
"probes": [
  {
    "properties": {
      "protocol": "Tcp",
      "port": 80,
      "intervalInSeconds": 15,
      "numberOfProbes": 2
    },
    "name": "lbprobe"
  }
]
```

hello terheléselosztói mintavétel hello Azure-portálon látható.

![Hálózati Terheléselosztói mintavétel](./media/dotnet-core-4-availability-scale/lbprobe.png)

## <a name="inbound-nat-rules"></a>Bejövő NAT-szabályok
A terheléselosztó használata esetén a szabályok toobe kell bevezetni által a virtuális gép nem elosztott terhelésű hozzáférés tooeach. Például az SSH-kapcsolat az egyes virtuális gépek létrehozásakor a forgalmat nem lehet elosztott terhelésű, ahelyett, hogy egy előre meghatározott elérési utat kell megadni. előre meghatározott elérési útjának konfigurálása bejövő forgalmat kezelő NAT-szabály erőforrást használ. Ezt az erőforrást használ, a bejövő kommunikáció lehet csatlakoztatott tooindividual virtuális gépek. 

Hello zene áruház-alkalmazás legyen az port, 5000 kezdődő csatlakoztatott tooport 22 minden egyes virtuális gépen az SSH-elérést. Hello `copyindex()` függvény használt tooincrement hello a bejövő portot, úgy, hogy a második virtuális gép hello fogad egy bejövő portot 5001, harmadik 5002 hello és így tovább. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [bejövő NAT-szabályok](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L270). 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'SSH-VM', copyIndex())]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "lbNatLoop",
    "count": "[parameters('numberOfInstances')]"
  },
  "dependsOn": [
    "[concat('Microsoft.Network/loadBalancers/', variables('loadBalancerName'))]"
  ],
  "properties": {
    "frontendIPConfiguration": {
      "id": "[variables('ipConfigID')]"
    },
    "protocol": "tcp",
    "frontendPort": "[copyIndex(5000)]",
    "backendPort": 22,
    "enableFloatingIP": false
  }
}
```

Egy példa látható a hello Azure-portálon, bejövő NAT-szabályát. Egy SSH NAT-szabály jön létre hello központi telepítés minden egyes virtuális géphez.

![Bejövő NAT-szabály](./media/dotnet-core-4-availability-scale/natrule.png)

Hello Azure hálózati terheléselosztó részletesebb információkért lásd: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás](../virtual-machines-linux-load-balance.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="deploy-multiple-vms"></a>Több virtuális gép telepítése
Végül egy rendelkezésre állási csoportban, vagy a terheléselosztó tooeffectively függvény több virtuális gép szükség. Több virtuális gép hello Azure Resource Manager sablon másolása függvény használatával is telepíthető. Hello másolási funkcióval, nem szükséges toodefine véges számú virtuális gépnek, ahelyett, hogy ez az érték dinamikusan biztosítható, hogy a központi telepítés hello időpontjában. hello másolási funkcióhoz feldolgozó hello példányok toocreated és központi telepítése a virtuális gépek és a kapcsolódó erőforrásokat megfelelő számú hello leírók száma.

Hello zene tároló minta sablonban megadott példányszám typedValue paraméter van meghatározva. Ezt a számot használja a rendszer keresztül hello sablon virtuális gépek és a kapcsolódó erőforrások létrehozásakor.

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 1,
  "metadata": {
    "description": "Number of VM instances toobe created behind load balancer."
  }
}
```

A virtuálisgép-erőforrás hello hello másolási ciklust, egy nevet kapnak, és példányok paraméter hello száma használt hello toocontrol eredményül kapott példányszámot.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép másolása függvény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L300). 

```json
"apiVersion": "2015-06-15",
"type": "Microsoft.Compute/virtualMachines",
"name": "[concat(variables('vmName'),copyindex())]",
"location": "[resourceGroup().location]",
"copy": {
  "name": "virtualMachineLoop",
  "count": "[parameters('numberOfInstances')]"
}
```

hello aktuális ismétlését hello másolási funkcióhoz is elérhetők a hello `copyIndex()` függvény. hello hello másolási index funkciót is lehet használt tooname virtuális gépek és egyéb erőforrásokat. Például ha egy virtuális gép két példánya van telepítve, szükségük eltérő nevet. Hello `copyIndex()` függvény része a hello virtuális gép neve toocreate egy egyedi módon használható. Példa hello `copyindex()` elnevezési célra használt függvény látható a hello virtuálisgép-erőforrást. Itt hello számítógépnév hello összefűzése `vmName` paraméter, és hello `copyIndex()` függvény. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [másolási Index függvény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L319). 

```json
"osProfile": {
  "computerName": "[concat(parameters('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "linuxConfiguration": {
    "disablePasswordAuthentication": "true",
    "ssh": {
      "publicKeys": [
        {
          "path": "[variables('sshKeyPath')]",
          "keyData": "[parameters('sshKeyData')]"
        }
      ]
    }
  }
}
```

Hello `copyIndex` függvény hello zeneáruház mintasablon többször szerepel. Erőforrások és a funkciók használata `copyIndex` semmi adott tooa hello virtuális gép hálózati illesztő, load balancer szabályok, például egyetlen példányát tartalmazza, és a funkciók függ. 

A másolási funkcióhoz hello további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](../../resource-group-create-multiple.md).

## <a name="next-step"></a>Következő lépés
<hr>

[4. lépés - alkalmazás központi telepítése Azure Resource Manager-sablonok](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

