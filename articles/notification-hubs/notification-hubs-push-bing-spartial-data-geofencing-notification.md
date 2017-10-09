---
title: "az Azure Notification Hubs és a Bing térbeli adatainak aaaGeo bekerített leküldéses értesítések |} Microsoft Docs"
description: "Ebből az oktatóanyagból megtudhatja, hogyan toodeliver helyalapú leküldéses értesítések az Azure Notification Hubs és a Bing térbeli adatainak."
services: notification-hubs
documentationcenter: windows
keywords: "leküldéses értesítés,leküldéses értesítés"
author: dend
manager: yuaxu
editor: dend
ms.assetid: f41beea1-0d62-4418-9ffc-c9d70607a1b7
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 05/31/2016
ms.author: dendeli
ms.openlocfilehash: d6efe473537f2159240c53e780741f12619c045d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a>Geokerítéses leküldéses értesítések az Azure Notification Hubs és a Bing térbeli adatainak használatával
> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).
> 
> 

Ebben az oktatóanyagban megtudhatja, hogyan toodeliver helyalapú leküldéses értesítések az Azure Notification Hubs és a Bing térbeli adatainak használatával egy univerzális Windows Platform-alkalmazásból.

## <a name="prerequisites"></a>Előfeltételek
Mindenekelőtt arról, hogy rendelkezik-e az összes hello szoftver és szolgáltatási előfeltétellel toomake szüksége:

