---
title: "aaaAzure SQL adatbázis Azure esettanulmány - Snelstart |} Microsoft Docs"
description: "További tudnivalók: hogyan SnelStart által használt SQL-adatbázis toorapidly kibontva 1000 új Azure SQL-adatbázisok havi gyakorisággal az üzleti szolgáltatás"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: fab506b2-439d-4f1a-bdc5-d1d25c80d267
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: 69572393f41a9baf44f41b4f5f4ebbef347de851
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="with-azure-snelstart-has-rapidly-expanded-its-business-services-at-a-rate-of-1000-new-azure-sql-databases-per-month"></a><span data-ttu-id="68510-103">Az Azure-SnelStart gyorsan bővített 1000 új Azure SQL-adatbázisok havi gyakorisággal az üzleti szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="68510-103">With Azure, SnelStart has rapidly expanded its business services at a rate of 1,000 new Azure SQL Databases per month</span></span>
![SnelStartLogo](./media/sql-database-implementation-snelstart/snelstartlogo.png)

<span data-ttu-id="68510-105">SnelStart hello Hollandia népszerű pénzügyi - és üzleti-kezelési szoftver a kis - és közepes méretű vállalkozások számára (SMB) hoz.</span><span class="sxs-lookup"><span data-stu-id="68510-105">SnelStart makes popular financial- and business-management software for small- and medium-sized businesses (SMBs) in hello Netherlands.</span></span> <span data-ttu-id="68510-106">A személyzet 110 alkalmazottak, beleértve az informatikai részleg 35 által kiszolgált 55,000 ügyfeleinek.</span><span class="sxs-lookup"><span data-stu-id="68510-106">Its 55,000 customers are serviced by a staff of 110 employees, including an IT staff of 35.</span></span> <span data-ttu-id="68510-107">Asztali szoftverek tooa szoftver-,-a-(SaaS) szolgáltatásajánlat épülő Azure mozgatásával SnelStart készített legtöbb hello beépített szolgáltatás, a megszokott környezetben a C# segítségével, és a teljesítmény és méretezhetőség optimalizálása által sem over - felügyeletének automatizálására vagy a kiépítés vállalatok rugalmas készleteket használó.</span><span class="sxs-lookup"><span data-stu-id="68510-107">By moving from desktop software tooa software-as-a-service (SaaS) offering built on Azure, SnelStart made hello most of built-in services, automating management using familiar environment in C#, and optimizing performance and scalability by neither over- or under-provisioning businesses using elastic pools.</span></span> <span data-ttu-id="68510-108">Azure használatával biztosít az ügyfelek a helyszíni és hello közötti áthelyezése SnelStart hello rugalmasság felhő.</span><span class="sxs-lookup"><span data-stu-id="68510-108">Using Azure provides SnelStart hello fluidity of moving customers between on-premises and hello cloud.</span></span>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-Case-Study-SnelStart/player]
> 
> 

## <a name="why-snelstart-extended-services-from-hello-desktop-toohello-cloud"></a><span data-ttu-id="68510-109">Miért SnelStart kiterjesztett hello asztali toohello felhőből szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="68510-109">Why SnelStart extended services from hello desktop toohello cloud</span></span>
> <span data-ttu-id="68510-110">"Az Azure-ral működő azt jelenti, hogy azt is kézbesíti szoftver gyorsabb, gyorsan reagálni toocustomer iránti igények kielégítése érdekében és méretezhető megoldás, amikor iránti igények kielégítése érdekében."</span><span class="sxs-lookup"><span data-stu-id="68510-110">“Working with Azure means we can deliver software faster, quickly react toocustomer demands, and scale solutions when demands increase.”</span></span>
> 
> <span data-ttu-id="68510-111">– Henry lett, a szoftver felelős mérnök</span><span class="sxs-lookup"><span data-stu-id="68510-111">— Henry Been, Software Architect</span></span>
> 
> 

