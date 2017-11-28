---
title: "aaaUse Notification Hubs toosend legfrissebb hírek (Windows Phone)"
description: "Azure Notification Hubs toouse címke regisztrációk toosend megtörje hírek tooa Windows Phone-alkalmazás használja."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3519a8701105f88198afe288e59e9204420234db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="210dc-103">Használja a Notification Hubs toosend legfrissebb hírek</span><span class="sxs-lookup"><span data-stu-id="210dc-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="210dc-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="210dc-104">Overview</span></span>
<span data-ttu-id="210dc-105">Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast legfrissebb hírek értesítések tooa Windows Phone 8.0/8.1 Silverlight-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="210dc-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Phone 8.0/8.1 Silverlight app.</span></span> <span data-ttu-id="210dc-106">Ha a Windows Áruházbeli és Windows Phone 8.1-alkalmazást céloz meg, tekintse meg a tootoohello [univerzális Windows-](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verziója.</span><span class="sxs-lookup"><span data-stu-id="210dc-106">If you are targeting Windows Store or Windows Phone 8.1 app, please refer tootoohello [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span></span> <span data-ttu-id="210dc-107">Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához.</span><span class="sxs-lookup"><span data-stu-id="210dc-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="210dc-108">Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.</span><span class="sxs-lookup"><span data-stu-id="210dc-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="210dc-109">Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="210dc-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="210dc-110">Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="210dc-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="210dc-111">Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe.</span><span class="sxs-lookup"><span data-stu-id="210dc-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="210dc-112">Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="210dc-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="210dc-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="210dc-113">Prerequisites</span></span>
<span data-ttu-id="210dc-114">Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="210dc-114">This topic builds on hello app you created in [Get started with Notification Hubs].</span></span> <span data-ttu-id="210dc-115">Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="210dc-115">Before starting this tutorial, you must have already completed [Get started with Notification Hubs].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="210dc-116">Kategória kiválasztása toohello alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="210dc-116">Add category selection toohello app</span></span>
<span data-ttu-id="210dc-117">első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő lapján, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister.</span><span class="sxs-lookup"><span data-stu-id="210dc-117">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="210dc-118">felhasználó által kijelölt hello kategóriák hello eszközön tárolja.</span><span class="sxs-lookup"><span data-stu-id="210dc-118">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="210dc-119">Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.</span><span class="sxs-lookup"><span data-stu-id="210dc-119">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="210dc-120">Nyissa meg a hello MainPage.xaml projektfájlt, majd cserélje le a hello **rács** nevű elem `TitlePanel` és `ContentPanel` a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="210dc-120">Open hello MainPage.xaml project file, then replace hello **Grid** elements named `TitlePanel` and `ContentPanel` with hello following code:</span></span>
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. <span data-ttu-id="210dc-121">Hello projektben, hozzon létre egy új osztályt **értesítések**, vegye fel a hello **nyilvános** módosító toohello definition osztályt, majd adja hozzá a következő hello **használatával** utasítások Új kód fájl toohello:</span><span class="sxs-lookup"><span data-stu-id="210dc-121">In hello project, create a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. <span data-ttu-id="210dc-122">Másolás hello következő hello új kódot **értesítések** osztály:</span><span class="sxs-lookup"><span data-stu-id="210dc-122">Copy hello following code into hello new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        // Registration task toocomplete registration in hello ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used tooreceive notifications while hello app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete hello registrationTask here.  
            // If it is null, hello registrationTask will be completed in hello ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // hello stored categories tags are passed with hello template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used tooreceive notifications while hello app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out hello information that was part of hello message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all hello fields in hello toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    <span data-ttu-id="210dc-123">Ez az osztály elkülönített hello tárolási toostore hello kategóriáinak híreket, hogy ez az eszköz megfelel-e tooreceive használja.</span><span class="sxs-lookup"><span data-stu-id="210dc-123">This class uses hello isolated storage toostore hello categories of news that this device is tooreceive.</span></span> <span data-ttu-id="210dc-124">Módszerek tooregister ezen kategóriák használatával is tartalmaz egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítési regisztrációt.</span><span class="sxs-lookup"><span data-stu-id="210dc-124">It also contains methods tooregister for these categories using a [template](notification-hubs-templates-cross-platform-push-messages.md) notification registration.</span></span>


