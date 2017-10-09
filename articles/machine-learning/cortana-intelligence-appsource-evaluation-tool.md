---
title: "aaaCortana Eszközintelligencia megoldás kiértékelési eszközével |} Microsoft Docs"
description: "Egy Microsoft partnert, mint az alábbiakban a Cortana Intelligence megoldás tooAppSource kell toofollow toopublish hello lépéseket."
services: machine-learning
documentationcenter: 
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: anupams;v-bruham;garye
ms.openlocfilehash: 76cde4e2090c121683b7026f3d80f90f64566607
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="cortana-intelligence-solution-evaluation-tool"></a>Cortana Intelligence megoldásértékelési eszköz
## <a name="overview"></a>Áttekintés
Használhatja a Cortana Intelligence megoldás kiértékelési eszköz tooassess hello a speciális elemzési megoldások a Microsoft által ajánlott gyakorlati eljárásoknak megfelelő beállításában. A Microsoft a partnerekkel együttműködve várakozással toowork (ISV-k / SIs) tooprovide kiváló minőségű megoldások ügyfelek, viszonteladók és végrehajtására. Ez az útmutató ismerteti hello folyamatot, amely a megoldás hello megoldás kiértékelési eszköz segítségével, és a hello adott a bevált gyakorlat az ellenőrzéseket ismertetik.

