---
title: "aaaMigrate az adatok tooSQL adatraktár |} Microsoft Docs"
description: "Tippek az adatok tooAzure SQL Data Warehouse adattárházzal történő, megoldások áttelepítéséhez."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: d78f954a-f54c-4aa4-9040-919bc6414887
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/29/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: fe4c6b7e82094c59c45e06be6da225fee1b707ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-data"></a><span data-ttu-id="bc630-103">Adatok áttelepítése</span><span class="sxs-lookup"><span data-stu-id="bc630-103">Migrate Your Data</span></span>
<span data-ttu-id="bc630-104">Adatok az SQL Data Warehouse egy különböző eszközeivel mozgathatók különböző forrásokból származó.</span><span class="sxs-lookup"><span data-stu-id="bc630-104">Data can be moved from different sources into your SQL Data Warehouse with a variety tools.</span></span>  <span data-ttu-id="bc630-105">ADF másolás, SSIS és bcp lehet az összes használt tooachieve a cél.</span><span class="sxs-lookup"><span data-stu-id="bc630-105">ADF Copy, SSIS, and bcp can all be used tooachieve this goal.</span></span> <span data-ttu-id="bc630-106">Azonban, ha hello adatok mennyisége növekszik gondolja bontásához hello adatok áttelepítési folyamat azokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="bc630-106">However, as hello amount of data increases you should think about breaking down hello data migration process into steps.</span></span> <span data-ttu-id="bc630-107">Ez számára biztosítja, hogy hello lehetőség toooptimize egyes lépések a teljesítmény és a rugalmasság tooensure zökkenőmentes adatáttelepítés.</span><span class="sxs-lookup"><span data-stu-id="bc630-107">This affords you hello opportunity toooptimize each step both for performance and for resilience tooensure a smooth data migration.</span></span>

<span data-ttu-id="bc630-108">A cikk ismerteti a hello egyszerű áttelepítési forgatókönyvek ADF másolása, SSIS és bcp először.</span><span class="sxs-lookup"><span data-stu-id="bc630-108">This article first discusses hello simple migration scenarios of ADF Copy, SSIS, and bcp.</span></span> <span data-ttu-id="bc630-109">Ezután kissé mélyebben hogyan optimalizálható a hello áttelepítési megjelenését.</span><span class="sxs-lookup"><span data-stu-id="bc630-109">It then look a little deeper into how hello migration can be optimized.</span></span>

## <a name="azure-data-factory-adf-copy"></a><span data-ttu-id="bc630-110">Az Azure Data Factory (ADF) másolása</span><span class="sxs-lookup"><span data-stu-id="bc630-110">Azure Data Factory (ADF) copy</span></span>
<span data-ttu-id="bc630-111">[ADF másolási] [ ADF Copy] része [Azure Data Factory][Azure Data Factory].</span><span class="sxs-lookup"><span data-stu-id="bc630-111">[ADF Copy][ADF Copy] is part of [Azure Data Factory][Azure Data Factory].</span></span> <span data-ttu-id="bc630-112">Használhatja ADF másolási tooexport a várt a helyi tároló tooremote egybesimított fájlokba tartani az Azure blob Storage tárolóban, vagy közvetlenül az SQL Data Warehouse-adatok tooflat fájlokat.</span><span class="sxs-lookup"><span data-stu-id="bc630-112">You can use ADF Copy tooexport your data tooflat files residing on local storage, tooremote flat files held in Azure blob storage or directly into SQL Data Warehouse.</span></span>

<span data-ttu-id="bc630-113">Ha az adatok indul egybesimított fájlokba, majd tootransfer először azt tooAzure tárolási blob egy terheléselosztási kezdeményezése előtt azt az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bc630-113">If your data starts in flat files, then you will first need tootransfer it tooAzure storage blob before initiating a load it into SQL Data Warehouse.</span></span> <span data-ttu-id="bc630-114">Miután hello adatátvitel Azure blob Storage tárolóban történő toouse választhat [ADF másolási] [ ADF Copy] újra toopush hello adatokat az SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="bc630-114">Once hello data is transferred into Azure blob storage you can choose toouse [ADF Copy][ADF Copy] again toopush hello data into SQL Data Warehouse.</span></span>

<span data-ttu-id="bc630-115">A PolyBase is biztosít egy nagy teljesítményű beállítása hello adatok betöltésekor.</span><span class="sxs-lookup"><span data-stu-id="bc630-115">PolyBase also provides a high-performance option for loading hello data.</span></span> <span data-ttu-id="bc630-116">Azonban, hogy: a két eszköz helyett egy.</span><span class="sxs-lookup"><span data-stu-id="bc630-116">However, that does mean using two tools instead of one.</span></span> <span data-ttu-id="bc630-117">Ha hello legjobb teljesítmény érdekében szükséges, akkor a polybase szolgáltatást akkor használja.</span><span class="sxs-lookup"><span data-stu-id="bc630-117">If you need hello best performance then use PolyBase.</span></span> <span data-ttu-id="bc630-118">Ha azt szeretné, hogy egy eszköz-élmény (és hello adatok nem jelentős) ADF a válasz.</span><span class="sxs-lookup"><span data-stu-id="bc630-118">If you want a single tool experience (and hello data is not massive) then ADF is your answer.</span></span>


