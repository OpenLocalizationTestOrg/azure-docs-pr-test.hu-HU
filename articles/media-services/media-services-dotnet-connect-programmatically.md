---
title: "aaaConnecting tooMedia Services-fiók segítségével .NET"
description: "Ez a témakör bemutatja, hogyan tooconnect tooMedia szolgáltatások előrejelzése .NET."
services: media-services
documentationcenter: 
author: juliako
manager: erikre
editor: 
ms.assetid: a8412a29-59dc-44a0-ace0-be79a97dab63
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
ms.openlocfilehash: a23bd285f7cae17ae5831e1e50e73947afbb9a3d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connecting-toomedia-services-account-using-media-services-sdk-for-net"></a>Csatlakozás tooMedia Services-fiók .NET-keretrendszerhez készült Media Services SDK használatával
> [!div class="op_single_selector"]
> * [REST](media-services-rest-connect-programmatically.md)
> * [.NET](media-services-dotnet-connect-programmatically.md)
> 
> 

Ez a témakör ismerteti, hogyan tooobtain egy Azure Media Services, amikor a rendszer programozás a szoftveres kapcsolatot tooMicrosoft hello .NET-keretrendszerhez készült Media Services SDK-t.

## <a name="connecting-toomedia-services"></a>Csatlakozás tooMedia szolgáltatások
tooconnect tooMedia szolgáltatások programozott módon, akkor kell korábban már beállított egy Azure-fiókra, beállította a Media Services a fiókban, és hozzon létre egy Visual Studio-projekt-val történő fejlesztés hello Media Services SDK for .NET. További információ: fejlesztési telepítő hello Media Services SDK-t a .NET-keretrendszerhez készült.

Hello Media Services-fiók telepítési folyamat végén hello szükséges hello következő beszerzett kapcsolati értékek. Ezek toomake programozott kapcsolatokhoz tooMedia szolgáltatások.

* A Media Services-fiók neve.
* A Media Services kulcsára.

Ezek az értékek toofind toohello Azure Managmentet Portal nyissa, válassza ki a Media Services-fiókját, és kattintson az hello "**kulcsok kezelése**" hello portál ablakának alján hello ikonra. Kattintson a hello ikon következő tooeach szöveg mezőben másolatok hello érték toohello rendszer vágólapjára.

## <a name="creating-a-cloudmediacontext-instance"></a>CloudMediaContext példány létrehozása
Media elleni programozási toocreate kell szolgáltatások toostart egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány. Hello **CloudMediaContext** magában foglalja többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők hivatkozások tooimportant gyűjtemények.

> [!NOTE]
> Hello **CloudMediaContext** osztály nincs többszálú futtatásra. Hozzon létre egy új CloudMediaContext szálankénti, vagy a műveletek.
> 
> 

CloudMediaContext öt konstruktor túlterheléssel rendelkezik. Ajánlott toouse konstruktorok használatát, amelyek már **MediaServicesCredentials** paraméterként. További információkért lásd: hello **újból felhasználja a vezérlő szolgáltatás jogkivonatot** , amely a következő. 

hello következő példában hello nyilvános CloudMediaContext(MediaServicesCredentials credentials) konstruktora:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Újbóli felhasználása a vezérlő szolgáltatás jogkivonatot
Ez a szakasz bemutatja, hogyan tooreuse hozzáférés-vezérlési szolgáltatásban jogkivonatok CloudMediaContext konstruktorok használatát, amelyek paraméterként MediaServicesCredentials használatával.

