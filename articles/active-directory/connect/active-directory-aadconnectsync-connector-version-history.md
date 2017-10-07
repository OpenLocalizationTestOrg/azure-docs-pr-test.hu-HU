---
title: "aaaConnector Verziókiadások |} Microsoft Docs"
description: "Ez a témakör hello összekötők összes kiadása a Forefront Identity Manager (FIM) és a Microsoft Identity Manager (MIM)"
services: active-directory
documentationcenter: 
author: fimguy
manager: femila
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: fimguy
ms.openlocfilehash: 3522f17c30e46542eaa367ecdefdfd2fc47f71a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connector-version-release-history"></a><span data-ttu-id="fdd78-103">Összekötő verziókiadásai</span><span class="sxs-lookup"><span data-stu-id="fdd78-103">Connector Version Release History</span></span>
<span data-ttu-id="fdd78-104">a Forefront Identity Manager (FIM) és a Microsoft Identity Manager (MIM) hello összekötők gyakran frissül.</span><span class="sxs-lookup"><span data-stu-id="fdd78-104">hello Connectors for Forefront Identity Manager (FIM) and Microsoft Identity Manager (MIM) are updated frequently.</span></span>

> [!NOTE]
> <span data-ttu-id="fdd78-105">Ez a témakör csak a FIM és a MIM rendszer.</span><span class="sxs-lookup"><span data-stu-id="fdd78-105">This topic is on FIM and MIM only.</span></span> <span data-ttu-id="fdd78-106">Az összekötők nem támogatottak az Azure AD Connect telepítése.</span><span class="sxs-lookup"><span data-stu-id="fdd78-106">These Connectors are not supported for install on Azure AD Connect.</span></span> <span data-ttu-id="fdd78-107">Az AADConnect kiadott összekötők előtelepített toospecified Build frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="fdd78-107">Released Connectors are preinstalled on AADConnect when upgrading toospecified Build.</span></span>

<span data-ttu-id="fdd78-108">Ez a témakör hello összekötők kiadott összes verziójának felsorolása.</span><span class="sxs-lookup"><span data-stu-id="fdd78-108">This topic list all versions of hello Connectors that have been released.</span></span>

<span data-ttu-id="fdd78-109">Kapcsolódó hivatkozások:</span><span class="sxs-lookup"><span data-stu-id="fdd78-109">Related links:</span></span>

