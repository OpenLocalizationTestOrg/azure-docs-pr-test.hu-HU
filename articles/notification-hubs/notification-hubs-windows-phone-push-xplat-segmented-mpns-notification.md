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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Használja a Notification Hubs toosend legfrissebb hírek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast legfrissebb hírek értesítések tooa Windows Phone 8.0/8.1 Silverlight-alkalmazást. Ha a Windows Áruházbeli és Windows Phone 8.1-alkalmazást céloz meg, tekintse meg a tootoohello [univerzális Windows-](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) verziója. Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához. Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely rendelkezik deklarálva érdeklődési rajtuk, például az RSS-olvasóval, az alkalmazások zene ventilátorok stb.

Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor. Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap. Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe. Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Előfeltételek
Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs]. Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs].

## <a name="add-category-selection-toohello-app"></a>Kategória kiválasztása toohello alkalmazás hozzáadása
első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő lapján, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister. felhasználó által kijelölt hello kategóriák hello eszközön tárolja. Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.

1. Nyissa meg a hello MainPage.xaml projektfájlt, majd cserélje le a hello **rács** nevű elem `TitlePanel` és `ContentPanel` a hello a következő kódot:
   
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
2. Hello projektben, hozzon létre egy új osztályt **értesítések**, vegye fel a hello **nyilvános** módosító toohello definition osztályt, majd adja hozzá a következő hello **használatával** utasítások Új kód fájl toohello:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Másolás hello következő hello új kódot **értesítések** osztály:
   
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

    Ez az osztály elkülönített hello tárolási toostore hello kategóriáinak híreket, hogy ez az eszköz megfelel-e tooreceive használja. Módszerek tooregister ezen kategóriák használatával is tartalmaz egy [sablon](notification-hubs-templates-cross-platform-push-messages.md) értesítési regisztrációt.


1. Hello App.xaml.cs projektfájlt, adja hozzá a következő tulajdonság toohello hello **App** osztály. Cserélje le a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása. Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el. hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.
   > 
   > 
2. A MainPage.xaml.cs adja hozzá a következő sor hello:
   
        using Windows.UI.Popups;
3. Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő metódus hello:
   
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
   
    Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban. Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.

Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.

## <a name="register-for-notifications"></a>Az értesítések regisztrálása
Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.

> [!NOTE]
> Mivel hello csatorna URI Azonosítóját, a Microsoft leküldéses értesítési szolgáltatásának (MPNS) hello által hozzárendelt bármikor módosíthatja, regisztrálnia kell az értesítések gyakran tooavoid értesítés sikertelen. Ez a példa regisztrálja az értesítések hello alkalmazás minden indításakor. Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.
> 
> 

1. Nyissa meg a hello App.xaml.cs fájlt, és adja hozzá a hello **aszinkron** módosító túl**Application_Launching** metódus és a név felülírandó hello Notification Hubs regisztrációs kódot, amelyet a [Ismerkedés a Notification Hubs] a hello a következő kódot:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri ezek kategóriák regisztráció.
2. Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő kódot, amely megvalósítja az hello hello **OnNavigatedTo** módszert:
   
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
   
    A frissítések hello főoldala hello állapota alapján korábban mentett kategóriák.

hello alkalmazás kész, és képes tárolni a kategóriák hello eszköz használt helyi tárhely tooregister hello értesítési központban az amikor hello felhasználót módosítások hello kategóriák kiválasztása. A következő kategória értesítések toothis app küldő háttérkiszolgálón meghatározzák azt.

## <a name="sending-tagged-notifications"></a>Címkézett értesítések küldése
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a>Hello alkalmazás futtatását, és értesítések
1. A Visual Studio nyomja le az F5 toocompile és hello alkalmazás indításához.
   
    ![][1]
   
    Megjegyzés: a felhasználói felület számos hello alkalmazást, amely váltja kiválaszthatja hello kategóriák toosubscribe számára.
2. Egy vagy több kategóriák váltógombok engedélyezése, majd kattintson az **előfizetés**.
   
    hello app hello kiválasztott kategóriák alakítja címkék, és egy új eszközök regisztrációja kijelölt hello címkék hello értesítési központ érkező kérelmeket. hello regisztrált kategóriák adott vissza, és megjelenik egy párbeszédpanel.
   
    ![][2]
3. Azután, hogy megérkezett igazolja, hogy van-e előfizetés befejezte a kategóriák, futtassa a hello konzol app toosend értesítések az egyes kategóriák. Győződjön meg arról, hogy csak az előfizetett hello kategóriák értesítést kap.
   
    ![][3]

Ez a témakör befejeződött.

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
