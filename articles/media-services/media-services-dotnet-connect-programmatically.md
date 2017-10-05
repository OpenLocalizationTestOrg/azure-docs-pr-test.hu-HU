---
title: "Kapcsolódás a Media Services-fiók .NET használatával"
description: "Ebben a témakörben bemutatjuk, hogyan csatlakozhat a Media Services előrejelzése .NET."
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
ms.openlocfilehash: 892932116934952265a21ab17aac3434b5760136
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connecting-to-media-services-account-using-media-services-sdk-for-net"></a>Kapcsolódás a Media Services SDK használatával a .NET-keretrendszerhez készült Media Services-fiók
> [!div class="op_single_selector"]
> * [REST](media-services-rest-connect-programmatically.md)
> * [.NET](media-services-dotnet-connect-programmatically.md)
> 
> 

Ez a témakör ismerteti az beszerzése programozott kapcsolódni a Microsoft Azure Media Services, ha a .NET-hez programozási a Media Services SDK-val.

## <a name="connecting-to-media-services"></a>Csatlakozás a Media Services
Csatlakozhat a Media Services programozott módon, meg kell rendelkezik korábban már beállított egy Azure-fiókra, a Media Services konfigurált fiók és hozzon létre egy Visual Studio-projekt fejlesztési a Media Services SDK-t a .NET-hez. További információkért lásd: a telepítő fejlesztési a Media Services SDK-t a .NET-hez.

A Media Services-fiók telepítési folyamat végén szerezte be a következő kapcsolathoz szükséges értékeket. Ezek segítségével programozott kapcsolat létrehozása a Media Services.

* A Media Services-fiók neve.
* A Media Services kulcsára.

Ezek az értékek megkereséséhez nyissa meg az Azure Managmentet portálra, válassza ki a Media Services-fiókját, és kattintson a a "**kulcsok kezelése**" ikon a portál ablakának alján. A szövegdobozok melletti ikonokra való kattintás a rendszer vágólapjára másolja az értékeket.

## <a name="creating-a-cloudmediacontext-instance"></a>CloudMediaContext példány létrehozása
Elleni kell létrehoznia a Media Services programozási elindítani egy **CloudMediaContext** példányt jelenti. a kiszolgáló a környezetben. A **CloudMediaContext** többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők fontos gyűjtemények mutató hivatkozásokat tartalmaz.

> [!NOTE]
> A **CloudMediaContext** osztály nincs többszálú futtatásra. Hozzon létre egy új CloudMediaContext szálankénti, vagy a műveletek.
> 
> 

CloudMediaContext öt konstruktor túlterheléssel rendelkezik. Javasoljuk, hogy használja a konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként. További információkért lásd: a **újból felhasználja a vezérlő szolgáltatás jogkivonatot** , amely a következő. 

A következő példában a CloudMediaContext(MediaServicesCredentials credentials) nyilvános konstruktora:

    // _cachedCredentials and _context are class member variables. 
    _cachedCredentials = new MediaServicesCredentials(
                    _mediaServicesAccountName,
                    _mediaServicesAccountKey);

    _context = new CloudMediaContext(_cachedCredentials);


## <a name="reusing-access-control-service-tokens"></a>Újbóli felhasználása a vezérlő szolgáltatás jogkivonatot
Ez a szakasz bemutatja, hogyan használja fel a hozzáférés-vezérlési szolgáltatásban jogkivonatok CloudMediaContext konstruktorok használatát, amelyek paraméterként MediaServicesCredentials használatával.

