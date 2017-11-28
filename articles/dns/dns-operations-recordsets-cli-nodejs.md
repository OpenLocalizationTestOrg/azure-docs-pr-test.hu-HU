---
title: "az Azure DNS használatával aaaManage DNS-rekordok hello Azure CLI 1.0 |} Microsoft Docs"
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
ms.openlocfilehash: 1f01450b0839f712cb1d96be318766bac581fea1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-in-azure-dns-using-hello-azure-cli-10"></a><span data-ttu-id="5f601-104">Az Azure DNS-hello Azure CLI 1.0 használata DNS-rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="5f601-104">Manage DNS records in Azure DNS using hello Azure CLI 1.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="5f601-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5f601-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="5f601-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="5f601-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="5f601-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="5f601-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="5f601-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5f601-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="5f601-109">Ez a cikk bemutatja, hogyan toomanage DNS-rekordokat a DNS-zóna használatával hello platformfüggetlen Azure parancssori felület (CLI), amely megtalálható a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="5f601-109">This article shows you how toomanage DNS records for your DNS zone by using hello cross-platform Azure command-line interface (CLI), which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="5f601-110">A DNS-rekordok segítségével is kezelheti [Azure PowerShell](dns-operations-recordsets.md) vagy hello [Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5f601-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="5f601-111">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="5f601-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="5f601-112">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="5f601-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="5f601-113">[Az Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="5f601-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="5f601-114">[Az Azure CLI 2.0](dns-operations-recordsets-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="5f601-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="5f601-115">a cikkben szereplő példák hello során feltételezzük, hogy már [hello Azure CLI 1.0-s jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones-cli-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="5f601-115">hello examples in this article assume you have already [installed hello Azure CLI 1.0, signed in, and created a DNS zone](dns-operations-dnszones-cli-nodejs.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="5f601-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="5f601-116">Introduction</span></span>

<span data-ttu-id="5f601-117">DNS-rekordok létrehozása az Azure DNS-, előtt először toounderstand miként Azure DNS rendezi a DNS-rekordhalmazok DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="5f601-117">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="5f601-118">Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="5f601-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="5f601-119">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-119">Create a DNS record</span></span>

<span data-ttu-id="5f601-120">egy DNS-rekord toocreate hello használata `azure network dns record-set add-record` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f601-120">toocreate a DNS record, use hello `azure network dns record-set add-record` command.</span></span> <span data-ttu-id="5f601-121">További segítségért lásd: `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-121">For help, see `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="5f601-122">Rekord létrehozásakor toospecify hello erőforráscsoport-nevet, zónanév van szüksége, a rekordhalmaz nevét, a hello rekord típusát és a létrehozandó hello rekord hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="5f601-122">When creating a record, you need toospecify hello resource group name, zone name, record set name, hello record type, and hello details of hello record being created.</span></span> <span data-ttu-id="5f601-123">hello megadott rekordhalmaz nevének kell lennie egy *relatív* nevét, azaz kell zárnia a hello zóna neve.</span><span class="sxs-lookup"><span data-stu-id="5f601-123">hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span>

<span data-ttu-id="5f601-124">Ha hello rekordkészlet már nem létezik, ezzel a paranccsal létrejön az Ön.</span><span class="sxs-lookup"><span data-stu-id="5f601-124">If hello record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="5f601-125">Ha hello rekordkészlet már létezik, a parancs adda hello toohello meglévő rekordhalmaz megadott rekord.</span><span class="sxs-lookup"><span data-stu-id="5f601-125">If hello record set already exists, this command adda hello record you specify toohello existing record set.</span></span>

<span data-ttu-id="5f601-126">Új rekordhalmaz létrehozásakor az alapértelmezett élettartam (time-to-live, TTL) értéke 3600 lesz.</span><span class="sxs-lookup"><span data-stu-id="5f601-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="5f601-127">Hogyan toouse különböző TTLs: kapcsolatos utasításokat [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="5f601-127">For instructions on how toouse different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="5f601-128">hello alábbi példa létrehoz egy A rekordot nevű *www* hello zónában *contoso.com* hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5f601-128">hello following example creates an A record called *www* in hello zone *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="5f601-129">IP-címét egy rekord hello hello *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="5f601-129">hello IP address of hello A record is *1.2.3.4*.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

<span data-ttu-id="5f601-130">toocreate rekord hello csúcs hello zóna (ebben az esetben "a contoso.com"), hello rekord nevet "@", hello idézőjelekkel együtt:</span><span class="sxs-lookup"><span data-stu-id="5f601-130">toocreate a record in hello apex of hello zone (in this case, "contoso.com"), use hello record name "@", including hello quotation marks:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" A -a 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="5f601-131">A DNS-rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-131">Create a DNS record set</span></span>

<span data-ttu-id="5f601-132">Hello DNS-rekord fenti példák hello, vagy meglévő rekordhalmazt hozzáadott tooan volt, vagy hello rekordhalmaz létrehozásának *implicit módon*.</span><span class="sxs-lookup"><span data-stu-id="5f601-132">In hello above examples, hello DNS record was either added tooan existing record set, or hello record set was created *implicitly*.</span></span> <span data-ttu-id="5f601-133">Hello rekordhalmaz is létrehozhat *explicit módon* előtt tooit hozzáadása rögzíti.</span><span class="sxs-lookup"><span data-stu-id="5f601-133">You can also create hello record set *explicitly* before adding records tooit.</span></span> <span data-ttu-id="5f601-134">Az Azure DNS támogatja az "empty" rekordhalmazok, amely működhet, és egy helyőrző tooreserve egy DNS-név DNS-rekordok létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="5f601-134">Azure DNS supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="5f601-135">Üres rekordhalmazok legyenek láthatók a hello Azure DNS vezérlősík vezérlőként, de nem szerepelnek a hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="5f601-135">Empty record sets are visible in hello Azure DNS control plane, but do not appear on hello Azure DNS name servers.</span></span>

<span data-ttu-id="5f601-136">Rekordhalmazok hello használatával hozhatók létre `azure network dns record-set create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f601-136">Record sets are created using hello `azure network dns record-set create` command.</span></span> <span data-ttu-id="5f601-137">További segítségért lásd: `azure network dns record-set create -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-137">For help, see `azure network dns record-set create -h`.</span></span>

<span data-ttu-id="5f601-138">Be explicit módon hello rekord létrehozása lehetővé teszi a toospecify rekordhalmaz tulajdonságait, például hello [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live) és metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="5f601-138">Creating hello record set explicitly allows you toospecify record set properties such as hello [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="5f601-139">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5f601-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="5f601-140">hello alábbi példa létrehoz egy üres rekordhalmaz 60 másodperces TTL, hello segítségével `--ttl` paraméter (rövid alak `-l`):</span><span class="sxs-lookup"><span data-stu-id="5f601-140">hello following example creates an empty record set with a 60-second TTL, by using hello `--ttl` parameter (short form `-l`):</span></span>

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --ttl 60
```

<span data-ttu-id="5f601-141">hello alábbi példa létrehoz két metaadat-bejegyzést, rekordkészlet "osztály pénzügyi =" és "környezet éles =", hello segítségével `--metadata` paraméter (rövid alak `-m`):</span><span class="sxs-lookup"><span data-stu-id="5f601-141">hello following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using hello `--metadata` parameter (short form `-m`):</span></span>

```azurecli
azure network dns record-set create MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

<span data-ttu-id="5f601-142">Egy üres rekordhalmaz hozunk létre, rekordok segítségével adhatók `azure network dns record-set add-record` leírtak [hozzon létre egy DNS-rekord](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="5f601-142">Having created an empty record set, records can be added using `azure network dns record-set add-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="5f601-143">Más típusú rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-143">Create records of other types</span></span>

<span data-ttu-id="5f601-144">Hogy látott részletesen hogyan toocreate "A" rögzíti, hello a következő példák szemléltetik, hogyan toocreate rekord egyéb típusú bejegyzés a támogatott Azure DNS által.</span><span class="sxs-lookup"><span data-stu-id="5f601-144">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="5f601-145">hello paraméterek használt toospecify hello rekord adatok hello típusú rekord lehet hello függenek.</span><span class="sxs-lookup"><span data-stu-id="5f601-145">hello parameters used toospecify hello record data vary depending on hello type of hello record.</span></span> <span data-ttu-id="5f601-146">Az "A" típusú rekord, akkor adja meg például hello IPv4-cím hello paraméterrel `-a <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="5f601-146">For example, for a record of type "A", you specify hello IPv4 address with hello parameter `-a <IPv4 address>`.</span></span> <span data-ttu-id="5f601-147">hello paraméterek, a különböző rekordtípusú is listázva lehet használatával `azure network dns record-set add-record -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-147">hello parameters for each record type can be listed using `azure network dns record-set add-record -h`.</span></span>

<span data-ttu-id="5f601-148">Minden esetben megmutatjuk, hogyan toocreate egyetlen rekordot.</span><span class="sxs-lookup"><span data-stu-id="5f601-148">In each case, we show how toocreate a single record.</span></span> <span data-ttu-id="5f601-149">hello rekord rekordhalmaz meglévő hozzáadott toohello, vagy rekordhalmaz implicit módon létrehozva.</span><span class="sxs-lookup"><span data-stu-id="5f601-149">hello record is added toohello existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="5f601-150">További információ a rekordhalmazok létrehozásához, és definiáló rekordot paraméter explicit módon, olvassa el [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="5f601-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="5f601-151">Nem ad egy példa toocreate egy SOA típusú rekordhalmaz SOAs elkészítése és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön.</span><span class="sxs-lookup"><span data-stu-id="5f601-151">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="5f601-152">Azonban [SOA módosítható, egy újabb példában látható módon hello](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="5f601-152">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="5f601-153">Az AAAA-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-153">Create an AAAA record</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-aaaa AAAA --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="5f601-154">Hozzon létre egy CNAME rekordot</span><span class="sxs-lookup"><span data-stu-id="5f601-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="5f601-155">hello DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna hello csúcsán (`-Name "@"`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="5f601-155">hello DNS standards do not permit CNAME records at hello apex of a zone (`-Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="5f601-156">További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="5f601-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
azure network dns record-set add-record  MyResourceGroup contoso.com  test-cname CNAME --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="5f601-157">Az MX-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-157">Create an MX record</span></span>

<span data-ttu-id="5f601-158">Ebben a példában hello rekordhalmaznevet használjuk "@" toocreate hello MX rekord: hello zóna felső pontja (ebben az esetben "a contoso.com").</span><span class="sxs-lookup"><span data-stu-id="5f601-158">In this example, we use hello record set name "@" toocreate hello MX record at hello zone apex (in this case, "contoso.com").</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "@" MX --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="5f601-159">NS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-159">Create an NS record</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup  contoso.com  test-ns NS --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="5f601-160">A PTR típusú rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-160">Create a PTR record</span></span>

<span data-ttu-id="5f601-161">Ebben az esetben "my-arpa-zónában (Zone.com)" jelöli hello ARPA zóna képviselő az IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="5f601-161">In this case, 'my-arpa-zone.com' represents hello ARPA zone representing your IP range.</span></span> <span data-ttu-id="5f601-162">Minden egyes PTR típusú rekord a zóna tooan IP-címet az IP-címtartományon belül felel meg.</span><span class="sxs-lookup"><span data-stu-id="5f601-162">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span>  <span data-ttu-id="5f601-163">hello rekordjának neve "10" hello utolsó oktett hello IP-cím, ez a bejegyzés által képviselt IP-címtartományon belül.</span><span class="sxs-lookup"><span data-stu-id="5f601-163">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup my-arpa-zone.com "10" PTR --ptrdname "myservice.contoso.com"
```

### <a name="create-an-srv-record"></a><span data-ttu-id="5f601-164">Az SRV rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-164">Create an SRV record</span></span>

<span data-ttu-id="5f601-165">Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a hello  *\_szolgáltatás* és  *\_protokoll* hello a rekordhalmaz neveként.</span><span class="sxs-lookup"><span data-stu-id="5f601-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="5f601-166">Nincs szükség tooinclude van "@" hello a rekordhalmaz neveként az SRV rekord létrehozása beállításakor: hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="5f601-166">There is no need tooinclude "@" in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com  "_sip._tls" SRV --priority 10 --weight 5 --port 8080 --target "sip.contoso.com"
```

### <a name="create-a-txt-record"></a><span data-ttu-id="5f601-167">TXT-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f601-167">Create a TXT record</span></span>

<span data-ttu-id="5f601-168">hello következő példa bemutatja, hogyan toocreate egy TXT rögzíti.</span><span class="sxs-lookup"><span data-stu-id="5f601-168">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="5f601-169">További információ a támogatott TXT rekord hello karakterlánc maximális hossza: [TXT rekord](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="5f601-169">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com test-txt TXT --text "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="5f601-170">Rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="5f601-170">Get a record set</span></span>

<span data-ttu-id="5f601-171">Használjon egy meglévő rekordhalmaz tooretrieve `azure network dns record-set show`.</span><span class="sxs-lookup"><span data-stu-id="5f601-171">tooretrieve an existing record set, use `azure network dns record-set show`.</span></span> <span data-ttu-id="5f601-172">További segítségért lásd: `azure network dns record-set show -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-172">For help, see `azure network dns record-set show -h`.</span></span>

<span data-ttu-id="5f601-173">Egy rekord és a rekordhalmaz létrehozásakor hello rekord állíthat be, a megadott név lehet a *relatív* nevét, azaz kell zárnia a hello zóna neve.</span><span class="sxs-lookup"><span data-stu-id="5f601-173">As when creating a record or record set, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="5f601-174">Szükség toospecify hello rekordtípust, hello rekordot tartalmazó hello zóna beállítása, hello hello zóna tartalmazó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="5f601-174">You also need toospecify hello record type, hello zone containing hello record set, and hello resource group containing hello zone.</span></span>

<span data-ttu-id="5f601-175">hello alábbi példa hello rekordot be *www* , adjon meg egy zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5f601-175">hello following example retrieves hello record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set show MyResourceGroup contoso.com www A
```

## <a name="list-record-sets"></a><span data-ttu-id="5f601-176">Lista rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="5f601-176">List record sets</span></span>

<span data-ttu-id="5f601-177">Minden rekordot a DNS-zónák hello segítségével listázhatja `azure network dns record-set list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f601-177">You can list all records in a DNS zone by using hello `azure network dns record-set list` command.</span></span> <span data-ttu-id="5f601-178">További segítségért lásd: `azure network dns record-set list -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-178">For help, see `azure network dns record-set list -h`.</span></span>

<span data-ttu-id="5f601-179">Ebben a példában adja vissza az összes rekordhalmazok hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, név vagy rekordtípus függetlenül:</span><span class="sxs-lookup"><span data-stu-id="5f601-179">This example returns all record sets in hello zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com
```

<span data-ttu-id="5f601-180">Ebben a példában, amelyek megfelelnek a megadott rekordtípus (ebben az esetben az "A" rekordok) hello minden rekordkészletet ad vissza:</span><span class="sxs-lookup"><span data-stu-id="5f601-180">This example returns all record sets that match hello given record type (in this case, 'A' records):</span></span>

```azurecli
azure network dns record-set list MyResourceGroup contoso.com --type A
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="5f601-181">A meglévő rekordhalmazt rekord tooan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5f601-181">Add a record tooan existing record set</span></span>

<span data-ttu-id="5f601-182">Használhat `azure network dns record-set add-record` mindkét toocreate egy új rekordot a rekordhalmaz vagy tooadd rekord tooan meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="5f601-182">You can use `azure network dns record-set add-record` both toocreate a record in a new record set, or tooadd a record tooan existing record set.</span></span>

<span data-ttu-id="5f601-183">További információkért lásd: [hozzon létre egy DNS-rekord](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) felett.</span><span class="sxs-lookup"><span data-stu-id="5f601-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="5f601-184">Távolítsa el a rekord egy meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="5f601-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="5f601-185">meglévő rekordhalmaz használata tooremove egy DNS-bejegyzést `azure network dns record-set delete-record`.</span><span class="sxs-lookup"><span data-stu-id="5f601-185">tooremove a DNS record from an existing record set, use `azure network dns record-set delete-record`.</span></span> <span data-ttu-id="5f601-186">További segítségért lásd: `azure network dns record-set delete-record -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-186">For help, see `azure network dns record-set delete-record -h`.</span></span>

<span data-ttu-id="5f601-187">Törli a DNS-rekordot a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="5f601-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="5f601-188">Utolsó rekordját hello a rekordkészlet törlésekor hello rekordhalmaz maga van **nem** törölték.</span><span class="sxs-lookup"><span data-stu-id="5f601-188">If hello last record in a record set is deleted, hello record set itself is **not** deleted.</span></span> <span data-ttu-id="5f601-189">Ehelyett egy üres rekordhalmaz marad.</span><span class="sxs-lookup"><span data-stu-id="5f601-189">Instead, an empty record set is left.</span></span> <span data-ttu-id="5f601-190">toodelete hello rekordhalmaz helyett, lásd: [rekordhalmaz törlése](#delete-a-record-set).</span><span class="sxs-lookup"><span data-stu-id="5f601-190">toodelete hello record set instead, see [Delete a record set](#delete-a-record-set).</span></span>

<span data-ttu-id="5f601-191">Toospecify kell hello törölt rekord toobe és azt törölni kell, használatával hello zóna hello ugyanazokkal a paraméterekkel, egy rekord segítségével létrehozásakor `azure network dns record-set add-record`.</span><span class="sxs-lookup"><span data-stu-id="5f601-191">You need toospecify hello record toobe deleted and hello zone it should be deleted from, using hello same parameters as when creating a record using `azure network dns record-set add-record`.</span></span> <span data-ttu-id="5f601-192">Ezek a paraméterek ismertetett [DNS-rekord létrehozása](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) fent.</span><span class="sxs-lookup"><span data-stu-id="5f601-192">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="5f601-193">Ez a parancs felszólítja megerősítést kér.</span><span class="sxs-lookup"><span data-stu-id="5f601-193">This command prompts for confirmation.</span></span> <span data-ttu-id="5f601-194">Ez a kérdés hello letilthatók `--quiet` váltás (rövid alak `-q`).</span><span class="sxs-lookup"><span data-stu-id="5f601-194">This prompt can be suppressed using hello `--quiet` switch (short form `-q`).</span></span>

<span data-ttu-id="5f601-195">a következő példa törlések hello hello "1.2.3.4" hello rekordból beállítása a nevesített értékű egy olyan rekordot *www* hello zónában *contoso.com*, hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="5f601-195">hello following example deletes hello A record with value '1.2.3.4' from hello record set named *www* in hello zone *contoso.com*, in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="5f601-196">hello megerősítési kérés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="5f601-196">hello confirmation prompt is suppressed.</span></span>

```azurecli
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4 --quiet
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="5f601-197">Módosíthatja egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="5f601-197">Modify an existing record set</span></span>

<span data-ttu-id="5f601-198">Minden rekordhalmaz tartalmaz egy [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live), [metaadatok](dns-zones-records.md#tags-and-metadata), és a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="5f601-198">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="5f601-199">hello alábbi szakaszok azt ismertetik, hogyan toomodify minden ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="5f601-199">hello following sections explain how toomodify each of these properties.</span></span>

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="5f601-200">az A, AAAA, MX, NS, PTR, SRV és TXT rekord toomodify</span><span class="sxs-lookup"><span data-stu-id="5f601-200">toomodify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="5f601-201">Adjon meg egy, AAAA, MX, NS, PTR, SRV és TXT meglévő bejegyzés toomodify, először adjon egy új rekordot, és a delete hello meglévő rekord.</span><span class="sxs-lookup"><span data-stu-id="5f601-201">toomodify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete hello existing record.</span></span> <span data-ttu-id="5f601-202">Részletes útmutatást toodelete és rekordok hozzáadásához, tekintse meg a cikk korábbi szakaszaiban hello.</span><span class="sxs-lookup"><span data-stu-id="5f601-202">For detailed instructions on how toodelete and add records, see hello earlier sections of this article.</span></span>

<span data-ttu-id="5f601-203">hello a következő példa bemutatja, hogyan toomodify az "A" rekord, az IP-cím 1.2.3.4 tooIP cím 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="5f601-203">hello following example shows how toomodify an 'A' record, from IP address 1.2.3.4 tooIP address 5.6.7.8:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www A -a 5.6.7.8
azure network dns record-set delete-record MyResourceGroup contoso.com www A -a 1.2.3.4
```

### <a name="toomodify-a-cname-record"></a><span data-ttu-id="5f601-204">egy olyan CNAME rekordot toomodify</span><span class="sxs-lookup"><span data-stu-id="5f601-204">toomodify a CNAME record</span></span>

<span data-ttu-id="5f601-205">egy olyan CNAME rekordot toomodify használja `azure network dns record-set add-record` tooadd hello új rekord értéket.</span><span class="sxs-lookup"><span data-stu-id="5f601-205">toomodify a CNAME record, use `azure network dns record-set add-record` tooadd hello new record value.</span></span> <span data-ttu-id="5f601-206">Egyéb típusú bejegyzés eltérően a CNAME rekordkészlete csak egyetlen rekordot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="5f601-206">Unlike other record types, a CNAME record set can only contain a single record.</span></span> <span data-ttu-id="5f601-207">Hello meglévő rekord ezért *helyett* amikor hello új rekord kerül, és nem kell külön-külön törölt toobe.</span><span class="sxs-lookup"><span data-stu-id="5f601-207">Therefore, hello existing record is *replaced* when hello new record is added, and does not need toobe deleted separately.</span></span>  <span data-ttu-id="5f601-208">Akkor lesz felszólító tooaccept a csere.</span><span class="sxs-lookup"><span data-stu-id="5f601-208">You will be prompted tooaccept this replacement.</span></span>

<span data-ttu-id="5f601-209">hello példa módosítja hello CNAME rekordkészlete *www* hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, toopoint túl "www.fabrikam.net" helyett a meglévő érték:</span><span class="sxs-lookup"><span data-stu-id="5f601-209">hello example modifies hello CNAME record set *www* in hello zone *contoso.com*, in resource group *MyResourceGroup*, toopoint too'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com www CNAME --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="5f601-210">egy SOA rekordot toomodify</span><span class="sxs-lookup"><span data-stu-id="5f601-210">toomodify an SOA record</span></span>

<span data-ttu-id="5f601-211">Használjon `azure network dns record-set set-soa-record` toomodify hello SOA a megadott DNS-zónák.</span><span class="sxs-lookup"><span data-stu-id="5f601-211">Use `azure network dns record-set set-soa-record` toomodify hello SOA for a given DNS zone.</span></span> <span data-ttu-id="5f601-212">További segítségért lásd: `azure network dns record-set set-soa-record -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-212">For help, see `azure network dns record-set set-soa-record -h`.</span></span>

<span data-ttu-id="5f601-213">hello következő példa bemutatja, hogyan hello SOA tooset hello "e-mail" tulajdonságának hello zóna rekordja *contoso.com* hello erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5f601-213">hello following example shows how tooset hello 'email' property of hello SOA record for hello zone *contoso.com* in hello resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set set-soa-record rg1 contoso.com --email admin.contoso.com
```


### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="5f601-214">hello zóna felső pontja toomodify Névkiszolgálói rekordok</span><span class="sxs-lookup"><span data-stu-id="5f601-214">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="5f601-215">Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="5f601-215">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="5f601-216">Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.</span><span class="sxs-lookup"><span data-stu-id="5f601-216">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="5f601-217">Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="5f601-217">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="5f601-218">Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="5f601-218">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="5f601-219">Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="5f601-219">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="5f601-220">Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="5f601-220">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="5f601-221">Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="5f601-221">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="5f601-222">hello a következő példa bemutatja, hogyan tooadd további neve server toohello NS-rekord állítják hello zóna felső pontja:</span><span class="sxs-lookup"><span data-stu-id="5f601-222">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```azurecli
azure network dns record-set add-record MyResourceGroup contoso.com "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a><span data-ttu-id="5f601-223">egy meglévő bejegyzés TTL toomodify hello beállítása</span><span class="sxs-lookup"><span data-stu-id="5f601-223">toomodify hello TTL of an existing record set</span></span>

<span data-ttu-id="5f601-224">toomodify hello TTL-t egy meglévő bejegyzés megadásához használja `azure network dns record-set set`.</span><span class="sxs-lookup"><span data-stu-id="5f601-224">toomodify hello TTL of an existing record set, use `azure network dns record-set set`.</span></span> <span data-ttu-id="5f601-225">További segítségért lásd: `azure network dns record-set set -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-225">For help, see `azure network dns record-set set -h`.</span></span>

<span data-ttu-id="5f601-226">hello a következő példa bemutatja, hogyan toomodify rekordhalmaz TTL-t, ez az eset too60 másodperc:</span><span class="sxs-lookup"><span data-stu-id="5f601-226">hello following example shows how toomodify a record set TTL, in this case too60 seconds:</span></span>

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --ttl 60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a><span data-ttu-id="5f601-227">egy meglévő rekordkészlet toomodify hello metaadatai</span><span class="sxs-lookup"><span data-stu-id="5f601-227">toomodify hello metadata of an existing record set</span></span>

<span data-ttu-id="5f601-228">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="5f601-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="5f601-229">egy meglévő bejegyzés toomodify hello metaadatok megadásához használja `azure network dns record-set set`.</span><span class="sxs-lookup"><span data-stu-id="5f601-229">toomodify hello metadata of an existing record set, use `azure network dns record-set set`.</span></span> <span data-ttu-id="5f601-230">További segítségért lásd: `azure network dns record-set set -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-230">For help, see `azure network dns record-set set -h`.</span></span>

<span data-ttu-id="5f601-231">hello következő példa bemutatja, hogyan toomodify nevű rekordhalmaz két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =", hello segítségével `--metadata` paraméter (rövid alak `-m`).</span><span class="sxs-lookup"><span data-stu-id="5f601-231">hello following example shows how toomodify a record set with two metadata entries, "dept=finance" and "environment=production", by using hello `--metadata` parameter (short form `-m`).</span></span> <span data-ttu-id="5f601-232">Vegye figyelembe, hogy a meglévő metaadatokat *helyett* által hello értékekből.</span><span class="sxs-lookup"><span data-stu-id="5f601-232">Note that any existing metadata is *replaced* by hello values given.</span></span>

```azurecli
azure network dns record-set set MyResourceGroup contoso.com www A --metadata "dept=finance;environment=production"
```

## <a name="delete-a-record-set"></a><span data-ttu-id="5f601-233">A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="5f601-233">Delete a record set</span></span>

<span data-ttu-id="5f601-234">Rekordhalmazok hello segítségével törölhetők `azure network dns record-set delete` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5f601-234">Record sets can be deleted by using hello `azure network dns record-set delete` command.</span></span> <span data-ttu-id="5f601-235">További segítségért lásd: `azure network dns record-set delete -h`.</span><span class="sxs-lookup"><span data-stu-id="5f601-235">For help, see `azure network dns record-set delete -h`.</span></span> <span data-ttu-id="5f601-236">Rekordhalmaz törlése is törli az összes rekordján hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="5f601-236">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="5f601-237">Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (`-Name "@"`).</span><span class="sxs-lookup"><span data-stu-id="5f601-237">You cannot delete hello SOA and NS record sets at hello zone apex (`-Name "@"`).</span></span>  <span data-ttu-id="5f601-238">Ezek automatikusan létrejönnek hello zóna mikor jött létre, és hello zóna törlésekor automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="5f601-238">These are created automatically when hello zone was created, and are deleted automatically when hello zone is deleted.</span></span>

<span data-ttu-id="5f601-239">hello alábbi példa törli hello rekordhalmaz elnevezett *www* , adjon meg egy hello zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="5f601-239">hello following example deletes hello record set named *www* of type A from hello zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
azure network dns record-set delete MyResourceGroup contoso.com www A
```

<span data-ttu-id="5f601-240">Biztosan felszólító tooconfirm hello törlési műveletet.</span><span class="sxs-lookup"><span data-stu-id="5f601-240">You are prompted tooconfirm hello delete operation.</span></span> <span data-ttu-id="5f601-241">a parancssorba toosuppress hello használata `--quiet` váltás (rövid alak `-q`).</span><span class="sxs-lookup"><span data-stu-id="5f601-241">toosuppress this prompt, use hello `--quiet` switch (short form `-q`).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5f601-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f601-242">Next steps</span></span>

<span data-ttu-id="5f601-243">További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="5f601-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="5f601-244">Ismerje meg, hogyan túl[védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.</span><span class="sxs-lookup"><span data-stu-id="5f601-244">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
