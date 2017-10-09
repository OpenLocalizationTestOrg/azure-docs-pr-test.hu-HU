---
title: "aaaGet elindítva az Azure Mobile Engagement Unity Android üzemelő"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Unity-alkalmazásokhoz tooiOS eszközök telepítése."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: d5f0ef79-be00-4cec-97a5-a0b2fdaa380e
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-android
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: c4d34691daeb7544b11c2d6895b2474af0f902b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-android-deployment"></a>Ismerkedés az Azure Mobile Engagement Unity Android üzemelő példánnyal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Unity-alkalmazás tooan Android-eszköz telepítése során.
A oktatóanyag használ hello Unity klasszikus Roll golyó oktatóanyag, hello kiindulási pontjaként. Ezen hello lépéseket kell követnie [oktatóanyag](mobile-engagement-unity-roll-a-ball.md) hello a Mobile Engagement-integrációs oktatóanyaggal az alábbi hello oktatóanyag folytatása előtt. 

Ez az oktatóanyag hello következő szükséges:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* Google Android SDK

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-android-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez
### <a name="import-hello-unity-package"></a>Hello Unity-csomag importálása
1. Töltse le a hello [Mobile Engagement Unity-csomagot](https://aka.ms/azmeunitysdk) , és mentse tooyour helyi számítógép. 
2. Nyissa meg túl**eszközök -> csomag importálása -> egyéni csomag** és a fenti lépés hello letöltött válassza hello csomagot. 
   
    ![][70] 
3. Ügyeljen arra, hogy minden fájl ki legyen választva, és kattintson az **Import** (Importálás) gombra. 
   
    ![][71] 
4. Ha sikeres importálás, látni fogja hello importált SDK-fájlok a projektben.  
   
    ![][72] 

### <a name="update-hello-engagementconfiguration"></a>Hello EngagementConfiguration frissítése
1. Nyissa meg hello **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **ANDROID\_kapcsolat\_karakterlánc** kapott hello kapcsolati karakterlánccal korábban a hello Azure-portálon.  
   
    ![][73]
2. Hello fájl mentése 
3. Futtassa a következőt: **File -> Engagement -> Generate Android Manifest** (Fájl -> Engagement -> Android-jegyzék létrehozása). Ez a hello Mobile Engagement SDK által hozzáadott hello beépülő modul, és azt automatikusan frissíti a projektbeállításokat. 
   
    ![][74]

> [!IMPORTANT]
> Győződjön meg arról, hogy tooexecute Ez minden egyes hello frissítése **EngagementConfiguration** fájlból, egyébként pedig a módosítások nem tükröződnek hello alkalmazásban. 
> 
> 

### <a name="configure-hello-app-for-basic-tracking"></a>Alapszintű nyomkövetéshez hello alkalmazás konfigurálása
1. Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram. 
2. Adja hozzá hello következő using utasítást:
   
        using Microsoft.Azure.Engagement.Unity;
3. Adja hozzá a következő toohello hello `Start()` módszer
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a>Regisztrálhat és futtathat hello alkalmazás
Győződjön meg arról, hogy rendelkezik-e telepítve a számítógépre, mielőtt megpróbálná toodeploy a Unity-alkalmazás tooyour eszköz Android SDK-t. 

1. Csatlakoztassa az Android-eszköz tooyour gépet. 
2. Nyissa meg a **File -> Build Settings** (Fájl -> Létrehozási beállítások) menüpontot. 
   
    ![][40]
3. Válassza az **Android** lehetőséget, majd kattintson a **Switch Platform** (Platformváltás) gombra.
   
    ![][51]
   
    ![][52]
4. Kattintson a **Player settings** (Lejátszó beállításai) lehetőségre, és adjon meg egy érvényes értéket a Bundle Identifier (Csomagazonosító) számára. 
   
    ![][53]
5. Végül kattintson a **Build And Run** (Létrehozás és futtatás) gombra.
   
    ![][54]
6. Előfordulhat, hogy ismételt tooprovide mappa neve toostore hello Android csomagot. 
7. Ha minden megfelelően konfigurálva, akkor hello csomag lesz csatlakoztatva telepített tooyour eszközt, és megjelenik a Unity-játék a telefonon! 

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

### <a name="update-hello-engagementconfiguration"></a>Hello EngagementConfiguration frissítése
1. Hello nyissa meg **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **ANDROID\_GOOGLE\_szám** a hello **Google-projekt Szám** hello Google Cloud Developer portálról korábban beszerzett. Ez egy értéket, ezért győződjön meg arról, hogy tooenclose legyen idézőjelek közé foglalt. 
   
    ![][75]
2. Hello fájl mentéséhez. 
3. Futtassa a következőt: **File -> Engagement -> Generate Android Manifest** (Fájl -> Engagement -> Android-jegyzék létrehozása). Ez a hello Mobile Engagement SDK által hozzáadott hello beépülő modul, és azt automatikusan frissíti a projektbeállításokat. 
   
    ![][74]

### <a name="configure-hello-app-tooreceive-notifications"></a>Hello app tooreceive értesítések konfigurálása
1. Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram. 
2. Adja hozzá a következő toohello hello `Start()` módszer
   
        EngagementReachAgent.Initialize();
3. Most, hogy hello app frissül, telepítése és hello alkalmazást futtatják egy eszközön / hello alábbi utasítások szerint. 

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

<!-- Images -->
[40]: ./media/mobile-engagement-unity-android-get-started/40.png
[70]: ./media/mobile-engagement-unity-android-get-started/70.png
[71]: ./media/mobile-engagement-unity-android-get-started/71.png
[72]: ./media/mobile-engagement-unity-android-get-started/72.png
[73]: ./media/mobile-engagement-unity-android-get-started/73.png
[74]: ./media/mobile-engagement-unity-android-get-started/74.png
[75]: ./media/mobile-engagement-unity-android-get-started/75.png
[51]: ./media/mobile-engagement-unity-android-get-started/51.png
[52]: ./media/mobile-engagement-unity-android-get-started/52.png
[53]: ./media/mobile-engagement-unity-android-get-started/53.png
[54]: ./media/mobile-engagement-unity-android-get-started/54.png
