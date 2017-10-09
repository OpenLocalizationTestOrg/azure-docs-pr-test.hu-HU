---
title: "aaaAzure DNS-hibaelhárítási útmutatója |} Microsoft Docs"
description: "Hogyan tootroubleshoot közös Azure DNS-problémák"
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
ms.openlocfilehash: 944aa1811c980063f739268cd2c79b647b2754a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-dns-troubleshooting-guide"></a><span data-ttu-id="0dc81-103">Az Azure DNS-hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="0dc81-103">Azure DNS troubleshooting guide</span></span>

<span data-ttu-id="0dc81-104">Ezen a lapon hibaelhárításával kapcsolatos kérdésekre Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="0dc81-104">This page provides troubleshooting information for common Azure DNS questions.</span></span>

<span data-ttu-id="0dc81-105">Ha ezeket a lépéseket nem sikerül megoldani a problémát, keresse meg vagy is a probléma írjon a [közösségi támogatási fórum MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span><span class="sxs-lookup"><span data-stu-id="0dc81-105">If these steps don't resolve your issue, you can also search for or post your issue on our [community support forum on MSDN](https://social.msdn.microsoft.com/Forums/en-US/home?forum=WAVirtualMachinesVirtualNetwork).</span></span> <span data-ttu-id="0dc81-106">Azt is megteheti nyissa meg az Azure támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="0dc81-106">Alternatively, open an Azure support request.</span></span>


## <a name="i-cant-create-a-dns-zone"></a><span data-ttu-id="0dc81-107">Nem lehet létrehozni egy DNS-zóna</span><span class="sxs-lookup"><span data-stu-id="0dc81-107">I can't create a DNS zone</span></span>

<span data-ttu-id="0dc81-108">tooresolve gyakori problémákat, próbálja meg egy vagy több hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0dc81-108">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="0dc81-109">Felülvizsgálati hello Azure DNS naplózási toodetermine hello hiba okát is megadva naplózza.</span><span class="sxs-lookup"><span data-stu-id="0dc81-109">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="0dc81-110">Minden DNS-zónanévnek egyedinek kell lennie a saját erőforráscsoportján belül.</span><span class="sxs-lookup"><span data-stu-id="0dc81-110">Each DNS zone name must be unique within its resource group.</span></span> <span data-ttu-id="0dc81-111">Ez azt jelenti, hogy két DNS-zóna hello az azonos név nem lehet megosztani az egy erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="0dc81-111">That is, two DNS zones with hello same name cannot share a resource group.</span></span> <span data-ttu-id="0dc81-112">Próbáljon meg eltérő zónanevet vagy erőforráscsoportot használni.</span><span class="sxs-lookup"><span data-stu-id="0dc81-112">Try using a different zone name, or a different resource group.</span></span>
3.  <span data-ttu-id="0dc81-113">Hiba jelenhet meg, "Elérte vagy túllépte a zónák az előfizetés {előfizetés-azonosító} hello maximális számát."</span><span class="sxs-lookup"><span data-stu-id="0dc81-113">You may see an error "You have reached or exceeded hello maximum number of zones in subscription {subscription id}."</span></span> <span data-ttu-id="0dc81-114">Vagy egy másik Azure-előfizetéssel, törölje néhány zónákat, vagy forduljon Azure támogatási tooraise az előfizetési határértéket.</span><span class="sxs-lookup"><span data-stu-id="0dc81-114">Either use a different Azure subscription, delete some zones, or contact Azure Support tooraise your subscription limit.</span></span>
4.  <span data-ttu-id="0dc81-115">Előfordulhat, hogy tekintse meg a hiba "hello zóna ({name} zóna) nem áll rendelkezésre."</span><span class="sxs-lookup"><span data-stu-id="0dc81-115">You may see an error "hello zone '{zone name}' is not available."</span></span> <span data-ttu-id="0dc81-116">Ez a hiba azt jelenti, hogy Azure DNS-ben a DNS-zóna névkiszolgálóit nem tooallocate.</span><span class="sxs-lookup"><span data-stu-id="0dc81-116">This error means that Azure DNS was unable tooallocate name servers for this DNS zone.</span></span> <span data-ttu-id="0dc81-117">Próbáljon meg egy másik zónanevet használni.</span><span class="sxs-lookup"><span data-stu-id="0dc81-117">Try using a different zone name.</span></span> <span data-ttu-id="0dc81-118">Azt is megteheti Ha hello tartomány neve tulajdonos, lépjen kapcsolatba az Azure-támogatás, akik névkiszolgálók foglalhatja meg.</span><span class="sxs-lookup"><span data-stu-id="0dc81-118">Alternatively, if you are hello domain name owner, contact Azure support, who can allocate name servers for you.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="0dc81-119">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="0dc81-119">**Recommended documents**</span></span>

<span data-ttu-id="0dc81-120">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="0dc81-120">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="0dc81-121">
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0dc81-121">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>

## <a name="i-cant-create-a-dns-record"></a><span data-ttu-id="0dc81-122">Nem tudok létrehozni egy DNS-rekordot</span><span class="sxs-lookup"><span data-stu-id="0dc81-122">I can't create a DNS record</span></span>

<span data-ttu-id="0dc81-123">tooresolve gyakori problémákat, próbálja meg egy vagy több hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0dc81-123">tooresolve common issues, try one or more of hello following steps:</span></span>

1.  <span data-ttu-id="0dc81-124">Felülvizsgálati hello Azure DNS naplózási toodetermine hello hiba okát is megadva naplózza.</span><span class="sxs-lookup"><span data-stu-id="0dc81-124">Review hello Azure DNS audit logs toodetermine hello failure reason.</span></span>
2.  <span data-ttu-id="0dc81-125">Már létezik hello rekordhalmaz?</span><span class="sxs-lookup"><span data-stu-id="0dc81-125">Does hello record set exist already?</span></span>  <span data-ttu-id="0dc81-126">Az Azure DNS-rekordok rekord kezeli *beállítja*, melyek azonos nevet, és azonos hello hello rekordjának hello gyűjteménye típusa.</span><span class="sxs-lookup"><span data-stu-id="0dc81-126">Azure DNS manages records using record *sets*, which are hello collection of records of hello same name and hello same type.</span></span> <span data-ttu-id="0dc81-127">Ha egy rekord, hello azonos nevet, és írja be a már létezik, majd tooadd egy másik ilyen rekord meglévő hello kell szerkeszteni rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="0dc81-127">If a record with hello same name and type already exists, then tooadd another such record you should edit hello existing record set.</span></span>
3.  <span data-ttu-id="0dc81-128">Próbálja toocreate egy kötetet a következő hello DNS zóna felső pontja (hello "Gyökér" hello zóna)?</span><span class="sxs-lookup"><span data-stu-id="0dc81-128">Are you trying toocreate a record at hello DNS zone apex (hello ‘root’ of hello zone)?</span></span> <span data-ttu-id="0dc81-129">Ha igen, a DNS egyezmény hello toouse hello "@" karakter hello rekord neveként.</span><span class="sxs-lookup"><span data-stu-id="0dc81-129">If so, hello DNS convention is toouse hello ‘@’ character as hello record name.</span></span> <span data-ttu-id="0dc81-130">Ne feledje, hogy hello DNS-szabványokból nem teszik lehetővé a CNAME-rekordok hello zóna csúcsán.</span><span class="sxs-lookup"><span data-stu-id="0dc81-130">Also note that hello DNS standards do not permit CNAME records at hello zone apex.</span></span>
4.  <span data-ttu-id="0dc81-131">CNAME-ütközést tapasztal?</span><span class="sxs-lookup"><span data-stu-id="0dc81-131">Do you have a CNAME conflict?</span></span>  <span data-ttu-id="0dc81-132">hello DNS-szabványokból nem teszik lehetővé az azonos név bármilyen más típusú rekordként hello egy CNAME rekordot.</span><span class="sxs-lookup"><span data-stu-id="0dc81-132">hello DNS standards do not allow a CNAME record with hello same name as a record of any other type.</span></span> <span data-ttu-id="0dc81-133">Ha egy meglévő CNAME rekord létrehozásával a hello nevétől eltérő típusú sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0dc81-133">If you have an existing CNAME, creating a record with hello same name of a different type fails.</span></span>  <span data-ttu-id="0dc81-134">Hasonlóképpen egy olyan CNAME létrehozása sikertelen lesz, ha hello neve megegyezik egy eltérő típusú létező rekordot.</span><span class="sxs-lookup"><span data-stu-id="0dc81-134">Likewise, creating a CNAME fails if hello name matches an existing record of a different type.</span></span> <span data-ttu-id="0dc81-135">Távolítsa el a hello ütközés hello eltávolításával más rekord vagy egy másik rekord név kiválasztásában.</span><span class="sxs-lookup"><span data-stu-id="0dc81-135">Remove hello conflict by removing hello other record or choosing a different record name.</span></span>
5.  <span data-ttu-id="0dc81-136">Elérte hello engedélyezett DNS-zóna a rekordhalmazok hello számára vonatkozó korlátozást? rekordhalmazok aktuális száma hello és hello rekordhalmazok maximális száma látható hello a hello zóna "Tulajdonságok" hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0dc81-136">Have you reached hello limit on hello number of record sets permitted in a DNS zone? hello current number of record sets and hello maximum number of record sets are shown in hello Azure portal, under hello 'Properties' for hello zone.</span></span> <span data-ttu-id="0dc81-137">Ha elérte ezt a határt, majd vagy néhány rekordhalmazok törlése, vagy forduljon az Azure támogatási tooraise rekordhalmaz korlát a zóna, majd próbálja meg újra.</span><span class="sxs-lookup"><span data-stu-id="0dc81-137">If you have reached this limit, then either delete some record sets or contact Azure Support tooraise your record set limit for this zone, then try again.</span></span> 


### <a name="recommended-documents"></a><span data-ttu-id="0dc81-138">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="0dc81-138">**Recommended documents**</span></span>

<span data-ttu-id="0dc81-139">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="0dc81-139">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="0dc81-140">
[DNS-zóna létrehozása](dns-getstarted-create-dnszone-portal.md)</span><span class="sxs-lookup"><span data-stu-id="0dc81-140">
[Create a DNS zone](dns-getstarted-create-dnszone-portal.md)</span></span>



## <a name="i-cant-resolve-my-dns-record"></a><span data-ttu-id="0dc81-141">Nem tudom feloldani a DNS-rekordomat</span><span class="sxs-lookup"><span data-stu-id="0dc81-141">I can't resolve my DNS record</span></span>

<span data-ttu-id="0dc81-142">A DNS-névfeloldás egy többlépéses folyamat, amely több okból is meghiúsulhat.</span><span class="sxs-lookup"><span data-stu-id="0dc81-142">DNS name resolution is a multi-step process, which can fail for many reasons.</span></span> <span data-ttu-id="0dc81-143">hello következő lépések segítségével megvizsgálhatja, hogy miért üzemeltetett az Azure DNS-zóna a DNS-rekord a DNS-feloldás sikertelen.</span><span class="sxs-lookup"><span data-stu-id="0dc81-143">hello following steps help you investigate why DNS resolution is failing for a DNS record in a zone hosted in Azure DNS.</span></span>

1.  <span data-ttu-id="0dc81-144">Győződjön meg arról, hogy hello DNS-rekordok vannak konfigurálva megfelelően Azure DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="0dc81-144">Confirm that hello DNS records have been configured correctly in Azure DNS.</span></span> <span data-ttu-id="0dc81-145">Tekintse át a DNS-rekordok hello hello Azure-portálon ellenőrzése hello Zónanév, rekord neve és típusa helyesek.</span><span class="sxs-lookup"><span data-stu-id="0dc81-145">Review hello DNS records in hello Azure portal, checking that hello zone name, record name, and record type are correct.</span></span>
2.  <span data-ttu-id="0dc81-146">Győződjön meg arról, hogy hello DNS-rekordok megfelelően feloldani hello Azure DNS névkiszolgálóit.</span><span class="sxs-lookup"><span data-stu-id="0dc81-146">Confirm that hello DNS records resolve correctly on hello Azure DNS name servers.</span></span>
    - <span data-ttu-id="0dc81-147">Ha DNS-lekérdezések a helyi számítógépről, gyorsítótárazott eredményt tükröző hello névkiszolgálók hello jelenlegi állapotában nem jelenhet meg.</span><span class="sxs-lookup"><span data-stu-id="0dc81-147">If you make DNS queries from your local PC, you may see cached results that don’t reflect hello current state of hello name servers.</span></span>  <span data-ttu-id="0dc81-148">Emellett a vállalati hálózatok gyakran használják DNS-proxy kiszolgálók, ami megakadályozhatja, DNS-lekérdezések toospecific névkiszolgálók irányítva.</span><span class="sxs-lookup"><span data-stu-id="0dc81-148">Also, corporate networks often use DNS proxy servers, which prevent DNS queries from being directed toospecific name servers.</span></span>  <span data-ttu-id="0dc81-149">tooavoid ezek a problémák, használja a web-alapú névfeloldási szolgáltatásnál például [digwebinterface](http://digwebinterface.com).</span><span class="sxs-lookup"><span data-stu-id="0dc81-149">tooavoid these problems, use a web-based name resolution service such as [digwebinterface](http://digwebinterface.com).</span></span>
    - <span data-ttu-id="0dc81-150">Kell, hogy toospecify hello megfelelő névkiszolgálók a DNS-zóna látható módon hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="0dc81-150">Be sure toospecify hello correct name servers for your DNS zone, as shown in hello Azure portal.</span></span>
    - <span data-ttu-id="0dc81-151">Ellenőrizze, hogy hello DNS-név helyes (kell toospecify hello teljesen minősített neve hello Zónanév együtt), hello rekordtípus helyességéről.</span><span class="sxs-lookup"><span data-stu-id="0dc81-151">Check that hello DNS name is correct (you have toospecify hello fully qualified name, including hello zone name) and hello record type is correct</span></span>
3.  <span data-ttu-id="0dc81-152">Győződjön meg arról, hogy hello DNS-neve megfelelően lett-e [toohello Azure DNS névkiszolgálóit meghatalmazott](dns-domain-delegation.md).</span><span class="sxs-lookup"><span data-stu-id="0dc81-152">Confirm that hello DNS domain name has been correctly [delegated toohello Azure DNS name servers](dns-domain-delegation.md).</span></span> <span data-ttu-id="0dc81-153">[Számos, harmadik féltől származó webhely létezik, amely lehetővé teszi a DNS-delegálás ellenőrzését](https://www.bing.com/search?q=dns+check+tool).</span><span class="sxs-lookup"><span data-stu-id="0dc81-153">There are a [many 3rd-party web sites that offer DNS delegation validation](https://www.bing.com/search?q=dns+check+tool).</span></span> <span data-ttu-id="0dc81-154">Ez a teszt egy *zóna* delegálási tesztet, ezért kell csak megadni hello DNS-zóna nevét, és nem hello teljesen minősített bejegyzés neve.</span><span class="sxs-lookup"><span data-stu-id="0dc81-154">This test is a *zone* delegation test, so you should only enter hello DNS zone name and not hello fully qualified record name.</span></span>
4.  <span data-ttu-id="0dc81-155">A fenti hello befejezését követően, a DNS-rekord most oldja fel az megfelelően.</span><span class="sxs-lookup"><span data-stu-id="0dc81-155">Having completed hello above, your DNS record should now resolve correctly.</span></span> <span data-ttu-id="0dc81-156">újra használható tooverify, [digwebinterface](http://digwebinterface.com), ezúttal hello alapértelmezett neve beállításait.</span><span class="sxs-lookup"><span data-stu-id="0dc81-156">tooverify, you can again use [digwebinterface](http://digwebinterface.com), this time using hello default name server settings.</span></span>


### <a name="recommended-documents"></a><span data-ttu-id="0dc81-157">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="0dc81-157">**Recommended documents**</span></span>

[<span data-ttu-id="0dc81-158">A tartomány tooAzure DNS delegálása</span><span class="sxs-lookup"><span data-stu-id="0dc81-158">Delegate a domain tooAzure DNS</span></span>](dns-domain-delegation.md)



## <a name="how-do-i-specify-hello-service-and-protocol-for-an-srv-record"></a><span data-ttu-id="0dc81-159">Hogyan meg hello "szolgáltatás" és "protokoll", az SRV-rekordot?</span><span class="sxs-lookup"><span data-stu-id="0dc81-159">How do I specify hello ‘service’ and ‘protocol’ for an SRV record?</span></span>

<span data-ttu-id="0dc81-160">Az Azure DNS rekordhalmazok mint DNS-rekordok kezelése – hello bejegyzések gyűjteményét hello azonos nevet, és hello ugyanaz a típusa.</span><span class="sxs-lookup"><span data-stu-id="0dc81-160">Azure DNS manages DNS records as record sets—hello collection of records with hello same name and hello same type.</span></span> <span data-ttu-id="0dc81-161">Az SRV rekordkészlete hello "szolgáltatás" és "protokoll" kell hello rekordhalmaz nevének részeként megadott toobe.</span><span class="sxs-lookup"><span data-stu-id="0dc81-161">For an SRV record set, hello 'service' and 'protocol' need toobe specified as part of hello record set name.</span></span> <span data-ttu-id="0dc81-162">hello SRV megadva más paraméter ("priority", "súly", "port" és "target") külön-külön az egyes rekordokhoz a hello rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="0dc81-162">hello other SRV parameters ('priority', 'weight', 'port' and 'target') are specified separately for each record in hello record set.</span></span>

<span data-ttu-id="0dc81-163">Példák az SRV-rekordnevekre („sip” szolgáltatásnév, „tcp” protokoll):</span><span class="sxs-lookup"><span data-stu-id="0dc81-163">Example SRV record names (service name 'sip', protocol 'tcp'):</span></span>

- <span data-ttu-id="0dc81-164">\_a SIP. \_tcp (létrehoz egy rekordot, állítsa be megfelelően hello zóna felső pontja)</span><span class="sxs-lookup"><span data-stu-id="0dc81-164">\_sip.\_tcp (creates a record set at hello zone apex)</span></span>
- <span data-ttu-id="0dc81-165">\_sip.\_tcp.sipservice (egy „sipservice” nevű rekordhalmazt hoz létre)</span><span class="sxs-lookup"><span data-stu-id="0dc81-165">\_sip.\_tcp.sipservice (creates a record set named 'sipservice')</span></span>

### <a name="recommended-documents"></a><span data-ttu-id="0dc81-166">**Ajánlott dokumentumok**</span><span class="sxs-lookup"><span data-stu-id="0dc81-166">**Recommended documents**</span></span>

<span data-ttu-id="0dc81-167">[DNS-zónák és -rekordok](dns-zones-records.md)
</span><span class="sxs-lookup"><span data-stu-id="0dc81-167">[DNS zones and records](dns-zones-records.md)
</span></span><br><span data-ttu-id="0dc81-168">
[DNS-rekordhalmazok és rekordok létrehozása hello Azure-portál használatával](dns-getstarted-create-recordset-portal.md)
</span><span class="sxs-lookup"><span data-stu-id="0dc81-168">
[Create DNS record sets and records by using hello Azure portal](dns-getstarted-create-recordset-portal.md)
</span></span><br><span data-ttu-id="0dc81-169">
[SRV rekordtípus (Wikipédia)](https://en.wikipedia.org/wiki/SRV_record)</span><span class="sxs-lookup"><span data-stu-id="0dc81-169">
[SRV record type (Wikipedia)](https://en.wikipedia.org/wiki/SRV_record)</span></span>


## <a name="next-steps"></a><span data-ttu-id="0dc81-170">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0dc81-170">Next steps</span></span>

* <span data-ttu-id="0dc81-171">További tudnivalók [Azure DNS-zónák és rekordok](dns-zones-records.md)</span><span class="sxs-lookup"><span data-stu-id="0dc81-171">Learn about [Azure DNS zones and records](dns-zones-records.md)</span></span>
* <span data-ttu-id="0dc81-172">az Azure DNS használatával toostart megtudhatja, hogyan túl[hozzon létre egy DNS-zóna](dns-getstarted-create-dnszone-portal.md) és [DNS-rekordok létrehozása](dns-getstarted-create-recordset-portal.md).</span><span class="sxs-lookup"><span data-stu-id="0dc81-172">toostart using Azure DNS, learn how too[create a DNS zone](dns-getstarted-create-dnszone-portal.md) and [create DNS records](dns-getstarted-create-recordset-portal.md).</span></span>
* <span data-ttu-id="0dc81-173">egy meglévő DNS-zóna toomigrate megtudhatja, hogyan túl[importálni és exportálni egy DNS-zónafájlját](dns-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="0dc81-173">toomigrate an existing DNS zone, learn how too[import and export a DNS zone file](dns-import-export.md).</span></span>

