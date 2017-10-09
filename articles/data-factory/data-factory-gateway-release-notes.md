---
title: "Az adatkezelési átjáró aaaRelease megjegyzései |} Microsoft Docs"
description: "Adatok adatkezelési átjáró címe kibocsátási megjegyzései"
services: data-factory
author: nabhishek
manager: jhubbard
editor: monicar
ms.assetid: 14762e82-76d9-41c4-ba9f-14a54da29c36
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/19/2017
ms.author: abnarain
published: True
ms.openlocfilehash: 3165d7537410a0531e0bb7f7fe584767f9155574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="84685-103">Az adatkezelési átjáró kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="84685-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="84685-104">Modern Adatintegráció hello kapcsolatban felmerülő kihívások egyik toomove adatok tooand a helyszíni toocloud.</span><span class="sxs-lookup"><span data-stu-id="84685-104">One of hello challenges for modern data integration is toomove data tooand from on-premises toocloud.</span></span> <span data-ttu-id="84685-105">Adat-előállító lehetővé teszi, hogy ez az integráció az adatkezelési átjáró, amely egy olyan ügynök, hogy a helyszíni tooenable hibrid adatmozgás telepítheti.</span><span class="sxs-lookup"><span data-stu-id="84685-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises tooenable hybrid data movement.</span></span>

<span data-ttu-id="84685-106">Tekintse meg a következő cikkekben további információk az adatkezelési átjáró hello és hogyan toouse azt:</span><span class="sxs-lookup"><span data-stu-id="84685-106">See hello following articles for detailed information about Data Management Gateway and how toouse it:</span></span>

