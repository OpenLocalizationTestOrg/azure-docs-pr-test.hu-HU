---
title: "az Azure Media Services segítségével élő adatfolyam aaaOverview |} Microsoft Docs"
description: "Ez a témakör áttekintést nyújt az Azure Media Services segítségével élő adatfolyam."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: fb63502e-914d-4c1f-853c-4a7831bb08e8
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 06/29/2017
ms.author: juliako
ms.openlocfilehash: edc49069db6b491902bdcbb808b1974858cc92f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-live-streaming-using-azure-media-services"></a>Élő adatfolyam-továbbítási az Azure Media Services áttekintése
## <a name="overview"></a>Áttekintés
Meghatározott élő adatfolyam-eseményeket az Azure Media Services a következő összetevők hello általában érintett:

* Egy kamera, amely használt toobroadcast egy eseményt.
* Egy élő videókódoló, amely átalakítja a jeleket a hello kamera toostreams tooa küldött élő adatfolyam-szolgáltatásra.

    Esetlegesen több, szinkronizált idejű élő kódoló. Az egyes kritikus helyezkednek el, hogy igény szerint magas rendelkezésre állást és minőséget, ajánlott tooemploy aktív-aktív redundáns kódolók idő szinkronizálási tooachieve zökkenőmentes feladatátvétel adatvesztés nélküli események.
* Egy élő adatfolyam-továbbítási szolgáltatás, amely lehetővé teszi a következő toodo hello:

  * élő tartalmak feldolgozása különböző élő adatfolyam-továbbítási protokollok használatával (például RTMP vagy Smooth Streaming);
  * az adatfolyam adaptív sávszélességűvé kódolása (opcionális)
  * az élő adatfolyam-továbbítás előnézete
  * rendelés toobe rekord és a tároló okozhatnak hello tartalma folyamatos átviteli újabb (Video-on-Demand)
  * hello tartalom gyakori adatfolyam-továbbítási protokollok (például MPEG DASH, Smooth, HLS) keresztül közvetlenül tooyour ügyfelek vagy a Content Delivery Network (CDN) tooa későbbi terjesztés.

**A Microsoft Azure Media Services** (AMS) képességét tooingest hello, kódolására, előzetes, tárolására és az élő adatfolyam-továbbítási tartalom biztosít.

Ha a tartalom toocustomers kézbesíti a cél van egy kiváló minőségű videó toovarious eszközök különböző hálózati körülmények toodeliver. tooachieve, használja az élő kódolók tooencode az adatfolyam tooa többféle sávszélességű (adaptív sávszélességűvé) video-adatfolyammá alakítja.  a különböző eszközökön streaming kiszolgálásához tootake használja a Media Services [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md) toodynamically átcsomagolja a adatfolyam toodifferent protokollokat. A Media Services hello a következő adaptív sávszélességű streamelési technológiákat támogatja: HTTP Live Streaming (HLS), Smooth Streaming, MPEG DASH.

Azure Media Services **csatornák**, **programok**, és **Streamvégpontok** összes hello élő adatfolyam-funkciók, beleértve a feldolgozást, a formázást, a DVR, leíró biztonsági, a méretezhetőség és a redundancia érdekében.

A **csatorna** egy olyan folyamatot jelent, amely az élő adatfolyamok tartalmát dolgozza fel. A csatorna egy élő fogadhat bemenetét a következő módokon hello:

