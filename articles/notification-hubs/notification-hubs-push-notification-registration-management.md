---
title: "Felügyeleti aaaRegistration"
description: "Ez a témakör ismerteti, hogyan tooregister eszközök a rendelés tooreceive a notification hubs leküldéses értesítések."
services: notification-hubs
documentationcenter: .net
author: ysxu
manager: erikre
editor: 
ms.assetid: fd0ee230-132c-4143-b4f9-65cef7f463a1
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-multiple
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 76471a45c7a0da1614ceed82b73cdb3319979ff7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="registration-management"></a><span data-ttu-id="a8113-103">Regisztrációkezelés</span><span class="sxs-lookup"><span data-stu-id="a8113-103">Registration management</span></span>
## <a name="overview"></a><span data-ttu-id="a8113-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="a8113-104">Overview</span></span>
<span data-ttu-id="a8113-105">Ez a témakör ismerteti, hogyan tooregister eszközök a rendelés tooreceive a notification hubs leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="a8113-105">This topic explains how tooregister devices with notification hubs in order tooreceive push notifications.</span></span> <span data-ttu-id="a8113-106">hello a témakör ismerteti a regisztrációk magas szinten, majd vezet be az eszközök regisztrálása a hello két fő minták: hello eszközről regisztrálása közvetlenül toohello értesítési központot, és regisztrálnia kell az alkalmazás-háttéralkalmazás segítségével.</span><span class="sxs-lookup"><span data-stu-id="a8113-106">hello topic describes registrations at a high level, then introduces hello two main patterns for registering devices: registering from hello device directly toohello notification hub, and registering through an application backend.</span></span> 

## <a name="what-is-device-registration"></a><span data-ttu-id="a8113-107">Mi az az eszközök regisztrációja</span><span class="sxs-lookup"><span data-stu-id="a8113-107">What is device registration</span></span>
<span data-ttu-id="a8113-108">Eszközregisztráció az egy értesítési központot teszi lehetővé a **regisztrációs** vagy **telepítési**.</span><span class="sxs-lookup"><span data-stu-id="a8113-108">Device registration with a Notification Hub is accomplished using a **Registration** or **Installation**.</span></span>

