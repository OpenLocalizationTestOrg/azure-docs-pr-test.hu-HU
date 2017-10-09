---
title: "aaaEDIFACT üzenetek B2B vállalati integrációs - Azure Logic Apps |} Microsoft Docs"
description: "Exchange EDIFACT üzenetek B2B vállalati integráció az Azure Logic Apps EDI-formátumban"
services: logic-apps
documentationcenter: .net,nodejs,java
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 2257d2c8-1929-4390-b22c-f96ca8b291bc
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/26/2016
ms.author: LADocs; jonfan
ms.openlocfilehash: 496fadcda58de2d36459202b839b0a63c9e5857c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-edifact-messages-for-enterprise-integration-with-logic-apps"></a>A logic apps vállalati integrációs Exchange EDIFACT-üzenetek

EDIFACT üzenetváltásra az Azure Logic Apps, mielőtt EDIFACT-egyezmény létrehozása, és tárolja, hogy a szerződés integrációs fiókját. Az alábbiakban hello lépései toocreate EDIFACT szerződést.

> [!NOTE]
> Ezen a lapon hello EDIFACT szolgáltatásaira vonatkozik, amelyek az Azure Logic Apps. További információkért lásd: [X12](logic-apps-enterprise-integration-x12.md).

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md) , amely már definiált és az Azure-előfizetéshez társított  
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiálva vannak az integráció-fiókban

> [!NOTE]
> Ha létrehoz egy szerződést, hello tartalom hello üzeneteket fogadni, vagy küldjön hello partnertől tooand hello szerződés típusának meg kell egyeznie a.

Miután [integrációs-fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-accounts.md) és [adja hozzá a partnerek](logic-apps-enterprise-integration-partners.md), EDIFACT szerződés ezeket a lépéseket követve hozhat létre.

## <a name="create-an-edifact-agreement"></a>EDIFACT-egyezmény létrehozása 

1.  Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com "Azure-portálon"). Hello bal oldali menüben válassza ki a **további szolgáltatások**.

    > [!TIP]
    > Ha nem lát **további szolgáltatások**, lehetséges, hogy tooexpand hello menü először. Hello összecsukott hello menü felső részén válassza **megjelenítése menü**.

    ![A bal oldali menüben válassza a "Szolgáltatás"](./media/logic-apps-enterprise-integration-edifact/edifact-0.png)

2. Hello a keresőmezőbe írja be a "integrációt" a szűrőhöz. Hello eredmények listában válassza ki a **integrációs fiókok**.

    ![A "integrációt" szűrheti, válassza ki a "Integrációs fiókok"](./media/logic-apps-enterprise-integration-edifact/edifact-1-3.png)

3. A hello **integrációs fiókok** panelt megnyitó, jelölje be hello integrációs fiókra, amelyhez toocreate hello szerződést.
Ha nem lát minden integrációs fiókok [hozzon létre egyet első](../logic-apps/logic-apps-enterprise-integration-accounts.md "integrációs fiókokkal kapcsolatos összes").  

    ![Ha toocreate hello megállapodás integrációs fiók kiválasztása](./media/logic-apps-enterprise-integration-edifact/edifact-1-4.png)

4. Válassza ki a hello **megállapodások** csempére. Ha egy megállapodások csempe nem rendelkezik, először adjon hozzá hello csempe.   

    ![Válassza a "Szerződés" csempe](./media/logic-apps-enterprise-integration-edifact/edifact-1-5.png)

5. Hello megállapodások paneljén megjelenő válassza **Hozzáadás**.

    ![Válassza a "Hozzáadás"](./media/logic-apps-enterprise-integration-edifact/edifact-agreement-2.png)