1. <span data-ttu-id="210dc-125">Hello App.xaml.cs projektfájlt, adja hozzá a következő tulajdonság toohello hello **App** osztály.</span><span class="sxs-lookup"><span data-stu-id="210dc-125">In hello App.xaml.cs project file, add hello following property toohello **App** class.</span></span> <span data-ttu-id="210dc-126">Cserélje le a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett.</span><span class="sxs-lookup"><span data-stu-id="210dc-126">Replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > <span data-ttu-id="210dc-127">Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="210dc-127">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="210dc-128">Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el.</span><span class="sxs-lookup"><span data-stu-id="210dc-128">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="210dc-129">hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.</span><span class="sxs-lookup"><span data-stu-id="210dc-129">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
2. <span data-ttu-id="210dc-130">A MainPage.xaml.cs adja hozzá a következő sor hello:</span><span class="sxs-lookup"><span data-stu-id="210dc-130">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
3. <span data-ttu-id="210dc-131">Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő metódus hello:</span><span class="sxs-lookup"><span data-stu-id="210dc-131">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    <span data-ttu-id="210dc-132">Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="210dc-132">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="210dc-133">Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.</span><span class="sxs-lookup"><span data-stu-id="210dc-133">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="210dc-134">Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.</span><span class="sxs-lookup"><span data-stu-id="210dc-134">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="210dc-135">Az értesítések regisztrálása</span><span class="sxs-lookup"><span data-stu-id="210dc-135">Register for notifications</span></span>
<span data-ttu-id="210dc-136">Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.</span><span class="sxs-lookup"><span data-stu-id="210dc-136">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="210dc-137">Mivel hello csatorna URI Azonosítóját, a Microsoft leküldéses értesítési szolgáltatásának (MPNS) hello által hozzárendelt bármikor módosíthatja, regisztrálnia kell az értesítések gyakran tooavoid értesítés sikertelen.</span><span class="sxs-lookup"><span data-stu-id="210dc-137">Because hello channel URI assigned by hello Microsoft Push Notification Service (MPNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="210dc-138">Ez a példa regisztrálja az értesítések hello alkalmazás minden indításakor.</span><span class="sxs-lookup"><span data-stu-id="210dc-138">This example registers for notifications every time that hello app starts.</span></span> <span data-ttu-id="210dc-139">Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.</span><span class="sxs-lookup"><span data-stu-id="210dc-139">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="210dc-140">Nyissa meg a hello App.xaml.cs fájlt, és adja hozzá a hello **aszinkron** módosító túl**Application_Launching** metódus és a név felülírandó hello Notification Hubs regisztrációs kódot, amelyet a [Ismerkedés a Notification Hubs] a hello a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="210dc-140">Open hello App.xaml.cs file and add hello **async** modifier too**Application_Launching** method and replace hello Notification Hubs registration code that you added in [Get started with Notification Hubs] with hello following code:</span></span>
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    <span data-ttu-id="210dc-141">Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri ezek kategóriák regisztráció.</span><span class="sxs-lookup"><span data-stu-id="210dc-141">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registration for these categories.</span></span>
2. <span data-ttu-id="210dc-142">Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő kódot, amely megvalósítja az hello hello **OnNavigatedTo** módszert:</span><span class="sxs-lookup"><span data-stu-id="210dc-142">In hello MainPage.xaml.cs project file, add hello following code that implements hello **OnNavigatedTo** method:</span></span>
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    <span data-ttu-id="210dc-143">A frissítések hello főoldala hello állapota alapján korábban mentett kategóriák.</span><span class="sxs-lookup"><span data-stu-id="210dc-143">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="210dc-144">hello alkalmazás kész, és képes tárolni a kategóriák hello eszköz használt helyi tárhely tooregister hello értesítési központban az amikor hello felhasználót módosítások hello kategóriák kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="210dc-144">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="210dc-145">A következő kategória értesítések toothis app küldő háttérkiszolgálón meghatározzák azt.</span><span class="sxs-lookup"><span data-stu-id="210dc-145">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="210dc-146">Címkézett értesítések küldése</span><span class="sxs-lookup"><span data-stu-id="210dc-146">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="210dc-147">Hello alkalmazás futtatását, és értesítések</span><span class="sxs-lookup"><span data-stu-id="210dc-147">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="210dc-148">A Visual Studio nyomja le az F5 toocompile és hello alkalmazás indításához.</span><span class="sxs-lookup"><span data-stu-id="210dc-148">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="210dc-149">Megjegyzés: a felhasználói felület számos hello alkalmazást, amely váltja kiválaszthatja hello kategóriák toosubscribe számára.</span><span class="sxs-lookup"><span data-stu-id="210dc-149">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="210dc-150">Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="210dc-150">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="210dc-151">hello app hello kiválasztott kategóriák alakítja címkék, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket.</span><span class="sxs-lookup"><span data-stu-id="210dc-151">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="210dc-152">hello regisztrált kategóriák adott vissza, és megjelenik egy párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="210dc-152">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][2]
3. <span data-ttu-id="210dc-153">Azután, hogy megérkezett igazolja, hogy van-e előfizetés befejezte a kategóriák, futtassa a hello konzol app toosend értesítések az egyes kategóriák.</span><span class="sxs-lookup"><span data-stu-id="210dc-153">After receiving a confirmation that your categories were subscription completed, run hello console app toosend notifications for each categories.</span></span> <span data-ttu-id="210dc-154">Győződjön meg arról, hogy csak az előfizetett hello kategóriák értesítést kap.</span><span class="sxs-lookup"><span data-stu-id="210dc-154">Verify you only receive a notification for hello categories you have subscribed to.</span></span>
   
    ![][3]

<span data-ttu-id="210dc-155">Ez a témakör befejeződött.</span><span class="sxs-lookup"><span data-stu-id="210dc-155">You have completed this topic.</span></span>

<!--##Next steps

In this tutorial we learned how toobroadcast breaking news by category. Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs toobroadcast localized breaking news]

    Learn how tooexpand hello breaking news app tooenable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how toopush notifications toospecific authenticated users. This is a good solution for sending notifications only toospecific users.
-->

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Ismerkedés a Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs toobroadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Phone]: ??

