---
title: "Üzemeltetési névkeresési DNS zónák az Azure DNS |} Microsoft Docs"
description: "A névkeresési DNS-címkeresési zónák az IP-címtartományok tárolásához Azure DNS használata"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: 3e10b25d2f9b91c96af2958fef6dc6a4fdbff301
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="50314-103">Az Azure DNS-névkeresési DNS-címkeresési zónák üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="50314-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="50314-104">Ez a cikk azt ismerteti, hogyan a hozzárendelt IP-címtartományokhoz az Azure DNS-névkeresési DNS-címkeresési zónák üzemeltetésére.</span><span class="sxs-lookup"><span data-stu-id="50314-104">This article explains how to host the reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="50314-105">Az IP-címtartományai a névkeresési zóna által képviselt kell rendelni a szervezetben, általában az Internetszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="50314-105">The IP ranges represented by the reverse lookup zone must be assigned to your organization, typically by your ISP.</span></span>

<span data-ttu-id="50314-106">Az Azure service rendelt Azure tulajdonában lévő IP-cím címfeloldási DNS konfigurálásával kapcsolatban lásd: [konfigurálása az Azure service lefoglalt IP-címek névkeresési](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="50314-106">To configure reverse DNS for Azure-owned IP address assigned to your Azure service, see [configure the reverse lookup for the IP addresses allocated to your Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="50314-107">A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="50314-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="50314-108">Ez a cikk végigvezeti a az első névkeresési DNS-zóna létrehozása, és jegyezze fel az Azure portál, Azure PowerShell, az Azure CLI 1.0 vagy az Azure CLI 2.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="50314-108">This article walks you through the steps to create your first reverse lookup DNS zone and record using the Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="50314-109">A névkeresési DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="50314-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="50314-110">Jelentkezzen be a [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="50314-110">Sign in to the [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="50314-111">A központ menüben kattintson, majd **új** > **hálózati** >, majd **DNS-zóna** megnyitásához a **hozzon létre DNS-zóna** panelen.</span><span class="sxs-lookup"><span data-stu-id="50314-111">On the Hub menu, click and click **New** > **Networking** > and then click **DNS zone** to open the **Create DNS zone** blade.</span></span>

   ![DNS-zóna](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="50314-113">Az a **hozzon létre DNS-zóna** panelen, a DNS-zóna neve.</span><span class="sxs-lookup"><span data-stu-id="50314-113">On the **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="50314-114">A zóna nevét kialakított az IPv4 és IPv6-előtagok másképp van.</span><span class="sxs-lookup"><span data-stu-id="50314-114">The name of the zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="50314-115">Vagy az utasításokat a [IPV4](#ipv4) vagy [IPv6](#ipv6) a zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="50314-115">Use either the instructions for [IPV4](#ipv4) or [IPv6](#ipv6) to name your zone.</span></span> <span data-ttu-id="50314-116">Ha végzett a kattintson **létrehozása** a zóna létrehozása.</span><span class="sxs-lookup"><span data-stu-id="50314-116">When complete click **Create** to create the zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="50314-117">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="50314-117">IPv4</span></span>

<span data-ttu-id="50314-118">Egy IPv4 névkeresési zóna neve az IP-címtartományt, amely alapul.</span><span class="sxs-lookup"><span data-stu-id="50314-118">The name of an IPv4 reverse lookup zone is based on the IP range it represents.</span></span> <span data-ttu-id="50314-119">A következő formátumúnak kell lennie: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="50314-119">It should be in the following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="50314-120">Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="50314-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="50314-121">Az Azure DNS-classless névkeresési DNS-címkeresési zónák létrehozásakor használnia kell a kötőjel (`-`) helyett egy perjel ("/") a zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="50314-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in the zone name.</span></span>
>
> <span data-ttu-id="50314-122">Például az IP-címtartomány 192.0.2.128/26 kell használnia `128-26.2.0.192.in-addr.arpa` ahelyett, hogy a zóna neveként `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="50314-122">For example, for the IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as the zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="50314-123">Ennek az az oka, mind támogatottak, a DNS-szabványokban, DNS-zónák neve a perjel (`/`) karakter használata nem támogatott Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="50314-123">This is because, while both are supported by the DNS standards, DNS zone names containing the forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="50314-124">A következő példa bemutatja, hogyan hozzon létre egy C osztályú címfeloldási DNS-zóna nevű `2.0.192.in-addr.arpa` az Azure DNS az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="50314-124">The following example shows how to create a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via the Azure portal:</span></span>

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="50314-126">Az "erőforráscsoport helye" határozza meg az erőforrásnak a helyét, és nincs hatással van a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="50314-126">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="50314-127">A DNS-zóna helye mindig "global", és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="50314-127">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="50314-128">Az alábbi példák bemutatják, hogyan lehet elvégezni ezt a feladatot az Azure PowerShell és az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="50314-128">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="50314-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="50314-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="50314-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="50314-132">IPv6</span></span>

<span data-ttu-id="50314-133">Egy IPv6-névkeresési zóna neve a következő formátumban kell lennie: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="50314-133">The name of an IPv6 reverse lookup zone should be in the following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="50314-134">Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="50314-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="50314-135">A következő példa bemutatja, hogyan hozzon létre egy IPv6 névkeresési DNS-zóna nevű `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` az Azure DNS az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="50314-135">The following example shows how to create an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via the Azure portal:</span></span>

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="50314-137">Az "erőforráscsoport helye" határozza meg az erőforrásnak a helyét, és nincs hatással van a DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="50314-137">The 'Resource group location' defines the location for the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="50314-138">A DNS-zóna helye mindig "global", és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="50314-138">The DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="50314-139">Az alábbi példák bemutatják, hogyan lehet elvégezni ezt a feladatot az Azure PowerShell és az Azure parancssori felület:</span><span class="sxs-lookup"><span data-stu-id="50314-139">The following examples show how to complete this task with Azure PowerShell and the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="50314-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="50314-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="50314-143">A címfeloldási DNS-zóna delegálása</span><span class="sxs-lookup"><span data-stu-id="50314-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="50314-144">A címfeloldási DNS-zóna hozunk létre, győződjön meg arról, hogy a zóna van átadva a szülőzónában.</span><span class="sxs-lookup"><span data-stu-id="50314-144">Having created your reverse DNS lookup zone, you must ensure that the zone is delegated from the parent zone.</span></span> <span data-ttu-id="50314-145">DNS-delegálás lehetővé teszi, hogy a DNS-név feloldási folyamat keresése a névkeresési DNS-címkeresési zónát üzemeltető névkiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="50314-145">DNS delegation enables the DNS name resolution process to find the name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="50314-146">Ez lehetővé teszi, hogy ezek névkiszolgálók megválaszolni a DNS-névkeresési lekérdezések a címtartomány IP-címek.</span><span class="sxs-lookup"><span data-stu-id="50314-146">This enables those name servers to answer DNS reverse queries for the IP addresses in your address range.</span></span>

<span data-ttu-id="50314-147">Címkeresési zónák, DNS-zóna delegálása folyamata ismertetett [tartomány delegálása az Azure DNS-](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="50314-147">For forward lookup zones, the process of delegating a DNS zone is described in [Delegate your domain to Azure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="50314-148">A delegálás névkeresési zónák ugyanúgy működik.</span><span class="sxs-lookup"><span data-stu-id="50314-148">Delegation for reverse lookup zones works the same way.</span></span> <span data-ttu-id="50314-149">Az egyetlen különbség, hogy szeretné-e a Névkiszolgálók állítson be a tartományregisztrálójához neve helyett az IP-címtartományt biztosító Internetszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="50314-149">The only difference is that you need to configure the name servers with the ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="50314-150">DNS PTR-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="50314-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="50314-151">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="50314-151">IPv4</span></span>

<span data-ttu-id="50314-152">A következő példa bemutatja, hogyan PTR típusú rekord létrehozása az Azure DNS-névkeresési DNS-zóna folyamatán.</span><span class="sxs-lookup"><span data-stu-id="50314-152">The following example walks you through the process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="50314-153">Más rekordtípusok és meglévő rekordok módosítása esetén lásd [a DNS-rekordok és -rekordhalmazok az Azure Portallal való kezelésével kapcsolatos](dns-operations-recordsets-portal.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="50314-153">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="50314-154">A **DNS-zóna** panel tetején válassza a **+ Rekordhalmaz** elemet a **Rekordhalmaz hozzáadása** panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="50314-154">At the top of the **DNS zone** blade, select **+ Record set** to open the **Add record set** blade.</span></span>

 ![DNS-zóna](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="50314-156">Az a **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="50314-156">On the **Add record set** blade.</span></span> 
1. <span data-ttu-id="50314-157">Válassza ki **PTR** a rekordból "**típus**" menü.</span><span class="sxs-lookup"><span data-stu-id="50314-157">Select **PTR** from the record "**Type**" menu.</span></span>  
1. <span data-ttu-id="50314-158">A PTR típusú rekord rekordhalmaz nevét kell lennie a IPv4-címét a többi fordított sorrendben.</span><span class="sxs-lookup"><span data-stu-id="50314-158">The name of the record set for a PTR record needs to be the rest of the IPv4 address in reverse order.</span></span> <span data-ttu-id="50314-159">Ebben a példában az első három oktettjének már fel vannak töltve a zóna nevét (.2.0.192) részeként.</span><span class="sxs-lookup"><span data-stu-id="50314-159">In this example, the first three octets are already populated as part of the zone name (.2.0.192).</span></span> <span data-ttu-id="50314-160">Ezért csak az utolsó oktett van megadva a neve mezőben.</span><span class="sxs-lookup"><span data-stu-id="50314-160">Therefore, only the last octet is supplied in the name field.</span></span> <span data-ttu-id="50314-161">Például nevezze el az volt a rekordhalmaz "**15**" amelynek IP-cím 192.0.2.15 erőforrás.</span><span class="sxs-lookup"><span data-stu-id="50314-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="50314-162">Az a "**tartománynév**" mezőbe írja be a teljesen minősített tartománynevét (FQDN) az erőforrás, az IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="50314-162">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
1. <span data-ttu-id="50314-163">Válassza a panel alján található **OK** gombot a DNS-rekord létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="50314-163">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

 ![rekordhalmaz hozzáadása](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="50314-165">A következő példák hogyan befejezheti a feladatot a PowerShell és a AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="50314-165">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="50314-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="50314-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="50314-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="50314-169">IPv6</span></span>

<span data-ttu-id="50314-170">A következő példa bemutatja, hogyan új "PTR" rekord létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="50314-170">The following example walks you through the process of creating new 'PTR' record.</span></span> <span data-ttu-id="50314-171">Más rekordtípusok és meglévő rekordok módosítása esetén lásd [a DNS-rekordok és -rekordhalmazok az Azure Portallal való kezelésével kapcsolatos](dns-operations-recordsets-portal.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="50314-171">For other record types and to modify existing records, see [Manage DNS records and record sets by using the Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="50314-172">Felső részén a **DNS-zóna panel**, jelölje be **+ rekordhalmaz** megnyitásához a **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="50314-172">At the top of the **DNS zone blade**, select **+ Record set** to open the **Add record set** blade.</span></span>

  ![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="50314-174">Az a **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="50314-174">On the **Add record set** blade.</span></span> 
3. <span data-ttu-id="50314-175">Válassza ki **PTR** a rekordból "**típus**" menü.</span><span class="sxs-lookup"><span data-stu-id="50314-175">Select **PTR** from the record "**Type**" menu.</span></span>  
4. <span data-ttu-id="50314-176">A PTR típusú rekord rekordhalmaz nevét kell lennie a IPv6-címet a többi fordított sorrendben.</span><span class="sxs-lookup"><span data-stu-id="50314-176">The name of the record set for a PTR record needs to be the rest of the IPv6 address in reverse order.</span></span> <span data-ttu-id="50314-177">Nem minden nulla tömörítési tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="50314-177">It must not include any zero compression.</span></span> <span data-ttu-id="50314-178">Ebben a példában az első 64 bit, az IPv6-már fel vannak töltve a zóna nevét (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa) részeként.</span><span class="sxs-lookup"><span data-stu-id="50314-178">In this example, the first 64 bits of the IPv6 are already populated as part of the zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="50314-179">Ezért csak az utolsó 64 bites megadva, a név mezőben.</span><span class="sxs-lookup"><span data-stu-id="50314-179">Therefore, only the last 64 bits are supplied in the name field.</span></span> <span data-ttu-id="50314-180">Az IP-cím utolsó 64 bites időszak használja, mint az elválasztó mindegyik hexadecimális szám között fordított sorrendben kerülnek.</span><span class="sxs-lookup"><span data-stu-id="50314-180">The last 64 bits of the IP address are entered in reverse order, using a period as the delimiter between each hexadecimal number.</span></span> <span data-ttu-id="50314-181">Például nevezze el az volt a rekordhalmaz "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" amelynek IP-cím 2001:0db8:abdc:0000:f524:10bc:1af9:405e erőforrás.</span><span class="sxs-lookup"><span data-stu-id="50314-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="50314-182">Az a "**tartománynév**" mezőbe írja be a teljesen minősített tartománynevét (FQDN) az erőforrás, az IP-cím használatával.</span><span class="sxs-lookup"><span data-stu-id="50314-182">In the "**Domain Name**" field, enter the fully qualified domain name (FQDN) of the resource using the IP.</span></span>
6. <span data-ttu-id="50314-183">Válassza a panel alján található **OK** gombot a DNS-rekord létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="50314-183">Select **OK** at the bottom of the blade to create the DNS record.</span></span>

![Adja hozzá a rekordhalmaz panel](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="50314-185">A következő példák hogyan befejezheti a feladatot a PowerShell és a AzureCLI:</span><span class="sxs-lookup"><span data-stu-id="50314-185">The following are examples on how to complete this task with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="50314-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="50314-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="50314-189">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="50314-189">View Records</span></span>

<span data-ttu-id="50314-190">A létrehozott rekordok megtekintéséhez keresse meg a DNS-zónát az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="50314-190">To view the records you created, navigate to your DNS zone in the Azure portal.</span></span> <span data-ttu-id="50314-191">Az alsó részén a **DNS-zóna** panelen láthatja, hogy a rekordokat a DNS-zónához.</span><span class="sxs-lookup"><span data-stu-id="50314-191">In the lower part of the **DNS zone** blade, you can see the records for the DNS zone.</span></span> <span data-ttu-id="50314-192">Meg kell jelennie az alapértelmezett NS és SOA típusú rekordoknak, amelyek minden zónában létrejönnek, valamint az összes új létrehozott rekordnak.</span><span class="sxs-lookup"><span data-stu-id="50314-192">You should see the default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="50314-193">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="50314-193">IPv4</span></span>

<span data-ttu-id="50314-194">DNS zóna panelen IPv4 PTR-rekordok:</span><span class="sxs-lookup"><span data-stu-id="50314-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="50314-196">A következő példák szemléltetik a PowerShell vagy az Azure parancssori felület használatával PTR-rekordok megtekintése:</span><span class="sxs-lookup"><span data-stu-id="50314-196">The following examples show how to view the PTR records using PowerShell or the Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="50314-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="50314-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="50314-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="50314-200">IPv6</span></span>

<span data-ttu-id="50314-201">DNS zóna panelen IPv6 PTR-rekordok:</span><span class="sxs-lookup"><span data-stu-id="50314-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="50314-203">A következő példák a PowerShell és a AzureCLI a rekordok megtekintése:</span><span class="sxs-lookup"><span data-stu-id="50314-203">The following are examples on how to view the records with PowerShell and the AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="50314-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="50314-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="50314-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="50314-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="50314-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="50314-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="50314-207">GYIK</span><span class="sxs-lookup"><span data-stu-id="50314-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="50314-208">Képes-e üzemeltetni névkeresési DNS-címkeresési zónák az Internetszolgáltató által hozzárendelt IP-címblokkok az Azure DNS szolgáltatásra?</span><span class="sxs-lookup"><span data-stu-id="50314-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="50314-209">Igen.</span><span class="sxs-lookup"><span data-stu-id="50314-209">Yes.</span></span> <span data-ttu-id="50314-210">A névkeresési (ARPA) a saját IP-címtartományok, az Azure DNS-zónákat üzemeltető teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="50314-210">Hosting the reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="50314-211">A névkeresési zóna létrehozása az Azure DNS-amint ez a cikk azt, akkor az Internetszolgáltatótól működik [delegálása a zóna](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="50314-211">Create the reverse lookup zone in Azure DNS as explained in this article, then work with your ISP to [delegate the zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="50314-212">A PTR rekordok minden névkeresési ugyanúgy, mint más rekordtípusokhoz majd felügyelheti.</span><span class="sxs-lookup"><span data-stu-id="50314-212">You can then manage the PTR records for each reverse lookup in the same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="50314-213">A fordított DNS keresési zónához költségének üzemeltető mennyi használ?</span><span class="sxs-lookup"><span data-stu-id="50314-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="50314-214">A címfeloldási DNS-zóna üzemeltető, az az Internetszolgáltató által hozzárendelt IP-Címblokk az Azure DNS-rendszer díjakon [Azure DNS normál díjszabás](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="50314-214">Hosting the reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="50314-215">IPv4 és IPv6 címek az Azure DNS-névkeresési DNS-címkeresési zónák is futtatni?</span><span class="sxs-lookup"><span data-stu-id="50314-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="50314-216">Igen.</span><span class="sxs-lookup"><span data-stu-id="50314-216">Yes.</span></span> <span data-ttu-id="50314-217">Ez a cikk ismerteti az IPv4 és IPv6 névkeresési DNS-címkeresési zónák létrehozása az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="50314-217">This article explains how to create both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="50314-218">Beimportálhatok egy meglévő címfeloldási DNS-zóna?</span><span class="sxs-lookup"><span data-stu-id="50314-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="50314-219">Igen.</span><span class="sxs-lookup"><span data-stu-id="50314-219">Yes.</span></span> <span data-ttu-id="50314-220">Az Azure CLI segítségével importálja a meglévő DNS-zónákat az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="50314-220">You can use the Azure CLI to import existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="50314-221">Ez a módszer a címkeresési zónák és a névkeresési zóna listáját.</span><span class="sxs-lookup"><span data-stu-id="50314-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="50314-222">További információkért lásd: [importálása és exportálása a DNS-zónafájlját az Azure parancssori felület használatával](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="50314-222">For more information, see [Import and export a DNS zone file using the Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="50314-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="50314-223">Next steps</span></span>

<span data-ttu-id="50314-224">A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="50314-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="50314-225">Megtudhatja, hogyan [címfeloldási DNS-rekordjait az Azure-szolgáltatások kezelése](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="50314-225">Learn how to [manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
