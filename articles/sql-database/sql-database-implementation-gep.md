---
title: "aaaAzure SQL adatbázis Azure esettanulmány - GEP |} Microsoft Docs"
description: "További tudnivalók arról, hogyan GEP használja az SQL-adatbázis tooreach több ügyfeleivel és nagyobb hatékonyság elérése"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: ae8bcb10-c251-4bac-b666-10a253918583
ms.service: sql-database
ms.custom: reference
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/10/2017
ms.author: carlrab
ms.openlocfilehash: d5f2c4cccc1f233afc8703b6d808c7771ef631af
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-gives-gep-global-reach-and-greater-efficiency"></a><span data-ttu-id="ee478-103">Azure ad globális reach GEP és hatékonyságot</span><span class="sxs-lookup"><span data-stu-id="ee478-103">Azure gives GEP global reach and greater efficiency</span></span>
![GEP embléma](./media/sql-database-implementation-gep/geplogo.png)

<span data-ttu-id="ee478-105">GEP nyújt, szoftverek és -szolgáltatások, amelyek lehetővé teszik a beszerzési vezetők körül hello world toomaximize a vállalatok számára műveleteit, azokat a stratégiákat és pénzügyi teljesítményének gyakorolt hatásának.</span><span class="sxs-lookup"><span data-stu-id="ee478-105">GEP delivers software and services that enable procurement leaders around hello world toomaximize their impact on their businesses’ operations, strategies, and financial performances.</span></span> <span data-ttu-id="ee478-106">Továbbá tooconsulting és felügyelt szolgáltatásokat hello vállalati INTELLIGENS GEP®, egy felhőalapú, átfogó beszerzési szoftver platform által kínál.</span><span class="sxs-lookup"><span data-stu-id="ee478-106">In addition tooconsulting and managed services, hello company offers SMART by GEP®, a cloud-based, comprehensive procurement-software platform.</span></span> <span data-ttu-id="ee478-107">Azonban GEP próbált toosupport megoldások, például saját GEP által INTELLIGENS a helyszíni adatközpontját korlátozásai miatt: szükséges hello beruházások voltak elsajátíthatják a használatát, és más országokban szabályozási követelmények volna végzett hello szükséges beruházásokat több ijesztőnek továbbra is.</span><span class="sxs-lookup"><span data-stu-id="ee478-107">However, GEP ran into limitations trying toosupport solutions like SMART by GEP with its own on-premises datacenters: hello investments needed were steep and regulatory requirements in other countries would have made hello necessary investments more daunting still.</span></span> <span data-ttu-id="ee478-108">Áthelyezése toohello felhő által GEP informatikai erőforrások felszabadult kisebb kapcsolatos IT-üzemeltetők és toofocus több érték új források fejlesztésével az ügyfelek a hello földgömb között, amely lehetővé teszi, hogy informatikai tooworry.</span><span class="sxs-lookup"><span data-stu-id="ee478-108">By moving toohello cloud, GEP has freed up IT resources, allowing it tooworry less about IT operations and toofocus more on developing new sources of value for its customers across hello globe.</span></span>

## <a name="expanding-services-and-growth-by-using-azure"></a><span data-ttu-id="ee478-109">Bővülő szolgáltatások és növekedés Azure használatával</span><span class="sxs-lookup"><span data-stu-id="ee478-109">Expanding services and growth by using Azure</span></span>
<span data-ttu-id="ee478-110">INTELLIGENS GEP ügyfelei által kedvelt hello platform funkciók és könnyen használható; az ügyfelek kezelheti a folyamatok bárhonnan, bármikor, bármilyen készülékről – hordozható számítógép, táblagép vagy telefon.</span><span class="sxs-lookup"><span data-stu-id="ee478-110">SMART by GEP customers love hello platform’s features and ease of use; customers can manage their processes from anywhere, at any time, and on any device—laptop, tablet, or phone.</span></span> <span data-ttu-id="ee478-111">TooMicrosoft Azure mozgatásával GEP rendelkezik képes tooaccommodate lett, a gyors növekedést és a potenciális tooexpand új piacokra.</span><span class="sxs-lookup"><span data-stu-id="ee478-111">By moving tooMicrosoft Azure, GEP has been able tooaccommodate its rapid growth and its potential tooexpand into new markets.</span></span> <span data-ttu-id="ee478-112">Függően tooGEP VP of Technology, Dhananjay Nagalkar "Microsoft Azure már játszott kulcsfontosságú szerepet a GEP meg sikeres tételével velünk toorapidly méretezési szolgáltatások az agilitást és, hogy segítsen igényeinek hello szabályozó a globális regionális adatközpontok megadásával az ügyfeleknek."</span><span class="sxs-lookup"><span data-stu-id="ee478-112">According tooGEP’s VP of Technology, Dhananjay Nagalkar, “Microsoft Azure has played a key role in GEP’s success by allowing us toorapidly scale services with agility, and by providing regional datacenters that help us meet hello regulatory needs of our global customers.”</span></span>

