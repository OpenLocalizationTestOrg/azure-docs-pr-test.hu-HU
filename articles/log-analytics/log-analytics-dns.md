---
title: "az Azure Naplóelemzés elemzési megoldások aaaDNS |} Microsoft Docs"
description: "Állítsa be, és hello DNS elemzési megoldások a DNS-infrastruktúra a biztonsággal, a teljesítmény és a műveletek Naplóelemzési toogather betekintést."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: f44a40c4-820a-406e-8c40-70bd8dc67ae7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: banders
ms.openlocfilehash: be7982c54b65ba0c4b1c15ae7516d02eced313f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="gather-insights-about-your-dns-infrastructure-with-hello-dns-analytics-preview-solution"></a>A DNS-infrastruktúra a hello DNS Analytics Preview megoldás észrevételeket összegyűjtése

![DNS Analytics szimbólum](./media/log-analytics-dns/dns-analytics-symbol.png)

Ez a cikk ismerteti, hogyan tooset fel és -felhasználási hello Azure DNS elemzési megoldás az Azure Naplóelemzés toogather betekintést DNS-infrastruktúra a biztonsággal, a teljesítmény és a műveletek.

DNS Analytics nyújt segítséget:

- Ügyfelek, amelyek tooresolve rosszindulatú tartománynév azonosítása.
- Elavult erőforrásrekordok azonosításához.
- Azonosítsa, gyakran lekérdezett tartománynevek és talkative DNS-ügyfeleket.
- DNS-kiszolgálók nézet kérelem terhelése.
- Tekintse meg a dinamikus DNS-regisztráció sikertelen.

hello megoldás gyűjti, elemzi és a Windows DNS elemzési és a vizsgálati naplók és egyéb kapcsolódó adatokat a DNS-kiszolgálók ad eredményül.

## <a name="connected-sources"></a>Összekapcsolt források

a következő táblázat hello hello csatlakoztatott adatforrások, ez a megoldás által támogatott ismerteti:

| **Csatlakoztatott adatforrás** | **Támogatás** | **Leírás** |
| --- | --- | --- |
| [Windows-ügynökök](log-analytics-windows-agents.md) | Igen | hello megoldás DNS információt gyűjt a Windows-ügynökök. |
| [Linux-ügynökök](log-analytics-linux-agents.md) | Nem | hello megoldás nem DNS-információkat gyűjtsön a közvetlen Linux-ügynököt. |
| [System Center Operations Manager felügyeleti csoport](log-analytics-om-agents.md) | Igen | hello megoldás DNS adatokat gyűjt az ügynökök a csatlakoztatott Operations Manager felügyeleti csoportban. A hello Operations Manager ügynök toohello Operations Management Suite közvetlen kapcsolatra szükség. Adattárból hello felügyeleti csoport toohello Operations Management Suite adat továbbítódik. |
| [Azure Storage-fiók](log-analytics-azure-storage.md) | Nem | Az Azure storage hello megoldás nem használja. |

### <a name="data-collection-details"></a>Az gyűjtemény adatait