6. A **Hozzáadás**, adjon meg egy **neve** a szerződés. A **szerződés típusának**, jelölje be **EDIFACT**. Jelölje be hello **fogadó Partner**, **gazdagép identitását**, **Vendég Partner**, és **Vendég identitás** a szerződés.

    ![Adja meg a szerződés részletei](./media/logic-apps-enterprise-integration-edifact/edifact-1.png)

    | Tulajdonság | Leírás |
    | --- | --- |
    | Név |Hello szerződés neve |
    | A szerződés típusa | EDIFACT kell lennie. |
    | Fogadó Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello fogadó partner által konfigurált házirendsablonok hello megállapodás hello szervezet jelöli. |
    | Gazdagép identitását |Hello fogadó partner azonosítója |
    | Vendég Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello Vendég partner hello szervezet, amely hello állomás partnerrel üzleti jelöli. |
    | Vendég identitás |Hello Vendég partner azonosítója |
    | Beállítások |Ezeket a tulajdonságokat a szerződés által fogadott üzenetek küldési tooall alkalmazni. |
    | Beállítások küldése |Ezek a Tulajdonságok tooall üzeneteket a szerződés vonatkozik. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Hogyan a szerződés leírók fogadott üzenetek konfigurálása

Most, hogy hello megállapodás tulajdonságok beállítása, beállíthatja a módját a jelen szerződés azonosítja, és kezeli a bejövő üzenetek és a szerződés partnerétől kapott.

1.  A **Hozzáadás**, jelölje be **fogadási beállítások**.
Állítsa be ezeket a tulajdonságokat a szerződés hello partnerrel, amely az üzenetek Önnel alapján. Tulajdonságleírások tekintse meg e szakasz táblázatai hello.

    **Beállítások** van felosztva ezekben a szakaszokban: azonosítók, visszaigazolás, sémákat, vezérlő számok, érvényesítése és belső beállítások.

    ![Konfigurálja a "Kapják meg a beállításokat"](./media/logic-apps-enterprise-integration-edifact/edifact-2.png)  

2. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

Mostantól a szerződés készen toohandle bejövő üzenetek tooyour tartó kiválasztott beállításokat.

### <a name="identifiers"></a>Azonosítók

| Tulajdonság | Leírás |
| --- | --- |
| UNB6.1 (címzett hivatkozás jelszó) |Adjon meg egy 1 és 14 karakter közötti alfanumerikus érték. |
| UNB6.2 (címzett hivatkozás minősítő) |Adjon meg egy alfanumerikus érték legalább egy karaktert, és legfeljebb két karakter. |

### <a name="acknowledgments"></a>Nyugták

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet visszaigazolása (CONTRL) |Válassza ki a jelölőnégyzet tooreturn egy műszaki (CONTRL) visszaigazolás toohello interchange küldő. hello visszaigazolás küldött toohello interchange küldő küldési beállítások hello hello szerződés alapján. |
| A nyugtázási (CONTRL) |Válassza ezt a funkcionális (CONTRL) visszaigazolás toohello interchange küldő hello visszaigazolás küldött toohello interchange küldő küldési beállítások hello hello szerződés alapján jelölőnégyzet tooreturn. |

### <a name="schemas"></a>Sémák

| Tulajdonság | Leírás |
| --- | --- |
| UNH2.1 (TÍPUS) |Válassza ki a tranzakció készlet típusa. |
| UNH2.2 (VERZIÓ) |Adja meg a hello üzenet verziószáma. (Minimális, egy karakter; maximális, három karakter). |
| UNH2.3 (KIADÁS) |Adja meg a hello üzenet kiadás száma. (Minimális, egy karakter; maximális, három karakter). |
| UNH2.5 (TÁRSÍTOTT HOZZÁRENDELT KÓD) |Adja meg a hozzárendelt hello kódot. (Legfeljebb hat karakterből állnak. Csak alfanumerikus karakterekből állhat). |
| UNG2.1 (APP-KÜLDŐAZONOSÍTÓ) |Adjon meg egy alfanumerikus érték legalább egy karaktert és egy legfeljebb 35 karakter hosszú lehet. |
| UNG2.2 (A KÜLDŐ ALKALMAZÁS KÓD MINŐSÍTŐ) |Adjon meg egy alfanumerikus érték egy legfeljebb négy karakter hosszú lehet. |
| SÉMA |Jelölje be hello korábban feltöltött sémát, toouse társított integrációs fiókjából. |

