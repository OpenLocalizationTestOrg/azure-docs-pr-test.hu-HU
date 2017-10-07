---
title: "aaaHosting névkeresési DNS zónák az Azure DNS |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse Azure DNS toohost hello névkeresési DNS-címkeresési zónák az IP-címtartományokhoz"
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
ms.openlocfilehash: 24feb8ef1c75a7d91938867f348fed1190046e4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="hosting-reverse-dns-lookup-zones-in-azure-dns"></a><span data-ttu-id="8fd99-103">Az Azure DNS-névkeresési DNS-címkeresési zónák üzemeltetéséhez</span><span class="sxs-lookup"><span data-stu-id="8fd99-103">Hosting reverse DNS lookup zones in Azure DNS</span></span>

<span data-ttu-id="8fd99-104">Ez a cikk azt ismerteti, hogyan toohost hello névkeresési DNS-keresési a hozzárendelt IP-címtartományokhoz az Azure DNS-zónák.</span><span class="sxs-lookup"><span data-stu-id="8fd99-104">This article explains how toohost hello reverse DNS lookup zones for your assigned IP ranges in Azure DNS.</span></span> <span data-ttu-id="8fd99-105">hello névkeresési zóna által képviselt hello IP-címtartományok hozzárendelése tooyour a szervezeten belül, általában az Internetszolgáltató által.</span><span class="sxs-lookup"><span data-stu-id="8fd99-105">hello IP ranges represented by hello reverse lookup zone must be assigned tooyour organization, typically by your ISP.</span></span>