<span data-ttu-id="68510-112">SnelStart lefutott egy sikeres szoftver üzleti évig, egy hagyományos fejlesztési modellel: code csomagot, küldje el, majd ismételje meg.</span><span class="sxs-lookup"><span data-stu-id="68510-112">SnelStart ran a successful software business for years, using a traditional development model: code, package, ship, and repeat.</span></span> <span data-ttu-id="68510-113">Az idő múlásával változás lépést hello nőtt, gyorsabb és gyorsabb.</span><span class="sxs-lookup"><span data-stu-id="68510-113">Over time, hello pace of change grew faster and faster.</span></span> <span data-ttu-id="68510-114">Szabályzat gyakran változó, és ügyfelek pénzügyi rekordok könnyebb módon tooprocess szükséges, és együttműködik a könyvelők és kormányzati tookeep be ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="68510-114">Regulations changed frequently, and customers needed easier ways tooprocess financial records and collaborate with their accountants and government tookeep up with those changes.</span></span>

<span data-ttu-id="68510-115">"Szállítási szoftver toocustomers volt, költséges és korlátozó," tooHenry megfelelően lett, a szoftver felelős mérnök.</span><span class="sxs-lookup"><span data-stu-id="68510-115">“Shipping software toocustomers was costly and limiting,” according tooHenry Been, software architect.</span></span> <span data-ttu-id="68510-116">"Éles csomagolás és szállítási költségek korlátozott azt sikerült a gyakoriságát kibocsátási szoftver.</span><span class="sxs-lookup"><span data-stu-id="68510-116">“Production, packaging, and shipping costs limited how often we could release software.</span></span> <span data-ttu-id="68510-117">Rendszeres szállítására toopackage frissítései volt, de, amely tette rögzített toomeet felhasználóink igények módosítása.</span><span class="sxs-lookup"><span data-stu-id="68510-117">We had toopackage updates for periodic shipments, but that made it hard toomeet our customers’ changing needs.</span></span> <span data-ttu-id="68510-118">A Microsoft sikerült soha nem biztos lehet benne, hogy az ügyfelek frissítése toohello hello termék legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="68510-118">We could never be assured that our customers upgraded toohello latest version of hello product.</span></span> <span data-ttu-id="68510-119">Ezért azt kellett toosupport több verziója is támogatja a nagyon nehéz feladat toodo hello végrehajtása."</span><span class="sxs-lookup"><span data-stu-id="68510-119">Therefore, we had toosupport multiple versions, making hello support job very hard toodo well.”</span></span>

<span data-ttu-id="68510-120">Nincs ad hozzá, "úgy meg akartunk tooprogram és a kiadás frissítéseket, egy gyorsított lépést, így azt nem sikerült innovációját gyorsabb és az ügyfelek érdekében új szolgáltatások létrehozására.</span><span class="sxs-lookup"><span data-stu-id="68510-120">Been adds, “We wanted a way tooprogram and release updates at an accelerated pace, so we could innovate faster and create new services for our customers.</span></span> <span data-ttu-id="68510-121">Is meg akartunk egy módon tooautomate több dolgozza fel a rendelés toosimplify felhasználóink üzleti-címfelügyeleti szükségletek."</span><span class="sxs-lookup"><span data-stu-id="68510-121">We also wanted a way tooautomate more processes in order toosimplify our customers’ business-administration needs.”</span></span>

<span data-ttu-id="68510-122">SnelStart, a hello megoldás volt tooextend szolgáltatásai egy felhőalapú Szolgáltatottszoftver-szolgáltató váljon.</span><span class="sxs-lookup"><span data-stu-id="68510-122">For SnelStart, hello solution was tooextend its services by becoming a cloud-based SaaS provider.</span></span> <span data-ttu-id="68510-123">hello Azure SQL Database platform segített SnelStart juthat el nélkül hello fő az informatikai részleg terhelését, amely egy infrastruktúra--a-szolgáltatásként (IaaS) megoldás lenne szükség.</span><span class="sxs-lookup"><span data-stu-id="68510-123">hello Azure SQL Database platform helped SnelStart get there without incurring hello major IT overhead that an infrastructure-as-a-service (IaaS) solution would have required.</span></span>

