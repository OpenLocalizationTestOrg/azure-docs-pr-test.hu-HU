---
title: "Tartomány delegálása az Azure DNS-be | Microsoft Docs"
description: "Ismerje meg, hogyan módosíthatja a tartományok delegálását és használhatja tartományszolgáltatóként az Azure DNS-névkiszolgálóit."
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
ms.openlocfilehash: 33b3ec24432ff1268860b9a2e9d5098600a8dedc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-a-domain-to-azure-dns"></a><span data-ttu-id="d14c3-103">Tartomány delegálása az Azure DNS-be</span><span class="sxs-lookup"><span data-stu-id="d14c3-103">Delegate a domain to Azure DNS</span></span>

<span data-ttu-id="d14c3-104">Az Azure DNS használatával DNS-zónákat üzemeltethet, és kezelheti a tartomány DNS-rekordjait az Azure felületén.</span><span class="sxs-lookup"><span data-stu-id="d14c3-104">Azure DNS allows you to host a DNS zone and manage the DNS records for a domain in Azure.</span></span> <span data-ttu-id="d14c3-105">Egy tartomány DNS-lekérdezései csak akkor érik el az Azure DNS-t, ha a tartomány delegálva van az Azure DNS-be a szülőtartományból.</span><span class="sxs-lookup"><span data-stu-id="d14c3-105">In order for DNS queries for a domain to reach Azure DNS, the domain has to be delegated to Azure DNS from the parent domain.</span></span> <span data-ttu-id="d14c3-106">Ne feledje: nem az Azure DNS a tartományregisztráló.</span><span class="sxs-lookup"><span data-stu-id="d14c3-106">Keep in mind Azure DNS is not the domain registrar.</span></span> <span data-ttu-id="d14c3-107">Ez a cikk a tartomány delegálását ismerteti az Azure DNS-be.</span><span class="sxs-lookup"><span data-stu-id="d14c3-107">This article explains how to delegate your domain to Azure DNS.</span></span>

<span data-ttu-id="d14c3-108">A tartományregisztrálótól vásárolt tartományokhoz a regisztráló felajánlja, hogy beállítja ezeket a névkiszolgálói rekordokat.</span><span class="sxs-lookup"><span data-stu-id="d14c3-108">For domains purchased from a registrar, your registrar offers the option to set up these NS records.</span></span> <span data-ttu-id="d14c3-109">Nem kell ahhoz egy tartománnyal rendelkeznie, hogy az adott tartománynévvel létrehozzon egy DNS-zónát az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="d14c3-109">You do not have to own a domain to create a DNS zone with that domain name in Azure DNS.</span></span> <span data-ttu-id="d14c3-110">Azonban a tartományregisztrálóval az Azure DNS-be való delegáláshoz már meg kell vásárolnia a tartományt.</span><span class="sxs-lookup"><span data-stu-id="d14c3-110">However, you do need to own the domain to set up the delegation to Azure DNS with the registrar.</span></span>

<span data-ttu-id="d14c3-111">Tegyük fel például, hogy megvette a „contoso.net” tartományt, és létrehozott egy „contoso.net” nevű zónát az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="d14c3-111">For example, suppose you purchase the domain 'contoso.net' and create a zone with the name 'contoso.net' in Azure DNS.</span></span> <span data-ttu-id="d14c3-112">A tartomány tulajdonosaként a regisztráló felajánlja, hogy konfigurálja a tartomány névkiszolgálóinak címeit (azaz a névkiszolgálói rekordokat).</span><span class="sxs-lookup"><span data-stu-id="d14c3-112">As the owner of the domain, your registrar offers you the option to configure the name server addresses (that is, the NS records) for your domain.</span></span> <span data-ttu-id="d14c3-113">A regisztráló ezeket a névkiszolgálói rekordokat a szülőtartományban, ebben az esetben a „.net” tartományban tárolja.</span><span class="sxs-lookup"><span data-stu-id="d14c3-113">The registrar stores these NS records in the parent domain, in this case '.net'.</span></span> <span data-ttu-id="d14c3-114">A világ különböző pontjain található ügyfelek ekkor az Azure DNS-zónabeli tartományához irányíthatók, amikor megpróbálják feloldani a „contoso.net” DNS-rekordjait.</span><span class="sxs-lookup"><span data-stu-id="d14c3-114">Clients around the world can then be directed to your domain in Azure DNS zone when trying to resolve DNS records in 'contoso.net'.</span></span>

## <a name="create-a-dns-zone"></a><span data-ttu-id="d14c3-115">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="d14c3-115">Create a DNS zone</span></span>

