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
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a>Használjon honosított toosend Notification Hubs – legfrissebb hírek
> [!div class="op_single_selector"]
> * [Windows áruház C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse hello **sablon** az Azure Notification Hubs toobroadcast nyelv és az eszköz honosított katalóguselemekről híreket tartalmazó értesítések megtörje szolgáltatása. Ez az oktatóanyag a kiindulási pont hello Windows Áruházbeli alkalmazás létrehozott [legfrissebb hírek Notification Hubs használata toosend]. Amikor végzett, képes tooregister érdekli kategóriákban fogja, adjon meg egy nyelvi mely tooreceive hello értesítések, és csak a kiválasztott hello kategóriák leküldéses értesítések fogadásához az adott nyelveken.

Nincsenek két részből toothis forgatókönyv:

* hello Windows Áruházbeli alkalmazás lehetővé teszi, hogy ügyfél eszközök toospecify egy nyelvet, és toosubscribe toodifferent megtörje hírek kategóriák;
* hello háttér-közzéteszi hello értesítéseket, hello **címke** és **sablon** az Azure Notification Hubs feautres.

## <a name="prerequisites"></a>Előfeltételek
Már végrehajtotta hello [legfrissebb hírek Notification Hubs használata toosend] oktatóanyag és hello kód érhető el, mert ez az oktatóanyag közvetlenül épít, hogy a kód.

Szükség a Visual Studio 2012 vagy újabb.

## <a name="template-concepts"></a>Sablon fogalmak
A [legfrissebb hírek Notification Hubs használata toosend] olyan alkalmazás, amelynek használt parancsfájlkezelő **címkék** toosubscribe toonotifications hírek különböző kategóriákban.
Számos alkalmazás, azonban több piacok célként, és honosítási igényelnek. Ez azt jelenti, hogy maguk hello értesítések hello tartalmának rendelkezik honosított toobe kézbesített toohello, javítsa ki az eszközök.
Ebben a témakörben bemutatjuk a hogyan toouse hello **sablon** tooeasily biztosítanak a Notification Hubs szolgáltatása honosított legfrissebb híreket tartalmazó értesítések.

Megjegyzés: egyirányú toosend honosított értesítések toocreate minden címke több verziója van. Például toosupport angol, francia és Mandarin, lenne szükséges három különböző címkék híreket: "world_en", "world_fr" és "world_ch". A Microsoft majd kellene toosend hello world hírek tooeach címkéket honosított verzióját. Ez a témakör használjuk, a címkéket a sablonok tooavoid hello elterjedése és hello követelmény több üzenetet küldeni.

Magas szinten, a sablonok olyan módon toospecify hogyan egy adott eszközhöz egy értesítést kell kapnia. hello sablon hello pontos az adattartalom formátuma hello által küldött üzenet az alkalmazás háttér-részét képező tooproperties alapján határozza meg. Ebben az esetben az összes támogatott nyelvek tartalmazó területibeállítás-független üzenetet küldünk:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Ezután azt fogja győződjön meg arról, hogy az eszközök regisztrálása a sablont, amely hivatkozik a toohello megfelelő tulajdonság. Például egy Windows Áruházbeli alkalmazást, amely tooreceive szeretne egy egyszerű bejelentési üzenet regisztrálja a sablont a megfelelő címkéket a következő hello:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Sablonok egyik újdonsága nagyon hatékony többet is megtudhat arról a a [sablonok](notification-hubs-templates-cross-platform-push-messages.md) cikk. 

## <a name="hello-app-user-interface"></a>hello alkalmazás felhasználói felülete
Most módosítja, a Microsoft hello Megtörje hírek app hello a témakör a [legfrissebb hírek Notification Hubs használata toosend] toosend honosított legfrissebb hírek sablonok használatával.

A Windows Áruházbeli alkalmazásban:

A MainPage.xaml tooinclude a területi beállítás combobox módosítása:

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

## <a name="building-hello-windows-store-client-app"></a>Hello Windows Store ügyfél alkalmazás elkészítése
1. Az értesítések osztályban adja hozzá a nyelv paraméter tooyour *StoreCategoriesAndSubscribe* és *SubscribeToCateories* módszerek.
   
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
   
    Vegye figyelembe, hogy a hívó hello helyett *RegisterNativeAsync* metódus hívása *RegisterTemplateAsync*: egy adott értesítés formátuma, mely hello sablon függ hello területi beállítás regisztrálja azt. Azt is nevezze el a hello sablon ("localizedWNSTemplateExample"), mert célszerű lehet tooregister egynél több sablont (például egy bejelentési értesítést) és egy a csempéket, ezért ellenőriznünk kell, hogy tooname azokat sorrendben toobe képes tooupdate vagy törölje őket.
   
    Vegye figyelembe, hogy ha egy eszköz több sablon regisztrálja azonos címke, a célcsoport-kezelési címke eredményez bejövő üzenet hello több értesítés is érkezett (egy mindegyik sablon) toohello eszköz kézbesíteni. Ez a viselkedés akkor hasznos, ha hello azonos logikai üzenetnek tooresult több vizuális értesítések, például a Windows Áruházbeli alkalmazások megjelenítő egy jelvény és egy bejelentési is.
2. Adja hozzá a következő metódus tooretrieve hello tárolt területi hello:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. A MainPage.xaml.cs, frissítse a gomb kezelő kattintson beolvasása hello hello területi kombinált lista aktuális értéke, és azt toohello hívás toohello értesítések osztály megadásával látható módon:
   
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
4. Végül az App.xaml.cs fájlban győződjön meg arról, hogy tooupdate a `InitNotificationsAsync` metódus tooretrieve hello területi beállítás, és használatra, ha feliratkozik:
   
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

## <a name="send-localized-notifications-from-your-back-end"></a>Honosított értesítések küldése a háttérrendszerből
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
