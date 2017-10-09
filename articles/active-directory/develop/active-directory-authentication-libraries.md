---
title: "Active Directory hitelesítési Kódtárai aaaAzure |} Microsoft Docs"
description: "hello Azure AD Authentication Library (ADAL) lehetővé teszi, hogy ügyfél alkalmazásfejlesztők tooeasily felhasználók toocloud hitelesítéséhez vagy a helyszíni Active Directory (AD), és majd a védelmét biztosító API-hívások hozzáférési tokenek beszerzése érdekében."
services: active-directory
documentationcenter: 
author: bryanla
manager: mbaldwin
editor: mbaldwin
ms.assetid: 2e4fc79a-0285-40be-8c77-65edee408a22
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/02/2017
ms.author: bryanla
ms.custom: aaddev
ms.openlocfilehash: 20fae18807ef03463ab1bc218e5f3548b5bd5717
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-authentication-libraries"></a>Az Azure Active Directory hitelesítési Kódtárai
hello Azure Active Directory Authentication Library (ADAL) lehetővé teszi, hogy ügyfél alkalmazás fejlesztők tooeasily felhasználók toocloud hitelesítéséhez a helyszíni Active Directory (AD), illetve az API-hívásokban biztonságához hozzáférési tokenek beszerzése érdekében. ADAL hitelesítési egyszerűbbé teszi a fejlesztők funkciók révén, mint:
 - aszinkron metódushívások támogatása
 - a konfigurálható jogkivonatok gyorsítótárát, hogy a tárolók hozzáférési jogkivonatok és frissítési jogkivonatok
 - automatikus token frissítés, ha az előző lejár, és egy frissítési jogkivonat érhető el
 - és további
 
Legtöbb hello összetettségi kezelnek, ADAL segít az üzleti logika fejlesztők helyezi a hangsúlyt, és könnyen biztonságos erőforrások, anélkül, hogy a biztonsági szakértő.

Adal-t a különböző platformok érhető el.

### <a name="client-libraries"></a>Ügyfélkönyvtárak

