---
title: "Az Azure DNS PowerShell-lel való használatának első lépései | Microsoft Docs"
description: "A cikkből megtudhatja, hogyan hozhat létre DNS-zónát és -rekordot az Azure DNS-ben. Ez egy lépésenkénti útmutató, amellyel a PowerShell használatával létrehozhatja és kezelheti az első DNS-zónáját és -rekordját."
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
ms.openlocfilehash: 48f7ba325f61b4a91c0208b4c99058da801bee19
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-powershell"></a><span data-ttu-id="486d8-104">Az Azure DNS PowerShell-lel való használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="486d8-104">Get Started with Azure DNS using PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="486d8-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="486d8-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="486d8-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="486d8-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="486d8-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="486d8-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="486d8-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="486d8-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="486d8-109">Ez a cikk végigvezeti az első DNS-zóna és -rekord Azure PowerShell-lel való létrehozásának lépésein.</span><span class="sxs-lookup"><span data-stu-id="486d8-109">This article walks you through the steps to create your first DNS zone and record using Azure PowerShell.</span></span> <span data-ttu-id="486d8-110">Ezek a lépések az Azure Portal vagy a platformfüggetlen Azure CLI használatával is elvégezhetőek.</span><span class="sxs-lookup"><span data-stu-id="486d8-110">You can also perform these steps using the Azure portal or the cross-platform Azure CLI.</span></span>

<span data-ttu-id="486d8-111">Az egyes tartományokhoz tartozó DNS-rekordok üzemeltetése DNS-zónákban történik.</span><span class="sxs-lookup"><span data-stu-id="486d8-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="486d8-112">A tartománya Azure DNS-ben való üzemeltetésének megkezdéséhez létre kell hoznia egy DNS-zónát az adott tartománynévhez.</span><span class="sxs-lookup"><span data-stu-id="486d8-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="486d8-113">Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre.</span><span class="sxs-lookup"><span data-stu-id="486d8-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="486d8-114">Végül a DNS-zóna interneten való közzétételéhez konfigurálnia kell a tartomány névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="486d8-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="486d8-115">Az egyes lépéseket az alábbiakban ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="486d8-115">Each of these steps is described below.</span></span>