* [Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) vagy újabb (a [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) is megfelelő). 
* Hello legújabb verziójának [Azure SDK](https://azure.microsoft.com/downloads/). 
* [Fiók a Bing Térképek fejlesztői központjához](https://www.bingmapsportal.com/) (ingyenesen létrehozható, és hozzárendelhető a Microsoft-fiókjához). 

## <a name="getting-started"></a>Első lépések
Először hozzon létre hello projekt. A Visual Studióban indítson egy **Üres alkalmazás (Univerzális Windows)** típusú új projektet.

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

Ha hello projekt létrehozása befejeződött, hello alkalmazás maga hello kihasználhatja kell rendelkezésre állnia. Most tegyük beállítását hello geokerítés-infrastruktúra. Dolgozunk a Bing ezen szolgáltatások folyamatos toouse, mert van egy nyilvános REST API-végpont használatával kérdezhetjük tooquery adott helyeket:

    http://spatial.virtualearth.net/REST/v1/data/

Szüksége lesz a toospecify hello paraméterek tooget következő működéséhez:

* **Adatforrás azonosítója** és **Adatforrás neve** – a Bing Térképek API-adatforrásai számos összegyűjtött metaadatot tartalmaznak, például a helyadatokat és a nyitvatartási időt. További információt itt talál. 
* **Entitás neve** – hello entitás toouse hivatkozási pontjaként hello értesítés. 
* **A Bing térképek API-kulcs** – Ez az hello kulcs korábban beszerzett hello Bing fejlesztői központban regisztrált fiókjában létrehozásakor.

Tekintsük át részletesen a hello beállítása az egyes hello fenti elemek beállításait.

## <a name="setting-up-hello-data-source"></a>Hello adatforrás beállítása
A Bing térképek fejlesztői központjához hello akkor is elvégezhető. Egyszerűen kattintson a **adatforrások** hello felső navigációs sáv, és válassza ki a **adatforrások kezelése**.

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

Ha nem dolgozott a Bing térképek API-t, valószínűleg nincs nem lehet egyetlen adatforrást sem található, hogy csak létrehozhasson egy új feltöltés tooa adatforrás kattintva. Ellenőrizze, hogy az összes hello kötelező mezőt kitöltötte:

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

Talán kíváncsi – mi hello partíciósadat-fájlnak, és mit kell feltöltenie? Ebben a tesztben hello célokra használjuk a hello San Francisco partszakasz egy területét pipe-alapú hello minta:

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

a fenti hello következő entitást képviselik:

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

Egyszerűen másolja és illessze be a fenti hello karakterláncot egy új fájlba, és mentse a fájt **NotificationHubsGeofence.pipe**, és töltse fel az hello Bing fejlesztői központjában.

> [!NOTE]
> Előfordulhat, hogy felszólító toospecify hello egy új kulcsot **főkulcs** , amely nem azonos a hello **lekérdezési kulcsot**. Egyszerűen hozzon létre egy új kulcsot hello irányítópult és a frissítési hello adatforrás-feltöltési oldalt.
> 
> 

Hello adatfájl feltöltése után kell, hogy közzé hello adatforrás toomake. 

Nyissa meg túl**adatforrások kezelése**, fentiekhez hasonló azt adta fent, hello listában keresse meg az adatforrást, és kattintson a **közzététel** a hello **műveletek** oszlop. Az egy kicsit kell megjelennie az adatokat az adatforrásból a hello **közzétett adatforrások** lapon:

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

Ha **szerkesztése**, akkor azt helyeket lesz képes toosee egy pillanat alatt:

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

Hello portál ezen a ponton nem jeleníti meg, hello hello geokerítésen létrehozott határok – csak meg kell erősítenünk, hogy a megadott hello helyre van hello megfelelő környéken található.

Most már rendelkezik hello adatforrás hello követelményeinek. hello kérelem URL-címen hello API-hívás, a Bing térképek fejlesztői központjához, hello tooget hello részletekért kattintson **adatforrások** válassza **az adatforrásra vonatkozó információ**.

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

Hello **lekérdezés URL-címe** mi vagyunk itt után van. Ez a hello végpont, amelyre vonatkozóan végezhetünk lekérdezések toocheck e hello eszköz jelenleg hello hely határain belül, vagy nem. Ezzel a beállítással ellenőrizheti, egyszerűen kell egy GET hívja meg hello lekérdezés URL-címe, szemben a következő paraméterek hozzáfűzésével hello tooexecute tooperform:

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

Ily módon ad meg a bázispont, hogy a Microsoft hello eszközről beszerzése és a Bing térképek automatikusan végrehajtják a hello számítások toosee hello geokerítésen belül van-e. Hello kérelem böngésző (vagy a cURL) használatával hajtható végre, miután elérhetővé válik szabványos JSON-választ:

![](./media/notification-hubs-geofence/bing-maps-json.png)

Ezt a választ csak akkor történik, ha hello pont ténylegesen hello kijelölt határain belül. Ha ez nem így van, az **Eredmények** gyűjtő üres lesz:

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a>Hello UWP-alkalmazás beállítása
Most, hogy hello adatforrás rendelkezésre áll, először is dolgozunk hello elkezdhetünk korábban elindított UWP-alkalmazással.

Mindenekelőtt engedélyezni kell a helyalapú szolgáltatásokat az alkalmazásban. toodo e, kattintson duplán a `Package.appxmanifest` fájlt **Megoldáskezelőben**.

![](./media/notification-hubs-geofence/vs-package-manifest.png)

Hello lapon imént megnyitott, kattintson a **képességek** és győződjön meg arról, hogy kiválassza **hely**:

![](./media/notification-hubs-geofence/vs-package-location.png)

Hello helymeghatározási képesség deklarálva van, mert hozzon létre egy új mappát a megoldásban nevű `Core`, és adjon hozzá egy új fájlt nevű `LocationHelper.cs`:

![](./media/notification-hubs-geofence/vs-location-helper.png)

Hello `LocationHelper` osztály itt meglehetősen egyszerű ezen a ponton – minden esetben van lehetővé teszik a számunkra tooobtain hello felhasználói hely hello rendszer API-n keresztül:

    using System;
    using System.Threading.Tasks;
    using Windows.Devices.Geolocation;

    namespace NotificationHubs.Geofence.Core
    {
        public class LocationHelper
        {
            private static readonly uint AppDesiredAccuracyInMeters = 10;

            public async static Task<Geoposition> GetCurrentLocation()
            {
                var accessStatus = await Geolocator.RequestAccessAsync();
                switch (accessStatus)
                {
                    case GeolocationAccessStatus.Allowed:
                        {
                            Geolocator geolocator = new Geolocator { DesiredAccuracyInMeters = AppDesiredAccuracyInMeters };

                            return await geolocator.GetGeopositionAsync();
                        }
                    default:
                        {
                            return null;
                        }
                }
            }

        }
    }

További információkat a felhasználói helyadatok UWP-alkalmazások hello hatósági hello érheti el [MSDN-dokumentumban](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).

toocheck hello hely beszerzési ténylegesen működik, nyissa meg a hello kód oldalán a főoldal (`MainPage.xaml.cs`). Hozzon létre egy új eseménykezelőt hello `Loaded` hello esemény `MainPage` konstruktor:

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

hello hello eseménykezelő megvalósítása a következőképpen történik:

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

Figyelje meg, hogy deklaráltuk hello kezelő aszinkron módon mert `GetCurrentLocation` várható, és ezért az aszinkron környezetben végrehajtott toobe igényel. Továbbá mivel bizonyos körülmények kaphatunk NULL helyadatot (pl. hello helyszolgáltatás le vannak tiltva vagy hello alkalmazás megtagadva engedélyek tooaccess helyét), igazolnia kell arról, hogy megfelelő kezeléséről egy null ellenőrzéssel toomake.

Hello alkalmazás futtatásához. Engedélyezze a helyadatokhoz való hozzáférést:

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

Egyszer hello az alkalmazás elindítása, meg kell tudni toosee hello koordinátákkal hello **kimeneti** ablakban:

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

Most már tudja, hogy helyadatok beszerzésének működését látja-e az ügyfélre a Loaded szabad tooremove hello teszt-eseménykezelőjét, mert azt nem használhatja többé.

következő lépés hello toocapture helyadatok módosításainak. Az adott, ugorjunk vissza toohello `LocationHelper` osztályhoz, és adja hozzá a hello eseménykezelője `PositionChanged`:

    geolocator.PositionChanged += Geolocator_PositionChanged;

hello végrehajtására fogja megjeleníteni az hello hely koordinátái hello **kimeneti** ablakban:

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a>Hello háttérkiszolgáló beállítása
Töltse le a hello [.NET háttérrendszer mintáját a Githubról](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). Hello letöltés befejezése után nyissa meg a hello `NotifyUsers` mappa, és ezt követően – hello `NotifyUsers.sln` fájlt.

Set hello `AppBackend` hello projekt **StartUp Project** , majd indítsa el azt.

![](./media/notification-hubs-geofence/vs-startup-project.png)

hello projekt már konfigurált toosend leküldéses értesítések tootarget eszközöket, így toodo csak két dolgot – kell kimenő hello értesítési központ megfelelő kapcsolati karakterláncát hello felcserélése, és adja hozzá a határ azonosító toosend hello értesítési csak akkor, ha hello felhasználói hello geokerítésen belül van.

tooconfigure hello kapcsolati karakterláncot, a hello `Models` nyissa meg a mappa `Notifications.cs`. Hello `NotificationHubClient.CreateClientFromConnectionString` függvénynek tartalmaznia kell az értesítési központ, amely hello kaphat információt hello [Azure Portal](https://portal.azure.com) (belső hello **hozzáférési házirendek** panel az  **Beállítások**). Mentse a hello frissített konfigurációs fájlt.

Most létre kell toocreate egy modellt a Bing térképek API-eredményéhez hello. Ennek legegyszerűbb módja toodo, kattintson a jobb gombbal a hello hello `Models` mappa, **Hozzáadás** > **osztály**. Nevezze el a következőképpen: `GeofenceBoundary.cs`. Ha végzett, hello JSON másolása és a Visual Studióban válassza hello első szakaszban tárgyalt hello API válasz **szerkesztése** > **Beillesztés** > **JSON beillesztése osztályokként**. 

Így biztosítható, hogy hello az objektum deszerializálása pontosan megfelelően. Az eredményül kapott osztály a következőhöz hasonló lesz:

    namespace AppBackend.Models
    {
        public class Rootobject
        {
            public D d { get; set; }
        }

        public class D
        {
            public string __copyright { get; set; }
            public Result[] results { get; set; }
        }

        public class Result
        {
            public __Metadata __metadata { get; set; }
            public string EntityID { get; set; }
            public string Name { get; set; }
            public float Longitude { get; set; }
            public float Latitude { get; set; }
            public string Boundary { get; set; }
            public string Confidence { get; set; }
            public string Locality { get; set; }
            public string AddressLine { get; set; }
            public string AdminDistrict { get; set; }
            public string CountryRegion { get; set; }
            public string PostalCode { get; set; }
        }

        public class __Metadata
        {
            public string uri { get; set; }
        }
    }

Ezután nyissa meg a következőt: `Controllers` > `NotificationsController.cs`. Hello cél szélességi és hosszúsági tootweak hello Post hívás tooaccount szükséges. Ehhez egyszerűen adja hozzá két karakterláncok toohello függvényaláíráshoz – `latitude` és `longitude`.

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

Hozzon létre egy új osztályt nevű hello projektben `ApiHelper.cs` – ezzel fogunk tooconnect tooBing toocheck pontok határvonalainak metszéspontjait. Adjon hozzá egy `IsPointWithinBounds` függvényt, amely a következőre hasonlít:

    public class ApiHelper
    {
        public static readonly string ApiEndpoint = "{YOUR_QUERY_ENDPOINT}?spatialFilter=intersects(%27POINT%20({0}%20{1})%27)&$format=json&key={2}";
        public static readonly string ApiKey = "{YOUR_API_KEY}";

        public static bool IsPointWithinBounds(string longitude,string latitude)
        {
            var json = new WebClient().DownloadString(string.Format(ApiEndpoint, longitude, latitude, ApiKey));
            var result = JsonConvert.DeserializeObject<Rootobject>(json);
            if (result.d.results != null && result.d.results.Count() > 0)
            {
                return true;
            }
            else
            {
                return false;
            }
        }
    }

> [!NOTE]
> Győződjön meg arról, hogy toosubstitute hello API végpont hello lekérdezés URL-címet, amely korábban beszerzett hello (Ugyanez vonatkozik toohello API-kulcs) a Bing fejlesztői központjában. 
> 
> 

Eredmények toohello lekérdezés esetén a, az azt jelenti, hogy a megadott pont hello hello geokerítésen hello határain belül van, a rendszer visszaadja a `true`. Ha nem, a Bing ezzel azt jelzi, hogy a hello pont hello keresési területen, kívül esik, így a rendszer visszaadja a `false`.

Vissza a `NotificationsController.cs`, hozzon létre egy ellenőrzést közvetlenül hello switch utasítás előtt:

    if (ApiHelper.IsPointWithinBounds(longitude, latitude))
    {
        switch (pns.ToLower())
        {
            case "wns":
                //// Windows 8.1 / Windows Phone 8.1
                var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                // Windows 10 specific Action Center support
                toast = @"<toast><visual><binding template=""ToastGeneric""><text id=""1"">" +
                            "From " + user + ": " + message + "</text></binding></visual></toast>";
                outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);

                break;
        }
    }

Ily módon hello értesítést csak küldi hello határain belül hello pont esetén.

## <a name="testing-push-notifications-in-hello-uwp-app"></a>Leküldéses értesítések tesztelése UWP-alkalmazásban hello
Ha visszalép toohello UWP-alkalmazást, igazolnia kell tudni tootest értesítések. Hello belül `LocationHelper` osztály, hozzon létre egy új függvényt – `SendLocationToBackend`:

    public static async Task SendLocationToBackend(string pns, string userTag, string message, string latitude, string longitude)
    {
        var POST_URL = "http://localhost:8741/api/notifications?pns=" +
            pns + "&to_tag=" + userTag + "&latitude=" + latitude + "&longitude=" + longitude;

        using (var httpClient = new HttpClient())
        {
            try
            {
                await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                    System.Text.Encoding.UTF8, "application/json"));
            }
            catch (Exception ex)
            {
                Debug.WriteLine(ex.Message);
            }
        }
    }

