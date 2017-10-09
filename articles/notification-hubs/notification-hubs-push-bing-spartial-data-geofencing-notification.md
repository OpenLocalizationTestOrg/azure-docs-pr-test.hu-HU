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
# <a name="geo-fenced-push-notifications-with-azure-notification-hubs-and-bing-spatial-data"></a><span data-ttu-id="44ab0-104">Geokerítéses leküldéses értesítések az Azure Notification Hubs és a Bing térbeli adatainak használatával</span><span class="sxs-lookup"><span data-stu-id="44ab0-104">Geo-fenced push notifications with Azure Notification Hubs and Bing Spatial Data</span></span>
> [!NOTE]
> <span data-ttu-id="44ab0-105">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="44ab0-105">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="44ab0-106">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="44ab0-106">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="44ab0-107">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span><span class="sxs-lookup"><span data-stu-id="44ab0-107">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02).</span></span>
> 
> 

<span data-ttu-id="44ab0-108">Ebben az oktatóanyagban megtudhatja, hogyan toodeliver helyalapú leküldéses értesítések az Azure Notification Hubs és a Bing térbeli adatainak használatával egy univerzális Windows Platform-alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="44ab0-108">In this tutorial, you will learn how toodeliver location-based push notifications with Azure Notification Hubs and Bing Spatial Data, leveraged from within a Universal Windows Platform application.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="44ab0-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="44ab0-109">Prerequisites</span></span>
<span data-ttu-id="44ab0-110">Mindenekelőtt arról, hogy rendelkezik-e az összes hello szoftver és szolgáltatási előfeltétellel toomake szüksége:</span><span class="sxs-lookup"><span data-stu-id="44ab0-110">First and foremost, you need toomake sure that you have all hello software and service pre-requisites:</span></span>

