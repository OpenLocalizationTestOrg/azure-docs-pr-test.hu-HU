---
title: "az Operations Management Suite - Azure Logic Apps aaaTrack B2B üzenetek |} Microsoft Docs"
description: "Az integráció fiók és a logikai alkalmazások a műveletek Suite (OMS) az Azure Naplóelemzés B2B kommunikációs nyomon követése"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: f385a72008b19408bb45d61c440df0505b688175
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="track-b2b-communication-in-hello-microsoft-operations-management-suite-oms"></a>A Microsoft Operations Management Suite (OMS) hello B2B kommunikációs nyomon követése

Miután beállította a B2B kommunikációját két üzleti folyamatok vagy a integrációs fiókon keresztül alkalmazásokat futtató entitásokból tudjon cserélni egymással üzeneteket. toocheck hogy ezek az üzenetek feldolgozása helyesen, AS2, X12, nyomon követheti és EDIFACT üzenetek [Azure Naplóelemzés](../log-analytics/log-analytics-overview.md) a hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Például a web-alapú nyomkövetési lehetőségeket biztosítanak is használhatja az üzenetek nyomon követése:

* Üzenet számán és állapota
* Nyugták állapota
* A nyugtázások összefüggésbe üzenetek
* Hibák részletes hiba leírása
* Keresési képességek

## <a name="requirements"></a>Követelmények