1. <span data-ttu-id="d14c3-116">Jelentkezzen be az Azure Portalra</span><span class="sxs-lookup"><span data-stu-id="d14c3-116">Sign in to the Azure portal</span></span>
1. <span data-ttu-id="d14c3-117">A központi menüben kattintson az **Új > Hálózatkezelés >** elemre, majd kattintson a **DNS-zóna** elemre a DNS-zóna létrehozása panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d14c3-117">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="d14c3-119">A **DNS-zóna létrehozása** panelen adja meg a következő értékeket, majd kattintson a **Létrehozás** elemre:</span><span class="sxs-lookup"><span data-stu-id="d14c3-119">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>

   | <span data-ttu-id="d14c3-120">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="d14c3-120">**Setting**</span></span> | <span data-ttu-id="d14c3-121">**Érték**</span><span class="sxs-lookup"><span data-stu-id="d14c3-121">**Value**</span></span> | <span data-ttu-id="d14c3-122">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="d14c3-122">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="d14c3-123">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="d14c3-123">**Name**</span></span>|<span data-ttu-id="d14c3-124">contoso.net</span><span class="sxs-lookup"><span data-stu-id="d14c3-124">contoso.net</span></span>|<span data-ttu-id="d14c3-125">A DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="d14c3-125">The name of the DNS zone</span></span>|
   |<span data-ttu-id="d14c3-126">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="d14c3-126">**Subscription**</span></span>|<span data-ttu-id="d14c3-127">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="d14c3-127">[Your subscription]</span></span>|<span data-ttu-id="d14c3-128">Válasszon ki egy előfizetést, amelyben létrehozza az alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="d14c3-128">Select a subscription to create the application gateway in.</span></span>|
   |<span data-ttu-id="d14c3-129">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="d14c3-129">**Resource group**</span></span>|<span data-ttu-id="d14c3-130">**Új létrehozása:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="d14c3-130">**Create new:** contosoRG</span></span>|<span data-ttu-id="d14c3-131">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d14c3-131">Create a resource group.</span></span> <span data-ttu-id="d14c3-132">Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="d14c3-132">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="d14c3-133">Az erőforráscsoportokkal kapcsolatos további információkért olvassa el [A Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) áttekintése című cikket.</span><span class="sxs-lookup"><span data-stu-id="d14c3-133">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="d14c3-134">**Hely**</span><span class="sxs-lookup"><span data-stu-id="d14c3-134">**Location**</span></span>|<span data-ttu-id="d14c3-135">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="d14c3-135">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="d14c3-136">Az erőforráscsoport az erőforráscsoport helyére vonatkozik, és nincs hatással a DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="d14c3-136">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="d14c3-137">A DNS-zóna helye mindig „globális”, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d14c3-137">The DNS zone location is always "global", and is not shown.</span></span>

## <a name="retrieve-name-servers"></a><span data-ttu-id="d14c3-138">Névkiszolgálók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="d14c3-138">Retrieve name servers</span></span>

<span data-ttu-id="d14c3-139">Mielőtt DNS-zónáját az Azure DNS-be delegálhatná, meg kell tudnia a zóna névkiszolgálóinak neveit.</span><span class="sxs-lookup"><span data-stu-id="d14c3-139">Before you can delegate your DNS zone to Azure DNS, you first need to know the name server names for your zone.</span></span> <span data-ttu-id="d14c3-140">Minden zóna létrehozásakor az Azure DNS egy névkiszolgálói készletből választ ki egyet.</span><span class="sxs-lookup"><span data-stu-id="d14c3-140">Azure DNS allocates name servers from a pool each time a zone is created.</span></span>

1. <span data-ttu-id="d14c3-141">Ha létrehozta a DNS-zónát, az Azure Portal **Kedvencek** panelén kattintson az **Összes erőforrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d14c3-141">With the DNS zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="d14c3-142">Az **Összes erőforrás** panelen kattintson a **contoso.net** DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="d14c3-142">Click the **contoso.net** DNS zone in the **All resources** blade.</span></span> <span data-ttu-id="d14c3-143">Ha a kiválasztott előfizetésben már több erőforrás szerepel, az alkalmazásátjáró egyszerű eléréséhez beírhatja a **contoso.net** nevet a Szűrés név alapján...</span><span class="sxs-lookup"><span data-stu-id="d14c3-143">If the subscription you selected already has several resources in it, you can enter **contoso.net** in the Filter by name…</span></span> <span data-ttu-id="d14c3-144">mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d14c3-144">box to easily access the application gateway.</span></span> 

