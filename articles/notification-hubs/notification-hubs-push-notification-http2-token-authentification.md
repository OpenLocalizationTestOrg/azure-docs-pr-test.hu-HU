---
title: "Jogkivonat-alapú (HTTP/2) hitelesítési apns az Azure Notification hubs használatával |} Microsoft Docs"
description: "Ez a témakör azt ismerteti, hogyan használhatók ki az új jogkivonat hitelesítési apns"
services: notification-hubs
documentationcenter: .net
author: kpiteira
manager: erikre
editor: 
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 05/17/2017
ms.author: kapiteir
ms.openlocfilehash: 5a21bcd9f12fc3f96b17a556ba15526c35ababe2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="token-based-http2-authentication-for-apns"></a><span data-ttu-id="48dbb-103">Az APNS-jogkivonatalapú (HTTP/2) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="48dbb-103">Token-based (HTTP/2) Authentication for APNS</span></span>
## <a name="overview"></a><span data-ttu-id="48dbb-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="48dbb-104">Overview</span></span>
<span data-ttu-id="48dbb-105">Ez a cikk részletezi az új APNS HTTP/2 protokoll használata hitelesítési token módszer.</span><span class="sxs-lookup"><span data-stu-id="48dbb-105">This article details how to use the new APNS HTTP/2 protocol with token based authentication.</span></span>

<span data-ttu-id="48dbb-106">Az új protokollal főbb előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="48dbb-106">The key benefits of using the new protocol include:</span></span>
-   <span data-ttu-id="48dbb-107">Jogkivonatok létrehozásához viszonylag teljesen szabad (tanúsítványok képest)</span><span class="sxs-lookup"><span data-stu-id="48dbb-107">Token generation is relatively hassle free (compared to certificates)</span></span>
-   <span data-ttu-id="48dbb-108">Nincs több lejárati dátumokat – jogkörrel rendelkezik arra a hitelesítési tokenek és a visszavont tanúsítványok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="48dbb-108">No more expiry dates – you are in control of your authentication tokens and their revocation</span></span>
-   <span data-ttu-id="48dbb-109">Legfeljebb 4 KB-os mostantól lehet hasznos adat található</span><span class="sxs-lookup"><span data-stu-id="48dbb-109">Payloads can now be up to 4 KB</span></span>
- <span data-ttu-id="48dbb-110">Szinkron visszajelzés</span><span class="sxs-lookup"><span data-stu-id="48dbb-110">Synchronous feedback</span></span>
-   <span data-ttu-id="48dbb-111">A legújabb protokoll Apple – tanúsítványok továbbra is használhatják a bináris protokoll, amely meg van jelölve érvénytelenítése</span><span class="sxs-lookup"><span data-stu-id="48dbb-111">You’re on Apple’s latest protocol – certificates still use the binary protocol, which is marked for deprecation</span></span>

<span data-ttu-id="48dbb-112">Ez új mechanizmus használatával is végrehajtható két lépésben néhány perc múlva:</span><span class="sxs-lookup"><span data-stu-id="48dbb-112">Using this new mechanism can be done in two steps in a few minutes:</span></span>
1.  <span data-ttu-id="48dbb-113">A szükséges adatok lekérését az Apple Developer-fiók portálon</span><span class="sxs-lookup"><span data-stu-id="48dbb-113">Obtain the necessary information from the Apple Developer Account portal</span></span>
2.  <span data-ttu-id="48dbb-114">Az új információkkal az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48dbb-114">Configure your notification hub with the new information</span></span>

<span data-ttu-id="48dbb-115">A Notification Hubs mostantól az új hitelesítési rendszere az apns-sel használni minden beállítás.</span><span class="sxs-lookup"><span data-stu-id="48dbb-115">Notification Hubs is now all set to use the new authentication system with APNS.</span></span> 

<span data-ttu-id="48dbb-116">Vegye figyelembe, hogy a beállítást, ha áttelepítette az APNS tanúsítvány hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="48dbb-116">Note that if you migrated from using certificate credentials for APNS:</span></span>
- <span data-ttu-id="48dbb-117">a token tulajdonságok felülírni a tanúsítványt a rendszerben,</span><span class="sxs-lookup"><span data-stu-id="48dbb-117">the token properties overwrite your certificate in our system,</span></span>
- <span data-ttu-id="48dbb-118">de az alkalmazás továbbra is problémamentesen értesítéseket.</span><span class="sxs-lookup"><span data-stu-id="48dbb-118">but your application continues to receive notifications seamlessly.</span></span>

