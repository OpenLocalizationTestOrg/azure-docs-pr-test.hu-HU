---
title: "aaaLive streaming használó Azure Media Services toocreate többféle sávszélességű adatfolyamok |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tooset egy csatornát, amely fogad egy egyféle sávszélességű élő adatfolyamot helyszíni forrása, és hajtja végre az élő kódolás tooadaptive sávszélességű adatfolyamot a Media Services. hello adatfolyam majd kézbesítése tooclient lejátszás alkalmazások számára pedig egy vagy több adatfolyam-végpontot, hello a következő adaptív adatfolyam-továbbítási protokollok egyikének használatával: HLS, Smooth Stream, MPEG DASH."
services: media-services
documentationcenter: 
author: anilmur
manager: cfowler
editor: 
ms.assetid: 30ce6556-b0ff-46d8-a15d-5f10e4c360e2
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: juliako;anilmur
ms.openlocfilehash: a8bbdd1570cc9a11bfc2de7bb4ceb9006cc25534
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="live-streaming-using-azure-media-services-toocreate-multi-bitrate-streams"></a>Live streaming használó Azure Media Services toocreate többféle sávszélességű adatfolyamok
## <a name="overview"></a>Áttekintés
Az Azure Media Services (AMS), egy **csatorna** élő adatfolyam-tartalmak feldolgozására adatcsatorna jelöli. A **csatorna** bemeneti élő adatfolyamok megkapja az alábbi két módszer egyikével:

* Egy helyszíni élő kódolók egy egyszeres sávszélességű streamet engedélyezett tooperform toohello csatornán élő kódolás Media Services hello a következő formátumok egyikében: RTP (MPEG-TS), RTMP vagy Smooth Streaming (töredékes MP4). hello csatorna majd élő kódolás útján bejövő hello egyféle sávszélességű adatfolyamot tooa többféle sávszélességű (adaptív) video-adatfolyammá alakítja. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.
* Egy helyszíni élő kódolók többféle sávszélességű **RTMP** vagy **Smooth Streaming** (töredékes MP4) toohello csatorna, amely nincs engedélyezve az élő kódolás AMS tooperform. hello okozhatnak adatfolyam továbbítása **csatorna**s további feldolgozás nélkül. Ezt a módszert nevezik **áteresztő**. Használhatja a következő élő kódolók képesek, amelyek kimenete többféle sávszélességű Smooth Streaming hello: MediaExcel, Ateme, kommunikációs képzelhető el, Envivo, Cisco és elemi. hello következő élő kódolók képesek RTMP-kimenetre: Adobe Flash Media élő kódoló (FMLE), Telestream Wirecast, Haivision, Teradek és Tricaster kódolók.  Az élő kódolók is küldhet egyféle sávszélességű adatfolyamot tooa csatorna, amely az élő kódolás nincs engedélyezve, de nem ajánlott. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.
  
  > [!NOTE]
  > Valamely áteresztő módszer használata hello leggazdaságosabb megoldás toodo élő adatfolyam.
  > 
  > 

Kezdődően hello Media Services 2.10, amikor létrehoz egy csatornát, megadhatja, mely a csatorna tooreceive hello bemeneti adatfolyam és-e hello csatorna tooperform használni szeretne a kívánt módon élő kódolás az adatfolyam. Erre két lehetősége van:

* **Nincs** – adja meg ezt az értéket, ha azt tervezi, hogy egy helyszíni élő kódoló, amely többszörös sávszélességű streammé (csatlakoztatott adatfolyam) kimeneteként toouse. Ebben az esetben hello bejövő streamből továbbítja a toohello kimeneti kódolási nélkül. Ez a csatorna előzetes too2.10 kiadásának hello működés.  Talál bővebb információt az ilyen típusú csatornák használata, [élő Stream továbbítása helyszíni kódolókkal, amely többféle sávszélességű adatfolyamok létrehozása](media-services-live-streaming-with-onprem-encoders.md).
* **Standard** – válassza ezt az értéket, ha azt tervezi, hogy a Media Services tooencode toouse a egyféle sávszélességű élő streamet toomulti sávszélességű adatfolyamot. Ne feledje, hogy egy számlázási hatással az élő kódolás és kell ne feledje, hogy egy élő kódolás csatorna elhagyása hello "Fut" állapotú adatforgalmi díjak gyakorisága.  Javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam-továbbítási esemény után van teljes tooavoid extra óránkénti díjakat.

