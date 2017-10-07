---
title: "helyszíni kódolókkal által létrehozott többféle sávszélességű adatfolyamok - Azure élő aaaStream |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tooset egy csatornát, amely megkapja a többszörös sávszélességű élő adatfolyam helyszíni forrása. hello adatfolyam majd kézbesítése tooclient lejátszás alkalmazások számára pedig egy vagy több adatfolyam-továbbítási végpontok, hello a következő adaptív adatfolyam-továbbítási protokollok egyikének használatával: HLS, Smooth Streaming, DASH."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: d9f0912d-39ec-4c9c-817b-e5d9fcf1f7ea
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 04/12/2017
ms.author: cenkd;juliako
ms.openlocfilehash: 00709cecfc3b5b5dcfaa8f1e4b25bcf9d470d50b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-with-on-premises-encoders-that-create-multi-bitrate-streams"></a>Élő Stream továbbítása helyszíni kódolókkal, amely többféle sávszélességű adatfolyamok létrehozása
## <a name="overview"></a>Áttekintés
Azure Media Services a *csatorna* live-streaming tartalom feldolgozása csővezeték jelöli. Egy csatorna élő bemeneti adatfolyamok megkapja az alábbi két módszer egyikével:

* Egy helyszíni élő kódolók egy többszörös sávszélességű RTMP vagy Smooth Streaming (töredezett MP4) adatfolyam toohello csatorna, amely nem engedélyezett tooperform élő kódolása a Media Services. hello adatfolyam továbbítása csatornákon keresztül a szervezetbe további feldolgozás nélkül. Ezt a módszert nevezik *áteresztő*. Használhatja a következő élő kódolók többféle sávszélességű Smooth Streaming kimenetként rendelkező hello: Media Excel Ateme, kommunikációs képzelhető el, Envivo, Cisco és elemi. a következő élő kódolók hello rendelkezik RTMP kimenetként: Adobe Flash Media élő kódoló, Telestream Wirecast, Haivision, Teradek és TriCaster. Az élő kódolók is küldhet egy egyféle sávszélességű adatfolyamot tooa csatorna élő kódolás nincs engedélyezve, de nem javasolt. Media Services továbbítja az azt kérő hello adatfolyam toocustomers.

  > [!NOTE]
  > Valamely áteresztő módszer használata hello leggazdaságosabb megoldás toodo élő adatfolyam.


* Egy helyszíni élő kódolók egyféle sávszélességű adatfolyamot toohello csatorna engedélyezett tooperform élő kódolás Media Services hello a következő formátumok egyikében: RTP (MPEG-TS), RTMP vagy Smooth Streaming (töredezett MP4). hello csatorna majd hello bejövő egyszeres sávszélességű adatfolyamot tooa többféle sávszélességű (adaptív) video-adatfolyamot élő kódolás útján. Media Services továbbítja az azt kérő hello adatfolyam toocustomers.

Kezdődően hello Media Services 2.10, amikor létrehoz egy csatornát, megadhatja, hogyan történjen a csatorna tooreceive hello bemeneti adatfolyam. Megadhatja, hogy kívánja-e hello csatorna tooperform élő kódolás az adatfolyam. Erre két lehetősége van:

* **Továbbítása**: Adja meg ezt az értéket, ha azt tervezi, hogy többféle sávszélességű adatfolyamot (csatlakoztatott adatfolyam) kimenetként a helyszíni élő kódoló toouse. Ebben az esetben hello bejövő streamből haladnak keresztül nélkül kódolási kimeneti toohello. Ez a csatorna hello 2.10 kiadása előtt hello működés. Ez a témakör tájékoztatást nyújt a csatornák a típus használata.
* **Élő kódolás**: válassza ezt az értéket, ha azt tervezi, toouse Media Services tooencode az egyszeres sávszélességű élő streamet tooa többszörös sávszélességű streamet. Vegye figyelembe, hogy egy élő kódolás hagyja a csatorna egy **futtató** állapot adatforgalmi díjak gyakorisága. Azt javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam esemény után van teljes tooavoid extra óránkénti díjakat. Media Services továbbítja az azt kérő hello adatfolyam toocustomers.