## <a name="getting-started"></a>Bevezetés
Adjon [letöltése](https://aka.ms/aa-evaluation-tool-download) és hello Cortana Intelligence megoldás kiértékelési eszközével telepítse.

Előfeltételek:
- Windows 10: [hivatalos webhely a Windows 10-hez](https://www.microsoft.com/en-us/windows)
- Az Azure Powershell: [telepítse és konfigurálja az Azure Powershellt](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-4.0.0).

## <a name="identifying-your-app"></a>Az alkalmazás azonosítása
A telepítést követően hello eszköz megnyitásához, és az első próbaverziójának megkezdéséhez.

![Nyissa meg kiértékelési eszközével](./media/cortana-intelligence-appsource-evaluation-tool/1-open-evaluation-tool.png)

Adja meg a megoldás azonosító információkat.

![Csatlakozás Azure-előfizetés](./media/cortana-intelligence-appsource-evaluation-tool/2-connect-azure-subscription.png)

Csatlakozás Azure-előfizetés tooyour, és adja meg az alkalmazást tartalmazó erőforráscsoportot hello.

![Erőforrások kiválasztása](./media/cortana-intelligence-appsource-evaluation-tool/3-select-resources.png)

Miután hello erőforráscsoport be van töltve, jelöljön ki hello erőforrásokat, amelyek szerepelnek a megoldás, és azonosíthatja a hello kisegítő egyetlen adatok erőforrás, vagyis:
- Adatfeldolgozást
- Használat
- Belső

Az információ felhasználása toobetter megérteni, hogyan a megoldás használatával van a különböző összetevők és tooensure felhasználók számára is elérhető összetevői megegyeznek az ajánlott eljárásoknak megfelelő beállításában.

### <a name="ingestion"></a>Adatfeldolgozást
Adatfeldolgozást ebben az esetben azt jelenti, hogy a egyetlen adatforrást sem, amelyek az adatok külső hello megoldásból használt toopull vagy kívül hello megoldás olyan szolgáltatásokat használó toopush adatok bele.

### <a name="consumption"></a>Használat
Felhasználás ebben az esetben azt jelenti, hogy a bármely adatkészletek használt toopush adatok tooend felhasználók közvetve vagy közvetlenül. Példa:
- A közvetlen lekérdezés a Power bi adatkészletekből.
- Az adathalmaz egy webalkalmazás kérdezhetők le.

>[!NOTE]
Válasszon ki egy adott erőforrás használata adatfeldolgozást és felhasználás esetén **fogyasztás**.

### <a name="internal"></a>Belső
Csak a belső alkalmazás feldolgozása használt adatok erőforrásokat belső használatos.

A következő kért tooprovide hello előző lépésben megadott adatbázisoknak érvényes hitelesítő adatokat fogja:

![Set teszt Előfeltételek](./media/cortana-intelligence-appsource-evaluation-tool/4-set-test-prerequisites.png)

## <a name="solution-test-cases"></a>Megoldás vizsgálati esetek
hello megoldás eszköz automatikus tesztek gyűjteménye a megoldás hajt végre.

![Teszt végrehajtásának](./media/cortana-intelligence-appsource-evaluation-tool/5-set-test-execution.png)

Hello tesztek befejeződése után ismételt tooprovide egy magyarázat vagy miért a megoldás nem felel meg hello követelmény indokát.

![Adja meg az üzleti indoklásának](./media/cortana-intelligence-appsource-evaluation-tool/6-provide-business-justification.png)

Például a megoldás tooAzure SQL DW tesz közzé, ha szükséges, tooalso hello értékelési tooAzure Analysis Services közzététele. 

A megoldás IaaS virtuális gépeket futtató Sql Server Analysis Services Azure Analysis Services helyett használhat. Ez lehet egy hiba hello vizsgálat elfogadható okát.
## <a name="packaging-your-evaluation-results"></a>Az értékelési eredmények-csomagban
Hello teszteseteinek befejezése után a kiértékelési csomag lesz exportált tooa zip-fájl, és megkérdezi tooprovide visszajelzés a hello kiértékelési eszközével. 

Ez a teszt eredménye zip-fájl a Microsoft számára a jóváhagyás toobe hozzáadott elérése előtt értékelni megoldás toobe tooshare kell tooAppSource

![Osztályú kiértékelési eszközével](./media/cortana-intelligence-appsource-evaluation-tool/7-grade-evaluation-tool.png)

Ez a szakasz fenti cikk ismerteti a különböző funkcióit hello eszköz, most ossza meg velünk tekintse át az eszköz kiértékelése bevált gyakorlatokat típusú.

## <a name="security-evaluation-considerations"></a>Biztonsági értékelési szempontok
### <a name="databases-should-use-azure-active-directory-authentication"></a>Adatbázisok Azure Active Directory hitelesítést kell használnia.
Hello sloution az Azure SQL- vagy Azure SQL DW erőforrásokat az Azure Active Directory (AAD) hitelesítéssel engedélyezni kell. Aad-ben tartalmaz egy egyetlen hely toomanage összes identitások és szerepkörök.

| További információ | Ebben a cikkben találhat |
| --- | --- |
| SQL-adatbázis és az SQL Data Warehouse az aad-ben | [Az SQL Database vagy az SQL Data Warehouse hitelesítéshez használandó Azure Active Directory-hitelesítés](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication) |
| Konfigurálhatja és kezelheti az aad-ben | [Konfigurálhatja és kezelheti az Azure Active Directory-hitelesítés az SQL Database vagy az SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) |
| Az Azure webalkalmazás-hitelesítés | [Hitelesítési és engedélyezési az Azure App Service-ben](https://docs.microsoft.com/en-us/azure/app-service/app-service-authentication-overview) |
| Webalkalmazás konfigurálása az aad-ben | [Hogyan tooconfigure App Service alkalmazás toouse Azure Active Directory bejelentkezési adatait](https://docs.microsoft.com/en-us/azure/app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication)|

### <a name="datasets-accessible-tooend-users-should-support-role-based-access-control"></a>Adatkészletek elérhető tooend-felhasználók támogatnia kell a szerepköralapú hozzáférés-vezérlés
Hello kiértékelési eszközével végrehajtásakor fogja toospecify ismételt reporting vagy erőforrások közzétételéhez. Feltételezzük, hogy ezeket az erőforrásokat szánt hozzáférés végfelhasználók, nem a fejlesztők által. Ezeket az erőforrásokat kell biztosítania kell szerepköralapú hozzáférés-vezérlést (RBAC) a rendelés tooensure, hogy a végfelhasználók számára legyenek csak képes tooaccess jogosult adatokat.

Pontosabban hello Azure-erőforrások a következő bármelyike RBAC konfigurálhatók, és elfogadható minősülnek:
- HDInsight biztonságos című [egy bevezető tooHadoop biztonsági HDInsight-fürtökkel a tartományhoz](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-domain-joined-introduction)
- Az Azure SQL, lásd: [AAD-hitelesítés és az Azure SQL]( https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication)
- Az Azure Analysis Services, lásd: [adatbázis-szerepkörök és a felhasználók kezelése az Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-database-users)
- Az SQL Data Warehouse (vegye figyelembe, hogy SQL DW nem támogatja a Szerepalapú, nem ajánlott a közvetlen végfelhasználói hozzáférése.)

Ha egy másik erőforrástípust, amely támogatja az RBAC használata esetén adja meg, amely hello Teszteset indoklás.

### <a name="azure-data-lake-store-should-use-at-rest-encryption"></a>Azure Data Lake Store kell használnia, inaktív adatok titkosítása
Azure Data Lake Store-(ADLS-), inaktív adatok titkosítása ADLS-felügyelet alatt álló titkosítási kulcsok használatával alapértelmezés szerint támogatja. Az Azure Key Vault használatával titkosítási is beállítható.

ADLS-titkosítási beállításait, információ [Azure Data Lake Store-fiók létrehozása](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-get-started-portal#create-an-azure-data-lake-store-account).

### <a name="azure-sql-and-azure-sql-data-warehouse-should-use-encryption"></a>Az Azure SQL és az Azure SQL Data Warehouse használandó titkosítási
Az Azure SQL és Azure SQL DW is támogatja a transzparens adatok titkosítás (TDE), amely biztosítja az adatok és a naplófájlok valós idejű titkosításához és visszafejtéséhez.

| További információ | Ebben a cikkben találhat |
| --- | --- |
| Az átlátható adattitkosítás (TDE) | [Átlátható adattitkosítás](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-tde) |
| Az Azure SQL Data Warehouse TDE | [SQL Data Warehouse-Encrption TDE TSQL](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-encryption-tde-tsql) |
| Az Azure SQL TDE konfigurálása | [Az Azure SQL Database átlátható adattitkosítás](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database) |
| Mindig titkosítja az Azure SQL konfigurálása | [SQL-adatbázis mindig titkosítja az Azure Key Vault](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-always-encrypted-azure-key-vault)|

TooTDE, Azure SQL is támogatja továbbá mindig titkosítja, egy új titkosítási technológiát, amely biztosítja az adatok titkosítása nem csak nyugalmi és adatátviteli ügyfél és kiszolgáló közötti, hanem adatainak közben során használja a hello kiszolgálón parancsok végrehajtása során.

### <a name="any-virtual-machines-must-be-deployed-from-hello-azure-marketplace"></a>A virtuális gépek kell telepíteni a hello Azure piactér
Rendelés tooprovide keresztül AppSource biztonsági egységes szintjét kérjük, bármely a Cortana Intelligence megoldás részeként telepített virtuális gépek hitelesített és közzétett hello Azure piactéren.

Lásd az Azure piactéren elérhető rendszerkép toosearch hello listába [Microsoft Azure piactérről](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/category/compute).

Hogyan toopublish egy virtuális gép rendszerképet az Azure piactér információkért lásd: [útmutató toocreate egy virtuálisgép-lemezkép az Azure piactér hello](https://docs.microsoft.com/en-us/azure/marketplace-publishing/marketplace-publishing-vm-image-creation).

## <a name="scalability-evaluation-considerations"></a>Méretezhetőség értékelési kapcsolatos szempontok
### <a name="cortana-intelligence-solutions-should-include-a-scalable-big-data-platform"></a>A Cortana Intelligence megoldások tartalmaznia kell egy méretezhető nagy adatplatform
A Cortana Intelligence megoldások toovery nagy adatmennyiség kell méretezni. Az Azure Ez azt jelenti, azok hello két Petabájtnyi méretű adatok rendszerek egyikét kell tartalmaznia:
- Azure Data Lake Store
- Azure SQL Data Warehouse

Ha a megoldás nem igényel támogatást ezen adatok mérete, vagy egy alternatív adatplatform használ, kérem, ez a hello Teszteset indoklás.
### <a name="cortana-intelligence-solutions-should-include-dedicated-ingestion-data-environments"></a>A Cortana Intelligence megoldások dedikált adatfeldolgozást adatok környezetekben kell tartalmaznia.
A Cortana Intelligence megoldások általában közvetlenül az adatok beszúrása relációs adatforrások kerülendő. Ehelyett nyers adatokat kell tárolni egy strukturálatlan környezetet az idempotent Beszúrások frissítések bármely Azure Data Factory használatával relációs tárolja azokat.

További információ az Azure Data Factory, az adatok másolásának [oktatóanyag: hozzon létre egy folyamatot Visual Studio használatával, a másolási tevékenység](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-copy-activity-tutorial-using-visual-studio).

### <a name="azure-sql-data-warehouse-should-use-polybase-for-data-ingestion"></a>Az SQL Data Warehouse PolyBase adatfeldolgozást kell használnia
Az Azure SQL DW PolyBase jól skálázható, párhuzamos adatfeldolgozást támogatja. A PolyBase lehetővé teszi a toouse Azure SQL DW tooissue lekérdezések írásában, vagy az Azure Blob Storage tárolóban, vagy az Azure Data Lake Store tárolt külső adatkészletek. Ez jobb teljesítményt biztosít tooalternative módszerek tömeges frissítése.

Ismerkedés a PolyBase és az Azure SQL DW, lásd: [adatok betöltése a PolyBase az SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-get-started-load-with-polybase).

A PolyBase és az Azure SQL DW ajánlott eljárásokra vonatkozó további információkért lásd: [útmutató az SQL Data Warehouse PolyBase használatával](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-load-polybase-guide).

## <a name="availability-evaluation-considerations"></a>Rendelkezésre állási értékelési kapcsolatos szempontok

### <a name="datasets-accessible-tooend-users-should-support-a-large-volume-of-concurrent-users"></a>Adatkészletek elérhető tooend felhasználók támogatnia kell a nagy egyidejű felhasználók
Hello kiértékelési eszközével végrehajtásakor fogja toospecify ismételt reporting vagy erőforrások közzétételéhez. Feltételezzük, hogy ezeket az erőforrásokat szánt hozzáférés végfelhasználók, nem a fejlesztők által. Ezeket az erőforrásokat támogatnia kell a közepes és nagy mennyiségű egyidejű felhasználók.

Azure SQL Data Warehouse kifejezetten, nem lehet hello egyetlen adatok forrása elérhető tooend felhasználók. Ha az Azure SQL DW kiemelt felhasználók előírt erőforrásként, kell Azure Analysis Services tenni elérhető tootypical felhasználók.

Azure SQL DW feldolgozási korlátok kapcsolatos további információkért lásd: [egyidejűségi és munkaterhelés-kezelés az SQL Data Warehouse](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-develop-concurrency).

Azure Analysis Services kapcsolatos további információkért lásd: [Analysis Services áttekintése](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview).

### <a name="azure-sql-resources-should-have-a-read-only-replica-for-failover"></a>Azure SQL-erőforrások feladatátvételhez csak olvasható replika kell rendelkeznie.
Az Azure SQL-adatbázisok georeplikáció tooa másodlagos példány támogatja. Ez a példány egy feladatátvételi példány tooprovide magas rendelkezésre állású alkalmazások majd használható.

Az Azure SQL-adatbázisok georeplikáció kapcsolatos további információkért lásd: [SQL adatbázis földrajzi régiók közötti replikáció áttekintése](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-overview).

Útmutatást tooconfigure a georeplikáció az Azure SQL, lásd: [aktív georeplikáció konfigurálása az Azure SQL Database Transact-SQL](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-geo-replication-transact-sql).

### <a name="azure-sql-data-warehouse-should-have-geo-redundant-backups-enabled"></a>Az SQL Data Warehouse georedundáns biztonsági mentések engedélyezve kell rendelkeznie.
Az Azure SQL DW támogatja a napi biztonsági mentések toogeo redundáns tárolást. A georeplikáció biztosítja, hogy visszaállíthassa hello adatraktárban még akkor is, ha nem fér hozzá az elsődleges régióban tárolt pillanatképek helyzetekben. Ez a funkció alapértelmezés szerint be van kapcsolva, és nem lehet letiltani a Cortana Intelligence megoldások.

További információ az Azure SQL DW biztonsági mentés és visszaállítás talál [SQL Data Warehouse biztonsági mentések](https://docs.microsoft.com/en-us/azure/sql-data-warehouse/sql-data-warehouse-backups).

### <a name="virtual-machines-should-be-configured-with-availability-sets"></a>Virtuális gépek rendelkezésre állási készletek kell konfigurálni
Az Azure virtuális gépek a rendelkezésre állási készletek rendelés toominimize hello tervezett és nem tervezett karbantartási események hatását a lehet beállítani.

Azure virtuális gép rendelkezésre állási kapcsolatos további információkért lásd: [hello Windows virtuális gépek Azure-ban rendelkezésre állásának kezelése](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/manage-availability).

## <a name="other-evaluation-considerations"></a>Egyéb értékelési szempontok
### <a name="cortana-intelligence-apps-should-use-a-centralized-tool-for-data-orchestration"></a>A Cortana Intelligence alkalmazásokat kell használnia egy központosított eszköz adatok előkészítése
Egyetlen eszközzel kezelésére és adatmozgás és átalakítás ütemezése kritikus fontosságú adatok körül konzisztencia biztosítja. Biztosít egy tiszta logika körül újrapróbálkozási logika, a függőségi felügyeleti, a riasztás/naplózási, stb. Azt javasoljuk, hogy hello használata [Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-introduction) az adatok előkészítése az Azure-ban.

Ha egy eszköz nem Azure Data Factory adatok előkészítése az használ, írja le mely eszköz vagy a használt eszközök.
### <a name="azure-machine-learning-models-should-be-retrained-using-azure-data-factory"></a>Az Azure Machine Learning modellek kell retrained Azure Data Factory használatával
Az Azure Machine Learning (AzureML) eszközeivel könnyen toouse hello létrehozásának és telepítésének prediktív modellezési és gépi tanulási folyamatok. Azonban fontos, hogy az AzureML modellek üzemi környezetek nem egyetlen rögzített adatkészlet alapján, de ehelyett alkalmazkodik valós jelenségek dynamics időeltolású toohello.

Megőrzési webszolgáltatások AzureML létrehozásáról további információk: [Machine Learning-modellek szoftveres](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-retrain-models-programmatically).

Automatizálása a hello modell betanítási Azure Data Factory használatával kapcsolatos további információkért lásd: [frissítése Azure Machine Learning modellek használata az Update-Erőforrástevékenység](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-azure-ml-update-resource-activity).

## <a name="existing-documentation"></a>Meglévő dokumentáció
[A Microsoft Azure hitelesített toogrow a felhő üzleti](https://azure.microsoft.com/en-us/marketplace/programs/certified/)

[A Microsoft Azure Cortana Intellignece tanúsítva](https://azure.microsoft.com/en-us/marketplace/programs/certified/cortana/)

