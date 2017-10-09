---
title: "a Mobile Engagement bemutatóalkalmazást aaaAzure |} Microsoft Docs"
description: "Tudhatja meg, hol toodownload, hogyan toouse és Azure Mobile Engagement használatának előnyei hello bemutató alkalmazás"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: f624d5aa-254b-4ad0-96a3-f00e6c3a2c97
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2016
ms.author: piyushjo
ms.openlocfilehash: 9ff0df0d21e1bad6aff573db49304a55593df1c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-mobile-engagement-demo-app"></a>Azure Mobile Engagement bemutató alkalmazás
Azt az Azure Mobile Engagement bemutató alkalmazás közzététele **iOS**, **Android**, és **Windows** platformok toohelp meg toofind hasznos források és tudjon meg többet a Mobile Bevonási.

hello app nyújt segítséget:

* Könnyedén megtalálhatja a hasznos hivatkozások tooMobile Engagement erőforrásokhoz, mint a videók, dokumentáció, hello támogatási fórum, és ahol toogo tooraise kéri.
* Felhasználói élmény minta értesítések a Mobile Engagement tooget ötleteket saját mobilalkalmazás által támogatott.
* Hogyan használható egy referencia-megvalósítási toostudy tooimplement a Mobile Engagement a saját alkalmazásba. Megismerheti, hogy:
  
  * Elemzés adatainak gyűjtéséről.
  * Speciális notification típusú például megvalósítása *teljes képernyős közbeszúrt* vagy *előugró*.
  * Felmérésekkel, szavazások valósítják meg.
  * Csendes leküldéses adatok és leküldéses forgatókönyvek megvalósításához.   

## <a name="app-installation"></a>Alkalmazás telepítése
Ez az alkalmazás a következő alkalmazás-áruházak hello érhető el:

* **Univerzális Windows-bemutatóalkalmazást**:
  
  * Töltse le a hello hello alkalmazást [Windows alkalmazás-áruház](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).
  * hello app fejlesztette ki egy univerzális Windows 10-alkalmazásként. hello a forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).
* **iOS app bemutató**:
  
  * Töltse le a hello hello alkalmazást [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).
  * hello alkalmazás iOS Swift jött létre. hello a forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).
* **Android bemutatóalkalmazást**:
  
  * Töltse le a hello hello alkalmazást [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).
  * hello a forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).

![Univerzális Windows-bemutatóalkalmazást][1]

![iOS bemutató alkalmazás][2]
![Android bemutatóalkalmazást][3]

## <a name="usage"></a>Használat
Ez az alkalmazás a következő módokon hello használhatja:

**Hello alkalmazást az eszközön letölthető hello alkalmazás tárolóban található hivatkozásokra (korábbi):**

> [!IMPORTANT]
> Nem kell Azure-fiókkal vagy kell tooconnect hello app tooa háttérhálózatként. hello app függetlenül működik.
> 
> 

