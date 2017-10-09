---
title: Media Services Telemetriai aaaAzure |} Microsoft Docs
description: "Ez a cikk áttekintést nyújt az Azure Media Services telemetriai adatokat."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 95c20ec4-c782-4063-8042-b79f95741d28
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: 659e1c947a77aad0e4acacb541d95714da4775ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-telemetry"></a>Az Azure Media Services-Telemetria

Az Azure Media Services (AMS) lehetővé teszi a hozzá tartozó szolgáltatások tooaccess telemetriai/metrikai adatok. AMS jelenlegi verziója hello lehetővé teszi, hogy a működés közbeni telemetriai adatokat gyűjthessen **csatorna**, **StreamingEndpoint**, és működés közbeni **archív** entitásokat. 

Telemetria íródott tooa tárolási tábla megadott Azure Storage-fiók (általában, használja az AMS-fiók társított hello tárfiók). 

hello telemetriai rendszer nem tudja kezelni az adatok megőrzése. Hello tárolási táblák törlésével eltávolíthatja hello régi telemetriai adatokat.

Ez a témakör ismerteti hogyan tooconfigure és hello AMS telemetriai adatok felhasználását.

## <a name="configuring-telemetry"></a>Telemetriai adatok konfigurálása

Telemetria összetevő szintű részletesség konfigurálhatók. Nincsenek két részletességi szintje "Normál" és "Részletes". Jelenleg mindkét szintek vissza hello ugyanazokat az információkat. Ajánlott toouse "normál. 

a következő témakörök megjelenítése hogyan hello tooenable telemetriai:

[A telemetriai adatok .NET engedélyezése](media-services-dotnet-telemetry.md) 

[A telemetriai adatok REST engedélyezése](media-services-rest-telemetry.md)

## <a name="consuming-telemetry-information"></a>Telemetria információk felhasználása

Telemetria tooan Azure Storage tábla íródott hello tárfiókot, amely a Media Services-fiók hello telemetriai konfigurálásakor megadott. Ez a szakasz ismerteti a hello storage-táblákat hello metrikákat.

Használatba vehetné a telemetriai adatokat a következő módokon hello egyikében:

