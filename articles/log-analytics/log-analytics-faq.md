---
title: "aaaLog Analytics – gyakori kérdések |} Microsoft Docs"
description: "Válaszok toofrequently kérdések a hello Azure Naplóelemzés szolgáltatás."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: ad536ff7-2c60-4850-a46d-230bc9e1ab45
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/18/2017
ms.author: magoedte
ms.openlocfilehash: 25931f521cbb6ec840184221c6c1a5794b3445f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-faq"></a>Log Analytics – gyakori kérdések
A Microsoft FAQ a Microsoft Operations Management Suite (OMS) szolgáltatáshoz gyakran feltett kérdésekre listáját. Ha Log Analyticshez kapcsolatos további kérdése van, lépjen a toohello [vitafóruma](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) és kérdéseit. Ha kérdése van gyakori, azt hozzá toothis cikket, hogy gyorsan és könnyen megtalálhatók.

## <a name="general"></a>Általános kérdések

### <a name="q-does-log-analytics-use-hello-same-agent-as-azure-security-center"></a>Q. Naplóelemzési használja hello ugyanaz az ügynök, az Azure Security Center?

A. Korai. június 2017 az Azure Security Center megkezdte hello Microsoft Monitoring Agent toocollect és a tároló adatok felhasználásával. több, lásd: toolearn [Azure Security Center Platform áttelepítési gyakran ismételt kérdések](../security-center/security-center-platform-migration-faq.md).

### <a name="q-what-checks-are-performed-by-hello-ad-and-sql-assessment-solutions"></a>Q. Milyen ellenőrzéseket hajtja végre hello AD és az SQL-értékelési megoldás?

A. hello következő lekérdezés leírását jeleníti meg az összes elvégzett jelenleg:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

hello eredmények majd lehet exportált tooExcel további ellenőrzésre.

### <a name="q-why-do-i-see-something-different-than-oms-in-system-center-operations-manager-console"></a>K: Miért látom azt egy másik, mint *OMS* System Center Operations Manager konzolon?

A: attól függően milyen frissítés összesítése az Operations Manager van, megjelenik egy csomópont *System Center Advisor*, *Operational Insights*, vagy *Naplóelemzési*.

szöveges karakterlánc frissítés túl hello*OMS* szerepel a felügyeleti csomag, amely toobe manuálisan importálni kell. toosee hello aktuális szöveg és a funkciókat, követésével hello hello legújabb System Center Operations Manager frissítés összesítő KB cikk és frissítési hello konzolon.

### <a name="q-is-there-an-on-premises-version-of-log-analytics"></a>K: van-e egy *helyszíni* Naplóelemzési verzióját?

V: nem. A Naplóelemzési dolgozza fel, és nagy mennyiségű adatot tárolja. Felhő alapú szolgáltatásként Log Analytics képes tooscale felfelé szükség, és bármely teljesítmény hatás tooyour környezetben elkerülése érdekében.

További előnyöket nyújtja:
- Microsoft fut hello Naplóelemzési infrastruktúra, így meg lehet spórolni költségek
- A következő szolgáltatás szokásos telepítési frissítések és javítások.

### <a name="q-how-do-i-troubleshoot-that-log-analytics-is-no-longer-collecting-data"></a>Q. Hogyan hibaelhárítása, hogy a Naplóelemzési már nem gyűjt adatokat?

A:, ha az ingyenes tarifacsomag hello és 500 MB-nál több adatok naponta elküldött, adatgyűjtés leállítja hello rest hello nap. Az hello napi korlát gyakori oka, hogy a Naplóelemzési leállítja az adatgyűjtést, vagy az adatok tűnik toobe hiányzik.

A Naplóelemzési hoz létre eseményt, típusú *művelet* amikor adatgyűjtés indítása és leállítása. 

Futtassa a következő keresési toocheck lekérdezést, ha hello napi korlát elérése és adathiányos hello:`Type=Operation OperationCategory="Data Collection Status"`

Amikor adatgyűjtés leáll, hello *OperationStatus* van **figyelmeztetés**. Adatgyűjtés indításakor, hello *OperationStatus* van **sikeres**. 

hello következő táblázatban található az oka, hogy leállítja az adatgyűjtést és a javasolt művelet tooresume adatok gyűjtése:

| OK adatgyűjtés leállítása                       | tooresume adatok gyűjtése |
| -------------------------------------------------- | ----------------  |
| Elérte az ingyenes adatok napi korlátot<sup>1</sup>       | Várja meg, amíg hello követő gyűjtemény tooautomatically újraindításhoz, vagy<br> Változás tooa fizetett tarifacsomag kiválasztása |
| Azure-előfizetés miatt felfüggesztett állapotban van: <br> Ingyenes próbaverzió befejeződött <br> Az Azure hozzáférési lejárt <br> Havi költségkeret érhető el (például a az MSDN webhelyen vagy a Visual Studio előfizetői)                          | Előfizetés a fizetős tooa átalakítása <br> Előfizetés a fizetős tooa átalakítása <br> Távolítsa el a korlátot, vagy várjon, amíg a korlát alaphelyzetbe állítása |

