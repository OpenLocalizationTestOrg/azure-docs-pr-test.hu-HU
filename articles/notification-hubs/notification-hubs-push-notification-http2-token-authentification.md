---
title: "az Azure Notification Hubs apns aaaToken-alapú (HTTP/2) hitelesítési |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan tooleverage hello apns új tokent használó hitelesítés"
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
ms.openlocfilehash: 3353d7f16033ce0b68edec9ee9aeb98f47faa1fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="token-based-http2-authentication-for-apns"></a><span data-ttu-id="0582f-103">Az APNS-jogkivonatalapú (HTTP/2) hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0582f-103">Token-based (HTTP/2) Authentication for APNS</span></span>
## <a name="overview"></a><span data-ttu-id="0582f-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0582f-104">Overview</span></span>
<span data-ttu-id="0582f-105">Ez a cikk részletesen, hogyan toouse hello új APNS HTTP/2-protokoll jogkivonat-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="0582f-105">This article details how toouse hello new APNS HTTP/2 protocol with token based authentication.</span></span>

<span data-ttu-id="0582f-106">hello fő előnyei a hello új protokoll a következők:</span><span class="sxs-lookup"><span data-stu-id="0582f-106">hello key benefits of using hello new protocol include:</span></span>
-   <span data-ttu-id="0582f-107">Jogkivonatok létrehozásához viszonylag teljesen szabad (összehasonlított toocertificates)</span><span class="sxs-lookup"><span data-stu-id="0582f-107">Token generation is relatively hassle free (compared toocertificates)</span></span>
-   <span data-ttu-id="0582f-108">Nincs több lejárati dátumokat – jogkörrel rendelkezik arra a hitelesítési tokenek és a visszavont tanúsítványok ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="0582f-108">No more expiry dates – you are in control of your authentication tokens and their revocation</span></span>
-   <span data-ttu-id="0582f-109">Másolatot too4 KB mostantól lehet hasznos adat található</span><span class="sxs-lookup"><span data-stu-id="0582f-109">Payloads can now be up too4 KB</span></span>
- <span data-ttu-id="0582f-110">Szinkron visszajelzés</span><span class="sxs-lookup"><span data-stu-id="0582f-110">Synchronous feedback</span></span>
-   <span data-ttu-id="0582f-111">A legújabb protokoll Apple – tanúsítványok továbbra is használhatják a hello bináris protokoll, amely meg van jelölve érvénytelenítése</span><span class="sxs-lookup"><span data-stu-id="0582f-111">You’re on Apple’s latest protocol – certificates still use hello binary protocol, which is marked for deprecation</span></span>

<span data-ttu-id="0582f-112">Ez új mechanizmus használatával is végrehajtható két lépésben néhány perc múlva:</span><span class="sxs-lookup"><span data-stu-id="0582f-112">Using this new mechanism can be done in two steps in a few minutes:</span></span>
1.  <span data-ttu-id="0582f-113">Hello szükséges adatok lekérését hello Apple Developer-fiók portáljára</span><span class="sxs-lookup"><span data-stu-id="0582f-113">Obtain hello necessary information from hello Apple Developer Account portal</span></span>
2.  <span data-ttu-id="0582f-114">Az értesítési központ konfigurálása hello új adatokkal</span><span class="sxs-lookup"><span data-stu-id="0582f-114">Configure your notification hub with hello new information</span></span>

<span data-ttu-id="0582f-115">A Notification Hubs mostantól minden set toouse hello új hitelesítési rendszere az apns-sel.</span><span class="sxs-lookup"><span data-stu-id="0582f-115">Notification Hubs is now all set toouse hello new authentication system with APNS.</span></span> 

<span data-ttu-id="0582f-116">Vegye figyelembe, hogy a beállítást, ha áttelepítette az APNS tanúsítvány hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="0582f-116">Note that if you migrated from using certificate credentials for APNS:</span></span>
- <span data-ttu-id="0582f-117">hello token tulajdonságok felülírni a tanúsítványt a rendszerben,</span><span class="sxs-lookup"><span data-stu-id="0582f-117">hello token properties overwrite your certificate in our system,</span></span>
- <span data-ttu-id="0582f-118">de az alkalmazás továbbra is tooreceive értesítések zökkenőmentesen.</span><span class="sxs-lookup"><span data-stu-id="0582f-118">but your application continues tooreceive notifications seamlessly.</span></span>