#### <a name="registrations"></a><span data-ttu-id="a8113-109">Regisztrációk</span><span class="sxs-lookup"><span data-stu-id="a8113-109">Registrations</span></span>
<span data-ttu-id="a8113-110">A regisztrációs Platform Notification szolgáltatás (PNS) kezelni egy eszköz számára a címkék, és valószínűleg egy sablon hello társítja.</span><span class="sxs-lookup"><span data-stu-id="a8113-110">A registration associates hello Platform Notification Service (PNS) handle for a device with tags and possibly a template.</span></span> <span data-ttu-id="a8113-111">hello PNS-leírójának lehet a ChannelURI, az eszköz jogkivonatát, illetve a GCM regisztrációs azonosítót. Címkék nem használt tooroute értesítést toohello megfelelő készletének eszközt kezeli.</span><span class="sxs-lookup"><span data-stu-id="a8113-111">hello PNS handle could be a ChannelURI, device token, or GCM registration id. Tags are used tooroute notifications toohello correct set of device handles.</span></span> <span data-ttu-id="a8113-112">További információkért lásd: [Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="a8113-112">For more information, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span> <span data-ttu-id="a8113-113">Sablonok használt tooimplement regisztrációs átalakítási.</span><span class="sxs-lookup"><span data-stu-id="a8113-113">Templates are used tooimplement per-registration transformation.</span></span> <span data-ttu-id="a8113-114">További információkért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="a8113-114">For more information, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>

#### <a name="installations"></a><span data-ttu-id="a8113-115">Telepítés</span><span class="sxs-lookup"><span data-stu-id="a8113-115">Installations</span></span>
<span data-ttu-id="a8113-116">Egy bővített egy telepítés, amely tartalmazza a leküldéses összessége regisztrációs kapcsolódó tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="a8113-116">An Installation is an enhanced registration that includes a bag of push related properties.</span></span> <span data-ttu-id="a8113-117">Hello tooregistering legújabb és legjobb módszer az eszközök.</span><span class="sxs-lookup"><span data-stu-id="a8113-117">It is hello latest and best approach tooregistering your devices.</span></span> <span data-ttu-id="a8113-118">Azonban ez nem támogatja az ügyféloldali .NET SDK-val ([Notification Hub SDK a háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) még érvényes.</span><span class="sxs-lookup"><span data-stu-id="a8113-118">However, it is not supported by client side .NET SDK ([Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) as of yet.</span></span>  <span data-ttu-id="a8113-119">Ez azt jelenti, ha regisztrál eszközről hello ügyfél magát, akkor egy toouse hello [Notification hub REST API](https://msdn.microsoft.com/library/mt621153.aspx) toosupport telepítéseket közelítse.</span><span class="sxs-lookup"><span data-stu-id="a8113-119">This means if you are registering from hello client device itself, you would have toouse hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx) approach toosupport installations.</span></span> <span data-ttu-id="a8113-120">Ha háttérszolgáltatás használ, el tudja toouse [Notification Hub SDK a háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="a8113-120">If you are using a backend service, you should be able toouse [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="a8113-121">hello az alábbiakban néhány fontos előnnyel toousing telepítés:</span><span class="sxs-lookup"><span data-stu-id="a8113-121">hello following are some key advantages toousing installations:</span></span>

* <span data-ttu-id="a8113-122">Teljes idempotent létrehozásához vagy frissítéséhez egy telepítés.</span><span class="sxs-lookup"><span data-stu-id="a8113-122">Creating or updating an installation is fully idempotent.</span></span> <span data-ttu-id="a8113-123">Úgy is újraindítható anélkül, duplikált regisztrációjával kapcsolatos bármilyen probléma merül fel.</span><span class="sxs-lookup"><span data-stu-id="a8113-123">So you can retry it without any concerns about duplicate registrations.</span></span>
* <span data-ttu-id="a8113-124">hello telepítési modell könnyen toodo egyedi leküldéses értesítések - eszközre célzó révén.</span><span class="sxs-lookup"><span data-stu-id="a8113-124">hello installation model makes it easy toodo individual pushes - targeting specific device.</span></span> <span data-ttu-id="a8113-125">A rendszer címke **"$InstallationId: [végrehajtott]"** automatikusan fel lesz véve az egyes telepítési alapú regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="a8113-125">A system tag **"$InstallationId:[installationId]"** is automatically added with each installation based registration.</span></span> <span data-ttu-id="a8113-126">Így hívása egy küldési toothis címke tootarget egy adott eszköz anélkül, hogy toodo további kódolása.</span><span class="sxs-lookup"><span data-stu-id="a8113-126">So you can call a send toothis tag tootarget a specific device without having toodo any additional coding.</span></span>
* <span data-ttu-id="a8113-127">Telepítés használatával is lehetővé teszi, hogy Ön toodo részleges regisztrációs frissítéseket.</span><span class="sxs-lookup"><span data-stu-id="a8113-127">Using installations also enables you toodo partial registration updates.</span></span> <span data-ttu-id="a8113-128">hello részleges frissítés telepítésének van szükség a javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902).</span><span class="sxs-lookup"><span data-stu-id="a8113-128">hello partial update of an installation is requested with a PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902).</span></span> <span data-ttu-id="a8113-129">Ez akkor különösen hasznos, ha azt szeretné, hogy hello regisztrációs tooupdate címke van megadva.</span><span class="sxs-lookup"><span data-stu-id="a8113-129">This is particularly useful when you want tooupdate tags on hello registration.</span></span> <span data-ttu-id="a8113-130">Nem rendelkezik toopull hello teljes regisztrációs le, és küldje el újra az összes hello előző címke.</span><span class="sxs-lookup"><span data-stu-id="a8113-130">You don't have toopull down hello entire registration and then resend all hello previous tags again.</span></span>

<span data-ttu-id="a8113-131">Telepítés a következő tulajdonságok hello hello is tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="a8113-131">An installation can contain hello hello following properties.</span></span> <span data-ttu-id="a8113-132">Teljes listáját lásd a hello telepítési tulajdonságok, [létrehozása vagy egy telepítési felülírása REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) vagy [telepítési tulajdonságokat](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) hello számára.</span><span class="sxs-lookup"><span data-stu-id="a8113-132">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) or [Installation Properties](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) for hello .</span></span>

    // Example installation format tooshow some supported properties
    {
        installationId: "",
        expirationTime: "",
        tags: [],
        platform: "",
        pushChannel: "",
        ………
        templates: {
            "templateName1" : {
                body: "",
                tags: [] },
            "templateName2" : {
                body: "",
                // Headers are for Windows Store only
                headers: {
                    "X-WNS-Type": "wns/tile" }
                tags: [] }
        },
        secondaryTiles: {
            "tileId1": {
                pushChannel: "",
                tags: [],
                templates: {
                    "otherTemplate": {
                        bodyTemplate: "",
                        headers: {
                            ... }
                        tags: [] }
                }
            }
        }
    }



<span data-ttu-id="a8113-133">Regisztráció és a telepítések alapértelmezés szerint nem lejáró fontos toonote.</span><span class="sxs-lookup"><span data-stu-id="a8113-133">It is important toonote that registrations and installations by default no longer expire.</span></span>

<span data-ttu-id="a8113-134">Regisztráció és a telepítés tartalmaznia kell egy érvényes PNS-leírójának minden eszköz vagy csatorna.</span><span class="sxs-lookup"><span data-stu-id="a8113-134">Registrations and installations must contain a valid PNS handle for each device/channel.</span></span> <span data-ttu-id="a8113-135">PNS-leírók regisztrálásáért hello eszközön ügyfél alkalmazásban csak érhető el, mert egy minta tooregister közvetlenül hello ügyfélalkalmazás az adott eszközön.</span><span class="sxs-lookup"><span data-stu-id="a8113-135">Because PNS handles can only be obtained in a client app on hello device, one pattern is tooregister directly on that device with hello client app.</span></span> <span data-ttu-id="a8113-136">Hello más aktuális, a biztonsági szempontok és az üzleti logika vonatkozó tootags szükség lehet hello app háttér-toomanage eszközregisztrációját.</span><span class="sxs-lookup"><span data-stu-id="a8113-136">On hello other hand, security considerations and business logic related tootags might require you toomanage device registration in hello app back-end.</span></span> 

#### <a name="templates"></a><span data-ttu-id="a8113-137">Sablonok</span><span class="sxs-lookup"><span data-stu-id="a8113-137">Templates</span></span>
<span data-ttu-id="a8113-138">Ha azt szeretné, hogy toouse [sablonok](notification-hubs-templates-cross-platform-push-messages.md), hello eszköz telepítése is rendelkezik az összes sablon egy JSON-ban, hogy eszközhöz társított (lásd a fenti minta) formátumban.</span><span class="sxs-lookup"><span data-stu-id="a8113-138">If you want toouse [Templates](notification-hubs-templates-cross-platform-push-messages.md), hello device installation also hold all templates associated with that device in a JSON format (see sample above).</span></span> <span data-ttu-id="a8113-139">hello sablonnevek segítségével különféle sablonok hello cél ugyanarra az eszközre.</span><span class="sxs-lookup"><span data-stu-id="a8113-139">hello template names help target different templates for hello same device.</span></span>

<span data-ttu-id="a8113-140">Vegye figyelembe, hogy minden a sablonnevet leképezhető tooa sablon szervezet és címkék nem kötelező megadni.</span><span class="sxs-lookup"><span data-stu-id="a8113-140">Note that each template name maps tooa template body and an optional set of tags.</span></span> <span data-ttu-id="a8113-141">Továbbá minden egyes platformhoz lehet további sablontulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="a8113-141">Moreover, each platform can have additional template properties.</span></span> <span data-ttu-id="a8113-142">A Windows Állapottárolójához (WNS) és Windows Phone 8 (MPNS segítségével) egy további-fejléc készlettel hello sablon része lehet.</span><span class="sxs-lookup"><span data-stu-id="a8113-142">For Windows Store (using WNS) and Windows Phone 8 (using MPNS), an additional set of headers can be part of hello template.</span></span> <span data-ttu-id="a8113-143">APNs hello esetben beállíthatja egy lejárati tulajdonság tooeither egy állandónak vagy tooa sablon kifejezést.</span><span class="sxs-lookup"><span data-stu-id="a8113-143">In hello case of APNs, you can set an expiry property tooeither a constant or tooa template expression.</span></span> <span data-ttu-id="a8113-144">Teljes listáját lásd a hello telepítési tulajdonságok, [létrehozása vagy felülírni a telepítés többi](https://msdn.microsoft.com/library/azure/mt621153.aspx) témakör.</span><span class="sxs-lookup"><span data-stu-id="a8113-144">For a complete listing of hello installation properties see, [Create or Overwrite an Installation with REST](https://msdn.microsoft.com/library/azure/mt621153.aspx) topic.</span></span>

#### <a name="secondary-tiles-for-windows-store-apps"></a><span data-ttu-id="a8113-145">Windows Áruházbeli alkalmazások másodlagos Csempéket</span><span class="sxs-lookup"><span data-stu-id="a8113-145">Secondary Tiles for Windows Store Apps</span></span>
<span data-ttu-id="a8113-146">A Windows áruház ügyfélalkalmazások toosecondary csempék van küldő értesítések hello azonos toohello elsődleges elküldeni őket.</span><span class="sxs-lookup"><span data-stu-id="a8113-146">For Windows Store client applications, sending notifications toosecondary tiles is hello same as sending them toohello primary one.</span></span> <span data-ttu-id="a8113-147">Ez is támogatott telepítések esetén.</span><span class="sxs-lookup"><span data-stu-id="a8113-147">This is also supported in installations.</span></span> <span data-ttu-id="a8113-148">Ne feledje, hogy másodlagos csempék egy másik ChannelUri, mely hello SDK az ügyfélalkalmazás transzparens módon kezeli.</span><span class="sxs-lookup"><span data-stu-id="a8113-148">Note that secondary tiles have a different ChannelUri, which hello SDK on your client app handles transparently.</span></span>

<span data-ttu-id="a8113-149">hello SecondaryTiles szótár használ, amely használt toocreate hello SecondaryTiles objektum a Windows Áruházbeli alkalmazásban azonos TileId hello.</span><span class="sxs-lookup"><span data-stu-id="a8113-149">hello SecondaryTiles dictionary uses hello same TileId that is used toocreate hello SecondaryTiles object in your Windows Store app.</span></span>
<span data-ttu-id="a8113-150">A hello ChannelUri elsődleges, másodlagos csempék ChannelUris bármikor is módosítása.</span><span class="sxs-lookup"><span data-stu-id="a8113-150">As with hello primary ChannelUri, ChannelUris of secondary tiles can change at any moment.</span></span> <span data-ttu-id="a8113-151">Rendelés tookeep hello telepítés esetén a hello az értesítési központ frissítése, hello eszköz frissíteni kell azokat a hello hello másodlagos csempék aktuális ChannelUris.</span><span class="sxs-lookup"><span data-stu-id="a8113-151">In order tookeep hello installations in hello notification hub updated, hello device must refresh them with hello current ChannelUris of hello secondary tiles.</span></span>

## <a name="registration-management-from-hello-device"></a><span data-ttu-id="a8113-152">Regisztrációs felügyeleti hello eszközről</span><span class="sxs-lookup"><span data-stu-id="a8113-152">Registration management from hello device</span></span>
<span data-ttu-id="a8113-153">Eszközregisztráció ügyfél alkalmazásokból való kezelésekor hello háttér felelős csak az értesítések küldése.</span><span class="sxs-lookup"><span data-stu-id="a8113-153">When managing device registration from client apps, hello backend is only responsible for sending notifications.</span></span> <span data-ttu-id="a8113-154">Ügyfélalkalmazások PNS-leírók regisztrálásáért mentése toodate megőrzése, és regisztrálja a címkék.</span><span class="sxs-lookup"><span data-stu-id="a8113-154">Client apps keep PNS handles up toodate, and register tags.</span></span> <span data-ttu-id="a8113-155">a következő kép hello ebben a mintában mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a8113-155">hello following picture illustrates this pattern.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

<span data-ttu-id="a8113-156">először hello eszköz PNS kezelni hello kikeresi hello PNS, majd közvetlenül regisztrálja hello értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="a8113-156">hello device first retrieves hello PNS handle from hello PNS, then registers with hello notification hub directly.</span></span> <span data-ttu-id="a8113-157">Hello regisztráció befejezését követően hello háttéralkalmazás, hogy a regisztrációs célzó értesítés küldése.</span><span class="sxs-lookup"><span data-stu-id="a8113-157">After hello registration is successful, hello app backend can send a notification targeting that registration.</span></span> <span data-ttu-id="a8113-158">További információ toosend értesítések, lásd: [Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="a8113-158">For more information about how toosend notifications, see [Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>
<span data-ttu-id="a8113-159">Vegye figyelembe, hogy ebben az esetben használhatja a notification hubs hello eszköz csak figyelni jogok tooaccess.</span><span class="sxs-lookup"><span data-stu-id="a8113-159">Note that in this case, you will use only Listen rights tooaccess your notification hubs from hello device.</span></span> <span data-ttu-id="a8113-160">További információkért lásd: [biztonsági](notification-hubs-push-notification-security.md).</span><span class="sxs-lookup"><span data-stu-id="a8113-160">For more information, see [Security](notification-hubs-push-notification-security.md).</span></span>

<span data-ttu-id="a8113-161">Hello eszközről regisztrálása hello legegyszerűbb módja, de a hátrányokkal azt.</span><span class="sxs-lookup"><span data-stu-id="a8113-161">Registering from hello device is hello simplest method, but it has some drawbacks.</span></span>
<span data-ttu-id="a8113-162">hello első hátránya, hogy egy ügyfélalkalmazás csak képes frissíteni a címkék, aktív hello alkalmazás esetén.</span><span class="sxs-lookup"><span data-stu-id="a8113-162">hello first drawback is that a client app can only update its tags when hello app is active.</span></span> <span data-ttu-id="a8113-163">Például ha egy felhasználó két eszközöket, amelyeket regisztrálni címkék kapcsolódó toosport csapatok, amikor egy további (például Seahawks) címke hello első eszközt regisztrál, hello második eszköz nem kap hello Seahawks hello értesítések amíg hello hello alkalmazást második eszköz végrehajtása még egyszer.</span><span class="sxs-lookup"><span data-stu-id="a8113-163">For example, if a user has two devices that register tags related toosport teams, when hello first device registers for an additional tag (for example, Seahawks), hello second device will not receive hello notifications about hello Seahawks until hello app on hello second device is executed a second time.</span></span> <span data-ttu-id="a8113-164">Általában több eszköz által érintett címkék, amikor címkék kezelése hello háttérrendszerből beállítás kívánatos.</span><span class="sxs-lookup"><span data-stu-id="a8113-164">More generally, when tags are affected by multiple devices, managing tags from hello backend is a desirable option.</span></span>
<span data-ttu-id="a8113-165">hello második regisztrációs felügyeleti hello ügyfél alkalmazásból hátránya, hogy mivel alkalmazásokat is megtámadott, biztonságossá tétele hello regisztrációs toospecific címkék igényel különös figyelmet "címke szintű biztonság." hello szakaszban leírtak szerint</span><span class="sxs-lookup"><span data-stu-id="a8113-165">hello second drawback of registration management from hello client app is that, since apps can be hacked, securing hello registration toospecific tags requires extra care, as explained in hello section “Tag-level security.”</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a><span data-ttu-id="a8113-166">Példa kód tooregister egy eszközről egy telepítést használ egy értesítési központban</span><span class="sxs-lookup"><span data-stu-id="a8113-166">Example code tooregister with a notification hub from a device using an installation</span></span>
<span data-ttu-id="a8113-167">Jelenleg ez csak akkor támogatott hello segítségével [Notification hub REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8113-167">At this time, this is only supported using hello [Notification Hubs REST API](https://msdn.microsoft.com/library/mt621153.aspx).</span></span>

<span data-ttu-id="a8113-168">Is használhatja a hello javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) hello telepítés frissítésének.</span><span class="sxs-lookup"><span data-stu-id="a8113-168">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

    class DeviceInstallation
    {
        public string installationId { get; set; }
        public string platform { get; set; }
        public string pushChannel { get; set; }
        public string[] tags { get; set; }
    }

    private async Task<HttpStatusCode> CreateOrUpdateInstallationAsync(DeviceInstallation deviceInstallation,
         string hubName, string listenConnectionString)
    {
        if (deviceInstallation.installationId == null)
            return HttpStatusCode.BadRequest;

        // Parse connection string (https://msdn.microsoft.com/library/azure/dn495627.aspx)
        ConnectionStringUtility connectionSaSUtil = new ConnectionStringUtility(listenConnectionString);
        string hubResource = "installations/" + deviceInstallation.installationId + "?";
        string apiVersion = "api-version=2015-04";

        // Determine hello targetUri that we will sign
        string uri = connectionSaSUtil.Endpoint + hubName + "/" + hubResource + apiVersion;

        //=== Generate SaS Security Token for Authorization header ===
        // See, https://msdn.microsoft.com/library/azure/dn495627.aspx
        string SasToken = connectionSaSUtil.getSaSToken(uri, 60);

        using (var httpClient = new HttpClient())
        {
            string json = JsonConvert.SerializeObject(deviceInstallation);

            httpClient.DefaultRequestHeaders.Add("Authorization", SasToken);

            var response = await httpClient.PutAsync(uri, new StringContent(json, System.Text.Encoding.UTF8, "application/json"));
            return response.StatusCode;
        }
    }

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    string installationId = null;
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a installation id in application data, create and store as application data.
    if (!settings.ContainsKey("__NHInstallationId"))
    {
        installationId = Guid.NewGuid().ToString();
        settings.Add("__NHInstallationId", installationId);
    }

    installationId = (string)settings["__NHInstallationId"];

    var deviceInstallation = new DeviceInstallation
    {
        installationId = installationId,
        platform = "wns",
        pushChannel = channel.Uri,
        //tags = tags.ToArray<string>()
    };

    var statusCode = await CreateOrUpdateInstallationAsync(deviceInstallation, 
                        "<HUBNAME>", "<SHARED LISTEN CONNECTION STRING>");

    if (statusCode != HttpStatusCode.Accepted)
    {
        var dialog = new MessageDialog(statusCode.ToString(), "Registration failed. Installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }
    else
    {
        var dialog = new MessageDialog("Registration successful using installation Id : " + installationId);
        dialog.Commands.Add(new UICommand("OK"));
        await dialog.ShowAsync();
    }



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a><span data-ttu-id="a8113-169">Példa kód tooregister a regisztráció egy eszközt az értesítési központban</span><span class="sxs-lookup"><span data-stu-id="a8113-169">Example code tooregister with a notification hub from a device using a registration</span></span>
<span data-ttu-id="a8113-170">Ezek a módszerek regisztráció létrehozása vagy módosítása egy hello eszközhöz, amelyre nevezzük.</span><span class="sxs-lookup"><span data-stu-id="a8113-170">These methods create or update a registration for hello device on which they are called.</span></span> <span data-ttu-id="a8113-171">Ez azt jelenti, hogy a rendelés tooupdate hello leíró vagy hello címkét, felül kell írni hello teljes regisztrációs.</span><span class="sxs-lookup"><span data-stu-id="a8113-171">This means that in order tooupdate hello handle or hello tags, you must overwrite hello entire registration.</span></span> <span data-ttu-id="a8113-172">Ne feledje, hogy regisztrációk tranziens, így mindig rendelkeznie kell egy megbízható tároló, amelyet egy adott eszköz aktuális címkékkel hello.</span><span class="sxs-lookup"><span data-stu-id="a8113-172">Remember that registrations are transient, so you should always have a reliable store with hello current tags that a specific device needs.</span></span>

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // hello Device id from hello PNS
    var pushChannel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    // If you are registering from hello client itself, then store this registration id in device
    // storage. Then when hello app starts, you can check if a registration id already exists or not before
    // creating.
    var settings = ApplicationData.Current.LocalSettings.Values;

    // If we have not stored a registration id in application data, store in application data.
    if (!settings.ContainsKey("__NHRegistrationId"))
    {
        // make sure there are no existing registrations for this push handle (used for iOS and Android)    
        string newRegistrationId = null;
        var registrations = await hub.GetRegistrationsByChannelAsync(pushChannel.Uri, 100);
        foreach (RegistrationDescription registration in registrations)
        {
            if (newRegistrationId == null)
            {
                newRegistrationId = registration.RegistrationId;
            }
            else
            {
                await hub.DeleteRegistrationAsync(registration);
            }
        }

        newRegistrationId = await hub.CreateRegistrationIdAsync();

        settings.Add("__NHRegistrationId", newRegistrationId);
    }

    string regId = (string)settings["__NHRegistrationId"];

    RegistrationDescription registration = new WindowsRegistrationDescription(pushChannel.Uri);
    registration.RegistrationId = regId;
    registration.Tags = new HashSet<string>(YourTags);

    try
    {
        await hub.CreateOrUpdateRegistrationAsync(registration);
    }
    catch (Microsoft.WindowsAzure.Messaging.RegistrationGoneException e)
    {
        settings.Remove("__NHRegistrationId");
    }


## <a name="registration-management-from-a-backend"></a><span data-ttu-id="a8113-173">Regisztrációs felügyeleti a háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="a8113-173">Registration management from a backend</span></span>
<span data-ttu-id="a8113-174">Regisztráció kezelése hello háttérrendszerből szükséges kód írása.</span><span class="sxs-lookup"><span data-stu-id="a8113-174">Managing registrations from hello backend requires writing additional code.</span></span> <span data-ttu-id="a8113-175">hello eszköz hello alkalmazásából frissített PNS-leírójának toohello háttér hello hello alkalmazás indításakor (valamint a címkék és sablonok), és hello háttér frissítenie kell a leíró hello értesítési központ kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="a8113-175">hello app from hello device must provide hello updated PNS handle toohello backend every time hello app starts (along with tags and templates), and hello backend must update this handle on hello notification hub.</span></span> <span data-ttu-id="a8113-176">a következő kép hello Ez a kialakítás mutatja be.</span><span class="sxs-lookup"><span data-stu-id="a8113-176">hello following picture illustrates this design.</span></span>

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

<span data-ttu-id="a8113-177">regisztráció kezelése hello háttérrendszerből hello előnyei közé tartozik a hello képességét toomodify címkék tooregistrations akkor is, ha a megfelelő alkalmazás hello hello eszközön nem aktív, és tooauthenticate hello ügyfélalkalmazás egy címke tooits regisztrációs hozzáadása előtt.</span><span class="sxs-lookup"><span data-stu-id="a8113-177">hello advantages of managing registrations from hello backend include hello ability toomodify tags tooregistrations even when hello corresponding app on hello device is inactive, and tooauthenticate hello client app before adding a tag tooits registration.</span></span>

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a><span data-ttu-id="a8113-178">Példa kód tooregister egy egy kiszolgáló nélküli telepítés használatával az értesítési központban</span><span class="sxs-lookup"><span data-stu-id="a8113-178">Example code tooregister with a notification hub from a backend using an installation</span></span>
<span data-ttu-id="a8113-179">hello ügyféleszköz továbbra is lekérdezi a PNS-leírójának és a vonatkozó telepítési tulajdonságait azt korábban és hello háttérkiszolgálón hello regisztrálására és engedélyezi egy egyéni API címkéket stb hello háttér hívások kihasználhatják a hello [Notification Hub SDK háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="a8113-179">hello client device still gets its PNS handle and relevant installation properties as before and calls a custom API on hello backend that can perform hello registration and authorize tags etc. hello backend can leverage hello [Notification Hub SDK for backend operations](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="a8113-180">Is használhatja a hello javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) hello telepítés frissítésének.</span><span class="sxs-lookup"><span data-stu-id="a8113-180">You can also use hello PATCH method using hello [JSON-Patch standard](https://tools.ietf.org/html/rfc6902) for updating hello installation.</span></span>

    // Initialize hello Notification Hub
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(listenConnString, hubName);

    // Custom API on hello backend
    public async Task<HttpResponseMessage> Put(DeviceInstallation deviceUpdate)
    {

        Installation installation = new Installation();
        installation.InstallationId = deviceUpdate.InstallationId;
        installation.PushChannel = deviceUpdate.Handle;
        installation.Tags = deviceUpdate.Tags;

        switch (deviceUpdate.Platform)
        {
            case "mpns":
                installation.Platform = NotificationPlatform.Mpns;
                break;
            case "wns":
                installation.Platform = NotificationPlatform.Wns;
                break;
            case "apns":
                installation.Platform = NotificationPlatform.Apns;
                break;
            case "gcm":
                installation.Platform = NotificationPlatform.Gcm;
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }


        // In hello backend we can control if a user is allowed tooadd tags
        //installation.Tags = new List<string>(deviceUpdate.Tags);
        //installation.Tags.Add("username:" + username);

        await hub.CreateOrUpdateInstallationAsync(installation);

        return Request.CreateResponse(HttpStatusCode.OK);
    }


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a><span data-ttu-id="a8113-181">Példa kód tooregister egy egy eszközt a regisztrációs azonosítót az értesítési központban</span><span class="sxs-lookup"><span data-stu-id="a8113-181">Example code tooregister with a notification hub from a device using a registration id</span></span>
<span data-ttu-id="a8113-182">Az alkalmazás háttérrendszeréből a regisztrációs alapvető CRUDS műveleteket végezheti el.</span><span class="sxs-lookup"><span data-stu-id="a8113-182">From your app backend, you can perform basic CRUDS operations on registrations.</span></span> <span data-ttu-id="a8113-183">Példa:</span><span class="sxs-lookup"><span data-stu-id="a8113-183">For example:</span></span>

    var hub = NotificationHubClient.CreateClientFromConnectionString("{connectionString}", "hubName");

    // create a registration description object of hello correct type, e.g.
    var reg = new WindowsRegistrationDescription(channelUri, tags);

    // Create
    await hub.CreateRegistrationAsync(reg);

    // Get by id
    var r = await hub.GetRegistrationAsync<RegistrationDescription>("id");

    // update
    r.Tags.Add("myTag");

    // update on hub
    await hub.UpdateRegistrationAsync(r);

    // delete
    await hub.DeleteRegistrationAsync(r);


<span data-ttu-id="a8113-184">hello háttér párhuzamossági közötti regisztrációs frissítéseket kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="a8113-184">hello backend must handle concurrency between registration updates.</span></span> <span data-ttu-id="a8113-185">A Service Bus egyidejű hozzáférések optimista vezérlését regisztrációs Management kínál.</span><span class="sxs-lookup"><span data-stu-id="a8113-185">Service Bus offers optimistic concurrency control for registration management.</span></span> <span data-ttu-id="a8113-186">Hello HTTP szinten ez implementálása hello használatának ETag regisztrációs felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a8113-186">At hello HTTP level, this is implemented with hello use of ETag on registration management operations.</span></span> <span data-ttu-id="a8113-187">Ez a szolgáltatás Microsoft SDKs, amelyek kivételt jelez a frissítés elutasítása párhuzamossági okokból átlátható módon használja.</span><span class="sxs-lookup"><span data-stu-id="a8113-187">This feature is transparently used by Microsoft SDKs, which throw an exception if an update is rejected for concurrency reasons.</span></span> <span data-ttu-id="a8113-188">hello háttéralkalmazás felelős az ilyen kivételek kezelése, valamint újrapróbálkozás hello frissítés, ha szükséges.</span><span class="sxs-lookup"><span data-stu-id="a8113-188">hello app backend is responsible for handling these exceptions and retrying hello update if required.</span></span>

