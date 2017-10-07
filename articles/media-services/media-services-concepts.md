---
title: "Media Services alapfogalmaiért aaaAzure |} Microsoft Docs"
description: "Ez a témakör áttekintést nyújt az Azure Media Services Alapfogalmaiért"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: dcefc8bc-e2ea-4b38-a643-9010f4436fb5
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: juliako
ms.openlocfilehash: 0a45deff32336dfcf778552f720c1ea21927951b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-concepts"></a>Az Azure Media Services alapfogalmaiért
Ez a témakör áttekintést hello legfontosabb Media Services alapfogalmaiért.

## <a id="assets"></a>Eszközök és tárolás
### <a name="assets"></a>Objektumok
Egy [eszköz](https://docs.microsoft.com/rest/api/media/operations/asset) digitális fájlokat (beleértve a videó, hang, képeket, miniatűröket, szöveges nyomon követi és feliratfájlokat fájlok) és hello mindezen fájlok metaadatait. Miután egy adategységbe hello digitális fájlok feltöltése után azok hello Media Services kódolási és adatfolyam-munkafolyamatok is használható.

Egy eszköz csatlakoztatott tooa blob tároló hello Azure Storage-fiók és hello hello eszközt fájljai blokkblobként a tárolóban. Lapblobokat nem Azure Media Services által támogatott.

Amikor eldönti, milyen media tartalom tooupload és egy eszköz, a tároló hello a következő szempontok érvényesek:

* Az eszköz csak egyetlen, egyedi példánya médiatartalom kell tartalmaznia. Például egy egyetlen Szerkesztés TV epizód, movie vagy hirdetmény.
* Egy eszköz nem tartalmazhat több interpretációk vagy audiovizuális fájlok módosításokat. Egy eszköz nem megfelelő használata egy példát volna kell kísérlet toostore egynél több TV epizód, a hirdetés vagy a több kamera beállítások egy eszköz belül egyetlen gyártási. Több interpretációk vagy módosításokat audiovizuális fájlok tárolása egy eszköz kódolási feladatok, streaming és biztonságossá tétele hello kézbesítési hello eszköz későbbi részében hello munkafolyamat elküldése nehézségeket okozhat.  

### <a name="asset-file"></a>Eszköz fájl
Egy [AssetFile](https://docs.microsoft.com/rest/api/media/operations/assetfile) egy video- vagy fájl a blob-tárolóban tárolt jelöli. Egy eszköz fájl mindig társítva van egy eszköz, és egy eszköz egy vagy több fájlt tartalmaz. hello Media Services kódoló feladat sikertelen lesz, ha egy eszköz fájl objektumhoz nincs társítva egy digitális fájlhoz egy blob-tárolóban.

Hello **AssetFile** példány és a hello tényleges media fájl is két különböző objektum. hello AssetFile példány hello media fájlról metaadatot tartalmaz, amíg hello médiafájl tartalmaz hello tényleges médiatartalmakat.

Ne próbáljon Media Service API-k használata nélkül Media Services által előállított blobtárolók toochange hello tartalmát.

### <a name="asset-encryption-options"></a>Eszköz titkosítási beállítások
A tartalom hello típusától függően meg tooupload, tárolási és kézbesítése, a Media Services biztosít a különböző titkosítási beállításokat, amelyek közül választhat.

>[!NOTE]
>Nincs titkosítás. Ez az alapértelmezett érték hello. Ez a beállítás használatakor a tartalom nem védett átvitel, sem tárolás közben.

Ha egy MP4 toodeliver fájlt progresszív letöltés útján tervez, használja ezt a beállítást tooupload a tartalmat.

**StorageEncrypted** – használja ezt a beállítást tooencrypt a tiszta tartalom helyileg az AES 256 bites titkosítás használata, majd töltse fel tooAzure helyén tárolás titkosítása. Storage-titkosítással védett adategységek automatikusan titkosítás és egy titkosított fájl rendszer előzetes tooencoding, és ha szükséges újra titkosítani előzetes toouploading egy új kimeneti eszközként helyezve. hello elsődleges használati eset a tárolás titkosítása akkor, ha azt szeretné, hogy a jó minőségű bemeneti médiafájljait erős titkosítással, rest-lemezen toosecure. 

A sorrend toodeliver tárolási titkosított eszköz konfigurálnia kell hello adategység továbbítási házirendjét, a Media Services tudja, hogyan szeretné toodeliver a tartalom. Mielőtt az eszköz továbbítható, hello adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével megadott továbbítási házirendjét (például AES, PlayReady vagy titkosítás nélkül). 

**CommonEncryptionProtected** -használja ezt a beállítást, ha szeretné tooencrypt (vagy már titkosítva feltöltés) tartalom általános titkosítás vagy a PlayReady DRM által (például védett Smooth Streaming egy PlayReady DRM).

**EnvelopeEncryptionProtected** – használja ezt a beállítást, ha azt szeretné, hogy tooprotect (vagy az már védett feltöltés) HTTP Live Streaming (HLS) titkosítva az Advanced Encryption Standard (AES). Ha már AES titkosított HLS, akkor titkosították Transform Manager használatával.

### <a name="access-policy"></a>Hozzáférési házirend
Egy [AccessPolicy](https://docs.microsoft.com/rest/api/media/operations/accesspolicy) engedélyek (például az olvasási, írási és lista) és a hozzáférés tooan eszköz időtartamát határozza meg. Általában akkor adja meg egy AccessPolicy objektum tooa kereső, amely az eszköz található fájlokat, használt tooaccess hello lesz.

>[!NOTE]
>A különböző AMS-szabályzatok (például a Locator vagy a ContentKeyAuthorizationPolicy) esetében a korlát 1 000 000 szabályzat. Használjon hello azonos házirend-azonosítója mindig használata hello azonos nap / hozzáférési engedélyek, például a lokátorokat, amelyek a helyen tervezett tooremain hosszú ideje (nem feltöltés házirendek) házirendek. További információ [ebben](media-services-dotnet-manage-entities.md#limit-access-policies) a témakörben érhető el.

### <a name="blob-container"></a>A BLOB-tároló
A blob-tároló blobok blobkészletek csoportosítását biztosítja. BLOB tárolók használatosak a Media Services határ pont a hozzáférés-vezérlés és a közös hozzáférésű Jogosultságkód (SAS) lokátorokat eszközök. Egy Azure Storage-fiók korlátlan számú blob tárolók is tartalmazhat. Egy tároló korlátlan számú blob tárolására használható.

>[!NOTE]
> Ne próbáljon Media Service API-k használata nélkül Media Services által előállított blobtárolók toochange hello tartalmát.
> 
> 

### <a id="locators"></a>Keresők
[Lokátor](https://docs.microsoft.com/rest/api/media/operations/locator)s adjon meg egy belépési pont tooaccess hello egy eszköz található fájlokat. A hozzáférési házirendek használt toodefine hello engedélyek és, hogy az ügyfél rendelkezik-e hozzáférési tooa eszköz megadott időtartamot. Keresők hozzáférési házirend sok tooone kapcsolattal is rendelkeznie, például, hogy különböző keresők biztosíthat a különböző indítási idejének és csatlakozási típusok toodifferent ügyfeleket, ha minden a hello ugyanazt az engedélyt és időtartama beállítások; azonban a megosztott elérési házirend korlátozása az Azure storage szolgáltatások által beállított, miatt nem lehet egy adott eszközhöz társított egyszerre legfeljebb öt egyedi lokátorokat. 

Media Services két lokátortípust támogat: OnDemandOrigin keresők toostream media (például MPEG DASH, HLS vagy Smooth Streaming) használt, vagy fokozatosan letölteni a média és a SAS URL-cím lokátorokat, használt tooupload vagy a letöltési adathordozót fájlok to\from az Azure storage. 

>[!NOTE]
>hello lista engedély (AccessPermissions.List) nem használható az OnDemandOrigin lokátor létrehozása során. 

### <a name="storage-account"></a>Tárfiók
Minden hozzáférési tooAzure tárolási cseréjén keresztül történik egy tárfiókot. Egy Media Services-fiókját tárfiókok egy vagy több társíthatja. Egy fiók tartalmazhat egy korlátlan számú tárolót, mindaddig, amíg a teljes mérete a tárfiók 500TB.  A Media Services SDK szint tooling eszköz tooallow biztosít, toomanage több tárfiókot és a betöltés egyenleg hello terjesztési a eszközök során feltöltés toothese fiókok metrikák vagy véletlenszerű terjesztési alapján. További információ kapcsolatban a Working with [Azure Storage](https://msdn.microsoft.com/library/azure/dn767951.aspx). 

## <a name="jobs-and-tasks"></a>Feladatok és feladatok
A [feladat](https://https://docs.microsoft.com/rest/api/media/operations/job) általánosan használt tooprocess van (például index vagy kódolni) egy hang-és videófolyamot bemutató. Több videók feldolgozása esetén, hozzon létre egy feladatot az egyes videokártya toobe kódolású.

Egy feladat végre hello feldolgozási toobe kapcsolatos metaadatokat tartalmaz. Minden feladat tartalmaz egy vagy több [feladat](https://docs.microsoft.com/rest/api/media/operations/task)s, amely egy atomi feldolgozási feladatot, a bemeneti eszközeinek, adja meg a kimeneti eszközök, media processzort és a kapcsolódó beállításokat. Feladat feladatok is összeláncolt, ahol van megadva egy feladat hello kimeneti adategységen hello bemeneti eszköz toohello következő feladata. Ezzel a módszerrel egy feladat tartalmazhat összes hello feldolgozási adathordozó szükséges.

## <a id="encoding"></a>Kódolás
Az Azure Media Services hello kódolás hello felhőben adathordozó több lehetőséget biztosít.

Amikor kezdte meg a Media Services, fontos toounderstand hello különbségének kodekeket és a fájl formátuma nem.
A kodekeket hello szoftver, amely hello tömörítési/kitömörítés algoritmusok, mivel fájlformátumok tárolói tömörített hello videó tartsa.

A Media Services dinamikus becsomagolást, amely lehetővé teszi az adaptív sávszélességű MP4 vagy Smooth Streaming-kódolású tartalmak (MPEG DASH, HLS, Smooth Streaming) Media Services által támogatott streamformátumok toodeliver biztosít anélkül, hogy toore-csomagját ezek adatfolyam-továbbítási formátumokba.

tootake előnyeit [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md), igénylő tooencode a mezzanine (forrás) fájlt egy adaptív sávszélességű MP4-fájlokat vagy adaptív sávszélességű Smooth Streaming-fájlsorozattá készlete, és van legalább egy standard vagy prémium streamvégpont elindított állapotban.

A Media Services a következő cikkben ismertetett igény kódolók hello támogatja:

* [Media Encoder Standard](media-services-encode-asset.md#media-encoder-standard)
* [Media Encoder Premium-munkafolyamat](media-services-encode-asset.md#media-encoder-premium-workflow)

További információ a támogatott kódolók: [kódolók](media-services-encode-asset.md).

## <a name="live-streaming"></a>Élő adások online közvetítése
Azure Media Services a csatorna élő adatfolyam-tartalmak feldolgozására adatcsatorna jelöli. Egy csatorna élő bemeneti adatfolyamok megkapja az alábbi két módszer egyikével:

* Egy helyszíni élő kódolók többféle sávszélességű RTMP vagy Smooth Streaming (töredékes MP4) toohello csatorna. Használhatja a következő élő kódolók képesek, amelyek kimenete többféle sávszélességű Smooth Streaming hello: MediaExcel, Ateme, kommunikációs képzelhető el, Envivo, Cisco és elemi. hello következő élő kódolók képesek RTMP-kimenetre: Adobe Flash Live kódoló, Telestream Wirecast, Teradek, Haivision és Tricaster kódolók. hello feldolgozott adatfolyamok haladnak át csatornák további átkódolás és kódolás nélkül. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.
* Egy egyféle sávszélességű adatfolyamot (hello a következő formátumok egyikében: RTP (MPEG-TS)), RTMP vagy Smooth Streaming (töredékes MP4)) küldött toohello csatornán élő kódolás Media Services tooperform engedélyezve van. hello csatorna majd élő kódolás útján bejövő hello egyféle sávszélességű adatfolyamot tooa többféle sávszélességű (adaptív) video-adatfolyammá alakítja. Ha kérelem érkezik, a Media Services továbbítja hello adatfolyam toocustomers.

### <a name="channel"></a>Csatorna
A Media Services szolgáltatásban [csatorna](https://docs.microsoft.com/rest/api/media/operations/channel)s felelősek az élő adatfolyam-tartalmak feldolgozása. Egy csatornát biztosít a bemeneti végpontja (feldolgozó URL-CÍMÉT) tooa élő transcoder majd meg. hello csatorna hello élő transcoder bemeneti élő adatfolyamokat fogad, és lehetővé teszi az adatfolyamként történő egy vagy több Streamvégpontok keresztül. Csatornák egy előnézeti végpont (előzetes verzió URL-cím) toopreview használni, és érvényesítse a stream, mielőtt további feldolgozás és a szállítási is megadhatja.

Hello kaphat betöltési és hello kép URL-címe hello csatorna létrehozásakor. tooget URL-, hello csatorna nincs toobe lépések hello állapotban. Amikor készen áll a toostart kérdez le adatokat az élő transcoder történő hello csatorna, hello csatorna el kell indítani. Miután hello élő transcoder megkezdi az adatok bevitele, megtekintheti az adatfolyam.

Minden egyes Media Services-fiók több csatornát, több alkalmazás, és több Streamvégpontok tartalmazhat. Attól függően, hogy hello sávszélesség és a biztonsági igényeket StreamingEndpoint szolgáltatások lehet dedikált tooone és további csatornák. Bármely StreamingEndpoint is lekéréses bármely csatornán.

### <a name="program-event"></a>A program (esemény)
A [(esemény) Program](https://docs.microsoft.com/rest/api/media/operations/program) toocontrol hello közzétételét és tárolását élő stream szegmenseinek lehetővé teszi. Programok (esemény) a csatornák kezelik. hello csatornák és programok viszonya hasonló tootraditional adathordozó áll ahol csatorna állandó adatfolyam a tartalom és a program túllépte az időkorlátot hatókörön belüli toosome esemény adott csatornán.
Megadhatja a hello óraszámon belül hello program hello beállítása kívánt tooretain rögzített hello tartalom **ArchiveWindowLength** tulajdonság. Ez az érték 5 perc tooa legfeljebb 25 óra közötti állítható be.

ArchiveWindowLength is határozzák meg, hogy a hello maximális időtartam ügyfeleket is kérhet időben hello aktuális élő pozíciótól. Hosszabbak lehetnek hello megadott időtartamig, de a rendszer folyamatosan elveti azokat a tartalmakat, amelyek korábbiak a hello időtartamnál. Ez a tulajdonság értékének is meghatározza, hogy mennyi ideig hello ügyfél jegyzékfájljai milyen mértékben növelhető.

Minden program (esemény) társítva egy eszköz. létre kell hoznia egy lokátort hello toopublish hello program társított eszköz. Ez a lokátor lehetővé teszi a toobuild tooyour ügyfeleknek biztosítani tudják adatfolyam-továbbítási URL-címet.

Egy csatorna legfeljebb egyidejűleg futó programok, így létrehozhat több archívumot hello toothree támogat egy bejövő streamből. Ez lehetővé teszi toopublish kapcsolatos és archiválási különböző részei egy eseményt, igény szerint. Például az üzleti igény szerint tooarchive program, de toobroadcast 6 óra csak az elmúlt 10 perc. tooaccomplish, toocreate két egyidejűleg zajló program van szüksége. Egy program van beállítva a tooarchive hello esemény 6 óra, de hello program nem lesz közzétéve. hello másik program beállítva tooarchive 10 percig, és a program közzé van téve.

További információkért lásd:

* [Hogy vannak engedélyezve tooPerform élő kódolás az Azure Media Services csatornák használata](media-services-manage-live-encoder-enabled-channels.md)
* [Helyszíni kódolóktól többszörös átviteli sebességű streameket fogadó csatornák használata](media-services-live-streaming-with-onprem-encoders.md)
* [Kvóták és korlátozások](media-services-quotas-and-limitations.md).

## <a name="protecting-content"></a>Tartalom védelme
### <a name="dynamic-encryption"></a>A dinamikus titkosítás
Azure Media Services lehetővé teszi, hogy Ön toosecure hello idő keresztül tárhely, feldolgozás és kézbesítési elhagyják a adathordozókról. A Media Services lehetővé teszi a tartalom titkosított dinamikusan Advanced Encryption Standard (AES) (a 128 bites titkosítási kulcsok használatával) és a PlayReady és/vagy Widevine DRM segítségével közös titkosítás (CENC) toodeliver. A Media Services AES-kulcsok és a PlayReady-licencek tooauthorized ügyfelek kézbesítéséhez szolgáltatást is nyújt.

Jelenleg a következő formátumban hello titkosíthatók: HLS, MPEG DASH vagy Smooth Streaming. Progresszív letöltés nem titkosítható.

Ha azt szeretné, a Media Services tooencrypt egy eszköz tooassociate egy titkosítási kulcsot (CommonEncryption vagy EnvelopeEncryption) kell az eszközt, és engedélyezési házirendeket hello kulcs is konfigurálhatja.

Ha azt szeretné, hogy a tároló titkosított eszköz toostream, konfigurálnia kell hello adategység továbbítási házirendjét rendelés toospecify módjának toodeliver az objektumot.

Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, Media Services használ-e a megadott hello kulcs toodynamically titkosítása a tartalom egy boríték (az AES) vagy közös titkosítást (a PlayReady vagy Widevine). toodecrypt hello stream, hello player fog igényelni hello kulcs hello kulcs kézbesítési szolgáltatásból. hello felhasználónak van-e toodecide engedélyezett tooget hello kulcs, hello szolgáltatás értékeli az hello kulcshoz megadott hello engedélyezési házirendeket.

### <a name="token-restriction"></a>Token korlátozása
hello tartalomkulcs-hitelesítési házirend rendelkezhet egy vagy több engedélyezési korlátozás: Nyissa meg a, lexikális elem: korlátozás vagy IP-korlátozás. hello token korlátozott házirend által a Secure Token Service (STS) kiadott tokennek kell csatolni. A Media Services hello Simple Web Tokens (SWT) és JSON webes jogkivonat (JWT) formátumú tokeneket támogatja. A Media Services nem nyújt Secure Token szolgáltatásokat. Hozzon létre egy egyéni STS, vagy használja a Microsoft Azure ACS tooissue jogkivonatokat. hello STS kell lennie a megadott hello aláírt jogkivonat konfigurált toocreate hello token korlátozás a konfigurációban megadott kulcs és a probléma jogcímeket. kulcs kézbesítési szolgáltatás visszaadható hello (vagy licencelési) toohello ügyfél Ha hello jogkivonat érvényes, és hello kért hello Media Services-jogcímek az hello token megfelelő azokat konfigurált hello (vagy licencelési).

Hello token korlátozott házirend konfigurálásakor hello elsődleges hitelesítési kulcs, a kibocsátó és a célközönség paramétereket kell megadnia. hello elsődleges hitelesítési kulcsot tartalmazó hello kulcsfontosságú, hogy hello token lett aláírva, illetve kibocsátó hello biztonságos biztonságijogkivonat-szolgáltatás által kiállított hello jogkivonat. hello célközönség (más néven hatókör) hello token hello célját ismerteti, vagy hello erőforrás hello token engedélyezi a hozzáférést. hello Media Services kulcs kézbesítési szolgáltatás ellenőrzi, hogy ezek az értékek hello token értékekre, hello hello sablont.

További információkért tekintse meg a következő cikkek hello:

[Tartalom áttekintése védelme](media-services-content-protection-overview.md)
[védelem az AES-128](media-services-protect-with-aes128.md)
[védelem DRM az](media-services-protect-with-drm.md)

## <a name="delivering"></a>Továbbítása
### <a id="dynamic_packaging"></a>A dinamikus csomagolás
Media Services használata esetén ajánlott a mezzanine fájlokat egy adaptív sávszélességű MP4 állítsa be, és alakítsa át hello tooencode toohello kívánt formátum használatával hello beállítása [dinamikus becsomagolás](media-services-dynamic-packaging-overview.md).

### <a name="streaming-endpoint"></a>Adatfolyam-továbbítási végpontra
Egy StreamingEndpoint jelöli egy adatfolyam-tartalom közvetlenül tooa player ügyfélalkalmazást, vagy tooa Content Delivery Network (CDN) későbbi terjesztés (Azure Media Services most biztosít hello Azure CDN integrációs.) által biztosított szolgáltatás hello kimenő adatfolyam adatfolyam-továbbítási végpont szolgáltatásból egy élő adatfolyam, vagy a Media Services-fiók egy videó igény eszköz lehet. Válassza a Media Services-ügyfél egy **szabványos** streaming endpoint vagy egy vagy több **prémium** adatfolyam-végpontok tootheir igényeinek megfelelően. Adatfolyam-továbbítási végpontra szabvány az adatfolyam-továbbítási munkaterhelések többségéhez alkalmas. 

A szabványos streamvégpont a legtöbb streamelési feladat ellátására alkalmas. Standard adatfolyam-végpontok kínálnak hello rugalmasságot toodeliver a tartalom toovirtually dinamikus becsomagolás kifejezést HLS, MPEG-DASH, Smooth Streaming, valamint a dinamikus titkosítás Microsoft PlayReady-, Google Widevine-, Apple Fairplay keresztül minden eszköz , és az AES128.  Azok a nagyon kis toovery nagy célközönség Azure CDN integrálásán keresztül egyidejű megjelenítők százait is méretezhető. Ha egy speciális munkaterhelés vagy az adatfolyam-továbbítási kapacitásigények nem férnek el a streaming endpoint átviteli célok toostandard vagy toocontrol hello kapacitásának kívánt hello StreamingEndpoint szolgáltatás toohandle növekvő sávszélességű van szükség, ajánlott tooallocate méretezési egységeket (más néven prémium szintű adatfolyam-egységek).

Ajánlott toouse dinamikus becsomagolás és/vagy a dinamikus titkosítás.

>[!NOTE]
>Az AMS-fiók létrehozásakor egy **alapértelmezett** adatfolyam-továbbítási végpontra tooyour fiók kerül hello **leállítva** állapotát. a dinamikus csomagolás és a dinamikus titkosítás tartalmat, és hajtsa végre a megfelelő előnyeit streaming toostart hello streamvégpontra, amelyből el kívánja toostream tartalom toobe rendelkezik hello **futtató** állapotát. 

További információ [ebben](media-services-portal-manage-streaming-endpoints.md) a témakörben érhető el.

Alapértelmezés szerint a Media Services-fiók mentése too2 streamvégpontok is rendelkeznek. toorequest magasabb határérték, lásd: [kvóták és korlátozások](media-services-quotas-and-limitations.md).

Csak számlázása, amikor a StreamingEndpoint futó állapotban van.

### <a name="asset-delivery-policy"></a>Objektumtovábbítási szabályzat
A Media Services-továbbítási munkafolyamat konfigurálását hello lépések hello egyikét [továbbítási házirendjeit eszközök ](https://docs.microsoft.com/rest/api/media/operations/assetdeliverypolicy), amelyet az adatátvitel toobe. hogyan szeretné az eszköz toobe kézbesíteni a közli hello objektumtovábbítási szabályzat Media Services: az adatfolyam-továbbítási protokoll kell az eszköz dinamikusan csomagolható (például MPEG DASH, HLS, Smooth Streaming, vagy az összes), toodynamically szeretné-e az eszköz titkosítása és hogyan (boríték vagy közös titkosítási).

Ha egy tárolási titkosított eszközt, mielőtt az eszköz továbbítható, az adatfolyam-kiszolgáló eltávolítása hello tárolás titkosítása és adatfolyamokat a tartalom hello segítségével hello megadott objektumtovábbítási szabályzat. Például toodeliver az eszköz kulccsal titkosított Advanced Encryption Standard (AES) titkosítást, set hello házirend típusa tooDynamicEnvelopeEncryption. tooremove tárolás titkosítása és adatfolyam hello eszköz törlése, hello beállítása hello házirend típusa tooNoDynamicEncryption.

### <a name="progressive-download"></a>Progresszív letöltés
Progresszív letöltés lehetővé teszi a media játszott előtt hello teljes fájl letöltött toostart. MP4-fájlokat csak fokozatosan lehet letölteni.

>[!NOTE]
>Ha a számukra vissza kell fejtenie titkosított eszközök toobe progresszív letöltés érhető el.

felhasználók tooprovide a progresszív letöltési URL-cím, akkor először létre kell hoznia egy OnDemandOrigin lokátort. Hello lokátor, akkor hello alap elérési toohello eszköz által biztosított létrehozása. Akkor kell tooappend hello MP4-fájl nevét. Példa:

http://amstest1.Streaming.mediaservices.Windows.NET/3c5fe676-199c-4620-9b03-ba014900f214/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4

### <a name="streaming-urls"></a>Adatfolyam-továbbítási URL-címek
A tartalom tooclients Streaming. tooprovide felhasználók adatfolyam-továbbítási URL-ekkel, akkor először hozzon létre egy OnDemandOrigin lokátort. Hello lokátor, akkor hello toostream kívánt hello tartalmat tartalmazó alap elérési toohello eszköz által biztosított létrehozása. Azonban toobe képes toostream a tartalom toomodify az elérési út további van szüksége. a teljes URL-cím toohello adatfolyam-jegyzékfájlt, kell összefűzésére hello lokátor elérési tooconstruct érték és hello jegyzékfájl (filename.ism) neve. Ezt követően hozzáfűzése /Manifest és egy (ha szükséges) megfelelő formátumban toohello lokátor útvonalat.

Is SSL-kapcsolaton keresztül adatfolyam formájában a tartalmat. toodo, ellenőrizze, hogy a HTTPS adatfolyam-továbbítási URL-címek kezdődik. Jelenleg az AMS nem támogatja az SSL az egyéni tartomány.  

>[!NOTE]
>Akkor is csak adatfolyam SSL-en keresztül Ha streamvégpontra, amelyről a tartalmat továbbít hello 2014. szeptember 10 után készült. Ha a streamelési URL-címek adatfolyam-végpontok követően 10. szeptember hello alapul, hello URL-címet tartalmaz "streaming.mediaservices.windows.net" (hello új formátumban). Adatfolyam-továbbítási URL-címek, amelyek tartalmazzák a "origin.mediaservices.windows.net" (hello régi formátum) nem támogatja az SSL. Ha SSL-en keresztül szeretné toobe képes toostream hello régi formátumban kell megadni az URL-cím, hozzon létre egy új streamvégpont. Új streaming endpoint toostream a tartalom SSL-en keresztül hello alapján létre URL-címeket használnak.

hello a következő lista ismerteti a különböző formátumban, és a példákat mutat be:

* Smooth Streaming

{stream végpontjának neve-Media Services fiók neve}.streaming.mediaservices.windows.net/{kereső azonosítója}/{fájlnév}.ism/Manifest

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest

* MPEG DASH

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=mpd-time-csf)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=mpd-Time-CSF)

* Apple HTTP élő adatfolyam-továbbítási (HLS) V4

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl)

* Apple HTTP élő adatfolyam-továbbítási (HLS) V3

{streaming endpoint név-media services fiók name}.streaming.mediaservices.windows.net/{locator ID}/{filename}.ism/Manifest(format=m3u8-aapl-v3)

http://testendpoint-testaccount.Streaming.mediaservices.Windows.NET/fecebb23-46F6-490d-8b70-203e86b0df58/BigBuckBunny.ISM/manifest(Format=m3u8-aapl-v3)

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