> [!NOTE]
> A felcserélendő hello `POST_URL` hello előző szakaszban létrehozott üzembe helyezett webalkalmazás toohello helyét. Most már OK toorun helyileg, akkor előfordulhat, hogy egy nyilvános verzió telepítéséhez, szüksége lesz a toohost, de azt egy külső szolgáltatón.
> 
> 

Most ellenőrizzük, hogy regisztráljuk hello UWP-alkalmazást a leküldéses értesítések. A Visual Studióban, kattintson a **projekt** > **tárolására** > **alkalmazás társítása hello tároló**.

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

Amikor bejelentkezik a fejlesztői fiókba tooyour, ellenőrizze, hogy válasszon ki egy meglévő alkalmazást, vagy hozzon létre egy újat és az hello csomag társítani. 

Nyissa meg a toohello Dev Center webhely és a nyitott hello alkalmazást, amely az újonnan létrehozott. Kattintson a következőkre: **Services** (Szolgáltatások)  > **Push Notifications** (Leküldéses értesítések)  > **Live Services site** (Live Services webhely).

![](./media/notification-hubs-geofence/ms-live-services.png)

Hello webhelyen jegyezze fel a hello **Alkalmazáskulcsot** és hello **CSOMAGAZONOSÍTÓT**. Szüksége lesz a egyaránt hello Azure portál – nyissa meg az értesítési központot, kattintson a **beállítások** > **értesítési szolgáltatások** > **Windows (WNS)**és adja meg a szükséges hello hello információt.

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