1. <span data-ttu-id="d14c3-145">Kérdezze le a névkiszolgálókat a DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="d14c3-145">Retrieve the name servers from the DNS zone blade.</span></span> <span data-ttu-id="d14c3-146">Ebben a példában a „contoso.net” zónához a következő névkiszolgálók tartoznak: „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” és „ns4-01.azure-dns.info”:</span><span class="sxs-lookup"><span data-stu-id="d14c3-146">In this example, the zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="d14c3-148">Az Azure DNS automatikusan létrehozza a zóna mérvadó névkiszolgálói rekordjait, amelyek a zónához rendelt névkiszolgálókat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="d14c3-148">Azure DNS automatically creates authoritative NS records in your zone containing the assigned name servers.</span></span>  <span data-ttu-id="d14c3-149">A névkiszolgálók neveit az Azure PowerShellen vagy az Azure parancssori felületén keresztül is megtekintheti ezeknek a rekordoknak a lekérésével.</span><span class="sxs-lookup"><span data-stu-id="d14c3-149">To see the name server names via Azure PowerShell or Azure CLI, you simply need to retrieve these records.</span></span>

<span data-ttu-id="d14c3-150">Az alábbi példák bemutatják az egyes zónák névkiszolgálói lekérdezésének lépéseit az Azure DNS-ben a PowerShell és az Azure CLI használatával.</span><span class="sxs-lookup"><span data-stu-id="d14c3-150">The following examples also provide the steps to retrieve the name servers for a zone in Azure DNS with PowerShell and Azure CLI.</span></span>

### <a name="powershell"></a><span data-ttu-id="d14c3-151">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d14c3-151">PowerShell</span></span>

```powershell
# The record name "@" is used to refer to records at the top of the zone.
$zone = Get-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
Get-AzureRmDnsRecordSet -Name "@" -RecordType NS -Zone $zone
```

<span data-ttu-id="d14c3-152">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="d14c3-152">The following example is the response.</span></span>

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

### <a name="azure-cli"></a><span data-ttu-id="d14c3-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d14c3-153">Azure CLI</span></span>

```azurecli
az network dns record-set show --resource-group contosoRG --zone-name contoso.net --type NS --name @
```

<span data-ttu-id="d14c3-154">A következő példa a válasz.</span><span class="sxs-lookup"><span data-stu-id="d14c3-154">The following example is the response.</span></span>

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

## <a name="delegate-the-domain"></a><span data-ttu-id="d14c3-155">A tartomány delegálása</span><span class="sxs-lookup"><span data-stu-id="d14c3-155">Delegate the domain</span></span>

<span data-ttu-id="d14c3-156">Most, hogy létrehozta a DNS-zónát, és megvannak a névkiszolgálók is, frissítenie kell a szülőtartományt az Azure DNS-névkiszolgálókkal.</span><span class="sxs-lookup"><span data-stu-id="d14c3-156">Now that the DNS zone is created and you have the name servers, the parent domain needs to be updated with the Azure DNS name servers.</span></span> <span data-ttu-id="d14c3-157">Minden tartományregisztráló a saját DNS-kezelési eszközeit használja a tartományok névkiszolgálói rekordjainak módosítására.</span><span class="sxs-lookup"><span data-stu-id="d14c3-157">Each registrar has their own DNS management tools to change the name server records for a domain.</span></span> <span data-ttu-id="d14c3-158">A regisztráló DNS-kezelési oldalán szerkessze a névkiszolgálói rekordokat, és cserélje le őket az Azure DNS által létrehozottakra.</span><span class="sxs-lookup"><span data-stu-id="d14c3-158">In the registrar's DNS management page, edit the NS records and replace the NS records with the ones Azure DNS created.</span></span>

<span data-ttu-id="d14c3-159">Amikor egy tartományt az Azure DNS-be delegál, az Azure DNS által nyújtott névkiszolgálói neveket kell használnia.</span><span class="sxs-lookup"><span data-stu-id="d14c3-159">When delegating a domain to Azure DNS, you must use the name server names provided by Azure DNS.</span></span> <span data-ttu-id="d14c3-160">Ajánlott használni mind a négy névkiszolgálói nevet, a tartomány nevétől függetlenül.</span><span class="sxs-lookup"><span data-stu-id="d14c3-160">It is recommended to use all four name server names, regardless of the name of your domain.</span></span> <span data-ttu-id="d14c3-161">A tartománydelegáláshoz nem szükséges, hogy a névkiszolgálói név ugyanazt a legfelső szintű tartományt használja, mint az Ön tartománya.</span><span class="sxs-lookup"><span data-stu-id="d14c3-161">Domain delegation does not require the name server name to use the same top-level domain as your domain.</span></span>

