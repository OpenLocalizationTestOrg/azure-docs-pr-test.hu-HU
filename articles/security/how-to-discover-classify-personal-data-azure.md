---
title: "aaaDiscover, és személyes adatok Microsoft Azure-ban minősítsen |} Microsoft Docs"
description: "További tudnivalók a keresési, zárolásának, felderítéséhez és azonosító adatait"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: af4ced1c57699dc751d55cfdf3229c7d294648a9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Ismerje meg, azonosítása és besorolni a személyes adatok Microsoft Azure-ban

Ez a cikk útmutatást toodiscover, azonosításához, és a több Azure eszközeivel és szolgáltatásaival, beleértve az Azure Data Catalog, Azure Active Directory, az SQL-adatbázis, a Power Query használatával az Azure hdinsight, Azure Hadoop-fürtök a személyes adatok besorolása Azure Cosmos DB adatvédelem, az Azure Search és az SQL lekérdezések.

## <a name="scenario-problem-statement-and-goal"></a>Forgatókönyv, problémamegállapítás és célja

Az Egyesült államokbeli sport vállalat különböző személyes és egyéb adatokat gyűjt. Ez magában foglalja az ügyfelek és az alkalmazott adatokat. hello vállalati tárolása a több adatbázist, és az Azure környezetben több különböző helyen tárolja. Továbbá a tooselling sportfelszerelések, is futtatni, és kezelése a regisztrációs elite athletic események körül hello world hello Európa, beleértve és az egyes esetekben hello gyűjtött ügyféladatok orvosi információkat tartalmaz.

Mivel hello vállalati sok nemzetközi bicycling bemutatók évente használja, és rendelkezik-e szerződéses alkalmazottjai hello földgömb pontján, hello adatkészletek néhány igen tekintélyes. hello vállalati ügyfelek és az alkalmazottak által használt alkalmazások fejlesztői beépített is rendelkezik.

hello vállalat szeretne tooaddress hello a következő problémákat:

- Ügyfél és az alkalmazottak a személyes adatok kell lennie besorolni/különböztetni a hello egyéb adatok hello vállalati rendelés tooensure megfelelő hozzáférést és biztonsági gyűjti.
- hello adatok rendszergazdának hozzá kell tooeasily személyes ügyféladatok hello hely felderítése között különböző területei hello Azure környezetben.
- Ügyfél és az alkalmazott személyes megjelenő adatok megosztott dokumentumok és e-mailek kommunikációs kell címkével toohelp érdekében, hogy folyamatosan biztonságos.
- hello vállalati alkalmazásfejlesztők kell egy módon tooeasily keresse meg a webes és mobilalkalmazások ügyfél és az alkalmazott személyes adatok.
- A fejlesztők is kell tooquery a dokumentum-adatbázis a személyes adatok.

### <a name="company-goals"></a>Vállalati célok

- Az Azure Data Catalog címkézett/feliratozva összes ügyfél és az alkalmazott személyes adatokat kell lennie, ezért egyszerűen található. Ideális esetben ügyfél és az alkalmazottak a személyes adatok is címkézett/feliratozva külön-külön.
- A felhasználói profilok felhasználói és az alkalmazottak és a munkahelyi adatokat az Azure Active Directoryban található személyes adatokat könnyen található kell lennie.
- Több SQL-adatbázisban található személyes adatokat könnyen lehet lekérdezni. 
- Néhány vállalat hello nagy adatkészletek Azure HDInsight segítségével kezelt és Hadoop tárolja. Akkor kell importálni az Excel, a személyes adatok kérhetők.
- Megosztott dokumentumok és e-mailek kommunikációs személyes adatok kell besorolni, címkével és az Azure Information Protection biztonságba.
- hello vállalat alkalmazásfejlesztők kell tudni toodiscover ügyfél és az alkalmazott személyes adatok hello alkalmazásokban létrehozott azokat, amelyek az Azure Search végezhetnek.
- A fejlesztők képes toofind személyes adatokat a dokumentum-adatbázisban kell lennie.

## <a name="azure-active-directory-data-discovery"></a>Az Azure Active Directory: Adatok felderítése