## <a name="hello-limitations-of-a-do-it-yourself-datacenter"></a><span data-ttu-id="ee478-113">a saját munka datacenter hello korlátozásai</span><span class="sxs-lookup"><span data-stu-id="ee478-113">hello limitations of a do-it-yourself datacenter</span></span>
<span data-ttu-id="ee478-114">2013-ban GEP vezetők felismeri, hogy azok szükség egy módon tooensure méretezhetőséget és teljesítményt nyújt, azok nőtt az ügyfelek alap.</span><span class="sxs-lookup"><span data-stu-id="ee478-114">In 2013, GEP leaders recognized that they needed a way tooensure scalability and performance as they grew their customer base.</span></span> <span data-ttu-id="ee478-115">Nagalkar ismertetése, ", amely a meglévő adatközpontjainkba igény, azt kellett volna tooexpand a infrastruktúra és az informatikai erőforrások jelentősen toomeet.</span><span class="sxs-lookup"><span data-stu-id="ee478-115">Nagalkar explained, “toomeet that demand with our existing datacenters, we would have had tooexpand our infrastructure and IT resources considerably.</span></span> <span data-ttu-id="ee478-116">hello szükségessége nélkül és az adott időkereten lett volna hatalmas."</span><span class="sxs-lookup"><span data-stu-id="ee478-116">hello investment and time frame for that would have been enormous.”</span></span> <span data-ttu-id="ee478-117">A helyszíni fizikai és virtuális gépek kiterjedt konfigurációs, felügyeleti, skálázás, biztonsági mentés és szükséges, amely a GEP akadályozó költség lett volna sebességgel javítását.</span><span class="sxs-lookup"><span data-stu-id="ee478-117">On-premises physical and virtual machines require extensive configuration, management, scaling, backup, and patching at a rate that would have been cost prohibitive for GEP.</span></span> <span data-ttu-id="ee478-118">A felhőalapú megoldások, a hello ugyanakkor, kínálnak az egyszerűség és kényelmi célokat szolgál, amelyek engedélyezve GEP toofocus további a fejlesztési kezelése helyett nagy- és növekvő – IT-üzemeltetők.</span><span class="sxs-lookup"><span data-stu-id="ee478-118">Cloud solutions, on hello other hand, offer simplicity and convenience that enabled GEP toofocus more on development instead of managing large—and growing—IT operations.</span></span> <span data-ttu-id="ee478-119">Nagalkar tudta, hogy GEP csökkentheti az infrastruktúra megvásárlásáról, beállítási és kezelési terhelés áttelepítése toohello felhő által.</span><span class="sxs-lookup"><span data-stu-id="ee478-119">Nagalkar knew that GEP could reduce its infrastructure purchasing, configuration, and management overhead by migrating toohello cloud.</span></span>

<span data-ttu-id="ee478-120">GEP egy módon tooovercome szabályozó akadályok, hogy az egyes globális piacokon kívül is szükséges.</span><span class="sxs-lookup"><span data-stu-id="ee478-120">GEP also needed a way tooovercome regulatory barriers that kept it out of some global markets.</span></span> <span data-ttu-id="ee478-121">Több GEP tartozó lehetséges Európai ügyfeleket előírásoknak való megfelelés igényelnének, mivel a helyi földrajzi régióban tárolt adatok.</span><span class="sxs-lookup"><span data-stu-id="ee478-121">For many of GEP’s potential European customers, regulatory compliance would require having data stored in their local geographic regions.</span></span> <span data-ttu-id="ee478-122">De nem lett volna a GEP toobuild ki több adatközpontot gyakorlati.</span><span class="sxs-lookup"><span data-stu-id="ee478-122">But it would not have been practical for GEP toobuild out multiple datacenters.</span></span> <span data-ttu-id="ee478-123">"Széles körű infrastrukturális beruházások értékét, és informatikai munka költségek volna kapcsolja a jelentős hatással toomargins" tooNagalkar alapján történik.</span><span class="sxs-lookup"><span data-stu-id="ee478-123">“Widespread infrastructure investments and IT labor costs would bring a significant impact toomargins,” according tooNagalkar.</span></span> <span data-ttu-id="ee478-124">"Ennek eredményeként, a rendszer ténylegesen kényszerített tooturn kötelező lehetséges ügyfelek a helyben tárolt adatok szükséges."</span><span class="sxs-lookup"><span data-stu-id="ee478-124">“As a result, we were actually forced tooturn away potential customers who required locally stored data.”</span></span>

<span data-ttu-id="ee478-125">Nagalkar tudta, hogy egy felhőalapú megoldás lehet az hello válasz toothis kapcsolatos dilemma megoldása.</span><span class="sxs-lookup"><span data-stu-id="ee478-125">Nagalkar knew that a cloud solution might be hello answer toothis dilemma.</span></span> <span data-ttu-id="ee478-126">GEP tooa felhőszolgáltatóként a globális jelenlét sikerült áthelyezni, ha ez sikerült jobban megfelelni az ügyfelei szabályozási és teljesítmény szükséges adatok szorosabb toohello ügyfelek fizikai helyen tárolja.</span><span class="sxs-lookup"><span data-stu-id="ee478-126">If GEP could move tooa cloud provider with a global presence, it could better meet its customers’ regulatory and performance needs by storing data closer toohello customers’ physical locations.</span></span>