<span data-ttu-id="68510-124">hello felhős modellnek is lehetővé teszi, hogy a SnelStart toofix hibákat, és új funkciók biztosítására gyorsan, anélkül, hogy az ügyfelek toodownload és frissítési szoftver kellene.</span><span class="sxs-lookup"><span data-stu-id="68510-124">hello cloud model also enables SnelStart toofix bugs and provide new features rapidly, without customers needing toodownload and upgrade software.</span></span> <span data-ttu-id="68510-125">Függően tooBeen, "Using Azure felhőszolgáltatások lehetővé teszi, hogy velünk tooquickly act változó követelmények harmadik felek során.</span><span class="sxs-lookup"><span data-stu-id="68510-125">According tooBeen, “Using Azure cloud services enables us tooquickly act upon changing requirements from third parties.</span></span> <span data-ttu-id="68510-126">Így ahelyett hogy tooship ügyfelek egy új verzió toothousands, azt módosíthatja az asztali alkalmazások toonew formátumokból szükséges harmadik felek által küldött adatokat."</span><span class="sxs-lookup"><span data-stu-id="68510-126">Instead of having tooship a new version toothousands of customers, we can adapt information sent from our desktop application toonew formats required by third parties.”</span></span>

## <a name="a-modern-company-with-traditional-roots"></a><span data-ttu-id="68510-127">A hagyományos gyökérrel rendelkező modern vállalati</span><span class="sxs-lookup"><span data-stu-id="68510-127">A modern company with traditional roots</span></span>
<span data-ttu-id="68510-128">SnelStart too1964, elrendeli indításakor hello is hello vállalati, a gyártó zenei szolgáltatás részek szerény gyökérrel rendelkező modern, a gyors, a csúcstechnológiai üzleti.</span><span class="sxs-lookup"><span data-stu-id="68510-128">SnelStart is a modern, agile, high-tech business with humble roots dating too1964, when hello founders started hello company as a manufacturer of musical instrument parts.</span></span> <span data-ttu-id="68510-129">hello SnelStart szoftver üzleti hello lelke valóban lépések során hello elterjedése hello személyi számítógép hello 1980-as években, a zúzása.</span><span class="sxs-lookup"><span data-stu-id="68510-129">hello heart of hello SnelStart software business really started beating in hello 1980s, during hello proliferation of hello personal computer.</span></span> <span data-ttu-id="68510-130">hello vállalati szükségesek egy jobb alternatív toohello nyilvántartási szoftverrel hello időpontban, így saját kezébe kérdések tartott.</span><span class="sxs-lookup"><span data-stu-id="68510-130">hello company needed a better alternative toohello accounting software available at hello time, so it took matters into its own hands.</span></span> <span data-ttu-id="68510-131">Hello vállalati 1982, mi végül válna SnelStart nyilvántartási szoftver hello rálátással hozott létre.</span><span class="sxs-lookup"><span data-stu-id="68510-131">In 1982, hello company created hello foundation of what would eventually become SnelStart accounting software.</span></span> <span data-ttu-id="68510-132">Hello kezdetektől hello szoftver az egyszerűség és sebesség lett admired –, hogy hello vállalati végül fókusz megváltozott, és magát a szoftver vállalatba reinvented túl nagy.</span><span class="sxs-lookup"><span data-stu-id="68510-132">From hello start, hello software was admired for its simplicity and speed—so much so that hello company eventually changed focus and reinvented itself into a software company.</span></span>

<span data-ttu-id="68510-133">SnelStart 1995-ben jelent meg az első könyvelés alkalmazás Windows.</span><span class="sxs-lookup"><span data-stu-id="68510-133">In 1995, SnelStart released its first bookkeeping application for Windows.</span></span> <span data-ttu-id="68510-134">hello alkalmazás, a Microsoft Visual Basic 1.0-s és a Microsoft Access-adatbázisok épül segített hello ügyfél alap toomore mint 5000 felhasználója nő.</span><span class="sxs-lookup"><span data-stu-id="68510-134">hello application, built on Microsoft Visual Basic 1.0 and Microsoft Access databases, helped grow hello customer base toomore than 5,000 users.</span></span>

