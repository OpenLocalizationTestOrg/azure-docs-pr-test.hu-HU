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
# <a name="using-axinom-toodeliver-widevine-licenses-tooazure-media-services"></a><span data-ttu-id="3e1fa-104">Axinom toodeliver Widevine-licencek tooAzure Media Services használatával</span><span class="sxs-lookup"><span data-stu-id="3e1fa-104">Using Axinom toodeliver Widevine licenses tooAzure Media Services</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e1fa-105">castLabs</span><span class="sxs-lookup"><span data-stu-id="3e1fa-105">castLabs</span></span>](media-services-castlabs-integration.md)
> * [<span data-ttu-id="3e1fa-106">Axinom</span><span class="sxs-lookup"><span data-stu-id="3e1fa-106">Axinom</span></span>](media-services-axinom-integration.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="3e1fa-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3e1fa-107">Overview</span></span>
<span data-ttu-id="3e1fa-108">Az Azure Media Services (AMS) hozzáadta a Google Widevine dynamic védelmi (lásd: [Mingfei's blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) részletekért).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-108">Azure Media Services (AMS) has added Google Widevine dynamic protection (see [Mingfei’s blog](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/) for details).</span></span> <span data-ttu-id="3e1fa-109">Emellett Azure Media Player (AMP) is felvette a Widevine-támogatás (lásd: [AMP dokumentum](http://amp.azure.net/libs/amp/latest/docs/) részletekért).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-109">In addition, Azure Media Player (AMP) has also added Widevine support (see [AMP document](http://amp.azure.net/libs/amp/latest/docs/) for details).</span></span> <span data-ttu-id="3e1fa-110">Ez az adatfolyam a több-native-DRM (PlayReady és Widevine) CENC által védett DASH-tartalom egy fő befejezéséről MSE és EME modern böngésző támogatja a.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-110">This is a major accomplishment in streaming DASH content protected by CENC with multi-native-DRM (PlayReady and Widevine) on modern browsers equipped with MSE and EME.</span></span>

<span data-ttu-id="3e1fa-111">Hello Media Services .NET SDK verzió 3.5.2 verziótól kezdődően a Media Services lehetővé teszi, hogy Ön tooconfigure Widevine-licencsablon és Widevine-licencek beolvasása.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-111">Starting with hello Media Services .NET SDK version 3.5.2, Media Services enables you tooconfigure Widevine license template and get Widevine licenses.</span></span> <span data-ttu-id="3e1fa-112">Használhatja a következő AMS-partnereket toohelp Widevine-licencek átadná hello: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-112">You can also use hello following AMS partners toohelp you deliver Widevine licenses: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).</span></span>

<span data-ttu-id="3e1fa-113">Ez a cikk ismerteti, hogyan toointegrate és tesztelési Widevine licenc Axinom által kezelt kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-113">This article describes how toointegrate and test Widevine license server managed by Axinom.</span></span> <span data-ttu-id="3e1fa-114">Pontosabban ismerteti:</span><span class="sxs-lookup"><span data-stu-id="3e1fa-114">Specifically, it covers:</span></span>  

* <span data-ttu-id="3e1fa-115">Konfigurálja a dynamic Common Encryption multi-DRM (PlayReady és Widevine) megfelelő licenc licenckérési URL;</span><span class="sxs-lookup"><span data-stu-id="3e1fa-115">Configuring dynamic Common Encryption with multi-DRM (PlayReady and Widevine) with corresponding license acquisition URLs;</span></span>
* <span data-ttu-id="3e1fa-116">A JWT jogkivonat létrehozása a rendelés server toomeet hello licenckövetelmények;</span><span class="sxs-lookup"><span data-stu-id="3e1fa-116">Generating a JWT token in order toomeet hello license server requirements;</span></span>
* <span data-ttu-id="3e1fa-117">Fejlesztés az Azure Media Player alkalmazást, mert a licenc beszerzési JWT jogkivonat hitelesítéssel; kezeli</span><span class="sxs-lookup"><span data-stu-id="3e1fa-117">Developing Azure Media Player app which handles license acquisition with JWT token authentication;</span></span>

<span data-ttu-id="3e1fa-118">hello teljes rendszert, és kulcsfontosságú, kulcs azonosítója, kulcs kezdőérték, JTW token és a jogcímek a következő diagram hello legjobb jelentheti tartalom áramló hello.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-118">hello complete system and hello flow of content key, key ID, key seed, JTW token and its claims can be best described by hello following diagram.</span></span>

![VONAL- és CENC](./media/media-services-axinom-integration/media-services-axinom1.png)

## <a name="content-protection"></a><span data-ttu-id="3e1fa-120">Tartalomvédelem</span><span class="sxs-lookup"><span data-stu-id="3e1fa-120">Content Protection</span></span>
<span data-ttu-id="3e1fa-121">A dinamikus védelem és a kulcs objektumtovábbítási szabályzat konfigurálása, lásd: Mingfei's blog: [hogyan tooconfigure Widevine packaging az Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-121">For configuring dynamic protection and key delivery policy, please see Mingfei’s blog: [How tooconfigure Widevine packaging with Azure Media Services](http://mingfeiy.com/how-to-configure-widevine-packaging-with-azure-media-services).</span></span>

<span data-ttu-id="3e1fa-122">Dinamikus CENC védelmet a streaming mindkét hello követő kötőjel multi-DRM konfigurálható:</span><span class="sxs-lookup"><span data-stu-id="3e1fa-122">You can configure dynamic CENC protection with multi-DRM for DASH streaming having both of hello following:</span></span>

1. <span data-ttu-id="3e1fa-123">PlayReady védelme MS Edge és IE11, amely a token engedélyezési korlátozások rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-123">PlayReady protection for MS Edge and IE11, that could have a token authorization restrictions.</span></span> <span data-ttu-id="3e1fa-124">hello token korlátozott házirend által a Secure Token Service (STS), például az Azure Active Directory; kiadott tokennek kell fűzni</span><span class="sxs-lookup"><span data-stu-id="3e1fa-124">hello token restricted policy must be accompanied by a token issued by a Secure Token Service (STS), such as Azure Active Directory;</span></span>
2. <span data-ttu-id="3e1fa-125">Chrome Widevine védelmét, akkor lehet szükség tokent használó hitelesítés az egy másik STS által kibocsátott jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-125">Widevine protection for Chrome, it can require token authentication with token issued by another STS.</span></span> 

<span data-ttu-id="3e1fa-126">Ellenőrizze a [JWT jogkivonat generációs](media-services-axinom-integration.md#jwt-token-generation) miért Azure Active Directory nem használható az STS szolgáltatással Axinom tartozó Widevine licenckiszolgáló szakaszát.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-126">Please see [JWT Token Generation](media-services-axinom-integration.md#jwt-token-generation) section for why Azure Active Directory cannot be used as an STS for Axinom’s Widevine license server.</span></span>

### <a name="considerations"></a><span data-ttu-id="3e1fa-127">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="3e1fa-127">Considerations</span></span>
1. <span data-ttu-id="3e1fa-128">Hello Axinom megadott kulcs kezdőérték (8888000000000000000000000000000000000000) és a létrehozott vagy kiválasztott kulcs azonosítója toogenerate hello tartalmát kulcs kulcs kézbesítési szolgáltatás kell használnia.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-128">You must use hello Axinom specified key seed (8888000000000000000000000000000000000000) and your generated or selected key ID toogenerate hello content key for configuring key delivery service.</span></span> <span data-ttu-id="3e1fa-129">Licenckiszolgáló Axinom állít ki a tartalomkulcs-alapú tartalmazó összes licenc a hello ugyanaz a kulcs kezdőérték, amely érvényes a teszteléshez és éles.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-129">Axinom license server will issue all licenses containing content keys based on hello same key seed, which is valid for both testing and production.</span></span>
2. <span data-ttu-id="3e1fa-130">hello Widevine licenc licenckérési URL tesztelési: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-130">hello Widevine license acquisition URL for testing: [https://drm-widevine-licensing.axtest.net/AcquireLicense](https://drm-widevine-licensing.axtest.net/AcquireLicense).</span></span> <span data-ttu-id="3e1fa-131">A HTTP és a HTTS engedélyezettek.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-131">Both HTTP and HTTS are allowed.</span></span>

## <a name="azure-media-player-preparation"></a><span data-ttu-id="3e1fa-132">Az Azure Media Player előkészítése</span><span class="sxs-lookup"><span data-stu-id="3e1fa-132">Azure Media Player Preparation</span></span>
<span data-ttu-id="3e1fa-133">AMP v1.4.0 PlayReady és Widevine DRM-Védelemmel is dinamikusan csomagolt AMS tartalom lejátszását támogatja.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-133">AMP v1.4.0 supports playback of AMS content that is dynamically packaged with both PlayReady and Widevine DRM.</span></span>
<span data-ttu-id="3e1fa-134">Widevine-licenckiszolgáló nem token hitelesítés szükséges, ha nincs szükség további toodo tootest DASH-tartalom védve által Widevine van szüksége.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-134">If Widevine license server does not require token authentication, there is nothing additional you need toodo tootest a DASH content protected by Widevine.</span></span> <span data-ttu-id="3e1fa-135">Tegyük fel, a hello AMP team biztosít egy egyszerű [minta](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), ahol megtekintheti a peremhálózati és IE11 a PlayReady és Widevine a Chrome működéséhez.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-135">For an example, hello AMP team provides a simple [sample](http://amp.azure.net/libs/amp/latest/samples/dynamic_multiDRM_PlayReadyWidevine_notoken.html), where you can see it working in Edge and IE11 with PlayReady and Chrome with Widevine.</span></span>
<span data-ttu-id="3e1fa-136">hello Widevine licenckiszolgáló Axinom által biztosított JWT jogkivonat hitelesítést igényel.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-136">hello Widevine license server provided by Axinom requires JWT token authentication.</span></span> <span data-ttu-id="3e1fa-137">hello JWT jogkivonat licenc kérelmet "X-AxDRM-üzenet" HTTP-fejléc az elküldött toobe kell.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-137">hello JWT token needs toobe submitted with license request through an HTTP header “X-AxDRM-Message”.</span></span> <span data-ttu-id="3e1fa-138">Erre a célra a következő javascript AMP üzemeltető előtt hello forrás hello weblapon tooadd hello kell:</span><span class="sxs-lookup"><span data-stu-id="3e1fa-138">For this purpose, you need tooadd hello following javascript in hello web page hosting AMP before setting hello source:</span></span>

    <script>AzureHtml5JS.KeySystem.WidevineCustomAuthorizationHeader = "X-AxDRM-Message"</script>

<span data-ttu-id="3e1fa-139">hello többi AMP kód pedig AMP dokumentumot mint szabványos AMP API [Itt](http://amp.azure.net/libs/amp/latest/docs/).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-139">hello rest of AMP code is standard AMP API as in AMP document [here](http://amp.azure.net/libs/amp/latest/docs/).</span></span>

<span data-ttu-id="3e1fa-140">Vegye figyelembe, hogy hello fent javascript egyéni engedélyezési fejléc még mindig a rövid távú megközelítés előtt hello hivatalos AMP hosszú távú megközelítést megjelenik a beállításhoz.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-140">Note that hello above javascript for setting custom authorization header is still a short term approach before hello official long term approach in AMP is released.</span></span>

## <a name="jwt-token-generation"></a><span data-ttu-id="3e1fa-141">JWT jogkivonat létrehozása</span><span class="sxs-lookup"><span data-stu-id="3e1fa-141">JWT Token Generation</span></span>
<span data-ttu-id="3e1fa-142">Tesztelési Axinom Widevine licenckiszolgáló JWT jogkivonat hitelesítés szükséges.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-142">Axinom Widevine license server for testing requires JWT token authentication.</span></span> <span data-ttu-id="3e1fa-143">Ezenkívül hello jogcímek hello JWT jogkivonat egyik egy összetett típusú egyszerű adattípus helyett.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-143">In addition, one of hello claims in hello JWT token is of a complex object type instead of primitive data type.</span></span>

<span data-ttu-id="3e1fa-144">Sajnos az Azure AD csak abban az esetben JWT-jogkivonatokat a primitív típusú.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-144">Unfortunately, Azure AD can only issue JWT tokens with primitive types.</span></span> <span data-ttu-id="3e1fa-145">Hasonlóképpen .NET-keretrendszer API (System.IdentityModel.Tokens.SecurityTokenHandler és JwtPayload) csak lehetővé teszi tooinput összetett objektumtípus jogcímekként.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-145">Similarly, .NET Framework API (System.IdentityModel.Tokens.SecurityTokenHandler and JwtPayload) only allows you tooinput complex object type as claims.</span></span> <span data-ttu-id="3e1fa-146">Azonban hello jogcímek továbbra is karakterláncként szerializálni.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-146">However, hello claims are still serialized as string.</span></span> <span data-ttu-id="3e1fa-147">Ezért nem tudjuk használni a két hello Widevine-licenc kérelem előállítása hello JWT jogkivonat esetében.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-147">Therefore we cannot use any of hello two for generating hello JWT token for Widevine license request.</span></span>

<span data-ttu-id="3e1fa-148">John Sheehan [JWT Nuget-csomag](https://www.nuget.org/packages/JWT) megfelel-e hello igényeinek, így fogjuk toouse a Nuget-csomagot.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-148">John Sheehan’s [JWT Nuget package](https://www.nuget.org/packages/JWT) meets hello needs so we are going toouse this Nuget package.</span></span>

<span data-ttu-id="3e1fa-149">Az alábbiakban hello kód a hello előállítása JWT jogkivonat esetében szükséges Axinom Widevine licenckiszolgáló által megkövetelt jogcímeket teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="3e1fa-149">Below is hello code for generating JWT token with hello needed claims as required by Axinom Widevine license server for testing:</span></span>

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

<span data-ttu-id="3e1fa-150">Licenckiszolgáló Axinom Widevine</span><span class="sxs-lookup"><span data-stu-id="3e1fa-150">Axinom Widevine license server</span></span>

    <add key="ax:laurl" value="http://drm-widevine-licensing.axtest.net/AcquireLicense" />
    <add key="ax:com_key_id" value="69e54088-e9e0-4530-8c1a-1eb6dcd0d14e" />
    <add key="ax:com_key" value="4861292d027e269791093327e62ceefdbea489a4c7e5a4974cc904b840fd7c0f" />
    <add key="ax:keyseed" value="8888000000000000000000000000000000000000" />

### <a name="considerations"></a><span data-ttu-id="3e1fa-151">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="3e1fa-151">Considerations</span></span>
1. <span data-ttu-id="3e1fa-152">Annak ellenére, hogy AMS PlayReady-licenctovábbítási szolgáltatásra szükséges "tulajdonosi =" megelőző egy hitelesítési jogkivonatot, Axinom Widevine licenckiszolgáló nem használja fel.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-152">Even though AMS PlayReady license delivery service requires “Bearer=” preceding an authentication token, Axinom Widevine license server does not use it.</span></span>
2. <span data-ttu-id="3e1fa-153">hello Axinom kommunikációs kulcs aláírási kulcs lesz.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-153">hello Axinom communication key is used as signing key.</span></span> <span data-ttu-id="3e1fa-154">Megjegyzés: hello kulcs egy hexadecimális karakterláncban azonban kezelni kell bájt karakterlánc sorozataként kódolás esetén.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-154">Note that hello key is a hex string, however it must be treated as a series of bytes not a string when encoding.</span></span> <span data-ttu-id="3e1fa-155">Ez ConvertHexStringToByteArray hello módszerrel érhető el.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-155">This is achieved by hello method ConvertHexStringToByteArray.</span></span>

## <a name="retrieving-key-id"></a><span data-ttu-id="3e1fa-156">Kulcsazonosítója beolvasása</span><span class="sxs-lookup"><span data-stu-id="3e1fa-156">Retrieving Key ID</span></span>
<span data-ttu-id="3e1fa-157">Talán észrevette, hogy hello kód generálásához. a JWT jogkivonat, kulcs azonosítója szükség.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-157">You may have noticed that in hello code for generating a JWT token, key ID is required.</span></span> <span data-ttu-id="3e1fa-158">Hello JWT jogkivonat kell toobe készen AMP player betöltése előtt, mert a kulcs azonosítója igények toobe olvassa be a rendezési toogenerate JWT jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-158">Since hello JWT token needs toobe ready before loading AMP player, key ID needs toobe retrieved in order toogenerate JWT token.</span></span>

<span data-ttu-id="3e1fa-159">Az állomásokon többféleképpen is tooget tartsa kulcs azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-159">Of course there are multiple ways tooget hold of key ID.</span></span> <span data-ttu-id="3e1fa-160">Például tárolhatjuk egy kulcs azonosítója és egy adatbázisban tartalmak metaadatait.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-160">For example, one may store key ID together with content metadata in a database.</span></span> <span data-ttu-id="3e1fa-161">Vagy kérheti le kulcsazonosító DASH MPD (Media bemutató leírása) fájlból.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-161">Or you can retrieve key ID from DASH MPD (Media Presentation Description) file.</span></span> <span data-ttu-id="3e1fa-162">az alábbi kód hello hello ez utóbbi van.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-162">hello code below is for hello latter.</span></span>

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

## <a name="summary"></a><span data-ttu-id="3e1fa-163">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3e1fa-163">Summary</span></span>
<span data-ttu-id="3e1fa-164">Widevine-támogatás az Azure Media Services Content Protection, mind az Azure Media Player legújabb hozzáadásával azt képesek tooimplement adatfolyam a kötőjel + több native DRM (PlayReady + Widevine) mindkét AMS és Widevine-licenc PlayReady licenc szolgáltatással kiszolgáló Axinom hello modern böngésző támogatja a következő számára:</span><span class="sxs-lookup"><span data-stu-id="3e1fa-164">With latest addition of Widevine support in both Azure Media Services Content Protection and Azure Media Player, we are able tooimplement streaming of DASH + Multi-native-DRM (PlayReady + Widevine) with both PlayReady license service in AMS and Widevine license server from Axinom for hello following modern browsers:</span></span>

* <span data-ttu-id="3e1fa-165">Chrome</span><span class="sxs-lookup"><span data-stu-id="3e1fa-165">Chrome</span></span>
* <span data-ttu-id="3e1fa-166">Windows 10-es Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="3e1fa-166">Microsoft Edge on Windows 10</span></span>
* <span data-ttu-id="3e1fa-167">A Windows 8.1 és Windows 10 IE 11</span><span class="sxs-lookup"><span data-stu-id="3e1fa-167">IE 11 on Windows 8.1 and Windows 10</span></span>
* <span data-ttu-id="3e1fa-168">Firefox (asztali verzió) és a Safari böngésző Mac (nem csak iOS) a Silverlight keresztül is támogatottak, és azonos URL-CÍMMEL rendelkező Azure Media Player hello</span><span class="sxs-lookup"><span data-stu-id="3e1fa-168">Both Firefox (Desktop) and Safari on Mac (not iOS) are also supported via Silverlight and hello same URL with Azure Media Player</span></span>

<span data-ttu-id="3e1fa-169">hello alábbi paramétereket kell megadni hello minimális megoldás emelés Axinom Widevine licenckiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-169">hello following parameters are required in hello mini-solution leveraging Axinom Widevine license server.</span></span> <span data-ttu-id="3e1fa-170">Kulcs kivételével Azonosítóját, a paraméterek hello rest által biztosított Axinom a Widevine-kiszolgáló beállításai alapján.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-170">Except for key ID, hello rest of parameters are provided by Axinom based on their Widevine server setup.</span></span>

| <span data-ttu-id="3e1fa-171">Paraméter</span><span class="sxs-lookup"><span data-stu-id="3e1fa-171">Parameter</span></span> | <span data-ttu-id="3e1fa-172">Hogyan használja fel azokat</span><span class="sxs-lookup"><span data-stu-id="3e1fa-172">How it is used</span></span> |
| --- | --- |
| <span data-ttu-id="3e1fa-173">Kommunikációs kulcs azonosítója</span><span class="sxs-lookup"><span data-stu-id="3e1fa-173">Communication key ID</span></span> |<span data-ttu-id="3e1fa-174">Meg kell adni a hello jogcím "com_key_id" JWT jogkivonat értékeként (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-174">Must be included as value of hello claim "com_key_id" in JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="3e1fa-175">Kommunikációs kulcs</span><span class="sxs-lookup"><span data-stu-id="3e1fa-175">Communication key</span></span> |<span data-ttu-id="3e1fa-176">Kell használni, az aláírási kulcs JWT jogkivonat hello (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-176">Must be used as hello signing key of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |
| <span data-ttu-id="3e1fa-177">Kulcs kezdőérték</span><span class="sxs-lookup"><span data-stu-id="3e1fa-177">Key seed</span></span> |<span data-ttu-id="3e1fa-178">Kell az összes használt toogenerate tartalomkulcsot adni tartalmat kulcs azonosítója (lásd: [ez](media-services-axinom-integration.md#content-protection) szakaszban).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-178">Must be used toogenerate content key with any given content key ID (see  [this](media-services-axinom-integration.md#content-protection) section).</span></span> |
| <span data-ttu-id="3e1fa-179">Widevine-licenc-licenckérési URL-cím</span><span class="sxs-lookup"><span data-stu-id="3e1fa-179">Widevine License acquisition URL</span></span> |<span data-ttu-id="3e1fa-180">Kell használni az adategység továbbítási házirendjét az DASH adatfolyamként történő konfigurálásával (lásd: [ez](media-services-axinom-integration.md#content-protection) szakaszban).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-180">Must be used in configuring asset delivery policy for DASH streaming (see  [this](media-services-axinom-integration.md#content-protection) section ).</span></span> |
| <span data-ttu-id="3e1fa-181">Tartalom azonosítója</span><span class="sxs-lookup"><span data-stu-id="3e1fa-181">Content Key ID</span></span> |<span data-ttu-id="3e1fa-182">Lehet, hogy tartalmazzák a JWT jogkivonat jogosultság üzenet jogcím hello értékét (lásd: [ez](media-services-axinom-integration.md#jwt-token-generation) szakaszban).</span><span class="sxs-lookup"><span data-stu-id="3e1fa-182">Must be included as part of hello value of Entitlement Message claim of JWT token (see [this](media-services-axinom-integration.md#jwt-token-generation) section).</span></span> |

## <a name="media-services-learning-paths"></a><span data-ttu-id="3e1fa-183">Media Services képzési tervek</span><span class="sxs-lookup"><span data-stu-id="3e1fa-183">Media Services learning paths</span></span>
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a><span data-ttu-id="3e1fa-184">Visszajelzés küldése</span><span class="sxs-lookup"><span data-stu-id="3e1fa-184">Provide feedback</span></span>
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

### <a name="acknowledgments"></a><span data-ttu-id="3e1fa-185">Nyugták</span><span class="sxs-lookup"><span data-stu-id="3e1fa-185">Acknowledgments</span></span>
<span data-ttu-id="3e1fa-186">Szeretnénk tooacknowledge hello személyek felé, hogy ez a dokumentum létrehozása része volt a következő: a Axinom Kristjan Jõgi Mingfei Pozsony és Amit Rajput.</span><span class="sxs-lookup"><span data-stu-id="3e1fa-186">We would like tooacknowledge hello following people who contributed towards creating this document: Kristjan Jõgi of Axinom, Mingfei Yan, and Amit Rajput.</span></span>

