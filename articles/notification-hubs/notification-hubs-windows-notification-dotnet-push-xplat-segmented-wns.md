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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Használja a Notification Hubs toosend legfrissebb hírek
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Áttekintés
Ez a témakör bemutatja, hogyan toouse Azure Notification Hubs toobroadcast friss hírek értesítések tooa Windows áruház vagy a Windows Phone 8.1 (nem Silverlight) alkalmazást. Ha a Windows Phone 8.1 Silverlight céloz meg, tekintse meg a toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) verziója. Amikor végzett, akkor tudja tooregister számára megtörje hírek kategóriák érdekli, és ezen kategóriák csak leküldéses értesítések fogadásához. Ebben a forgatókönyvben számos alkalmazás általános felépítését, amelyben értesítések vannak küldött toobe toogroups, amely azokat, pl. RSS-olvasóval, zene ventilátorok alkalmazások érdeklődik elemnek már deklarálva, és így tovább. 

Szórási forgatókönyvek engedélyezve vannak, beleértve a következőket egy vagy több *címkék* hello értesítési központ regisztráció létrehozásakor. Ha az értesítések küldése tooa címke hello címke regisztrált minden eszköz hello értesítést kap. Mivel a címkékkel egyszerűen csak karakterláncok, nincs kiépítve előzetes toobe. Címkékkel kapcsolatos további információkért tekintse meg túl[Notification Hubs útválasztási és címke kifejezések](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows áruház és Windows Phone-projektek verzió 8.1 és korábbi verziók nem támogatottak a Visual Studio 2017.  További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs). 

