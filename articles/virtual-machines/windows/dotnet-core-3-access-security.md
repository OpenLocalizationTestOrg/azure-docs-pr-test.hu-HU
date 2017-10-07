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
# <a name="access-and-security-in-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="6edea-103">A hozzáférési és az Azure Resource Manager sablonokban Windows virtuális gépek biztonsági</span><span class="sxs-lookup"><span data-stu-id="6edea-103">Access and security in Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="6edea-104">Alkalmazások üzemeltetett Azure valószínűleg kell toobe Access keresztül hello internet vagy egy VPN-vagy Azure Expressroute-kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="6edea-104">Applications hosted in Azure likely need toobe access over hello internet or a VPN / Express Route connection with Azure.</span></span> <span data-ttu-id="6edea-105">Hello zene Áruházbeli alkalmazás példával, hello webhely szeretné elérhetővé tenni az internet hello nyilvános IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="6edea-105">With hello Music Store application sample, hello web site is made available on hello internet with a public IP address.</span></span> <span data-ttu-id="6edea-106">A létrehozott hozzáférés kapcsolatok toohello alkalmazás és szolgáltatás toohello virtuálisgép-erőforrások maguk védve legyenek.</span><span class="sxs-lookup"><span data-stu-id="6edea-106">With access established, connections toohello application and access toohello virtual machine resources themselves should be secured.</span></span> <span data-ttu-id="6edea-107">A hozzáférés biztonsági által biztosított hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="6edea-107">This access security is provided with a Network Security Group.</span></span> 

<span data-ttu-id="6edea-108">Ez a dokumentum részletesen hello zene áruházból származó alkalmazás hello minta Azure Resource Manager sablon titkosításának módját.</span><span class="sxs-lookup"><span data-stu-id="6edea-108">This document details how hello Music Store application is secured in hello sample Azure Resource Manager template.</span></span> <span data-ttu-id="6edea-109">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="6edea-109">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="6edea-110">Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya.</span><span class="sxs-lookup"><span data-stu-id="6edea-110">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="6edea-111">hello teljes sablon – itt található [zene tároló központi telepítést a Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="6edea-111">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="public-ip-address"></a><span data-ttu-id="6edea-112">Nyilvános IP-cím</span><span class="sxs-lookup"><span data-stu-id="6edea-112">Public IP Address</span></span>
<span data-ttu-id="6edea-113">tooprovide nyilvános hozzáférés tooan Azure-erőforrás, egy nyilvános IP-cím erőforrás is használható.</span><span class="sxs-lookup"><span data-stu-id="6edea-113">tooprovide public access tooan Azure resource, a public IP address resource can be used.</span></span> <span data-ttu-id="6edea-114">Nyilvános IP-cím konfigurálható statikus vagy dinamikus IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="6edea-114">Public IP address can be configured with a static or dynamic IP address.</span></span> <span data-ttu-id="6edea-115">Ha egy dinamikus címet használja, és hello virtuális gép leállítása és felszabadítása. lehetséges, a rendszer eltávolítja hello címek.</span><span class="sxs-lookup"><span data-stu-id="6edea-115">If a dynamic address is used, and hello virtual machine is stopped and deallocated, hello addresses is removed.</span></span> <span data-ttu-id="6edea-116">Ha hello gép indítja újra, akkor rendelt egy másik nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="6edea-116">When hello machine is started again, it may be assigned a different public IP address.</span></span> <span data-ttu-id="6edea-117">tooprevent egy IP-cím módosítsák, egy fenntartott IP-cím is használható.</span><span class="sxs-lookup"><span data-stu-id="6edea-117">tooprevent an IP address from changing, a reserved IP address can be used.</span></span> 

<span data-ttu-id="6edea-118">A nyilvános IP-cím felveheti tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont.</span><span class="sxs-lookup"><span data-stu-id="6edea-118">A Public IP Address can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span> 

<span data-ttu-id="6edea-119">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).</span><span class="sxs-lookup"><span data-stu-id="6edea-119">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L110).</span></span>

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

<span data-ttu-id="6edea-120">Lehet, hogy a virtuális hálózati Adapter vagy egy adott terheléselosztóhoz társított egy nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="6edea-120">A Public IP Address can be associated with a Virtual Network Adapter, or a Load Balancer.</span></span> <span data-ttu-id="6edea-121">Ebben a példában a mert hello zeneáruház webhely el több virtuális gépre, elosztott terhelésű hello nyilvános IP-cím csatolt toohello terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="6edea-121">In this example, because hello Music Store website is load balanced across several virtual machines, hello Public IP Address is attached toohello Load Balancer.</span></span>