[Az Azure Active Directory](https://azure.microsoft.com/services/active-directory/) a Microsoft felhőalapú, több-bérlős directory és az identity management szolgáltatás. Megkeresheti a felhasználói profilok felhasználói és az alkalmazottak és a felhasználó munkahelyi adatokat, amelyek személyes adatokat tartalmaznak a [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) környezetben hello segítségével [Azure-portálon](https://portal.azure.com/).

Ez akkor különösen hasznos, ha szeretné, hogy toofind vagy módosítani az adott felhasználó személyes adatokat. Is hozzáadhat, vagy módosítsa a felhasználói profil és vonatkozó. Hello könyvtár egy globális rendszergazdai fiókkal kell bejelentkeznie.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Hogyan keresse meg és felhasználói profil megtekintése és vonatkozó?

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.

2. Válassza ki **további szolgáltatások**, adja meg **felhasználók és csoportok** hello szövegmezőbe, és válassza ki **Enter**.

   ![hogyan keresse meg a felhasználói profil és vonatkozó](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. A hello **felhasználók és csoportok** panelen válassza **felhasználók**.

  ![Nyitó felhasználók és csoportok](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. A hello **felhasználók és csoportok - felhasználók** panelen hello listából válasszon ki egy felhasználót, és ezt követően a kiválasztott felhasználó hello hello panelen válassza **profil** tooview felhasználói profillal kapcsolatos információk, amelyek személyes adatokat tartalmazhat .

  ![felhasználó kiválasztása](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Ha tooadd kell, vagy módosítsa a felhasználói profillal kapcsolatos információk, is ehhez, és ezt követően hello parancssávon válassza **mentéséhez.**
6. A kiválasztott felhasználó hello hello panelen válassza ki a **munkahelyi adatai** tooview felhasználói munkahelyi adatokat, amelyek személyes adatokat is tartalmazhat.

 ![megtekintés munkahely adatai](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Ha tooadd kell, vagy módosítsa a felhasználó munkahelyi adatokat, is ehhez, és ezt követően hello parancssávon válassza **mentéséhez.**

## <a name="azure-sql-database-data-discovery"></a>Az Azure SQL Database: Adatok felderítése

[Az Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) egy felhő-adatbázis, amely a fejlesztőket létrehozása és alkalmazások karbantartása. Személyes adatok található [Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) szabványos SQL-lekérdezések használatával. Az Azure SQL rugalmas lekérdezés (előzetes verzió) lehetővé teszi, hogy a felhasználók tooperform közötti adatbázis-lekérdezések.

A részletes [SQL-adatbázis](../sql-database/sql-database-technical-overview.md) az oktatóanyag azt ismerteti, SQL-adatbázis használata sok aspektusainak egyebek között egy toobuild és hogyan toorun lekérdezések. hello hivatkozások toospecific szakaszok hello oktatóanyag elérhető hello adatainak összegzése látható.

### <a name="how-do-i-build-a-sql-database"></a>Hogyan építhető SQL-adatbázis?

Nincsenek a három módon toodo azt:

- Egy Azure SQL Database adatbázist hozhat létre hello [Azure-portálon](https://portal.azure.com/). Hello az oktatóanyagban egy meghatározott számítási és tárolási erőforrásokat egy erőforráscsoport és logikai kiszolgálót fogja használni. Egy fiktív cég, AdventureWorks minta adatait fogja használni. Létre fog hozni egy kiszolgálószintű tűzfalszabályt is. toolearn hogyan toodo a, látogasson el hello [hello Azure-portálon az Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-portal.md) oktatóanyag.

  ![Az Azure SQL-adatbázis létrehozása](media/how-to-discover-classify-personal-data-azure/create-database.png)
- SQL-adatbázis is létrehozhatók hello [Azure Cloud rendszerhéj](https://azure.microsoft.com/features/cloud-shell/) CLI, egy webböngésző-alapú parancssori eszközt. hello eszköz hello Azure-portál érhető el, és közvetlenül futtatható. Ebben az oktatóanyagban meg hello eszköz indítása, adja meg a parancsfájl-változókat, hozzon létre egy erőforráscsoportot és a logikai kiszolgáló és tűzfalszabály konfigurálása. Majd hoz létre egy adatbázist mintaadatokkal. toolearn hogyan toocreate az adatbázis ezzel a módszerrel a Microsoft hello [hello Azure parancssori felület használatával egyetlen Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-cli.md) oktatóanyag.

  ![CLIT oktatóanyag](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Az Azure CLI Linux rendszergazdák és fejlesztők használják. Egyes felhasználók található egyszerűbbé és intuitívabb PowerShell, mint amelyek a harmadik lehetőség.

- Végül powershellel, amely egy parancssor, a parancsfájl használt eszköz toocreate SQL-adatbázis létrehozása és kezelése az Azure és más erőforrásokat. Ebben az oktatóanyagban meg hello eszköz indítása, adja meg a parancsfájl-változókat, hozzon létre egy erőforráscsoportot és a logikai kiszolgáló és tűzfalszabály konfigurálása. Ezután létre fog hozni a egy adatbázist mintaadatokkal.

hello oktatóanyag hello Azure PowerShell 4.0-s vagy újabb verziója szükséges. Get-Module - ListAvailable AzureRM toofind a verzióját futtatják. Ha tooinstall vagy frissítés van szüksége, tekintse meg a telepítése Azure PowerShell modul.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

toolearn hogyan toocreate az adatbázis ezzel a módszerrel a Microsoft hello [Powershell használatával egyetlen Azure SQL-adatbázis létrehozása](../sql-database/sql-database-get-started-powershell.md) oktatóanyag.

>[!Note]
A Windows rendszergazdák általában toouse PowerShell, de némelyikük inkább az Azure parancssori felület.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-hello-azure-portal"></a>Hogyan keresse meg az SQL-adatbázis hello Azure-portálon a személyes adatok? **

Eszközzel hello beépített lekérdezést szerkesztő hello Azure portál toosearch belül a személyes adatok. Jelentkezzen be az SQL server rendszergazdai bejelentkezés és jelszavával toohello eszköz lesz, és írja be a lekérdezést.

  ![keresési sql hello portál használatával](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Hello oktatóanyag 5. lépés példalekérdezést megjeleníti hello lekérdezés-szerkesztő panelen, de nem összpontosítson, személyes vagy bizalmas adatok (is egyesíti a két táblázatok adatait, és hozza létre, hello forrásoszlop aliasok visszaadott hello adatkészlet). hello alábbi képernyőfelvételen látható hello lekérdezés 5. lépés, valamint a visszaadott eredménypanelen hello:

  ![lekérdezésszerkesztő](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Ha az adatbázis MyTable lett meghívva, pedig mintalekérdezést személyes adatokat tartalmazhat neve, társadalombiztosítási szám és azonosítószámát, és néz ki:

"Válassza ki a neve, társadalombiztosítási szám, azonosító szám a MyTable"

Hello lekérdezés futtatása, és a hello majd hello dokumentumkönyvtárából **eredmények** ablaktáblán.

Hogyan tooquery egy SQL-adatbázis használati ideje hello Azure-portálon a további információkért látogasson el a hello [hello SQL-adatbázis lekérdezése](../sql-database/sql-database-get-started-portal.md) hello oktatóanyag szakasza.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Hogyan kereshető adatokat több adatbázis között?

A rugalmas SQL-lekérdezés (előzetes verzió) lehetővé teszi, hogy Ön tooperform adatbázisok közötti és több adatbázis-lekérdezés, és egyetlen eredményt. Hello [oktatóanyag – áttekintés](../sql-database/sql-database-elastic-query-overview.md) forgatókönyvek részletes leírását tartalmazza, és ismerteti a függőleges és vízszintes adatbázis particionálási hello különbsége. Vízszintes particionálás neve "horizontális."

  ![A vertikális particionálás](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![vízszintes particionálás](media/how-to-discover-classify-personal-data-azure/horizontal.png)

tooget indult el, keresse fel a hello [Azure SQL Database rugalmas lekérdezési áttekintése (előzetes verzió)](../sql-database/sql-database-elastic-query-overview.md) lap.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Power Query (az Azure HDInsight Hadoop-fürtök importálásához): nagy adatkészletek adatainak felderítése

Hadoop egy nyílt forráskódú nagy méretű adatkészletekhez, elemzése és a Hadoop-fürtök tárolja az Apache tárolás és feldolgozás szolgáltatás. [Az Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) lehetővé teszi a felhasználók toowork a Hadoop-fürtök az Azure-ban. A Power Query az Excel-bővítmény, amely, többek között segít felderíteni a különböző forrásokból származó adatok.

Lehet, hogy az Azure hdinsight Hadoop-fürtök társított személyes adatokat a Power Query importált tooExcel. Miután hello adatok az Excel egy lekérdezés tooidentify használhatja azt.

#### <a name="how-do-i-use-excel-power-query-tooimport-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Miként használható az Excel Power Query tooimport Hadoop-fürtök az Azure HDInsight Excelbe?

A HDInsight az oktatóanyag végigvezeti a teljes folyamat. Az Előfeltételek ismerteti, és tartalmaz egy hivatkozást tooa [Ismerkedés az Azure HDInsight](../hdinsight/hdinsight-hadoop-linux-tutorial-get-started.md) oktatóanyag. Útmutatás fedik le a Excel 2016, valamint 2010 és 2013 (lépésekre némileg eltérő hello az Excel régebbi verzióira). Ha nem rendelkezik hello Excel Power Query beépülő modul, hello oktatóanyag bemutatja, hogyan tooget azt. Hello oktatóanyag kezdi, az Excel programban, és a fürthöz tartozó Azure Blob storage-fiók toohave kell.

  ![A lekérdezés az Excel programban](media/how-to-discover-classify-personal-data-azure/excel.png)

toolearn hogyan toodo a, látogasson el hello [kapcsolódás Excel tooHadoop Power Query használatával](../hdinsight/hdinsight-connect-excel-power-query.md) oktatóanyag.

Forrás: [kapcsolódás Excel tooHadoop Power Query használatával](../hdinsight/hdinsight-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Az Azure Information Protection: a dokumentumok és e-mailek személyes adatok besorolása

[Az Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) Azure-ügyfél címkék tooclassify alkalmazása és belső vagy külső megosztott dokumentumok védelme és e-mail-kommunikáció segítségével. Ezek az elemek egy része az ügyfél vagy az alkalmazott személyes adatokat tartalmazhat. Szabályok és a feltételek meghatározása automatikusan vagy manuálisan, rendszergazdák és felhasználók. Például ha egy felhasználó egy dokumentumot, amely tartalmazza a hitelkártya adatait menti, ő jelenik meg hello rendszergazda által beállított címke ajánlást.

### <a name="how-do-i-try-it"></a>Hogyan próbálja ki?

Ha azt szeretné, hogy a program toogive Azure Information Protection egy try toosee, ha előfordulhat, hogy egy beválik, ha a szervezet, látogasson el a hello [gyors üzembe helyezési útmutató](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Azt végigvezeti öt alapvető – a telepítés tooconfiguring házirend tooseeing besorolás, címkézés és a működés közben megosztása – és kisebb, mint fél óra kell végrehajtani.

### <a name="how-do-i-deploy-it"></a>Hogyan telepíthetem azt?

Ha a szervezet Azure Information Protection toodeploy kíváncsi, látogasson el a hello [üzembe helyezési menetrend a besorolás, címkézés és védelem](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Van-e más tisztában van szükségem?

Kiegészítő információkat, amelyek segítséget gondolja, hogy hogyan tooset, akkor keresse fel a hello [kész, állítsa be, védelme!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
blogbejegyzést. És ellenőrzés hello Azure Information Protection bővebben az alább felsorolt további hivatkozások.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Az Azure Search: adatok felderítési fejlesztői alkalmazások

[Az Azure Search](https://azure.microsoft.com/services/search/) egy felhőalapú keresési megoldás a fejlesztők számára, és részletes adatok keresési élményt nyújt az alkalmazások. Az Azure Search lehetővé teszi toolocate adatok között felhasználói indexek, Azure Cosmo DB, az Azure SQL Database, Azure Blob Storage, Azure Table storage vagy egyéni felhasználói JSON-adatok forrása. Hello Azure Search REST API toosearch használja típusú személyes adatokat, vagy konkrét személyek hello személyes adatainak Lucene lekérdezések felépítésének is lehet. Szolgáltatások közé tartozik a teljes szöveges keresés, egyszerű lekérdezés szintaxisát és Lucene lekérdezés szintaxisa. 

## <a name="how-do-i-use-sql-tooquery-data"></a>Miként használható az SQL tooquery adatokat?

hello alapjaival toobegin látogasson el a hello [Azure CosmosD DB: hogyan tooquery SQL használatával](../cosmos-db/tutorial-query-documentdb.md) oktatóanyag. hello az oktatóanyag egy minta dokumentumot és két minta az SQL-lekérdezések és eredmények biztosít.

További részletes útmutató az SQL-lekérdezések felépítésével, a Microsoft [Azure Cosmos DB dokumentum DB API SQL-lekérdezések.](../cosmos-db/documentdb-sql-query.md)

Ha Ön új tooAzure Cosmos DB és volna toolearn például hogyan toocreate egy adatbázis felvesznek egy gyűjteményt, majd adja meg adatokat, látogasson el hello [Azure Cosmos DB: a DocumentDB API webalkalmazás összeállítása](../cosmos-db/create-documentdb-dotnet.md) gyors üzembe helyezési útmutató. Ha azt szeretné, hogy toodo ez .NET, Java vagy Python, például nyelven csak válassza ki a kívánt nyelvet Miután toohello hely.

## <a name="next-steps"></a>Következő lépések

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Mi az SQL Database?](../sql-database/sql-database-technical-overview.md)

[SQL adatbázis lekérdezés-szerkesztő elérhető Azure-portálon] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Mi az az Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Mi az az Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Az Azure Information Protection: Kész, állítsa be, védelme!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