> [!NOTE]
> Ez a témakör ismerteti a csatornák engedélyezett attribútumok tooperform élő kódolás (**szabványos** kódolási típus). Információ a csatornák, amelyek nincsenek engedélyezve tooperform élő kódolás, lásd: [élő Stream továbbítása helyszíni kódolókkal, amely többféle sávszélességű adatfolyamok létrehozása](media-services-live-streaming-with-onprem-encoders.md).
> 
> Győződjön meg arról, hogy tooreview hello [szempontok](media-services-manage-live-encoder-enabled-channels.md#Considerations) szakasz.
> 
> 

## <a name="billing-implications"></a>Számlázási gyakorolt hatása
Egy élő kódolás csatorna megkezdődik, amint az állapota közeledik túl "fut" hello API keresztül számlázási.   Hello állapot hello Azure-portálon, illetve hello Azure Media Services Explorer eszköz (http://aka.ms/amse) is megtekintheti.

hello következő táblázatban hogyan csatorna állapotok hozzárendelését a hello API toobilling állapotait és az Azure-portálon. Ne feledje, hogy hello állapotok némileg eltérő hello API és a portál UX között Amint a csatorna egy olyan hello "Fut" állapotú hello API használatával, vagy hello "Kész" vagy "Streaming" állapotban a hello Azure-portálon, számlázási aktív lesz.
toostop hello csatorna további számlázási meg, hogy tooStop hello csatornán keresztül hello API vagy hello Azure-portálon.
Ön felelősséggel tartozik a csatornák leállítása, ha befejezte az hello élő kódolás csatornák.  Hiba toostop egy kódolási csatorna folyamatos számlázási eredményez.

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

### <a name="automatic-shut-off-for-unused-channels"></a>Automatikus leállítási nem használt csatornák
Media Services megkezdődött kezdve a 2016. január 25 automatikusan leáll egy csatorna (élő kódolás engedélyezve), hogy a frissítés után futott nem használt állapotban hosszú ideig. Ez vonatkozik, amelyek nem aktív programot, és amelyek nem érkezett meg egy bemeneti hozzájárulás hosszabb ideig hírcsatorna tooChannels.

használaton kívüli időszakra hello küszöbérték névlegesen 12 óra, de tulajdonos toochange.

## <a name="live-encoding-workflow"></a>Élő kódolás munkafolyamat
hello következő diagram jelöli egy élő adatfolyam-továbbítási munkafolyamat, amikor egy csatorna kap hello a következő protokollok valamelyikével egyféle sávszélességű adatfolyamot: RTMP, Smooth Streaming vagy RTP (MPEG-TS); majd hello adatfolyam tooa többféle sávszélességűvé kódolja. 

![Élő munkafolyamat][live-overview]

## <a id="scenario"></a>Gyakori élő adatfolyam-továbbítási forgatókönyv
Az alábbiakban hello általános lépéseket streamelő alkalmazásokat létrehozni.

> [!NOTE]
> Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg. Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen. Ne feledje, hogy egy számlázási hatással az élő kódolás és kell ne feledje, hogy egy élő kódolás csatorna elhagyása hello "Fut" állapotú óránkénti adatforgalmi díjak gyakorisága.  Javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam-továbbítási esemény után van teljes tooavoid extra óránkénti díjakat. 
> 
> 

1. Csatlakozás egy videokamerát tooa számítógép. Indítson el és konfiguráljon egy helyszíni élő kódoló a kimenetre küldheti egy **egyetlen** sávszélességű adatfolyamot hello a következő protokollok valamelyikével: RTMP, Smooth Streaming vagy RTP (MPEG-TS). 
   
    Ezt a lépést a csatorna létrehozása után is elvégezheti.
2. Hozzon létre és indítson el egy csatornát. 
3. Kérje le hello csatorna feldolgozó URL-CÍMÉT. 
   
    hello betöltési URL-cím hello élő kódoló toosend hello adatfolyam toohello csatorna által használt.
4. Hello csatorna előnézeti URL-cím beolvasása. 
   
    Az URL-cím tooverify, hogy a csatornája megfelelően fogadja-hello élő adatfolyam használata.
5. Hozzon létre egy programot. 
   
    Ha az Azure portál használatával hello, létrehozhat egy programot egy objektumot is létrehoz. 
   
    .NET SDK és a többi használata ehhez kell toocreate egy eszköz és toouse Ez az eszköz egy Program létrehozásakor. 
6. Hello programhoz társított hello objektum közzététele.   
   
    >[!NOTE]
    >Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. adatfolyam-továbbítási végpontra, amelyből el kívánja toostream tartalom hello toobe rendelkezik hello **futtató** állapotát. 
    
7. Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el hello programot.
8. Másik lehetőségként hello élő kódoló jelzett toostart hirdetést is lehet. hello hirdetés bekerül hello kimeneti adatfolyam.
9. Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény.
10. Törli a hello Program (és opcionálisan a hello eszköz törlése).   

> [!NOTE]
> Nagyon fontos nem tooforget tooStop élő kódolás csatornát. Vegye figyelembe, hogy nincs hatással az élő kódolás számlázási egy óránként és kell ne feledje, hogy egy élő kódolás csatorna elhagyása hello "Fut" állapotú adatforgalmi díjak gyakorisága.  Javasoljuk, hogy azonnal leállítja a futó csatornák az élő adatfolyam-továbbítási esemény után van teljes tooavoid extra óránkénti díjakat. 
> 
> 

## <a id="channel"></a>Csatorna bemeneti (betöltési) konfigurációk
### <a id="Ingest_Protocols"></a>Betöltési folyamatos átviteli protokollt
Ha hello **kódolótípusához** értéke túl**szabványos**, érvényes beállítások:

* **RTP** (MPEG-TS): MPEG-2 Transport Stream RTP keresztül.  
* Egyszeres sávszélességű **RTMP**
* Egyszeres sávszélességű **töredékes MP4** (Smooth Streaming)

#### <a name="rtp-mpeg-ts---mpeg-2-transport-stream-over-rtp"></a>RTP (MPEG-TS) - MPEG-2 Transport Stream RTP keresztül.
Tipikus használati eset: 

Professional műsorszolgáltatók általában együttműködve csúcskategóriás helyszíni élő kódolók képesek például elemi technológiákat, Ericsson, Ateme, Imagine vagy Envivo toosend szállítóktól származó adatfolyam. Az informatikai részleg és magánhálózatokon együtt gyakran használják.

Szempontok:

* Adjon meg egy program átviteli adatfolyam (SPTS) hello használata erősen ajánlott. 
* Szövegszerkesztőben mentése too8 hang adatfolyamok over RTP MPEG-2 TS használatával. 
* hello video-adatfolyamot rendelkeznie kell egy, a alatt 15 MB/s átlagos átviteli sebesség
* hello átlagos összesített sávszélességű hello hang adatfolyam rövidebbek 1 MB/s
* Következő vannak hello támogatott kodekek:
  
  * MPEG-2 / H.262 videó 
    
    * Fő profil (4:2:0)
    * Erős (4:2:0, 4:2:2)
    * 422 profil (4:2:0, 4:2:2)
  * MPEG-4 AVC / H.264 videó  
    
    * Alapkonfiguráció, fő, nagy profil (8 bites 4:2:0)
    * Magas 10 profil (10 bites 4:2:0)
    * Magas 422 profil (10 bites 4:2:2)
  * MPEG-2 AAC-LC hang 
    
    * Monó, Sztereó, (5.1, 7.1) között legyen
    * MPEG-2 stílus ADTS csomagolás
  * Dolby digitális (AC-3) hang 
    
    * Monó, Sztereó, (5.1, 7.1) között legyen
  * MPEG hang (II és III réteg) 
    
    * Monó, sztereó
* Ajánlott szórás kódolók tartalmazza:
  
  * Képzelje el kommunikációs Selenio ENC 1
  * Képzelje el kommunikációs Selenio ENC 2
  * Élő elemi

#### <a id="single_bitrate_RTMP"></a>Egyszeres sávszélességű RTMP
Szempontok:

* hello bejövő adatfolyam nem tartalmazhat többszörös sávszélességű videó
* hello video-adatfolyamot rendelkeznie kell egy, a alatt 15 MB/s átlagos átviteli sebesség
* hello hangadatfolyam rendelkeznie kell egy, a alatt 1 MB/s átlagos átviteli sebesség
* Következő vannak hello támogatott kodekek:
* MPEG-4 AVC / H.264 videó
* Alapkonfiguráció, fő, nagy profil (8 bites 4:2:0)
* Magas 10 profil (10 bites 4:2:0)
* Magas 422 profil (10 bites 4:2:2)
* MPEG-2 AAC-LC hang
* Monó, Sztereó, (5.1, 7.1) között legyen
* 44,1 kHz mintavételi ráta
* MPEG-2 stílus ADTS csomagolás
* Ajánlott kódolók a következők:
* Telestream Wirecast
* Élő adás Flash adathordozó

#### <a name="single-bitrate-fragmented-mp4-smooth-streaming"></a>Single bitrate Fragmented MP4 (Egyszeres sávszélességű, fragmentált MP4) (Smooth Streaming)
Tipikus használati eset:

A helyszíni élő kódolók képesek elemi technológiákat, Ericsson, Ateme, Envivo toosend hello bemeneti adatfolyam keresztül hello nyissa meg az internet tooa közelben Azure-adatközpont például szállítóktól származó használja.

Szempontok:

Ugyanaz, mint a [egyszeres sávszélességű RTMP](media-services-manage-live-encoder-enabled-channels.md#single_bitrate_RTMP).

#### <a name="other-considerations"></a>Egyéb szempontok
* Hello csatorna hello bemeneti protokoll nem módosítható, vagy a hozzá tartozó programok futnak. Ha más protokollt szeretne használni, hozzon létre külön-külön csatornákat az egyes bemeneti protokollokhoz.
* Maximális felbontás hello bejövő video-adatfolyamok 1920 x 1080, és legfeljebb 60 mezők/második ha váltakozó, vagy 30 keretek/másodperc Ha fokozatos.

### <a name="ingest-urls-endpoints"></a>Betöltési URL-címek (végpont)
Egy csatornát biztosít a bemeneti végpontja (feldolgozó URL-CÍMÉT), hogy adjon meg hello élő kódoló, így hello kódoló tolható adatfolyamokat tooyour csatornák.

Hello kaphat betöltési URL-címek egy csatorna létrehozása után. tooget URL-, hello csatorna nincs toobe hello **futtató** állapotát. Amikor készen áll a toostart kérdez le adatokat a hello csatorna, meg kell legyen hello **futtató** állapotát. Miután hello csatorna megkezdi az adatok bevitele, megtekintheti a fájlfolyamot hello előnézeti URL-CÍMÉT.

Lehetősége van a választásával dolgozhat fel töredékes MP4) (Smooth Streaming) élő adatfolyam SSL-kapcsolaton keresztül. tooingest SSL, győződjön meg arról, hogy tooupdate hello betöltési URL-cím tooHTTPS. Vegye figyelembe, hogy jelenleg AMS nem támogatja az SSL az egyéni tartomány.  

### <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek
Hello IP-címek, amelyek számára engedélyezett toopublish videó toothis csatorna adhat meg. Az engedélyezett IP-címek köre tartalmazhat egyetlen IP-címet (például „10.0.0.1”), vagy egy IP-tartományt, amelyet egy IP-cím és egy CIDR alhálózati maszk segítségével (például „10.0.0.1/22”), vagy egy IP-cím és egy pontozott decimális alhálózati maszk (például „10.0.0.1(255.255.252.0)”) segítségével lehet megadni.

Ha nem ad meg IP-címeket, és nem határoz meg szabálydefiníciót, a rendszer egyetlen IP-címet sem engedélyez. tooallow IP-címeket, hozzon létre egy szabályt, és állítsa be a 0.0.0.0/0.

## <a name="channel-preview"></a>Csatorna előnézeti
### <a name="preview-urls"></a>Kép URL-címek
Csatornák adjon meg egy előnézeti végpont (előzetes verzió URL-cím) toopreview használni, és az adatfolyam további feldolgozás és a szállítási előtt érvényesítse.

Hello előnézeti URL-CÍMÉT is ki lehet hello csatorna létrehozásakor. tooget hello URL-címe, hello csatorna nincs toobe hello **futtató** állapotát.

Miután hello csatorna megkezdi az adatok bevitele, megtekintheti az adatfolyam.

> [!NOTE]
> Jelenleg hello preview adatfolyam is csak akkor kézbesíti a töredékes MP4) (Smooth Streaming) formátum hello függetlenül meg a bemeneti típus. Használhatja a hello [http://smf.cloudapp.net/healthmonitor](http://smf.cloudapp.net/healthmonitor) player tootest hello Smooth Stream. Használhatja az Azure portál tooview hello szolgáltatásban üzemeltetett egy player az adatfolyam.
> 
> 

### <a name="allowed-ip-addresses"></a>Engedélyezett IP-címek
Hello IP-címek, amelyek számára engedélyezett tooconnect toohello előnézeti végpont adhat meg. Ha IP-cím megadva, az engedélyezett IP-címeket. Az engedélyezett IP-címek köre tartalmazhat egyetlen IP-címet (például „10.0.0.1”), vagy egy IP-tartományt, amelyet egy IP-cím és egy CIDR alhálózati maszk segítségével (például „10.0.0.1/22”), vagy egy IP-cím és egy pontozott decimális alhálózati maszk (például „10.0.0.1(255.255.252.0)”) segítségével lehet megadni.

## <a name="live-encoding-settings"></a>Kódolási beállítások élő
Ez a szakasz ismerteti, hogyan hello hello csatorna élő kódolója hello beállításait lehet beállítani, amikor hello **kódolási típus** csatorna értéke túl**szabványos**.

> [!NOTE]
> Ha a bevitel, több nyelv nyomon követi, valamint arról, hogy az Azure-ral élő kódolás, csak RTP beszélő bemenet esetén támogatott. Másolatot too8 hang adatfolyamok over RTP MPEG-2 TS használatával adhatja meg. Választásával dolgozhat fel RTMP vagy Smooth streaming több zeneszámok jelenleg nem támogatott. Ha ez az élő kódolás [helyszíni élő kódolja](media-services-live-streaming-with-onprem-encoders.md), nincs korlátozás nélkül, mivel egy csatornán keresztül további feldolgozás nélkül átadja tooAMS függetlenül küld.
> 
> 

### <a name="ad-marker-source"></a>Az ad jelölőt forrás
Ad reklámkonfiguráció hello forrást is megadhat. Alapértelmezett érték **Api**, ami azt jelenti, hogy hello hello csatorna élő kódolója figyelnie kell aszinkron tooan **Ad jelölőt API**.

hello másik érvényes lehetőség egy **Scte35** (csak engedélyezett, ha hello betöltési tooRTP (MPEG-TS) adatfolyam-protokoll van beállítva. Scte35 megadása esetén a hello élő kódoló fogja elemezni SCTE-35 jelek adatfolyamból hello bemeneti RTP (MPEG-TS).

### <a name="cea-708-closed-captions"></a>CEA 708 feliratok bezárása
Egy nem kötelező jelzőt, amely közli hello élő kódoló tooignore CEA 708 feliratok adatokat ágyazott hello bejövő videó. Ha hello jelző értéke toofalse (alapértelmezett), hello kódoló azonosítja, és újra CEA 708 adatok beszúrása hello kimeneti video-adatfolyamot.

### <a name="video-stream"></a>Video-adatfolyammá alakítja
Választható. Hello bemeneti video-adatfolyamot ismerteti. Ha ez a mező nincs megadva, hello alapértelmezett érték lesz érvényben. Ez a beállítás csak akkor, ha a hello bemeneti adatfolyam-továbbítási protokoll van beállítva (MPEG-TS) tooRTP engedélyezett.

#### <a name="index"></a>Index
Arról, hogy mely bemeneti video-adatfolyamot kell feldolgozni hello hello csatorna élő kódolója nulla alapú indexét. Ez a beállítás csak akkor, ha érvényes betöltési adatfolyam-protokoll RTP (MPEG-TS).

Alapértelmezett értéke nulla. Ajánlott toosend egyetlen program transport Stream (SPTS). Ha hello bemeneti adatfolyam több programokat is tartalmaz, hello élő kódoló hello Program térkép tábla (részlet) hello bemeneti elemez, MPEG-2 Video- vagy H.264 adatfolyam típusú csoportnevet hello felhasználandó azonosítja és elrendezi a fizetési hello megadott hello sorrendben hello nulla alapú index használja fel hello n-edik bejegyzést, hogy az elrendezéssel toopick.

### <a name="audio-stream"></a>Hangadatfolyam
Választható. Hello bemeneti hang adatfolyamok ismerteti. Ha ebben a mezőben nem szerepel, a megadott hello alapértelmezett értékek érvényesek. Ez a beállítás csak akkor, ha a hello bemeneti adatfolyam-továbbítási protokoll van beállítva (MPEG-TS) tooRTP engedélyezett.

#### <a name="index"></a>Index
Ajánlott toosend egyetlen program transport Stream (SPTS). Ha hello bemeneti adatfolyam több programokat is tartalmaz, hello hello csatorna élő kódolója hello Program térkép tábla (részlet) hello bemeneti elemez, MPEG-2 AAC ADTS vagy AC-3 rendszer-A vagy B AC-3-rendszer vagy MPEG-2 privát adatfolyam típusú csoportnevet hello felhasználandó azonosítja PES vagy a hang MPEG-1 vagy a hallható MPEG-2, és elrendezi a fizetési hello megadott hello sorrendben hello nulla alapú index használja fel hello n-edik bejegyzést, hogy az elrendezéssel toopick.

#### <a name="language"></a>Nyelv
hello hello hangadatfolyam, tooISO 639-2, például a ENG. megfelelő nyelvi azonosítója Ha nem található, a hello alapértelmezett érték (nincs definiálva) és.

Lehet too8 hangadatfolyam beállítása adni, ha hello bemeneti toohello csatorna MPEG-2 TS keresztül RTP be. Azonban lehetnek nem két bejegyzés hello az index ugyanazt az értéket.

### <a id="preset"></a>Rendszer-készlet
Megadja a hello beállított toobe hello Ez a csatorna élő kódolója használják. Csak az engedélyezett értéket jelenleg hello **Default720p** (alapértelmezett).

Vegye figyelembe, hogy ha egyéni készletek van szüksége, forduljon amslived@Microsoft.com e-mail címen.

**Default720p** be a következő 7 rétegek hello hello videó kódolja.

#### <a name="output-video-stream"></a>Kimeneti Video-adatfolyamot
| Átviteli sebesség | Szélessége | Magassága | MaxFPS | Profil | Kimeneti adatfolyam neve |
| --- | --- | --- | --- | --- | --- |
| 3500 |1280 |720 |30 |Magas |Video_1280x720_3500kbps |
| 2200 |960 |540 |30 |Fő |Video_960x540_2200kbps |
| 1350 |704 |396 |30 |Fő |Video_704x396_1350kbps |
| 850 |512 |288 |30 |Fő |Video_512x288_850kbps |
| 550 |384 |216 |30 |Fő |Video_384x216_550kbps |
| 350 |340 |192 |30 |Alapkonfiguráció |Video_340x192_350kbps |
| 200 |340 |192 |30 |Alapkonfiguráció |Video_340x192_200kbps |

#### <a name="output-audio-stream"></a>Kimeneti hangadatfolyam
Hang kódolt toostereo AAC-LC, 64 KB/s, a mintavételi arány 44,1 kHz.

## <a name="signaling-advertisements"></a>Hirdetmények jelzés
Ha a csatorna élő kódolás engedélyezve van, a folyamat, amely feldolgozó videó tartalmaz összetevőt, és kezelheti azokat. Képes jelezni, hello csatorna tooinsert táblagépükkel és/vagy hello kimenő adaptív sávszélességűvé hirdetéseket. A program táblagépükkel továbbra is használható fel hello bemeneti élő adatcsatorna toocover bizonyos esetekben (például során kereskedelmi szünet) lemezképeket. A hello kimenő adatfolyam tootell hello videólejátszó tootake különleges művelet – például tooswitch tooan hirdetmény hello megfelelő időben beágyazása idő szinkronizálva jelek hirdetési jelek, vannak. Ez [blog](https://codesequoia.wordpress.com/2014/02/24/understanding-scte-35/) hello SCTE-35 jelképző mechanizmus, amely erre a célra áttekintését. Alább a jellemző forgatókönyv, az élő esemény sikerült megvalósítása van.

1. Az beszerzése a előtti esemény lemezkép hello esemény indítása előtt megjelenítők rendelkezik.
2. Az beszerzése a utáni esemény lemezkép hello esemény befejeződését követően megjelenítők rendelkezik.
3. Az beszerzése HIBAESEMÉNY lemezkép, ha probléma van (például a áramkimaradás hello stadium) hello esemény során megjelenítők rendelkezik.
4. AD-BREAK kép toohide hello élő eseményt küldjön hírcsatorna kereskedelmi szünet során.

Az alábbiakban hello hello tulajdonságok adhatók meg jelzés hirdetéseket. 

### <a name="duration"></a>Időtartam
hello időtartama (másodpercben), a hello kereskedelmi szünet. Ez van toobe nullától eltérő pozitív értéket rendelés toostart hello kereskedelmi sortörés. Ha kereskedelmi szünet folyamatban van, és hello időtartama van set toozero hello CueId a megfelelő hello folyamatos kereskedelmi break, majd, hogy skálamegszakítás meg lett szakítva.

### <a name="cueid"></a>CueId
Hello kereskedelmi break, alsóbb rétegbeli alkalmazás tootake megfelelő elemek által használt toobe egyedi azonosítója. Toobe pozitív egész számnak kell. Állítsa be az érték tooany véletlenszerű pozitív egész, vagy egy felsőbb rétegbeli rendszer tootrack hello köteg azonosítókat használjon. Bizonyos toonormalize győződjön meg semmilyen azonosítók toopositive egész számok keresztül hello API elküldése előtt.

### <a name="show-slate"></a>Lappal megjelenítése
Választható. Jelzi a hello élő kódoló tooswitch toohello [lappal alapértelmezett](media-services-manage-live-encoder-enabled-channels.md#default_slate) során egy kereskedelmi break rendszerképet, a bejövő hello videoanyagához elrejtése. Hang is némított lappal során. Alapértelmezett érték a **hamis**. 

hello használt kép egy hello csatorna létrehozása a hello időpontjában hello alapértelmezett lappal eszköz Id tulajdonsághoz megadott hello lesz. hello lappal lesz felhőbe toofit hello megjelenített kép méretét. 

## <a name="insert-slate--images"></a>Helyezze be a lappal lemezképek
hello hello csatorna élő kódolója jelzett tooswitch tooa lappal kép lehet. Azt is jelzett tooend egy folyamatos lappal. 

hello élő kódoló konfigurált tooswitch tooa lappal kép lehet, és elrejtése hello bejövő videó jel bizonyos esetekben – például egy ad break során. Ha ilyen lappal nincs konfigurálva, a bemeneti videó nem adott ad break során van maszkolva.

### <a name="duration"></a>Időtartam
hello a időtartama másodpercben hello lappal. Ez van toobe nullától eltérő pozitív értéket rendelés toostart hello lappal. Ha egy folyamatos lappal, és nulla értékű időtartamot van megadva, majd, hogy a folyamatban lévő lappal befejeződik.

### <a name="insert-slate-on-ad-marker"></a>Befutó vagy hirdetésjelző beszúrása
Ha set tootrue, ez a beállítás során az ad-break hello élő kódoló tooinsert a lappal lemezkép állítja be. hello alapértelmezett érték: igaz. 

### <a id="default_slate"></a>Alapértelmezett lappal eszköz azonosítója

Választható. Megadja a hello hello Media Services eszköz hello lappal lemezképet tartalmazó objektum azonosítója. Alapértelmezett értéke null. 


>[!NOTE] 
>Hello csatorna létrehozása, előtt hello lappal kép a következő korlátozások hello fel kell tölteni egy dedikált eszközként (más fájlokat az eszköz kellene lennie). Ez a rendszerkép szolgál, csak ha hello élő kódoló szúr be egy lappal tooan ad megszakítás miatt, vagy explicit módon lett jelzést tooinsert a lappal. hello élő kódoló is végrehajthatja a lappal módba során bizonyos hibakörülményeket – például ha hello bemeneti jel elvész. Nincs jelenleg nincs lehetőség toouse egyéni lemezkép amikor hello élő kódoló ilyen "bemenő jel elveszett" állapotba kerül. Akkor is szavazzon Ez a szolgáltatás [Itt](https://feedback.azure.com/forums/169396-azure-media-services/suggestions/10190457-define-custom-slate-image-on-a-live-encoder-channel).


* Legfeljebb 1920 x 1080 közben.
* Legfeljebb 3 MB-nál.
* hello fájlnév egy *.jpg kiterjesztésűnek kell lennie.
* hello kép hello csak AssetFile az adott eszköz és a AssetFile hello elsődleges fájl állapotúként kell megjelölni, fel kell tölteni egy adategységbe. hello eszköz nem lehet alkalmaz.

Ha hello **alapértelmezett épületkő eszköz azonosítója** nincs megadva, és **beszúrása az ad jelölőt lappal** értéke túl**igaz**, alapértelmezett Azure Media Services kép lesz bemeneti használt toohide hello Video-adatfolyammá alakítja. Hang is némított lappal során. 

## <a name="channels-programs"></a>Csatorna programok
Egy csatorna programok társítva, amelyek lehetővé teszik a toocontrol hello közzétételét és tárolását élő stream szegmenseinek. A programokat a csatornák kezelik. hello csatornák és programok viszonya nagyon hasonló tootraditional media, ahol egy csatornát állandó adatfolyam tartalom és a program túllépte az időkorlátot hatókörön belüli toosome esemény adott csatornán.

Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **archiválási időtartammal** hossza. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be. Az archiválási időtartam is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden program társítva egy eszköz, amely tárolja a folyamatos átviteli hello tartalmat. Egy eszköz csatlakoztatott tooa blokk blobtárolóban hello Azure Storage-fiók és hello eszköz hello fájlokat a tárolóban lévő blobok tárolódnak. toopublish hello program, így az ügyfelek megtekintheti létre kell hoznia egy OnDemand-lokátort hello hello adatfolyam társított eszköz. Ez a lokátor lehetővé teszi a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb egyidejűleg futó programok, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint. Például az üzleti igény szerint tooarchive program, de toobroadcast 6 óra csak az elmúlt 10 perc. tooaccomplish, toocreate két egyidejűleg zajló program van szüksége. Egy program van beállítva a tooarchive hello esemény 6 óra, de hello program nem lesz közzétéve. hello másik program beállítva tooarchive 10 percig, és a program közzé van téve.

A meglévő programokat nem szabad új eseményekhez ismét felhasználni. Ehelyett hozzon létre, és indítsa el egy új programot az egyes eseményekhez hello programozási élő adatfolyam-alkalmazásokhoz című részben leírtak szerint.

Amikor készen áll a toostart streamelésre és az archiválásra, indítsa el hello programot. Állítsa le hello programot, ha azt szeretné, hogy toostop streamelésre és az archiválásra hello esemény. 

toodelete archivált tartalmat, állítsa le és törölje hello programot, és törölje a hozzá társított objektumot hello. Egy eszköz nem törölhető, ha a program; használják hello program először törölni kell. 

Állítsa le és törölje hello programot, után is hello felhasználók állapotban tud toostream az archivált tartalmakat, igény szerint lekért videóként mindaddig, amíg nem törli hello eszköz.

Ha szeretné tooretain hello archivált tartalom, de nem rendelkezik az adatfolyamként történő elérhetővé, törölje a streamelési locator hello.

## <a name="getting-a-thumbnail-preview-of-a-live-feed"></a>Első élő adatcsatornára miniatűr előnézete
Ha élő kódolás engedélyezve van, most kaphat hello élő adatcsatorna előnézetét, hello csatorna eléri. Ez lehet egy hasznos eszköze toocheck, hogy az élő adatcsatorna ténylegesen közel jár hello csatorna. 

## <a id="states"></a>Csatorna állapotait és az hogyan állapotok leképezése toohello számlázási mód
egy csatorna hello aktuális állapota. A lehetséges értékek:

* **Leállítva**. Hello csatorna hello kezdeti állapotában Ez az a létrehozása után. Ebben az állapotban lévő hello csatorna tulajdonságainak frissítése is, de streaming nem engedélyezett.
* **Kezdési**. hello csatorna indítása folyamatban van. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett. Ha hiba lép fel, a csatorna hello toohello leállítva állapotú adja vissza.
* **Futó**. hello csatorna élő adatstreamek feldolgozására képes.
* **Leállítása**. hello csatorna leáll. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.
* **Törlés**. hello csatorna törlése folyamatban van. Ebben az állapotban sem a frissítés, sem a streamelés nem engedélyezett.

hello következő táblázatban hogyan csatorna szerint térkép toohello számlázási mód. 

| Csatorna állapota | Jelzése a portál kezelőfelületén | Számlázandó? |
| --- | --- | --- |
| Indulás alatt |Indulás alatt |Nem (átmeneti állapot) |
| Fut |Üzemkész (nincs futó program)<br/>vagy<br/>Streamelés (legalább egy futó program) |Igen |
| Leállítás |Leállítás |Nem (átmeneti állapot) |
| Leállítva |Leállítva |Nem |

> [!NOTE]
> Jelenleg hello csatorna start átlagos körülbelül 2 percet, de időnként too20 + percig eltarthat. Csatorna alaphelyzetbe állítását too5 percig is eltarthat.
> 
> 

## <a id="Considerations"></a>Szempontok
* Ha a csatorna **szabványos** bemeneti forrás/hozzájárulás adatcsatorna adatvesztést kódolási típus észlel, akkor kiegyenlíti az azáltal, hogy egy hiba lappal és csend hello forrás videó/hang. hello csatorna tooemit egy lappal folytatódik, amíg hello bemeneti/hozzájárulás hírcsatorna folytatása. Azt javasoljuk, hogy egy élő csatorna nem hagyható hosszabb, mint 2 órán át alkalmas állapotban. Hello csatorna bemeneti újbóli kapcsolódáskor lesznek elküldve hello viselkedését túli, nem garantált, sem pedig az annak a válasz tooa visszaállítási parancs viselkedését. Akkor lesz toostop hello csatorna rendelkezik, törölje azt, majd hozzon létre egy újat.
* Hello csatorna hello bemeneti protokoll nem módosítható, vagy a hozzá tartozó programok futnak. Ha más protokollt szeretne használni, hozzon létre külön-külön csatornákat az egyes bemeneti protokollokhoz.
* Minden alkalommal, amikor újrakonfigurálja az élő kódoló hello, hívja hello **alaphelyzetbe** hello csatorna metódust. Hello csatorna visszaállítása előtt toostop hello program rendelkezik. Hello csatorna visszaállítása után indítsa újra a hello programot.
* Egy csatornát csak akkor, ha hello futó állapotban van, és le lett állítva az összes programok hello csatornán állítható le.
* Alapértelmezés szerint csak 5 csatornák tooyour Media Services-fiók adhat hozzá. Ez az enyhe kvóták az összes új fiókot. További információkért lásd: [kvóták és korlátozások](media-services-quotas-and-limitations.md).
* Hello csatorna hello bemeneti protokoll nem módosítható, vagy a hozzá tartozó programok futnak. Ha más protokollt szeretne használni, hozzon létre külön-külön csatornákat az egyes bemeneti protokollokhoz.
* Ha a csatorna hello csak számlázása **futtató** állapotát. További információkért tekintse meg túl[ez](media-services-manage-live-encoder-enabled-channels.md#states) szakasz.
* Maximális hello élő esemény időtartama ajánlott 8 órára is jelenleg. Ha hosszabb ideig kell toorun egy csatornát, forduljon amslived@Microsoft.com e-mail címen.
* Ellenőrizze, hogy toohave hello streamvégpontra, amelyből el kívánja hello toostream tartalma **futtató** állapotát.
* Ha a bevitel, több nyelv nyomon követi, valamint arról, hogy az Azure-ral élő kódolás, csak RTP beszélő bemenet esetén támogatott. Másolatot too8 hang adatfolyamok over RTP MPEG-2 TS használatával adhatja meg. Választásával dolgozhat fel RTMP vagy Smooth streaming több zeneszámok jelenleg nem támogatott. Ha ez az élő kódolás [helyszíni élő kódolja](media-services-live-streaming-with-onprem-encoders.md), nincs korlátozás nélkül, mivel egy csatornán keresztül további feldolgozás nélkül átadja tooAMS függetlenül küld.
* hello kódolási előre definiált használ hello "maximális sebessége" 30 fps fogalmát. Így ha hello bemeneti 60fps / 59.97i, hello bemeneti keretek eldobott/inaktiválása-interlaced too30/29,97 fps. Ha hello bemeneti 50fps/50i, hello bemeneti keretek eldobott/inaktiválása-interlaced too25 fps. Ha 25 fps hello bemeneti, kimeneti 25 fps értéken marad.
* Ne feledje tooSTOP YOUR csatornák végzett. Ha ezt elmulasztja, számlázási továbbra is.

## <a name="known-issues"></a>Ismert problémák
* Javult a csatorna indítási ideje tooan átlagos 2 percet, hanem néha igényeknek továbbra is eltarthat, too20 + perc.
* RTP támogatási szakemberek műsorszolgáltatók felé van catered. Tekintse át a RTP hello kiegészítő [ez](https://azure.microsoft.com/blog/2015/04/13/an-introduction-to-live-encoding-with-azure-media-services/) blog.
* Lappal képek meg kell felelnie a leírt toorestrictions [Itt](media-services-manage-live-encoder-enabled-channels.md#default_slate). Ha úgy próbálja csatornát létrehozni egy alapértelmezett lappal, amely nagyobb, mint 1920 x 1080, hello kérelem rendszer végül hiba.
* Még egyszer... tooSTOP YOUR csatornák ne felejtse el, ha befejezte az adatfolyam. Ha ezt elmulasztja, számlázási továbbra is.

## <a name="next-step"></a>Következő lépés
Tekintse át a Media Services képzési terveket.

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="related-topics"></a>Kapcsolódó témakörök
[Az Azure Media Services események élő adatfolyamainak továbbítása](media-services-overview.md)

[Hozzon létre egy konfigurál egy sávszélességű tooadaptive portállal sávszélességű adatfolyamot élő kódolás csatornák](media-services-portal-creating-live-encoder-enabled-channel.md)

[Hozzon létre egy konfigurál egy sávszélességű tooadaptive .NET SDK-val sávszélességű adatfolyamot élő kódolás csatornák](media-services-dotnet-creating-live-encoder-enabled-channel.md)

[REST API-val csatornák kezelése](https://docs.microsoft.com/rest/api/media/operations/channel)
 
[Media Services-fogalmak](media-services-concepts.md)

[Az Azure Media Services töredezett MP4 élő betöltési meghatározása](media-services-fmp4-live-ingest-overview.md)

[live-overview]: ./media/media-services-manage-live-encoder-enabled-channels/media-services-live-streaming-new.png