- Olvassa el az adatokat közvetlenül az Azure Table Storage (pl. a hello Storage szolgáltatás SDK használatával). Telemetriai adatok tárolási táblák hello ismertetését lásd: hello **telemetriai adatokat fel** a [ez](https://msdn.microsoft.com/library/mt742089.aspx) témakör.

Vagy

- Használjon hello támogatási hello Media Services .NET SDK tárolási adatok olvasása a [ez](media-services-dotnet-telemetry.md) témakör. 


Az alábbiakban hello telemetriai séma tervezett toogive a megfelelő teljesítmény Azure Table Storage hello határain belül:

- Adatok particionálása és szolgáltatás azonosító fiókkal tooallow telemetriai az egyes service toobe lekérdezett egymástól függetlenül.
- Partíció mindegyike hello dátum toogive hello partícióméret egy ésszerű felső határa.
- Sor kulcsok fordított idő rendelés tooallow hello legutóbbi telemetriai elemek toobe a rendszer megkérdezi a egy adott szolgáltatáshoz.

Ez lehetővé teszi hello közös lekérdezések toobe számos hatékony:

- Párhuzamos, független különböző szolgáltatások adatainak letöltése.
- Egy adott szolgáltatáshoz dátumtartomány az összes adat beolvasása.
- Egy szolgáltatás hello a legfrissebb adatok beolvasása.

### <a name="telemetry-table-storage-output-schema"></a>Kimeneti tárolási telemetriai táblaséma

Egy tábla, hol található az "20160321" a létrehozott hello tábla dátuma "TelemetryMetrics20160321" összesítés telemetriai adatokat tárolja. Telemetria rendszer táblázatot hoz létre külön: 00:00 UTC alapú új naponként. hello táblával toostore ismétlődő értékek, például a betöltési sávszélességű egy adott időszakban idő, a küldött bájtok mennyiségét, stb. 

Tulajdonság|Érték|Példák/megjegyzések
---|---|---
PartitionKey|{szolgáltatásfiók azonosítója} _ {Entitásazonosító}|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66 < br /<br/>fiók azonosító szerepel hello partíciós kulcs toosimplify munkafolyamatok ahol több Media Services-fiók toohello írás hello ugyanazt a tárfiókot.
RowKey|{másodperc toomidnight} _ {véletlenszerű értéket}|01688_00199<br/><br/>hello sorkulcsa másodperc toomidnight tooallow felső n stílus lekérdezések a partíción belül hello száma kezdődik. További információkért lásd: [ez](../cosmos-db/table-storage-design-guide.md#log-tail-pattern) cikk. 
időbélyeg|Dátum és idő|Automatikus hello Azure-tábla 2016 az időbélyeg-09-09T22:43:42.241Z
Típus|telemetriai adatokat szolgáltató hello entitás hello típusa|Csatorna/StreamingEndpoint/archív<br/><br/>Esemény típusa csak karakterlánc-értéke.
Név|hello telemetriai esemény hello neve|ChannelHeartbeat/StreamingEndpointRequestLog
ObservedTime|hello hello telemetriai esemény történt (idő szerint UTC)|2016-09-09T22:42:36.924Z<br/><br/>hello megfigyelt idő hello entitás küldő hello telemetriai adatokat (például egy csatorna) biztosítja. Nem határozható meg, ez az érték összetevői közötti szinkronizálás problémák megközelítőleges időpontja
ServiceID|{felügyeletiszolgáltatás-azonosító}|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Entitás jellemző tulajdonságok|Hello esemény által meghatározott módon|StreamName: stream1, sávszélességű 10123...<br/><br/>hello többi tulajdonság az adott esemény típusú hello vannak definiálva. Az Azure tábla tartalma kulcs-érték párokat.  (Ez azt jelenti, hogy hello tábla különböző soraira különböző tulajdonságkészletekkel rendelkező tulajdonságai).

### <a name="entity-specific-schema"></a>Entitás-specifikus séma

Minden egyes topológiájával együtt hello gyakorisága a következő entitásra vonatkozó távmérést adatokat bejegyzések három típusa van:

- Adatfolyam-végpontok: minden 30 másodperc
- Az élő csatornák: percenként
- Archív Live: percenként

**A Streamvégpont**

Tulajdonság|Érték|Példák
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
időbélyeg|időbélyeg|Automatikus Azure tábla 2016 időbélyeg-09-09T22:43:42.241Z
Típus|Típus|StreamingEndpoint
Név|Név|StreamingEndpointRequestLog
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Felügyeletiszolgáltatás-azonosító|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
Állomásnév|Hello végpont állomásneve|builddemoserver.origin.mediaservices.Windows.NET
statusCode|Rögzíti a HTTP-állapot|200
ResultCode|Eredmény kódja részletei|ÉRTÉKKEL
RequestCount|Hello összesítésbe teljes kérelem|3
BytesSent|Küldött bájtok összesített száma|2987358
ServerLatency|Átlagos kiszolgáló késleltetése (beleértve a tároló)|129
E2ELatency|Átlagos végpontok közötti késés|250

**Élő csatorna**

Tulajdonság|Érték|Példák/megjegyzések
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
időbélyeg|időbélyeg|Automatikus hello Azure-tábla 2016 az időbélyeg-09-09T22:43:42.241Z
Típus|Típus|Csatorna
Név|Név|ChannelHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Felügyeletiszolgáltatás-azonosító|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
TrackType|Írja be a videó/hang/szöveg nyomon követése|videó/hang
TrackName|Hello követése neve|videó/audio_1
Átviteli sebesség|Track sávszélességű|785000
CustomAttributes||   
IncomingBitrate|Tényleges bejövő sávszélességű|784548
OverlapCount|Átfedésben lévő hello betöltési|0
DiscontinuityCount|A track folytonosságában|0
LastTimestamp|Utolsó feldolgozott adatokat időbélyeg|1800488800
NonincreasingCount|Időbélyeg toonon növekvő miatt elvetett töredék száma|2
UnalignedKeyFrames|E fragment(s) érkezett (különböző szolgáltatásminőségi szinteket) ahol kulcs nincsenek egyeztetve keretek |True (Igaz)
UnalignedPresentationTime|E megkaptuk fragment(s) (egyes szolgáltatásminőségi szinteket /), ahol nem illeszkednek a bemutató idő|True (Igaz)
UnexpectedBitrate|Igaz Ha a hang-és videófolyamot számított/tényleges sávszélességű > 40 000 bps és IncomingBitrate nyomon == vagy IncomingBitrate és actualBitrate eltérő 50 % 0 |True (Igaz)
Kifogástalan|IGAZ, ha <br/>overlapCount, <br/>DiscontinuityCount, <br/>NonIncreasingCount, <br/>UnalignedKeyFrames, <br/>UnalignedPresentationTime, <br/>UnexpectedBitrate<br/> a rendszer minden 0|True (Igaz)<br/><br/>Kifogástalan hamis értéket, ha a következő feltételek fenntartási hello bármelyikét összetett függvény van:<br/><br/>-OverlapCount > 0<br/>-DiscontinuityCount > 0<br/>-NonincreasingCount > 0<br/>-UnalignedKeyFrames == True<br/>-UnalignedPresentationTime == True<br/>-UnexpectedBitrate == True

**Élő archív**

Tulajdonság|Érték|Példák/megjegyzések
---|---|---
PartitionKey|PartitionKey|e49bef329c29495f9b9570989682069d_64435281c50a4dd8ab7011cb0f4cdf66
RowKey|RowKey|01688_00199
időbélyeg|időbélyeg|Automatikus hello Azure-tábla 2016 az időbélyeg-09-09T22:43:42.241Z
Típus|Típus|Archívum
Név|Név|ArchiveHeartbeat
ObservedTime|ObservedTime|2016-09-09T22:42:36.924Z
ServiceID|Felügyeletiszolgáltatás-azonosító|f70bd731-691d-41c6-8f2d-671d0bdc9c7e
ManifestName|Program URL-címe|asset-eb149703-ed0a-483c-91c4-e4066e72cce3/a0a5cfbf-71ec-4BD2-8c01-a92a2b38c9ba.ISM
TrackName|Hello követése neve|audio_1
TrackType|Hello követése típusa|Hang-és videófolyamot
CustomAttribute|Hexadecimális karakterlánc, amely állapottípust határoz meg különböző nyomon követése, azonos nevű és átviteli sebesség (multi kamera szög)|
Átviteli sebesség|Track sávszélességű|785000
Kifogástalan|IGAZ, ha FragmentDiscardedCount == 0 & & ArchiveAcquisitionError == False|Igaz (e két érték nem találhatók a hello metrika, de azok szerepelnek a hello forrás esemény)<br/><br/>Kifogástalan hamis értéket, ha a következő feltételek fenntartási hello bármelyikét összetett függvény van:<br/><br/>-FragmentDiscardedCount > 0<br/>-ArchiveAcquisitionError == True

## <a name="general-qa"></a>Általános kérdések és válaszok

### <a name="how-tooconsume-metrics-data"></a>Hogyan tooconsume metrikai adatok?

Egy Azure-táblákban sorozatát hello ügyfél tárfiókjával metrikai adatok tárolja. Ezeket az adatokat képes használni a következő eszközök hello használata:

- AMS SDK
- A Microsoft Azure Tártallózó (támogatja a toocomma tagolt az exportálási formátumát, és a feldolgozott Excel)
- REST API

### <a name="how-toofind-average-bandwidth-consumption"></a>Hogyan toofind átlagos sávszélesség-használat?

hello átlagos sávszélesség-felhasználást az időtartam, a hello BytesSent átlagát.

### <a name="how-toodefine-streaming-unit-count"></a>Hogyan toodefine streamelési egységet száma?

adatfolyam-egységek száma hello hello maximális átviteli sebesség a hello szolgáltatás streamvégpontok hello maximális átviteli sebesség egy adatfolyam-továbbítási végpontjának osztva adható meg. hello maximális átviteli használható egy adatfolyam-továbbítási végpontjának 160 MB/s.
Tegyük fel, hogy az ügyfél szolgáltatásból hello maximális átviteli sebesség 40 MB/s (hello maximális értékének BytesSent alatt az időtartam,). Ezután adatfolyam-egységek száma hello egyenlő too(40 MBps) * (8 Bit/bájt) /(160 Mbps) = 2 adatfolyam-továbbítási egység.

### <a name="how-toofind-average-requestssecond"></a>Hogyan toofind átlagos kérelmek/másodperc?

toofind hello átlagos száma kérelmek/másodperc, a számítási hello (RequestCount) kérelmek átlagos száma az időtartam alatt.

### <a name="how-toodefine-channel-health"></a>Hogyan toodefine csatorna állapotát?

Csatorna állapotfigyelő logikai függvény összetett lehet meghatározni, hogy legyen hamis tartsa hello a következő feltételek bármelyike:

- OverlapCount > 0
- DiscontinuityCount > 0
- NonincreasingCount > 0
- UnalignedKeyFrames == True 
- UnalignedPresentationTime == True 
- UnexpectedBitrate == True


### <a name="how-toodetect-discontinuities"></a>Hogyan toodetect folytonosság megszakítását?

a folytonosság toodetect megszakítását található összes csatorna adatok bejegyzés ahol DiscontinuityCount > 0. hello megfelelő ObservedTime időbélyeg hello alkalommal hello folytonosság megszakítását létrejöttének időpontját jelzi.

### <a name="how-toodetect-timestamp-overlaps"></a>Hogyan toodetect időbélyeg átfedésben van?

toodetect időbélyeg átfedéseket, található összes csatorna adatok bejegyzés ahol OverlapCount > 0. hello megfelelő ObservedTime timestamp, mely hello időbélyeg átfedi hányszor történt hello jelzi.

### <a name="how-toofind-streaming-request-failures-and-reasons"></a>Hogyan toofind streaming kérelem hibákat és azokat az oka?

toofind adatfolyam-továbbítási kérelem hibák és okokból található összes Streamvégponton adatok bejegyzés ResultCode esetén nem egyenlő tooS_OK. hello megfelelő StatusCode mező azt jelzi, hogy hello hello kérelem sikertelenségének okát.

### <a name="how-tooconsume-data-with-external-tools"></a>Hogyan tooconsume adatok külső eszközöket?

Távmérést adatok feldolgozása, illetve a következő eszközök hello formájában jelenik meg:

- PowerBI
- Application Insights
- Az Azure Monitor (korábbi nevén Shoebox)
- AMS élő irányítópult
- Azure-portálon (függőben lévő kiadás)

### <a name="how-toomanage-data-retention"></a>Hogyan toomanage az adatmegőrzés?

hello telemetriai rendszer nem adja meg megőrzési adatkezelés vagy automatikus törlési régi rögzíti. Ebből kifolyólag toomanage kell, és törölje manuálisan a régi rekordok hello tárolási táblából. Olvassa el a toostorage SDK arról, hogyan toodo azt.

## <a name="next-steps"></a>Következő lépések

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése

[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