> 
> 

<span data-ttu-id="bc630-119">A következő cikk néhány nagy toohello keresztül központi [ADF minták][ADF samples].</span><span class="sxs-lookup"><span data-stu-id="bc630-119">Head over toohello following article for some great [ADF samples][ADF samples].</span></span>

## <a name="integration-services"></a><span data-ttu-id="bc630-120">Integrációs szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="bc630-120">Integration Services</span></span>
<span data-ttu-id="bc630-121">Integration Services (SSIS) egy hatékony és rugalmas bontsa ki az átalakítási és betöltési (ETL) eszközt, amely támogatja a bonyolult munkafolyamatok, adatok átalakítása és több adatok betöltését lehetőség.</span><span class="sxs-lookup"><span data-stu-id="bc630-121">Integration Services (SSIS) is a powerful and flexible Extract Transform and Load (ETL) tool that supports complex workflows, data transformation, and several data loading options.</span></span> <span data-ttu-id="bc630-122">SSIS toosimply átviteli adatok tooAzure használja vagy szélesebb körű áttelepítés részeként.</span><span class="sxs-lookup"><span data-stu-id="bc630-122">Use SSIS toosimply transfer data tooAzure or as part of a broader migration.</span></span>

> [!NOTE]
> <span data-ttu-id="bc630-123">SSIS exportálhatók tooUTF-8 nélkül hello bájt rendelés hello fájl be van jelölve.</span><span class="sxs-lookup"><span data-stu-id="bc630-123">SSIS can export tooUTF-8 without hello byte order mark in hello file.</span></span> <span data-ttu-id="bc630-124">Ez előbb használnia kell hello származtatott oszlop összetevő tooconvert hello karakteres adatokat tooconfigure hello adatok folyamata toouse hello 65001 UTF-8 kódlapján találhatók.</span><span class="sxs-lookup"><span data-stu-id="bc630-124">tooconfigure this you must first use hello derived column component tooconvert hello character data in hello data flow toouse hello 65001 UTF-8 code page.</span></span> <span data-ttu-id="bc630-125">Hello oszlopok konvertálás után írható hello adatok toohello egybesimított fájl cél adapter győződjön meg arról, hogy 65001-es is van kiválasztva hello kódlapjának hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="bc630-125">Once hello columns have been converted, write hello data toohello flat file destination adapter ensuring that 65001 has also been selected as hello code page for hello file.</span></span>
> 
> 

<span data-ttu-id="bc630-126">SSIS tooSQL adatraktár csatlakozik, mint amelyhez tooa SQL Server-telepítés csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="bc630-126">SSIS connects tooSQL Data Warehouse just as it would connect tooa SQL Server deployment.</span></span> <span data-ttu-id="bc630-127">Azonban a kapcsolatokat. használja az ADO.NET Csatlakozáskezelő toobe kell.</span><span class="sxs-lookup"><span data-stu-id="bc630-127">However, your connections will need toobe using an ADO.NET connection manager.</span></span> <span data-ttu-id="bc630-128">Meg is gondoskodunk a tooconfigure hello "Használata tömeges beszúrás eredményről" beállítás toomaximize átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="bc630-128">You should also take care tooconfigure hello "Use bulk insert when available" setting toomaximize throughput.</span></span> <span data-ttu-id="bc630-129">Tekintse meg a toohello [ADO.NET céladapter] [ ADO.NET destination adapter] cikk toolearn további információk a tulajdonság</span><span class="sxs-lookup"><span data-stu-id="bc630-129">Please refer toohello [ADO.NET destination adapter][ADO.NET destination adapter] article toolearn more about this property</span></span>

> [!NOTE]
> <span data-ttu-id="bc630-130">Nem támogatott a Kapcsolódás az SQL Data Warehouse tooAzure OLEDB használatával.</span><span class="sxs-lookup"><span data-stu-id="bc630-130">Connecting tooAzure SQL Data Warehouse by using OLEDB is not supported.</span></span>
> 
> 

<span data-ttu-id="bc630-131">Ezenkívül, mindig fennáll hello lehetséges, hogy a csomag sikertelen lehet toothrottling vagy hálózati problémák miatt.</span><span class="sxs-lookup"><span data-stu-id="bc630-131">In addition, there is always hello possibility that a package might fail due toothrottling or network issues.</span></span> <span data-ttu-id="bc630-132">Tervezési csomagok, így azok folytathatók hello pontján, hiba nélkül megismétlése működnek az elvégzett hello meghibásodása előtt.</span><span class="sxs-lookup"><span data-stu-id="bc630-132">Design packages so they can be resumed at hello point of failure, without redoing work that completed before hello failure.</span></span>

