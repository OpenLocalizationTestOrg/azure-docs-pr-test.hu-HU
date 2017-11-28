---
title: "az Azure DNS az Azure PowerShell rögzíti aaaManage DNS |} Microsoft Docs"
description: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése esetén az Azure DNS-tartomány. Az összes PowerShell-parancsokat rekordhalmazokat és rekordokat műveleteket."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 7136a373-0682-471c-9c28-9e00d2add9c2
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 12/21/2016
ms.author: gwallace
ms.openlocfilehash: bfdf116e174d06db0514abdc0ec3f4fc4ee0a079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="60b82-104">DNS-rekordok és az Azure PowerShell használata Azure DNS rekordhalmazok kezelése</span><span class="sxs-lookup"><span data-stu-id="60b82-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="60b82-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="60b82-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="60b82-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="60b82-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="60b82-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="60b82-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="60b82-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="60b82-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="60b82-109">Ez a cikk bemutatja, hogyan toomanage DNS rögzíti a DNS-zónát az Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="60b82-109">This article shows you how toomanage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="60b82-110">DNS-rekordokat is hello platformfüggetlen használatával felügyelendő [Azure CLI](dns-operations-recordsets-cli.md) vagy hello [Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="60b82-110">DNS records can also be managed by using hello cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="60b82-111">a cikkben szereplő példák hello során feltételezzük, hogy már [Azure PowerShell jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="60b82-111">hello examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="60b82-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="60b82-112">Introduction</span></span>

<span data-ttu-id="60b82-113">DNS-rekordok létrehozása az Azure DNS-, előtt először toounderstand miként Azure DNS rendezi a DNS-rekordhalmazok DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="60b82-113">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="60b82-114">Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="60b82-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="60b82-115">Hozzon létre egy új DNS-rekord</span><span class="sxs-lookup"><span data-stu-id="60b82-115">Create a new DNS record</span></span>

