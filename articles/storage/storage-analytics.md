---
title: "Azure Storage Analytics toocollect naplók és a metrikák adatforgalom aaaUse |} Microsoft Docs"
description: "Tárolási analitika tootrack metrikai adatok az összes tároló-szolgáltatás lehetővé teszi, és a Blob, Queue és Table storage toocollect naplókat."
services: storage
documentationcenter: 
author: robinsh
manager: timlt
editor: tysonn
ms.assetid: 7894993b-ca42-4125-8f17-8f6dfe3dca76
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: robinsh
ms.openlocfilehash: aba1a236cb69fa2e60bcb8fda5d34841e36e379a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storage-analytics"></a>Storage Analytics

A Storage szolgáltatás analitikai funkciói naplózást végeznek, valamint elérhetővé teszik a tárfiókok mérőszámadatait. Használja a tootrace kérelmek, használati trendeket elemzése és a storage-fiók eseményadatokat.

Tárolási analitika toouse, engedélyeznie kell azt egyedileg minden egyes szolgáltatás toomonitor keresi. Engedélyezheti a hello [Azure Portal](https://portal.azure.com). További információkért lásd: [figyelheti a tárfiókokat hello Azure Portal](storage-monitor-storage-account.md). Tárolási analitika programozott módon hello REST API-n keresztül vagy hello ügyféloldali kódtár is engedélyezheti. Használjon hello [Blob szolgáltatás Tulajdonságok beolvasása](https://msdn.microsoft.com/library/hh452239.aspx), [várólista-tulajdonságok beolvasása](https://msdn.microsoft.com/library/hh452243.aspx), [tábla szolgáltatástulajdonságok](https://msdn.microsoft.com/library/hh452238.aspx), és [beolvasása File Service Properties](https://msdn.microsoft.com/library/mt427369.aspx) műveletek tooenable tárolási analitika az egyes szolgáltatásokhoz.

hello összesített adatokat tárolja egy jól ismert blob (naplózás), és jól ismert táblában (a metrika), előfordulhat, hogy elérhetők, hello Blob és Table szolgáltatások API-k használatával.

Tárolási analitika 20 TB maximális hello teljes korlátját a tárfiók független tárolt adatok mennyisége hello rendelkezik. További információ a számlázással és az adatmegőrzési házirendek: [tárolási analitika és számlázási](https://msdn.microsoft.com/library/hh360997.aspx). Információ a tárfiókok korlátairól további információkért lásd: [Azure Storage méretezhetőségi és teljesítménycéloknak](storage-scalability-targets.md).

A részletes útmutatót a tárolási analitika és egyéb eszközök tooidentify, diagnosztizálása, és az Azure tárolással kapcsolatos problémák elhárításához, lásd: [figyelése, diagnosztizálása és elhárítása a Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Tárolási analitika naplózásról
Tárolási analitika sikeres és sikertelen kérelmek tooa storage szolgáltatással kapcsolatos részletes információkat naplózza. Ezeket az információkat csak használt toomonitor egyes kérelmeket és egy társzolgáltatás toodiagnose problémákat. Kérelmek is be vannak jelentkezve a legjobb alapul.

Naplóbejegyzéseket tárolási szolgáltatás tevékenység esetén csak jönnek létre. Például ha a tárfiók tevékenység a Blob szolgáltatás, de nem a tábla- vagy várólista szolgáltatásai, csak a Blob szolgáltatás toohello vonatkozó naplókat létrejön.

Storage Analytics naplózás nem érhető el az Azure File storage.

### <a name="logging-authenticated-requests"></a>Naplózási hitelesített kérelmek
a következő típusú hitelesített kérelmet hello bejelentkezett:

* A sikeres kérelmek száma.
* Sikertelen kérelmek, beleértve az időtúllépés, a sávszélesség-szabályozás, hálózati, engedélyezési és egyéb hibák.
* A kérelem egy közös hozzáférésű Jogosultságkód (SAS), például és a sikertelen kérelmek használatával.
* Kérelmek tooanalytics adatokat.

Tárolási analitika, például a napló létrehozásakor vagy törlésekor, kérelmét a rendszer nem naplózza. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx) és [Storage Analytics naplóformátumban](https://msdn.microsoft.com/library/hh343259.aspx) témaköröket.

### <a name="logging-anonymous-requests"></a>Naplózási névtelen kérésekkel
a következő típusú névtelen kérelmet hello bejelentkezett:

* A sikeres kérelmek száma.
* Server hibákat.
* Az ügyfél és kiszolgáló egyaránt időtúllépést.
* Sikertelen GET kérelmek hibakód 304 (nem módosított).

Minden más sikertelen névtelen kérelmek nem naplózza a rendszer. Hello naplózott adatok teljes listáját a rendszer részletes ismertetését lásd: hello [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx) és [Storage Analytics naplóformátumban](https://msdn.microsoft.com/library/hh343259.aspx) témaköröket.

### <a name="how-logs-are-stored"></a>Naplófájlok tárolási módját
Összes napló tároló $logs, amely automatikusan jön létre, ha tárolási analitika engedélyezve van a tárfiókon nevű blokkblobok vannak tárolva. hello $logs tárolóban található hello blob névtérben hello tárfiók, például: `http://<accountname>.blob.core.windows.net/$logs`. Ebben a tárolóban nem lehet törölni tárolási analitika engedélyezése után abban az esetben, ha tartalmának törlése.

> [!NOTE]
> hello $logs tároló nem akkor látható, ha egy művelet listázása tároló hajtja végre, például a hello [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) metódust. Akkor kell érhető el közvetlenül. Például használhatja hello [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) metódus tooaccess hello blobokat hello `$logs` tároló.
> Kérések naplózása, mint tárolási analitika fel kell töltenie köztes eredményeket, blokkolja. Rendszeres időközönként tárolási analitika ezeket adatblokkokat véglegesítése és blob-ként elérhetővé teszi azokat.
> 
> 

Ismétlődő rekordok hello létrehozott naplók előfordulhat, hogy létezik azonos órához. Meghatározhatja, hogy-e rekord duplikált hello ellenőrzésével **kérelemazonosító** és **művelet** számát.

### <a name="log-naming-conventions"></a>Napló elnevezési konvenciók
A formátum a következő hello egyes naplókon lesz írva.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

hello alábbi táblázat minden egyes attribútumaként hello naplófájl neve.

| Attribútum | Leírás |
| --- | --- |
| < szolgáltatásnév > |hello társzolgáltatás hello neve. Például: blob, table vagy várólista. |
| ÉÉÉÉ |hello négyjegyű év hello napló. Például: 2011. |
| MM |hello kétjegyű hónap hello napló. Például: 07. |
| DD |hello kétjegyű hónap hello napló. Például: 07. |
| hh |hello kétjegyű óra, amely jelzi a hello órával hello naplózza, 24 órás UTC formátumban. Például: 18. |
| mm |hello kétjegyű szám, amely jelzi a hello hello naplók perc indítása. Tárolási analitika hello jelenlegi verziójában nem támogatott ez az érték, és az érték mindig lesz 00. |
| <counter> |A nulla alapú számláló hat számjegynél egy időtartamot órában hello társzolgáltatás előállított napló blobok hello számát jelzi. Ez a számláló a 000000 indul. Például: 000001. |

hello a teljes minta a neve, amely egyesíti az előző példákban hello látható.

    blob/2011/07/31/1800/000001.log

hello az alábbiakban látható egy minta URI-Azonosítóhoz használt tooaccess hello előző naplóban is lehet.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

A tárolási kérés naplózásakor hello eredményül kapott napló neve korrelálja toohello óra, amikor hello kért művelete befejeződött. Például, ha egy GetBlob kérés végre lett hajtva, 6:30-kor: a 7 31/2011 hello napló tartalmazná meg az előtag hello:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Napló metaadatai
Összes naplózási BLOB lehet használt tooidentify milyen naplózási adatok hello blob tartalmaz metaadatot tárolja. a következő táblázat hello minden metaadat-attribútum ismerteti.

| Attribútum | Leírás |
| --- | --- |
| LogType |Ismerteti, hogy hello napló tartalmaz-e tooread, írási vagy törlési műveletek vonatkozó adatokat. Ez az érték egy típus vagy három, vesszővel elválasztva kombinációja tartalmazhatnak. 1. példa: írási; 2. példa: Olvasás, írás; 3. példa: Olvasás, írás, törölje. |
| Kezdő időpont |hello legkorábbi idő egyik bejegyzésének hello napló, a hello formában: éééé-hh-SSz. Például: 2011-07-31T18:21:46Z. |
| Befejezés időpontja |hello legutóbbi időpont egyik bejegyzésének hello napló, a hello formában: éééé-hh-SSz. Például: 2011-07-31T18:22:09Z. |
| LogVersion |hello naplóformátumban hello verzióját. Jelenleg csak a támogatott hello értéke 1,0. |

hello következő listája teljes mintát metaadatok hello előző példákat.

* LogType írási =
* StartTime = 2011-07-31T18:21:46Z
* EndTime = 2011-07-31T18:22:09Z
* LogVersion = 1.0

### <a name="accessing-logging-data"></a>Naplózási adatok elérése
Az összes adat hello `$logs` tároló hello Blob szolgáltatás API-k használatával is elérhetők, beleértve a .NET API-k hello által biztosított hello Azure felügyelt kódtárára. hello tárolási fiók rendszergazdájához olvassa el és törölheti naplókat, de nem létrehozása vagy frissítse azokat. Hello napló metaadatai és a napló nevét is használható az naplók lekérdezésekor. Lehetséges, hogy egy adott óra naplók tooappear sorrendje, de hello metaadatok mindig adja meg a naplóbejegyzések hello timespan egy naplóban. Ezért használható naplófájl nevét és a metaadatok az adott naplók keresésekor.

## <a name="about-storage-analytics-metrics"></a>Tárolási analitika metrikák kapcsolatos
Storage Analytics képes tárolni, amely tartalmazza az összevont tranzakció statisztikák és a kapacitás adatok kérelmek tooa storage szolgáltatással kapcsolatos metrikákat. Tranzakciók jelentett mindkét hello API művelet szinten, továbbá hello tárolási szolgáltatás szintjén, és a kapacitás hello tárolási szolgáltatás szintjén jelenti. Metrikai adatok is használt tooanalyze szolgáltatás tárhelyhasználatot, eseményadatokat az felé irányuló hello társzolgáltatás és tooimprove hello teljesítményét egy szolgáltatást használó alkalmazások által.

Tárolási analitika toouse, engedélyeznie kell azt egyedileg minden egyes szolgáltatás toomonitor keresi. Engedélyezheti a hello [Azure Portal](https://portal.azure.com). További információkért lásd: [figyelheti a tárfiókokat hello Azure Portal](storage-monitor-storage-account.md). Tárolási analitika programozott módon hello REST API-n keresztül vagy hello ügyféloldali kódtár is engedélyezheti. Használjon hello **szolgáltatás Tulajdonságok beolvasása** műveletek tooenable tárolási analitika az egyes szolgáltatásokhoz.

### <a name="transaction-metrics"></a>Tranzakció metrikák
Egy robusztus, az adatok óránkénti vagy perces időközönként minden egyes tárolási szolgáltatás rögzítése, és API műveletet kért, beleértve a be-és kilépési, a rendelkezésre állási, a hibákat, és kategóriába sorolt kérelmek százalékos aránya. Hello tranzakció részleteit hello teljes listáját megtekintheti [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/hh343264.aspx) témakör.

Két szinten – hello szolgáltatási szinteket és hello API művelet szintű tranzakciós adatokat rögzíti. Hello szolgáltatás szintjén összefoglalójához összes statisztika kért API műveletek írt tooa táblaentitássá óránként akkor is, ha a kérelmek nem történt toohello szolgáltatás. Hello API művelet szinten statisztikákat csak írásbeli tooan entitás Ha hello műveletre vonatkozó kérelem érkezett, hogy órán belül.

Ha például egy **GetBlob** a Blob szolgáltatás Storage Analytics Metrics művelet hello kérelem feljegyzi és hello összesített adatokat adja hozzá a mindkét hello Blob szolgáltatás, valamint a hello **GetBlob** műveletet. Azonban ha nem **GetBlob** művelet hello órán belül van szükség, egy entitás nem ír toohello `$MetricsTransactionsBlob` , hogy a művelethez.

Tranzakció metrikák tárolja a felhasználói kérelmek és a tárolási analitika maga kérelmeire. Például tárolási analitika toowrite naplókat, valamint a táblaentitásokat kérések rögzítését. Ezek a kérelmek számlázása hogyan kapcsolatos további információkért lásd: [tárolási analitika és számlázási](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>A kapacitás metrikák
> [!NOTE]
> Teljesítmény-mérőszámait jelenleg csak hello Blob szolgáltatáshoz érhető el. A kapacitás metrikák hello Table szolgáltatás és a Queue szolgáltatás rendelkezésre áll a tárolási analitika jövőbeli verzióiban.
> 
> 

Kapacitástervezési adatok naponta rögzítik egy tárfiók a Blob szolgáltatás, és két tábla entitás készültek. Egy entitás nyújt statisztikai adatokat a felhasználói adatok és más hello biztosít hello statisztikája `$logs` tárolási analitika által használt blob-tároló. Hello `$MetricsCapacityBlob` táblázat tartalmazza a következő statisztikák hello:

* **Kapacitás**: hello tárfiók a Blob szolgáltatás bájtban által használt tárolási hello mennyiségét.
* **ContainerCount**: hello hello tárfiók a Blob szolgáltatás blob tárolók száma.
* **ObjectCount**: hello véglegesítése nem véglegesített blokk vagy a lap blobok és hello tárfiók a Blob szolgáltatás száma.

Hello kapacitási mérőszámokat kapcsolatos további információkért lásd: [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Metrikák tárolási módját
Az egyes hello tárolási szolgáltatások összes metrikai adatok három táblát, hogy a szolgáltatás számára fenntartott tárolja: tranzakció információt egy tábla, egy perc tranzakció információt, egy másik táblázat és kapacitás információt. Tranzakció és percben tranzakció információinak kérelem-válasz adatok áll, és kapacitásadatait tárolási használati adatok áll. Óra metrikákat, percenkénti metrikákat és a tárfiók a Blob szolgáltatás kapacitását elérhető táblák nevű hello a következő táblázatban leírtak szerint.

| Metrikák szint | Tábla neve | Támogatott verziók |
| --- | --- | --- |
| Óránkénti metrika, elsődleges hely |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Verziók előzetes too2013-08-15 csak. Ezeket a neveket továbbra is támogatottak, javasoljuk, hogy alább felsorolt toousing hello táblák vált. |
| Óránkénti metrika, elsődleges hely |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Az összes verzió, beleértve a 2013-08-15. |
| Percenkénti metrikákat, elsődleges hely |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Az összes verzió, beleértve a 2013-08-15. |
| Óránkénti metrika, másodlagos hely |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Az összes verzió, beleértve a 2013-08-15. Írásvédett georedundáns replikáció engedélyezve kell lennie. |
| Percenkénti metrikákat, másodlagos hely |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Az összes verzió, beleértve a 2013-08-15. Írásvédett georedundáns replikáció engedélyezve kell lennie. |
| Kapacitás (csak a Blob szolgáltatás) |$MetricsCapacityBlob |Az összes verzió, beleértve a 2013-08-15. |

Ezek a táblázatok jönnek létre automatikusan tárolási analitika engedélyezve van a tárfiók. Hozzáférhetők keresztül hello névtér hello tárfiók, például:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Metrikai adatok elérése
Hello metrikák táblák összes adatot hello Table szolgáltatás API-k használatával is elérhetők, beleértve a .NET API-k hello által biztosított hello Azure felügyelt kódtárára. hello tárolási fiók rendszergazdájához olvassa el és törölheti táblaentitásokat, de nem létrehozása vagy frissítse azokat.

## <a name="billing-for-storage-analytics"></a>A tárolási analitika számlázási
A tárfiók hello szolgáltatások összes metrikai adatok írja. Ennek eredményeképpen minden egyes tárolási analitika által végrehajtott írási művelet számlázható. Emellett hello metrikai adatok által használt tároló mérete is számlázható.

a következő tárolási analitika végrehajtott műveletekről hello számlázható:

* Kérelmek toocreate blobok naplózásához. 
* Kérelmek toocreate tábla entitások metrikáihoz.

Az adatmegőrzési házirend van beállítva, ha van nem szó, a delete tranzakciók tárolási analitika régi naplózás és a metrikák adatok törlését. Azonban egy ügyfél delete-tranzakciók le számlázható. Adatmegőrzési kapcsolatos további információkért lásd: [Storage Analytics az adatmegőrzési házirend beállítása](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Számlázható kérelmek ismertetése
Minden kérelmet tooan fiók társzolgáltatás számlázható, vagy nem számlázható. Tárolási analitika tooa szolgáltatás, így egy állapotüzenetet, amely jelzi, hogyan hello kérelem kezelése történt meg minden egyes kérelmet naplózza. Tárolási analitika ehhez hasonlóan az adott szolgáltatás, beleértve a hello százalékos és bizonyos állapotüzenetek száma a szolgáltatás és a hello API műveletek metrikáját tárolja. Együtt ezeket a funkciókat segítséget a számlázható kérelmek elemzése, fejlesztései adja meg az alkalmazás a, és eseményadatokat kérelmek tooyour szolgáltatásokkal. Számlázással kapcsolatos további információkért lásd: [az Azure Storage számlázási - sávszélesség, a tranzakciók és a kapacitás](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Ha tárolási analitikai adatok, használhatja a hello hello táblák [Storage Analytics naplózott műveletekkel és az állapotüzenetek](https://msdn.microsoft.com/library/azure/hh343260.aspx) témakör toodetermine mi kérelmek számlázható. Ezután összehasonlíthatja a naplók és a metrikák adatforgalom toohello állapot üzenetek toosee Ha egy adott kérelem volt számítjuk fel. Hello táblák hello előző témakörben tooinvestigate rendelkezésre állási tárolási szolgáltatás vagy egyéni API-művelet is használja.

## <a name="next-steps"></a>Következő lépések
### <a name="setting-up-storage-analytics"></a>Tárolási analitika beállítása
* [A figyelő a tárfiókokat hello Azure portál](storage-monitor-storage-account.md)
* [Engedélyezése és konfigurálása a tárolási analitika](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Storage Analytics naplózás
* [Storage Analytics naplózásról](https://msdn.microsoft.com/library/hh343262.aspx)
* [Storage Analytics naplóformátumban](https://msdn.microsoft.com/library/hh343259.aspx)
* [Tárolási analitika naplózott műveletek és az állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Storage Analytics metrikák
* [Storage Analytics metrikák kapcsolatos](https://msdn.microsoft.com/library/hh343258.aspx)
* [Storage Analytics metrikák táblaséma](https://msdn.microsoft.com/library/hh343264.aspx)
* [Tárolási analitika naplózott műveletek és az állapotüzenetek](https://msdn.microsoft.com/library/hh343260.aspx)  