## <a name="prerequisites"></a>Előfeltételek
Ebben a témakörben megfogalmazott célra épül létrehozott hello app [Ismerkedés a Notification Hubs][get-started]. Az oktatóanyag elindítása előtt már végrehajtotta [Ismerkedés a Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Kategória kiválasztása toohello alkalmazás hozzáadása
első lépés hello tooadd hello felhasználói felületi elemek tooyour meglévő fő lapján, amelyek lehetővé teszik a hello felhasználói tooselect kategóriák tooregister. felhasználó által kijelölt hello kategóriák hello eszközön tárolja. Hello alkalmazás indításakor a eszközregisztráció címkeként az értesítési központ kijelölt hello kategóriákhoz jön létre.

1. Nyissa meg a hello MainPage.xaml projektfájlt, majd a következő kódot a hello másolási hello **rács** elem:
   
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
2. Kattintson jobb gombbal a hello **megosztott** projektre, majd adja hozzá egy új osztályt **értesítések**, vegye fel a hello **nyilvános** módosító toohello osztály definícióját, és vegye fel a következő hello **használatával** utasítások toohello új kódfájl:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. Másolás hello következő hello új kódot **értesítések** osztály:
   
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
   
    Ez az osztály hello helyi tároló toostore hello kategóriáinak híreket, hogy az eszköz rendelkezik-e tooreceive használja. Vegye figyelembe, hogy a hívó hello helyett *RegisterNativeAsync* metódus hívása *RegisterTemplateAsync* tooregister hello kategóriákban sablon regisztráció használatával. 
   
    Azt is nevezze el a hello sablon ("simpleWNSTemplateExample"), mert célszerű lehet tooregister egynél több sablont (például egy bejelentési értesítést) és egy a csempéket, ezért ellenőriznünk kell, hogy tooname azokat sorrendben toobe képes tooupdate vagy törölje őket.
   
    Vegye figyelembe, hogy ha egy eszköz több sablon regisztrálja azonos címke, a célcsoport-kezelési címke eredményez bejövő üzenet hello több értesítés is érkezett (egy mindegyik sablon) toohello eszköz kézbesíteni. Ez a viselkedés akkor hasznos, ha hello azonos logikai üzenetnek tooresult több vizuális értesítések, például a Windows Áruházbeli alkalmazások megjelenítő egy jelvény és egy bejelentési is.
   
    A sablonok további információkért lásd: [sablonok](notification-hubs-templates-cross-platform-push-messages.md).
4. Hello App.xaml.cs projektfájlt, adja hozzá a következő tulajdonság toohello hello **App** osztály:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Ez a tulajdonság akkor használt toocreate és a hozzáférés a **értesítések** példány.
   
    A fenti kódot hello, cserélje le a hello `<hub name>` és `<connection string with listen access>` helyőrzőket az értesítési központ nevét és hello kapcsolati karakterláncot *DefaultListenSharedAccessSignature* korábban beszerzett.
   
   > [!NOTE]
   > Ügyfél alkalmazáshoz elosztott hitelesítő adatok nem általában biztonságos, mert csak kell terjeszteni figyelési hozzáférési kulcs hello ügyfél alkalmazása. Figyeljen hozzáférés lehetővé teszi, hogy az alkalmazás tooregister az értesítéseket, de a meglévő regisztrációját nem lehet módosítani, és értesítések nem küldhető el. hello teljes körű hozzáférési kulcsot használnak a következő biztonságos háttérszolgáltatás értesítések küldését, és meglévő regisztrációk módosítása.
   > 
   > 
5. A MainPage.xaml.cs adja hozzá a következő sor hello:
   
        using Windows.UI.Popups;
6. Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő metódus hello:
   
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
   
    Ezzel a módszerrel hoz létre a kategóriák és a hello listáját **értesítések** osztály toostore hello lista hello helyi tárolóban, és regisztrálja a hello megfelelő címkéket az értesítési központban. Kategóriák megváltozásakor hello regisztrációs hello új kategóriák újra létrejön.

Az alkalmazás most már tudja toostore kategóriák készlete hello eszközön helyi tárolóban és regisztrálása az értesítési központban hello hello felhasználót módosítások hello kategóriák kiválasztását.

## <a name="register-for-notifications"></a>Az értesítések regisztrálása
Ezeket a lépéseket hello értesítési központ használatával a helyi tárolóban tárolt hello kategóriák indításkor regisztrálja.

> [!NOTE]
> Mivel hello csatorna URI Azonosítóját, a Windows értesítési szolgáltatása (WNS) hello által hozzárendelt bármikor módosíthatja, regisztrálnia kell az értesítések gyakran tooavoid értesítés sikertelen. Ebben a példában regisztrál az értesítési hello alkalmazás minden indításakor. Gyakran futtatott alkalmazások esetén naponta csak egyszer, valószínűleg kihagyhatja regisztrációs toopreserve sávszélesség Ha kevesebb mint egy nappal hello előző regisztráció óta eltelt.
> 
> 

1. Nyissa meg hello App.xaml.cs fájlt, és a frissítés hello **InitNotificationsAsync** metódus toouse hello `notifications` osztály toosubscribe kategóriák alapján.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Ez biztosítja, hogy hello alkalmazás minden indításakor azt hello kategóriák átmásolja a helyi tárolóhoz, és kéri a regisztrálás ezen kategóriák. Hello **InitNotificationsAsync** metódus hello részeként létrejött [Ismerkedés a Notification Hubs] [ get-started] oktatóanyag.
2. Hello MainPage.xaml.cs projekt fájlban adja hozzá a következő kód toohello hello *OnNavigatedTo* módszert:
   
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
   
    ![][19]
3. Új értesítés küldése a következő módokon hello egyikében hello háttérrendszerből:
   
   * **Konzolalkalmazás:** hello konzol alkalmazás indításához.
   * **Java/php-ből:** az alkalmazást, a parancsfájl futtatásához.
     
     Értesítések a kiválasztott hello kategóriák bejelentési értesítések jelennek meg.
     
     ![][14]

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag azt megtanulta, hogyan toobroadcast legfrissebb hírek kategória szerint. Vegye figyelembe, hogy befejezése hello oktatóprogramot kínál, amelyek más speciális Notification Hubs forgatókönyvek jelölje ki a következő egyikét:

* [Használjon honosított toobroadcast Notification Hubs – legfrissebb hírek]
  
    Ismerje meg, hogyan megtörje hírek app tooenable küldése tooexpand hello honosított értesítések.

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
