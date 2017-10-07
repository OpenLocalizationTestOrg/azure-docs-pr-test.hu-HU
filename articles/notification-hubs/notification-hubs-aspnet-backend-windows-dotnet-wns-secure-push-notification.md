---
title: "Notification Hubs biztonságos leküldéses aaaAzure"
description: "Tudnivalók a biztonságos toosend a leküldéses értesítések az Azure-ban. A Kódminták C# hello .NET API használatával."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Biztonságos Azure Notification Hubs leküldéses
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Áttekintés
Leküldéses értesítési támogatása a Microsoft Azure lehetővé teszi tooaccess egy könnyen használható, többplatformos, kibővített leküldéses infrastruktúrában, ami jelentősen egyszerűbb a leküldéses értesítések a mobile fogyasztói, valamint a vállalati alkalmazások hello végrehajtása platformok.

Tooregulatory vagy biztonsági korlátozások miatt néha egy alkalmazás érdemes tooinclude valamit a hello szabványos leküldéses értesítési infrastruktúrát keresztül nem továbbítható hello értesítést. Ez az oktatóanyag leírja, hogyan tooachieve hello úgy, hogy a bizalmas adatokat hello ügyféleszköz- és hello háttéralkalmazás közötti biztonságos és hitelesített kapcsolaton keresztül küld ugyanazt a felhasználói élményt.

Magas szinten hello folyamat a következőképpen történik:

1. hello app háttér:
   * Háttér-adatbázisban tárolja biztonságos hasznos.
   * Küldi hello azonosítója (nem biztonságos információk küldése) értesítési toohello eszköz.
2. hello alkalmazást hello eszközön, hello értesítés fogadása közben:
   * hello eszköz kapcsolatba lép a hello háttér-kérelmező hello biztonságos hasznos.
   * hello hasznos mutatja az hello app értesítésként hello eszközön.

Fontos, hogy megelőző folyamata hello (és az oktatóanyag) feltételezzük, hogy hello eszköz toonote tárol egy hitelesítési jogkivonatot helyi tárhelyre – hello felhasználó bejelentkezése után. Ez biztosítja, hogy teljesen zökkenőmentes élményt, mivel hello eszköz hello értesítési biztonságos hasznos a token használatával kérheti le. Ha az alkalmazás nem tárolja a hitelesítési tokenek hello eszköz, vagy ezeket a jogkivonatokat is járhatott, hello eszközalkalmazás hello értesítés fogadásakor megjelenjen-e arra kéri a hello felhasználói toolaunch hello alkalmazás általános értesítést. hello app majd hello felhasználó hitelesíti, és hello értesítési tartalom jeleníti meg.

