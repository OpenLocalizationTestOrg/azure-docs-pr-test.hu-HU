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
# <a name="registration-management"></a>Regisztrációkezelés
## <a name="overview"></a>Áttekintés
Ez a témakör ismerteti, hogyan tooregister eszközök a rendelés tooreceive a notification hubs leküldéses értesítések. hello a témakör ismerteti a regisztrációk magas szinten, majd vezet be az eszközök regisztrálása a hello két fő minták: hello eszközről regisztrálása közvetlenül toohello értesítési központot, és regisztrálnia kell az alkalmazás-háttéralkalmazás segítségével. 

## <a name="what-is-device-registration"></a>Mi az az eszközök regisztrációja
Eszközregisztráció az egy értesítési központot teszi lehetővé a **regisztrációs** vagy **telepítési**.

#### <a name="registrations"></a>Regisztrációk
A regisztrációs Platform Notification szolgáltatás (PNS) kezelni egy eszköz számára a címkék, és valószínűleg egy sablon hello társítja. hello PNS-leírójának lehet a ChannelURI, az eszköz jogkivonatát, illetve a GCM regisztrációs azonosítót. Címkék nem használt tooroute értesítést toohello megfelelő készletének eszközt kezeli. További információkért lásd: [Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md). Sablonok használt tooimplement regisztrációs átalakítási. További információkért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).

#### <a name="installations"></a>Telepítés
Egy bővített egy telepítés, amely tartalmazza a leküldéses összessége regisztrációs kapcsolódó tulajdonságok. Hello tooregistering legújabb és legjobb módszer az eszközök. Azonban ez nem támogatja az ügyféloldali .NET SDK-val ([Notification Hub SDK a háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/)) még érvényes.  Ez azt jelenti, ha regisztrál eszközről hello ügyfél magát, akkor egy toouse hello [Notification hub REST API](https://msdn.microsoft.com/library/mt621153.aspx) toosupport telepítéseket közelítse. Ha háttérszolgáltatás használ, el tudja toouse [Notification Hub SDK a háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

hello az alábbiakban néhány fontos előnnyel toousing telepítés:

* Teljes idempotent létrehozásához vagy frissítéséhez egy telepítés. Úgy is újraindítható anélkül, duplikált regisztrációjával kapcsolatos bármilyen probléma merül fel.
* hello telepítési modell könnyen toodo egyedi leküldéses értesítések - eszközre célzó révén. A rendszer címke **"$InstallationId: [végrehajtott]"** automatikusan fel lesz véve az egyes telepítési alapú regisztrációs. Így hívása egy küldési toothis címke tootarget egy adott eszköz anélkül, hogy toodo további kódolása.
* Telepítés használatával is lehetővé teszi, hogy Ön toodo részleges regisztrációs frissítéseket. hello részleges frissítés telepítésének van szükség a javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902). Ez akkor különösen hasznos, ha azt szeretné, hogy hello regisztrációs tooupdate címke van megadva. Nem rendelkezik toopull hello teljes regisztrációs le, és küldje el újra az összes hello előző címke.

