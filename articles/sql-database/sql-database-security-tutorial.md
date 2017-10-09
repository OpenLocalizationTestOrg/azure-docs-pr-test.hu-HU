---
title: "aaaSecure az Azure SQL Database adatbázishoz |} Microsoft Docs"
description: "További információk a módszerek és szolgáltatások toosecure az Azure SQL-adatbázis."
services: sql-database
documentationcenter: 
author: DRediske
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: 
ms.date: 06/28/2017
ms.author: daredis
ms.openlocfilehash: 1450d633d6f65faf1b8a2dc0dc7dfe996fb0719d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-your-azure-sql-database"></a>Az Azure SQL-adatbázis védelme

SQL-adatbázis adatait korlátozza a hozzáférést tooyour adatbázis tűzfalszabályokat igénylő felhasználók tooprove azok identitáskezelési és engedélyezési toodata szerepköralapú tagságát és engedélyeket, valamint a hitelesítési mechanizmusok használatával biztonságossá tételére. a sorszintű biztonsági és dinamikus adatmaszkolási.

Az adatbázis rosszindulatú felhasználók vagy néhány egyszerű lépésben jogosulatlan hozzáférés elleni védelmének hello növelhető. Ebben az oktatóanyagban elsajátíthatja, hogy: 

> [!div class="checklist"]
> * A kiszolgáló hello Azure-portálon a kiszolgálószintű tűzfal-szabályok beállítása
> * Az adatbázisban az SSMS használatával vonatkozó adatbázis-szintű tűzfalszabályok beállítása
> * Csatlakozás a biztonságos kapcsolati karakterláncok használatával tooyour adatbázis
> * Felhasználói hozzáférés kezelése
> * A titkosított adatok védelme
> * SQL-adatbázis naplózásának engedélyezése
> * SQL-adatbázis fenyegetésészlelés engedélyezéséhez

