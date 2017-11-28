---
title: "aaaNotification hubok honosított Megtörje hírek oktatóanyag"
description: "Ismerje meg, hogyan toouse Azure Notification Hubs toosend honosított legfrissebb híreket tartalmazó értesítések."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d273a6b384df311dea7b76ca83ccd94d9a989c4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a><span data-ttu-id="794f4-103">Használjon honosított toosend Notification Hubs – legfrissebb hírek</span><span class="sxs-lookup"><span data-stu-id="794f4-103">Use Notification Hubs toosend localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="794f4-104">Windows áruház C#</span><span class="sxs-lookup"><span data-stu-id="794f4-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="794f4-105">iOS</span><span class="sxs-lookup"><span data-stu-id="794f4-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="794f4-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="794f4-106">Overview</span></span>
<span data-ttu-id="794f4-107">Ez a témakör bemutatja, hogyan toouse hello **sablon** az Azure Notification Hubs toobroadcast nyelv és az eszköz honosított katalóguselemekről híreket tartalmazó értesítések megtörje szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="794f4-107">This topic shows you how toouse hello **template** feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="794f4-108">Ez az oktatóanyag a kiindulási pont hello Windows Áruházbeli alkalmazás létrehozott [legfrissebb hírek Notification Hubs használata toosend].</span><span class="sxs-lookup"><span data-stu-id="794f4-108">In this tutorial you start with hello Windows Store app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="794f4-109">Amikor végzett, képes tooregister érdekli kategóriákban fogja, adjon meg egy nyelvi mely tooreceive hello értesítések, és csak a kiválasztott hello kategóriák leküldéses értesítések fogadásához az adott nyelveken.</span><span class="sxs-lookup"><span data-stu-id="794f4-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="794f4-110">Nincsenek két részből toothis forgatókönyv:</span><span class="sxs-lookup"><span data-stu-id="794f4-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="794f4-111">hello Windows Áruházbeli alkalmazás lehetővé teszi, hogy ügyfél eszközök toospecify egy nyelvet, és toosubscribe toodifferent megtörje hírek kategóriák;</span><span class="sxs-lookup"><span data-stu-id="794f4-111">hello Windows Store app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="794f4-112">hello háttér-közzéteszi hello értesítéseket, hello **címke** és **sablon** az Azure Notification Hubs feautres.</span><span class="sxs-lookup"><span data-stu-id="794f4-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="794f4-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="794f4-113">Prerequisites</span></span>
<span data-ttu-id="794f4-114">Már végrehajtotta hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag és hello kód érhető el, mert ez az oktatóanyag közvetlenül épít, hogy a kód.</span><span class="sxs-lookup"><span data-stu-id="794f4-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="794f4-115">Szükség a Visual Studio 2012 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="794f4-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="794f4-116">Sablon fogalmak</span><span class="sxs-lookup"><span data-stu-id="794f4-116">Template concepts</span></span>
<span data-ttu-id="794f4-117">A [legfrissebb hírek Notification Hubs használata toosend] olyan alkalmazás, amelynek használt parancsfájlkezelő **címkék** toosubscribe toonotifications hírek különböző kategóriákban.</span><span class="sxs-lookup"><span data-stu-id="794f4-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="794f4-118">Számos alkalmazás, azonban több piacok célként, és honosítási igényelnek.</span><span class="sxs-lookup"><span data-stu-id="794f4-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="794f4-119">Ez azt jelenti, hogy maguk hello értesítések hello tartalmának rendelkezik honosított toobe kézbesített toohello, javítsa ki az eszközök.</span><span class="sxs-lookup"><span data-stu-id="794f4-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="794f4-120">Ebben a témakörben bemutatjuk a hogyan toouse hello **sablon** tooeasily biztosítanak a Notification Hubs szolgáltatása honosított legfrissebb híreket tartalmazó értesítések.</span><span class="sxs-lookup"><span data-stu-id="794f4-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="794f4-121">Megjegyzés: egyirányú toosend honosított értesítések toocreate minden címke több verziója van.</span><span class="sxs-lookup"><span data-stu-id="794f4-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="794f4-122">Például toosupport angol, francia és Mandarin, lenne szükséges három különböző címkék híreket: "world_en", "world_fr" és "world_ch".</span><span class="sxs-lookup"><span data-stu-id="794f4-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="794f4-123">A Microsoft majd kellene toosend hello world hírek tooeach címkéket honosított verzióját.</span><span class="sxs-lookup"><span data-stu-id="794f4-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="794f4-124">Ez a témakör használjuk, a címkéket a sablonok tooavoid hello elterjedése és hello követelmény több üzenetet küldeni.</span><span class="sxs-lookup"><span data-stu-id="794f4-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="794f4-125">Magas szinten, a sablonok olyan módon toospecify hogyan egy adott eszközhöz egy értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="794f4-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="794f4-126">hello sablon hello pontos az adattartalom formátuma hello által küldött üzenet az alkalmazás háttér-részét képező tooproperties alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="794f4-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="794f4-127">Ebben az esetben az összes támogatott nyelvek tartalmazó területibeállítás-független üzenetet küldünk:</span><span class="sxs-lookup"><span data-stu-id="794f4-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="794f4-128">Ezután azt fogja győződjön meg arról, hogy az eszközök regisztrálása a sablont, amely hivatkozik a toohello megfelelő tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="794f4-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="794f4-129">Például egy Windows Áruházbeli alkalmazást, amely tooreceive szeretne egy egyszerű bejelentési üzenet regisztrálja a sablont a megfelelő címkéket a következő hello:</span><span class="sxs-lookup"><span data-stu-id="794f4-129">For instance, a Windows Store app that wants tooreceive a simple toast message will register for hello following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="794f4-130">Sablonok egyik újdonsága nagyon hatékony többet is megtudhat arról a a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="794f4-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="hello-app-user-interface"></a><span data-ttu-id="794f4-131">hello alkalmazás felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="794f4-131">hello app user interface</span></span>
<span data-ttu-id="794f4-132">Most módosítja, a Microsoft hello Megtörje hírek app hello a témakör a [legfrissebb hírek Notification Hubs használata toosend] toosend honosított legfrissebb hírek sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="794f4-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="794f4-133">A Windows Áruházbeli alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="794f4-133">In your Windows Store app:</span></span>