<span data-ttu-id="bc630-133">További információt talál részletes hello [SSIS-dokumentáció][SSIS documentation].</span><span class="sxs-lookup"><span data-stu-id="bc630-133">For more information consult hello [SSIS documentation][SSIS documentation].</span></span>

## <a name="bcp"></a><span data-ttu-id="bc630-134">bcp</span><span class="sxs-lookup"><span data-stu-id="bc630-134">bcp</span></span>
<span data-ttu-id="bc630-135">BCP parancssori segédprogram, amely a strukturálatlan fájl adatok importálásának és exportálásának szolgál.</span><span class="sxs-lookup"><span data-stu-id="bc630-135">bcp is a command-line utility that is designed for flat file data import and export.</span></span> <span data-ttu-id="bc630-136">Néhány átalakítási adatok exportálásakor akkor kerül sor.</span><span class="sxs-lookup"><span data-stu-id="bc630-136">Some transformation can take place during data export.</span></span> <span data-ttu-id="bc630-137">tooperform egyszerű átalakítások egy lekérdezés tooselect használja, és hello adatok.</span><span class="sxs-lookup"><span data-stu-id="bc630-137">tooperform simple transformations use a query tooselect and transform hello data.</span></span> <span data-ttu-id="bc630-138">Exportálása után hello egybesimított fájlokba majd töltődnek be közvetlenül a hello cél hello SQL Data Warehouse-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="bc630-138">Once exported, hello flat files can then be loaded directly into hello target hello SQL Data Warehouse database.</span></span>

> [!NOTE]
> <span data-ttu-id="bc630-139">Általában célszerű a jó ötlet tooencapsulate hello átalakítások során adatok exportálása hello forrásrendszer nézetben.</span><span class="sxs-lookup"><span data-stu-id="bc630-139">It is often a good idea tooencapsulate hello transformations used during data export in a view on hello source system.</span></span> <span data-ttu-id="bc630-140">Ez biztosítja, hogy hello logika megmarad hello folyamat, és ismételhető.</span><span class="sxs-lookup"><span data-stu-id="bc630-140">This ensures that hello logic is retained and hello process is repeatable.</span></span>
> 
> 

<span data-ttu-id="bc630-141">Bcp előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="bc630-141">Advantages of bcp are:</span></span>

* <span data-ttu-id="bc630-142">Egyszerű.</span><span class="sxs-lookup"><span data-stu-id="bc630-142">Simplicity.</span></span> <span data-ttu-id="bc630-143">egyszerű toobuild BCP parancsok és végrehajtása</span><span class="sxs-lookup"><span data-stu-id="bc630-143">bcp commands are simple toobuild and execute</span></span>
* <span data-ttu-id="bc630-144">Újra elindítható betöltési folyamatot.</span><span class="sxs-lookup"><span data-stu-id="bc630-144">Re-startable load process.</span></span> <span data-ttu-id="bc630-145">Egyszer exportált hello terhelés lehet tetszőleges számú alkalommal végre</span><span class="sxs-lookup"><span data-stu-id="bc630-145">Once exported hello load can be executed any number of times</span></span>

<span data-ttu-id="bc630-146">A bcp korlátozásai a következők:</span><span class="sxs-lookup"><span data-stu-id="bc630-146">Limitations of bcp are:</span></span>

* <span data-ttu-id="bc630-147">BCP egyszerű fájlok táblázatos működik.</span><span class="sxs-lookup"><span data-stu-id="bc630-147">bcp works with tabulated flat files only.</span></span> <span data-ttu-id="bc630-148">Nem működik az XML- vagy JSON például fájlok</span><span class="sxs-lookup"><span data-stu-id="bc630-148">It does not work with files such as xml or JSON</span></span>
* <span data-ttu-id="bc630-149">Adatok átalakítása képességek csak korlátozott toohello exportálási előkészítés, és egyszerű jellegű</span><span class="sxs-lookup"><span data-stu-id="bc630-149">Data transformation capabilities are limited toohello export stage only and are simple in nature</span></span>
* <span data-ttu-id="bc630-150">BCP nem lett igazítani toobe robusztus, amikor az adatok betöltésekor keresztül hello internet.</span><span class="sxs-lookup"><span data-stu-id="bc630-150">bcp has not been adapted toobe robust when loading data over hello internet.</span></span> <span data-ttu-id="bc630-151">Minden hálózati instabilitás okozhat a betöltési hiba történt.</span><span class="sxs-lookup"><span data-stu-id="bc630-151">Any network instability may cause a load error.</span></span>
* <span data-ttu-id="bc630-152">BCP megtalálhatók-e a hello cél adatbázis előzetes toohello terhelés hello séma támaszkodik.</span><span class="sxs-lookup"><span data-stu-id="bc630-152">bcp relies on hello schema being present in hello target database prior toohello load</span></span>

