---
title: "aaaDiagnose hibák - Azure Logic Apps |} Microsoft Docs"
description: "Ha nem működnek a logic apps közös módon toounderstand"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: a6727ebd-39bd-4298-9e68-2ae98738576e
ms.service: logic-apps
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: LADocs; jehollan
ms.openlocfilehash: 46d318625820034c95e6df3a71ab84c58f076dd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="diagnose-logic-app-failures"></a>Logic app hibák diagnosztizálása
A a logic apps a problémákat vagy hibákat tapasztal, van néhány módszer segítségével jobb megértése érdekében ahol hello hibák érkező.  

## <a name="azure-portal-tools"></a>Az Azure portál eszközök
hello Azure-portált biztosít sok eszközök toodiagnose minden logikai alkalmazás lépésről lépésre.

### <a name="trigger-history"></a>Eseményindító előzmények

Minden logikai alkalmazás legalább egy eseményindító tartozik. Ha azt észleli, hogy alkalmazásokat nem kiváltó, tanulmányozni, hello eseményindító előzmények további információt. Hello eseményindító előzmények hello logika app'ss fő paneljén érheti el.

![Hello eseményindító előzmények keresése][1]

hello eseményindító előzmények összes eseményindító kísérletet a Logic Apps alkalmazást sorolja fel. Kattintson minden eseményindító kísérlet toodrill hello részleteinek, különösen, bármilyen bemenetek vagy kimenettel, amelyek hello eseményindító kísérlet jön létre. Ha nem sikerült az eseményindítók, jelölje ki a hello eseményindító kísérletet, és válassza a hello **kimenetek** tooreview bármely létrehozott hibaüzeneteket, például a FTP hitelesítő adatokat, amelyek nem érvényes hivatkozás.

megjelenhet hello különböző állapotok az alábbiak:

* **Kihagyott**. hello végpont lekérdezéses toocheck adatok, és hogy adatot nem volt elérhető választ kapott.
* **Sikeres**. hello eseményindító, hogy az adatok volt elérhető választ kapott. Ez az állapot manuális eseményindítót, ismétlődési eseményindító vagy egy lekérdezési eseményindító okozhatja. Ez az állapot általában fűzni hello **Fired** állapotát, de előfordulhat, ha rendelkezik olyan feltétel vagy SplitOn parancsot a kód nézetre, amely nem volt megfelelő.
* **Nem sikerült**. Egy hiba történt.

#### <a name="start-a-trigger-manually"></a>Indítsa el manuálisan a eseményindító

Hello logic app toocheck egy rendelkezésre álló eseményindító azonnal Várakozás a következő hello ismétlődés nélkül, kattintson a **válasszon eseményindító** a hello fő panelje tooforce ellenőrzése. Ez a hivatkozás az Dropbox eseményindító például okoz hello munkafolyamat tooimmediately lekérdezési Dropbox új fájlokat.

### <a name="run-history"></a>futtatási előzményei

Minden égetett eseményindító futását eredményezi. Futtatási adatainak hello fő paneljén, alapján megtudhatja, mi történt hello munkafolyamat során számos részletet tartalmazó érheti el.

![Futtatási előzményei hello keresése][2]

Futtatás a következő állapotok hello egyikét jeleníti meg:

* **Sikeres**. Minden művelet sikeresen befejeződött. Hiba történt, ha a hiba látta később hello munkafolyamat történt művelet. Ez azt jelenti, hogy hello hiba látta el egy műveletet, amely be lett állítva toorun sikertelen művelet után.
* **Nem sikerült**. Legalább egy műveletet, amely később a hello munkafolyamat művelet nem kezelt hiba volt.
* **Megszakítva**. hello munkafolyamat működő állapotban volt, de a megszakítási kérelmet kapott.
* **Futó**. hello munkafolyamat jelenleg fut. Ez az állapot akkor fordulhat elő, a szabályozottan halmozott munkafolyamatok vagy aktuális árképzési terv hello miatt. További információkért lásd: [művelet vonatkozó korlátozások árképzést ismertető oldalra hello](https://azure.microsoft.com/pricing/details/app-service/plans/). (A futtatási előzményei hello alatt megjelenő hello diagramok) diagnosztika konfigurálása is is információt nyújt a késleltetési eseményeket.

Ha éppen megtekintett futtatási előzményei, megtekintheti további részletekért.  

#### <a name="trigger-outputs"></a>Eseményindító kimenete

Eseményindító kimenete, amely innen származik: hello eseményindító hello adatok megjelenítése. A kimenetek segítségével meghatározhatja, hogy az összes tulajdonság adott vissza a várt módon.

> [!NOTE]
> Ha megjelenik a telepített tartalmak nem világos, ismerje meg az Azure Logic Apps [kezeli a különböző típusú tartalmakat](../logic-apps/logic-apps-content-type.md).
> 

![Eseményindító kimeneti példák][3]

#### <a name="action-inputs-and-outputs"></a>A művelet bemenetekhez és kimenetekhez

Hello bemenetekhez és kimenetekhez, művelet fogadó részletezhető. Ezek az adatok akkor hasznos, hello méretű és alakú hello kimenetek annak megértéséhez, valamint a hibaüzeneteket, előfordulhat, hogy létrejöttek-kereséshez.

![A művelet bemenetekhez és kimenetekhez][4]

## <a name="debug-workflow-runtime"></a>A munkafolyamat futásidejű hibakeresése

Hello bemenetek, kimenetek és eseményindítók futtató a figyelés, valamint hozzáadhatja az egyes lépéseket tooa munkafolyamat, amelyek segítenek a hibakeresés. 
[RequestBin](http://requestb.in) hatékony eszköz, amely a munkafolyamat lépésben adhat hozzá. RequestBin használatával állíthat be egy HTTP-kérelmek inspector toodetermine hello pontos méretére, alakzat és HTTP-kérelem formátuma. Hozzon létre egy RequestBin, és illessze be a hello URL-cím a logikai alkalmazás HTTP POST műveletet. a szervezet tootest, például kívánt tartalmat, kifejezés vagy egy másik lépés kimeneti. Hello logikai alkalmazás futtatása után is frissítheti a RequestBin toosee hogyan hello kérés formátuma hello Logic Apps motortól generálásakor.

<!-- image references -->
[1]: ./media/logic-apps-diagnosing-failures/triggerhistory.png
[2]: ./media/logic-apps-diagnosing-failures/runhistory.png
[3]: ./media/logic-apps-diagnosing-failures/triggeroutputslink.png
[4]: ./media/logic-apps-diagnosing-failures/actionoutputs.png
