---
title: "aaaUse Notification Hubs toosend legfrissebb hírek (univerzális Windows)"
description: "Azure Notification Hubs használata címkék hello regisztrációs toosend megtörje hírek tooa univerzális Windows-alkalmazást."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f102d286d2c7bd387fcfa2c7eab2ba31a0298517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="e77c3-103">Használja a Notification Hubs toosend legfrissebb hírek</span><span class="sxs-lookup"><span data-stu-id="e77c3-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="e77c3-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="e77c3-104">Overview</span></span>
<span data-ttu-id="e77c3-105">Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast friss hírek értesítések tooa Windows áruház vagy a Windows Phone 8.1 (nem Silverlight) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="e77c3-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Store or Windows Phone 8.1 (non-Silverlight) app.</span></span> <span data-ttu-id="e77c3-106">Ha a Windows Phone 8.1 Silverlight céloz meg, tekintse meg a toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verziója.</span><span class="sxs-lookup"><span data-stu-id="e77c3-106">If you are targeting Windows Phone 8.1 Silverlight, please refer toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version.</span></span> <span data-ttu-id="e77c3-107">Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="e77c3-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="e77c3-108">Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely azokat, pl. RSS-olvasóval, zene ventilátorok alkalmazások érdeklődik elemnek már deklarálva, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="e77c3-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, and so on.</span></span> 