## <a name="obtaining-authentication-information-from-apple"></a><span data-ttu-id="0582f-119">Hitelesítési adatok beszerzése az Apple-től</span><span class="sxs-lookup"><span data-stu-id="0582f-119">Obtaining authentication information from Apple</span></span>
<span data-ttu-id="0582f-120">tooenable jogkivonat-alapú hitelesítés kell hello Apple Developer fiókjából következő tulajdonságai:</span><span class="sxs-lookup"><span data-stu-id="0582f-120">tooenable token-based authentication, you need hello following properties from your Apple Developer Account:</span></span>
### <a name="key-identifier"></a><span data-ttu-id="0582f-121">Kulcsazonosító</span><span class="sxs-lookup"><span data-stu-id="0582f-121">Key Identifier</span></span>
<span data-ttu-id="0582f-122">az Apple fejlesztői fiókban hello "Kulcsok" oldalról hello kulcsazonosító érhető el</span><span class="sxs-lookup"><span data-stu-id="0582f-122">hello key identifier can be obtained from hello "Keys" page in your Apple Developer Account</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/obtaining-auth-information-from-apple.png)

### <a name="application-identifier--application-name"></a><span data-ttu-id="0582f-123">Alkalmazásazonosító & alkalmazás nevét</span><span class="sxs-lookup"><span data-stu-id="0582f-123">Application Identifier & Application Name</span></span>
<span data-ttu-id="0582f-124">hello alkalmazásnév hello App IDs lapján hello fejlesztői fiókjába keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0582f-124">hello application name is available via hello App IDs page in hello Developer Account.</span></span> 
![](./media/notification-hubs-push-notification-http2-token-authentification/app-name.png)

<span data-ttu-id="0582f-125">hello alkalmazásazonosító hello tagsági részleteit megjelenítő oldalra hello fejlesztői fiókjába keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="0582f-125">hello application identifier is available via hello membership details page in hello Developer Account.</span></span>
![](./media/notification-hubs-push-notification-http2-token-authentification/app-id.png)


