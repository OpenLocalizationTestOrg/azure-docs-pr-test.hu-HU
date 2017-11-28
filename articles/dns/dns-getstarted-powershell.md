---
title: "aaaGet lépések az Azure DNS PowerShell-lel |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate DNS zóna, illetve az Azure DNS-rekord. Ez egy részletes útmutató toocreate és kezelése az első DNS-zónát, és rögzítse a PowerShell használatával."
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
ms.openlocfilehash: 0f9dead1e4b44fcc74c84a024c41cdfaeb02b5d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a><span data-ttu-id="e57d3-104">Az Azure DNS PowerShell-lel való használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="e57d3-104">Get Started with Azure DNS using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="e57d3-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="e57d3-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="e57d3-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="e57d3-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="e57d3-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="e57d3-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="e57d3-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="e57d3-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="e57d3-109">Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első DNS-zónát és az Azure PowerShell rögzítése.</span><span class="sxs-lookup"><span data-stu-id="e57d3-109">This article walks you through hello steps toocreate your first DNS zone and record using Azure PowerShell.</span></span> <span data-ttu-id="e57d3-110">A következő lépésekkel hello Azure-portál használatával vagy platformok közötti Azure CLI hello is.</span><span class="sxs-lookup"><span data-stu-id="e57d3-110">You can also perform these steps using hello Azure portal or hello cross-platform Azure CLI.</span></span>

<span data-ttu-id="e57d3-111">A DNS-zónák használt toohost hello DNS-rekordok az adott tartományban.</span><span class="sxs-lookup"><span data-stu-id="e57d3-111">A DNS zone is used toohost hello DNS records for a particular domain.</span></span> <span data-ttu-id="e57d3-112">az Azure DNS, a tartomány toostart toocreate DNS-zóna van szüksége a tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="e57d3-112">toostart hosting your domain in Azure DNS, you need toocreate a DNS zone for that domain name.</span></span> <span data-ttu-id="e57d3-113">Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre.</span><span class="sxs-lookup"><span data-stu-id="e57d3-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="e57d3-114">Végezetül toopublish a DNS-zónák toohello Internet, tooconfigure hello névkiszolgálók hello tartomány van szüksége.</span><span class="sxs-lookup"><span data-stu-id="e57d3-114">Finally, toopublish your DNS zone toohello Internet, you need tooconfigure hello name servers for hello domain.</span></span> <span data-ttu-id="e57d3-115">Az egyes lépéseket az alábbiakban ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="e57d3-115">Each of these steps is described below.</span></span>