*  [<span data-ttu-id="84685-107">Adatkezelési átjáró</span><span class="sxs-lookup"><span data-stu-id="84685-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="84685-108">Adatok áthelyezése között a helyszíni és felhőalapú Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="84685-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="84685-109">AKTUÁLIS VERZIÓ (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="84685-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="84685-110">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-110">Enhancements-</span></span>
- <span data-ttu-id="84685-111">Is hozzáadhat engedélyezése helyett a DNS-bejegyzések toowhitelist szolgáltatásbusz összes Azure IP-címet a tűzfal (ha szükséges).</span><span class="sxs-lookup"><span data-stu-id="84685-111">You can add DNS entries toowhitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="84685-112">Azure-portál is megtalálhatja a megfelelő DNS-bejegyzés (Data Factory -> "Fejlesztés és üzembe helyezés" -> "Átjárók" -> "serviceUrls" (a JSON-ban)</span><span class="sxs-lookup"><span data-stu-id="84685-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="84685-113">HDFS-összekötő mostantól támogatja a nyilvános önaláírt tanúsítványt azáltal, hogy SSL-ellenőrzésének kihagyására.</span><span class="sxs-lookup"><span data-stu-id="84685-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="84685-114">Rögzített: Problémát átjáró kapcsolat nélküli (miatt tooclock döntés) frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="84685-114">Fixed: Issue with gateway offline during update (due tooclock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="84685-115">Korábbi verziói</span><span class="sxs-lookup"><span data-stu-id="84685-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="84685-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="84685-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="84685-117">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-117">Enhancements-</span></span>
-   <span data-ttu-id="84685-118">Is hozzáadhat engedélyezése helyett a DNS-bejegyzések toowhitelist Service Bus összes Azure IP-címet a tűzfal (ha szükséges).</span><span class="sxs-lookup"><span data-stu-id="84685-118">You can add DNS entries toowhitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="84685-119">Itt további részleteket.</span><span class="sxs-lookup"><span data-stu-id="84685-119">More details here.</span></span>
-   <span data-ttu-id="84685-120">Ekkor átmásolhatja too4.75 TB, amely az blokkblob hello maximális támogatott fájlméret fel egyetlen blokkblob vagy származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="84685-120">You can now copy data to/from a single block blob up too4.75 TB, which is hello max supported size of block blob.</span></span> <span data-ttu-id="84685-121">(a korábbi korlát 195 GB volt).</span><span class="sxs-lookup"><span data-stu-id="84685-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="84685-122">Rögzített: Kívüli memória probléma során több kisebb fájlt kicsomagolás másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="84685-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="84685-123">Rögzített: Indexe kívül tartomány probléma Document DB rendszerbe tooan másolása a helyszíni SQL Server idempotencia szolgáltatással.</span><span class="sxs-lookup"><span data-stu-id="84685-123">Fixed: Index out of range issue while copying from Document DB tooan on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="84685-124">Rögzített: SQL-karbantartási parancsprogramot nem működik a helyszíni SQL Server a másolása varázsló segítségével.</span><span class="sxs-lookup"><span data-stu-id="84685-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="84685-125">Rögzített: Hello végén területtel rendelkező oszlopnév nem működik a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="84685-125">Fixed: Column name with space at hello end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="84685-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="84685-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="84685-127">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-127">Enhancements-</span></span>
- <span data-ttu-id="84685-128">Rögzített: Probléma átjáró újraindítás hitelesítő adatok hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="84685-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="84685-129">Rögzített: Regisztráció során átjáró problémát állítsa vissza egy biztonságimásolat-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="84685-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="84685-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="84685-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="84685-131">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-131">Enhancements-</span></span>
- <span data-ttu-id="84685-132">Rögzített: Oracle forrásaként null értéke helytelen olvasható.</span><span class="sxs-lookup"><span data-stu-id="84685-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="84685-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="84685-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="84685-134">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="84685-134">What’s new</span></span>
- <span data-ttu-id="84685-135">Az ügyfelek visszajelzést adhat az átjáró regisztrálása a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="84685-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="84685-136">Egy új tömörítési formátum támogatja: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="84685-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="84685-137">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-137">Enhancements-</span></span>
- <span data-ttu-id="84685-138">A teljesítmény javítása a Oracle gyűjtése, HDFS forrás.</span><span class="sxs-lookup"><span data-stu-id="84685-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="84685-139">Az átjáró automatikus hibajavítás frissítéséhez átjáró párhuzamos feldolgozási kapacitás.</span><span class="sxs-lookup"><span data-stu-id="84685-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="84685-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="84685-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="84685-141">Fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-141">Enhancements</span></span>
- <span data-ttu-id="84685-142">Hatékonyabb és megbízhatóbb átjáró regisztrációs élmény-most követheti a folyamat állapota teszi gyorsabban élmény hello regisztrációs hello átjáró regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="84685-142">Improved and more robust Gateway registration experience- Now you can track progress status during hello Gateway registration process, which makes hello registration experience more responsive.</span></span>
- <span data-ttu-id="84685-143">Növekedés az átjáró visszaállítása folyamat -, de ilyenkor is helyreállíthatók az átjáró, még akkor is, ha nem rendelkezik hello átjáró biztonságimásolat-fájl ezzel a frissítéssel.</span><span class="sxs-lookup"><span data-stu-id="84685-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have hello gateway backup file with this update.</span></span> <span data-ttu-id="84685-144">Ez megköveteli tooreset csatolt szolgáltatás hitelesítő adatait a portálon.</span><span class="sxs-lookup"><span data-stu-id="84685-144">This would require you tooreset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="84685-145">Hiba a javítás.</span><span class="sxs-lookup"><span data-stu-id="84685-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="84685-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="84685-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="84685-147">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="84685-147">What’s new</span></span>

- <span data-ttu-id="84685-148">Most már az adatforrás hitelesítő adatainak helyileg tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="84685-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="84685-149">hello azonosító adatok titkosítása.</span><span class="sxs-lookup"><span data-stu-id="84685-149">hello credentials are encrypted.</span></span> <span data-ttu-id="84685-150">hello adatforrás hitelesítő adatai állíthatók helyre, és hello biztonságimásolat-fájl, amely a meglévő átjáró, minden helyszíni hello exportálható segítségével visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="84685-150">hello data source credentials can be recovered and restored using hello backup file that can be exported from hello existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="84685-151">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="84685-151">Enhancements-</span></span>

- <span data-ttu-id="84685-152">Hatékonyabb és megbízhatóbb átjáró regisztrációs felületet.</span><span class="sxs-lookup"><span data-stu-id="84685-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="84685-153">Az automatikus észlelés quotechar tulajdonság-konfiguráció támogatása másolása varázsló a szöveges formátumú, és javíthatja a hello teljes formázza az észlelési pontosságát.</span><span class="sxs-lookup"><span data-stu-id="84685-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve hello overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="84685-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="84685-154">2.3.6100.2</span></span>

- <span data-ttu-id="84685-155">Támogatja a helyszíni fájlrendszer és a HDFS firstRowAsHeader és SkipLineCount az automatikus észlelés szövegfájlok másolása varázsló.</span><span class="sxs-lookup"><span data-stu-id="84685-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="84685-156">Átjáró és a Service Bus közötti hálózati kapcsolat hello stabilitásának javítása</span><span class="sxs-lookup"><span data-stu-id="84685-156">Enhance hello stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="84685-157">Néhány hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="84685-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="84685-158">2.2.6072.1</span></span>

*  <span data-ttu-id="84685-159">Támogatja a HTTP-proxy hello átjáró konfigurációkezelőjének használatával hello átjáró beállítását.</span><span class="sxs-lookup"><span data-stu-id="84685-159">Supports setting HTTP proxy for hello gateway using hello Gateway Configuration Manager.</span></span> <span data-ttu-id="84685-160">Ha konfigurálva, Azure Blob, az Azure Table, az Azure Data Lake és a Document DB rendszerbe HTTP-proxyn keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="84685-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="84685-161">Támogatja a fejléc kezelése a szöveges, az adatok másolásakor / tooAzure Blob, az Azure Data Lake Store, helyszíni fájlrendszer és a helyszíni HDFS.</span><span class="sxs-lookup"><span data-stu-id="84685-161">Supports header handling for TextFormat when copying data from/tooAzure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="84685-162">Az adatok másolása hozzáfűző Blob és az oldalakra vonatkozó Blob együtt hello már támogatja a Blokkblob támogatott.</span><span class="sxs-lookup"><span data-stu-id="84685-162">Supports copying data from Append Blob and Page Blob along with hello already supported Block Blob.</span></span>
*  <span data-ttu-id="84685-163">Bevezet egy új az átjáró állapotának **Online (korlátozott)**, ami azt jelenti, hogy hello hello átjáró fő funkcióit működik, kivéve másolása varázsló hello interaktív művelet támogatása.</span><span class="sxs-lookup"><span data-stu-id="84685-163">Introduces a new gateway status **Online (Limited)**, which indicates that hello main functionality of hello gateway works except hello interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="84685-164">Fokozza a hello megbízhatóságát, átjáró regisztrálása regisztrációs kulccsal.</span><span class="sxs-lookup"><span data-stu-id="84685-164">Enhances hello robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="84685-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="84685-165">2.1.6040.</span></span>

*  <span data-ttu-id="84685-166">DB2 illesztőprogram most hello átjárótelepítési csomag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="84685-166">DB2 driver is included in hello gateway installation package now.</span></span> <span data-ttu-id="84685-167">Nem kell tooinstall, külön-külön.</span><span class="sxs-lookup"><span data-stu-id="84685-167">You do not need tooinstall it separately.</span></span>
*  <span data-ttu-id="84685-168">DB2-illesztőprogram mostantól támogatja a z/OS- és a DB2 i (AS / 400) együtt hello platformon már támogatott (Linux, Unix, és a Windows).</span><span class="sxs-lookup"><span data-stu-id="84685-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with hello platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="84685-169">A cél- és a helyszíni adattárolókhoz Azure Cosmos DB használatával támogatja</span><span class="sxs-lookup"><span data-stu-id="84685-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="84685-170">Hello együtt a/toocold/közbeni blob-tároló adatok másolása már támogatja az általános célú tárfiók támogatott.</span><span class="sxs-lookup"><span data-stu-id="84685-170">Supports copying data from/toocold/hot blob storage along with hello already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="84685-171">Lehetővé teszi a tooconnect tooon helyszíni SQL Server távoli bejelentkezési jogokkal átjárón keresztül.</span><span class="sxs-lookup"><span data-stu-id="84685-171">Allows you tooconnect tooon-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="84685-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="84685-172">2.0.6013.1</span></span>

*  <span data-ttu-id="84685-173">Hello nyelvi/kulturális környezet toobe manuális telepítése során egy átjáró által használt választhatja ki.</span><span class="sxs-lookup"><span data-stu-id="84685-173">You can select hello language/culture toobe used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="84685-174">Átjáró nem várt módon működik, beállíthatja a elmúlt hét napban tooMicrosoft toofacilitate hibaelhárítási hello probléma toosend átjáró naplói.</span><span class="sxs-lookup"><span data-stu-id="84685-174">When gateway does not work as expected, you can choose toosend gateway logs of last seven days tooMicrosoft toofacilitate troubleshooting of hello issue.</span></span> <span data-ttu-id="84685-175">Ha az átjáró nincs csatlakoztatva toohello felhőalapú szolgáltatás, toosave választhat, és archiválja az átjáró naplói.</span><span class="sxs-lookup"><span data-stu-id="84685-175">If gateway is not connected toohello cloud service, you can choose toosave and archive gateway logs.</span></span>  

*  <span data-ttu-id="84685-176">Felhasználói felület fejlesztései az átjáró a configuration manager:</span><span class="sxs-lookup"><span data-stu-id="84685-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="84685-177">Adja meg az átjáró állapotának több látható a hello Kezdőlap lap.</span><span class="sxs-lookup"><span data-stu-id="84685-177">Make gateway status more visible on hello Home tab.</span></span>

    *  <span data-ttu-id="84685-178">Átszervezett és egyszerűsített szabályozza.</span><span class="sxs-lookup"><span data-stu-id="84685-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="84685-179">Adatok másolása egy tárolóhely-hello [kódmentes másolási preview eszköz](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="84685-179">You can copy data from a storage using hello [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="84685-180">Lásd: [előkészített másolási](data-factory-copy-activity-performance.md#staged-copy) részleteiről a szolgáltatás általában.</span><span class="sxs-lookup"><span data-stu-id="84685-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="84685-181">Az adatkezelési átjáró tooingress adatok közvetlenül a helyi SQL Server adatbázis Azure Machine Learning is használhatja.</span><span class="sxs-lookup"><span data-stu-id="84685-181">You can use Data Management Gateway tooingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="84685-182">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-182">Performance improvements</span></span>

    * <span data-ttu-id="84685-183">Séma/Preview SQL-kiszolgálón a kódmentes másolási preview eszköz megjelenítéséről teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="84685-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="84685-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="84685-184">1.12.5953.1</span></span>

*  <span data-ttu-id="84685-185">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="84685-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="84685-186">1.11.5918.1</span></span>

*  <span data-ttu-id="84685-187">Hello adatátjáró eseménynaplójában maximális mérete 1 MB too40 MB nőtt.</span><span class="sxs-lookup"><span data-stu-id="84685-187">Maximum size of hello gateway event log has been increased from 1 MB too40 MB.</span></span>

*  <span data-ttu-id="84685-188">Egy figyelmeztető párbeszédpanel jelenik meg, abban az esetben, ha újraindítás szükséges átjáró automatikus frissítés során.</span><span class="sxs-lookup"><span data-stu-id="84685-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="84685-189">Kiválaszthatja a toorestart jobbra, majd vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="84685-189">You can choose toorestart right then or later.</span></span>

*  <span data-ttu-id="84685-190">Abban az esetben, ha az automatikus frissítés nem sikerül, a gateway installer háromszor a maximális automatikus frissítési újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="84685-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="84685-191">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-191">Performance improvements</span></span>

    * <span data-ttu-id="84685-192">Nagy táblák tölt be a helyi kiszolgálóról a kódmentes másolásának esetéhez teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="84685-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="84685-193">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="84685-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="84685-194">1.10.5892.1</span></span>

*  <span data-ttu-id="84685-195">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-195">Performance improvements</span></span>

*  <span data-ttu-id="84685-196">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="84685-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="84685-197">1.9.5865.2</span></span>

*  <span data-ttu-id="84685-198">Nulla touch automatikus frissítési funkció</span><span class="sxs-lookup"><span data-stu-id="84685-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="84685-199">Az átjáró állapotának mutatók új tálcaikon</span><span class="sxs-lookup"><span data-stu-id="84685-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="84685-200">Képes túl "frissítés most" hello ügyfélről</span><span class="sxs-lookup"><span data-stu-id="84685-200">Ability too“Update now” from hello client</span></span>
*  <span data-ttu-id="84685-201">Képes tooset frissítés időpontjának ütemezése</span><span class="sxs-lookup"><span data-stu-id="84685-201">Ability tooset update schedule time</span></span>
*  <span data-ttu-id="84685-202">PowerShell-parancsfájl az elvégezte az automatikus frissítés be-és kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="84685-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="84685-203">JSON-formátum támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-203">Support for JSON format</span></span>  
*  <span data-ttu-id="84685-204">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-204">Performance improvements</span></span>
*  <span data-ttu-id="84685-205">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="84685-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="84685-206">1.8.5822.1</span></span>

*  <span data-ttu-id="84685-207">Hibaelhárítási élmény javítása</span><span class="sxs-lookup"><span data-stu-id="84685-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="84685-208">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-208">Performance improvements</span></span>
*  <span data-ttu-id="84685-209">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="84685-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="84685-210">1.7.5795.1</span></span>

*  <span data-ttu-id="84685-211">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-211">Performance improvements</span></span>
*  <span data-ttu-id="84685-212">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="84685-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="84685-213">1.7.5764.1</span></span>

*  <span data-ttu-id="84685-214">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-214">Performance improvements</span></span>
*  <span data-ttu-id="84685-215">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="84685-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="84685-216">1.6.5735.1</span></span>

*  <span data-ttu-id="84685-217">Támogatja a helyszíni HDFS forrás/fogadó</span><span class="sxs-lookup"><span data-stu-id="84685-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="84685-218">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-218">Performance improvements</span></span>
*  <span data-ttu-id="84685-219">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="84685-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="84685-220">1.6.5696.1</span></span>

*  <span data-ttu-id="84685-221">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-221">Performance improvements</span></span>
*  <span data-ttu-id="84685-222">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="84685-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="84685-223">1.6.5676.1</span></span>

*  <span data-ttu-id="84685-224">Diagnosztikai eszközök a Configuration Manager támogatására</span><span class="sxs-lookup"><span data-stu-id="84685-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="84685-225">Az Azure Data Factory táblázatos adatforrások táblaoszlopok támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-226">Az Azure Data Factory SQL DW támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-227">Az Azure Data Factory Reclusive BlobSource és FileSource támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-228">BlobSink és FileSink bináris másolatot az Azure Data Factory CopyBehavior – mergefiles típusú PreserveHierarchy és FlattenHierarchy támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-229">Támogatja a másolási tevékenység során az Azure Data Factory jelentési</span><span class="sxs-lookup"><span data-stu-id="84685-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-230">Az Azure Data Factory támogatási adatok forrás kapcsolat érvényesítése</span><span class="sxs-lookup"><span data-stu-id="84685-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-231">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="84685-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="84685-232">1.6.5672.1</span></span>

*  <span data-ttu-id="84685-233">Az ODBC-adatforrás az Azure Data Factory támogatási tábla neve</span><span class="sxs-lookup"><span data-stu-id="84685-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-234">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-234">Performance improvements</span></span>
*  <span data-ttu-id="84685-235">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="84685-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="84685-236">1.6.5658.1</span></span>

*  <span data-ttu-id="84685-237">Támogatási fájlt az Azure Data Factory gyűjtése</span><span class="sxs-lookup"><span data-stu-id="84685-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-238">Hierarchia megőrzése bináris másolása az Azure Data Factory támogatási</span><span class="sxs-lookup"><span data-stu-id="84685-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-239">Az Azure Data Factory támogatja a másolási tevékenység idempotencia</span><span class="sxs-lookup"><span data-stu-id="84685-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-240">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="84685-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="84685-241">1.6.5640.1</span></span>

*  <span data-ttu-id="84685-242">3 további adatforrások támogatja az Azure Data Factory (ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="84685-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="84685-243">Az Azure Data Factory idézőjel, a fürt megosztott kötetei szolgáltatás elemző támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-244">Tömörítés támogatása (BZip2)</span><span class="sxs-lookup"><span data-stu-id="84685-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="84685-245">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="84685-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="84685-246">1.5.5612.1</span></span>

*  <span data-ttu-id="84685-247">Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata és Sybase) öt relációs adatbázisok támogatása</span><span class="sxs-lookup"><span data-stu-id="84685-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="84685-248">Tömörítés támogatása (Gzip és a Deflate)</span><span class="sxs-lookup"><span data-stu-id="84685-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="84685-249">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-249">Performance improvements</span></span>
*  <span data-ttu-id="84685-250">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="84685-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="84685-251">1.4.5549.1</span></span>

*  <span data-ttu-id="84685-252">Az Azure Data Factory Oracle data forrás támogatását</span><span class="sxs-lookup"><span data-stu-id="84685-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="84685-253">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="84685-253">Performance improvements</span></span>
*  <span data-ttu-id="84685-254">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="84685-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="84685-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="84685-255">1.4.5492.1</span></span>

*  <span data-ttu-id="84685-256">Egyesített bináris, amely támogatja a Microsoft Azure Data Factory és az Office 365 Power BI szolgáltatásokat</span><span class="sxs-lookup"><span data-stu-id="84685-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="84685-257">Pontosítsa a hello konfigurációs UI felületen és a regisztrációs folyamat</span><span class="sxs-lookup"><span data-stu-id="84685-257">Refine hello Configuration UI and registration process</span></span>
*  <span data-ttu-id="84685-258">Az Azure Data Factory – Azure bemenő és kimenő támogatja az SQL Server-adatforrás</span><span class="sxs-lookup"><span data-stu-id="84685-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="84685-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="84685-259">1.2.5303.1</span></span>

*  <span data-ttu-id="84685-260">Javítsa ki a időtúllépési problémát toosupport több időigényes adatforrásához való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="84685-260">Fix timeout issue toosupport more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="84685-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="84685-261">1.1.5526.8</span></span>

*  <span data-ttu-id="84685-262">.NET-keretrendszer 4.5.1 előfeltételként igényel a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="84685-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="84685-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="84685-263">1.0.5144.2</span></span>

*  <span data-ttu-id="84685-264">Nincs változás, amelyek hatással vannak az Azure Data Factory forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="84685-264">No changes that affect Azure Data Factory scenarios.</span></span>