Telepítés a következő tulajdonságok hello hello is tartalmazhat. Teljes listáját lásd a hello telepítési tulajdonságok, [létrehozása vagy egy telepítési felülírása REST API](https://msdn.microsoft.com/library/azure/mt621153.aspx) vagy [telepítési tulajdonságokat](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.installation_properties.aspx) hello számára.

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



Regisztráció és a telepítések alapértelmezés szerint nem lejáró fontos toonote.

Regisztráció és a telepítés tartalmaznia kell egy érvényes PNS-leírójának minden eszköz vagy csatorna. PNS-leírók regisztrálásáért hello eszközön ügyfél alkalmazásban csak érhető el, mert egy minta tooregister közvetlenül hello ügyfélalkalmazás az adott eszközön. Hello más aktuális, a biztonsági szempontok és az üzleti logika vonatkozó tootags szükség lehet hello app háttér-toomanage eszközregisztrációját. 

#### <a name="templates"></a>Sablonok
Ha azt szeretné, hogy toouse [sablonok](notification-hubs-templates-cross-platform-push-messages.md), hello eszköz telepítése is rendelkezik az összes sablon egy JSON-ban, hogy eszközhöz társított (lásd a fenti minta) formátumban. hello sablonnevek segítségével különféle sablonok hello cél ugyanarra az eszközre.

Vegye figyelembe, hogy minden a sablonnevet leképezhető tooa sablon szervezet és címkék nem kötelező megadni. Továbbá minden egyes platformhoz lehet további sablontulajdonságok. A Windows Állapottárolójához (WNS) és Windows Phone 8 (MPNS segítségével) egy további-fejléc készlettel hello sablon része lehet. APNs hello esetben beállíthatja egy lejárati tulajdonság tooeither egy állandónak vagy tooa sablon kifejezést. Teljes listáját lásd a hello telepítési tulajdonságok, [létrehozása vagy felülírni a telepítés többi](https://msdn.microsoft.com/library/azure/mt621153.aspx) témakör.

#### <a name="secondary-tiles-for-windows-store-apps"></a>Windows Áruházbeli alkalmazások másodlagos Csempéket
A Windows áruház ügyfélalkalmazások toosecondary csempék van küldő értesítések hello azonos toohello elsődleges elküldeni őket. Ez is támogatott telepítések esetén. Ne feledje, hogy másodlagos csempék egy másik ChannelUri, mely hello SDK az ügyfélalkalmazás transzparens módon kezeli.

hello SecondaryTiles szótár használ, amely használt toocreate hello SecondaryTiles objektum a Windows Áruházbeli alkalmazásban azonos TileId hello.
A hello ChannelUri elsődleges, másodlagos csempék ChannelUris bármikor is módosítása. Rendelés tookeep hello telepítés esetén a hello az értesítési központ frissítése, hello eszköz frissíteni kell azokat a hello hello másodlagos csempék aktuális ChannelUris.

## <a name="registration-management-from-hello-device"></a>Regisztrációs felügyeleti hello eszközről
Eszközregisztráció ügyfél alkalmazásokból való kezelésekor hello háttér felelős csak az értesítések küldése. Ügyfélalkalmazások PNS-leírók regisztrálásáért mentése toodate megőrzése, és regisztrálja a címkék. a következő kép hello ebben a mintában mutatja be.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-device.png)

először hello eszköz PNS kezelni hello kikeresi hello PNS, majd közvetlenül regisztrálja hello értesítési központban. Hello regisztráció befejezését követően hello háttéralkalmazás, hogy a regisztrációs célzó értesítés küldése. További információ toosend értesítések, lásd: [Útválasztás és címke kifejezések](notification-hubs-tags-segment-push-message.md).
Vegye figyelembe, hogy ebben az esetben használhatja a notification hubs hello eszköz csak figyelni jogok tooaccess. További információkért lásd: [biztonsági](notification-hubs-push-notification-security.md).

Hello eszközről regisztrálása hello legegyszerűbb módja, de a hátrányokkal azt.
hello első hátránya, hogy egy ügyfélalkalmazás csak képes frissíteni a címkék, aktív hello alkalmazás esetén. Például ha egy felhasználó két eszközöket, amelyeket regisztrálni címkék kapcsolódó toosport csapatok, amikor egy további (például Seahawks) címke hello első eszközt regisztrál, hello második eszköz nem kap hello Seahawks hello értesítések amíg hello hello alkalmazást második eszköz végrehajtása még egyszer. Általában több eszköz által érintett címkék, amikor címkék kezelése hello háttérrendszerből beállítás kívánatos.
hello második regisztrációs felügyeleti hello ügyfél alkalmazásból hátránya, hogy mivel alkalmazásokat is megtámadott, biztonságossá tétele hello regisztrációs toospecific címkék igényel különös figyelmet "címke szintű biztonság." hello szakaszban leírtak szerint

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-an-installation"></a>Példa kód tooregister egy eszközről egy telepítést használ egy értesítési központban
Jelenleg ez csak akkor támogatott hello segítségével [Notification hub REST API](https://msdn.microsoft.com/library/mt621153.aspx).

Is használhatja a hello javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) hello telepítés frissítésének.

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



#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration"></a>Példa kód tooregister a regisztráció egy eszközt az értesítési központban
Ezek a módszerek regisztráció létrehozása vagy módosítása egy hello eszközhöz, amelyre nevezzük. Ez azt jelenti, hogy a rendelés tooupdate hello leíró vagy hello címkét, felül kell írni hello teljes regisztrációs. Ne feledje, hogy regisztrációk tranziens, így mindig rendelkeznie kell egy megbízható tároló, amelyet egy adott eszköz aktuális címkékkel hello.

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


## <a name="registration-management-from-a-backend"></a>Regisztrációs felügyeleti a háttérrendszerből
Regisztráció kezelése hello háttérrendszerből szükséges kód írása. hello eszköz hello alkalmazásából frissített PNS-leírójának toohello háttér hello hello alkalmazás indításakor (valamint a címkék és sablonok), és hello háttér frissítenie kell a leíró hello értesítési központ kell megadnia. a következő kép hello Ez a kialakítás mutatja be.

![](./media/notification-hubs-registration-management/notification-hubs-registering-on-backend.png)

regisztráció kezelése hello háttérrendszerből hello előnyei közé tartozik a hello képességét toomodify címkék tooregistrations akkor is, ha a megfelelő alkalmazás hello hello eszközön nem aktív, és tooauthenticate hello ügyfélalkalmazás egy címke tooits regisztrációs hozzáadása előtt.

#### <a name="example-code-tooregister-with-a-notification-hub-from-a-backend-using-an-installation"></a>Példa kód tooregister egy egy kiszolgáló nélküli telepítés használatával az értesítési központban
hello ügyféleszköz továbbra is lekérdezi a PNS-leírójának és a vonatkozó telepítési tulajdonságait azt korábban és hello háttérkiszolgálón hello regisztrálására és engedélyezi egy egyéni API címkéket stb hello háttér hívások kihasználhatják a hello [Notification Hub SDK háttérbeli műveletek](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Is használhatja a hello javítás módszerrel hello [JSON-javítás standard](https://tools.ietf.org/html/rfc6902) hello telepítés frissítésének.

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


#### <a name="example-code-tooregister-with-a-notification-hub-from-a-device-using-a-registration-id"></a>Példa kód tooregister egy egy eszközt a regisztrációs azonosítót az értesítési központban
Az alkalmazás háttérrendszeréből a regisztrációs alapvető CRUDS műveleteket végezheti el. Példa:

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


hello háttér párhuzamossági közötti regisztrációs frissítéseket kell kezelni. A Service Bus egyidejű hozzáférések optimista vezérlését regisztrációs Management kínál. Hello HTTP szinten ez implementálása hello használatának ETag regisztrációs felügyeleti műveleteket. Ez a szolgáltatás Microsoft SDKs, amelyek kivételt jelez a frissítés elutasítása párhuzamossági okokból átlátható módon használja. hello háttéralkalmazás felelős az ilyen kivételek kezelése, valamint újrapróbálkozás hello frissítés, ha szükséges.

