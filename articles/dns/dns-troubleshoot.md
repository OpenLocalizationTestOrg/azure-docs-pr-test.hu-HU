---
title: "Az Azure DNS-hibaelhárítási útmutatója |} Microsoft Docs"
description: "Az Azure DNS kapcsolatos gyakori hibák elhárítása"
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
editor: 
ms.assetid: 95b01dc3-ee69-4575-a259-4227131e4f9c
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/20/2017
ms.author: jonatul
ms.openlocfilehash: 1d9bb681a864bdc3e5a2f9c9a531d9566b16ada4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="86ee2-103">Az Azure DNS-hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="86ee2-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="86ee2-104">Ezen a lapon hibaelhárításával kapcsolatos kérdésekre Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="86ee2-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="86ee2-105">Ha ezeket a lépéseket nem sikerül megoldani a problémát, keresse meg vagy is a probléma írjon a [közösségi támogatási fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="86ee2-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="86ee2-106">Azt is megteheti nyissa meg az Azure támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="86ee2-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="86ee2-107">Nem lehet létrehozni egy DNS-zóna</span><span class="sxs-lookup"><span data-stu-id="86ee2-107">I can't create a DNS zone</span></span>

<span data-ttu-id="86ee2-108">A leggyakoribb hibák elhárításához próbálja ki az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="86ee2-108">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="86ee2-109">Tekintse át az Azure DNS naplókat a probléma okának megállapításához.</span><span class="sxs-lookup"><span data-stu-id="86ee2-109">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="86ee2-110">Minden DNS-zónanévnek egyedinek kell lennie a saját erőforráscsoportján belül.</span><span class="sxs-lookup"><span data-stu-id="86ee2-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="86ee2-111">Ugyanazzal a névvel rendelkező két DNS-zóna nem használhatja közösen az erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="86ee2-111">That is, two DNS zones with the same name cannot share a resource group.</span></span> <span data-ttu-id="86ee2-112">Próbáljon meg eltérő zónanevet vagy erőforráscsoportot használni.</span><span class="sxs-lookup"><span data-stu-id="86ee2-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="86ee2-113">Előfordulhat, hogy megjelenik az „Elérte vagy túllépte a zónák maximális számát az előfizetésben ({előfizetés azonosítója})” hibaüzenet.</span><span class="sxs-lookup"><span data-stu-id="86ee2-113">You may see an error "You have reached or exceeded the maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="86ee2-114">Használjon másik Azure-előfizetést, töröljön néhány zónát, vagy vegye fel a kapcsolatot az Azure ügyfélszolgálatával az előfizetésre vonatkozó korlát megemeléséhez.</span><span class="sxs-lookup"><span data-stu-id="86ee2-114">Either use a different Azure subscription, delete some zones, or contact Azure Support to raise your subscription limit.</span></span>
4.  <span data-ttu-id="86ee2-115">„A(z) {zónanév} zóna nem érhető el” hibaüzenet is megjelenhet.</span><span class="sxs-lookup"><span data-stu-id="86ee2-115">You may see an error "The zone '{zone name}' is not available."</span></span> <span data-ttu-id="86ee2-116">Ez a hiba azt jelzi, hogy az Azure DNS nem tudott névkiszolgálót lefoglalni ehhez a DNS-zónához.</span><span class="sxs-lookup"><span data-stu-id="86ee2-116">This error means that Azure DNS was unable to allocate name servers for this DNS zone.</span></span> <span data-ttu-id="86ee2-117">Próbáljon meg egy másik zónanevet használni.</span><span class="sxs-lookup"><span data-stu-id="86ee2-117">Try using a different zone name.</span></span> <span data-ttu-id="86ee2-118">Ha Ön a tartománynév tulajdonosa, felveheti a kapcsolatot az Azure ügyfélszolgálatával, ahol le tudnak foglalni egy névkiszolgálót az Ön számára.</span><span class="sxs-lookup"><span data-stu-id="86ee2-118">Alternatively, if you are the domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="86ee2-119">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="86ee2-119">**Recommended documents**</span></span>

<span data-ttu-id="86ee2-120">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="86ee2-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="86ee2-121">
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="86ee2-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="86ee2-122">Nem tudok létrehozni egy DNS-rekordot</span><span class="sxs-lookup"><span data-stu-id="86ee2-122">I can't create a DNS record</span></span>

<span data-ttu-id="86ee2-123">A leggyakoribb hibák elhárításához próbálja ki az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="86ee2-123">To resolve common issues, try one or more of the following steps:</span></span>

1.  <span data-ttu-id="86ee2-124">Tekintse át az Azure DNS naplókat a probléma okának megállapításához.</span><span class="sxs-lookup"><span data-stu-id="86ee2-124">Review the Azure DNS audit logs to determine the failure reason.</span></span>
2.  <span data-ttu-id="86ee2-125">A rekordhalmaz már létezik?</span><span class="sxs-lookup"><span data-stu-id="86ee2-125">Does the record set exist already?</span></span>  <span data-ttu-id="86ee2-126">Az Azure DNS a rekordokat *rekordhalmazok* használatával kezeli, amelyek ugyanazzal a névvel és típussal rendelkező rekordok gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="86ee2-126">Azure DNS manages records using record *sets*, which are the collection of records of the same name and the same type.</span></span> <span data-ttu-id="86ee2-127">Ha egy ugyanolyan nevű és típusú rekord már létezik, akkor egy másik ugyanilyen rekord hozzáadásához szerkesztenie kell a meglévő rekordhalmazt.</span><span class="sxs-lookup"><span data-stu-id="86ee2-127">If a record with the same name and type already exists, then to add another such record you should edit the existing record set.</span></span>
3.  <span data-ttu-id="86ee2-128">A DNS-zóna legfelső pontján (a zóna „gyökerén”) próbál rekordot létrehozni?</span><span class="sxs-lookup"><span data-stu-id="86ee2-128">Are you trying to create a record at the DNS zone apex (the ‘root’ of the zone)?</span></span> <span data-ttu-id="86ee2-129">Ha igen, az általános egyezmény a DNS rendszerben a „@” karakter használata a rekord neveként.</span><span class="sxs-lookup"><span data-stu-id="86ee2-129">If so, the DNS convention is to use the ‘@’ character as the record name.</span></span> <span data-ttu-id="86ee2-130">Vegye figyelembe, hogy a DNS-szabványok nem engedélyeznek CNAME-rekordokat a zóna legfelső pontján.</span><span class="sxs-lookup"><span data-stu-id="86ee2-130">Also note that the DNS standards do not permit CNAME records at the zone apex.</span></span>
4.  <span data-ttu-id="86ee2-131">CNAME-ütközést tapasztal?</span><span class="sxs-lookup"><span data-stu-id="86ee2-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="86ee2-132">A DNS-szabványok nem engedélyeznek bármilyen más típusú rekorddal megegyező nevű CNAME-rekordokat.</span><span class="sxs-lookup"><span data-stu-id="86ee2-132">The DNS standards do not allow a CNAME record with the same name as a record of any other type.</span></span> <span data-ttu-id="86ee2-133">Ha rendelkezik egy meglévő CNAME-mel, nem tud egy másik típusú, de ugyanolyan nevű rekordot létrehozni.</span><span class="sxs-lookup"><span data-stu-id="86ee2-133">If you have an existing CNAME, creating a record with the same name of a different type fails.</span></span>  <span data-ttu-id="86ee2-134">A CNAME létrehozása szintén sikertelen lesz, ha a neve megegyezik egy más típusú, már létező rekord nevével.</span><span class="sxs-lookup"><span data-stu-id="86ee2-134">Likewise, creating a CNAME fails if the name matches an existing record of a different type.</span></span> <span data-ttu-id="86ee2-135">Szüntesse meg az ütközést a másik rekord eltávolításával vagy egy eltérő rekordnév beállításával.</span><span class="sxs-lookup"><span data-stu-id="86ee2-135">Remove the conflict by removing the other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="86ee2-136">Elérte a DNS-zónában engedélyezett rekordhalmazok számának korlátját?</span><span class="sxs-lookup"><span data-stu-id="86ee2-136">Have you reached the limit on the number of record sets permitted in a DNS zone?</span></span> <span data-ttu-id="86ee2-137">A rekordhalmazok aktuális száma és legfelső korlátja az Azure Portalon látható, a zóna „Tulajdonságainál”.</span><span class="sxs-lookup"><span data-stu-id="86ee2-137">The current number of record sets and the maximum number of record sets are shown in the Azure portal, under the 'Properties' for the zone.</span></span> <span data-ttu-id="86ee2-138">Ha elérte ezt a korlátot, töröljön néhány rekordhalmazt, vagy vegye fel a kapcsolatot az Azure ügyfélszolgálatával a zónához tartozó korlát emeléséhez, majd próbálkozzon újra.</span><span class="sxs-lookup"><span data-stu-id="86ee2-138">If you have reached this limit, then either delete some record sets or contact Azure Support to raise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="86ee2-139">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="86ee2-139">**Recommended documents**</span></span>

<span data-ttu-id="86ee2-140">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="86ee2-140">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="86ee2-141">
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="86ee2-141">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="86ee2-142">Nem tudom feloldani a DNS-rekordomat</span><span class="sxs-lookup"><span data-stu-id="86ee2-142">I can't resolve my DNS record</span></span>

<span data-ttu-id="86ee2-143">A DNS-névfeloldás egy többlépéses folyamat, amely több okból is meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="86ee2-143">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="86ee2-144">Az alábbi lépések segítenek megvizsgálni, hogy a DNS-feloldás miért sikertelen egy DNS-rekordnál az Azure DNS-ben üzemeltetett zónában.</span><span class="sxs-lookup"><span data-stu-id="86ee2-144">The following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="86ee2-145">Győződjön meg róla, hogy a DNS-rekordok helyesen lettek beállítva az Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="86ee2-145">Confirm that the DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="86ee2-146">Tekintse át a DNS-rekordokat az Azure Portalon, és győződjön meg róla, hogy a zónanév, a rekordnév és a rekordtípus helyes.</span><span class="sxs-lookup"><span data-stu-id="86ee2-146">Review the DNS records in the Azure portal, checking that the zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="86ee2-147">Győződjön meg róla, hogy a DNS-rekordok feloldhatók az Azure DNS névkiszolgálóin.</span><span class="sxs-lookup"><span data-stu-id="86ee2-147">Confirm that the DNS records resolve correctly on the Azure DNS name servers.</span></span>
    - <span data-ttu-id="86ee2-148">Ha DNS-lekérdezéseket hajt végre a helyi számítógépről, gyorsítótárazott eredményeket láthat, amelyek nem tükrözik a névkiszolgálók aktuális állapotát.</span><span class="sxs-lookup"><span data-stu-id="86ee2-148">If you make DNS queries from your local PC, you may see cached results that don’t reflect the current state of the name servers.</span></span>  <span data-ttu-id="86ee2-149">Továbbá a vállalati hálózatok gyakran olyan DNS-proxykiszolgálókat használnak, amelyek megakadályozzák a DNS-lekérdezések adott névkiszolgálókhoz történő továbbítását.</span><span class="sxs-lookup"><span data-stu-id="86ee2-149">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed to specific name servers.</span></span>  <span data-ttu-id="86ee2-150">Ha el szeretné kerülni ezeket a problémákat, használjon webes névfeloldási szolgáltatást (ilyen például a [digwebinterface](http://digwebinterface.com)).</span><span class="sxs-lookup"><span data-stu-id="86ee2-150">To avoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="86ee2-151">Mindenképpen adja meg a DNS-zónához tartozó helyes névkiszolgálót, ahogy az Azure Portalon látható.</span><span class="sxs-lookup"><span data-stu-id="86ee2-151">Be sure to specify the correct name servers for your DNS zone, as shown in the Azure portal.</span></span>
    - <span data-ttu-id="86ee2-152">Ellenőrizze a DNS-név (meg kell adnia a teljes nevet, a zónanevet is beleértve) és a rekordtípus helyességét</span><span class="sxs-lookup"><span data-stu-id="86ee2-152">Check that the DNS name is correct (you have to specify the fully qualified name, including the zone name) and the record type is correct</span></span>
3.  <span data-ttu-id="86ee2-153">Győződjön meg róla, hogy a DNS-tartománynév helyesen lett [delegálva az Azure DNS-névkiszolgálókra](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="86ee2-153">Confirm that the DNS domain name has been correctly [delegated to the Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="86ee2-154">[Számos, harmadik féltől származó webhely létezik, amely lehetővé teszi a DNS-delegálás ellenőrzését](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="86ee2-154">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="86ee2-155">Ez egy *zónadelegálási* teszt, így csak a DNS-zónanevét, és nem a teljes rekordnevet kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="86ee2-155">This test is a *zone* delegation test, so you should only enter the DNS zone name and not the fully qualified record name.</span></span>
4.  <span data-ttu-id="86ee2-156">A fentiek elvégzése után meg kell történnie a DNS-rekord feloldásának.</span><span class="sxs-lookup"><span data-stu-id="86ee2-156">Having completed the above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="86ee2-157">Az ellenőrzéshez ismét használható a [digwebinterface](http://digwebinterface.com), ezúttal az alapértelmezett névkiszolgáló-beállításokkal.</span><span class="sxs-lookup"><span data-stu-id="86ee2-157">To verify, you can again use [digwebinterface](http://digwebinterface.com), this time using the default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="86ee2-158">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="86ee2-158">**Recommended documents**</span></span>

[<span data-ttu-id="86ee2-159">Tartomány delegálása az Azure DNS-be</span><span class="sxs-lookup"><span data-stu-id="86ee2-159">Delegate a domain to Azure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-the-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="86ee2-160">Hogyan adható meg a „service” és a „protocol” egy SRV-rekordnál?</span><span class="sxs-lookup"><span data-stu-id="86ee2-160">How do I specify the ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="86ee2-161">Az Azure DNS DNS-rekordokat és -rekordhalmazokat (amely az ugyanazzal a névvel és típussal rendelkező rekordok gyűjteménye) kezel.</span><span class="sxs-lookup"><span data-stu-id="86ee2-161">Azure DNS manages DNS records as record sets—the collection of records with the same name and the same type.</span></span> <span data-ttu-id="86ee2-162">Egy SRV-rekordhalmaznál a „service” és a „protocol” a rekordhalmaz nevének részeként adható meg.</span><span class="sxs-lookup"><span data-stu-id="86ee2-162">For an SRV record set, the 'service' and 'protocol' need to be specified as part of the record set name.</span></span> <span data-ttu-id="86ee2-163">Az egyéb SRV-paraméterek („priority”, „weight”, „port” és „target”) külön vannak megadva, a rekordhalmaz egyes rekordjainál.</span><span class="sxs-lookup"><span data-stu-id="86ee2-163">The other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in the record set.</span></span>

<span data-ttu-id="86ee2-164">Példák az SRV-rekordnevekre („sip” szolgáltatásnév, „tcp” protokoll):</span><span class="sxs-lookup"><span data-stu-id="86ee2-164">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="86ee2-165">\_sip.\_tcp (egy rekordhalmazt hoz létre a zóna legfelső pontján)</span><span class="sxs-lookup"><span data-stu-id="86ee2-165">\_sip.\_tcp (creates a record set at the zone apex)</span></span>
- <span data-ttu-id="86ee2-166">\_sip.\_tcp.sipservice (egy „sipservice” nevű rekordhalmazt hoz létre)</span><span class="sxs-lookup"><span data-stu-id="86ee2-166">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="86ee2-167">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="86ee2-167">**Recommended documents**</span></span>

<span data-ttu-id="86ee2-168">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="86ee2-168">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="86ee2-169">
[DNS-rekordhalmazok és -rekordok létrehozása az Azure Portal használatával](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="86ee2-169">
[Create DNS record sets and records by using the Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="86ee2-170">
[SRV rekordtípus (Wikipédia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="86ee2-170">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="86ee2-171">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86ee2-171">Next steps</span></span>

* <span data-ttu-id="86ee2-172">További tudnivalók [Azure DNS-zónák és rekordok](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="86ee2-172">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="86ee2-173">Azure DNS használatának megkezdéséhez megtudhatja, hogyan [hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone-portal.md) és [DNS-rekordok létrehozása](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="86ee2-173">To start using Azure DNS, learn how to [create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="86ee2-174">Egy meglévő DNS-zóna áttelepítéséhez megtudhatja, hogyan [importálni és exportálni egy DNS-zónafájlját](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="86ee2-174">To migrate an existing DNS zone, learn how to [import and export a DNS zone file](dns-import-export.md).</span></span>