<span data-ttu-id="68510-135">Napjainkban SnelStart egy fő szolgáltató holland SMB és az önálló dinamikus irányuló üzleti-felügyeleti alkalmazás és az üzletági (LOB).</span><span class="sxs-lookup"><span data-stu-id="68510-135">Today, SnelStart is a major provider of a line-of-business (LOB) and business-administration application targeted at Dutch SMBs and self-employed entrepreneurs.</span></span> <span data-ttu-id="68510-136">Carlo Kuip, informatikai rendszerfejlesztők szerint, mert "célunk tooprovide 100 %-os automation üzleti-felügyeleti szolgáltatások adhatunk ügyfeleink."</span><span class="sxs-lookup"><span data-stu-id="68510-136">As Carlo Kuip, IT architect, says, “Our goal is tooprovide 100-percent automation of business-administration services for our customers.”</span></span>

## <a name="optimizing-performance-and-cost-with-elastic-pools"></a><span data-ttu-id="68510-137">Teljesítmény és a rugalmas készletek költséget optimalizálása</span><span class="sxs-lookup"><span data-stu-id="68510-137">Optimizing performance and cost with elastic pools</span></span>
<span data-ttu-id="68510-138">SnelStart rugalmas készletek nagyméretű korai alkalmazó volt.</span><span class="sxs-lookup"><span data-stu-id="68510-138">SnelStart was a large-scale early adopter of elastic pools.</span></span> <span data-ttu-id="68510-139">Rugalmas készletek hello vállalati korlát kapcsolatos költségek, és hatékonyabban kezelheti a teljesítményre vonatkozó követelmények.</span><span class="sxs-lookup"><span data-stu-id="68510-139">Elastic pools help hello company limit costs and manage performance requirements more efficiently.</span></span> <span data-ttu-id="68510-140">Szerint tooBeen "rugalmas készletek használatával azt is teljesítményének optimalizálásához hello igények ügyfeleink, alapján túlzott kiosztása nélkül.</span><span class="sxs-lookup"><span data-stu-id="68510-140">According tooBeen, “By using elastic pools, we can optimize performance based on hello needs of our customers, without over-provisioning.</span></span> <span data-ttu-id="68510-141">Ha le kellett tooprovision csúcsterhelés alapján, elég költséges lenne.</span><span class="sxs-lookup"><span data-stu-id="68510-141">If we had tooprovision based on peak load, it would be quite costly.</span></span> <span data-ttu-id="68510-142">Ehelyett hello beállítás tooshare erőforrások között, több alacsony kihasználtságú adatbázisok kiválaszthatjuk toocreate is végez, és költséghatékony megoldást. "</span><span class="sxs-lookup"><span data-stu-id="68510-142">Instead, hello option tooshare resources between multiple, low-usage databases allows us toocreate a solution that performs well and is cost effective.”</span></span>

## <a name="azure-sql-databases-help-containerize-data-for-isolation-and-security"></a><span data-ttu-id="68510-143">Az Azure SQL-adatbázisok súgó containerize elkülönítési és biztonsági adatait</span><span class="sxs-lookup"><span data-stu-id="68510-143">Azure SQL Databases help containerize data for isolation and security</span></span>
<span data-ttu-id="68510-144">Az Azure SQL Database SnelStart tooeasily lehetővé teszi, és ügyfelek helyszíni üzleti-felügyeleti adatok tooAzure transzparens módon történő áthelyezését.</span><span class="sxs-lookup"><span data-stu-id="68510-144">Azure SQL Database enables SnelStart tooeasily and transparently move customers’ on-premises business-administration data tooAzure.</span></span> <span data-ttu-id="68510-145">hello Azure SQL Database egy kényelmes tároló, amely elszigetelésére, a határ hitelesítési, engedélyezési és egyszerű biztonsági mentési és visszaállítási képességeket biztosítja.</span><span class="sxs-lookup"><span data-stu-id="68510-145">hello Azure SQL Database is a convenient container that provides isolation, a boundary for authentication, authorization, and easy backup and restore capabilities.</span></span> <span data-ttu-id="68510-146">Adatbázisok jól alkalmazható fogalmi modell üzleti felügyeleti adja meg.</span><span class="sxs-lookup"><span data-stu-id="68510-146">Databases provide a well-suited conceptual model for business administration.</span></span> <span data-ttu-id="68510-147">Függően tooCarlo Kuip, informatikai felelős mérnök, "a tároló határon belül tartalmazhatnak bizalmas adatokat, amely kritikus fontosságú tooa üzleti, és tárolja azokat a cikkeket egy elkülönített adatbázis tartja őket védettek.</span><span class="sxs-lookup"><span data-stu-id="68510-147">According tooCarlo Kuip, IT architect, “Items within this container boundary contain sensitive data that is crucial tooa business, and storing those items in an isolated database keeps them well protected.</span></span> <span data-ttu-id="68510-148">Azt is fiókonkénti hello adatbázis szintjén, és még automatizálásához hello felügyeleti és ezeknek az adatbázisoknak a kibővített anélkül, hogy az adatbázis-rendszergazdák (DBAs) személyzeti."</span><span class="sxs-lookup"><span data-stu-id="68510-148">We can manage authorization at hello database level, and even automate hello management and scale-out of these databases without requiring database administrators (DBAs) on staff.”</span></span>

