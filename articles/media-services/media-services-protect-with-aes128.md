---
title: "AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás |} Microsoft Docs"
description: "A 128 bites AES titkosítási kulcsokat a Microsoft Azure Media Services használatával titkosított tartalmat továbbít. Media Services is biztosít a kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok engedéllyel rendelkező felhasználók számára. Ez a témakör bemutatja, hogyan dinamikusan titkosítani az AES-128, és a kulcs kézbesítési szolgáltatás használata."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.assetid: 4d2c10af-9ee0-408f-899b-33fa4c1d89b9
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: juliako
ms.openlocfilehash: 013c14c00096c9958a732d1f0eaacc9248f57da9
ms.sourcegitcommit: d6984ef8cc057423ff81efb4645af9d0b902f843
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 01/05/2018
---
# <a name="use-aes-128-dynamic-encryption-and-the-key-delivery-service"></a>AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás
> [!div class="op_single_selector"]
> * [.NET](media-services-protect-with-aes128.md)
> * [Java](https://github.com/southworkscom/azure-sdk-for-media-services-java-samples)
> * [PHP](https://github.com/Azure/azure-sdk-for-php/tree/master/examples/MediaServices)
> 

> [!NOTE]
> A legújabb verzióra a Java SDK és az első lépései a Java fejlesztése [Ismerkedés a Java-ügyfél SDK az Azure Media Services](https://docs.microsoft.com/azure/media-services/media-services-java-how-to-use). <br/>
> A Media Services SDK legújabb PHP letöltéséhez 0.5.7 a Microsoft/WindowsAzure csomag verziója a keres a [Packagist tárház](https://packagist.org/packages/microsoft/windowsazure#v0.5.7).  

## <a name="overview"></a>Áttekintés
> [!NOTE]
> Az az Advanced Encryption Standard (AES) a Safari kézbesítése macOS a tartalom titkosítása a további információkért lásd: [ebben a blogbejegyzésben](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).
> Hogyan védi meg a médiatartalom AES titkosítással áttekintését lásd: [Ez a videó](https://channel9.msdn.com/Shows/Azure-Friday/Azure-Media-Services-Protecting-your-Media-Content-with-AES-Encryption).
> 
> 

 A Media Services segítségével HTTP Live Streaming (HLS) és a Smooth Streaming az AES 128 bites titkosítási kulcsok használatával titkosítja. Media Services is biztosít a kulcs kézbesítési szolgáltatás letölti a titkosítási kulcsok engedéllyel rendelkező felhasználók számára. Ha azt szeretné, hogy a Media Services az objektum titkosítására, rendelje hozzá egy titkosítási kulcsot az eszköz, és a kulcs engedélyezési házirendeket is konfigurálhatja. Ha olyan adatfolyamot kell megadni a Windows Media Player van szükség, a Media Services megadott kulcsot használja az dinamikusan titkosítani az AES titkosítással. Az adatfolyam visszafejtése, a Windows Media player kér a kulcsot a fő kézbesítési szolgáltatás. Annak megállapításához, hogy a felhasználó jogosult-e a kulcs eléréséhez, a szolgáltatás értékeli az engedélyezési házirendeket, amelyek a kulcshoz megadott.

A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. A tartalomkulcs-hitelesítési házirend egy vagy több engedélyezési korlátozások, nyitott vagy token korlátozások is rendelkezhetnek. A token által korlátozott házirend biztonságijogkivonat-szolgáltatás (STS) által kiadott tokennek kell csatolni. A Media Services tokeneket támogatja a [egyszerű webes jogkivonat](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_2) (SWT) és [JSON Web Token](https://msdn.microsoft.com/library/gg185950.aspx#BKMK_3) (JWT) formátumú. További információkért lásd: [a tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy).

A dinamikus titkosítás által nyújtott előnyök kihasználásához többszörös sávszélességű MP4-fájlokat vagy Smooth Streaming-forrásfájlokat tartalmazó objektummal kell rendelkeznie. Azt is konfigurálnia kell az eszköz (a cikk későbbi részében leírt) továbbítási szabályzatát. Ezt követően a streamelési URL-címben megadott formátumnak megfelelően, az igény szerinti adatfolyam-továbbítási kiszolgáló biztosítja, hogy az adatfolyam kiválasztott protokollal továbbítsa van. Ennek eredményeképpen kell tárolni, és csak a fájlok egyetlen tárolási formátumban kell fizetnie. A Media Services alapszik, és betölti az ügyféltől érkező kérésnek megfelelő választ.

Ez a cikk akkor hasznos, a fejlesztők számára, akik védett médiafájlok továbbításával foglalkoznak. A cikk bemutatja, hogyan konfigurálja a kulcs kézbesítési szolgáltatás engedélyezési házirendeket, hogy csak az arra jogosult ügyfelek megkaphatja a titkosítási kulcsokat. Azt is bemutatja, hogyan dinamikus titkosítás használatához.


## <a name="aes-128-dynamic-encryption-and-key-delivery-service-workflow"></a>AES-128, a dinamikus titkosítás és a kulcs kézbesítési szolgáltatás munkafolyamat

A Media Services kulcs kézbesítési szolgáltatás segítségével, valamint a dinamikus titkosítás segítségével a AES eszközök titkosításakor, hajtsa végre a következő általános lépéseket:

1. [Hozzon létre egy eszközt, és a fájlok feltöltése az objektumba](media-services-protect-with-aes128.md#create_asset).

2. [Az adaptív sávszélességű MP4-készlet fájlt tartalmazó objektum kódolása](media-services-protect-with-aes128.md#encode_asset).

3. [Hozzon létre egy tartalomkulcsot, majd társítsa a kódolt objektumhoz](media-services-protect-with-aes128.md#create_contentkey). A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítási kulcsát.

4. [A tartalomkulcs hitelesítési szabályzatának konfigurálása](media-services-protect-with-aes128.md#configure_key_auth_policy). Konfigurálnia kell a tartalomkulcs-hitelesítési házirendet. Az ügyfél a tartalomkulcsot a rendszer az ügyfél előtt meg kell felelnie a házirendet.

5. [Konfigurálja az az objektum továbbítási szabályzatát](media-services-protect-with-aes128.md#configure_asset_delivery_policy). A továbbítási szabályzat konfigurációjához a kulcs licenckérési URL-cím és egy inicializálási vektor (IV) tartalmaz. (Az AES-128 azonos IV a szükséges titkosítási és visszafejtési.) A konfiguráció emellett a továbbítási protokoll (például MPEG-DASH, HLS, Smooth Streaming vagy az összes) és a dinamikus titkosítás (például a boríték vagy a dinamikus titkosítás nélkül).

    Minden protokoll, az azonos eszköz egy másik házirendet alkalmazhat. Alkalmazhat például PlayReady-titkosítás Smooth/DASH és az AES envelope pedig csak a HLS. Nem a továbbítási szabályzatban meghatározott protokollok streameléshez sem. (Például akkor, ha egyetlen házirend hozzáadása, amely a HLS csak protokollként.) Kivételt jelent, ha egyáltalán nem állít be objektumtovábbítási szabályzatot egyáltalán. Ezután minden protokoll engedélyezett szövegként.

6. [Hozzon létre egy OnDemand-kereső](media-services-protect-with-aes128.md#create_locator) lekérni egy adatfolyam-továbbítási URL-címet.

A cikk azt is ismerteti [hogyan ügyfélalkalmazás is kérhet egy kulcsot a fő kézbesítési szolgáltatás](media-services-protect-with-aes128.md#client_request).

Teljes található [.NET típusú példát](media-services-protect-with-aes128.md#example) a cikk végén.

Az alábbi képen a korábban leírt munkafolyamatot láthatja. Itt a jogkivonat-hitelesítéshez használt.

![Védelem 128 bites AES-titkosítással](./media/media-services-content-protection-overview/media-services-content-protection-with-aes.png)

Ez a cikk fennmaradó magyarázatokat, kódmintákat és olyan, amelyek bemutatják a korábban leírt műveleteket eléréséhez témakörökre mutató hivatkozásokat biztosít.

## <a name="current-limitations"></a>Aktuális korlátozások
Ha hozzáadásakor vagy módosításakor az adategység továbbítási házirendjét, akkor törölje a meglévő lokátort, és hozzon létre egy új lokátort.

## <a id="create_asset"></a>Hozzon létre egy eszközt, majd fájlok feltöltése az objektumba
Kezeléséhez, kódolásához és streameléséhez videók, hogy először tölteni a tartalmat a Media Services. Miután feltöltése, a lesz biztonságosan tárolva a tartalom további feldolgozás és adatfolyam-felhő. 

További információkért lásd: [fájlok feltöltése a Media Services-fiók](media-services-dotnet-upload-files.md).

## <a id="encode_asset"></a>Az adaptív sávszélességű MP4 típusú beállításkészlettel fájlt tartalmazó objektum kódolása
A dinamikus titkosítás amelynek többszörös sávszélességű MP4-fájlokat vagy többféle sávszélességű Smooth Streaming-forrásfájlokat tartalmazó objektumot hoz létre. Ezt követően a jegyzék vagy töredék kérelemben megadott formátumnak megfelelően, az igény szerinti adatfolyam-továbbítási kiszolgáló biztosítja az adatfolyam kapni a kiválasztott protokoll. Ezt követően csak szeretné tárolni és kifizetni a fájlok egyetlen tárolási formátumban. A Media Services alapszik, és betölti az ügyféltől érkező kérésnek megfelelő választ. További információkért lásd: [dinamikus becsomagolás áttekintése](media-services-dynamic-packaging-overview.md).

>[!NOTE]
>A Media Services-fiók létrehozásakor egy alapértelmezett streamvégpontból kerül fiókját a "Leállítva" állapotban. Indítsa el a tartalom streaming és a dinamikus csomagolás és a dinamikus titkosítás, a streamvégpontján, amelyből el kívánja adatfolyamot kell a "Fut" állapotú. 
>
>Emellett a dinamikus csomagolás és a dinamikus titkosítás használatához az objektumot kell foglal magában adaptív sávszélességű MP4 vagy Smooth Streaming-fájlsorozattá.

Kódolással útmutatásért lásd: [egy eszköz kódolása a Media Encoder Standard használatával](media-services-dotnet-encode-with-media-encoder-standard.md).

## <a id="create_contentkey"></a>Tartalomkulcs létrehozása és társítása a kódolt objektumhoz
A Media Services szolgáltatásban a tartalomkulcs tartalmazza az objektum titkosítására használható kulcsot.

További információkért lásd: [hozzon létre egy tartalomkulcsot](media-services-dotnet-create-contentkey.md).

## <a id="configure_key_auth_policy"></a>A tartalomkulcs hitelesítési szabályzatának konfigurálása
A Media Services szolgáltatásban több különböző módot is beállíthat, amelynek segítségével a rendszer hitelesítheti a kulcskérelmet küldő felhasználókat. Konfigurálnia kell a tartalomkulcs-hitelesítési házirendet. Az ügyfél (a lejátszó) meg kell felelnie a házirendet, a kulcs el lehet juttatni az ügyfél előtt. A tartalomkulcs-hitelesítési házirend egy vagy több engedélyezési korlátozás, vagy nyissa meg a, lexikális elem: korlátozás vagy IP-korlátozás is rendelkezhetnek.

További információkért lásd: [konfigurálja a tartalomkulcs-hitelesítési házirendet](media-services-dotnet-configure-content-key-auth-policy.md).

## <a id="configure_asset_delivery_policy"></a>Objektumtovábbítási szabályzat konfigurálása
Konfigurálja az objektum továbbítási szabályzatát. Néhány dolog, amely tartalmazza az eszköz továbbítási szabályzat konfigurációjához a következők:

* A kulcs licenckérési URL-címe. 
* Az inicializálási vektor (IV) használata a boríték titkosításhoz. AES-128 azonos IV titkosításához és visszafejtéséhez szükséges. 
* Az objektumtovábbítási protokoll (például MPEG-DASH, HLS, Smooth Streaming vagy az összes).
* A dinamikus titkosítás (például AES envelope) típusú vagy a dinamikus titkosítás nélkül. 

További információkért lásd: [objektumtovábbítási szabályzat konfigurálása](media-services-dotnet-configure-asset-delivery-policy.md).

## <a id="create_locator"></a>Hozzon létre egy OnDemand-lokátor a streamelési URL-cím beszerzése
Meg kell adnia a felhasználó a streamelési URL-címet, Smooth Streaming, DASH vagy HLS.

> [!NOTE]
> Ha hozzáadásakor vagy módosításakor az adategység továbbítási házirendjét, akkor törölje a meglévő lokátort, és hozzon létre egy új lokátort.
> 
> 

További információk az objektumok közzétételéről és a streamelési URL-cím létrehozásáról: [Build a streaming URL](media-services-deliver-streaming-content.md) (Streamelési URL-cím létrehozása).

## <a name="get-a-test-token"></a>Tesztjogkivonat lekérése
Kérje le a kulcshitelesítési szabályzatban használt jogkivonat-korlátozásoknak megfelelő tesztjogkivonatot.

    // Deserializes a string containing an Xml representation of a TokenRestrictionTemplate
    // back into a TokenRestrictionTemplate class instance.
    TokenRestrictionTemplate tokenTemplate = 
        TokenRestrictionTemplateSerializer.Deserialize(tokenTemplateString);

    // Generate a test token based on the data in the given TokenRestrictionTemplate.
    //The GenerateTestToken method returns the token without the word "Bearer" in front
    //so you have to add it in front of the token string. 
    string testToken = TokenRestrictionTemplateSerializer.GenerateTestToken(tokenTemplate);
    Console.WriteLine("The authorization token is:\nBearer {0}", testToken);

Használhatja a [Azure Media Services Player](http://amsplayer.azurewebsites.net/azuremediaplayer.html) a stream kipróbálásához.

## <a id="client_request"></a>Hogyan is az ügyfél kérhet egy kulcsot a fő kézbesítési szolgáltatás?
Az előző lépésben összeállított jegyzékfájlt mutató URL-CÍMÉT. Az ügyfél a szükséges információk kinyerése indítson egy lekérdezést a kulcs kézbesítési szolgáltatás az adatfolyam-továbbítási fájlok kell.

### <a name="manifest-files"></a>Fájlok
Az ügyfélnek kell bontsa ki az URL-cím (tartalom is tartalmazó kulcsazonosító [kid]) a jegyzékfájl közötti értéket. Az ügyfél megpróbálja a titkosítási kulcs beszerzése a kulcs kézbesítési szolgáltatás. Az ügyfél kell IV értékének kinyerése és felhasználhatják az adatfolyam dekódolására. Az alábbi kódrészletben látható a <Protection> elem a Smooth Streaming jegyzékfájl:

    <Protection>
      <ProtectionHeader SystemID="B47B251A-2409-4B42-958E-08DBAE7B4EE9">
        <ContentProtection xmlns:sea="urn:mpeg:dash:schema:sea:2012" schemeIdUri="urn:mpeg:dash:sea:2012">
          <sea:SegmentEncryption schemeIdUri="urn:mpeg:dash:sea:aes128-cbc:2013"/>
          <sea:KeySystem keySystemUri="urn:mpeg:dash:sea:keysys:http:2013"/>
          <sea:CryptoPeriod IV="0xD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7" 
                            keyUriTemplate="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
                                            kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d"/>
        </ContentProtection>
      </ProtectionHeader>
    </Protection>

HLS, ha a legfelső szintű jegyzékfájl lebontva szegmens fájlokat. 

Például a legfelső szintű jegyzékfájl van: http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/manifest(format=m3u8-aapl). A szegmens fájlnevek listáját tartalmazza.

    . . . 
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=630133,RESOLUTION=424x240,CODECS="avc1.4d4015,mp4a.40.2",AUDIO="audio"
    QualityLevels(514369)/Manifest(video,format=m3u8-aapl)
    #EXT-X-STREAM-INF:PROGRAM-ID=1,BANDWIDTH=965441,RESOLUTION=636x356,CODECS="avc1.4d401e,mp4a.40.2",AUDIO="audio"
    QualityLevels(842459)/Manifest(video,format=m3u8-aapl)
    …

Ha egy szegmens fájlt egy szövegszerkesztőben (például http://test001.origin.mediaservices.windows.net/8bfe7d6f-34e3-4d1a-b289-3e48a8762490/BigBuckBunny.ism/QualityLevels(514369)/Manifest(video,format=m3u8-aapl), it contains # megnyitása EXT X-kulcs, ami azt jelenti, hogy a fájl titkosítva van.

    #EXTM3U
    #EXT-X-VERSION:4
    #EXT-X-ALLOW-CACHE:NO
    #EXT-X-MEDIA-SEQUENCE:0
    #EXT-X-TARGETDURATION:9
    #EXT-X-KEY:METHOD=AES-128,
    URI="https://wamsbayclus001kd-hs.cloudapp.net/HlsHandler.ashx?
         kid=da3813af-55e6-48e7-aa9f-a4d6031f7b4d",
            IV=0XD7D7D7D7D7D7D7D7D7D7D7D7D7D7D7D7
    #EXT-X-PROGRAM-DATE-TIME:1970-01-01T00:00:00.000+00:00
    #EXTINF:8.425708,no-desc
    Fragments(video=0,format=m3u8-aapl)
    #EXT-X-ENDLIST

>[!NOTE] 
>Ha azt tervezi, egy AES által titkosított HLS játszanak Safari, lásd: [ebben a blogban](https://azure.microsoft.com/blog/how-to-make-token-authorized-aes-encrypted-hls-stream-working-in-safari/).

### <a name="request-the-key-from-the-key-delivery-service"></a>A kulcs kér a kulcs kézbesítési szolgáltatás

A következő kód bemutatja, hogyan kérelmet küld a Media Services kulcs kézbesítési szolgáltatás egy kulcs kézbesítési URI-azonosítóhoz (a jegyzékfájl kinyert) használatával, valamint a jogkivonatot. (Ez a cikk nem azt ismertetik, hogyan SWTs beszerezni az STS szolgáltatással.)

    private byte[] GetDeliveryKey(Uri keyDeliveryUri, string token)
    {
        HttpWebRequest request = (HttpWebRequest)WebRequest.Create(keyDeliveryUri);

        request.Method = "POST";
        request.ContentType = "text/xml";
        if (!string.IsNullOrEmpty(token))
        {
            request.Headers[AuthorizationHeader] = token;
        }
        request.ContentLength = 0;

        var response = request.GetResponse();

        var stream = response.GetResponseStream();
        if (stream == null)
        {
            throw new NullReferenceException("Response stream is null");
        }

        var buffer = new byte[256];
        var length = 0;
        while (stream.CanRead && length <= buffer.Length)
        {
            var nexByte = stream.ReadByte();
            if (nexByte == -1)
            {
                break;
            }
            buffer[length] = (byte)nexByte;
            length++;
        }
        response.Close();

        // AES keys must be exactly 16 bytes (128 bits).
        var key = new byte[length];
        Array.Copy(buffer, key, length);
        return key;
    }

## <a name="protect-your-content-with-aes-128-by-using-net"></a>Tartalomvédelemre az AES-128 .NET használatával

### <a name="create-and-configure-a-visual-studio-project"></a>Egy Visual Studio-projekt létrehozása és konfigurálása

1. A fejlesztési környezet beállítását, és feltöltése az app.config fájlban a kapcsolatadatok, a [Media Services-fejlesztés a .NET](media-services-dotnet-how-to-use.md).

2. Vegye fel a következő elemek appSettings, az app.config fájlban meghatározottak szerint:

        <add key="Issuer" value="http://testacs.com"/>
        <add key="Audience" value="urn:test"/>

### <a id="example"></a>Példa

Írja felül a Program.cs fájlban található kódot az itt látható kóddal.
 
>[!NOTE]
>1 000 000 házirendek eltérő Media Services házirendek (például a lokátor házirend vagy ContentKeyAuthorizationPolicy) korlátozva van. Az azonos házirend-azonosító akkor használhatók, ha a ugyanazok a nap/hozzáférési engedélyek mindig használja. Példa: a lokátorokat, amelyek célja, hogy továbbra is érvényben hosszú ideje (nem feltöltés házirendek) házirendeket. További információkért lásd: a "Korlát hozzáférési házirendek" szakasz [kezelése az eszközök és a kapcsolódó entitásokból a Media Services .NET SDK-val](media-services-dotnet-manage-entities.md#limit-access-policies).

Módosítsa úgy a változókat, hogy a bemeneti fájlok tárolásához Ön által használt mappákra mutassanak.

    [!code-csharp[Main](../../samples-mediaservices-encryptionaes/DynamicEncryptionWithAES/DynamicEncryptionWithAES/Program.cs)]

## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

