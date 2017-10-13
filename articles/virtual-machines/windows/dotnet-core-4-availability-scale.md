---
title: "aaaAvailability és az Azure Resource Manager sablonokban méretezési |} Microsoft Docs"
description: "Azure virtuális gép DotNet fő oktatóanyag"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 494724b5-06af-4dea-a774-ba580cf27527
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 05/12/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a122d8e9536ea5fc2dc9c3f84042ed5c5179d783
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="availability-and-scale-in-azure-resource-manager-templates-for-windows-vms"></a><span data-ttu-id="0922f-103">Rendelkezésre állása és méretezése a Windows virtuális gépek Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="0922f-103">Availability and scale in Azure Resource Manager templates for Windows VMs</span></span>

<span data-ttu-id="0922f-104">Rendelkezésre állás és méretezhetőség tekintse meg a toouptime és hello képességét toomeet igény szerint.</span><span class="sxs-lookup"><span data-stu-id="0922f-104">Availability and scale refer toouptime and hello ability toomeet demand.</span></span> <span data-ttu-id="0922f-105">Egy alkalmazás hello idő 99,9 %-át kell lennie, hogy szükséges-e toohave egy architektúra, amely lehetővé teszi, hogy több egyidejű számítási erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0922f-105">If an application must be up 99.9% of hello time, it needs toohave an architecture that allows for multiple concurrent compute resources.</span></span> <span data-ttu-id="0922f-106">Ahelyett, hogy egy webhely, például egy rendelkezésre állási magasabb szintű konfiguráció azonos hely, a terheléselosztás előtti technológia hello több példányát tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="0922f-106">For instance, rather than having a single website, a configuration with a higher level of availability includes multiple instances of hello same site, with balancing technology in front of them.</span></span> <span data-ttu-id="0922f-107">Ebben a konfigurációban hello alkalmazás egy példánya le lehessen állítani karbantartásra, fennmaradó hello leállítaná toofunction.</span><span class="sxs-lookup"><span data-stu-id="0922f-107">In this configuration, one instance of hello application can be taken down for maintenance, while hello remaining continue toofunction.</span></span> <span data-ttu-id="0922f-108">Skála a hello ugyanakkor tooan alkalmazások képességét tooserve igény szerint hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="0922f-108">Scale on hello other hand refers tooan applications ability tooserve demand.</span></span> <span data-ttu-id="0922f-109">Terhelésű kiegyensúlyozott alkalmazás hozzáadása vagy eltávolítása a példányok hello készlet lehetővé teszi, hogy egy alkalmazás tooscale toomeet igény szerint.</span><span class="sxs-lookup"><span data-stu-id="0922f-109">With a load balanced application, adding or removing instances from hello pool allows an application tooscale toomeet demand.</span></span>

