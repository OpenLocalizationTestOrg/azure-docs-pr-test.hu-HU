---
title: "aaaAccess és a Linux virtuális gépek Azure-sablonok biztonsági |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 07e47189-680e-4102-a8d4-5a8eb9c00213
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 88fedc8287c1f8ab8397a03ddefe1e60a686815e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-linux-vms"></a>A hozzáférési és az Azure Resource Manager sablonokban Linux virtuális gépek biztonsági

Alkalmazások üzemeltetett Azure valószínűleg kell toobe Access keresztül hello internet vagy egy VPN-vagy Azure Expressroute-kapcsolatot. Hello zene Áruházbeli alkalmazás példával, hello webhely szeretné elérhetővé tenni az internet hello nyilvános IP-címmel. A létrehozott hozzáférés kapcsolatok toohello alkalmazás és szolgáltatás toohello virtuálisgép-erőforrások maguk védve legyenek. A hozzáférés biztonsági által biztosított hálózati biztonsági csoport. 

Ez a dokumentum részletesen hello zene áruházból származó alkalmazás hello minta Azure Resource Manager sablon titkosításának módját. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítését, az Ubuntu](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-linux). 

## <a name="public-ip-address"></a>Nyilvános IP-cím
tooprovide nyilvános hozzáférés tooan Azure-erőforrás, egy nyilvános IP-cím erőforrás is használható. Nyilvános IP-cím konfigurálható statikus vagy dinamikus IP-címmel. Ha egy dinamikus címet használja, és hello virtuális gép leállítása és felszabadítása. lehetséges, a rendszer eltávolítja hello címek. Ha hello gép indítja újra, akkor rendelt egy másik nyilvános IP-címet. tooprevent egy IP-cím módosítsák, egy fenntartott IP-cím is használható. 

A nyilvános IP-cím felveheti tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L121).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicipaddressName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "public-ip-front"
  },
  "properties": {
    "publicIPAllocationMethod": "Dynamic",
    "dnsSettings": {
      "domainNameLabel": "[parameters('publicipaddressDnsName')]"
    }
  }
}
```

Lehet, hogy a virtuális hálózati Adapter vagy egy adott terheléselosztóhoz társított egy nyilvános IP-címet. Ebben a példában a mert hello zeneáruház webhely el több virtuális gépre, elosztott terhelésű hello nyilvános IP-cím csatolt toohello terheléselosztó.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím hozzárendelését a Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L208).

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

hello nyilvános IP-címre látható hello Azure-portálon. Figyelje meg, hogy hello nyilvános IP-cím-e a terheléselosztóhoz társított tooa és nem a virtuális gép. Hálózati terheléselosztók részletes leírást talál a következő dokumentum hello a sor.

![Nyilvános IP-cím](./media/dotnet-core-3-access-security/pubip.png)

Az Azure nyilvános IP-címek további információkért lásd: [IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Hálózati biztonsági csoport
Hozzáférés a meghatározott tooAzure erőforrások követően a hozzáférés kell korlátozni. Az Azure virtuális gépek biztonságos hozzáférést teszi lehetővé a hálózati biztonsági csoport. Hello zene Áruházbeli alkalmazás példával minden toohello virtuálisgép korlátozódik kivételével a 80-as port HTTP-hozzáférést, és az SSH-elérést 22-es portot. A hálózati biztonsági csoportok adhatók tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L68).

```json
{
  "apiVersion": "2015-05-01-preview",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('nsgfront')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "nsg-front"
  },
  "properties": {
    "securityRules": [
      {
        "name": "http",
        "properties": {
          "description": "http endpoint",
          "protocol": "Tcp",
          "sourcePortRange": "*",
          "destinationPortRange": "80",
          "sourceAddressPrefix": "*",
          "destinationAddressPrefix": "*",
          "access": "Allow",
          "priority": 100,
          "direction": "Inbound"
        }
      },
      ........<truncated> 
    ]
  }
}
```

Ebben a példában hello hálózati biztonsági csoport társítható hello-alhálózati objektum hello virtuális hálózati erőforrás deklarálva. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport hozzárendelését a virtuális hálózati](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-linux/azuredeploy.json#L158).

```json
"subnets": [
  {
    "name": "[variables('subnetName')]",
    "properties": {
      "addressPrefix": "10.0.0.0/24",
      "networkSecurityGroup": {
        "id": "[resourceId('Microsoft.Network/networkSecurityGroups', variables('networkSecurityGroup'))]"
      }
    }
  }
```

Ez milyen hello hálózati biztonsági csoportot a következőképpen néz hello az Azure-portálon. Figyelje meg, hogy egy NSG-t is társíthat egy alhálózat és / vagy hálózati illesztő. Ebben az esetben a hello NSG társított tooa alhálózat. Ebben a konfigurációban hello bejövő szabályok alkalmazása tooall csatlakozó virtuális gépek toohello alhálózat.

![Hálózati biztonsági csoport](./media/dotnet-core-3-access-security/nsg.png)

Hálózati biztonsági csoportokkal kapcsolatos részletes információkért lásd: [Mi az a hálózati biztonsági csoport](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Következő lépés
<hr>

[3. lépés – rendelkezésre állásának és méretezésének az Azure Resource Manager-sablonok](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