## <a name="obtaining-authentication-information-from-apple"></a><span data-ttu-id="48dbb-119">Hitelesítési adatok beszerzése az Apple-től</span><span class="sxs-lookup"><span data-stu-id="48dbb-119">Obtaining authentication information from Apple</span></span>
<span data-ttu-id="48dbb-120">Jogkivonat-alapú hitelesítés engedélyezéséhez a következő tulajdonságok Apple Developer fiókjából kell:</span><span class="sxs-lookup"><span data-stu-id="48dbb-120">To enable token-based authentication, you need the following properties from your Apple Developer Account:</span></span>
### <a name="key-identifier"></a><span data-ttu-id="48dbb-121">Kulcsazonosító</span><span class="sxs-lookup"><span data-stu-id="48dbb-121">Key Identifier</span></span>
<span data-ttu-id="48dbb-122">A kulcsazonosító érhető el az Apple Developer-fiók "Kulcsok" oldaláról</span><span class="sxs-lookup"><span data-stu-id="48dbb-122">The key identifier can be obtained from the "Keys" page in your Apple Developer Account</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a><span data-ttu-id="48dbb-123">Alkalmazásazonosító & alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="48dbb-123">Application Identifier & Application Name</span></span>
<span data-ttu-id="48dbb-124">Az alkalmazás nevét a fejlesztői fiók App IDs oldalon keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="48dbb-124">The application name is available via the App IDs page in the Developer Account.</span></span> 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

