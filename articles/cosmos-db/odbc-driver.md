---
title: "aaaConnect tooAzure Cosmos DB BI elemzőeszközök segítségével |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure Cosmos DB ODBC-illesztőprogram toocreate táblák és nézetek, hogy a BI és az adatok analytics szoftver normalizált adatok tekinthetők."
keywords: "ODBC, odbc-illesztőprogram"
services: cosmos-db
author: mimig1
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 9967f4e5-4b71-4cd7-8324-221a8c789e6b
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: rest-api
ms.topic: article
ms.date: 05/24/2017
ms.author: mimig
ms.openlocfilehash: e12a70f7805445f09fac01411e4bfbccc9a29c7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooazure-cosmos-db-using-bi-analytics-tools-with-hello-odbc-driver"></a>Csatlakozás tooAzure Cosmos DB BI elemzőeszközök segítségével hello ODBC-illesztőprogram

hello Azure Cosmos DB ODBC illesztőprogram lehetővé teszi, hogy tooconnect tooAzure Cosmos Adatbázisba BI analytics használatával eszközök, például az SQL Server Integration Services, a Power BI Desktop és a Tableau, hogy a képi megjelenítések Azure Cosmos DB adatait létrehozása és elemzése megoldások.

hello Azure Cosmos DB ODBC-illesztőprogram ODBC 3.8 megfelelő, és támogatja az ANSI SQL-92 szintaxis. hello illesztőprogram ajánlatok funkciók gazdag választékát toohelp meg renormalize adatokat az Azure Cosmos-Adatbázisba. Hello illesztőprogrammal táblák és nézetek az Adatbázisba az Azure Cosmos-adatait tartalmazzák. hello illesztőprogram lehetővé teszi SQL-műveletek tooperform hello táblák és nézetek, beleértve a lekérdezéseket, a beszúrások, a frissítések és a törlések csoportosítás ellen.

## <a name="why-do-i-need-toonormalize-my-data"></a>Miért kell toonormalize adataimat?
Azure Cosmos-adatbázis egy séma nélküli adatbázisban, így az alkalmazások tooiterate engedélyezésével lehetővé teszi az alkalmazások gyors fejlesztést adataikat hello menet közben a modell, és nem korlátozza őket tooa szigorú séma. Egyetlen Azure Cosmos DB adatbázisban tartalmazhat különböző struktúrák JSON-dokumentumokat. Ez gyors alkalmazásfejlesztés nagy, de ha szeretné, hogy tooanalyze, és az adatok adatelemzés és BI eszközök használatával jelentéseket készíthet, hello adatok gyakran egybesimított toobe kell és tooa adott séma igazodik.

Ez azért, ahol hello ODBC-illesztőprogram érkezik. Hello ODBC-illesztőprogram használatával azt is most adatokat az Adatbázisba az Azure Cosmos táblák és nézetek tooyour adatok elemzési és jelentéskészítési illeszkedő renormalized kell. hello renormalized sémák befolyásolni hello alapjául szolgáló adatok, és nem korlátozza a fejlesztők tooadhere toothem, egyszerűen lehetővé teszik az Ön tooleverage ODBC-kompatibilis eszközök tooaccess hello adatokat. Ezért most az Azure Cosmos DB adatbázis nem csak lesz a fejlesztői csapat kedvenc, de az adatelemzők szeretni fognak azt túl.

Most már megadható, hogy a hello ODBC-illesztőprogram megismerésében.

## <a id="install"></a>1. lépés: Hello Azure Cosmos DB ODBC-illesztőprogram telepítése