<span data-ttu-id="e57d3-116">Ezek az utasítások azt feltételezik, ha már telepítette, és tooAzure PowerShell bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="e57d3-116">These instructions assume you have already installed and signed in tooAzure PowerShell.</span></span> <span data-ttu-id="e57d3-117">Útmutatásért lásd: [hogyan toomanage DNS zónák PowerShell-lel](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-117">For help, see [How toomanage DNS zones using PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-hello-resource-group"></a><span data-ttu-id="e57d3-118">Hello erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="e57d3-118">Create hello resource group</span></span>

<span data-ttu-id="e57d3-119">Mielőtt létrehozna hello DNS-zónát, egy erőforráscsoportot toocontain hello DNS-zóna jön létre.</span><span class="sxs-lookup"><span data-stu-id="e57d3-119">Before creating hello DNS zone, a resource group is created toocontain hello DNS Zone.</span></span> <span data-ttu-id="e57d3-120">hello következő hello parancs jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="e57d3-120">hello following shows hello command.</span></span>

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="e57d3-121">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="e57d3-121">Create a DNS zone</span></span>

<span data-ttu-id="e57d3-122">A DNS-zónák hello segítségével hozhatók létre `New-AzureRmDnsZone` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e57d3-122">A DNS zone is created by using hello `New-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="e57d3-123">hello alábbi példa létrehoz egy DNS-zónát *contoso.com* nevű hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="e57d3-123">hello following example creates a DNS zone called *contoso.com* in hello resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="e57d3-124">Hello példa toocreate egy DNS-zónát és hello értékeket a saját használja.</span><span class="sxs-lookup"><span data-stu-id="e57d3-124">Use hello example toocreate a DNS zone, substituting hello values for your own.</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a><span data-ttu-id="e57d3-125">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="e57d3-125">Create a DNS record</span></span>

<span data-ttu-id="e57d3-126">Rekordhalmazok hello segítségével hozhatja létre `New-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="e57d3-126">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="e57d3-127">hello alábbi példa létrehoz egy rekordot hello relatív névvel "www" hello "contoso.com", az erőforráscsoportban "Contoso.com" DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="e57d3-127">hello following example creates a record with hello relative name "www" in hello DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="e57d3-128">hello rekordhalmaz hello teljesen minősített neve "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="e57d3-128">hello fully-qualified name of hello record set is "www.contoso.com".</span></span> <span data-ttu-id="e57d3-129">hello rekordtípus "A", "1.2.3.4" IP-címmel, pedig hello élettartam 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="e57d3-129">hello record type is "A", with IP address "1.2.3.4", and hello TTL is 3600 seconds.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

<span data-ttu-id="e57d3-130">Egyéb típusú bejegyzés a rekordhalmazok egynél több rekordot, és toomodify meglévő rekordokat, lásd: [kezelése DNS-rekordok és a rekordhalmazok az Azure PowerShell](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-130">For other record types, for record sets with more than one record, and toomodify existing records, see [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md).</span></span> 


## <a name="view-records"></a><span data-ttu-id="e57d3-131">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="e57d3-131">View records</span></span>

<span data-ttu-id="e57d3-132">toolist hello DNS-rekordokat, amelyek a zónához használja:</span><span class="sxs-lookup"><span data-stu-id="e57d3-132">toolist hello DNS records in your zone, use:</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a><span data-ttu-id="e57d3-133">A névkiszolgálók frissítése</span><span class="sxs-lookup"><span data-stu-id="e57d3-133">Update name servers</span></span>

<span data-ttu-id="e57d3-134">Ha mindent megfelelőnek talált, hogy a DNS-zóna és a rekordok beállított megfelelően, tooconfigure van szüksége a tartománynév toouse hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="e57d3-134">Once you are satisfied that your DNS zone and records have been set up correctly, you need tooconfigure your domain name toouse hello Azure DNS name servers.</span></span> <span data-ttu-id="e57d3-135">Ez lehetővé teszi a más felhasználóktól a hello Internet toofind a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="e57d3-135">This enables other users on hello Internet toofind your DNS records.</span></span>

<span data-ttu-id="e57d3-136">Adja meg a zóna névkiszolgálóit hello hello `Get-AzureRmDnsZone` parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="e57d3-136">hello name servers for your zone are given by hello `Get-AzureRmDnsZone` cmdlet:</span></span>

```powershell
Get-AzureRmDnsZone -ZoneName contoso.com -ResourceGroupName MyResourceGroup

Name                  : contoso.com
ResourceGroupName     : myresourcegroup
Etag                  : 00000003-0000-0000-b40d-0996b97ed101
Tags                  : {}
NameServers           : {ns1-01.azure-dns.com., ns2-01.azure-dns.net., ns3-01.azure-dns.org., ns4-01.azure-dns.info.}
NumberOfRecordSets    : 3
MaxNumberOfRecordSets : 5000
```

<span data-ttu-id="e57d3-137">Ezeket a kiszolgálókat (vásárolta hello tartománynevet) hello regisztrációs kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="e57d3-137">These name servers should be configured with hello domain name registrar (where you purchased hello domain name).</span></span> <span data-ttu-id="e57d3-138">A regisztráló felajánlja, hello beállítás tooset hello neve kiszolgáló hello tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="e57d3-138">Your registrar will offer hello option tooset up hello name servers for hello domain.</span></span> <span data-ttu-id="e57d3-139">További információkért lásd: [delegálása a tartományi tooAzure DNS](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-139">For more information, see [Delegate your domain tooAzure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="e57d3-140">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="e57d3-140">Delete all resources</span></span>

<span data-ttu-id="e57d3-141">toodelete ebben a cikkben a következő lépés hajtsa végre a megfelelő hello létrehozott összes erőforrás:</span><span class="sxs-lookup"><span data-stu-id="e57d3-141">toodelete all resources created in this article, take hello following step:</span></span>

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="e57d3-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e57d3-142">Next steps</span></span>

<span data-ttu-id="e57d3-143">További információ az Azure DNS-beli toolearn lásd [Azure DNS áttekintése](dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-143">toolearn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="e57d3-144">További információ az Azure DNS-, DNS-zónák kezelése toolearn lásd [kezelése DNS-zónák az Azure DNS PowerShell-lel](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-144">toolearn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md).</span></span>

<span data-ttu-id="e57d3-145">További információ az Azure DNS-, DNS-rekordok kezelése toolearn lásd [kezelése DNS-rekordok és a rekord beállítja az Azure DNS PowerShell-lel](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="e57d3-145">toolearn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md).</span></span>