> [!NOTE]
> Ez a témakör ismerteti a csatornák nem engedélyezett attribútumok tooperform élő kódolás. Információ a csatornákat a tooperform élő kódolás engedélyezve van, a következő témakörben: [használó Azure Media Services toocreate többféle sávszélességű adatfolyamot élő Stream továbbítása](media-services-manage-live-encoder-enabled-channels.md).
>
>

a következő diagram hello live-streaming használó munkafolyamatot, egy helyszíni élő kódoló toohave többszörös sávszélességű RTMP vagy töredezett MP4 (Smooth Streaming) adatfolyamok kimenetként jelöli.

![Élő munkafolyamat][live-overview]

## <a id="scenario"></a>Live streaming egy gyakori alaphelyzete
hello következő lépések ismertetik a gyakori élő adatfolyam-alkalmazások létrehozása.

1. Csatlakozás egy videokamerát tooa számítógép. Elindítása és konfigurálása a helyszíni élő kódoló, amely többszörös sávszélességű RTMP vagy töredezett MP4) (Smooth Streaming) adatfolyam output típusúként. További tudnivalók: [Azure Media Services RMTP-támogatása és valós idejű kódolók](http://go.microsoft.com/fwlink/?LinkId=532824)

    Ezt a lépést a csatorna létrehozása után is elvégezheti.
2. Csatorna létrehozása és elindítása.

3. Kérje le hello csatorna feldolgozó URL-CÍMÉT.

    hello élő kódoló által használt hello betöltési URL-cím toosend hello adatfolyam toohello csatorna.
4. Hello csatorna előnézeti URL-cím beolvasása.

    Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.
5. Hozzon létre egy programot.

    Hello Azure portál használata esetén létrehozhat egy programot egy objektumot is létrehoz.

    Hello .NET SDK vagy REST használatakor toocreate egy eszköz szükséges, és adjon meg toouse Ez az eszköz, amikor a program létrehozása.
6. Tegye közzé hello programhoz társított adategységet hello.   

    >[!NOTE]
    >Az Azure Media Services-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. adatfolyam-továbbítási végpontra, amelyből el kívánja toostream tartalom hello toobe rendelkezik hello **futtató** állapotát.

7. Ha már készen áll a toostart streamelésre és az archiválásra, indítsa el hello programot.

8. Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet. hello hirdetés bekerül hello kimeneti adatfolyam.

9. Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.

10. Törölje hello programot (és választhatóan a hello eszköz törlése).     

## <a id="channel"></a>A csatorna és a kapcsolódó összetevők leírása
### <a id="channel_input"></a>Bemeneti csatorna (betöltési) konfigurációk
#### <a id="ingest_protocols"></a>Betöltési folyamatos átviteli protokollt
A Media Services támogatja a választásával dolgozhat fel élő hírcsatornák segítségével többszörös sávszélességű töredékes MP4 és többféle sávszélességű RTMP protokollok streameléshez. Amikor hello RTMP betöltési protokollt streaming meg van adva, a két feldolgozó (bemeneti) végpontok hello csatorna jön létre:

* **Elsődleges URL-cím**: hello határozza meg teljes URL hello csatorna elsődleges RTMP betöltési végpont.
* **Másodlagos URL-cím** (nem kötelező): Adja meg a hello abszolút URL-címe hello csatorna másodlagos RTMP betöltési végpont.

Használja a hello másodlagos URL-cím tooimprove hello tartósság és a hibatűrés tűrés a bemeneti adatfolyam (valamint kódoló feladatátvételi és a hibatűrés tolerancia), különösen a következő forgatókönyvek hello:

- Egyetlen kódoló dupla küldését tooboth elsődleges és másodlagos URL-címek:

    Ebben a forgatókönyvben fő célja hello további rugalmasságot toonetwork ingadozását és jitters tooprovide van. Néhány RTMP kódolók kezelik hálózati bontja a kapcsolatot is. Történik, a hálózati kapcsolata megszakad, amikor egy kódoló előfordulhat, hogy leállítja a kódolást, majd küldje el hello pufferelt adatok újracsatlakozás történik, amikor. Ennek hatására a folytonosság megszakítását és adatvesztés. Hálózati bontja a kapcsolatot egy hibás hálózati vagy karbantartás miatt fordulhat elő a hello Azure oldalán. Elsődleges és másodlagos URL-címek hello hálózati problémák csökkentése, és adjon meg egy ellenőrzött frissítési folyamat. Minden alkalommal, amikor egy ütemezett hálózati kapcsolat bontása történik, a Media Services kezeli hello elsődleges és másodlagos bontja a kapcsolatot, és biztosítja a késleltetett hello két kapcsolata. Kódolók majd adatküldés idő tookeep rendelkezik, és csatlakozzon újra újra. hello hello sorrendjének való leválasztás véletlenszerű lehet, de mindig lesz késleltetés elsődleges és másodlagos vagy másodlagos/elsődleges URL-címek között. Ebben a forgatókönyvben hello kódoló még mindig hello egypontos meghibásodás kockázatát.

- Több kódolók, minden egyes tooa küldését Encoder dedikált pont:

    Ez a forgatókönyv mindkét kódoló biztosít, és redundanciát betöltési. Ebben a forgatókönyvben encoder1 leküldéses értesítések toohello elsődleges URL-címet, és encoder2 leküldéses értesítések toohello másodlagos URL-CÍMÉT. Ha az Encoder elemek esetében nem sikerül, hello más kódoló is tartsa adatküldés. Adatredundanciát tarthatjuk fenn, mert a Media Services nem választja le az elsődleges és másodlagos URL-címek: hello ugyanannyi időt vesz igénybe. Ebben a forgatókönyvben feltételezi, hogy kódolók idő szinkronizálva-e, és adja meg pontosan hello ugyanazokat az adatokat.  

- Több kódolók dupla küldését tooboth elsődleges és másodlagos URL-címek:

    Ebben a forgatókönyvben mindkét kódolók leküldéses adatok tooboth elsődleges és másodlagos URL-címeket. Így lehetővé teszi a hello legjobb megbízhatóság és a hibatűrés, valamint adatredundanciát. Ez a forgatókönyv tűri mindkét kódoló hibák, és megszakítja a kapcsolatot, akkor is, ha egy kódoló nem működik. Azt feltételezi, hogy kódolók idő szinkronizálva, és adja meg pontosan hello ugyanazokat az adatokat.  

További információ a RTMP élő kódolók képesek: [Azure Media Services RMTP-támogatása és valós idejű kódolók](http://go.microsoft.com/fwlink/?LinkId=532824).

#### <a name="ingest-urls-endpoints"></a>Betöltési URL-címek (végpont)
Egy csatornát biztosít a bemeneti végpontja (feldolgozó URL-CÍMÉT), hogy adjon meg hello élő kódoló, így hello kódoló tolható adatfolyamokat tooyour csatornák.   

Hello kaphat betöltési URL-címek hello csatorna létrehozásakor. Ön tooget az alábbi URL-címek, hello csatorna nincs toobe hello **futtató** állapotát. Amikor készen áll a toostart toohello adatcsatorna küldését, hello csatorna hello kell **futtató** állapotát. Adatok bevitele hello csatorna elindulása után megtekintheti a fájlfolyamot hello előnézeti URL-CÍMÉT.

Lehetősége van a választásával dolgozhat fel töredezett MP4) (Smooth Streaming) élő adatfolyam SSL-kapcsolaton keresztül. tooingest SSL, győződjön meg arról, hogy tooupdate hello betöltési URL-cím tooHTTPS. RTMP jelenleg nem betöltési SSL-en keresztül.

#### <a id="keyframe_interval"></a>Keyframe időköz
Amikor egy helyszíni élő kódoló toogenerate többféle sávszélességűvé használja, hello keyframe időköz hello időtartama (GOP) képek hello csoportja, hogy külső kódoló által használt. Miután hello csatorna megkapja a bejövő streamből, biztosíthat az élő adatfolyam tooclient lejátszás alkalmazások hello a következő formátumok valamelyikében: Smooth Streaming, dinamikus adaptív Streameléshez HTTP (kötőjel), és a HTTP-Live Streaming (HLS) keresztül. Akkor, ha az élő adatfolyam-, HLS mindig dinamikusan csomagolni. Alapértelmezés szerint a Media Services automatikusan hello HLS szegmens csomagolás arány (töredék szegmensenkénti) hello élő kódoló érkezett hello keyframe intervallum alapján számítja ki.

hello a következő táblázat bemutatja, hogyan számítható hello szegmens időtartama:

| Keyframe időköz | HLS szegmens csomagolás arány (FragmentsPerSegment) | Példa |
| --- | --- | --- |
| Kisebb vagy egyenlő too3 másodpercben |3:1 |Ha KeyFrameInterval (vagy GOP) 2 másodperc, a hello alapértelmezett HLS szegmens csomagolás arány, 3 too1. Ezzel létrehoz egy 6 másodperces HLS szegmens. |
| 3 too5 másodpercben |2:1 |Ha KeyFrameInterval (vagy GOP) 4 másodperc, a hello alapértelmezett HLS szegmens csomagolás arány, 2 too1. Ezzel létrehoz egy 8 másodperces HLS szegmenst. |
| 5 másodpercnél nagyobb |1:1 |Ha KeyFrameInterval (vagy GOP) 6 másodperc, a hello alapértelmezett HLS szegmens csomagolás arány, 1 too1. Ezzel létrehoz egy 6 másodperces HLS szegmens. |

Hello töredék száma szegmens arány konfigurálása hello csatorna kimeneti és FragmentsPerSegment ChannelOutputHls a beállítás módosítható.

Hello keyframe intervallumértéket ChanneInput hello KeyFrameInterval tulajdonság beállításával is módosíthatja. Ha explicit módon beállítva KeyFrameInterval, hello HLS szegmens csomagolás arány FragmentsPerSegment kiszámítása a korábban meghatározott hello szabályokat.  

Ha explicit módon beállítva KeyFrameInterval és FragmentsPerSegment is, a Media Services beállított hello értékeket fogja használni.

#### <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek
Hello IP-címek, amelyek számára engedélyezett toopublish videó toothis csatorna adhat meg. Hello következő engedélyezett IP-címet adhat meg:

* Egyetlen IP-címet (például 10.0.0.1)
* Az IP-címet és egy CIDR alhálózati maszk (például 10.0.0.1/22) használó IP-tartomány
* Az IP-címet és egy pontozott decimális alhálózati maszk (például 10.0.0.1(255.255.252.0)) használó IP-tartomány

Ha egyetlen IP-címek vannak megadva, és nincs határoz meg szabálydefiníciót, majd IP-cím engedélyezett lesz. tooallow IP-címeket, hozzon létre egy szabályt, és állítsa be a 0.0.0.0/0.

### <a name="channel-preview"></a>Csatorna előnézeti
#### <a name="preview-urls"></a>Kép URL-címek
Csatornák adjon meg egy előnézeti végpont (előzetes verzió URL-cím) toopreview használni, és az adatfolyam további feldolgozás és a szállítási előtt érvényesítse.

Hello előnézeti URL-CÍMÉT is ki lehet hello csatorna létrehozásakor. Ön tooget hello URL hello csatorna nincs toobe hello **futtató** állapotát. Adatok bevitele hello csatorna elindulása után megtekintheti az adatfolyam.

Hello preview adatfolyam letöltéséhez jelenleg csak a töredezett MP4) (Smooth Streaming) formátumban, függetlenül attól, hello megadott bemeneti típus. Használhatja a hello [Smooth Streaming figyelő](http://smf.cloudapp.net/healthmonitor) player tootest hello smooth stream. Egy, az Azure portál tooview hello birtokolt az adatfolyam player is használható.

#### <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek
Hello IP-címek, amelyek számára engedélyezett tooconnect toohello előnézeti végpont adhat meg. Ha nincs megadva IP-cím, IP-címeket engedélyezett lesz. Hello következő engedélyezett IP-címet adhat meg:

* Egyetlen IP-címet (például 10.0.0.1)
* Az IP-címet és egy CIDR alhálózati maszk (például 10.0.0.1/22) használó IP-tartomány
* Az IP-címet és egy pontozott decimális alhálózati maszk (például 10.0.0.1(255.255.252.0)) használó IP-tartomány

### <a name="channel-output"></a>A kimeneti csatornát
Csatorna kimeneti kapcsolatos információkért lásd: hello [Keyframe időköz](#keyframe_interval) szakasz.

### <a name="channel-managed-programs"></a>Programok csatorna által felügyelt
Egy csatorna programok társítva használható toocontrol hello közzétételét és tárolását szegmensek élő Stream. Programokat a csatornák kezelik. hello csatornák és programok viszonya egy nagyon hasonló tootraditional adathordozóra mutat, ahol egy csatornát állandó adatfolyam tartalom és a program túllépte az időkorlátot hatókörön belüli toosome esemény adott csatornán.

Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **archiválási időtartammal** hossza. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be. Az archiválási időtartam is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden program társítva egy eszköz, amely tárolja a folyamatos átviteli hello tartalmat. Egy eszköz csatlakoztatott tooa hello Azure storage-fiókok blokkolása blobtárolóban, és hello eszköz hello fájlokat a tárolóban lévő blobok tárolódnak. toopublish hello program, így az ügyfelek hello adatfolyam megtekintheti, létre kell hoznia egy OnDemand-kereső hello társított eszköz. Használhatja a lokátor toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb egyidejűleg programok futtatására, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Közzététele, és archiválja esemény különböző részeinek szükség szerint. Tegyük fel például, hogy az üzleti igény szerint tooarchive program, de csak hello toobroadcast 6 óra utolsó 10 perc. tooaccomplish, toocreate két egyidejűleg zajló program van szüksége. Egy program beállítása tooarchive hello esemény 6 óra, de hello program nincs közzétéve. hello más program set tooarchive 10 percig, és a program közzé van téve.

A meglévő programokat nem szabad új eseményekhez ismét felhasználni. Ehelyett hozzon létre egy új programot az egyes eseményekhez. Ha már készen áll a toostart streamelésre és az archiválásra, indítsa el hello programot. Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.

toodelete archivált tartalmat, állítsa le és törölje hello programot, és törölje a hello hozzá társított objektumot. Egy eszköz nem törölhető, ha a program akkor használja ezt a. hello program először törölni kell.

Állítsa le és törölje hello programot, után is felhasználók is játszani az archivált tartalmat, igény szerint lekért videóként hello eszköz törléséig. Ha szeretné, hogy tooretain hello archivált tartalmat, de nem rendelkezik az adatfolyamként történő elérhetővé, törölje a streamelési locator hello.

## <a id="states"></a>Csatorna állapotok és számlázási
Egy csatorna hello jelenlegi állapotában a lehetséges értékek a következők:

* **Leállítva**: hello csatorna hello kezdeti állapotában Ez az a létrehozása után. Ebben az állapotban lévő hello csatorna tulajdonságainak frissítése is, de streaming nem engedélyezett.
* **Kezdési**: hello csatorna indítása folyamatban van. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett. Ha hiba lép fel, a hello csatorna toohello adja vissza **leállítva** állapotát.
* **Futó**: hello csatorna élő adatfolyamok tud feldolgozni.
* **Leállítása**: hello csatorna leáll. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.
* **Törlés**: hello csatorna törlése folyamatban van. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.

hello következő táblázatban hogyan csatorna szerint térkép toohello számlázási mód.

| Csatorna állapota | Portál felhasználói felületének mutatók | Számlázandó? |
| --- | --- | --- | --- |
| **Indítása** |**Indítása** |Nem (átmeneti állapot) |
| **Fut** |**Készen áll a** (nincs futó programok)<p><p>vagy<p>**Adatfolyam-** (legalább egy futó program) |Igen |
| **Leállítása** |**Leállítása** |Nem (átmeneti állapot) |
| **Leállt** |**Leállt** |Nem |

## <a id="cc_and_ads"></a>Lezárt feliratok és az ad beszúrási
a következő táblázat hello támogatott szabványok kódolt és ad beszúrási mutatja be.

| Standard | Megjegyzések |
| --- | --- |
| CEA-708 és EIA-608 (708/608) |CEA-708 és EIA-608 zárva-feliratok hello az Amerikai Egyesült Államokon és Kanadán szabványainak.<p><p>Jelenleg feliratok támogatott, csak ha hello kódolt bemeneti adatfolyamban végzik. Egy élő media kódoló 608 vagy 708 feliratok beszúrásához hello kódolt adatfolyamban tooMedia szolgáltatások küldött toouse van szüksége. Beszúrt feliratok tooyour megjelenítők hello tartalmat a Media Services továbbítja. |
| TTML belül .ismt (a szöveges számok Smooth Streaming) |A Media Services dinamikus becsomagolást lehetővé teszi, hogy az ügyfelek toostream tartalom hello a következő formátumok valamelyikében: DASH, HLS vagy Smooth Streaming. Azonban ha azt a betöltési töredékes MP4) (Smooth Streaming) .ismt (Smooth Streaming a nyomait szöveg) belüli feliratok esetében a biztosíthat hello adatfolyam tooonly Smooth Streaming ügyfelek. |
| SCTE-35 |SCTE-35 rendszer digitális jelképző toocue hirdetési beszúrási által használt. Alsóbb rétegbeli fogadók hello adatfolyamba való mentésre hello jel toosplice hirdetési használata engedélyezett idő hello. A bemeneti adatfolyam hello ritka nyomon, SCTE-35 kell elküldeni.<p><p>Jelenleg csak a támogatott hello bemeneti adatfolyam formátumban ad jelek végrehajtó töredezett MP4) (Smooth Streaming). hello csak támogatott kimeneti formátum Smooth Streaming is. |

