---
title: "aaaLog Analytics Adatbiztonság |} Microsoft Docs"
description: "Információ hogyan Naplóelemzési adatvédelmi és titkosítja az adatokat."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: a33bb05d-b310-4f2c-8f76-f627e600c8e7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/03/2017
ms.author: magoedte
ms.openlocfilehash: 130b59f22fc3dd249f32717367cc62ea25c55a21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-data-security"></a>Naplófájl Analytics adatok biztonsága
A Microsoft véglegesített tooprotecting az adatok védelmére és biztonságossá tétele az adatokat, miközben nagy szoftverek és -szolgáltatások, amelyek segítségével kezelheti a szervezet informatikai infrastruktúrájának hello. Felismertük, hogy megbízhat az adatok tooothers, amikor adott megbízhatósági szigorú biztonsági igényel. A Microsoft megfelelő toostrict megfelelőségi és biztonsági irányelvek – toooperating egy szolgáltatás-ból.

Biztonságossá tétele és az adatok védelmének a Microsoft a legnagyobb prioritással. Kapcsolatfelvétel az esetleges kérdéseivel, a javaslatok és a problémák kapcsolatos összes információt, beleértve a biztonsági házirendek, a következő hello [az Azure támogatási lehetőségek](http://azure.microsoft.com/support/options/).

Ez a cikk azt ismerteti, hogyan adatok gyűjtése, feldolgozása, és az Operations Management Suite (OMS) hello Naplóelemzési által védett. Ügynökök tooconnect toohello webszolgáltatás, használja a System Center Operations Manager toocollect működési adatokat, vagy adatainak lekérése az Azure diagnostics Naplóelemzési általi használatra. hello összegyűjtött adatok továbbítása hello Internet tanúsítvány alapú hitelesítést & SSL 3 toohello Naplóelemzés szolgáltatás használatával, amely egy Microsoft Azure-ban. Adatok tömörítése hello ügynök elküldés előtt.

hello Naplóelemzés szolgáltatás a felhőalapú adatokat biztonságosan kezelő hello a következő módszerek használatával:

* Az adatok elkülönítése
* az adatok megőrzése
* Fizikai biztonság
* Incidenskezelés
* Megfelelőségi
* biztonsági szabványok tanúsítványok

## <a name="data-segregation"></a>Az adatok elkülönítése
Ügyféladatok tartják logikailag külön minden egyes összetevő teljes hello OMS szolgáltatáshoz. Az összes adat szervezet szerint van megcímkézve. A címkézés továbbra is fennáll, hello adatok életciklus során, és van kényszerítve hello szolgáltatást minden egyes rétegben. Minden egyes ügyfélnek van dedikált Azure blob, amely hello hosszú távú adatok

## <a name="data-retention"></a>Adatmegőrzés
Indexelt naplóadatokat keresési tárolja, és a terv árképzési függően tooyour maradnak. További információkért lásd: [napló Analytics Díjszabásáról](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft 30 nap hello OMS-munkaterület bezárása után törli az ügyféladatokat. A Microsoft hello hello adatokat tároló Azure Storage-fiókot is törli. Felhasználói adatok eltávolítása után nem fizikai meghajtók megsemmisülnek.

hello a következő táblázatban soroljuk hello elérhető megoldásokra az OMS Szolgáltatáshoz, és példák hello gyűjtenek adatokat.

| **Megoldás** | **Adattípusok** |
| --- | --- |
| Konfiguráció felmérése |Konfigurációs adatok, a metaadatok és a rendszerállapot-adatok |
| Kapacitástervezés |Teljesítményadatok és metaadatok |
| Kártevőirtó |Konfigurációs adatok és metaadatok |
| Rendszerfrissítési felmérés |Metaadat-és állapot |
| Naplókezelés |Felhasználó által definiált eseménynaplók, Windows-Eseménynapló és/vagy az IIS-napló |
| Változások követése |A szoftverleltár és a Windows szolgáltatás metaadatai |
| Az SQL és az Active Directory értékelése |WMI-adatok, a beállításjegyzék-adatok, a teljesítményadatokat és az SQL Server dinamikus felügyeleti eredmények megtekintése |

a következő táblázat hello adattípusok példákat mutat:

| **Adattípus** | **Mezők** |
| --- | --- |
| Riasztás |Riasztás neve riasztás leírása, BaseManagedEntityId, probléma azonosítója, IsMonitorAlert, RuleId, ResolutionState, prioritás, súlyosság, kategória, tulajdonos, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfiguráció |CustomerID, ügynökazonosító, entityid beállítást, ManagedTypeID, ManagedTypePropertyID, CurrentValue, ChangeDate |
| Esemény |Eseményazonosító, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, számot, kategória, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Megjegyzés:** írásakor események egyéni mezőket tartalmazó toohello Windows eseménynaplóban, OMS gyűjti azokat. |
| Metaadatok |BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, IP-cím, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, az IP- Cím NetbiosDomainName, LogicalProcessors, DNSName, DisplayName, DomainDnsName, ActiveDirectorySite, egyszerű név, OffsetInMinuteFromGreenwichTime |
| Teljesítmény |ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Állapot |StateChangeEventId, StateId, NewHealthState, OldHealthState, a környezetben, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, monitorid attribútumként, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Fizikai biztonság
hello Naplóelemzési az OMS szolgáltatáshoz van működtetett Microsoft személyzete és az összes tevékenység naplózza, és naplózhatók. hello szolgáltatás teljesen lefut az Azure-ban, és megfelel hello Azure közös mérnöki feltételeket. Megtekintheti a fizikai biztonsági hello Azure eszközök adatait hello 18 oldalán [Microsoft Azure biztonsági áttekintése](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf). Fizikai hozzáférési jogok toosecure területek bárki, aki már nem rendelkezik a hello OMS szolgáltatás, így az átvitel és a megszakítási felelősséget módosítja egy munkanapon belül. Hello globális fizikai infrastruktúra használjuk, olvashat [Microsoft Datacenters](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Incidenskezelés
OMS rendelkezik egy incidenskezelési folyamat, amely az összes Microsoft-szolgáltatás igazodik. toosummarize, azt:

* Használjon egy megosztott felelősségi modell, ahol egy biztonsági felelős része tartozik tooMicrosoft és egy részét toohello ügyfél tartozik
* Az Azure biztonsági incidensek kezeléséhez
  * Indítsa el a vizsgálatot esemény észlelése
  * Mérje fel hello hatás és a súlyosság egy incidens által egy a készenléti incidensválasz-csoport tagja. Bizonyító adatok alapján, hello assessment is, vagy előfordulhat, hogy nem eredményez további eszkalációs toohello biztonsági csoportnak.
  * Incidens diagnosztizálása biztonsági válasz szakértők tooconduct hello technikai vagy igazságügyi elemzésekhez lehet vizsgálat, elszigetelési, a megoldás és a megoldás stratégiák meghatározása. Ha hello biztonsági csoportja úgy véli, hogy ügyféladatokat vált elérhetővé tooan törvénybe ütköző vagy jogosulatlan hello ügyfél incidens értesítési folyamat egyes, párhuzamos végrehajtása párhuzamosan kezdődik.  
  * Stabilizálását és hello incidens helyreállíthatók. hello incidensválasz-csapat létrehoz egy helyreállítási terv toomitigate hello probléma. Válság elszigetelési lépéseket karanténba helyezés érintett rendszerek például akkor fordulhat elő, azonnal, és diagnosztikai párhuzamosan. Hosszabb távú megoldást is lehet tervezett bekövetkező hello azonnali kockázat letelte után.  
  * Zárja be a hello incidens, és végezze el a levágást. hello incidensválasz-csapat hoz létre egy levágást, amelyik felvázolja hello hello részleteit incidens, hello szándékát toorevise házirendek, eljárások és folyamatok tooprevent hello esemény ismétlődése.
* A biztonsági események ügyfelek értesítése
  * Érintett felhasználók és tooprovide hello hatókörének meghatározásához birtokában bárki, aki egy értesítést a lehető legrészletesebb kihatással van
  * Hozzon létre egy értesítés tooprovide ügyfelek elég részletes információkat, hogy vizsgálatot végez a végfelhasználók és bármely tootheir végfelhasználók számára nem jogosulatlanul késleltetése hello értesítési folyamat során tett kötelezettségvállalások teljesítésére.
  * Erősítse meg, majd deklarálja hello incidens, szükség szerint.
  * Értesítse az incidens értesítések ésszerűtlenül haladéktalanul és jogi vagy szerződéses kötelezettségeket rendelkező ügyfelek. A biztonsági események értesítések érkeznek tooone vagy több ügyfél rendszergazdák bármilyen módon választják ki a Microsoft, beleértve az e-mailben.
* Végezze el a csapat készültségi és képzése
  * Microsoft személyzete szükséges toocomplete biztonsági és tájékoztatási képzési, amely segít tooidentify és biztonsági problémákat gyanús jelentés.  
  * A Microsoft Azure service hello használata operátorok hozzáadása képzési kötelezettségek hozzáférés toosensitive rendszereik ügyféladatok üzemeltető körülvevő rendelkeznek.
  * A Microsoft biztonsági válasz személyzet speciális képzésben a szerepkörükhöz

A felhasználói adatok elvesztése esetén értesítést mindegyik ügyfél egy napon belül. Azonban ügyfél soha nem történt adatvesztés az OMS Szolgáltatáshoz. Ezenkívül azt másolatait a létrehozott adatok, vagy földrajzilag elosztott van.

Hogyan reagál a Microsoft a toosecurity incidensek kapcsolatos további információkért lásd: [Microsoft Azure biztonsági válasz hello felhő](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in hello cloud.pdf).

## <a name="compliance"></a>Megfelelőség
hello OMS szoftverek fejlesztési és szolgáltatási csoport információ-biztonság és a cégirányítási program támogatja az üzleti követelmények és toolaws és a szabályozásoknak megfelelő, részben ismertetett módon [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) és [Microsoft adatvédelmi központ megfelelőségi](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Hogyan OMS biztonsági követelményeket állapítja, biztonsági vezérlők azonosítja, kezeli, és figyeli a kockázatok is létezik ismerteti. Évente, azt felülvizsgálati házirendeket, szabványokat, eljárásokra és útmutatást.

Minden egyes OMS fejlesztési csoport egy tagja megkapja a formális alkalmazás biztonsági képzés. A verziókezelő rendszer belsőleg, szoftverek fejlesztésére használjuk. Minden szoftver projekt hello verziókezelő rendszer által védett.

A Microsoft hogyan felügyeli, és értékeli az összes szolgáltatás a Microsoft biztonsági és megfelelőségi csoport rendelkezik. Biztonsági informatikusok hello team alkotják, és nincs társítva a műszaki osztály, amely a fejlesztés OMS részlegek hello. hello biztonsági tisztviselő saját felügyeleti lánc rendelkezik, és végezze el a termékek és szolgáltatások tooensure biztonsági és megfelelőségi független értékelése.

A Microsoft board vezetésében kapcsolatos összes információ biztonsági programok Microsoft éves jelentést értesíti.

hello OMS szoftverek fejlesztési és szolgáltatás team aktívan hello Microsoft Legal és megfelelőségi csoportok és egyéb iparági partnerekkel tooacquire különböző minősítései közül.

## <a name="certifications-and-attestations"></a>Tanúsítványok és tanúsítványok
OMS Naplóelemzési megfelel a követelményeknek hello:

* [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm)
* [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498)
* [ISO 22301](https://azure.microsoft.com/en-us/blog/iso22301/)
* [Payment Card Industry (PCI-kompatibilis) adatvédelmi szabvány (PCI DSS)](https://www.microsoft.com/en-us/TrustCenter/Compliance/PCI) hello PCI biztonsági szabványok Tanács által.
* [Szolgáltatás szervezet vezérlők (SOC) 1 1 és SOC 2 1-es típusú](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) megfelelő
* [HIPAA és HITECH](https://www.microsoft.com/en-us/TrustCenter/Compliance/HIPAA) vállalatok számára megállapodással rendelkező HIPAA üzleti társítása
* A Windows gyakori mérnöki feltételek
* Microsoft Trustworthy Computing
* Az Azure-szolgáltatások, mint az OMS használó hello összetevők igazodik tooAzure megfelelőségi követelmények. A további [Microsoft Megbízhatósági központ megfelelőségi](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).

> [!NOTE]
> Az egyes tanúsítványok/tanúsítványokat, Log Analyticshez megtalálható-e a korábbi nevét *Operational Insights*.
>
>


## <a name="cloud-computing-security-data-flow"></a>A felhőalapú informatika biztonsági adatfolyama
hello alábbi ábrán egy biztonsági architektúra hello irányuló információáramlást, a vállalat, és hogyan, titkosítása áthelyezése toohello Naplóelemzés szolgáltatás Ön által az OMS-portálon hello végső soron látható. További információ az egyes lépések hello diagram követi.

![OMS adatok összegyűjtése és védelme képe](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. A Naplóelemzési és adatokat gyűjthet a Regisztrálás
A szervezet toosend adatok tooLog elemzés Windows-ügynökök, Azure virtuális gépeken, vagy az OMS-ügynököt a Linux operációs rendszert futtató ügynökökön kell konfigurálni. Ha az Operations Manager-ügynökök, akkor a konfiguráció varázsló segítségével hello operatív konzol tooconfigure őket. Felhasználók (ami lehet, hogy Ön, más felhasználókat vagy egy csoport tagjainak) hozzon létre egy vagy több OMS fiókokat (OMS-munkaterület), és ügynökök regisztrálni a következő fiókok hello egyikének használatával:

* [Szervezeti Azonosítóval](../active-directory/sign-up-organization.md)
* [Microsoft-fiók – Outlook-Office Live, az MSN](http://www.microsoft.com/account/default.aspx)

Az OMS-munkaterület, ahol adatokat gyűjti, összesítve, elemzése, és jelenik meg. A munkaterület elsősorban a azt jelenti, hogy toopartition adatokat, és egyes munkaterületeken egyedi. Például érdemes a termelési adatok használatával egy másik munkaterület OMS-munkaterület és a vizsgálati adatok kezelt felügyelt toohave. A munkaterületek között is hozzájárulhat a egy rendszergazda vezérlő felhasználói hozzáférés toohello adatai. Minden felhasználói fiók hozzáférhessen a több OMS-munkaterület, és egyes munkaterületeken rendelkezhet több felhasználói fiókhoz társítva. A munkaterületek datacenter régió alapján hoz létre. Egyes munkaterületeken replikált tooother adatközpontok hello régióban, amely elsősorban az OMS-szolgáltatás rendelkezésre állása.

Az Operations Manager hello konfigurációs varázsló befejezése után minden egyes Operations Manager felügyeleti csoport kapcsolatot létesít hello Naplóelemzés szolgáltatás. Ezt követően az hello számítógépek hozzáadása varázsló toochoose mely hello felügyeleti csoportban lévő számítógépekhez toosend toohello adatszolgáltatás számára engedélyezett. A más ügynök típusú csatlakozik biztonságosan toohello OMS szolgáltatáshoz.

Csatlakoztatott és a hello Naplóelemzés szolgáltatás közötti összes kommunikáció titkosítva van.  hello TLS (HTTPS) protokollt használják a titkosítás.  Microsoft SDL-folyamat hello követi tooensure Naplóelemzési kerüljön hello legutóbbi fejlett titkosítási protokollokat.

Minden ügynök típusú adatokat gyűjt a Naplóelemzési. hello a gyűjtött adatok típus használt megoldások hello típusú függ. Adatok gyűjtése: összefoglalása látható [hozzáadni a Naplóelemzési megoldásokat az hello megoldások gyűjtemény](log-analytics-add-solutions.md). Ezenkívül részletesebb gyűjtemény áll rendelkezésre információ a legtöbb megoldások. A megoldás egy előre meghatározott nézeteket, a naplófájl-keresési lekérdezések, az adatok gyűjtési szabályok és a feldolgozó logika kötegelt. Csak rendszergazdák használhatják a Naplóelemzési tooimport megoldást. Hello megoldás az importálása után is áthelyezett toohello Operations Manager felügyeleti kiszolgálón (ha van), majd a választott tooany ügynökök. A későbbiekben hello ügynökök hello adatainak gyűjtéséről.

## <a name="2-send-data-from-agents"></a>2. Adatküldés az ügynökök
Minden ügynök típusok regisztrálása egy regisztrációs kulcsot, és hello ügynök és a tanúsítvány alapú hitelesítést és az SSL használata a 443-as porton hello Naplóelemzés szolgáltatás közötti biztonságos kapcsolatot létesít. OMS egy tároló titkos toogenerate használ, és a kulcsok karbantartása. A titkos kulcsok legyenek-e elforgatva 90 naponta, és az Azure-ban tároljuk, és hello Azure által kezelt szigorú szabályozási és megfelelőségi vonatkozásairól követő műveleteket.

Az Operations Manager munkaterület regisztrálása hello Log Analytics szolgáltatásban, és egy biztonságos HTTPS-kapcsolat létrehozása hello Operations Manager felügyeleti kiszolgáló között.

Az Azure virtuális gépeken futó Windows-ügynökök a csak olvasható tároló kulcsa használt tooread diagnosztikai események Azure-táblákban.

Ha minden ügynök bármilyen okból nem toocommunicate toohello szolgáltatás, hello összegyűjtött adatok helyben tárolódnak a ideiglenes gyorsítótár, és megpróbál tooresend hello adatok két órán át nyolc percenként hello felügyeleti kiszolgáló. a gyorsítótárazott adatokat hello ügynök hello operációs rendszer tanúsítványtároló védi. Hello szolgáltatás nem tudja feldolgozni a hello adatok két órával később, ha a hello ügynökök hello adatok várólistára kerülnek. Ha hello várólista megtelik, OMS elindul, adattípusok, teljesítményadatokat kezdve eldobását. hello ügynök várólistára vonatkozó korlátozást egy beállításkulcsot, így módosítható, ha szükséges. Összegyűjtött adatok tömörített és toohello szolgáltatás, helyszíni adatbázisokhoz, kihagyásával, ezért nem adható hozzá bármilyen terhelés toothem küldött. Hello összegyűjtését követően adatokat küldi el, a rendszer eltávolítja hello gyorsítótár.

A fent leírtaknak megfelelően a ügynököktől származó adatok továbbítása SSL tooMicrosoft Azure adatközpontjaiban. Másik lehetőségként hello adatok ExpressRoute tooprovide további biztonsági is használhatja. ExpressRoute egy módon toodirectly tooAzure csatlakozzon a meglévő WAN hálózaton, például a többprotokollos átváltását (MPLS) VPN-profilok, a szolgáltató által megadott címkével. További információkért lásd: [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

## <a name="3-hello-log-analytics-service-receives-and-processes-data"></a>3. hello Naplóelemzés szolgáltatás fogad és dolgoz fel adatokat
hello Naplóelemzés szolgáltatás biztosítja, hogy a bejövő adatok megbízható forrásból származó érvényesítésével azonosítsa a tanúsítványok és az Azure authentication hello adatok integritását. hello feldolgozatlan nyers adatok majd tárolja a blob [Microsoft Azure Storage](../storage/common/storage-introduction.md) és nem titkosított. Azonban minden Azure storage-blob tartozik egy kulcsok, egyedi készletének elérhető csak toothat felhasználó. tárolt adatok hello típusú lett importálva, és a használt toocollect adatok megoldások hello típusú függ. Ezt követően hello Naplóelemzés szolgáltatás dolgozza fel a nyers adatok hello hello Azure storage-blob.

## <a name="4-use-log-analytics-tooaccess-hello-data"></a>4. A Naplóelemzési tooaccess hello adatok használata
Az OMS-portálon hello Analytics tooLog bejelentkezhet hello szervezeti fiók vagy a korábban állítsa be Microsoft-fiók használatával. Hello OMS-portálon, és az OMS szolgáltatáshoz közötti összes forgalom HTTPS biztonságos csatornán keresztül zajlik. Ha hello OMS-portálon, a munkamenet-azonosító (webböngésző) hello felhasználói ügyfélen jön létre, és adatai a helyi gyorsítótárba hello munkamenet végéig. Ha leállt, hello gyorsítótár törlődik. Ügyféloldali cookie-kat, amelyek nem tartalmaznak személyes azonosításra alkalmas adatokat, nem lesznek automatikusan eltávolítva. Munkamenet-cookie-k HTTPOnly megjelölve, és biztosított. Egy előre meghatározott tétlen időszak után hello OMS-portálon munkamenet megszakítása.

Hello OMS-portálon használ, exportálhatja adatok tooa CSV-fájlt, és a keresési API-k segítségével az adatok eléréséhez. Exportálás CSV korlátozott too50, az exportálás és API-adatok 000 sorszám korlátozott too5, keresési 000 sorszám.

## <a name="next-steps"></a>Következő lépések
* [Ismerkedés a Naplóelemzési](log-analytics-get-started.md) Naplóelemzési és get percben lépéseivel kapcsolatos további toolearn.
