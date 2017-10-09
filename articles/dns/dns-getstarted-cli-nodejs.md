---
title: "aaaGet Azure DNS használata az Azure CLI 1.0-s használatába |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate DNS zóna, illetve az Azure DNS-rekord. Ez egy részletes útmutató toocreate, és kezeli az első DNS-zóna és a rekord hello Azure CLI 1.0 használatával."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: e200c848ad261160e593306dbb8a1d92bf26880b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-10"></a><span data-ttu-id="94d8e-104">Az Azure DNS használatának első lépései az Azure CLI 1.0-val</span><span class="sxs-lookup"><span data-stu-id="94d8e-104">Get started with Azure DNS using Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="94d8e-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="94d8e-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="94d8e-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="94d8e-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="94d8e-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="94d8e-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="94d8e-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="94d8e-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="94d8e-109">Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első DNS-zónát, és rekord segítségével hello Azure CLI 1.0 platformfüggetlen, elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="94d8e-109">This article walks you through hello steps toocreate your first DNS zone and record using hello cross-platform Azure CLI 1.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="94d8e-110">Ezeket a lépéseket hello Azure-portálon vagy az Azure PowerShell használatával is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="94d8e-110">You can also perform these steps using hello Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="94d8e-111">A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban.</span><span class="sxs-lookup"><span data-stu-id="94d8e-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="94d8e-112">az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="94d8e-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="94d8e-113">Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre.</span><span class="sxs-lookup"><span data-stu-id="94d8e-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="94d8e-114">Végezetül toopublish a DNS-zónák toohello Internet, tooconfigure hello névkiszolgálók hello tartomány van szüksége.</span><span class="sxs-lookup"><span data-stu-id="94d8e-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="94d8e-115">Az egyes lépéseket az alábbiakban ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="94d8e-115">Each of these steps is described below.</span></span>