<sup>1</sup> Ha a munkaterületet hello szabad IP-címek, Ön korlátozott toosending 500 MB adatot toohello nap-szolgáltatás esetében. Hello napi korlát elérésekor adatgyűjtés leáll, amíg hello másnap. Adatgyűjtés leállítása küldött adatok nem indexelt, és nem érhető el a kereséshez. Adatok gyűjtése folytatódik, ha csak az új adatok feldolgozása következik be. 

A Naplóelemzési UTC időt használja, és minden nap UTC idő szerint éjfélkor kezdődik. Hello munkaterület eléri hello napi korlátot, ha a feldolgozási folytatja hello első óra hello következő UTC nap során.

### <a name="q-how-can-i-be-notified-when-data-collection-stops"></a>Q. Hogyan tudom értesítést kaphat leállítja az adatok gyűjtését?

V: lépésekkel hello ismertetett [riasztási szabályt létrehozni](log-analytics-alerts-creating.md#create-an-alert-rule) toobe értesítést kap, amikor adatgyűjtés leáll.

Amikor leállítja az adatgyűjtést hello riasztás létrehozásakor állítsa be a:
- **Név** túl*adatgyűjtés leállítása*
- **Súlyossági** túl*figyelmeztetés*
- **Keresési lekérdezés** túl`Type=Operation OperationCategory="Data Collection Status" OperationStatus=Warning`
- **Időablak** túl*2 óra*.
- **Riasztási gyakoriságot** toobe egy órával óta hello használati adatok csak óránként egyszer frissíti.
- **Riasztás alapján** toobe *eredmények száma*
- **Találatok száma** toobe *nagyobb, mint 0*

Ismertetett hello lépésekkel [műveletek tooalert szabályok hozzáadása](log-analytics-alerts-actions.md) egy e-mail, a webhook vagy a runbook műveletet a riasztási szabály hello konfigurálása.


## <a name="configuration"></a>Konfiguráció
### <a name="q-can-i-change-hello-name-of-hello-tableblob-container-used-tooread-from-azure-diagnostics-wad"></a>Q. Módosítható hello tábla/blob tároló használt tooread hello neve az Azure Diagnostics (ÜVEGVATTA)?

A. Nem, nincs jelenleg lehetséges tooread tetszőleges táblák vagy a tárolókat az Azure-tárfiókba.

### <a name="q-what-ip-addresses-does-hello-log-analytics-service-use-how-do-i-ensure-that-my-firewall-only-allows-traffic-toohello-log-analytics-service"></a>Q. IP-címek hello Naplóelemzés szolgáltatás használata? Hogyan ellenőrizze, hogy a tűzfal csak lehetővé teszi, hogy forgalom toohello Naplóelemzés szolgáltatás?

A. Log Analytics szolgáltatás hello Azure épül. Napló Analytics IP-címek szerepelnek hello [Microsoft Azure Datacenter IP-címtartományok](http://www.microsoft.com/download/details.aspx?id=41653).

Mivel szolgáltatástelepítések történik, megváltozik hello Naplóelemzés szolgáltatás hello tényleges IP-címe. hello DNS-nevek tooallow a tűzfalon keresztül leírása a következő [proxy és tűzfal beállításainak konfigurálása a Naplóelemzési](log-analytics-proxy-firewall.md).

### <a name="q-i-use-expressroute-for-connecting-tooazure-does-my-log-analytics-traffic-use-my-expressroute-connection"></a>Q. ExpressRoute tooAzure csatlakozáshoz használni. A Naplóelemzési forgalom használ a saját ExpressRoute-kapcsolatot?

A. ismerteti a különböző típusú hello ExpressRoute forgalom hello [ExpressRoute dokumentációja](../expressroute/expressroute-faqs.md#supported-services).

Forgalom tooLog Analytics hello nyilvános társviszony ExpressRoute-kapcsolatcsoportot használja.

### <a name="q-is-there-a-simple-and-easy-way-toomove-an-existing-log-analytics-workspace-tooanother-log-analytics-workspaceazure-subscription"></a>Q. Van egy egyszerű és egyszerűen elvégezhető toomove egy meglévő Naplóelemzési munkaterület tooanother Naplóelemzési munkaterület/Azure-előfizetést?

A. Hello `Move-AzureRmResource` parancsmag lehetővé teszi, hogy a Naplóelemzési munkaterület, valamint az Automation-fiók áthelyezése egy Azure-előfizetés tooanother. További információkért lásd: [Move-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Ez a változás is létrehozható hello Azure-portálon.

Nem tárolt adatok mozgatása egy Naplóelemzési munkaterület tooanother és Naplóelemzési adatok tárolt hello régió módosítása.

### <a name="q-how-do-i-add-log-analytics-toosystem-center-operations-manager"></a>K: hogyan adja hozzá a napló Analytics tooSystem Center Operations Manager?

V: toohello legújabb kumulatív frissítése és a felügyeleti csomagok importálása teszi tooconnect Operations Manager tooLog elemzés.

>[!NOTE]
>az Operations Manager-kapcsolat tooLog hello Analytics csak érhető el a System Center Operations Manager 2012 SP1 és újabb verziók.

### <a name="q-how-can-i-confirm-that-an-agent-is-able-toocommunicate-with-log-analytics"></a>K: hogyan győződhet meg, hogy egy ügynök nem tudja toocommunicate a Log Analytics?

V: tooensure adott hello ügynök képes kommunikálni OMS-ben, Ugrás: Vezérlőpult, a biztonság és a beállítások, **Microsoft Monitoring Agent**.

A hello **Azure Naplóelemzés (OMS)** lapon, a zöld pipa jelzi. Egy zöld pipa ikon megerősíti, hogy az hello ügynök képes toocommunicate a hello OMS szolgáltatáshoz.

Sárga figyelmeztető ikon azt jelenti, hogy hello ügynök nehézségekkel OMS problémák kommunikál. Egy leggyakoribb oka, hello Microsoft Monitoring Agent szolgáltatás leállt. Service control manager toorestart hello szolgáltatást használja.

### <a name="q-how-do-i-stop-an-agent-from-communicating-with-log-analytics"></a>K: hogyan állítsa le a az ügynök kommunikáljon a Log Analytics?

V: a System Center Operations Manager, távolítsa el a hello számítógép hello Advisor felügyelt számítógép listáról. Az Operations Manager-frissítések hello hello ügynök toono hosszabb jelentés tooLog Analytics konfigurációját. Az ügynökök közvetlenül csatlakoztatott tooLog elemzés, leállíthatja azok keresztül: Vezérlőpult, a biztonság és a beállítások, **Microsoft Monitoring Agent**.
A **Azure Naplóelemzés (OMS)**, távolítsa el a felsorolt összes munkaterületet.

### <a name="q-why-am-i-getting-an-error-when-i-try-toomove-my-workspace-from-one-azure-subscription-tooanother"></a>K: Miért jelenik hiba jelenik meg toomove a saját munkaterületen, egy Azure-előfizetéssel tooanother?

V: Ha hello Azure-portált használja, győződjön meg arról, csak hello munkaterület hello áthelyezés van kiválasztva. Ne válasszon hello megoldások – automatikusan helyre fog Miután hello munkaterületen. 

Ellenőrizze, hogy Ön a két Azure-előfizetések.

## <a name="agent-data"></a>Ügynök adatok
### <a name="q-how-much-data-can-i-send-through-hello-agent-toolog-analytics-is-there-a-maximum-amount-of-data-per-customer"></a>Q. Mennyi adatot lehet küldeni keresztül hello ügynök tooLog Analytics? Van egy / felhasználói adatok maximális mérete?
A. hello csomagra 500 MB-os napi maximális száma munkaterület állítja be. hello standard és premium tervek nincs korlátozva van hello feltöltött adatok mennyiségét. Felhő alapú szolgáltatásként Naplóelemzési úgy van kialakítva, toohandle hello kötet tooautomatically felskálázott érkező ügyfél –, akkor is, ha TB naponta.

hello Naplóelemzési ügynöke (agent) kialakított tooensure kevés erőforrást tartalmaz. Ügyfeleink egyik megírt blog kapcsolatos hello tesztek azok hajt végre a az ügynök, és hogyan impressed voltak. hello adatmennyiség engedélyezi hello megoldások függ. Részletes információk a hello adatmennyiség, és tekintse meg a hello darabolását hello a megoldás [használati](log-analytics-usage.md) lap.

További információkért olvassa a [ügyfél blog](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) kapcsolatos hello alacsony helyigénnyel, hello OMS-ügynököt.

### <a name="q-how-much-network-bandwidth-is-used-by-hello-microsoft-management-agent-mma-when-sending-data-toolog-analytics"></a>Q. Mekkora hálózati sávszélességet hello Microsoft Management Agent (MMA) használják, amikor a küldő adatok tooLog Analytics?

A. A sávszélessége használható funkció hello elküldött adatok mennyisége. A rendszer tömöríti az adatokat, hello hálózaton keresztül küldött.

### <a name="q-how-much-data-is-sent-per-agent"></a>Q. Mennyi adatot küldött ügynök /?

A. egy ügynök elküldött adatok mennyisége hello függ:

* engedélyezte a hello megoldások
* naplók és a szabály a gyűjtendő kérelemteljesítmény-számlálókat hello száma
* az adatok hello naplók hello kötet

hello szabad IP-címek egy jó módszer tooonboard több kiszolgáló, és fel tudja mérni hello tipikus adatmennyiség. Általános felhasználás jelenik meg a hello [használati](log-analytics-usage.md) lap.

Azok a számítógépek, amelyek képesek toorun hello WireData ügynök használja a következő lekérdezés toosee mennyi adatot küldi hello:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Következő lépések
* [Ismerkedés a Naplóelemzési](log-analytics-get-started.md) Naplóelemzési és get percben lépéseivel kapcsolatos további toolearn.
