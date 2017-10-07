---
title: "aaaDelegate a tartomány tooAzure DNS |} Microsoft Docs"
description: "Ismerje meg, hogyan toochange tartományok delegálását és használata Azure DNS neve kiszolgálók tooprovide tartomány üzemeltetéséhez."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 257da6ec-d6e2-4b6f-ad76-ee2dde4efbcc
ms.service: dns
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/12/2017
ms.author: gwallace
ms.openlocfilehash: f780bdaa416150e5e3afe6c6845dc75ba54b6203
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-a-domain-tooazure-dns"></a><span data-ttu-id="80fc9-103">A tartomány tooAzure DNS delegálása</span><span class="sxs-lookup"><span data-stu-id="80fc9-103">Delegate a domain tooAzure DNS</span></span>

<span data-ttu-id="80fc9-104">Az Azure DNS lehetővé teszi a DNS-zóna toohost, és egy tartományhoz az Azure-ban hello DNS-rekordok kezelése.</span><span class="sxs-lookup"><span data-stu-id="80fc9-104">Azure DNS allows you toohost a DNS zone and manage hello DNS records for a domain in Azure.</span></span> <span data-ttu-id="80fc9-105">Ahhoz, hogy a tartomány tooreach Azure DNS-ben a DNS-lekérdezések, hello tartomány rendelkezik toobe hello szülőtartomány tooAzure DNS átadva.</span><span class="sxs-lookup"><span data-stu-id="80fc9-105">In order for DNS queries for a domain tooreach Azure DNS, hello domain has toobe delegated tooAzure DNS from hello parent domain.</span></span> <span data-ttu-id="80fc9-106">Ne feledje Azure DNS nincs hello tartományregisztráló.</span><span class="sxs-lookup"><span data-stu-id="80fc9-106">Keep in mind Azure DNS is not hello domain registrar.</span></span> <span data-ttu-id="80fc9-107">Ez a cikk azt ismerteti, hogyan toodelegate a tartomány tooAzure DNS.</span><span class="sxs-lookup"><span data-stu-id="80fc9-107">This article explains how toodelegate your domain tooAzure DNS.</span></span>

<span data-ttu-id="80fc9-108">A tartományokhoz a regisztráló vásárolt a regisztráló kínál hello beállítás tooset be ezeket a Névkiszolgálói rekordokat.</span><span class="sxs-lookup"><span data-stu-id="80fc9-108">For domains purchased from a registrar, your registrar offers hello option tooset up these NS records.</span></span> <span data-ttu-id="80fc9-109">Nincs tooown egy tartományi toocreate ezt a nevet a DNS-zóna Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="80fc9-109">You do not have tooown a domain toocreate a DNS zone with that domain name in Azure DNS.</span></span> <span data-ttu-id="80fc9-110">Azonban hello regisztráló tooown hello tartomány tooset hello delegálás tooAzure DNS mentése szükséges.</span><span class="sxs-lookup"><span data-stu-id="80fc9-110">However, you do need tooown hello domain tooset up hello delegation tooAzure DNS with hello registrar.</span></span>

<span data-ttu-id="80fc9-111">Tegyük fel például, a "contoso.net" hello tartomány vásárlása, és hozzon létre egy hello neve "contoso.net" zónát az Azure DNS.</span><span class="sxs-lookup"><span data-stu-id="80fc9-111">For example, suppose you purchase hello domain 'contoso.net' and create a zone with hello name 'contoso.net' in Azure DNS.</span></span> <span data-ttu-id="80fc9-112">Hello tartomány hello tulajdonosaként a regisztráló kínál, a tartomány hello beállítás tooconfigure hello névkiszolgáló-címet (Ez azt jelenti, hogy hello Névkiszolgálói rekordokat).</span><span class="sxs-lookup"><span data-stu-id="80fc9-112">As hello owner of hello domain, your registrar offers you hello option tooconfigure hello name server addresses (that is, hello NS records) for your domain.</span></span> <span data-ttu-id="80fc9-113">hello regisztráló ezeket a Névkiszolgálói rekordokat hello szülőtartományban, ebben az esetben ".net" tárolja.</span><span class="sxs-lookup"><span data-stu-id="80fc9-113">hello registrar stores these NS records in hello parent domain, in this case '.net'.</span></span> <span data-ttu-id="80fc9-114">Hello world különböző pontjain található ügyfelek majd lehet irányított tooyour tartományhoz az Azure DNS-zóna tooresolve DNS-rekordokat a "contoso.net" tett kísérlet során.</span><span class="sxs-lookup"><span data-stu-id="80fc9-114">Clients around hello world can then be directed tooyour domain in Azure DNS zone when trying tooresolve DNS records in 'contoso.net'.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="80fc9-115">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="80fc9-115">Create a DNS zone</span></span>