* <span data-ttu-id="44ab0-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) vagy újabb (a [Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) is megfelelő).</span><span class="sxs-lookup"><span data-stu-id="44ab0-111">[Visual Studio 2015 Update 1](https://www.visualstudio.com/en-us/downloads/download-visual-studio-vs.aspx) or later ([Community Edition](https://go.microsoft.com/fwlink/?LinkId=691978&clcid=0x409) will do as well).</span></span> 
* <span data-ttu-id="44ab0-112">Hello legújabb verziójának [Azure SDK](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="44ab0-112">Latest version of hello [Azure SDK](https://azure.microsoft.com/downloads/).</span></span> 
* <span data-ttu-id="44ab0-113">[Fiók a Bing Térképek fejlesztői központjához](https://www.bingmapsportal.com/) (ingyenesen létrehozható, és hozzárendelhető a Microsoft-fiókjához).</span><span class="sxs-lookup"><span data-stu-id="44ab0-113">[Bing Maps Dev Center account](https://www.bingmapsportal.com/) (you can create one for free and associate it with your Microsoft account).</span></span> 

## <a name="getting-started"></a><span data-ttu-id="44ab0-114">Első lépések</span><span class="sxs-lookup"><span data-stu-id="44ab0-114">Getting Started</span></span>
<span data-ttu-id="44ab0-115">Először hozzon létre hello projekt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-115">Let’s start by creating hello project.</span></span> <span data-ttu-id="44ab0-116">A Visual Studióban indítson egy **Üres alkalmazás (Univerzális Windows)** típusú új projektet.</span><span class="sxs-lookup"><span data-stu-id="44ab0-116">In Visual Studio, start a new project of type **Blank App (Universal Windows)**.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-create-blank-app.png)

<span data-ttu-id="44ab0-117">Ha hello projekt létrehozása befejeződött, hello alkalmazás maga hello kihasználhatja kell rendelkezésre állnia.</span><span class="sxs-lookup"><span data-stu-id="44ab0-117">Once hello project creation is complete, you should have hello harness for hello app itself.</span></span> <span data-ttu-id="44ab0-118">Most tegyük beállítását hello geokerítés-infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="44ab0-118">Now let’s set up everything for hello geo-fencing infrastructure.</span></span> <span data-ttu-id="44ab0-119">Dolgozunk a Bing ezen szolgáltatások folyamatos toouse, mert van egy nyilvános REST API-végpont használatával kérdezhetjük tooquery adott helyeket:</span><span class="sxs-lookup"><span data-stu-id="44ab0-119">Because we are going toouse Bing services for this, there is a public REST API endpoint that allows us tooquery specific location frames:</span></span>

    http://spatial.virtualearth.net/REST/v1/data/

<span data-ttu-id="44ab0-120">Szüksége lesz a toospecify hello paraméterek tooget következő működéséhez:</span><span class="sxs-lookup"><span data-stu-id="44ab0-120">You will need toospecify hello following parameters tooget it working:</span></span>

* <span data-ttu-id="44ab0-121">**Adatforrás azonosítója** és **Adatforrás neve** – a Bing Térképek API-adatforrásai számos összegyűjtött metaadatot tartalmaznak, például a helyadatokat és a nyitvatartási időt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-121">**Data Source ID** and **Data Source Name** – in Bing Maps API, data sources contain various bucketed metadata, such as locations and business hours of operation.</span></span> <span data-ttu-id="44ab0-122">További információt itt talál.</span><span class="sxs-lookup"><span data-stu-id="44ab0-122">You can read more about those here.</span></span> 
* <span data-ttu-id="44ab0-123">**Entitás neve** – hello entitás toouse hivatkozási pontjaként hello értesítés.</span><span class="sxs-lookup"><span data-stu-id="44ab0-123">**Entity Name** – hello entity you want toouse as a reference point for hello notification.</span></span> 
* <span data-ttu-id="44ab0-124">**A Bing térképek API-kulcs** – Ez az hello kulcs korábban beszerzett hello Bing fejlesztői központban regisztrált fiókjában létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="44ab0-124">**Bing Maps API Key** – this is hello key that you obtained earlier when you created hello Bing Dev Center account.</span></span>

<span data-ttu-id="44ab0-125">Tekintsük át részletesen a hello beállítása az egyes hello fenti elemek beállításait.</span><span class="sxs-lookup"><span data-stu-id="44ab0-125">Let’s do a deep-dive on hello setup for each of hello elements above.</span></span>

## <a name="setting-up-hello-data-source"></a><span data-ttu-id="44ab0-126">Hello adatforrás beállítása</span><span class="sxs-lookup"><span data-stu-id="44ab0-126">Setting up hello data source</span></span>
<span data-ttu-id="44ab0-127">A Bing térképek fejlesztői központjához hello akkor is elvégezhető.</span><span class="sxs-lookup"><span data-stu-id="44ab0-127">You can do it in hello Bing Maps Dev Center.</span></span> <span data-ttu-id="44ab0-128">Egyszerűen kattintson a **adatforrások** hello felső navigációs sáv, és válassza ki a **adatforrások kezelése**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-128">Simply click on **Data sources** in hello top navigation bar and select **Manage Data Sources**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-manage-data.png)

<span data-ttu-id="44ab0-129">Ha nem dolgozott a Bing térképek API-t, valószínűleg nincs nem lehet egyetlen adatforrást sem található, hogy csak létrehozhasson egy új feltöltés tooa adatforrás kattintva.</span><span class="sxs-lookup"><span data-stu-id="44ab0-129">If you have not worked with Bing Maps API before, most likely there won’t be any data sources present, so you can just create a new one by clicking on Upload data tooa data source.</span></span> <span data-ttu-id="44ab0-130">Ellenőrizze, hogy az összes hello kötelező mezőt kitöltötte:</span><span class="sxs-lookup"><span data-stu-id="44ab0-130">Make sure you fill out all hello required fields:</span></span>

![](./media/notification-hubs-geofence/bing-maps-create-data.png)

<span data-ttu-id="44ab0-131">Talán kíváncsi – mi hello partíciósadat-fájlnak, és mit kell feltöltenie?</span><span class="sxs-lookup"><span data-stu-id="44ab0-131">You might be wondering – what is hello data file and what should you be uploading?</span></span> <span data-ttu-id="44ab0-132">Ebben a tesztben hello célokra használjuk a hello San Francisco partszakasz egy területét pipe-alapú hello minta:</span><span class="sxs-lookup"><span data-stu-id="44ab0-132">For hello purposes of this test, we can just use hello sample pipe-based that frames an area of hello San Francisco waterfront:</span></span>

    Bing Spatial Data Services, 1.0, TestBoundaries
    EntityID(Edm.String,primaryKey)|Name(Edm.String)|Longitude(Edm.Double)|Latitude(Edm.Double)|Boundary(Edm.Geography)
    1|SanFranciscoPier|||POLYGON ((-122.389825 37.776598,-122.389438 37.773087,-122.381885 37.771849,-122.382186 37.777022,-122.389825 37.776598))

<span data-ttu-id="44ab0-133">a fenti hello következő entitást képviselik:</span><span class="sxs-lookup"><span data-stu-id="44ab0-133">hello above represents this entity:</span></span>

![](./media/notification-hubs-geofence/bing-maps-geofence.png)

<span data-ttu-id="44ab0-134">Egyszerűen másolja és illessze be a fenti hello karakterláncot egy új fájlba, és mentse a fájt **NotificationHubsGeofence.pipe**, és töltse fel az hello Bing fejlesztői központjában.</span><span class="sxs-lookup"><span data-stu-id="44ab0-134">Simply copy and paste hello string above into a new file and save it as **NotificationHubsGeofence.pipe**, and upload it in hello Bing Dev Center.</span></span>

> [!NOTE]
> <span data-ttu-id="44ab0-135">Előfordulhat, hogy felszólító toospecify hello egy új kulcsot **főkulcs** , amely nem azonos a hello **lekérdezési kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-135">You might be prompted toospecify a new key for hello **Master Key** that is different from hello **Query Key**.</span></span> <span data-ttu-id="44ab0-136">Egyszerűen hozzon létre egy új kulcsot hello irányítópult és a frissítési hello adatforrás-feltöltési oldalt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-136">Simply create a new key through hello dashboard and refresh hello data source upload page.</span></span>
> 
> 

<span data-ttu-id="44ab0-137">Hello adatfájl feltöltése után kell, hogy közzé hello adatforrás toomake.</span><span class="sxs-lookup"><span data-stu-id="44ab0-137">Once you upload hello data file, you will need toomake sure that you publish hello data source.</span></span> 

<span data-ttu-id="44ab0-138">Nyissa meg túl**adatforrások kezelése**, fentiekhez hasonló azt adta fent, hello listában keresse meg az adatforrást, és kattintson a **közzététel** a hello **műveletek** oszlop.</span><span class="sxs-lookup"><span data-stu-id="44ab0-138">Go too**Manage Data Sources**, just like we did above, find your data source in hello list and click on **Publish** in hello **Actions** column.</span></span> <span data-ttu-id="44ab0-139">Az egy kicsit kell megjelennie az adatokat az adatforrásból a hello **közzétett adatforrások** lapon:</span><span class="sxs-lookup"><span data-stu-id="44ab0-139">In a bit, you should see your data source in hello **Published Data Sources** tab:</span></span>

![](./media/notification-hubs-geofence/bing-maps-published-data.png)

<span data-ttu-id="44ab0-140">Ha **szerkesztése**, akkor azt helyeket lesz képes toosee egy pillanat alatt:</span><span class="sxs-lookup"><span data-stu-id="44ab0-140">If you click **Edit**, you will be able toosee at a glance what locations we introduced in it:</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-details.png)

<span data-ttu-id="44ab0-141">Hello portál ezen a ponton nem jeleníti meg, hello hello geokerítésen létrehozott határok – csak meg kell erősítenünk, hogy a megadott hello helyre van hello megfelelő környéken található.</span><span class="sxs-lookup"><span data-stu-id="44ab0-141">At this point, hello portal does not show you hello boundaries for hello geofence that we created – all we need is a confirmation that hello location specified is in hello right vicinity.</span></span>

<span data-ttu-id="44ab0-142">Most már rendelkezik hello adatforrás hello követelményeinek.</span><span class="sxs-lookup"><span data-stu-id="44ab0-142">Now you have all hello requirements for hello data source.</span></span> <span data-ttu-id="44ab0-143">hello kérelem URL-címen hello API-hívás, a Bing térképek fejlesztői központjához, hello tooget hello részletekért kattintson **adatforrások** válassza **az adatforrásra vonatkozó információ**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-143">tooget hello details on hello request URL for hello API call, in hello Bing Maps Dev Center, click **Data sources** and select **Data Source Information**.</span></span>

![](./media/notification-hubs-geofence/bing-maps-data-info.png)

<span data-ttu-id="44ab0-144">Hello **lekérdezés URL-címe** mi vagyunk itt után van.</span><span class="sxs-lookup"><span data-stu-id="44ab0-144">hello **Query URL** is what we’re after here.</span></span> <span data-ttu-id="44ab0-145">Ez a hello végpont, amelyre vonatkozóan végezhetünk lekérdezések toocheck e hello eszköz jelenleg hello hely határain belül, vagy nem.</span><span class="sxs-lookup"><span data-stu-id="44ab0-145">This is hello endpoint against which we can execute queries toocheck whether hello device is currently within hello boundaries of a location or not.</span></span> <span data-ttu-id="44ab0-146">Ezzel a beállítással ellenőrizheti, egyszerűen kell egy GET hívja meg hello lekérdezés URL-címe, szemben a következő paraméterek hozzáfűzésével hello tooexecute tooperform:</span><span class="sxs-lookup"><span data-stu-id="44ab0-146">tooperform this check, we simply need tooexecute a GET call against hello query URL, with hello following parameters appended:</span></span>

    ?spatialFilter=intersects(%27POINT%20LONGITUDE%20LATITUDE)%27)&$format=json&key=QUERY_KEY

<span data-ttu-id="44ab0-147">Ily módon ad meg a bázispont, hogy a Microsoft hello eszközről beszerzése és a Bing térképek automatikusan végrehajtják a hello számítások toosee hello geokerítésen belül van-e.</span><span class="sxs-lookup"><span data-stu-id="44ab0-147">That way you're specifying a target point that we obtain from hello device and Bing Maps will automatically perform hello calculations toosee whether it is within hello geofence.</span></span> <span data-ttu-id="44ab0-148">Hello kérelem böngésző (vagy a cURL) használatával hajtható végre, miután elérhetővé válik szabványos JSON-választ:</span><span class="sxs-lookup"><span data-stu-id="44ab0-148">Once you execute hello request through a browser (or cURL), you will get standard a JSON response:</span></span>

![](./media/notification-hubs-geofence/bing-maps-json.png)

<span data-ttu-id="44ab0-149">Ezt a választ csak akkor történik, ha hello pont ténylegesen hello kijelölt határain belül.</span><span class="sxs-lookup"><span data-stu-id="44ab0-149">This response only happens when hello point is actually within hello designated boundaries.</span></span> <span data-ttu-id="44ab0-150">Ha ez nem így van, az **Eredmények** gyűjtő üres lesz:</span><span class="sxs-lookup"><span data-stu-id="44ab0-150">If it is not, you will get an empty **results** bucket:</span></span>

![](./media/notification-hubs-geofence/bing-maps-nores.png)

## <a name="setting-up-hello-uwp-application"></a><span data-ttu-id="44ab0-151">Hello UWP-alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="44ab0-151">Setting up hello UWP application</span></span>
<span data-ttu-id="44ab0-152">Most, hogy hello adatforrás rendelkezésre áll, először is dolgozunk hello elkezdhetünk korábban elindított UWP-alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="44ab0-152">Now that we have hello data source ready, we can start working on hello UWP application that we bootstrapped earlier.</span></span>

<span data-ttu-id="44ab0-153">Mindenekelőtt engedélyezni kell a helyalapú szolgáltatásokat az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="44ab0-153">First and foremost, we must enable location services for our application.</span></span> <span data-ttu-id="44ab0-154">toodo e, kattintson duplán a `Package.appxmanifest` fájlt **Megoldáskezelőben**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-154">toodo this, double-click on `Package.appxmanifest` file in **Solution Explorer**.</span></span>

![](./media/notification-hubs-geofence/vs-package-manifest.png)

<span data-ttu-id="44ab0-155">Hello lapon imént megnyitott, kattintson a **képességek** és győződjön meg arról, hogy kiválassza **hely**:</span><span class="sxs-lookup"><span data-stu-id="44ab0-155">In hello package properties tab that just opened, click on **Capabilities** and make sure that you select **Location**:</span></span>

![](./media/notification-hubs-geofence/vs-package-location.png)

<span data-ttu-id="44ab0-156">Hello helymeghatározási képesség deklarálva van, mert hozzon létre egy új mappát a megoldásban nevű `Core`, és adjon hozzá egy új fájlt nevű `LocationHelper.cs`:</span><span class="sxs-lookup"><span data-stu-id="44ab0-156">As hello location capability is declared, create a new folder in your solution named `Core`, and add a new file within it, called `LocationHelper.cs`:</span></span>

![](./media/notification-hubs-geofence/vs-location-helper.png)

<span data-ttu-id="44ab0-157">Hello `LocationHelper` osztály itt meglehetősen egyszerű ezen a ponton – minden esetben van lehetővé teszik a számunkra tooobtain hello felhasználói hely hello rendszer API-n keresztül:</span><span class="sxs-lookup"><span data-stu-id="44ab0-157">hello `LocationHelper` class itself is fairly basic at this point – all it does is allow us tooobtain hello user location through hello system API:</span></span>

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

<span data-ttu-id="44ab0-158">További információkat a felhasználói helyadatok UWP-alkalmazások hello hatósági hello érheti el [MSDN-dokumentumban](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span><span class="sxs-lookup"><span data-stu-id="44ab0-158">You can read more about getting hello user’s location in UWP apps in hello official [MSDN document](https://msdn.microsoft.com/library/windows/apps/mt219698.aspx).</span></span>

<span data-ttu-id="44ab0-159">toocheck hello hely beszerzési ténylegesen működik, nyissa meg a hello kód oldalán a főoldal (`MainPage.xaml.cs`).</span><span class="sxs-lookup"><span data-stu-id="44ab0-159">toocheck that hello location acquisition is actually working, open hello code side of your main page (`MainPage.xaml.cs`).</span></span> <span data-ttu-id="44ab0-160">Hozzon létre egy új eseménykezelőt hello `Loaded` hello esemény `MainPage` konstruktor:</span><span class="sxs-lookup"><span data-stu-id="44ab0-160">Create a new event handler for hello `Loaded` event in hello `MainPage` constructor:</span></span>

    public MainPage()
    {
        this.InitializeComponent();
        this.Loaded += MainPage_Loaded;
    }

<span data-ttu-id="44ab0-161">hello hello eseménykezelő megvalósítása a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="44ab0-161">hello implementation of hello event handler is as follows:</span></span>

    private async void MainPage_Loaded(object sender, RoutedEventArgs e)
    {
        var location = await LocationHelper.GetCurrentLocation();

        if (location != null)
        {
            Debug.WriteLine(string.Concat(location.Coordinate.Longitude,
                " ", location.Coordinate.Latitude));
        }
    }

<span data-ttu-id="44ab0-162">Figyelje meg, hogy deklaráltuk hello kezelő aszinkron módon mert `GetCurrentLocation` várható, és ezért az aszinkron környezetben végrehajtott toobe igényel.</span><span class="sxs-lookup"><span data-stu-id="44ab0-162">Notice that we declared hello handler as async because `GetCurrentLocation` is awaitable, and therefore requires toobe executed in an async context.</span></span> <span data-ttu-id="44ab0-163">Továbbá mivel bizonyos körülmények kaphatunk NULL helyadatot (pl. hello helyszolgáltatás le vannak tiltva vagy hello alkalmazás megtagadva engedélyek tooaccess helyét), igazolnia kell arról, hogy megfelelő kezeléséről egy null ellenőrzéssel toomake.</span><span class="sxs-lookup"><span data-stu-id="44ab0-163">Also, because under certain circumstances we might end up with a null location (e.g. hello location services are disabled or hello application was denied permissions tooaccess location), we need toomake sure that it is properly handled with a null check.</span></span>

<span data-ttu-id="44ab0-164">Hello alkalmazás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="44ab0-164">Run hello application.</span></span> <span data-ttu-id="44ab0-165">Engedélyezze a helyadatokhoz való hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="44ab0-165">Make sure you allow location access:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-access.png)

<span data-ttu-id="44ab0-166">Egyszer hello az alkalmazás elindítása, meg kell tudni toosee hello koordinátákkal hello **kimeneti** ablakban:</span><span class="sxs-lookup"><span data-stu-id="44ab0-166">Once hello application launches, you should be able toosee hello coordinates in hello **Output** window:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-location-output.png)

<span data-ttu-id="44ab0-167">Most már tudja, hogy helyadatok beszerzésének működését látja-e az ügyfélre a Loaded szabad tooremove hello teszt-eseménykezelőjét, mert azt nem használhatja többé.</span><span class="sxs-lookup"><span data-stu-id="44ab0-167">Now you know that location acquisition works – feel free tooremove hello test event handler for Loaded because we won’t be using it anymore.</span></span>

<span data-ttu-id="44ab0-168">következő lépés hello toocapture helyadatok módosításainak.</span><span class="sxs-lookup"><span data-stu-id="44ab0-168">hello next step is toocapture location changes.</span></span> <span data-ttu-id="44ab0-169">Az adott, ugorjunk vissza toohello `LocationHelper` osztályhoz, és adja hozzá a hello eseménykezelője `PositionChanged`:</span><span class="sxs-lookup"><span data-stu-id="44ab0-169">For that, let’s go back toohello `LocationHelper` class and add hello event handler for `PositionChanged`:</span></span>

    geolocator.PositionChanged += Geolocator_PositionChanged;

<span data-ttu-id="44ab0-170">hello végrehajtására fogja megjeleníteni az hello hely koordinátái hello **kimeneti** ablakban:</span><span class="sxs-lookup"><span data-stu-id="44ab0-170">hello implementation will show hello location coordinates in hello **Output** window:</span></span>

    private static async void Geolocator_PositionChanged(Geolocator sender, PositionChangedEventArgs args)
    {
        await CoreApplication.MainView.CoreWindow.Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () =>
        {
            Debug.WriteLine(string.Concat(args.Position.Coordinate.Longitude, " ", args.Position.Coordinate.Latitude));
        });
    }

