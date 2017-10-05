---
title: "Az Azure DNS az Azure CLI 1.0 használata DNS-rekordok kezelése |} Microsoft Docs"
description: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése esetén az Azure DNS-tartomány. Minden CLI 1.0 parancsok rekordhalmazokat és rekordokat műveleteket."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/20/2016
ms.author: jonatul
ms.openlocfilehash: 307b327e4c04a0461e39930114eb193791cbda9a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-dns-records-in-azure-dns-using-the-azure-cli-10"></a><span data-ttu-id="8886d-104">Az Azure DNS az Azure CLI 1.0 használata DNS-rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="8886d-104">Manage DNS records in Azure DNS using the Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="8886d-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="8886d-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="8886d-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="8886d-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="8886d-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="8886d-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="8886d-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="8886d-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="8886d-109">Ez a cikk bemutatja, hogyan DNS-rekordokat a DNS-zóna kezelése a platformok közötti Azure parancssori felület (CLI), amely megtalálható a Windows, Mac és Linux használatával.</span><span class="sxs-lookup"><span data-stu-id="8886d-109">This article shows you how to manage DNS records for your DNS zone by using the cross-platform Azure command-line interface (CLI), which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="8886d-110">A DNS-rekordok segítségével is kezelheti [Azure PowerShell](dns-operations-recordsets.md) vagy a [Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="8886d-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or the [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="8886d-111">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="8886d-111">CLI versions to complete the task</span></span>

<span data-ttu-id="8886d-112">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="8886d-112">You can complete the task using one of the following CLI versions:</span></span>

* <span data-ttu-id="8886d-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) – parancssori felületünk a klasszikus és a Resource Management üzemi modellekhez.</span><span class="sxs-lookup"><span data-stu-id="8886d-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for the classic and resource management deployment models.</span></span>
* <span data-ttu-id="8886d-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="8886d-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for the resource management deployment model.</span></span>

<span data-ttu-id="8886d-115">Ebben a cikkben szereplő példák azt feltételezik, hogy már rendelkezik [az Azure CLI 1.0-s jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="8886d-115">The examples in this article assume you have already [installed the Azure CLI 1.0, signed in, and created a DNS zone](dns-operations-dnszones-cli-nodejs.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="8886d-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="8886d-116">Introduction</span></span>

<span data-ttu-id="8886d-117">Mielőtt létrehozná a DNS-rekordokat Azure DNS-ben, tisztában kell lennie azzal, hogyan rendezi az Azure DNS DNS-rekordhalmazokba a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="8886d-117">Before creating DNS records in Azure DNS, you first need to understand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="8886d-118">Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="8886d-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="8886d-119">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-119">Create a DNS record</span></span>

<span data-ttu-id="8886d-120">DNS-rekordokat az `azure network dns record-set add-record` paranccsal lehet létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8886d-120">To create a DNS record, use the `azure network dns record-set add-record` command.</span></span> <span data-ttu-id="8886d-121">További segítségért lásd: `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-121">For help, see `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="8886d-122">Egy rekord létrehozásakor meg kell adni az erőforráscsoport, a zóna és a rekordhalmaz nevét, a rekordtípust és a létrehozandó rekord részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="8886d-122">When creating a record, you need to specify the resource group name, zone name, record set name, the record type, and the details of the record being created.</span></span> <span data-ttu-id="8886d-123">A megadott rekordhalmaz nevének kell lennie egy *relatív* neve, ami azt jelenti, azt kell zárnia a zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="8886d-123">The record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span>

<span data-ttu-id="8886d-124">Ha a rekordhalmaz még nem létezik, akkor a parancs létrehozza.</span><span class="sxs-lookup"><span data-stu-id="8886d-124">If the record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="8886d-125">Ha a rekordkészlet már létezik, a parancs adda a meg kell adnia a meglévő rekord rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="8886d-125">If the record set already exists, this command adda the record you specify to the existing record set.</span></span>

<span data-ttu-id="8886d-126">Új rekordhalmaz létrehozásakor az alapértelmezett élettartam (time-to-live, TTL) értéke 3600 lesz.</span><span class="sxs-lookup"><span data-stu-id="8886d-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="8886d-127">Különböző TTLs használatával, lásd: [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="8886d-127">For instructions on how to use different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="8886d-128">Az alábbi példaparancs a *MyResourceGroup* erőforráscsoport *contoso.com* zónájában egy *www* nevű, „A” típusú rekordot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="8886d-128">The following example creates an A record called *www* in the zone *contoso.com* in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="8886d-129">Az „A” rekord IP-címe: *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="8886d-129">The IP address of the A record is *1.2.3.4*.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

<span data-ttu-id="8886d-130">A zóna tetején a rekord létrehozásához (ebben az esetben "a contoso.com"), használja az "@", idézőjelekkel együtt:</span><span class="sxs-lookup"><span data-stu-id="8886d-130">To create a record in the apex of the zone (in this case, "contoso.com"), use the record name "@", including the quotation marks:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="8886d-131">A DNS-rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-131">Create a DNS record set</span></span>

<span data-ttu-id="8886d-132">A fenti példák a DNS-rekord vagy adtak hozzá egy meglévő rekordhalmaz, vagy a rekordhalmaz létrehozásának *implicit módon*.</span><span class="sxs-lookup"><span data-stu-id="8886d-132">In the above examples, the DNS record was either added to an existing record set, or the record set was created *implicitly*.</span></span> <span data-ttu-id="8886d-133">A rekordhalmaz is létrehozhat *explicit módon* rekordok hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="8886d-133">You can also create the record set *explicitly* before adding records to it.</span></span> <span data-ttu-id="8886d-134">Az Azure DNS támogatja az "empty" rekordhalmazok, amely működhet, és egy helyőrző lefoglalhat egy DNS-nevet a DNS-rekordok létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="8886d-134">Azure DNS supports 'empty' record sets, which can act as a placeholder to reserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="8886d-135">Üres rekordhalmazok az Azure DNS-vezérlő vezérlősík látható, de nem jelennek meg az Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="8886d-135">Empty record sets are visible in the Azure DNS control plane, but do not appear on the Azure DNS name servers.</span></span>

<span data-ttu-id="8886d-136">Rekordhalmazok használatával hozhatók létre a `azure network dns record-set create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8886d-136">Record sets are created using the `azure network dns record-set create` command.</span></span> <span data-ttu-id="8886d-137">További segítségért lásd: `azure network dns record-set create -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-137">For help, see `azure network dns record-set create -h`.</span></span>

<span data-ttu-id="8886d-138">Explicit módon a rekordhalmaz létrehozása lehetővé teszi annak megadását, a rekordhalmaz tulajdonságok, mint a [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live) és metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="8886d-138">Creating the record set explicitly allows you to specify record set properties such as the [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="8886d-139">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) rendelje hozzá az alkalmazás-specifikus adatok minden rekordhalmaz, a kulcs-érték párként használható.</span><span class="sxs-lookup"><span data-stu-id="8886d-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="8886d-140">Az alábbi példa létrehoz egy üres rekordhalmaz 60 másodperces TTL, segítségével a `--ttl` paraméter (rövid alak `-l`):</span><span class="sxs-lookup"><span data-stu-id="8886d-140">The following example creates an empty record set with a 60-second TTL, by using the `--ttl` parameter (short form `-l`):</span></span>

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

<span data-ttu-id="8886d-141">Az alábbi példa létrehoz két metaadat-bejegyzést, rekordkészlet "osztály pénzügyi =" és "környezet éles =", segítségével a `--metadata` paraméter (rövid alak `-m`):</span><span class="sxs-lookup"><span data-stu-id="8886d-141">The following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using the `--metadata` parameter (short form `-m`):</span></span>

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

<span data-ttu-id="8886d-142">Egy üres rekordhalmaz hozunk létre, rekordok segítségével adhatók `azure network dns record-set add-record` leírtak [hozzon létre egy DNS-rekord](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="8886d-142">Having created an empty record set, records can be added using `azure network dns record-set add-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="8886d-143">Más típusú rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-143">Create records of other types</span></span>

<span data-ttu-id="8886d-144">Hogy látott részletesen "A" rekordok létrehozása, a következő példák szemléltetik Azure DNS által támogatott más rekord típusú rekordot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8886d-144">Having seen in detail how to create 'A' records, the following examples show how to create record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="8886d-145">A rekordadatok megadásához használt paraméterek a rekord típusától függnek.</span><span class="sxs-lookup"><span data-stu-id="8886d-145">The parameters used to specify the record data vary depending on the type of the record.</span></span> <span data-ttu-id="8886d-146">Az „A” típusú rekordok esetén például a `-a <IPv4 address>` paraméterrel lehet megadni az IPv4-címet.</span><span class="sxs-lookup"><span data-stu-id="8886d-146">For example, for a record of type "A", you specify the IPv4 address with the parameter `-a <IPv4 address>`.</span></span> <span data-ttu-id="8886d-147">A különböző rekordtípusú paramétereinek is listázva lehet használatával `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-147">The parameters for each record type can be listed using `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="8886d-148">Minden esetben azt mutatják be egyetlen rekordot kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="8886d-148">In each case, we show how to create a single record.</span></span> <span data-ttu-id="8886d-149">A bejegyzés kerül a meglévő rekordkészlet vagy implicit módon létrehozott rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="8886d-149">The record is added to the existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="8886d-150">További információ a rekordhalmazok létrehozásához, és definiáló rekordot paraméter explicit módon, olvassa el [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="8886d-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="8886d-151">Egy példa egy SOA típusú rekordhalmaz létrehozása óta SOAs jönnek létre nem felállításához és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön.</span><span class="sxs-lookup"><span data-stu-id="8886d-151">We do not give an example to create an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="8886d-152">Azonban [módosíthatják a SOA, egy újabb példában látható módon](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="8886d-152">However, [the SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="8886d-153">Az AAAA-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-153">Create an AAAA record</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="8886d-154">Hozzon létre egy CNAME rekordot</span><span class="sxs-lookup"><span data-stu-id="8886d-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="8886d-155">A DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna tetején (`-Name "@"`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="8886d-155">The DNS standards do not permit CNAME records at the apex of a zone (`-Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="8886d-156">További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="8886d-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="8886d-157">Az MX-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-157">Create an MX record</span></span>

<span data-ttu-id="8886d-158">Ebben a példában a „@” rekordhalmaznevet használjuk az MX-rekord zóna felső pontjánál történő létrehozásához (amely ebben az esetben: contoso.com).</span><span class="sxs-lookup"><span data-stu-id="8886d-158">In this example, we use the record set name "@" to create the MX record at the zone apex (in this case, "contoso.com").</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="8886d-159">NS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-159">Create an NS record</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="8886d-160">A PTR típusú rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-160">Create a PTR record</span></span>

<span data-ttu-id="8886d-161">Ebben az esetben "my-arpa-zónában (Zone.com)" a ARPA zóna az IP-címtartomány képviselő jelöli.</span><span class="sxs-lookup"><span data-stu-id="8886d-161">In this case, 'my-arpa-zone.com' represents the ARPA zone representing your IP range.</span></span> <span data-ttu-id="8886d-162">A zóna minden PTR típusú rekordhalmaza az IP-címtartomány egyik IP-címének felel meg.</span><span class="sxs-lookup"><span data-stu-id="8886d-162">Each PTR record set in this zone corresponds to an IP address within this IP range.</span></span>  <span data-ttu-id="8886d-163">A rekordnév a "10" Ez a bejegyzés által képviselt IP-címtartományon belül az IP-cím utolsó oktett.</span><span class="sxs-lookup"><span data-stu-id="8886d-163">The record name '10' is the last octet of the IP address within this IP range represented by this record.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a><span data-ttu-id="8886d-164">Az SRV rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-164">Create an SRV record</span></span>

<span data-ttu-id="8886d-165">Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a  *\_szolgáltatás* és  *\_protokoll* a rekordhalmaz-neve.</span><span class="sxs-lookup"><span data-stu-id="8886d-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify the *\_service* and *\_protocol* in the record set name.</span></span> <span data-ttu-id="8886d-166">Nincs szükség közé tartoznak a "@" a rekordhalmaz nevében, ha a zóna felső pontja az SRV rekord létrehozása beállítva.</span><span class="sxs-lookup"><span data-stu-id="8886d-166">There is no need to include "@" in the record set name when creating an SRV record set at the zone apex.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a><span data-ttu-id="8886d-167">TXT-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="8886d-167">Create a TXT record</span></span>

<span data-ttu-id="8886d-168">A következő példa bemutatja, hogyan TXT rekord létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="8886d-168">The following example shows how to create a TXT record.</span></span> <span data-ttu-id="8886d-169">A maximális hossz támogatja a TXT-rekord kapcsolatos további információkért lásd: [TXT rekord](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="8886d-169">For more information about the maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="8886d-170">Rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="8886d-170">Get a record set</span></span>

<span data-ttu-id="8886d-171">Egy meglévő rekordhalmaz lekéréséhez használja `azure network dns record-set show`.</span><span class="sxs-lookup"><span data-stu-id="8886d-171">To retrieve an existing record set, use `azure network dns record-set show`.</span></span> <span data-ttu-id="8886d-172">További segítségért lásd: `azure network dns record-set show -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-172">For help, see `azure network dns record-set show -h`.</span></span>

<span data-ttu-id="8886d-173">Rekord vagy rekordhalmaz létrehozása, ha a megadott rekordhalmaz nevének kell lennie egy *relatív* neve, ami azt jelenti, azt kell zárnia a zóna nevét.</span><span class="sxs-lookup"><span data-stu-id="8886d-173">As when creating a record or record set, the record set name given must be a *relative* name, meaning it must exclude the zone name.</span></span> <span data-ttu-id="8886d-174">Meg kell adnia a rekordtípust, a rekordhalmaz és a zóna tartalmazó erőforráscsoportot tartalmazó zóna is.</span><span class="sxs-lookup"><span data-stu-id="8886d-174">You also need to specify the record type, the zone containing the record set, and the resource group containing the zone.</span></span>

<span data-ttu-id="8886d-175">A következő példa a rekordot be *www* , adjon meg egy zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="8886d-175">The following example retrieves the record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a><span data-ttu-id="8886d-176">Lista rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="8886d-176">List record sets</span></span>

<span data-ttu-id="8886d-177">Minden rekordot a DNS-zónák használatával listázhatja a `azure network dns record-set list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8886d-177">You can list all records in a DNS zone by using the `azure network dns record-set list` command.</span></span> <span data-ttu-id="8886d-178">További segítségért lásd: `azure network dns record-set list -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-178">For help, see `azure network dns record-set list -h`.</span></span>

<span data-ttu-id="8886d-179">Ez a példa adja vissza az összes rekordot a zónában beállítja *contoso.com*, erőforráscsoportban *MyResourceGroup*, név vagy rekordtípus függetlenül:</span><span class="sxs-lookup"><span data-stu-id="8886d-179">This example returns all record sets in the zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

<span data-ttu-id="8886d-180">Ebben a példában, amelyek megfelelnek a megadott rekordtípus (ebben az esetben az "A" rekordok) minden rekordkészletet ad vissza:</span><span class="sxs-lookup"><span data-stu-id="8886d-180">This example returns all record sets that match the given record type (in this case, 'A' records):</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-to-an-existing-record-set"></a><span data-ttu-id="8886d-181">Rekord hozzáadása egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="8886d-181">Add a record to an existing record set</span></span>

<span data-ttu-id="8886d-182">Használhat `azure network dns record-set add-record` mindkét új Rekordkészletben lévő rekordot kell létrehozni vagy hozzáadni egy rekordot egy meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="8886d-182">You can use `azure network dns record-set add-record` both to create a record in a new record set, or to add a record to an existing record set.</span></span>

<span data-ttu-id="8886d-183">További információkért lásd: [hozzon létre egy DNS-rekord](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) felett.</span><span class="sxs-lookup"><span data-stu-id="8886d-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="8886d-184">Távolítsa el a rekord egy meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="8886d-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="8886d-185">Egy meglévő rekordkészlet a DNS-rekord eltávolításához használja `azure network dns record-set delete-record`.</span><span class="sxs-lookup"><span data-stu-id="8886d-185">To remove a DNS record from an existing record set, use `azure network dns record-set delete-record`.</span></span> <span data-ttu-id="8886d-186">További segítségért lásd: `azure network dns record-set delete-record -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-186">For help, see `azure network dns record-set delete-record -h`.</span></span>

<span data-ttu-id="8886d-187">Törli a DNS-rekordot a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="8886d-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="8886d-188">A utolsó rekordját a rekordhalmaz törlése, ha van-e a rekordhalmaz maga **nem** törölték.</span><span class="sxs-lookup"><span data-stu-id="8886d-188">If the last record in a record set is deleted, the record set itself is **not** deleted.</span></span> <span data-ttu-id="8886d-189">Ehelyett egy üres rekordhalmaz marad.</span><span class="sxs-lookup"><span data-stu-id="8886d-189">Instead, an empty record set is left.</span></span> <span data-ttu-id="8886d-190">Ehelyett a rekordhalmaz törlése, lásd: [rekordhalmaz törlése](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="8886d-190">To delete the record set instead, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="8886d-191">Meg kell adni a törölni kívánt rekordot, és a zóna törölni kell, a telepítést, azonos paraméterekkel, egy rekord segítségével létrehozásakor `azure network dns record-set add-record`.</span><span class="sxs-lookup"><span data-stu-id="8886d-191">You need to specify the record to be deleted and the zone it should be deleted from, using the same parameters as when creating a record using `azure network dns record-set add-record`.</span></span> <span data-ttu-id="8886d-192">Ezek a paraméterek ismertetett [DNS-rekord létrehozása](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) fent.</span><span class="sxs-lookup"><span data-stu-id="8886d-192">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="8886d-193">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="8886d-193">This command prompts for confirmation.</span></span> <span data-ttu-id="8886d-194">Ez a kérdés is letiltja, használja a `--quiet` váltás (rövid alak `-q`).</span><span class="sxs-lookup"><span data-stu-id="8886d-194">This prompt can be suppressed using the `--quiet` switch (short form `-q`).</span></span>

<span data-ttu-id="8886d-195">A következő példa törli az A rekordot "1.2.3.4" a rekordból beállítása a nevesített értékű *www* a zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="8886d-195">The following example deletes the A record with value '1.2.3.4' from the record set named *www* in the zone *contoso.com*, in the resource group *MyResourceGroup*.</span></span> <span data-ttu-id="8886d-196">A megerősítést kérő le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="8886d-196">The confirmation prompt is suppressed.</span></span>

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="8886d-197">Módosíthatja egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="8886d-197">Modify an existing record set</span></span>

<span data-ttu-id="8886d-198">Minden rekordhalmaz tartalmaz egy [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live), [metaadatok](dns-zones-records.md#tags-and-metadata), és a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="8886d-198">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="8886d-199">Az alábbi szakaszok ismertetik, hogyan módosíthatja a ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="8886d-199">The following sections explain how to modify each of these properties.</span></span>

### <a name="to-modify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="8886d-200">A, AAAA, MX, NS, PTR, SRV és TXT-rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-200">To modify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="8886d-201">Adjon meg egy, AAAA, MX, NS, PTR, SRV és TXT meglévő bejegyzés módosításához kell először adjon hozzá egy új rekordot, és ezután törölje a meglévő.</span><span class="sxs-lookup"><span data-stu-id="8886d-201">To modify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete the existing record.</span></span> <span data-ttu-id="8886d-202">Részletes útmutatást a Törlés és rekordok hozzáadásához Ez a cikk a korábbi szakaszban talál.</span><span class="sxs-lookup"><span data-stu-id="8886d-202">For detailed instructions on how to delete and add records, see the earlier sections of this article.</span></span>

<span data-ttu-id="8886d-203">A következő példa bemutatja, hogyan lehet módosítani az "A" rekord, a következő IP-1.2.3.4 5.6.7.8 IP-cím:</span><span class="sxs-lookup"><span data-stu-id="8886d-203">The following example shows how to modify an 'A' record, from IP address 1.2.3.4 to IP address 5.6.7.8:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="to-modify-a-cname-record"></a><span data-ttu-id="8886d-204">Egy CNAME rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-204">To modify a CNAME record</span></span>

<span data-ttu-id="8886d-205">Egy olyan CNAME rekordot módosításához használható `azure network dns record-set add-record` hozzáadása az új rekord értéket.</span><span class="sxs-lookup"><span data-stu-id="8886d-205">To modify a CNAME record, use `azure network dns record-set add-record` to add the new record value.</span></span> <span data-ttu-id="8886d-206">Egyéb típusú bejegyzés eltérően a CNAME rekordkészlete csak egyetlen rekordot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="8886d-206">Unlike other record types, a CNAME record set can only contain a single record.</span></span> <span data-ttu-id="8886d-207">A meglévő rekord ezért *helyett* amikor az új rekordban kerül, és nem kell külön-külön kell hagyni.</span><span class="sxs-lookup"><span data-stu-id="8886d-207">Therefore, the existing record is *replaced* when the new record is added, and does not need to be deleted separately.</span></span>  <span data-ttu-id="8886d-208">Kérni fogja a csere fogadásához.</span><span class="sxs-lookup"><span data-stu-id="8886d-208">You will be prompted to accept this replacement.</span></span>

<span data-ttu-id="8886d-209">A példa módosítja a CNAME rekord *www* a zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, mutasson a "www.fabrikam.net" helyett a meglévő értéket:</span><span class="sxs-lookup"><span data-stu-id="8886d-209">The example modifies the CNAME record set *www* in the zone *contoso.com*, in resource group *MyResourceGroup*, to point to 'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="to-modify-an-soa-record"></a><span data-ttu-id="8886d-210">Egy SOA-rekord módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-210">To modify an SOA record</span></span>

<span data-ttu-id="8886d-211">Használjon `azure network dns record-set set-soa-record` módosítása a megadott DNS-zónák SOA.</span><span class="sxs-lookup"><span data-stu-id="8886d-211">Use `azure network dns record-set set-soa-record` to modify the SOA for a given DNS zone.</span></span> <span data-ttu-id="8886d-212">További segítségért lásd: `azure network dns record-set set-soa-record -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-212">For help, see `azure network dns record-set set-soa-record -h`.</span></span>

<span data-ttu-id="8886d-213">A következő példa bemutatja, hogyan állítsa be a "e-mail" tulajdonságot a SOA-rekord a zóna *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="8886d-213">The following example shows how to set the 'email' property of the SOA record for the zone *contoso.com* in the resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="to-modify-ns-records-at-the-zone-apex"></a><span data-ttu-id="8886d-214">A zóna felső pontja a Névkiszolgálói rekordok módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-214">To modify NS records at the zone apex</span></span>

<span data-ttu-id="8886d-215">Állítsa be a zóna felső pontja NS-rekord automatikusan létrejön minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="8886d-215">The NS record set at the zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="8886d-216">Az Azure DNS névkiszolgálóit, a zóna nevét tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8886d-216">It contains the names of the Azure DNS name servers assigned to the zone.</span></span>

<span data-ttu-id="8886d-217">Hozzáadhat további névhez a kiszolgálók e NS-rekord, támogatja a párhuzamos üzemeltetési tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="8886d-217">You can add additional name servers to this NS record set, to support co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="8886d-218">A TTL-t és a metaadatok a rekordhalmaz is módosíthatók.</span><span class="sxs-lookup"><span data-stu-id="8886d-218">You can also modify the TTL and metadata for this record set.</span></span> <span data-ttu-id="8886d-219">Azonban nem lehet eltávolítani, vagy módosítsa az előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="8886d-219">However, you cannot remove or modify the pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="8886d-220">Vegye figyelembe, hogy csak vonatkozik az NS típusú rekordhalmaz zóna tetején.</span><span class="sxs-lookup"><span data-stu-id="8886d-220">Note that this applies only to the NS record set at the zone apex.</span></span> <span data-ttu-id="8886d-221">A zónában (a használt gyermekzónákhoz delegálása) más NS-rekordhalmazok lehet módosítani, korlátozás nélkül.</span><span class="sxs-lookup"><span data-stu-id="8886d-221">Other NS record sets in your zone (as used to delegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="8886d-222">A következő példa bemutatja, hogyan hozzáadása egy további nevét, a zóna felső pontja Névkiszolgálói rekordhalmazt:</span><span class="sxs-lookup"><span data-stu-id="8886d-222">The following example shows how to add an additional name server to the NS record set at the zone apex:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="to-modify-the-ttl-of-an-existing-record-set"></a><span data-ttu-id="8886d-223">A TTL-t egy meglévő rekordkészlet módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-223">To modify the TTL of an existing record set</span></span>

<span data-ttu-id="8886d-224">A TTL-t egy meglévő rekordkészlet módosításához használható `azure network dns record-set set`.</span><span class="sxs-lookup"><span data-stu-id="8886d-224">To modify the TTL of an existing record set, use `azure network dns record-set set`.</span></span> <span data-ttu-id="8886d-225">További segítségért lásd: `azure network dns record-set set -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-225">For help, see `azure network dns record-set set -h`.</span></span>

<span data-ttu-id="8886d-226">A következő példa bemutatja, hogyan lehet módosítani egy rekordhalmaz TTL, ebben az esetben 60 másodperc:</span><span class="sxs-lookup"><span data-stu-id="8886d-226">The following example shows how to modify a record set TTL, in this case to 60 seconds:</span></span>

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="to-modify-the-metadata-of-an-existing-record-set"></a><span data-ttu-id="8886d-227">Egy meglévő rekordhalmaz metaadatainak módosítása</span><span class="sxs-lookup"><span data-stu-id="8886d-227">To modify the metadata of an existing record set</span></span>

<span data-ttu-id="8886d-228">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) rendelje hozzá az alkalmazás-specifikus adatok minden rekordhalmaz, a kulcs-érték párként használható.</span><span class="sxs-lookup"><span data-stu-id="8886d-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used to associate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="8886d-229">A metaadatok egy meglévő rekordkészlet módosításához használható `azure network dns record-set set`.</span><span class="sxs-lookup"><span data-stu-id="8886d-229">To modify the metadata of an existing record set, use `azure network dns record-set set`.</span></span> <span data-ttu-id="8886d-230">További segítségért lásd: `azure network dns record-set set -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-230">For help, see `azure network dns record-set set -h`.</span></span>

<span data-ttu-id="8886d-231">A következő példa bemutatja, hogyan lehet módosítani egy rekordot a két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =", segítségével a `--metadata` paraméter (rövid alak `-m`).</span><span class="sxs-lookup"><span data-stu-id="8886d-231">The following example shows how to modify a record set with two metadata entries, "dept=finance" and "environment=production", by using the `--metadata` parameter (short form `-m`).</span></span> <span data-ttu-id="8886d-232">Vegye figyelembe, hogy a meglévő metaadatokat *helyett* által megadott értéket.</span><span class="sxs-lookup"><span data-stu-id="8886d-232">Note that any existing metadata is *replaced* by the values given.</span></span>

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a><span data-ttu-id="8886d-233">A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="8886d-233">Delete a record set</span></span>

<span data-ttu-id="8886d-234">Rekordhalmazok használatával törölheti a `azure network dns record-set delete` parancsot.</span><span class="sxs-lookup"><span data-stu-id="8886d-234">Record sets can be deleted by using the `azure network dns record-set delete` command.</span></span> <span data-ttu-id="8886d-235">További segítségért lásd: `azure network dns record-set delete -h`.</span><span class="sxs-lookup"><span data-stu-id="8886d-235">For help, see `azure network dns record-set delete -h`.</span></span> <span data-ttu-id="8886d-236">A rekordhalmaz összes rekordján is rekordhalmaz törlése törli.</span><span class="sxs-lookup"><span data-stu-id="8886d-236">Deleting a record set also deletes all records within the record set.</span></span>

> [!NOTE]
> <span data-ttu-id="8886d-237">Nem lehet törölni a SOA és NS-rekord beállítja a zóna felső pontja (`-Name "@"`).</span><span class="sxs-lookup"><span data-stu-id="8886d-237">You cannot delete the SOA and NS record sets at the zone apex (`-Name "@"`).</span></span>  <span data-ttu-id="8886d-238">Ezek automatikusan létrejönnek a zóna mikor jött létre, és a zóna törlésekor automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="8886d-238">These are created automatically when the zone was created, and are deleted automatically when the zone is deleted.</span></span>

<span data-ttu-id="8886d-239">A következő példa törli a rekordhalmaz elnevezett *www* típus a zónából származó *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="8886d-239">The following example deletes the record set named *www* of type A from the zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

<span data-ttu-id="8886d-240">A delete művelet megerősítését kéri.</span><span class="sxs-lookup"><span data-stu-id="8886d-240">You are prompted to confirm the delete operation.</span></span> <span data-ttu-id="8886d-241">Ne jelenjen meg többé ez a kérdés, használja a `--quiet` váltás (rövid alak `-q`).</span><span class="sxs-lookup"><span data-stu-id="8886d-241">To suppress this prompt, use the `--quiet` switch (short form `-q`).</span></span>

## <a name="next-steps"></a><span data-ttu-id="8886d-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8886d-242">Next steps</span></span>

<span data-ttu-id="8886d-243">További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="8886d-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="8886d-244">Megtudhatja, hogyan [védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.</span><span class="sxs-lookup"><span data-stu-id="8886d-244">Learn how to [protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