1. <span data-ttu-id="80fc9-116">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="80fc9-116">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="80fc9-117">Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-117">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="80fc9-119">A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="80fc9-119">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="80fc9-120">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="80fc9-120">**Setting**</span></span> | <span data-ttu-id="80fc9-121">**Érték**</span><span class="sxs-lookup"><span data-stu-id="80fc9-121">**Value**</span></span> | <span data-ttu-id="80fc9-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="80fc9-122">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="80fc9-123">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="80fc9-123">**Name**</span></span>|<span data-ttu-id="80fc9-124">contoso.net</span><span class="sxs-lookup"><span data-stu-id="80fc9-124">contoso.net</span></span>|<span data-ttu-id="80fc9-125">hello hello DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="80fc9-125">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="80fc9-126">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="80fc9-126">**Subscription**</span></span>|<span data-ttu-id="80fc9-127">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="80fc9-127">[Your subscription]</span></span>|<span data-ttu-id="80fc9-128">Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.</span><span class="sxs-lookup"><span data-stu-id="80fc9-128">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="80fc9-129">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="80fc9-129">**Resource group**</span></span>|<span data-ttu-id="80fc9-130">**Új létrehozása:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="80fc9-130">**Create new:** contosoRG</span></span>|<span data-ttu-id="80fc9-131">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80fc9-131">Create a resource group.</span></span> <span data-ttu-id="80fc9-132">az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="80fc9-132">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="80fc9-133">További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.</span><span class="sxs-lookup"><span data-stu-id="80fc9-133">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="80fc9-134">**Hely**</span><span class="sxs-lookup"><span data-stu-id="80fc9-134">**Location**</span></span>|<span data-ttu-id="80fc9-135">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="80fc9-135">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="80fc9-136">hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="80fc9-136">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="80fc9-137">mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80fc9-137">hello DNS zone location is always "global", and is not shown.</span></span>

## <a name="retrieve-name-servers"></a><span data-ttu-id="80fc9-138">Névkiszolgálók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="80fc9-138">Retrieve name servers</span></span>

<span data-ttu-id="80fc9-139">A DNS-zóna tooAzure DNS delegálhatja, mielőtt először tooknow hello névkiszolgálói neveket a zóna.</span><span class="sxs-lookup"><span data-stu-id="80fc9-139">Before you can delegate your DNS zone tooAzure DNS, you first need tooknow hello name server names for your zone.</span></span> <span data-ttu-id="80fc9-140">Minden zóna létrehozásakor az Azure DNS egy névkiszolgálói készletből választ ki egyet.</span><span class="sxs-lookup"><span data-stu-id="80fc9-140">Azure DNS allocates name servers from a pool each time a zone is created.</span></span>

1. <span data-ttu-id="80fc9-141">A hello DNS-zóna hello Azure-portálon létrehozott **Kedvencek** ablaktáblán kattintson a **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="80fc9-141">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="80fc9-142">Kattintson a hello **contoso.net** hello DNS-zónát **összes erőforrás** panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-142">Click hello **contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="80fc9-143">Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **contoso.net** a hello szűrő név szerint...</span><span class="sxs-lookup"><span data-stu-id="80fc9-143">If hello subscription you selected already has several resources in it, you can enter **contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="80fc9-144">mezőbe tooeasily hozzáférés hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="80fc9-144">box tooeasily access hello application gateway.</span></span> 

1. <span data-ttu-id="80fc9-145">Hello névkiszolgálók lekérése hello DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-145">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="80fc9-146">Ebben a példában a "contoso.net" zónához hello rendelték névkiszolgálókat "ns1-01.azure-dns.com", "ns2-01.azure-DNS.NET", "ns3-01.azure-dns.org", és "ns4-01.azure-dns.info":</span><span class="sxs-lookup"><span data-stu-id="80fc9-146">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="80fc9-148">Az Azure DNS automatikusan létrehozza a mérvadó Névkiszolgálói rekordokat, amelyek a zónához hozzárendelt névkiszolgálók hello tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="80fc9-148">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="80fc9-149">toosee hello névkiszolgáló nevek Azure PowerShell vagy Azure parancssori felületen keresztül, egyszerűen szükséges tooretrieve ezeket a rekordokat.</span><span class="sxs-lookup"><span data-stu-id="80fc9-149">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

