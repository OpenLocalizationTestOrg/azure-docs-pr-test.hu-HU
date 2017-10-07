---
title: "aaaWhat által az Azure Data Catalog |} Microsoft Docs"
description: "Ez a cikk ismerteti, új funkcióiról a Data Catalog tooAzure hozzá."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 1201f8d4-6f26-4182-af3f-91e758a12303
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/22/2017
ms.author: maroche
ms.openlocfilehash: f60130487ece39e110446b68544945089d2ab37e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="whats-new-in-azure-data-catalog"></a>What's new in Azure Data Catalog
Frissítések túl**Azure Data Catalog** rendszeresen kiadásakor. Nem minden kiadás szolgáltatásai új felhasználók számára is elérhető, néhány kiadásokban a háttér-szolgáltatás jellemzőinek lévőként. Ezen az oldalon az új felhasználók számára is elérhető képességek hozzáadott toohello Azure Data Catalog szolgáltatás mutatja be.

## <a name="whats-new-for-august-2017"></a>Új 2017. augusztus 
Augusztus 2017 hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

*   Egy új fejlesztői minta létrehozásával és kezelésével kapcsolatban metaadatok hello Data Catalog REST API használatával érhető el. Hello *kapcsolatadatokat importálnia kell a Data Catalog* minta érhető el hello [Data Catalog minták kódlap](https://azure.microsoft.com/resources/samples/?service=data-catalog&sort=0). 
* Kibontása támogatása csatlakoztassa a kapcsolat metaadatai a Teradata-adatforrások hello adatforrás-regisztráló eszköz használatával, a kapcsolódó táblák regisztrálásakor.
* Támogatja az SQL Server táblázat értékű függvényhez (TVF) objektumok regisztrálhatja az SQL Server-adatforrásait hello adatforrás-regisztráló eszköz használatával.
* Több frissítés és finomításokra tooincrease hello teljesítményük és használhatóságuk hello Data Catalog-portál.

## <a name="whats-new-for-july-2017"></a>Új 2017. július 
2017. júliusi a következő képességeket hello rendelkezik lett hozzáadva tooAzure Data Catalog:
*   Részletesebben vezérelhető, beleértve lehetővé teszik metaadat-művelet keresztül támogatása:
    - Katalógus-rendszergazdák korlátozhatja a felhasználói lehetőséget toocontribute címkék és kapcsolódó metaadatok toohello katalógus, csak olvasási hozzáféréssel toohello katalógus engedélyezése.
    - Katalógus-rendszergazdák korlátozhatja a felhasználók képességét tooregister új adatforrások hello katalógusban.
    - Katalógus-rendszergazdák korlátozhatja a felhasználók képességét tootake tulajdonjogát adatok eszköz metaadatok hello katalógusban.
    - Hozzárendelhető tooAzure Active Directory biztonsági csoportok és felhasználók engedélyek kezelésének megkönnyítése érdekében.
* Regisztrált adategységeket és hello Data Catalog-portált, felderítő kapcsolódó adatok eszközök közötti kapcsolatok támogatása többek között:
    - Kibontási kapcsolat metaadatok (beleértve az Azure SQL Database) SQL Server, az Oracle és a MySQL használata esetén az adatforrások hello Data Catalog adatforrás-regisztráló eszköz.
    - Felderítési kapcsolódó adategységet hello Data Catalog-portált az eszköz metaadatok megtekintésekor.
    - Műveletek toodefine felderíteni, és kezelni hello Data Catalog REST API használatával adatok eszközök közötti kapcsolatokat.

További engedélyek a Data Catalog kezeléséről további információkért lásd: [hogyan férhetnek hozzá a toosecure toodata katalógust és az adatok eszközök](data-catalog-how-to-secure-catalog.md).
A Data Catalog kapcsolatoknál további részletekért lásd: [hogyan tooview kapcsolódó adategységeket az Azure Data Catalog](data-catalog-how-to-view-related-data-assets.md).

## <a name="whats-new-for-june-2017"></a>2017. júniusi Újdonságok 
2017. június a következő képességeket hello rendelkezik lett hozzáadva tooAzure Data Catalog:
*   Támogatja a Sybase, Apache Cassandra és MongoDB adatforrásokat. A felhasználók most regisztrálni és Cassandra és MongoDB adatbázisok és táblák és Sybase-adatbázisok, táblák és nézetek felderítése.

> [!NOTE]
> Ha regisztrálja az például rögzíti és tömböket összetett adattípusú oszlopok, ezeket az oszlopokat tartalmazó táblák MongoDB és Cassandra nem szerepelni fog a hello metaadatok hozzáadott tooData katalógus.

## <a name="whats-new-for-may-2017"></a>Előfordulhat, hogy 2017 újdonságai 
Előfordulhat, hogy 2017 hello a következő képességekkel rendelkezik lett hozzáadva tooAzure Data Catalog:
*   Egy új fejlesztői minta hello tömeges importálásához üzleti szószedet kifejezések érhető el. hello szószedet tömeges importálással minta érhető el hello [Data Catalog fejlesztői minták lap](https://docs.microsoft.com/azure/data-catalog/data-catalog-samples). 
*   Támogatási ODBC kapcsolati információit hello Azure Data Catalog-portál szerkesztésre. Adatok eszköz tulajdonosait, és a Data Catalog rendszergazdák is szerkesztésével toore-nyilvántartás hello adatforrások anélkül hello regisztrált ODBC-adatforrás-kapcsolódási információt.
*   Az üzleti szószedet és kattintható URL-címek támogatása. Amikor URL-címek üzleti szószedet feltételek hello leírását és definíció tulajdonságai szerepelnek, hello URL-címeket a Data Catalog-portál hello hivatkozásként jelenik meg.
*   Támogatási szakértő megjelenő nevet továbbá tooexpert e-mail címet. Egy adategységet hello tulajdonságaiban szakértők hello Data Catalog-portált, megtekintésekor hello szakértő teljes nevét az Azure Active Directory hello elemleírás jelenik meg.

## <a name="whats-new-for-april-2017"></a>Új 2017. április 
2017. április a következő képességeket hello rendelkezik lett hozzáadva a Data Catalog tooAzure:
*   ODBC adatforrások támogatása. A felhasználók most regisztrálni és Fedezze fel az ODBC adatbázisok, táblák és nézetek hello Data Catalog adatforrás-regisztráló eszköz használatával.

## <a name="whats-new-for-march-2017"></a>Új március 2017 
2017. március a következő képességeket hello rendelkezik lett hozzáadva tooAzure Data Catalog:
*   AAD-biztonságicsoportok használata a Data Catalog rendszergazdák támogatása.
*   Az Azure Data Catalog mostantól egy új Azure-régiót. Az ügyfelek hozhat létre Azure Data Catalog régióban hello nyugati középső Régiójában, továbbá tooEast USA, USA nyugati régiója, Nyugat-Európában, és Kelet-Ausztrália, Észak-Európában és Délkelet-Ázsiában található hello. További információkért lásd: [Azure-régiókat](https://azure.microsoft.com/regions/).

## <a name="whats-new-for-february-2017"></a>Új 2017. február 
2017. február a következő képességeket hello rendelkezik lett hozzáadva tooAzure Data Catalog:
*   Speciális beállítások hello Data Catalog adatforrás-regisztráló eszköz támogatása. Felhasználók parancs időkorlátja értékeket adhat meg, amikor nagy adatforrások regisztrálása.
*   FTP-HELYET és PostgreSQL adatforrások támogatása. A felhasználók most már regisztrálása és az FTP-fájlok és könyvtárak, és PostgreSQL adatbázisok, táblák és nézetek felderítése.

## <a name="whats-new-for-january-2017"></a>Új 2017. január 
2017. január a következő képességeket hello rendelkezik lett hozzáadva a Data Catalog tooAzure:
*   Az Azure Data Catalog mostantól [CSA csillag](https://www.microsoft.com/trustcenter/compliance/csa-star-certification) kompatibilis.
*   Integráció a [beszerzése és az Excel 2016 és a Power Query az Excel alakítás](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605). Excel felhasználók lekérdezések megosztása és Fedezze fel az Azure Data Catalog használatával lekérdezéseket az Excel belül. Ez a funkció a Power BI Pro-licenccel rendelkező elérhető toousers.

## <a name="whats-new-for-december-2016"></a>Újdonságok 2016. December
2016. December a következő képességeket hello bővült tooAzure Data Catalog:
*   Az Azure Data Catalog mostantól [HIPAA](https://www.microsoft.com/trustcenter/Compliance/HIPAA) és [EU Modellcikkelyeknek](https://www.microsoft.com/TrustCenter/Compliance/EU-Model-Clauses) kompatibilis.
*   Támogatási szerkesztését az adatforrás kapcsolódási adatait. Adatok eszköz tulajdonosait, és a Data Catalog rendszergazdák is szerkesztésével toore-nyilvántartás hello adatforrások anélkül hello regisztrált adatforrás-kapcsolódási információt.
*   Támogatás a Salesforce.com-adatforrásokhoz. A felhasználók most regisztrálni és Salesforce objektumok felderítése.


## <a name="whats-new-for-november-2016"></a>Újdonságok 2016. November
November 2016 hello a következő képességekkel bővült tooAzure Data Catalog:
*   Az Azure Data Catalog mostantól [ISO/IEC 27001](https://www.microsoft.com/trustcenter/compliance/iso-iec-27001) és [ISO/IEC 27018](https://www.microsoft.com/TrustCenter/Compliance/iso-iec-27018) kompatibilis.
*   ODBC adatforrások hello Data Catalog-portál és a REST API használatával manuális regisztrációja hello támogatása.

## <a name="whats-new-for-september-2016"></a>2016 szeptemberétől újdonságai
2016 szeptemberétől a következő képességeket hello rendelkezik lett hozzáadva a Data Catalog tooAzure:

* IBM DB2 adatforrások támogatása. A felhasználók most regisztrálni és DB2-adatbázisok, a táblák és nézetek felderítése.
* Támogatás az Azure Cosmos DB-adatforrásokhoz. A felhasználók most regisztrálni és Cosmos DB adatbázisok és gyűjteményeket.
* Hello katalógusnév hello Data Catalog-portál testreszabása támogatása. Katalógus-rendszergazdák mostantól biztosíthat hello portál nevét, például hello szervezetnév megjelenő szöveg.

## <a name="whats-new-for-august-2016"></a>2016 augusztusától újdonságai
2016 augusztusától hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

* Az SQL Server Master Data Services (MDS) adatforrások regisztrálási fejlesztései. Felhasználók is között már elérhető az előzetes verziójú funkciók és az adatok profilok regisztrálhatja az MDS-entitások hello Data Catalog adatforrás-regisztráló eszköz használatával.
* Rendszergazda által meghatározott szervezeti mentett keresések támogatása. Keresés a Data Catalog-portál hello mentésekor a Data Catalog rendszergazdák választhat a toosave keresések személyes használatra, vagy a katalógus összes felhasználója számára. Szervezeti mentett keresések senki katalógus vannak megosztva, és a szabványos kezdőpontja biztosíthat az adatforrás-felderítés.
* A Data Catalog-portál hello toohello tulajdonságai nézet frissíti. Minden eszköz tulajdonságai jelennek meg, és egy egységes, méretezhető ablak tooprovide egységesebb és felderíthető élmény kezelhetők.

## <a name="whats-new-for-july-2016"></a>Újdonságok 2016. július
2016. július a következő képességeket hello bővült tooAzure Data Catalog:

* SQL Server Master Data Services (MDS) adatforrások támogatása. A felhasználók most regisztrálni és Fedezze fel az MDS-modellek és entitások.
* Támogatás az SQL Server tárolt eljárást. A felhasználók most már regisztrálása és az SQL Server-adatforrásait tárolt eljárás objektumok felderítése.
* További nyelvek hello Azure Data Catalog-portál és hello adatforrás-regisztráló eszköz, összesen 18 támogatott nyelvek támogatása. hello Azure Data Catalog felhasználói élmény honosítva van, vagy hello webböngészőben a Windows hello nyelvi beállítások.
* Frissítések és a hello Data Catalog portál kezdőlapján, beleértve a teljesítménnyel kapcsolatos fejlesztések és zökkenőmentes felhasználói élmény pontosítás.

## <a name="whats-new-for-june-2016"></a>2016. júniusi Újdonságok
2016. június a következő képességeket hello bővült tooAzure Data Catalog:

* Hello lista nézet oszlopainak átméretezéséhez, amikor adategységek hello Data Catalog-portál a támogatási szolgálathoz. Mostantól a felhasználók átméretezhetik egyedi oszlop toomore könnyen olvasható hosszadalmas eszköz metaadatokat, például címkék és leírásokat.
* A Power Query az Excel programhoz hozzá lett adva toohello "Megnyitás" menü hello Data Catalog-portált. Felhasználók is most nyissa meg a támogatott adatforrások Excel 2016 vagy az Excel 2010 és az Excel 2013 hello [Power Query az Excel](https://support.office.com/article/Introduction-to-Microsoft-Power-Query-for-Excel-6E92E2F4-2079-4E1F-BAD5-89F6269CD605) bővítményt.
* Támogatás az Azure Table Storage adatforrások. A felhasználók most regisztrálása és az Azure Storage adatforrások tábla objektumok felderítése.

## <a name="whats-new-for-may-2016"></a>Újdonságok 2016. május
2016. május a következő képességeket hello bővült tooAzure Data Catalog:

* Egy üzleti szószedetet, amely lehetővé teszi, hogy a katalógus-rendszergazdák toodefine üzleti feltételek és a hierarchiák toocreate egy közös üzleti szószedet. Felhasználók címkézheti szószedet feltételek toomake regisztrált adategységeket az egyszerűbb toodiscover és értelmezhetővé hello katalógus hello tartalmát. További információkért lásd: [hogyan mentése tooset hello szabályozott címkézésre üzleti szószedet](data-catalog-how-to-business-glossary.md)  
* Továbbfejlesztett toohello Data Catalog üzleti szószedet, amely lehetővé teszi a felhasználók tooupdate több szószedetben egyetlen műveletben. A következő mezők több feltételek tooedit hello bejelölésével:
  * Szülő kifejezés: hello felhasználó kiválaszthatja a kívánt új szülő kifejezést, és minden kiválasztott feltételek frissített toobe gyermekek hello kijelölt szülő kifejezés. Mindegyik rendelkezik feltételek hello választásakor hello ugyanazon szülő, majd, hogy a szülő megjelenik-e a hello szövegmező, ellenkező esetben hello kifejezés mező set tooblank.   
  * Címkék és az érdekelt felekkel: felhasználók hozzáadása és távolítsa el a címkéket és az érdekelt felek több, azonos élmény hello használatával egyszerre több adategységet címkézés szószedet feltételek.

> [!NOTE]
> hello üzleti szószedet csak a Standard Edition Azure Data Catalog hello érhető el. hello ingyenes kiadás nem biztosít az irányadó címkézéssel, illetve egy üzleti szószedetet képességeit.

## <a name="whats-new-for-march-2016"></a>Újdonságok 2016. március
2016. március a következő képességeket hello bővült tooAzure Data Catalog: g:

* Egy összevont REST API-végpont programozott módon a hello keresési képességek és a katalógus-kezelési képességei eszköz hello Azure Data Catalog szolgáltatás eléréséhez. A keresési API-végpont és a katalógus API-végpont lett elavult, és nem támogatja a 2016. március 21-én. Nincsenek módosítások hello API toohello szemantikáját. Csak hello végpont URI azonosító módosította. További információkért lásd: hello [Azure Data Catalog REST API-referencia](https://msdn.microsoft.com/library/azure/mt267595.aspx). API-mintákat, lásd: [Azure Data Catalog fejlesztői minták](data-catalog-samples.md).

## <a name="whats-new-for-february-2016"></a>Újdonságok 2016. február
2016. február a következő képességeket hello bővült tooAzure Data Catalog:

* Egy újonnan áttervezett forrás kiválasztása hello Azure Data Catalog adatforrás-regisztráló eszköz a felhasználói élmény. hello adatforrás-regisztráló eszköz lett frissített toomake informatikai egyszerűbb, toolocate és válassza ki a hello adatforrások támogatott Azure Data Catalog által.
* Hello Azure Data Catalog-portál és az adatforrás-regisztráló eszköz hello 10 további nyelvek támogatásával. Ezenkívül tooEnglish, hello Azure Data Catalog élmény már elérhető a német, spanyol, francia, olasz, japán, koreai, brazíliai portugál, orosz, egyszerűsített és hagyományos kínai. hello Azure Data Catalog felhasználói élmény honosítva van vagy hello felhasználó böngészőben a Windows hello nyelvi beállításai alapján.
* Az Azure Data Catalog adatok az üzleti folytonossági és vészhelyreállítási helyreállítási georeplikáció támogatása. Minden Azure Data Catalog tartalma, beleértve az adatok forrása metaadatok és a közösségi jegyzeteket, most nem jelent többletköltséget toocustomers, két Azure-régiók közötti replikálódnak. hello Azure-régiók előre van párosítva, legalább 500 miles egymástól, és kövesse hello leképezés a [üzleti folytonossági és vészhelyreállítási helyreállítási (BCDR): Azure-régiókat párosítva](../best-practices-availability-paired-regions.md).
* Hello Azure Data Catalog által használt Azure-előfizetés módosítása támogatása. Az Azure Data Catalog rendszergazdák számlázási célokra használható hello Azure Data Catalog-portál tooselect egy másik Azure-előfizetés hello beállítások lapon.

## <a name="whats-new-for-january-2016"></a>Újdonságok 2016. január
2016. január a következő képességeket hello bővült tooAzure Data Catalog:

* Manuálisan regisztrálja a további adatforrások támogatása. Most "Manuális bejegyzés létrehozása" hello Azure Data Catalog-portált használja, vagy használja a következő adatforrások hello Azure Data Catalog REST API tooregister hello:
  * Az OData - függvény, Entitáskészletet és Entitástároló
  * HTTP - fájl, a végpont, a jelentés és a hely
  * Fájlrendszer - fájl
  * SharePoint - lista
  * FTP - fájlokra és könyvtárakra
  * A Salesforce.com - objektum
  * DB2 - tábla, nézet és adatbázis
  * PostgreSQL - tábla, nézet és adatbázis
* "Nyissa meg az SQL Server Data Tools" támogatása (beleértve az Azure SQL Database és Azure SQL Data Warehouse) SQL Server-adatforrásait.  
* Regisztrálja, és felderíti az SAP HANA-nézetek és csomagok támogatását. SAP HANA adatok hello Azure Data Catalog adatokat használó adatforrás-regisztráló eszközzel és is megjegyzésekkel és hello Azure Data Catalog-portál használatával regisztrált SAP HANA adatforrások felfedezése tud regisztrálni.
* képes toopin hello és rögzítését adategységhez a hello Azure Data Catalog-portált. Toopin adatok eszközök toomake kiválaszthatja azokat könnyebben toorediscover és újbóli.
* Egy újonnan újratervezett kezdőlap hello Azure Data Catalog-portál. hello új kezdőlap tartalmazza hello aktuális felhasználók tevékenység - nemrégiben publikált eszközök, beleértve a rögzített eszközök, és a mentett kereséseket - betekintést és a hello tevékenység betekintést hello katalógus egész.
* Hello Azure Data Catalog portál állandó felhasználói beállítások támogatása. Felhasználói élményt befolyásoló beállításait – többek között a következőket rács vagy Mozaik elrendezés nézetben, hello laponként eredmények számát, majd nyomja le Kiemelés a be- és kikapcsolása - megmaradnak a felhasználói munkamenetek között.
* Az Azure Data Catalog már elérhető két új Azure-régió. Az ügyfelek hello régiókban hello Észak-Európában és Délkelet-Ázsiában, továbbá tooEast USA, Nyugat-Európa, Kelet-Ausztrália, és az USA nyugati régiója, az Azure Data Catalog hozhat létre. További információkért lásd: [Azure-régiókat](https://azure.microsoft.com/regions/).

> [!NOTE]
> "Nyissa meg az SQL Server Data Tools" igényel a Visual Studio 2013 Update 4 és az SQL Server tooling eszköz toobe telepítve a. az SQL Server Data Tools tooinstall hello letöltéséhez látogasson el [letöltése SQL Server Data Tools (SSDT)](https://msdn.microsoft.com/library/mt204009.aspx).


## <a name="whats-new-for-december-2015"></a>2015. decemberi újdonságai
2015 decemberében hello a következő képességekkel bővült tooAzure Data Catalog:

* Azure SQL Data Warehouse-adatforrásokhoz tartozó adatok profilok támogatása. Azure SQL Data Warehouse-táblák és nézetek regisztrálásakor a felhasználók eldönthetik tooinclude adatok profil metrikák hello adatforrás kinyert hello metaadatokkal.
* Regisztrálja, és felderíti az MySQL objektumok és adatbázisok támogatása. A felhasználók regisztrálhatják MySQL adatok hello Azure Data Catalog adatokat használó adatforrás-regisztráló eszközzel és is megjegyzésekkel és hello Azure Data Catalog-portál használatával regisztrált MySQL adatforrások felderítésére.
* A Teradata-adatforrásokhoz SPNEGO és Windows hitelesítés támogatása. Amikor regisztrál a Teradata-táblák és nézetek, a felhasználók maguk dönthetik tooconnect tooTeradata SPNEGO és a Windows, és az LDAP és a TD2 hitelesítést használ.
* Azure Data Lake Store adatforrások támogatása. A felhasználók most regisztrálni és Azure Data Catalog használatával az Azure Data Lake Store-adatforrások felderítését.
* Manuálisan adja meg hálózati proxybeállításai hello Azure Data Catalog adatforrás-regisztráló eszköz támogatása. Felhasználók "Módosítás proxybeállítások" hello eszköz üdvözlőoldal választja ki, és megadhatja a hello proxy címe és portja toobe hello eszköz által használt.

## <a name="whats-new-for-november-2015"></a>Újdonságok a 2015. November
2015. November hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

* képes tooview hello, és másolja a kapcsolati karakterláncok az Azure Data Catalog-portál hello belül (beleértve az Azure SQL Database) SQL Server és Oracle-adatforrásokat. Felhasználók egy SQL Server-kapcsolódási információt hello "View kapcsolódási karakterláncok" hivatkozásra kattinthat, vagy Oracle tábla, nézet vagy adatbázis, toosee hello kapcsolati karakterláncok használt tooconnect toohello adatforrás. SQL Server-adatforrásait ADO.NET, ODBC, OLEDB és JDBC kapcsolati karakterláncok célokat szolgálnak. ODBC és OLEDB kapcsolati karakterláncok Oracle-adatforrások célokat szolgálnak.
* Támogatja az adatok profilok például Teradata-táblák és nézetek regisztrálásakor.
* Támogatása "Nyissa meg a Power BI Desktop" SQL Server (beleértve az Azure SQL Database és Azure SQL Data Warehouse), az SQL Server Analysis Services, Azure Storage és HDFS források.  
* A Teradata-adatforrásokhoz LDAP-hitelesítés támogatása. Regisztrálás a Teradata-táblák és nézetek, amikor a felhasználók maguk dönthetik tooconnect tooTeradata LDAP, segítségével és TD2 hitelesítést.
* "Megnyitás az Excel" támogatása a Teradata-adatforrásokhoz.
* Legutóbbi keresőkifejezéseket hello Azure Data Catalog portálon támogatása. Hello portálon keresésekor felhasználók legutóbb használt keresési feltételek tooaccelerate hello feltárási folyamata is kijelölhet.
* Előzetes Teradata adatforrások támogatása. Teradata-táblák és nézetek regisztrálásakor a felhasználók eldönthetik tooinclude pillanatkép rekordok hello adatforrás kinyert hello metaadatokkal.
* Az Azure SQL Data Warehouse adatforrásokat "Megnyitás az Excel" támogatása.
* Támogatja a meghatározása, és manuálisan regisztrált adategységeket oszlopszintű sémái szerkesztése. Után manuálisan hoz létre egy adategységet hello Azure Data Catalog-portált használja, a felhasználók hozzáadhatnak oszlopdefiníciók hello adatok eszköz tulajdonságai.
* Támogatás az Azure Data Catalog, az adott metaadatait megkapnák regisztrált adategységeket tooenable hello felderítése keresésekor "tartalmaz" lekérdezések. Most már tartalmazza az Azure Data Catalog-lekérdezés szintaxisa:

| Lekérdezés szintaxisa | Cél |
| --- | --- |
| `has:previews` |Megkeresi a adategységeiről, amelyeket az előképet |
| `has:documentation` |Megkeresi az adategységek, amelynek dokumentáció adtak meg |
| `has:tableDataProfiles` |Megkeresi a tábla szintű adatok profiladatok az adategységeket |
| `has:columnsDataProfiles` |Megkeresi az adategységeket oszlopszintű adatok profiladatok |


> [!NOTE]
> "Megnyitás Power BI Desktop a" hello Power BI Desktop alkalmazás toobe telepített jelenlegi verziója szükséges. Ha problémákat vagy hibákat ezzel a szolgáltatással, gondoskodjon arról, hogy hello legújabb verzióját a Power BI Desktop [powerbi.com webhelyen](https://powerbi.com).


## <a name="whats-new-for-october-2015"></a>Újdonságok a 2015 október
A 2015 október hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

* Az előzetes verziójú funkciók és az adatok profilokat regisztrált adatforrások támogatják az adatok titkosítását. Az Azure Data Catalog transzparens módon titkosítja az előzetes rekordokat, és adatok profilok hello szolgáltatásban, nem kell a kulcskezeléshez katalógus-rendszergazdák által regisztrált adatforrások.
* Támogatás a Teradata-adatforrásokhoz. A felhasználók most regisztrálni és Teradata-táblák és nézetek felderítése.
* A helyszíni Hive adatforrások támogatása. A felhasználók most regisztrálni és Hive táblák felderíteni az Apache Hadoop a helyszíni adatforrások Hive.
* Mentett keresések hello Azure Data Catalog portálon támogatása. Felhasználók menthetik a keresési feltételek és szűrési beállítások tooeasily ismételje meg az előző keresések és hello katalógus tartalma hasznos nézeteinek definiálásához. Felhasználó, az alapértelmezett keresési is jelölheti meg a mentett kereséseket. Ha a felhasználó hello Azure Data Catalog-portál kezdőlapján, illetve hello "első lépések" lap "nagyítóüveg" hello keresés ikonra kattint, hello felhasználói lesz végrehajtva közvetlenül toohello mentett keresés megjelölve alapértelmezettként.
* Rich text formátumú dokumentáció regisztrált adategységeket és hello Azure Data Catalog-portál a tárolók támogatása. Felhasználók mostantól biztosíthat dokumentáció adatok erőforrásokat, például a táblákat, nézeteket és jelentéseket, valamint a tárolókhoz, például adatbázisok és -modellek, forgatókönyvek, amelyben címkék és leírásait is nem megfelelő.
* Manuálisan regisztrálja az ismert adatforrásokat támogatása. Felhasználó manuálisan adhat meg az adatforrásra vonatkozó információ hello Azure Data Catalog-portál használata az Azure Data Catalog által támogatott összes adatforrásokat.
* Támogatás, amelyek engedélyezik az Azure Active Directory biztonsági csoportokat. Katalógus-rendszergazdák hozzáférési toosecurity katalóguscsoportokat, a felhasználói fiókokat, így könnyebben toomanage hozzáférés tooAzure Data Catalog használatával engedélyezheti.
* Hive adatforrások hello Azure Data Catalog portálról ellenőrzéséhez nyissa meg Excel támogatása.

> [!NOTE]
> Hello a jelenlegi kiadásban csak a Teradata TD2 hitelesítést is támogatja. Feloldja a további hitelesítési mechanizmusok támogatottak a jövőben.

> [!NOTE]
> toouse hello "Megnyitás az Excel" funkció Hive adatforrások, felhasználók kell telepített ODBC-illesztőprogram hello a Hive.

## <a name="whats-new-for-september-2015"></a>Újdonságok a 2015. szeptember
2015. szeptember hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

* Hive-adatforrás regisztrálásakor, többek között adatok profilok támogatása.
* Programozott módon felderítéséhez hello Catalog API, ami megkönnyíti az Azure Data Catalog alkalmazások toointegrate támogatása.
* Egy új "első lépések" az adatforrás-felderítés élményt hello Azure Data Catalog-portált. Amikor a felhasználók hello "felderítése" oldalán hello Azure Data Catalog-portál a keresett kifejezés beírása nélkül adja meg, jelenjenek meg ezek hello katalógus gyökeret is beleértve, a leggyakrabban használt hello címkék, szakértők, adatforrásokat és objektumtípusok áttekintést.
* Regisztrálja, és felderíti az Azure SQL Data Warehouse-objektumok és adatbázisok támogatása. További információ az Azure SQL Data Warehouse: [SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
* Regisztrálja, és felderíti az SQL Server Analysis Services-modellek és tárolóként SQL Server Reporting Services-kiszolgálók támogatása. Az SSAS és az SSRS objektumok regisztrálásakor Azure Data Catalog bejegyzést hoz létre a hello SSAS modellhez és az SSRS-kiszolgáló, valamint a hello jelentések és egyéb objektumok. hello tárolók könnyen megtalálhatók legyenek, és jegyzetelve hello Azure Data Catalog-portál használatával. A felhasználók is keresés és szűrés hello tartalmát egy modell vagy a kiszolgáló hozzáadását toosearching és szűrés hello katalógus hello tartalmát.
* Regisztráció és a HTTP/HTTPS kapcsolaton az SQL Server Analysis Services felderítéséhez támogatása. Felhasználók most már képes kapcsolódni a tooSSAS kiszolgálókkal (például https://servername/olap/msmdpump.dll) URL-cím segítségével ahelyett, hogy a kiszolgáló nevét, és az egyszerű hitelesítés és anonim felhasználói kapcsolatok hozzáadása tooWindows hitelesítést. A HTTP/HTTPS-kapcsolatok tooSSAS további információkért lásd: [HTTP elérésének konfigurálása tooAnalysis szolgáltatások](https://msdn.microsoft.com/library/gg492140.aspx).
* A HDInsight Hive adatforrások támogatása. A felhasználók most már regisztrálása és az Apache Hive az adatforrásokban HDInsight Hadoop Hive táblák felderíteni. A HDInsight Hive további információkért lásd: hello [HDInsight dokumentációs központban](../hdinsight/hdinsight-use-hive.md).
* Regisztrálja, és felderíti az Oracle-adatbázis és a HDFS-fürt tárolóként támogatása. Oracle-táblák és nézetek vagy HDFS regisztrálásakor az Azure Data Catalog hello adatbázis, a táblák és nézetek bejegyzést hoz létre. hello adatbázis felderíthető és jegyzetelve hello Azure Data Catalog-portál használatával. A felhasználók is keresés és szűrés hello adatbázis tartalmát, vagy a fürt ezen felül a toosearching és a szűrés hello katalógus hello tartalmát.
* Manuálisan regisztrálja a ismeretlen adatforrásokat támogatása. Felhasználók kézzel írhatja hello Azure Data Catalog-portált használja, az adatforrásra vonatkozó információ, hogy az adatforrások nem explicit adatforrás-regisztráló eszköz feliratozva, valamint felfedezett hello által támogatott.
* Regisztrálja és SQL Server-adatbázisok mint tárolók felderítésével támogatása. SQL Server-táblák és nézetek regisztrálásakor Azure Data Catalog hello adatbázis, a táblák és nézetek bejegyzést hoz létre. hello adatbázis felderíthető és jegyzetelve hello Azure Data Catalog-portál használatával. A felhasználók is egy adatbázis hozzáadása toosearching és a szűrés hello katalógus hello tartalmát a Keresés és szűrés hello tartalmát.

> [!NOTE]
> Az SSAS és az SSRS objektumok regisztrált előzetes toohello 18. szeptemberi kiadás újra regisztrálni kell hello adatforrás-regisztráló eszköz használatával hello modell vagy a kiszolgáló bejegyzés hozzáadása előtt toohello katalógus. Újra az adatforrás regisztrálása nem befolyásolja a jegyzetek hello Azure Data Catalog portálon a felhasználók által hozzáadott.

> [!NOTE]
> Oracle-táblák és nézetek és HDFS fájlok és könyvtárak, már regisztrált előzetes toohello 11. szeptemberi kiadás újra regisztrálni kell hello adatforrás-regisztráló eszköz használatával hello adatbázis vagy a fürt bejegyzés hozzáadása előtt toohello katalógus. Újra az adatforrás regisztrálása nem befolyásolja a jegyzetek hello Azure Data Catalog portálon a felhasználók által hozzáadott.

> [!NOTE]
> SQL Server-táblák és nézetek, már regisztrált előzetes toohello 4. szeptemberi kiadás újra regisztrálni kell hello adatforrás-regisztráló eszköz használatával toohello katalógus hello adatbázis bejegyzés hozzáadása előtt. Újra az adatforrás regisztrálása nem befolyásolja a jegyzetek hello Azure Data Catalog portálon a felhasználók által hozzáadott.

## <a name="whats-new-for-august-2015"></a>Újdonságok a 2015. augusztus
2015. augusztus hello a következő képességekkel rendelkeznek lett hozzáadva tooAzure Data Catalog:

* Az SQL Server és Oracle-adatforrások profilkészítési adatok támogatása. Regisztrálás az SQL Server és Oracle-táblák és nézetek, amikor a felhasználók maguk dönthetik tooinclude adatok profiladatok regisztrált hello objektumok. hello adatok profil objektumszintű és oszlopszintű statisztikai adatokat tartalmaz.
* Hadoop HDFS adatforrások támogatása. A felhasználók most regisztrálni és HDFS fájlok és könyvtárak felderítése.
* Hozzáférési kérelem részleteit regisztrált adatforrások biztosító támogatása. Bármely regisztrált eszköz felhasználók is most utasításokkal hozzáférés kérelmezésének, beleértve az e-mailek hivatkozásokat vagy URL-címek, tooeasily integrálható a meglévő eszközök és a folyamatok.
* Eszköz tippek címkék és szakértők, toomake azt könnyebben toodiscover a felhasználók milyen milyen metaadatok megadott regisztrált adategységeket.
* Egy új "User" és a menü tooour felső navigációs sáv a következőkkel egészült ki. Ebben a menüben lehetővé teszi, hogy a hello felhasználók láthatják a hello használt fiók toolog a Data Catalog tooAzure és toosign ki, ha szükséges. Ebben a menüben jelennek meg azok hello katalógus neve, amely értékes toodevelopers Azure Data Catalog REST API használatával hello.
* Csak Standard Edition: Tulajdonosok toodata eszközök hozzáadásakor Azure Data Catalog mostantól támogatja a felhasználói fiókok és biztonsági csoportok tulajdonosként. tooadd adategységek tulajdonosát biztonsági csoportot, vagy adhat meg vagy hello csoport megjelenített név hello csoport UPN e-mail címet, ha van ilyen.
* Támogatás az Azure Blob Storage-adatforrásokhoz. A felhasználók most regisztrálni és Fedezze fel az Azure Storage blobs és könyvtárakat.