### <a name="control-numbers"></a>Vezérlő számok
| Tulajdonság | Leírás |
| --- | --- |
| Ellenőrző szám Interchange ismétlődések letiltása |ismétlődő kereszteződéseket tooblock, válassza ezt a tulajdonságot. Választásakor hello EDIFACT dekódolási művelet ellenőrzi a hello interchange ellenőrző szám (UNB5) kapott hello adatcserét nem egyezik meg a korábban feldolgozott interchange ellenőrzési számot. Ha találat esetén elvégez, majd hello interchange nem lett feldolgozva. |
| Ismétlődő UNB5 ellenőrzésének gyakorisága (napokban) |Ha úgy dönt, hogy toodisallow ismétlődő interchange vezérlő számok, megadhatja a hello napok számát, amelyek mikor tooperform hello ellenőrizze, mivel a hello megfelelő értékre állítja ezt a beállítást. |
| Csoport-ellenőrzőszám ismétlődésének letiltása |a duplikált csoportokhoz vezérlő számok (UNG5) felcserélődések tooblock, jelölje be ezt a tulajdonságot. |
| Tranzakciókészlet-ellenőrzőszám ismétlődésének letiltása |tooblock felcserélődések számokkal ismétlődő tranzakció set vezérlő (UNH1), jelölje be ezt a tulajdonságot. |
| EDIFACT nyugtázási ellenőrző szám |toodesignate hello tranzakció hivatkozási számok való használatra beállított nyugtázás, hello előtag, hivatkozási számos és egy utótagot adjon meg egy értéket. |

### <a name="validations"></a>Ellenőrzések

Érvényesítési sorokban befejeződése után, egy másik automatikusan kerül. Ha nem adja meg az összes szabály, érvényesítési hello "Alapértelmezett" sor használja.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet típusa |Válassza ki a hello EDI-üzenet típusa. |
| EDI érvényesítése |EDI-érvényesítést hajt végre hello séma EDI tulajdonságok, hosszára, üres adatelemek és záró elválasztók által definiált adattípusok. |
| Kiterjesztett érvényesítése |Ha hello adattípus nem EDI, érvényesítési hello adatok elem követelmény és engedélyezett ismétlési, enumerálások és adatok elem hossza érvényesítési (min vagy max). |
| Kezdő/záró nullából engedélyezése |Minden további, kezdő vagy záró nulla, és az szóköz karakter. Ne távolítsa el ezeket a karaktereket. |
| Trim kezdő/záró nullából |Távolítsa el a kezdő vagy záró nulla és karaktereket. |
| Záró elválasztó házirend |Záró elválasztók készítése. <p>Válassza ki **nem engedélyezett** tooprohibit záró határoló és elválasztók, hello kapott adatcserét. Ha hello interchange záró határoló és elválasztókat, hello interchange van deklarálva nem érvényes. <p>Válassza ki **nem kötelező** tooaccept felcserélődések vagy záró határoló és elválasztók nélkül. <p>Válassza ki **kötelező** Ha kapott hello interchange rendelkeznie kell záró határoló és elválasztókat. |

### <a name="internal-settings"></a>Belső beállítások

| Tulajdonság | Leírás |
| --- | --- |
| Üres XML-címkék létrehozása, ha a záró elválasztók engedélyezve vannak |Válassza ki az jelölőnégyzetet toohave hello interchange küldő tartalmazza XML-címkék elválasztók záró üres. |
| Adatcsere felosztása tranzakciókészletekre – tranzakciókészletek felfüggesztése hiba esetén|Elemzi az egyes tranzakciókra, állítsa be egy külön XML-dokumentum cseréje a hello megfelelő boríték toohello tranzakció készlet alkalmazásával. Felfüggesztheti csak hello tranzakció készleteket érvényesítése sikertelen. |
| Adatcsere felosztása tranzakciókészletekre – adatcsere felfüggesztése hiba esetén|Elemzi az egyes tranzakciókra, állítsa be egy külön XML-dokumentum cseréje a hello megfelelő boríték alkalmazásával. Hello teljes adatcsere felfüggesztése, ha egy vagy több tranzakció készletet hello interchange érvényesítése sikertelen. | 
| Megőrizheti a adatcsere - tranzakció készletek felfüggeszteni hiba |Hello interchange érintetlenül hagyja, létrehoz egy XML-dokumentum hello teljes kötegelt adatcserét. Felfüggesztése csak hello tranzakció készleteket érvényesítése sikertelen, miközben továbbra tooprocess minden más tranzakció állítja be. |
| Adatcsere megőrzése – adatcsere felfüggesztése hiba esetén |Hello interchange érintetlenül hagyja, létrehoz egy XML-dokumentum hello teljes kötegelt adatcserét. Hello teljes adatcsere felfüggesztése, ha egy vagy több tranzakció készletet hello interchange érvényesítése sikertelen. |

