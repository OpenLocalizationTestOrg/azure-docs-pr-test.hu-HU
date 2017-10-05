---
title: "Az Azure DNS az Azure PowerShell DNS-rekordok kezelése |} Microsoft Docs"
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
ms.openlocfilehash: 2962e30e5d9c60b8e786e2ba79647cabfc5925cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-azure-powershell"></a><span data-ttu-id="9ea96-104">DNS-rekordok és az Azure PowerShell használata Azure DNS rekordhalmazok kezelése</span><span class="sxs-lookup"><span data-stu-id="9ea96-104">Manage DNS records and recordsets in Azure DNS using Azure PowerShell</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="9ea96-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="9ea96-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="9ea96-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="9ea96-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="9ea96-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="9ea96-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="9ea96-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="9ea96-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="9ea96-109">Ez a cikk bemutatja, hogyan kezelheti a DNS-rekordokat a DNS-zóna Azure PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="9ea96-109">This article shows you how to manage DNS records for your DNS zone by using Azure PowerShell.</span></span> <span data-ttu-id="9ea96-110">DNS-rekordokat is kezelhetők a platformfüggetlen használatával [Azure CLI](dns-operations-recordsets-cli.md) vagy a [Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9ea96-110">DNS records can also be managed by using the cross-platform [Azure CLI](dns-operations-recordsets-cli.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

<span data-ttu-id="9ea96-111">Ebben a cikkben szereplő példák azt feltételezik, hogy már rendelkezik [Azure PowerShell jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones.md).</span><span class="sxs-lookup"><span data-stu-id="9ea96-111">The examples in this article assume you have already [installed Azure PowerShell, signed in, and created a DNS zone](dns-operations-dnszones.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="9ea96-112">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9ea96-112">Introduction</span></span>

<span data-ttu-id="9ea96-113">Mielőtt létrehozná a DNS-rekordokat Azure DNS-ben, tisztában kell lennie azzal, hogyan rendezi az Azure DNS DNS-rekordhalmazokba a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="9ea96-113">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="9ea96-114">Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="9ea96-114">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>


## <a name="create-a-new-dns-record"></a><span data-ttu-id="9ea96-115">Hozzon létre egy új DNS-rekord</span><span class="sxs-lookup"><span data-stu-id="9ea96-115">Create a new DNS record</span></span>

<span data-ttu-id="9ea96-116">Ha az új rekord tartalmazza-e ilyen nevű és típusú, mint egy már létező rekordot, akkor [vegye fel a meglévő rekordkészlet](#add-a-record-to-an-existing-record-set).</span><span class="sxs-lookup"><span data-stu-id="9ea96-116">If your new record has the same name and type as an existing record, you need to [add it to the existing record set](#add-a-record-to-an-existing-record-set).</span></span> <span data-ttu-id="9ea96-117">Ha az új rekord tartalmazza-e egy másik nevet, az összes meglévő rekordok be, akkor kell új rekord létrehozása.</span><span class="sxs-lookup"><span data-stu-id="9ea96-117">If your new record has a different name and type to all existing records, you need to create a new record set.</span></span> 

### <a name="create-a-records-in-a-new-record-set"></a><span data-ttu-id="9ea96-118">"A" rekordokat létrehozni egy új rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="9ea96-118">Create 'A' records in a new record set</span></span>

<span data-ttu-id="9ea96-119">Rekordhalmazt a `New-AzureRmDnsRecordSet` parancsmag használatával hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="9ea96-119">You create record sets by using the `New-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="9ea96-120">A rekordhalmaz létrehozásakor meg kell adnia a rekordhalmaz a nevét, a zóna a, az élettartam (TTL), a rekordtípust, és a rekordokat létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9ea96-120">When creating a record set, you need to specify the record set name, the zone, the time to live (TTL), the record type, and the records to be created.</span></span>

<span data-ttu-id="9ea96-121">A rekordok rekordhalmazhoz adásának paraméterei a rekordhalmaz típusától függően eltérnek.</span><span class="sxs-lookup"><span data-stu-id="9ea96-121">The parameters for adding records to a record set vary depending on the type of the record set.</span></span> <span data-ttu-id="9ea96-122">Például egy "A" típusú rekordkészlet használatakor kell az IP-cím, a paraméter használatával `-IPv4Address`.</span><span class="sxs-lookup"><span data-stu-id="9ea96-122">For example, when using a record set of type 'A', you need to specify the IP address using the parameter `-IPv4Address`.</span></span> <span data-ttu-id="9ea96-123">Más paramétereket az egyéb típusú bejegyzés használnak.</span><span class="sxs-lookup"><span data-stu-id="9ea96-123">Other parameters are used for other record types.</span></span> <span data-ttu-id="9ea96-124">Lásd: [további rekordtípusokra](#additional-record-type-examples) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="9ea96-124">See [Additional record type examples](#additional-record-type-examples) for details.</span></span>

<span data-ttu-id="9ea96-125">Az alábbi példa létrehoz egy rekordot a DNS-zónában "contoso.com" a "www" relatív névvel beállítása.</span><span class="sxs-lookup"><span data-stu-id="9ea96-125">The following example creates a record set with the relative name 'www' in the DNS Zone 'contoso.com'.</span></span> <span data-ttu-id="9ea96-126">A rekordhalmaz teljesen minősített neve "www.contoso.com".</span><span class="sxs-lookup"><span data-stu-id="9ea96-126">The fully-qualified name of the record set is 'www.contoso.com'.</span></span> <span data-ttu-id="9ea96-127">A típus az "A", és az élettartam 3600 másodperc.</span><span class="sxs-lookup"><span data-stu-id="9ea96-127">The record type is 'A', and the TTL is 3600 seconds.</span></span> <span data-ttu-id="9ea96-128">A rekordhalmaz egyetlen rekordot, IP-cím "1.2.3.4" tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9ea96-128">The record set contains a single record, with IP address '1.2.3.4'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="9ea96-129">Állítsa be a "csúcs" zóna rekord létrehozásához (ebben az esetben "contoso.com"), használja a következő rekordhalmaznevet "@" (idézőjelek nélkül):</span><span class="sxs-lookup"><span data-stu-id="9ea96-129">To create a record set at the 'apex' of a zone (in this case, 'contoso.com'), use the record set name '@' (excluding quotation marks):</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") 
```

<span data-ttu-id="9ea96-130">Ha szeretné beállítani a több rekordot tartalmazó rekordot kell létrehozni, először hozzon létre egy helyi tömb adja hozzá a rekordokat, majd adja át a tömbben `New-AzureRmDnsRecordSet` az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="9ea96-130">If you need to create a record set containing more than one record, first create a local array and add the records, then pass the array to `New-AzureRmDnsRecordSet` as follows:</span></span>

```powershell
$aRecords = @()
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4"
$aRecords += New-AzureRmDnsRecordConfig -IPv4Address "2.3.4.5"
New-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName MyResourceGroup -Ttl 3600 -RecordType A -DnsRecords $aRecords
```

<span data-ttu-id="9ea96-131">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) rendelje hozzá az alkalmazás-specifikus adatok minden rekordhalmaz, a kulcs-érték párként használható.</span><span class="sxs-lookup"><span data-stu-id="9ea96-131">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="9ea96-132">A következő példa bemutatja, hogyan hozzon létre két metaadat-bejegyzést, rekordkészlet "osztály pénzügyi =" és "környezet éles =".</span><span class="sxs-lookup"><span data-stu-id="9ea96-132">The following example shows how to create a record set with two metadata entries, 'dept=finance' and 'environment=production'.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -IPv4Address "1.2.3.4") -Metadata @{ dept="finance"; environment="production" } 
```

<span data-ttu-id="9ea96-133">Az Azure DNS "empty" rekordhalmazok, amely működhet-e a DNS-név DNS-rekordok létrehozása előtt lefoglalni helyőrzőként is támogatja.</span><span class="sxs-lookup"><span data-stu-id="9ea96-133">Azure DNS also supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="9ea96-134">Üres rekordhalmazok az Azure DNS-vezérlő vezérlősík látható, de jelennek meg az Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="9ea96-134">Empty record sets are visible in the Azure DNS control plane, but do appear on the Azure DNS name servers.</span></span> <span data-ttu-id="9ea96-135">Az alábbi példa létrehoz egy üres rekordhalmaz:</span><span class="sxs-lookup"><span data-stu-id="9ea96-135">The following example creates an empty record set:</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords @()
```

## <a name="create-records-of-other-types"></a><span data-ttu-id="9ea96-136">Más típusú rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-136">Create records of other types</span></span>

<span data-ttu-id="9ea96-137">Hogy látott részletesen "A" rekordok létrehozása, a következő példák szemléltetik Azure DNS által támogatott más rekord típusú rekordok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-137">Having seen in detail how to create 'A' records, the following examples show how to create records of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="9ea96-138">Minden esetben megmutatjuk, hogyan rekordhalmaz egyetlen rekordot tartalmazó létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-138">In each case, we show how to create a record set containing a single record.</span></span> <span data-ttu-id="9ea96-139">A korábbi példák "A" rekordok hozzáigazítható üres rekordhalmazok létrehozásához vagy más típusú metaadatokkal, a több rekordot tartalmazó rekordhalmazok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-139">The earlier examples for 'A' records can be adapted to create record sets of other types containing multiple records, with metadata, or to create empty record sets.</span></span>

<span data-ttu-id="9ea96-140">Egy példa egy SOA típusú rekordhalmaz létrehozása óta SOAs jönnek létre nem felállításához és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön.</span><span class="sxs-lookup"><span data-stu-id="9ea96-140">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="9ea96-141">Azonban [módosíthatják a SOA, egy újabb példában látható módon](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="9ea96-141">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-142">Egyetlen rekordot tartalmazó AAAA típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-142">Create an AAAA record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-aaaa" -RecordType AAAA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ipv6Address "2607:f8b0:4009:1803::1005") 
```

### <a name="create-a-cname-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-143">Egyetlen rekordot tartalmazó CNAME típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-143">Create a CNAME record set with a single record</span></span>

> [!NOTE]
> <span data-ttu-id="9ea96-144">A DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna tetején (`-Name '@'`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="9ea96-144">The DNS standards do not permit CNAME records at the apex of a zone (`-Name '@'`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="9ea96-145">További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="9ea96-145">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "test-cname" -RecordType CNAME -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Cname "www.contoso.com") 
```

### <a name="create-an-mx-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-146">Egyetlen rekordot tartalmazó MX típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-146">Create an MX record set with a single record</span></span>

<span data-ttu-id="9ea96-147">Ebben a példában a következő rekordhalmaznevet használjuk "@" lehet létrehozni az MX-rekord a zóna felső pontja (ebben az esetben a "contoso.com").</span><span class="sxs-lookup"><span data-stu-id="9ea96-147">In this example, we use the record set name '@' to create an MX record at the zone apex (in this case, 'contoso.com').</span></span>


```powershell
New-AzureRmDnsRecordSet -Name "@" -RecordType MX -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Exchange "mail.contoso.com" -Preference 5) 
```

### <a name="create-an-ns-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-148">Egyetlen rekordot tartalmazó NS típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-148">Create an NS record set with a single record</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-ns" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Nsdname "ns1.contoso.com") 
```

### <a name="create-a-ptr-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-149">Egyetlen rekordot tartalmazó PTR típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-149">Create a PTR record set with a single record</span></span>

<span data-ttu-id="9ea96-150">Ebben az esetben "my-arpa-zónában (Zone.com)" a ARPA-névkeresési zónát, az IP-címtartomány képviselő jelöli.</span><span class="sxs-lookup"><span data-stu-id="9ea96-150">In this case, 'my-arpa-zone.com' represents the ARPA reverse lookup zone representing your IP range.</span></span> <span data-ttu-id="9ea96-151">A zóna minden PTR típusú rekordhalmaza az IP-címtartomány egyik IP-címének felel meg.</span><span class="sxs-lookup"><span data-stu-id="9ea96-151">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span> <span data-ttu-id="9ea96-152">A rekordnév a "10" Ez a bejegyzés által képviselt IP-címtartományon belül az IP-cím utolsó oktett.</span><span class="sxs-lookup"><span data-stu-id="9ea96-152">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name 10 -RecordType PTR -ZoneName "my-arpa-zone.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Ptrdname "myservice.contoso.com") 
```

### <a name="create-an-srv-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-153">Egyetlen rekordot tartalmazó SRV típusú rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-153">Create an SRV record set with a single record</span></span>

<span data-ttu-id="9ea96-154">Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a  *\_szolgáltatás* és  *\_protokoll* a rekordhalmaz-neve.</span><span class="sxs-lookup"><span data-stu-id="9ea96-154">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="9ea96-155">Nincs szükség felvenni "@" a rekordhalmaz nevében, ha a zóna felső pontja az SRV rekord létrehozása beállítva.</span><span class="sxs-lookup"><span data-stu-id="9ea96-155">There is no need to include '@' in the record set name when creating an SRV record set at the zone apex.</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "_sip._tls" -RecordType SRV -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Priority 0 -Weight 5 -Port 8080 -Target "sip.contoso.com") 
```


### <a name="create-a-txt-record-set-with-a-single-record"></a><span data-ttu-id="9ea96-156">Egyetlen rekordot tartalmazó TXT rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="9ea96-156">Create a TXT record set with a single record</span></span>

<span data-ttu-id="9ea96-157">A következő példa bemutatja, hogyan TXT rekord létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-157">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="9ea96-158">A maximális hossz támogatja a TXT-rekord kapcsolatos további információkért lásd: [TXT rekord](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="9ea96-158">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```powershell
New-AzureRmDnsRecordSet -Name "test-txt" -RecordType TXT -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -Ttl 3600 -DnsRecords (New-AzureRmDnsRecordConfig -Value "This is a TXT record") 
```


## <a name="get-a-record-set"></a><span data-ttu-id="9ea96-159">Rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ea96-159">Get a record set</span></span>

<span data-ttu-id="9ea96-160">Egy meglévő rekordhalmaz lekéréséhez használja `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="9ea96-160">To retrieve an existing record set, use `Get-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="9ea96-161">Ez a parancsmag egy helyi objektum az az Azure DNS-rekordot jelölő adja vissza.</span><span class="sxs-lookup"><span data-stu-id="9ea96-161">This cmdlet returns a local object that represents the record set in Azure DNS.</span></span>

<span data-ttu-id="9ea96-162">A `New-AzureRmDnsRecordSet`, a megadott rekordhalmaz nevének kell lennie egy *relatív* neve, ami azt jelenti, azt kell zárnia a zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="9ea96-162">As with `New-AzureRmDnsRecordSet`, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="9ea96-163">Is meg kell adnia a rekordtípust, és a zóna a rekordot tartalmazó állítsa be.</span><span class="sxs-lookup"><span data-stu-id="9ea96-163">You also need to specify the record type, and the zone containing the record set.</span></span>

<span data-ttu-id="9ea96-164">A következő példa bemutatja, hogyan rekordkészlet beolvasása.</span><span class="sxs-lookup"><span data-stu-id="9ea96-164">The following example shows how to retrieve a record set.</span></span> <span data-ttu-id="9ea96-165">Ebben a példában a zóna adott meg a `-ZoneName` és `-ResourceGroupName` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="9ea96-165">In this example, the zone is specified using the `-ZoneName` and `-ResourceGroupName` parameters.</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="9ea96-166">Másik lehetőségként azt is megadhatja a zóna egy zóna objektummal, használja az átadott a `-Zone` paraméter.</span><span class="sxs-lookup"><span data-stu-id="9ea96-166">Alternatively, you can also specify the zone using a zone object, passed using the `-Zone` parameter.</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs = Get-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

## <a name="list-record-sets"></a><span data-ttu-id="9ea96-167">Lista rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="9ea96-167">List record sets</span></span>

<span data-ttu-id="9ea96-168">Is `Get-AzureRmDnsZone` lista parancs zónában, kihagyva a `-Name` és/vagy `-RecordType` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="9ea96-168">You can also use `Get-AzureRmDnsZone` to list record sets in a zone, by omitting the `-Name` and/or `-RecordType` parameters.</span></span>

<span data-ttu-id="9ea96-169">Az alábbi példa adja vissza, az összes rekordot a zónában állítja be:</span><span class="sxs-lookup"><span data-stu-id="9ea96-169">The following example returns all record sets in the zone:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="9ea96-170">A következő példa bemutatja, hogyan összes rekordkészletek egy adott típusú lekérhető a rekordtípus megadásával, amíg a rekord kihagyásával nevének megadása:</span><span class="sxs-lookup"><span data-stu-id="9ea96-170">The following example shows how all record sets of a given type can be retrieved by specifying the record type while omitting the record set name:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="9ea96-171">Ezzel a névvel, minden rekordkészletet típusok között lekéréséhez kell minden rekordkészletet kérnie és el kell majd az eredmények szűréséhez:</span><span class="sxs-lookup"><span data-stu-id="9ea96-171">To retrieve all record sets with a given name, across record types, you need to retrieve all record sets and then filter the results:</span></span>

```powershell
$recordsets = Get-AzureRmDnsRecordSet -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | where {$_.Name.Equals("www")}
```

<span data-ttu-id="9ea96-172">Az összes fenti példákban a zóna adható meg valamelyikét a `-ZoneName` és `-ResourceGroupName`paraméterek (ahogy), vagy adjon meg egy zóna objektum:</span><span class="sxs-lookup"><span data-stu-id="9ea96-172">In all the above examples, the zone can be specified either by using the `-ZoneName` and `-ResourceGroupName`parameters (as shown), or by specifying a zone object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
$recordsets = Get-AzureRmDnsRecordSet -Zone $zone
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="9ea96-173">Rekord hozzáadása egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="9ea96-173">Add a record to an existing record set</span></span>

<span data-ttu-id="9ea96-174">Egy meglévő rekordhalmaz hozzáadni egy rekordot, kövesse az alábbi három lépést:</span><span class="sxs-lookup"><span data-stu-id="9ea96-174">To add a record to an existing record set, follow the following three steps:</span></span>

1. <span data-ttu-id="9ea96-175">A meglévő rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ea96-175">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="9ea96-176">Az új bejegyzés hozzáadása a helyi rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="9ea96-176">Add the new record to the local record set.</span></span> <span data-ttu-id="9ea96-177">Ez az offline művelet.</span><span class="sxs-lookup"><span data-stu-id="9ea96-177">This is an off-line operation.</span></span>

    ```powershell
    Add-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="9ea96-178">A módosítás véglegesítése vissza az Azure DNS szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9ea96-178">Commit the change back to the Azure DNS service.</span></span> 

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $rs
    ```

<span data-ttu-id="9ea96-179">Használatával `Set-AzureRmDnsRecordSet` *lecseréli* a meglévő Azure DNS-ben (és a benne található összes rekord) tartalmazó rekordhalmaz megadott rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="9ea96-179">Using `Set-AzureRmDnsRecordSet` *replaces* the existing record set in Azure DNS (and all records it contains) with the record set specified.</span></span> <span data-ttu-id="9ea96-180">[ETag ellenőrzések](dns-zones-records.md#etags) biztosítják az egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="9ea96-180">[Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="9ea96-181">A választható használható `-Overwrite` kapcsoló ezen ellenőrzések letiltásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-181">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="9ea96-182">Ez a feladatütemezési műveletek is lehet *adatcsatornán*, ami azt jelenti, használja a csőről való átadása paraméterként helyett adja meg a rekordhalmaz objektum:</span><span class="sxs-lookup"><span data-stu-id="9ea96-182">This sequence of operations can also be *piped*, meaning you pass the record set object by using the pipe rather than passing it as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name "www" –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Add-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="9ea96-183">A fenti példák bemutatják, hogyan egy "A" bejegyzés hozzáadása egy meglévő rekordhalmaz "A" típusú.</span><span class="sxs-lookup"><span data-stu-id="9ea96-183">The examples above show how to add an 'A' record to an existing record set of type 'A'.</span></span> <span data-ttu-id="9ea96-184">A hasonló feladatütemezési műveletek használatával adja hozzá rekordokat rekordhalmazok más típusú, és a `-Ipv4Address` paramétere `Add-AzureRmDnsRecordConfig` különböző rekordtípusú jellemző más paraméterekkel.</span><span class="sxs-lookup"><span data-stu-id="9ea96-184">A similar sequence of operations is used to add records to record sets of other types, substituting the `-Ipv4Address` parameter of `Add-AzureRmDnsRecordConfig` with other parameters specific to each record type.</span></span> <span data-ttu-id="9ea96-185">Az egyes rekordok paraméterei megegyezik a a `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.</span><span class="sxs-lookup"><span data-stu-id="9ea96-185">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>

<span data-ttu-id="9ea96-186">"CNAME" vagy "SOA" típusú rekordhalmazok nem tartalmazhat egynél több rekordot.</span><span class="sxs-lookup"><span data-stu-id="9ea96-186">Record sets of type 'CNAME' or 'SOA' cannot contain more than one record.</span></span> <span data-ttu-id="9ea96-187">Ennél a határértéknél a DNS-szabványokból ered.</span><span class="sxs-lookup"><span data-stu-id="9ea96-187">This constraint arises from the DNS standards.</span></span> <span data-ttu-id="9ea96-188">Az Azure DNS korlátozása nincs.</span><span class="sxs-lookup"><span data-stu-id="9ea96-188">It is not a limitation of Azure DNS.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="9ea96-189">Távolítsa el a rekord egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="9ea96-189">Remove a record from an existing record set</span></span>

<span data-ttu-id="9ea96-190">A folyamat egy olyan rekordot eltávolítása rekordhalmaz hozzáadni egy rekordot egy meglévő rekordhalmaz a folyamat hasonlít:</span><span class="sxs-lookup"><span data-stu-id="9ea96-190">The process to remove a record from a record set is similar to the process to add a record to an existing record set:</span></span>

1. <span data-ttu-id="9ea96-191">A meglévő rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="9ea96-191">Get the existing record set</span></span>

    ```powershell
    $rs = Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A
    ```

2. <span data-ttu-id="9ea96-192">A helyi rekordhalmaz objektum adatainak eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="9ea96-192">Remove the record from the local record set object.</span></span> <span data-ttu-id="9ea96-193">Ez az offline művelet.</span><span class="sxs-lookup"><span data-stu-id="9ea96-193">This is an off-line operation.</span></span> <span data-ttu-id="9ea96-194">Az eltávolított kell bejegyzést terméknévnek pontosan egyeznie kell a meglévő bejegyzés minden paraméterek között.</span><span class="sxs-lookup"><span data-stu-id="9ea96-194">The record that's being removed must be an exact match with an existing record across all parameters.</span></span>

    ```powershell
    Remove-AzureRmDnsRecordConfig -RecordSet $rs -Ipv4Address "5.6.7.8"
    ```

3. <span data-ttu-id="9ea96-195">A módosítás véglegesítése vissza az Azure DNS szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="9ea96-195">Commit the change back to the Azure DNS service.</span></span> <span data-ttu-id="9ea96-196">Használja az opcionális `-Overwrite` kapcsolót, hogy ne jelenjen meg többé [Etag ellenőrzi](dns-zones-records.md#etags) egyidejű változásait.</span><span class="sxs-lookup"><span data-stu-id="9ea96-196">Use the optional `-Overwrite` switch to suppress [Etag checks](dns-zones-records.md#etags) for concurrent changes.</span></span>

    ```powershell
    Set-AzureRmDnsRecordSet -RecordSet $Rs
    ```

<span data-ttu-id="9ea96-197">A fenti feladatütemezési segítségével rekordhalmaz utolsó adatainak eltávolítása nem törli a rekordhalmaz, inkább egy üres rekordhalmaz elhagyják.</span><span class="sxs-lookup"><span data-stu-id="9ea96-197">Using the above sequence to remove the last record from a record set does not delete the record set, rather it leaves an empty record set.</span></span> <span data-ttu-id="9ea96-198">A rekordhalmaz teljesen eltávolítja, lásd: [rekordhalmaz törlése](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="9ea96-198">To remove a record set entirely, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="9ea96-199">Hasonló módon való hozzáadásakor rekordok rekordhalmaz, a rekordhalmaz eltávolítása műveletek sorrendjét is átirányítható:</span><span class="sxs-lookup"><span data-stu-id="9ea96-199">Similarly to adding records to a record set, the sequence of operations to remove a record set can also be piped:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www –ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" -RecordType A | Remove-AzureRmDnsRecordConfig -Ipv4Address "5.6.7.8" | Set-AzureRmDnsRecordSet
```

<span data-ttu-id="9ea96-200">Különböző típusok támogatottak úgy, hogy a megfelelő típus-specifikus paramétereket `Remove-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="9ea96-200">Different record types are supported by passing the appropriate type-specific parameters to `Remove-AzureRmDnsRecordSet`.</span></span> <span data-ttu-id="9ea96-201">Az egyes rekordok paraméterei megegyezik a a `New-AzureRmDnsRecordConfig` parancsmag, ahogy az [további rekordtípusokra](#additional-record-type-examples) felett.</span><span class="sxs-lookup"><span data-stu-id="9ea96-201">The parameters for each record type are the same as for the `New-AzureRmDnsRecordConfig` cmdlet, as shown in [Additional record type examples](#additional-record-type-examples) above.</span></span>


## <a name="modify-an-existing-record-set"></a><span data-ttu-id="9ea96-202">Módosíthatja egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="9ea96-202">Modify an existing record set</span></span>

<span data-ttu-id="9ea96-203">Egy meglévő rekordhalmaz módosítása lépései hasonlóak lépései hozzáadása vagy eltávolítása a bejegyzések rekordhalmaz:</span><span class="sxs-lookup"><span data-stu-id="9ea96-203">The steps for modifying an existing record set are similar to the steps you take when adding or removing records from a record set:</span></span>

1. <span data-ttu-id="9ea96-204">A meglévő rekordhalmazt beolvasása `Get-AzureRmDnsRecordSet`.</span><span class="sxs-lookup"><span data-stu-id="9ea96-204">Retrieve the existing record set by using `Get-AzureRmDnsRecordSet`.</span></span>
2. <span data-ttu-id="9ea96-205">A helyi rekordhalmaz objektum által módosítható:</span><span class="sxs-lookup"><span data-stu-id="9ea96-205">Modify the local record set object by:</span></span>
    * <span data-ttu-id="9ea96-206">Hozzáadása vagy eltávolítása a bejegyzések</span><span class="sxs-lookup"><span data-stu-id="9ea96-206">Adding or removing records</span></span>
    * <span data-ttu-id="9ea96-207">A meglévő rekordok paraméterek módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-207">Changing the parameters of existing records</span></span>
    * <span data-ttu-id="9ea96-208">Élettartam (TTL) a metaadatok és az idő beállítása a rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-208">Changing the record set metadata and time to live (TTL)</span></span>
3. <span data-ttu-id="9ea96-209">A változtatások véglegesítése a határidő használatával a `Set-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="9ea96-209">Commit your changes by using the `Set-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="9ea96-210">Ez *lecseréli* a meglévő Azure DNS-ben tartalmazó rekordhalmaz megadott rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="9ea96-210">This *replaces* the existing record set in Azure DNS with the record set specified.</span></span>

<span data-ttu-id="9ea96-211">Használata esetén `Set-AzureRmDnsRecordSet`, [Etag ellenőrzi](dns-zones-records.md#etags) biztosítják az egyidejű változtatások nem íródnak felül.</span><span class="sxs-lookup"><span data-stu-id="9ea96-211">When using `Set-AzureRmDnsRecordSet`, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not overwritten.</span></span> <span data-ttu-id="9ea96-212">A választható használható `-Overwrite` kapcsoló ezen ellenőrzések letiltásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-212">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

### <a name="to-update-a-record-in-an-existing-record-set"></a><span data-ttu-id="9ea96-213">Egy meglévő rekordhalmaz egy rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-213">To update a record in an existing record set</span></span>

<span data-ttu-id="9ea96-214">Ebben a példában az IP-cím, egy létező "A" rekord módosítjuk:</span><span class="sxs-lookup"><span data-stu-id="9ea96-214">In this example, we change the IP address of an existing 'A' record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Ipv4Address = "9.8.7.6"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="9ea96-215">Egy SOA-rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-215">To modify an SOA record</span></span>

<span data-ttu-id="9ea96-216">Nem adja hozzá vagy rekordok eltávolítása az automatikusan létrehozott SOA típusú rekordját, állítsa be a zóna felső pontja (`-Name "@"`, beleértve az ajánlat jelek).</span><span class="sxs-lookup"><span data-stu-id="9ea96-216">You cannot add or remove records from the automatically created SOA record set at the zone apex (`-Name "@"`, including quote marks).</span></span> <span data-ttu-id="9ea96-217">Azonban módosíthatja a SOA típusú rekordját (kivéve az "állomás") belül paraméterek egyikét, és a rekordhalmaz TTL-t.</span><span class="sxs-lookup"><span data-stu-id="9ea96-217">However, you can modify any of the parameters within the SOA record (except "Host") and the record set TTL.</span></span>

<span data-ttu-id="9ea96-218">A következő példa bemutatja, hogyan módosíthatja a *E-mail* SOA típusú rekordjának tulajdonsága:</span><span class="sxs-lookup"><span data-stu-id="9ea96-218">The following example shows how to change the *Email* property of the SOA record:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType SOA -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
$rs.Records[0].Email = "admin.contoso.com"
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="9ea96-219">A zóna felső pontja a Névkiszolgálói rekordok módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-219">To modify NS records at the zone apex</span></span>

<span data-ttu-id="9ea96-220">Állítsa be a zóna felső pontja NS-rekord automatikusan létrejön minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="9ea96-220">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="9ea96-221">Az Azure DNS névkiszolgálóit, a zóna nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9ea96-221">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="9ea96-222">Hozzáadhat további névhez a kiszolgálók e NS-rekord, támogatja a párhuzamos üzemeltetési tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="9ea96-222">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="9ea96-223">A TTL-t és a metaadatok a rekordhalmaz is módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="9ea96-223">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="9ea96-224">Azonban nem lehet eltávolítani, vagy módosítsa az előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="9ea96-224">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="9ea96-225">Vegye figyelembe, hogy csak vonatkozik az NS típusú rekordhalmaz zóna tetején.</span><span class="sxs-lookup"><span data-stu-id="9ea96-225">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="9ea96-226">A zónában (a használt gyermekzónákhoz delegálása) más NS-rekordhalmazok lehet módosítani, korlátozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="9ea96-226">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="9ea96-227">A következő példa bemutatja, hogyan hozzáadása egy további nevét, a zóna felső pontja Névkiszolgálói rekordhalmazt:</span><span class="sxs-lookup"><span data-stu-id="9ea96-227">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Add-AzureRmDnsRecordConfig -RecordSet $rs -Nsdname ns1.myotherdnsprovider.com
Set-AzureRmDnsRecordSet -RecordSet $rs
```

### <a name="to-modify-record-set-metadata"></a><span data-ttu-id="9ea96-228">Rekordhalmaz metaadatok módosítása</span><span class="sxs-lookup"><span data-stu-id="9ea96-228">To modify record set metadata</span></span>

<span data-ttu-id="9ea96-229">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) rendelje hozzá az alkalmazás-specifikus adatok minden rekordhalmaz, a kulcs-érték párként használható.</span><span class="sxs-lookup"><span data-stu-id="9ea96-229">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="9ea96-230">A következő példa bemutatja, hogyan lehet módosítani egy meglévő rekordhalmaz metaadatait:</span><span class="sxs-lookup"><span data-stu-id="9ea96-230">The following example shows how to modify the metadata of an existing record set:</span></span>

```powershell
# Get the record set
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"

# Add 'dept=finance' name-value pair
$rs.Metadata.Add('dept', 'finance') 

# Remove metadata item named 'environment'
$rs.Metadata.Remove('environment')  

# Commit changes
Set-AzureRmDnsRecordSet -RecordSet $rs
```


## <a name="delete-a-record-set"></a><span data-ttu-id="9ea96-231">A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="9ea96-231">Delete a record set</span></span>

<span data-ttu-id="9ea96-232">Rekordhalmazok használatával törölheti a `Remove-AzureRmDnsRecordSet` parancsmag.</span><span class="sxs-lookup"><span data-stu-id="9ea96-232">Record sets can be deleted by using the `Remove-AzureRmDnsRecordSet` cmdlet.</span></span> <span data-ttu-id="9ea96-233">A rekordhalmaz összes rekordján is rekordhalmaz törlése törli.</span><span class="sxs-lookup"><span data-stu-id="9ea96-233">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="9ea96-234">Nem lehet törölni a SOA és NS-rekord beállítja a zóna felső pontja (`-Name '@'`).</span><span class="sxs-lookup"><span data-stu-id="9ea96-234">You cannot delete the SOA and NS record sets at the zone apex (`-Name '@'`).</span></span>  <span data-ttu-id="9ea96-235">Az Azure DNS automatikusan létrehozza ezeket a beállítást, ha a zóna lett létrehozva, és automatikusan törli azokat az a zóna törlődik.</span><span class="sxs-lookup"><span data-stu-id="9ea96-235">Azure DNS created these automatically when the zone was created, and deletes them automatically when the zone is deleted.</span></span>

<span data-ttu-id="9ea96-236">A következő példa bemutatja, hogyan rekordhalmaz törlése.</span><span class="sxs-lookup"><span data-stu-id="9ea96-236">The following example shows how to delete a record set.</span></span> <span data-ttu-id="9ea96-237">Ebben a példában a rekordhalmazának neve, a rekordhalmaz típusa, a zóna nevét és a erőforráscsoport minden egyes megadott explicit módon.</span><span class="sxs-lookup"><span data-stu-id="9ea96-237">In this example, the record set name, record set type, zone name, and resource group are each specified explicitly.</span></span>

```powershell
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
```

<span data-ttu-id="9ea96-238">Másik lehetőségként a rekordhalmaz neve és típusa és egy objektummal megadott zóna adható meg:</span><span class="sxs-lookup"><span data-stu-id="9ea96-238">Alternatively, the record set can be specified by name and type, and the zone specified using an object:</span></span>

```powershell
$zone = Get-AzureRmDnsZone -Name "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -Name "www" -RecordType A -Zone $zone
```

<span data-ttu-id="9ea96-239">Egy harmadik beállítás, mert a rekordhalmaz maga adható meg egy rekordhalmaz-objektummal:</span><span class="sxs-lookup"><span data-stu-id="9ea96-239">As a third option, the record set itself can be specified using a record set object:</span></span>

```powershell
$rs = Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup"
Remove-AzureRmDnsRecordSet -RecordSet $rs
```

<span data-ttu-id="9ea96-240">Amikor megad egy rekordhalmaz objektum használatával a törlendő rekordhalmaz [Etag ellenőrzi](dns-zones-records.md#etags) biztosítják az egyidejű változtatások nem törlődnek.</span><span class="sxs-lookup"><span data-stu-id="9ea96-240">When you specify the record set to be deleted by using a record set object, [Etag checks](dns-zones-records.md#etags) are used to ensure concurrent changes are not deleted.</span></span> <span data-ttu-id="9ea96-241">A választható használható `-Overwrite` kapcsoló ezen ellenőrzések letiltásához.</span><span class="sxs-lookup"><span data-stu-id="9ea96-241">You can use the optional `-Overwrite` switch to suppress these checks.</span></span>

<span data-ttu-id="9ea96-242">A rekordhalmaz objektum is átirányítható paraméterként átadott helyett:</span><span class="sxs-lookup"><span data-stu-id="9ea96-242">The record set object can also be piped instead of being passed as a parameter:</span></span>

```powershell
Get-AzureRmDnsRecordSet -Name www -RecordType A -ZoneName "contoso.com" -ResourceGroupName "MyResourceGroup" | Remove-AzureRmDnsRecordSet
```

## <a name="confirmation-prompts"></a><span data-ttu-id="9ea96-243">Megerősítés</span><span class="sxs-lookup"><span data-stu-id="9ea96-243">Confirmation prompts</span></span>

<span data-ttu-id="9ea96-244">A `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, és `Remove-AzureRmDnsRecordSet` parancsmagok minden megerősítés támogatja.</span><span class="sxs-lookup"><span data-stu-id="9ea96-244">The `New-AzureRmDnsRecordSet`, `Set-AzureRmDnsRecordSet`, and `Remove-AzureRmDnsRecordSet` cmdlets all support confirmation prompts.</span></span>

<span data-ttu-id="9ea96-245">Minden parancsmagot jóváhagyást kérni fogja, ha a `$ConfirmPreference` PowerShell preferenciaváltozót értéke `Medium` vagy alacsonyabb.</span><span class="sxs-lookup"><span data-stu-id="9ea96-245">Each cmdlet prompts for confirmation if the `$ConfirmPreference` PowerShell preference variable has a value of `Medium` or lower.</span></span> <span data-ttu-id="9ea96-246">Az alapértelmezett érték óta `$ConfirmPreference` van `High`, ezek az üzenetek nem adja az alapbeállításokat PowerShell használata esetén.</span><span class="sxs-lookup"><span data-stu-id="9ea96-246">Since the default value for `$ConfirmPreference` is `High`, these prompts are not given when using the default PowerShell settings.</span></span>

<span data-ttu-id="9ea96-247">Ha szeretné felülbírálni az aktuális `$ConfirmPreference` használatának beállítása a `-Confirm` paraméter.</span><span class="sxs-lookup"><span data-stu-id="9ea96-247">You can override the current `$ConfirmPreference` setting using the `-Confirm` parameter.</span></span> <span data-ttu-id="9ea96-248">Ha megad `-Confirm` vagy `-Confirm:$True` , a parancsmag megerősítést kér, mielőtt futtatja.</span><span class="sxs-lookup"><span data-stu-id="9ea96-248">If you specify `-Confirm` or `-Confirm:$True` , the cmdlet prompts you for confirmation before it runs.</span></span> <span data-ttu-id="9ea96-249">Ha megad `-Confirm:$False` , a parancsmag nem figyelmeztet megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="9ea96-249">If you specify `-Confirm:$False` , the cmdlet does not prompt you for confirmation.</span></span> 

<span data-ttu-id="9ea96-250">További információ `-Confirm` és `$ConfirmPreference`, lásd: [kapcsolatos Preferenciaváltozók](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span><span class="sxs-lookup"><span data-stu-id="9ea96-250">For more information about `-Confirm` and `$ConfirmPreference`, see [About Preference Variables](https://msdn.microsoft.com/powershell/reference/5.1/Microsoft.PowerShell.Core/about/about_Preference_Variables).</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ea96-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ea96-251">Next steps</span></span>

<span data-ttu-id="9ea96-252">További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="9ea96-252">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="9ea96-253">Megtudhatja, hogyan [védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.</span><span class="sxs-lookup"><span data-stu-id="9ea96-253">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
<br>
<span data-ttu-id="9ea96-254">Tekintse át a [Azure DNS PowerShell referenciadokumentációt](/powershell/module/azurerm.dns).</span><span class="sxs-lookup"><span data-stu-id="9ea96-254">Review the [Azure DNS PowerShell reference documentation](/powershell/module/azurerm.dns).</span></span>
