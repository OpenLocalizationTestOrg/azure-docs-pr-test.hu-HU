---
title: "Az adatkezelési átjáró kibocsátási megjegyzései |} Microsoft Docs"
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
ms.openlocfilehash: c052d7e9f757164429ce867201b96305e405dce9
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="release-notes-for-data-management-gateway"></a><span data-ttu-id="1cb0f-103">Az adatkezelési átjáró kibocsátási megjegyzései</span><span class="sxs-lookup"><span data-stu-id="1cb0f-103">Release notes for Data Management Gateway</span></span>
<span data-ttu-id="1cb0f-104">A modern Adatintegráció kapcsolatban felmerülő kihívások egyik áthelyezni az adatokat a helyszíni felhőbe érkező vagy oda irányuló.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-104">One of the challenges for modern data integration is to move data to and from on-premises to cloud.</span></span> <span data-ttu-id="1cb0f-105">Adat-előállító lehetővé teszi, hogy ez az integráció az adatkezelési átjáró, amely egy olyan ügynök, hogy a helyszíni hibrid adatátvitel engedélyezése telepítheti.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-105">Data Factory makes this integration with Data Management Gateway, which is an agent that you can install on-premises to enable hybrid data movement.</span></span>

<span data-ttu-id="1cb0f-106">Az adatkezelési átjáró, és hogy miképpen lehet vele kapcsolatos részletes információkat a következő cikkekben talál:</span><span class="sxs-lookup"><span data-stu-id="1cb0f-106">See the following articles for detailed information about Data Management Gateway and how to use it:</span></span>

*  [<span data-ttu-id="1cb0f-107">Adatkezelési átjáró</span><span class="sxs-lookup"><span data-stu-id="1cb0f-107">Data Management Gateway</span></span>](data-factory-data-management-gateway.md)
*  [<span data-ttu-id="1cb0f-108">Adatok áthelyezése között a helyszíni és felhőalapú Azure Data Factory használatával</span><span class="sxs-lookup"><span data-stu-id="1cb0f-108">Move data between on-premises and cloud using Azure Data Factory</span></span>](data-factory-move-data-between-onprem-and-cloud.md)