## <a name="configure-how-your-agreement-sends-messages"></a>Hogyan a szerződés küld üzeneteket konfigurálása

Hogyan a jelen szerződés azonosítja, és kezeli a kimenő üzenetek küldését a jelen szerződés tooyour partnerekkel is konfigurálhatja.

1.  A **Hozzáadás**, jelölje be **küldési beállítások**.
Állítsa be ezeket a tulajdonságokat a partnerrel, aki adatcseréihez használható üzenetek Önnel a szerződés alapján. Tulajdonságleírások tekintse meg e szakasz táblázatai hello.

    **Küldési beállításainak** van felosztva ezekben a szakaszokban: azonosítók, visszaigazolás, sémákat, borítékok, karakterkészletek és elválasztók, vezérlő számok és érvényesítést.

    ![Konfigurálja a "Küldési beállításai"](./media/logic-apps-enterprise-integration-edifact/edifact-3.png)    

2. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

A szerződés most már készen áll a kimenő üzenetek tooyour kiválasztott beállítások tartó toohandle áll.

### <a name="identifiers"></a>Azonosítók

| Tulajdonság | Leírás |
| --- | --- |
| UNB1.2 (szintaxis verzió) |Válasszon ki egy értéket közötti **1** és **4**. |
| UNB2.3 (feladó fordított irányú útválasztási címe) |Adja meg legalább egy karaktert és 14 karakterek alfanumerikus értékét. |
| UNB3.3 (címzett fordított irányú útválasztási címe) |Adja meg legalább egy karaktert és 14 karakterek alfanumerikus értékét. |
| UNB6.1 (címzett hivatkozás jelszó) |Adja meg legalább egy és 14 karakterek alfanumerikus értékét. |
| UNB6.2 (címzett hivatkozás minősítő) |Adjon meg egy alfanumerikus érték legalább egy karaktert, és legfeljebb két karakter. |
| UNB7 (Alkalmazásazonosító referencia) |Adjon meg egy legalább egy karaktert és 14 karakterek alfanumerikus érték |

### <a name="acknowledgment"></a>Tudomásul vétele
| Tulajdonság | Leírás |
| --- | --- |
| Üzenet visszaigazolása (CONTRL) |Akkor válassza ezt a jelölőnégyzetet, ha üzemeltetett hello partner tooreceive a műszaki (CONTRL) visszaigazolás vár. Ez a beállítás meghatározza, hogy üzemeltetett hello partner, aki hello üzenetet küld, kérelmek nyugtázást hello Vendég partnerről. |
| A nyugtázási (CONTRL) |Akkor válassza ezt a jelölőnégyzetet, ha üzemeltetett hello partner tooreceive a működési (CONTRL) visszaigazolás vár. Ez a beállítás meghatározza, hogy üzemeltetett hello partner, aki hello üzenetet küld, kérelmek nyugtázást hello Vendég partnerről. |
| SG1-/SG4-hurok létrehozása az elfogadott tranzakciókészletekhez |Ha toorequest működési nyugtázási, jelölje ki a jelölőnégyzet tooforce generációja SG1/HK4 hurkok működési CONTRL nyugták elfogadott tranzakció beállítása. |

