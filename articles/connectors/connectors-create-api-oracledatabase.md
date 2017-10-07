---
title: "az Azure Logic Apps aaaAdd hello Oracle adatbázis-összekötőjét |} Microsoft Docs"
description: "A logikai alkalmazás hello Oracle-adatbázishoz összekötő használatára"
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/29/2017
ms.author: mandia; ladocs
ms.openlocfilehash: 8a802a6c4782e210ff71848614152cb46ba5d651
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-oracle-database-connector"></a>Hello Oracle-adatbázishoz összekötő az első lépései

Hello Oracle-adatbázishoz connector segítségével hoz létre a meglévő adatbázis adatokat használó szervezeti munkafolyamatok. Ez az összekötő a helyszíni Oracle-adatbázishoz, vagy Oracle Database rendszert futtató Azure virtuális gépként telepített tooan is elérheti. Ez az összekötő segítségével:

* A munkafolyamat egy új ügyfél tooa ügyfelek adatbázist, vagy frissítésétől egészen a rendelések adatbázisban egy rendelés hozhat létre.
* Műveletek tooget soraiban levő adatok használni, új sor beszúrására és még akkor is törli. Például egy rekord létrehozásakor a Dynamics CRM Online (trigger), majd sor beszúrása az Oracle-adatbázisban (művelet). 

Ez a témakör bemutatja, hogyan toouse hello Oracle adatbázis-összekötőt a logikai alkalmazás.

## <a name="prerequisites"></a>Előfeltételek

* Oracle támogatott verziók: 
    * Oracle 9-es és újabb verziók
    * Oracle ügyfélszoftvert 8.1.7-es vagy újabb

* Hello helyszíni adatátjáró telepítése. [A logic apps tooon helyszíni adatokhoz kapcsolódó](../logic-apps/logic-apps-gateway-connection.md) listák hello lépéseket. hello átjáró szükséges tooconnect tooan helyszíni Oracle-adatbázishoz, vagy egy Azure virtuális Gépen az Oracle-adatbázis telepítve. 

    > [!NOTE]
    > hello helyszíni adatátjáró egy hidat és biztonságos helyszíni (adatokat, amely nincs a hello) és a logic Apps alkalmazások közötti adatátvitel tartalmaz. hello ugyanahhoz az átjáróhoz használható több szolgáltatásra, és több adatforrást. Igen előfordulhat, hogy csak akkor kell tooinstall hello átjáró egyszer.