<span data-ttu-id="e77c3-109">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="e77c3-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="e77c3-110">Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="e77c3-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="e77c3-111">Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe.</span><span class="sxs-lookup"><span data-stu-id="e77c3-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="e77c3-112">Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="e77c3-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e77c3-113">Windows áruház és Windows Phone-projektek verzió 8.1 és korábbi verziók nem támogatottak a Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e77c3-113">Windows Store and Windows Phone projects version 8.1 and earlier are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="e77c3-114">További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span><span class="sxs-lookup"><span data-stu-id="e77c3-114">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e77c3-115">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e77c3-115">Prerequisites</span></span>
<span data-ttu-id="e77c3-116">Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="e77c3-116">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="e77c3-117">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="e77c3-117">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="e77c3-118">Kategória kiválasztása toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="e77c3-118">Add category selection toohello app</span></span>
<span data-ttu-id="e77c3-119">első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő lapján, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister.</span><span class="sxs-lookup"><span data-stu-id="e77c3-119">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="e77c3-120">felhasználó által kijelölt hello kategóriák hello eszközön tárolja.</span><span class="sxs-lookup"><span data-stu-id="e77c3-120">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="e77c3-121">Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.</span><span class="sxs-lookup"><span data-stu-id="e77c3-121">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="e77c3-122">Nyissa meg a hello MainPage.xaml projektfájlt, majd a következő kódot a hello másolási hello **rács** elem:</span><span class="sxs-lookup"><span data-stu-id="e77c3-122">Open hello MainPage.xaml project file, then copy hello following code in hello **Grid** element:</span></span>
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>
2. <span data-ttu-id="e77c3-123">Kattintson jobb gombbal a hello **megosztott** projektre, majd adja hozzá egy új osztályt **értesítések**, vegye fel a hello **nyilvános** módosító toohello osztály definícióját, és vegye fel a következő hello **használatával** utasítások toohello új kódfájl:</span><span class="sxs-lookup"><span data-stu-id="e77c3-123">Right click hello **Shared** project and add a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. <span data-ttu-id="e77c3-124">Másolás hello következő hello új kódot **értesítések** osztály:</span><span class="sxs-lookup"><span data-stu-id="e77c3-124">Copy hello following code into hello new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    <span data-ttu-id="e77c3-125">Ez az osztály hello helyi tároló toostore hello kategóriáinak híreket, hogy az eszköz rendelkezik-e tooreceive használja.</span><span class="sxs-lookup"><span data-stu-id="e77c3-125">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="e77c3-126">Vegye figyelembe, hogy a hívó hello helyett *RegisterNativeAsync* metódus hívása *RegisterTemplateAsync* tooregister hello kategóriákban sablon regisztráció használatával.</span><span class="sxs-lookup"><span data-stu-id="e77c3-126">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync* tooregister for hello categories using a template registration.</span></span> 
   
    <span data-ttu-id="e77c3-127">Azt is nevezze el a hello sablon ("simpleWNSTemplateExample"), mert célszerű lehet tooregister egynél több sablont (például egy bejelentési értesítést) és egy a csempéket, ezért ellenőriznünk kell, hogy tooname azokat sorrendben toobe képes tooupdate vagy törölje őket.</span><span class="sxs-lookup"><span data-stu-id="e77c3-127">We also provide a name for hello template ("simpleWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="e77c3-128">Vegye figyelembe, hogy ha egy eszköz több sablon regisztrálja azonos címke, a célcsoport-kezelési címke eredményez bejövő üzenet hello több értesítés is érkezett (egy mindegyik sablon) toohello eszköz kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="e77c3-128">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="e77c3-129">Ez a viselkedés akkor hasznos, ha hello azonos logikai üzenetnek tooresult több vizuális értesítések, például a Windows Áruházbeli alkalmazások megjelenítő egy jelvény és egy bejelentési is.</span><span class="sxs-lookup"><span data-stu-id="e77c3-129">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
   
    <span data-ttu-id="e77c3-130">A sablonok további információkért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="e77c3-130">For more information on templates, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>
4. <span data-ttu-id="e77c3-131">Hello App.xaml.cs projektfájlt, adja hozzá a következő tulajdonság toohello hello **App** osztály:</span><span class="sxs-lookup"><span data-stu-id="e77c3-131">In hello App.xaml.cs project file, add hello following property toohello **App** class:</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    <span data-ttu-id="e77c3-132">Ez a tulajdonság akkor használt toocreate és a hozzáférés a **értesítések** példány.</span><span class="sxs-lookup"><span data-stu-id="e77c3-132">This property is used toocreate and access a **Notifications** instance.</span></span>
   
    <span data-ttu-id="e77c3-133">A fenti kódot hello, cserélje le a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="e77c3-133">In hello above code, replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="e77c3-134">Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="e77c3-134">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="e77c3-135">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="e77c3-135">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="e77c3-136">hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.</span><span class="sxs-lookup"><span data-stu-id="e77c3-136">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
5. <span data-ttu-id="e77c3-137">A MainPage.xaml.cs adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="e77c3-137">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
6. <span data-ttu-id="e77c3-138">Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="e77c3-138">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    <span data-ttu-id="e77c3-139">Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="e77c3-139">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="e77c3-140">Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.</span><span class="sxs-lookup"><span data-stu-id="e77c3-140">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="e77c3-141">Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="e77c3-141">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="e77c3-142">Az értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="e77c3-142">Register for notifications</span></span>
<span data-ttu-id="e77c3-143">Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="e77c3-143">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="e77c3-144">Mivel hello csatorna URI Azonosítóját, a Windows értesítési szolgáltatása (WNS) hello által hozzárendelt bármikor módosíthatja, regisztrálnia kell az értesítések gyakran tooavoid értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="e77c3-144">Because hello channel URI assigned by hello Windows Notification Service (WNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="e77c3-145">Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="e77c3-145">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="e77c3-146">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="e77c3-146">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="e77c3-147">Nyissa meg hello App.xaml.cs fájlt, és a frissítés hello **InitNotificationsAsync** metódus toouse hello `notifications` osztály toosubscribe kategóriák alapján.</span><span class="sxs-lookup"><span data-stu-id="e77c3-147">Open hello App.xaml.cs file and update hello **InitNotificationsAsync** method toouse hello `notifications` class toosubscribe based on categories.</span></span>
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    <span data-ttu-id="e77c3-148">Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri a regisztrálás ezen kategóriák.</span><span class="sxs-lookup"><span data-stu-id="e77c3-148">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> <span data-ttu-id="e77c3-149">Hello **InitNotificationsAsync** metódus hello részeként létrejött [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="e77c3-149">hello **InitNotificationsAsync** method was created as part of hello [Get started with Notification Hubs][get-started] tutorial.</span></span>
2. <span data-ttu-id="e77c3-150">Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő kód toohello hello *OnNavigatedTo* módszert:</span><span class="sxs-lookup"><span data-stu-id="e77c3-150">In hello MainPage.xaml.cs project file, add hello following code toohello *OnNavigatedTo* method:</span></span>
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    <span data-ttu-id="e77c3-151">A frissítések hello főoldala hello állapota alapján korábban mentett kategóriák.</span><span class="sxs-lookup"><span data-stu-id="e77c3-151">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="e77c3-152">hello alkalmazás kész, és képes tárolni a kategóriák hello eszköz használt helyi tárhely tooregister hello értesítési központban az amikor hello felhasználót módosítások hello kategóriák kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="e77c3-152">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="e77c3-153">A következő kategória értesítések toothis app küldő háttérkiszolgálón meghatározzák azt.</span><span class="sxs-lookup"><span data-stu-id="e77c3-153">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="e77c3-154">Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="e77c3-154">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="e77c3-155">Hello alkalmazás futtatását, és értesítések</span><span class="sxs-lookup"><span data-stu-id="e77c3-155">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="e77c3-156">A Visual Studio nyomja le az F5 toocompile és hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="e77c3-156">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="e77c3-157">Megjegyzés: a felhasználói felület számos hello alkalmazást, amely váltja kiválaszthatja hello kategóriák toosubscribe számára.</span><span class="sxs-lookup"><span data-stu-id="e77c3-157">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="e77c3-158">Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="e77c3-158">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="e77c3-159">hello app hello kiválasztott kategóriák alakítja címkék, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="e77c3-159">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="e77c3-160">hello regisztrált kategóriák adott vissza, és megjelenik egy párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="e77c3-160">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][19]
3. <span data-ttu-id="e77c3-161">Új értesítés küldése a következő módokon hello egyikében hello háttérrendszerből:</span><span class="sxs-lookup"><span data-stu-id="e77c3-161">Send a new notification from hello backend in one of hello following ways:</span></span>
   
   * <span data-ttu-id="e77c3-162">**Konzolalkalmazás:** hello konzol alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="e77c3-162">**Console app:** start hello console app.</span></span>
   * <span data-ttu-id="e77c3-163">**Java/php-ből:** az alkalmazást, a parancsfájl futtatásához.</span><span class="sxs-lookup"><span data-stu-id="e77c3-163">**Java/PHP:** run your app/script.</span></span>
     
     <span data-ttu-id="e77c3-164">Értesítések a kiválasztott hello kategóriák bejelentési értesítések jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="e77c3-164">Notifications for hello selected categories appear as toast notifications.</span></span>
     
     ![][14]

## <a name="next-steps"></a><span data-ttu-id="e77c3-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e77c3-165">Next steps</span></span>
<span data-ttu-id="e77c3-166">Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint.</span><span class="sxs-lookup"><span data-stu-id="e77c3-166">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="e77c3-167">Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:</span><span class="sxs-lookup"><span data-stu-id="e77c3-167">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="e77c3-168">[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]</span><span class="sxs-lookup"><span data-stu-id="e77c3-168">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="e77c3-169">Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.</span><span class="sxs-lookup"><span data-stu-id="e77c3-169">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