[Az Azure Active Directory hozzáférés-vezérlés](https://msdn.microsoft.com/library/hh147631.aspx) (más néven a hozzáférés-vezérlési szolgáltatásban vagy az ACS) ahhoz, hogy hozzáférjenek a webes alkalmazások és felhasználók hitelesítésére egyszerűen biztosító felhőalapú szolgáltatás. A Microsoft Azure Media Services, ha meghatározza a a szolgáltatásokhoz való hozzáférés, OAuth protokollt igényel az ACS-jogkivonatot. A Media Services az ACS-jogkivonatokat kap egy engedélyezési kiszolgálóról.

A Media Services SDK-val fejlesztésekor választhat foglalkozik a jogkivonatokat, mert az SDK-kód kezelők meg őket. Azonban ha engedélyezi, hogy a teljes körű felügyeletéhez az ACS-jogkivonatokat SDK vezet szükségtelen jogkivonat-kérelmeket. A kért jogkivonatok időt vesz igénybe, és az ügyfél és kiszolgáló-erőforrásokat használ fel. Emellett az ACS-kiszolgálónak azelőtt gyorsítja fel a kérelmeket, ha az érték túl magas. A határérték 30 kérelmek / másodperc, lásd: [ACS Service korlátozásai](https://msdn.microsoft.com/library/gg185909.aspx) további részleteket.

A Media Services SDK verzió 3.0.0.0 verziótól kezdődően az ACS-jogkivonatokat is felhasználhatja. A **CloudMediaContext** konstruktorok használatát, amelyek **MediaServicesCredentials** paraméterként a ACS jogkivonatok több környezet közötti megosztásának engedélyezése. A MediaServicesCredentials osztály magában foglalja a Media Services hitelesítő adataival. Ha rendelkezésre áll az ACS-jogkivonatot, és a lejárati ideje ismert, hozzon létre egy új MediaServicesCredentials-példányt a jogkivonatot, és adja át CloudMediaContext konstruktorának. Vegye figyelembe, hogy a Media Services SDK automatikusan frissíti jogkivonatokat, amikor a várósorban. Két módon újból az ACS-jogkivonatokat, az alábbi példában látható módon.

* Gyorsítótárazhatja a **MediaServicesCredentials** a memóriában (például a statikus osztály változóban) objektum. Ezt követően adja át a gyorsítótárazott objektum a CloudMediaContext konstruktor. A MediaServicesCredentials objektum tartalmaz, amelyek felhasználhatók, ha továbbra is érvényes ACS-jogkivonatot. Ha a jogkivonat nem érvényes, akkor frissülnek a Media Services SDK-ban a MediaServicesCredentials konstruktor megadott hitelesítő adatok használatával.
  
    Vegye figyelembe, hogy a **MediaServicesCredentials** objektum egy érvényes tokent kap, miután a RefreshToken nevezik. A **CloudMediaContext** hívások a **RefreshToken** módszer a konstruktorban. Ha azt tervezi, hogy a jogkivonat értékeket menteni egy külső tárhelyen, mindenképpen ellenőrizze, hogy TokenExpiration értéke érvénytelen a jogkivonat-adatainak mentése előtt. Érvénytelen, hívjuk RefreshToken gyorsítótárazás előtt.
  
        // Create and cache the Media Services credentials in a static class variable.
        _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);

        // Use the cached credentials to create a new CloudMediaContext object.
        if(_cachedCredentials == null)
        {
            _cachedCredentials = new MediaServicesCredentials(_mediaServicesAccountName, _mediaServicesAccountKey);
        }

        CloudMediaContext context = new CloudMediaContext(_cachedCredentials);

* Az AccessToken karakterlánc és a TokenExpiration értékek is gyorsítótárazza. Az értékek később használható hozzon létre egy új MediaServicesCredentials objektumot a gyorsítótárazott token adatokkal.  Ez különösen fontos a forgatókönyvek, ahol a token biztonságosan megoszthatók több folyamatok vagy számítógépek között.
  
    Az alábbi kódrészleteket metódushívások a SaveTokenDataToExternalStorage GetTokenDataFromExternalStorage és UpdateTokenDataInExternalStorageIfNeeded, amelyek nincsenek meghatározva ebben a példában. Ezek a módszerek tárolásához, beolvasása, és frissíti egy külső tároló token adatait adható meg. 
  
        CloudMediaContext context1 = new CloudMediaContext(_mediaServicesAccountName, _mediaServicesAccountKey);
  
        // Get token values from the context.
        var accessToken = context1.Credentials.AccessToken;
        var tokenExpiration = context1.Credentials.TokenExpiration;
  
        // Save token values for later use. 
        // The SaveTokenDataToExternalStorage method should check 
        // whether the TokenExpiration value is valid before saving the token data. 
        // If it is not valid, call MediaServicesCredentials’s RefreshToken before caching.
        SaveTokenDataToExternalStorage(accessToken, tokenExpiration);
  
    A mentett lexikális elemek értékének MediaServicesCredentials létrehozásához használja.

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

    A token példány frissítése, abban az esetben, ha a jogkivonat a Media Services SDK frissítette. 

        if(tokenExpiration != context2.Credentials.TokenExpiration)
        {
            UpdateTokenDataInExternalStorageIfNeeded(accessToken, context2.Credentials.TokenExpiration);
        }


* Ha több Media Services-fiókot (például földrajzi terjesztési vagy célú terheléselosztási) gyorsítótárazhatja a System.Collections.Concurrent.ConcurrentDictionary gyűjtemény (a ConcurrentDictionary MediaServicesCredentials az objektumok gyűjtemény számoknak szálbiztos kulcs/érték párok elérhető szálak egyidejű). Ezután használhatja a GetOrAdd módszert a gyorsítótárazott hitelesítő adatok elkérése céljából. 
  
        // Declare a static class variable of the ConcurrentDictionary type in which the Media Services credentials will be cached.  
        private static readonly ConcurrentDictionary<string, MediaServicesCredentials> mediaServicesCredentialsCache = 
            new ConcurrentDictionary<string, MediaServicesCredentials>();

        // Cache (or get already cached) Media Services credentials. Use these credentials to create a new CloudMediaContext object.
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

## <a name="connecting-to-a-media-services-account-located-in-the-north-china-region"></a>Észak-Kína régióban Media Services-fiókhoz való csatlakozást
Ha a fiók az Észak-Kína régióban található, használja az alábbi konstruktort:

    public CloudMediaContext(Uri apiServer, string accountName, string accountKey, string scope, string acsBaseAddress)

Példa:

    _context = new CloudMediaContext(
        new Uri("https://wamsbjbclus001rest-hs.chinacloudapp.cn/API/"),
        _mediaServicesAccountName,
        _mediaServicesAccountKey,
        "urn:WindowsAzureMediaServices",
        "https://wamsprodglobal001acs.accesscontrol.chinacloudapi.cn");


## <a name="storing-connection-values-in-configuration"></a>A konfigurációban kapcsolat értékek tárolására
Erősen ajánlott gyakorlat konfigurációban kapcsolat értékek, a fiók nevét és jelszavát, például különösen bizalmas értékek tárolására is. Azt is ajánlott gyakorlat bizalmas konfigurációs adatainak titkosításához. A Windows titkosított fájlrendszer (EFS) használatával titkosíthatja a teljes konfigurációs fájlt. Ahhoz, hogy a fájl EFS, kattintson jobb gombbal a fájlra, válassza ki **tulajdonságok**, és a titkosítás engedélyezéséhez a **speciális** beállítások lapon. Vagy egy egyéni megoldás kijelölt részei egy konfigurációs fájl titkosítása védett konfigurációs használatával hozhat létre. Lásd: [titkosított konfigurációs információit védett konfigurációs](https://msdn.microsoft.com/library/53tyfkaw.aspx).

A következő App.config fájl kapcsolathoz szükséges értékeket tartalmazza. Az értékek a <appSettings> elem, amely a Media Services-fiók telepítő folyamat során kapott azonosítóértékeket szükséges értékeket.

    <configuration>
      <appSettings>
        <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
        <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
      </appSettings>
    </configuration>


Konfigurációs kapcsolati értékek lekéréséhez használja a **ConfigurationManager** osztályhoz, és hozzárendelheti a értékek mezők a kódban:

    private static readonly string _accountName = ConfigurationManager.AppSettings["MediaServicesAccountName"];
    private static readonly string _accountKey = ConfigurationManager.AppSettings["MediaServicesAccountKey"];



## <a name="media-services-learning-paths"></a>Media Services képzési tervek
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Visszajelzés küldése
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

