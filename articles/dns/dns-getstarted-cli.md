---
title: "Az Azure DNS használatának első lépései az Azure CLI 2.0-val | Microsoft Docs"
description: "A cikkből megtudhatja, hogyan hozhat létre DNS-zónát és -rekordot az Azure DNS-ben. Ez egy lépésenkénti útmutató, amellyel az Azure CLI 2.0 használatával létrehozhatja és kezelheti az első DNS-zónáját és -rekordját."
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: fb0aa0a6-d096-4d6a-b2f6-eda1c64f6182
ms.service: dns
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2017
ms.author: jonatul
ms.openlocfilehash: 6958d61b29961f59cb22f62bec55f2d467e7e7cb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-dns-using-azure-cli-20"></a><span data-ttu-id="26e4e-104">Az Azure DNS használatának első lépései az Azure CLI 2.0-val</span><span class="sxs-lookup"><span data-stu-id="26e4e-104">Get started with Azure DNS using Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="26e4e-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="26e4e-105">Azure portal</span></span>](dns-getstarted-portal.md)
> * [<span data-ttu-id="26e4e-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="26e4e-106">PowerShell</span></span>](dns-getstarted-powershell.md)
> * [<span data-ttu-id="26e4e-107">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="26e4e-107">Azure CLI 1.0</span></span>](dns-getstarted-cli-nodejs.md)
> * [<span data-ttu-id="26e4e-108">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="26e4e-108">Azure CLI 2.0</span></span>](dns-getstarted-cli.md)

<span data-ttu-id="26e4e-109">Ez a cikk bemutatja az első DNS-zóna és -rekord létrehozásának lépéseit a platformfüggetlen Azure CLI 2.0 használatával, amely Windows, Mac és Linux platformokon is elérhető.</span><span class="sxs-lookup"><span data-stu-id="26e4e-109">This article walks you through the steps to create your first DNS zone and record using the cross-platform Azure CLI 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="26e4e-110">Ezeket a lépéseket az Azure Portal vagy az Azure PowerShell használatával is elvégezheti.</span><span class="sxs-lookup"><span data-stu-id="26e4e-110">You can also perform these steps using the Azure portal or Azure PowerShell.</span></span>

<span data-ttu-id="26e4e-111">Az egyes tartományokhoz tartozó DNS-rekordok üzemeltetése DNS-zónákban történik.</span><span class="sxs-lookup"><span data-stu-id="26e4e-111">A DNS zone is used to host the DNS records for a particular domain.</span></span> <span data-ttu-id="26e4e-112">A tartománya Azure DNS-ben való üzemeltetésének megkezdéséhez létre kell hoznia egy DNS-zónát az adott tartománynévhez.</span><span class="sxs-lookup"><span data-stu-id="26e4e-112">To start hosting your domain in Azure DNS, you need to create a DNS zone for that domain name.</span></span> <span data-ttu-id="26e4e-113">Ezután a tartománya összes DNS-rekordja ebben a DNS-zónában jön létre.</span><span class="sxs-lookup"><span data-stu-id="26e4e-113">Each DNS record for your domain is then created inside this DNS zone.</span></span> <span data-ttu-id="26e4e-114">Végül a DNS-zóna interneten való közzétételéhez konfigurálnia kell a tartomány névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="26e4e-114">Finally, to publish your DNS zone to the Internet, you need to configure the name servers for the domain.</span></span> <span data-ttu-id="26e4e-115">Az egyes lépéseket az alábbiakban ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="26e4e-115">Each of these steps is described below.</span></span>

<span data-ttu-id="26e4e-116">Ezek az utasítások feltételezik, hogy már telepítette az Azure CLI 2.0-t, és bejelentkezett.</span><span class="sxs-lookup"><span data-stu-id="26e4e-116">These instructions assume you have already installed and signed in to Azure CLI 2.0.</span></span> <span data-ttu-id="26e4e-117">További segítségért lásd [a DNS-zónák az Azure CLI 2.0 használatával való kezelésével kapcsolatos](dns-operations-dnszones-cli.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="26e4e-117">For help, see [How to manage DNS zones using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

## <a name="create-the-resource-group"></a><span data-ttu-id="26e4e-118">Az erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="26e4e-118">Create the resource group</span></span>

<span data-ttu-id="26e4e-119">A DNS-zóna létrehozása előtt egy erőforráscsoportot kell létrehozni, amely a DNS-zónát tartalmazza majd.</span><span class="sxs-lookup"><span data-stu-id="26e4e-119">Before creating the DNS zone, a resource group is created to contain the DNS Zone.</span></span> <span data-ttu-id="26e4e-120">Az alábbiakban a parancs látható.</span><span class="sxs-lookup"><span data-stu-id="26e4e-120">The following shows the command.</span></span>

```azurecli
az group create --name MyResourceGroup --location "West US"
```

## <a name="create-a-dns-zone"></a><span data-ttu-id="26e4e-121">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="26e4e-121">Create a DNS zone</span></span>

<span data-ttu-id="26e4e-122">A DNS-zóna az `az network dns zone create` parancs használatával hozható létre.</span><span class="sxs-lookup"><span data-stu-id="26e4e-122">A DNS zone is created using the `az network dns zone create` command.</span></span> <span data-ttu-id="26e4e-123">A paranccsal kapcsolatos súgó megtekintéséhez írja be a következőt: `az network dns zone create -h`.</span><span class="sxs-lookup"><span data-stu-id="26e4e-123">To see help for this command, type `az network dns zone create -h`.</span></span>

<span data-ttu-id="26e4e-124">Az alábbi példaparancs a *MyResourceGroup* nevű erőforráscsoportban létrehozza a *contoso.com* DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="26e4e-124">The following example creates a DNS zone called *contoso.com* in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="26e4e-125">A példát követve, és az értékeket a sajátjaira cserélve hozza létre a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="26e4e-125">Use the example to create a DNS zone, substituting the values for your own.</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n contoso.com
```


## <a name="create-a-dns-record"></a><span data-ttu-id="26e4e-126">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="26e4e-126">Create a DNS record</span></span>

<span data-ttu-id="26e4e-127">DNS-rekordokat az `az network dns record-set [record type] add-record` paranccsal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="26e4e-127">To create a DNS record, use the `az network dns record-set [record type] add-record` command.</span></span> <span data-ttu-id="26e4e-128">Az A-rekordokkal kapcsolatos segítségért például lásd: `azure network dns record-set A add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="26e4e-128">For help, for A records for example, see `azure network dns record-set A add-record -h`.</span></span>

<span data-ttu-id="26e4e-129">Az alábbi példa a „MyResourceGroup” erőforráscsoport „contoso.com” DNS-zónájában egy „www” relatív nevű rekordot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="26e4e-129">The following example creates a record with the relative name "www" in the DNS Zone "contoso.com", in resource group "MyResourceGroup".</span></span> <span data-ttu-id="26e4e-130">A beállított rekord teljes neve „www.contoso.com”.</span><span class="sxs-lookup"><span data-stu-id="26e4e-130">The fully-qualified name of the record set is "www.contoso.com".</span></span> <span data-ttu-id="26e4e-131">A rekord típusa „A”, az IP-címe „1.2.3.4”, és a rendszer a 3600 másodperces (1 órás) alapértelmezett élettartamot használja.</span><span class="sxs-lookup"><span data-stu-id="26e4e-131">The record type is "A", with IP address "1.2.3.4", and a default TTL of 3600 seconds (1 hour) is used.</span></span>

```azurecli
az network dns record-set a add-record -g MyResourceGroup -z contoso.com -n www -a 1.2.3.4
```

<span data-ttu-id="26e4e-132">Más rekordtípusok, több rekordot tartalmazó rekordhalmazok, alternatív élettartam-értékek és meglévő rekordok módosítása esetén lásd: [DNS-rekordok és -rekordhalmazok kezelése az Azure CLI 2.0 használatával](dns-operations-recordsets-cli.md).</span><span class="sxs-lookup"><span data-stu-id="26e4e-132">For other record types, for record sets with more than one record, for alternative TTL values, and to modify existing records, see [Manage DNS records and record sets using the Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>


## <a name="view-records"></a><span data-ttu-id="26e4e-133">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="26e4e-133">View records</span></span>

<span data-ttu-id="26e4e-134">A zónájában lévő DNS-rekordokat a következő paranccsal listázhatja:</span><span class="sxs-lookup"><span data-stu-id="26e4e-134">To list the DNS records in your zone, use:</span></span>

```azurecli
az network dns record-set list -g MyResourceGroup -z contoso.com
```


## <a name="update-name-servers"></a><span data-ttu-id="26e4e-135">A névkiszolgálók frissítése</span><span class="sxs-lookup"><span data-stu-id="26e4e-135">Update name servers</span></span>

<span data-ttu-id="26e4e-136">Ha a DNS-zóna és -rekordok megfelelően be lettek állítva, konfigurálnia kell a tartománynevet az Azure DNS-névkiszolgálók használatára.</span><span class="sxs-lookup"><span data-stu-id="26e4e-136">Once you are satisfied that your DNS zone and records have been set up correctly, you need to configure your domain name to use the Azure DNS name servers.</span></span> <span data-ttu-id="26e4e-137">Így más internetes felhasználók megkereshetik a DNS-rekordjait.</span><span class="sxs-lookup"><span data-stu-id="26e4e-137">This enables other users on the Internet to find your DNS records.</span></span>

<span data-ttu-id="26e4e-138">A zóna névkiszolgálói az `az network dns zone show` paranccsal vannak megadva.</span><span class="sxs-lookup"><span data-stu-id="26e4e-138">The name servers for your zone are given by the `az network dns zone show` command.</span></span> <span data-ttu-id="26e4e-139">A névkiszolgáló nevek megtekintéséhez használjon JSON-kimenetet az alábbi példában látható módon.</span><span class="sxs-lookup"><span data-stu-id="26e4e-139">To see the name server names, use JSON output, as shown in the following example.</span></span>

```azurecli
az network dns zone show -g MyResourceGroup -n contoso.com -o json

{
  "etag": "00000003-0000-0000-b40d-0996b97ed101",
  "id": "/subscriptions/a385a691-bd93-41b0-8084-8213ebc5bff7/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-01.azure-dns.com.",
    "ns2-01.azure-dns.net.",
    "ns3-01.azure-dns.org.",
    "ns4-01.azure-dns.info."
  ],
  "numberOfRecordSets": 3,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="26e4e-140">Ezeket a névkiszolgálókat a tartományregisztrálóhoz kell konfigurálni (ahol a tartománynevet vásárolta).</span><span class="sxs-lookup"><span data-stu-id="26e4e-140">These name servers should be configured with the domain name registrar (where you purchased the domain name).</span></span> <span data-ttu-id="26e4e-141">A regisztráló felajánlja, hogy beállítja a névkiszolgálókat a tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="26e4e-141">Your registrar will offer the option to set up the name servers for the domain.</span></span> <span data-ttu-id="26e4e-142">További információért lásd: [Tartomány delegálása az Azure DNS-be](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="26e4e-142">For more information, see [Delegate your domain to Azure DNS](dns-domain-delegation.md).</span></span>

## <a name="delete-all-resources"></a><span data-ttu-id="26e4e-143">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="26e4e-143">Delete all resources</span></span>
 
<span data-ttu-id="26e4e-144">A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépést:</span><span class="sxs-lookup"><span data-stu-id="26e4e-144">To delete all resources created in this article, take the following step:</span></span>

```azurecli
az group delete --name MyResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="26e4e-145">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="26e4e-145">Next steps</span></span>

<span data-ttu-id="26e4e-146">Az Azure DNS-sel kapcsolatos további információért lásd [az Azure DNS áttekintését biztosító](dns-overview.md) cikket.</span><span class="sxs-lookup"><span data-stu-id="26e4e-146">To learn more about Azure DNS, see [Azure DNS overview](dns-overview.md).</span></span>

<span data-ttu-id="26e4e-147">DNS-zónák az Azure DNS-ben való kezelésével kapcsolatos további információért lásd [a DNS-zónák az Azure DNS-ben az Azure CLI 2.0-val való kezelésével kapcsolatos](dns-operations-dnszones-cli.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="26e4e-147">To learn more about managing DNS zones in Azure DNS, see [Manage DNS zones in Azure DNS using Azure CLI 2.0](dns-operations-dnszones-cli.md).</span></span>

<span data-ttu-id="26e4e-148">DNS-rekordok az Azure DNS-ben való kezelésével kapcsolatos további információért lásd [a DNS-rekordok és -rekordhalmazok az Azure DNS-ben az Azure CLI 2.0-val való kezelésével kapcsolatos](dns-operations-recordsets-cli.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="26e4e-148">To learn more about managing DNS records in Azure DNS, see [Manage DNS records and record sets in Azure DNS using Azure CLI 2.0](dns-operations-recordsets-cli.md).</span></span>