Kattintson a **Mentés** gombra.

Kattintson a jobb gombbal a **Hivatkozások** elemre a **Megoldáskezelőben**, és válassza a **NuGet-csomagok kezelése** lehetőséget. Fel kell a hivatkozás toohello tooadd **Microsoft Azure Service Bus felügyelt kódtárára** – ehhez csak keressen a `WindowsAzure.Messaging.Managed` és tooyour projekt hozzáadása.

![](./media/notification-hubs-geofence/vs-nuget.png)

Tesztelési célokra vannak, létrehozható hello `MainPage_Loaded` eseménykezelő még egyszer, és adja hozzá a kódot részlet tooit:

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

a fenti hello regisztrálja hello app hello értesítési központban. Toogo készen áll! 

A `LocationHelper`, belül hello `Geolocator_PositionChanged` kezelőjében hozzáadhat egy tesztkódszakaszt amely hello hely hello geokerítésen belül:

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

Jelenleg nem átadott hello valós koordinátákat (amely nem feltétlenül hello határain belül hello pillanatban), és előre meghatározott érték, mert azt egy frissítési értesítés jelenik meg:

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a>Hogyan tovább?
Nincsenek további lépésekre, hogy szükség lehet a fent arra, hogy a hello megoldás éles használatra kész toomake hozzáadása toohello toofollow.

Mindenekelőtt szükség lehet, hogy a geokerítések dinamikus tooensure. Ez fogja kell beállítani a Bing API hello rendelés toobe képes tooupload új határain belül hello meglévő adatforráson belül. Tekintse át a hello [Bing Térbeliadat-szolgáltatási API dokumentációjában](https://msdn.microsoft.com/library/ff701734.aspx) hello tulajdonos olvashat.

Második, vagy sem, hogy a hello kézbesítési történik-e a megfelelő személyek toohello működő tooensure, érdemes lehet tootarget őket keresztül [címkézés](notification-hubs-tags-segment-push-message.md).

a fenti hello megoldás egy olyan forgatókönyvet, ahol lehetséges, hogy sokféle célplatform lehetséges, így azt adta korlátozza hello geokerítések toosystem-specifikus képességek ismerteti. Említett, az univerzális Windows Platform hello túl képességeket biztosít[észleli a geokerítések jobb out-of-az-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).

A Notification Hubs képességeiről a [dokumentációs portálon](https://azure.microsoft.com/documentation/services/notification-hubs/) talál további részleteket.

