---
title: "az Azure Mobile Engagement a Xamarin.Android elindítva aaaGet"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Xamarin.Android-alkalmazásokhoz."
services: mobile-engagement
documentationcenter: xamarin
author: piyushjo
manager: erikre
editor: 
ms.assetid: fb68cf98-08a2-41b5-8e59-757469de3fe7
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 06/16/2016
ms.author: piyushjo
ms.openlocfilehash: 9d584fea8e8153d511258cf9b6f87f31dac6aeca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a>Ismerkedés az Azure Mobile Engagement Xamarin.Android-alkalmazásokkal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Xamarin.Android-alkalmazásba.
Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement. Az oktatóanyagban létrehoz egy üres Xamarin.Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.

> [!NOTE]
> hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek. További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Ez az oktatóanyag hello következő szükséges:

* [Xamarin Studio](http://xamarin.com/studio). Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja. A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).
* [Mobile Engagement Xamarin SDK](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez. 

Létre fogunk hozni egy alapszintű alkalmazást a Xamarin Studio toodemonstrate hello integrációja.

### <a name="create-a-new-xamarinandroid-project"></a>Új Xamarin.Android-projekt létrehozása
1. Indítsa el **Xamarin Studio** túl Ugrás**fájl** -> **új** -> **megoldás** 
   
    ![][1]
2. Válassza ki **Android-alkalmazás** majd ellenőrizze, hogy a kiválasztott hello nyelv **C#** kattintson **következő**.
   
    ![][2]
3. Adja meg a hello **alkalmazásnév** és hello **Organization Identifier**. Győződjön meg arról, hogy toocheckmark **Google Play-szolgáltatások** majd **következő**. 
   
    ![][3]
4. Frissítés hello **projektnevet**, **megoldás neve** és **hely** Ha szükséges, majd kattintson a **létrehozása**.
   
    ![][4]

Xamarin Studio, amelyben integrálni fogjuk a Mobile Engagement hello alkalmazást hoz létre. 

### <a name="connect-your-app-toomobile-engagement-backend"></a>Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez
1. Kattintson jobb gombbal a hello **csomagok** megoldás a windows hello, és válassza ki a mappát **csomagok hozzáadása...**
   
    ![][5]
2. Keresse meg a hello **Microsoft Azure Mobile Engagement Xamarin SDK** , és adja hozzá tooyour megoldás.  
   
    ![][6]
3. Nyissa meg **MainActivity.cs** és adja hozzá hello következő using utasításokat:
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. A hello `OnCreate` módszer, adja hozzá a következő tooinitialize hello kapcsolat Mobile Engagement háttérrendszeréhez való hello. Győződjön meg arról, hogy tooadd a **ConnectionString**. 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a>Engedélyek és szolgáltatásdeklaráció hozzáadása
1. Nyissa meg hello **Manifest.xml** fájl hello Properties mappában. Válassza ki (forrás) lapot, így hello XML-forrás közvetlen frissítéséhez.
2. Ezen engedélyek toohello Manifest.xml hozzáadása (amely hello található **tulajdonságok** mappa) a projekt azonnal előtt vagy után hello `<application>` címke:
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. Adja hozzá hello következő közötti hello `<application>` és `</application>` toodeclare hello ügynökszolgáltatás címkéket:
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. Csak a beillesztett hello kódot, cserélje le `"<Your application name>"` hello címke. Ez jelenik meg a hello **beállítások** menü, ahol a felhasználók láthatják hello eszközön futó szolgáltatásokat. A label például hozzáadhat hello "Szolgáltatás" szót.

### <a name="send-a-screen-toomobile-engagement"></a>A képernyő tooMobile Engagement küldése
A sorrend toostart adatküldés és biztosítása hello felhasználók aktívak legalább egy képernyőt toohello Mobile Engagement háttérrendszeréhez el kell küldenie. Mindezt-győződjön meg arról, hogy hello `MainActivity` örököl `EngagementActivity` helyett `Activity`.

    public class MainActivity : EngagementActivity

Ha az `EngagementActivity` elemtől nem lehet örökölni, akkor hozzá kell adnia a `.StartActivity` és `.EndActivity` metódust az `OnResume` és `OnPause` elemhez.  

        protected override void OnResume()
            {
                EngagementAgent.StartActivity(EngagementAgentUtils.BuildEngagementActivityName(Java.Lang.Class.FromType(this.GetType())), null);
                base.OnResume();             
            }

            protected override void OnPause()
            {
                EngagementAgent.EndActivity();
                base.OnPause();            
            }

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
A Mobile Engagement lehetővé teszi a toointeract, és a felhasználók ELÉRÉSÉT a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok felhasználókat. Ez a modul REACH neve hello a Mobile Engagement portálon.
a következő szakaszok hello állít be az alkalmazás tooreceive őket.

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[1]: ./media/mobile-engagement-xamarin-android-get-started/1.png
[2]: ./media/mobile-engagement-xamarin-android-get-started/2.png
[3]: ./media/mobile-engagement-xamarin-android-get-started/3.png
[4]: ./media/mobile-engagement-xamarin-android-get-started/4.png
[5]: ./media/mobile-engagement-xamarin-android-get-started/5.png
[6]: ./media/mobile-engagement-xamarin-android-get-started/6.png