<span data-ttu-id="68510-149">Az SQL Data Warehouse is szerepet játszik hello SnelStart biztonságot és felügyeletet szövegegység segíti a hello vállalati gyűjt telemetrikus adatokat, például a behatolás-észlelés, a felhasználó naplózása és a kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="68510-149">Azure SQL Data Warehouse also plays a role in hello SnelStart security and management story by helping hello company gather telemetry data, such as intrusion detection, user activity logging, and connectivity.</span></span>

## <a name="azure-removes-overhead-so-that-developers-can-spend-more-time-delivering-value"></a><span data-ttu-id="68510-150">Azure terhet eltávolítja az, hogy a fejlesztők is időt több érték továbbítása</span><span class="sxs-lookup"><span data-stu-id="68510-150">Azure removes overhead so that developers can spend more time delivering value</span></span>
<span data-ttu-id="68510-151">hello Azure platformon modell infrastruktúra terhelés eltávolítva, és SnelStart tooautomate üzembe helyezése C# kezelési kódtárakat engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="68510-151">hello Azure platform model removed infrastructure overhead and enabled SnelStart tooautomate deployments using C# management libraries.</span></span> <span data-ttu-id="68510-152">Kuip szerint, mert "volt a folyamatban lévő műveletek egy nagyon kis személyzet egyidejűleg növekvő méretezhetőséget, a sebesség és a katasztrófa utáni helyreállítás során a beállítások az ügyfelek képesek toogrow.</span><span class="sxs-lookup"><span data-stu-id="68510-152">As Kuip states, “We were able toogrow our current operations with a very small staff while simultaneously increasing scalability, speed, and disaster recovery options for our clients.</span></span> <span data-ttu-id="68510-153">hello shift tooservices fejlesztési felszabadulnak erőforrások toofocus a új szolgáltatások és funkciók helyett csak frissítése meglévő kódot tookeep fel az új szabályzat vagy adó kódot."</span><span class="sxs-lookup"><span data-stu-id="68510-153">hello shift tooservices development freed up resources toofocus on new services and features, instead of just updating existing code tookeep up with new regulations or tax codes.”</span></span> <span data-ttu-id="68510-154">Hozzáad, "Felügyeletének automatizálására, és hello SaaS ajánlat használatával, dolgozunk több érték, az ügyfelek számára anélkül, hogy a nagy beruházások toomake működési személyzeti képes toodeliver."</span><span class="sxs-lookup"><span data-stu-id="68510-154">He adds, “By automating management and using hello SaaS offering, we are able toodeliver more value for our clients without having toomake large investments in operational staff.”</span></span> <span data-ttu-id="68510-155">Például az Azure és a rugalmas készletek használatával SnelStart volt képes tooadd számos új funkciót, beleértve a robusztusabb ügyféladatok integráció a bankokhoz, új számlázási szolgáltatások kisvállalati átvilágítást és e-mailek szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="68510-155">For example, by using Azure and elastic pools, SnelStart was able tooadd a variety of new features, including more robust customer-data integration with banks, new billing services, small-business background checks, and email services.</span></span>

