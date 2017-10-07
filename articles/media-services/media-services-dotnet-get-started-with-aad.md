---
title: "az Azure AD hitelesítési tooaccess Azure Media Services API a .NET aaaUse |} Microsoft Docs"
description: "Ez a témakör bemutatja, hogyan toouse Azure Active Directory (Azure AD) hitelesítési tooaccess Azure Media Services (AMS) API-t .NET."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 2f750e460d9e476ad92e96adeac6500cb692cd77
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-ad-authentication-tooaccess-azure-media-services-api-with-net"></a>Az Azure AD hitelesítési tooaccess Azure Media Services API használata a .NET

Windowsazure.mediaservices 4.0.0.4 verziótól kezdődően Azure Media Services Azure Active Directory (Azure AD) alapuló hitelesítést támogatja. Ez a témakör bemutatja, hogyan toouse az Azure AD hitelesítési tooaccess Azure Media Services API a Microsoft .NET.

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. További részletek: [Ingyenes Azure-próbafiók](https://azure.microsoft.com/pricing/free-trial/). 
- Egy Media Services-fiók. További információkért lásd: [hello Azure-portál használatával Azure Media Services-fiók létrehozása](media-services-portal-create-account.md).
- hello legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag.
- Hello témakör ismeretét [eléréséhez Azure Media Services API-t az AAD-hitelesítés áttekintése](media-services-use-aad-auth-to-access-ams-api.md). 

Ha az Azure AD-alapú hitelesítés az Azure Media Services használata esetén az alábbi két módszer egyikével hitelesítheti:

- **Felhasználó hitelesítése** hitelesíti a személy, aki az Azure Media Services erőforrások hello app toointeract használ. hello interaktív alkalmazás először kell hello felhasználók hitelesítő adatainak kéréséhez. Példa: a felügyeleti konzol alkalmazást, amely jogosult felhasználók toomonitor kódolási feladatok által használt vagy live streaming. 
- **Szolgáltatás egyszerű hitelesítési** szolgáltatás hitelesíti. Alkalmazásokat, amelyek általában arra használják ezt a hitelesítési módszert démon szolgáltatások, a középső rétegbeli szolgáltatások vagy az ütemezett feladatok, például webalkalmazások, függvény alkalmazások, a logic apps, API-k vagy mikroszolgáltatások futó alkalmazások.

>[!IMPORTANT]
>Az Azure Media Services jelenleg olyan Azure Access Control Service hitelesítési modellt. Azonban a hozzáférés-vezérlés engedélyezési érintetlen toobe 2018. június 1. az elavult. Azt javasoljuk, hogy az áttelepített tooan Azure Active Directory hitelesítési modell lehető legrövidebb időn belül.

## <a name="get-an-azure-ad-access-token"></a>Az Azure AD hozzáférési jogkivonat beszerzése

tooconnect toohello Azure Media Services API az Azure AD-alapú hitelesítés, az Azure AD hozzáférési tokent toorequest kell hello ügyfél alkalmazást. Hello Media Services .NET ügyfél SDK, hogyan az Azure AD hozzáférési tokent tooacquire burkolt, és a hello meg egyszerűsített hello adatait számos használatakor [AzureAdTokenProvider](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenProvider.cs) és [AzureAdTokenCredentials ](https://github.com/Azure/azure-sdk-for-media-services/blob/dev/src/net/Client/Common/Common.Authentication/AzureAdTokenCredentials.cs) osztályok. 

Például nincs szükség a tooprovide hello Azure AD-szolgáltatóként, a Media Services erőforrás URI vagy a natív Azure AD alkalmazás részletei. Ezek a jól ismert hello Azure AD hozzáférési jogkivonat-szolgáltató osztály által már beállított értékeket. 

Ha nem használ az Azure Media Service .NET SDK-t, azt javasoljuk, hogy használja-e hello [Azure AD Authentication Library](../active-directory/develop/active-directory-authentication-libraries.md). telepíteni kell, amely az Azure AD Authentication Library toouse hello paraméterek értékeit tooget lásd [hello Azure portál tooaccess az Azure AD hitelesítési beállítások](media-services-portal-get-started-with-aad.md).

Akkor is, hogy az alapértelmezett implementációja hello hello hello beállítás **AzureAdTokenProvider** a saját példánnyal.

## <a name="install-and-configure-azure-media-services-net-sdk"></a>Telepítse és konfigurálja az Azure Media Services .NET SDK

>[!NOTE] 
>a Media Services .NET SDK hello toouse az Azure AD hitelesítési, kell toohave hello legújabb [NuGet](https://www.nuget.org/packages/windowsazure.mediaservices) csomag. Továbbá adja hozzá egy hivatkozást toohello **Microsoft.IdentityModel.Clients.ActiveDirectory** szerelvény. Ha egy meglévő alkalmazást használ, a következők hello **Microsoft.WindowsAzure.MediaServices.Client.Common.Authentication.dll** szerelvény. 

1. Hozzon létre egy új C# konzolalkalmazást a Visual Studio.
2. Használjon hello [windowsazure.mediaservices](https://www.nuget.org/packages/windowsazure.mediaservices) NuGet csomag tooinstall **Azure Media Services .NET SDK**. 

    tooadd hivatkozások használatával NuGet, érvénybe hello a következő lépéseket: a **Megoldáskezelőben**, kattintson a jobb gombbal a hello projekt nevét, majd válassza ki **kezelése NuGet-csomagok**. Ezután keresse meg **windowsazure.mediaservices** válassza **telepítése**.
    
    – vagy –

    Futtatási hello parancs a következő **Csomagkezelő konzol** a Visual Studióban.

        Install-Package windowsazure.mediaservices -Version 4.0.0.4

3. Adja hozzá **használatával** tooyour forráskódot.

        using Microsoft.WindowsAzure.MediaServices.Client; 

## <a name="use-user-authentication"></a>Hitelesítés használatára

tooconnect toohello Azure Media Service API hello felhasználói hitelesítési lehetőséggel, hello ügyfélalkalmazás kell toorequest használatával az Azure AD-tokent hello a következő paramétereket:  

- Azure AD-bérlő végpont. hello bérlői adatait lekérhetők hello Azure-portálon. Hello bejelentkezett felhasználó hello jobb felső sarkában található mutasson.
- A Media Services erőforrás URI azonosítója.
- A Media Services (natív) alkalmazás ügyfél-azonosító. 
- Media Services (natív) alkalmazás átirányítási URI-címe. 

Ezek a paraméterek értékeit hello található **AzureEnvironments.AzureCloudEnvironment**. Hello **AzureEnvironments.AzureCloudEnvironment** állandó van a .NET SDK tooget hello segítő hello jobb környezetiváltozó-beállításainak egy nyilvános Azure-adatközpont. 

Előre megadott környezeti beállításokat tartalmaz hello nyilvános adatközpontokban csak a Media Services eléréséhez. A állami vagy kormányzati felhőalapú területeket, használhat **AzureChinaCloudEnvironment**, **AzureUsGovernmentEnvrionment**, vagy **AzureGermanCloudEnvironment** kulcsattribútumokkal.

a következő példakód hello létrehoz egy tokent:
    
    var tokenCredentials = new AzureAdTokenCredentials("microsoft.onmicrosoft.com", AzureEnvironments.AzureCloudEnvironment);
    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
  
toostart programozási Media Services ellen, toocreate kell egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány. Hello **CloudMediaContext** magában foglalja többek között a feladatok, eszközök, fájlok, hozzáférési házirendek és keresők hivatkozások tooimportant gyűjtemények. 

Szükség toopass hello **erőforrás URI azonosítója a Media Services-REST** toohello **CloudMediaContext** konstruktor. tooget hello erőforrás URI REST médiaszolgáltatások, jelentkezzen be Azure-portálon toohello, válassza ki az Azure Media Services-fiókhoz, válasszon **API-hozzáférés**, majd válassza ki **tooAzure Media Services felhasználói csatlakozás hitelesítési**. 

hello alábbi példakód létrehoz egy **CloudMediaContext** példány:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);

hello a következő példa bemutatja, hogyan toocreate hello Azure AD-jogkivonat és hello környezetben:

    namespace AADAuthSample
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Specify your Azure AD tenant domain, for example "microsoft.onmicrosoft.com".
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR AAD TENANT DOMAIN HERE}", AzureEnvironments.AzureCloudEnvironment);
    
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
            }
    
        }
    }