## <a name="finding-smooth-skies-in-hello-cloud"></a><span data-ttu-id="ee478-127">Zökkenőmentes skies hello felhő keresése</span><span class="sxs-lookup"><span data-stu-id="ee478-127">Finding smooth skies in hello cloud</span></span>
<span data-ttu-id="ee478-128">Nagalkar és csapata felfedezte több felhőalapú lehetőség, de a legtöbb infrastruktúra,--szolgáltatás (IaaS)-alapú megoldások, amelyek lenne szükséges GEP tooinvest fokozottan az informatikai erőforrások tooconfigure és hello szolgáltatás kezelése.</span><span class="sxs-lookup"><span data-stu-id="ee478-128">Nagalkar and his team explored several cloud options, but most were infrastructure-as-a-service (IaaS)-based solutions that would have required GEP tooinvest heavily in IT resources tooconfigure and manage hello service.</span></span> <span data-ttu-id="ee478-129">hello Azure platform,--szolgáltatás (PaaS) megoldás toobe ki van kapcsolva sokkal jobban megfelelnek.</span><span class="sxs-lookup"><span data-stu-id="ee478-129">hello Azure platform-as-a-service (PaaS) solution turned out toobe a much better fit.</span></span>

<span data-ttu-id="ee478-130">"Az Azure-GEP nem szükséges az adatbázis felügyeleti, a virtuális gépekkel kapcsolatos konfigurációját, a javítás vagy más infrastruktúra-kezelési feladatok toodeal" közölt Nagalkar.</span><span class="sxs-lookup"><span data-stu-id="ee478-130">“With Azure, GEP doesn’t need toodeal with database management, virtual-machine configuration, patching, or other infrastructure-management tasks,” stated Nagalkar.</span></span> <span data-ttu-id="ee478-131">"Ehelyett összpontosíthatunk az erőforrások milyen műveleteket végezzük legjobb: kihasználva a beszerzési toowrite szoftver valóban letölti a eredmények adhatunk ügyfeleink szakértői."</span><span class="sxs-lookup"><span data-stu-id="ee478-131">“Instead, we can focus our resources on what we do best: leveraging our expertise in procurement toowrite software that truly delivers results for our customers.”</span></span> 

<span data-ttu-id="ee478-132">Valójában hello áthelyezés tooAzure engedélyezte GEP tooshrink az informatikai dolgozók nagyobb funkció egyidejű engedélyezése az ügyfelek közben.</span><span class="sxs-lookup"><span data-stu-id="ee478-132">In fact, hello move tooAzure has enabled GEP tooshrink its IT operations staff while simultaneously enabling greater functionality for customers.</span></span>

<span data-ttu-id="ee478-133">Ha kihasználja a Azure adatközpontjaiban hello földgömb között, GEP könnyen kiterjesztheti a reach Európában és Ázsiában között.</span><span class="sxs-lookup"><span data-stu-id="ee478-133">By taking advantage of Azure datacenters across hello globe, GEP can easily extend its reach across Europe and Asia.</span></span> <span data-ttu-id="ee478-134">Ezek globális adatközpontjai GEP tooscale az agilitást és toomeet ügyfelek igényeinek megfelelően, amely csökkenti a késést és szabályozási követelmények megfelel a helyben tárolt adatok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="ee478-134">Those global datacenters enable GEP tooscale with agility and toomeet customer needs for locally stored data that reduces latency and meets regulatory requirements.</span></span>

## <a name="smart-by-gep-architecture"></a><span data-ttu-id="ee478-135">INTELLIGENS által GEP architektúrája</span><span class="sxs-lookup"><span data-stu-id="ee478-135">SMART by GEP architecture</span></span>
<span data-ttu-id="ee478-136">GEP INTELLIGENS GEP által létrehozott hello szabad az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="ee478-136">GEP built SMART by GEP from hello ground up on Azure.</span></span> <span data-ttu-id="ee478-137">Egy kritikus kifejlesztésének GEP a hello nagyobb méretezhetőségét, kevesebb mint állásidővel, és csökkentheti a karbantartási költségek, hogy az Azure SQL Database képest toowhat GEP GEP sikerült el lehet érni, a helyszíni.</span><span class="sxs-lookup"><span data-stu-id="ee478-137">A critical motivation for GEP was hello greater scalability, less downtime, and reduced maintenance costs that GEP could experience with Azure SQL Database compared toowhat GEP could achieve on-premises.</span></span> <span data-ttu-id="ee478-138">Azonban toohello felhő át, ha GEP felderített az új fejlesztési lehetőségek hello felhőben, például a gyors prototípusának és lean mérnöki toobetter toocustomer kell válaszolnia.</span><span class="sxs-lookup"><span data-stu-id="ee478-138">However, once moved toohello cloud, GEP discovered new development opportunities in hello cloud, like rapid prototyping and lean engineering toobetter respond toocustomer needs.</span></span> <span data-ttu-id="ee478-139">Fejlesztés az Azure-ban segítségével GEP hello szoftverlicenc-fejfájást okozott saját helyszíni futtatható a fejlesztők a számítógépnél érheti el.</span><span class="sxs-lookup"><span data-stu-id="ee478-139">Developing in Azure let GEP do away with hello software licensing headaches that its developers could run into on-premises.</span></span> <span data-ttu-id="ee478-140">hello core az INTELLIGENS GEP által nem Azure SQL Database, noha GEP sok más Azure-szolgáltatások tooeasily használ, és gyorsan továbbra is tooimprove INTELLIGENS GEP által.</span><span class="sxs-lookup"><span data-stu-id="ee478-140">hello core of SMART by GEP is Azure SQL Database, though GEP uses many other Azure services tooeasily and rapidly continue tooimprove SMART by GEP.</span></span>