> <span data-ttu-id="68510-156">"Az első néhány hónappal 2016 hello pedig csak azt az Azure SQL Database 12 000-nél készül 5500 toomore telepítéseit kibontva, és azt jelenleg visszaigazolása havonta körülbelül 1000 adatbázisok."</span><span class="sxs-lookup"><span data-stu-id="68510-156">"In just hello first few months of 2016, we expanded our Azure SQL Database deployments from about 5,500 toomore than 12,000, and we’re currently adding about 1,000 databases per month.”</span></span>
> 
> <span data-ttu-id="68510-157">– Henry lett, a szoftver felelős mérnök</span><span class="sxs-lookup"><span data-stu-id="68510-157">— Henry Been, Software Architect</span></span>
> 
> 

<span data-ttu-id="68510-158">Adatbázis-felügyeleti további automatizált hello rugalmas feladatok szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="68510-158">Database management is further automated by using hello elastic jobs feature.</span></span> <span data-ttu-id="68510-159">Kuip szerint, mert "magas Köszönjük hello automatikus észlelése az adatbázisok SQL DB [kiszolgáló] példányán."</span><span class="sxs-lookup"><span data-stu-id="68510-159">As Kuip states, “We highly appreciate hello automatic discovery of databases on a [server] instance of SQL DB.”</span></span> <span data-ttu-id="68510-160">Ez lehetővé teszi SnelStart tooexecute felügyeleti műveletek a dinamikusan növekvő ügyfél alap többletterhelést nélkül.</span><span class="sxs-lookup"><span data-stu-id="68510-160">This allows SnelStart tooexecute management operations across their dynamically growing customer base without additional overhead.</span></span>

<span data-ttu-id="68510-161">SnelStart is fejleszti az API-k, amelyek a felhasználói adatok és a harmadik féltől származó szoftverek partnerek által létrehozott alkalmazások közötti közvetítőként működik.</span><span class="sxs-lookup"><span data-stu-id="68510-161">SnelStart is also developing an API that acts as a broker between customer data and apps built by third-party software partners.</span></span> <span data-ttu-id="68510-162">Kuip állapotok, mint "Ez az API lehetővé teszi más szállítók tooadd funkció tooour szoftverek, például számlákat és egyéb dokumentumokat esetében bevitt adat kiküszöbölése."</span><span class="sxs-lookup"><span data-stu-id="68510-162">As Kuip states, “This API will enable other vendors tooadd functionality tooour software, such as eliminating data entry for invoices and other documents.”</span></span> <span data-ttu-id="68510-163">Az ügyfelek már nem fog rendelkezni toomanually típus számlák azok kisvállalati nyilvántartási szoftver; hello SnelStart szoftver közvetlenül cserélnek hello szükséges információkat.</span><span class="sxs-lookup"><span data-stu-id="68510-163">Customers will no longer have toomanually type invoices into their small-business accounting software; hello SnelStart software will exchange hello necessary information directly.</span></span> <span data-ttu-id="68510-164">Ez lehetővé teszi az ügyfelek toojoin az üzleti-felügyeleti feladatok és, amely a digitális átalakítása hello iparágban egyre inkább hello-ökoszisztéma.</span><span class="sxs-lookup"><span data-stu-id="68510-164">This allows customers toojoin their business-administration tasks with hello ecosystem of information that is emerging from digital transformation in hello industry.</span></span>  

## <a name="how-azure-services-enable-saas-for-snelstart"></a><span data-ttu-id="68510-165">Hogyan Azure-szolgáltatások engedélyezése SaaS SnelStart</span><span class="sxs-lookup"><span data-stu-id="68510-165">How Azure services enable SaaS for SnelStart</span></span>
<span data-ttu-id="68510-166">Azure használatával SnelStart ki tud szolgálni az ügyfelek és a könyvelők többet zökkenőmentesen hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="68510-166">By using Azure, SnelStart can serve its customers and their accountants more seamlessly in hello cloud.</span></span> <span data-ttu-id="68510-167">Például az ügyfelek és a könyvelők információkat oszthat meg az Azure-on tárolt SnelStart tartozó ügyfél API közvetlen elérésével.</span><span class="sxs-lookup"><span data-stu-id="68510-167">For example, both customers and accountants can share information by directly accessing SnelStart’s client API, hosted on Azure.</span></span> <span data-ttu-id="68510-168">Kuip állapota "újrafelhasználható szolgáltatásokról elérhető tooour ügyfélkapcsolati web apps, és ezek biztosítanak közös infrastruktúrát és funkciók tooallow toobusiness felügyelete az ügyfelek és az ügyfelek által használt toothird szoftver.</span><span class="sxs-lookup"><span data-stu-id="68510-168">Kuip states, “These reusable services are available tooour customer-facing web apps, and they provide common infrastructure and functions tooallow access toobusiness administration for customers and toothird-party software used by our customers.</span></span> <span data-ttu-id="68510-169">Például a termék-konfiguráció képességek biztosítása, tűzfal-szabályok kezelése és a hosszú futású folyamatokat, például biztonsági másolatok kezelése. "</span><span class="sxs-lookup"><span data-stu-id="68510-169">Examples include providing product-configuration capabilities, managing firewall rules, and managing long-running processes like backups.”</span></span>