* Telepítse a hello Oracle ügyfél telepítési hello helyszíni adatátjáró hello számítógépen. Győződjön meg arról, hogy az Oracle .NET tooinstall hello 64 bites Oracle-adatszolgáltatóban:  

  [64 bites ODAC Windows x64 12c kiadás 4 (12.1.0.2.4)](http://www.oracle.com/technetwork/database/windows/downloads/index-090165.html)

    > [!TIP]
    > Hello Oracle ügyfél nincs telepítve, ha hiba lép fel, próbálkozzon toocreate vagy hello kapcsolat használata. Gyakori hibák hello ebben a témakörben talál.


## <a name="add-hello-connector"></a>Hello összekötő hozzáadása

> [!IMPORTANT]
> Ez az összekötő nem rendelkezik a eseményindítókat. Csak a műveleteket tartalmaz. Így a logikai alkalmazás létrehozásakor adja hozzá egy másik eseményindító toostart a Logic Apps alkalmazást, mint **ütemezés - ismétlődési**, vagy **kérelem / válasz - válasz**. 

1. A hello [Azure-portálon](https://portal.azure.com), egy üres logikai alkalmazás létrehozása.

2. Elején hello a Logic Apps alkalmazást, válassza ki a hello **kérelem / válasz - kérelem** eseményindító: 

    ![](./media/connectors-create-api-oracledatabase/request-trigger.png)

3. Kattintson a **Mentés** gombra. Mentésekor, a rendszer automatikusan előállítja a kérelem URL-CÍMÉT. 

4. Válassza ki **új lépés**, és válassza ki **művelet hozzáadása**. Írja be a `oracle` toosee hello elérhető műveletek: 

    ![](./media/connectors-create-api-oracledatabase/oracledb-actions.png)

    > [!TIP]
    > Ez egyben hello leggyorsabb módon toosee hello eseményindítók és műveletek a csatlakozókat érhető el. Írjon be egy részén hello összekötő nevét, például `oracle`. hello designer bármely eseményindítók és műveletek sorolja fel. 

5. Válasszon egyet a hello műveletek, például a **Oracle adatbázis - Get sor**. Válassza ki **keresztül, a helyszíni adatátjáró**. Adja meg a hello Oracle-kiszolgáló neve, a hitelesítési módszert, a felhasználónevet, a jelszó és a válassza hello átjáró:

    ![](./media/connectors-create-api-oracledatabase/create-oracle-connection.png)

6. A csatlakozás után hello listából válasszon táblát, és adja meg a hello sor azonosítója tooyour tábla. Tooknow hello azonosító toohello táblában van szüksége. Ha nem ismeri, a Oracle adatbázis rendszergazdájától, és hello eredményének beolvasása `select * from yourTableName`. Ez lehetőséget biztosít, hello tooproceed kell azonosításra alkalmas adatokat.

    A következő példa hello, a feladat visszaadott adat egy az emberi erőforrások adatbázisból: 

    ![](./media/connectors-create-api-oracledatabase/table-rowid.png)

7. A következő lépésben használhatja bármelyik hello más összekötők toobuild a munkafolyamat. Ha szeretné, hogy Oracle tootest beolvasásakor adatait, majd küldhet magának hello egyikével hello Oracle adatokkal egy e-mailt küldjön e-mailek összekötők, például Office 365 vagy Gmailes. Használjon dinamikus tokeneket hello a hello Oracle tábla toobuild hello `Subject` és `Body` az e-mail:

    ![](./media/connectors-create-api-oracledatabase/oracle-send-email.png)

8. **Mentés** a Logic Apps alkalmazást, és válassza **futtatása**. Hello-Tervező bezárása, és nézze meg hello futtatása előzmények hello állapot. Ha a hiba, válassza ki a hello sikertelen üzenet sor. hello designer megnyílik, és azt mutatja be, amely lépés sikertelen, valamint azt mutatja be hello hibainformáció. Ha ez sikeres, akkor hozzáadott hello információkat e-mailt kell kapnia.


### <a name="workflow-ideas"></a>Munkafolyamat ötletek

* Szeretné, hogy toomonitor hello #oracle hashtaggel történő, és hello Twitter-üzeneteket be egy adatbázist, így kérdezhetők le, és más alkalmazásokban használt. A logikai alkalmazás, adja hozzá a hello `Twitter - When a new tweet is posted` indítható el, és írja be a hello **#oracle** hashtaggel történő. Adja hozzá a hello `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:

    ![](./media/connectors-create-api-oracledatabase/twitter-oracledb.png)

* Üzenetek küldése történik tooa Service Bus-üzenetsorba. Szeretné, hogy ezek az üzenetek tooget, és helyezze őket egy adatbázis. A logikai alkalmazás, adja hozzá a hello `Service Bus - when a message is received in a queue` eseményindító, és jelölje be hello várólista. Adja hozzá a hello `Oracle Database - Insert row` művelet, és válassza ki a táblázatban:

    ![](./media/connectors-create-api-oracledatabase/sbqueue-oracledb.png)

## <a name="common-errors"></a>Gyakori hibák

#### <a name="error-cannot-reach-hello-gateway"></a>**Hiba**: hello átjáró nem érhető el.

**OK**: hello helyszíni az adatátjáró nem tud tooconnect toohello felhő. 

**Megoldás**: Győződjön meg arról, hogy az átjáró fut hello a helyi számítógépen, amelyen telepítették őket, valamint, hogy az informatikai csatlakozhatnak toohello internet.  Azt javasoljuk, hogy nem hello-átjáró telepítése egy számítógépre, amely ki van kapcsolva vagy alvó. Indítsa újra a hello helyszíni átjáró szolgáltatás (PBIEgwService) is.

#### <a name="error-hello-provider-being-used-is-deprecated-systemdataoracleclient-requires-oracle-client-software-version-817-or-greater-please-visit-httpsgomicrosoftcomfwlinkplinkid272376httpsgomicrosoftcomfwlinkplinkid272376-tooinstall-hello-official-provider"></a>**Hiba**: hello használt szolgáltató elavult: "System.Data.OracleClient szükséges verziójú Oracle ügyfélszoftvert 8.1.7-es vagy újabb.". Látogasson el a [https://go.microsoft.com/fwlink/p/?LinkID=272376](https://go.microsoft.com/fwlink/p/?LinkID=272376) tooinstall hello hivatalos szolgáltató.

**OK**: hello Oracle ügyfél SDK nem települ hello számítógépen, ahol hello a helyszíni adatok átjáró fut-e.  

**Megoldási**: Töltse le és telepítse hello Oracle ügyfél SDK hello hello helyszíni adatátjáró ugyanarra a számítógépre.

#### <a name="error-table-tablename-does-not-define-any-key-columns"></a>**Hiba**: "[Tablename]" tábla nem definiál bármely Kulcsoszlopok

**OK**: hello táblának nincs elsődleges kulcs.  

**Megoldási**: hello Oracle-adatbázishoz connector megköveteli, hogy a tábla elsődleges kulcsként megadott oszlop használható.

#### <a name="currently-not-supported"></a>Jelenleg nem támogatott

* Nézetek és tárolt eljárások 
* A tábla az összetett kulcsok
* Táblák egymásba ágyazott objektumtípusok
 
## <a name="connector-specific-details"></a>Összekötő-specifikus részletei

Bármely eseményindítók és hello swagger definiált műveletek megtekintése, és semmilyen határnak hello a Lásd még: [connector részleteket](/connectors/oracle/). 

## <a name="get-some-help"></a>Segítséget

Hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) nagyszerű tooask kérdések helyezze, kérdések megválaszolása, és tekintse meg a többi Logic Apps felhasználók tevékenységeit. 

Növelheti Logic Apps alkalmazások és összekötők szavazás, és a következő ötletek elküldésével [http://aka.ms/logicapps-wish](http://aka.ms/logicapps-wish). 


## <a name="next-steps"></a>Következő lépések
[Logikai alkalmazás létrehozása](../logic-apps/logic-apps-create-a-logic-app.md), és vizsgálja meg hello rendelkezésre álló összekötők Logic Apps, az [API-k lista](apis-list.md).