hello megoldás gyűjti DNS-készlet és a DNS-esemény kapcsolatos adatok hello DNS-kiszolgálókról ahol Log Analytics-ügynök telepítve van. Ezeket az adatokat, majd feltöltött tooLog elemzés és hello megoldás irányítópultja jelenik meg. Hello DNS PowerShell-parancsmagok futtatását leltárral kapcsolatos adatok, például a DNS-kiszolgálók, a zóna és a erőforrásrekordokat, hello számát gyűjti. hello adatok két naponta egyszer frissül. hello esemény kapcsolatos adatokat közel valós idejű gyűjtött hello [elemzési és a naplók](https://technet.microsoft.com/library/dn800669.aspx#enhanc) továbbfejlesztett DNS-naplózás és diagnosztika a Windows Server 2012 R2 által biztosított.

## <a name="configuration"></a>Konfiguráció

Információ tooconfigure hello megoldáson hello használata:

- Rendelkeznie kell egy [Windows](log-analytics-windows-agents.md) vagy [Operations Manager](log-analytics-om-agents.md) minden DNS-kiszolgálón, amelyet az toomonitor ügynök.
- Hello is hozzáadhat hello DNS elemzési megoldás tooyour Operations Management Suite-munkaterülettel [Azure piactér](https://aka.ms/dnsanalyticsazuremarketplace). Hello ismertetett folyamatot is használható [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).

hello megoldás hello szükség további konfiguráció nélkül adatgyűjtés megkezdése. A következő konfigurációs toocustomize adatgyűjtés hello is használhatja.

### <a name="configure-hello-solution"></a>Hello megoldás konfigurálása

Az hello megoldás irányítópultján kattintson **konfigurációs** tooopen hello DNS Analytics konfiguráció lapon. Konfigurációs változások, hogy két típusa van:

- **Tartománynevek szerepel az engedélyezési listán**. hello megoldás nem dolgozza fel az összes hello keresési lekérdezés. A tartománynév-utótagokat az engedélyezési lista tart fenn. Hárítsa el a megfelelő tartománynév-utótagokat az engedélyezési lista a tartománynevek toohello hello keresési lekérdezések nem dolgozza fel hello megoldás. Nem dolgozza fel az engedélyezési listán szereplő tartománynevek toooptimize hello adatforgalom tooLog Analytics segítségével. hello alapértelmezett engedélyezési lista népszerű nyilvános tartomány nevét, például www.google.com és www.facebook.com tartalmazza. Hello alapértelmezett teljes lista megtekintéséhez görgetéshez használható.

 Hello lista tooadd módosíthatja bármely tooview keresési elemzések a használni kívánt tartománynév utótagja. Eltávolíthatja a nem kívánt tooview keresési elemzések a tartománynév utótagja is.

- **Talkative ügyfél küszöbérték**. DNS-ügyfelek, amelyek mérete meghaladja hello küszöbérték keresési kérelmek száma hello kijelölt hello **DNS-ügyfelek** panelen. hello alapértelmezett küszöbértéke 1000. Szerkesztheti a hello küszöbértéket.

    ![Szerepel az engedélyezési listán tartománynevek](./media/log-analytics-dns/dns-config.png)

## <a name="management-packs"></a>Felügyeleti csomagok

Hello Microsoft-Figyelőügynök tooconnect tooyour Operations Management Suite-munkaterülettel használatakor hello következő felügyeleti csomag telepítve van:

- Microsoft DNS adatokat gyűjtő Intelligence Pack (Microsft.IntelligencePacks.Dns)

Ha az Operations Manager felügyeleti csoportjának Operations Management Suite-munkaterülettel csatlakoztatott tooyour, hello következő felügyeleti csomagjai vannak telepítve az Operations Manager ebben a megoldásban hozzáadásakor. Nincs szükség konfigurációs vagy karbantartási a felügyeleti csomagok:

- Microsoft DNS adatokat gyűjtő Intelligence Pack (Microsft.IntelligencePacks.Dns)
- A Microsoft System Center Advisor DNS konfigurációja (Microsoft.IntelligencePack.Dns.Configuration)

A megoldás felügyeleti csomagok frissítésének további információkért lásd: [csatlakozás az Operations Manager tooLog Analytics](log-analytics-om-agents.md).

## <a name="use-hello-dns-analytics-solution"></a>Hello DNS elemzési megoldás használja

Ez a szakasz ismerteti az összes hello irányítópult funkciók és hogyan toouse őket.

Hello megoldás tooyour munkaterület hozzáadása után hello megoldás csempe hello Operations Management Suite – Áttekintés lapon a DNS-infrastruktúra gyors összegzését tartalmazza. Ez magában foglalja a DNS-kiszolgálók, ahol hello folyik adatgyűjtés hello száma. Hello kérelmek száma az ügyfelek tooresolve rosszindulatú tartományra hello elmúlt 24 óra is tartalmaz. Amikor hello csempére kattint, megnyílik a hello megoldás irányítópultja.

![DNS Analytics csempe](./media/log-analytics-dns/dns-tile.png)

### <a name="solution-dashboard"></a>A megoldás irányítópultja

hello megoldás irányítópultja hello megoldás különböző funkcióhoz hello összefoglaló információit jeleníti meg. Hivatkozásokat is tartalmaz toohello részletes nézet bírósági elemzéshez és elemzés céljából. Alapértelmezés szerint hello adatok jelennek meg hello elmúlt hét napban. Hello segítségével módosíthatja a hello dátum és idő tartomány **dátum és idő kiválasztása vezérlő**, ahogy az a következő kép hello:

![Kijelölés vezérlő](./media/log-analytics-dns/dns-time.png)

hello megoldás irányítópult a következő paneleken hello jeleníti meg:

**DNS-biztonság**. Jelentések hello DNS-ügyfelek, amelyek megadásakor a rosszindulatú tartományokkal toocommunicate. Microsoft fenyegetés eszközintelligencia hírcsatornák használatával DNS Analytics ügyfél IP-címe, amelyhez tooaccess rosszindulatú tartományok képes észlelni. Sok esetben a kártevők fertőzött eszközök "telefonos" toohello "parancs és vezérlő" center hello rosszindulatú tartomány által feloldása hello kártevő tartomány neve.

![DNS-biztonság panel](./media/log-analytics-dns/dns-security-blade.png)

Ha egy ügyfél IP-cím hello listában gombra kattint, a naplófájl-keresési megnyílik, és hello megfelelő lekérdezés hello keresési részletes adatainak megjelenítése. A következő példa hello, DNS-elemzés azt észlelte, hogy hello kommunikációs végezhető el az olyan [IRCbot](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=Win32/IRCbot):

![Napló keresési eredményeket megjelenítő ircbot](./media/log-analytics-dns/ircbot.png)

hello információk megkönnyítik a tooidentify a:

- Ügyfél IP-cím hello kommunikációt kezdeményezett.
- Oldja fel a toohello kártékony IP tartománynevet.
- IP-címek, amelyek tartománynév hello oldja fel.
- Kártékony IP-címet.
- Hello probléma súlyossága.
- Letiltott hello kártékony IP okát.
- Észlelés ideje.

**Tartományok lekérdezett**. Itt hello leggyakoribb tartománynevek lekérdezett hello DNS-ügyfelek számára a környezetben. Hello lekérdezett összes hello tartománynevek listáját tekintheti meg. Le is mehet le hello keresési kérelem részleteinek napló keresés egy adott tartomány nevét.

![Tartományok lekérdezett panel](./media/log-analytics-dns/domains-queried-blade.png)

**DNS-ügyfelek**. Jelentések hello ügyfelek *megsértése hello küszöbérték* a választott időszakban hello a lekérdezések száma. Hello DNS-ügyfelek és hello lekérdezéseiről azokat a keresési napló hello részleteit hello listája tekintheti meg.

![DNS-ügyfelek panel](./media/log-analytics-dns/dns-clients-blade.png)

**Dinamikus DNS-regisztráció**. Jelentések eszközregisztrációs hibák nevet. Minden eszközregisztrációs hibák cím [erőforrásrekordok](https://en.wikipedia.org/wiki/List_of_DNS_record_types) (típus A és AAAA) hello ügyfél IP-címek, amelyek hello regisztrációs kérelmet együtt vannak kiemelve. Ezen információk toofind hello okának hello regisztrációs hiba használhatja, ha az alábbi lépéseket követve:

1. Hello zóna mérvadó hello neve, amely az ügyfél hello próbál tooupdate megkeresése

2. Hello megoldás toocheck hello leltáradatokat, hogy a zóna használja.

3. Ügyeljen a hello dinamikus frissítést, a zóna hello engedélyezve van.

4. Ellenőrizze, hogy hello zóna konfigurálták biztonságos dinamikus frissítés vagy sem.

    ![Dinamikus DNS-regisztráció panel](./media/log-analytics-dns/dynamic-dns-reg-blade.png)

**Regisztrációs kérelem neve**. hello felső csempe sikeres és sikertelen DNS dinamikus frissítési kérelmek trendvonal jeleníti meg. hello alacsonyabb csempe hello első 10 ügyfelek, amelyek üzenetet küld sikertelen DNS-frissítési kérelmek toohello DNS-kiszolgálók, a sikertelen hello száma szerint rendezve vannak felsorolva.

![Név regisztrációs kérelmek panel ](./media/log-analytics-dns/name-reg-req-blade.png)

**Eszközillesztő elemzési lekérdezések minta**. Hello leggyakoribb keresési lekérdezések raw analytics adatlehívás közvetlenül listáját tartalmazza.

[!include[log-analytics-log-search-nextgeneration](../../includes/log-analytics-log-search-nextgeneration.md)]

![A lekérdezés](./media/log-analytics-dns/queries.png)

Ezeket a lekérdezéseket kiindulási pontként használható a saját testreszabott jelentéskészítéshez lekérdezések létrehozásáról. hello lekérdezések hivatkozás toohello DNS Analytics napló keresése oldal, ahol eredmény jelenik meg:

- **DNS-kiszolgálók listája**. Az összes DNS-kiszolgálók, a kapcsolódó teljes Tartománynevét, tartománynév, erdő neve, és a kiszolgáló IP-címek listáját jeleníti meg.
- **DNS-zónák listájának**. Hello társított zóna nevét, a dinamikus frissítési állapot, a kiszolgálókat és a DNSSEC-aláírási állapot az összes DNS-zóna listáját tartalmazza.
- **Nem használt erőforrásrekordok**. Hello minden nem használt vagy elavult erőforrásrekordok listáját tartalmazza. Ez a lista azokat a hello erőforrásrekord-névhez, erőforrásrekord-típus, hello tartozó DNS-kiszolgáló, rekord eseménygenerálás és zóna nevét. Használhatja a lista tooidentify hello DNS-erőforrásrekordok, amelyeket már nem használnak. Ezen információ alapján, majd eltávolíthat tételekhez hello DNS-kiszolgálók.
- **DNS-kiszolgálók lekérdezése terhelés**. Információit jeleníti meg, hogy a DNS-kiszolgálókon is ki lehet egy DNS-betöltése hello szempontjából. Ez az információ segíthet hello kapacitás hello kiszolgálók. Elvégezheti a toohello **metrikák** toochange hello nézet tooa grafikus képi megjelenítés fülre. Ez a nézet segít megérteni, hogyan hello DNS betölteni a DNS-kiszolgálók pontjain. DNS-lekérdezések aránya trendek kiszolgálónként mutatja.

    ![DNS-kiszolgálók lekérdezések napló keresési eredményei](./media/log-analytics-dns/dns-servers-query-load.png)

- **DNS-zóna lekérdezése a terhelés**. Jeleníti meg az összes hello zóna DNS zóna-lekérdezés-másodpercenként statisztika hello hello megoldás által kezelt hello DNS-kiszolgálókon. Kattintson a hello **metrikák** toochange hello nézetet részletes rekordok tooa grafikus képi megjelenítés hello eredmények lapon.
- **Konfigurációs események**. Minden hello DNS Konfigurációmódosítási események és társított üzeneteket jeleníti meg. Ezek az események hello esemény, eseményazonosító, a DNS-kiszolgáló ideje alapján majd végezhet vagy Feladatkategória. hello adatok segítségével végrehajtott módosítások toospecific DNS-kiszolgálók naplózni a megadott időpontban.
- **DNS-analitikai naplója**. Hello megoldás által kezelt összes hello DNS-kiszolgáló összes hello elemzési eseményeket tartalmazza. Ezek az események idő hello esemény, eseményazonosító, DNS-kiszolgáló, ügyfél IP-cím hello keresési lekérdezés, és a típus Feladatkategória lekérdezése alapján majd végezhet. DNS-kiszolgáló elemzési események nyomon követése hello DNS-kiszolgálón tevékenység engedélyezése. Elemzési eseményt naplózott, minden alkalommal, amikor hello server küld vagy fogad a DNS-adatok.

### <a name="search-by-using-dns-analytics-log-search"></a>DNS-Analytics napló keresés a keresés

Hello napló keresése oldal, a lekérdezést is létrehozhat. A keresési eredményeket szűrheti értékkorlátozás vezérlők használatával. Az eredmények fejlett lekérdezési tootransform, a szűrő és a jelentés is létrehozhat. Indítsa el a következő lekérdezések hello segítségével:

1. A hello **lekérdezés keresőmezőbe**, típus `Type=DnsEvents` tooview összes hello hello megoldás által kezelt hello DNS-kiszolgálók által létrehozott DNS-események. hello eredmények hello naplóadatokat összes események kapcsolódó toolookup lekérdezéseket, a dinamikus regisztráció és a konfigurációs módosítások listában.

    ![DnsEvents napló keresése](./media/log-analytics-dns/log-search-dnsevents.png)  

    a. tooview hello naplóadatokat keresési lekérdezések esetén válassza **LookUpQuery** , hello **altípus** szűrő hello bal oldali hello értékkorlátozás vezérlőből. Egy táblázat hello minden hello keresési lekérdezés eseménye kiválasztott időszak jelenik meg.

    b. tooview hello adatainak naplózása dinamikus regisztrációk, válassza ki **DynamicRegistration** , hello **altípus** szűrő hello bal oldali hello értékkorlátozás vezérlőből. Egy táblázat minden hello dinamikus regisztrációs eseménye hello kijelölt időszakra vonatkozó jelenik meg.

    c. tooview hello naplóadatokat konfigurációs módosítások, válassza ki **konfigurációváltozás** , hello **altípus** szűrő hello bal oldali hello értékkorlátozás vezérlőből. Minden hello Konfigurációmódosítási események a kijelölt időszakban hello tartalmazó táblázat jelenik meg.

2. A hello **lekérdezés keresőmezőbe**, típus `Type=DnsInventory` tooview összes hello hello DNS-kiszolgálók hello megoldás által kezelt DNS-leltárral kapcsolatos adatok. hello eredmények hello naplóadatok a DNS-kiszolgálók, DNS-zónák és erőforrásrekordok listában.

    ![DnsInventory napló keresése](./media/log-analytics-dns/log-search-dnsinventory.png)

## <a name="feedback"></a>Visszajelzés

Két módon visszajelzést is:

- **UserVoice**. A DNS elemzési szolgáltatások toowork ötleteket fel. A Microsoft hello [Operations Management Suite UserVoice lap](https://aka.ms/dnsanalyticsuservoice).
- **Csatlakozás a kohorszok**. Azt mindig kíváncsiak vagyunk abban, csatlakoztassa a cohorts tooget korai hozzáférési toonew funkciókat és DNS elemzési javítása érdekében az új ügyfelek. Ha érdekli a cohorts csatlakoztatása, töltse ki [a gyors felmérés](https://aka.ms/dnsanalyticssurvey).

## <a name="next-steps"></a>Következő lépések

[Naplók keresése](log-analytics-log-searches.md) tooview részletes DNS naplóban rögzíti.