> <span data-ttu-id="68510-170">Célunk tooprovide 100 %-os automation üzleti-felügyeleti szolgáltatások adhatunk ügyfeleink."</span><span class="sxs-lookup"><span data-stu-id="68510-170">Our goal is tooprovide 100-percent automation of business-administration services for our customers.”</span></span> 
> 
> <span data-ttu-id="68510-171">– Carlo Kuip, informatikai felelős mérnök</span><span class="sxs-lookup"><span data-stu-id="68510-171">— Carlo Kuip, IT Architect</span></span>
> 
> 

<span data-ttu-id="68510-172">Ezenkívül SnelStart webszolgáltatások engedélyezése az ügyfelek és a könyvelők tooeasily access adatok az Azure SQL Database rugalmas készletek.</span><span class="sxs-lookup"><span data-stu-id="68510-172">In addition, SnelStart web services allow both customers and accountants tooeasily access data in Azure SQL Database elastic pools.</span></span> <span data-ttu-id="68510-173">A Szolgáltatottszoftver modell, adatbázis a rugalmasság és az Azure Resource Manager alapján kialakulhat SnelStart biztosít minden Azure-telepítés kiegészítő méretezhetőség szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="68510-173">This SaaS model, coupled with database elasticity and Azure Resource Manager, provides SnelStart with scalability features that complement every Azure deployment.</span></span> <span data-ttu-id="68510-174">hello megvalósítási teljesen automatizált kezelési kódtárakat C# használatával.</span><span class="sxs-lookup"><span data-stu-id="68510-174">hello implementation is fully automated using C# management libraries.</span></span>

![SnelStart architektúra](./media/sql-database-implementation-snelstart/figure1.png)

<span data-ttu-id="68510-176">1. ábra.</span><span class="sxs-lookup"><span data-stu-id="68510-176">Figure 1.</span></span> <span data-ttu-id="68510-177">2016. június SnelStart rendelkezik, több mint 11 000 adatbázisok és több mint 50 rugalmas készletek</span><span class="sxs-lookup"><span data-stu-id="68510-177">As of June 2016, SnelStart has more than 11,000 databases and more than 50 elastic pools</span></span>

## <a name="simplicity-from-hello-cloud"></a><span data-ttu-id="68510-178">Egyszerűség hello felhőből</span><span class="sxs-lookup"><span data-stu-id="68510-178">Simplicity from hello cloud</span></span>
<span data-ttu-id="68510-179">Helyezze át az Azure felhőalapú megoldás tooan, SnelStart óta képes toosupport gyors felhasználói növekedési kínálja a új szolgáltatások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="68510-179">Since moving tooan Azure cloud-based solution, SnelStart has been able toosupport rapid customer growth while offering innovative features and services.</span></span> <span data-ttu-id="68510-180">TooBeen, szerint "az Azure-ral, azt biztosíthat közelében folyamatos frissítések adhatunk ügyfeleink, a műveleti személyzet kiterjesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="68510-180">According tooBeen, “With Azure, we can deliver near-continuous updates for our customers, without expanding our operations staff.</span></span> <span data-ttu-id="68510-181">És azt minden hello nagyszerű Azure szolgáltatásokat – például a méretezhetőséget és katasztrófa-helyreállítás – kötegelve. "</span><span class="sxs-lookup"><span data-stu-id="68510-181">And we get all hello other great Azure features—like scalability and disaster recovery—bundled in.”</span></span>