<span data-ttu-id="60b82-116">Ha az új rekord hello azonos nevet, és írja be egy meglévő rekordként, kell túl[hozzá toohello meglévő rekordhalmaz](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="60b82-116">If your new record has hello same name and type as an existing record, you need too[add it toohello existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="60b82-117">Ha az új rekord tartalmazza-e a meglévő rekordok különböző nevű és típusú tooall, új rekordhalmaz toocreate kell.</span><span class="sxs-lookup"><span data-stu-id="60b82-117">If your new record has a different name and type tooall existing records, you need toocreate a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="60b82-118">"A" rekordokat létrehozni egy új rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="60b82-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="60b82-119">Rekordhalmazok hello segítségével hozhatja létre `New-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="60b82-119">You create record sets by using hello `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="60b82-120">A rekordhalmaz létrehozásakor kell toospecify hello rekordhalmazának neve, hello zóna, hello létrehozásakor toolive (TTL), hello rekordtípus és hello rekordok toobe.</span><span class="sxs-lookup"><span data-stu-id="60b82-120">When creating a record set, you need toospecify hello record set name, hello zone, hello time toolive (TTL), hello record type, and hello records toobe created.</span></span>

<span data-ttu-id="60b82-121">hello paramétereket a rekordok tooa rekordhalmaz hozzáadásához hello hello rekordhalmaz típusától függenek.</span><span class="sxs-lookup"><span data-stu-id="60b82-121">hello parameters for adding records tooa record set vary depending on hello type of hello record set.</span></span> <span data-ttu-id="60b82-122">Például egy "A" típusú rekordkészlet használatakor meg kell toospecify hello IP-cím hello paraméter használatával `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="60b82-122">For example, when using a record set of type 'A', you need toospecify hello IP address using hello parameter `-IPv4Address`.</span></span> <span data-ttu-id="60b82-123">Más paramétereket az egyéb típusú bejegyzés használnak.</span><span class="sxs-lookup"><span data-stu-id="60b82-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="60b82-124">Lásd: [további rekordtípusokra](#additional-record-type-examples) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="60b82-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="60b82-125">hello alábbi példa létrehoz egy hello relatív nevű DNS-zóna "contoso.com" hello "www" rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-125">hello following example creates a record set with hello relative name 'www' in hello DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="60b82-126">hello rekordhalmaz hello teljesen minősített neve "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="60b82-126">hello fully-qualified name of hello record set is 'www.contoso.com'.</span></span> <span data-ttu-id="60b82-127">hello típus az "A", és hello élettartam 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="60b82-127">hello record type is 'A', and hello TTL is 3600 seconds.</span></span> <span data-ttu-id="60b82-128">hello rekordhalmaz egyetlen rekordot, IP-cím "1.2.3.4" tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-128">hello record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="60b82-129">toocreate hello "csúcs" zóna beállított egy olyan rekordot (ebben az esetben "contoso.com"), használjon hello rekordhalmazának neve "@" (idézőjelek nélkül):</span><span class="sxs-lookup"><span data-stu-id="60b82-129">toocreate a record set at hello 'apex' of a zone (in this case, 'contoso.com'), use hello record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="60b82-130">Ha rekordhalmaz egynél több rekordot tartalmazó toocreate van szüksége, először hozzon létre egy helyi tömb hello rekordok hozzáadásához, majd adjon át hello tömb túl`New-AzureRmDnsRecordSet` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="60b82-130">If you need toocreate a record set containing more than one record, first create a local array and add hello records, then pass hello array too`New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="60b82-131">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="60b82-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="60b82-132">hello következő példa bemutatja, hogyan toocreate nevű rekordhalmaz két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =".</span><span class="sxs-lookup"><span data-stu-id="60b82-132">hello following example shows how toocreate a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="60b82-133">Az Azure DNS "empty" rekordhalmazok, amely működhet, és egy helyőrző tooreserve egy DNS-név DNS-rekordok létrehozása előtt is támogatja.</span><span class="sxs-lookup"><span data-stu-id="60b82-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="60b82-134">Üres rekordhalmazok hello Azure DNS vezérlő vezérlősík látható, de jelennek meg hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="60b82-134">Empty record sets are visible in hello Azure DNS control plane, but do appear on hello Azure DNS name servers.</span></span> <span data-ttu-id="60b82-135">a következő példa hello létrehoz egy üres rekordhalmaz:</span><span class="sxs-lookup"><span data-stu-id="60b82-135">hello following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="60b82-136">Más típusú rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-136">Create records of other types</span></span>

<span data-ttu-id="60b82-137">Ha látható részletesen hogyan toocreate "A" rögzíti, a következő példák szemléltetik, hogyan toocreate rekordok egyéb bejegyzéstípusokat Azure DNS által támogatott hello.</span><span class="sxs-lookup"><span data-stu-id="60b82-137">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="60b82-138">Minden esetben megmutatjuk, hogyan állíthatja toocreate rekord egyetlen rekordot tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="60b82-138">In each case, we show how toocreate a record set containing a single record.</span></span> <span data-ttu-id="60b82-139">hello korábbi példák "A" rekordok lehet igazítani toocreate metaadatokkal, a több rekordot tartalmazó más típusú rekordhalmazok vagy üres rekordhalmazok toocreate.</span><span class="sxs-lookup"><span data-stu-id="60b82-139">hello earlier examples for 'A' records can be adapted toocreate record sets of other types containing multiple records, with metadata, or toocreate empty record sets.</span></span>

<span data-ttu-id="60b82-140">Nem ad egy példa toocreate egy SOA típusú rekordhalmaz SOAs elkészítése és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön.</span><span class="sxs-lookup"><span data-stu-id="60b82-140">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="60b82-141">Azonban [SOA módosítható, egy újabb példában látható módon hello](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="60b82-141">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="60b82-142">Egyetlen rekordot tartalmazó AAAA típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="60b82-143">Egyetlen rekordot tartalmazó CNAME típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="60b82-144">hello DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna hello csúcsán (`-Name '@'`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="60b82-144">hello DNS standards do not permit CNAME records at hello apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="60b82-145">További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="60b82-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="60b82-146">Egyetlen rekordot tartalmazó MX típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="60b82-147">Ebben a példában hello rekordhalmaznevet használjuk "@" toocreate egy MX rekord: hello zóna felső pontja (ebben az esetben "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="60b82-147">In this example, we use hello record set name '@' toocreate an MX record at hello zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="60b82-148">Egyetlen rekordot tartalmazó NS típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="60b82-149">Egyetlen rekordot tartalmazó PTR típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="60b82-150">Ebben az esetben "my-arpa-zónában (Zone.com)" jelöli hello ARPA névkeresési zóna képviselő az IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="60b82-150">In this case, 'my-arpa-zone.com' represents hello ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="60b82-151">Minden egyes PTR típusú rekord a zóna tooan IP-címet az IP-címtartományon belül felel meg.</span><span class="sxs-lookup"><span data-stu-id="60b82-151">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span> <span data-ttu-id="60b82-152">hello rekordjának neve "10" hello utolsó oktett hello IP-cím, ez a bejegyzés által képviselt IP-címtartományon belül.</span><span class="sxs-lookup"><span data-stu-id="60b82-152">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="60b82-153">Egyetlen rekordot tartalmazó SRV típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="60b82-154">Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a hello  *\_szolgáltatás* és  *\_protokoll* hello a rekordhalmaz neveként.</span><span class="sxs-lookup"><span data-stu-id="60b82-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="60b82-155">Nincs szükség tooinclude van "@" hello a rekordhalmaz neveként az SRV rekord létrehozása beállításakor: hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="60b82-155">There is no need tooinclude '@' in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="60b82-156">Egyetlen rekordot tartalmazó TXT rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="60b82-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="60b82-157">hello következő példa bemutatja, hogyan toocreate egy TXT rögzíti.</span><span class="sxs-lookup"><span data-stu-id="60b82-157">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="60b82-158">További információ a támogatott TXT rekord hello karakterlánc maximális hossza: [TXT rekord](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="60b82-158">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="60b82-159">Rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="60b82-159">Get a record set</span></span>

<span data-ttu-id="60b82-160">Használjon egy meglévő rekordhalmaz tooretrieve `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="60b82-160">tooretrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="60b82-161">Ez a parancsmag adja vissza egy helyi hello az Azure DNS-rekordot képviselő objektum.</span><span class="sxs-lookup"><span data-stu-id="60b82-161">This cmdlet returns a local object that represents hello record set in Azure DNS.</span></span>

<span data-ttu-id="60b82-162">A `New-AzureRmDnsRecordSet`, megadott hello rekordhalmaz nevének kell lennie egy *relatív* nevét, azaz kell zárnia a hello zóna neve.</span><span class="sxs-lookup"><span data-stu-id="60b82-162">As with `New-AzureRmDnsRecordSet`, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="60b82-163">Szükség toospecify hello rekordtípust, és hello rekordhalmaz tartalmazó hello zóna.</span><span class="sxs-lookup"><span data-stu-id="60b82-163">You also need toospecify hello record type, and hello zone containing hello record set.</span></span>

<span data-ttu-id="60b82-164">hello következő példa bemutatja, hogyan tooretrieve nevű rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-164">hello following example shows how tooretrieve a record set.</span></span> <span data-ttu-id="60b82-165">Ebben a példában hello zóna adott hello `-ZoneName` és `-ResourceGroupName` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="60b82-165">In this example, hello zone is specified using hello `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="60b82-166">Azt is megteheti, azt is megadhatja a zóna objektummal, átadott hello segítségével hello zóna `-Zone` paraméter.</span><span class="sxs-lookup"><span data-stu-id="60b82-166">Alternatively, you can also specify hello zone using a zone object, passed using hello `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="60b82-167">Lista rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="60b82-167">List record sets</span></span>

<span data-ttu-id="60b82-168">Is `Get-AzureRmDnsZone` toolist rekordhalmazok zónában, hello kihagyásával őriznek `-Name` és/vagy `-RecordType` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="60b82-168">You can also use `Get-AzureRmDnsZone` toolist record sets in a zone, by omitting hello `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="60b82-169">hello alábbi példa adja vissza az összes rekordhalmazok hello zónában:</span><span class="sxs-lookup"><span data-stu-id="60b82-169">hello following example returns all record sets in hello zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="60b82-170">hello következő példa bemutatja, hogyan összes rekordkészletek egy adott típusú neve hello rekord kihagyásával beállítása közben hello rekordtípus megadásával kérhető:</span><span class="sxs-lookup"><span data-stu-id="60b82-170">hello following example shows how all record sets of a given type can be retrieved by specifying hello record type while omitting hello record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="60b82-171">a megadott nevű rekordkészletek összes tooretrieve rekordtípusokhoz, keresztül kell tooretrieve rekordhalmazok majd szűrő hello eredmények:</span><span class="sxs-lookup"><span data-stu-id="60b82-171">tooretrieve all record sets with a given name, across record types, you need tooretrieve all record sets and then filter hello results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="60b82-172">A fenti példa minden hello hello zóna is megadott felhasználónévvel hello `-ZoneName` és `-ResourceGroupName`paraméterek (ahogy), vagy adjon meg egy zóna objektum:</span><span class="sxs-lookup"><span data-stu-id="60b82-172">In all hello above examples, hello zone can be specified either by using hello `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="60b82-173">A meglévő rekordhalmazt rekord tooan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="60b82-173">Add a record tooan existing record set</span></span>

<span data-ttu-id="60b82-174">egy rekord tooan meglévő rekord tooadd beállításához hajtsa végre a következő három lépéseket hello:</span><span class="sxs-lookup"><span data-stu-id="60b82-174">tooadd a record tooan existing record set, follow hello following three steps:</span></span>

1. <span data-ttu-id="60b82-175">Hello meglévő rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="60b82-175">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="60b82-176">Adja hozzá a hello új rekord toohello helyi rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-176">Add hello new record toohello local record set.</span></span> <span data-ttu-id="60b82-177">Ez az offline művelet.</span><span class="sxs-lookup"><span data-stu-id="60b82-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="60b82-178">Véglegesítse hello módosítás vissza toohello Azure DNS-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="60b82-178">Commit hello change back toohello Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="60b82-179">Használatával `Set-AzureRmDnsRecordSet` *lecseréli* hello meglévő Azure DNS-ben (és a benne található összes rekord) tartalmazó rekordhalmaz hello rekordhalmaz megadott.</span><span class="sxs-lookup"><span data-stu-id="60b82-179">Using `Set-AzureRmDnsRecordSet` *replaces* hello existing record set in Azure DNS (and all records it contains) with hello record set specified.</span></span> <span data-ttu-id="60b82-180">[ETag ellenőrzések](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="60b82-180">[Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="60b82-181">Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="60b82-181">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="60b82-182">Ez a feladatütemezési műveletek is lehet *adatcsatornán*, azaz hello rekordhalmaz objektum használatával hello cső, ahelyett hogy paraméterként átadja azt át:</span><span class="sxs-lookup"><span data-stu-id="60b82-182">This sequence of operations can also be *piped*, meaning you pass hello record set object by using hello pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="60b82-183">a fenti hello példák azt szemléltetik, hogyan állítsa be az "A" rekord tooan meglévő rekord tooadd "A" típusú.</span><span class="sxs-lookup"><span data-stu-id="60b82-183">hello examples above show how tooadd an 'A' record tooan existing record set of type 'A'.</span></span> <span data-ttu-id="60b82-184">A hasonló feladatütemezési műveletek toorecord rekordhalmazok használt tooadd más típusú, és hello `-Ipv4Address` paramétere `Add-AzureRmDnsRecordConfig` más paramétereket adott tooeach rekordtípussal.</span><span class="sxs-lookup"><span data-stu-id="60b82-184">A similar sequence of operations is used tooadd records toorecord sets of other types, substituting hello `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific tooeach record type.</span></span> <span data-ttu-id="60b82-185">hello különböző rekordtípusú paramétereinek vannak hello ugyanaz, mint hello `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.</span><span class="sxs-lookup"><span data-stu-id="60b82-185">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="60b82-186">"CNAME" vagy "SOA" típusú rekordhalmazok nem tartalmazhat egynél több rekordot.</span><span class="sxs-lookup"><span data-stu-id="60b82-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="60b82-187">Ennél a határértéknél hello DNS-szabványokból ered.</span><span class="sxs-lookup"><span data-stu-id="60b82-187">This constraint arises from hello DNS standards.</span></span> <span data-ttu-id="60b82-188">Az Azure DNS korlátozása nincs.</span><span class="sxs-lookup"><span data-stu-id="60b82-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="60b82-189">Távolítsa el a rekord egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="60b82-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="60b82-190">hello folyamat tooremove rekordhalmaz rekord egy rekord tooan létező, hasonló toohello folyamat tooadd rekordhalmaz:</span><span class="sxs-lookup"><span data-stu-id="60b82-190">hello process tooremove a record from a record set is similar toohello process tooadd a record tooan existing record set:</span></span>

1. <span data-ttu-id="60b82-191">Hello meglévő rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="60b82-191">Get hello existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="60b82-192">Távolítsa el a hello bejegyzést hello helyi rekordhalmaz objektumból.</span><span class="sxs-lookup"><span data-stu-id="60b82-192">Remove hello record from hello local record set object.</span></span> <span data-ttu-id="60b82-193">Ez az offline művelet.</span><span class="sxs-lookup"><span data-stu-id="60b82-193">This is an off-line operation.</span></span> <span data-ttu-id="60b82-194">hello rekordot eltávolított összes paraméterek között kell lennie terméknévnek pontosan egyeznie kell a meglévő bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="60b82-194">hello record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="60b82-195">Véglegesítse hello módosítás vissza toohello Azure DNS-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="60b82-195">Commit hello change back toohello Azure DNS service.</span></span> <span data-ttu-id="60b82-196">Választható használata hello `-Overwrite` toosuppress kapcsoló [Etag ellenőrzi](dns-zones-records.md#etags) egyidejű változásait.</span><span class="sxs-lookup"><span data-stu-id="60b82-196">Use hello optional `-Overwrite` switch toosuppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="60b82-197">Feladatütemezési tooremove hello utolsó rekordját rekordhalmaz fent hello használata nem hello rekordhalmaz törlése, inkább egy üres rekordhalmaz elhagyják.</span><span class="sxs-lookup"><span data-stu-id="60b82-197">Using hello above sequence tooremove hello last record from a record set does not delete hello record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="60b82-198">a rekordhalmaz teljesen, tooremove lásd: [rekordhalmaz törlése](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="60b82-198">tooremove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="60b82-199">Hasonlóképpen tooadding rekordok tooa rekordhalmaz, műveletek tooremove is átirányítható a rekordhalmaz hello sorozatát:</span><span class="sxs-lookup"><span data-stu-id="60b82-199">Similarly tooadding records tooa record set, hello sequence of operations tooremove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="60b82-200">Különböző típusok támogatottak úgy, hogy túl hello megfelelő típusra vonatkozó paramétereket`Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="60b82-200">Different record types are supported by passing hello appropriate type-specific parameters too`Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="60b82-201">hello különböző rekordtípusú paramétereinek vannak hello ugyanaz, mint hello `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.</span><span class="sxs-lookup"><span data-stu-id="60b82-201">hello parameters for each record type are hello same as for hello `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="60b82-202">Módosíthatja egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="60b82-202">Modify an existing record set</span></span>

<span data-ttu-id="60b82-203">hello a módosítását egy meglévő rekordhalmaz lépésekre hasonló toohello lépései hozzáadása vagy eltávolítása a bejegyzések rekordhalmaz:</span><span class="sxs-lookup"><span data-stu-id="60b82-203">hello steps for modifying an existing record set are similar toohello steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="60b82-204">Hello meglévő rekordhalmazt használatával beolvasása `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="60b82-204">Retrieve hello existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="60b82-205">Hello helyi rekordhalmaz objektum által módosítható:</span><span class="sxs-lookup"><span data-stu-id="60b82-205">Modify hello local record set object by:</span></span>
    * <span data-ttu-id="60b82-206">Hozzáadása vagy eltávolítása a bejegyzések</span><span class="sxs-lookup"><span data-stu-id="60b82-206">Adding or removing records</span></span>
    * <span data-ttu-id="60b82-207">A meglévő rekordok hello paraméterek módosítása</span><span class="sxs-lookup"><span data-stu-id="60b82-207">Changing hello parameters of existing records</span></span>
    * <span data-ttu-id="60b82-208">Metaadatok és az idő toolive (TTL) hello rekord módosítása beállítása</span><span class="sxs-lookup"><span data-stu-id="60b82-208">Changing hello record set metadata and time toolive (TTL)</span></span>
3. <span data-ttu-id="60b82-209">A változtatások véglegesítése a határidő hello segítségével `Set-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="60b82-209">Commit your changes by using hello `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="60b82-210">Ez *lecseréli* hello meglévő az Azure DNS-tartalmazó rekordhalmaz hello rekordhalmaz megadott.</span><span class="sxs-lookup"><span data-stu-id="60b82-210">This *replaces* hello existing record set in Azure DNS with hello record set specified.</span></span>

<span data-ttu-id="60b82-211">Használata esetén `Set-AzureRmDnsRecordSet`, [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="60b82-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="60b82-212">Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="60b82-212">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

### <a name="tooupdate-a-record-in-an-existing-record-set"></a><span data-ttu-id="60b82-213">meglévő rekord rekord tooupdate beállítása</span><span class="sxs-lookup"><span data-stu-id="60b82-213">tooupdate a record in an existing record set</span></span>

<span data-ttu-id="60b82-214">Ebben a példában a Microsoft hello IP-cím egy létező "A" rekord módosítása:</span><span class="sxs-lookup"><span data-stu-id="60b82-214">In this example, we change hello IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="60b82-215">egy SOA rekordot toomodify</span><span class="sxs-lookup"><span data-stu-id="60b82-215">toomodify an SOA record</span></span>

<span data-ttu-id="60b82-216">Nem adja hozzá vagy rekordok eltávolítása automatikusan létrehozott hello zóna felső pontja, SOA rekordot hello (`-Name "@"`, beleértve az ajánlat jelek).</span><span class="sxs-lookup"><span data-stu-id="60b82-216">You cannot add or remove records from hello automatically created SOA record set at hello zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="60b82-217">Azonban módosíthatja hello SOA típusú rekordját (kivéve az "állomás") belül hello paraméterek egyikét, és hello rekordhalmaz TTL-t.</span><span class="sxs-lookup"><span data-stu-id="60b82-217">However, you can modify any of hello parameters within hello SOA record (except "Host") and hello record set TTL.</span></span>

<span data-ttu-id="60b82-218">a következő példa azt mutatja meg hogyan hello toochange hello *E-mail* hello SOA típusú rekordjának tulajdonsága:</span><span class="sxs-lookup"><span data-stu-id="60b82-218">hello following example shows how toochange hello *Email* property of hello SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="60b82-219">hello zóna felső pontja toomodify Névkiszolgálói rekordok</span><span class="sxs-lookup"><span data-stu-id="60b82-219">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="60b82-220">Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="60b82-220">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="60b82-221">Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.</span><span class="sxs-lookup"><span data-stu-id="60b82-221">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="60b82-222">Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="60b82-222">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="60b82-223">Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="60b82-223">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="60b82-224">Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="60b82-224">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="60b82-225">Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="60b82-225">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="60b82-226">Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="60b82-226">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="60b82-227">hello a következő példa bemutatja, hogyan tooadd további neve server toohello NS-rekord állítják hello zóna felső pontja:</span><span class="sxs-lookup"><span data-stu-id="60b82-227">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="toomodify-record-set-metadata"></a><span data-ttu-id="60b82-228">a rekordhalmaz toomodify metaadatok</span><span class="sxs-lookup"><span data-stu-id="60b82-228">toomodify record set metadata</span></span>

<span data-ttu-id="60b82-229">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="60b82-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="60b82-230">hello következő példa bemutatja, hogyan toomodify hello metaadatok egy meglévő bejegyzés be:</span><span class="sxs-lookup"><span data-stu-id="60b82-230">hello following example shows how toomodify hello metadata of an existing record set:</span></span>

```powershell
# Get hello record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="60b82-231">A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="60b82-231">Delete a record set</span></span>

<span data-ttu-id="60b82-232">Rekordhalmazok hello segítségével törölhetők `Remove-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="60b82-232">Record sets can be deleted by using hello `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="60b82-233">Rekordhalmaz törlése is törli az összes rekordján hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-233">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="60b82-234">Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="60b82-234">You cannot delete hello SOA and NS record sets at hello zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="60b82-235">Az Azure DNS automatikusan létrehozza ezeket a beállítást, ha hello zóna lett létrehozva, és automatikusan törli azokat az hello zóna törlődik.</span><span class="sxs-lookup"><span data-stu-id="60b82-235">Azure DNS created these automatically when hello zone was created, and deletes them automatically when hello zone is deleted.</span></span>

<span data-ttu-id="60b82-236">hello következő példa bemutatja, hogyan toodelete nevű rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="60b82-236">hello following example shows how toodelete a record set.</span></span> <span data-ttu-id="60b82-237">Ebben a példában hello rekordhalmazának neve, a rekordhalmaz típusa, a zóna neve és a erőforráscsoport minden egyes megadott explicit módon.</span><span class="sxs-lookup"><span data-stu-id="60b82-237">In this example, hello record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="60b82-238">Azt is megteheti hello rekordhalmaz nevét és típusát is megadható, és hello zónát a megadott objektum használatával:</span><span class="sxs-lookup"><span data-stu-id="60b82-238">Alternatively, hello record set can be specified by name and type, and hello zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="60b82-239">Egy harmadik lehetőségként hello rekordhalmaz maga adható meg a rekordhalmaz-objektummal:</span><span class="sxs-lookup"><span data-stu-id="60b82-239">As a third option, hello record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="60b82-240">Hello rekordhalmaz egy rekordhalmaz objektum törölt toobe megadásakor [Etag ellenőrzi](dns-zones-records.md#etags) használt tooensure egyidejű változtatások nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="60b82-240">When you specify hello record set toobe deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used tooensure concurrent changes are not deleted.</span></span> <span data-ttu-id="60b82-241">Használhatja a választható hello `-Overwrite` kapcsoló toosuppress ezen ellenőrzések.</span><span class="sxs-lookup"><span data-stu-id="60b82-241">You can use hello optional `-Overwrite` switch toosuppress these checks.</span></span>

<span data-ttu-id="60b82-242">hello rekordhalmaz objektum is átirányítható paraméterként átadott helyett:</span><span class="sxs-lookup"><span data-stu-id="60b82-242">hello record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="60b82-243">Megerősítés</span><span class="sxs-lookup"><span data-stu-id="60b82-243">Confirmation prompts</span></span>

<span data-ttu-id="60b82-244">Hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, és `Remove-AzureRmDnsRecordSet` parancsmagok minden megerősítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="60b82-244">hello `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="60b82-245">Minden parancsmagot jóváhagyást kérni fogja, ha hello `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb.</span><span class="sxs-lookup"><span data-stu-id="60b82-245">Each cmdlet prompts for confirmation if hello `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="60b82-246">Hello alapértelmezett értéke óta `$ConfirmPreference` van `High`, ezek az üzenetek nem kapnak, amikor hello az alapértelmezett beállításokkal PowerShell.</span><span class="sxs-lookup"><span data-stu-id="60b82-246">Since hello default value for `$ConfirmPreference` is `High`, these prompts are not given when using hello default PowerShell settings.</span></span>

<span data-ttu-id="60b82-247">Felülbírálhatja hello aktuális `$ConfirmPreference` hello használata beállítást `-Confirm` paraméter.</span><span class="sxs-lookup"><span data-stu-id="60b82-247">You can override hello current `$ConfirmPreference` setting using hello `-Confirm` parameter.</span></span> <span data-ttu-id="60b82-248">Ha megad `-Confirm` vagy `-Confirm:$True` , hello parancsmag megerősítést kér, mielőtt futtatja.</span><span class="sxs-lookup"><span data-stu-id="60b82-248">If you specify `-Confirm` or `-Confirm:$True` , hello cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="60b82-249">Ha megad `-Confirm:$False` , hello parancsmag nem figyelmeztet megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="60b82-249">If you specify `-Confirm:$False` , hello cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="60b82-250">További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="60b82-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="60b82-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="60b82-251">Next steps</span></span>

<span data-ttu-id="60b82-252">További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="60b82-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="60b82-253">Ismerje meg, hogyan túl[védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.</span><span class="sxs-lookup"><span data-stu-id="60b82-253">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="60b82-254">Felülvizsgálati hello [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="60b82-254">Review hello [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