## <a id="considerations"></a>Szempontok
Amikor egy helyszíni élő kódoló toosend többféle sávszélességű adatfolyamot tooa csatornát használja, hello a következő korlátozások vonatkoznak:

* Győződjön meg arról, hogy elegendő szabad Internet kapcsolat toosend adatok toohello feldolgozó pontokat.
* Egy másodlagos használatával feldolgozó URL-CÍMÉT további sávszélesség szükséges.
* hello bejövő többféle sávszélességűvé legfeljebb 10 videominőséget szintek (réteg) és legfeljebb 5 zeneszámok rendelkezhet.
* bármely hello videominőséget szintek átlagos sávszélességű legmagasabb hello 10 MB/s alatt kell lennie.
* hello hello átlagos bitrates összes hello video- és adatfolyamok összesítésére alatt kell lennie 25 MB/s.
* Hello csatorna hello bemeneti protokoll nem módosítható, vagy a hozzá tartozó programok futnak. Ha más protokollt szeretne használni, hozzon létre külön-külön csatornákat az egyes bemeneti protokollokhoz.
* A csatorna fogadására képes egy egyféle sávszélességű. De hello csatorna hello adatfolyam nem dolgoz fel, mert hello ügyfélalkalmazások is kap egy egyféle sávszélességű adatfolyamot. (Nem ajánlott ezt a lehetőséget.)