<span data-ttu-id="6edea-122">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [nyilvános IP-cím hozzárendelését a Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span><span class="sxs-lookup"><span data-stu-id="6edea-122">Follow this link toosee hello JSON sample within hello Resource Manager template – [Public IP Address association with Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span></span>

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

<span data-ttu-id="6edea-123">hello nyilvános IP-címre látható hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6edea-123">hello public IP Address as seen from hello Azure portal.</span></span> <span data-ttu-id="6edea-124">Figyelje meg, hogy hello nyilvános IP-cím-e a terheléselosztóhoz társított tooa és nem a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6edea-124">Notice that hello public IP address is associated tooa load balancer and not a virtual machine.</span></span> <span data-ttu-id="6edea-125">Hálózati terheléselosztók részletes leírást talál a következő dokumentum hello a sor.</span><span class="sxs-lookup"><span data-stu-id="6edea-125">Network load balancers are detailed in hello next document of this series.</span></span>

![Nyilvános IP-cím](./media/dotnet-core-3-access-security/pubip-win.png)

<span data-ttu-id="6edea-127">Az Azure nyilvános IP-címek további információkért lásd: [IP-címek az Azure-ban](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6edea-127">For more information on Azure Public IP Addresses, see [IP addresses in Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md).</span></span>

## <a name="network-security-group"></a><span data-ttu-id="6edea-128">Hálózati biztonsági csoport</span><span class="sxs-lookup"><span data-stu-id="6edea-128">Network Security Group</span></span>
<span data-ttu-id="6edea-129">Hozzáférés a meghatározott tooAzure erőforrások követően a hozzáférés kell korlátozni.</span><span class="sxs-lookup"><span data-stu-id="6edea-129">Once access has been established tooAzure resources, this access should be limited.</span></span> <span data-ttu-id="6edea-130">Az Azure virtuális gépek biztonságos hozzáférést teszi lehetővé a hálózati biztonsági csoport.</span><span class="sxs-lookup"><span data-stu-id="6edea-130">For Azure virtual machines, secure access is accomplished using a network security group.</span></span> <span data-ttu-id="6edea-131">Hello zene Áruházbeli alkalmazás példával minden toohello virtuálisgép korlátozódik kivételével a 80-as port HTTP-hozzáférést, és az RDP-hozzáférést a 3389-es portot.</span><span class="sxs-lookup"><span data-stu-id="6edea-131">With hello Music Store application sample, all access toohello virtual machine is restricted except for over port 80 for http access, and port 3389 for RDP access.</span></span> <span data-ttu-id="6edea-132">A hálózati biztonsági csoportok adhatók tooan Azure Resource Manager-sablon használatával hello Visual Studio új erőforrás hozzáadása varázsló, vagy érvényes JSON beszúrása egy sablont.</span><span class="sxs-lookup"><span data-stu-id="6edea-132">A Network Security Group can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or by inserting valid JSON into a template.</span></span>

<span data-ttu-id="6edea-133">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).</span><span class="sxs-lookup"><span data-stu-id="6edea-133">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L57).</span></span>

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

<span data-ttu-id="6edea-134">Ebben a példában hello hálózati biztonsági csoport társítható hello-alhálózati objektum hello virtuális hálózati erőforrás deklarálva.</span><span class="sxs-lookup"><span data-stu-id="6edea-134">In this example, hello network security group is associate with hello subnet object declared in hello Virtual Network resource.</span></span> 

<span data-ttu-id="6edea-135">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati biztonsági csoport hozzárendelését a virtuális hálózati](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).</span><span class="sxs-lookup"><span data-stu-id="6edea-135">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Security Group association with Virtual Network](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L143).</span></span>

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

<span data-ttu-id="6edea-136">Ez milyen hello hálózati biztonsági csoportot a következőképpen néz hello az Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="6edea-136">Here is what hello network security group looks like from hello Azure portal.</span></span> <span data-ttu-id="6edea-137">Figyelje meg, hogy egy NSG-t is társíthat egy alhálózat és / vagy hálózati illesztő.</span><span class="sxs-lookup"><span data-stu-id="6edea-137">Notice that an NSG can be associate with a subnet and / or network interface.</span></span> <span data-ttu-id="6edea-138">Ebben az esetben a hello NSG társított tooa alhálózat.</span><span class="sxs-lookup"><span data-stu-id="6edea-138">In this case, hello NSG is associated tooa subnet.</span></span> <span data-ttu-id="6edea-139">Ebben a konfigurációban hello bejövő szabályok alkalmazása tooall csatlakozó virtuális gépek toohello alhálózat.</span><span class="sxs-lookup"><span data-stu-id="6edea-139">In this configuration, hello inbound rules apply tooall virtual machines connected toohello subnet.</span></span>

![Hálózati biztonsági csoport](./media/dotnet-core-3-access-security/nsg-win.png)

<span data-ttu-id="6edea-141">Hálózati biztonsági csoportokkal kapcsolatos részletes információkért lásd: [Mi az a hálózati biztonsági csoport](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span><span class="sxs-lookup"><span data-stu-id="6edea-141">For in-depth information on Network Security Groups, see [What is a Network Security Group](https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/).</span></span>

## <a name="next-step"></a><span data-ttu-id="6edea-142">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="6edea-142">Next step</span></span>
<hr>

[<span data-ttu-id="6edea-143">3. lépés – rendelkezésre állásának és méretezésének az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="6edea-143">Step 3 - Availability and Scale in Azure Resource Manager Templates</span></span>](dotnet-core-4-availability-scale.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