>[!NOTE]
>Ha egy kivételt, amely szerint "hello távoli kiszolgáló hibát adott vissza: (401) nem engedélyezett," hello lásd [hozzáférés-vezérlés](media-services-use-aad-auth-to-access-ams-api.md#access-control) Azure Media Services API elérése az Azure AD hitelesítési – Áttekintés szakasza.

## <a name="use-service-principal-authentication"></a>Szolgáltatás egyszerű hitelesítés használata
    
tooconnect toohello Azure Media Services API hello szolgáltatás egyszerű beállítás, a középső rétegbeli (webes API-t vagy webalkalmazás) kell toorequests az Azure AD-tokent a következő paraméterek hello:  

- Azure AD-bérlő végpont. hello bérlői adatait lekérhetők hello Azure-portálon. Hello bejelentkezett felhasználó hello jobb felső sarkában található mutasson.
- A Media Services erőforrás URI azonosítója.
- Az Azure AD alkalmazás értékek: hello **ügyfél-azonosító** és **ügyfélkulcs**.

hello értékeinek hello **ügyfél-azonosító** és **ügyfélkulcs** paraméterek hello Azure-portálon található. További információkért lásd: [Ismerkedés az Azure AD használatával hello Azure-portálon](media-services-portal-get-started-with-aad.md).

hello alábbi példakód létrehoz egy tokent hello segítségével **AzureAdTokenCredentials** konstruktort **AzureAdClientSymmetricKey** paramétert: 
    
    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                AzureEnvironments.AzureCloudEnvironment);

    var tokenProvider = new AzureAdTokenProvider(tokenCredentials);