## <a name="current-version-21063477"></a><span data-ttu-id="1cb0f-109">AKTUÁLIS VERZIÓ (2.10.6347.7)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-109">CURRENT VERSION (2.10.6347.7)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="1cb0f-110">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-110">Enhancements-</span></span>
- <span data-ttu-id="1cb0f-111">Is hozzáadhat DNS-bejegyzések engedélyezése helyett engedélyezési lista a service bus összes Azure IP-címet a tűzfal (ha szükséges).</span><span class="sxs-lookup"><span data-stu-id="1cb0f-111">You can add DNS entries to whitelist service bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="1cb0f-112">Azure-portál is megtalálhatja a megfelelő DNS-bejegyzés (Data Factory -> "Fejlesztés és üzembe helyezés" -> "Átjárók" -> "serviceUrls" (a JSON-ban)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-112">You can find respective DNS entry on Azure portal (Data Factory -> ‘Author and Deploy’ -> ‘Gateways’ -> "serviceUrls" (in JSON)</span></span>
- <span data-ttu-id="1cb0f-113">HDFS-összekötő mostantól támogatja a nyilvános önaláírt tanúsítványt azáltal, hogy SSL-ellenőrzésének kihagyására.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-113">HDFS connector now supports self-signed public certificate by letting you skip SSL validation.</span></span>
- <span data-ttu-id="1cb0f-114">Rögzített: Problémát átjáró kapcsolat nélküli (miatt az óra eltérésére) frissítése közben.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-114">Fixed: Issue with gateway offline during update (due to clock skew)</span></span>



## <a name="earlier-versions"></a><span data-ttu-id="1cb0f-115">Korábbi verziói</span><span class="sxs-lookup"><span data-stu-id="1cb0f-115">Earlier versions</span></span>

## <a name="2963132"></a><span data-ttu-id="1cb0f-116">2.9.6313.2</span><span class="sxs-lookup"><span data-stu-id="1cb0f-116">2.9.6313.2</span></span>
### <a name="enhancements-"></a><span data-ttu-id="1cb0f-117">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-117">Enhancements-</span></span>
-   <span data-ttu-id="1cb0f-118">Is hozzáadhat DNS-bejegyzések az engedélyezett engedélyezése helyett a Service Bus összes Azure IP-címet a tűzfal (ha szükséges).</span><span class="sxs-lookup"><span data-stu-id="1cb0f-118">You can add DNS entries to whitelist Service Bus rather than whitelisting all Azure IP addresses from your firewall (if needed).</span></span> <span data-ttu-id="1cb0f-119">Itt további részleteket.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-119">More details here.</span></span>
-   <span data-ttu-id="1cb0f-120">Adatok másolása egyetlen blokkot és a blob akár 4.75 TB-ig, amely az a maximális támogatott fájlméret blokkblob most is.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-120">You can now copy data to/from a single block blob up to 4.75 TB, which is the max supported size of block blob.</span></span> <span data-ttu-id="1cb0f-121">(a korábbi korlát 195 GB volt).</span><span class="sxs-lookup"><span data-stu-id="1cb0f-121">(earlier limit was 195 GB).</span></span>
-   <span data-ttu-id="1cb0f-122">Rögzített: Kívüli memória probléma során több kisebb fájlt kicsomagolás másolási tevékenység során.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-122">Fixed: Out of memory issue while unzipping several small files during copy activity.</span></span>
-   <span data-ttu-id="1cb0f-123">Rögzített: Indexe kívül tartomány probléma másolása egy helyi SQL Server idempotencia szolgáltatással Document DB rendszerbe.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-123">Fixed: Index out of range issue while copying from Document DB to an on-premises SQL Server with idempotency feature.</span></span>
-   <span data-ttu-id="1cb0f-124">Rögzített: SQL-karbantartási parancsprogramot nem működik a helyszíni SQL Server a másolása varázsló segítségével.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-124">Fixed: SQL cleanup script doesn't work with on-premises SQL Server from Copy Wizard.</span></span>
-   <span data-ttu-id="1cb0f-125">Rögzített: Végén területtel rendelkező oszlopnév nem működik a másolási tevékenység.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-125">Fixed: Column name with space at the end does not work in copy activity.</span></span>

## <a name="28662833"></a><span data-ttu-id="1cb0f-126">2.8.66283.3</span><span class="sxs-lookup"><span data-stu-id="1cb0f-126">2.8.66283.3</span></span>
### <a name="enhancements-"></a><span data-ttu-id="1cb0f-127">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-127">Enhancements-</span></span>
- <span data-ttu-id="1cb0f-128">Rögzített: Probléma átjáró újraindítás hitelesítő adatok hiányoznak.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-128">Fixed: Issue with missing credentials on gateway machine reboot.</span></span>
- <span data-ttu-id="1cb0f-129">Rögzített: Regisztráció során átjáró problémát állítsa vissza egy biztonságimásolat-fájl használatával.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-129">Fixed: Issue with registration during gateway restore using a backup file.</span></span>


## <a name="2762401"></a><span data-ttu-id="1cb0f-130">2.7.6240.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-130">2.7.6240.1</span></span>
### <a name="enhancements-"></a><span data-ttu-id="1cb0f-131">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-131">Enhancements-</span></span>
- <span data-ttu-id="1cb0f-132">Rögzített: Oracle forrásaként null értéke helytelen olvasható.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-132">Fixed: Incorrect read of Decimal null value from Oracle as source.</span></span>

## <a name="2661922"></a><span data-ttu-id="1cb0f-133">2.6.6192.2</span><span class="sxs-lookup"><span data-stu-id="1cb0f-133">2.6.6192.2</span></span>
### <a name="whats-new"></a><span data-ttu-id="1cb0f-134">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="1cb0f-134">What’s new</span></span>
- <span data-ttu-id="1cb0f-135">Az ügyfelek visszajelzést adhat az átjáró regisztrálása a felhasználói élmény.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-135">Customers can provide feedback on gateway registering experience.</span></span>
- <span data-ttu-id="1cb0f-136">Egy új tömörítési formátum támogatja: ZIP (Deflate)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-136">Support a new compression format: ZIP (Deflate)</span></span>

### <a name="enhancements-"></a><span data-ttu-id="1cb0f-137">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-137">Enhancements-</span></span>
- <span data-ttu-id="1cb0f-138">A teljesítmény javítása a Oracle gyűjtése, HDFS forrás.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-138">Performance improvement for Oracle Sink, HDFS source.</span></span>
- <span data-ttu-id="1cb0f-139">Az átjáró automatikus hibajavítás frissítéséhez átjáró párhuzamos feldolgozási kapacitás.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-139">Bug fix for gateway auto update, gateway parallel processing capacity.</span></span>


## <a name="2561641"></a><span data-ttu-id="1cb0f-140">2.5.6164.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-140">2.5.6164.1</span></span>
### <a name="enhancements"></a><span data-ttu-id="1cb0f-141">Fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-141">Enhancements</span></span>
- <span data-ttu-id="1cb0f-142">Hatékonyabb és megbízhatóbb átjáró regisztrációs élmény-most átjáró regisztráció során, így gyorsabban tapasztalhat a regisztrációs folyamat állapota tudja követni.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-142">Improved and more robust Gateway registration experience- Now you can track progress status during the Gateway registration process, which makes the registration experience more responsive.</span></span>
- <span data-ttu-id="1cb0f-143">Növekedés az átjáró visszaállítása folyamat -, de ilyenkor is helyreállíthatók az átjáró, még akkor is, ha nem rendelkezik az átjáró biztonságimásolat-fájl ezzel a frissítéssel.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-143">Improvement in Gateway Restore Process- You can still recover gateway even if you do not have the gateway backup file with this update.</span></span> <span data-ttu-id="1cb0f-144">Ehhez szükség lenne állíthatja vissza a társított szolgáltatás hitelesítő adatait a portálon.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-144">This would require you to reset Linked Service credentials in Portal.</span></span>
- <span data-ttu-id="1cb0f-145">Hiba a javítás.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-145">Bug fix.</span></span>

## <a name="2461511"></a><span data-ttu-id="1cb0f-146">2.4.6151.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-146">2.4.6151.1</span></span>

### <a name="whats-new"></a><span data-ttu-id="1cb0f-147">Újdonságok</span><span class="sxs-lookup"><span data-stu-id="1cb0f-147">What’s new</span></span>

- <span data-ttu-id="1cb0f-148">Most már az adatforrás hitelesítő adatainak helyileg tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-148">You can now store data source credentials locally.</span></span> <span data-ttu-id="1cb0f-149">Az azonosító adatok titkosítása.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-149">The credentials are encrypted.</span></span> <span data-ttu-id="1cb0f-150">Az adatforrás hitelesítő adatai állíthatók helyre, és a biztonságimásolat-fájl, amely a jelenlegi átjárót, az összes helyszíni exportálható segítségével visszaállítani.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-150">The data source credentials can be recovered and restored using the backup file that can be exported from the existing Gateway, all on-premises.</span></span>

### <a name="enhancements-"></a><span data-ttu-id="1cb0f-151">Továbbfejlesztett-</span><span class="sxs-lookup"><span data-stu-id="1cb0f-151">Enhancements-</span></span>

- <span data-ttu-id="1cb0f-152">Hatékonyabb és megbízhatóbb átjáró regisztrációs felületet.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-152">Improved and more robust Gateway registration experience.</span></span>
- <span data-ttu-id="1cb0f-153">Az automatikus észlelés quotechar tulajdonság-konfiguráció támogatása másolása varázsló a szöveges formátumú, és átfogó formátum észlelési pontosságának javítása.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-153">Support auto detection of QuoteChar configuration for Text format in copy wizard, and improve the overall format detection accuracy.</span></span>

## <a name="2361002"></a><span data-ttu-id="1cb0f-154">2.3.6100.2</span><span class="sxs-lookup"><span data-stu-id="1cb0f-154">2.3.6100.2</span></span>

- <span data-ttu-id="1cb0f-155">Támogatja a helyszíni fájlrendszer és a HDFS firstRowAsHeader és SkipLineCount az automatikus észlelés szövegfájlok másolása varázsló.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-155">Support firstRowAsHeader and SkipLineCount auto detection in copy wizard for text files in on-premises File system and HDFS.</span></span>
- <span data-ttu-id="1cb0f-156">A hálózati kapcsolatot az átjáró és a Service Bus stabilitásának javítása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-156">Enhance the stability of network connection between gateway and Service Bus</span></span>
- <span data-ttu-id="1cb0f-157">Néhány hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-157">A few bug fixes</span></span>


## <a name="2260721"></a><span data-ttu-id="1cb0f-158">2.2.6072.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-158">2.2.6072.1</span></span>

*  <span data-ttu-id="1cb0f-159">Az átjáró konfigurációkezelőjének használatával az átjáró HTTP-proxy beállítást támogatja.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-159">Supports setting HTTP proxy for the gateway using the Gateway Configuration Manager.</span></span> <span data-ttu-id="1cb0f-160">Ha konfigurálva, Azure Blob, az Azure Table, az Azure Data Lake és a Document DB rendszerbe HTTP-proxyn keresztül érhetők el.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-160">If configured, Azure Blob, Azure Table, Azure Data Lake, and Document DB are accessed through HTTP proxy.</span></span>
*  <span data-ttu-id="1cb0f-161">Támogatja a fejléc kezelése a szöveges, /, az Azure Blob, az Azure Data Lake Store, az adatok másolásakor helyszíni fájlrendszer, és a helyszíni HDFS.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-161">Supports header handling for TextFormat when copying data from/to Azure Blob, Azure Data Lake Store, on-premises File System, and on-premises HDFS.</span></span>
*  <span data-ttu-id="1cb0f-162">Az adatok másolása hozzáfűző Blob és a már támogatott Blokkblob együtt az oldalakra vonatkozó Blob támogatja.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-162">Supports copying data from Append Blob and Page Blob along with the already supported Block Blob.</span></span>
*  <span data-ttu-id="1cb0f-163">Bevezet egy új az átjáró állapotának **Online (korlátozott)**, ami azt jelenti, hogy működik-e az átjáró fő funkcióit kivéve másolása varázsló interaktív művelet támogatása.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-163">Introduces a new gateway status **Online (Limited)**, which indicates that the main functionality of the gateway works except the interactive operation support for Copy Wizard.</span></span>
*  <span data-ttu-id="1cb0f-164">Fokozza a megbízhatóságát, az átjáró regisztrálása regisztrációs kulccsal.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-164">Enhances the robustness of gateway registration using registration key.</span></span>

## <a name="216040"></a><span data-ttu-id="1cb0f-165">2.1.6040.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-165">2.1.6040.</span></span>

*  <span data-ttu-id="1cb0f-166">DB2 illesztőprogram most már szerepel az átjárótelepítési csomag.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-166">DB2 driver is included in the gateway installation package now.</span></span> <span data-ttu-id="1cb0f-167">Nem kell külön telepíteni.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-167">You do not need to install it separately.</span></span>
*  <span data-ttu-id="1cb0f-168">DB2-illesztőprogram mostantól támogatja a z/OS- és a DB2 i (AS / 400) együtt a már támogatott platformon (Linux, Unix és a Windows).</span><span class="sxs-lookup"><span data-stu-id="1cb0f-168">DB2 driver now supports z/OS and DB2 for i (AS/400) along with the platforms already supported (Linux, Unix, and Windows).</span></span>
*  <span data-ttu-id="1cb0f-169">A cél- és a helyszíni adattárolókhoz Azure Cosmos DB használatával támogatja</span><span class="sxs-lookup"><span data-stu-id="1cb0f-169">Supports using Azure Cosmos DB as a source or destination for on-premises data stores</span></span>
*  <span data-ttu-id="1cb0f-170">Támogatja az adatok másolásának/cold/használt adatok rétegére való blob-tároló már támogatott általános célú tárfiók együtt.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-170">Supports copying data from/to cold/hot blob storage along with the already supported general-purpose storage account.</span></span>
*  <span data-ttu-id="1cb0f-171">Lehetővé teszi a helyszíni SQL Server távoli bejelentkezési jogokkal átjárón keresztül csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-171">Allows you to connect to on-premises SQL Server via gateway with remote login privileges.</span></span>  

## <a name="2060131"></a><span data-ttu-id="1cb0f-172">2.0.6013.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-172">2.0.6013.1</span></span>

*  <span data-ttu-id="1cb0f-173">Kiválaszthatja a nyelvet/kulturális környezet manuális telepítése során egy átjáró által használt.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-173">You can select the language/culture to be used by a gateway during manual installation.</span></span>

*  <span data-ttu-id="1cb0f-174">Ha az átjáró nem várt módon működik, dönthet úgy, az elmúlt hét napban átjáró naplókat küldeni a Microsoftnak a problémát a hibaelhárítás elősegítése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-174">When gateway does not work as expected, you can choose to send gateway logs of last seven days to Microsoft to facilitate troubleshooting of the issue.</span></span> <span data-ttu-id="1cb0f-175">Ha az átjáró nem csatlakozik a felhőszolgáltatáshoz, dönthet úgy, mentse, és átjáró naplók archiválása.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-175">If gateway is not connected to the cloud service, you can choose to save and archive gateway logs.</span></span>  

*  <span data-ttu-id="1cb0f-176">Felhasználói felület fejlesztései az átjáró a configuration manager:</span><span class="sxs-lookup"><span data-stu-id="1cb0f-176">User interface improvements for gateway configuration manager:</span></span>

    *  <span data-ttu-id="1cb0f-177">Adja meg az átjáró állapotának több látható a kezdőlap.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-177">Make gateway status more visible on the Home tab.</span></span>

    *  <span data-ttu-id="1cb0f-178">Átszervezett és egyszerűsített szabályozza.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-178">Reorganized and simplified controls.</span></span>

    *  <span data-ttu-id="1cb0f-179">Adatok másolása egy tárolási használatával a [kódmentes másolási preview eszköz](data-factory-copy-data-wizard-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="1cb0f-179">You can copy data from a storage using the [code-free copy preview tool](data-factory-copy-data-wizard-tutorial.md).</span></span> <span data-ttu-id="1cb0f-180">Lásd: [előkészített másolási](data-factory-copy-activity-performance.md#staged-copy) részleteiről a szolgáltatás általában.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-180">See [Staged Copy](data-factory-copy-activity-performance.md#staged-copy) for details about this feature in general.</span></span>
*  <span data-ttu-id="1cb0f-181">Az adatkezelési átjáró érkező adatokat a helyszíni SQL Server adatbázis-ről Azure Machine Learning is használhatja.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-181">You can use Data Management Gateway to ingress data directly from an on-premises SQL Server database into Azure Machine Learning.</span></span>

*  <span data-ttu-id="1cb0f-182">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-182">Performance improvements</span></span>

    * <span data-ttu-id="1cb0f-183">Séma/Preview SQL-kiszolgálón a kódmentes másolási preview eszköz megjelenítéséről teljesítmény javításához.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-183">Improve performance on viewing Schema/Preview against SQL Server in code-free copy preview tool.</span></span>

## <a name="11259531"></a><span data-ttu-id="1cb0f-184">1.12.5953.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-184">1.12.5953.1</span></span>

*  <span data-ttu-id="1cb0f-185">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-185">Bug fixes</span></span>

## <a name="11159181"></a><span data-ttu-id="1cb0f-186">1.11.5918.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-186">1.11.5918.1</span></span>

*  <span data-ttu-id="1cb0f-187">Az adatátjáró eseménynaplójában maximális mérete 1 MB, 40 MB-ra nőtt.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-187">Maximum size of the gateway event log has been increased from 1 MB to 40 MB.</span></span>

*  <span data-ttu-id="1cb0f-188">Egy figyelmeztető párbeszédpanel jelenik meg, abban az esetben, ha újraindítás szükséges átjáró automatikus frissítés során.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-188">A warning dialog is displayed in case a restart is needed during gateway auto-update.</span></span> <span data-ttu-id="1cb0f-189">Ha szeretné, indítsa újra a jobb oldali majd vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-189">You can choose to restart right then or later.</span></span>

*  <span data-ttu-id="1cb0f-190">Abban az esetben, ha az automatikus frissítés nem sikerül, a gateway installer háromszor a maximális automatikus frissítési újrapróbálkozik.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-190">In case auto-update fails, gateway installer retries auto-updating three times at maximum.</span></span>

*  <span data-ttu-id="1cb0f-191">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-191">Performance improvements</span></span>

    * <span data-ttu-id="1cb0f-192">Nagy táblák tölt be a helyi kiszolgálóról a kódmentes másolásának esetéhez teljesítményének javítása.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-192">Improve performance for loading large tables from on-premises server in code-free copy scenario.</span></span>

*  <span data-ttu-id="1cb0f-193">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-193">Bug fixes</span></span>

## <a name="11058921"></a><span data-ttu-id="1cb0f-194">1.10.5892.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-194">1.10.5892.1</span></span>

*  <span data-ttu-id="1cb0f-195">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-195">Performance improvements</span></span>

*  <span data-ttu-id="1cb0f-196">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-196">Bug fixes</span></span>

## <a name="1958652"></a><span data-ttu-id="1cb0f-197">1.9.5865.2</span><span class="sxs-lookup"><span data-stu-id="1cb0f-197">1.9.5865.2</span></span>

*  <span data-ttu-id="1cb0f-198">Nulla touch automatikus frissítési funkció</span><span class="sxs-lookup"><span data-stu-id="1cb0f-198">Zero touch auto update capability</span></span>
*  <span data-ttu-id="1cb0f-199">Az átjáró állapotának mutatók új tálcaikon</span><span class="sxs-lookup"><span data-stu-id="1cb0f-199">New tray icon with gateway status indicators</span></span>
*  <span data-ttu-id="1cb0f-200">Az ügyfél képes "Frissítés most"</span><span class="sxs-lookup"><span data-stu-id="1cb0f-200">Ability to “Update now” from the client</span></span>
*  <span data-ttu-id="1cb0f-201">Frissítés időpontjának ütemezése meg</span><span class="sxs-lookup"><span data-stu-id="1cb0f-201">Ability to set update schedule time</span></span>
*  <span data-ttu-id="1cb0f-202">PowerShell-parancsfájl az elvégezte az automatikus frissítés be-és kikapcsolása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-202">PowerShell script for toggling auto-update on/off</span></span>
*  <span data-ttu-id="1cb0f-203">JSON-formátum támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-203">Support for JSON format</span></span>  
*  <span data-ttu-id="1cb0f-204">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-204">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-205">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-205">Bug fixes</span></span>

## <a name="1858221"></a><span data-ttu-id="1cb0f-206">1.8.5822.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-206">1.8.5822.1</span></span>

*  <span data-ttu-id="1cb0f-207">Hibaelhárítási élmény javítása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-207">Improve troubleshooting experience</span></span>
*  <span data-ttu-id="1cb0f-208">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-208">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-209">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-209">Bug fixes</span></span>

### <a name="1757951"></a><span data-ttu-id="1cb0f-210">1.7.5795.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-210">1.7.5795.1</span></span>

*  <span data-ttu-id="1cb0f-211">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-211">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-212">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-212">Bug fixes</span></span>

### <a name="1757641"></a><span data-ttu-id="1cb0f-213">1.7.5764.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-213">1.7.5764.1</span></span>

*  <span data-ttu-id="1cb0f-214">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-214">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-215">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-215">Bug fixes</span></span>

### <a name="1657351"></a><span data-ttu-id="1cb0f-216">1.6.5735.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-216">1.6.5735.1</span></span>

*  <span data-ttu-id="1cb0f-217">Támogatja a helyszíni HDFS forrás/fogadó</span><span class="sxs-lookup"><span data-stu-id="1cb0f-217">Support on-premises HDFS Source/Sink</span></span>
*  <span data-ttu-id="1cb0f-218">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-218">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-219">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-219">Bug fixes</span></span>

### <a name="1656961"></a><span data-ttu-id="1cb0f-220">1.6.5696.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-220">1.6.5696.1</span></span>

*  <span data-ttu-id="1cb0f-221">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-221">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-222">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-222">Bug fixes</span></span>

### <a name="1656761"></a><span data-ttu-id="1cb0f-223">1.6.5676.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-223">1.6.5676.1</span></span>

*  <span data-ttu-id="1cb0f-224">Diagnosztikai eszközök a Configuration Manager támogatására</span><span class="sxs-lookup"><span data-stu-id="1cb0f-224">Support diagnostic tools on Configuration Manager</span></span>
*  <span data-ttu-id="1cb0f-225">Az Azure Data Factory táblázatos adatforrások táblaoszlopok támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-225">Support table columns for tabular data sources for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-226">Az Azure Data Factory SQL DW támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-226">Support SQL DW for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-227">Az Azure Data Factory Reclusive BlobSource és FileSource támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-227">Support Reclusive in BlobSource and FileSource for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-228">BlobSink és FileSink bináris másolatot az Azure Data Factory CopyBehavior – mergefiles típusú PreserveHierarchy és FlattenHierarchy támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-228">Support CopyBehavior – MergeFiles, PreserveHierarchy, and FlattenHierarchy in BlobSink and FileSink with Binary Copy for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-229">Támogatja a másolási tevékenység során az Azure Data Factory jelentési</span><span class="sxs-lookup"><span data-stu-id="1cb0f-229">Support Copy Activity reporting progress for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-230">Az Azure Data Factory támogatási adatok forrás kapcsolat érvényesítése</span><span class="sxs-lookup"><span data-stu-id="1cb0f-230">Support Data Source Connectivity Validation for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-231">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-231">Bug fixes</span></span>

### <a name="1656721"></a><span data-ttu-id="1cb0f-232">1.6.5672.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-232">1.6.5672.1</span></span>

*  <span data-ttu-id="1cb0f-233">Az ODBC-adatforrás az Azure Data Factory támogatási tábla neve</span><span class="sxs-lookup"><span data-stu-id="1cb0f-233">Support table name for ODBC data source for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-234">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-234">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-235">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-235">Bug fixes</span></span>

### <a name="1656581"></a><span data-ttu-id="1cb0f-236">1.6.5658.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-236">1.6.5658.1</span></span>

*  <span data-ttu-id="1cb0f-237">Támogatási fájlt az Azure Data Factory gyűjtése</span><span class="sxs-lookup"><span data-stu-id="1cb0f-237">Support File Sink for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-238">Hierarchia megőrzése bináris másolása az Azure Data Factory támogatási</span><span class="sxs-lookup"><span data-stu-id="1cb0f-238">Support preserving hierarchy in binary copy for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-239">Az Azure Data Factory támogatja a másolási tevékenység idempotencia</span><span class="sxs-lookup"><span data-stu-id="1cb0f-239">Support Copy Activity Idempotency for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-240">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-240">Bug fixes</span></span>

### <a name="1656401"></a><span data-ttu-id="1cb0f-241">1.6.5640.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-241">1.6.5640.1</span></span>

*  <span data-ttu-id="1cb0f-242">3 további adatforrások támogatja az Azure Data Factory (ODBC, OData, HDFS)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-242">Support 3 more data sources for Azure Data Factory (ODBC, OData, HDFS)</span></span>
*  <span data-ttu-id="1cb0f-243">Az Azure Data Factory idézőjel, a fürt megosztott kötetei szolgáltatás elemző támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-243">Support quote character in csv parser for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-244">Tömörítés támogatása (BZip2)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-244">Compression support (BZip2)</span></span>
*  <span data-ttu-id="1cb0f-245">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-245">Bug fixes</span></span>

### <a name="1556121"></a><span data-ttu-id="1cb0f-246">1.5.5612.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-246">1.5.5612.1</span></span>

*  <span data-ttu-id="1cb0f-247">Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata és Sybase) öt relációs adatbázisok támogatása</span><span class="sxs-lookup"><span data-stu-id="1cb0f-247">Support five relational databases for Azure Data Factory (MySQL, PostgreSQL, DB2, Teradata, and Sybase)</span></span>
*  <span data-ttu-id="1cb0f-248">Tömörítés támogatása (Gzip és a Deflate)</span><span class="sxs-lookup"><span data-stu-id="1cb0f-248">Compression support (Gzip and Deflate)</span></span>
*  <span data-ttu-id="1cb0f-249">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-249">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-250">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-250">Bug fixes</span></span>

### <a name="1455491"></a><span data-ttu-id="1cb0f-251">1.4.5549.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-251">1.4.5549.1</span></span>

*  <span data-ttu-id="1cb0f-252">Az Azure Data Factory Oracle data forrás támogatását</span><span class="sxs-lookup"><span data-stu-id="1cb0f-252">Add Oracle data source support for Azure Data Factory</span></span>
*  <span data-ttu-id="1cb0f-253">Teljesítménnyel kapcsolatos fejlesztések</span><span class="sxs-lookup"><span data-stu-id="1cb0f-253">Performance improvements</span></span>
*  <span data-ttu-id="1cb0f-254">Hibajavítások</span><span class="sxs-lookup"><span data-stu-id="1cb0f-254">Bug fixes</span></span>

### <a name="1454921"></a><span data-ttu-id="1cb0f-255">1.4.5492.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-255">1.4.5492.1</span></span>

*  <span data-ttu-id="1cb0f-256">Egyesített bináris, amely támogatja a Microsoft Azure Data Factory és az Office 365 Power BI szolgáltatásokat</span><span class="sxs-lookup"><span data-stu-id="1cb0f-256">Unified binary that supports both Microsoft Azure Data Factory and Office 365 Power BI services</span></span>
*  <span data-ttu-id="1cb0f-257">Pontosítsa a konfigurációs UI felületen és a regisztrációs folyamat</span><span class="sxs-lookup"><span data-stu-id="1cb0f-257">Refine the Configuration UI and registration process</span></span>
*  <span data-ttu-id="1cb0f-258">Az Azure Data Factory – Azure bemenő és kimenő támogatja az SQL Server-adatforrás</span><span class="sxs-lookup"><span data-stu-id="1cb0f-258">Azure Data Factory – Azure Ingress and Egress support for SQL Server data source</span></span>

### <a name="1253031"></a><span data-ttu-id="1cb0f-259">1.2.5303.1</span><span class="sxs-lookup"><span data-stu-id="1cb0f-259">1.2.5303.1</span></span>

*  <span data-ttu-id="1cb0f-260">További időigényes adatforrás-kapcsolatok támogatásához időtúllépés problémának a megoldásához.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-260">Fix timeout issue to support more time-consuming data source connections.</span></span>

### <a name="1155268"></a><span data-ttu-id="1cb0f-261">1.1.5526.8</span><span class="sxs-lookup"><span data-stu-id="1cb0f-261">1.1.5526.8</span></span>

*  <span data-ttu-id="1cb0f-262">.NET-keretrendszer 4.5.1 előfeltételként igényel a telepítés során.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-262">Requires .NET Framework 4.5.1 as a prerequisite during setup.</span></span>

### <a name="1051442"></a><span data-ttu-id="1cb0f-263">1.0.5144.2</span><span class="sxs-lookup"><span data-stu-id="1cb0f-263">1.0.5144.2</span></span>

*  <span data-ttu-id="1cb0f-264">Nincs változás, amelyek hatással vannak az Azure Data Factory forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="1cb0f-264">No changes that affect Azure Data Factory scenarios.</span></span>