<span data-ttu-id="ee478-141">![INTELLIGENS által GEP architektúra](./media/sql-database-implementation-gep/figure1.png) 1. ábra.</span><span class="sxs-lookup"><span data-stu-id="ee478-141">![SMART by GEP Architecture](./media/sql-database-implementation-gep/figure1.png) Figure 1.</span></span> <span data-ttu-id="ee478-142">INTELLIGENS által GEP architektúrája</span><span class="sxs-lookup"><span data-stu-id="ee478-142">SMART by GEP architecture</span></span>

## <a name="structured-data"></a><span data-ttu-id="ee478-143">Strukturált adatok</span><span class="sxs-lookup"><span data-stu-id="ee478-143">Structured data</span></span>
<span data-ttu-id="ee478-144">Hello lelke hello INTELLIGENS GEP alkalmazás adott power hello vállalati beszerzési-kezelési megoldás olyan hello Azure SQL adatbázis-példány.</span><span class="sxs-lookup"><span data-stu-id="ee478-144">At hello heart of hello SMART by GEP application are hello Azure SQL Database instances that power hello enterprise procurement-management solution.</span></span> <span data-ttu-id="ee478-145">GEP által GEP INTELLIGENS fejthetők vissza, ha azt látott Azure SQL Database egy tökéletes igényei az architektúra, amelyet hello vállalati tooachieve hello legmagasabb szintű adatvédelem és toomeet lehetővé tenné a szabályozási követelmények szerinti.</span><span class="sxs-lookup"><span data-stu-id="ee478-145">When GEP engineered SMART by GEP, it saw Azure SQL Database as a perfect fit for its architecture, one that would enable hello company tooachieve hello highest degree of data protection and toomeet its regulatory requirements.</span></span> <span data-ttu-id="ee478-146">GEP teszi rétege, amely az Azure SQL Database nyújt, beleértve az adatvédelem hello használata:</span><span class="sxs-lookup"><span data-stu-id="ee478-146">GEP makes use of hello multiple layers of data protection that Azure SQL Database offers, including:</span></span>

* <span data-ttu-id="ee478-147">Átlátható adattitkosítás keresztül inaktív adatok titkosítása.</span><span class="sxs-lookup"><span data-stu-id="ee478-147">Encrypting data at rest through transparent data encryption.</span></span>
* <span data-ttu-id="ee478-148">A hitelesítés biztonságossá tétele az Azure SQL Database integrálja az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ee478-148">Securing authentication by integrating Azure SQL Database with Azure Active Directory.</span></span>
* <span data-ttu-id="ee478-149">Korlátozó hozzáférési tooan megfelelő adatok részhalmazát sorszintű biztonsággal rendelkező.</span><span class="sxs-lookup"><span data-stu-id="ee478-149">Limiting access tooan appropriate subset of data with Row-Level Security.</span></span>
* <span data-ttu-id="ee478-150">Házirendek valós idejű adatok maszkolása.</span><span class="sxs-lookup"><span data-stu-id="ee478-150">Masking data in real time through policies.</span></span>
* <span data-ttu-id="ee478-151">Adatbázis-események keresztül Azure SQL Database Auditing követi nyomon.</span><span class="sxs-lookup"><span data-stu-id="ee478-151">Tracking database events through Azure SQL Database Auditing.</span></span>

> <span data-ttu-id="ee478-152">"Is használhatók. Ezek közül az összes fő kód módosítása nélkül és minimális hatással van a teljesítményre," említett Nagalkar.</span><span class="sxs-lookup"><span data-stu-id="ee478-152">“We can use all of these options without making any major code changes and with minimal impact on performance,” said Nagalkar.</span></span>
> 
> 

