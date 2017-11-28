---
title: "az Azure DNS használatával aaaManage DNS-rekordok hello Azure CLI 2.0 |} Microsoft Docs"
description: "DNS-rekordhalmazok és az Azure DNS-rekordok kezelése esetén az Azure DNS-tartomány. Minden CLI 2.0 parancsok rekordhalmazokat és rekordokat műveleteket."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: 5356a3a5-8dec-44ac-9709-0c2b707f6cb5
ms.service: dns
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: H1Hack27Feb2017
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: jonatul
ms.openlocfilehash: 9d6f8e74ebad55ccd2381fd84a830d2c7bbb1f30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-dns-records-and-recordsets-in-azure-dns-using-hello-azure-cli-20"></a><span data-ttu-id="cffbd-104">DNS-rekordok és az Azure CLI 2.0 hello használata Azure DNS rekordhalmazok kezelése</span><span class="sxs-lookup"><span data-stu-id="cffbd-104">Manage DNS records and recordsets in Azure DNS using hello Azure CLI 2.0</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="cffbd-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="cffbd-105">Azure Portal</span></span>](dns-operations-recordsets-portal.md)
> * [<span data-ttu-id="cffbd-106">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cffbd-106">Azure CLI 1.0</span></span>](dns-operations-recordsets-cli-nodejs.md)
> * [<span data-ttu-id="cffbd-107">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="cffbd-107">Azure CLI 2.0</span></span>](dns-operations-recordsets-cli.md)
> * [<span data-ttu-id="cffbd-108">PowerShell</span><span class="sxs-lookup"><span data-stu-id="cffbd-108">PowerShell</span></span>](dns-operations-recordsets.md)

<span data-ttu-id="cffbd-109">Ez a cikk bemutatja, hogyan toomanage DNS-rekordokat a DNS-zóna használatával hello platformfüggetlen Azure parancssori felület (CLI) 2.0-s, amelyik elérhető a Windows, Mac és Linux.</span><span class="sxs-lookup"><span data-stu-id="cffbd-109">This article shows you how toomanage DNS records for your DNS zone by using hello cross-platform Azure command-line interface (CLI) 2.0, which is available for Windows, Mac and Linux.</span></span> <span data-ttu-id="cffbd-110">A DNS-rekordok segítségével is kezelheti [Azure PowerShell](dns-operations-recordsets.md) vagy hello [Azure-portálon](dns-operations-recordsets-portal.md).</span><span class="sxs-lookup"><span data-stu-id="cffbd-110">You can also manage your DNS records using [Azure PowerShell](dns-operations-recordsets.md) or hello [Azure portal](dns-operations-recordsets-portal.md).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="cffbd-111">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="cffbd-111">CLI versions toocomplete hello task</span></span>

<span data-ttu-id="cffbd-112">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="cffbd-112">You can complete hello task using one of hello following CLI versions:</span></span>

* <span data-ttu-id="cffbd-113">[Az Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) -hello klasszikus és resource management üzembe helyezési modellel a parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="cffbd-113">[Azure CLI 1.0](dns-operations-recordsets-cli-nodejs.md) - our CLI for hello classic and resource management deployment models.</span></span>
* <span data-ttu-id="cffbd-114">[Az Azure CLI 2.0](dns-operations-recordsets-cli.md) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="cffbd-114">[Azure CLI 2.0](dns-operations-recordsets-cli.md) - our next generation CLI for hello resource management deployment model.</span></span>

<span data-ttu-id="cffbd-115">a cikkben szereplő példák hello során feltételezzük, hogy már [hello Azure CLI 2.0 jelentkezik be, és a DNS-zóna létrehozásakor](dns-operations-dnszones-cli.md).</span><span class="sxs-lookup"><span data-stu-id="cffbd-115">hello examples in this article assume you have already [installed hello Azure CLI 2.0, signed in, and created a DNS zone](dns-operations-dnszones-cli.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="cffbd-116">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cffbd-116">Introduction</span></span>

<span data-ttu-id="cffbd-117">DNS-rekordok létrehozása az Azure DNS-, előtt először toounderstand miként Azure DNS rendezi a DNS-rekordhalmazok DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-117">Before creating DNS records in Azure DNS, you first need toounderstand how Azure DNS organizes DNS records into DNS record sets.</span></span>

[!INCLUDE [dns-about-records-include](../../includes/dns-about-records-include.md)]

<span data-ttu-id="cffbd-118">Az Azure DNS DNS-rekordjaival kapcsolatos további információért tekintse meg a [DNS-zónákkal és -rekordokkal](dns-zones-records.md) foglalkozó cikket.</span><span class="sxs-lookup"><span data-stu-id="cffbd-118">For more information about DNS records in Azure DNS, see [DNS zones and records](dns-zones-records.md).</span></span>

## <a name="create-a-dns-record"></a><span data-ttu-id="cffbd-119">DNS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-119">Create a DNS record</span></span>

<span data-ttu-id="cffbd-120">toocreate egy DNS-rekordot, használja a hello `az network dns record-set <record-type> set-record` parancs (ahol `<record-type>` hello típusú rekord, azaz</span><span class="sxs-lookup"><span data-stu-id="cffbd-120">toocreate a DNS record, use hello `az network dns record-set <record-type> set-record` command (where `<record-type>` is hello type of record, i.e</span></span> <span data-ttu-id="cffbd-121">egy, srv, txt, stb.) További segítségért lásd: `az network dns record-set --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-121">a, srv, txt, etc.) For help, see `az network dns record-set --help`.</span></span>

<span data-ttu-id="cffbd-122">Rekord létrehozásakor toospecify hello erőforráscsoport-nevet, zónanév van szüksége, a rekordhalmaz nevét, a hello rekord típusát és a létrehozandó hello rekord hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="cffbd-122">When creating a record, you need toospecify hello resource group name, zone name, record set name, hello record type, and hello details of hello record being created.</span></span> <span data-ttu-id="cffbd-123">hello megadott rekordhalmaz nevének kell lennie egy *relatív* nevét, azaz kell zárnia a hello zóna neve.</span><span class="sxs-lookup"><span data-stu-id="cffbd-123">hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span>

<span data-ttu-id="cffbd-124">Ha hello rekordkészlet már nem létezik, ezzel a paranccsal létrejön az Ön.</span><span class="sxs-lookup"><span data-stu-id="cffbd-124">If hello record set does not already exist, this command creates it for you.</span></span> <span data-ttu-id="cffbd-125">Ha hello rekordhalmaz már létezik, a parancs hozzáad toohello meglévő rekordhalmaz megadott hello rekord.</span><span class="sxs-lookup"><span data-stu-id="cffbd-125">If hello record set already exists, this command adds hello record you specify toohello existing record set.</span></span>

<span data-ttu-id="cffbd-126">Új rekordhalmaz létrehozásakor az alapértelmezett élettartam (time-to-live, TTL) értéke 3600 lesz.</span><span class="sxs-lookup"><span data-stu-id="cffbd-126">If a new record set is created, a default time-to-live (TTL) of 3600 is used.</span></span> <span data-ttu-id="cffbd-127">Hogyan toouse különböző TTLs: kapcsolatos utasításokat [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="cffbd-127">For instructions on how toouse different TTLs, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="cffbd-128">hello alábbi példa létrehoz egy A rekordot nevű *www* hello zónában *contoso.com* hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="cffbd-128">hello following example creates an A record called *www* in hello zone *contoso.com* in hello resource group *MyResourceGroup*.</span></span> <span data-ttu-id="cffbd-129">IP-címét egy rekord hello hello *1.2.3.4*.</span><span class="sxs-lookup"><span data-stu-id="cffbd-129">hello IP address of hello A record is *1.2.3.4*.</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="cffbd-130">toocreate rekord beállított hello csúcs hello zóna (ebben az esetben "a contoso.com"), hello rekord nevet "@", hello idézőjelekkel együtt:</span><span class="sxs-lookup"><span data-stu-id="cffbd-130">toocreate a record set in hello apex of hello zone (in this case, "contoso.com"), use hello record name "@", including hello quotation marks:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --ipv4-address 1.2.3.4
```

## <a name="create-a-dns-record-set"></a><span data-ttu-id="cffbd-131">A DNS-rekordhalmaz létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-131">Create a DNS record set</span></span>

<span data-ttu-id="cffbd-132">Hello DNS-rekord fenti példák hello, vagy meglévő rekordhalmazt hozzáadott tooan volt, vagy hello rekordhalmaz létrehozásának *implicit módon*.</span><span class="sxs-lookup"><span data-stu-id="cffbd-132">In hello above examples, hello DNS record was either added tooan existing record set, or hello record set was created *implicitly*.</span></span> <span data-ttu-id="cffbd-133">Hello rekordhalmaz is létrehozhat *explicit módon* előtt tooit hozzáadása rögzíti.</span><span class="sxs-lookup"><span data-stu-id="cffbd-133">You can also create hello record set *explicitly* before adding records tooit.</span></span> <span data-ttu-id="cffbd-134">Az Azure DNS támogatja az "empty" rekordhalmazok, amely működhet, és egy helyőrző tooreserve egy DNS-név DNS-rekordok létrehozása előtt.</span><span class="sxs-lookup"><span data-stu-id="cffbd-134">Azure DNS supports 'empty' record sets, which can act as a placeholder tooreserve a DNS name before creating DNS records.</span></span> <span data-ttu-id="cffbd-135">Üres rekordhalmazok legyenek láthatók a hello Azure DNS vezérlősík vezérlőként, de nem szerepelnek a hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="cffbd-135">Empty record sets are visible in hello Azure DNS control plane, but do not appear on hello Azure DNS name servers.</span></span>

<span data-ttu-id="cffbd-136">Rekordhalmazok hello használatával hozhatók létre `az network dns record-set <record-type> create` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cffbd-136">Record sets are created using hello `az network dns record-set <record-type> create` command.</span></span> <span data-ttu-id="cffbd-137">További segítségért lásd: `az network dns record-set <record-type> create --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-137">For help, see `az network dns record-set <record-type> create --help`.</span></span>

<span data-ttu-id="cffbd-138">Be explicit módon hello rekord létrehozása lehetővé teszi a toospecify rekordhalmaz tulajdonságait, például hello [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live) és metaadatokat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-138">Creating hello record set explicitly allows you toospecify record set properties such as hello [Time-To-Live (TTL)](dns-zones-records.md#time-to-live) and metadata.</span></span> <span data-ttu-id="cffbd-139">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="cffbd-139">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span>

<span data-ttu-id="cffbd-140">hello alábbi példa használatával hozza létre egy üres rekordhalmaz típusú 60 másodperces TTL, "A" hello `--ttl` paraméter (rövid alak `-l`):</span><span class="sxs-lookup"><span data-stu-id="cffbd-140">hello following example creates an empty record set of type 'A' with a 60-second TTL, by using hello `--ttl` parameter (short form `-l`):</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --ttl 60
```

<span data-ttu-id="cffbd-141">hello alábbi példa létrehoz két metaadat-bejegyzést, rekordkészlet "osztály pénzügyi =" és "környezet éles =", hello segítségével `--metadata` paraméter:</span><span class="sxs-lookup"><span data-stu-id="cffbd-141">hello following example creates a record set with two metadata entries, "dept=finance" and "environment=production", by using hello `--metadata` parameter :</span></span>

```azurecli
az network dns record-set a create --resource-group myresourcegroup --zone-name contoso.com --name www --metadata "dept=finance" "environment=production"
```

<span data-ttu-id="cffbd-142">Egy üres rekordhalmaz hozunk létre, rekordok segítségével adhatók `azure network dns record-set <record-type> set-record` leírtak [hozzon létre egy DNS-rekord](#create-a-dns-record).</span><span class="sxs-lookup"><span data-stu-id="cffbd-142">Having created an empty record set, records can be added using `azure network dns record-set <record-type> set-record` as described in [Create a DNS record](#create-a-dns-record).</span></span>

## <a name="create-records-of-other-types"></a><span data-ttu-id="cffbd-143">Más típusú rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-143">Create records of other types</span></span>

<span data-ttu-id="cffbd-144">Hogy látott részletesen hogyan toocreate "A" rögzíti, hello a következő példák szemléltetik, hogyan toocreate rekord egyéb típusú bejegyzés a támogatott Azure DNS által.</span><span class="sxs-lookup"><span data-stu-id="cffbd-144">Having seen in detail how toocreate 'A' records, hello following examples show how toocreate record of other record types supported by Azure DNS.</span></span>

<span data-ttu-id="cffbd-145">hello paraméterek használt toospecify hello rekord adatok hello típusú rekord lehet hello függenek.</span><span class="sxs-lookup"><span data-stu-id="cffbd-145">hello parameters used toospecify hello record data vary depending on hello type of hello record.</span></span> <span data-ttu-id="cffbd-146">Az "A" típusú rekord, akkor adja meg például hello IPv4-cím hello paraméterrel `--ipv4-address <IPv4 address>`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-146">For example, for a record of type "A", you specify hello IPv4 address with hello parameter `--ipv4-address <IPv4 address>`.</span></span> <span data-ttu-id="cffbd-147">hello paraméterek, a különböző rekordtípusú is listázva lehet használatával `az network dns record-set <record-type> set-record --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-147">hello parameters for each record type can be listed using `az network dns record-set <record-type> set-record --help`.</span></span>

<span data-ttu-id="cffbd-148">Minden esetben megmutatjuk, hogyan toocreate egyetlen rekordot.</span><span class="sxs-lookup"><span data-stu-id="cffbd-148">In each case, we show how toocreate a single record.</span></span> <span data-ttu-id="cffbd-149">hello rekord rekordhalmaz meglévő hozzáadott toohello, vagy rekordhalmaz implicit módon létrehozva.</span><span class="sxs-lookup"><span data-stu-id="cffbd-149">hello record is added toohello existing record set, or a record set created implicitly.</span></span> <span data-ttu-id="cffbd-150">További információ a rekordhalmazok létrehozásához, és definiáló rekordot paraméter explicit módon, olvassa el [DNS rekordhalmaz létrehozása](#create-a-dns-record-set).</span><span class="sxs-lookup"><span data-stu-id="cffbd-150">For more information on creating record sets and defining record set parameter explicitly, see [Create a DNS record set](#create-a-dns-record-set).</span></span>

<span data-ttu-id="cffbd-151">Nem ad egy példa toocreate egy SOA típusú rekordhalmaz SOAs elkészítése és minden DNS-zóna törlődik, és nem hozható létre vagy törölt külön-külön.</span><span class="sxs-lookup"><span data-stu-id="cffbd-151">We do not give an example toocreate an SOA record set, since SOAs are created and deleted with each DNS zone and cannot be created or deleted separately.</span></span> <span data-ttu-id="cffbd-152">Azonban [SOA módosítható, egy újabb példában látható módon hello](#to-modify-an-SOA-record).</span><span class="sxs-lookup"><span data-stu-id="cffbd-152">However, [hello SOA can be modified, as shown in a later example](#to-modify-an-SOA-record).</span></span>

### <a name="create-an-aaaa-record"></a><span data-ttu-id="cffbd-153">Az AAAA-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-153">Create an AAAA record</span></span>

```azurecli
az network dns record-set aaaa set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-aaaa --ipv6-address 2607:f8b0:4009:1803::1005
```

### <a name="create-a-cname-record"></a><span data-ttu-id="cffbd-154">Hozzon létre egy CNAME rekordot</span><span class="sxs-lookup"><span data-stu-id="cffbd-154">Create a CNAME record</span></span>

> [!NOTE]
> <span data-ttu-id="cffbd-155">hello DNS-szabványokból nem teszik lehetővé a CNAME rekordot a zóna hello csúcsán (`--Name "@"`), és nem teszik egynél több rekordot tartalmazó rekordhalmazok.</span><span class="sxs-lookup"><span data-stu-id="cffbd-155">hello DNS standards do not permit CNAME records at hello apex of a zone (`--Name "@"`), nor do they permit record sets containing more than one record.</span></span>
> 
> <span data-ttu-id="cffbd-156">További információkért lásd: [CNAME rekordok](dns-zones-records.md#cname-records).</span><span class="sxs-lookup"><span data-stu-id="cffbd-156">For more information, see [CNAME records](dns-zones-records.md#cname-records).</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.contoso.com
```

### <a name="create-an-mx-record"></a><span data-ttu-id="cffbd-157">Az MX-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-157">Create an MX record</span></span>

<span data-ttu-id="cffbd-158">Ebben a példában hello rekordhalmaznevet használjuk "@" toocreate hello MX rekord: hello zóna felső pontja (ebben az esetben "a contoso.com").</span><span class="sxs-lookup"><span data-stu-id="cffbd-158">In this example, we use hello record set name "@" toocreate hello MX record at hello zone apex (in this case, "contoso.com").</span></span>

```azurecli
az network dns record-set mx set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --exchange mail.contoso.com --preference 5
```

### <a name="create-an-ns-record"></a><span data-ttu-id="cffbd-159">NS-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-159">Create an NS record</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-ns --nsdname ns1.contoso.com
```

### <a name="create-a-ptr-record"></a><span data-ttu-id="cffbd-160">A PTR típusú rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-160">Create a PTR record</span></span>

<span data-ttu-id="cffbd-161">Ebben az esetben "my-arpa-zónában (Zone.com)" jelöli hello ARPA zóna képviselő az IP-címtartományt.</span><span class="sxs-lookup"><span data-stu-id="cffbd-161">In this case, 'my-arpa-zone.com' represents hello ARPA zone representing your IP range.</span></span> <span data-ttu-id="cffbd-162">Minden egyes PTR típusú rekord a zóna tooan IP-címet az IP-címtartományon belül felel meg.</span><span class="sxs-lookup"><span data-stu-id="cffbd-162">Each PTR record set in this zone corresponds tooan IP address within this IP range.</span></span>  <span data-ttu-id="cffbd-163">hello rekordjának neve "10" hello utolsó oktett hello IP-cím, ez a bejegyzés által képviselt IP-címtartományon belül.</span><span class="sxs-lookup"><span data-stu-id="cffbd-163">hello record name '10' is hello last octet of hello IP address within this IP range represented by this record.</span></span>

```azurecli
az network dns record-set ptr set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name my-arpa.zone.com --ptrdname myservice.contoso.com
```

### <a name="create-an-srv-record"></a><span data-ttu-id="cffbd-164">Az SRV rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-164">Create an SRV record</span></span>

<span data-ttu-id="cffbd-165">Létrehozásakor egy [SRV-rekordhalmaz](dns-zones-records.md#srv-records), adja meg a hello  *\_szolgáltatás* és  *\_protokoll* hello a rekordhalmaz neveként.</span><span class="sxs-lookup"><span data-stu-id="cffbd-165">When creating an [SRV record set](dns-zones-records.md#srv-records), specify hello *\_service* and *\_protocol* in hello record set name.</span></span> <span data-ttu-id="cffbd-166">Nincs szükség tooinclude van "@" hello a rekordhalmaz neveként az SRV rekord létrehozása beállításakor: hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="cffbd-166">There is no need tooinclude "@" in hello record set name when creating an SRV record set at hello zone apex.</span></span>

```azurecli
az network dns record-set srv set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name _sip._tls --priority 10 --weight 5 --port 8080 --target sip.contoso.com
```

### <a name="create-a-txt-record"></a><span data-ttu-id="cffbd-167">TXT-rekord létrehozása</span><span class="sxs-lookup"><span data-stu-id="cffbd-167">Create a TXT record</span></span>

<span data-ttu-id="cffbd-168">hello következő példa bemutatja, hogyan toocreate egy TXT rögzíti.</span><span class="sxs-lookup"><span data-stu-id="cffbd-168">hello following example shows how toocreate a TXT record.</span></span> <span data-ttu-id="cffbd-169">További információ a támogatott TXT rekord hello karakterlánc maximális hossza: [TXT rekord](dns-zones-records.md#txt-records).</span><span class="sxs-lookup"><span data-stu-id="cffbd-169">For more information about hello maximum string length supported in TXT records, see [TXT records](dns-zones-records.md#txt-records).</span></span>

```azurecli
az network dns record-set txt set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-txt --value "This is a TXT record"
```

## <a name="get-a-record-set"></a><span data-ttu-id="cffbd-170">Rekordkészlet beolvasása</span><span class="sxs-lookup"><span data-stu-id="cffbd-170">Get a record set</span></span>

<span data-ttu-id="cffbd-171">Használjon egy meglévő rekordhalmaz tooretrieve `az network dns record-set <record-type> show`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-171">tooretrieve an existing record set, use `az network dns record-set <record-type> show`.</span></span> <span data-ttu-id="cffbd-172">További segítségért lásd: `az network dns record-set <record-type> show --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-172">For help, see `az network dns record-set <record-type> show --help`.</span></span>

<span data-ttu-id="cffbd-173">Egy rekord és a rekordhalmaz létrehozásakor hello rekord állíthat be, a megadott név lehet a *relatív* nevét, azaz kell zárnia a hello zóna neve.</span><span class="sxs-lookup"><span data-stu-id="cffbd-173">As when creating a record or record set, hello record set name given must be a *relative* name, meaning it must exclude hello zone name.</span></span> <span data-ttu-id="cffbd-174">Szükség toospecify hello rekordtípust, hello rekordot tartalmazó hello zóna beállítása, hello hello zóna tartalmazó erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="cffbd-174">You also need toospecify hello record type, hello zone containing hello record set, and hello resource group containing hello zone.</span></span>

<span data-ttu-id="cffbd-175">hello alábbi példa hello rekordot be *www* , adjon meg egy zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="cffbd-175">hello following example retrieves hello record *www* of type A from zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a show --resource-group myresourcegroup --zone-name contoso.com --name www
```

## <a name="list-record-sets"></a><span data-ttu-id="cffbd-176">Lista rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="cffbd-176">List record sets</span></span>

<span data-ttu-id="cffbd-177">Minden rekordot a DNS-zónák hello segítségével listázhatja `az network dns record-set list` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cffbd-177">You can list all records in a DNS zone by using hello `az network dns record-set list` command.</span></span> <span data-ttu-id="cffbd-178">További segítségért lásd: `az network dns record-set list --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-178">For help, see `az network dns record-set list --help`.</span></span>

<span data-ttu-id="cffbd-179">Ebben a példában adja vissza az összes rekordhalmazok hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, név vagy rekordtípus függetlenül:</span><span class="sxs-lookup"><span data-stu-id="cffbd-179">This example returns all record sets in hello zone *contoso.com*, in resource group *MyResourceGroup*, regardless of name or record type:</span></span>

```azurecli
az network dns record-set list --resource-group myresourcegroup --zone-name contoso.com
```

<span data-ttu-id="cffbd-180">Ebben a példában, amelyek megfelelnek a megadott rekordtípus (ebben az esetben az "A" rekordok) hello minden rekordkészletet ad vissza:</span><span class="sxs-lookup"><span data-stu-id="cffbd-180">This example returns all record sets that match hello given record type (in this case, 'A' records):</span></span>

```azurecli
az network dns record-set a list --resource-group myresourcegroup --zone-name contoso.com 
```

## <a name="add-a-record-tooan-existing-record-set"></a><span data-ttu-id="cffbd-181">A meglévő rekordhalmazt rekord tooan hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cffbd-181">Add a record tooan existing record set</span></span>

<span data-ttu-id="cffbd-182">Használhat `az network dns record-set <record-type> set-record` mindkét toocreate egy új rekordot a rekordhalmaz vagy tooadd rekord tooan meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="cffbd-182">You can use `az network dns record-set <record-type> set-record` both toocreate a record in a new record set, or tooadd a record tooan existing record set.</span></span>

<span data-ttu-id="cffbd-183">További információkért lásd: [hozzon létre egy DNS-rekord](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) felett.</span><span class="sxs-lookup"><span data-stu-id="cffbd-183">For more information, see [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

## <a name="remove-a-record-from-an-existing-record-set"></a><span data-ttu-id="cffbd-184">Távolítsa el a rekord egy meglévő rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="cffbd-184">Remove a record from an existing record set.</span></span>

<span data-ttu-id="cffbd-185">meglévő rekordhalmaz használata tooremove egy DNS-bejegyzést `az network dns record-set <record-type> remove-record`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-185">tooremove a DNS record from an existing record set, use `az network dns record-set <record-type> remove-record`.</span></span> <span data-ttu-id="cffbd-186">További segítségért lásd: `az network dns record-set <record-type> remove-record -h`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-186">For help, see `az network dns record-set <record-type> remove-record -h`.</span></span>

<span data-ttu-id="cffbd-187">Törli a DNS-rekordot a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="cffbd-187">This command deletes a DNS record from a record set.</span></span> <span data-ttu-id="cffbd-188">Utolsó rekordját hello a rekordkészlet törlésekor hello rekordhalmaz maga is törlődik.</span><span class="sxs-lookup"><span data-stu-id="cffbd-188">If hello last record in a record set is deleted, hello record set itself is also deleted.</span></span> <span data-ttu-id="cffbd-189">tookeep hello üres rekordhalmaz inkább hello `--keep-empty-record-set` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="cffbd-189">tookeep hello empty record set instead, use hello `--keep-empty-record-set` option.</span></span>

<span data-ttu-id="cffbd-190">Toospecify kell hello törölt rekord toobe és azt törölni kell, használatával hello zóna hello ugyanazokkal a paraméterekkel, egy rekord segítségével létrehozásakor `az network dns record-set <record-type> set-record`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-190">You need toospecify hello record toobe deleted and hello zone it should be deleted from, using hello same parameters as when creating a record using `az network dns record-set <record-type> set-record`.</span></span> <span data-ttu-id="cffbd-191">Ezek a paraméterek ismertetett [DNS-rekord létrehozása](#create-a-dns-record) és [más típusú rekordok létrehozásának](#create-records-of-other-types) fent.</span><span class="sxs-lookup"><span data-stu-id="cffbd-191">These parameters are described in [Create a DNS record](#create-a-dns-record) and [Create records of other types](#create-records-of-other-types) above.</span></span>

<span data-ttu-id="cffbd-192">a következő példa törlések hello hello "1.2.3.4" hello rekordból beállítása a nevesített értékű egy olyan rekordot *www* hello zónában *contoso.com*, hello erőforráscsoportban *MyResourceGroup*.</span><span class="sxs-lookup"><span data-stu-id="cffbd-192">hello following example deletes hello A record with value '1.2.3.4' from hello record set named *www* in hello zone *contoso.com*, in hello resource group *MyResourceGroup*.</span></span>

```azurecli
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "www" --ipv4-address 1.2.3.4
```

## <a name="modify-an-existing-record-set"></a><span data-ttu-id="cffbd-193">Módosíthatja egy meglévő rekordkészlete</span><span class="sxs-lookup"><span data-stu-id="cffbd-193">Modify an existing record set</span></span>

<span data-ttu-id="cffbd-194">Minden rekordhalmaz tartalmaz egy [idő-Élettartam (TTL)](dns-zones-records.md#time-to-live), [metaadatok](dns-zones-records.md#tags-and-metadata), és a DNS-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-194">Each record set contains a [time-to-live (TTL)](dns-zones-records.md#time-to-live), [metadata](dns-zones-records.md#tags-and-metadata), and DNS records.</span></span> <span data-ttu-id="cffbd-195">hello alábbi szakaszok azt ismertetik, hogyan toomodify minden ezeket a tulajdonságokat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-195">hello following sections explain how toomodify each of these properties.</span></span>

### <a name="toomodify-an-a-aaaa-mx-ns-ptr-srv-or-txt-record"></a><span data-ttu-id="cffbd-196">az A, AAAA, MX, NS, PTR, SRV és TXT rekord toomodify</span><span class="sxs-lookup"><span data-stu-id="cffbd-196">toomodify an A, AAAA, MX, NS, PTR, SRV, or TXT record</span></span>

<span data-ttu-id="cffbd-197">Adjon meg egy, AAAA, MX, NS, PTR, SRV és TXT meglévő bejegyzés toomodify, először adjon egy új rekordot, és a delete hello meglévő rekord.</span><span class="sxs-lookup"><span data-stu-id="cffbd-197">toomodify an existing record of type A, AAAA, MX, NS, PTR, SRV, or TXT, you should first add a new record and then delete hello existing record.</span></span> <span data-ttu-id="cffbd-198">Részletes útmutatást toodelete és rekordok hozzáadásához, tekintse meg a cikk korábbi szakaszaiban hello.</span><span class="sxs-lookup"><span data-stu-id="cffbd-198">For detailed instructions on how toodelete and add records, see hello earlier sections of this article.</span></span>

<span data-ttu-id="cffbd-199">hello a következő példa bemutatja, hogyan toomodify az "A" rekord, az IP-cím 1.2.3.4 tooIP cím 5.6.7.8:</span><span class="sxs-lookup"><span data-stu-id="cffbd-199">hello following example shows how toomodify an 'A' record, from IP address 1.2.3.4 tooIP address 5.6.7.8:</span></span>

```azurecli
az network dns record-set a set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 5.6.7.8
az network dns record-set a remove-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name www --ipv4-address 1.2.3.4
```

<span data-ttu-id="cffbd-200">Nem lehet hozzáadása, eltávolítása vagy módosítása hello rekord automatikusan létrejön az NS típusú rekordhalmaz hello zóna csúcsán hello (`--Name "@"`, beleértve az ajánlat jelek).</span><span class="sxs-lookup"><span data-stu-id="cffbd-200">You cannot add, remove, or modify hello records in hello automatically created NS record set at hello zone apex (`--Name "@"`, including quote marks).</span></span> <span data-ttu-id="cffbd-201">A rekordhalmaz hello csak történt változások engedélyezve, toomodify a hello rekordhalmaz a TTL-t és a metaadatok.</span><span class="sxs-lookup"><span data-stu-id="cffbd-201">For this record set, hello only changes permitted are toomodify hello record set TTL and metadata.</span></span>

### <a name="toomodify-a-cname-record"></a><span data-ttu-id="cffbd-202">egy olyan CNAME rekordot toomodify</span><span class="sxs-lookup"><span data-stu-id="cffbd-202">toomodify a CNAME record</span></span>

<span data-ttu-id="cffbd-203">Ellentétben a legtöbb más rekordtípusokhoz egy olyan CNAME-rekordhalmazt csak egyetlen rekordot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-203">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="cffbd-204">Új rekord hozzáadása és eltávolítása hello meglévő rekord, mint az egyéb típusú bejegyzés által, ezért nem cserélhető le hello aktuális értéke.</span><span class="sxs-lookup"><span data-stu-id="cffbd-204">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="cffbd-205">Egy olyan CNAME rekordot toomodify inkább `az network dns record-set cname set-record`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-205">Instead, toomodify a CNAME record, use `az network dns record-set cname set-record`.</span></span> <span data-ttu-id="cffbd-206">Útmutatásért lásd:`az network dns record-set cname set-record --help`</span><span class="sxs-lookup"><span data-stu-id="cffbd-206">For help, see `az network dns record-set cname set-record --help`</span></span>

<span data-ttu-id="cffbd-207">hello példa módosítja hello CNAME rekordkészlete *www* hello zónában *contoso.com*, erőforráscsoportban *MyResourceGroup*, toopoint túl "www.fabrikam.net" helyett a meglévő érték:</span><span class="sxs-lookup"><span data-stu-id="cffbd-207">hello example modifies hello CNAME record set *www* in hello zone *contoso.com*, in resource group *MyResourceGroup*, toopoint too'www.fabrikam.net' instead of its existing value:</span></span>

```azurecli
az network dns record-set cname set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name test-cname --cname www.fabrikam.net
``` 

### <a name="toomodify-an-soa-record"></a><span data-ttu-id="cffbd-208">egy SOA rekordot toomodify</span><span class="sxs-lookup"><span data-stu-id="cffbd-208">toomodify an SOA record</span></span>

<span data-ttu-id="cffbd-209">Ellentétben a legtöbb más rekordtípusokhoz egy olyan CNAME-rekordhalmazt csak egyetlen rekordot tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="cffbd-209">Unlike most other record types, a CNAME record set can only contain a single record.</span></span>  <span data-ttu-id="cffbd-210">Új rekord hozzáadása és eltávolítása hello meglévő rekord, mint az egyéb típusú bejegyzés által, ezért nem cserélhető le hello aktuális értéke.</span><span class="sxs-lookup"><span data-stu-id="cffbd-210">Therefore, you cannot replace hello current value by adding a new record and removing hello existing record, as for other record types.</span></span>

<span data-ttu-id="cffbd-211">Toomodify hello SOA típusú rekordjának, inkább `az network dns record-set soa update`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-211">Instead, toomodify hello SOA record, use `az network dns record-set soa update`.</span></span> <span data-ttu-id="cffbd-212">További segítségért lásd: `az network dns record-set soa update --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-212">For help, see `az network dns record-set soa update --help`.</span></span>

<span data-ttu-id="cffbd-213">hello következő példa bemutatja, hogyan hello SOA tooset hello "e-mail" tulajdonságának hello zóna rekordja *contoso.com* hello erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="cffbd-213">hello following example shows how tooset hello 'email' property of hello SOA record for hello zone *contoso.com* in hello resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set soa update --resource-group myresourcegroup --zone-name contoso.com --email admin.contoso.com
```

### <a name="toomodify-ns-records-at-hello-zone-apex"></a><span data-ttu-id="cffbd-214">hello zóna felső pontja toomodify Névkiszolgálói rekordok</span><span class="sxs-lookup"><span data-stu-id="cffbd-214">toomodify NS records at hello zone apex</span></span>

<span data-ttu-id="cffbd-215">Állítsa be megfelelően hello zóna felső pontja hello NS-rekord automatikusan hozza létre minden DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="cffbd-215">hello NS record set at hello zone apex is automatically created with each DNS zone.</span></span> <span data-ttu-id="cffbd-216">Hello Azure DNS-név kiszolgálók hozzárendelt toohello zóna hello szerepel benne.</span><span class="sxs-lookup"><span data-stu-id="cffbd-216">It contains hello names of hello Azure DNS name servers assigned toohello zone.</span></span>

<span data-ttu-id="cffbd-217">Hozzáadhat további neve kiszolgálók toothis NS rekordhalmaz, toosupport közös futtató tartományok egynél több DNS-szolgáltatónál.</span><span class="sxs-lookup"><span data-stu-id="cffbd-217">You can add additional name servers toothis NS record set, toosupport co-hosting domains with more than one DNS provider.</span></span> <span data-ttu-id="cffbd-218">Hello TTL-t és a rekordhalmaz metaadatait is módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="cffbd-218">You can also modify hello TTL and metadata for this record set.</span></span> <span data-ttu-id="cffbd-219">Azonban nem lehet eltávolítani vagy módosítani hello előre megadott Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="cffbd-219">However, you cannot remove or modify hello pre-populated Azure DNS name servers.</span></span>

<span data-ttu-id="cffbd-220">Vegye figyelembe, hogy ez érvényes csak toohello NS rekord beállított hello zóna felső pontja.</span><span class="sxs-lookup"><span data-stu-id="cffbd-220">Note that this applies only toohello NS record set at hello zone apex.</span></span> <span data-ttu-id="cffbd-221">Más NS rekordhalmazok, amelyek a zónához (a használt toodelegate gyermekzónákhoz) korlátozás nélkül lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="cffbd-221">Other NS record sets in your zone (as used toodelegate child zones) can be modified without constraint.</span></span>

<span data-ttu-id="cffbd-222">hello a következő példa bemutatja, hogyan tooadd további neve server toohello NS-rekord állítják hello zóna felső pontja:</span><span class="sxs-lookup"><span data-stu-id="cffbd-222">hello following example shows how tooadd an additional name server toohello NS record set at hello zone apex:</span></span>

```azurecli
az network dns record-set ns set-record --resource-group myresourcegroup --zone-name contoso.com --record-set-name "@" --nsdname ns1.myotherdnsprovider.com 
```

### <a name="toomodify-hello-ttl-of-an-existing-record-set"></a><span data-ttu-id="cffbd-223">egy meglévő bejegyzés TTL toomodify hello beállítása</span><span class="sxs-lookup"><span data-stu-id="cffbd-223">toomodify hello TTL of an existing record set</span></span>

<span data-ttu-id="cffbd-224">toomodify hello TTL-t egy meglévő bejegyzés megadásához használja `azure network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-224">toomodify hello TTL of an existing record set, use `azure network dns record-set <record-type> update`.</span></span> <span data-ttu-id="cffbd-225">További segítségért lásd: `azure network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-225">For help, see `azure network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="cffbd-226">hello a következő példa bemutatja, hogyan toomodify rekordhalmaz TTL-t, ez az eset too60 másodperc:</span><span class="sxs-lookup"><span data-stu-id="cffbd-226">hello following example shows how toomodify a record set TTL, in this case too60 seconds:</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set ttl=60
```

### <a name="toomodify-hello-metadata-of-an-existing-record-set"></a><span data-ttu-id="cffbd-227">egy meglévő rekordkészlet toomodify hello metaadatai</span><span class="sxs-lookup"><span data-stu-id="cffbd-227">toomodify hello metadata of an existing record set</span></span>

<span data-ttu-id="cffbd-228">[A rekordhalmaz metaadatok](dns-zones-records.md#tags-and-metadata) kulcs-érték párként minden rekordhalmaz használt tooassociate az alkalmazás-specifikus adatok is lehetnek.</span><span class="sxs-lookup"><span data-stu-id="cffbd-228">[Record set metadata](dns-zones-records.md#tags-and-metadata) can be used tooassociate application-specific data with each record set, as key-value pairs.</span></span> <span data-ttu-id="cffbd-229">egy meglévő bejegyzés toomodify hello metaadatok megadásához használja `az network dns record-set <record-type> update`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-229">toomodify hello metadata of an existing record set, use `az network dns record-set <record-type> update`.</span></span> <span data-ttu-id="cffbd-230">További segítségért lásd: `az network dns record-set <record-type> update --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-230">For help, see `az network dns record-set <record-type> update --help`.</span></span>

<span data-ttu-id="cffbd-231">hello következő példa bemutatja, hogyan toomodify nevű rekordhalmaz két metaadat-bejegyzést, "osztály pénzügyi =" és "környezet éles =".</span><span class="sxs-lookup"><span data-stu-id="cffbd-231">hello following example shows how toomodify a record set with two metadata entries, "dept=finance" and "environment=production".</span></span> <span data-ttu-id="cffbd-232">Vegye figyelembe, hogy a meglévő metaadatokat *helyett* által hello értékekből.</span><span class="sxs-lookup"><span data-stu-id="cffbd-232">Note that any existing metadata is *replaced* by hello values given.</span></span>

```azurecli
az network dns record-set a update --resource-group myresourcegroup --zone-name contoso.com --name www --set metadata.dept=finance metadata.environment=production
```

## <a name="delete-a-record-set"></a><span data-ttu-id="cffbd-233">A rekordhalmaz törlése</span><span class="sxs-lookup"><span data-stu-id="cffbd-233">Delete a record set</span></span>

<span data-ttu-id="cffbd-234">Rekordhalmazok hello segítségével törölhetők `az network dns record-set <record-type> delete` parancsot.</span><span class="sxs-lookup"><span data-stu-id="cffbd-234">Record sets can be deleted by using hello `az network dns record-set <record-type> delete` command.</span></span> <span data-ttu-id="cffbd-235">További segítségért lásd: `azure network dns record-set <record-type> delete --help`.</span><span class="sxs-lookup"><span data-stu-id="cffbd-235">For help, see `azure network dns record-set <record-type> delete --help`.</span></span> <span data-ttu-id="cffbd-236">Rekordhalmaz törlése is törli az összes rekordján hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="cffbd-236">Deleting a record set also deletes all records within hello record set.</span></span>

> [!NOTE]
> <span data-ttu-id="cffbd-237">Nem lehet törölni hello SOA és NS rekordhalmazok hello zóna csúcsán (`--name "@"`).</span><span class="sxs-lookup"><span data-stu-id="cffbd-237">You cannot delete hello SOA and NS record sets at hello zone apex (`--name "@"`).</span></span>  <span data-ttu-id="cffbd-238">Ezek automatikusan létrejönnek hello zóna mikor jött létre, és hello zóna törlésekor automatikusan törlődik.</span><span class="sxs-lookup"><span data-stu-id="cffbd-238">These are created automatically when hello zone was created, and are deleted automatically when hello zone is deleted.</span></span>

<span data-ttu-id="cffbd-239">hello alábbi példa törli hello rekordhalmaz elnevezett *www* , adjon meg egy hello zónából *contoso.com* erőforráscsoportban *MyResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="cffbd-239">hello following example deletes hello record set named *www* of type A from hello zone *contoso.com* in resource group *MyResourceGroup*:</span></span>

```azurecli
az network dns record-set a delete --resource-group myresourcegroup --zone-name contoso.com --name www
```

<span data-ttu-id="cffbd-240">Biztosan felszólító tooconfirm hello törlési műveletet.</span><span class="sxs-lookup"><span data-stu-id="cffbd-240">You are prompted tooconfirm hello delete operation.</span></span> <span data-ttu-id="cffbd-241">a parancssorba toosuppress hello használata `--yes` váltani.</span><span class="sxs-lookup"><span data-stu-id="cffbd-241">toosuppress this prompt, use hello `--yes` switch.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cffbd-242">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cffbd-242">Next steps</span></span>

<span data-ttu-id="cffbd-243">További információ [zónák és az Azure DNS-rekordok](dns-zones-records.md).</span><span class="sxs-lookup"><span data-stu-id="cffbd-243">Learn more about [zones and records in Azure DNS](dns-zones-records.md).</span></span>
<br>
<span data-ttu-id="cffbd-244">Ismerje meg, hogyan túl[védelme a zóna és a rekordok](dns-protect-zones-recordsets.md) Azure DNS használata esetén.</span><span class="sxs-lookup"><span data-stu-id="cffbd-244">Learn how too[protect your zones and records](dns-protect-zones-recordsets.md) when using Azure DNS.</span></span>