<span data-ttu-id="d14c3-162">Ne használjon „összetartó rekordokat” az Azure DNS névkiszolgálói IP-címeire való rámutatáshoz, mert ezek az IP-címek megváltozhatnak a jövőben.</span><span class="sxs-lookup"><span data-stu-id="d14c3-162">You should not use 'glue records' to point to the Azure DNS name server IP addresses, since these IP addresses may change in future.</span></span> <span data-ttu-id="d14c3-163">A saját zónájában történő, névkiszolgálói neveket használó delegálásokat – más néven „személyes névkiszolgálókat” – az Azure DNS jelenleg nem támogatja.</span><span class="sxs-lookup"><span data-stu-id="d14c3-163">Delegations using name server names in your own zone, sometimes called 'vanity name servers', are not currently supported in Azure DNS.</span></span>

## <a name="verify-name-resolution-is-working"></a><span data-ttu-id="d14c3-164">A névfeloldás működésének ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d14c3-164">Verify name resolution is working</span></span>

<span data-ttu-id="d14c3-165">A delegálás befejezése után ellenőrizheti, hogy a névfeloldás működik-e. Ezt például az „nslookup” vagy egy hasonló eszköz segítségével teheti meg, amely lekérdezi a zónája SOA típusú rekordját (amely szintén automatikusan létrejön a zóna létrehozásakor).</span><span class="sxs-lookup"><span data-stu-id="d14c3-165">After completing the delegation, you can verify that name resolution is working by using a tool such as 'nslookup' to query the SOA record for your zone (which is also automatically created when the zone is created).</span></span>

<span data-ttu-id="d14c3-166">Nem kell megadnia az Azure DNS névkiszolgálóit, mivel ha a delegálást helyesen végezte el, a hagyományos DNS-feloldási folyamat automatikusan megtalálja a névkiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="d14c3-166">You do not have to specify the Azure DNS name servers, if the delegation has been set up correctly, the normal DNS resolution process finds the name servers automatically.</span></span>

```
nslookup -type=SOA contoso.com
```

<span data-ttu-id="d14c3-167">Az alábbiakban egy példát láthat az előző parancsra adott válaszra:</span><span class="sxs-lookup"><span data-stu-id="d14c3-167">The following is an example response from the preceding command:</span></span>

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

## <a name="delegate-sub-domains-in-azure-dns"></a><span data-ttu-id="d14c3-168">Altartományok delegálása az Azure DNS-ben</span><span class="sxs-lookup"><span data-stu-id="d14c3-168">Delegate sub-domains in Azure DNS</span></span>

<span data-ttu-id="d14c3-169">Ha különálló gyermekzónát szeretne létrehozni, azt megteheti egy altartomány Azure DNS-beli delegálásával.</span><span class="sxs-lookup"><span data-stu-id="d14c3-169">If you want to set up a separate child zone, you can delegate a sub-domain in Azure DNS.</span></span> <span data-ttu-id="d14c3-170">Tegyük fel például, hogy a „contoso.net” Azure DNS-beli beállítása és delegálása után szeretne egy különálló gyermekzónát is létrehozni, „partners.contoso.net” néven.</span><span class="sxs-lookup"><span data-stu-id="d14c3-170">For example, having set up and delegated 'contoso.net' in Azure DNS, suppose you would like to set up a separate child zone, 'partners.contoso.net'.</span></span>

1. <span data-ttu-id="d14c3-171">Hozza létre a „partners.contoso.net” gyermekzónát az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="d14c3-171">Create the child zone 'partners.contoso.net' in Azure DNS.</span></span>
2. <span data-ttu-id="d14c3-172">Keresse meg a mérvadó névkiszolgálói rekordokat a gyermekzónában, így megtalálja a gyermekzónát az Azure DNS-ben üzemeltető névkiszolgálókat.</span><span class="sxs-lookup"><span data-stu-id="d14c3-172">Look up the authoritative NS records in the child zone to obtain the name servers hosting the child zone in Azure DNS.</span></span>
3. <span data-ttu-id="d14c3-173">A gyermekzónára mutató szülőzónában delegálja a gyermekzónát a névkiszolgálói rekordok konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="d14c3-173">Delegate the child zone by configuring NS records in the parent zone pointing to the child zone.</span></span>

### <a name="create-a-dns-zone"></a><span data-ttu-id="d14c3-174">DNS-zóna létrehozása</span><span class="sxs-lookup"><span data-stu-id="d14c3-174">Create a DNS zone</span></span>