[Az Azure Active Directory hozzáférés-vezérlés](https://msdn.microsoft.com/library/hh147631.aspx) (más néven a hozzáférés-vezérlési szolgáltatásban vagy az ACS), amely lehetővé teszi a felhasználók toogain hozzáférés hitelesítése és engedélyező egyszerűen tootheir webes felhőalapú szolgáltatás. A Microsoft Azure Media Services szabályozza a hozzáférést tooits szolgáltatások azonban OAuth protokollt igényel az ACS-jogkivonatot. A Media Services hello ACS-jogkivonatokat kap az engedélyezési kiszolgálóról.

A Media Services SDK hello fejlesztésekor toonot üzlet hello jogkivonatokkal választhat, mert SDK kód kezelők hello meg őket. Azzal kapcsolatban hello SDK azonban teljes körű felügyeletéhez hello ACS jogkivonatok érdeklődők toounnecessary jogkivonat-kérelmeket. A kért jogkivonatok időt vesz igénybe, és hello ügyfél és kiszolgáló-erőforrásokat használ fel. Emellett hello ACS-kiszolgálónak azelőtt gyorsítja fel hello kérelmek Ha hello érték túl magas. hello határérték 30 kérelmek / másodperc, lásd: [ACS Service korlátozásai](https://msdn.microsoft.com/library/gg185909.aspx) további részleteket.

Media Services SDK verzió 3.0.0.0 hello verziótól kezdődően hello ACS-jogkivonatokat is felhasználhatja. Hello **CloudMediaContext** konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként engedélyezése a megosztási hello ACS jogkivonatok több környezet között. hello MediaServicesCredentials osztály magában foglalja a hello Media Services hitelesítő adataival. Ha rendelkezésre áll az ACS-jogkivonatot, és a lejárati ideje ismert, hozzon létre egy új MediaServicesCredentials példányt hello jogkivonatot, és adja át a CloudMediaContext toohello konstruktor. Vegye figyelembe, hogy a Media Services SDK hello automatikusan frissíti a jogkivonatokat, amikor járnak. Két módon tooreuse ACS jogkivonatokat, ahogy az alábbi példák hello.

* Gyorsítótárazhatja a hello **MediaServicesCredentials** a memóriában (például a statikus osztály változóban) objektum. Ezt követően adja át a gyorsítótárazott hello objektum toohello CloudMediaContext konstruktor. hello MediaServicesCredentials objektum tartalmaz, amelyek felhasználhatók, ha továbbra is érvényes ACS-jogkivonatot. Ha hello jogkivonat nem érvényes, hello frissülnek a Media Services SDK hello hitelesítő adataival megadott toohello MediaServicesCredentials konstruktor.
  
    Vegye figyelembe, hogy hello **MediaServicesCredentials** objektum egy érvényes tokent kap hello RefreshToken neve után. Hello **CloudMediaContext** hívások hello **RefreshToken** hello konstruktor metódust. Ha azt tervezi, toosave hello lexikális elemek értékének tooan külső tárhelyen, hogy hello TokenExpiration érték érvényes hello token adatok mentése előtt győződjön meg arról, hogy toocheck. Érvénytelen, hívjuk RefreshToken gyorsítótárazás előtt.
  
        // Create and cache hello Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use hello cached credentials toocreate a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* Hello AccessToken karakterlánc és hello TokenExpiration értékeket is gyorsítótárazza. hello értékek később lehet egy új MediaServicesCredentials objektum gyorsítótárazott hello token adatokkal használt toocreate.  Ez különösen fontos a forgatókönyvek, ahol hello token biztonságosan megoszthatók több folyamatok vagy számítógépek között.
  
    hello alábbi kódtöredékek metódushívások hello SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage és UpdateTokenDataInExternalStorageIfNeeded, amelyek nincsenek meghatározva ebben a példában. Ezen módszerek toostore, lekérése és token adatok frissítése sikerült megadása egy külső tárhelyen. 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from hello context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // hello SaveTokenDataToExternalStorage method should check 
        // whether hello TokenExpiration value is valid before saving hello token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    Mentett lexikális elemek értékének toocreate MediaServicesCredentials hello használata.

        var accessToken = "";
        var tokenExpiration = DateTime.UtcNow;

        // Retrieve saved token values.
        GetTokenDataFromExternalStorage(out accessToken, out tokenExpiration);

        // Create a new MediaServicesCredentials object using saved token values.
        MediaServicesCredentials credentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey)
        {
            AccessToken = accessToken,
            TokenExpiration = tokenExpiration
        };

        CloudMediaContext context2 = new CloudMediaContext(credentials);

    Hello token példány frissítése, abban az esetben hello token hello Media Services SDK frissítette. 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* Ha több Media Services-fiókot (például földrajzi terjesztési vagy célú terheléselosztási) gyorsítótárazhatja a MediaServicesCredentials objektumok hello System.Collections.Concurrent.ConcurrentDictionary gyűjtemény (hello használata ConcurrentDictionary gyűjtemény számoknak szálbiztos kulcs/érték párok elérhető szálak egyidejű). Ezután használhatja a hello GetOrAdd metódus tooget hello gyorsítótárazott hitelesítő adatokat. 
  
        // Declare a static class variable of hello ConcurrentDictionary type in which hello Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials toocreate a new CloudMediaContext object.
        static public CloudMediaContext CreateMediaServicesContext(string accountName, string accountKey)
        {
            CloudMediaContext cloudMediaContext;
            MediaServicesCredentials mediaServicesCredentials;

            mediaServicesCredentials = mediaServicesCredentialsCache.GetOrAdd(
                accountName,
                valueFactory => new MediaServicesCredentials(accountName, accountKey));

            cloudMediaContext = new CloudMediaContext(mediaServicesCredentials);

            return cloudMediaContext;
        }

## <a name="connecting-tooa-media-services-account-located-in-hello-north-china-region"></a>Csatlakozás tooa Media Services-fiók hello Észak Kína régióban található
Ha a fiók hello Észak Kína régióban található, használja a hello konstruktor a következő:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Példa:

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>A konfigurációban kapcsolat értékek tárolására
Egy erősen ajánlott gyakorlat toostore kapcsolati értékek, különösen bizalmas értékeket, például a fiók nevét és jelszavát, a konfigurációban is. Azt is egy ajánlott gyakorlat tooencrypt bizalmas konfigurációs adatokat. Hello Windows titkosított fájlrendszer (EFS) használatával titkosíthatja hello teljes konfigurációs fájlt. Válasszon egy fájlt, kattintson a jobb gombbal hello fájlt, a titkosított fájlrendszer tooenable **tulajdonságok**, és engedélyezheti a titkosítást a hello **speciális** beállítások lapon. Vagy egy egyéni megoldás kijelölt részei egy konfigurációs fájl titkosítása védett konfigurációs használatával hozhat létre. Lásd: [titkosított konfigurációs információit védett konfigurációs](https://msdn.microsoft.com/library/53tyfkaw.aspx).

következő App.config fájl hello szükséges hello kapcsolat értékeket tartalmaz. hello értékek hello <appSettings> elem hello Media Services-fiók telepítő folyamat során kapott azonosítóértékeket hello szükséges értékeket.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


konfigurációs tooretrieve kapcsolat értékeit, hello használható **ConfigurationManager** osztály, és hozzárendelheti a hello értékek toofields a kódban:

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

