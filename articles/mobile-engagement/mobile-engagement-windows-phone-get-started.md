---
title: "aaaGet lépések az Azure Mobile Engagement a Windows Phone Silverlight-alkalmazásokhoz"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések."
services: mobile-engagement
documentationcenter: windows
author: piyushjo
manager: erikre
editor: 
ms.assetid: aa34692f-87f7-47c6-a20c-a1972750bc25
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: b39a838ab03217b2dc845cbf59d7bf8b094dac1f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-windows-phone-silverlight-apps"></a>Ismerkedés az Azure Mobile Engagement Windows Phone Silverlight-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és a küldési leküldéses értesítések toosegmented felhasználók Windows Phone Silverlight-alkalmazás.
Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement. Az oktatóanyagban létrehoz egy üres Windows Phone Silverlight-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Microsoft leküldéses értesítéseket kezelő szolgáltatása (MPNS) használatával.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

> [!NOTE]
> A Windows Phone 8.1-es és korábbi verziójú projektek nem támogatottak a Visual Studio 2017-ben.  További információ: [A Visual Studio 2017 platform célcsoportkezelése és kompatibilitása](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).

> [!NOTE]
> Ha a Windows Phone 8.1 (nem Silverlight) céloz meg, tekintse meg a toohello [univerzális Windows-oktatóanyag](mobile-engagement-windows-store-dotnet-get-started.md).
> 
> 

Ez az oktatóanyag hello következő szükséges:

* Visual Studio 2013
* [MicrosoftAzure.MobileEngagement] NuGet-csomag

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-windows-phone-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása a Windows Phone-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez. hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement Windows Phone SDK-integráció](mobile-engagement-windows-phone-sdk-overview.md)

Létre fogunk hozni egy alapszintű alkalmazást a Visual Studio toodemonstrate hello integrációja.

### <a name="create-a-new-windows-phone-silverlight-project"></a>Új Windows Phone Silverlight-projekt létrehozása
hello következő lépések azt feltételezik hello Visual Studio 2015 használatát, ha hello lépések hasonlóak a Visual Studio korábbi verzióiban. 

1. Indítsa el a Visual Studio és a hello **Home** képernyőn válassza ki **új projekt**.
2. Hello előugró ablakban válassza **Windows 8** -> **Windows Phone** -> **üres alkalmazás (Windows Phone Silverlight)**. Töltse ki a hello app **neve** és **megoldásnév**, és kattintson a **OK**.
   
    ![][1]
3. Dönthet úgy tootarget vagy **Windows Phone 8.0** vagy **Windows Phone 8.1**.

Most létrehozott egy új Windows Phone Silverlight-alkalmazást, amelybe integrálni fogjuk a hello Azure Mobile Engagement SDK-t.

### <a name="connect-your-app-toohello-mobile-engagement-backend"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
1. Telepítse a hello [MicrosoftAzure.MobileEngagement] nuget-csomagot a projektben.
2. Nyissa meg `WMAppManifest.xml` (hello Properties mappában), és győződjön meg arról, hogy hello következő deklarált (hozzá, ha azok nem) a hello `<Capabilities />` címke:
   
        <Capability Name="ID_CAP_NETWORKING" />
        <Capability Name="ID_CAP_IDENTITY_DEVICE" />
   
    ![][2]
3. Most illessze be a hello korábban a Mobile Engagement-alkalmazáshoz kimásolt kapcsolati karakterláncot, és illessze be hello `Resources\EngagementConfiguration.xml` fájl közötti hello `<connectionString>` és `</connectionString>` címkék:
   
    ![][3]
4. A hello `App.xaml.cs` fájlt:
   
    a. Adja hozzá a hello `using` utasítást:
   
            using Microsoft.Azure.Engagement;
   
    b. A hello SDK hello inicializálása `Application_Launching` módszert:
   
            private void Application_Launching(object sender, LaunchingEventArgs e)
            {
              EngagementAgent.Instance.Init();
            }
   
    c. Helyezze be a következő hello hello `Application_Activated`:
   
            private void Application_Activated(object sender, ActivatedEventArgs e)
            {
               EngagementAgent.Instance.OnActivated(e);
            }