### <a name="schemas"></a>Sémák
| Tulajdonság | Leírás |
| --- | --- |
| UNH2.1 (TÍPUS) |Válassza ki a tranzakció készlet típusa. |
| UNH2.2 (VERZIÓ) |Adja meg a hello üzenet verziószáma. |
| UNH2.3 (KIADÁS) |Adja meg a hello üzenet kiadás száma. |
| SÉMA |Válassza ki a hello séma toouse. Sémák integrációs fiókjában található. tooaccess a sémák először csatolja a integrációs fiók tooyour Logic Apps alkalmazást. |

### <a name="envelopes"></a>Borítékok
| Tulajdonság | Leírás |
| --- | --- |
| UNB8 (feldolgozás prioritását kód) |Adjon meg egy szedett egynél több karakter hosszú. |
| UNB10 (kommunikációs megállapodás) |Adjon meg egy alfanumerikus érték legalább egy karaktert, és legfeljebb 40 karakter. |
| UNB11 (tesztelése mutató) |Válasszon a jelölőnégyzet tooindicate, amely generált interchange hello adatok tesztelésére. |
| UNA-szegmens (szolgáltatás sztringjére vonatkozó javaslatok) használata |Válassza ki a jelölőnégyzet toogenerate egy UNA szegmens hello interchange toobe küldött. |
| UNG-szegmensek (funkciócsoport-fejléc) használata |Válassza ki a jelölőnégyzet toocreate hello funkcionális csoport fejlécben hello küldött üzenetek toohello Vendég partner szegmensek csoportosítása. hello következő értékei használt toocreate hello UNG szegmensek: <p>A **UNG1**, adja meg legalább egy karaktert és legfeljebb hat karakterből álló alfanumerikus értékét. <p>A **UNG2.1**, adjon meg egy alfanumerikus érték legalább egy karaktert és egy legfeljebb 35 karakter hosszú lehet. <p>A **UNG2.2**, adjon meg egy alfanumerikus érték egy legfeljebb négy karakter hosszú lehet. <p>A **UNG3.1**, adjon meg egy alfanumerikus érték legalább egy karaktert és egy legfeljebb 35 karakter hosszú lehet. <p>A **UNG3.2**, adjon meg egy alfanumerikus érték egy legfeljebb négy karakter hosszú lehet. <p>A **UNG6**, adja meg legalább egy és legfeljebb három karakterből álló alfanumerikus értékét. <p>A **UNG7.1**, adja meg legalább egy karaktert és legfeljebb három karakterből álló alfanumerikus értékét. <p>A **UNG7.2**, adja meg legalább egy karaktert és legfeljebb három karakterből álló alfanumerikus értékét. <p>A **UNG7.3**, adjon meg egy legalább 1 karakter és 6 karakterből álló alfanumerikus érték. <p>A **UNG8**, adja meg legalább egy karaktert és 14 karakterek alfanumerikus értékét. |

### <a name="character-sets-and-separators"></a>Karakterkészletek és elválasztók

Eltérő hello karakterkészlet, az egyes üzenet használt elválasztó karaktert toobe különböző szabálykészleteket adhatja meg. Ha egy karakterkészletet nincs megadva, az adott üzenethez a séma, hello alapértelmezett karakterkészlet használata.

| Tulajdonság | Leírás |
| --- | --- |
| UNB1.1 (rendszerazonosító) |SELECT hello EDIFACT karakter interchange kimenő hello alkalmazott toobe beállítása. |
| Séma |Válassza ki a séma hello legördülő listából. Minden egyes sorára befejezése után a rendszer automatikusan hozzáadja az új sort. Hello kijelölt séma, a select hello elválasztók beállítása, amelyet az toouse elválasztó leírása a következő hello alapján. |
| A bemeneti típus |Válasszon egy bemeneti típus hello legördülő listából. |
| Az összetevő elválasztó |tooseparate összetett adatelemek, adjon meg egy egyetlen karaktert. |
| Adatok elem elválasztó |tooseparate egyszerű adatelemek belül összetett adatelemek, adjon meg egy egyetlen karaktert. |
| Szegmens lezáró |egy EDI szegmens tooindicate hello végét adja meg egy egyetlen karaktert. |
| Utótag |Válassza ki a használt hello szegmensazonosító hello karakter. Ha egy utótagot kijelölni, majd hello szegmens lezáró adatok elem lehet üres. Ha hello szegmens lezáró üres, majd ki kell jelölnie egy utótagot. |

