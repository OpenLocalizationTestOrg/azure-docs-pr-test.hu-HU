---
title: "az Active Directory replikációs állapotát az Azure Naplóelemzés aaaMonitor |} Microsoft Docs"
description: "Active Directory replikációs állapotát megoldáscsomag hello rendszeresen figyeli az Active Directory-környezet minden replikációs hibák és jelenti a hello eredmények az OMS irányítópulton."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: carmonm
editor: 
ms.assetid: 1b988972-8e01-4f83-a7f4-87f62778f91d
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: banders
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 235e4f7a066ea50b79f681398182b22c91fb6d31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-active-directory-replication-status-with-log-analytics"></a>A Naplóelemzési Active Directory replikációs állapot figyelése

![AD-replikáció állapotát szimbólum](./media/log-analytics-ad-replication-status/ad-replication-status-symbol.png)

Az Active Directory az informatikai környezetben vállalati nyilvános kulcsokra épülő. tooensure magas rendelkezésre állású és nagy teljesítményű, minden egyes tartományvezérlő van a saját hello Active Directory-adatbázis másolatát. Tartományvezérlők replikálják egymás mellett rendelés toopropagate hello vállalaton belül. A replikálási folyamat-hibák hatására problémák számos hello vállalaton belül.

AD-replikáció állapotát megoldáscsomag hello rendszeresen figyeli az Active Directory-környezet minden replikációs hibák és jelenti a hello eredmények az OMS irányítópulton.

## <a name="installing-and-configuring-hello-solution"></a>Telepítése és konfigurálása hello megoldás
A következő információk tooinstall hello használja, és hello megoldás konfigurálása.

* Telepítenie kell ügynökök olyan tartományvezérlőn, amely tagjai hello tartomány toobe értékeli ki. Vagy telepíthet ügynököket a tagkiszolgálók és hello ügynökök toosend AD replikációs adatok tooOMS konfigurálnia kell. hogyan: a Windows-számítógépek tooOMS, tooconnect toounderstand [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md). Ha a tartományvezérlő már, amelyet az tooconnect tooOMS meglévő System Center Operations Manager környezet része, lásd: [csatlakozás az Operations Manager tooLog Analytics](log-analytics-om-agents.md).
* Adja hozzá a hello Active Directory replikációs állapotát megoldás tooyour ismertetett eljárással hello OMS-munkaterület [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md).  Nincs szükség további konfigurációra.

## <a name="ad-replication-status-data-collection-details"></a>AD replikációs állapot adatok gyűjtemény részletei
hello következő táblázatban adatgyűjtési módszerek és egyéb adatok gyűjtése hogyan AD replikációs állapot részleteit.

| Platform | Közvetlen ügynök | SCOM-ügynököt | Azure Storage | SCOM szükséges? | Felügyeleti csoport keresztül küldött SCOM ügynök adatok | Gyűjtemény gyakorisága |
| --- | --- | --- | --- | --- | --- | --- |
| Windows |&#8226; |&#8226; |  |  |&#8226; |öt naponta |

## <a name="optionally-enable-a-non-domain-controller-toosend-ad-data-toooms"></a>Szükség esetén engedélyezze az egy nem tartományvezérlő toosend AD adatok tooOMS
Ha nem szeretné tooconnect bármely tartományvezérlő közvetlen tooOMS, használható OMS csatlakozó többi számítógép a tartományi toocollect hello AD replikációs állapot megoldás az adatok csomagot és annak hello adatküldéshez.

### <a name="tooenable-a-non-domain-controller-toosend-ad-data-toooms"></a>egy nem tartományvezérlő toosend AD adatok tooOMS tooenable
1. Győződjön meg arról, hogy hello számítógép, hogy kívánja-e hello AD-replikáció állapotát megoldással toomonitor hello tartomány tagja.
2. [Csatlakozás a Windows-számítógép tooOMS hello](log-analytics-windows-agents.md) vagy [csatlakoztassa a meglévő Operations Manager környezet tooOMS használatával](log-analytics-om-agents.md), ha az nincs csatlakoztatva.
3. Az adott számítógépen állítsa be a következő beállításkulcs hello:

   * Kulcs: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management csoportok\<ManagementGroupName > \Solutions\ADReplication**
   * Érték: **IsTarget**
   * Érték: **igaz**

   > [!NOTE]
   > A módosítások csak az újraindítás hello Microsoft Monitoring Agent szolgáltatása (HealthService.exe) nem lépnek életbe.
   >
   >