<span data-ttu-id="794f4-134">A MainPage.xaml tooinclude a területi beállítás combobox módosítása:</span><span class="sxs-lookup"><span data-stu-id="794f4-134">Change your MainPage.xaml tooinclude a locale combobox:</span></span>

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-hello-windows-store-client-app"></a><span data-ttu-id="794f4-135">Hello Windows Store ügyfél alkalmazás elkészítése</span><span class="sxs-lookup"><span data-stu-id="794f4-135">Building hello Windows Store client app</span></span>
1. <span data-ttu-id="794f4-136">Az értesítések osztályban adja hozzá a nyelv paraméter tooyour *StoreCategoriesAndSubscribe* és *SubscribeToCateories* módszerek.</span><span class="sxs-lookup"><span data-stu-id="794f4-136">In your Notifications class, add a locale parameter tooyour  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using hello localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    <span data-ttu-id="794f4-137">Vegye figyelembe, hogy a hívó hello helyett *RegisterNativeAsync* metódus hívása *RegisterTemplateAsync*: egy adott értesítés formátuma, mely hello sablon függ hello területi beállítás regisztrálja azt.</span><span class="sxs-lookup"><span data-stu-id="794f4-137">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which hello template depends on hello locale.</span></span> <span data-ttu-id="794f4-138">Azt is nevezze el a hello sablon ("localizedWNSTemplateExample"), mert célszerű lehet tooregister egynél több sablont (például egy bejelentési értesítést) és egy a csempéket, ezért ellenőriznünk kell, hogy tooname azokat sorrendben toobe képes tooupdate vagy törölje őket.</span><span class="sxs-lookup"><span data-stu-id="794f4-138">We also provide a name for hello template ("localizedWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="794f4-139">Vegye figyelembe, hogy ha egy eszköz több sablon regisztrálja azonos címke, a célcsoport-kezelési címke eredményez bejövő üzenet hello több értesítés is érkezett (egy mindegyik sablon) toohello eszköz kézbesíteni.</span><span class="sxs-lookup"><span data-stu-id="794f4-139">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="794f4-140">Ez a viselkedés akkor hasznos, ha hello azonos logikai üzenetnek tooresult több vizuális értesítések, például a Windows Áruházbeli alkalmazások megjelenítő egy jelvény és egy bejelentési is.</span><span class="sxs-lookup"><span data-stu-id="794f4-140">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="794f4-141">Adja hozzá a következő metódus tooretrieve hello tárolt területi hello:</span><span class="sxs-lookup"><span data-stu-id="794f4-141">Add hello following method tooretrieve hello stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="794f4-142">A MainPage.xaml.cs, frissítse a gomb kezelő kattintson beolvasása hello hello területi kombinált lista aktuális értéke, és azt toohello hívás toohello értesítések osztály megadásával látható módon:</span><span class="sxs-lookup"><span data-stu-id="794f4-142">In your MainPage.xaml.cs, update your button click handler by retrieving hello current value of hello Locale combo box and providing it toohello call toohello Notifications class, as shown:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. <span data-ttu-id="794f4-143">Végül az App.xaml.cs fájlban győződjön meg arról, hogy tooupdate a `InitNotificationsAsync` metódus tooretrieve hello területi beállítás, és használatra, ha feliratkozik:</span><span class="sxs-lookup"><span data-stu-id="794f4-143">Finally, in your App.xaml.cs file, make sure tooupdate your `InitNotificationsAsync` method tooretrieve hello locale and use it when subscribing:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="794f4-144">Honosított értesítések küldése a háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="794f4-144">Send localized notifications from your back-end</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[hello app user interface]: #ui
[Building hello Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[legfrissebb hírek Notification Hubs használata toosend]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