### <a name="control-numbers"></a>Vezérlő számok
| Tulajdonság | Leírás |
| --- | --- |
| UNB5 (Interchange ellenőrző szám) |Adja meg az előtag, egy hello interchange ellenőrző szám értékek tartományán és egy utótagként. Ezek az értékek használt toogenerate kimenő cseréje. hello előtag és utótag opcionálisak, amíg hello vezérlő számot kell megadni. hello ellenőrző szám értéke akkor nő, minden új üzenet; hello előtag és az utótag maradnak hello azonos. |
| UNG5 (csoportos ellenőrző szám) |Adja meg az előtag, egy hello interchange ellenőrző szám értékek tartományán és egy utótagként. Ezek az értékek használt toogenerate hello csoport ellenőrző szám. hello előtag és utótag opcionálisak, amíg hello vezérlő számot kell megadni. hello ellenőrző szám értéke akkor nő, minden új üzenet hello maximális érték eléréséig; hello előtag és az utótag maradnak hello azonos. |
| UNH1 (üzenet fejlécének hivatkozás száma) |Adja meg az előtag, egy hello interchange ellenőrző szám értékek tartományán és egy utótagként. Ezek az értékek használt toogenerate hello üzenet fejlécének hivatkozási számot. hello előtag és utótag opcionálisak, amíg hello hivatkozási számot kell megadni. hello hivatkozási szám értéke akkor nő, minden új üzenet; hello előtag és az utótag maradnak hello azonos. |

### <a name="validations"></a>Ellenőrzések

Érvényesítési sorokban befejeződése után, egy másik automatikusan kerül. Ha nem adja meg az összes szabály, érvényesítési hello "Alapértelmezett" sor használja.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet típusa |Válassza ki a hello EDI-üzenet típusa. |
| EDI érvényesítése |EDI-érvényesítést hajt végre hello EDI tulajdonságainak hello séma, hosszára, üres adatelemek és záró elválasztók által definiált adattípusok. |
| Kiterjesztett érvényesítése |Ha hello adattípus nem EDI, érvényesítési hello adatok elem követelmény és engedélyezett ismétlési, enumerálások és adatok elem hossza érvényesítési (min vagy max). |
| Kezdő/záró nullából engedélyezése |Minden további, kezdő vagy záró nulla, és az szóköz karakter. Ne távolítsa el ezeket a karaktereket. |
| Trim kezdő/záró nullából |Távolítsa el a kezdő vagy záró nulla karakterből. |
| Záró elválasztó házirend |Záró elválasztók készítése. <p>Válassza ki **nem engedélyezett** tooprohibit záró határoló és elválasztók, hello küldött interchange. Ha hello interchange záró határoló és elválasztókat, hello interchange van deklarálva nem érvényes. <p>Válassza ki **nem kötelező** toosend felcserélődések vagy záró határoló és elválasztók nélkül. <p>Válassza ki **kötelező** Ha küldi hello interchange záró határoló és elválasztókat rendelkeznie kell. |

## <a name="find-your-created-agreement"></a>A létrehozott megállapodás keresése

1.  Hello a szerződés tulajdonságok beállítása után **Hozzáadás** paneljén válassza **OK** toofinish létrehozása a szerződés és a visszatérési tooyour integrációs fiók panelen.

    Az újonnan hozzáadott foglalt most megjelenik a **megállapodások** listája.

2.  Az integrációs fiókok áttekintése is megtekintheti a szerződéseket. Az integráció-fiók panelen válassza **áttekintése**, majd jelölje be hello **megállapodások** csempére. 

    ![Válassza ki a "Szerződés" csempe tooview minden szerződés](./media/logic-apps-enterprise-integration-edifact/edifact-4.png)   

## <a name="view-swagger-file"></a>A Swagger-fájl megtekintése
tooview hello Swagger részletek hello EDIFACT-összekötőhöz: [EDIFACT](/connectors/edifact/).

## <a name="learn-more"></a>Részletek
* [További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  

