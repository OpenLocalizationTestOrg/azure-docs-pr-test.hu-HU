---
title: "aaaAccess és az Azure-sablonok Windows virtuális gépek biztonsági |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: e671fc45-5e4d-40fd-aac5-290892713cc0
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 4b8227ae745b3b0a22d136e98d18479f8b1504c8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a>A hozzáférési és az Azure Resource Manager sablonokban Windows virtuális gépek biztonsági

Alkalmazások üzemeltetett Azure valószínűleg kell toobe Access keresztül hello internet vagy egy VPN-vagy Azure Expressroute-kapcsolatot. Hello zene Áruházbeli alkalmazás példával, hello webhely szeretné elérhetővé tenni az internet hello nyilvános IP-címmel. A létrehozott hozzáférés kapcsolatok toohello alkalmazás és szolgáltatás toohello virtuálisgép-erőforrások maguk védve legyenek. A hozzáférés biztonsági által biztosított hálózati biztonsági csoport. 

Ez a dokumentum részletesen hello zene áruházból származó alkalmazás hello minta Azure Resource Manager sablon titkosításának módját. Minden függőségeket és külön konfigurációt vannak kiemelve. Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya. hello teljes sablon – itt található [zene tároló központi telepítést a Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="public-ip-address"></a>Nyilvános IP-cím
tooprovide nyilvános hozzáférés tooan Azure-erőforrás, egy nyilvános IP-cím erőforrás is használható. Nyilvános IP-cím konfigurálható statikus vagy dinamikus IP-címmel. Ha egy dinamikus címet használja, és hello virtuális gép leállítása és felszabadítása. lehetséges, a rendszer eltávolítja hello címek. Ha hello gép indítja újra, akkor rendelt egy másik nyilvános IP-címet. tooprevent egy IP-cím módosítsák, egy fenntartott IP-cím is használható. 

A nyilvános IP-cím felveheti tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/publicIPAddresses",
  "name": "[variables('publicIpAddressName')]",
  "location": "[resourceGroup().location]",
  "dependsOn": [],
  "tags": {
    "displayName": "public-ip"
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

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím hozzárendelését a Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).

```json
"frontendIPConfigurations": [
  {
    "properties": {
      "publicIPAddress": {
        "id": "[resourceId('Microsoft.Network/publicIPAddresses', variables('publicIpAddressName'))]"
      }
    },
    "name": "LoadBalancerFrontend"
  }
]
```

hello nyilvános IP-címre látható hello Azure-portálon. Figyelje meg, hogy hello nyilvános IP-cím-e a terheléselosztóhoz társított tooa és nem a virtuális gép. Hálózati terheléselosztók részletes leírást talál a következő dokumentum hello a sor.

![Nyilvános IP-cím](./media/dotnet-core-3-access-security/pubip-win.png)

Az Azure nyilvános IP-címek további információkért lásd: [IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).

## <a name="network-security-group"></a>Hálózati biztonsági csoport
Hozzáférés a meghatározott tooAzure erőforrások követően a hozzáférés kell korlátozni. Az Azure virtuális gépek biztonságos hozzáférést teszi lehetővé a hálózati biztonsági csoport. Hello zene Áruházbeli alkalmazás példával minden toohello virtuálisgép korlátozódik kivételével a 80-as port HTTP-hozzáférést, és az RDP-hozzáférést a 3389-es portot. A hálózati biztonsági csoportok adhatók tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont.

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Network/networkSecurityGroups",
  "name": "[variables('networkSecurityGroup')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "network-security-group"
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
},
```

Ebben a példában hello hálózati biztonsági csoport társítható hello-alhálózati objektum hello virtuális hálózati erőforrás deklarálva. 

Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport hozzárendelését a virtuális hálózati](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).

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
]
```

Ez milyen hello hálózati biztonsági csoportot a következőképpen néz hello az Azure-portálon. Figyelje meg, hogy egy NSG-t is társíthat egy alhálózat és / vagy hálózati illesztő. Ebben az esetben a hello NSG társított tooa alhálózat. Ebben a konfigurációban hello bejövő szabályok alkalmazása tooall csatlakozó virtuális gépek toohello alhálózat.

![Hálózati biztonsági csoport](./media/dotnet-core-3-access-security/nsg-win.png)

Hálózati biztonsági csoportokkal kapcsolatos részletes információkért lásd: [Mi az a hálózati biztonsági csoport](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).

## <a name="next-step"></a>Következő lépés
<hr>

[3. lépés – rendelkezésre állásának és méretezésének az Azure Resource Manager-sablonok](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