<span data-ttu-id="8fd99-106">tooconfigure névkeresési DNS az Azure tulajdonában lévő IP-cím tooyour Azure-szolgáltatások című [hello névkeresési konfigurálásához hello IP-címek lefoglalt tooyour Azure szolgáltatás](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-106">tooconfigure reverse DNS for Azure-owned IP address assigned tooyour Azure service, see [configure hello reverse lookup for hello IP addresses allocated tooyour Azure service](dns-reverse-dns-for-azure-services.md).</span></span>

<span data-ttu-id="8fd99-107">A cikk elolvasása előtt meg kell ismernie a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-107">Before reading this article, you should be familiar with this [Overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md).</span></span>

<span data-ttu-id="8fd99-108">Ez a cikk bemutatja, hogyan hello lépéseket toocreate az első névkeresési DNS-zóna és a rekord hello Azure portál Azure PowerShell, az Azure CLI 1.0 vagy az Azure CLI 2.0 használatával.</span><span class="sxs-lookup"><span data-stu-id="8fd99-108">This article walks you through hello steps toocreate your first reverse lookup DNS zone and record using hello Azure portal, Azure PowerShell, Azure CLI 1.0 or Azure CLI 2.0.</span></span>

## <a name="create-a-reverse-lookup-dns-zone"></a><span data-ttu-id="8fd99-109">A névkeresési DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fd99-109">Create a reverse lookup DNS zone</span></span>

1. <span data-ttu-id="8fd99-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com)</span><span class="sxs-lookup"><span data-stu-id="8fd99-110">Sign in toohello [Azure portal](https://portal.azure.com)</span></span>
1. <span data-ttu-id="8fd99-111">Hello központ menüben kattintson, majd **új** > **hálózati** >, majd **DNS-zóna** tooopen hello **hozzon létre DNS-zóna**panelen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-111">On hello Hub menu, click and click **New** > **Networking** > and then click **DNS zone** tooopen hello **Create DNS zone** blade.</span></span>

   ![DNS-zóna](./media/dns-reverse-dns-hosting/figure1.png)

1. <span data-ttu-id="8fd99-113">A hello **hozzon létre DNS-zóna** panelen, a DNS-zóna neve.</span><span class="sxs-lookup"><span data-stu-id="8fd99-113">On hello **Create DNS zone** blade, name your DNS zone.</span></span> <span data-ttu-id="8fd99-114">hello zóna neve hello kialakított az IPv4 és IPv6-előtagok másképp van.</span><span class="sxs-lookup"><span data-stu-id="8fd99-114">hello name of hello zone is crafted differently for IPv4 and IPv6 prefixes.</span></span> <span data-ttu-id="8fd99-115">Vagy hello utasításokat a [IPV4](#ipv4) vagy [IPv6](#ipv6) tooname a zónát.</span><span class="sxs-lookup"><span data-stu-id="8fd99-115">Use either hello instructions for [IPV4](#ipv4) or [IPv6](#ipv6) tooname your zone.</span></span> <span data-ttu-id="8fd99-116">Ha végzett a kattintson **létrehozása** toocreate hello zóna.</span><span class="sxs-lookup"><span data-stu-id="8fd99-116">When complete click **Create** toocreate hello zone.</span></span>

### <a name="ipv4"></a><span data-ttu-id="8fd99-117">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="8fd99-117">IPv4</span></span>

<span data-ttu-id="8fd99-118">egy IPv4-névkeresési zóna neve hello hello IP-címtartomány, amely alapul.</span><span class="sxs-lookup"><span data-stu-id="8fd99-118">hello name of an IPv4 reverse lookup zone is based on hello IP range it represents.</span></span> <span data-ttu-id="8fd99-119">Nem lehet a következő formátumban hello: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8fd99-119">It should be in hello following format: `<IPv4 network prefix in reverse order>.in-addr.arpa`.</span></span> <span data-ttu-id="8fd99-120">Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv4).</span><span class="sxs-lookup"><span data-stu-id="8fd99-120">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv4).</span></span>

> [!NOTE]
> <span data-ttu-id="8fd99-121">Az Azure DNS-classless névkeresési DNS-címkeresési zónák létrehozásakor használnia kell a kötőjel (`-`) helyett egy perjel ("/") hello zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="8fd99-121">When creating classless reverse DNS lookup zones in Azure DNS, you must use a hyphen (`-`) rather than a forward slash ('/') in hello zone name.</span></span>
>
> <span data-ttu-id="8fd99-122">Például a hello IP-címtartomány 192.0.2.128/26, kell használnia `128-26.2.0.192.in-addr.arpa` hello zóna neve helyett `128/26.2.0.192.in-addr.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8fd99-122">For example, for hello IP range 192.0.2.128/26, you must use `128-26.2.0.192.in-addr.arpa` as hello zone name instead of `128/26.2.0.192.in-addr.arpa`.</span></span>
>
> <span data-ttu-id="8fd99-123">Ennek az az oka, mind támogatottak hello DNS szabványokkal, DNS-zónák tartalmazó hello perjel (`/`) karakter használata nem támogatott Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="8fd99-123">This is because, while both are supported by hello DNS standards, DNS zone names containing hello forward slash (`/`) character are not supported in Azure DNS.</span></span>

<span data-ttu-id="8fd99-124">hello következő példa bemutatja, hogyan toocreate egy osztály C fordított nevű DNS-zóna `2.0.192.in-addr.arpa` az Azure DNS hello Azure-portálon keresztül:</span><span class="sxs-lookup"><span data-stu-id="8fd99-124">hello following example shows how toocreate a 'Class C' reverse DNS zone named `2.0.192.in-addr.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure2.png)

<span data-ttu-id="8fd99-126">hello "Erőforráscsoport helye" hello erőforrásnak hello helyét határozza meg, és nincs hatással van a hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="8fd99-126">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="8fd99-127">mindig "global" Hello DNS-zóna helyét, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8fd99-127">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="8fd99-128">hello következő példák azt szemléltetik, hogyan toocomplete ennek a feladatnak az Azure PowerShell és az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="8fd99-128">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-129">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-129">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8fd99-130">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-130">Azure CLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8fd99-131">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-131">Azure CLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="8fd99-132">IPv6</span><span class="sxs-lookup"><span data-stu-id="8fd99-132">IPv6</span></span>

<span data-ttu-id="8fd99-133">egy IPv6-névkeresési zóna neve hello hello a következő formátumban kell lennie: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span><span class="sxs-lookup"><span data-stu-id="8fd99-133">hello name of an IPv6 reverse lookup zone should be in hello following form: `<IPv6 network prefix in reverse order>.ip6.arpa`.</span></span>  <span data-ttu-id="8fd99-134">Tekintse meg a [címfeloldási DNS- és támogatás az Azure-ban – áttekintés](dns-reverse-dns-overview.md#ipv6).</span><span class="sxs-lookup"><span data-stu-id="8fd99-134">For examples, see [overview of reverse DNS and support in Azure](dns-reverse-dns-overview.md#ipv6).</span></span>


<span data-ttu-id="8fd99-135">hello következő példa bemutatja, hogyan toocreate egy IPv6 névkeresési DNS-zóna nevű `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` az Azure DNS hello Azure-portálon keresztül:</span><span class="sxs-lookup"><span data-stu-id="8fd99-135">hello following example shows how toocreate an IPv6 reverse DNS lookup zone named `0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa` in Azure DNS via hello Azure portal:</span></span>

 ![DNS-zóna létrehozása](./media/dns-reverse-dns-hosting/figure3.png)

<span data-ttu-id="8fd99-137">hello "Erőforráscsoport helye" hello erőforrásnak hello helyét határozza meg, és nincs hatással van a hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="8fd99-137">hello 'Resource group location' defines hello location for hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="8fd99-138">mindig "global" Hello DNS-zóna helyét, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8fd99-138">hello DNS zone location is always 'global', and is not shown.</span></span>

<span data-ttu-id="8fd99-139">hello következő példák azt szemléltetik, hogyan toocomplete ennek a feladatnak az Azure PowerShell és az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="8fd99-139">hello following examples show how toocomplete this task with Azure PowerShell and hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-140">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-140">PowerShell</span></span>

```powershell
New-AzureRmDnsZone -Name 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azurecli-10"></a><span data-ttu-id="8fd99-141">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-141">AzureCLI 1.0</span></span>

```azurecli
azure network dns zone create MyResourceGroup 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azurecli-20"></a><span data-ttu-id="8fd99-142">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-142">AzureCLI 2.0</span></span>

```azurecli
az network dns zone create -g MyResourceGroup -n 0.0.0.0.d.c.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="delegate-a-reverse-dns-lookup-zone"></a><span data-ttu-id="8fd99-143">A címfeloldási DNS-zóna delegálása</span><span class="sxs-lookup"><span data-stu-id="8fd99-143">Delegate a reverse DNS lookup zone</span></span>

<span data-ttu-id="8fd99-144">A címfeloldási DNS-zóna hozunk létre, gondoskodnia kell hello zóna hello szülőzónában van átadva.</span><span class="sxs-lookup"><span data-stu-id="8fd99-144">Having created your reverse DNS lookup zone, you must ensure that hello zone is delegated from hello parent zone.</span></span> <span data-ttu-id="8fd99-145">DNS-delegálás lehetővé teszi, hogy a hello DNS nevét feloldási folyamat toofind hello üzemeltető névkiszolgálókat a címfeloldási DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="8fd99-145">DNS delegation enables hello DNS name resolution process toofind hello name servers hosting your reverse DNS lookup zone.</span></span> <span data-ttu-id="8fd99-146">Ez lehetővé teszi, hogy ezen kiszolgálók tooanswer DNS fordított névlekérdezései hello IP-címek a címtartomány.</span><span class="sxs-lookup"><span data-stu-id="8fd99-146">This enables those name servers tooanswer DNS reverse queries for hello IP addresses in your address range.</span></span>

<span data-ttu-id="8fd99-147">Címkeresési zónák, DNS-zóna delegálása hello folyamatán ismertetett [delegálása a tartományi tooAzure DNS](dns-delegate-domain-azure-dns.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-147">For forward lookup zones, hello process of delegating a DNS zone is described in [Delegate your domain tooAzure DNS](dns-delegate-domain-azure-dns.md).</span></span> <span data-ttu-id="8fd99-148">A delegálás névkeresési zónák hello működik ugyanúgy.</span><span class="sxs-lookup"><span data-stu-id="8fd99-148">Delegation for reverse lookup zones works hello same way.</span></span> <span data-ttu-id="8fd99-149">hello egyetlen különbség, hogy kell-e tooconfigure hello névkiszolgálók a hello a tartományregisztráló neve helyett az IP-címtartományt biztosító Internetszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="8fd99-149">hello only difference is that you need tooconfigure hello name servers with hello ISP who provided your IP range, rather than your domain name registrar.</span></span>

## <a name="create-a-dns-ptr-record"></a><span data-ttu-id="8fd99-150">DNS PTR-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8fd99-150">Create a DNS PTR record</span></span>

### <a name="ipv4"></a><span data-ttu-id="8fd99-151">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="8fd99-151">IPv4</span></span>

<span data-ttu-id="8fd99-152">hello alábbi példa bemutatja, hogyan PTR típusú rekord létrehozása az Azure DNS-névkeresési DNS-zóna hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="8fd99-152">hello following example walks you through hello process of creating a PTR record in a reverse DNS zone in Azure DNS.</span></span> <span data-ttu-id="8fd99-153">Más rekordtípusokhoz és toomodify meglévő rekordokat: [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-153">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1.  <span data-ttu-id="8fd99-154">Hello hello tetején **DNS-zóna** panelen válassza **+ rekordhalmaz** tooopen hello **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-154">At hello top of hello **DNS zone** blade, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

 ![DNS-zóna](./media/dns-reverse-dns-hosting/figure4.png)

1. <span data-ttu-id="8fd99-156">A hello **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-156">On hello **Add record set** blade.</span></span> 
1. <span data-ttu-id="8fd99-157">Válassza ki **PTR** hello record "**típus**" menü.</span><span class="sxs-lookup"><span data-stu-id="8fd99-157">Select **PTR** from hello record "**Type**" menu.</span></span>  
1. <span data-ttu-id="8fd99-158">hello hello rekordhalmaz nevét PTR-rekordot kell toobe hello többi hello IPv4-cím fordított sorrendben.</span><span class="sxs-lookup"><span data-stu-id="8fd99-158">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv4 address in reverse order.</span></span> <span data-ttu-id="8fd99-159">Ebben a példában hello első három oktettjének már fel vannak töltve hello Zónanév (.2.0.192) részeként.</span><span class="sxs-lookup"><span data-stu-id="8fd99-159">In this example, hello first three octets are already populated as part of hello zone name (.2.0.192).</span></span> <span data-ttu-id="8fd99-160">Ezért csak hello utolsó oktett hello neve mezőben van megadva.</span><span class="sxs-lookup"><span data-stu-id="8fd99-160">Therefore, only hello last octet is supplied in hello name field.</span></span> <span data-ttu-id="8fd99-161">Például nevezze el az volt a rekordhalmaz "**15**" amelynek IP-cím 192.0.2.15 erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8fd99-161">For example, you could name your record set "**15**" for a resource whose IP address is 192.0.2.15.</span></span>  
1. <span data-ttu-id="8fd99-162">A hello "**tartománynév**" mezőbe írja be a hello teljesen minősített tartománynevét (FQDN) használatával hello IP hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8fd99-162">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
1. <span data-ttu-id="8fd99-163">Válassza ki **OK** hello panel toocreate hello DNS-rekord hello alján.</span><span class="sxs-lookup"><span data-stu-id="8fd99-163">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

 ![rekordhalmaz hozzáadása](./media/dns-reverse-dns-hosting/figure5.png)

<span data-ttu-id="8fd99-165">hello következő példákban hogyan toocomplete PowerShell és hello AzureCLI ezt a feladatot:</span><span class="sxs-lookup"><span data-stu-id="8fd99-165">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-166">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-166">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 15 -RecordType PTR -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc1.contoso.com")
```
#### <a name="azurecli-10"></a><span data-ttu-id="8fd99-167">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-167">AzureCLI 1.0</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup 2.0.192.in-addr.arpa 15 PTR --ptrdname dc1.contoso.com  
```

#### <a name="azurecli-20"></a><span data-ttu-id="8fd99-168">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-168">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 2.0.192.in-addr.arpa -n 15 --ptrdname dc1.contoso.com
```

### <a name="ipv6"></a><span data-ttu-id="8fd99-169">IPv6</span><span class="sxs-lookup"><span data-stu-id="8fd99-169">IPv6</span></span>

<span data-ttu-id="8fd99-170">hello a következő példa bemutatja, hogyan hello új "PTR" rekord létrehozásának folyamatán.</span><span class="sxs-lookup"><span data-stu-id="8fd99-170">hello following example walks you through hello process of creating new 'PTR' record.</span></span> <span data-ttu-id="8fd99-171">Más rekordtípusokhoz és toomodify meglévő rekordokat: [kezelése DNS-rekordok és a rekordhalmazok hello Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-171">For other record types and toomodify existing records, see [Manage DNS records and record sets by using hello Azure portal](dns-operations-recordsets-portal.md).</span></span>

1. <span data-ttu-id="8fd99-172">Hello hello tetején **DNS-zóna panel**, jelölje be **+ rekordhalmaz** tooopen hello **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-172">At hello top of hello **DNS zone blade**, select **+ Record set** tooopen hello **Add record set** blade.</span></span>

  ![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure6.png)

2. <span data-ttu-id="8fd99-174">A hello **adja hozzá a rekordhalmaz** panelen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-174">On hello **Add record set** blade.</span></span> 
3. <span data-ttu-id="8fd99-175">Válassza ki **PTR** hello record "**típus**" menü.</span><span class="sxs-lookup"><span data-stu-id="8fd99-175">Select **PTR** from hello record "**Type**" menu.</span></span>  
4. <span data-ttu-id="8fd99-176">hello hello rekordhalmaz nevét PTR-rekordot kell toobe hello többi hello IPv6-cím fordított sorrendben.</span><span class="sxs-lookup"><span data-stu-id="8fd99-176">hello name of hello record set for a PTR record needs toobe hello rest of hello IPv6 address in reverse order.</span></span> <span data-ttu-id="8fd99-177">Nem minden nulla tömörítési tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="8fd99-177">It must not include any zero compression.</span></span> <span data-ttu-id="8fd99-178">Ebben a példában hello első 64 bites hello IPv6 már fel van töltve hello Zónanév (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa) részeként.</span><span class="sxs-lookup"><span data-stu-id="8fd99-178">In this example, hello first 64 bits of hello IPv6 are already populated as part of hello zone name (0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa).</span></span> <span data-ttu-id="8fd99-179">Ezért csak hello utolsó 64 bites hello neve mezőben megadva.</span><span class="sxs-lookup"><span data-stu-id="8fd99-179">Therefore, only hello last 64 bits are supplied in hello name field.</span></span> <span data-ttu-id="8fd99-180">hello hello IP-cím utolsó 64 bites időszak használata hello elválasztó mindegyik hexadecimális szám között fordított sorrendben kerülnek.</span><span class="sxs-lookup"><span data-stu-id="8fd99-180">hello last 64 bits of hello IP address are entered in reverse order, using a period as hello delimiter between each hexadecimal number.</span></span> <span data-ttu-id="8fd99-181">Például nevezze el az volt a rekordhalmaz "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" amelynek IP-cím 2001:0db8:abdc:0000:f524:10bc:1af9:405e erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8fd99-181">For example, you could name your record set "**e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f**" for a resource whose IP address is 2001:0db8:abdc:0000:f524:10bc:1af9:405e.</span></span>  
5. <span data-ttu-id="8fd99-182">A hello "**tartománynév**" mezőbe írja be a hello teljesen minősített tartománynevét (FQDN) használatával hello IP hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="8fd99-182">In hello "**Domain Name**" field, enter hello fully qualified domain name (FQDN) of hello resource using hello IP.</span></span>
6. <span data-ttu-id="8fd99-183">Válassza ki **OK** hello panel toocreate hello DNS-rekord hello alján.</span><span class="sxs-lookup"><span data-stu-id="8fd99-183">Select **OK** at hello bottom of hello blade toocreate hello DNS record.</span></span>

![Adja hozzá a rekordhalmaz panel](./media/dns-reverse-dns-hosting/figure7.png)

<span data-ttu-id="8fd99-185">hello következő példákban hogyan toocomplete PowerShell és hello AzureCLI ezt a feladatot:</span><span class="sxs-lookup"><span data-stu-id="8fd99-185">hello following are examples on how toocomplete this task with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-186">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-186">PowerShell</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f" -RecordType PTR -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "dc2.contoso.com")
```

#### <a name="azurecli-10"></a><span data-ttu-id="8fd99-187">AzureCLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-187">AzureCLI 1.0</span></span>

```
azure network dns record-set add-record MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f PTR --ptrdname dc2.contoso.com 
```
 
#### <a name="azurecli-20"></a><span data-ttu-id="8fd99-188">AzureCLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-188">AzureCLI 2.0</span></span>

```azurecli
    az network dns record-set ptr add-record -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -n e.5.0.4.9.f.a.1.c.b.0.1.4.2.5.f --ptrdname dc2.contoso.com
```

## <a name="view-records"></a><span data-ttu-id="8fd99-189">A rekordok megtekintése</span><span class="sxs-lookup"><span data-stu-id="8fd99-189">View Records</span></span>

<span data-ttu-id="8fd99-190">tooview hello rekordok hozott létre, keresse meg a DNS-zónát tooyour hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="8fd99-190">tooview hello records you created, navigate tooyour DNS zone in hello Azure portal.</span></span> <span data-ttu-id="8fd99-191">A hello alacsonyabb hello részét **DNS-zóna** panelen láthatja hello rekordok hello DNS-zónához.</span><span class="sxs-lookup"><span data-stu-id="8fd99-191">In hello lower part of hello **DNS zone** blade, you can see hello records for hello DNS zone.</span></span> <span data-ttu-id="8fd99-192">Hello alapértelmezett NS és SOA típusú rekordok, minden zóna jöttek létre, és a létrehozott új rekordok kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="8fd99-192">You should see hello default NS and SOA records, which are created in every zone, plus any new records you have created.</span></span>

### <a name="ipv4"></a><span data-ttu-id="8fd99-193">IPv4-alapú</span><span class="sxs-lookup"><span data-stu-id="8fd99-193">IPv4</span></span>

<span data-ttu-id="8fd99-194">DNS zóna panelen IPv4 PTR-rekordok:</span><span class="sxs-lookup"><span data-stu-id="8fd99-194">DNS zone blade, showing IPv4 PTR records:</span></span>

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure8.png)

<span data-ttu-id="8fd99-196">hello alábbi példák megjelenítése hogyan tooview hello PTR rögzíti a PowerShell használatával vagy az Azure parancssori felület hello:</span><span class="sxs-lookup"><span data-stu-id="8fd99-196">hello following examples show how tooview hello PTR records using PowerShell or hello Azure CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-197">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-197">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 2.0.192.in-addr.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8fd99-198">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-198">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 2.0.192.in-addr.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8fd99-199">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-199">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 2.0.192.in-addr.arpa
```

### <a name="ipv6"></a><span data-ttu-id="8fd99-200">IPv6</span><span class="sxs-lookup"><span data-stu-id="8fd99-200">IPv6</span></span>

<span data-ttu-id="8fd99-201">DNS zóna panelen IPv6 PTR-rekordok:</span><span class="sxs-lookup"><span data-stu-id="8fd99-201">DNS zone blade, showing IPv6 PTR records:</span></span>

![DNS-zónák panel](./media/dns-reverse-dns-hosting/figure9.png)

<span data-ttu-id="8fd99-203">Az alábbiakban hello hogyan tooview hello rögzíti a PowerShell és hello AzureCLI példák:</span><span class="sxs-lookup"><span data-stu-id="8fd99-203">hello following are examples on how tooview hello records with PowerShell and hello AzureCLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="8fd99-204">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8fd99-204">PowerShell</span></span>

```powershell
Get-AzureRmDnsRecordSet -ZoneName 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa -ResourceGroupName MyResourceGroup
```

#### <a name="azure-cli-10"></a><span data-ttu-id="8fd99-205">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-205">Azure CLI 1.0</span></span>

```azurecli
    azure network dns record-set list MyResourceGroup 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

#### <a name="azure-cli-20"></a><span data-ttu-id="8fd99-206">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8fd99-206">Azure CLI 2.0</span></span>

```azurecli
    azure network dns record-set list -g MyResourceGroup -z 0.0.0.0.c.d.b.a.8.b.d.0.1.0.0.2.ip6.arpa
```

## <a name="faq"></a><span data-ttu-id="8fd99-207">GYIK</span><span class="sxs-lookup"><span data-stu-id="8fd99-207">FAQ</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-my-isp-assigned-ip-blocks-on-azure-dns"></a><span data-ttu-id="8fd99-208">Képes-e üzemeltetni névkeresési DNS-címkeresési zónák az Internetszolgáltató által hozzárendelt IP-címblokkok az Azure DNS szolgáltatásra?</span><span class="sxs-lookup"><span data-stu-id="8fd99-208">Can I host reverse DNS lookup zones for my ISP-assigned IP blocks on Azure DNS?</span></span>

<span data-ttu-id="8fd99-209">Igen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-209">Yes.</span></span> <span data-ttu-id="8fd99-210">Hello (ARPA) névkeresési zónák az Azure DNS-ben a saját IP-címtartományok teljes mértékben támogatott.</span><span class="sxs-lookup"><span data-stu-id="8fd99-210">Hosting hello reverse lookup (ARPA) zones for your own IP ranges in Azure DNS is fully supported.</span></span>

<span data-ttu-id="8fd99-211">Hozzon létre hello névkeresési zónát az Azure DNS-amint ez a cikk azt, majd az Internetszolgáltató túl együttműködve[delegált hello zóna](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-211">Create hello reverse lookup zone in Azure DNS as explained in this article, then work with your ISP too[delegate hello zone](dns-domain-delegation.md).</span></span>  <span data-ttu-id="8fd99-212">Hello PTR-rekordok kezelhetők az egyes névkeresési a hello módon egyéb bejegyzéstípusokat.</span><span class="sxs-lookup"><span data-stu-id="8fd99-212">You can then manage hello PTR records for each reverse lookup in hello same way as other record types.</span></span>

### <a name="how-much-does-hosting-my-reverse-dns-lookup-zone-cost"></a><span data-ttu-id="8fd99-213">A fordított DNS keresési zónához költségének üzemeltető mennyi használ?</span><span class="sxs-lookup"><span data-stu-id="8fd99-213">How much does hosting my reverse DNS lookup zone cost?</span></span>

<span data-ttu-id="8fd99-214">Hello címfeloldási DNS-zóna üzemeltető, az az Internetszolgáltató által hozzárendelt IP-Címblokk az Azure DNS-rendszer díjakon [Azure DNS normál díjszabás](https://azure.microsoft.com/pricing/details/dns/).</span><span class="sxs-lookup"><span data-stu-id="8fd99-214">Hosting hello reverse DNS lookup zone for your ISP-assigned IP block in Azure DNS is charged at [standard Azure DNS rates](https://azure.microsoft.com/pricing/details/dns/).</span></span>

### <a name="can-i-host-reverse-dns-lookup-zones-for-both-ipv4-and-ipv6-addresses-in-azure-dns"></a><span data-ttu-id="8fd99-215">IPv4 és IPv6 címek az Azure DNS-névkeresési DNS-címkeresési zónák is futtatni?</span><span class="sxs-lookup"><span data-stu-id="8fd99-215">Can I host reverse DNS lookup zones for both IPv4 and IPv6 addresses in Azure DNS?</span></span>

<span data-ttu-id="8fd99-216">Igen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-216">Yes.</span></span> <span data-ttu-id="8fd99-217">Ez a cikk azt ismerteti, hogyan toocreate IPv4 és IPv6 névkeresési DNS zónák Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="8fd99-217">This article explains how toocreate both IPv4 and IPv6 reverse DNS lookup zones in Azure DNS.</span></span>

### <a name="can-i-import-an-existing-reverse-dns-lookup-zone"></a><span data-ttu-id="8fd99-218">Beimportálhatok egy meglévő címfeloldási DNS-zóna?</span><span class="sxs-lookup"><span data-stu-id="8fd99-218">Can I import an existing reverse DNS lookup zone?</span></span>

<span data-ttu-id="8fd99-219">Igen.</span><span class="sxs-lookup"><span data-stu-id="8fd99-219">Yes.</span></span> <span data-ttu-id="8fd99-220">Hello Azure CLI tooimport meglévő DNS-zóna Azure DNS-ben is használhatja.</span><span class="sxs-lookup"><span data-stu-id="8fd99-220">You can use hello Azure CLI tooimport existing DNS zones into Azure DNS.</span></span> <span data-ttu-id="8fd99-221">Ez a módszer a címkeresési zónák és a névkeresési zóna listáját.</span><span class="sxs-lookup"><span data-stu-id="8fd99-221">This works for both forward lookup zones and reverse lookup zones.</span></span>

<span data-ttu-id="8fd99-222">További információkért lásd: [importálásának és exportálásának használatával DNS zóna fájlt hello Azure CLI](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-222">For more information, see [Import and export a DNS zone file using hello Azure CLI](dns-import-export.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8fd99-223">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8fd99-223">Next steps</span></span>

<span data-ttu-id="8fd99-224">A címfeloldási DNS-további információkért lásd: [Wikipedia névkeresési DNS](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span><span class="sxs-lookup"><span data-stu-id="8fd99-224">For more information on reverse DNS, see [reverse DNS lookup on Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).</span></span>
<br>
<span data-ttu-id="8fd99-225">Ismerje meg, hogyan túl[címfeloldási DNS-rekordjait az Azure-szolgáltatások kezelése](dns-reverse-dns-for-azure-services.md).</span><span class="sxs-lookup"><span data-stu-id="8fd99-225">Learn how too[manage reverse DNS records for your Azure services](dns-reverse-dns-for-azure-services.md).</span></span>