<span data-ttu-id="ee478-153">Az Azure SQL Database segítségével GEP automatikusan nagyobb vész-helyreállítási lehetőségeket, mint az sikerült rendelkezik gazdaságosan fejthetők vissza a helyszíni tooAzure SQL Database beépített toohello hibatűrést funkciók miatt van.</span><span class="sxs-lookup"><span data-stu-id="ee478-153">By using Azure SQL Database, GEP automatically has greater disaster-recovery capabilities than it could have economically engineered on premises due toohello fault-tolerance features built in tooAzure SQL Database.</span></span> <span data-ttu-id="ee478-154">GEP hello aktív georeplikáció funkció az Azure SQL Database, a több aktív, olvasható és online másodlagos replika (az Always On rendelkezésre állási csoportok) alapján kialakulhat használ a különböző földrajzi régióban, tooform magas rendelkezésre állású párokat.</span><span class="sxs-lookup"><span data-stu-id="ee478-154">GEP uses hello active geo-replication capability in Azure SQL Database, coupled with multiple active, readable, and online secondary replicas (Always On Availability Groups) in different geographical regions, tooform high-availability pairs.</span></span> <span data-ttu-id="ee478-155">INTELLIGENS által GEP replikálása adatok régiók közötti azt jelenti, hogy, hogy a régió kiterjedő katasztrófa hello esetben GEP egyszerűen helyre lehet állítani egy olyan minimális helyreállítási-helyreállításipont-célkitűzés (RPO) és a helyreállítási idő célkitűzése (RTO) ügyféladatokat.</span><span class="sxs-lookup"><span data-stu-id="ee478-155">Replicating SMART by GEP data across regions means that, in hello case of a region-wide disaster, GEP can easily recover customer data with a minimum recovery-point objective (RPO) and recovery-time objective (RTO).</span></span>

<span data-ttu-id="ee478-156">Minden INTELLIGENS GEP ügyfél két Azure SQL Database példánya van: egy online tranzakció-feldolgozási (OLTP), egy, az elemzés (például az ügyfél töltött és elemző jelentés).</span><span class="sxs-lookup"><span data-stu-id="ee478-156">Each SMART by GEP customer has two Azure SQL Database instances: one for online transaction processing (OLTP) and one for analysis (such as customer spend and report analysis).</span></span> <span data-ttu-id="ee478-157">Az Azure SQL Database rugalmas készletek engedélyezése GEP tooeasily kezelése akár több ezer adatbázis globálisan toohandle előre nem látható adatbázis-erőforrás iránti igények kielégítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="ee478-157">Azure SQL Database elastic pools enable GEP tooeasily manage thousands of databases globally toohandle unpredictable database-resource demands.</span></span> <span data-ttu-id="ee478-158">Rugalmas készletek azt, hogy felhasználói adatbázisokat is méretezhető szükség szerint túlzott kiosztása, vagy a-kiépítés, miközben is lehetővé teszi a GEP toocontrol költségek nélkül GEP tooensure adja meg.</span><span class="sxs-lookup"><span data-stu-id="ee478-158">Elastic pools provide a means for GEP tooensure that customer databases can scale as necessary, without over-provisioning or under-provisioning, while also allowing GEP toocontrol costs.</span></span> <span data-ttu-id="ee478-159">Ezenkívül, mivel ez egy PaaS szolgáltatás GEP összes hello új Azure SQL Database szolgáltatás az automatikus frissítések kap.</span><span class="sxs-lookup"><span data-stu-id="ee478-159">Moreover, because this is a PaaS service, GEP gets all hello new Azure SQL Database features with automatic upgrades.</span></span>

## <a name="unstructured-and-semi-structured-data"></a><span data-ttu-id="ee478-160">Strukturálatlan és félig strukturált adatok számára</span><span class="sxs-lookup"><span data-stu-id="ee478-160">Unstructured and semi-structured data</span></span>
<span data-ttu-id="ee478-161">Néhány INTELLIGENS által GEP ügyféladatok azonban kevésbé szigorúan strukturált tárolási kell.</span><span class="sxs-lookup"><span data-stu-id="ee478-161">However, some SMART by GEP customer data needs less-rigidly structured storage.</span></span> <span data-ttu-id="ee478-162">Az ilyen típusú adatok GEP Azure Blob storage, Azure Table Storage és Azure Redis Cache funkcióit használja.</span><span class="sxs-lookup"><span data-stu-id="ee478-162">For this type of data, GEP employs Azure Blob storage, Azure Table Storage, and Azure Redis Cache.</span></span> <span data-ttu-id="ee478-163">Az Azure Blob storage Kezelőkód bármilyen tetszés szerinti INTELLIGENS GEP felhasználók töltse fel hello alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="ee478-163">Azure Blob storage houses any attachments that SMART by GEP users upload into hello application.</span></span> <span data-ttu-id="ee478-164">Akkor is ha INTELLIGENS GEP tárolók statikus tartalom, például egymásra épülő stíluslapok (CSS) és a JavaScript-fájlt.</span><span class="sxs-lookup"><span data-stu-id="ee478-164">It is also where SMART by GEP stores static content, such as cascading style sheets (CSS) and JavaScript files.</span></span>