Azt is megadhatja, hello **AzureAdTokenCredentials** konstruktort **AzureAdClientCertificate** paraméterként. 

Kapcsolatos útmutatásért toocreate és a tanúsítvány konfigurálása az Azure AD használatával, lásd: űrlapon [tooAzure AD a tanúsítványok – kézi konfigurálás lépéseit démon alkalmazások hitelesítése](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md).

    var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                new AzureAdClientCertificate("{YOUR CLIENT ID HERE}", "{YOUR CLIENT CERTIFICATE THUMBPRINT}"), 
                                AzureEnvironments.AzureCloudEnvironment);

toostart programozási Media Services ellen, toocreate kell egy **CloudMediaContext** hello kiszolgálói környezetbe jelölő példány. Szükség toopass hello **erőforrás URI azonosítója a Media Services-REST** toohello **CloudMediaContext** konstruktor. Hello kaphat **erőforrás URI azonosítója a Media Services-REST** hello Azure-portálon is közötti értéket.

hello alábbi példakód létrehoz egy **CloudMediaContext** példány:

    CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
hello a következő példa bemutatja, hogyan toocreate hello Azure AD-jogkivonat és hello környezetben:

    namespace AADAuthSample
    {
    
        class Program
        {
            static void Main(string[] args)
            {
                var tokenCredentials = new AzureAdTokenCredentials("{YOUR Azure AD TENANT DOMAIN HERE}", 
                                            new AzureAdClientSymmetricKey("{YOUR CLIENT ID HERE}", "{YOUR CLIENT SECRET}"), 
                                            AzureEnvironments.AzureCloudEnvironment);
            
                var tokenProvider = new AzureAdTokenProvider(tokenCredentials);
    
                // Specify your REST API endpoint, for example "https://accountname.restv2.westcentralus.media.azure.net/API".      
                CloudMediaContext context = new CloudMediaContext(new Uri("YOUR REST API ENDPOINT HERE"), tokenProvider);
    
                var assets = context.Assets;
                foreach (var a in assets)
                {
                    Console.WriteLine(a.Name);
                }
    
                Console.ReadLine();
            }
    
        }
    }

## <a name="next-steps"></a>Következő lépések

Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-dotnet-upload-files.md).
