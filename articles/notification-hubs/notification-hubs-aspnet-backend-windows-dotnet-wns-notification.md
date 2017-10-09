---
title: "aaaAzure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések az Azure-ban. A Kódminták C# hello .NET API használatával."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: a366181faa81e78adf4de61435ef2790c3aa29d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Az Azure Notification Hubs – felhasználók értesítése .NET-háttérrendszerrel
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Áttekintés
Leküldéses értesítési támogatás az Azure-ban lehetővé teszi tooaccess egy könnyen kezelhető, multiplatform és kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok. Ez az oktatóanyag toouse Azure Notification Hubs toosend a leküldéses értesítések tooa adott alkalmazás felhasználói az adott eszközön. ASP.NET WebAPI háttérrendszerből használt tooauthenticate ügyfelek. Ügyfél felhasználói hello segítségével hitelesíti, és címke hello háttér toonotification regisztrációs automatikusan felveszi. Ezt a címkét hello háttér toogenerate értesítések egy adott felhasználó által használt toosend lesz. További információ a Apps-háttéralkalmazás segítségével értesítések regisztrálása a témakörben hello útmutatást [az alkalmazás háttérrendszeréből regisztrálása](http://msdn.microsoft.com/library/dn743807.aspx). Ez az oktatóanyag épít hello értesítési központ és az hello létrehozott projekt [Ismerkedés a Notification Hubs] oktatóanyag.

Ez az oktatóanyag egyben hello előfeltétel toohello [biztonságos leküldéses] oktatóanyag. Hello lépéseit az oktatóanyag befejezése után folytathatja toohello [biztonságos leküldéses] oktatóanyag, amely bemutatja, hogyan toomodify hello kódot az oktatóanyag toosend leküldéses értesítés biztonságos helyen.

## <a name="before-you-begin"></a>Előkészületek
Visszajelzéseit komolyan vesszük. Ha bármilyen nehézségbe ütközik a témakör, vagy javaslatai vannak, a tartalom javítása befejezése, azt fogadjuk visszajelzéseit hello lap hello alján.

az oktatóanyag befejezése hello kódja a Githubon található [Itt](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elindítása előtt már végrehajtotta a Mobile Services-oktatóanyag:

* [Ismerkedés a Notification Hubs]<br/>Az értesítési központ létrehozása és hello alkalmazásnév lefoglalása, és ebben az oktatóanyagban tooreceive értesítések regisztrálása. Ez az oktatóanyag feltételezi, hogy már végrehajtotta ezeket a lépéseket. Ha nem, kövesse a hello lépéseket [Ismerkedés a Notification Hubs (a Windows áruházban)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); pontosabban szakaszok hello [hello Windows Áruházbeli alkalmazás regisztrálása](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) és [konfigurálása az értesítési központ](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Különösen, győződjön meg arról, hogy megadta-e hello **CSOMAGAZONOSÍTÓT** és **Ügyfélkulcs** hello portálon, a hello értékek **konfigurálása** az értesítési központ lapján. A konfigurációs eljárást hello szakasz [az értesítési központ konfigurálása](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Ez az egy fontos lépés: Ha hello portal hello-felhasználó hitelesítő adatai nem egyezik meg a hello alkalmazás neve mellett dönt, hello leküldéses értesítés nem fog sikerülni.

> [!NOTE]
> Ha a Mobile Apps az Azure App Service-ben, a háttérszolgáltatás használ, tekintse meg a hello [Mobile Apps-verziójáért](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) oktatóanyag.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-hello-code-for-hello-client-project"></a>Hello ügyfélprojekt hello kódját frissítése
Ebben a szakaszban hello elvégezte hello projektben hello kód frissítése [Ismerkedés a Notification Hubs] oktatóanyag. hello már kell hello áruházhoz kapcsolódó és az értesítési központ konfigurálva. Ebben a szakaszban kód toocall hello új WebAPI háttérrendszerből hozzáadása, és a regisztráció és értesítések küldésének használni.

1. A Visual Studióban nyissa meg a hello hello megoldást hello létrehozott [Ismerkedés a Notification Hubs] oktatóanyag.
2. A Megoldáskezelőben kattintson a jobb gombbal hello **(Windows 8.1)** projektre, majd kattintson **NuGet-csomagok kezelése**.
3. Kattintson a bal oldalon hello **Online**.
4. A hello **keresési** mezőbe írja be **HTTP-alapú**.
5. Hello eredmények listájában kattintson **Microsoft HTTP ügyfél függvénytárainak**, és kattintson a **telepítése**. Hello telepítését.
6. Vissza a hello NuGet **keresési** mezőbe írja be **Json.net**. Telepítse a hello **Json.NET** csomagot, és majd Bezárás hello NuGet Package Manager ablak.
7. Ismételje meg hello fent hello **(Windows Phone 8.1)** projekt tooinstall hello **JSON.NET** hello Windows Phone-projektet a NuGet-csomagot.
8. A Megoldáskezelőben a hello **(Windows 8.1)** projektre, kattintson duplán a **MainPage.xaml** tooopen azt hello Visual Studio szerkesztőjében.
9. A hello **MainPage.xaml** XML-kódot, a név felülírandó hello `<Grid>` hello kód a következő szakasz ismerteti. Ez a kód hozzáadása a felhasználónév és jelszó szövegmezőben, amely hello felhasználó hitelesíti magát az. Bővíti ezenkívül a szövegmezőből hello értesítési üzenet és hello felhasználónév címke, amely hello értesítést kell kapnia:
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag tooSend To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. A Megoldáskezelőben a hello **(Windows Phone 8.1)** projektben nyissa meg **MainPage.xaml** , és cserélje le a Windows Phone 8.1 hello `<Grid>` szakasz ismerteti, hogy ugyanazt a fenti kódot. hello felület alábbihoz hasonló toowhats alább látható.
    
    ![][13]
11. A Solution Explorerben nyissa meg a hello **MainPage.xaml.cs** hello fájlt **(Windows 8.1)** és **(Windows Phone 8.1)** projektek. Adja hozzá a következő hello `using` hello felső fájlokra utasításokat:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. A **MainPage.xaml.cs** a hello **(Windows 8.1)** és **(Windows Phone 8.1)** projektek, adja hozzá a következő tag toohello hello `MainPage` osztály. Lehet, hogy tooreplace `<Enter Your Backend Endpoint>` a hello a tényleges háttér-végpontot szerezte be korábban. Például: `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. Adja hozzá hello kódot alább toohello MainPage osztály **MainPage.xaml.cs** a hello **(Windows 8.1)** és **(Windows Phone 8.1)** projektek.
    
    Hello `PushClick` metódus hello hello kezelője kattintson **küldése leküldéses** gombra. Eszközök címkével ellátott felhasználónév, amely megfelel a hello meghívja hello háttér tootrigger egy értesítési tooall `to_tag` paraméter. hello értesítési üzenettel JSON-tartalomként hello kérés törzsében.
    
    Hello `LoginAndRegisterClick` metódus hello hello kezelője kattintson **jelentkezzen be, és regisztrálja** gombra. Alapszintű hello tárol hitelesítési jogkivonat a helyi tároló (vegye figyelembe, hogy a hitelesítési sémát használja bármely jogkivonat jelképez), majd használja `RegisterClient` tooregister hello backend használata az értesítésekhez.

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed toosend " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // hello "username:<user name>" tag gets automatically added by hello message handler in hello backend.
            // hello tag passed here can be whatever other tags you may want toouse.
            try
            {
                // hello device handle used will be different depending on hello device and PNS. 
                // Windows devices use hello channel uri as hello PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed tooregister with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. A Megoldáskezelőben a hello **megosztott** projektet, nyissa meg hello **App.xaml.cs** fájlt. Keresése túl hello hívás`InitNotificationsAsync()` a hello `OnLaunched()` eseménykezelő. Megjegyzéssé vagy hello hívás túl törlése`InitNotificationsAsync()`. hello gomb kezelő fent inicializálja értesítési regisztráció.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. A Megoldáskezelőben kattintson a jobb gombbal hello **megosztott** projektre, majd kattintson a **Hozzáadás**, és kattintson a **osztály**. Hello osztály neve **RegisterClient.cs**, majd kattintson a **OK** toogenerate hello osztály.
   
   Ez az osztály fog tegye hello REST hívások szükséges toocontact hello háttéralkalmazás, sorrendben tooregister leküldéses értesítéseket. Helyileg tárolja a hello *registrationIds* hello által létrehozott értesítési központot, ahogy az az [az alkalmazás háttérrendszeréből regisztrálása](http://msdn.microsoft.com/library/dn743807.aspx). Vegye figyelembe, hogy használja-e egy engedélyezési jogkivonatot hello kattintva helyi storage-ban tárolt **jelentkezzen be, és regisztrálja** gombra.
2. Adja hozzá a következő hello `using` hello RegisterClient.cs fájl hello tetején utasításokat:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Adja hozzá a következő kódot hello hello `RegisterClient` definition osztályban.
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. Mentse az összes módosítást.

## <a name="testing-hello-application"></a>Hello alkalmazás tesztelése
1. Indítsa el a Windows 8.1 és Windows Phone 8.1 hello alkalmazás. A Windows Phone 8.1-es futtathat hello példány hello emulátor vagy tényleges eszközön.
2. A Windows 8.1 hello példány hello alkalmazás, írja be a **felhasználónév** és **jelszó** üdvözlő képernyőt az alábbi ábrán. Ez eltér a hello felhasználónevet és jelszót ad meg a Windows Phone kell.
3. Kattintson a **jelentkezzen be, és regisztrálja** , és ellenőrizze, hogy a párbeszédpanelen látható, hogy már bejelentkezett. Ez azt is lehetővé teszi hello **küldése leküldéses** gombra.
   
    ![][14]
4. Hello Windows Phone 8.1-példányon, adja meg a felhasználói név karakterláncát mindkét hello **felhasználónév** és **jelszó** mezők kattintson **bejelentkezési és regisztrációs**.
5. Ezt a hello **címzett felhasználónév címke** mezőbe írja be a regisztrált Windows 8.1 hello felhasználónevet. Adjon meg egy értesítési üzenetet, és kattintson a **küldése leküldéses**.
   
    ![][16]
6. Csak a regisztrált hello megfelelő felhasználónév címke hello eszközök hello értesítési üzenetet küld.
   
    ![][15]

## <a name="next-steps"></a>Következő lépések
* Ha a felhasználókat érdeklődési körök alapján szeretné toosegment, lásd: [legfrissebb hírek Notification Hubs használata toosend].
* További kapcsolatos toolearn toouse Notification hubs használatával, lásd: [Notification Hubs használatával].

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Ismerkedés a Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[biztonságos leküldéses]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[legfrissebb hírek Notification Hubs használata toosend]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Notification Hubs használatával]: http://msdn.microsoft.com/library/jj927170.aspx
