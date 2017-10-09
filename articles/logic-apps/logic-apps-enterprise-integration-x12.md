---
title: "aaaX12 üzenetek B2B vállalati integrációs - Azure Logic Apps |} Microsoft Docs"
description: "Exchange X12 üzenetek B2B vállalati integráció az Azure Logic Apps EDI-formátumban"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 20a077b299875a16ada66a500d5f1c8f9972d309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-x12-messages-for-enterprise-integration-with-logic-apps"></a>A logic apps vállalati integrációs Exchange X12 állapotüzenete

Előtt X12 tudjon cserélni az Azure Logic Apps üzenetek, létre kell hoznia egy X12 szerződés és a megállapodás tárolása integrációs fiókját. Hello lépéseit az alábbiakban egy X12 toocreate szerződést.

> [!NOTE]
> Ezen a lapon hello X12 szolgáltatásaira vonatkozik, amelyek az Azure Logic Apps. További információkért lásd: [EDIFACT](logic-apps-enterprise-integration-edifact.md).

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md) , amely már definiált és az Azure-előfizetéshez társított
* Legalább két [partnerek](../logic-apps/logic-apps-enterprise-integration-partners.md) , amely az integráció fiókban definiált és hello X12 azonosítója alapján konfigurált **üzleti identitások**    
* Egy szükséges [séma](../logic-apps/logic-apps-enterprise-integration-schemas.md) tooyour feltöltése [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md)

Miután [integrációs-fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-accounts.md), [adja hozzá a partnerek](logic-apps-enterprise-integration-partners.md), és egy [séma](../logic-apps/logic-apps-enterprise-integration-schemas.md) adott meg toouse, létrehozhat egy X12 szerződés által a következő lépéseket.

## <a name="create-an-x12-agreement"></a>Hozzon létre egy X12 megállapodás

1.  Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com "Azure-portálon"). Hello bal oldali menüben válassza ki a **további szolgáltatások**. 

    > [!TIP]
    > Ha nem lát **további szolgáltatások**, lehetséges, hogy tooexpand hello menü először. Hello összecsukott hello menü felső részén válassza **megjelenítése menü**.

    ![A bal oldali menüben válassza a "Szolgáltatás"](./media/logic-apps-enterprise-integration-x12/account-1.png)

2.  Hello keresési mezőbe írja be a "integrációt" szűrőként. Hello eredmények listában válassza ki a **integrációs fiókok**.  

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-x12/account-2.png)

3. A hello **integrációs fiókok** panelt megnyitó, jelölje be hello integrációs fiókra, amelyhez tooadd hello szerződést.
Ha nem lát minden integrációs fiókok [hozzon létre egyet első](../logic-apps/logic-apps-enterprise-integration-accounts.md "integrációs fiókokkal kapcsolatos összes").

    ![Ha toocreate hello megállapodás integrációs fiók kiválasztása](./media/logic-apps-enterprise-integration-x12/account-3.png)