> <span data-ttu-id="68510-182">"Ha a rendszer ténylegesen keresztül Redmondban található irodájába... Hívás érkezett egy fejlesztő vissza a hello Hollandia, a megadott problémáról hívnia nekem</span><span class="sxs-lookup"><span data-stu-id="68510-182">"When we were actually over in Redmond...I received a call from a developer back in hello Netherlands, phoning me about a specific problem.</span></span> <span data-ttu-id="68510-183">Azt is képes tooget Microsoft toodeliver az éles környezetben toosolve 48 órán belül változása a probléma. "</span><span class="sxs-lookup"><span data-stu-id="68510-183">We were able tooget Microsoft toodeliver a change in their production environment within 48 hours toosolve our problem."</span></span>
> 
> <span data-ttu-id="68510-184">– Henry lett, a szoftver felelős mérnök</span><span class="sxs-lookup"><span data-stu-id="68510-184">— Henry Been, Software Architect</span></span>
> 
> 

<span data-ttu-id="68510-185">SnelStart is hello szoros együttműködésben azok már hello Azure SQL-adatbázis a Microsoft szakértőivel közösen kifejlesztett figyelemreméltónak tartja.</span><span class="sxs-lookup"><span data-stu-id="68510-185">SnelStart also appreciates hello strong partnership they’ve developed with hello Microsoft Azure SQL DB team.</span></span> <span data-ttu-id="68510-186">Kuip állapotok, mint "beszélgetéseket van a szolgáltatások és hogyan toouse technológia, amely valami mindkét oldalon értékeljük."</span><span class="sxs-lookup"><span data-stu-id="68510-186">As Kuip states, “we have discussions on features and how toouse technology, which is something appreciated on both sides.”</span></span>
<span data-ttu-id="68510-187">hello azonnali a SnelStart célja meg ügyfelének alap növekvő tookeep.</span><span class="sxs-lookup"><span data-stu-id="68510-187">hello immediate goal for SnelStart is tookeep growing its satisfied customer base.</span></span> <span data-ttu-id="68510-188">Mivel állapotai "Hello műszaki és erőforrás-korlátozások le kellett, mint egy független, nélkül nincs nincs korlát toohow, amennyiben azt a milyen mértékben növelhető."</span><span class="sxs-lookup"><span data-stu-id="68510-188">As Been states, “Without hello technical and resource limitations that we had as an ISV, there’s no limit toohow far we can grow.”</span></span>

## <a name="more-information"></a><span data-ttu-id="68510-189">További információ</span><span class="sxs-lookup"><span data-stu-id="68510-189">More information</span></span>
* <span data-ttu-id="68510-190">toolearn Azure rugalmas készletek, kapcsolatos további információkért lásd: [rugalmas készletek](sql-database-elastic-pool.md).</span><span class="sxs-lookup"><span data-stu-id="68510-190">toolearn more about Azure elastic pools, see [elastic pools](sql-database-elastic-pool.md).</span></span>
* <span data-ttu-id="68510-191">További információ a webes és feldolgozói szerepkörök toolearn lásd [feldolgozói szerepkörök](../fundamentals-introduction-to-azure.md#compute).</span><span class="sxs-lookup"><span data-stu-id="68510-191">toolearn more about Web roles and worker roles, see [worker roles](../fundamentals-introduction-to-azure.md#compute).</span></span>    
* <span data-ttu-id="68510-192">További információ az Azure SQL Data Warehouse toolearn lásd [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)</span><span class="sxs-lookup"><span data-stu-id="68510-192">toolearn more about Azure SQL Data Warehouse,see [SQL Data Warehouse](https://azure.microsoft.com/documentation/services/sql-data-warehouse/)</span></span>
* <span data-ttu-id="68510-193">toolearn SnelStart, kapcsolatos további információkért lásd: [SnelStart](http://www.snelstart.nl).</span><span class="sxs-lookup"><span data-stu-id="68510-193">toolearn more about SnelStart, see [SnelStart](http://www.snelstart.nl).</span></span>