<span data-ttu-id="ee478-165">GEP GEP biztosít Azure Table Storage-ban tárolja a nem ügyfélkapcsolati adatok, például INTELLIGENS által GEP naplóadatokat, gyakorlatilag korlátlan költséghatékony tárolását és gyors lekérés alkalommal anélkül, hogy a séma hello adatok beállításával kapcsolatos tooworry.</span><span class="sxs-lookup"><span data-stu-id="ee478-165">GEP stores non-customer-facing data, like SMART by GEP log data, in Azure Table Storage, which gives GEP effectively unlimited cost-efficient storage and fast retrieval times without having tooworry about setting up a schema for hello data.</span></span> <span data-ttu-id="ee478-166">GEP Azure Redis Cache fő gyorsítótárat használ.</span><span class="sxs-lookup"><span data-stu-id="ee478-166">GEP uses Azure Redis Cache for a master cache.</span></span>

## <a name="authentication-and-routing"></a><span data-ttu-id="ee478-167">Hitelesítés és az Útválasztás</span><span class="sxs-lookup"><span data-stu-id="ee478-167">Authentication and routing</span></span>
<span data-ttu-id="ee478-168">Az Azure Access Control Service (ACS) biztosít INTELLIGENS GEP felhasználók különböző beállítások hello szoftver bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="ee478-168">Azure Access Control Service (ACS) provides SMART by GEP users with a wide variety of options for signing into hello software.</span></span> <span data-ttu-id="ee478-169">Az Azure ACS összevonni bármely identitásszolgáltató, amelyek használatával a Security Assertion Markup Language (SAML), például az Active Directory tartományi szolgáltatásokban, Ping identitás, OneLogin vagy SiteMinder hitelesítési adatokat is.</span><span class="sxs-lookup"><span data-stu-id="ee478-169">Azure ACS can federate with any identity provider that exchanges authentication data using Security Assertion Markup Language (SAML), such as Active Directory Domain Services, Ping Identity, OneLogin, or SiteMinder.</span></span> <span data-ttu-id="ee478-170">Ezzel a megoldással valósítja meg az egyszeri bejelentkezés (SSO) GEP ügyfelek számára anélkül, hogy felhasználói hitelesítő adatok tárolása és ügyfél-jelszó házirendek karbantartására.</span><span class="sxs-lookup"><span data-stu-id="ee478-170">This helps GEP implement single sign-on (SSO) for customers without worrying about storing user credentials and maintaining customer-password policies.</span></span>

<span data-ttu-id="ee478-171">Miután bejelentkezett, javítsa ki az INTELLIGENS GEP által az üzleti erőforrások hozzáférés toohello rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="ee478-171">Once signed in, customers have access toohello correct business resources in SMART by GEP.</span></span> <span data-ttu-id="ee478-172">GEP tooredirect és a terheléselosztás ügyfelek mobileszközök és a böngésző-munkamenetek érkező kérelmek Azure Traffic Manager használja.</span><span class="sxs-lookup"><span data-stu-id="ee478-172">GEP uses Azure Traffic Manager tooredirect and load-balance requests coming from customers’ mobile devices and browser sessions.</span></span>

## <a name="other-azure-services"></a><span data-ttu-id="ee478-173">Más Azure-szolgáltatásokkal</span><span class="sxs-lookup"><span data-stu-id="ee478-173">Other Azure services</span></span>
<span data-ttu-id="ee478-174">GEP más Azure-szolgáltatások toomake INTELLIGENS GEP rugalmas toocustomer igényeitől számos funkcióit használja.</span><span class="sxs-lookup"><span data-stu-id="ee478-174">GEP employs a number of other Azure services toomake SMART by GEP responsive toocustomer needs.</span></span> <span data-ttu-id="ee478-175">GEP használja az Azure cloud services (a webes és feldolgozói szerepkörök) toohost alkalmazás-bemutató, és hello védett logikájának szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="ee478-175">GEP uses Azure cloud services (both web and worker roles) toohost application-presentation and hello secured business-logic services.</span></span> <span data-ttu-id="ee478-176">Cloud services csomag teszi a fejlesztők toomanage infrastruktúra, a helyszíni adatközpontokkal szükség hello idő töredéke alatt GEP alkalmazások INTELLIGENS kódot (IAC) és új toodeploy – a bármely beavatkozás nélkül informatikai.</span><span class="sxs-lookup"><span data-stu-id="ee478-176">Cloud services make it possible for developers toomanage infrastructure as code (IAC) and toodeploy new SMART by GEP applications in a fraction of hello time that it required with on-premises datacenters—all without any involvement from IT.</span></span> <span data-ttu-id="ee478-177">GEP a fejlesztők a hello cloud-szolgáltatások átmeneti környezet tootest új feloldja a hello aktuális éles környezet befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="ee478-177">GEP developers can use hello cloud-services staging environment tootest new releases without impacting hello current production deployment.</span></span> <span data-ttu-id="ee478-178">Tesztel, miután GEP használ hello VIP swap szolgáltatások az Azure felhőalapú szolgáltatások toomove hello átmeneti kód toohello éles tárolóhelyre egy percen belül csökkentve a központi telepítés állásidő.</span><span class="sxs-lookup"><span data-stu-id="ee478-178">Once tested, GEP uses hello VIP swap features of Azure cloud services toomove hello staging code toohello production slot within a minute, thus reducing deployment downtime.</span></span>