## <a id="monitor"></a>Valós idejű figyelés engedélyezése
Rendelés toostart adatküldés és annak biztosítására, hogy hello felhasználók aktív, a el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.

1. A hello MainPage.xaml.cs, adja hozzá a hello `using` utasítást:
   
        using Microsoft.Azure.Engagement;
2. Cserélje le az alaposztály alaposztályát hello **MainPage**, amely előtt van **PhoneApplicationPage**, a **EngagementPage**.
   
        class MainPage : EngagementPage 
3. A `MainPage.xml` fájlban:
   
    a. Tooyour névtér-deklarációk hozzáadása:
   
            xmlns:engagement="clr-namespace:Microsoft.Azure.Engagement;assembly=Microsoft.Azure.Engagement.EngagementAgent.WP"
   
    b. Cserélje le `phone:PhoneApplicationPage` hello XML-címkenév a `engagement:EngagementPage`.

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
A Mobile Engagement lehetővé teszi a toointeract, és a felhasználókat leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel hello kampányok elérni. Ez a modul REACH neve hello a Mobile Engagement portálon.
hello alábbi szakaszok állítják be az alkalmazás tooreceive őket.

### <a name="enable-your-app-tooreceive-mpns-push-notifications"></a>Az alkalmazás tooreceive MPNS leküldéses értesítések engedélyezése
Adja hozzá az új képességek tooyour `WMAppManifest.xml` fájlt:

        ID_CAP_PUSH_NOTIFICATION
        ID_CAP_WEBBROWSERCOMPONENT

   ![][5]

### <a name="initialize-hello-reach-sdk"></a>Hello REACH SDK inicializálása
1. A `App.xaml.cs`, hívja `EngagementReach.Instance.Init();` a hello **Application_Launching** függvény hello ügynök inicializálása után:
   
        private void Application_Launching(object sender, LaunchingEventArgs e)
        {
           EngagementAgent.Instance.Init();
           EngagementReach.Instance.Init();
        }
2. A `App.xaml.cs`, hívja `EngagementReach.Instance.OnActivated(e);` a hello **Application_Activated** függvény hello ügynök inicializálása után:
   
        private void Application_Activated(object sender, ActivatedEventArgs e)
        {
           EngagementAgent.Instance.OnActivated(e);
           EngagementReach.Instance.OnActivated(e);
        }

Készen is van. Most ellenőrizzük, hogy ezt az alapszintű integrációt megfelelően végezte-e el.

## <a id="send"></a>Egy értesítési tooyour app küldése
[!INCLUDE [Create Windows Push campaign](../../includes/mobile-engagement-windows-push-campaign.md)]

Ekkor megjelenik egy értesítés az eszközön, amely megjelenik az alkalmazáson belüli értesítések, ha hello alkalmazás meg nyitva egy bejelentési értesítést hello hasonló, ellenkező esetben: 

![][6]

<!-- URLs. -->
[MicrosoftAzure.MobileEngagement]: http://go.microsoft.com/?linkid=9874664
[Mobile Engagement Windows Phone SDK documentation]: ../mobile-engagement-windows-phone-integrate-engagement/

<!-- Images. -->
[1]: ./media/mobile-engagement-windows-phone-get-started/project-properties.png
[2]: ./media/mobile-engagement-windows-phone-get-started/wmappmanifest-capabilities.png
[3]: ./media/mobile-engagement-windows-phone-get-started/add-connection-string.png
[5]: ./media/mobile-engagement-windows-phone-get-started/reach-capabilities.png
[6]: ./media/mobile-engagement-windows-phone-get-started/push-screenshot.png