<span data-ttu-id="48dbb-125">Az alkalmazásazonosító a tagság részleteit megjelenítő oldalon a fejlesztői fiókban keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="48dbb-125">The application identifier is available via the membership details page in the Developer Account.</span></span>
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a><span data-ttu-id="48dbb-126">Hitelesítési jogkivonata</span><span class="sxs-lookup"><span data-stu-id="48dbb-126">Authentication token</span></span>
<span data-ttu-id="48dbb-127">A hitelesítési jogkivonat letölthető az alkalmazás jogkivonat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="48dbb-127">The authentication token can be downloaded after you generate a token for your application.</span></span> <span data-ttu-id="48dbb-128">Hogyan hozza létre a jogkivonatot a részletekért tekintse meg a [az Apple fejlesztői dokumentációját](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span><span class="sxs-lookup"><span data-stu-id="48dbb-128">For details on how to generate this token, refer to [Apple’s Developer documentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span></span>

## <a name="configuring-your-notification-hub-to-use-token-based-authentication"></a><span data-ttu-id="48dbb-129">Jogkivonat-alapú hitelesítés használata az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48dbb-129">Configuring your notification hub to use token-based authentication</span></span>
### <a name="configure-via-the-azure-portal"></a><span data-ttu-id="48dbb-130">Az Azure-portál konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48dbb-130">Configure via the Azure portal</span></span>
<span data-ttu-id="48dbb-131">Ahhoz, hogy a jogkivonat alapú hitelesítés a portálon, jelentkezzen be az Azure-portálon, majd nyissa meg az értesítési központba > értesítési szolgáltatások > APNS panel.</span><span class="sxs-lookup"><span data-stu-id="48dbb-131">To enable token based authentication in the portal, log in to the Azure portal and go to your Notification Hub > Notification Services > APNS panel.</span></span> 

<span data-ttu-id="48dbb-132">Új tulajdonság – *hitelesítési mód*.</span><span class="sxs-lookup"><span data-stu-id="48dbb-132">There is a new property – *Authentication Mode*.</span></span> <span data-ttu-id="48dbb-133">Token kijelölésével a központ frissítése az összes releváns token tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="48dbb-133">Selecting Token allows you to update your hub with all the relevant token properties.</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- <span data-ttu-id="48dbb-134">Adja meg az Apple fejlesztői fiókját lekért tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="48dbb-134">Enter the properties you retrieved from your Apple developer account,</span></span> 
- <span data-ttu-id="48dbb-135">Válassza ki az alkalmazás módot (éles vagy védőfal)</span><span class="sxs-lookup"><span data-stu-id="48dbb-135">choose your application mode (Production or Sandbox)</span></span> 
- <span data-ttu-id="48dbb-136">Kattintson a Mentés az APN szolgáltatás hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="48dbb-136">click Save to update your APNS credentials.</span></span> 

### <a name="configure-via-management-api-rest"></a><span data-ttu-id="48dbb-137">(REST) API Management szolgáltatáson keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="48dbb-137">Configure via Management API (REST)</span></span>

<span data-ttu-id="48dbb-138">Használhatja a [felügyeleti API-k](https://msdn.microsoft.com/library/azure/dn495827.aspx) jogkivonat-alapú hitelesítés használata az értesítési központ frissítése.</span><span class="sxs-lookup"><span data-stu-id="48dbb-138">You can use our [management APIs](https://msdn.microsoft.com/library/azure/dn495827.aspx) to update your notification hub to use token-based authentication.</span></span>
<span data-ttu-id="48dbb-139">Attól függően, hogy az alkalmazás, konfigurálja a védőfal vagy üzemi alkalmazások (Apple Developer fiókjához megadott) használja a megfelelő végpont közül:</span><span class="sxs-lookup"><span data-stu-id="48dbb-139">Depending on whether the application you’re configuring is a Sandbox or Production app (specified in your Apple Developer Account), use one of the corresponding endpoints:</span></span>

- <span data-ttu-id="48dbb-140">A védőfal végpont: [https://api.development.push.apple.com:443/3/eszköz](https://api.development.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="48dbb-140">Sandbox Endpoint: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)</span></span>
- <span data-ttu-id="48dbb-141">Éles végpont: [https://api.push.apple.com:443/3/eszköz](https://api.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="48dbb-141">Production Endpoint: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="48dbb-142">Jogkivonat-alapú hitelesítéshez, az API-verziót: **2017-04 vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="48dbb-142">Token-based authentication requires an API version of: **2017-04 or later**.</span></span>
> 
> 

<span data-ttu-id="48dbb-143">Íme egy példa a központ frissítése a jogkivonat-alapú hitelesítéssel PUT kérelmet:</span><span class="sxs-lookup"><span data-stu-id="48dbb-143">Here’s an example of a PUT request to update a hub with token-based authentication:</span></span>


        PUT https://{namespace}.servicebus.windows.net/{Notification Hub}?api-version=2017-04
          "Properties": {
            "ApnsCredential": {
              "Properties": {
                "KeyId": "<Your Key Id>",
                "Token": "<Your Authentication Token>",
                "AppName": "<Your Application Name>",
                "AppId": "<Your Application Id>",
                "Endpoint":"<Sandbox/Production Endpoint>"
              }
            }
          }
        

### <a name="configure-via-the-net-sdk"></a><span data-ttu-id="48dbb-144">Konfigurálja a .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="48dbb-144">Configure via the .NET SDK</span></span>
<span data-ttu-id="48dbb-145">A központ használandó jogkivonat-alapú hitelesítés használatával konfigurálhatja a [legújabb ügyfél SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span><span class="sxs-lookup"><span data-stu-id="48dbb-145">You can configure your hub to use token based authentication using our [latest client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span></span> 

<span data-ttu-id="48dbb-146">Íme egy helyes használatát bemutató példát:</span><span class="sxs-lookup"><span data-stu-id="48dbb-146">Here’s a code sample illustrating the correct usage:</span></span>


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH TO YOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-to-using-certificate-based-authentication"></a><span data-ttu-id="48dbb-147">Visszatérés a Tanúsítványalapú hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="48dbb-147">Reverting to using certificate-based authentication</span></span>
<span data-ttu-id="48dbb-148">Bármikor visszatérhet előző módszerrel, és átadja a tanúsítványt a jogkivonat tulajdonságok helyett a Tanúsítványalapú hitelesítés használatával.</span><span class="sxs-lookup"><span data-stu-id="48dbb-148">You can revert at any time to using certificate-based authentication by using any preceding method and passing the certificate instead of the token properties.</span></span> <span data-ttu-id="48dbb-149">A művelet felülírja a korábban tárolt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="48dbb-149">That action overwrites the previously stored credentials.</span></span>
