---
title: "aaaGet Windows univerzális alkalmazások Azure Mobile Engagement használatába"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az univerzális Windows-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítéseket."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: 48103867-7f64-4646-b019-42bd797d38e2
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/12/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 8224a6d3789cfe4784bbc9472005f9eddb94a8b8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-universal-apps"></a>Ismerkedés az Azure Mobile Engagement univerzális Windows-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók egy univerzális Windows-alkalmazás.
Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement. Ennek során létrehoz egy üres univerzális Windows-alkalmazást, amely alapszintű alkalmazáshasználati adatokat gyűjt, és leküldéses értesítéseket fogad a Windows értesítési szolgáltatása (WNS) használatával.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

## <a name="prerequisites"></a>Előfeltételek
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="set-up-mobile-engagement-for-your-windows-universal-app"></a>A Mobile Engagement beállítása az univerzális Windows-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt," hello minimális határozza meg a szükséges toocollect dátumát és leküldéses értesítés küldéséhez mutat. hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement univerzális Windows SDK-integráció](mobile-engagement-windows-store-sdk-overview.md).

Létrehozhat egy alapszintű alkalmazást a Visual Studio toodemonstrate hello integrációja.

### <a name="create-a-windows-universal-app-project"></a>Univerzális Windows-alkalmazás projekt létrehozása
hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban.

1. Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.
2. Hello előugró ablakban válassza **Windows** -> **univerzális** -> **üres alkalmazás (univerzális Windows)**. Töltse ki a hello app **neve** és **megoldásnév**, és kattintson a **OK**.

    ![][1]

Most létrehozott egy univerzális Windows-alkalmazás projektet, amelybe mellett integrálása hello Azure Mobile Engagement SDK-t.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Telepítse a hello [MicrosoftAzure.MobileEngagement] Nuget-csomagot a projektben. Windows és Windows Phone platformot céloz meg, ha szüksége toodo ezt mindkét projekt. A Windows 8.x és Windows Phone 8.1, hello azonos Nuget csomag helyek hello megfelelő platformspecifikus bináris fájlokat mindkét projektben.
2. Nyissa meg **Package.appxmanifest** és győződjön meg arról, hogy a következő képességet hello hiba jelenik meg:

        Internet (Client)

    ![][2]
3. Most másolja, amely korábban a Mobile Engagement-alkalmazáshoz kimásolt hello kapcsolati karakterláncot, és illessze be hello `Resources\EngagementConfiguration.xml` fájl közötti hello `<connectionString>` és `</connectionString>` címkék:

    ![][3]

    > [!TIP]
    > Ha az alkalmazása a Windows és a Windows Phone platformot is célozza, hozzon létre két Mobile Engagement-alkalmazást – egyet-egyet mindegyik támogatott platformhoz. Két alkalmazások biztosítja, hogy helyes szegmentálását hello célközönség hozhat létre, és az egyes platformokon megfelelően célzott értesítéseket küldhet.

    > [!IMPORTANT]
    > NuGet nem automatikusan hello SDK-erőforrások másolása a Windows 10 UWP-alkalmazásban. Hogy toodo, manuális lépések hello amely jelenik meg (readme.txt), ha hello Nuget-csomagja telepítve van.  

1. A hello `App.xaml.cs` fájlt:

    a. Adja hozzá a hello `using` utasítást:

            using Microsoft.Azure.Engagement;

    b. Inicializálja az hello Engagement metódus hozzáadása:

           private void InitEngagement(IActivatedEventArgs e)
           {
             EngagementAgent.Instance.Init(e);

             //... rest of hello code
           }

    c. A hello SDK hello inicializálása **OnLaunched** módszert:

            protected override void OnLaunched(LaunchActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

    c. Helyezze be a következő hello hello **OnActivated** metódust, és adja hozzá a hello metódust, ha még nincs jelen:

            protected override void OnActivated(IActivatedEventArgs e)
            {
              InitEngagement(e);

              //... rest of hello code
            }

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
adatküldés, és győződjön meg arról, hogy hello felhasználók aktívak, el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello toostart.

1. A hello **MainPage.xaml.cs**, adja hozzá a következő hello `using` utasítást:

    using Microsoft.Azure.Engagement.Overlay;
2. Módosítsa az alaposztály alaposztályát hello **MainPage** a **lap** túl**EngagementPageOverlay**:

        class MainPage : EngagementPageOverlay
3. A hello `MainPage.xaml` fájlt:

    a. Tooyour névtér-deklarációk hozzáadása:

        xmlns:engagement="using:Microsoft.Azure.Engagement.Overlay"

    b. Cserélje le a hello **lap** hello XML-címkenév a **engagement: EngagementPageOverlay**

> [!IMPORTANT]
> Ha az oldala felülírja hello `OnNavigatedTo` módszer, lehet, hogy toocall `base.OnNavigatedTo(e)`. Ellenkező esetben hello tevékenység nem jelentett `EngagementPage` hívások `StartActivity` belül a `OnNavigatedTo` metódus). Ez különösen fontos a Windows Phone-projektben ahol hello alapértelmezett sablon rendelkezik egy `OnNavigatedTo` metódust.
>
> A **univerzális Windows 10-alkalmazások**, hello módszer ajánlott a hello "javasolt módszer: a lap osztályok túlterhelés" szakasza [speciális jelentéskészítés hello Windows univerzális alkalmazások Engagement SDK-t a](mobile-engagement-windows-store-advanced-reporting.md) , ahelyett, hogy hello egyet fent említett.

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
A Mobile Engagement lehetővé teszi a toointeract, és a felhasználók elérését a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok felhasználókat. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="enable-your-app-tooreceive-wns-push-notifications"></a>Az alkalmazás tooreceive WNS leküldéses értesítések engedélyezése
1. A hello `Package.appxmanifest` hello-fájlban **alkalmazás** lap **értesítések**, beállíthatja **képes bejelentési:** túl**Igen**

    ![][5]