<span data-ttu-id="bc630-153">További információkért lásd: [bcp tooload adatokat használja az SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="bc630-153">For more information, see [Use bcp tooload data into SQL Data Warehouse][Use bcp tooload data into SQL Data Warehouse].</span></span>

## <a name="optimizing-data-migration"></a><span data-ttu-id="bc630-154">Adatok áttelepítése optimalizálása</span><span class="sxs-lookup"><span data-stu-id="bc630-154">Optimizing data migration</span></span>
<span data-ttu-id="bc630-155">Egy SQLDW adatok áttelepítési folyamat hatékonyan sorolhatók három különálló lépésből áll:</span><span class="sxs-lookup"><span data-stu-id="bc630-155">A SQLDW data migration process can be effectively broken down into three discrete steps:</span></span>

1. <span data-ttu-id="bc630-156">A forrásadatok exportálása</span><span class="sxs-lookup"><span data-stu-id="bc630-156">Export of source data</span></span>
2. <span data-ttu-id="bc630-157">Adatok tooAzure átruházása</span><span class="sxs-lookup"><span data-stu-id="bc630-157">Transfer of data tooAzure</span></span>
3. <span data-ttu-id="bc630-158">Hello SQLDW céladatbázis betöltése</span><span class="sxs-lookup"><span data-stu-id="bc630-158">Load into hello target SQLDW database</span></span>

<span data-ttu-id="bc630-159">Az egyes lépések lehet külön-külön optimalizált toocreate egy robusztus, újra elindítható és rugalmas áttelepítési folyamat, amely a lehető legnagyobbra növeli a teljesítményt lépésről lépésre.</span><span class="sxs-lookup"><span data-stu-id="bc630-159">Each step can be individually optimized toocreate a robust, re-startable and resilient migration process that maximizes performance at each step.</span></span>

## <a name="optimizing-data-load"></a><span data-ttu-id="bc630-160">Optimalizálás adatok betöltése</span><span class="sxs-lookup"><span data-stu-id="bc630-160">Optimizing data load</span></span>
<span data-ttu-id="bc630-161">A következő fordított sorrendben egy pillanatra; keresése hello leggyorsabb módon tooload adata polybase.</span><span class="sxs-lookup"><span data-stu-id="bc630-161">Looking at these in reverse order for a moment; hello fastest way tooload data is via PolyBase.</span></span> <span data-ttu-id="bc630-162">PolyBase-betöltési folyamat optimalizálása helyezi Előfeltételek megelőző lépéseket, hogy ajánlott toounderstand ez hello társaságuk.</span><span class="sxs-lookup"><span data-stu-id="bc630-162">Optimizing for a PolyBase load process places prerequisites on hello preceding steps so it's best toounderstand this upfront.</span></span> <span data-ttu-id="bc630-163">Ezek a következők:</span><span class="sxs-lookup"><span data-stu-id="bc630-163">They are:</span></span>

1. <span data-ttu-id="bc630-164">Fájlok kódolása</span><span class="sxs-lookup"><span data-stu-id="bc630-164">Encoding of data files</span></span>
2. <span data-ttu-id="bc630-165">Az adatfájlok formátuma</span><span class="sxs-lookup"><span data-stu-id="bc630-165">Format of data files</span></span>
3. <span data-ttu-id="bc630-166">Adatforrás adatfájljainak helyét</span><span class="sxs-lookup"><span data-stu-id="bc630-166">Location of data files</span></span>

### <a name="encoding"></a><span data-ttu-id="bc630-167">Encoding</span><span class="sxs-lookup"><span data-stu-id="bc630-167">Encoding</span></span>
<span data-ttu-id="bc630-168">A PolyBase adatok fájlok toobe UTF-8 vagy UTF-16FE van szükség.</span><span class="sxs-lookup"><span data-stu-id="bc630-168">PolyBase requires data files toobe UTF-8 or UTF-16FE.</span></span> 



### <a name="format-of-data-files"></a><span data-ttu-id="bc630-169">Az adatfájlok formátuma</span><span class="sxs-lookup"><span data-stu-id="bc630-169">Format of data files</span></span>
<span data-ttu-id="bc630-170">A PolyBase csak egy rögzített méretű sor lezárójele \n vagy új sor.</span><span class="sxs-lookup"><span data-stu-id="bc630-170">PolyBase mandates a fixed row terminator of \n or newline.</span></span> <span data-ttu-id="bc630-171">Az adatfájlok meg kell felelnie toothis szabvány.</span><span class="sxs-lookup"><span data-stu-id="bc630-171">Your data files must conform toothis standard.</span></span> <span data-ttu-id="bc630-172">Nincs karakterlánc- vagy lezárói korlátozásait.</span><span class="sxs-lookup"><span data-stu-id="bc630-172">There aren't any restrictions on string or column terminators.</span></span>

<span data-ttu-id="bc630-173">Hogy toodefine minden egyes oszlophoz hello fájlban a PolyBase külső táblájában részeként.</span><span class="sxs-lookup"><span data-stu-id="bc630-173">You will have toodefine every column in hello file as part of your external table in PolyBase.</span></span> <span data-ttu-id="bc630-174">Győződjön meg arról, hogy minden exportált oszlopokra szükség, és hogy hello típusok megfelelnek-e a szükséges toohello szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="bc630-174">Make sure that all exported columns are required and that hello types conform toohello required standards.</span></span>

<span data-ttu-id="bc630-175">Tekintse meg ismét toohello [telepítse át a séma] cikk a támogatott adattípusokat részletek.</span><span class="sxs-lookup"><span data-stu-id="bc630-175">Please refer back toohello [migrate your schema] article for detail on supported data types.</span></span>

### <a name="location-of-data-files"></a><span data-ttu-id="bc630-176">Adatforrás adatfájljainak helyét</span><span class="sxs-lookup"><span data-stu-id="bc630-176">Location of data files</span></span>
<span data-ttu-id="bc630-177">Az SQL Data Warehouse PolyBase tooload adatokat az Azure Blob Storage kizárólag használja.</span><span class="sxs-lookup"><span data-stu-id="bc630-177">SQL Data Warehouse uses PolyBase tooload data from Azure Blob Storage exclusively.</span></span> <span data-ttu-id="bc630-178">Következésképpen hello adatokat kell először átadott a blob-tárolóba.</span><span class="sxs-lookup"><span data-stu-id="bc630-178">Consequently, hello data must have been first transferred into blob storage.</span></span>

## <a name="optimizing-data-transfer"></a><span data-ttu-id="bc630-179">Optimalizálás adatátvitel</span><span class="sxs-lookup"><span data-stu-id="bc630-179">Optimizing data transfer</span></span>
<span data-ttu-id="bc630-180">Adatok áttelepítése a leghosszabb részei hello egyik hello adatok tooAzure hello adatátvitelt.</span><span class="sxs-lookup"><span data-stu-id="bc630-180">One of hello slowest parts of data migration is hello transfer of hello data tooAzure.</span></span> <span data-ttu-id="bc630-181">Nem csak lehet hálózati sávszélesség problémát, de a hálózat megbízhatóságát is komolyan gátolja folyamatban van.</span><span class="sxs-lookup"><span data-stu-id="bc630-181">Not only can network bandwidth be an issue but also network reliability can seriously hamper progress.</span></span> <span data-ttu-id="bc630-182">Alapértelmezés szerint az adatok áttelepítése tooAzure internet így hello ésszerűen valószínűleg előforduló átviteli hibák előfordulási esélyét hello felett van.</span><span class="sxs-lookup"><span data-stu-id="bc630-182">By default migrating data tooAzure is over hello internet so hello chances of transfer errors occurring are reasonably likely.</span></span> <span data-ttu-id="bc630-183">Ezek a hibák azonban egészben vagy részben újra küldött adatok toobe lehet szükség.</span><span class="sxs-lookup"><span data-stu-id="bc630-183">However, these errors may require data toobe re-sent either in whole or in part.</span></span>

<span data-ttu-id="bc630-184">Szerencsére még több beállítások tooimprove hello sebesség és a rugalmasság, a folyamat során:</span><span class="sxs-lookup"><span data-stu-id="bc630-184">Fortunately you have several options tooimprove hello speed and resilience of this process:</span></span>

### <a name="expressrouteexpressroute"></a><span data-ttu-id="bc630-185">[ExpressRoute][ExpressRoute]</span><span class="sxs-lookup"><span data-stu-id="bc630-185">[ExpressRoute][ExpressRoute]</span></span>
<span data-ttu-id="bc630-186">Érdemes lehet tooconsider használatával [ExpressRoute] [ ExpressRoute] toospeed hello átviteli be.</span><span class="sxs-lookup"><span data-stu-id="bc630-186">You may want tooconsider using [ExpressRoute][ExpressRoute] toospeed up hello transfer.</span></span> <span data-ttu-id="bc630-187">[ExpressRoute] [ ExpressRoute] biztosít hozzá, és egy személyes kapcsolat tooAzure, hello kapcsolat nem halad át hello nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="bc630-187">[ExpressRoute][ExpressRoute] provides you with an established private connection tooAzure so hello connection does not go over hello public internet.</span></span> <span data-ttu-id="bc630-188">Ez egy kötelező lépés semmiképpen sem.</span><span class="sxs-lookup"><span data-stu-id="bc630-188">This is by no means a mandatory step.</span></span> <span data-ttu-id="bc630-189">Azonban azt fogja javítják a teljesítményt adatok tooAzure küldését egy helyszíni vagy a közös elhelyezés létesítmény.</span><span class="sxs-lookup"><span data-stu-id="bc630-189">However, it will improve throughput when pushing data tooAzure from an on-premises or co-location facility.</span></span>

<span data-ttu-id="bc630-190">hello használatának előnyei [ExpressRoute] [ ExpressRoute] vannak:</span><span class="sxs-lookup"><span data-stu-id="bc630-190">hello benefits of using [ExpressRoute][ExpressRoute] are:</span></span>

1. <span data-ttu-id="bc630-191">Nagyobb megbízhatóságot</span><span class="sxs-lookup"><span data-stu-id="bc630-191">Increased reliability</span></span>
2. <span data-ttu-id="bc630-192">Gyorsabb hálózati sebesség</span><span class="sxs-lookup"><span data-stu-id="bc630-192">Faster network speed</span></span>
3. <span data-ttu-id="bc630-193">Kisebb hálózati késés</span><span class="sxs-lookup"><span data-stu-id="bc630-193">Lower network latency</span></span>
4. <span data-ttu-id="bc630-194">magasabb szintű hálózati biztonság</span><span class="sxs-lookup"><span data-stu-id="bc630-194">higher network security</span></span>

<span data-ttu-id="bc630-195">[ExpressRoute] [ ExpressRoute] forgatókönyvek számos hasznos; nem csak az áttelepítési hello.</span><span class="sxs-lookup"><span data-stu-id="bc630-195">[ExpressRoute][ExpressRoute] is beneficial for a number of scenarios; not just hello migration.</span></span>

<span data-ttu-id="bc630-196">Felkeltettük az érdeklődését?</span><span class="sxs-lookup"><span data-stu-id="bc630-196">Interested?</span></span> <span data-ttu-id="bc630-197">További információt és az árképzés terén adja a Microsoft hello [ExpressRoute dokumentációja][ExpressRoute documentation].</span><span class="sxs-lookup"><span data-stu-id="bc630-197">For more information and pricing please visit hello [ExpressRoute documentation][ExpressRoute documentation].</span></span>

### <a name="azure-import-and-export-service"></a><span data-ttu-id="bc630-198">Az Azure Import és Export szolgáltatásról</span><span class="sxs-lookup"><span data-stu-id="bc630-198">Azure Import and Export Service</span></span>
<span data-ttu-id="bc630-199">hello Azure importálása és exportálása szolgáltatás rendkívül adatok átviteli folyamat nagy (GB ++) toomassive (TB ++) adatok továbbítása az Azure tervezték.</span><span class="sxs-lookup"><span data-stu-id="bc630-199">hello Azure Import and Export Service is a data transfer process designed for large (GB++) toomassive (TB++) transfers of data into Azure.</span></span> <span data-ttu-id="bc630-200">Az adatok toodisks írást, és a szállítási őket az Azure data center tooan magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="bc630-200">It involves writing your data toodisks and shipping them tooan Azure data center.</span></span> <span data-ttu-id="bc630-201">hello lemez tartalma majd betölti Azure Storage blobs szolgáltatásában az Ön nevében.</span><span class="sxs-lookup"><span data-stu-id="bc630-201">hello disk contents will then be loaded into Azure Storage Blobs on your behalf.</span></span>

<span data-ttu-id="bc630-202">Hello importálási exportálási folyamat áttekintése a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="bc630-202">A high-level view of hello import export process is as follows:</span></span>

1. <span data-ttu-id="bc630-203">Egy Azure Blob Storage tárolóban tooreceive hello adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc630-203">Configure an Azure Blob Storage container tooreceive hello data</span></span>
2. <span data-ttu-id="bc630-204">Az adattároló toolocal exportálása</span><span class="sxs-lookup"><span data-stu-id="bc630-204">Export your data toolocal storage</span></span>
3. <span data-ttu-id="bc630-205">Másolja a hello adatok too3.5 hüvelyk SATA II/III merevlemez-meghajtók [Azure Import/Export eszköz] hello használata</span><span class="sxs-lookup"><span data-stu-id="bc630-205">Copy hello data too3.5 inch SATA II/III hard disk drives using hello [Azure Import/Export Tool]</span></span>
4. <span data-ttu-id="bc630-206">Hello Azure Import és hello Adatbázisnapló-fájlok hello [Azure Import/Export eszköz] által visszaadott biztosító exportálása szolgáltatás importálási feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="bc630-206">Create an Import Job using hello Azure Import and Export Service providing hello journal files produced by hello [Azure Import/Export Tool]</span></span>
5. <span data-ttu-id="bc630-207">A kijelölt Azure-adatközpont küldje el a hello lemezek</span><span class="sxs-lookup"><span data-stu-id="bc630-207">Ship hello disks your nominated Azure data center</span></span>
6. <span data-ttu-id="bc630-208">Az adatok egy átvitt tooyour Azure Blob Storage-tároló</span><span class="sxs-lookup"><span data-stu-id="bc630-208">Your data is transferred tooyour Azure Blob Storage container</span></span>
7. <span data-ttu-id="bc630-209">Hello adatok betöltése a PolyBase használatával SQLDW</span><span class="sxs-lookup"><span data-stu-id="bc630-209">Load hello data into SQLDW using PolyBase</span></span>

### <a name="azcopyazcopy-utility"></a><span data-ttu-id="bc630-210">[AZCopy][AZCopy] segédprogram</span><span class="sxs-lookup"><span data-stu-id="bc630-210">[AZCopy][AZCopy] utility</span></span>
<span data-ttu-id="bc630-211">Hello [AZCopy][AZCopy] segédprogram egy remek eszköz az első Azure Storage Blobs átvihetők az adatokat.</span><span class="sxs-lookup"><span data-stu-id="bc630-211">hello [AZCopy][AZCopy] utility is a great tool for getting your data transferred into Azure Storage Blobs.</span></span> <span data-ttu-id="bc630-212">Kis (MB ++) toovery nagy (GB ++) adatátviteli tervezték.</span><span class="sxs-lookup"><span data-stu-id="bc630-212">It is designed for small (MB++) toovery large (GB++) data transfers.</span></span> <span data-ttu-id="bc630-213">[AZCopy] is kialakították tervezett tooprovide jó rugalmas átviteli, amikor adatokat tooAzure, ezért átvitele hello adatok átvitel lépés kiváló választás.</span><span class="sxs-lookup"><span data-stu-id="bc630-213">[AZCopy] has also been designed tooprovide good resilient throughput when transferring data tooAzure and so is a great choice for hello data transfer step.</span></span> <span data-ttu-id="bc630-214">Egyszer átvitt hello adatokkal a PolyBase használatával az SQL Data Warehouse betöltése.</span><span class="sxs-lookup"><span data-stu-id="bc630-214">Once transferred you can load hello data using PolyBase into SQL Data Warehouse.</span></span> <span data-ttu-id="bc630-215">AZCopy is beépítheti a SSIS-csomagok használata egy "Hajtható végre folyamat" feladatot.</span><span class="sxs-lookup"><span data-stu-id="bc630-215">You can also incorporate AZCopy into your SSIS packages using an "Execute Process" task.</span></span>

<span data-ttu-id="bc630-216">toouse AZCopy először fog toodownload kell, és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="bc630-216">toouse AZCopy you will first need toodownload and install it.</span></span> <span data-ttu-id="bc630-217">Van egy [éles verziójával] [ production version] és egy [előzetes verzióval] [ preview version] érhető el.</span><span class="sxs-lookup"><span data-stu-id="bc630-217">There is a [production version][production version] and a [preview version][preview version] available.</span></span>

<span data-ttu-id="bc630-218">tooupload egy fájlt a fájlrendszer alatt kell egy hello hasonló parancsot:</span><span class="sxs-lookup"><span data-stu-id="bc630-218">tooupload a file from your file system you will need a command like hello one below:</span></span>

```
AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:key /Pattern:abc.txt
```

<span data-ttu-id="bc630-219">Egy magas szintű folyamat összegzése lehet:</span><span class="sxs-lookup"><span data-stu-id="bc630-219">A high-level process summary could be:</span></span>

1. <span data-ttu-id="bc630-220">Egy Azure storage blob tároló tooreceive hello adatok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="bc630-220">Configure an Azure storage blob container tooreceive hello data</span></span>
2. <span data-ttu-id="bc630-221">Az adattároló toolocal exportálása</span><span class="sxs-lookup"><span data-stu-id="bc630-221">Export your data toolocal storage</span></span>
3. <span data-ttu-id="bc630-222">AZCopy hello Azure Blob Storage tárolóban adatait</span><span class="sxs-lookup"><span data-stu-id="bc630-222">AZCopy your data in hello Azure Blob Storage container</span></span>
4. <span data-ttu-id="bc630-223">Hello adatok betöltése az SQL Data Warehouse PolyBase használatával</span><span class="sxs-lookup"><span data-stu-id="bc630-223">Load hello data into SQL Data Warehouse using PolyBase</span></span>

<span data-ttu-id="bc630-224">Teljes rendelkezésre álló dokumentáció: [AZCopy][AZCopy].</span><span class="sxs-lookup"><span data-stu-id="bc630-224">Full documentation available: [AZCopy][AZCopy].</span></span>

## <a name="optimizing-data-export"></a><span data-ttu-id="bc630-225">Optimalizálás adatok exportálása</span><span class="sxs-lookup"><span data-stu-id="bc630-225">Optimizing data export</span></span>
<span data-ttu-id="bc630-226">Továbbá, hogy hello exportálási megfelel-e toohello követelményeinek megfelelnek a PolyBase által meg tooensuring toooptimize hello exportálási hello adatok tooimprove hello folyamat további is kérhet.</span><span class="sxs-lookup"><span data-stu-id="bc630-226">In addition tooensuring that hello export conforms toohello requirements laid out by PolyBase you can also seek toooptimize hello export of hello data tooimprove hello process further.</span></span>



### <a name="data-compression"></a><span data-ttu-id="bc630-227">Adattömörítés</span><span class="sxs-lookup"><span data-stu-id="bc630-227">Data compression</span></span>
<span data-ttu-id="bc630-228">A PolyBase gzip tömörített adatok olvashatók.</span><span class="sxs-lookup"><span data-stu-id="bc630-228">PolyBase can read gzip compressed data.</span></span> <span data-ttu-id="bc630-229">Ha tudja toocompress toogzip az adatfájlokat, akkor minimálisra csökkentheti hello adatmennyiség alatt leküldött hello hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="bc630-229">If you are able toocompress your data toogzip files then you will minimize hello amount of data being pushed over hello network.</span></span>

### <a name="multiple-files"></a><span data-ttu-id="bc630-230">Több fájl</span><span class="sxs-lookup"><span data-stu-id="bc630-230">Multiple files</span></span>
<span data-ttu-id="bc630-231">Több fájlokba nagy táblák összeállításának nem csak tooimprove sebesség exportálása, emellett segít az átviteli re-startability, és egyszer az hello Azure blob storage hello adatok teljes kezelhetőségi hello.</span><span class="sxs-lookup"><span data-stu-id="bc630-231">Breaking up large tables into several files not only helps tooimprove export speed, it also helps with transfer re-startability, and hello overall manageability of hello data once in hello Azure blob storage.</span></span> <span data-ttu-id="bc630-232">A PolyBase sok töltött szolgáltatását, hogy emellett a mappában lévő összes hello fájlok olvasása és egy táblázat azt tekinti hello egyikét.</span><span class="sxs-lookup"><span data-stu-id="bc630-232">One of hello many nice features of PolyBase is that it will read all hello files inside a folder and treat it as one table.</span></span> <span data-ttu-id="bc630-233">Éppen ezért egy jó ötlet tooisolate hello fájlok minden táblához saját mappába.</span><span class="sxs-lookup"><span data-stu-id="bc630-233">It is therefore a good idea tooisolate hello files for each table into its own folder.</span></span>

<span data-ttu-id="bc630-234">PolyBase az úgynevezett "rekurzív mappa átjárás" is támogatja.</span><span class="sxs-lookup"><span data-stu-id="bc630-234">PolyBase also supports a feature known as "recursive folder traversal".</span></span> <span data-ttu-id="bc630-235">A szolgáltatás használható toofurther javíthatja az exportált adatok tooimprove hello felépítése az adatok kezelése.</span><span class="sxs-lookup"><span data-stu-id="bc630-235">You can use this feature toofurther enhance hello organization of your exported data tooimprove your data management.</span></span>

<span data-ttu-id="bc630-236">toolearn PolyBase, az adatok betöltése kapcsolatos további információkért lásd: [az SQL Data Warehouse PolyBase használata tooload adatok][Use PolyBase tooload data into SQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="bc630-236">toolearn more about loading data with PolyBase, see [Use PolyBase tooload data into SQL Data Warehouse][Use PolyBase tooload data into SQL Data Warehouse].</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc630-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc630-237">Next steps</span></span>
<span data-ttu-id="bc630-238">Áttelepítési kapcsolatban bővebben lásd: [telepítse át a megoldás tooSQL adatraktár][Migrate your solution tooSQL Data Warehouse].</span><span class="sxs-lookup"><span data-stu-id="bc630-238">For more about migration, see [Migrate your solution tooSQL Data Warehouse][Migrate your solution tooSQL Data Warehouse].</span></span>
<span data-ttu-id="bc630-239">További fejlesztési tippek, lásd: [fejlesztői áttekintés][development overview].</span><span class="sxs-lookup"><span data-stu-id="bc630-239">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[AZCopy]:../storage/common/storage-use-azcopy.md
[ADF Copy]: ../data-factory/data-factory-data-movement-activities.md 
[ADF samples]: ../data-factory/data-factory-samples.md
[ADF Copy examples]: ../data-factory/data-factory-copy-activity-tutorial-using-visual-studio.md
[development overview]: sql-data-warehouse-overview-develop.md
[Migrate your solution tooSQL Data Warehouse]: sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse development overview]: sql-data-warehouse-overview-develop.md
[Use bcp tooload data into SQL Data Warehouse]: sql-data-warehouse-load-with-bcp.md
[Use PolyBase tooload data into SQL Data Warehouse]: sql-data-warehouse-get-started-load-with-polybase.md


<!--MSDN references-->

<!--Other Web references-->
[Azure Data Factory]: http://azure.microsoft.com/services/data-factory/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[ExpressRoute documentation]: http://azure.microsoft.com/documentation/services/expressroute/

[production version]: http://aka.ms/downloadazcopy/
[preview version]: http://aka.ms/downloadazcopypr/
[ADO.NET destination adapter]: https://msdn.microsoft.com/library/bb934041.aspx
[SSIS documentation]: https://msdn.microsoft.com/library/ms141026.aspx