Ha nem rendelkezik Azure-előfizetéssel, [ingyenes fiók létrehozását](https://azure.microsoft.com/free/) megkezdése előtt.

## <a name="prerequisites"></a>Előfeltételek

toocomplete ezen oktatóanyag, győződjön meg arról, hogy rendelkezik a következő hello:

- Telepített hello legújabb verziójának [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS). 
- Telepített Microsoft Excel
- Létrehozott egy Azure SQL server és adatbázis - lásd [hello Azure-portálon az Azure SQL-adatbázis létrehozása](sql-database-get-started-portal.md), [hello Azure parancssori felület használatával egyetlen Azure SQL-adatbázis létrehozása](sql-database-get-started-cli.md), és [hozzon létre egy Azure SQL adatbázis-PowerShell-lel](sql-database-get-started-powershell.md). 

## <a name="log-in-toohello-azure-portal"></a>Jelentkezzen be toohello Azure-portálon

Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Hozzon létre egy kiszolgálószintű tűzfalszabályt hello Azure-portálon

SQL-adatbázisok védelmének tűzfal az Azure-ban. Alapértelmezés szerint minden kapcsolatok toohello server és hello adatbázisok hello-kiszolgálón belül kivételével az egyéb Azure-szolgáltatásokhoz érkező kapcsolatokat utasítja el. További információkért lásd: [Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok](sql-database-firewall-configure.md).

Ez a legbiztonságosabb konfiguráció hello tooset "Hozzáférés engedélyezése tooAzure szolgáltatások" tooOFF. Ha tooconnect toohello adatbázis egy Azure virtuális vagy felhőalapú szolgáltatásból kell, hogy hozzon létre egy [foglalt IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) , és csak hello szolgáltatás számára fenntartott IP-cím elérést hello tűzfalon keresztül teszi lehetővé. 

Kövesse az alábbi lépéseket toocreate egy [SQL-adatbázis kiszolgálószintű tűzfalszabály](sql-database-firewall-configure.md) egy adott IP-címről a kiszolgáló tooallow kapcsolatokhoz. 

> [!NOTE]
> Ha létrehozott egy mintaadatbázis az Azure-ban egy előző oktatóanyagok hello vagy quickstarts és egy számítógépen hello az oktatóanyag végrehajtásával azonos IP-címet, hogy az informatikai rendelkezett, amikor ezen oktatóanyag segítségével, telefonon, ezt a lépést kihagyhatja, hogy már létrehozott egy kiszolgálószintű tűzfalszabályt.
>

1. Kattintson a **SQL-adatbázisok** adatbázisból hello bal menüre, majd hello szeretné tooconfigure hello tűzfalszabályt a hello **SQL-adatbázisok** lap. hello áttekintő lapjára jut a adatbázis megnyílik, teljes mértékben hello megjelenítő minősített kiszolgáló neve (például **mynewserver-20170313.database.windows.net**) és további konfigurációs lehetőségeket.

      ![kiszolgálói tűzfalszabály](./media/sql-database-security-tutorial/server-firewall-rule.png) 

2. Kattintson a **kiszolgáló tűzfalának beállítása** hello eszköztár hello előző ábrának megfelelően. Hello **tűzfalbeállítások** hello SQL Database-kiszolgálóhoz tartozó lapon nyílik meg. 

3. Kattintson a **ügyfél IP-cím hozzáadása** az eszköztár tooadd hello nyilvános IP-címe hello számítógéphez csatlakoztatott toohello portál hello vagy hello tűzfalszabály manuálisan adja meg, majd kattintson **mentése**.

      ![kiszolgálótűzfal-szabály beállítása](./media/sql-database-security-tutorial/server-firewall-rule-set.png) 

4. Kattintson a **OK** majd hello **X** tooclose hello **tűzfalbeállítások** lap.

Csatlakoztathatja a hello server tooany adatbázis a megadott hello IP-cím vagy IP-címtartomány.

> [!NOTE]
> Az SQL Database az 1433-as porton kommunikál. Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a 1433-as port kimenő forgalmát. Ha igen, csak akkor tudja tooconnect tooyour Azure SQL adatbázis-kiszolgáló kivéve, ha az IT-részleg megnyitja az 1433-as port.
>

## <a name="create-a-database-level-firewall-rule-using-ssms"></a>Hozzon létre egy adatbázis-szintű tűzfalszabályt SSMS használatával

Adatbázis-szintű tűzfalszabályok lehetővé teszik a különböző tűzfalbeállítások toocreate azonos logikai kiszolgáló és toocreate tűzfalszabályokat, amelyek hordozható – azaz követik az hello adatbázis során hello belül a különböző adatbázisokhoz egy [feladatátvételi ](sql-database-geo-replication-overview.md) hello SQL-kiszolgálón tárolt helyett. Adatbázis-szintű tűzfalszabályok csak akkor lehet Transact-SQL-utasítások segítségével konfigurált csak hello első kiszolgálószintű tűzfalszabály konfigurálása után. További információkért lásd: [Azure SQL Database kiszolgáló- és adatbázis tűzfalszabályok](sql-database-firewall-configure.md).

A következő ezeket a lépéseket toocreate egy adatbázis-specifikus tűzfalszabályt.

1. Csatlakozás tooyour adatbázis, például a használatával [SQL Server Management Studio](./sql-database-connect-query-ssms.md).

2. Az Object Explorerben kattintson a jobb gombbal a hello adatbázis tooadd tűzfal vonatkozó szabály, és kattintson a kívánt **új lekérdezés**. Üres lekérdezés megnyílik egy ablak, amely csatlakoztatott tooyour adatbázis.

3. Hello lekérdezési ablakban hello IP cím tooyour nyilvános IP-cím módosítása, és majd hajtsa végre a lekérdezés a következő hello:

    ```sql
    EXECUTE sp_set_database_firewall_rule N'Example DB Rule','0.0.0.4','0.0.0.4';
    ```

4. Hello eszköztáron kattintson **Execute** toocreate hello tűzfalszabály.

## <a name="view-how-tooconnect-an-application-tooyour-database-using-a-secure-connection-string"></a>Hogyan tooconnect egy alkalmazás tooyour adatbázis használata a biztonságos kapcsolati karakterláncok megjelenítése

tooensure egy ügyfélalkalmazást és az SQL-adatbázis közötti biztonságos, titkosított kapcsolatot, hello kapcsolati karakterlánc toobe konfigurálva van:

- Egy titkosított kapcsolati kérelmek és
- toonot megbízhatósági hello kiszolgálói tanúsítvány. 

Ez kapcsolatot hoz létre Transport Layer Security (TLS) használatával, és csökkenti-átjárójának hello kockázatát. Szerezhetők be megfelelően konfigurált kapcsolati karakterláncokat az SQL-adatbázis, a támogatott ügyfél illesztőprogramok hello Azure-portálon az ADO.net ezen a képernyőfelvételen látható módon.

1. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap.

2. A hello **áttekintése** az adatbázis lapján kattintson **adatbázis-kapcsolati karakterláncok megjelenítése**.

3. Felülvizsgálati hello teljes **ADO.NET** kapcsolati karakterláncot.

    ![ADO.NET kapcsolati karakterlánc](./media/sql-database-security-tutorial/adonet-connection-string.png)

## <a name="creating-database-users"></a>Adatbázis-felhasználók létrehozásával

Mielőtt létrehozna azokat a felhasználókat, először közül kell választania az Azure SQL Database által támogatott két hitelesítési típusok egyikét: 

**SQL-hitelesítés**, felhasználónevet és jelszót használó a bejelentkezések során, és a felhasználók, akik érvényes csak a hello logikai kiszolgáló az adott adatbázis környezetében. 

**Az Azure Active Directory-hitelesítéssel**, amely Azure Active Directory által kezelt identitások használja. 

Ha azt szeretné, hogy toouse [Azure Active Directory](./sql-database-aad-authentication.md) tooauthenticate SQL-adatbázis, a feltöltött Azure Active Directory léteznie kell a folytatás előtt.

Kövesse ezeket a lépéseket toocreate SQL-hitelesítést használó felhasználó:

1. Csatlakozás tooyour adatbázis, például a használatával [SQL Server Management Studio](./sql-database-connect-query-ssms.md) server rendszergazdai hitelesítő adataival.

2. Az Object Explorerben kattintson a jobb gombbal a hello adatbázis, a kívánt tooadd egy új felhasználót, majd kattintson az **új lekérdezés**. Üres lekérdezés megnyílik egy ablak, amely csatlakoztatott toohello kiválasztott adatbázishoz.

3. Adja meg a következő lekérdezés hello hello lekérdezési ablakban:

    ```sql
    CREATE USER ApplicationUser WITH PASSWORD = 'YourStrongPassword1';
    ```

4. Hello eszköztáron kattintson **Execute** toocreate hello felhasználó.

5. Alapértelmezés szerint a hello felhasználói kapcsolódhatnak toohello adatbázis, de nincsenek engedélyek tooread vagy írási adatok. toogrant ezen engedélyek toohello az újonnan létrehozott felhasználó, a következő két parancsot egy új lekérdezési ablak hello végrehajtása

    ```sql
    ALTER ROLE db_datareader ADD MEMBER ApplicationUser;
    ALTER ROLE db_datawriter ADD MEMBER ApplicationUser;
    ```

A nem rendszergazdai fiókok hello adatbázis szintű tooconnect tooyour adatbázis-kivéve, ha szüksége tooexecute rendszergazdai feladatokat, például új felhasználók létrehozása ajánlott eljárás toocreate. Tekintse át a hello [Azure Active Directory-oktatóanyag](./sql-database-aad-authentication-configure.md) hogyan tooauthenticate Azure Active Directory használatával.


## <a name="protect-your-data-with-encryption"></a>A titkosított adatok védelme

Az Azure SQL Database átlátható adattitkosítás (TDE) automatikusan titkosítja az adatok aktívan, anélkül, hogy a módosítások toohello elérése során hello titkosított adatbázis-alkalmazás. Az újonnan létrehozott adatbázisok esetén TDE alapértelmezés szerint van bekapcsolva. az adatbázis vagy tooverify, amely TDE, a TDE tooenable kövesse az alábbi lépéseket:

1. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 

2. Kattintson a **átlátható adattitkosítás** tooopen hello konfigurálása lapon TDE.

    ![Transzparens adattitkosítás](./media/sql-database-security-tutorial/transparent-data-encryption-enabled.png)

3. Ha szükséges, állítsa be **adattitkosítás** tooON kattintson **mentése**.

hello titkosítási folyamat elindul hello háttérben. Kísérheti hello segítségével csatlakozó tooSQL adatbázis [SQL Server Management Studio](./sql-database-connect-query-ssms.md) hello encryption_state oszlopa hello lekérdezésével `sys.dm_database_encryption_keys` nézet.

## <a name="enable-sql-database-auditing-if-necessary"></a>SQL-adatbázis naplózásának engedélyezése, ha szükséges

Az Azure SQL Database Auditing követi az adatbázisban, és beírja őket az Azure Storage-fiókban tooan naplót. Naplózás segít törvényi megfelelőség fenntartásában, ismerje meg adatbázis-tevékenység, és betekintést az eltérések és rendellenességek feltárását, jelezheti a potenciális biztonsági problémát. Kövesse ezeket a lépéseket toocreate az SQL-adatbázis alapértelmezett naplózási házirend:

1. Válassza ki **SQL-adatbázisok** hello bal oldali menüben kattintson a hello adatbázis **SQL-adatbázisok** lap. 

2. Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**. Figyelje meg, hogy kiszolgálószintű naplózás letiltott állapotában és, hogy egy **beállításainak megtekintéséhez** hivatkozás, amely lehetővé teszi a tooview hello kiszolgáló naplózási beállításainak és módosítása az ebben a környezetben.

    ![Naplózás panel](./media/sql-database-security-tutorial/auditing-get-started-settings.png)

3. Ha jobban szeret tooenable egy naplózási típust (vagy a hely?) különbözik a hello kiszolgálószinten megadott hello kapcsolja **ON** naplózás esetén hello válassza **Blob** naplózás típusát. Ha server Blobnaplózási funkció engedélyezve van, hello konfigurált adatbázis-ellenőrzéshez és hello server Blob naplózási jelen.

    ![A naplózás bekapcsolása](./media/sql-database-security-tutorial/auditing-get-started-turn-on.png)

4. Válassza ki **tárolási részletek** tooopen hello naplózási naplók tárolás panel. Jelölje be hello Azure storage-fiók naplók menteni és hello megőrzési időtartam, mely hello után törlődik a régi naplókat, majd kattintson **OK** hello lap alján. 

   > [!TIP]
   > Használja az azonos hello összes naplózott adatbázisok tooget hello hatékonyabb működtetését hello naplózás tárfiók jelentések sablonok.
   > 

5. Kattintson a **Save** (Mentés) gombra.

> [!IMPORTANT]
> Ha azt szeretné, hogy toocustomize hello naplózott események, ehhez PowerShell vagy a REST API - lásd: hello [automatizálási (PowerShell / REST-API)](sql-database-auditing.md#subheading-7) című szakaszban talál további információt.
>

## <a name="enable-sql-database-threat-detection"></a>SQL-adatbázis fenyegetésészlelés engedélyezéséhez

A Fenyegetésészlelés biztonsági, amely lehetővé teszi, hogy az ügyfelek toodetect és toopotential fenyegetések válaszol, a rendellenes tevékenységek adja meg a biztonsági riasztások előforduló új réteget biztosít. Felhasználók felfedezheti hello gyanús események SQL Database Auditing toodetermine használatával, ha egy kísérlet tooaccess, a biztonsági szabályok megsértésére eredménye, vagy hello adatbázisban kihasználására. A Fenyegetésészlelés teszi egyszerű tooaddress potenciális fenyegetések toohello adatbázis hello kell toobe egy biztonsági szakértő nélkül, vagy speciális biztonsági rendszerek figyelése kezelése.
A Fenyegetésészlelés például bizonyos adatbázist érintő rendellenes tevékenységeket utaló esetleges SQL injektálási kísérletek begyűjtésére észleli. SQL-injektálás az egyik hello közös webes alkalmazás biztonsági problémái hello Internet, adatvezérelt használt tooattack alkalmazások. A támadók előnyeit alkalmazás biztonsági rések tooinject rosszindulatú SQL-utasítások mezőkbe alkalmazás bejegyzést, megsértése vagy hello adatbázis adatainak módosítása.

1. Keresse meg a toohello konfigurációs panelen hello toomonitor kívánt SQL-adatbázis található. Hello-beállítások panelen válassza ki **naplózás és Fenyegetésészlelés**.

    ![Navigációs ablaktábla](./media/sql-database-security-tutorial/auditing-get-started-settings.png)
2. A hello **naplózás és Fenyegetésészlelés** konfigurációs panelen kapcsolja **ON** naplózás, amely megjeleníti hello fenyegetés észlelési beállítások.

3. Kapcsolja be **ON** veszélyforrások detektálása.

4. Konfigurálhatja az e-mailek adatbázist érintő rendellenes tevékenységeket észlelése a biztonsági riasztásokat fogadó hello listáját.

5. Kattintson a **mentése** a hello **naplózási & Threat detection** panel toosave hello új vagy frissített naplózás és a fenyegetések szabályzat.

    ![Navigációs ablaktábla](./media/sql-database-security-tutorial/td-turn-on-threat-detection.png)

    Adatbázist érintő rendellenes tevékenységeket a rendszer észleli, ha adatbázist érintő rendellenes tevékenységeket a észlelésekor e-mailben értesítést fog kapni. hello e-mail hello gyanús biztonsági esemény hello rendellenes tevékenységek, az adatbázis neve, a kiszolgáló nevét és hello esemény időpontja hello jellege beleértve tájékoztatást fogunk adni. Emellett az információt nyújt a lehetséges okok és ajánlott műveletek tooinvestigate és hello potenciális fenyegetést toohello adatbázis mérsékelni. hello milyen toodo végig kell hogy következő lépések bejárása ilyen egy e-maileket kapni:

    ![Fenyegetések észlelése e-mail](./media/sql-database-threat-detection-get-started/4_td_email.png)

6. Hello e-mailben, kattintson a hello **Azure SQL-naplózás napló** hivatkozás, amely hello Azure-portálon elindul, és hello vonatkozó naplózási bejegyzések hello gyanús esemény hello időpontja körül megjelenítése.

    ![Auditálási rekordok](./media/sql-database-threat-detection-get-started/5_td_audit_records.png)

7. Kattintson a hello naplózási bejegyzések tooview további részleteket a hello adatbázis gyanús tevékenységek, például az SQL-utasítás hiba okát, és az ügyfél IP.

    ![Rekord részletei](./media/sql-database-security-tutorial/6_td_audit_record_details.png)

8. Hello rekordok naplózása paneljén kattintson **Megnyitás az Excelben** előre konfigurált tooopen excel-sablon tooimport és futtatási mélyebb elemzésének hello audit napló hello gyanús esemény hello időpontja körül.

    > [!NOTE]
    > Az Excel 2010 vagy újabb, a Power Query és hello **gyors összevonás** beállításra akkor szükség.

    ![Nyitott rekordokat az Excel programban](./media/sql-database-threat-detection-get-started/7_td_audit_records_open_excel.png)

9. tooconfigure hello **gyors összevonás** beállítás - hello **POWER QUERY** menüszalag lapon jelölje be **beállítások** toodisplay hello beállítások párbeszédpanelen. Jelölje ki hello adatvédelmi szakaszt, és válassza ki a hello második lehetőség - "Hello adatvédelmi szintek figyelmen kívül, és a teljesítmény lehetséges javítása":

    ![Gyors Excel egyesítése](./media/sql-database-threat-detection-get-started/8_td_excel_fast_combine.png)

10. tooload SQL naplókat, győződjön meg arról, hogy hello paraméterek hello beállítások lapon helyesen van beállítva és majd hello "Data" menüszalagon válassza hello "Az összes frissítés" gombra.

    ![Excel-paraméterek](./media/sql-database-threat-detection-get-started/9_td_excel_parameters.png)

11. hello eredményei jelennek meg hello **SQL naplók** lap, amely mélyebb elemzése toorun hello rendellenes tevékenységet észlelt, és csökkenteni hello hatását hello biztonsági esemény az alkalmazás lehetővé teszi.


## <a name="next-steps"></a>Következő lépések
Az adatbázis rosszindulatú felhasználók vagy néhány egyszerű lépésben jogosulatlan hozzáférés elleni védelmének hello növelhető. Ebben az oktatóanyagban elsajátíthatja, hogy: 

> [!div class="checklist"]
> * A kiszolgáló és vagy az adatbázis vonatkozó tűzfalszabályok beállítása
> * Csatlakozás a biztonságos kapcsolati karakterláncok használatával tooyour adatbázis
> * Felhasználói hozzáférés kezelése
> * A titkosított adatok védelme
> * SQL-adatbázis naplózásának engedélyezése
> * SQL-adatbázis fenyegetésészlelés engedélyezéséhez

> [!div class="nextstepaction"]
>[Az SQL Database teljesítményének növelése](sql-database-performance-tutorial.md)