<span data-ttu-id="94d8e-116">Ezek az utasítások azt feltételezik, ha már telepítette, és tooAzure CLI 1.0 bejelentkezve.</span><span class="sxs-lookup"><span data-stu-id="94d8e-116">These instructions assume you have already installed and signed in tooAzure CLI 1.0.</span></span> <span data-ttu-id="94d8e-117">Útmutatásért lásd: [hogyan toomanage DNS zónák használata az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-117">For help, see [How toomanage DNS zones using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="94d8e-118">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="94d8e-118">Create hello resource group</span></span>

<span data-ttu-id="94d8e-119">Mielőtt létrehozna hello DNS-zónát, egy erőforráscsoportot toocontain hello DNS-zóna jön létre.</span><span class="sxs-lookup"><span data-stu-id="94d8e-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="94d8e-120">hello következő hello parancs jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="94d8e-120">hello following shows hello command.</span></span>

```azurecli
azure group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="94d8e-121">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="94d8e-121">Create a DNS zone</span></span>

<span data-ttu-id="94d8e-122">A DNS-zóna létrehozása hello használatával `azure network dns zone create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94d8e-122">A DNS zone is created using hello `azure network dns zone create` command.</span></span> <span data-ttu-id="94d8e-123">a parancs toosee súgójának, írja be `azure network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="94d8e-123">toosee help for this command, type `azure network dns zone create -h`.</span></span>

<span data-ttu-id="94d8e-124">hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="94d8e-124">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="94d8e-125">Hello példa toocreate egy DNS-zónát és hello értékeket a saját használja.</span><span class="sxs-lookup"><span data-stu-id="94d8e-125">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```azurecli
azure network dns zone create MyResourceGroup contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="94d8e-126">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="94d8e-126">Create a DNS record</span></span>

<span data-ttu-id="94d8e-127">egy DNS-rekord toocreate hello használata `azure network dns record-set add-record` parancsot.</span><span class="sxs-lookup"><span data-stu-id="94d8e-127">toocreate a DNS record, use hello `azure network dns record-set add-record` command.</span></span> <span data-ttu-id="94d8e-128">További segítségért lásd: `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="94d8e-128">For help, see `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="94d8e-129">hello alábbi példa létrehoz egy rekordot hello relatív névvel "www" hello "contoso.com", az erőforráscsoportban "Contoso.com" DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="94d8e-129">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="94d8e-130">hello rekordhalmaz hello teljesen minősített neve "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="94d8e-130">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="94d8e-131">hello rekordtípus "A", "1.2.3.4" IP-címmel, és egy alapértelmezett élettartam 3600 másodperc (1 óra) használt.</span><span class="sxs-lookup"><span data-stu-id="94d8e-131">hello record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

<span data-ttu-id="94d8e-132">Egyéb típusú bejegyzés a rekordhalmazok alternatív TTL értékeket, és toomodify meglévő rekordokat, a több rekordot tartalmazó lásd: [kezelése DNS-rekordok és a rekordhalmazok használatával hello Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-132">For other record types, for record sets with more than one record, for alternative TTL values, and toomodify existing records, see [Manage DNS records and record sets using hello Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="94d8e-133">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="94d8e-133">View records</span></span>

<span data-ttu-id="94d8e-134">toolist hello DNS-rekordokat, amelyek a zónához használja:</span><span class="sxs-lookup"><span data-stu-id="94d8e-134">toolist hello DNS records in your zone, use:</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="94d8e-135">A névkiszolgálók frissítése</span><span class="sxs-lookup"><span data-stu-id="94d8e-135">Update name servers</span></span>

<span data-ttu-id="94d8e-136">Ha mindent megfelelőnek talált, hogy a DNS-zóna és a rekordok beállított megfelelően, tooconfigure van szüksége a tartománynév toouse hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="94d8e-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="94d8e-137">Ez lehetővé teszi a más felhasználóktól a hello Internet toofind a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="94d8e-137">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="94d8e-138">Adja meg a zóna névkiszolgálóit hello hello `azure network dns zone show` parancs:</span><span class="sxs-lookup"><span data-stu-id="94d8e-138">hello name servers for your zone are given by hello `azure network dns zone show` command:</span></span>

```azurecli
azure network dns zone show MyResourceGroup contoso.com

info:    Executing command network dns zone show
+ Looking up hello dns zone "contoso.com"
data:    Id                              : /subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com
data:    Name                            : contoso.com
data:    Type                            : Microsoft.Network/dnszones
data:    Location                        : global
data:    Number of record sets           : 3
data:    Max number of record sets       : 5000
data:    Name servers:
data:        ns1-01.azure-dns.com.
data:        ns2-01.azure-dns.net.
data:        ns3-01.azure-dns.org.
data:        ns4-01.azure-dns.info.
data:    Tags                            :
info:    network dns zone show command OK
```

<span data-ttu-id="94d8e-139">Ezeket a kiszolgálókat (vásárolta hello tartománynevet) hello regisztrációs kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="94d8e-139">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="94d8e-140">A regisztráló felajánlja, hello beállítás tooset hello neve kiszolgáló hello tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="94d8e-140">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="94d8e-141">További információkért lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-141">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="94d8e-142">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="94d8e-142">Delete all resources</span></span>
 
<span data-ttu-id="94d8e-143">toodelete ebben a cikkben a következő lépés hajtsa végre a megfelelő hello létrehozott összes erőforrás:</span><span class="sxs-lookup"><span data-stu-id="94d8e-143">toodelete all resources created in this article, take hello following step:</span></span>

```azurecli
azure group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="94d8e-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="94d8e-144">Next steps</span></span>

<span data-ttu-id="94d8e-145">További információ az Azure DNS-beli toolearn lásd [Azure DNS áttekintése](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-145">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="94d8e-146">További információ az Azure DNS-, DNS-zónák kezelése toolearn lásd [kezelése DNS-zónák az Azure DNS használata az Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-146">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md).</span></span>

<span data-ttu-id="94d8e-147">További információ az Azure DNS-, DNS-rekordok kezelése toolearn lásd [kezelése DNS-rekordok és a rekord beállítja az Azure DNS használata az Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="94d8e-147">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md).</span></span>

