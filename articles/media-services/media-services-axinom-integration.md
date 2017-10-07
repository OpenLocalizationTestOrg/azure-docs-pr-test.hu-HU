---
title: aaaUsing Axinom toodeliver Widevine-licencek tooAzure Media Services |} Microsoft Docs
description: "Ez a cikk ismerteti, hogyan használhatja az Azure Media Services (AMS) toodeliver olyan adatfolyamra, amely dinamikusan titkosítja a PlayReady vagy a Widevine DRMs AMS. hello PlayReady licenc Media Services PlayReady licenckiszolgáló származik, és Widevine-licenc Axinom licenckiszolgáló hozta."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.assetid: 9c93fa4e-b4da-4774-ab6d-8b12b371631d
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: willzhan;Mingfeiy;rajputam;Juliako
ms.openlocfilehash: 2245d9269c30712ef779973ae021c00c76174d0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-axinom-toodeliver-widevine-licenses-tooazure-media-services"></a>Axinom toodeliver Widevine-licencek tooAzure Media Services használatával
> [!div class="op_single_selector"]
> * [castLabs](media-services-castlabs-integration.md)
> * [Axinom](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a>Áttekintés
Az Azure Media Services (AMS) hozzáadta a Google Widevine dynamic védelmi (lásd: [Mingfei's blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) részletekért). Emellett Azure Media Player (AMP) is felvette a Widevine-támogatás (lásd: [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) részletekért). Ez az adatfolyam a több-native-DRM (PlayReady és Widevine) CENC által védett DASH-tartalom egy fő befejezéséről MSE és EME modern böngésző támogatja a.

Hello Media Services .NET SDK verzió 3.5.2 verziótól kezdődően a Media Services lehetővé teszi, hogy Ön tooconfigure Widevine-licencsablon és Widevine-licencek beolvasása. Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

Ez a cikk ismerteti, hogyan toointegrate és tesztelési Widevine licenc Axinom által kezelt kiszolgálón. Pontosabban ismerteti:  

* Konfigurálja a dynamic Common Encryption multi-DRM (PlayReady és Widevine) megfelelő licenc licenckérési URL;
* A JWT jogkivonat létrehozása a rendelés server toomeet hello licenckövetelmények;
* Fejlesztés az Azure Media Player alkalmazást, mert a licenc beszerzési JWT jogkivonat hitelesítéssel; kezeli

hello teljes rendszert, és kulcsfontosságú, kulcs azonosítója, kulcs kezdőérték, JTW token és a jogcímek a következő diagram hello legjobb jelentheti tartalom áramló hello.

![VONAL- és CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a>Tartalomvédelem
A dinamikus védelem és a kulcs objektumtovábbítási szabályzat konfigurálása, lásd: Mingfei's blog: [hogyan tooconfigure Widevine packaging az Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).

Dinamikus CENC védelmet a streaming mindkét hello követő kötőjel multi-DRM konfigurálható:

1. PlayReady védelme MS Edge és IE11, amely a token engedélyezési korlátozások rendelkezhetnek. hello token korlátozott házirend által a Secure Token Service (STS), például az Azure Active Directory; kiadott tokennek kell fűzni
2. Chrome Widevine védelmét, akkor lehet szükség tokent használó hitelesítés az egy másik STS által kibocsátott jogkivonatot. 

Ellenőrizze a [JWT jogkivonat generációs](media-services-axinom-integration.md#jwt-token-generation) miért Azure Active Directory nem használható az STS szolgáltatással Axinom tartozó Widevine licenckiszolgáló szakaszát.

### <a name="considerations"></a>Megfontolandó szempontok
1. Hello Axinom megadott kulcs kezdőérték (8888000000000000000000000000000000000000) és a létrehozott vagy kiválasztott kulcs azonosítója toogenerate hello tartalmát kulcs kulcs kézbesítési szolgáltatás kell használnia. Licenckiszolgáló Axinom állít ki a tartalomkulcs-alapú tartalmazó összes licenc a hello ugyanaz a kulcs kezdőérték, amely érvényes a teszteléshez és éles.
2. hello Widevine licenc licenckérési URL tesztelési: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense). A HTTP és a HTTS engedélyezettek.

## <a name="azure-media-player-preparation"></a>Az Azure Media Player előkészítése
AMP v1.4.0 PlayReady és Widevine DRM-Védelemmel is dinamikusan csomagolt AMS tartalom lejátszását támogatja.
Widevine-licenckiszolgáló nem token hitelesítés szükséges, ha nincs szükség további toodo tootest DASH-tartalom védve által Widevine van szüksége. Tegyük fel, a hello AMP team biztosít egy egyszerű [minta](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), ahol megtekintheti a peremhálózati és IE11 a PlayReady és Widevine a Chrome működéséhez.
hello Widevine licenckiszolgáló Axinom által biztosított JWT jogkivonat hitelesítést igényel. hello JWT jogkivonat licenc kérelmet "X-AxDRM-üzenet" HTTP-fejléc az elküldött toobe kell. Erre a célra a következő javascript AMP üzemeltető előtt hello forrás hello weblapon tooadd hello kell:

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

hello többi AMP kód pedig AMP dokumentumot mint szabványos AMP API [Itt](http://amp.azure.net/libs/amp/latest/docs/).

Vegye figyelembe, hogy hello fent javascript egyéni engedélyezési fejléc még mindig a rövid távú megközelítés előtt hello hivatalos AMP hosszú távú megközelítést megjelenik a beállításhoz.

## <a name="jwt-token-generation"></a>JWT jogkivonat létrehozása
Tesztelési Axinom Widevine licenckiszolgáló JWT jogkivonat hitelesítés szükséges. Ezenkívül hello jogcímek hello JWT jogkivonat egyik egy összetett típusú egyszerű adattípus helyett.

Sajnos az Azure AD csak abban az esetben JWT-jogkivonatokat a primitív típusú. Hasonlóképpen .NET-keretrendszer API (System.IdentityModel.Tokens.SecurityTokenHandler és JwtPayload) csak lehetővé teszi tooinput összetett objektumtípus jogcímekként. Azonban hello jogcímek továbbra is karakterláncként szerializálni. Ezért nem tudjuk használni a két hello Widevine-licenc kérelem előállítása hello JWT jogkivonat esetében.

John Sheehan [JWT Nuget-csomag](https://www.nuget.org/packages/JWT) megfelel-e hello igényeinek, így fogjuk toouse a Nuget-csomagot.

Az alábbiakban hello kód a hello előállítása JWT jogkivonat esetében szükséges Axinom Widevine licenckiszolgáló által megkövetelt jogcímeket teszteléséhez:

    using System;
    using System.Collections.Generic;
    using System.Linq;
    using System.Web;
    using System.IdentityModel.Tokens;
    using System.IdentityModel.Protocols.WSTrust;
    using System.Security.Claims;

    namespace OpenIdConnectWeb.Utils
    {
        public class JwtUtils
        {
            //using John Sheehan's NuGet JWT library: https://www.nuget.org/packages/JWT/
            public static string CreateJwtSheehan(string symmetricKeyHex, string key_id)
            {
                byte[] symmetricKey = ConvertHexStringToByteArray(symmetricKeyHex);  //hex string toobyte[] Note: Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.

                var payload = new Dictionary<string, object>()
                             {
                                 { "version", 1 },
                                 { "com_key_id", System.Configuration.ConfigurationManager.AppSettings["ax:com_key_id"] },
                                 { "message", new { type = "entitlement_message", key_ids = new string[] { key_id } }  }
                             };

                string token = JWT.JsonWebToken.Encode(payload, symmetricKey, JWT.JwtHashAlgorithm.HS256);

                return token;
            }

            //convert hex string toobyte[]
            public static byte[] ConvertHexStringToByteArray(string hexString)
            {
                if (hexString.Length % 2 != 0)
                {
                    throw new ArgumentException(String.Format(System.Globalization.CultureInfo.InvariantCulture, "hello binary key cannot have an odd number of digits: {0}", hexString));
                }

                byte[] HexAsBytes = new byte[hexString.Length / 2];
                for (int index = 0; index < HexAsBytes.Length; index++)
                {
                    string byteValue = hexString.Substring(index * 2, 2);
                    HexAsBytes[index] = byte.Parse(byteValue, System.Globalization.NumberStyles.HexNumber, System.Globalization.CultureInfo.InvariantCulture);
                }

                return HexAsBytes;
            }

        }  

    }  

Licenckiszolgáló Axinom Widevine

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a>Megfontolandó szempontok
1. Annak ellenére, hogy AMS PlayReady-licenctovábbítási szolgáltatásra szükséges "tulajdonosi =" megelőző egy hitelesítési jogkivonatot, Axinom Widevine licenckiszolgáló nem használja fel.
2. hello Axinom kommunikációs kulcs aláírási kulcs lesz. Megjegyzés: hello kulcs egy hexadecimális karakterláncban azonban kezelni kell bájt karakterlánc sorozataként kódolás esetén. Ez ConvertHexStringToByteArray hello módszerrel érhető el.

## <a name="retrieving-key-id"></a>Kulcsazonosítója beolvasása
Talán észrevette, hogy hello kód generálásához. a JWT jogkivonat, kulcs azonosítója szükség. Hello JWT jogkivonat kell toobe készen AMP player betöltése előtt, mert a kulcs azonosítója igények toobe olvassa be a rendezési toogenerate JWT jogkivonat.

Az állomásokon többféleképpen is tooget tartsa kulcs azonosítóját. Például tárolhatjuk egy kulcs azonosítója és egy adatbázisban tartalmak metaadatait. Vagy kérheti le kulcsazonosító DASH MPD (Media bemutató leírása) fájlból. az alábbi kód hello hello ez utóbbi van.

    //get key_id from DASH MPD
    public static string GetKeyID(string dashUrl)
    {
        if (!dashUrl.EndsWith("(format=mpd-time-csf)"))
        {
            dashUrl += "(format=mpd-time-csf)";
        }

        XPathDocument objXPathDocument = new XPathDocument(dashUrl);
        XPathNavigator objXPathNavigator = objXPathDocument.CreateNavigator();
        XmlNamespaceManager objXmlNamespaceManager = new XmlNamespaceManager(objXPathNavigator.NameTable);
        objXmlNamespaceManager.AddNamespace("",     "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("ns1",  "urn:mpeg:dash:schema:mpd:2011");
        objXmlNamespaceManager.AddNamespace("cenc", "urn:mpeg:cenc:2013");
        objXmlNamespaceManager.AddNamespace("ms",   "urn:microsoft");
        objXmlNamespaceManager.AddNamespace("mspr", "urn:microsoft:playready");
        objXmlNamespaceManager.AddNamespace("xsi",  "http://www.w3.org/2001/XMLSchema-instance");
        objXmlNamespaceManager.PushScope();

        XPathNodeIterator objXPathNodeIterator;
        objXPathNodeIterator = objXPathNavigator.Select("//ns1:MPD/ns1:Period/ns1:AdaptationSet/ns1:ContentProtection[@value='cenc']", objXmlNamespaceManager);

        string key_id = string.Empty;
        if (objXPathNodeIterator.MoveNext())
        {
            key_id = objXPathNodeIterator.Current.GetAttribute("default_KID", "urn:mpeg:cenc:2013");
        }

        return key_id;
    }

## <a name="summary"></a>Összefoglalás
Widevine-támogatás az Azure Media Services Content Protection, mind az Azure Media Player legújabb hozzáadásával azt képesek tooimplement adatfolyam a kötőjel + több native DRM (PlayReady + Widevine) mindkét AMS és Widevine-licenc PlayReady licenc szolgáltatással kiszolgáló Axinom hello modern böngésző támogatja a következő számára:

* Chrome
* Windows 10-es Microsoft Edge
* A Windows 8.1 és Windows 10 IE 11
* Firefox (asztali verzió) és a Safari böngésző Mac (nem csak iOS) a Silverlight keresztül is támogatottak, és azonos URL-CÍMMEL rendelkező Azure Media Player hello

hello alábbi paramétereket kell megadni hello minimális megoldás emelés Axinom Widevine licenckiszolgáló. Kulcs kivételével Azonosítóját, a paraméterek hello rest által biztosított Axinom a Widevine-kiszolgáló beállításai alapján.

| Paraméter | Hogyan használja fel azokat |
| --- | --- |
| Kommunikációs kulcs azonosítója |Meg kell adni a hello jogcím "com_key_id" JWT jogkivonat értékeként (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban). |
| Kommunikációs kulcs |Kell használni, az aláírási kulcs JWT jogkivonat hello (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban). |
| Kulcs kezdőérték |Kell az összes használt toogenerate tartalomkulcsot adni tartalmat kulcs azonosítója (lásd: [ez](media-services-axinom-integration.md#content-protection) szakaszban). |
| Widevine-licenc-licenckérési URL-cím |Kell használni az adategység továbbítási házirendjét az DASH adatfolyamként történő konfigurálásával (lásd: [ez](media-services-axinom-integration.md#content-protection) szakaszban). |
| Tartalom azonosítója |Lehet, hogy tartalmazzák a JWT jogkivonat jogosultság üzenet jogcím hello értékét (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban). |

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a>Nyugták
Szeretnénk tooacknowledge hello személyek felé, hogy ez a dokumentum létrehozása része volt a következő: a Axinom Kristjan Jõgi Mingfei Pozsony és Amit Rajput.