## <a name="understanding-replication-errors"></a>Replikációs hibák ismertetése
Miután tooOMS küldött AD állapot adatokat, a lemezkép jelenleg hány replikálási hibákat jelző hello OMS-irányítópulton a következő csempe hasonló toohello láthatja.  
![AD-replikáció állapota csempe](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Kritikus fontosságú replikációs hibák** hibák, amelyek vagy annál újabb 75 %-a hello [szemétgyűjtési](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) az Active Directory-erdőben.

Hello csempére kattintva megtekintheti a hello hibákkal kapcsolatos további információkat.
![AD-replikáció állapotát irányítópult](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)

### <a name="destination-server-status-and-source-server-status"></a>Cél kiszolgálójának állapotát és a forrás-kiszolgáló állapota
Ezekben az oszlopokban a célkiszolgáló és adatforrás-kiszolgálók, amelyeken a replikációs hibák hello állapotának megjelenítése. minden egyes tartományvezérlő neve után hello szám replikációs hibák tartományvezérlő hello számát jelzi.

a célkiszolgáló és a forráskiszolgálókra hello hibák láthatók, mert bizonyos problémák könnyebb tootroubleshoot hello forrás server szempontjából, és mások hello cél kiszolgáló szempontjából.

Ebben a példában láthatja, hogy sok célkiszolgálókon nagyjából rendelkezik-e hibák azonos számú hello, de egy forráskiszolgáló (ADDC35), amelyen számos további hibák összes hello mint mások. Valószínű, hogy van-e probléma okozza, hogy toofail toosend adatok tooits replikációs partnerek ADDC35 meg. ADDC35 kijavította hello problémák megoldására hello hibák hello cél kiszolgáló területen megjelenő számos.

### <a name="replication-error-types"></a>Replikációs hiba típusa
Ez a terület hello típusú a vállalaton belüli észlelt hibák adatait jeleníti meg. Minden egyes hibához egyedi numerikus kód és egy üzenetet, amely segít meghatározni hello hiba okának hello rendelkezik.

hello fánk hello felső lehetővé egy ötletet, amelynek hibák jelennek meg több vagy kevesebb, gyakran az adott környezetben.

Jelzi, hogy ha több tartományvezérlőt üzemeltető tapasztalhat hello azonos replikációs hiba. Ebben az esetben, akkor előfordulhat, hogy képes toodiscover kell vagy megoldásán egy tartományvezérlőn, majd ismételje meg, más tartományvezérlők által érintett hello azonos hiba.

### <a name="tombstone-lifetime"></a>Szemétgyűjtési
hello szemétgyűjtési határozza meg, hogy mennyi ideig törölt objektum, a törlésre kijelölt tooas említett, hello Active Directory adatbázisban marad. Ha a törölt objektum fázisok hello szemétgyűjtési a szemétgyűjtő folyamatának automatikusan eltávolítja a hello Active Directory-adatbázis.

hello alapértelmezett szemétgyűjtési 180 nappal a Windows, a legújabb verzióját, de régebbi verzióin 60 nap volt, és egy Active Directory-rendszergazda explicit módon módosítható.

Ha vannak eléri vagy korábbi hello szemétgyűjtési replikációs hibák nem fontos tooknow. Ha két tartományvezérlő egy replikációs hiba, amely a legutóbbi szemétgyűjtési hello továbbra is fennáll, replikáció le van tiltva a két tartományvezérlők közötti akkor is, ha az alapul szolgáló replikációs hiba hello rögzített.

hello szemétgyűjtési terület könnyebb legyen azonosítani annak a veszélye, azonban letiltott replikáció esetén helyek. Minden egyes hiba a hello **több mint 100 %-os TSL** kategória javasoljuk, hogy a forrás és cél kiszolgáló között a következő nem replikálása legalább hello szemétgyűjtési hello erdő jelöli.

Ilyen esetben egyszerűen a hello replikációs hiba elhárítása nem lesz elég. Minimálisan szükséges toomanually tooidentify vizsgálata és a fennmaradó objektumok replikációs újraindításához először tisztítása. Egy tartományvezérlő toodecommission is szükség lehet.

Továbbá tooidentifying túli hello szemétgyűjtési, fennállásának replikációs hibákat is érdemes toopay figyelmet tooany hibák hello tartozó **50-75 % TSL** vagy **75-100 %-os TSL** kategóriák.

Ezek a hibák, amelyek egyértelműen fennmaradó, nem átmeneti, ezért valószínűleg a beavatkozás tooresolve kell. hello jó hírünk, hogy nem még elérték hello szemétgyűjtési. Ha azonnal a problémák kijavításához és *előtt* hello szemétgyűjtési érik, replikációs minimális kézi beavatkozás indíthatja újra.

Ahogy azt korábban említettük, a hello irányítópult csempe hello AD a replikációs állapot megoldáshoz hello számát mutatja *kritikus* a környezetben, amelyet hibaként, amelyek több mint 75 %-a szemétgyűjtési (beleértve a replikációs hibák hibák, amelyek több mint 100 %-a TSL). Szükség tookeep Ez az érték 0.

> [!NOTE]
> Minden hello törlésre élettartama százalékos számítás tényleges szemétgyűjtési hello az Active Directory-erdő alapulnak, úgy, hogy ezeket a szolgáltatásajánlatokat pontosak, még akkor is, ha egy egyéni törlésre való kijelölés élettartamértékénél, állítsa be még megbízik.
>
>

### <a name="ad-replication-status-details"></a>AD replikációs állapotának részletei
Amikor egy hello listák valamely elemére kattint, további részleteit naplófájl-keresési látható. hello eredményei szűrt tooshow csak hello hibák kapcsolódó toothat elemet. Például, ha rákattint az első tartományvezérlő hello felsorolt **cél kiszolgáló állapota (ADDC02)**, megjelenik a keresési eredmények alapján szűrt tooshow hibák hello célkiszolgáló tulajdonosaként tartományvezérlő:

![Replikációs állapothibákat AD a találatok között](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Itt további szűréséhez, módosítsa hello keresési lekérdezést, és így tovább. Hello napló keresése használatával kapcsolatos további információkért lásd: [keresések jelentkezzen](log-analytics-log-searches.md).

Hello **HelpLink** mezőben látható, a TechNet-oldal, hogy adott hibával kapcsolatban további részletekkel hello URL-CÍMÉT. Másolja, és ez a hivatkozás illessze be a böngésző-ablak toosee hibaelhárítási és hello hiba kijavítása kapcsolatos információkat.

Is **exportálása** tooexport hello tooExcel eredménye. Hello adatexportálási segítségével megjelenítheti a replikációs hiba adatok milyen módon.

![exportált AD replikációs állapothibákat az Excel programban](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD-replikáció állapotát – gyakori kérdések
**K: milyen gyakran az AD replikációs állapot adatok frissítése?**
V: hello információ öt naponta frissül.

**K: van egy módon tooconfigure, milyen gyakran frissül, ezeket az adatokat?**
Jelenleg nem A:.

**K: van tooadd összes saját tartomány tartományvezérlői toomy OMS-munkaterület rendelés toosee replikálási állapottal?**
V: nem, csak egy tartományvezérlő hozzá kell adni. Ha az OMS-munkaterület több tartományvezérlőn is van, ezek az adatok tooOMS küld.

**K: most nem kívánok tooadd bármely tartományi vezérlők toomy OMS-munkaterület. Továbbra is használható hello AD replikációs állapot megoldás?**
V: Igen. Beállíthatja, hogy a beállításjegyzék-kulcs tooenable hello értéke azt. Lásd: [tooenable egy nem tartományvezérlő toosend AD adatok tooOMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**K: Mi az az adatgyűjtés hello hello folyamat hello neve?**
V: AdvisorAssessment.exe

**K: mennyi ideig tart a gyűjtött adatok toobe?**
V: gyűjtemény ideje hello hello Active Directory-környezet méretétől függ, de általában a kisebb, mint 15 percet vesz igénybe.

**K: milyen típusú adatokat gyűjt a rendszer?**
V: replikációs adatok összegyűjtése LDAP keresztül történik.

**K: van egy módon tooconfigure adatainak a gyűjtése?**
Jelenleg nem A:.

**K: engedélyekkel mire toocollect adatok van szükségem?**
V: normál felhasználói engedélyek tooActive Directory elegendőek.

## <a name="troubleshoot-data-collection-problems"></a>Adatok gyűjtése kapcsolatos problémák elhárítása
Rendelés toocollect adatokban hello AD replikációs állapot megoldáscsomag legalább egy tartomány tartományvezérlői csatlakoztatott toobe tooyour OMS-munkaterület szükséges. Csatlakozzon egy olyan tartományvezérlőre, amíg megjelenik egy üzenet azt jelzi, hogy **továbbra is gyűjtenek adatokat**.

Csatlakozás egy tartományvezérlő segítségre van szüksége, ha címen tekintheti [csatlakozás Windows számítógépek tooLog Analytics](log-analytics-windows-agents.md). Azt is megteheti, ha a tartományvezérlő már csatlakoztatott tooan meglévő System Center Operations Manager környezet, megtekintheti címen [csatlakozás a System Center Operations Manager tooLog Analytics](log-analytics-om-agents.md).

Ha nem szeretné tooconnect bármely tartományvezérlő közvetlen tooOMS vagy tooSCOM, lásd: [tooenable egy nem tartományvezérlő toosend AD adatok tooOMS](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

## <a name="next-steps"></a>Következő lépések
* Használjon [Log Analytics-e jelentkezni a keresések](log-analytics-log-searches.md) tooview részletes Active Directory replikációs adatokat.
