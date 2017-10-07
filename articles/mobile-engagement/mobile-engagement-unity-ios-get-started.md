---
title: "aaaGet elindítva az Azure Mobile Engagement Unity iOS üzemelő"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Unity-alkalmazásokhoz tooiOS eszközök telepítése."
services: mobile-engagement
documentationcenter: unity
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7ddfbac3-8d13-4ebe-b061-c865f357297f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-unity-ios
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: f4247b0a9240cbe2bf1a6618388919d3554c07fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-unity-ios-deployment"></a>Ismerkedés az Azure Mobile Engagement Unity iOS üzemelő példánnyal való használatával
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Unity-alkalmazás tooan iOS-eszköz telepítése során.
A oktatóanyag használ hello Unity klasszikus Roll golyó oktatóanyag, hello kiindulási pontjaként. Ezen hello lépéseket kell követnie [oktatóanyag](mobile-engagement-unity-roll-a-ball.md) hello a Mobile Engagement-integrációs oktatóanyaggal az alábbi hello oktatóanyag folytatása előtt. 

Ez az oktatóanyag hello következő szükséges:

* [Unity Editor](http://unity3d.com/get-unity)
* [Mobile Engagement Unity SDK](https://aka.ms/azmeunitysdk)
* XCode Editor

> [!NOTE]
> toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra. Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot. További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-unity-ios-get-started).
> 
> 

## <a id="setup-azme"></a>A Mobile Engagement beállítása az iOS-alkalmazáshoz
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
1. Nyissa meg hello **EngagementConfiguration** hello SDK mappából, és frissítse hello parancsfájlt **IOS\_kapcsolat\_karakterlánc** korábban beszerzett hello kapcsolati karakterlánccal a hello Azure-portálon.  
   
    ![][73]
2. Hello fájl mentéséhez. 

### <a name="configure-hello-app-for-basic-tracking"></a>Alapszintű nyomkövetéshez hello alkalmazás konfigurálása
1. Nyissa meg hello **PlayerController** toohello Player objektum szerkesztésre csatolt parancsprogram. 
2. Adja hozzá hello következő using utasítást:
   
        using Microsoft.Azure.Engagement.Unity;
3. Adja hozzá a következő toohello hello `Start()` módszer
   
        EngagementAgent.Initialize();
        EngagementAgent.StartActivity("Home");

### <a name="deploy-and-run-hello-app"></a>Regisztrálhat és futtathat hello alkalmazás
1. Csatlakoztassa az iOS eszköz tooyour gépet. 
2. Nyissa meg a **File -> Build Settings** (Fájl -> Létrehozási beállítások) menüpontot. 
   
    ![][40]
3. Válassza az **iOS** lehetőséget, majd kattintson a **Switch Platform** (Platformváltás) gombra.
   
    ![][41]
   
    ![][42]
4. Kattintson a **Player settings** (Lejátszó beállításai) lehetőségre, és adjon meg egy érvényes értéket a Bundle Identifier (Csomagazonosító) számára. 
   
    ![][53]
5. Végül kattintson a **Build And Run** (Létrehozás és futtatás) gombra.
   
    ![][54]
6. Előfordulhat, hogy ismételt tooprovide mappa neve toostore hello iOS csomagot. 
   
    ![][43]
7. Ha a művelet sikeres, hello projekt le lesz fordítva, és hogy megnyitható az XCode-alkalmazással a. 
8. Győződjön meg arról, hogy hello **csomagazonosítót** helyes hello projektben.  
   
    ![][75]
9. Most futtassa hello alkalmazást az xcode-ban, hogy hello csomag telepített tooyour csatlakoztatott eszközön, és a Unity-játék megjelenik a telefonon! 

## <a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése
Mobile Engagement lehetővé teszi a felhasználókkal toointeract és elérése révén a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok. Ez a modul REACH neve hello a Mobile Engagement portálon.
Nincs toodo további konfiguráció a alkalmazáson belüli tooreceive értesítések, és már állítva.

[!INCLUDE [mobile-engagement-ios-send-push-push](../../includes/mobile-engagement-ios-send-push.md)]

<!-- Images. -->
[40]: ./media/mobile-engagement-unity-ios-get-started/40.png
[41]: ./media/mobile-engagement-unity-ios-get-started/41.png
[42]: ./media/mobile-engagement-unity-ios-get-started/42.png
[43]: ./media/mobile-engagement-unity-ios-get-started/43.png
[53]: ./media/mobile-engagement-unity-ios-get-started/53.png
[54]: ./media/mobile-engagement-unity-ios-get-started/54.png
[70]: ./media/mobile-engagement-unity-ios-get-started/70.png
[71]: ./media/mobile-engagement-unity-ios-get-started/71.png
[72]: ./media/mobile-engagement-unity-ios-get-started/72.png
[73]: ./media/mobile-engagement-unity-ios-get-started/73.png
[74]: ./media/mobile-engagement-unity-ios-get-started/74.png
[75]: ./media/mobile-engagement-unity-ios-get-started/75.png