<span data-ttu-id="486d8-116">Ezek az utasítások feltételezik, hogy már telepítette és az Azure Powershellt, és bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="486d8-116">These instructions assume you have already installed and signed in to Azure PowerShell.</span></span> <span data-ttu-id="486d8-117">További segítségért lásd [a DNS-zónák a PowerShell használatával való kezelésével kapcsolatos](dns-operations-dnszones.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="486d8-117">For help, see [How to manage DNS zones using PowerShell](dns-operations-dnszones.md).</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="486d8-118">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="486d8-118">Create the resource group</span></span>

<span data-ttu-id="486d8-119">A DNS-zóna létrehozása előtt egy erőforráscsoportot kell létrehozni, amely a DNS-zónát tartalmazza majd.</span><span class="sxs-lookup"><span data-stu-id="486d8-119">Before creating the DNS zone, a resource group is created to contain the DNS Zone.</span></span> <span data-ttu-id="486d8-120">Az alábbiakban a parancs látható.</span><span class="sxs-lookup"><span data-stu-id="486d8-120">The following shows the command.</span></span>

```powershell
New-AzureRMResourceGroup -name MyResourceGroup -location "westus"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="486d8-121">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="486d8-121">Create a DNS zone</span></span>

<span data-ttu-id="486d8-122">A DNS-zóna az `New-AzureRmDnsZone` parancsmag használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="486d8-122">A DNS zone is created by using the `New-AzureRmDnsZone` cmdlet.</span></span> <span data-ttu-id="486d8-123">Az alábbi példaparancs a *MyResourceGroup* erőforráscsoportban létrehozza a *contoso.com* DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="486d8-123">The following example creates a DNS zone called *contoso.com* in the resource group called *MyResourceGroup*.</span></span> <span data-ttu-id="486d8-124">A példát követve, és az értékeket a sajátjaira cserélve hozza létre a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="486d8-124">Use the example to create a DNS zone, substituting the values for your own.</span></span>

```powershell
New-AzureRmDnsZone -Name contoso.com -ResourceGroupName MyResourceGroup
```

## <a name="create-a-dns-record"></a><span data-ttu-id="486d8-125">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="486d8-125">Create a DNS record</span></span>

<span data-ttu-id="486d8-126">Rekordhalmazt a `New-AzureRmDnsRecordSet` parancsmag használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="486d8-126">You create record sets by using the `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="486d8-127">Az alábbi példa a „MyResourceGroup” erőforráscsoport „contoso.com” DNS-zónájában egy „www” relatív nevű rekordot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="486d8-127">The following example creates a record with the relative name "www" in the DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="486d8-128">A beállított rekord teljes neve „www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="486d8-128">The fully-qualified name of the record set is "www.contoso.com".</span></span> <span data-ttu-id="486d8-129">A rekord típusa „A”, az IP-címe „1.2.3.4”, az élettartama pedig 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="486d8-129">The record type is "A", with IP address "1.2.3.4", and the TTL is 3600 seconds.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName contoso.com -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4")
```

<span data-ttu-id="486d8-130">Más rekordtípusok, több rekordot tartalmazó rekordhalmazok és meglévő rekordok módosítása esetén lásd: [DNS-rekordok és -rekordhalmazok kezelése az Azure PowerShell használatával](dns-operations-recordsets.md).</span><span class="sxs-lookup"><span data-stu-id="486d8-130">For other record types, for record sets with more than one record, and to modify existing records, see [Manage DNS records and record sets using Azure PowerShell](dns-operations-recordsets.md).</span></span> 


## <a name="view-records"></a><span data-ttu-id="486d8-131">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="486d8-131">View records</span></span>

<span data-ttu-id="486d8-132">A zónájában lévő DNS-rekordokat a következő paranccsal listázhatja:</span><span class="sxs-lookup"><span data-stu-id="486d8-132">To list the DNS records in your zone, use:</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName contoso.com -ResourceGroupName MyResourceGroup
```


## <a name="update-name-servers"></a><span data-ttu-id="486d8-133">A névkiszolgálók frissítése</span><span class="sxs-lookup"><span data-stu-id="486d8-133">Update name servers</span></span>

<span data-ttu-id="486d8-134">Ha a DNS-zóna és -rekordok megfelelően be lettek állítva, konfigurálnia kell a tartománynevet az Azure DNS-névkiszolgálók használatára.</span><span class="sxs-lookup"><span data-stu-id="486d8-134">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="486d8-135">Így más internetes felhasználók megkereshetik a DNS-rekordjait.</span><span class="sxs-lookup"><span data-stu-id="486d8-135">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="486d8-136">A zóna névkiszolgálói az `Get-AzureRmDnsZone` parancsmaggal vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="486d8-136">The name servers for your zone are given by the `Get-AzureRmDnsZone` cmdlet:</span></span>

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

<span data-ttu-id="486d8-137">Ezeket a névkiszolgálókat a tartományregisztrálóhoz kell konfigurálni (ahol a tartománynevet vásárolta).</span><span class="sxs-lookup"><span data-stu-id="486d8-137">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="486d8-138">A regisztráló felajánlja, hogy beállítja a névkiszolgálókat a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="486d8-138">Your registrar will offer the option to set up the name servers for the domain.</span></span> <span data-ttu-id="486d8-139">További információért lásd: [Tartomány delegálása az Azure DNS-be](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="486d8-139">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="486d8-140">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="486d8-140">Delete all resources</span></span>

<span data-ttu-id="486d8-141">A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépést:</span><span class="sxs-lookup"><span data-stu-id="486d8-141">To delete all resources created in this article, take the following step:</span></span>

```powershell
Remove-AzureRMResourceGroup -Name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="486d8-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="486d8-142">Next steps</span></span>

<span data-ttu-id="486d8-143">Az Azure DNS-sel kapcsolatos további információért lásd [az Azure DNS áttekintését biztosító](dns-overview.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="486d8-143">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="486d8-144">DNS-zónák az Azure DNS-ben való kezelésével kapcsolatos további információért lásd [a DNS-zónák az Azure DNS-ben a PowerShell-lel való kezelésével kapcsolatos](dns-operations-dnszones.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="486d8-144">To learn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using PowerShell](dns-operations-dnszones.md).</span></span>

<span data-ttu-id="486d8-145">DNS-rekordok az Azure DNS-ben való kezelésével kapcsolatos további információért lásd [a DNS-rekordok és -rekordhalmazok az Azure DNS-ben a PowerShell-lel való kezelésével kapcsolatos](dns-operations-recordsets.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="486d8-145">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using PowerShell](dns-operations-recordsets.md).</span></span>

