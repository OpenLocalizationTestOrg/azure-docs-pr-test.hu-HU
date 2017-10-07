---
title: "aaaMedia szolgáltatások kibocsátási megjegyzései |} Microsoft Docs"
description: "Médiaszolgáltatások kibocsátási megjegyzései"
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 3ca2d7af-1cf0-45fa-9585-3b73f3ee057d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: media
ms.devlang: dotnet
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.openlocfilehash: c365b1133987267173ec858298c4c6de62744946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-media-services-release-notes"></a>Az Azure Media Services kibocsátási megjegyzései
A kibocsátási megjegyzések összesítse a módosításokat a korábbi kiadásokban és ismert problémákat.

> [!NOTE]
> Azt szeretné, hogy ügyfeleink toohear pedig a előforduló problémák elhárításához. probléma tooreport vagy kérdései vannak, tegye a hello [Azure Media Services MSDN fórumon].
> 
> 

## <a id="issues"></a>Ismert problémák
### <a id="general_issues"></a>Media Services kapcsolatos általános problémák
| Probléma | Leírás |
| --- | --- |
| Több, közös HTTP-fejlécek nem találhatók: hello REST API-t. |Hello REST API-t használó Media Services-alkalmazások fejlesztése, ha úgy gondolja, hogy néhány közös HTTP-fejléc mező (beleértve CLIENT-REQUEST-ID-azonosító, és VISSZATÉRÉSI-CLIENT-REQUEST-ID) nem támogatottak. hello fejlécek az egy későbbi kiadásban lesz hozzáadva. |
| Százalék-kódolás nem engedélyezett. |Media Services hello hello IAssetFile.Name tulajdonság értékét használja, amikor a hello adatfolyam-tartalmat (például http://{AMSAccount}.origin.mediaservices.windows.net/{GUID}/{IAssetFile.Name}/streamingParameters.) URL-címek kiépítéséhez Emiatt százalék-kódolás nem engedélyezett. hello értékének hello **neve** tulajdonság nem lehet hello következő [százalék kódolás-fenntartott karakterek](http://en.wikipedia.org/wiki/Percent-encoding#Percent-encoding_reserved_characters):! *' ();: @& = + $, /? % # [] ". Emellett csak lehet egy "." hello fájlnévkiterjesztés. |
| szolgáltatás részét képező ListBlobs metódus hello hello Azure Storage szolgáltatás SDK verzió 3.x sikertelen lesz. |A Media Services állít elő, SAS URL-címek alapján hello [2012-02-12](https://docs.microsoft.com/rest/api/storageservices/Version-2012-02-12) verziója. Ha azt szeretné, hogy toouse Azure Storage szolgáltatás SDK toolist blobot, amely egy blob-tároló, használja a hello [CloudBlobContainer.ListBlobs](http://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.cloudblobcontainer.listblobs.aspx) módszer, amelynek része az Azure Storage szolgáltatás SDK verzió 2.x. szolgáltatás részét képező ListBlobs metódus hello hello Azure Storage szolgáltatás SDK verzió 3.x sikertelen lesz. |
| Media Services-szabályozás mechanizmus korlátozza az alkalmazások, amelyek túl sok kérelem toohello szolgáltatás hello erőforrás-felhasználása. hello szolgáltatás hello szolgáltatás nem érhető el (503-as) HTTP-állapotkód adhatnak vissza. |További információkért lásd: hello 503-as HTTP-állapotkód hello hello leírása [Azure Media Services hibakódok](media-services-encoding-error-codes.md) témakör. |
| Entitások lekérdezésekor korlátozás van adja vissza egy időben, mert a nyilvános REST v2 korlátozza a lekérdezési eredmények too1000 eredmények 1000 entitások. |Toouse kell **kihagyása** és **érvénybe** (.NET) / **felső** (REST) leírtak szerint [.NET példában](media-services-dotnet-manage-entities.md#enumerating-through-large-collections-of-entities) és [a REST API Példa](media-services-rest-manage-entities.md#enumerating-through-large-collections-of-entities). |
| Egyes ügyfelek között egy ismétlődő címke probléma hello Smooth Streaming jegyzékben származhatnak. |További információ [itt](media-services-deliver-content-overview.md#known-issues) érhető el. |
| Az Azure Media Services .NET SDK-objektum nem szerializálható, és emiatt sem működik együtt az Azure-gyorsítótárazás. |Ha megpróbál tooserialize hello SDK AssetCollection objektum tooadd azt tooAzure gyorsítótárazását, kivétel történt. |
| Kódolási feladatok sikertelenek üzenet karakterláncot "szakasz: DownloadFile. Kód: System.NullReferenceException ". |hello tipikus kódolási munkafolyamat tooupload bemeneti videó (oka) t tooan bemeneti eszköz, és küldje el egy vagy több kódolási feladat az adott eszköz, adjon meg további a módosítása nélkül, amely a bemeneti eszköz. Azonban ha módosítja hello bemeneti az eszköznek (például hozzáadása/törlés/fájlok átnevezése hello eszköz belül), majd azt követő feladatok sikertelenek lehetnek DownloadFile hiba miatt. hello megoldás, toodelete hello bemeneti eszköz, és töltse fel újra (oka) t tooa bemeneti új eszköz. |

## <a id="rest_version_history"></a>REST API verziójának előzményei
További információ a Media Services REST API verziójának előzményei hello: [Azure Media Services REST API-referencia].

## <a name="june-2017-release"></a>2017. június kiadás

Most már támogatja a Media Services [Azure Active Directory (Azure AD)-alapú hitelesítés](media-services-use-aad-auth-to-access-ams-api.md).

> [!IMPORTANT]
> Jelenleg a Media Services hello Azure Access Control service hitelesítési modellt támogatja. Azonban a 2018. június 1 elavulttá válik hozzáférés-vezérlés engedélyt. Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.

## <a name="march-2017-release"></a>2017. március kiadás

Ezután már használhatja Azure Media Standard túl[automatikus létrehozása egy sávszélességű létra](media-services-autogen-bitrate-ladder-with-mes.md) "Adaptív Streameléshez" hello beállított karakterlánc egy kódolási feladat létrehozásakor megadásával. "Adaptív adatfolyam" hello javasolt készlet, ha azt szeretné, tooencode videó az adatfolyamként történő Media Services. Ha az adott forgatókönyv szerint kell kódolással beállított toocustomize, megkezdheti a [ezek](media-services-mes-presets-overview.md) készletek.

Ezután már használhatja Azure Media Standard vagy a Media Encoder prémium munkafolyamat túl[állít elő, fMP4 adattömbök kódolási feladat létrehozása](media-services-generate-fmp4-chunks.md). 


## <a name="febuary-2017-release"></a>Febuary 2017 kiadás

2017. április 1., kezdési bármely feladat rekord 90 napnál régebbi fiókja automatikusan törlődik, a társított feladat bejegyzéseket, valamint akkor is, ha hello rekordok teljes száma nem éri el hello kvóta felső határát. Ha tooarchive hello/feladat tájékoztatásra van szüksége, használhatja a leírt hello kód [Itt](media-services-dotnet-manage-entities.md).

## <a name="january-2017-release"></a>2017. január kiadás

A Microsoft Azure Media Services (AMS), egy **Streamvégponton** egy adatfolyam-szolgáltatás által biztosított tartalom közvetlenül tooa player ügyfélalkalmazást, vagy későbbi terjesztés Content Delivery Network (CDN) tooa jelöli. A Media Services is biztosít az Azure CDN való zökkenőmentes integrációt. élő stream, igény szerint, vagy az eszköz a Media Services-fiók a progresszív letöltés videó hello kimenő adatfolyam egy StreamingEndpoint szolgáltatásból lehet. Minden Azure Media Services-fiók egy alapértelmezett StreamingEndpoint tartalmazza. További Streamvégpontok hello fiókkal hozhatók létre. Streamvégpontok, 1.0-s és 2.0 két verziója van. 10 2017. január verziótól kezdődően az újonnan létrehozott AMS számlákat tartalmazza 2.0-s verziójának **alapértelmezett** StreamingEndpoint. További végpontok hozzáadása toothis fiók streaming is 2.0-s verziójában. Ez a változás nem befolyásolja a meglévő fiókokat hello; meglévő Streamvégpontok 1.0-s verziója és frissített tooversion 2.0 lehet. A módosítás nem lesznek viselkedés, a számlázásra és a szolgáltatás módosításokat (további információkért lásd: [ez](media-services-streaming-endpoints-overview.md) témakör).

Ezenkívül hello 2.15 verziójától kezdve Azure Media Services hozzá a következő tulajdonságok toohello Streamvégponton entitás hello: **CdnProvider**, **CdnProfile**,  **FreeTrialEndTime**, **StreamingEndpointVersion**. Részletes megtudhatja, ezeket a tulajdonságokat, [ez](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint). 

## <a name="december-2016-release"></a>2016. december kiadás

Az Azure Media Services most lehetővé teszi a hozzá tartozó szolgáltatások tooaccess telemetriai/metrikai adatok. AMS hello jelenlegi verziója lehetővé teszi az élő csatorna, StreamingEndpoint, telemetriai adatokat gyűjt, és a működés közbeni archív entitások. További információ [ebben](media-services-telemetry-overview.md) a témakörben érhető el.

## <a id="july_changes16"></a>2016. július kiadás
### <a name="updates-toomanifest-file-ism-generated-by-encoding-tasks"></a>Frissítések toomanifest fájl (*. ISM) generálja a kódolási feladatokhoz
Ha egy kódolási feladat elküldött tooMedia Encoder Standard vagy az Azure Media Encoder, hello kódolási feladat hoz létre egy [adatfolyam jegyzékfájl](media-services-deliver-content-overview.md) (* .ism) fájl hello a kimeneti eszköz. Hello legújabb szolgáltatás kiadástól kezdve a folyamatos átviteli jegyzékfájl hello szintaxis frissítve lett.

> [!NOTE]
> adatfolyam-jegyzékfájl (.ism) fájl hello hello szintaxisát belső használatra fenntartva, és a későbbi kiadásokban tulajdonos toochange. Adjon nem módosíthatók, ez a fájl tartalmát hello kezelheti.
> 
> 

### <a name="a-new-client-manifest-ismc-file-is-generated-in-hello-output-asset-when-an-encoding-task-outputs-one-or-more-mp4-files"></a>Egy új ügyfél jegyzékfájl (*. ISMC) fájl jön létre a hello eszköz kimenet, amikor egy kódolási feladatok kimenete egy vagy több MP4-fájlok
Hello legújabb szolgáltatás kiadása kezdve egy kódolási feladat, amely hoz létre egy további MP4-fájlok, hello befejezése után hello kimeneti, eszköz is tartalmazni fog egy adatfolyam-továbbítási ügyfél jegyzékfájl (*.ismc) fájlt. hello .ismc fájl javítja hello teljesítmény, a dinamikus adatfolyamként való továbbítására. 

> [!NOTE]
> hello szintaxis hello ügyfél jegyzékfájl (.ismc) fájl belső használatra fenntartva, és a későbbi kiadásokban tulajdonos toochange. Adjon nem módosíthatók, ez a fájl tartalmát hello kezelheti.
> 
> 

További információkért lásd: [ez](https://blogs.msdn.microsoft.com/randomnumber/2016/07/08/encoder-changes-within-azure-media-services-now-create-ismc-file/) blog.

### <a name="known-issues"></a>Ismert problémák
Egyes ügyfelek között egy ismétlődő címke probléma hello Smooth Streaming jegyzékben származhatnak. További információ [itt](media-services-deliver-content-overview.md#known-issues) érhető el.

## <a id="apr_changes16"></a>2016. április kiadás
### <a name="azure-media-analytics"></a>Az Azure Médiaelemzés használatával
Az Azure Media Servces Azure Médiaelemzés használatával hatékony videó eszközintelligencia bevezetni. Részletes információkért lásd: [Azure Media Services elemző áttekintése](media-services-analytics-overview.md).

### <a name="apple-fairplay-preview"></a>Apple FairPlay (előzetes verzió)
Az Azure Media Services most meg toodynamically titkosítása a HTTP Live Streaming (HLS) lehetővé teszi, hogy az Apple FairPlay tartalom. AMS licenc kézbesítési szolgáltatás toodeliver FairPlay licencek tooclients is használható. További részletekért lásd: [használata Azure Media Services tooStream a HLS tartalom védelme a Apple FairPlay ](media-services-protect-hls-with-fairplay.md).

## <a id="feb_changes16"></a>2016. februári kiadás
Azure Media Services SDK for .NET (3.5.3) legújabb verziójának hello Widevine kapcsolódó hibajavítás tartalmazza. hello hiba történt: AssetDeliveryPolicy nem használható fel újra, több eszköz szolgáltatással végzett Widevine titkosítása. A hibajavítás hello részeként a következő tulajdonság hozzá lett adva toohello SDK: **WidevineBaseLicenseAcquisitionUrl**.

    Dictionary<AssetDeliveryPolicyConfigurationKey, string> assetDeliveryPolicyConfiguration =
        new Dictionary<AssetDeliveryPolicyConfigurationKey, string>
    {
        {AssetDeliveryPolicyConfigurationKey.WidevineBaseLicenseAcquisitionUrl,"http://testurl"},

    };

## <a id="jan_changes_16"></a>2016. január kiadás
Kódolási fenntartott egységek kódoló nevek tooreduce összetéveszthető átnevezték.

hello Basic, Standard és Premium kódolási fenntartott egységek átnevezett tooS1, S2, és S3 szolgáltatás számára fenntartott egység, illetve.  Alapszintű kódolás RUs ma használó ügyfelek látni fogják S1 hello címke Azure Portal (és hello számlázási) során Standard és Premium rendre hello címkék S2 és S3 fog látni. 

## <a id="dec_changes_15"></a>2015. decemberi kiadása

### <a name="azure-media-encoder-deprecation-announcement"></a>Az Azure Media Encoder érvénytelenítése bejelentés

Az Azure Media Encoder megszűnnek, körülbelül 12 hónapon Media Encoder Standard hello kiadása kezdve.

### <a name="azure-sdk-for-php"></a>Azure SDK a PHP-hoz
hello Azure SDK team közzétett hello új kiadása [Azure SDK for PHP](http://github.com/Azure/azure-sdk-for-php) csomagot, amely tartalmazza a frissítéseket és új funkciók a Microsoft Azure Media Services szolgáltatásokhoz. Különösen hello Azure Media Services SDK PHP mostantól támogatja a legújabb hello [védelmi tartalom](media-services-content-protection-overview.md) szolgáltatások: a dinamikus titkosítás AES és DRM (PlayReady és Widevine) és a Token korlátozás nélkül. Azt is támogatja a skálázás [kódoláshoz egységek](media-services-dotnet-encoding-units.md).

További információkért lásd:

* Hello [Microsoft Azure Media Services SDK for PHP](http://southworks.com/blog/2015/12/09/new-microsoft-azure-media-services-sdk-for-php-release-available-with-new-features-and-samples/) blog.
* hello következő [Kódminták](http://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices) toohelp első lépések megtételéhez gyorsan:
  * **vodworkflow_aes.php**: a PHP-fájl, amely bemutatja, hogyan toouse AES-128, a dinamikus titkosítás és a kulcs továbbítási szolgáltatást hozhat létre. Tekintse meg a hello .NET minta alapul [ez](media-services-protect-with-aes128.md) cikk.
  * **vodworkflow_aes.php**: a PHP-fájl, amely bemutatja, hogyan toouse dinamikus titkosítást a PlayReady és Licenctovábbítási szolgáltatásra. Tekintse meg a hello .NET minta alapul [ez](media-services-protect-with-drm.md) cikk.
  * **scale_encoding_units.php**: a PHP-fájl, amely bemutatja, hogyan tooscale kódolás szolgáltatás számára fenntartott egység.

## <a id="nov_changes_15"></a>2015. november kiadás
Az Azure Media Services licenctovábbítási szolgáltatása Google Widevine hello felhőben kínál. További részletekért lásd a túl[a közlemény blog](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/). Lásd még [ebben az oktatóanyagban](media-services-protect-with-drm.md) és [GitHub-tárházban](http://github.com/Azure-Samples/media-services-dotnet-dynamic-encryption-with-drm). 

Vegye figyelembe, hogy Azure Media Sevices által biztosított Widevine licenctovábbítási szolgáltatások Preview. További információ: [ebben a blogban](https://azure.microsoft.com/blog/announcing-google-widevine-license-delivery-services-public-preview-in-azure-media-services/).

## <a id="oct_changes_15"></a>2015 október kiadás
Az Azure Media Services (AMS) mostantól a következő adatközpontokban hello élő: Dél-Brazília, Nyugat-India, Dél-India és közép-India. Ezután már használhatja az Azure portál hello túl[Media Services-fiókok létrehozása](media-services-portal-create-account.md) és leírt különböző feladatok elvégzésére [Itt](https://azure.microsoft.com/documentation/services/media-services/). Azonban ezekben az adatközpontokban a Live Encoding nem támogatott. Emellett ezekben az adatközpontokban a kódoláshoz fenntartott egységeknek sem minden típusa érhető el.

* Dél-Brazília: Csak a standard és alapszintű kódoláshoz fenntartott egységek érhetők el
* Nyugat-, Dél- és Közép-India: Csak az alapszintű kódoláshoz fenntartott egységek érhetők el

## <a id="september_changes_15"></a>2015. szeptemberi kiadás
* AMS most ajánlatok hello képességét tooprotect Video-On-Demand (VOD) és az élő adatfolyamok moduláris Widevine DRM technológiával is. Használhatja a következő kézbesítési szolgáltatások partnerek toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/). További információkért lásd: [ebben a blogban](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/).
  
    Használhat [AMS .NET SDK](https://www.nuget.org/packages/windowsazure.mediaservices/) (3.5.1 hello verziójával kezdődően) vagy a REST API tooconfigure az AssetDeliveryConfiguration toouse Widevine.  
* AMS Apple ProRes videók támogatása. A forrás QuickTime-videók Apple ProRes vagy más használó fájlok most már feltöltheti. További információkért lásd: [ebben a blogban](https://azure.microsoft.com/blog/announcing-support-for-apple-prores-videos-in-azure-media-services/).
* Most már használhatja a Media Encoder Standard toodo részterv levágott és élő archív kiolvasásához. További információkért lásd: [ebben a blogban](https://azure.microsoft.com/blog/sub-clipping-and-live-archive-extraction-with-media-encoder-standard/).
* a következő szűrési frissítések hello történtek: 
  
  * Csak szűrővel most már használhatja az Apple HTTP Live Streaming (HLS) formátumban. A frissítés lehetővé teszi tooremove csak követése megadásával (csak = false) hello URL-címben.
  * Az eszközök szűrők meghatározásakor most már rendelkezik képességét toocombine több (felfelé too3) szűrése a egyetlen URL-címében.
    
    További információ: [ez](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.
* AMS I-keretek mostantól támogatja a HLS v4. I-keret támogatási előretekerés és visszatekerés műveletek optimalizálja. Alapértelmezés szerint az összes HLS v4 kimenetének I-keret lista (EXT-X-I-FRAME-STREAM-INF) tartalmazza.
  
    További információ: [ez](https://azure.microsoft.com/blog/azure-media-services-release-dynamic-manifest-composition-remove-hls-audio-only-track-and-hls-i-frame-track-support/) blog.

## <a id="august_changes_15"></a>2015. augusztus kiadás
* Az Azure Media Services SDK Java V0.8.0 kiadás és az új mintát is elérhető. További információkért lásd:
  
  * [Blogbejegyzés](http://southworks.com/blog/2015/08/25/microsoft-azure-media-services-sdk-for-java-v0-8-0-released-and-new-samples-available/)
  * [Java-tárház minták](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
* Az Azure Media Player frissítés több hangadatfolyam támogatásával. További információkért lásd:
  * [Blogbejegyzés](https://azure.microsoft.com/blog/2015/08/13/azure-media-player-update-with-multi-audio-stream-support/)

## <a id="july_changes_15"></a>2015. július kiadás
* Hello általános rendelkezésre állását Media Encoder Standard bejelentése. További információkért lásd: [ebben a blogbejegyzésben](https://azure.microsoft.com/blog/2015/07/16/announcing-the-general-availability-of-media-encoder-standard/).
  
    Media Encoder Standard használ ismertetett készletek [ez](http://go.microsoft.com/fwlink/?LinkId=618336) szakasz. Vegye figyelembe, hogy használatakor egy előre definiált a 4 KB-os kódolja szerezheti be hello **prémium** szolgáltatás számára fenntartott egység típusát. További információkért lásd: [hogyan tooScale kódolás](media-services-scale-media-processing-overview.md).
* Élő és az Azure Media Services Player valós idejű feliratok. További információkért lásd: [ebben a blogbejegyzésben](https://azure.microsoft.com/blog/2015/07/08/live-real-time-captions-with-azure-media-services-and-player/)

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések
Az Azure Media Services .NET SDK verziója most 3.4.0.0. a következő funkciók hello ebben a kiadásban lett hozzáadva:  

* Élő archív megvalósított támogatása. Vegye figyelembe, hogy egy élő archív tartalmazó objektumot nem lehet letölteni.
* Dinamikus szűrők megvalósított támogatása.
* Megvalósított funkció, amely lehetővé teszi a felhasználók tookeep tároló eszköz törlése közben.
* Hibajavítások házirendjeinek tooretry csatornák.
* Engedélyezett **Media Encoder prémium munkafolyamat**.

## <a id="june_changes_15"></a>2015. június kiadás
### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések
Az Azure Media Services .NET SDK verziója most 3.3.0.0. a következő funkciók hello ebben a kiadásban lett hozzáadva:  

* OpenId Connect felderítési spec támogatása
* támogatott kezelése az identity provider oldalán kulcsok kulcsváltás. 

Ha használja az identitásszolgáltató, amely felfedi a felderítési dokumentum OpenID Connect (mint hello következő szolgáltatók tegye: Azure Active Directoryban, Google, Salesforce), a JWT jogkivonat érvényességének aláírókulcsok Azure Media Services tooobtain találhatók OpenID connect felderítési specifikációi. 

További információkért lásd: [használatával Json webes kulcsokat felderítési fájlmegadásában toowork OpenID Connect jwt-t az Azure Media Services hitelesítési lexikális elem](http://gtrifonov.com/2015/06/07/using-json-web-keys-from-openid-connect-discovery-spec-to-work-with-jwt-token-authentication-in-azure-media-services/).

## <a id="may_changes_15"></a>2015. május kiadás
A következő új szolgáltatások hello bejelentése:

* [A Media Services élő kódolás előnézete](media-services-manage-live-encoder-enabled-channels.md)
* [Dinamikus jegyzék](media-services-dynamic-manifest-overview.md)
* [Azure Media Hyperlapse media processzor előnézete](https://azure.microsoft.com/blog/?p=286281&preview=1&_ppp=61e1a0b3db)

## <a id="april_changes_15"></a>2015. áprilisi kiadás
### <a name="general-media-services-updates"></a>Általános Media Services-frissítések
* [Az Azure Media Player bejelentése](https://azure.microsoft.com/blog/2015/04/15/announcing-azure-media-player/).
* Media Services REST 2.10, konfigurált tooingest RTMP protokollként csatornákat kezdve jönnek létre, az elsődleges és másodlagos betöltési URL-címeket. További információkért lásd: [csatorna betöltési konfigurációk](media-services-live-streaming-with-onprem-encoders.md#channel_input)
* Az Azure Media Indexer frissítések
* Spanyol nyelvi támogatása
* Új konfigurációs XML-formátuma

További információ: [ebben a blogban](https://azure.microsoft.com/blog/2015/04/13/azure-media-indexer-spanish-v1-2/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések
Az Azure Media Services .NET SDK verziója most 3.2.0.0.

hello közé tartoznak a következők hello ügyfél elérhető frissítések:

* **Változás megtörje**: megváltozott **TokenRestrictionTemplate.Issuer** és **TokenRestrictionTemplate.Audience** toobe karakterlánc típusú.
* Frissítések házirendjeinek toocreating egyéni próbálkozzon újra.
* A kapcsolódó fájlokat toouploading/letöltése hibajavításokat tartalmaz.
* Hello **MediaServicesCredentials** osztály most elfogad elsődleges és másodlagos hozzáférést vezérlő végpont tooauthenticate ellen.

## <a id="march_changes_15"></a>2015. márciusi kiadás
### <a name="general-media-services-updates"></a>Általános Media Services-frissítések
* A Media Services most Azure CDN integrációt biztosít. toosupport hello integrációs, hello **CdnEnabled** tulajdonság hozzá lett adva túl**StreamingEndpoint**.  **CdnEnabled** együtt 2.9 verziójától kezdve REST API-k (további információkért lásd: [StreamingEndpoint](https://docs.microsoft.com/rest/api/media/operations/streamingendpoint)).  **CdnEnabled** 3.1.0.2 verziójától kezdve a .NET SDK-val használható (további információkért lásd: [StreamingEndpoint](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.mediaservices.client.istreamingendpoint\(v=azure.10\).aspx)).
* A közlemény **Media Encoder prémium munkafolyamat**. További információkért lásd: [prémium szintű kódolás Introducing Azure Media Services](https://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services/).

## <a id="february_changes_15"></a>2015. február kiadás
### <a name="general-media-services-updates"></a>Általános Media Services-frissítések
Media Services REST API verziója most 2.9. Ezen verziójával kezdődően hello streamvégpontok Azure CDN integráció engedélyezéséhez. További információkért lásd: [StreamingEndpoint](https://msdn.microsoft.com/library/dn783468.aspx).

## <a id="january_changes_15"></a>2015. januári kiadás
### <a name="general-media-services-updates"></a>Általános Media Services-frissítések
Az általános rendelkezésre állási (GA) a dinamikus titkosítás tartalom védelmi közlemény. További információkért lásd: [Azure Media Services javítja a folyamatos átviteli biztonsági DRM általános rendelkezésre állási technológiával](https://azure.microsoft.com/blog/2015/01/29/azure-media-services-enhances-streaming-security-with-general-availability-of-drm-technology/).

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések
Az Azure Media Services .NET SDK verziója most 3.1.0.1.

Ebben a kiadásban megjelölve hello alapértelmezett Microsoft.WindowsAzure.MediaServices.Client.ContentKeyAuthorization.TokenRestrictionTemplate konstruktor elavultak. hello új konstruktor készít TokenType argumentumként.

    TokenRestrictionTemplate template = new TokenRestrictionTemplate(TokenType.SWT);


## <a id="december_changes_14"></a>2014. decemberi kiadása
### <a name="general-media-services-updates"></a>Általános Media Services-frissítések
* Egyes frissítések és az új szolgáltatások hozzáadott toohello Azure indexelő Media processzor. További információkért lásd: [Azure Media Indexer verzió 1.1.6.7 kibocsátási megjegyzések](https://azure.microsoft.com/blog/2014/12/03/azure-media-indexer-version-1-1-6-7-release-notes/).
* Egy új REST API, amely lehetővé teszi a kódoláshoz fenntartott egység tooupdate hozzáadott: [EncodingReservedUnitType többi](https://docs.microsoft.com/rest/api/media/operations/encodingreservedunittype).
* A hozzáadott CORS-támogatás kulcs kézbesítési szolgáltatás.
* Teljesítménnyel kapcsolatos fejlesztések az engedélyezési házirend-beállítások lekérdezése volt történik.
* Kínai adatközpontban hello [kulcs kézbesítési URL-cím](https://docs.microsoft.com/rest/api/media/operations/contentkey#get_delivery_service_url) már egy ügyfél (akárcsak más adatközpontokban).
* A hozzáadott HLS automatikus cél időtartama. Ennek az élő Stream továbbítása során, amikor HLS mindig dinamikusan csomagolni. Alapértelmezés szerint a Media Services automatikusan HLS szegmens csomagolás arány (FragmentsPerSegment) hello keyframe időköz (KeyFrameInterval), más néven tooas képek csoport – GOP, hello élő kódoló érkezett alapján számítja ki. További információkért lásd: [működik-e az Azure Media Services élő adatfolyam].

### <a name="media-services-net-sdk-updates"></a>Media Services .NET SDK frissítések
* [Az Azure Media Services .NET SDK](http://www.nuget.org/packages/windowsazure.mediaservices/) verziója most 3.1.0.0-s.
* Frissített .net SDK hello függőségi too.NET 4.5 keretrendszert.
* Egy olyan új API, amely lehetővé teszi a tooupdate, kódoláshoz fenntartott egység hozzá. További információkért lásd: [fenntartott egységtípus frissítése és növelése kódolás RUs .NET használatával](media-services-dotnet-encoding-units.md).
* (JSON Web Token) hozzáadott JWT jogkivonat hitelesítés támogatása. További információkért lásd: [JWT jogkivonat hitelesítés az Azure Media Services és a dinamikus titkosítás](http://www.gtrifonov.com/2015/01/03/jwt-token-authentication-in-azure-media-services-and-dynamic-encryption/).
* A hozzáadott relatív eltolások BeginDate és ExpirationDate hello PlayReady licenc sablonban.

## <a id="november_changes_14"></a>2014. novemberi kiadásban
* A Media Services mostantól lehetővé teszi egy élő Smooth Streaming (FMP4) tartalom tooingest SSL-kapcsolaton keresztül. tooingest SSL, győződjön meg arról, hogy tooupdate hello betöltési URL-cím tooHTTPS.  Vegye figyelembe, hogy jelenleg AMS nem támogatja az SSL az egyéni tartomány.  További információ az élő Stream továbbítása: [működik-e az Azure Media Services élő adatfolyam].
* Jelenleg egy RTMP élő adatfolyam nem betöltési SSL-kapcsolaton keresztül.
* Akkor is csak adatfolyam SSL-en keresztül Ha streamvégpontra, amelyről a tartalmat továbbít hello 2014. szeptember 10 után készült. Ha a streamelési URL-címek adatfolyam-végpontok követően 10. szeptember hello alapul, hello URL-címet tartalmaz "streaming.mediaservices.windows.net" (hello új formátumban). Adatfolyam-továbbítási URL-címek, amelyek tartalmazzák a "origin.mediaservices.windows.net" (hello régi formátum) nem támogatja az SSL. Ha hello régi formátumban kell megadni az URL-cím és toobe képes toostream SSL,-en keresztül [hozzon létre egy új streamvégpont](media-services-portal-manage-streaming-endpoints.md). Új streaming endpoint toostream a tartalom SSL-en keresztül hello alapján létre URL-címeket használnak.

## <a id="october_changes_14"></a>2014. októberi kiadás
### <a id="new_encoder_release"></a>Media Services-kódoló kiadás
Media Services Azure Media Encoder új kiadása hello bejelentése. A hello van csak szó, a legújabb Azure Media Encoder kimenet GB-ban, de egyébként hello új kódoló kompatibilis a korábbi kódoló hello szolgáltatást. További információ [Media Services díjszabása]).

### <a id="oct_sdk"></a>Media Services .NET SDK-val
Media Services SDK for .NET-bővítmények verziója most 2.0.0.3.

Media Services SDK for .NET verziója most 3.0.0.8.

hello a következő változások történtek:

* Újrabontása újrapróbálkozási házirend osztályokat.
* Felhasználói ügynök karakterlánca toohttp kérelem fejlécek hozzáadását.
* Nuget visszaállítási összeállítása lépés felvételéhez.
* Forgatókönyv tesztek toouse x509 cert kijavítása a tárházból.
* Beállítások ellenőrzése csatorna frissítésekor, és adatfolyam-célból.

### <a name="new-github-repository-toohost-media-services-samples"></a>Új GitHub tárház toohost Media Services minták
Minták találhatók [Azure Media Services minták GitHub-tárházban](https://github.com/Azure/Azure-Media-Services-Samples).

## <a id="september_changes_14"></a>2014. szeptember kiadás
Media Services REST metaadatok verziója most 2.7. Hello legújabb REST frissítésével kapcsolatos további információkért lásd: [Azure Media Services REST API-referencia].

Media Services SDK for .NET verziója most 3.0.0.7

### <a id="sept_14_breaking_changes"></a>Módosítások megszakítása
* **Forrás** túl át lett nevezve[StreamingEndpoint].
* Hello alapértelmezett működése hello használata esetén megváltozik, **Azure-portálon** tooencode, majd tegye közzé az MP4-fájlokat.

Korábban, hello klasszikus Azure portál toopublish használata esetén egy egyfájlos MP4 video asset SAS URL-címet hozhatók létre (SAS URL-címek lehetővé teszik toodownload hello egy blobtárolóból videó). Jelenleg Ha hello klasszikus Azure portál tooencode használja, majd tegye közzé a egyfájlos MP4 videó eszköz, a hello URL-cím pontok tooan Azure Media Services adatfolyam-továbbítási végpontra jönnek létre.  Ez a változás nincs hatással közvetlenül feltöltött tooMedia szolgáltatást, és az Azure Media Services által kódolás nélkül közzétett MP4-videók.

Jelenleg a következő két beállítások toosolve hello probléma hello rendelkezik.

* Engedélyezze a folyamatos átviteli egységeket, és a dinamikus becsomagolás toostream hello .mp4 eszköz zavartalan adatfolyam bemutató használja.
* Hozzon létre egy SAS URL-cím toodownload (vagy fokozatosan lejátszása) hello .mp4. További információ a SAS-kereső toocreate lásd: [tartalom továbbítása].

### <a id="sept_14_GA_changes"></a>Új funkciók/forgatókönyvek GA kiadás részét képező
* **Az indexelő Media processzor**. További információ: [médiafájlok indexelése az Azure Media Indexer].
* Hello [StreamingEndpoint] entitás mostantól lehetővé teszi tooadd egyéni (gazda) tartományneve.
  
    Egy egyéni tartomány nevét toobe hello Media Services adatfolyam-végpont nevét használja meg kell tooadd egyéni állomást nevek tooyour streamvégpontra. Hello Media Services REST API-k vagy a .NET SDK tooadd egyéni állomásneveket használja.
  
    a következő szempontok hello vonatkoznak:
  
  * Egyéni tartománynév hello hello tulajdonjogát kell rendelkeznie.
  * Azure Media Services által is ellenőrizni kell hello tartománynév hello tulajdonjogát. toovalidate hello tartomány, hozzon létre egy CNAME rekordot, amely leképezhető <MediaServicesAccountId>.<parent domain> tooverifydns. < mediaservices-dns-zóna >. 
  * Létre kell hoznia egy másik olyan CNAME rekordot, amely leképezhető hello egyéni gazdagép nevét (például sports.contoso.com) tooyour Media Services StreamingEndpont a gazdagép nevét (például amstest.streaming.mediaservices.windows.net).

    További információkért lásd: hello **CustomHostNames** hello tulajdonság [StreamingEndpoint] témakör.

### <a id="sept_14_preview_changes"></a>Új funkciók/forgatókönyvek hello nyilvános előzetes kiadás részét képező
* Élő adatfolyam-továbbítási minta. További információkért lásd: [működik-e az Azure Media Services élő adatfolyam].
* Kulcs kézbesítési szolgáltatás. További információkért lásd: [AES-128 a dinamikus titkosítás segítségével és a kulcs kézbesítési szolgáltatás].
* A dinamikus titkosítás AES. További információkért lásd: [AES-128 a dinamikus titkosítás segítségével és a kulcs kézbesítési szolgáltatás].
* PlayReady Licenctovábbítási szolgáltatásra. További információkért lásd: [használatával dinamikus PlayReady-titkosítás és Licenctovábbítási szolgáltatása].
* PlayReady dinamikus titkosítást. További információkért lásd: [használatával dinamikus PlayReady-titkosítás és Licenctovábbítási szolgáltatása].
* Media Services-PlayReady licenc sablonja. További információkért lásd: [Media Services PlayReady licenc sablon áttekintése].
* Adatfolyam-tároló titkosított eszközök. További információkért lásd: [Streaming tárolási titkosított tartalom].

## <a id="august_changes_14"></a>Augusztus 2014 kiadásban
Kódolása egy eszköz, amikor egy kimeneti eszköz hozott létre a hello kódolási feladat befejezése után. Ebben a kiadásban, amíg az Azure Media Services kódoló kimeneti eszközökre vonatkozó metaadatok hozott létre. Hello kiadási kódolónak kezdve is bemeneti eszközökre vonatkozó metaadatok hoz létre. További információkért lásd: hello [bemeneti metaadatok] és [kimeneti metaadatok] témaköröket.

## <a id="july_changes_14"></a>2014. július kiadás
a következő hibajavítások hello történtek hello Azure Media Services csomagoló és titkosító:

* Csak hang lejátszása vissza, ha egy élő archív eszköz tooHTTP transmuxing Live Streaming – ez rögzített, és most hang- és a videó lejátszása közben.
* Amikor egy eszköz tooHTTP élő adatfolyam-továbbítási és a 128 bites AES envelope titkosítási csomagolás, csomagolt hello adatfolyamok nem játssza le az Android-eszközökön – ez a hiba kijavítása és hello csomagolt adatfolyam lejátssza Android-eszközökön, amelyek támogatják a HTTP Live Streaming.

## <a id="may_changes_14"></a>Előfordulhat, hogy a 2014 kiadásban
### <a id="may_14_changes"></a>Általános Media Services-frissítések
Ezután már használhatja [dinamikus becsomagolás] toostream HTTP Live Streaming (HLS) v3. toostream HLS v3, adja hozzá a következő formátumban toohello lokátor elérési útját hello: * .ism/manifest(format=m3u8-aapl-v3). További információkért lásd: [Nick Drouin Blog].

A dinamikus csomagolás most is támogatja a PlayReady titkosított HLS (a v3 és a v4) kézbesítéséhez Smooth Streaming PlayReady statikusan titkosított alapján. Információ a PlayReady, Smooth Streaming tooencrypt lásd: [védelme Smooth Stream a Playreadyvel].

### <a name="may_14_donnet_changes"></a>Media Services .NET SDK frissítések
Media Services .NET SDK 3.0.0.5 kiadás hello hello a következő fejlesztéseket tartalmazza:

* Nagyobb sebesség és a rugalmasság letöltésre feltöltése/media eszközök.
* Újrapróbálkozási logika és átmeneti kivételkezelést fejlesztései: 
  
  * Átmeneti hiba felderítését és újrapróbálkozási logika fejlesztettek tovább a lekérdezése, módosításainak mentése folyamatban van, vagy feltöltése a fájlok által okozott kivételeket. 
  * Első webes kivételek (például egy ACS-jogkivonat kérelem) során, megfigyelheti, hogy végzetes hibák sikertelenek gyorsabban most.

További információkért lásd: [ismételje meg a logikai hello Media Services SDK for .NET].

## <a id="april_changes_14"></a>2014. április kódoló kiadás
### <a name="april_14_enocer_changes"></a>Media Services-kódoló frissítések
* A tömörített választásával dolgozhat fel AVI fájlt létrehozott szerkesztőjével hello fű Valley EDIUS nem lineáris, ahol videó hello enyhén támogatása fű Valley HQ/HQX kodek. További információkért lásd: [fű Valley időről EDIUS 7 Streaming keresztül hello felhő].
* Támogatja a hello hello Media Encoder által előállított hello fájlok elnevezési megadása. További információkért lásd: [Media Service kódoló kimeneti fájlnév szabályozása].
* Videó és/vagy a hang átfedések támogatása. További információkért lásd: [létrehozása átfedések].
* Szeretne összefűzni több videó szegmenseknek támogatja. További információkért lásd: [varrással videó szegmensek].
* Rögzített kapcsolatos hiba, ahol hello hang kódolású MPEG-1 hang réteg 3 (más néven MP3) MP4 tootranscoding.

## <a id="jan_feb_changes_14"></a>Január/február 2014 kiadások
### <a name="jan_fab_14_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.1, 3.0.0.2 és 3.0.0.3
3.0.0.1 és 3.0.0.2 hello változását a következők:

* Az OrderBy utasítások LINQ-lekérdezések toousage kapcsolódó rögzített problémák.
* Teszt megoldások vágási [GitHub] egység alapján tesztek és a forgatókönyv-alapú teszteket.

Hello módosításokkal kapcsolatos további részletekért lásd: [3.0.0.1 és 3.0.0.2 Azure Media Services .NET SDK kiadott].

a következő módosításokat hello 3.0.0.3 került sor:

* Az Azure storage függőségek toouse verzió 3.0.3.0 frissítve. 
* Előző verziókkal való kompatibilitás probléma 3.0 rögzített. *.* kiadását. 

## <a id="december_changes_13"></a>2013 decembere kiadás
### <a name="dec_13_donnet_changes"></a>Azure Media Services .NET SDK 3.0.0.0
> [!NOTE]
> 3.0.x.x kiadásokban nincsenek 2.4.x.x kiadásainak visszamenőlegesen kompatibilis.
> 
> 

már 3.0.0.0 hello hello Media Services SDK legújabb verzióját. Nuget hello legújabb csomag letöltését, vagy hello bits az beszerzése [GitHub].

Media Services SDK verzió 3.0.0.0 hello verziótól kezdődően újrahasználhatja hello [Azure Active Directory Access Control Service (ACS)] jogkivonatokat. További információkért lásd: hello "újból felhasználja a vezérlő szolgáltatás jogkivonatot" című szakaszában találhat hello [tooMedia szolgáltatások összekapcsolása hello Media Services SDK for .NET] témakör.

### <a name="dec_13_donnet_ext_changes"></a>Azure Media Services .NET SDK-bővítmények 2.0.0.0-s
hello Azure Media Services .NET SDK-bővítmények olyan kiegészítő módszerek és segédfüggvények találhatók, amelyek egyszerűbbé teszik a kódolást és révén könnyebben toodevelop az Azure Media Services lesz. Kaphat a legújabb bits hello a [Azure Media Services .NET SDK-bővítmények].

## <a id="november_changes_13"></a>A 2013. novemberi kiadásban
### <a name="nov_13_donnet_changes"></a>Azure Media Services .NET SDK változások
Ezen verziójával kezdődően hello Media Services SDK for .NET kezeli az átmeneti hiba hívások toohello Media Services REST API réteg meghozásakor előforduló hibákat.

## <a id="august_changes_13"></a>Augusztus 2013 kibocsátási
### <a name="aug_13_powershell_changes"></a>Media Services PowerShell parancsmagok Azure Sdk Tools
Media Services PowerShell-parancsmagok a következő hello mostantól beletartoznak [azure-sdk-eszközök].

* Get-AzureMediaServices 
  
    Például: `Get-AzureMediaServicesAccount`.
* New-AzureMediaServicesAccount 
  
    Például: `New-AzureMediaServicesAccount -Name “MediaAccountName” -Location “Region” -StorageAccountName “StorageAccountName”`.
* New-AzureMediaServicesKey 
  
    Például: `New-AzureMediaServicesKey -Name “MediaAccountName” -KeyType Secondary -Force`.
* Remove-AzureMediaServicesAccount 
  
    Például: `Remove-AzureMediaServicesAccount -Name “MediaAccountName” -Force`.

## <a id="june_changes_13"></a>2013. június kiadás
### <a name="june_13_general_changes"></a>Az Azure Media Services változások
Ebben a szakaszban említett hello módosítások feloldja a 2013. június Media Services hello szereplő frissítések érhetők el.

* Képes toolink több tárfiókot tooa Media Services-fiókját. 
  
    StorageAccount
  
    Asset.StorageAccountName és Asset.StorageAccount
* Képes tooupdate Job.Priority. 
* Értesítési kapcsolódó vállalatok és a tulajdonságok: 
  
    JobNotificationSubscription
  
    NotificationEndPoint
  
    Feladat
* Asset.Uri 
* Locator.Name 

### <a name="june_13_dotnet_changes"></a>Az Azure Media Services .NET SDK-változások
hello változásai-e adva a 2013. június Media Services SDK kiadását. hello Media Services SDK legújabb érhető el a Githubon.

* 2.3.0.0, a Media Services SDK-t támogatja, több tároló fiókok tooa Media Services-fiók csatolása hello hello verziójától kezdve. hello következő API-kat támogatja ezt a szolgáltatást:
  
    hello IStorageAccount típusa.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.CloudMediaContext.StorageAccounts tulajdonság.
  
    hello StorageAccount tulajdonság.
  
    hello StorageAccountName tulajdonság.
  
    További információkért lásd: [Media Services eszközök kezelése több Tárfiókok között].
* Értesítési kapcsolódó API-k. Hello képességét toolisten tooAzure várólista tárolási értesítések rendelkezik 2.2.0.0 hello verziójától kezdve. További információ: [kezelése Media Services feladat értesítések].
  
    hello Microsoft.WindowsAzure.MediaServices.Client.IJob.JobNotificationSubscriptions tulajdonság.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.INotificationEndPoint típusa.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.IJobNotificationSubscription típusa.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointCollection típusa.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.NotificationEndPointType típusa.
  
    hello Microsoft.WindowsAzure.MediaServices.Client.NotificationJobState típusa.
* Hello Azure Storage ügyfél SDK 2.0 (Microsoft.WindowsAzure.StorageClient.dll) függ.
* OData 5.5 (Microsoft.Data.OData.dll) függ.

## <a id="december_changes_12"></a>2012. decemberi kiadása
### <a name="dec_12_dotnet_changes"></a>Az Azure Media Services .NET SDK-változások
* IntelliSense: Hiányzó Intellisense dokumentációjában számos különböző hozzá.
* Microsoft.Practices.TransientFaultHandling.Core: Megtörtént egy probléma javítása ahol hello SDK továbbra is egy függőségi tooan régi verziója volt ezt a szerelvényt. hello SDK most hivatkozások 5.1.1209.1 a szerelvény verziója.

2012. November SDK hello található probléma javítását:

* IAsset.Locators.Count: Ez a szám most már megfelelően jelentenek új IAsset felületek után minden keresők törölve lett.
* IAssetFile.ContentFileSize: A most már megfelelően értéke feltöltés után IAssetFile.Upload(filepath) által.
* IAssetFile.ContentFileSize: Ez a tulajdonság most már beállítható egy eszköz-fájl létrehozásakor. Korábban csak olvasható volt.
* IAssetFile.Upload(filepath): Rögzített problémát, ahol a szinkron feltöltés metódus lett kiváltása a hello feltöltésekor a rendszer több fájlok toohello eszköz a következő hiba történt. hello hiba történt a "kiszolgáló nem tudta tooauthenticate hello kérelem. Győződjön meg arról, hogy az engedélyezési fejléc hello értékének formátuma helyes beleértve hello aláírás."
* IAssetFile.UploadAsync: Megtörtént egy probléma javítása ahol legfeljebb 5 fájl tölthető fel egy időben.
* IAssetFile.UploadProgressChanged: Hello SDK mostantól megjeleníti ezt az eseményt.
* (Karakterlánc, BlobTransferClient, ILocator, CancellationToken) IAssetFile.DownloadAsync: Ez most valósul meg.
* IAssetFile.DownloadAsync: Megtörtént egy probléma javítása ahol legfeljebb 5 fájlok letöltése sikerült egyidejűleg.
* IAssetFile.Delete(): Megtörtént egy probléma javítása hol történt a delete előfordulhat, hogy kivételt jelez, ha nincs fájl feltöltése a hello IAssetFile.
* Feladatok: Megtörtént egy probléma javítása ahol láncolás "MP4 tooSmooth adatfolyamok feladat" "PlayReady védelmi feladatokkal" feladat sablon használatával volna létrehozni minden feladatot minden.
* EncryptionUtils.GetCertificateFromStore(): Ez a módszer nullhivatkozás okozta kivétel hello tanúsítványalapú tanúsítvány konfigurációs problémák keresése tooa hibák miatt már nem jelez.

## <a id="november_changes_12"></a>2012. november kiadás
hello ebben a szakaszban említett módosításra hello 2012. November (verzió: 2.0.0.0-s) szereplő frissítés SDK. Ezeket a változásokat igényelhet bármely hello 2012. júniusi preview SDK kiadás toobe módosítva vagy írni írt kódot.

* Objektumok
  
    IAsset.Create(assetName) hello csak eszköz létrehozása függvény. IAsset.Create már nem hello metódus hívása részeként fel a fájlokat. Használjon IAssetFile feltöltése.
  
    hello IAsset.Publish metódus és hello AssetState.Publish számbavételi érték eltávolított hello SDK-t. Ez az érték a ügyfélkulcshoz tartozó kódok újbóli írásbeli kell lennie.
* FileInfo
  
    Ez az osztály eltávolítottak és IAssetFile cserélve.
  
    IAssetFiles
  
    IAssetFile FileInfo váltja fel, és egy másik viselkedik. toouse, hello IAssetFiles objektumpéldányt, hello Media Services SDK vagy az Azure Storage szolgáltatás SDK hello segítségével a fájlfeltöltés követ. a következő IAssetFile.Upload túlterhelések hello használhatók:
  
  * IAssetFile.Upload(filePath): Szinkron módszer, amely letiltja a hello szál, és akkor javasolt, ha egy fájl feltöltése.
  * IAssetFile.UploadAsync (fájl elérési útja, blobTransferClient, lokátor, cancellationToken): egy aszinkron metódus. Ez az előnyben részesített hello feltöltési módot. 
    
    Ismert hiba: hello cancellationToken használatával valóban megszakítja az hello feltöltés; azonban hello feladatok állapotának hello cancellation bármelyike lehet állapotok számát. Meg kell megfelelően dolgozza fel, és kivételek tud kezelni.
* Keresők
  
    hello eredetű-specifikus verziójával el lettek távolítva. SAS-specifikus hello környezetben. Elavult, vagy eltávolította GA Locators.CreateSasLocator (eszköz, accessPolicy) lesz megjelölve. Lásd: hello keresők frissített viselkedését az új funkciók szakaszát.

## <a id="june_changes_12"></a>2012. júniusi előzetes kiadás
a következő funkciók hello volt új hello SDK hello. novemberi kiadásában.

* Entitások törlése
  
    IAsset, IAssetFile, ILocator, IAccessPolicy, IContentKey objektumok hello objektum szintjén, azaz IObject.Delete() ahelyett, hogy a gyűjtemény, cloudMediaContext.ObjCollection.Delete(objInstance) hello törlési most törlődnek.
* Keresők
  
    Keresők most már léteznie kell hello CreateLocator metódussal és hello LocatorType.SAS vagy LocatorType.OnDemandOrigin számbavételi értékek lokátor típusát hello argumentumként toocreate szeretné.
  
    Új tulajdonságok tooLocators toomake lettek hozzáadva az egyszerűbb tooobtain használható URI-azonosítók a tartalomhoz. A keresők újratervezése lett azt jelentette, hogy tooprovide nagyobb rugalmasságot jövőbeli külső kiterjeszthetőség, és növelje a könnyebb használat media ügyfélalkalmazások esetében.
* Aszinkron metódus támogatása
  
    Aszinkron már támogatja az tooall módszerek.

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!-- Anchors. -->

<!-- Images. -->

<!--- URLs. --->
[Azure Media Services MSDN fórumon]: http://social.msdn.microsoft.com/forums/azure/home?forum=MediaServices
[Azure Media Services REST API-referencia]: https://docs.microsoft.com/rest/api/media/operations/azure-media-services-rest-api-reference
[Media Services díjszabása]: http://azure.microsoft.com/pricing/details/media-services/
[bemeneti metaadatok]: http://msdn.microsoft.com/library/azure/dn783120.aspx
[kimeneti metaadatok]: http://msdn.microsoft.com/library/azure/dn783217.aspx
[tartalom továbbítása]: http://msdn.microsoft.com/library/azure/hh973618.aspx
[médiafájlok indexelése az Azure Media Indexer]: http://msdn.microsoft.com/library/azure/dn783455.aspx
[StreamingEndpoint]: http://msdn.microsoft.com/library/azure/dn783468.aspx
[működik-e az Azure Media Services élő adatfolyam]: http://msdn.microsoft.com/library/azure/dn783466.aspx
[AES-128 a dinamikus titkosítás segítségével és a kulcs kézbesítési szolgáltatás]: http://msdn.microsoft.com/library/azure/dn783457.aspx
[használatával dinamikus PlayReady-titkosítás és Licenctovábbítási szolgáltatása]: http://msdn.microsoft.com/library/azure/dn783467.aspx
[Preview features]: http://azure.microsoft.com/services/preview/
[Media Services PlayReady licenc sablon áttekintése]: http://msdn.microsoft.com/library/azure/dn783459.aspx
[Streaming tárolási titkosított tartalom]: http://msdn.microsoft.com/library/azure/dn783451.aspx
[Azure portal]: https://manage.windowsazure.com
[dinamikus becsomagolás]: http://msdn.microsoft.com/library/azure/jj889436.aspx
[Nick Drouin Blog]: http://blog-ndrouin.azurewebsites.net/hls-v3-new-old-thing/
[védelme Smooth Stream a Playreadyvel]: http://msdn.microsoft.com/library/azure/dn189154.aspx
[ismételje meg a logikai hello Media Services SDK for .NET]: http://msdn.microsoft.com/library/azure/dn745650.aspx
[fű Valley időről EDIUS 7 Streaming keresztül hello felhő]: http://www.streamingmedia.com/Producer/Articles/ReadArticle.aspx?ArticleID=96351&utm_source=dlvr.it&utm_medium=twitter
[Media Service kódoló kimeneti fájlnév szabályozása]: http://msdn.microsoft.com/library/azure/dn303341.aspx
[létrehozása átfedések]: http://msdn.microsoft.com/library/azure/dn640496.aspx
[varrással videó szegmensek]: http://msdn.microsoft.com/library/azure/dn640504.aspx
[3.0.0.1 és 3.0.0.2 Azure Media Services .NET SDK kiadott]: http://www.gtrifonov.com/2014/02/07/windows-azure-media-services-.net-sdk-3.0.0.2-release/
[Azure Active Directory Access Control Service (ACS)]: http://msdn.microsoft.com/library/hh147631.aspx
[tooMedia szolgáltatások összekapcsolása hello Media Services SDK for .NET]: http://msdn.microsoft.com/library/azure/jj129571.aspx
[Azure Media Services .NET SDK-bővítmények]: https://github.com/Azure/azure-sdk-for-media-services-extensions/tree/dev
[azure-sdk-eszközök]: https://github.com/Azure/azure-sdk-tools
[GitHub]: https://github.com/Azure/azure-sdk-for-media-services
[Media Services eszközök kezelése több Tárfiókok között]: http://msdn.microsoft.com/library/azure/dn271889.aspx
[kezelése Media Services feladat értesítések]: http://msdn.microsoft.com/library/azure/dn261241.aspx