1. <span data-ttu-id="d14c3-175">Jelentkezzen be az Azure Portalra</span><span class="sxs-lookup"><span data-stu-id="d14c3-175">Sign in to the Azure portal</span></span>
1. <span data-ttu-id="d14c3-176">A központi menüben kattintson az **Új > Hálózatkezelés >** elemre, majd kattintson a **DNS-zóna** elemre a DNS-zóna létrehozása panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d14c3-176">On the Hub menu, click and click **New > Networking >** and then click **DNS zone** to open the Create DNS zone blade.</span></span>

    ![DNS-zóna](./media/dns-domain-delegation/dns.png)

1. <span data-ttu-id="d14c3-178">A **DNS-zóna létrehozása** panelen adja meg a következő értékeket, majd kattintson a **Létrehozás** elemre:</span><span class="sxs-lookup"><span data-stu-id="d14c3-178">On the **Create DNS zone** blade enter the following values, then click **Create**:</span></span>

   | <span data-ttu-id="d14c3-179">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="d14c3-179">**Setting**</span></span> | <span data-ttu-id="d14c3-180">**Érték**</span><span class="sxs-lookup"><span data-stu-id="d14c3-180">**Value**</span></span> | <span data-ttu-id="d14c3-181">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="d14c3-181">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="d14c3-182">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="d14c3-182">**Name**</span></span>|<span data-ttu-id="d14c3-183">partners.contoso.net</span><span class="sxs-lookup"><span data-stu-id="d14c3-183">partners.contoso.net</span></span>|<span data-ttu-id="d14c3-184">A DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="d14c3-184">The name of the DNS zone</span></span>|
   |<span data-ttu-id="d14c3-185">**Előfizetés**</span><span class="sxs-lookup"><span data-stu-id="d14c3-185">**Subscription**</span></span>|<span data-ttu-id="d14c3-186">[Az Ön előfizetése]</span><span class="sxs-lookup"><span data-stu-id="d14c3-186">[Your subscription]</span></span>|<span data-ttu-id="d14c3-187">Válasszon ki egy előfizetést, amelyben létrehozza az alkalmazásátjárót.</span><span class="sxs-lookup"><span data-stu-id="d14c3-187">Select a subscription to create the application gateway in.</span></span>|
   |<span data-ttu-id="d14c3-188">**Erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="d14c3-188">**Resource group**</span></span>|<span data-ttu-id="d14c3-189">**Meglévő használata:** contosoRG</span><span class="sxs-lookup"><span data-stu-id="d14c3-189">**Use Existing:** contosoRG</span></span>|<span data-ttu-id="d14c3-190">Hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="d14c3-190">Create a resource group.</span></span> <span data-ttu-id="d14c3-191">Az erőforráscsoport nevének egyedinek kell lennie a kiválasztott előfizetésen belül.</span><span class="sxs-lookup"><span data-stu-id="d14c3-191">The resource group name must be unique within the subscription you selected.</span></span> <span data-ttu-id="d14c3-192">Az erőforráscsoportokkal kapcsolatos további információkért olvassa el [A Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) áttekintése című cikket.</span><span class="sxs-lookup"><span data-stu-id="d14c3-192">To learn more about resource groups, read the [Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fdns%2ftoc.json#resource-groups) overview article.</span></span>|
   |<span data-ttu-id="d14c3-193">**Hely**</span><span class="sxs-lookup"><span data-stu-id="d14c3-193">**Location**</span></span>|<span data-ttu-id="d14c3-194">USA nyugati régiója</span><span class="sxs-lookup"><span data-stu-id="d14c3-194">West US</span></span>||

> [!NOTE]
> <span data-ttu-id="d14c3-195">Az erőforráscsoport az erőforráscsoport helyére vonatkozik, és nincs hatással a DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="d14c3-195">The resource group refers to the location of the resource group, and has no impact on the DNS zone.</span></span> <span data-ttu-id="d14c3-196">A DNS-zóna helye mindig „globális”, és nem jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d14c3-196">The DNS zone location is always "global", and is not shown.</span></span>

### <a name="retrieve-name-servers"></a><span data-ttu-id="d14c3-197">Névkiszolgálók lekérdezése</span><span class="sxs-lookup"><span data-stu-id="d14c3-197">Retrieve name servers</span></span>

1. <span data-ttu-id="d14c3-198">Ha létrehozta a DNS-zónát, az Azure Portal **Kedvencek** panelén kattintson az **Összes erőforrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d14c3-198">With the DNS zone created, in the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="d14c3-199">Az **Összes erőforrás** panelen kattintson a **partners.contoso.net** DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="d14c3-199">Click the **partners.contoso.net** DNS zone in the **All resources** blade.</span></span> <span data-ttu-id="d14c3-200">Ha a kiválasztott előfizetésben már több erőforrás szerepel, a DNS-zóna egyszerű eléréséhez beírhatja a **partners.contoso.net** nevet a Szűrés név alapján...</span><span class="sxs-lookup"><span data-stu-id="d14c3-200">If the subscription you selected already has several resources in it, you can enter **partners.contoso.net** in the Filter by name…</span></span> <span data-ttu-id="d14c3-201">mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d14c3-201">box to easily access the DNS zone.</span></span>

1. <span data-ttu-id="d14c3-202">Kérdezze le a névkiszolgálókat a DNS-zóna panelen.</span><span class="sxs-lookup"><span data-stu-id="d14c3-202">Retrieve the name servers from the DNS zone blade.</span></span> <span data-ttu-id="d14c3-203">Ebben a példában a „contoso.net” zónához a következő névkiszolgálók tartoznak: „ns1-01.azure-dns.com”, „ns2-01.azure-dns.net”, „ns3-01.azure-dns.org” és „ns4-01.azure-dns.info”:</span><span class="sxs-lookup"><span data-stu-id="d14c3-203">In this example, the zone 'contoso.net' has been assigned name servers 'ns1-01.azure-dns.com', 'ns2-01.azure-dns.net', 'ns3-01.azure-dns.org', and 'ns4-01.azure-dns.info':</span></span>

 ![DNS-névkiszolgáló](./media/dns-domain-delegation/viewzonens500.png)

<span data-ttu-id="d14c3-205">Az Azure DNS automatikusan létrehozza a zóna mérvadó névkiszolgálói rekordjait, amelyek a zónához rendelt névkiszolgálókat tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="d14c3-205">Azure DNS automatically creates authoritative NS records in your zone containing the assigned name servers.</span></span>  <span data-ttu-id="d14c3-206">A névkiszolgálók neveit az Azure PowerShellen vagy az Azure parancssori felületén keresztül is megtekintheti ezeknek a rekordoknak a lekérésével.</span><span class="sxs-lookup"><span data-stu-id="d14c3-206">To see the name server names via Azure PowerShell or Azure CLI, you simply need to retrieve these records.</span></span>

### <a name="create-name-server-record-in-parent-zone"></a><span data-ttu-id="d14c3-207">Névkiszolgáló-rekord létrehozása a szülőzónában</span><span class="sxs-lookup"><span data-stu-id="d14c3-207">Create name server record in parent zone</span></span>

1. <span data-ttu-id="d14c3-208">Az Azure Portalon lépjen a **contoso.net** DNS-zónára.</span><span class="sxs-lookup"><span data-stu-id="d14c3-208">Navigate to the **contoso.net** DNS zone in the Azure portal.</span></span>
1. <span data-ttu-id="d14c3-209">Kattintson a **+ Rekordhalmaz** gombra.</span><span class="sxs-lookup"><span data-stu-id="d14c3-209">Click **+ Record set**</span></span>
1. <span data-ttu-id="d14c3-210">A **Rekordhalmaz hozzáadása** panelen adja meg az alábbi értékeket, és kattintson az **OK** gombra:</span><span class="sxs-lookup"><span data-stu-id="d14c3-210">On the **Add record set** blade, enter the following values, then click **OK**:</span></span>

   | <span data-ttu-id="d14c3-211">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="d14c3-211">**Setting**</span></span> | <span data-ttu-id="d14c3-212">**Érték**</span><span class="sxs-lookup"><span data-stu-id="d14c3-212">**Value**</span></span> | <span data-ttu-id="d14c3-213">**Részletek**</span><span class="sxs-lookup"><span data-stu-id="d14c3-213">**Details**</span></span> |
   |---|---|---|
   |<span data-ttu-id="d14c3-214">**Name (Név)**</span><span class="sxs-lookup"><span data-stu-id="d14c3-214">**Name**</span></span>|<span data-ttu-id="d14c3-215">partners</span><span class="sxs-lookup"><span data-stu-id="d14c3-215">partners</span></span>|<span data-ttu-id="d14c3-216">A gyermek DNS-zóna neve</span><span class="sxs-lookup"><span data-stu-id="d14c3-216">The name of the child DNS zone</span></span>|
   |<span data-ttu-id="d14c3-217">**Típus**</span><span class="sxs-lookup"><span data-stu-id="d14c3-217">**Type**</span></span>|<span data-ttu-id="d14c3-218">NS</span><span class="sxs-lookup"><span data-stu-id="d14c3-218">NS</span></span>|<span data-ttu-id="d14c3-219">A névkiszolgálók esetében használja az NS típust.</span><span class="sxs-lookup"><span data-stu-id="d14c3-219">Use NS for name server records.</span></span>|
   |<span data-ttu-id="d14c3-220">**TTL**</span><span class="sxs-lookup"><span data-stu-id="d14c3-220">**TTL**</span></span>|<span data-ttu-id="d14c3-221">1</span><span class="sxs-lookup"><span data-stu-id="d14c3-221">1</span></span>|<span data-ttu-id="d14c3-222">Élettartam.</span><span class="sxs-lookup"><span data-stu-id="d14c3-222">Time to live.</span></span>|
   |<span data-ttu-id="d14c3-223">**TTL mértékegysége**</span><span class="sxs-lookup"><span data-stu-id="d14c3-223">**TTL unit**</span></span>|<span data-ttu-id="d14c3-224">Óra</span><span class="sxs-lookup"><span data-stu-id="d14c3-224">Hours</span></span>|<span data-ttu-id="d14c3-225">Az élettartam mértékegységét órára állítja.</span><span class="sxs-lookup"><span data-stu-id="d14c3-225">sets time to live unit to hours</span></span>|
   |<span data-ttu-id="d14c3-226">**NÉVKISZOLGÁLÓ**</span><span class="sxs-lookup"><span data-stu-id="d14c3-226">**NAME SERVER**</span></span>|<span data-ttu-id="d14c3-227">{névkiszolgálók a partners.contoso.net zónából}</span><span class="sxs-lookup"><span data-stu-id="d14c3-227">{name servers from partners.contoso.net zone}</span></span>|<span data-ttu-id="d14c3-228">Adja meg mind a négy névkiszolgálót a partners.contoso.net zónából.</span><span class="sxs-lookup"><span data-stu-id="d14c3-228">Enter all 4 of the name servers from partners.contoso.net zone.</span></span> |

   ![DNS-névkiszolgáló](./media/dns-domain-delegation/partnerzone.png)


### <a name="delegating-sub-domains-in-azure-dns-with-other-tools"></a><span data-ttu-id="d14c3-230">Altartományok delegálása az Azure DNS-ben más eszközökkel</span><span class="sxs-lookup"><span data-stu-id="d14c3-230">Delegating sub-domains in Azure DNS with other tools</span></span>

<span data-ttu-id="d14c3-231">Az alábbi példák az altartományok delegálásának lépéseit mutatják be az Azure DNS-ben a PowerShell és a CLI használatával:</span><span class="sxs-lookup"><span data-stu-id="d14c3-231">The following examples provide the steps to delegate sub-domains in Azure DNS with PowerShell and CLI:</span></span>

#### <a name="powershell"></a><span data-ttu-id="d14c3-232">PowerShell</span><span class="sxs-lookup"><span data-stu-id="d14c3-232">PowerShell</span></span>

<span data-ttu-id="d14c3-233">Az alábbi PowerShell-példa bemutatja ennek működését.</span><span class="sxs-lookup"><span data-stu-id="d14c3-233">The following PowerShell example demonstrates how this works.</span></span> <span data-ttu-id="d14c3-234">Ugyanezek a lépések az Azure-portálon vagy az Azure platformfüggetlen parancssori felületén keresztül is végrehajthatók.</span><span class="sxs-lookup"><span data-stu-id="d14c3-234">The same steps can be executed via the Azure portal, or via the cross-platform Azure CLI.</span></span>

```powershell
# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
$parent = New-AzureRmDnsZone -Name contoso.net -ResourceGroupName contosoRG
$child = New-AzureRmDnsZone -Name partners.contoso.net -ResourceGroupName contosoRG

# Retrieve the authoritative NS records from the child zone as shown in the next example. This contains the name servers assigned to the child zone.
$child_ns_recordset = Get-AzureRmDnsRecordSet -Zone $child -Name "@" -RecordType NS

# Create the corresponding NS record set in the parent zone to complete the delegation. The record set name in the parent zone matches the child zone name, in this case "partners".
$parent_ns_recordset = New-AzureRmDnsRecordSet -Zone $parent -Name "partners" -RecordType NS -Ttl 3600
$parent_ns_recordset.Records = $child_ns_recordset.Records
Set-AzureRmDnsRecordSet -RecordSet $parent_ns_recordset
```

<span data-ttu-id="d14c3-235">Az `nslookup` használatával a gyermekzóna SOA típusú rekordjának megkeresésével ellenőrizze, hogy minden helyesen van-e beállítva.</span><span class="sxs-lookup"><span data-stu-id="d14c3-235">Use `nslookup` to verify that everything is set up correctly by looking up the SOA record of the child zone.</span></span>

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

#### <a name="azure-cli"></a><span data-ttu-id="d14c3-236">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="d14c3-236">Azure CLI</span></span>

```azurecli
#!/bin/bash

# Create the parent and child zones. These can be in same resource group or different resource groups as Azure DNS is a global service.
az network dns zone create -g contosoRG -n contoso.net
az network dns zone create -g contosoRG -n partners.contoso.net
```

<span data-ttu-id="d14c3-237">Kérdezze le a `partners.contoso.net` zóna névkiszolgálóit a kimenetből.</span><span class="sxs-lookup"><span data-stu-id="d14c3-237">Retrieve the name servers for the `partners.contoso.net` zone from the output.</span></span>

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

<span data-ttu-id="d14c3-238">Hozza létre a rekordkészletet és a névkiszolgálói rekordokat mindegyik névkiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="d14c3-238">Create the record set and NS records for each name server.</span></span>

```azurecli
#!/bin/bash

# Create the record set
az network dns record-set ns create --resource-group contosorg --zone-name contoso.net --name partners

# Create a ns record for each name server.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns1-09.azure-dns.com.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns2-09.azure-dns.net.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns3-09.azure-dns.org.
az network dns record-set ns add-record --resource-group contosorg --zone-name contoso.net --record-set-name partners --nsdname ns4-09.azure-dns.info.
```

## <a name="delete-all-resources"></a><span data-ttu-id="d14c3-239">Az összes erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="d14c3-239">Delete all resources</span></span>

<span data-ttu-id="d14c3-240">A jelen cikkben létrehozott összes erőforrás törléséhez hajtsa végre az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="d14c3-240">To delete all resources created in this article, complete the following steps:</span></span>

1. <span data-ttu-id="d14c3-241">Az Azure Portal **Kedvencek** panelén kattintson az **Összes erőforrás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d14c3-241">In the Azure portal **Favorites** pane, click **All resources**.</span></span> <span data-ttu-id="d14c3-242">Az Összes erőforrás panelen kattintson a **contosorg** erőforráscsoportra.</span><span class="sxs-lookup"><span data-stu-id="d14c3-242">Click the **contosorg** resource group in the All resources blade.</span></span> <span data-ttu-id="d14c3-243">Ha a kiválasztott előfizetésben már több erőforrás szerepel, az erőforráscsoport egyszerű eléréséhez beírhatja a **contosorg** nevet a **Szűrés név alapján...**</span><span class="sxs-lookup"><span data-stu-id="d14c3-243">If the subscription you selected already has several resources in it, you can enter **contosorg** in the **Filter by name…**</span></span> <span data-ttu-id="d14c3-244">mezőbe.</span><span class="sxs-lookup"><span data-stu-id="d14c3-244">box to easily access the resource group.</span></span>
1. <span data-ttu-id="d14c3-245">A **contosorg** panelen kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d14c3-245">In the **contosorg** blade, click the **Delete** button.</span></span>
1. <span data-ttu-id="d14c3-246">A portál megköveteli, hogy az erőforráscsoport törlésének megerősítéséhez beírja annak nevét.</span><span class="sxs-lookup"><span data-stu-id="d14c3-246">The portal requires you to type the name of the resource group to confirm that you want to delete it.</span></span> <span data-ttu-id="d14c3-247">Írja be a *contosorg* nevet az erőforráscsoport nevéhez, majd kattintson a **Törlés** gombra.</span><span class="sxs-lookup"><span data-stu-id="d14c3-247">Type *contosorg* for the resource group name, then click **Delete**.</span></span> <span data-ttu-id="d14c3-248">Az erőforráscsoport törlésével az abban foglalt összes erőforrás törölve lesz, ezért mindenképp ellenőrizze az erőforráscsoportok tartalmát azok törlése előtt.</span><span class="sxs-lookup"><span data-stu-id="d14c3-248">Deleting a resource group deletes all resources within the resource group, so always be sure to confirm the contents of a resource group before deleting it.</span></span> <span data-ttu-id="d14c3-249">A portál törli az erőforráscsoportban lévő összes erőforrást, majd magát az erőforráscsoportot is.</span><span class="sxs-lookup"><span data-stu-id="d14c3-249">The portal deletes all resources contained within the resource group, then deletes the resource group itself.</span></span> <span data-ttu-id="d14c3-250">Ez a folyamat több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="d14c3-250">This process takes several minutes.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d14c3-251">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d14c3-251">Next steps</span></span>

[<span data-ttu-id="d14c3-252">DNS-zónák kezelése</span><span class="sxs-lookup"><span data-stu-id="d14c3-252">Manage DNS zones</span></span>](dns-operations-dnszones.md)

[<span data-ttu-id="d14c3-253">DNS-rekordok kezelése</span><span class="sxs-lookup"><span data-stu-id="d14c3-253">Manage DNS records</span></span>](dns-operations-recordsets.md)