<span data-ttu-id="80fc9-150">hello következő példákban is lépéseit írják le hello tooretrieve hello névkiszolgálók az PowerShell és az Azure CLI Azure DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="80fc9-150">hello following examples also provide hello steps tooretrieve hello name servers for a zone in Azure DNS with PowerShell and Azure CLI.</span></span>

### <a name="powershell"></a><span data-ttu-id="80fc9-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80fc9-151">PowerShell</span></span>

```powershell
# hello record name "@" is used toorefer toorecords at hello top of hello zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

<span data-ttu-id="80fc9-152">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="80fc9-152">hello following example is hello response.</span></span>

```
Name              : @
ZoneName          : contoso.net
ResourceGroupName : contosorg
Ttl               : 172800
Etag              : 03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5
RecordType        : NS
Records           : {ns1-07.azure-dns.com., ns2-07.azure-dns.net., ns3-07.azure-dns.org.,
                    ns4-07.azure-dns.info.}
Metadata          :
```

### <a name="azure-cli"></a><span data-ttu-id="80fc9-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80fc9-153">Azure CLI</span></span>

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

<span data-ttu-id="80fc9-154">a következő példa hello hello válasz.</span><span class="sxs-lookup"><span data-stu-id="80fc9-154">hello following example is hello response.</span></span>

```json
{
  "etag": "03bff8f1-9c60-4a9b-ad9d-ac97366ee4d5",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosoRG/providers/Microsoft.Network/dnszones/contoso.net/NS/@",
  "metadata": null,
  "name": "@",
  "nsRecords": [
    {
      "nsdname": "ns1-07.azure-dns.com."
    },
    {
      "nsdname": "ns2-07.azure-dns.net."
    },
    {
      "nsdname": "ns3-07.azure-dns.org."
    },
    {
      "nsdname": "ns4-07.azure-dns.info."
    }
  ],
  "resourceGroup": "contosoRG",
  "ttl": 172800,
  "type": "Microsoft.Network/dnszones/NS"
}
```

## <a name="delegate-hello-domain"></a><span data-ttu-id="80fc9-155">Delegált hello tartomány</span><span class="sxs-lookup"><span data-stu-id="80fc9-155">Delegate hello domain</span></span>

<span data-ttu-id="80fc9-156">Most, hogy hello DNS-zóna jön létre, és el kell hello névkiszolgálókat, hello szülőtartomány kell toobe hello Azure DNS névkiszolgálóit frissült.</span><span class="sxs-lookup"><span data-stu-id="80fc9-156">Now that hello DNS zone is created and you have hello name servers, hello parent domain needs toobe updated with hello Azure DNS name servers.</span></span> <span data-ttu-id="80fc9-157">Minden tartományregisztráló a saját DNS felügyeleti eszközök toochange hello névkiszolgálójának rekordjai egy tartományhoz tartozik.</span><span class="sxs-lookup"><span data-stu-id="80fc9-157">Each registrar has their own DNS management tools toochange hello name server records for a domain.</span></span> <span data-ttu-id="80fc9-158">Hello regisztráló DNS kezelése lapon hello NS rekord szerkesztését, és cserélje le hello Névkiszolgálói rekordokat Azure DNS-ben létrehozott hello néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="80fc9-158">In hello registrar's DNS management page, edit hello NS records and replace hello NS records with hello ones Azure DNS created.</span></span>

<span data-ttu-id="80fc9-159">Amikor delegálása a tartományi tooAzure DNS, az Azure DNS által nyújtott hello névkiszolgálói neveket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="80fc9-159">When delegating a domain tooAzure DNS, you must use hello name server names provided by Azure DNS.</span></span> <span data-ttu-id="80fc9-160">Ajánlott minden toouse négy kiszolgálók nevei, függetlenül a tartomány nevét hello neve.</span><span class="sxs-lookup"><span data-stu-id="80fc9-160">It is recommended toouse all four name server names, regardless of hello name of your domain.</span></span> <span data-ttu-id="80fc9-161">Tartományok delegálását nem kell hello neve server name toouse hello a tartomány legfelső szintű tartományában.</span><span class="sxs-lookup"><span data-stu-id="80fc9-161">Domain delegation does not require hello name server name toouse hello same top-level domain as your domain.</span></span>

<span data-ttu-id="80fc9-162">Mivel a következő IP-címek megváltozhatnak a jövőben ne használjon "összetartó rekordokat" toopoint toohello Azure DNS neve kiszolgáló IP-címek.</span><span class="sxs-lookup"><span data-stu-id="80fc9-162">You should not use 'glue records' toopoint toohello Azure DNS name server IP addresses, since these IP addresses may change in future.</span></span> <span data-ttu-id="80fc9-163">A saját zónájában történő, névkiszolgálói neveket használó delegálásokat – más néven „személyes névkiszolgálókat” – az Azure DNS jelenleg nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="80fc9-163">Delegations using name server names in your own zone, sometimes called 'vanity name servers', are not currently supported in Azure DNS.</span></span>

## <a name="verify-name-resolution-is-working"></a><span data-ttu-id="80fc9-164">A névfeloldás működésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="80fc9-164">Verify name resolution is working</span></span>

<span data-ttu-id="80fc9-165">Hello delegálás befejezése után ellenőrizheti, hogy névfeloldás működik-e olyan eszköz, például az "nslookup" tooquery hello SOA rekordot a zóna (amely szintén automatikusan létrejön hello zóna létrehozásakor) használatával.</span><span class="sxs-lookup"><span data-stu-id="80fc9-165">After completing hello delegation, you can verify that name resolution is working by using a tool such as 'nslookup' tooquery hello SOA record for your zone (which is also automatically created when hello zone is created).</span></span>

<span data-ttu-id="80fc9-166">Nem rendelkezik toospecify hello Azure DNS névkiszolgálóit, ha hello delegálás beállítása helyes, hello normál DNS-feloldási folyamat megkeresi hello névkiszolgálók automatikusan.</span><span class="sxs-lookup"><span data-stu-id="80fc9-166">You do not have toospecify hello Azure DNS name servers, if hello delegation has been set up correctly, hello normal DNS resolution process finds hello name servers automatically.</span></span>

```
nslookup -type=SOA contoso.com
```

<span data-ttu-id="80fc9-167">hello az alábbiakban látható egy példa egy válasz az előző parancs hello:</span><span class="sxs-lookup"><span data-stu-id="80fc9-167">hello following is an example response from hello preceding command:</span></span>

```
Server: ns1-04.azure-dns.com
Address: 208.76.47.4