<span data-ttu-id="ee478-179">toolower alkalmazás késés, GEP hello Azure tartalom Delivery Network (CDN) tooput statikus tartalmat tárolja az Azure Blob storage (például CSS- és JavaScript) használja a peremhálózati kiszolgáló Bezárás toousers.</span><span class="sxs-lookup"><span data-stu-id="ee478-179">toolower application latency, GEP uses hello Azure Content Delivery Network (CDN) tooput static content stored in Azure Blob storage (such as CSS and JavaScript files) on edge servers close toousers.</span></span> <span data-ttu-id="ee478-180">GEP által használt Azure Service Bus toosupport alkalmazás-architektúra minták kezdve a közzétételi-feliratkozási toopartially parancs lekérdezés rugalmas elkülönítése (CQRS), és réteges architektúra laza kapcsoló és aszinkron kommunikációt tesznek lehetővé.</span><span class="sxs-lookup"><span data-stu-id="ee478-180">GEP uses Azure Service Bus toosupport application-architecture patterns ranging from publish-subscribe toopartially Command Query Responsive Segregation (CQRS) and layered architecture with loose coupling and asynchronous communication.</span></span> <span data-ttu-id="ee478-181">GEP az Azure Media Services tooimprove a ügyfél-támogatási szolgáltatását használja.</span><span class="sxs-lookup"><span data-stu-id="ee478-181">GEP uses Azure Media Services tooimprove its customer-support service.</span></span> <span data-ttu-id="ee478-182">GEP található, hogy azt könnyen feltehet videók felhasználói-támogatás az Azure Media Services.</span><span class="sxs-lookup"><span data-stu-id="ee478-182">GEP found that it could easily post user-support videos on Azure Media Services.</span></span> <span data-ttu-id="ee478-183">Videók válaszolja meg felhasználói gyakori kérdésekre, amely segít INTELLIGENS továbbfejlesztésében GEP felhasználói elégedettségének során néhány hello támogatási terhelés GEP tartozó ügyfél-támogatási csapatát ki.</span><span class="sxs-lookup"><span data-stu-id="ee478-183">These videos answer common user questions, which helps improve SMART by GEP user satisfaction while taking some of hello support load off of GEP’s customer-support staff.</span></span>

<span data-ttu-id="ee478-184">toosend hello tranzakciós e-mailek által GEP INTELLIGENS által naponta generált ezer, hello vállalkozás hello SendGrid .NET API toointegrate használja az Azure-ral.</span><span class="sxs-lookup"><span data-stu-id="ee478-184">toosend hello thousands of transactional emails generated on a daily basis by SMART by GEP, hello firm uses hello SendGrid .NET API toointegrate with Azure.</span></span> <span data-ttu-id="ee478-185">A GEP fejlesztők számára, ez az egyszerű toodo – hello SendGrid bővítmény, az Azure hello Azure piactéren elérhető közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="ee478-185">For GEP developers, this is easy toodo—hello SendGrid add-on for Azure is available right in hello Azure Marketplace.</span></span> <span data-ttu-id="ee478-186">GEP fejlesztők tudta konfigurálni az INTELLIGENS által GEP SendGrid NuGet csomag jobb hello segítségével a Microsoft Visual Studio; GEP informatikai figyelheti hello szoftver SendGrid e-mail forgalom közvetlenül az Azure-ból.</span><span class="sxs-lookup"><span data-stu-id="ee478-186">GEP developers could configure SMART by GEP by using hello SendGrid NuGet package right in Microsoft Visual Studio; GEP IT can monitor hello software’s SendGrid email traffic directly from Azure.</span></span>

<span data-ttu-id="ee478-187">Végül INTELLIGENS GEP által használ Azure virtuális gépek – hello Azure IaaS szolgáltatásába – toohost alkalmazásokhoz és szolgáltatásokhoz nem hajtott végre a következő értelemben hello mérnöki, szoftverek,--szolgáltatás (SaaS) vagy PaaS-megoldásokkal együtt tooreplace idején.</span><span class="sxs-lookup"><span data-stu-id="ee478-187">Finally, SMART by GEP uses Azure Virtual Machines—hello Azure IaaS service—toohost applications and services that did not make sense, at hello time of engineering, tooreplace with software-as-a-service (SaaS) or PaaS solutions.</span></span> <span data-ttu-id="ee478-188">Például GEP futtatja-e virtuális gépek API integrációs szolgáltatásokat az ügyfelek üzleti vállalatközi (B2B) integrálva a helyszíni vállalati erőforrás-tervező (ERP) rendszereket például SAP, Oracle, PeopleSoft, JD Edwards, Microsoft Dynamics GP, és Lawson és az ügyfél SaaS megoldásokkal tooefficiently exchange beszerzési dokumentumok, például számlák.</span><span class="sxs-lookup"><span data-stu-id="ee478-188">For example, GEP hosts integration API services in virtual machines for business-to-business (B2B) integration with customers’ on-premises enterprise-resource-planning (ERP) systems like SAP, Oracle, PeopleSoft, JD Edwards, Microsoft Dynamics GP, and Lawson and with customer SaaS solutions tooefficiently exchange procurement documents, such as invoices.</span></span>