| Platform | Részletes ismertetés | Letöltés | Forráskód | Minta | Referencia
| --- | --- | --- | --- | --- | --- |
| .NET ügyfél, a Windows Store UWP, Xamarin iOS és Android rendszerhez |A .NET-hez (előzetes verzió) MSAL |[NuGet](https://www.nuget.org/packages/Microsoft.Identity.Client/1.1.0-preview) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-dotnet) | [Egy asztali alkalmazás](~/articles/active-directory/develop/guidedsetups/active-directory-windesktop.md) |[Referencia](https://docs.microsoft.com/dotnet/api/?view=identityclient-1.1.0-preview) | 
| JavaScript |MSAL a JavaScript (előzetes verzió) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-js) | [Egyetlen alkalmazás](~/articles/active-directory/develop/GuidedSetups/active-directory-javascriptspa.md) | [Referencia](https://htmlpreview.github.io/?https://raw.githubusercontent.com/AzureAD/microsoft-authentication-library-for-js/blob/dev/docs/classes/_useragentapplication_.msal.useragentapplication.html) | 
| iOS |Az iOS számára (előzetes verzió) MSAL |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-objc) | [iOS-alkalmazás](~/articles/active-directory/develop/GuidedSetups/active-directory-ios.md) | [Referencia](https://azuread.github.io/docs/objc/) |
| Android |MSAL Android (előzetes verzió) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) |[GitHub](https://github.com/AzureAD/microsoft-authentication-library-for-android) | [Android-alkalmazás](~/articles/active-directory/develop/GuidedSetups/active-directory-android.md) | [Referencia](http://javadoc.io/doc/com.microsoft.identity.client/msal/0.1.1) |
| .NET ügyfél, a Windows Store UWP, Xamarin iOS és Android rendszerhez |ADAL .NET v3 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet) | [Egy asztali alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-dotnet) |[Referencia](https://docs.microsoft.com/dotnet/api/?view=identitymodelclientsad-3.13.9) | 
| .NET ügyfél, a Windows áruház, Windows Phone 8.1 |ADAL .NET v2 |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.4) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.4) | [Egy asztali alkalmazás](https://github.com/AzureADQuickStarts/NativeClient-DotNet/releases/tag/v2.X) | | 
| JavaScript |ADAL.js |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-js) |[Egyetlen alkalmazás](https://github.com/Azure-Samples/active-directory-javascript-singlepageapp-dotnet-webapi) | |
| iOS, macOS |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc/releases) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-objc) |[iOS-alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-ios) | [Referencia](https://cocoapods.org/pods/ADAL)|
| Android |ADAL |[hello központi tárházban](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-android) |[Android-alkalmazás](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-android) | [JavaDocs](http://javadoc.io/doc/com.microsoft.aad/adal/)|
| Node.js |ADAL |[npm](https://www.npmjs.com/package/adal-node) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs) | | |
| Java |ADAL4J |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-java) |[Java webalkalmazások](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-java) | |
| Python |ADAL |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) |[GitHub](https://github.com/AzureAD/azure-activedirectory-library-for-python) | | |
### <a name="server-libraries"></a>Server könyvtárak 

| Platform | Részletes ismertetés | Letöltés | Forráskód | Minta | Referencia
| --- | --- | --- | --- | --- | --- |
| .NET |OWIN az AzureAD|[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/) |[Codeplex webhelyen](http://katanaproject.codeplex.com) |[MVC-alkalmazáshoz](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapp-dotnet) | |
| .NET |A OpenIDConnect OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect) |[Codeplex webhelyen](http://katanaproject.codeplex.com) |[Webalkalmazás](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet) | |
| Node.js |Az Azure AD Passport |[npm](https://www.npmjs.com/package/passport-azure-ad) |[GitHub](https://github.com/AzureAD/passport-azure-ad) | [Webes API](https://docs.microsoft.com/azure/active-directory/active-directory-devquickstarts-webapi-nodejs)| |
| .NET |A WS-Federation OWIN |[NuGet](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation) |[Codeplex webhelyen](http://katanaproject.codeplex.com) |[MVC webalkalmazás](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet) | |
| .NET |A .NET 4.5 identitás protokoll bővítményei |[NuGet](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |
| .NET |A .NET 4.5 JWT-kezelő |[NuGet](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt) |[GitHub](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet) | | |



## <a name="scenarios"></a>Forgatókönyvek

Az alábbiakban a három gyakori forgatókönyvek, amelyben az adal-t távoli erőforráshoz hozzáférő ügyfél hitelesítésére használható:  

### <a name="authenticating-users-of-a-native-client-application-running-on-a-device"></a>Az eszközön futó natív ügyfélalkalmazás a felhasználók hitelesítéséhez 

Ebben a forgatókönyvben egy fejlesztő WPF ügyfélalkalmazás, az Azure AD, például a webes API-k által biztosított távoli erőforráshoz tooaccess igénylő rendelkezik. Ezután Azure-előfizetéssel rendelkezik, tudja, hogyan tooinvoke hello alsóbb rétegbeli webes API-t, és ismeri hello Azure AD-bérlő hello webes API-t használ. Ennek eredményeképpen használhat ADAL toofacilitate-hitelesítés az Azure ad-vel, teljesen delegálása hello hitelesítési élmény tooADAL vagy explicit módon a felhasználó hitelesítő adatainak kezelése. ADAL teszi, hogy könnyen tooauthenticate hello felhasználó, egy hozzáférési jogkivonatot, és a frissítési jogkivonat beszerzése az Azure AD, és aztán hello access token toomake kérelmek toohello webes API.

Azt mutatja be ezt a forgatókönyvet hitelesítési tooAzure AD kódminta, lásd: [natív ügyfélalkalmazás WPF tooWeb API](https://github.com/azureadsamples/nativeclient-dotnet).

### <a name="authenticating-a-confidential-client-application-running-on-a-web-server"></a>A webkiszolgálón futó bizalmas ügyfélalkalmazás hitelesítése

Ebben a forgatókönyvben egy fejlesztő rendelkezik egy Azure AD, például egy webes API által biztosított távoli erőforráshoz, amelyet a tooaccess kiszolgálón futó alkalmazást. Ezután a Azure-előfizetéssel rendelkezik, tudja, hogyan tooinvoke hello alárendelt szolgáltatás, és ismeri a hello Azure AD bérlő hello webes API-t használ. Ennek eredményeképpen használhat ADAL toofacilitate-hitelesítés az Azure ad-val explicit módon kezelnek hello alkalmazás hitelesítő adatait. ADAL teszi könnyen tooretrieve Azure ad-jogkivonat hello alkalmazás ügyfél-hitelesítő adatok használatával, majd a token toomake kérelmek toohello web API. ADAL is hello hello élettartama kezelése leírók hozzáférési tokent, gyorsítótárazás, és szükség szerint megújítása azt. Azt mutatja be, ebben a forgatókönyvben kódminta, lásd: [démon console Application tooWeb API](https://github.com/AzureADSamples/Daemon-DotNet).

### <a name="authenticating-a-confidential-client-application-running-on-a-server-on-behalf-of-a-user"></a>Egy felhasználó nevében a kiszolgálón futó bizalmas ügyfélalkalmazás hitelesítése 

Ebben a forgatókönyvben egy fejlesztő rendelkezik egy Azure AD, például egy webes API által biztosított távoli erőforráshoz, amelyet a tooaccess kiszolgálón futó alkalmazást. hello kérelem kell egy Azure AD-felhasználó nevében végrehajtott toobe. Ezután a Azure-előfizetéssel rendelkezik, tudja, hogyan tooinvoke hello alsóbb rétegbeli webes API-t, és ismeri a hello Azure AD bérlő hello szolgáltatás használja. Ha hello felhasználó hitelesített toohello webalkalmazás, az hello alkalmazás érheti el az Azure AD egy hello felhasználó hitelesítési kódját. hello webalkalmazás használható ADAL tooobtain egy hozzáférési és frissítési jogkivonatot egy hello engedélyezési kódot és az ügyfél hitelesítő adatokkal az Azure AD hello alkalmazáshoz rendelt felhasználó nevében. Miután hello webalkalmazás hello access token birtokában van, akkor meghívhatja hello webes API, amíg hello jogkivonat lejár. Amikor hello-token érvényessége lejár, a hello webalkalmazás használhatja ADAL tooget egy új hozzáférési jogkivonat korábban fogadott hello frissítési jogkivonat használatával. Azt mutatja be, ebben a forgatókönyvben kódminta, lásd: [natív ügyfél tooWeb API tooWeb API](https://github.com/Azure-Samples/active-directory-dotnet-webapi-onbehalfof).

## <a name="see-also"></a>Lásd még:

- [hello Azure Active Directory fejlesztői útmutatója](active-directory-developers-guide.md)
- [Az Azure Active directory hitelesítési forgatókönyvei](active-directory-authentication-scenarios.md)
- [Az Azure Active Directory-Kódminták](active-directory-code-samples.md)