## <a name="setting-up-hello-backend"></a><span data-ttu-id="44ab0-171">Hello háttérkiszolgáló beállítása</span><span class="sxs-lookup"><span data-stu-id="44ab0-171">Setting up hello backend</span></span>
<span data-ttu-id="44ab0-172">Töltse le a hello [.NET háttérrendszer mintáját a Githubról](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="44ab0-172">Download hello [.NET Backend Sample from GitHub](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> <span data-ttu-id="44ab0-173">Hello letöltés befejezése után nyissa meg a hello `NotifyUsers` mappa, és ezt követően – hello `NotifyUsers.sln` fájlt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-173">Once hello download completes, open hello `NotifyUsers` folder, and subsequently – hello `NotifyUsers.sln` file.</span></span>

<span data-ttu-id="44ab0-174">Set hello `AppBackend` hello projekt **StartUp Project** , majd indítsa el azt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-174">Set hello `AppBackend` project as hello **StartUp Project** and launch it.</span></span>

![](./media/notification-hubs-geofence/vs-startup-project.png)

<span data-ttu-id="44ab0-175">hello projekt már konfigurált toosend leküldéses értesítések tootarget eszközöket, így toodo csak két dolgot – kell kimenő hello értesítési központ megfelelő kapcsolati karakterláncát hello felcserélése, és adja hozzá a határ azonosító toosend hello értesítési csak akkor, ha hello felhasználói hello geokerítésen belül van.</span><span class="sxs-lookup"><span data-stu-id="44ab0-175">hello project is already configured toosend push notifications tootarget devices, so we’ll need toodo only two things – swap out hello right connection string for hello notification hub and add boundary identification toosend hello notification only when hello user is within hello geofence.</span></span>

<span data-ttu-id="44ab0-176">tooconfigure hello kapcsolati karakterláncot, a hello `Models` nyissa meg a mappa `Notifications.cs`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-176">tooconfigure hello connection string, in hello `Models` folder open `Notifications.cs`.</span></span> <span data-ttu-id="44ab0-177">Hello `NotificationHubClient.CreateClientFromConnectionString` függvénynek tartalmaznia kell az értesítési központ, amely hello kaphat információt hello [Azure Portal](https://portal.azure.com) (belső hello **hozzáférési házirendek** panel az  **Beállítások**).</span><span class="sxs-lookup"><span data-stu-id="44ab0-177">hello `NotificationHubClient.CreateClientFromConnectionString` function should contain hello information about your notification hub that you can get in hello [Azure Portal](https://portal.azure.com) (look inside hello **Access Policies** blade in **Settings**).</span></span> <span data-ttu-id="44ab0-178">Mentse a hello frissített konfigurációs fájlt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-178">Save hello updated configuration file.</span></span>

<span data-ttu-id="44ab0-179">Most létre kell toocreate egy modellt a Bing térképek API-eredményéhez hello.</span><span class="sxs-lookup"><span data-stu-id="44ab0-179">Now we need toocreate a model for hello Bing Maps API result.</span></span> <span data-ttu-id="44ab0-180">Ennek legegyszerűbb módja toodo, kattintson a jobb gombbal a hello hello `Models` mappa, **Hozzáadás** > **osztály**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-180">hello easiest way toodo that is right-click on hello `Models` folder, **Add** > **Class**.</span></span> <span data-ttu-id="44ab0-181">Nevezze el a következőképpen: `GeofenceBoundary.cs`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-181">Name it `GeofenceBoundary.cs`.</span></span> <span data-ttu-id="44ab0-182">Ha végzett, hello JSON másolása és a Visual Studióban válassza hello első szakaszban tárgyalt hello API válasz **szerkesztése** > **Beillesztés** > **JSON beillesztése osztályokként**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-182">Once done, copy hello JSON from hello API response that we discussed in hello first section and in Visual Studio use **Edit** > **Paste Special** > **Paste JSON as Classes**.</span></span> 

<span data-ttu-id="44ab0-183">Így biztosítható, hogy hello az objektum deszerializálása pontosan megfelelően.</span><span class="sxs-lookup"><span data-stu-id="44ab0-183">That way we ensure that hello object will be deserialized exactly as it was intended.</span></span> <span data-ttu-id="44ab0-184">Az eredményül kapott osztály a következőhöz hasonló lesz:</span><span class="sxs-lookup"><span data-stu-id="44ab0-184">Your resulting class set should resemble this:</span></span>

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

<span data-ttu-id="44ab0-185">Ezután nyissa meg a következőt: `Controllers` > `NotificationsController.cs`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-185">Next, open `Controllers` > `NotificationsController.cs`.</span></span> <span data-ttu-id="44ab0-186">Hello cél szélességi és hosszúsági tootweak hello Post hívás tooaccount szükséges.</span><span class="sxs-lookup"><span data-stu-id="44ab0-186">We need tootweak hello Post call tooaccount for hello target longitude and latitude.</span></span> <span data-ttu-id="44ab0-187">Ehhez egyszerűen adja hozzá két karakterláncok toohello függvényaláíráshoz – `latitude` és `longitude`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-187">For that, simply add two strings toohello function signature – `latitude` and `longitude`.</span></span>

    public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag, string latitude, string longitude)

<span data-ttu-id="44ab0-188">Hozzon létre egy új osztályt nevű hello projektben `ApiHelper.cs` – ezzel fogunk tooconnect tooBing toocheck pontok határvonalainak metszéspontjait.</span><span class="sxs-lookup"><span data-stu-id="44ab0-188">Create a new class within hello project called `ApiHelper.cs` – we’ll use it tooconnect tooBing toocheck point boundary intersections.</span></span> <span data-ttu-id="44ab0-189">Adjon hozzá egy `IsPointWithinBounds` függvényt, amely a következőre hasonlít:</span><span class="sxs-lookup"><span data-stu-id="44ab0-189">Implement a `IsPointWithinBounds` function, like this:</span></span>

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
> <span data-ttu-id="44ab0-190">Győződjön meg arról, hogy toosubstitute hello API végpont hello lekérdezés URL-címet, amely korábban beszerzett hello (Ugyanez vonatkozik toohello API-kulcs) a Bing fejlesztői központjában.</span><span class="sxs-lookup"><span data-stu-id="44ab0-190">Make sure toosubstitute hello API endpoint with hello query URL that you obtained earlier from hello Bing Dev Center (same applies toohello API key).</span></span> 
> 
> 

<span data-ttu-id="44ab0-191">Eredmények toohello lekérdezés esetén a, az azt jelenti, hogy a megadott pont hello hello geokerítésen hello határain belül van, a rendszer visszaadja a `true`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-191">If there are results toohello query, that means that hello specified point is within hello boundaries of hello geofence, so we return `true`.</span></span> <span data-ttu-id="44ab0-192">Ha nem, a Bing ezzel azt jelzi, hogy a hello pont hello keresési területen, kívül esik, így a rendszer visszaadja a `false`.</span><span class="sxs-lookup"><span data-stu-id="44ab0-192">If there are no results, Bing is telling us that hello point is outside hello lookup frame, so we return `false`.</span></span>

<span data-ttu-id="44ab0-193">Vissza a `NotificationsController.cs`, hozzon létre egy ellenőrzést közvetlenül hello switch utasítás előtt:</span><span class="sxs-lookup"><span data-stu-id="44ab0-193">Back in `NotificationsController.cs`, create a check right before hello switch statement:</span></span>

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

<span data-ttu-id="44ab0-194">Ily módon hello értesítést csak küldi hello határain belül hello pont esetén.</span><span class="sxs-lookup"><span data-stu-id="44ab0-194">That way, hello notification is only sent when hello point is within hello boundaries.</span></span>

## <a name="testing-push-notifications-in-hello-uwp-app"></a><span data-ttu-id="44ab0-195">Leküldéses értesítések tesztelése UWP-alkalmazásban hello</span><span class="sxs-lookup"><span data-stu-id="44ab0-195">Testing push notifications in hello UWP app</span></span>
<span data-ttu-id="44ab0-196">Ha visszalép toohello UWP-alkalmazást, igazolnia kell tudni tootest értesítések.</span><span class="sxs-lookup"><span data-stu-id="44ab0-196">Going back toohello UWP app, we should now be able tootest notifications.</span></span> <span data-ttu-id="44ab0-197">Hello belül `LocationHelper` osztály, hozzon létre egy új függvényt – `SendLocationToBackend`:</span><span class="sxs-lookup"><span data-stu-id="44ab0-197">Within hello `LocationHelper` class, create a new function – `SendLocationToBackend`:</span></span>

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
> <span data-ttu-id="44ab0-198">A felcserélendő hello `POST_URL` hello előző szakaszban létrehozott üzembe helyezett webalkalmazás toohello helyét.</span><span class="sxs-lookup"><span data-stu-id="44ab0-198">Swap hello `POST_URL` toohello location of your deployed web application that we created in hello previous section.</span></span> <span data-ttu-id="44ab0-199">Most már OK toorun helyileg, akkor előfordulhat, hogy egy nyilvános verzió telepítéséhez, szüksége lesz a toohost, de azt egy külső szolgáltatón.</span><span class="sxs-lookup"><span data-stu-id="44ab0-199">For now, it’s OK toorun it locally, but as you work on deploying a public version, you will need toohost it with an external provider.</span></span>
> 
> 

<span data-ttu-id="44ab0-200">Most ellenőrizzük, hogy regisztráljuk hello UWP-alkalmazást a leküldéses értesítések.</span><span class="sxs-lookup"><span data-stu-id="44ab0-200">Let’s now make sure that we register hello UWP app for push notifications.</span></span> <span data-ttu-id="44ab0-201">A Visual Studióban, kattintson a **projekt** > **tárolására** > **alkalmazás társítása hello tároló**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-201">In Visual Studio, click on **Project** > **Store** > **Associate app with hello store**.</span></span>

![](./media/notification-hubs-geofence/vs-associate-with-store.png)

<span data-ttu-id="44ab0-202">Amikor bejelentkezik a fejlesztői fiókba tooyour, ellenőrizze, hogy válasszon ki egy meglévő alkalmazást, vagy hozzon létre egy újat és az hello csomag társítani.</span><span class="sxs-lookup"><span data-stu-id="44ab0-202">Once you sign in tooyour developer account, make sure you select an existing app or create a new one and associate hello package with it.</span></span> 

<span data-ttu-id="44ab0-203">Nyissa meg a toohello Dev Center webhely és a nyitott hello alkalmazást, amely az újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="44ab0-203">Go toohello Dev Center and open hello app that you just created.</span></span> <span data-ttu-id="44ab0-204">Kattintson a következőkre: **Services** (Szolgáltatások)  > **Push Notifications** (Leküldéses értesítések)  > **Live Services site** (Live Services webhely).</span><span class="sxs-lookup"><span data-stu-id="44ab0-204">Click **Services** > **Push Notifications** > **Live Services site**.</span></span>

![](./media/notification-hubs-geofence/ms-live-services.png)

<span data-ttu-id="44ab0-205">Hello webhelyen jegyezze fel a hello **Alkalmazáskulcsot** és hello **CSOMAGAZONOSÍTÓT**.</span><span class="sxs-lookup"><span data-stu-id="44ab0-205">On hello site, take note of hello **Application Secret** and hello **Package SID**.</span></span> <span data-ttu-id="44ab0-206">Szüksége lesz a egyaránt hello Azure portál – nyissa meg az értesítési központot, kattintson a **beállítások** > **értesítési szolgáltatások** > **Windows (WNS)**és adja meg a szükséges hello hello információt.</span><span class="sxs-lookup"><span data-stu-id="44ab0-206">You will need both in hello Azure Portal – open your notification hub, click on **Settings** > **Notification Services** > **Windows (WNS)** and enter hello information in hello required fields.</span></span>

![](./media/notification-hubs-geofence/notification-hubs-wns.png)

<span data-ttu-id="44ab0-207">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="44ab0-207">Click on **Save**.</span></span>

<span data-ttu-id="44ab0-208">Kattintson a jobb gombbal a **Hivatkozások** elemre a **Megoldáskezelőben**, és válassza a **NuGet-csomagok kezelése** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="44ab0-208">Right click on **References** in **Solution Explorer** and select **Manage NuGet Packages**.</span></span> <span data-ttu-id="44ab0-209">Fel kell a hivatkozás toohello tooadd **Microsoft Azure Service Bus felügyelt kódtárára** – ehhez csak keressen a `WindowsAzure.Messaging.Managed` és tooyour projekt hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="44ab0-209">We will need tooadd a reference toohello **Microsoft Azure Service Bus managed library** – simply search for `WindowsAzure.Messaging.Managed` and add it tooyour project.</span></span>

![](./media/notification-hubs-geofence/vs-nuget.png)

<span data-ttu-id="44ab0-210">Tesztelési célokra vannak, létrehozható hello `MainPage_Loaded` eseménykezelő még egyszer, és adja hozzá a kódot részlet tooit:</span><span class="sxs-lookup"><span data-stu-id="44ab0-210">For testing purposes, we can create hello `MainPage_Loaded` event handler once again, and add this code snippet tooit:</span></span>

    var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

    var hub = new NotificationHub("HUB_NAME", "HUB_LISTEN_CONNECTION_STRING");
    var result = await hub.RegisterNativeAsync(channel.Uri);

    // Displays hello registration ID so you know it was successful
    if (result.RegistrationId != null)
    {
        Debug.WriteLine("Reg successful.");
    }

<span data-ttu-id="44ab0-211">a fenti hello regisztrálja hello app hello értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="44ab0-211">hello above registers hello app with hello notification hub.</span></span> <span data-ttu-id="44ab0-212">Toogo készen áll!</span><span class="sxs-lookup"><span data-stu-id="44ab0-212">You are ready toogo!</span></span> 

<span data-ttu-id="44ab0-213">A `LocationHelper`, belül hello `Geolocator_PositionChanged` kezelőjében hozzáadhat egy tesztkódszakaszt amely hello hely hello geokerítésen belül:</span><span class="sxs-lookup"><span data-stu-id="44ab0-213">In `LocationHelper`, inside hello `Geolocator_PositionChanged` handler, you can add a piece of test code that will forcefully put hello location inside hello geofence:</span></span>

    await LocationHelper.SendLocationToBackend("wns", "TEST_USER", "TEST", "37.7746", "-122.3858");

<span data-ttu-id="44ab0-214">Jelenleg nem átadott hello valós koordinátákat (amely nem feltétlenül hello határain belül hello pillanatban), és előre meghatározott érték, mert azt egy frissítési értesítés jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="44ab0-214">Because we are not passing hello real coordinates (which might not be within hello boundaries at hello moment) and are using predefined test values, we will see a notification show up on update:</span></span>

![](./media/notification-hubs-geofence/notification-hubs-test-notification.png)

## <a name="whats-next"></a><span data-ttu-id="44ab0-215">Hogyan tovább?</span><span class="sxs-lookup"><span data-stu-id="44ab0-215">What’s next?</span></span>
<span data-ttu-id="44ab0-216">Nincsenek további lépésekre, hogy szükség lehet a fent arra, hogy a hello megoldás éles használatra kész toomake hozzáadása toohello toofollow.</span><span class="sxs-lookup"><span data-stu-id="44ab0-216">There are a couple of steps that you might need toofollow in addition toohello above toomake sure that hello solution is production-ready.</span></span>

<span data-ttu-id="44ab0-217">Mindenekelőtt szükség lehet, hogy a geokerítések dinamikus tooensure.</span><span class="sxs-lookup"><span data-stu-id="44ab0-217">First and foremost, you might need tooensure that geofences are dynamic.</span></span> <span data-ttu-id="44ab0-218">Ez fogja kell beállítani a Bing API hello rendelés toobe képes tooupload új határain belül hello meglévő adatforráson belül.</span><span class="sxs-lookup"><span data-stu-id="44ab0-218">This will require some extra work with hello Bing API in order toobe able tooupload new boundaries within hello existing data source.</span></span> <span data-ttu-id="44ab0-219">Tekintse át a hello [Bing Térbeliadat-szolgáltatási API dokumentációjában](https://msdn.microsoft.com/library/ff701734.aspx) hello tulajdonos olvashat.</span><span class="sxs-lookup"><span data-stu-id="44ab0-219">Consult hello [Bing Spatial Data Services API documentation](https://msdn.microsoft.com/library/ff701734.aspx) for more details on hello subject.</span></span>

<span data-ttu-id="44ab0-220">Második, vagy sem, hogy a hello kézbesítési történik-e a megfelelő személyek toohello működő tooensure, érdemes lehet tootarget őket keresztül [címkézés](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="44ab0-220">Second, as you are working tooensure that hello delivery is done toohello right participants, you might want tootarget them via [tagging](notification-hubs-tags-segment-push-message.md).</span></span>

<span data-ttu-id="44ab0-221">a fenti hello megoldás egy olyan forgatókönyvet, ahol lehetséges, hogy sokféle célplatform lehetséges, így azt adta korlátozza hello geokerítések toosystem-specifikus képességek ismerteti.</span><span class="sxs-lookup"><span data-stu-id="44ab0-221">hello solution shown above describes a scenario in which you might have a wide variety of target platforms, so we did not limit hello geofencing toosystem-specific capabilities.</span></span> <span data-ttu-id="44ab0-222">Említett, az univerzális Windows Platform hello túl képességeket biztosít[észleli a geokerítések jobb out-of-az-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span><span class="sxs-lookup"><span data-stu-id="44ab0-222">That said, hello Universal Windows Platform offers capabilities too[detect geofences right out-of-the-box](https://msdn.microsoft.com/windows/uwp/maps-and-location/set-up-a-geofence).</span></span>

<span data-ttu-id="44ab0-223">A Notification Hubs képességeiről a [dokumentációs portálon](https://azure.microsoft.com/documentation/services/notification-hubs/) talál további részleteket.</span><span class="sxs-lookup"><span data-stu-id="44ab0-223">For more details regarding Notification Hubs capabilities, check out our [documentation portal](https://azure.microsoft.com/documentation/services/notification-hubs/).</span></span>

