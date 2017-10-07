---
title: "az Azure AD hitelesítési tooaccess Azure Media Services API többi aaaUse |} Microsoft Docs"
description: "Ismerje meg, hogyan REST-tooaccess Azure Media Services API az Azure Active Directory-hitelesítés használatával."
services: media-services
documentationcenter: 
author: willzhan
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: willzhan;juliako
ms.openlocfilehash: 114f7bdde3a8f5fe6189d712e05b6afdc8a8787a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-hello-azure-media-services-api-with-rest"></a>Használja az Azure AD hitelesítési tooaccess hello Azure Media Services API többi

hello Azure Media Services team közzétette az Azure Media Services access Azure Active Directory (Azure AD-) hitelesítés támogatása. Azt is bejelentette tervek toodeprecate Azure hozzáférés-vezérlés szolgáltatás hitelesítési a Media Services-hozzáféréshez. Mivel minden Azure-előfizetés, és minden Media Services-fiók csatolt tooan az Azure AD-bérlőt, az Azure AD hitelesítési támogatás számos lehetőséget kínál számos biztonsági szempontból előnyökkel járhat. A változás- és az áttelepítés (ha az alkalmazás hello Media Services .NET SDK-t használ) kapcsolatos részleteket hello következő blog küldi, és annak:

- [Az Azure Media Services időről támogatás az Azure AD és a hozzáférés-vezérlés hitelesítési elavulása](https://azure.microsoft.com/blog/azure%20media%20service%20aad%20auth%20and%20acs%20deprecation)
- [Access Azure Media Services API az Azure AD-hitelesítés használatával](media-services-use-aad-auth-to-access-ams-api.md)
- [Az Azure AD hitelesítési tooaccess Azure Media Services API használata a Microsoft .NET használatával](media-services-dotnet-get-started-with-aad.md)
- [Ismerkedés az Azure AD-alapú hitelesítés használatával hello Azure-portálon](media-services-portal-get-started-with-aad.md)

Néhány szükséges toodevelop a Media Services-megoldások hello megkötések a következő alapján:

*   Microsoft .NET vagy C# programozási nyelven használata, vagy hello futtatókörnyezetben nincs Windows.
*   Az Azure AD szalagtárak, például az Active Directory hitelesítési Kódtárai válnak elérhetővé a programozási nyelv hello, vagy nem használható a futtatókörnyezetben.

Egyes ügyfelek alkalmazások hozzáférés-vezérlés hitelesítéséhez és access Azure Media Services REST API használatával fejlesztett ki. Ezek az ügyfelek olyan módon toouse csak hello REST API-t az Azure AD-alapú hitelesítés van szüksége, és további Azure Media Services eléréséhez. Toonot kell bármelyik hello Azure AD-tárak vagy a Media Services .NET SDK hello támaszkodjon. Ez a cikk azt írják le a megoldást, és adja meg mintakód ehhez a forgatókönyvhöz. Mivel hello kód minden REST API-hívásokkal, minden Azure ad-val nem függőség vagy az Azure Media Services library, hello kód könnyen lehet lefordított tooany más programozási nyelv.

> [!IMPORTANT]
> Jelenleg a Media Services hello Azure hozzáférés-vezérlés szolgáltatások hitelesítési modellt támogatja. Azonban hozzáférés-vezérlés hitelesítési elavulttá válik 2018. június 1. Azt javasoljuk, hogy az áttelepített toohello az Azure AD hitelesítési modell lehető legrövidebb időn belül.


## <a name="design"></a>Tervezés

Ez a cikk a következő hitelesítési és engedélyezési tervezési hello használjuk:

*  Protokoll: OAuth 2.0-s. OAuth 2.0 olyan webes biztonsági szabvány, amely lefedi a hitelesítést és az engedélyezést. Azt támogatják Google, a Microsoft, a Facebook-on és a PayPal. 2012. októberi azt lett erősítette meg. A Microsoft az OAuth 2.0 és az OpenID Connect erősen támogatja. Mindkét ezeknek a szabványoknak támogatottak több szolgáltatások és ügyfél könyvtárak, beleértve az Azure Active Directoryban, Open Web Interface .NET (OWIN) Katana, a és az Azure AD-könyvtárak.
*  Támogatás típusa: ügyfél hitelesítő adatai megadják típusa. Ügyfél hitelesítő adatait az OAuth 2.0 hello négy grant típusok egyike. A Microsoft Azure AD Graph API access gyakran használják azt.
*  Hitelesítési módszer: egyszerű szolgáltatást. hello más hitelesítési mód felhasználói vagy interaktív hitelesítés.

Összesen négy alkalmazáshoz vagy szolgáltatáshoz is érintett hello Azure AD hitelesítési és engedélyezési folyamat a Media Services használatával. hello folyamata, hello alkalmazások és szolgáltatások hello a következő táblázat ismerteti:

|Alkalmazás típusa |Alkalmazás |Folyamat|
|---|---|---|
|Ügyfél | Felhasználói alkalmazás vagy megoldás | Ez az alkalmazás (ténylegesen, a proxy), mely hello Azure-előfizetést és hello media szolgáltatásfiók hozni hello Azure AD-bérlő regisztrálva van. hello szolgáltatás egyszerű hello regisztrált alkalmazás majd kap hello Access Control (IAM) hello media szolgáltatásfiók a tulajdonos vagy közreműködő szerepkörrel. hello szolgáltatás egyszerű hello app ID és az ügyfél ügyfélkulcs jelképezi. |
|Az identitásszolgáltató (IDP) | Azure AD szolgáltatásba IDP | hello regisztrált alkalmazás egyszerű szolgáltatásnév (ügyfél-azonosító és a titkos ügyfélkódot) hitelesítése az Azure AD szolgáltatásba hello IDP. Ez a hitelesítés a belső és implicit módon történik. Ügyfél-hitelesítő adatok folyamata, mint hello ügyfél hitelesítése hello felhasználó helyett. |
|Secure Token Service (STS) / OAuth-kiszolgáló | Azure AD szolgáltatásba STS | Hello IDP (vagy az OAuth 2.0 feltételeit OAuth-kiszolgáló) a hitelesítés után egy hozzáférési jogkivonat vagy JSON webes jogkivonat (JWT) által kiadott STS/OAuth-kiszolgálójaként, az Azure AD hozzáférési toohello középső rétegbeli erőforrás: hello ebben az esetben a Media Services REST API végpontra. |
|Erőforrás | Media Services REST API-k | Minden Media Services REST API hívása olyan hozzáférési jogkivonatot STS vagy hello OAuth kiszolgálóként az Azure AD által kiadott engedélyezve van. |

Hello mintakód futtatja, és rögzítheti a jwt-t vagy a hozzáférési tokent, ha a hello JWT rendelkezik a következő attribútumok hello:

    aud: "https://rest.media.azure.net",

    iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    iat: 1497146280,

    nbf: 1497146280,
    exp: 1497150180,

    aio: "Y2ZgYDjuy7SptPzO/muf+uRu1B+ZDQA=",

    appid: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6",

    appidacr: "1",

    idp: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/",

    oid: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    sub: "a938cfcc-d3de-479c-b0dd-d4ffe6f50f7c",

    tid: "72f988bf-86f1-41af-91ab-2d7cd011db47",

Íme hello hozzárendelések hello attribútumok hello jwt-t és hello négy alkalmazásokban vagy a tábla megelőző hello szolgáltatást között:

|Alkalmazás típusa |Alkalmazás |JWT-attribútum |
|---|---|---|
|Ügyfél |Felhasználói alkalmazás vagy megoldás |AppID: "02ed1e8e-af8b-477e-af3d-7e7219a99ac6". hello ügyfél-Azonosítóját az alkalmazás regisztrálni fogja tooAzure AD hello a következő szakaszban. |
|Az identitásszolgáltató (IDP) | Azure AD szolgáltatásba IDP |IDP: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/".  hello GUID az hello azonosítója a Microsoft bérlő (microsoft.onmicrosoft.com). Mindegyik bérlő saját, egyedi azonosítóval rendelkezik |
|Secure Token Service (STS) / OAuth-kiszolgáló |Azure AD szolgáltatásba STS | iss: "https://sts.windows.net/72f988bf-86f1-41af-91ab-2d7cd011db47/". hello GUID az hello azonosítója a Microsoft bérlő (microsoft.onmicrosoft.com). |
|Erőforrás | Media Services REST API-k |és: "https://rest.media.azure.net". hello címzett vagy a célközönség hello hozzáférési jogkivonat. |

## <a name="steps-for-setup"></a>A telepítés lépései

tooregister, és állítsa be az Azure AD-alapú hitelesítés és tooobtain hello Azure Media Services REST API-végpont, a következő lépéseket teljes hello meghívása a hozzáférési token egy Azure AD-alkalmazást:

1.  A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), regisztrálja az Azure AD alkalmazás (például wzmediaservice) toohello az Azure AD-bérlő (például microsoft.onmicrosoft.com). Nem számít, hogy Ön a webes alkalmazás vagy a natív alkalmazással regisztrált-e. Választhat is, a bejelentkezés és válasz URL-címe (például mindkét http://wzmediaservice.com).
2. A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), nyissa meg toohello **konfigurálása** fülre az alkalmazás. Megjegyzés: hello **ügyfél-azonosító**. Ekkor a **kulcsok**, létrehozni egy **ügyfélkulcsot** (titkos). 

    > [!NOTE] 
    > Jegyezze fel a hello titkos ügyfélkulcsot. Ez nem jeleníthető meg újra.
    