* [<span data-ttu-id="fdd78-110">Töltse le a legfrissebb összekötők</span><span class="sxs-lookup"><span data-stu-id="fdd78-110">Download Latest Connectors</span></span>](http://go.microsoft.com/fwlink/?LinkId=717495)
* <span data-ttu-id="fdd78-111">[Általános LDAP-összekötő](active-directory-aadconnectsync-connector-genericldap.md) dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdd78-111">[Generic LDAP Connector](active-directory-aadconnectsync-connector-genericldap.md) reference documentation</span></span>
* <span data-ttu-id="fdd78-112">[Általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md) dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdd78-112">[Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md) reference documentation</span></span>
* <span data-ttu-id="fdd78-113">[Webalkalmazás-Services-összekötő](http://go.microsoft.com/fwlink/?LinkID=226245) dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdd78-113">[Web Services Connector](http://go.microsoft.com/fwlink/?LinkID=226245) reference documentation</span></span>
* <span data-ttu-id="fdd78-114">[PowerShell-összekötő](active-directory-aadconnectsync-connector-powershell.md) dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdd78-114">[PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) reference documentation</span></span>
* <span data-ttu-id="fdd78-115">[Lotus Domino-összekötő](active-directory-aadconnectsync-connector-domino.md) dokumentáció</span><span class="sxs-lookup"><span data-stu-id="fdd78-115">[Lotus Domino Connector](active-directory-aadconnectsync-connector-domino.md) reference documentation</span></span>


## <a name="116040-aadconnect-pending-release"></a><span data-ttu-id="fdd78-116">1.1.604.0 (AADConnect függőben lévő kiadás)</span><span class="sxs-lookup"><span data-stu-id="fdd78-116">1.1.604.0 (AADConnect Pending Release)</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="fdd78-117">Javított problémák:</span><span class="sxs-lookup"><span data-stu-id="fdd78-117">Fixed issues:</span></span>

* <span data-ttu-id="fdd78-118">Általános webszolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="fdd78-118">Generic Web Services:</span></span>
  * <span data-ttu-id="fdd78-119">Megtörtént egy probléma javítása meggátolja, hogy a SOAP-projekt jöjjenek létre, amikor két vagy több végpontot történt.</span><span class="sxs-lookup"><span data-stu-id="fdd78-119">Fixed an issue preventing a SOAP project from being created when there were two or more endpoints.</span></span>
* <span data-ttu-id="fdd78-120">Általános SQL:</span><span class="sxs-lookup"><span data-stu-id="fdd78-120">Generic SQL:</span></span>
  * <span data-ttu-id="fdd78-121">Az importálás hello működésének hello GSQL lett nem konvertálása idő megfelelően, mentésekor tooconnector terület.</span><span class="sxs-lookup"><span data-stu-id="fdd78-121">In hello operation of import hello GSQL was not converting time correctly, when saved tooconnector space.</span></span> <span data-ttu-id="fdd78-122">hello hello GSQL összekötő lemezterület az alapértelmezett dátum és idő formátuma megváltozott too'yyyy-hh-nn "éééé-hh-nn hh:mm:ssZ" HH:mm:ssZ ".</span><span class="sxs-lookup"><span data-stu-id="fdd78-122">hello default date and time format for connector space of hello GSQL was changed from 'yyyy-MM-dd hh:mm:ssZ' too'yyyy-MM-dd HH:mm:ssZ'.</span></span>

## <a name="115510-aadconnect-115530"></a><span data-ttu-id="fdd78-123">1.1.551.0 (AADConnect 1.1.553.0)</span><span class="sxs-lookup"><span data-stu-id="fdd78-123">1.1.551.0 (AADConnect 1.1.553.0)</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="fdd78-124">Javított problémák:</span><span class="sxs-lookup"><span data-stu-id="fdd78-124">Fixed issues:</span></span>

* <span data-ttu-id="fdd78-125">Általános webszolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="fdd78-125">Generic Web Services:</span></span>
  * <span data-ttu-id="fdd78-126">hello Wsconfig eszköz nem megfelelő átalakítani "minta kérelem" hello REST szolgáltatás metódus hello Json-tömb.</span><span class="sxs-lookup"><span data-stu-id="fdd78-126">hello Wsconfig tool did not convert correctly hello Json array from "sample request" for hello REST service method.</span></span> <span data-ttu-id="fdd78-127">A Json-tömb hello REST kérelem ennek oka a szerializálás problémákat.</span><span class="sxs-lookup"><span data-stu-id="fdd78-127">This caused problems with serialization this Json array for hello REST request.</span></span>
  * <span data-ttu-id="fdd78-128">Web Service Connector konfigurációs eszköz nem támogatja a hely szimbólumok használata a JSON-attribútum neve</span><span class="sxs-lookup"><span data-stu-id="fdd78-128">Web Service Connector Configuration Tool does not support usage of space symbols in JSON attribute names</span></span> 
    * <span data-ttu-id="fdd78-129">A behelyettesítések felveheti manuálisan toohello WSConfigTool.exe.config fájl, pl.```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span><span class="sxs-lookup"><span data-stu-id="fdd78-129">A Substitution pattern can be added manually toohello WSConfigTool.exe.config file, e.g. ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```</span></span>

* <span data-ttu-id="fdd78-130">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="fdd78-130">Lotus Notes:</span></span>
  * <span data-ttu-id="fdd78-131">Ha a beállítás hello **lehetővé teszik egyéni képesítést adók engedélyezése a szervezet vagy szervezeti egység** le van tiltva, akkor a következők exportált tooDomino hello összekötő sikertelen (frissítés) után hello exportálási flow attribútumainak exportálás során, de hello helyreállításkor Exportálás egy KeyNotFoundException tooSync adja vissza.</span><span class="sxs-lookup"><span data-stu-id="fdd78-131">When hello option **Allow custom certifiers for Organization/Organizational Units** is disabled then hello connector fails during export (Update) After hello export flow all attributes are exported tooDomino but at hello time of export a KeyNotFoundException is returned tooSync.</span></span> 
    * <span data-ttu-id="fdd78-132">Ennek oka hello átnevezése művelet sikertelen lesz, amikor toochange megkülönböztető név (felhasználónév attribútum) módosításával az alábbi hello attribútumok egyikét:</span><span class="sxs-lookup"><span data-stu-id="fdd78-132">This happens because hello rename operation fails when it tries toochange DN (UserName attribute) by changing one of hello attributes below:</span></span>  
      - <span data-ttu-id="fdd78-133">Vezetéknév</span><span class="sxs-lookup"><span data-stu-id="fdd78-133">LastName</span></span>
      - <span data-ttu-id="fdd78-134">Utónév</span><span class="sxs-lookup"><span data-stu-id="fdd78-134">FirstName</span></span>
      - <span data-ttu-id="fdd78-135">MiddleInitial</span><span class="sxs-lookup"><span data-stu-id="fdd78-135">MiddleInitial</span></span>
      - <span data-ttu-id="fdd78-136">AltFullName</span><span class="sxs-lookup"><span data-stu-id="fdd78-136">AltFullName</span></span>
      - <span data-ttu-id="fdd78-137">AltFullNameLanguage</span><span class="sxs-lookup"><span data-stu-id="fdd78-137">AltFullNameLanguage</span></span>
      - <span data-ttu-id="fdd78-138">szervezeti egység</span><span class="sxs-lookup"><span data-stu-id="fdd78-138">ou</span></span>
      - <span data-ttu-id="fdd78-139">altcommonname</span><span class="sxs-lookup"><span data-stu-id="fdd78-139">altcommonname</span></span>

  * <span data-ttu-id="fdd78-140">Ha **lehetővé teszik egyéni képesítést adók engedélyezése a szervezet vagy szervezeti egység** engedélyezve van, de szükséges képesítést adók engedélyezése továbbra is üres, akkor KeyNotFoundException következik be.</span><span class="sxs-lookup"><span data-stu-id="fdd78-140">When **Allow custom certifiers for Organization/Organizational Units** option is enabled, but required certifiers are still empty, then KeyNotFoundException occurs.</span></span>

### <a name="enhancements"></a><span data-ttu-id="fdd78-141">Fejlesztései:</span><span class="sxs-lookup"><span data-stu-id="fdd78-141">Enhancements:</span></span>

* <span data-ttu-id="fdd78-142">Általános SQL:</span><span class="sxs-lookup"><span data-stu-id="fdd78-142">Generic SQL:</span></span>
  * <span data-ttu-id="fdd78-143">**Forgatókönyv: újratervezett megvalósítva:** "*" funkció</span><span class="sxs-lookup"><span data-stu-id="fdd78-143">**Scenario: redesigned Implemented:** "*" feature</span></span>
  * <span data-ttu-id="fdd78-144">**Megoldás leírása:** megközelítés megváltozott [többértékű hivatkozási attribútum kezelési](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="fdd78-144">**Solution description:** Changed approach for [multi-valued reference attributes handling](active-directory-aadconnectsync-connector-genericsql.md).</span></span>


### <a name="fixed-issues"></a><span data-ttu-id="fdd78-145">Javított problémák:</span><span class="sxs-lookup"><span data-stu-id="fdd78-145">Fixed issues:</span></span>

* <span data-ttu-id="fdd78-146">Általános webszolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="fdd78-146">Generic Web Services:</span></span>
  * <span data-ttu-id="fdd78-147">Kiszolgálókonfiguráció nem importálható, ha az megtalálható a WebService összekötő</span><span class="sxs-lookup"><span data-stu-id="fdd78-147">Can’t import Server configuration if WebService Connector is present</span></span>
  * <span data-ttu-id="fdd78-148">Több webkiszolgáló szolgáltatással nem működik a WebService összekötő</span><span class="sxs-lookup"><span data-stu-id="fdd78-148">WebService Connector is not working with multiple  Web Services</span></span>

* <span data-ttu-id="fdd78-149">Általános SQL:</span><span class="sxs-lookup"><span data-stu-id="fdd78-149">Generic SQL:</span></span>
  * <span data-ttu-id="fdd78-150">Egyetlen értéket hivatkozott attribútum nem objektumtípusok találhatók</span><span class="sxs-lookup"><span data-stu-id="fdd78-150">No object types are listed for single value referenced attribute</span></span>
  * <span data-ttu-id="fdd78-151">Különbözeti importálás objektumon változások követése stratégia törlések érték többértékű táblából eltávolításakor</span><span class="sxs-lookup"><span data-stu-id="fdd78-151">Delta import on Change Tracking strategy deletes object when value is removed from multi-value table</span></span>
  * <span data-ttu-id="fdd78-152">Az AS a DB2 GSQL Connector OverflowException / 400</span><span class="sxs-lookup"><span data-stu-id="fdd78-152">OverflowException in GSQL connector with DB2 on AS/400</span></span>

<span data-ttu-id="fdd78-153">Lotus:</span><span class="sxs-lookup"><span data-stu-id="fdd78-153">Lotus:</span></span>
  * <span data-ttu-id="fdd78-154">Keresés a szervezeti egységek GlobalParameters lap megnyitása előtt hozzáadott beállítás tooenable\disable</span><span class="sxs-lookup"><span data-stu-id="fdd78-154">Added option tooenable\disable searching OUs before opening GlobalParameters page</span></span>

## <a name="114430"></a><span data-ttu-id="fdd78-155">1.1.443.0</span><span class="sxs-lookup"><span data-stu-id="fdd78-155">1.1.443.0</span></span>

<span data-ttu-id="fdd78-156">Kiadás dátuma: 2017. március</span><span class="sxs-lookup"><span data-stu-id="fdd78-156">Released: 2017 March</span></span>

### <a name="enhancements"></a><span data-ttu-id="fdd78-157">Fejlesztések</span><span class="sxs-lookup"><span data-stu-id="fdd78-157">Enhancements</span></span>

* <span data-ttu-id="fdd78-158">Általános SQL:</span><span class="sxs-lookup"><span data-stu-id="fdd78-158">Generic SQL:</span></span></br><span data-ttu-id="fdd78-159">
  **A forgatókönyvben a jelenség:** az SQL-összekötő, ahol azt csak egy hivatkozás tooone objektumtípus és engedélyezése szükséges tagokkal kereszthivatkozás hello egy jól ismert korlátozás.</span><span class="sxs-lookup"><span data-stu-id="fdd78-159">
  **Scenario Symptoms:**  It is a well-known limitation with hello SQL Connector where we only allow a reference tooone object type and require cross reference with members.</span></span> </br><span data-ttu-id="fdd78-160">
**Megoldás leírása:** hello feldolgozási lépésben hivatkozásainak volt "*" lehetőséget választja, minden kombinációi objektumtípusok visszaadott hátsó toohello szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="fdd78-160">
**Solution description:** In hello processing step for references were "*" option is chosen, ALL combinations of object types will be returned back toohello sync engine.</span></span>

>[!Important]
- <span data-ttu-id="fdd78-161">Ezzel létrehoz sok helyőrzők</span><span class="sxs-lookup"><span data-stu-id="fdd78-161">This will create many placeholders</span></span>
- <span data-ttu-id="fdd78-162">Szükséges toomake meg arról, hogy hello elnevezési közötti objektumtípusok egyedi legyen.</span><span class="sxs-lookup"><span data-stu-id="fdd78-162">It is required toomake sure hello naming is unique cross object types.</span></span>


* <span data-ttu-id="fdd78-163">Általános LDAP:</span><span class="sxs-lookup"><span data-stu-id="fdd78-163">Generic LDAP:</span></span></br><span data-ttu-id="fdd78-164">
 **Forgatókönyv:** Ha csak néhány tárolók adott partíció van kijelölve, majd hello keresési továbbra is megtörténik a teljes partíció.</span><span class="sxs-lookup"><span data-stu-id="fdd78-164">
**Scenario:** When only few containers are selected in specific partition, then hello search still will be done in whole partition.</span></span> <span data-ttu-id="fdd78-165">Specifikus szűri a Synchronization szolgáltatás által, de nem MA, ami problémát okozhat teljesítménycsökkenést.</span><span class="sxs-lookup"><span data-stu-id="fdd78-165">Specific will be filtered by Synchronization Service, but not by MA which might cause performance degradation.</span></span> </br>

 <span data-ttu-id="fdd78-166">**Megoldás leírása:** megváltozott GLDAP összekötő kód toomake lehetőség halad át az összes tároló és azok helyett a hello teljes partíció keresése objektumok kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="fdd78-166">**Solution description:** Changed GLDAP connector's code toomake it possible go through all containers and search objects in each of them, instead of searching in hello whole partition.</span></span>


* <span data-ttu-id="fdd78-167">Lotus Domino:</span><span class="sxs-lookup"><span data-stu-id="fdd78-167">Lotus Domino:</span></span>

  <span data-ttu-id="fdd78-168">**Forgatókönyv:** Domino mail törlés támogatása egy személy eltávolítása az exportálás során.</span><span class="sxs-lookup"><span data-stu-id="fdd78-168">**Scenario:** Domino mail deletion support for a person removal during an export.</span></span> </br><span data-ttu-id="fdd78-169">
  **Megoldás:** konfigurálható mail törlés támogatása egy személy eltávolítása az exportálás során.</span><span class="sxs-lookup"><span data-stu-id="fdd78-169">
**Solution:** Configurable mail deletion support for a person removal during an export.</span></span>

### <a name="fixed-issues"></a><span data-ttu-id="fdd78-170">Javított problémák:</span><span class="sxs-lookup"><span data-stu-id="fdd78-170">Fixed issues:</span></span>
* <span data-ttu-id="fdd78-171">Általános webszolgáltatások:</span><span class="sxs-lookup"><span data-stu-id="fdd78-171">Generic Web Services:</span></span>
 * <span data-ttu-id="fdd78-172">Az alapértelmezett hello szolgáltatás URL-címe módosításakor SAP wsconfig projektek WebService konfigurációs eszköz hello a következő hiba történik, akkor: hello elérési út egy része nem található</span><span class="sxs-lookup"><span data-stu-id="fdd78-172">When changing hello service URL in Default SAP wsconfig projects through WebService Configuration Tool then hello following error happens: Could not find a part of hello path</span></span>

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* <span data-ttu-id="fdd78-173">Általános LDAP:</span><span class="sxs-lookup"><span data-stu-id="fdd78-173">Generic LDAP:</span></span>
 * <span data-ttu-id="fdd78-174">GLDAP Connector nem látja az AD LDS összes attribútum</span><span class="sxs-lookup"><span data-stu-id="fdd78-174">GLDAP Connector does not see all attributes in AD LDS</span></span>
 * <span data-ttu-id="fdd78-175">Varázsló oldaltörések nem UPN attribútum hello LDAP directory séma észlelésekor</span><span class="sxs-lookup"><span data-stu-id="fdd78-175">Wizard breaks when no UPN attributes are detected from hello LDAP directory schema</span></span>
 * <span data-ttu-id="fdd78-176">Különbözeti importálása sikertelen teljes importálás során, ha nincs bejelölve a "objectclass" attribútum nem található felderítési hibákkal</span><span class="sxs-lookup"><span data-stu-id="fdd78-176">Delta Imports Failing with discovery errors not present during full import, when "objectclass" attribute is not selected</span></span>
 * <span data-ttu-id="fdd78-177">Egy "Partíciók és hierarchiák konfigurálása" konfigurációs lapján mely típus egyenlő toohello partíció hello általános új kiszolgálókhoz tartozó objektumok nem jeleníti meg</span><span class="sxs-lookup"><span data-stu-id="fdd78-177">A "Configure Partitions and Hierarchies” configuration page, doesn’t show any objects which type is equal toohello partition for Novel servers in hello Generic</span></span>  
<span data-ttu-id="fdd78-178">LDAP MA.</span><span class="sxs-lookup"><span data-stu-id="fdd78-178">LDAP MA.</span></span> <span data-ttu-id="fdd78-179">Ezek kimutatták RootDSE partíció csak objektumokat.</span><span class="sxs-lookup"><span data-stu-id="fdd78-179">They showed only objects from RootDSE partition.</span></span>


* <span data-ttu-id="fdd78-180">Általános SQL:</span><span class="sxs-lookup"><span data-stu-id="fdd78-180">Generic SQL:</span></span>
 * <span data-ttu-id="fdd78-181">Javítsa ki általános SQL vízjel különbözeti importálás többértékű attribútum nem importált hiba</span><span class="sxs-lookup"><span data-stu-id="fdd78-181">Fix for Generic SQL watermark Delta Import multivalued attribute not imported bug</span></span>
 * <span data-ttu-id="fdd78-182">Többértékű attribútum deleted\added értékek exportálásakor nincsenek deleted\added adatforrás.</span><span class="sxs-lookup"><span data-stu-id="fdd78-182">When exporting deleted\added values of multivalued attribute, they are not deleted\added in data source.</span></span>  


* <span data-ttu-id="fdd78-183">Lotus Notes:</span><span class="sxs-lookup"><span data-stu-id="fdd78-183">Lotus Notes:</span></span>
 * <span data-ttu-id="fdd78-184">Egy adott mező "Teljes neve" megjelenik-e a hello metaverse megfelelően azonban amikor exportáló tooNotes hello értéke hello attribútum értéke Null vagy üres.</span><span class="sxs-lookup"><span data-stu-id="fdd78-184">A specific field "Full Name" is shown in hello metaverse correctly however when exporting tooNotes hello value for hello attribute is Null or Empty.</span></span>
 * <span data-ttu-id="fdd78-185">Hárítsa el az ismétlődő Certifier hiba</span><span class="sxs-lookup"><span data-stu-id="fdd78-185">Fix for duplicate Certifier error</span></span>
 * <span data-ttu-id="fdd78-186">Hello objektum adatok nélkül van jelölve, a más objektumokkal Lotus Domino-összekötő hello majd azt hibaüzenet hello felderítési teljes-importálás végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="fdd78-186">When hello Object without any data is selected on hello Lotus Domino Connector with other objects then we receive hello Discovery error while performing Full-Import.</span></span>
 * <span data-ttu-id="fdd78-187">Különbözeti importálás esetén a folyamatban futó hello Lotus Domino-összekötő, hogy futási, hello hello végén Microsoft.IdentityManagement.MA.LotusDomino.Service.exe szolgáltatás néha alkalmazás hibát ad vissza.</span><span class="sxs-lookup"><span data-stu-id="fdd78-187">When Delta Import is being running on hello Lotus Domino Connector, at hello end of that run, hello Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service sometimes returns an Application Error.</span></span>
 * <span data-ttu-id="fdd78-188">A csoporttagságot általános szolgáltatás megfelelően működik, és megmarad, azonban a felhasználó hello exportálás tootry tooremove futtatásakor tagság a sikeres frissítéshez látható, de hello felhasználó nem lesz beolvasása eltávolítva Lotus Notes tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="fdd78-188">Group membership overall works fine and is maintained, except when running hello export tootry tooremove a user from membership it shows as successful with an update, but hello user doesn’t actually get removed from membership in Lotus Notes.</span></span>
 * <span data-ttu-id="fdd78-189">Egy lehetőség toochoose módban "Append elemmel alul" Exportálás hozzá lett adva a konfigurációs Lotus grafikus felhasználói Felülettel MA tooappend új cikkek alján többértékű attribútumok hello exportálás során.</span><span class="sxs-lookup"><span data-stu-id="fdd78-189">An opportunity toochoose mode of export as “Append Item at bottom” was added in configuration GUI of Lotus MA tooappend new items at bottom during hello export for multi-valued attributes.</span></span>
 * <span data-ttu-id="fdd78-190">Összekötő hello szükséges logika toodelete hello fájlt az E-mail mappa hello és azonosító tároló adja hozzá.</span><span class="sxs-lookup"><span data-stu-id="fdd78-190">Connector will add hello needed logic toodelete hello file from hello Mail Folder and ID Vault.</span></span>
 * <span data-ttu-id="fdd78-191">Törölje a tagság közötti NAB tag nem működik.</span><span class="sxs-lookup"><span data-stu-id="fdd78-191">Delete membership not working for cross NAB member.</span></span>
 * <span data-ttu-id="fdd78-192">Értékek sikeresen törlődjenek többértékű attribútum</span><span class="sxs-lookup"><span data-stu-id="fdd78-192">Values should be successfully deleted from multi-valued attribute</span></span>

## <a name="111170"></a><span data-ttu-id="fdd78-193">1.1.117.0</span><span class="sxs-lookup"><span data-stu-id="fdd78-193">1.1.117.0</span></span>
<span data-ttu-id="fdd78-194">Kiadás dátuma: 2016. március</span><span class="sxs-lookup"><span data-stu-id="fdd78-194">Released: 2016 March</span></span>

<span data-ttu-id="fdd78-195">**Új összekötő**</span><span class="sxs-lookup"><span data-stu-id="fdd78-195">**New Connector**</span></span>  
<span data-ttu-id="fdd78-196">A kezdeti hello kiadása [általános SQL-összekötő](active-directory-aadconnectsync-connector-genericsql.md).</span><span class="sxs-lookup"><span data-stu-id="fdd78-196">Initial release of hello [Generic SQL Connector](active-directory-aadconnectsync-connector-genericsql.md).</span></span>

<span data-ttu-id="fdd78-197">**Új szolgáltatások:**</span><span class="sxs-lookup"><span data-stu-id="fdd78-197">**New features:**</span></span>

* <span data-ttu-id="fdd78-198">Általános LDAP-összekötőhöz:</span><span class="sxs-lookup"><span data-stu-id="fdd78-198">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="fdd78-199">Különbözeti importálás Isode való támogatása.</span><span class="sxs-lookup"><span data-stu-id="fdd78-199">Added support for delta import with Isode.</span></span>
* <span data-ttu-id="fdd78-200">Web Services-összekötővel:</span><span class="sxs-lookup"><span data-stu-id="fdd78-200">Web Services Connector:</span></span>
  * <span data-ttu-id="fdd78-201">Frissített hello csEntryChangeResult tevékenység és setImportErrorCode tevékenység tooallow objektum szintű hibák toobe visszaadott hátsó toohello szinkronizálási motor.</span><span class="sxs-lookup"><span data-stu-id="fdd78-201">Updated hello csEntryChangeResult activity and setImportErrorCode activity tooallow object level errors toobe returned back toohello sync engine.</span></span>
  * <span data-ttu-id="fdd78-202">Frissített hello SAP6 és SAP6User sablonok toouse hello új objektum szint hiba funkcióit.</span><span class="sxs-lookup"><span data-stu-id="fdd78-202">Updated hello SAP6 and SAP6User templates toouse hello new object level error functionality.</span></span>
* <span data-ttu-id="fdd78-203">Lotus Domino-összekötő:</span><span class="sxs-lookup"><span data-stu-id="fdd78-203">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="fdd78-204">Exportálás van szüksége egy certifier címjegyzék száma.</span><span class="sxs-lookup"><span data-stu-id="fdd78-204">For export, you need one certifier per address book.</span></span> <span data-ttu-id="fdd78-205">Most használja az azonos hello is összes képesítést adók engedélyezése toomake hello felügyeleti könnyebb jelszavát.</span><span class="sxs-lookup"><span data-stu-id="fdd78-205">You can now use hello same password for all certifiers toomake hello management easier.</span></span>

<span data-ttu-id="fdd78-206">**Javított problémák:**</span><span class="sxs-lookup"><span data-stu-id="fdd78-206">**Fixed issues:**</span></span>

* <span data-ttu-id="fdd78-207">Általános LDAP-összekötőhöz:</span><span class="sxs-lookup"><span data-stu-id="fdd78-207">Generic LDAP Connector:</span></span>
  * <span data-ttu-id="fdd78-208">Az IBM Tivoli DS, néhány hivatkozási attribútum a rendszer nem észlelt megfelelően.</span><span class="sxs-lookup"><span data-stu-id="fdd78-208">For IBM Tivoli DS, some reference attributes were not detected correctly.</span></span>
  * <span data-ttu-id="fdd78-209">Nyissa meg az LDAP a különbözeti importálás során, a rendszer csonkolt hello elején és végén karakterláncok szóközöket.</span><span class="sxs-lookup"><span data-stu-id="fdd78-209">For Open LDAP during a delta import, whitespaces at hello beginning and end of strings were truncated.</span></span>
  * <span data-ttu-id="fdd78-210">Novell és NetIQ, objektum áthelyezése, szervezeti egységek/tárolók közötti és hello exportálás azonos időben átnevezett hello objektum sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fdd78-210">For Novell and NetIQ, an export that moved an object between OUs/containers and at hello same time renamed hello object failed.</span></span>
* <span data-ttu-id="fdd78-211">Web Services-összekötővel:</span><span class="sxs-lookup"><span data-stu-id="fdd78-211">Web Services Connector:</span></span>
  * <span data-ttu-id="fdd78-212">Ha hello webszolgáltatás ugyanezt a kötést a több végpontok közé, majd hello Connector nem megfelelően találta ezeket a végpontokat.</span><span class="sxs-lookup"><span data-stu-id="fdd78-212">If hello web service had multiple end-points for same binding, then hello Connector did not correctly discover these end-points.</span></span>
* <span data-ttu-id="fdd78-213">Lotus Domino-összekötő:</span><span class="sxs-lookup"><span data-stu-id="fdd78-213">Lotus Domino Connector:</span></span>
  * <span data-ttu-id="fdd78-214">Hello fullName attribútum tooa mail-adatbázisban exportálása sikertelen volt.</span><span class="sxs-lookup"><span data-stu-id="fdd78-214">An export of hello fullName attribute tooa mail-in database did not work.</span></span>
  * <span data-ttu-id="fdd78-215">Egy exportálás, amely egyaránt felvétele, illetve tag eltávolítása egy csoportból, csak exportált hello hozzáadott tagok.</span><span class="sxs-lookup"><span data-stu-id="fdd78-215">An export which both added and removed member from a group only exported hello added members.</span></span>
  * <span data-ttu-id="fdd78-216">Ha egy megjegyzések dokumentum érvénytelen (hello attribútum isValid toofalse beállítása), majd hello összekötő sikertelen.</span><span class="sxs-lookup"><span data-stu-id="fdd78-216">If a Notes Document is invalid (hello attribute isValid set toofalse), then hello Connector fails.</span></span>

## <a name="older-releases"></a><span data-ttu-id="fdd78-217">Régebbi kiadásai</span><span class="sxs-lookup"><span data-stu-id="fdd78-217">Older releases</span></span>
<span data-ttu-id="fdd78-218">2016. március, mielőtt hello összekötők kiadott támogatási témakörök szerint.</span><span class="sxs-lookup"><span data-stu-id="fdd78-218">Before March 2016, hello Connectors were released as support topics.</span></span>

<span data-ttu-id="fdd78-219">**Általános LDAP**</span><span class="sxs-lookup"><span data-stu-id="fdd78-219">**Generic LDAP**</span></span>

* <span data-ttu-id="fdd78-220">[KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, 2015. szeptember</span><span class="sxs-lookup"><span data-stu-id="fdd78-220">[KB3078617](https://support.microsoft.com/kb/3078617) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="fdd78-221">[KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, 2015. március</span><span class="sxs-lookup"><span data-stu-id="fdd78-221">[KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="fdd78-222">[KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, 2015. január</span><span class="sxs-lookup"><span data-stu-id="fdd78-222">[KB3031009](https://support.microsoft.com/kb/3031009) - 1.0.0534, 2015 January</span></span>
* <span data-ttu-id="fdd78-223">[KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, 2014. szeptember</span><span class="sxs-lookup"><span data-stu-id="fdd78-223">[KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September</span></span>
* <span data-ttu-id="fdd78-224">[KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, 2014. március</span><span class="sxs-lookup"><span data-stu-id="fdd78-224">[KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March</span></span>

<span data-ttu-id="fdd78-225">**WebServices**</span><span class="sxs-lookup"><span data-stu-id="fdd78-225">**WebServices**</span></span>

* <span data-ttu-id="fdd78-226">[KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, 2014. szeptember</span><span class="sxs-lookup"><span data-stu-id="fdd78-226">[KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="fdd78-227">**PowerShell**</span><span class="sxs-lookup"><span data-stu-id="fdd78-227">**PowerShell**</span></span>

* <span data-ttu-id="fdd78-228">[KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, 2014. szeptember</span><span class="sxs-lookup"><span data-stu-id="fdd78-228">[KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September</span></span>

<span data-ttu-id="fdd78-229">**Lotus Domino**</span><span class="sxs-lookup"><span data-stu-id="fdd78-229">**Lotus Domino**</span></span>

* <span data-ttu-id="fdd78-230">[KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, 2015. szeptember</span><span class="sxs-lookup"><span data-stu-id="fdd78-230">[KB3096533](https://support.microsoft.com/kb/3096533) - 1.0.0597, 2015 September</span></span>
* <span data-ttu-id="fdd78-231">[KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, 2015. március</span><span class="sxs-lookup"><span data-stu-id="fdd78-231">[KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March</span></span>
* <span data-ttu-id="fdd78-232">[KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014. augusztus</span><span class="sxs-lookup"><span data-stu-id="fdd78-232">[KB2977286](https://support.microsoft.com/kb/2977286) - 5.3.0712, 2014 August</span></span>
* <span data-ttu-id="fdd78-233">[KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, 2014. február</span><span class="sxs-lookup"><span data-stu-id="fdd78-233">[KB2932635](https://support.microsoft.com/kb/2932635) - 5.3.1003, 2014 February</span></span>  
* <span data-ttu-id="fdd78-234">[KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, 2013. október</span><span class="sxs-lookup"><span data-stu-id="fdd78-234">[KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October</span></span>
* <span data-ttu-id="fdd78-235">[KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013. augusztus</span><span class="sxs-lookup"><span data-stu-id="fdd78-235">[KB2875551](https://support.microsoft.com/kb/2875551) - 5.3.0534, 2013 August</span></span>

## <a name="next-steps"></a><span data-ttu-id="fdd78-236">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="fdd78-236">Next steps</span></span>
<span data-ttu-id="fdd78-237">További tudnivalók hello [az Azure AD Connect szinkronizálási szolgáltatás](active-directory-aadconnectsync-whatis.md) konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="fdd78-237">Learn more about hello [Azure AD Connect sync](active-directory-aadconnectsync-whatis.md) configuration.</span></span>

<span data-ttu-id="fdd78-238">További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).</span><span class="sxs-lookup"><span data-stu-id="fdd78-238">Learn more about [Integrating your on-premises identities with Azure Active Directory](active-directory-aadconnect.md).</span></span>