### <a name="initialize-hello-reach-sdk"></a>Hello REACH SDK inicializálása
A `App.xaml.cs`, hívja **Initengagement** a hello **Engagementreach.instance.Init(e)** függvény hello ügynök inicializálása után:

        private void InitEngagement(IActivatedEventArgs e)
        {
           EngagementAgent.Instance.Init(e);
           EngagementReach.Instance.Init(e);
        }

Most készen áll a toosend egy bejelentési. A következő lépésben ellenőrizni fogjuk, hogy ezt az alapszintű integrációt megfelelően végezte-e el.

### <a name="grant-access-toomobile-engagement-toosend-notifications"></a>Adjon hozzáférést tooMobile Engagement toosend értesítések
1. Nyissa meg a [Windows fejlesztői központot] a webböngészőjében, és jelentkezzen be, vagy ha szükséges, hozzon létre egy fiókot.
2. Kattintson a **irányítópult** : hello jobb felső sarokban, és kattintson a **hozzon létre egy új alkalmazást** hello bal oldali panelmenüben.

    ![][9]
3. Hozza létre az alkalmazást a neve lefoglalásával.

    ![][10]
4. Hello alkalmazás létrehozása után, nyissa meg túl**szolgáltatások -> leküldéses értesítések** hello bal oldali menüből.

    ![][11]
5. Hello a leküldéses értesítések szakaszban, kattintson a hello **Live Services webhely** hivatkozásra.

    ![][12]
6. Navigálás toohello leküldési hitelesítő adatok szakasz. Győződjön meg arról, hogy a hello **Alkalmazásbeállítások** szakaszt, és másolja a **CSOMAGAZONOSÍTÓT** és **ügyfélkulcs**

    ![][13]
7. Keresse meg a toohello **beállítások** a Mobile Engagement portál hello kattintson **natív leküldés** hello bal oldali szakasz. Kattintson a hello **szerkesztése** gomb tooenter a **csomag biztonsági azonosítóját (SID)** és a **titkos kulcs** látható módon:

    ![][6]
8. Végül győződjön meg arról, hogy ezzel az alkalmazással létrehozott hello App Store-ból a Visual Studio alkalmazás társítva. A Visual Studióban kattintson a következőkre: **Associate app with the store** (Alkalmazás társítása az Áruházzal).

    ![][7]

## <a id="send"></a>Egy értesítési tooyour app küldése
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Hello webalkalmazás működik, ha egy alkalmazásbeli értesítés megjelenik. Ellenkező esetben ha hello app le van zárva, megjelenik egy bejelentési értesítést.
Ha egy alkalmazásbeli értesítés, de a bejelentési értesítés nem látja, és hello alkalmazást a Visual Studio hibakeresési módban futtatja, majd próbálja meg **életciklus-események -> Suspend** a hello eszköztár tooensure hello alkalmazást fel van függesztve. A Visual Studio hello alkalmazás hibakeresése során hello kezdőképernyő gombra kattintott, majd azt nem mindig lesz felfüggesztve, és alkalmazáson belüli hello értesítési akkor látható, amíg azt nem jelenik a bejelentési értesítés.  

![][8]

<!-- URLs. -->
[Mobile Engagement Windows Universal SDK documentation]: ../mobile-engagement-windows-store-integrate-engagement/
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9864592
[Windows fejlesztői központot]: https://dev.windows.com
[Windows Universal Apps - Overlay integration]: ../mobile-engagement-windows-store-integrate-engagement-reach/#overlay-integration

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-store-dotnet-get-started/universal-app-creation.png
[2]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-capabilities.png
[3]: ./media/mobile-engagement-windows-store-dotnet-get-started/add-connection-info.png
[5]: ./media/mobile-engagement-windows-store-dotnet-get-started/manifest-toast.png
[6]: ./media/mobile-engagement-windows-store-dotnet-get-started/enter-credentials.png
[7]: ./media/mobile-engagement-windows-store-dotnet-get-started/associate-app-store.png
[8]: ./media/mobile-engagement-windows-store-dotnet-get-started/vs-suspend.png
[9]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_create_app.png
[10]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_app_name.png
[11]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push.png
[12]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_1.png
[13]: ./media/mobile-engagement-windows-store-dotnet-get-started/dashboard_services_push_creds.png