* Egy helyszíni élő kódolók többféle sávszélességű **RTMP** vagy **Smooth Streaming** (töredékes MP4) konfigurált toohello csatorna **áteresztő** kézbesítését. Hello **áteresztő** kézbesítési esetén okozhatnak hello adatfolyam továbbítása **csatorna**s további feldolgozás nélkül. Használhatja a következő élő kódolók képesek, amelyek kimenete többféle sávszélességű Smooth Streaming hello: MediaExcel, Ateme, kommunikációs képzelhető el, Envivo, Cisco és elemi. hello következő élő kódolók képesek RTMP-kimenetre: Adobe Flash Media élő kódoló (FMLE), Telestream Wirecast, Haivision, Teradek és Tricaster átkódolók.  Az élő kódolók is küldhet egyféle sávszélességű adatfolyamot tooa csatorna, amely az élő kódolás nincs engedélyezve, de nem ajánlott. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.

  > [!NOTE]
  > Valamely áteresztő módszer használata hello toodo élő adatfolyamként, amikor több esemény hosszú időn keresztül végzi, és már befektetett helyszíni kódolókba a leggazdaságosabb megoldás. További információt a [díjszabás](https://azure.microsoft.com/pricing/details/media-services/) nyújt.
  > 
  > 
* Egy helyszíni élő kódolók egy egyszeres sávszélességű streamet engedélyezett tooperform toohello csatornán élő kódolás Media Services hello a következő formátumok egyikében: RTMP vagy Smooth Streaming (töredezett MP4). RTP (MPEG-TS) is támogatott, amennyiben rendelkezik egy dedikált kapcsolat toohello Azure adatközpontba. a következő élő kódolók képesek az RTMP hello kimeneti toowork csatornák ilyen típusú ismert: Telestream Wirecast, FMLE. hello csatorna majd élő kódolás útján bejövő hello egyféle sávszélességű adatfolyamot tooa többféle sávszélességű (adaptív) video-adatfolyammá alakítja. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.

Kezdődően hello Media Services 2.10, amikor létrehoz egy csatornát, megadhatja, mely a csatorna tooreceive hello bemeneti adatfolyam és-e hello csatorna tooperform használni szeretne a kívánt módon élő kódolás az adatfolyam. Erre két lehetősége van:

* **Nincs** (áteresztő) – adja meg ezt az értéket, ha azt tervezi, hogy egy helyszíni élő kódoló, amely többszörös sávszélességű streammé (csatlakoztatott adatfolyam) kimeneteként toouse. Ebben az esetben hello bejövő streamből továbbítja a toohello kimeneti kódolási nélkül. Ez a csatorna előzetes too2.10 kiadásának hello működés.  
* **Standard** – válassza ezt az értéket, ha azt tervezi, hogy a Media Services tooencode toouse a egyféle sávszélességű élő streamet toomulti sávszélességű adatfolyamot. Ez a módszer a gazdaságosabb, a vertikális felskálázásával gyorsan ritka eseményekhez. Ne feledje, hogy egy számlázási hatással az élő kódolás és kell ne feledje, hogy egy élő kódolás csatorna elhagyása hello "Fut" állapotú adatforgalmi díjak gyakorisága.  Javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam-továbbítási esemény után van teljes tooavoid extra óránkénti díjakat.

## <a name="comparison-of-channel-types"></a>Csatornatípus összehasonlítása
Következő táblázatban egy útmutató toocomparing hello két csatorna támogatott típusok a Media Services

| Szolgáltatás | Átmenő csatornát | Standard csatorna |
| --- | --- | --- |
| Egyszeres sávszélességű bemeneti több bitrates hello felhőben van kódolva |Nem |Igen |
| Maximális felbontás, a rétegek száma |1080p, 8 rétegek 60 + fps |720p, 6 rétegek 30 fps |
| Bemeneti protokollok |RTMP, Smooth Streaming |RTMP, Smooth Streaming vagy RTP |
| Ár |Lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/media-services/) , majd kattintson a "Live videó" lap |Lásd: hello [árképzést ismertető oldalra](https://azure.microsoft.com/pricing/details/media-services/) |
| Maximálisan engedélyezett futási idő |24 x 7 |8 óra |
| Táblagépükkel beszúrása támogatása |Nem |Igen |
| Ad-jelzés támogatása |Nem |Igen |
| Áteresztő CEA 608/708 feliratok |Igen |Igen |
| A hírcsatorna hozzájárulás a rövid leállások képességét toorecover |Igen |Nem (csatorna megkezdődik slating másodperc 6 + bemeneti adatok nélkül) |
| Nem egységes bemeneti GOPs támogatása |Igen |Nem – bemeneti javítani kell a 2 mp GOPs |
| Változó keretének aránya bemeneti támogatása |Igen |Nem – bemeneti képkockasebessége kell meghatározni.<br/>Kisebb módosításokat elviseli, például magas mozgásérzékelési színfalak során. De kódoló too10 képkockák másodpercenkénti száma nem lehet eldobni. |
| Automatikus – gyors csatornát, amikor a bemeneti hírcsatorna elvész. |Nem |Ha nincs fut Program 12 óra elteltével |

## <a name="working-with-channels-that-receive-multi-bitrate-live-stream-from-on-premises-encoders-pass-through"></a>Helyszíni kódolóktól többféle sávszélességű adatfolyamot fogadó (áteresztő) csatornák használata
hello következő diagramon láthatók hello hello AMS platform fontosabb részei, amelyek szerepet játszanak az hello **áteresztő** munkafolyamat.

![Élő munkafolyamat](./media/media-services-live-streaming-workflow/media-services-live-streaming-current.png)

Tovább információk: [Helyszíni kódolóktól többféle sávszélességű adatfolyamot fogadó csatornák használata](media-services-live-streaming-with-onprem-encoders.md)

## <a name="working-with-channels-that-are-enabled-tooperform-live-encoding-with-azure-media-services"></a>Csatornákat végzett élő kódolás az Azure Media Services tooperform engedélyezve
hello alábbi ábrán látható fő hello hello AMS platform részei, amelyek szerepet játszanak az élő adatfolyam-továbbítási munkafolyamat ahol a csatorna egy olyan élő kódolás Media Services tooperform engedélyezve van.

![Élő munkafolyamat](./media/media-services-live-streaming-workflow/media-services-live-streaming-new.png)

További információkért lásd: [csatornák használata, hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services](media-services-manage-live-encoder-enabled-channels.md).

## <a name="description-of-a-channel-and-its-related-components"></a>A csatorna és a kapcsolódó összetevők leírása
### <a name="channel"></a>Csatorna
A Media Services szolgáltatásban [csatorna](https://docs.microsoft.com/rest/api/media/operations/channel)s felelősek az élő adatfolyam-tartalmak feldolgozása. Egy csatornát biztosít a bemeneti végpontja (feldolgozó URL-CÍMÉT) tooa élő transcoder majd meg. hello csatorna hello élő transcoder bemeneti élő adatfolyamokat fogad, és lehetővé teszi az adatfolyamként történő egy vagy több Streamvégpontok keresztül. Csatornák egy előnézeti végpont (előzetes verzió URL-cím) toopreview használni, és érvényesítse a stream, mielőtt további feldolgozás és a szállítási is megadhatja.

Hello kaphat betöltési és hello kép URL-címe hello csatorna létrehozásakor. tooget URL-, hello csatorna nincs toobe lépések hello állapotban. Amikor készen áll a toostart kérdez le adatokat az élő transcoder történő hello csatorna, hello csatorna el kell indítani. Miután hello élő transcoder megkezdi az adatok bevitele, megtekintheti az adatfolyam.

Minden egyes Media Services-fiók több csatornát, több alkalmazás, és több Streamvégpontok tartalmazhat. Attól függően, hogy hello sávszélesség és a biztonsági igényeket StreamingEndpoint szolgáltatások lehet dedikált tooone és további csatornák. Bármely StreamingEndpoint is lekéréses bármely csatornán.

### <a name="program"></a>Program
A [Program](https://docs.microsoft.com/rest/api/media/operations/program) toocontrol hello közzétételét és tárolását élő stream szegmenseinek lehetővé teszi. A programokat a csatornák kezelik. hello csatornák és programok viszonya nagyon hasonló tootraditional media, ahol egy csatornát állandó adatfolyam tartalom és a program túllépte az időkorlátot hatókörön belüli toosome esemény adott csatornán.
Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **ArchiveWindowLength** tulajdonság. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be.

ArchiveWindowLength is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden program egy objektumhoz van társítva. létre kell hoznia egy lokátort hello toopublish hello program társított eszköz. Ez a lokátor lehetővé teszi a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb egyidejűleg futó programok, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint. Például az üzleti igény szerint tooarchive program, de toobroadcast 6 óra csak az elmúlt 10 perc. tooaccomplish, toocreate két egyidejűleg zajló program van szüksége. Egy program van beállítva a tooarchive hello esemény 6 óra, de hello program nem lesz közzétéve. hello másik program beállítva tooarchive 10 percig, és a program közzé van téve.

## <a name="billing-implications"></a>Számlázási gyakorolt hatása
Egy csatorna kezdődik, számlázási, amint az állapota közeledik túl "fut" hello API segítségével.  

hello következő táblázatban hogyan csatorna állapotok hozzárendelését a hello API toobilling állapotait és az Azure-portálon. Ne feledje, hogy hello állapotok némileg eltérő hello API és a portál UX között Amint a csatorna egy olyan hello "Fut" állapotú hello API használatával, vagy hello "Kész" vagy "Streaming" állapotban a hello Azure-portálon, számlázási aktív lesz.

toostop hello csatorna további számlázási meg, hogy tooStop hello csatornán keresztül hello API vagy hello Azure-portálon.
Ön felelősséggel tartozik a csatornák leállítása, ha befejezte az hello csatornák. Hiba toostop hello csatorna folyamatos számlázási eredményez.

> [!NOTE]
> Standard csatornák használata, amikor AMS lesz az automatikus gyors bármely csatorna, amely még a "Fut" állapotú 12 óra után hello bemeneti adatcsatorna elvész, és nincsenek futó programok. Azonban továbbra is a számlázás történik a hello idő hello csatorna "Fut" állapotú volt.
>
>

### <a id="states"></a>Csatorna állapotait és az hogyan leképezik toohello számlázási mód
egy csatorna hello aktuális állapota. A lehetséges értékek:

* **Leállítva**. Ez az hello kezdeti állapotában hello csatorna a létrehozása után (kivéve, ha az automatikus indítási kiválasztott hello portálon.) Ebben az állapotban nem számlázási következik be. Ebben az állapotban lévő hello csatorna tulajdonságainak frissítése is, de streaming nem engedélyezett.
* **Kezdési**. hello csatorna indítása folyamatban van. Ebben az állapotban nem számlázási következik be. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett. Ha hiba lép fel, a csatorna hello toohello leállítva állapotú adja vissza.
* **Futó**. hello csatorna élő adatstreamek feldolgozására képes. Azt a használat számlázási most van. További számlázási hello csatorna tooprevent le kell állítania.
* **Leállítása**. hello csatorna leáll. Ebben az átmeneti állapotban nem számlázási következik be. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.
* **Törlés**. hello csatorna törlése folyamatban van. Ebben az átmeneti állapotban nem számlázási következik be. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.

hello következő táblázatban hogyan csatorna szerint térkép toohello számlázási mód.

| Csatorna állapota | Jelzése a portál kezelőfelületén | Ennyi az egész számlázási? |
| --- | --- | --- |
| Indulás alatt |Indulás alatt |Nem (átmeneti állapot) |
| Fut |Üzemkész (nincs futó program)<br/>vagy<br/>Streamelés (legalább egy futó program) |IGEN |
| Leállítás |Leállítás |Nem (átmeneti állapot) |
| Leállítva |Leállítva |Nem |

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Kapcsolódó témakörök
[Az Azure Media Services töredezett MP4 élő betöltési meghatározása](media-services-fmp4-live-ingest-overview.md)

[Hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services csatornák használata](media-services-manage-live-encoder-enabled-channels.md)

[Helyszíni kódolóktól többszörös átviteli sebességű streameket fogadó csatornák használata](media-services-live-streaming-with-onprem-encoders.md)

[Kvóták és korlátozások](media-services-quotas-and-limitations.md).  

[Media Services-fogalmak](media-services-concepts.md)