> <span data-ttu-id="ee478-189">"Által hello Microsoft Azure felhőben GEP INTELLIGENS épület teljesen eltávolított hello szükség helyszíni informatikai, nem csupán a is a felhasználóink beszerzési műveletek GEP."</span><span class="sxs-lookup"><span data-stu-id="ee478-189">“Building SMART by GEP in hello Microsoft Azure cloud has completely removed hello need for on-premises IT, not only for GEP but also for our customers’ procurement operations.”</span></span> 
> 
> <span data-ttu-id="ee478-190">– Dhananjay Nagalkar, VP technológia megoldások</span><span class="sxs-lookup"><span data-stu-id="ee478-190">— Dhananjay Nagalkar, VP of Technology Solutions</span></span>
> 
> 

## <a name="expand-customer-satisfaction-without-expanding-it"></a><span data-ttu-id="ee478-191">Bontsa ki az ügyfelek elégedettségének kiterjesztése nélkül informatikai</span><span class="sxs-lookup"><span data-stu-id="ee478-191">Expand customer satisfaction without expanding IT</span></span>
<span data-ttu-id="ee478-192">A helyszíni adatközpontját tooAzure áttelepítése, és teljesen új által GEP INTELLIGENS kialakításának hello Azure platformon, mert GEP nőtt méretezhetőséget és a rugalmasságot az infrastruktúra és az informatikai munkatársak tooexpand nélkül.</span><span class="sxs-lookup"><span data-stu-id="ee478-192">Since migrating from on-premises datacenters tooAzure, and building SMART by GEP from scratch on hello Azure platform, GEP has increased scalability and flexibility without having tooexpand its infrastructure or IT staff.</span></span> <span data-ttu-id="ee478-193">Valójában hello vállalati informatikai erőforrások még nem szerepel négy évnél hosszabb.</span><span class="sxs-lookup"><span data-stu-id="ee478-193">In fact, hello company hasn’t added IT resources in more than four years.</span></span> <span data-ttu-id="ee478-194">hello kényelmes PaaS modell Azure engedélyezte GEP tooreduce a szállító támogatása és az operations management beszállítói költségeit.</span><span class="sxs-lookup"><span data-stu-id="ee478-194">hello convenient PaaS model of Azure has enabled GEP tooreduce its spending on vendor support and operations management.</span></span> <span data-ttu-id="ee478-195">Ennek eredményeképpen GEP lett képes fókusz erőforrások szoftverfejlesztői; hello felhőben fejlesztése lehetővé teszi, hogy GEP fejlesztők tooquickly teszt új ötleteket toospend idő összehangolja és informatikai vagy aggódni helyszíni licencelőírások.</span><span class="sxs-lookup"><span data-stu-id="ee478-195">As a result, GEP has been able focus resources on software development; and developing in hello cloud enables GEP developers tooquickly test new ideas without having toospend time coordinating with IT or worrying about on-premises licensing requirements.</span></span> <span data-ttu-id="ee478-196">Az Azure SQL Database segítségével jobban győződjön meg arról, hogy az ügyfelek mindig kivételes szolgáltatás- és GEP.</span><span class="sxs-lookup"><span data-stu-id="ee478-196">Azure SQL Database helps GEP better ensure that its customers always have exceptional service and performance.</span></span>

## <a name="more-information"></a><span data-ttu-id="ee478-197">További információ</span><span class="sxs-lookup"><span data-stu-id="ee478-197">More information</span></span>
* <span data-ttu-id="ee478-198">GEP kezdőlap: [GEP](http://www.gep.com)</span><span class="sxs-lookup"><span data-stu-id="ee478-198">GEP home page: [GEP](http://www.gep.com)</span></span>
* <span data-ttu-id="ee478-199">Intelligens által GEP: [intelligens GEP által](http://www.smartbygep.com)</span><span class="sxs-lookup"><span data-stu-id="ee478-199">Smart by GEP: [Smart by GEP](http://www.smartbygep.com)</span></span>

## <a name="gep-contributors"></a><span data-ttu-id="ee478-200">GEP közreműködők</span><span class="sxs-lookup"><span data-stu-id="ee478-200">GEP contributors</span></span>
* <span data-ttu-id="ee478-201">Huzaifa Matawala társítása igazgató – felelős mérnök, GEP</span><span class="sxs-lookup"><span data-stu-id="ee478-201">Huzaifa Matawala, Associate Director—Architect, GEP</span></span>
* <span data-ttu-id="ee478-202">Sathyan Narasingh, műszaki osztály Manager GEP</span><span class="sxs-lookup"><span data-stu-id="ee478-202">Sathyan Narasingh, Engineering Manager, GEP</span></span>
* <span data-ttu-id="ee478-203">Deepa Velukutty, adatbázis rendszerfejlesztők GEP</span><span class="sxs-lookup"><span data-stu-id="ee478-203">Deepa Velukutty, Database Architect, GEP</span></span>