<span data-ttu-id="0922f-110">Ez a dokumentum részletesen, hogyan zene tároló üzembe helyezési minta hello beállítása rendelkezésre állása és méretezése.</span><span class="sxs-lookup"><span data-stu-id="0922f-110">This document details how hello Music Store sample deployment is configured for availability and scale.</span></span> <span data-ttu-id="0922f-111">Minden függőségeket és külön konfigurációt vannak kiemelve.</span><span class="sxs-lookup"><span data-stu-id="0922f-111">All dependencies and unique configurations are highlighted.</span></span> <span data-ttu-id="0922f-112">Hello legjobb élmény érdekében előtelepítése hello megoldás tooyour Azure-előfizetés és a munka hello Azure Resource Manager-sablon mellett egy példánya.</span><span class="sxs-lookup"><span data-stu-id="0922f-112">For hello best experience, pre-deploy an instance of hello solution tooyour Azure subscription and work along with hello Azure Resource Manager template.</span></span> <span data-ttu-id="0922f-113">hello teljes sablon – itt található [zene tároló központi telepítést a Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span><span class="sxs-lookup"><span data-stu-id="0922f-113">hello complete template can be found here – [Music Store Deployment on Windows](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).</span></span>

## <a name="availability-set"></a><span data-ttu-id="0922f-114">Rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="0922f-114">Availability Set</span></span>
<span data-ttu-id="0922f-115">Rendelkezésre állási csoport logikailag kiterjedésű Azure virtuális gépek fizikai állomások és egyéb infrastrukturális összetevők, például áramforrások és a fizikai hálózati eszközt.</span><span class="sxs-lookup"><span data-stu-id="0922f-115">An Availability Set logically spans Azure Virtual Machines across physical hosts and other infrastructural components such as power supplies and physical networking hardware.</span></span> <span data-ttu-id="0922f-116">Rendelkezésre állási készletek győződjön meg arról, hogy közben karbantartási, az eszköz nem vagy más állásidő, nem az összes virtuális gép történik.</span><span class="sxs-lookup"><span data-stu-id="0922f-116">Availability sets ensure that during maintenance, device failure, or other down time, not all virtual machines are effected.</span></span> <span data-ttu-id="0922f-117">Rendelkezésre állási csoport is hozzáadhatók a tooan Azure Resource Manager sablon hello Visual Studio új erőforrás hozzáadása varázsló használatával, vagy érvényes JSON beszúrása egy sablont.</span><span class="sxs-lookup"><span data-stu-id="0922f-117">An Availability Set can be added tooan Azure Resource Manager template using hello Visual Studio Add New Resource Wizard, or inserting valid JSON into a template.</span></span>

<span data-ttu-id="0922f-118">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [rendelkezésre állási csoport](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span><span class="sxs-lookup"><span data-stu-id="0922f-118">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L368).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/availabilitySets",
  "name": "[variables('availabilitySetName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "availability-set"
  },
  "properties": {
  }
}
```

<span data-ttu-id="0922f-119">Rendelkezésre állási csoport egy virtuális gép erőforrásának tulajdonságainál deklarálva van.</span><span class="sxs-lookup"><span data-stu-id="0922f-119">An Availability Set is declared as a property of a Virtual Machine resource.</span></span> 

<span data-ttu-id="0922f-120">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [rendelkezésre állási csoport hozzárendelését a virtuális gép](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span><span class="sxs-lookup"><span data-stu-id="0922f-120">Follow this link toosee hello JSON sample within hello Resource Manager template – [Availability Set association with Virtual Machine](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L302).</span></span>

```json
"properties": {
  "availabilitySet": {
    "id": "[resourceId('Microsoft.Compute/availabilitySets', variables('availabilitySetName'))]"
  }
```
<span data-ttu-id="0922f-121">hello rendelkezésre állási csoportban, amint az Azure-portálon hello látható.</span><span class="sxs-lookup"><span data-stu-id="0922f-121">hello availability set as seen from hello Azure portal.</span></span> <span data-ttu-id="0922f-122">Minden virtuális gép és a hello konfigurációs részleteit itt részletes.</span><span class="sxs-lookup"><span data-stu-id="0922f-122">Each virtual machine and details about hello configuration are detailed here.</span></span>

![Rendelkezésre állási csoport](./media/dotnet-core-4-availability-scale/ase-win.png)

<span data-ttu-id="0922f-124">A rendelkezésre állási készletek részletes információkért lásd: [virtuális gépek rendelkezésre állásának kezelése](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0922f-124">For in-depth information on Availability Sets, see [Manage availability of virtual machines](manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span> 

## <a name="network-load-balancer"></a><span data-ttu-id="0922f-125">Hálózati terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="0922f-125">Network Load Balancer</span></span>
<span data-ttu-id="0922f-126">Mivel a rendelkezésre állási csoportok alkalmazás hibatűrést biztosít, a terheléselosztó elérhetővé hello alkalmazás hány példánya egyetlen hálózati címhez.</span><span class="sxs-lookup"><span data-stu-id="0922f-126">Whereas an availability set provides application fault tolerance, a load balancer makes many instances of hello application available on a single network address.</span></span> <span data-ttu-id="0922f-127">Egy alkalmazás több példánya lehet üzemeltetni a több virtuális gép, mindegyik kapcsolódó tooa terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="0922f-127">Multiple instances of an application can be hosted on many virtual machines, each one connected tooa load balancer.</span></span> <span data-ttu-id="0922f-128">Hello alkalmazás érhető el, mert a hello load balancer útvonalak bejövő kérelem hello csatolt hello tagok között.</span><span class="sxs-lookup"><span data-stu-id="0922f-128">As hello application is accessed, hello load balancer routes hello incoming request across hello attached members.</span></span> <span data-ttu-id="0922f-129">A terheléselosztó hello Visual Studio új erőforrás hozzáadása varázsló használatával adhatók, vagy beszúrva megfelelően formázott JSON-erőforrás hello Azure Resource Manager sablonba.</span><span class="sxs-lookup"><span data-stu-id="0922f-129">A Load Balancer can be added using hello Visual Studio Add New Resource Wizard, or by inserting properly formatted JSON resource into hello Azure Resource Manager template.</span></span>

<span data-ttu-id="0922f-130">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati terheléselosztó](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span><span class="sxs-lookup"><span data-stu-id="0922f-130">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L198).</span></span>

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers",
  "name": "[variables('loadBalancerName')]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer"
  },
  ........<truncated>
}
```

<span data-ttu-id="0922f-131">Mivel hello mintaalkalmazás kitett toohello internet nyilvános IP-címek, ez a cím tartozik hello terheléselosztót.</span><span class="sxs-lookup"><span data-stu-id="0922f-131">Because hello sample application is exposed toohello internet with a public IP address, this address is associated with hello load balancer.</span></span> 

<span data-ttu-id="0922f-132">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [hálózati terheléselosztó hozzárendelését a nyilvános IP-cím](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span><span class="sxs-lookup"><span data-stu-id="0922f-132">Follow this link toosee hello JSON sample within hello Resource Manager template – [Network Load Balancer association with Public IP Address](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L211).</span></span>

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

<span data-ttu-id="0922f-133">Hello Azure-portálon, a hello hálózati terheléselosztó áttekintése látható hello hozzárendelését a hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="0922f-133">From hello Azure portal, hello network load balancer overview shows hello association with hello public IP address.</span></span>

![Hálózati terheléselosztó](./media/dotnet-core-4-availability-scale/nlb-win.png)

## <a name="load-balancer-rule"></a><span data-ttu-id="0922f-135">Terheléselosztási szabály</span><span class="sxs-lookup"><span data-stu-id="0922f-135">Load Balancer Rule</span></span>
<span data-ttu-id="0922f-136">Ha terheléselosztót használ, a szabályok konfigurálva vannak, hogyan forgalom szánt hello erőforrások között elosztott terhelésű szabályozó.</span><span class="sxs-lookup"><span data-stu-id="0922f-136">When using a load balancer, rules are configured that control how traffic is balanced across hello intended resources.</span></span> <span data-ttu-id="0922f-137">Zeneáruház hello mintaalkalmazást a forgalom érkezik a hello nyilvános IP-cím 80-as porton, és van elosztva a 80-as port az összes virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="0922f-137">With hello sample Music Store application, traffic arrives on port 80 of hello public IP address and is distributed across port 80 of all virtual machines.</span></span> 

<span data-ttu-id="0922f-138">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [terheléselosztási szabály](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span><span class="sxs-lookup"><span data-stu-id="0922f-138">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Rule](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L226).</span></span>

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

<span data-ttu-id="0922f-139">Hello hálózatot terheléselosztó szabályhoz hello portálról betölteni.</span><span class="sxs-lookup"><span data-stu-id="0922f-139">A view of hello network load balancer rule from hello portal.</span></span>

![Hálózati terheléselosztási szabály](./media/dotnet-core-4-availability-scale/lbrule-win.png)

## <a name="load-balancer-probe"></a><span data-ttu-id="0922f-141">Terheléselosztói mintavétel</span><span class="sxs-lookup"><span data-stu-id="0922f-141">Load Balancer Probe</span></span>
<span data-ttu-id="0922f-142">hello terheléselosztónak is kell toomonitor virtuális gépeken, hogy a kérelmek szolgáltatott csak toorunning rendszerek.</span><span class="sxs-lookup"><span data-stu-id="0922f-142">hello load balancer also needs toomonitor each virtual machine so that requests are served only toorunning systems.</span></span> <span data-ttu-id="0922f-143">Ez a figyelő akkor történik meg által állandó probing egy előre definiált port.</span><span class="sxs-lookup"><span data-stu-id="0922f-143">This monitoring takes place by constant probing of a pre-defined port.</span></span> <span data-ttu-id="0922f-144">hello zeneáruház telepítési konfigurált tooprobe 80-as port az összes szereplő virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="0922f-144">hello Music Store deployment is configured tooprobe port 80 on all included virtual machines.</span></span> 

<span data-ttu-id="0922f-145">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [Load Balancer mintavételi](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span><span class="sxs-lookup"><span data-stu-id="0922f-145">Follow this link toosee hello JSON sample within hello Resource Manager template – [Load Balancer Probe](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L247).</span></span>

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

<span data-ttu-id="0922f-146">hello terheléselosztói mintavétel hello Azure-portálon látható.</span><span class="sxs-lookup"><span data-stu-id="0922f-146">hello load balancer probe seen from hello Azure portal.</span></span>

![Hálózati Terheléselosztói mintavétel](./media/dotnet-core-4-availability-scale/lbprobe-win.png)

## <a name="inbound-nat-rules"></a><span data-ttu-id="0922f-148">Bejövő NAT-szabályok</span><span class="sxs-lookup"><span data-stu-id="0922f-148">Inbound NAT Rules</span></span>
<span data-ttu-id="0922f-149">A terheléselosztó használata esetén a szabályok toobe kell bevezetni által a virtuális gép nem elosztott terhelésű hozzáférés tooeach.</span><span class="sxs-lookup"><span data-stu-id="0922f-149">When using a Load Balancer, rules need toobe put into place that provide non-load balanced access tooeach Virtual Machine.</span></span> <span data-ttu-id="0922f-150">Például egy RDP-kapcsolat az egyes virtuális gépek létrehozásakor a forgalmat nem lehet elosztott terhelésű, ahelyett, hogy egy előre meghatározott elérési utat kell megadni.</span><span class="sxs-lookup"><span data-stu-id="0922f-150">For instance, when creating an RDP connection with each virtual machine, this traffic should not be load balanced, rather a pre-determined path should be configured.</span></span> <span data-ttu-id="0922f-151">előre meghatározott elérési útjának konfigurálása bejövő forgalmat kezelő NAT-szabály erőforrást használ.</span><span class="sxs-lookup"><span data-stu-id="0922f-151">Pre-determined paths are configured using an Inbound NAT Rule resource.</span></span> <span data-ttu-id="0922f-152">Ezt az erőforrást használ, a bejövő kommunikáció lehet csatlakoztatott tooindividual virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="0922f-152">Using this resource, inbound communication can be mapped tooindividual Virtual Machines.</span></span> 

<span data-ttu-id="0922f-153">Hello zene áruház-alkalmazás legyen az port, 5000 kezdődő csatlakoztatott tooport 3389-es minden egyes virtuális gépen az RDP-hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="0922f-153">With hello Music Store application, a port starting at 5000 is mapped tooport 3389 on each Virtual Machine for RDP access.</span></span> <span data-ttu-id="0922f-154">Hello `copyindex()` függvény használt tooincrement hello a bejövő portot, úgy, hogy a második virtuális gép hello fogad egy bejövő portot 5001, harmadik 5002 hello és így tovább.</span><span class="sxs-lookup"><span data-stu-id="0922f-154">hello `copyindex()` function is used tooincrement hello incoming port, such that hello second Virtual Machine receives an incoming port of 5001, hello third 5002, and so on.</span></span>

<span data-ttu-id="0922f-155">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [bejövő NAT-szabályok](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span><span class="sxs-lookup"><span data-stu-id="0922f-155">Follow this link toosee hello JSON sample within hello Resource Manager template – [Inbound NAT Rules](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L260).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Network/loadBalancers/inboundNatRules",
  "name": "[concat(variables('loadBalancerName'), '/', 'RDP-VM', copyIndex())]",
  "location": "[resourceGroup().location]",
  "tags": {
    "displayName": "load-balancer-nat-rule"
  },
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
    "backendPort": 3389,
    "enableFloatingIP": false
  }
}
```

<span data-ttu-id="0922f-156">Egy példa látható a hello Azure-portálon, bejövő NAT-szabályát.</span><span class="sxs-lookup"><span data-stu-id="0922f-156">One example inbound NAT rule as seen in hello Azure portal.</span></span> <span data-ttu-id="0922f-157">Az RDP NAT-szabály jön létre hello központi telepítés minden egyes virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="0922f-157">An RDP NAT rule is created for each virtual machine in hello deployment.</span></span>

![Bejövő NAT-szabály](./media/dotnet-core-4-availability-scale/natrule-win.png)

<span data-ttu-id="0922f-159">Hello Azure hálózati terheléselosztó részletesebb információkért lásd: [Azure infrastruktúra-szolgáltatásokat a terheléselosztás](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="0922f-159">For in-depth information on hello Azure Network Load Balancer, see [Load balancing for Azure infrastructure services](load-balance.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="deploy-multiple-vms"></a><span data-ttu-id="0922f-160">Több virtuális gép telepítése</span><span class="sxs-lookup"><span data-stu-id="0922f-160">Deploy multiple VMs</span></span>
<span data-ttu-id="0922f-161">Végül egy rendelkezésre állási csoportban, vagy a terheléselosztó tooeffectively függvény több virtuális gép szükség.</span><span class="sxs-lookup"><span data-stu-id="0922f-161">Finally, for an Availability Set or Load Balancer tooeffectively function, multiple virtual machines are required.</span></span> <span data-ttu-id="0922f-162">Több virtuális gép hello Azure Resource Manager sablon másolása függvény használatával is telepíthető.</span><span class="sxs-lookup"><span data-stu-id="0922f-162">Multiple VMs can be deployed using hello Azure Resource Manager template copy function.</span></span> <span data-ttu-id="0922f-163">Hello másolási funkcióval, nem szükséges toodefine véges számú virtuális gépnek, ahelyett, hogy ez az érték dinamikusan biztosítható, hogy a központi telepítés hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="0922f-163">Using hello copy function, it is not necessary toodefine a finite number of Virtual Machines, rather this value can be dynamically provided at hello time of deployment.</span></span> <span data-ttu-id="0922f-164">hello másolási funkcióhoz használ fel a létrehozott példányokat toobe hello száma és központi telepítése a virtuális gépek és a kapcsolódó erőforrásokat megfelelő számú hello kezeli.</span><span class="sxs-lookup"><span data-stu-id="0922f-164">hello copy function consumes hello number of instances toobe created and handles deploying hello proper number of virtual machines and associated resources.</span></span>

<span data-ttu-id="0922f-165">Hello zene tároló minta sablonban megadott példányszám typedValue paraméter van meghatározva.</span><span class="sxs-lookup"><span data-stu-id="0922f-165">In hello Music Store Sample template, a parameter is defined that takes in an instance count.</span></span> <span data-ttu-id="0922f-166">Ezt a számot használja a rendszer keresztül hello sablon virtuális gépek és a kapcsolódó erőforrások létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0922f-166">This number is used throughout hello template when creating virtual machines and related resources.</span></span>

```json
"numberOfInstances": {
  "type": "int",
  "minValue": 1,
  "defaultValue": 2,
  "metadata": {
    "description": "Number of VM instances toobe created behind load balancer."
  }
},
```

<span data-ttu-id="0922f-167">A virtuálisgép-erőforrás hello hello másolási ciklust, egy nevet kapnak, és példányok paraméter hello száma használt hello toocontrol eredményül kapott példányszámot.</span><span class="sxs-lookup"><span data-stu-id="0922f-167">On hello Virtual Machine resource, hello copy loop is given a name and hello number of instances parameter used toocontrol hello number of resulting copies.</span></span>

<span data-ttu-id="0922f-168">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [virtuális gép másolása függvény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span><span class="sxs-lookup"><span data-stu-id="0922f-168">Follow this link toosee hello JSON sample within hello Resource Manager template – [Virtual Machine Copy Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L290).</span></span> 

```json
{
  "apiVersion": "2015-06-15",
  "type": "Microsoft.Compute/virtualMachines",
  "name": "[concat(variables('vmName'),copyindex())]",
  "location": "[resourceGroup().location]",
  "copy": {
    "name": "virtualMachineLoop",
    "count": "[parameters('numberOfInstances')]"
  }
```

<span data-ttu-id="0922f-169">hello aktuális ismétlését hello másolási funkcióhoz is elérhetők a hello `copyIndex()` függvény.</span><span class="sxs-lookup"><span data-stu-id="0922f-169">hello current iteration of hello copy function can be accessed with hello `copyIndex()` function.</span></span> <span data-ttu-id="0922f-170">hello hello másolási index funkciót is lehet használt tooname virtuális gépek és egyéb erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="0922f-170">hello value of hello copy index function can be used tooname virtual machines and other resources.</span></span> <span data-ttu-id="0922f-171">Például ha egy virtuális gép két példánya van telepítve, szükségük eltérő nevet.</span><span class="sxs-lookup"><span data-stu-id="0922f-171">For instance, if two instances of a virtual machine are deployed, they need different names.</span></span> <span data-ttu-id="0922f-172">Hello `copyIndex()` függvény része a hello virtuális gép neve toocreate egy egyedi módon használható.</span><span class="sxs-lookup"><span data-stu-id="0922f-172">hello `copyIndex()` function can be used as part of hello virtual machine name toocreate a unique name.</span></span> <span data-ttu-id="0922f-173">Példa hello `copyindex()` elnevezési célra használt függvény látható a hello virtuálisgép-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="0922f-173">An example of hello `copyindex()` function used for naming purposes can be seen in hello Virtual Machine resource.</span></span> <span data-ttu-id="0922f-174">Itt hello számítógépnév hello összefűzése `vmName` paraméter, és hello `copyIndex()` függvény.</span><span class="sxs-lookup"><span data-stu-id="0922f-174">Here, hello computer name is a concatenation of hello `vmName` parameter, and hello `copyIndex()` function.</span></span> 

<span data-ttu-id="0922f-175">Kövesse a hivatkozást toosee hello JSON minta belül hello Resource Manager-sablon – [másolási Index függvény](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span><span class="sxs-lookup"><span data-stu-id="0922f-175">Follow this link toosee hello JSON sample within hello Resource Manager template – [Copy Index Function](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L309).</span></span> 

```json
"osProfile": {
  "computerName": "[concat(variables('vmName'),copyindex())]",
  "adminUsername": "[parameters('adminUsername')]",
  "adminPassword": "[parameters('adminPassword')]"
}
```

<span data-ttu-id="0922f-176">Hello `copyIndex` függvény hello zeneáruház mintasablon többször szerepel.</span><span class="sxs-lookup"><span data-stu-id="0922f-176">hello `copyIndex` function is used several times in hello Music Store sample template.</span></span> <span data-ttu-id="0922f-177">Erőforrások és a funkciók használata `copyIndex` semmi adott tooa hello virtuális gép ilyen egyetlen példányát és a hálózati illesztő, load balancer szabályok, és bármely funkciók függ.</span><span class="sxs-lookup"><span data-stu-id="0922f-177">Resources and functions utilizing `copyIndex` include anything specific tooa single instance of hello virtual machine such and network interface, load balancer rules, and any depends on functions.</span></span> 

<span data-ttu-id="0922f-178">A másolási funkcióhoz hello további információkért lásd: [erőforrások több példánya létrehozása az Azure Resource Manager](../../resource-group-create-multiple.md).</span><span class="sxs-lookup"><span data-stu-id="0922f-178">For more information on hello copy function, see [Create multiple instances of resources in Azure Resource Manager](../../resource-group-create-multiple.md).</span></span>

## <a name="next-step"></a><span data-ttu-id="0922f-179">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="0922f-179">Next step</span></span>
<hr>

[<span data-ttu-id="0922f-180">4. lépés - alkalmazás központi telepítése Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="0922f-180">Step 4 - Application Deployment with Azure Resource Manager Templates</span></span>](dotnet-core-5-app-deployment.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