contoso.com
primary name server = ns1-04.azure-dns.com
responsible mail addr = msnhst.microsoft.com
serial = 1
refresh = 900 (15 mins)
retry = 300 (5 mins)
expire = 604800 (7 days)
default TTL = 300 (5 mins)
```

## <a name="delegate-sub-domains-in-azure-dns"></a><span data-ttu-id="80fc9-168">Altartományok delegálása az Azure DNS-ben</span><span class="sxs-lookup"><span data-stu-id="80fc9-168">Delegate sub-domains in Azure DNS</span></span>

<span data-ttu-id="80fc9-169">Ha azt szeretné, hogy fel egy különálló gyermekzónát tooset, delegálhat egy altartomány Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="80fc9-169">If you want tooset up a separate child zone, you can delegate a sub-domain in Azure DNS.</span></span> <span data-ttu-id="80fc9-170">Például, hogy beállítása és delegált "contoso.net" Azure DNS-beli tegyük fel, hogy tooset fel egy különálló gyermekzónát szeretne "partners.contoso.net".</span><span class="sxs-lookup"><span data-stu-id="80fc9-170">For example, having set up and delegated 'contoso.net' in Azure DNS, suppose you would like tooset up a separate child zone, 'partners.contoso.net'.</span></span>

1. <span data-ttu-id="80fc9-171">Hozzon létre hello gyermek zóna "partners.contoso.net" Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="80fc9-171">Create hello child zone 'partners.contoso.net' in Azure DNS.</span></span>
2. <span data-ttu-id="80fc9-172">Kereshet a mérvadó Névkiszolgálói rekordokat hello hello gyermek zóna tooobtain hello üzemeltető névkiszolgálókat hello gyermekzónát az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="80fc9-172">Look up hello authoritative NS records in hello child zone tooobtain hello name servers hosting hello child zone in Azure DNS.</span></span>
3. <span data-ttu-id="80fc9-173">Delegálja a gyermekzónát hello hello gyermekzónára toohello gyermekzónát a Névkiszolgálói rekordok konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="80fc9-173">Delegate hello child zone by configuring NS records in hello parent zone pointing toohello child zone.</span></span>

### <a name="create-a-dns-zone"></a><span data-ttu-id="80fc9-174">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="80fc9-174">Create a DNS zone</span></span>

1. <span data-ttu-id="80fc9-175">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="80fc9-175">Sign in toohello Azure portal</span></span>
1. <span data-ttu-id="80fc9-176">Hello központ menüben kattintson, majd **új > Hálózat >** , majd **DNS-zóna** tooopen hello hozzon létre DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-176">On hello Hub menu, click and click **New > Networking >** and then click **DNS zone** tooopen hello Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="80fc9-178">A hello **hozzon létre DNS-zóna** panelen adja meg a következő értékek hello, majd kattintson a **létrehozása**:</span><span class="sxs-lookup"><span data-stu-id="80fc9-178">On hello **Create DNS zone** blade enter hello following values, then click **Create**:</span></span>

   | <span data-ttu-id="80fc9-179">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="80fc9-179">**Setting**</span></span> | <span data-ttu-id="80fc9-180">**Érték**</span><span class="sxs-lookup"><span data-stu-id="80fc9-180">**Value**</span></span> | <span data-ttu-id="80fc9-181">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="80fc9-181">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="80fc9-182">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="80fc9-182">**Name**</span></span>|<span data-ttu-id="80fc9-183">partners.contoso.net</span><span class="sxs-lookup"><span data-stu-id="80fc9-183">partners.contoso.net</span></span>|<span data-ttu-id="80fc9-184">hello hello DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="80fc9-184">hello name of hello DNS zone</span></span>|
   |<span data-ttu-id="80fc9-185">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="80fc9-185">**Subscription**</span></span>|<span data-ttu-id="80fc9-186">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="80fc9-186">[Your subscription]</span></span>|<span data-ttu-id="80fc9-187">Válasszon egy előfizetés toocreate hello Alkalmazásátjáró a.</span><span class="sxs-lookup"><span data-stu-id="80fc9-187">Select a subscription toocreate hello application gateway in.</span></span>|
   |<span data-ttu-id="80fc9-188">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="80fc9-188">**Resource group**</span></span>|<span data-ttu-id="80fc9-189">**Meglévő használata:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="80fc9-189">**Use Existing:** contosoRG</span></span>|<span data-ttu-id="80fc9-190">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80fc9-190">Create a resource group.</span></span> <span data-ttu-id="80fc9-191">az erőforráscsoport neve hello kiválasztott hello előfizetésen belül egyedinek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="80fc9-191">hello resource group name must be unique within hello subscription you selected.</span></span> <span data-ttu-id="80fc9-192">További tudnivalók az erőforráscsoportokról, olvassa el a hello toolearn [erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) a cikk áttekintése.</span><span class="sxs-lookup"><span data-stu-id="80fc9-192">toolearn more about resource groups, read hello [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="80fc9-193">**Hely**</span><span class="sxs-lookup"><span data-stu-id="80fc9-193">**Location**</span></span>|<span data-ttu-id="80fc9-194">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="80fc9-194">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="80fc9-195">hello erőforráscsoport hello erőforráscsoport toohello helyre hivatkozik, és nincs hatással van a hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="80fc9-195">hello resource group refers toohello location of hello resource group, and has no impact on hello DNS zone.</span></span> <span data-ttu-id="80fc9-196">mindig "globális" Hello DNS-zóna helyét, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="80fc9-196">hello DNS zone location is always "global", and is not shown.</span></span>

### <a name="retrieve-name-servers"></a><span data-ttu-id="80fc9-197">Névkiszolgálók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="80fc9-197">Retrieve name servers</span></span>

1. <span data-ttu-id="80fc9-198">A hello DNS-zóna hello Azure-portálon létrehozott **Kedvencek** ablaktáblán kattintson a **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="80fc9-198">With hello DNS zone created, in hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="80fc9-199">Kattintson a hello **partners.contoso.net** hello DNS-zónát **összes erőforrás** panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-199">Click hello **partners.contoso.net** DNS zone in hello **All resources** blade.</span></span> <span data-ttu-id="80fc9-200">Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **partners.contoso.net** a hello szűrő név szerint...</span><span class="sxs-lookup"><span data-stu-id="80fc9-200">If hello subscription you selected already has several resources in it, you can enter **partners.contoso.net** in hello Filter by name…</span></span> <span data-ttu-id="80fc9-201">mezőbe tooeasily hozzáférés hello DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="80fc9-201">box tooeasily access hello DNS zone.</span></span>

1. <span data-ttu-id="80fc9-202">Hello névkiszolgálók lekérése hello DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="80fc9-202">Retrieve hello name servers from hello DNS zone blade.</span></span> <span data-ttu-id="80fc9-203">Ebben a példában a "contoso.net" zónához hello rendelték névkiszolgálókat "ns1-01.azure-dns.com", "ns2-01.azure-DNS.NET", "ns3-01.azure-dns.org", és "ns4-01.azure-dns.info":</span><span class="sxs-lookup"><span data-stu-id="80fc9-203">In this example, hello zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="80fc9-205">Az Azure DNS automatikusan létrehozza a mérvadó Névkiszolgálói rekordokat, amelyek a zónához hozzárendelt névkiszolgálók hello tartalmazó.</span><span class="sxs-lookup"><span data-stu-id="80fc9-205">Azure DNS automatically creates authoritative NS records in your zone containing hello assigned name servers.</span></span>  <span data-ttu-id="80fc9-206">toosee hello névkiszolgáló nevek Azure PowerShell vagy Azure parancssori felületen keresztül, egyszerűen szükséges tooretrieve ezeket a rekordokat.</span><span class="sxs-lookup"><span data-stu-id="80fc9-206">toosee hello name server names via Azure PowerShell or Azure CLI, you simply need tooretrieve these records.</span></span>

### <a name="create-name-server-record-in-parent-zone"></a><span data-ttu-id="80fc9-207">Névkiszolgáló-rekord létrehozása a szülőzónában</span><span class="sxs-lookup"><span data-stu-id="80fc9-207">Create name server record in parent zone</span></span>

1. <span data-ttu-id="80fc9-208">Keresse meg a toohello **contoso.net** hello Azure-portálon DNS-zónát.</span><span class="sxs-lookup"><span data-stu-id="80fc9-208">Navigate toohello **contoso.net** DNS zone in hello Azure portal.</span></span>
1. <span data-ttu-id="80fc9-209">Kattintson a **+ Rekordhalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="80fc9-209">Click **+ Record set**</span></span>
1. <span data-ttu-id="80fc9-210">A hello **adja hozzá a rekordhalmaz** panelen adja meg a következő értékek hello, majd kattintson a **OK**:</span><span class="sxs-lookup"><span data-stu-id="80fc9-210">On hello **Add record set** blade, enter hello following values, then click **OK**:</span></span>

   | <span data-ttu-id="80fc9-211">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="80fc9-211">**Setting**</span></span> | <span data-ttu-id="80fc9-212">**Érték**</span><span class="sxs-lookup"><span data-stu-id="80fc9-212">**Value**</span></span> | <span data-ttu-id="80fc9-213">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="80fc9-213">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="80fc9-214">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="80fc9-214">**Name**</span></span>|<span data-ttu-id="80fc9-215">partners</span><span class="sxs-lookup"><span data-stu-id="80fc9-215">partners</span></span>|<span data-ttu-id="80fc9-216">hello hello gyermek DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="80fc9-216">hello name of hello child DNS zone</span></span>|
   |<span data-ttu-id="80fc9-217">**Típus**</span><span class="sxs-lookup"><span data-stu-id="80fc9-217">**Type**</span></span>|<span data-ttu-id="80fc9-218">NS</span><span class="sxs-lookup"><span data-stu-id="80fc9-218">NS</span></span>|<span data-ttu-id="80fc9-219">A névkiszolgálók esetében használja az NS típust.</span><span class="sxs-lookup"><span data-stu-id="80fc9-219">Use NS for name server records.</span></span>|
   |<span data-ttu-id="80fc9-220">**TTL**</span><span class="sxs-lookup"><span data-stu-id="80fc9-220">**TTL**</span></span>|<span data-ttu-id="80fc9-221">1</span><span class="sxs-lookup"><span data-stu-id="80fc9-221">1</span></span>|<span data-ttu-id="80fc9-222">Az idő toolive.</span><span class="sxs-lookup"><span data-stu-id="80fc9-222">Time toolive.</span></span>|
   |<span data-ttu-id="80fc9-223">**TTL mértékegysége**</span><span class="sxs-lookup"><span data-stu-id="80fc9-223">**TTL unit**</span></span>|<span data-ttu-id="80fc9-224">Óra</span><span class="sxs-lookup"><span data-stu-id="80fc9-224">Hours</span></span>|<span data-ttu-id="80fc9-225">Beállítja az időt toolive egység toohours</span><span class="sxs-lookup"><span data-stu-id="80fc9-225">sets time toolive unit toohours</span></span>|
   |<span data-ttu-id="80fc9-226">**NÉVKISZOLGÁLÓ**</span><span class="sxs-lookup"><span data-stu-id="80fc9-226">**NAME SERVER**</span></span>|<span data-ttu-id="80fc9-227">{névkiszolgálók a partners.contoso.net zónából}</span><span class="sxs-lookup"><span data-stu-id="80fc9-227">{name servers from partners.contoso.net zone}</span></span>|<span data-ttu-id="80fc9-228">Adja meg minden 4 hello névkiszolgálók partners.contoso.net zónából.</span><span class="sxs-lookup"><span data-stu-id="80fc9-228">Enter all 4 of hello name servers from partners.contoso.net zone.</span></span> |

   ![DNS-névkiszolgáló](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a><span data-ttu-id="80fc9-230">Altartományok delegálása az Azure DNS-ben más eszközökkel</span><span class="sxs-lookup"><span data-stu-id="80fc9-230">Delegating sub-domains in Azure DNS with other tools</span></span>

<span data-ttu-id="80fc9-231">hello alábbi példák megadják hello lépéseket toodelegate altartományok a PowerShell és a CLI Azure DNS-ben:</span><span class="sxs-lookup"><span data-stu-id="80fc9-231">hello following examples provide hello steps toodelegate sub-domains in Azure DNS with PowerShell and CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="80fc9-232">PowerShell</span><span class="sxs-lookup"><span data-stu-id="80fc9-232">PowerShell</span></span>

<span data-ttu-id="80fc9-233">hello a következő PowerShell-példa bemutatja ennek működését.</span><span class="sxs-lookup"><span data-stu-id="80fc9-233">hello following PowerShell example demonstrates how this works.</span></span> <span data-ttu-id="80fc9-234">Ugyanezek a lépések hello Azure-portálon keresztül hajtható végre, vagy keresztül hello platformfüggetlen Azure CLI hello.</span><span class="sxs-lookup"><span data-stu-id="80fc9-234">hello same steps can be executed via hello Azure portal, or via hello cross-platform Azure CLI.</span></span>

```powershell
# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve hello authoritative NS records from hello child zone as shown in hello next example. This contains hello name servers assigned toohello child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create hello corresponding NS record set in hello parent zone toocomplete hello delegation. hello record set name in hello parent zone matches hello child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