### <a name="authentication-token"></a><span data-ttu-id="0582f-126">Hitelesítési jogkivonata</span><span class="sxs-lookup"><span data-stu-id="0582f-126">Authentication token</span></span>
<span data-ttu-id="0582f-127">hello hitelesítésére szolgáló jogkivonat letölthető az alkalmazás jogkivonat létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="0582f-127">hello authentication token can be downloaded after you generate a token for your application.</span></span> <span data-ttu-id="0582f-128">Hogyan toogenerate Ez a token, a részletekért tekintse meg túl[az Apple fejlesztői dokumentációját](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span><span class="sxs-lookup"><span data-stu-id="0582f-128">For details on how toogenerate this token, refer too[Apple’s Developer documentation](http://help.apple.com/xcode/mac/current/#/dev11b059073?sub=dev1eb5dfe65).</span></span>

## <a name="configuring-your-notification-hub-toouse-token-based-authentication"></a><span data-ttu-id="0582f-129">A notification hub toouse-jogkivonat-alapú hitelesítés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0582f-129">Configuring your notification hub toouse token-based authentication</span></span>
### <a name="configure-via-hello-azure-portal"></a><span data-ttu-id="0582f-130">Hello Azure-portálon keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0582f-130">Configure via hello Azure portal</span></span>
<span data-ttu-id="0582f-131">tooenable jogkivonat-alapú hitelesítés hello portál, Azure-portálon toohello bejelentkezés, és folytassa a tooyour értesítési központ > értesítési szolgáltatások > APNS panel.</span><span class="sxs-lookup"><span data-stu-id="0582f-131">tooenable token based authentication in hello portal, log in toohello Azure portal and go tooyour Notification Hub > Notification Services > APNS panel.</span></span> 

<span data-ttu-id="0582f-132">Új tulajdonság – *hitelesítési mód*.</span><span class="sxs-lookup"><span data-stu-id="0582f-132">There is a new property – *Authentication Mode*.</span></span> <span data-ttu-id="0582f-133">Kiválasztása a Token lehetővé teszi a tooupdate a központ az összes hello vonatkozó token tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="0582f-133">Selecting Token allows you tooupdate your hub with all hello relevant token properties.</span></span>

![](./media/notification-hubs-push-notification-http2-token-authentification/azure-portal-apns-settings.png)

- <span data-ttu-id="0582f-134">Adja meg az Apple developer-fiók lekért hello tulajdonságait</span><span class="sxs-lookup"><span data-stu-id="0582f-134">Enter hello properties you retrieved from your Apple developer account,</span></span> 
- <span data-ttu-id="0582f-135">Válassza ki az alkalmazás módot (éles vagy védőfal)</span><span class="sxs-lookup"><span data-stu-id="0582f-135">choose your application mode (Production or Sandbox)</span></span> 
- <span data-ttu-id="0582f-136">Kattintson a Mentés tooupdate APN szolgáltatás hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="0582f-136">click Save tooupdate your APNS credentials.</span></span> 

### <a name="configure-via-management-api-rest"></a><span data-ttu-id="0582f-137">(REST) API Management szolgáltatáson keresztül konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0582f-137">Configure via Management API (REST)</span></span>

<span data-ttu-id="0582f-138">Használhatja a [felügyeleti API-k](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate a notification hub toouse-jogkivonat-alapú hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="0582f-138">You can use our [management APIs](https://msdn.microsoft.com/library/azure/dn495827.aspx) tooupdate your notification hub toouse token-based authentication.</span></span>
<span data-ttu-id="0582f-139">Attól függően, hogy konfigurálja a hello alkalmazás egy védőfal vagy éles alkalmazás (az Apple fejlesztői fiókban megadva) használja a megfelelő végpont hello közül:</span><span class="sxs-lookup"><span data-stu-id="0582f-139">Depending on whether hello application you’re configuring is a Sandbox or Production app (specified in your Apple Developer Account), use one of hello corresponding endpoints:</span></span>

- <span data-ttu-id="0582f-140">A védőfal végpont: [https://api.development.push.apple.com:443/3/eszköz](https://api.development.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="0582f-140">Sandbox Endpoint: [https://api.development.push.apple.com:443/3/device](https://api.development.push.apple.com:443/3/device)</span></span>
- <span data-ttu-id="0582f-141">Éles végpont: [https://api.push.apple.com:443/3/eszköz](https://api.push.apple.com:443/3/device)</span><span class="sxs-lookup"><span data-stu-id="0582f-141">Production Endpoint: [https://api.push.apple.com:443/3/device](https://api.push.apple.com:443/3/device)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0582f-142">Jogkivonat-alapú hitelesítéshez, az API-verziót: **2017-04 vagy újabb**.</span><span class="sxs-lookup"><span data-stu-id="0582f-142">Token-based authentication requires an API version of: **2017-04 or later**.</span></span>
> 
> 

<span data-ttu-id="0582f-143">Íme egy példa a PUT kérés tooupdate jogkivonat-alapú hitelesítéssel hub:</span><span class="sxs-lookup"><span data-stu-id="0582f-143">Here’s an example of a PUT request tooupdate a hub with token-based authentication:</span></span>


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
        

### <a name="configure-via-hello-net-sdk"></a><span data-ttu-id="0582f-144">Hello .NET SDK használatával konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0582f-144">Configure via hello .NET SDK</span></span>
<span data-ttu-id="0582f-145">A központ toouse jogkivonat-alapú hitelesítés használatával konfigurálhatja a [legújabb ügyfél SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span><span class="sxs-lookup"><span data-stu-id="0582f-145">You can configure your hub toouse token based authentication using our [latest client SDK](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/1.0.8).</span></span> 

<span data-ttu-id="0582f-146">Íme egy hello helyes használatát bemutató példát:</span><span class="sxs-lookup"><span data-stu-id="0582f-146">Here’s a code sample illustrating hello correct usage:</span></span>


        NamespaceManager nm = NamespaceManager.CreateFromConnectionString(_endpoint);
        string token = "YOUR TOKEN HERE";
        string keyId = "YOUR KEY ID HERE";
        string appName = "YOUR APP NAME HERE";
        string appId = "YOUR APP ID HERE";
        NotificationHubDescription desc = new NotificationHubDescription("PATH tooYOUR HUB");
        desc.ApnsCredential = new ApnsCredential(token, keyId, appId, appName);
        desc.ApnsCredential.Endpoint = @"https://api.development.push.apple.com:443/3/device";
        nm.UpdateNotificationHubAsync(desc);

## <a name="reverting-toousing-certificate-based-authentication"></a><span data-ttu-id="0582f-147">Visszaállítási toousing Tanúsítványalapú hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0582f-147">Reverting toousing certificate-based authentication</span></span>
<span data-ttu-id="0582f-148">Visszaállíthatja bármely idő toousing tanúsítványalapú hitelesítési helyett hello token tulajdonságok bármelyik előző metódus és sikeres hello tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="0582f-148">You can revert at any time toousing certificate-based authentication by using any preceding method and passing hello certificate instead of hello token properties.</span></span> <span data-ttu-id="0582f-149">Korábban, hogy a művelet felülírja a hello tárolt hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="0582f-149">That action overwrites hello previously stored credentials.</span></span>