4. Válassza ki **áttekintése**, majd jelölje be hello **megállapodások** csempére. Ha egy megállapodások csempe nem rendelkezik, először adjon hozzá hello csempe. 

    ![Válassza a "Szerződés" csempe](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. Hello megállapodások paneljén megjelenő válassza **Hozzáadás**.

    ![Válassza a "Hozzáadás"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)     

6. A **Hozzáadás**, adjon meg egy **neve** a szerződés. Hello típusát, válassza a **X12**. Jelölje be hello **fogadó Partner**, **gazdagép identitását**, **Vendég Partner**, és **Vendég identitás** a szerződés. Tulajdonság kapcsolatos további információkért lásd: hello tábla ebben a lépésben.

    ![Adja meg a szerződés részletei](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

    | Tulajdonság | Leírás |
    | --- | --- |
    | Név |Hello szerződés neve |
    | A szerződés típusa | X12 kell lennie. |
    | Fogadó Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello fogadó partner által konfigurált házirendsablonok hello megállapodás hello szervezet jelöli. |
    | Gazdagép identitását |Hello fogadó partner azonosítója |
    | Vendég Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello Vendég partner hello szervezet, amely hello állomás partnerrel üzleti jelöli. |
    | Vendég identitás |Hello Vendég partner azonosítója |
    | Beállítások |Ezeket a tulajdonságokat a szerződés által fogadott üzenetek küldési tooall alkalmazni. |
    | Beállítások küldése |Ezek a Tulajdonságok tooall üzeneteket a szerződés vonatkozik. |  

  > [!NOTE]
  > Szerződés függ hello küldő minősítő és azonosítóját, és hello fogadó minősítő és hello partner és a bejövő üzenet azonosítója X12 feloldását. Ha ezek az értékek módosítása esetén a partner, frissítse a hello megállapodás túl.

## <a name="configure-how-your-agreement-handles-received-messages"></a>Hogyan a szerződés leírók fogadott üzenetek konfigurálása

Most, hogy hello megállapodás tulajdonságok beállítása, beállíthatja a módját a jelen szerződés azonosítja, és kezeli a bejövő üzenetek és a szerződés partnerétől kapott.

1.  A **Hozzáadás**, jelölje be **fogadási beállítások**.
Állítsa be ezeket a tulajdonságokat a szerződés hello partnerrel, amely az üzenetek Önnel alapján. Tulajdonságleírások tekintse meg e szakasz táblázatai hello.

    **Beállítások** van felosztva ezekben a szakaszokban: azonosítók, visszaigazolás, sémákat, borítékok, vezérlő számok, érvényesítést és belső beállítások.

2. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

Mostantól a szerződés készen toohandle bejövő üzenetek tooyour tartó kiválasztott beállításokat.

### <a name="identifiers"></a>Azonosítók

![Azonosító tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Tulajdonság | Leírás |
| --- | --- |
| ISA1 (engedélyezési minősítő) |Válassza ki a hello engedélyezési minősítőt érték hello legördülő listából. |
| ISA2 |Választható. Adjon meg engedélyezési adatokat értéket. Ha ISA1 megadott hello értéke 00 eltérő, adjon meg legalább egy alfanumerikus karaktert, és legfeljebb 10. |
| ISA3 (biztonsági minősítő) |Válassza ki a hello biztonsági minősítőt érték hello legördülő listából. |
| ISA4 |Választható. Adja meg a hello biztonsági információinak értékét. Ha ISA3 megadott hello értéke 00 eltérő, adjon meg legalább egy alfanumerikus karaktert, és legfeljebb 10. |

### <a name="acknowledgment"></a>Tudomásul vétele

![Nyugtázási tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Várt TA1 |Egy technikai visszaigazolás toohello interchange küldő adja vissza |
| Várt be |A működési visszaigazolás toohello interchange küldő adja vissza. Ezután válassza ki hello 997 vagy 999 nyugták kíván-e hello séma verziója alapján |
| AK2/IK2 hurkot tartalmaz |Engedélyezi az AK2 hurkok a működési nyugták elfogadott tranzakció beállítása |

### <a name="schemas"></a>Sémák

Válassza ki a sémát minden tranzakció típusa (ST1) és a küldő alkalmazás (GS2). hello fogadási folyamat visszafejti hello bejövő üzenet által azonos hello érték ST1, és a bejövő üzenet hello hello GS2 értékekkel, az itt hello séma hello hello bejövő üzenet sémája, és itt beállított.

![Válassza ki a sémát](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Verzió |Válassza ki a hello X12 verziója |
| Tranzakció típusa (ST01) |Hello tranzakció típusának kiválasztása |
| Küldő alkalmazás (GS02) |Hello küldő alkalmazás kiválasztása |
| Séma |Válassza ki a kívánt toouse hello sémafájl. Sémák tooyour integrációs fiók hozzáadásakor. |

> [!NOTE]
> Konfigurálja a szükséges hello [séma](../logic-apps/logic-apps-enterprise-integration-schemas.md) , amely feltöltött tooyour [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Borítékok

![Adja meg a hello elválasztó egy tranzakció készlet: válassza a szabványos azonosító vagy ismétlődési elválasztó](./media/logic-apps-enterprise-integration-x12/x12-34.png)

| Tulajdonság | Leírás |
| --- | --- |
| ISA11 kihasználtsága |Határozza meg a hello elválasztó toouse tranzakció meg: <p>Válassza ki **szabványos azonosító** egy pontot (.) a decimális jelöléssel, nem pedig hello decimális jelöléssel hello bejövő hello EDI dokumentum toouse fogadási folyamat. <p>Válassza ki **ismétlődési elválasztó** toospecify hello elválasztó az egyszerű adatelem vagy ismétlődő adatszerkezet ismételt előfordulását. Például általában hello beszúrási (^) használatos hello ismétlődési elválasztójelként. A HIPAA lehetséges hello beszúrási csak használhat. |

### <a name="control-numbers"></a>Vezérlő számok

![Válassza ki, hogyan szabályozza a toohandle a szám ismétlődések](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Ellenőrző szám Interchange ismétlődések letiltása |Ismétlődő kereszteződéseket blokkolása. Hello interchange ellenőrző szám (ISA13) kapott hello interchange ellenőrző szám ellenőrzi. Ha találat esetén elvégez, hello kap adatcsatorna nem feldolgozni hello interchange. Hello napok számát, amelyek a hello keresése adjon értéket is megadhat *keressen ismétlődő ISA13 (naponta)*. |
| Csoport-ellenőrzőszám ismétlődésének letiltása |Blokk felcserélődések duplikált csoportokhoz vezérlő számokkal. |
| Tranzakciókészlet-ellenőrzőszám ismétlődésének letiltása |Blokk felcserélődések ismétlődő tranzakció set vezérlő számokkal. |

### <a name="validations"></a>Ellenőrzések

![A fogadott üzenetek érvényesítési tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

Érvényesítési sorokban befejeződése után, egy másik automatikusan kerül. Ha nem adja meg az összes szabály, érvényesítési hello "Alapértelmezett" sor használja.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet típusa |Válassza ki a hello EDI-üzenet típusa. |
| EDI érvényesítése |EDI-érvényesítést hajt végre hello séma EDI tulajdonságok, hosszára, üres adatelemek és záró elválasztók által definiált adattípusok. |
| Kiterjesztett érvényesítése |Ha hello adattípus nem EDI, érvényesítési hello adatok elem követelmény és engedélyezett ismétlési, enumerálások és adatok elem hossza érvényesítési (min vagy max). |
| Kezdő/záró nullából engedélyezése |Minden további, kezdő vagy záró nulla, és az szóköz karakter. Ne távolítsa el ezeket a karaktereket. |
| Trim kezdő/záró nullából |Távolítsa el a kezdő vagy záró nulla és karaktereket. |
| Záró elválasztó házirend |Záró elválasztók készítése. <p>Válassza ki **nem engedélyezett** tooprohibit záró határoló és elválasztók, hello kapott adatcserét. Ha hello interchange záró határoló és elválasztókat, hello interchange van deklarálva nem érvényes. <p>Válassza ki **nem kötelező** tooaccept felcserélődések vagy záró határoló és elválasztók nélkül. <p>Válassza ki **kötelező** amikor hello interchange rendelkeznie kell záró határoló és elválasztókat. |

### <a name="internal-settings"></a>Belső beállítások

![Belső beállítások](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Implicit decimális formátum "Nn" tooa alap 10 numerikus érték konvertálása |Hello formátumú "Nn" be 10-es számértéket megadott EDI számot alakít át. |
| Üres XML-címkék létrehozása, ha a záró elválasztók engedélyezve vannak |Válassza ki az jelölőnégyzetet toohave hello interchange küldő tartalmazza XML-címkék elválasztók záró üres. |
| Adatcsere felosztása tranzakciókészletekre – tranzakciókészletek felfüggesztése hiba esetén|Elemzi az egyes tranzakciókra, állítsa be egy külön XML-dokumentum cseréje a hello megfelelő boríték toohello tranzakció készlet alkalmazásával. Sikertelen érvényesítés hello felfüggeszti csak hello tranzakciókat. |
| Adatcsere felosztása tranzakciókészletekre – adatcsere felfüggesztése hiba esetén|Elemzi az egyes tranzakciókra, állítsa be egy külön XML-dokumentum cseréje a hello megfelelő boríték alkalmazásával. Ha egy vagy több tranzakció készletet hello interchange érvényesítése sikertelen, mert a teljes adatcsere felfüggeszti. | 
| Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba |Hello interchange érintetlenül hagyja, létrehoz egy XML-dokumentum hello teljes kötegelt adatcserét. Csak hello tranzakció készleteket, miközben továbbra tooprocess érvényesítése sikertelen, mert felfüggeszti az összes többi tranzakció beállítása. |
| Adatcsere megőrzése – adatcsere felfüggesztése hiba esetén |Hello interchange érintetlenül hagyja, létrehoz egy XML-dokumentum hello teljes kötegelt adatcserét. Hello teljes adatcsere felfüggesztése, ha egy vagy több tranzakció készletet hello interchange érvényesítése sikertelen. |

## <a name="configure-how-your-agreement-sends-messages"></a>Hogyan a szerződés küld üzeneteket konfigurálása

Beállíthatja, hogyan a jelen szerződés azonosítja, és a kimenő küldendő üzenetek, és ez a szerződés tooyour partner kezeli.

1.  A **Hozzáadás**, jelölje be **küldési beállítások**.
Állítsa be ezeket a tulajdonságokat a partnerrel, aki adatcseréihez használható üzenetek Önnel a szerződés alapján. Tulajdonságleírások tekintse meg e szakasz táblázatai hello.

    **Küldési beállításainak** van felosztva ezekben a szakaszokban: azonosítók, visszaigazolás, sémákat, borítékok, karakterkészletek és elválasztók, vezérlő számok és érvényesítése.

2. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

A szerződés most már készen áll a kimenő üzenetek tooyour kiválasztott beállítások tartó toohandle áll.

### <a name="identifiers"></a>Azonosítók

![Azonosító tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Tulajdonság | Leírás |
| --- | --- |
| Engedélyezési minősítő (ISA1) |Válassza ki a hello engedélyezési minősítőt érték hello legördülő listából. |
| ISA2 |Adjon meg engedélyezési adatokat értéket. Ha ez az érték nem a 00, majd adja meg, legalább egy alfanumerikus karaktert, és legfeljebb 10. |
| Biztonsági minősítő (ISA3) |Válassza ki a hello biztonsági minősítőt érték hello legördülő listából. |
| ISA4 |Adja meg a hello biztonsági információinak értékét. Ha ez az érték nem a 00 hello érték (ISA4) mezőben, majd adja meg, legalább egy alfanumerikus érték, és legfeljebb 10. |

### <a name="acknowledgment"></a>Tudomásul vétele

![Nyugtázási tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Tulajdonság | Leírás |
| --- | --- |
| Várt TA1 |Egy technikai visszaigazolás (TA1) toohello interchange küldő visszaadása. A beállítással megadható, hogy hello fogadó partner, aki küldi hello üzenet kérelmek nyugtázás hello Vendég partnerről hello feltételeit. A nyugtázások hello-fogadási hello megállapodás beállításai alapján a hello állomás partner által várt. |
| Várt be |A működési visszaigazolás (be) toohello interchange küldő visszaadása. Válassza ki, hogy hello 997 vagy 999 nyugták dolgozunk hello séma verziója alapján. A nyugtázások hello-fogadási hello megállapodás beállításai alapján a hello állomás partner által várt. |
| BE verziója |Válassza ki a hello be verziója |

### <a name="schemas"></a>Sémák

![Válassza ki a séma toouse](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Tulajdonság | Leírás |
| --- | --- |
| Verzió |Válassza ki a hello X12 verziója |
| Tranzakció típusa (ST01) |Hello tranzakció típusának kiválasztása |
| SÉMA |Válassza ki a hello séma toouse. Sémák integrációs fiókjában található. Ha először válassza ki a sémát, a rendszer automatikusan beállítja verziója és a tranzakció típusa  |

> [!NOTE]
> Konfigurálja a szükséges hello [séma](../logic-apps/logic-apps-enterprise-integration-schemas.md) , amely feltöltött tooyour [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md).

### <a name="envelopes"></a>Borítékok

![Adja meg a hello elválasztó egy tranzakció készlet: válassza a szabványos azonosító vagy ismétlődési elválasztó](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Tulajdonság | Leírás |
| --- | --- |
| ISA11 kihasználtsága |Határozza meg a hello elválasztó toouse tranzakció meg: <p>Válassza ki **szabványos azonosító** egy pontot (.) a decimális jelöléssel, nem pedig hello decimális jelöléssel hello bejövő hello EDI dokumentum toouse fogadási folyamat. <p>Válassza ki **ismétlődési elválasztó** toospecify hello elválasztó az egyszerű adatelem vagy ismétlődő adatszerkezet ismételt előfordulását. Például általában hello beszúrási (^) használatos hello ismétlődési elválasztójelként. A HIPAA lehetséges hello beszúrási csak használhat. |

### <a name="control-numbers"></a>Vezérlő számok

![Ellenőrző szám tulajdonságainak megadása](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Vezérlő verziószáma (ISA12) |Válassza ki a szabványos X12 hello hello verziója |
| Használati mutató (ISA15) |Válassza ki a cseréje hello környezetében.  hello értékeket információk, termelési adatok, és vizsgálati adatok |
| Séma |Hello GS, és az, hogy elküldi toohello küldése adatcsatorna X12 kódolású adatcserét ST szegmensek |
| GS1 |Nem kötelező, ki kell jelölnie egy működő kódokat hello hello legördülő listából értéke |
| GS2 |Nem kötelező, alkalmazás-feladó |
| GS3 |Nem kötelező, alkalmazás fogadó |
| GS4 |Nem kötelező, jelölje be a CCYYMMDD, vagy a ÉÉHHNN |
| GS5 |Nem kötelező, jelölje be HHMM, ÓÓPPMM vagy HHMMSSdd |
| GS7 |Nem kötelező, válassza ki a megfelelő érték hello felelős ügynökség hello legördülő listából |
| GS8 |Hello dokumentum nem kötelező, verziója |
| Interchange ellenőrző szám (ISA13) |Hello interchange ellenőrző szám értéktartománya szükséges, adja meg. Adjon meg egy legalább 1 és legfeljebb 999999999 numerikus értéket |
| Csoport ellenőrző szám (GS06) |Számok számos hello vezérlő csoportazonosító szükséges, adja meg. Adjon meg egy legalább 1 és legfeljebb 999999999 numerikus értéket |
| Tranzakció Set ellenőrző szám (ST02) |Hello tranzakció beállítása ellenőrző szám számok számos szükséges, adja meg. Adjon meg egy numerikus értékek tartományán legalább 1 és legfeljebb 999999999 |
| Körzetszám |Nem kötelező, tranzakció set vezérlő számok visszaigazolás használt hello számos jelöltek ki. Adjon meg számértéket hello középső két mezőbe, és egy alfanumerikus érték (ha szükséges) hello előtag és utótag mezőkben. hello középső mező kötelező, és hello minimális és maximális értékei hello ellenőrző szám tartalmaz |
| Utótag |Nem kötelező, tranzakció set vezérlő számok nyugtázás használt hello számos jelöltek ki. Adjon meg egy numerikus hello középső két mezők és alfanumerikus értékkel (ha szükséges) hello előtag és utótag mezőkben. hello középső mező kötelező, és hello minimális és maximális értékei hello ellenőrző szám tartalmaz |

### <a name="character-sets-and-separators"></a>Karakterkészletek és elválasztók

Eltérő hello karakterkészlet, az egyes üzenet egy másik halmaz adhatja meg. Ha egy karakterkészletet adott üzenethez a séma nincs megadva, alapértelmezett karakterkészlet hello szolgál.

![Adja meg a üzenettípusok elválasztó karaktert](./media/logic-apps-enterprise-integration-x12/x12-9.png) 

| Tulajdonság | Leírás |
| --- | --- |
| Set toobe használt karakter |toovalidate hello tulajdonságok, jelölje be hello X12 karakterkészlet. Alapszintű, a kibővített és az UTF8 hello közül. |
| Séma |Válassza ki a séma hello legördülő listából. Minden egyes sorára befejezése után a rendszer automatikusan hozzáadja az új sort. Hello kijelölt séma, a select hello elválasztók beállítása, amelyet az toouse elválasztó leírása a következő hello alapján. |
| A bemeneti típus |Válasszon egy bemeneti típus hello legördülő listából. |
| Az összetevő elválasztó |tooseparate összetett adatelemek, adjon meg egy egyetlen karaktert. |
| Adatok elem elválasztó |tooseparate egyszerű adatelemek belül összetett adatelemek, adjon meg egy egyetlen karaktert. |
| Helyettesítő karakter |Adjon meg egy hello adattartalom minden elválasztó karaktere helyettesítésére kimenő X12 üdvözlőüzenetére létrehozásakor használt helyettesítő karakter. |
| Szegmens lezáró |egy EDI szegmens tooindicate hello végét adja meg egy egyetlen karaktert. |
| Utótag |Válassza ki a használt hello szegmensazonosító hello karakter. Ha egy utótagot kijelölni, majd hello szegmens lezáró adatok elem lehet üres. Ha hello szegmens lezáró üres, majd ki kell jelölnie egy utótagot. |

> [!TIP]
> tooprovide speciális karakter értékek, szerkessze a JSON-ként hello megállapodás, és adjon meg hello ASCII érték hello speciális karakter.

### <a name="validation"></a>Ellenőrzés

![Az üzenetküldésre érvényesítési tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

Érvényesítési sorokban befejeződése után, egy másik automatikusan kerül. Ha nem adja meg az összes szabály, érvényesítési hello "Alapértelmezett" sor használja.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet típusa |Válassza ki a hello EDI-üzenet típusa. |
| EDI érvényesítése |EDI-érvényesítést hajt végre hello séma EDI tulajdonságok, hosszára, üres adatelemek és záró elválasztók által definiált adattípusok. |
| Kiterjesztett érvényesítése |Ha hello adattípus nem EDI, érvényesítési hello adatok elem követelmény és engedélyezett ismétlési, enumerálások és adatok elem hossza érvényesítési (min vagy max). |
| Kezdő/záró nullából engedélyezése |Minden további, kezdő vagy záró nulla, és az szóköz karakter. Ne távolítsa el ezeket a karaktereket. |
| Trim kezdő/záró nullából |Távolítsa el a kezdő vagy záró nulla karakterből. |
| Záró elválasztó házirend |Záró elválasztók készítése. <p>Válassza ki **nem engedélyezett** tooprohibit záró határoló és elválasztók, hello küldött interchange. Ha hello interchange záró határoló és elválasztókat, hello interchange van deklarálva nem érvényes. <p>Válassza ki **nem kötelező** toosend felcserélődések vagy záró határoló és elválasztók nélkül. <p>Válassza ki **kötelező** Ha küldi hello interchange záró határoló és elválasztókat rendelkeznie kell. |

## <a name="find-your-created-agreement"></a>A létrehozott megállapodás keresése

1.  Hello a szerződés tulajdonságok beállítása után **Hozzáadás** paneljén válassza **OK** toofinish létrehozása a szerződés és a visszatérési tooyour integrációs fiók panelen.

    Az újonnan hozzáadott foglalt most megjelenik a **megállapodások** listája.

2.  Az integrációs fiókok áttekintése is megtekintheti a szerződéseket. Az integráció-fiók panelen válassza **áttekintése**, majd jelölje be hello **megállapodások** csempére.

    ![Válassza ki a "Szerződés" csempe tooview minden szerződés](./media/logic-apps-enterprise-integration-x12/x12-1-5.png)   

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/x12/). 

## <a name="learn-more"></a>Részletek
* [További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  