Ez biztonságos leküldéses az oktatóanyag bemutatja, hogyan toosend leküldéses értesítés biztonságos helyen. hello oktatóanyag épít, hello [felhasználók értesítése](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóanyag, ezért el kell végeznie hello lépéseket, hogy az oktatóanyagban először.

> [!NOTE]
> Ez az oktatóanyag feltételezi, hogy létrehozta és leírtak szerint konfigurálta az értesítési központ [Ismerkedés a Notification Hubs (a Windows áruházban)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Emellett vegye figyelembe, hogy a Windows Phone 8.1 (nem Windows Phone) Windows hitelesítő adatok szükségesek, és hogy háttérfeladatok nem működnek a Windows Phone 8.0-s vagy Silverlight 8.1. A Windows Áruházbeli alkalmazásokhoz is fogadhatja az értesítéseket keresztül háttérfeladat csak akkor, ha hello app zárolási képernyő engedélyezve (jelölőnégyzettel hello a hello Appmanifest).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a>Windows Phone-projekt hello módosítása
1. A hello **NotifyUserWindowsPhone** projektre, adja hozzá a következő kód tooApp.xaml.cs tooregister hello leküldéses háttérfeladat hello. Adja hozzá a következő kódsort hello hello végén hello `OnLaunched()` módszert:
   
        RegisterBackgroundTask();
2. Továbbra is az App.xaml.cs fájlban adja hozzá a következő kódot közvetlenül után hello hello `OnLaunched()` módszert:
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. Adja hozzá a következő hello `using` hello App.xaml.cs fájlt hello tetején utasításokat:
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. A hello **fájl** a Visual Studio menüjében kattintson **összes mentése**.

## <a name="create-hello-push-background-component"></a>Hello leküldéses háttér-összetevő létrehozása
hello következő lépés egy toocreate hello leküldéses háttér összetevő.

1. A Megoldáskezelőben kattintson jobb gombbal a legfelső szintű csomópontja hello hello megoldás (**megoldás SecurePush** ebben az esetben), majd kattintson a **Hozzáadás**, majd kattintson **új projekt**.
2. Bontsa ki a **Áruházbeli alkalmazások**, majd kattintson a **Windows Phone-alkalmazások**, kattintson a **Windows-Futtatókörnyezetű összetevőben (Windows Phone)**. Név hello projekt **PushBackgroundComponent**, és kattintson a **OK** toocreate hello projekt.
   
    ![][12]
3. A Megoldáskezelőben kattintson a jobb gombbal hello **PushBackgroundComponent (Windows Phone 8.1)** projektre, majd kattintson a **Hozzáadás**, majd kattintson a **osztály**. Hello új osztály neve **PushBackgroundTask.cs**. Kattintson a **Hozzáadás** toogenerate hello osztály.
4. Cserélje le a teljes tartalma hello hello **PushBackgroundComponent** névtér-definíciót az alábbi kódot, és hello helyőrző hello `{back-end endpoint}` hello háttér-végponttal másikban kapott a Háttér:
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. A Megoldáskezelőben kattintson a jobb gombbal hello **PushBackgroundComponent (Windows Phone 8.1)** projektre, majd kattintson **NuGet-csomagok kezelése**.
6. Kattintson a bal oldalon hello **Online**.
7. A hello **keresési** mezőbe írja be **HTTP-alapú**.
8. Hello eredmények listájában kattintson **Microsoft HTTP ügyfél függvénytárainak**, és kattintson a **telepítése**. Hello telepítését.
9. Vissza a hello NuGet **keresési** mezőbe írja be **Json.net**. Telepítse a hello **Json.NET** csomagot, majd Bezárás hello NuGet Package Manager ablak.
10. Adja hozzá a következő hello `using` hello hello tetején utasítások **PushBackgroundTask.cs** fájlt:
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. A Megoldáskezelőben a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projektre, kattintson a jobb gombbal **hivatkozások**, majd kattintson a **hivatkozás hozzáadása...** . Hello hivatkozáskezelő párbeszédpanelen jelölőnégyzetet hello mellett túl**PushBackgroundComponent**, és kattintson a **OK**.
12. A Megoldáskezelőben kattintson duplán a **Package.appxmanifest** a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt. A **értesítések**, beállíthatja **bejelentési képes** túl**Igen**.
    
    ![][3]
13. Még mindig **Package.appxmanifest**, kattintson a hello **nyilatkozatok** hello felső menüből. A hello **elérhető nyilatkozatok** legördülő menüben kattintson a **háttérfeladatok**, és kattintson a **Hozzáadás**.
14. A **Package.appxmanifest**a **tulajdonságok**, ellenőrizze **leküldéses értesítési**.
15. A **Package.appxmanifest**a **Alkalmazásbeállítások**, típus **PushBackgroundComponent.PushBackgroundTask** a hello **belépési pont** a mező.
    
    ![][13]
16. A hello **fájl** menüben kattintson a **összes mentése**.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása
toorun hello alkalmazás, a következő hello:

1. A Visual Studio, futtassa a hello **AppBackend** webes API-alkalmazás. Az ASP.NET-weblap jelenik meg.
2. A Visual Studio, futtassa a hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-alkalmazás. Windows Phone-emulátor hello fut, és automatikusan betölti hello alkalmazást.
3. A hello **NotifyUserWindowsPhone** alkalmazás felhasználói felületén, adja meg a felhasználónevet és jelszót. Ezek karakterlánc lehet, de kell hello ugyanazt az értéket.
4. A hello **NotifyUserWindowsPhone** alkalmazás felhasználói felületén kattintson **jelentkezzen be, és regisztrálja**. Kattintson a **leküldéses küldése**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