<span data-ttu-id="80fc9-235">Használjon `nslookup` tooverify, hogy minden helyesen van beállítva hello hello gyermekzóna SOA típusú rekordjának megkeresésével.</span><span class="sxs-lookup"><span data-stu-id="80fc9-235">Use `nslookup` tooverify that everything is set up correctly by looking up hello SOA record of hello child zone.</span></span>

```
nslookup -type=SOA partners.contoso.com
```

```
Server: ns1-08.azure-dns.com
Address: 208.76.47.8

partners.contoso.com
    primary name server = ns1-08.azure-dns.com
    responsible mail addr = msnhst.microsoft.com
    serial = 1
    refresh = 900 (15 mins)
    retry = 300 (5 mins)
    expire = 604800 (7 days)
    default TTL = 300 (5 mins)
```

#### <a name="azure-cli"></a><span data-ttu-id="80fc9-236">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="80fc9-236">Azure CLI</span></span>

```azurecli
#!/bin/bash

# Create hello parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

<span data-ttu-id="80fc9-237">Hello névkiszolgálóit hello beolvasása `partners.contoso.net` zóna hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="80fc9-237">Retrieve hello name servers for hello `partners.contoso.net` zone from hello output.</span></span>

```
{
  "etag": "00000003-0000-0000-418f-250de2b2d201",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/contosorg/providers/Microsoft.Network/dnszones/partners.contoso.net",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "partners.contoso.net",
  "nameServers": [
    "ns1-09.azure-dns.com.",
    "ns2-09.azure-dns.net.",
    "ns3-09.azure-dns.org.",
    "ns4-09.azure-dns.info."
  ],
  "numberOfRecordSets": 2,
  "resourceGroup": "contosorg",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

<span data-ttu-id="80fc9-238">Hozzon létre hello rekordhalmaz és minden névkiszolgáló rekordjait.</span><span class="sxs-lookup"><span data-stu-id="80fc9-238">Create hello record set and NS records for each name server.</span></span>

```azurecli
#!/bin/bash

# Create hello record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a><span data-ttu-id="80fc9-239">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="80fc9-239">Delete all resources</span></span>

<span data-ttu-id="80fc9-240">toodelete összes erőforrás létrehozása ebben a cikkben teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="80fc9-240">toodelete all resources created in this article, complete hello following steps:</span></span>

1. <span data-ttu-id="80fc9-241">Az Azure-portálon hello **Kedvencek** ablaktáblán kattintson a **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="80fc9-241">In hello Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="80fc9-242">Kattintson a hello **contosorg** erőforráscsoport hello az összes erőforrás panel.</span><span class="sxs-lookup"><span data-stu-id="80fc9-242">Click hello **contosorg** resource group in hello All resources blade.</span></span> <span data-ttu-id="80fc9-243">Ha már kiválasztott hello előfizetés több erőforrást tartalmaz, megadhatja **contosorg** a hello **Szűrés név alapján...**</span><span class="sxs-lookup"><span data-stu-id="80fc9-243">If hello subscription you selected already has several resources in it, you can enter **contosorg** in hello **Filter by name…**</span></span> <span data-ttu-id="80fc9-244">mezőbe tooeasily hozzáférés hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="80fc9-244">box tooeasily access hello resource group.</span></span>
1. <span data-ttu-id="80fc9-245">A hello **contosorg** panelen kattintson a hello **törlése** gomb.</span><span class="sxs-lookup"><span data-stu-id="80fc9-245">In hello **contosorg** blade, click hello **Delete** button.</span></span>
1. <span data-ttu-id="80fc9-246">hello portal meg tootype hello nevét, amelyet az toodelete hello erőforrás csoport tooconfirm azt.</span><span class="sxs-lookup"><span data-stu-id="80fc9-246">hello portal requires you tootype hello name of hello resource group tooconfirm that you want toodelete it.</span></span> <span data-ttu-id="80fc9-247">Típus *contosorg* hello erőforráscsoport-nevet, majd kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="80fc9-247">Type *contosorg* for hello resource group name, then click **Delete**.</span></span> <span data-ttu-id="80fc9-248">Erőforráscsoport törlésével törli az összes erőforrás hello erőforráscsoporton belül, ezért mindig meg arról, hogy tooconfirm erőforráscsoport hello tartalmának törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="80fc9-248">Deleting a resource group deletes all resources within hello resource group, so always be sure tooconfirm hello contents of a resource group before deleting it.</span></span> <span data-ttu-id="80fc9-249">hello portal hello erőforráscsoporton belül található összes erőforrást törli, majd törli a hello erőforráscsoport magát.</span><span class="sxs-lookup"><span data-stu-id="80fc9-249">hello portal deletes all resources contained within hello resource group, then deletes hello resource group itself.</span></span> <span data-ttu-id="80fc9-250">Ez a folyamat több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="80fc9-250">This process takes several minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="80fc9-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="80fc9-251">Next steps</span></span>

[<span data-ttu-id="80fc9-252">DNS-zónák kezelése</span><span class="sxs-lookup"><span data-stu-id="80fc9-252">Manage DNS zones</span></span>](dns-operations-dnszones.md)

[<span data-ttu-id="80fc9-253">DNS-rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="80fc9-253">Manage DNS records</span></span>](dns-operations-recordsets.md)