* Egy logikai alkalmazást a diagnosztikai naplózás be van állítva. Ismerje meg, [hogyan toocreate logikai alkalmazás](logic-apps-create-a-logic-app.md) és [hogyan tooset be a naplózást az adott logikai alkalmazás](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Integráció fiók be van állítva a figyelés és naplózás. Ismerje meg, [hogyan toocreate integrációs fiók](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) és [hogyan figyelés és naplózás fiók tooset](../logic-apps/logic-apps-monitor-b2b-message.md).

* Ha még nem tette, [diagnosztikai adatok tooLog Analytics közzététele](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) az OMS Szolgáltatáshoz.

> [!NOTE]
> Miután teljesítette hello előző követelmények, hello munkaterületeinek rendelkeznie kell [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Használjon hello azonos OMS-munkaterület nyomon követése a B2B kommunikáció az OMS Szolgáltatáshoz. 
>  
> Ha még nem rendelkezik az OMS-munkaterület, [hogyan toocreate OMS-munkaterület](../log-analytics/log-analytics-get-started.md).

## <a name="add-hello-logic-apps-b2b-solution-toohello-operations-management-suite-oms"></a>Adja hozzá a Logic Apps B2B hello megoldás toohello Operations Management Suite (OMS)

toohave OMS nyomon B2B messages a Logic Apps alkalmazást, hozzá kell adnia a hello **Logic Apps B2B** megoldás toohello OMS-portálon. További információ [megoldások tooOMS hozzáadása](../log-analytics/log-analytics-get-started.md).

1. A hello [Azure-portálon](https://portal.azure.com), válassza a **több szolgáltatások**. Keresse meg a "naplóelemzési", és válassza a **Naplóelemzési** itt látható módon:

   ![A Naplóelemzési keresése](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. A **Naplóelemzési**, található, és válassza ki az OMS-munkaterület. 

   ![Az OMS-munkaterület kiválasztása](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. A **felügyeleti**, válassza a **OMS-portálon**.

   ![Válassza ki az OMS-portálon](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Ha megnyílt az hello OMS kezdőlapját, válassza ki a **megoldások gyűjtemény**.    

   ![Válassza ki a megoldások gyűjteménye](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. A **minden megoldás**, keresése és kiválasztása **Logic Apps B2B**.     

   ![Válassza ki a Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. A **Logic Apps B2B**, válassza a **Hozzáadás**.

   ![Válasszon hozzáadása](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   A hello OMS kezdőlapján hello csempéjére a hozzá tartozó **Logic Apps B2B üzenetek** csomópontként jelenik meg. 
   Ez a csempe frissíti hello üzenetek száma, amikor a B2B üzenetek feldolgozása.

   ![Logic Apps B2B üzenetek csempe OMS kezdőlapján](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-hello-operations-management-suite"></a>Üzenet állapotával és részleteivel kapcsolatban az Operations Management Suite hello nyomon követése

1. Után a B2B üzenetek feldolgozása, megtekintheti a hello állapotával és részleteivel kapcsolatban az azokat az üzeneteket. A hello OMS kezdőlapján válassza ki a hello **Logic Apps B2B üzenetek** csempére.

   ![Frissített üzenetek száma](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Alapértelmezés szerint hello **Logic Apps B2B üzenetek** csempe megjeleníti az adatokat egy nap alapján. toochange hello adatok tooa különböző időköz, hatókör hello hatókör vezérlő hello OMS oldal hello tetején válassza:
   > 
   > ![Adatok hatókörének módosítása](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. Hello Messaging-állapot irányítópult megjelenése után további részleteket az adott üzenettípus, megjelenítheti az adatokat egy nap alapján. Válassza ki a hello csempéjére a hozzá tartozó **AS2**, **X12**, vagy **EDIFACT**.

   ![Üzenet állapotának megtekintése](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   A kiválasztott csempe jelenik meg az üzenetek listáját. 
   További információ az egyes üzenettípushoz hello tulajdonságok toolearn üzenet tulajdonság leírások lásd:

   * [AS2-üzenet tulajdonságai](#as2-message-properties)
   * [X12 üzenet tulajdonságai](#x12-message-properties)
   * [EDIFACT üzenet tulajdonságai](#EDIFACT-message-properties)

   Például ez hogyan nézhet ki egy AS2 állapotüzenet-listában:

   ![AS2-üzenet megtekintése](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. tooview vagy exportálási hello bemenetekhez és kimenetekhez adott üzenetek, válassza ki azokat az üzeneteket, és válassza a **letöltése**. Amikor a rendszer kéri, mentés hello .zip fájl tooyour helyi számítógépen, és bontsa ki a fájlt. 

   hello kibontott mappát egy mappát az összes kijelölt üzenet tartalmazza. 
   Ha beállította a nyugtázásokhoz, hello üzenet mappa is fájlok nyugtázási adatokkal. 
   Minden üzenet mappa van legalább ezeket a fájlokat: 
   
   * Hello emberek számára olvasható fájlok bemeneti forgalma és a kimeneti hasznos adatait
   * Hello bemenetekhez és kimenetekhez kódolt fájlok

   Az egyes üzenet hello mappa és fájl formátumok itt található:

   * [AS2 mappához és fájlhoz formátumok](#as2-folder-file-names)
   * [X12 mappa és fájl neve formátumok](#x12-folder-file-names)
   * [EDIFACT mappához és fájlhoz formátumok](#edifact-folder-file-names)

   ![Fájlok letöltése](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. tooview hello azonos lévő műveleteket futtatásához Azonosítóját, hello a **naplófájl-keresési** lapon, válassza ki az üzenet hello üzenet listából.

   Ezek a Műveletek oszlop, vagy keresse meg az adott eredmények rendezheti.

   ![Hello műveletek azonos futtatás azonosítója](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * előre elkészített lekérdezések toosearch eredmények kiválasztása **Kedvencek**.

   * Ismerje meg, [hogyan toobuild lekérdezi szűrők hozzáadásával](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   További információ vagy [hogyan napló toofind adatok keres a Naplóelemzési](../log-analytics/log-analytics-log-searches.md).

   * toochange lekérdezés hello keresési mezőbe, a frissítés hello lekérdezés hello oszlopok és értékek, amelyet az toouse szűrőként.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Tulajdonság-leírások és formátumok az AS2, X 12 és EDIFACT-üzenetek

Az egyes üzenet az alábbiakban hello tulajdonságleírások és a letöltött fájlok formátumok.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2-üzenet tulajdonságleírások

Az alábbiakban hello tulajdonságleírások minden egyes AS2-üzenet esetében.

| Tulajdonság | Leírás |
| --- | --- |
| Feladó | hello Vendég partner megadott **fogadási beállítások**, vagy a megadott hello fogadó partner **küldési beállítások** AS2 megállapodás |
| Fogadó | hello fogadó partner megadott **fogadási beállítások**, vagy a megadott hello Vendég partner **küldési beállítások** AS2 megállapodás |
| Logikai alkalmazás | hello logikai alkalmazás hol hello AS2 műveletek beállítása |
| status | hello AS2 Messaging-állapot <br>Sikeres = fogadott, vagy egy érvényes AS2 üzenetet küldött. Nincs MDN be van állítva. <br>Sikeres = fogadott, vagy egy érvényes AS2 üzenetet küldött. MDN beállítása és kapott, vagy MDN zajlik. <br>Nem sikerült = érvénytelen AS2 üzenetet kapott. Nincs MDN be van állítva. <br>Függőben lévő = fogadott, vagy egy érvényes AS2 üzenetet küldött. MDN be van állítva, és MDN várt. |
| Nyugtázási | hello MDN Messaging-állapot <br>Elfogadható = fogadott vagy elküldött egy pozitív MDN. <br>Függőben lévő = tooreceive várakozik, vagy egy MDN küldése. <br>Elutasított = fogadott vagy elküldött egy negatív MDN. <br>Nem szükséges = MDN nincs beállítva a hello szerződést. |
| Irány | AS2-üzenet irányát hello |
| Korrelációs azonosító | minden hello eseményindítók és műveletek a logikai alkalmazás hibához hello azonosítója |
| Üzenet azonosítója | az hello AS2 üzenetfejlécek hello AS2-üzenet azonosítója |
| időbélyeg | hello AS2 művelet feldolgozásának üdvözlőüzenetére hello időpontja |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>A letöltött fájlok AS2 formátumok

Az alábbiakban hello formátumok minden letöltött AS2 üzenet mappában és fájlokat.

| Fájl vagy mappa | Nevének formátuma |
| :------------- | :---------- |
| Üzenet mappa | [küldő] \_[fogadó]\_AS2\_[korrelációs azonosító]\_[üzenet-azonosítója]\_[időbélyeg] |
| Bemeneti, kimeneti és if beállítása nyugtázási fájlok | **Bemeneti forgalma**: [küldő]\_[fogadó]\_AS2\_[korrelációs azonosító]\_input_payload.txt </p>**Kimeneti hasznos**: [küldő]\_[fogadó]\_AS2\_[korrelációs azonosító]\_kimeneti\_payload.txt </p></p>**Bemenetek**: [küldő]\_[fogadó]\_AS2\_[korrelációs azonosító]\_inputs.txt </p></p>**Kimenetek**: [küldő]\_[fogadó]\_AS2\_[korrelációs azonosító]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 tulajdonságleírások üzenet

Az alábbiakban hello tulajdonság leírásainak minden X12 üzenet.

| Tulajdonság | Leírás |
| --- | --- |
| Feladó | hello Vendég partner megadott **fogadási beállítások**, vagy a megadott hello fogadó partner **küldési beállítások** egy X12 a megállapodás |
| Fogadó | hello fogadó partner megadott **fogadási beállítások**, vagy a megadott hello Vendég partner **küldési beállítások** egy X12 a megállapodás |
| Logikai alkalmazás | hello logikai alkalmazás hol hello X12 műveletek beállítása |
| status | hello X12 Messaging-állapot <br>Sikeres = fogadott, vagy egy érvényes X12 küldött üzenetet. Egyetlen működési nyugtázási be van állítva. <br>Sikeres = fogadott, vagy egy érvényes X12 küldött üzenetet. Működési nyugtázási beállítása és kapott, illetve funkcionális nyugtázási elküldésekor történik. <br>Nem sikerült = fogadott vagy elküldött egy érvénytelen X12 üzenet. <br>Függőben lévő = fogadott, vagy egy érvényes X12 küldött üzenetet. Funkcionális nyugtázási be van állítva, és működési nyugtázási várt. |
| Nyugtázási | Funkcionális nyugtázási (997) állapota <br>Elfogadható = fogadott, vagy egy pozitív működési nyugtát küldött <br>Elutasított = fogadott, vagy egy negatív működési nyugtát küldött <br>Függőben lévő = működési nyugtázási várt, de nem érkezett meg. <br>Függőben lévő = előállított működési nyugtázási, de nem lehet elküldeni a toopartner. <br>Nem szükséges funkcionális = nyugtázási nincs beállítva. |
| Irány | hello X12 üzenet iránya |
| Korrelációs azonosító | minden hello eseményindítók és műveletek a logikai alkalmazás hibához hello azonosítója |
| Üzenet típusa | hello EDI X 12 üzenet típusa |
| ICN | hello Interchange ellenőrző szám hello X12 üzenet |
| TSCN | hello beállítása vezérlő tranzakciószám hello X12 üzenet |
| időbélyeg | hello X12 művelet feldolgozásának üdvözlőüzenetére hello időpontja |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 formátumok nevet letöltött Üzenetfájl

Az alábbiakban a hello formátumok egyes X12 letölteni az üzenetek mappa és a fájlokat.

| Fájl vagy mappa | Nevének formátuma |
| :------------- | :---------- |
| Üzenet mappa | [küldő] \_[fogadó]\_X12\_[adatcsere-ellenőrző-szám]\_[globális-ellenőrző-szám]\_[tranzakció-set-ellenőrző-szám]\_[időbélyeg] |
| Bemeneti, kimeneti és if beállítása nyugtázási fájlok | **Bemeneti forgalma**: [küldő]\_[fogadó]\_X12\_[adatcsere-ellenőrző-szám]\_input_payload.txt </p>**Kimeneti hasznos**: [küldő]\_[fogadó]\_X12\_[adatcsere-ellenőrző-szám]\_kimeneti\_payload.txt </p></p>**Bemenetek**: [küldő]\_[fogadó]\_X12\_[adatcsere-ellenőrző-szám]\_inputs.txt </p></p>**Kimenetek**: [küldő]\_[fogadó]\_X12\_[adatcsere-ellenőrző-szám]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT üzenet tulajdonságleírások

Az alábbiakban hello tulajdonságleírások minden egyes EDIFACT-üzenet esetében.

| Tulajdonság | Leírás |
| --- | --- |
| Feladó | hello Vendég partner megadott **fogadási beállítások**, vagy a megadott hello fogadó partner **küldési beállítások** EDIFACT megállapodás |
| Fogadó | hello fogadó partner megadott **fogadási beállítások**, vagy a megadott hello Vendég partner **küldési beállítások** EDIFACT megállapodás |
| Logikai alkalmazás | hello logikai alkalmazás hol hello EDIFACT műveletek beállítása |
| status | hello EDIFACT Messaging-állapot <br>Sikeres = fogadott, vagy egy érvényes EDIFACT üzenetet küldött. Egyetlen működési nyugtázási be van állítva. <br>Sikeres = fogadott, vagy egy érvényes EDIFACT üzenetet küldött. Működési nyugtázási beállítása és kapott, illetve funkcionális nyugtázási elküldésekor történik. <br>Nem sikerült fogadott = vagy EDIFACT érvénytelen üzenetet küldött <br>Függőben lévő = fogadott, vagy egy érvényes EDIFACT üzenetet küldött. Funkcionális nyugtázási be van állítva, és működési nyugtázási várt. |
| Nyugtázási | Funkcionális nyugtázási (997) állapota <br>Elfogadható = fogadott, vagy egy pozitív működési nyugtát küldött <br>Elutasított = fogadott, vagy egy negatív működési nyugtát küldött <br>Függőben lévő = működési nyugtázási várt, de nem érkezett meg. <br>Függőben lévő = előállított működési nyugtázási, de nem lehet elküldeni a toopartner. <br>Nem szükséges = működési nyugtázási nincs beállítva. |
| Irány | hello EDIFACT üzenet iránya |
| Korrelációs azonosító | minden hello eseményindítók és műveletek a logikai alkalmazás hibához hello azonosítója |
| Üzenet típusa | hello EDIFACT-üzenet típusa |
| ICN | hello Interchange ellenőrző szám hello EDIFACT üzenet |
| TSCN | hello beállítása vezérlő tranzakciószám hello EDIFACT üzenet |
| időbélyeg | hello EDIFACT művelet feldolgozásának üdvözlőüzenetére hello időpontja |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>A letöltött fájlok EDIFACT formátumok

Az alábbiakban hello formátumok minden letöltött EDIFACT üzenet mappában és fájlokat.

| Fájl vagy mappa | Nevének formátuma |
| :------------- | :---------- |
| Üzenet mappa | [küldő] \_[fogadó]\_EDIFACT\_[adatcsere-ellenőrző-szám]\_[globális-ellenőrző-szám]\_[tranzakció-set-ellenőrző-szám]\_[időbélyeg] |
| Bemeneti, kimeneti és if beállítása nyugtázási fájlok | **Bemeneti forgalma**: [küldő]\_[fogadó]\_EDIFACT\_[adatcsere-ellenőrző-szám]\_input_payload.txt </p>**Kimeneti hasznos**: [küldő]\_[fogadó]\_EDIFACT\_[adatcsere-ellenőrző-szám]\_kimeneti\_payload.txt </p></p>**Bemenetek**: [küldő]\_[fogadó]\_EDIFACT\_[adatcsere-ellenőrző-szám]\_inputs.txt </p></p>**Kimenetek**: [küldő]\_[fogadó]\_EDIFACT\_[adatcsere-ellenőrző-szám]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Következő lépések

* [Az Operations Management Suite B2B üzenetek lekérdezés](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [AS2-követési sémák](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-követési sémák](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Egyéni követési sémák](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)