1. Töltse le a környezet hello illesztőprogramjait:

    * [A Microsoft Azure Cosmos DB ODBC 64-bit.msi](https://aka.ms/documentdb-odbc-64x64) 64 bites Windows
    * [A Microsoft Azure Cosmos DB ODBC 32 x 64-bit.msi](https://aka.ms/documentdb-odbc-32x64) a 32 bites, 64 bites Windows rendszeren
    * [A Microsoft Azure Cosmos DB ODBC 32-bit.msi](https://aka.ms/documentdb-odbc-32x32) 32 bites Windows

    Futtatási hello msi fájl helyileg, mely indítása hello **Microsoft Azure Cosmos DB ODBC illesztőprogram telepítővarázsló**. 
2. Hello alapértelmezett bemeneti tooinstall hello ODBC-illesztőprogram használatával hello telepítési varázsló befejezéséhez.
3. Nyissa meg hello **ODBC-adatforrás felügyeleti** alkalmazást a számítógépen, akkor ehhez írja be **ODBC adatforrások** a hello Windows keresse a mezőbe. 
    Ellenőrizheti a hello illesztőprogram lett telepítve a hello kattintva **illesztőprogramok** lapra, és győződjön meg arról **Microsoft Azure Cosmos DB ODBC-illesztőprogram** szerepel.

    ![Az Azure Cosmos DB ODBC-adatforrás felügyelő](./media/odbc-driver/odbc-driver.png)

## <a id="connect"></a>2. lépés: Csatlakozás tooyour Azure Cosmos DB adatbázis

1. Után [telepítése hello Azure Cosmos DB ODBC-illesztőprogram](#install), a hello **ODBC-adatforrás felügyelő** ablak, kattintson a **Hozzáadás**. Létrehozhat egy felhasználó vagy rendszer DSN-NEVET. Ebben a példában létrehozzuk a felhasználó DSN-NEVET.
2. A hello **új adatforrás létrehozása** ablakban válassza ki **Microsoft Azure Cosmos DB ODBC-illesztőprogram**, és kattintson a **Befejezés**.
3. A hello **Azure Cosmos DB ODBC illesztőprogram SDN-alapú telepítő** ablak, írja be a következő hello: 

    ![Az Azure Cosmos DB ODBC illesztőprogram DSN beállítása ablakban](./media/odbc-driver/odbc-driver-dsn-setup.png)
    - **Adatforrás neve**: hello ODBC Adatforrásnevet saját rövid nevét. Ez a név egyedi tooyour Azure Cosmos DB fiók, ezért nevezze el megfelelően, ha több fiókot.
    - **Leírás**: hello adatforrás rövid leírása.
    - **Állomás**: URI Azure Cosmos DB fiókjához. Beolvasható ez hello Azure Cosmos DB kulcsok paneljén hello Azure-portálon, ahogy az alábbi képernyőfelvétel a hello. 
    - **Hívóbetű**: hello hello Azure Cosmos DB kulcsok paneljén hello Azure-portálon az elsődleges vagy másodlagos, olvasási és írási vagy olvasási kulcs, ahogy az alábbi képernyőfelvétel a hello. Azt javasoljuk, hogy hello DSN használata csak olvasható adatok feldolgozása és a jelentéskészítési hello írásvédett kulcsot használ.
    ![Az Azure Cosmos DB kulcsok panelen](./media/odbc-driver/odbc-driver-keys.png)
    - **A hozzáférési kulcs titkosításához**: válassza ki a legjobb választás hello hello felhasználókat a gépet. 
4. Kattintson a hello **teszt** gomb toomake, hogy csatlakozni tud az tooyour Azure Cosmos DB fiók. 
5. Kattintson a **speciális beállítások** és hello beállítása a következő értékeket:
    - **Lekérdezési konzisztencia**: Select hello [konzisztenciaszint](consistency-levels.md) a műveletekhez. hello alapértelmezett érték a munkamenet.
    - **Az újrapróbálkozások számát**: Adja meg a hello ennyiszer tooretry Ha hello kezdeti kérelem befejezése sikertelennek bizonyul tooservice szabályozás miatt egy művelet.
    - **Sémafájl**: több lehetőség itt rendelkeznek.
        - Alapértelmezés szerint ez a bejegyzés (üres), így hello illesztőprogram összes gyűjtemények toodetermine hello séma egyes gyűjtemények hello első oldal adatok ellenőrzése. Ez más néven gyűjtemény a hozzárendelése. Nélkül definiált sémafájl hello illesztőprogram tooperform hello vizsgálat mindegyik illesztőprogram munkamenethez rendelkezik, és egy magasabb indítási ideje hello DSN használó alkalmazások eredményezheti. Azt javasoljuk, hogy mindig társít egy sémafájl a DSN-hez.
        - Ha már rendelkezik egy sémafájl (valószínűleg egy létrehozott hello [séma szerkesztő](#schema-editor)), kattintson **Tallózás**tooyour fájl lépjen, kattintson **mentése**, és kattintson a **OK**.
        - Egy új séma toocreate, kattintson a **OK**, és kattintson a **séma szerkesztő** hello fő ablakban. Majd folytassa a toohello [séma szerkesztő](#schema-editor) információkat. Hello új séma fájl létrehozása után ne feledje toogo hátsó toohello **speciális beállítások** ablak tooinclude az újonnan létrehozott hello sémafájl.

6. Miután befejezte a, és zárja be a hello **Azure Cosmos DB ODBC illesztőprogram DSN beállítása** ablak, új felhasználói DSN van hello hozzáadott toohello felhasználói DSN lap.

    ![Új Azure Cosmos DB ODBC Adatforrásnevet hello felhasználói DSN lap](./media/odbc-driver/odbc-driver-user-dsn.png)

## <a id="#collection-mapping"></a>3. lépés: Hello gyűjtemény leképezési metódussal sémadefiníciót létrehozása

Két különböző mintavételi módszerek közül választhat: **gyűjtemény leképezési** vagy **tábla-határolójelek**. A mintavételi munkamenet nyújthatnak mindkét mintavételi módszer, de egyes gyűjtemények csak egy adott mintavételi metódus használatával. hello lépéseket hello adatok sémát egy vagy több gyűjteményt hello gyűjtemény leképezési módszerrel hozzon létre. Ez a mintavételi módszer lekéri hello adatokat egy gyűjtemény toodetermine hello struktúra hello adatok hello oldalán. Egy gyűjtemény tooa tábla a hello ODBC ügyféloldali transposes azt. A mintavételi metódus gyors és hatékony homogén hello adatok egy gyűjtemény esetén. Ha egy gyűjtemény heterogén típusú adatot tartalmaz, azt javasoljuk, használjon hello [tábla-határolójelek metódus leképezése](#table-mapping) lehetővé teszi egy megbízhatóbb mintavételi módszer toodetermine hello adatstruktúrák hello gyűjteményben. 

1. Lépéseket 1-4 befejezése után [Connect tooyour Azure Cosmos DB adatbázis](#connect), kattintson a **séma szerkesztő** a hello **Azure Cosmos DB ODBC illesztőprogram DSN beállítása** ablak.

    ![Hello Azure Cosmos DB ODBC illesztőprogram DSN beállítása ablakban séma szerkesztő gombra](./media/odbc-driver/odbc-driver-schema-editor.png)
2. A hello **séma szerkesztő** ablak, kattintson a **hozzon létre új**.
    Hello **készítése séma** ablak hello Azure Cosmos DB fiók hello összes gyűjteményt jeleníti meg. 
3. Válassza ki egy vagy több gyűjteményt toosample, és kattintson a **minta**. 
4. A hello **Tervező nézetben** lapon, a hello adatbázis, a séma és a tábla. Hello tábla nézetben hello vizsgálat hello oszlopnevek (SQL-neve, adatforrás neve, stb.) társított tulajdonságok hello készletét jeleníti meg.
    Az oszlopok, módosíthatja a hello SQL oszlopnév, hello SQL-típus, SQL hosszát (ha van ilyen), a Méretezés (ha van ilyen), a pontosság (ha alkalmazható) és a Nullable.
    - Beállíthat **oszlop elrejtése** túl**igaz** Ha tooexclude adott oszlopot a lekérdezés eredményeit. Oszlopok jelölte meg az oszlop elrejtése = true nem lehet megjeleníteni a kijelölés és a leképezés, annak ellenére, hogy azok továbbra is hello séma része. Például elrejthetők kezdődő, és "_" hello Azure Cosmos DB a rendszer szükséges tulajdonságokat.
    - Hello **azonosító** oszlop, amely nem lehet rejtett hello normalizált sémában hello elsődleges kulcsként használt hello mezőhöz. 
5. Ha hello séma meghatározása végzett, kattintson a **fájl** | **mentése**, keresse meg a hello toosave toohello címtárséma, és kattintson **mentése**.

    Ha a jövőbeli hello toouse ebben a sémában az adatbázis, hello Azure Cosmos DB ODBC illesztőprogram DSN beállítása ablakban (keresztül hello ODBC-adatforrás felügyelő), kattintson a Speciális beállítások, és hello sémafájl mezőbe, majd keresse meg séma mentett toohello. A séma fájl tooan mentése meglévő DSN módosítja a hello DSN kapcsolati tooscope toohello adatait és struktúra séma határozza meg.

## <a id="table-mapping"></a>4. lépés:, Hozzon létre hello tábla-határolójelek használata sémadefiníciót metódus leképezése

Két különböző mintavételi módszerek közül választhat: **gyűjtemény leképezési** vagy **tábla-határolójelek**. A mintavételi munkamenet nyújthatnak mindkét mintavételi módszer, de egyes gyűjtemények csak egy adott mintavételi metódus használatával. 

hello lépések hozzon létre egy séma hello adatok hello segítségével egy vagy több gyűjteményt **tábla-határolójelek** metódus leképezése. Azt javasoljuk, hogy ezt a módszert mintavételi, ha a gyűjtemények heterogén típusú adatok tartalmazzák. A metódus tooscope hello mintavételi attribútumok és a megfelelő értékek tooa készletét használhatja. Például egy dokumentum tartalmaz egy "Type" tulajdonságot, ha hatókörét megadhatja a hello mintavételi toohello értékek ennek a tulajdonságnak. hello végeredménynek hello mintavételi lenne az egyes hello értékeket a megadott típus táblázatok halmazát. Írja be például = Car a művelet létrehoz egy autó tábla típusú közben = Vezérlősík Vezérlősík tábla eredményezne.

1. Lépéseket 1-4 befejezése után [Connect tooyour Azure Cosmos DB adatbázis](#connect), kattintson a **séma szerkesztő** hello Azure Cosmos DB ODBC illesztőprogram DSN beállítása ablakban.
2. A hello **séma szerkesztő** ablak, kattintson a **hozzon létre új**.
    Hello **készítése séma** ablak hello Azure Cosmos DB fiók hello összes gyűjteményt jeleníti meg. 
3. Válasszon ki egy gyűjteményt a hello **minta** lap hello **leképezési Definition** oszlop hello gyűjtemény, kattintson a **szerkesztése**. Ezt a hello **leképezési Definition** ablakban válassza ki **tábla határolójelek** metódust. Majd hello a következő:

    a. A hello **attribútumok** mezőbe egy elválasztó tulajdonság hello nevét. Ez a dokumentum kívánt tooscope hello mintavételi, például város tulajdonság, és nyomja le az enter. 

    b. Csak tooscope hello mintavételi toocertain értékek hello attribútum csak a megadott, hello attribútum hello kiválasztási mezőben válassza ki, majd adjon meg egy értéket hello **érték** mezőben, például Budapesten és nyomja le az adja meg a. Folytatás tooadd attribútumok több értéket. Csak győződjön meg arról, hogy helyes értékeket meg van jelölve attribútum hello.

    Ha például egy **attribútumok** értékének Cityben, és szeretné, hogy toolimit a tábla tooonly Győr és Dubai város értékű sorainak befoglalása, akkor beírnia város hello attribútumok mezőbe, és New York közt, majd a Dubai hello  **Értékek** mezőbe.
4. Kattintson az **OK** gombra. 
5. Hello leképezési definíciók hello gyűjtemények befejezése után toosample hello a kívánt **séma szerkesztő** ablak, kattintson a **minta**.
     Az oszlopok, módosíthatja a hello SQL oszlopnév, hello SQL-típus, SQL hosszát (ha van ilyen), a Méretezés (ha van ilyen), a pontosság (ha alkalmazható) és a Nullable.
    - Beállíthat **oszlop elrejtése** túl**igaz** Ha tooexclude adott oszlopot a lekérdezés eredményeit. Oszlopok jelölte meg az oszlop elrejtése = true nem lehet megjeleníteni a kijelölés és a leképezés, annak ellenére, hogy azok továbbra is hello séma része. Például minden hello Azure Cosmos DB szükséges Rendszertulajdonságok kezdődő, és "_" elrejthetők.
    - Hello **azonosító** oszlop, amely nem lehet rejtett hello normalizált sémában hello elsődleges kulcsként használt hello mezőhöz. 
6. Ha hello séma meghatározása végzett, kattintson a **fájl** | **mentése**, keresse meg a hello toosave toohello címtárséma, és kattintson **mentése**.
7. Vissza a hello **Azure Cosmos DB ODBC illesztőprogram DSN beállítása** ablak, kattintson a ** Speciális beállítások **. Ezt követően a hello **sémafájl** mezőben mentett toohello sémafájl keresse meg és kattintson **OK**. Kattintson a **OK** újra toosave hello DSN-NEVET. Ez menti hello séma létrehozott toohello DSN-NEVET. 

## <a name="optional-creating-views"></a>(Választható) Nézetek létrehozása
Adja meg, és nézetek létrehozása hello mintavételi folyamat részeként. Ezek a nézetek egyenértékű tooSQL nézetei. Ezek csak olvasható és hatókör hello beállításokat és hello Azure Cosmos DB SQL definiált vetületét. 

egy nézet adatai, hello toocreate **séma szerkesztő** ablak hello **nézet definíciók** oszlop, kattintson a **hozzáadása** hello gyűjtemény toosample hello sorát a. Ezt a hello **nézet definíciók** ablakban, a következő hello:
1. Kattintson a **új**adjon hello nézet, például EmployeesfromSeattleView nevét, majd kattintson **OK**.
2. A hello **nézet szerkesztése** ablak, írja be az Azure Cosmos DB lekérdezést. Ez lehet például Azure Cosmos adatbázis SQL-lekérésen`SELECT c.City, c.EmployeeName, c.Level, c.Age, c.Gender, c.Manager FROM c WHERE c.City = “Seattle”`, és kattintson a **OK**.

Létrehozhat egy számos nézet kedve szerint elrendezheti. Miután a definiáló hello nézeteket, majd segítségével mintavételi hello adatokat. 

## <a name="step-5-view-your-data-in-bi-tools-such-as-power-bi-desktop"></a>5. lépés: Az adatok megtekintése a Power BI Desktop Üzletiintelligencia-eszközök

Csak akkor használhatja az új DSN tooconnect DocumentADB bármely ODBC-kompatibilis eszközök – Ez a lépés egyszerűen bemutatja, hogyan tooconnect tooPower BI Desktopban, és hozzon létre egy Power BI képi megjelenítés.

1. Nyissa meg a Power BI Desktop.
2. Kattintson a **adatok**.
3. A hello **adatok beolvasása** ablak, kattintson a **más** | **ODBC** | **Connect**.
4. A hello **az ODBC** ablakban, a select hello az adatforrásnév hozott létre, és kattintson a **OK**. Meghagyhatja hello **speciális beállítások** bejegyzés üres.
5. A hello **ODBC illesztővel adatforrás elérése** ablakban válassza ki **alapértelmezett vagy egyéni** majd **Connect**. Nem kell tooinclude hello **hitelesítőadat-kapcsolatikarakterlánc-tulajdonságokat**.
6. A hello **Navigator** ablakban hello bal oldali ablaktáblán bontsa ki a hello adatbázis, hello séma-, és jelölje ki hello tábla. hello eredménypanelen létrehozott hello sémával hello adatokat tartalmazza.
7. toovisualize hello adatokat a Power BI desktop hello tábla neve elé hello jelölőnégyzetet, majd kattintson a **terhelés**.
8. A Power BI Desktopban a bal szélen hello válassza ki a hello adatok lap ![A Power BI Desktop adatok lap](./media/odbc-driver/odbc-driver-data-tab.png) az adatok importálása tooconfirm.
9. Hozhatók létre látványelemek a Power BI használatával hello jelentés fülre kattintva ![jelentés lapot a Power BI Desktop](./media/odbc-driver/odbc-driver-report-tab.png)gombra, majd **új Visual**, és a csempe majd testreszabása. A Power BI Desktop képi megjelenítések létrehozásával kapcsolatos további információkért lásd: [Power BI-ban a képi megjelenítés típusok](https://powerbi.microsoft.com/documentation/powerbi-service-visualization-types-for-reports-and-q-and-a/).

## <a name="troubleshooting"></a>Hibaelhárítás

Hiba a következő hello megjelenésekor biztosítása hello **állomás** és **hozzáférési kulcs** másolt értékek hello Azure portálra az [2. lépés](#connect) helyes-e, és próbálkozzon újra. Hello másolási gombok toohello sarkában hello használata **állomás** és **hozzáférési kulcs** hello Azure portál toocopy hello értékek hiba szabad értékek.

    [HY000]: [Microsoft][Azure Cosmos DB] (401) HTTP 401 Authentication Error: {"code":"Unauthorized","message":"hello input authorization token can't serve hello request. Please check that hello expected payload is built as per hello protocol, and check hello key being used. Server used hello following payload toosign: 'get\ndbs\n\nfri, 20 jan 2017 03:43:55 gmt\n\n'\r\nActivityId: 9acb3c0d-cb31-4b78-ac0a-413c8d33e373"}`

## <a name="next-steps"></a>Következő lépések

toolearn Azure Cosmos DB, kapcsolatos további információkért lásd: [Mi az Azure Cosmos DB?](introduction.md).
