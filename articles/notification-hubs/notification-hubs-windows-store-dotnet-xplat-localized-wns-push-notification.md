---
title: "A Notification Hubs honosított Breaking News oktatóanyag"
description: "Megtudhatja, hogyan használható az Azure Notification Hubs honosított legfrissebb híreket tartalmazó értesítések küldése."
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
ms.openlocfilehash: e864e832b4c50644bf4062dee29d34ff9fe2774e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a><span data-ttu-id="d9559-103">Honosított legfrissebb hírek elküldése a Notification Hubs használatával</span><span class="sxs-lookup"><span data-stu-id="d9559-103">Use Notification Hubs to send localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d9559-104">Windows áruház C#</span><span class="sxs-lookup"><span data-stu-id="d9559-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="d9559-105">iOS</span><span class="sxs-lookup"><span data-stu-id="d9559-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="d9559-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d9559-106">Overview</span></span>
<span data-ttu-id="d9559-107">Ez a témakör bemutatja, hogyan használható a **sablon** az Azure Notification Hubs – legfrissebb híreket tartalmazó értesítések nyelv és az eszköz honosított katalóguselemekről adás szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="d9559-107">This topic shows you how to use the **template** feature of Azure Notification Hubs to broadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="d9559-108">Ez az oktatóanyag a kiindulási pont a Windows Áruházbeli alkalmazásban létrehozott [legfrissebb hírek küldése Notification Hubs használata].</span><span class="sxs-lookup"><span data-stu-id="d9559-108">In this tutorial you start with the Windows Store app created in [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="d9559-109">Amikor végzett, lesz kategóriák szeretné regisztrálni, adja meg a nyelvet, amelyen az értesítéseket, és csak a kiválasztott kategóriákra leküldéses értesítéseket kapni az adott nyelveken.</span><span class="sxs-lookup"><span data-stu-id="d9559-109">When complete, you will be able to register for categories you are interested in, specify a language in which to receive the notifications, and receive only push notifications for the selected categories in that language.</span></span>

<span data-ttu-id="d9559-110">Ez a forgatókönyv két részből áll:</span><span class="sxs-lookup"><span data-stu-id="d9559-110">There are two parts to this scenario:</span></span>

* <span data-ttu-id="d9559-111">a Windows Áruházbeli alkalmazás tesz lehetővé az nyelv megadása, és fizessen elő a különböző breaking news kategóriák;</span><span class="sxs-lookup"><span data-stu-id="d9559-111">the Windows Store app allows client devices to specify a language, and to subscribe to different breaking news categories;</span></span>
* <span data-ttu-id="d9559-112">a háttér-közzéteszi az értesítéseket, a **címke** és **sablon** az Azure Notification Hubs feautres.</span><span class="sxs-lookup"><span data-stu-id="d9559-112">the back-end broadcasts the notifications, using the **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d9559-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d9559-113">Prerequisites</span></span>
<span data-ttu-id="d9559-114">Már végrehajtotta a [legfrissebb hírek küldése Notification Hubs használata] oktatóanyagot, és a kód érhető el, mert ez az oktatóanyag közvetlenül épít, hogy a kód.</span><span class="sxs-lookup"><span data-stu-id="d9559-114">You must have already completed the [Use Notification Hubs to send breaking news] tutorial and have the code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="d9559-115">Szükség a Visual Studio 2012 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d9559-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="d9559-116">Sablon fogalmak</span><span class="sxs-lookup"><span data-stu-id="d9559-116">Template concepts</span></span>
<span data-ttu-id="d9559-117">A [legfrissebb hírek küldése Notification Hubs használata] olyan alkalmazás, amelynek használt parancsfájlkezelő **címkék** előfizetés az értesítésekre hírek különböző kategóriákban.</span><span class="sxs-lookup"><span data-stu-id="d9559-117">In [Use Notification Hubs to send breaking news] you built an app that used **tags** to subscribe to notifications for different news categories.</span></span>
<span data-ttu-id="d9559-118">Számos alkalmazás, azonban több piacok célként, és honosítási igényelnek.</span><span class="sxs-lookup"><span data-stu-id="d9559-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="d9559-119">Ez azt jelenti, hogy a tartalom maguk értesítést kell honosított és kívánt eszközök beküldeni.</span><span class="sxs-lookup"><span data-stu-id="d9559-119">This means that the content of the notifications themselves have to be localized and delivered to the correct set of devices.</span></span>
<span data-ttu-id="d9559-120">Ebben a témakörben bemutatjuk, hogyan használható a **sablon** könnyen képes biztosítani a honosított legfrissebb híreket tartalmazó értesítések a Notification Hubs szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="d9559-120">In this topic we will show how to use the **template** feature of Notification Hubs to easily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="d9559-121">Megjegyzés: egy honosított értesítések küldéséhez módja az egyes címkék több verzióját.</span><span class="sxs-lookup"><span data-stu-id="d9559-121">Note: one way to send localized notifications is to create multiple versions of each tag.</span></span> <span data-ttu-id="d9559-122">Például angol, francia és Mandarin támogatásához lenne szükséges három különböző címkék híreket: "world_en", "world_fr" és "world_ch".</span><span class="sxs-lookup"><span data-stu-id="d9559-122">For instance, to support English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="d9559-123">A Microsoft majd kellene elküldeni a híreket honosított verzióját az egyes ezekkel a címkékkel.</span><span class="sxs-lookup"><span data-stu-id="d9559-123">We would then have to send a localized version of the world news to each of these tags.</span></span> <span data-ttu-id="d9559-124">Ebben a témakörben a sablonok a címkék elterjedése és több üzenetet küldeni a követelmény elkerülése érdekében használjuk.</span><span class="sxs-lookup"><span data-stu-id="d9559-124">In this topic we use templates to avoid the proliferation of tags and the requirement of sending multiple messages.</span></span>

<span data-ttu-id="d9559-125">Magas szinten sablonok, amelyek egy adja meg, hogy egy adott eszközhöz egy értesítést kell kapnia.</span><span class="sxs-lookup"><span data-stu-id="d9559-125">At a high level, templates are a way to specify how a specific device should receive a notification.</span></span> <span data-ttu-id="d9559-126">A sablon a pontos az adattartalom formátuma az app-háttér által küldött üzenet részét képező tulajdonságok alapján határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d9559-126">The template specifies the exact payload format by referring to properties that are part of the message sent by your app back-end.</span></span> <span data-ttu-id="d9559-127">Ebben az esetben az összes támogatott nyelvek tartalmazó területibeállítás-független üzenetet küldünk:</span><span class="sxs-lookup"><span data-stu-id="d9559-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="d9559-128">Ezután azt fogja győződjön meg arról, hogy az eszközök regisztrálása a sablont, amely a megfelelő tulajdonság hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d9559-128">Then we will ensure that devices register with a template that refers to the correct property.</span></span> <span data-ttu-id="d9559-129">Például egy Windows Áruházbeli alkalmazást, amely kéri a egyszerű bejelentési üzenet regisztrálja a következő sablon bármely megfelelő címkékkel:</span><span class="sxs-lookup"><span data-stu-id="d9559-129">For instance, a Windows Store app that wants to receive a simple toast message will register for the following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="d9559-130">Sablonok egyik újdonsága nagyon hatékony többet is megtudhat arról a a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="d9559-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="the-app-user-interface"></a><span data-ttu-id="d9559-131">Az alkalmazás felhasználói felülete</span><span class="sxs-lookup"><span data-stu-id="d9559-131">The app user interface</span></span>
<span data-ttu-id="d9559-132">A Microsoft most módosítja a Megtörje hírek alkalmazást, amely létrehozta a következő témakör [legfrissebb hírek küldése Notification Hubs használata] küldése honosított a legfrissebb hírek sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="d9559-132">We will now modify the Breaking News app that you created in the topic [Use Notification Hubs to send breaking news] to send localized breaking news using templates.</span></span>

<span data-ttu-id="d9559-133">A Windows Áruházbeli alkalmazásban:</span><span class="sxs-lookup"><span data-stu-id="d9559-133">In your Windows Store app:</span></span>

<span data-ttu-id="d9559-134">A területi beállítás kombinált lista tartalmazza a MainPage.xaml módosítása:</span><span class="sxs-lookup"><span data-stu-id="d9559-134">Change your MainPage.xaml to include a locale combobox:</span></span>

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

## <a name="building-the-windows-store-client-app"></a><span data-ttu-id="d9559-135">A Windows áruház-ügyfélalkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d9559-135">Building the Windows Store client app</span></span>
1. <span data-ttu-id="d9559-136">Az értesítések osztályban adja hozzá a területi beállítással a *StoreCategoriesAndSubscribe* és *SubscribeToCateories* módszerek.</span><span class="sxs-lookup"><span data-stu-id="d9559-136">In your Notifications class, add a locale parameter to your  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
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
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    <span data-ttu-id="d9559-137">Vegye figyelembe, hogy hívása helyett a *RegisterNativeAsync* metódus hívása *RegisterTemplateAsync*: egy adott értesítés formátuma, amelyben a sablont függ a területi beállítás regisztrálja azt.</span><span class="sxs-lookup"><span data-stu-id="d9559-137">Note that instead of calling the *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which the template depends on the locale.</span></span> <span data-ttu-id="d9559-138">Egy nevet a sablonnak ("localizedWNSTemplateExample"), azt is adja meg, mert előfordulhat, hogy szeretné regisztrálni (például egy bejelentési értesítést) és a csempék egy több sablon, ezért ellenőriznünk kell a nevét ahhoz, hogy a frissítés vagy törlés őket.</span><span class="sxs-lookup"><span data-stu-id="d9559-138">We also provide a name for the template ("localizedWNSTemplateExample"), because we might want to register more than one template (for instance one for toast notifications and one for tiles) and we need to name them in order to be able to update or delete them.</span></span>
   
    <span data-ttu-id="d9559-139">Vegye figyelembe, hogy ha egy eszköz több sablon regisztrálja az azonos címkével, egy bejövő üzenet célcsoport-kezelési eredményező címke több értesítés is érkezett kell juttatni az eszközre (minden sablon egy).</span><span class="sxs-lookup"><span data-stu-id="d9559-139">Note that if a device registers multiple templates with the same tag, an incoming message targeting that tag will result in multiple notifications delivered to the device (one for each template).</span></span> <span data-ttu-id="d9559-140">Ez a viselkedés akkor hasznos, ha ugyanazon logikai üzenet van több vizuális értesítések, például a Windows Áruházbeli alkalmazások megjelenítő egy jelvény és egy bejelentési is eredményezheti.</span><span class="sxs-lookup"><span data-stu-id="d9559-140">This behavior is useful when the same logical message has to result in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="d9559-141">Adja hozzá a következő metódust beolvasni a tárolt nyelvterületi beállításokat:</span><span class="sxs-lookup"><span data-stu-id="d9559-141">Add the following method to retrieve the stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="d9559-142">A MainPage.xaml.cs, frissítse a gomb látható beolvasása a területi beállítás kombinált lista aktuális értéke, és hogy az értesítések osztály hívása kezelő kattintson:</span><span class="sxs-lookup"><span data-stu-id="d9559-142">In your MainPage.xaml.cs, update your button click handler by retrieving the current value of the Locale combo box and providing it to the call to the Notifications class, as shown:</span></span>
   
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
4. <span data-ttu-id="d9559-143">Végezetül az App.xaml.cs fájlban ellenőrizze, hogy frissítse a `InitNotificationsAsync` beolvasni a területi és használatra, ha az előfizetés módszert:</span><span class="sxs-lookup"><span data-stu-id="d9559-143">Finally, in your App.xaml.cs file, make sure to update your `InitNotificationsAsync` method to retrieve the locale and use it when subscribing:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="d9559-144">Honosított értesítések küldése a háttérrendszerből</span><span class="sxs-lookup"><span data-stu-id="d9559-144">Send localized notifications from your back-end</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
<span data-ttu-id="d9559-145">[legfrissebb hírek küldése Notification Hubs használata]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="d9559-145">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