* Miután hello alkalmazást az eszközön, majd lépjen hello bal oldali menü toofind hello hasznos kapcsolatos források a Mobile Engagement hello hivatkozásokkal.
* Fel lett véve a hello [szolgáltatás RSS-hírcsatorna](https://aka.ms/azmerssfeed) az alkalmazásba, hogy még mindig frissíteni kapcsolatos hello legújabb frissítéseket.
* Az értesítések a Mobile Engagement által az egyes platformokon támogatott hello minta értesítési forgatókönyvek tooexperience hello típus keresztül is végrehajthatja. Ezek az értesítések lehet tapasztalt helyileg – Ez azt jelenti, hogy kattinthat hello képernyők tooshow hello gombok akkor hello értesítések tapasztalatain hello a Mobile Engagement platform azonos toosending hello értesítéseket.

![A Windows App menü][4]

![Az iOS App menü][5]
![Android App menü][6]

**Hello forráskódja letölthető hello GitHub hivatkozásokra (korábbi):**

* Hello forráskód letöltése után nyissa meg a hello megfelelő fejlesztői környezetben – az iOS, Android és a Windowshoz készült Visual Studio Android Studio xcode-ban.
* Ezután kövesse a [SDK-integráció lépéseken](mobile-engagement-windows-store-dotnet-get-started.md) , hogy meg tudja tooconnect az alkalmazás tooits saját Mobilmarketing háttér-példány.
  * Egy kapcsolati karakterláncot a hello app tooconfigure van szüksége.
  * Az alkalmazás tooconfigure hello leküldéses értesítési platform is kell.
* Láthatja, hogy hello alkalmazás maga a Mobile Engagement van tagolva. Ezért hello alkalmazás megnyitása után csatlakoztatásával toohello háttér, lesz képes toosee hello felhasználói munkamenet, tevékenységeket, eseményeket, és egyéb, a hello **figyelő** fülre.
* A saját a Mobile Engagement-példányból, helyi értesítések használata helyett is képes toosend értesítések toothis app lesz.
  
  * Itt is hozzáadhat az eszköz egy vizsgálati eszköz hello segítségével **Get hello Eszközazonosító** menüpont hello alkalmazásban. Ez lehetővé teszi egy eszköz azonosítója, amely majd regisztrál egy vizsgálati eszköz, a platform háttér-példányát.
    
    ![A Windows eszköz azonosítója][7]
    
    ![Eszközazonosító IOS][8]
    ![az Android eszköz azonosítója][9]

## <a name="key-features-of-hello-demo-app"></a>A legfontosabb jellemzők hello bemutató alkalmazás
* Már említettük, az alkalmazással, mivel vannak összes hello fontos erőforrásokat a Mobile Engagement az aktuális. A bal oldali menüben hello Lépkedjen végig hello hivatkozásokat.
* Alkalmazáson belüli out értesítések az egyes platformokon fedezheti. Ezek az értesítések kézbesítése **csak értesítés**, ahol hello értesítési kattintva egyszerűen megnyitja hello alkalmazás natív képernyőt (használatával **mély linking**) – vagy regisztrációja, mivel egy **webes bejelentés**, ahol biztosíthat további HTML-tartalmakat a Mobile Engagement vissza end hello toobe hello értesítési kattintáskor jelenik meg.
  
    ![Alkalmazáson belüli out értesítések][29]
* IOS rendszerű eszközökön hogy tooclose hello app toosee hello alkalmazáson belüli out vagy a rendszer a leküldéses értesítések. Megnézheti hello implementációja itt hozzáadása **akciógombok**, például hello azokat, amelyek felvett toothis az alkalmazáson belüli out értesítési *visszajelzés* és *megosztás* (úgy, hogy hello felhasználói is igénybe vehet származó önmagát hello értesítési művelet jobbra).
  
    ![Alkalmazáson belüli out értesítések iOS][11] ![IOS alkalmazáson belüli out értesítés megjelenítése][14]
* Az Android rendszerben támogatott hello-beállítások adunk többsoros (**nagy szöveg**) vagy egy értesítés (**nagy vonalakban tekinti**) toohello értesítési, valamint hello **akciógombok** (a által támogatott iOS).
  
    ![Alkalmazáson belüli out értesítések Android rendszeren][12] ![Az Android alkalmazáson belüli out értesítés megjelenítése][15]
* A Windows 10 láthatja, hogyan hello értesítések hello PC jelennek meg. Ezt az értesítést is megjelennek a Windows 10 hello **értesítési központba**. Nem támogatott hozzáadása **akciógombok** hello jelenleg a hello Windows SDK-t.
  
    ![Alkalmazáson belüli out értesítések Windows rendszeren][10] ![Alkalmazáson belüli out megjelenítési Windows rendszeren][13]
* Az egyes platformokon alapértelmezett "alkalmazásbeli" értesítések fedezheti. Ez az egy kétlépéses élmény ahol egy **értesítési** ablak akkor jelenik meg először. Kattintson rá, amikor tárja fel a teljes képernyős **közlemény**, mert a következő képernyőkép hello jelenik meg. Ezt a hirdetményt hello tartalmát a Mobile Engagement háttér-példány származik. hello SDK hello sablonokat értesítések és a hirdetményeket tartalmaz. Könnyen teste szabhatja őket, ez az embléma és színezés hello hozzáadását bemutató alkalmazás látható módon.  
  
    ![Az alkalmazásbeli értesítésekben Windows rendszeren][16]
  
    ![Alkalmazáson belüli értesítések iOS][17]  ![Az alkalmazásbeli értesítésekben Android rendszeren][18]
  
    **iOS**, **Android**
* Is használhatja a hello **kategória** a Mobile Engagement toocustomize szolgáltatása az alapértelmezett felhasználói élmény. Hello bemutatóalkalmazást, az azt korábban bemutatott két gyakori módszer toochange hello élmény hello értesítések. Vegye figyelembe, hogy adott hello kategória szolgáltatás még nem támogatott a Windows SDK hello.
  
    **Teljes képernyős közbeszúrt:**
  
    ![Az alkalmazáson belüli értesítés - közbeszúrt kategória][30]
  
    ![IOS közbeszúrt kategória][21]     ![Az Android közbeszúrt kategória][22]
  
    **Előugró értesítések:**
  
    ![Az alkalmazáson belüli értesítés - előugró kategória][31]
  
    ![IOS előugró értesítések][19]    ![Az Android előugró értesítések][20]

**iOS**, **Android**

* A Mobile Engagement is támogat egy speciális típusa az alkalmazáson belüli értesítés nevű **szavazások**. Ez lehetővé teszi toosend gyors felmérések tooyour szegmentált felhasználók ki. Kérdések és a beállítások a következő képernyőkép hello hasonlóan minden kérdés is hozzáadhat. Ez fogja majd beolvasása jelenik meg az alkalmazáson belüli értesítés toohello app felhasználóként.   
  
    ![Lekérdezési értesítések][32]
  
    ![Windows felmérése][26]
  
    ![IOS-felmérés][27]   ![Android felmérése][28]

**iOS**, **Android**

* A Mobile Engagement is támogatja csendes **adatok leküldéses** értesítések. Ezek az értesítések úgy küldhet adatokat a szolgáltatásból (például hello JSON-adatokat az alábbi példa hello), amely az alkalmazás kezelni, és bizonyos művelet végrehajtása. Példa: hogyan azt épp módosított cikk hello árának szelektív adatok leküldéses értesítések segítségével.
  
    ![Adatok leküldéses értesítés][33]
  
    ![Adatok leküldéses értesítés az Windows][23]
  
    ![Adatok leküldéses értesítések IOS rendszerű eszközökön][24]  ![Adatok leküldéses értesítések Android rendszeren][25]

**iOS**, **Android**

> [!NOTE]
> Ezek az értesítések további részletes útmutatásért kattintva megtekintheti **ide útmutatást toosend ezek az értesítések a Mobile Engagement platform** bármely minta értesítési képernyőn.
> 
> 

[1]: ./media/mobile-engagement-demo-apps/home-windows.png
[2]: ./media/mobile-engagement-demo-apps/home-ios.png
[3]: ./media/mobile-engagement-demo-apps/home-android.png
[4]: ./media/mobile-engagement-demo-apps/menu-windows.png
[5]: ./media/mobile-engagement-demo-apps/menu-ios.png
[6]: ./media/mobile-engagement-demo-apps/menu-android.png
[7]: ./media/mobile-engagement-demo-apps/device-id-windows.png
[8]: ./media/mobile-engagement-demo-apps/device-id-ios.png
[9]: ./media/mobile-engagement-demo-apps/device-id-android.png
[10]: ./media/mobile-engagement-demo-apps/out-of-app-windows.png
[11]: ./media/mobile-engagement-demo-apps/out-of-app-ios.png
[12]: ./media/mobile-engagement-demo-apps/out-of-app-android.png
[13]: ./media/mobile-engagement-demo-apps/out-of-app-display-windows.png
[14]: ./media/mobile-engagement-demo-apps/out-of-app-display-ios.png
[15]: ./media/mobile-engagement-demo-apps/out-of-app-display-android.png
[16]: ./media/mobile-engagement-demo-apps/in-app-windows.png
[17]: ./media/mobile-engagement-demo-apps/in-app-ios.png
[18]: ./media/mobile-engagement-demo-apps/in-app-android.png
[19]: ./media/mobile-engagement-demo-apps/pop-up-ios.png
[20]: ./media/mobile-engagement-demo-apps/pop-up-android.png
[21]: ./media/mobile-engagement-demo-apps/interstitial-ios.png
[22]: ./media/mobile-engagement-demo-apps/interstitial-android.png
[23]: ./media/mobile-engagement-demo-apps/data-push-windows.png
[24]: ./media/mobile-engagement-demo-apps/data-push-ios.png
[25]: ./media/mobile-engagement-demo-apps/data-push-android.png
[26]: ./media/mobile-engagement-demo-apps/survey-windows.png
[27]: ./media/mobile-engagement-demo-apps/survey-ios.png
[28]: ./media/mobile-engagement-demo-apps/survey-android.png
[29]: ./media/mobile-engagement-demo-apps/out-of-app.png
[30]: ./media/mobile-engagement-demo-apps/in-app-interstitial.png
[31]: ./media/mobile-engagement-demo-apps/in-app-pop-up.png
[32]: ./media/mobile-engagement-demo-apps/notification-poll.png
[33]: ./media/mobile-engagement-demo-apps/notification-data-push.png