Az alábbiakban egyéb szempontok kapcsolódó tooworking csatornák és az ahhoz kapcsolódó összetevőket:

* Minden alkalommal, amikor újrakonfigurálja az élő kódoló hello, hívja hello **alaphelyzetbe** hello csatorna metódust. Hello csatorna visszaállítása előtt toostop hello program rendelkezik. Hello csatorna visszaállítása után indítsa újra a hello programot.
* Egy csatornát csak akkor, ha az hello leállítható **futtató** állapotát, és minden program hello csatornán lett leállítva.
* Alapértelmezés szerint csak 5 csatornák tooyour Media Services-fiókot is hozzáadhat. További információkért lásd: [kvóták és korlátozások](media-services-quotas-and-limitations.md).
* Csak akkor, ha a csatorna van hello kell fizetni **futtató** állapotát. További információkért tekintse meg a toohello [állapotok és számlázási csatorna](media-services-live-streaming-with-onprem-encoders.md#states) szakasz.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="feedback"></a>Visszajelzés
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Kapcsolódó témakörök
[Az Azure Media Services töredezett MP4 élő betöltési meghatározása](media-services-fmp4-live-ingest-overview.md)

[Az Azure Media Services áttekintése és gyakori alkalmazási esetei](media-services-overview.md)

[Media Services alapfogalmaiért](media-services-concepts.md)

[live-overview]: ./media/media-services-manage-channels-overview/media-services-live-streaming-current.png