3.  A hello [Azure-portálon](http://ms.portal.azure.com), nyissa meg toohello Media Services-fiók. Jelölje be hello **hozzáférés-vezérlés** (IAM) ablak. Adja hozzá egy új tag a hello tulajdonos vagy a hello közreműködői szerepkört. Hello egyszerű keressen rá a hello alkalmazás neve (a példában wzmediaservice) 1. lépésben regisztrált.

## <a name="info-toocollect"></a>Információ toocollect

tooprepare REST-kódolási, a következő adatok pontok tooinclude hello kódban hello gyűjtése:

*   Azure AD szolgáltatásba STS-végpont: https://login.microsoftonline.com/microsoft.onmicrosoft.com/oauth2/token. Az ehhez a végponthoz a JWT jogkivonat van szükség. Ezen kívül, az IDP tooserving, az Azure AD is szolgál az STS szolgáltatással. Az Azure AD kibocsát egy jwt-t, az erőforrások eléréséhez (hozzáférési jogkivonat). A JWT jogkivonat különböző jogcímek rendelkezik.
*   Az Azure Media Services REST API erőforrás vagy a célközönség: https://rest.media.azure.net.
*   Ügyfél-azonosító: Lásd a 2. lépés a [lépéseket a telepítés](#steps-for-setup).
*   Ügyfélkulcs: 2 lépés lásd: [lépéseket a telepítés](#steps-for-setup).
*   A Media Services REST API-végpont a következő formátum hello fiókja:

    https://[media_service_account_name].restv2. [data_center].media.azure.net/API 

    Ez a elleni mely összes Media Services REST API-t az alkalmazás hívások végrehajtott hello végpont. Például https://willzhanmswjapan.restv2.japanwest.media.azure.net/API.

Ezek a paraméterek öt majd be a web.config vagy az app.config fájlt, a kódban toouse.

## <a name="sample-code"></a>Mintakód

Mintakód hello található [az Azure AD-alapú hitelesítés az Azure Media Services Access: mindkét REST API-n keresztül](https://github.com/willzhan/WAMSRESTSoln).

hello példakód két részből áll:

*   A dll-fájl hordozhatóosztálytár-projektjének, amely rendelkezik az összes hello REST API kód az Azure AD-alapú hitelesítés és engedélyezés céljából. Adja meg a megfelelő REST API hívások toohello Media Services REST API-végpont, hello jogkivonatokkal metódust is tartalmaz.
*   Konzol teszt ügyfél, amely kezdeményezi az Azure AD-alapú hitelesítés, és különböző Media Services REST API-t hívja.

hello mintaprojektet a három lehetőséggel rendelkezik:

*   Az Azure AD hitelesítési keresztül hello ügyfél hitelesítő adatait adja meg csak hello REST API használatával.
*   Azure Media Services hozzáférés csak hello REST API használatával.
*   Az Azure Storage access használatával csak hello REST API-t (a használt toocreate Media Services-fiók, REST API használatával).


## <a name="where-is-hello-refresh-token"></a>Hol található hello frissítési jogkivonat?

Néhány olvasók kérheti: hello frissítési jogkivonat hol van? Miért nem használja a frissítési jogkivonat Itt?

hello egy frissítési jogkivonat célja nem toorefresh olyan hozzáférési jogkivonatot. Ehelyett tervezett toobypass végfelhasználói hitelesítési vagy felhasználói beavatkozás és egy érvényes jogkivonat továbbra is megjelenik, amikor egy korábbi jogkivonat lejár. A frissítési jogkivonat jobb nevét lehet, hogy valami like "kihagyása a felhasználó újra-sign-in jogkivonatát."

Hello OAuth 2.0 engedélyezési adja meg a folyamat (felhasználónévvel és jelszóval, egy felhasználó nevében eljáró) használatakor a frissítési jogkivonat segítséget nyújt a megújított access token beszerzése a kért felhasználói beavatkozás nélkül. Azonban hello OAuth 2.0 ügyfél hitelesítő adatai megadják a folyamat, amely ebben a cikkben azt ismertetik, hello ügyfél működik a saját nevében. Minden felhasználói beavatkozás nem szükséges, és hello engedélyezési server túl nem szükséges (és nem) adjon meg egy frissítési jogkivonat. Ha hello hibakeresése **GetUrlEncodedJWT** metódust, akkor figyelje meg, hogy hello válaszát hello token-végpont rendelkezik-e a hozzáférési tokent, de nincs frissítési jogkivonat.

## <a name="next-steps"></a>Következő lépések

Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-dotnet-upload-files.md).
