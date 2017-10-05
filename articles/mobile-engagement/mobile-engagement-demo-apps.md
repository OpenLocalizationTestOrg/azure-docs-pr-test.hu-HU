---
title: "Az Azure Mobile Engagement bemutatóalkalmazást |} Microsoft Docs"
description: "Ismerteti a letöltési helyét, használata és az Azure Mobile Engagement bemutatóalkalmazást használatának előnyei"
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
ms.openlocfilehash: 8381edb569e19a85c1259f7791b477cfa6e51ea3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-mobile-engagement-demo-app"></a><span data-ttu-id="8141c-103">Azure Mobile Engagement bemutató alkalmazás</span><span class="sxs-lookup"><span data-stu-id="8141c-103">Azure Mobile Engagement demo app</span></span>
<span data-ttu-id="8141c-104">Azt az Azure Mobile Engagement bemutató alkalmazás közzététele **iOS**, **Android**, és **Windows** segítséget nyújtanak hasznos forrásokat és tudjon meg többet a Mobile platformok Bevonási.</span><span class="sxs-lookup"><span data-stu-id="8141c-104">We've published an Azure Mobile Engagement demo app for **iOS**, **Android**, and **Windows** platforms to help you to find useful resources and learn more about Mobile Engagement.</span></span>

<span data-ttu-id="8141c-105">Az alkalmazás nyújt segítséget:</span><span class="sxs-lookup"><span data-stu-id="8141c-105">The app helps you to:</span></span>

* <span data-ttu-id="8141c-106">Könnyedén megtalálhatja a Mobile Engagement erőforrásokhoz, mint a videók, dokumentáció, a támogatási fórum és hol kell kiadni a funkciókra vonatkozó kérések hasznos hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="8141c-106">Easily find useful links to Mobile Engagement resources like videos, documentation, the support forum, and where to go to raise feature requests.</span></span>
* <span data-ttu-id="8141c-107">Felhasználói élmény minta értesítések a Mobile Engagement ötleteket saját mobilalkalmazás által támogatott.</span><span class="sxs-lookup"><span data-stu-id="8141c-107">Experience sample notifications that are supported by Mobile Engagement to get ideas for your own mobile applications.</span></span>
* <span data-ttu-id="8141c-108">A hivatkozás megvalósításának segítségével tanulmányozza a Mobile Engagement megvalósítható a saját alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="8141c-108">Use a reference implementation to study how to implement Mobile Engagement into your own app.</span></span> <span data-ttu-id="8141c-109">Megismerheti, hogy:</span><span class="sxs-lookup"><span data-stu-id="8141c-109">You can learn to:</span></span>
  
  * <span data-ttu-id="8141c-110">Elemzés adatainak gyűjtéséről.</span><span class="sxs-lookup"><span data-stu-id="8141c-110">Collect analytics data.</span></span>
  * <span data-ttu-id="8141c-111">Speciális notification típusú például megvalósítása *teljes képernyős közbeszúrt* vagy *előugró*.</span><span class="sxs-lookup"><span data-stu-id="8141c-111">Implement advanced notification scenarios of types such as *Full-screen interstitial* or *Pop-up*.</span></span>
  * <span data-ttu-id="8141c-112">Felmérésekkel, szavazások valósítják meg.</span><span class="sxs-lookup"><span data-stu-id="8141c-112">Implement surveys and polls.</span></span>
  * <span data-ttu-id="8141c-113">Csendes leküldéses adatok és leküldéses forgatókönyvek megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="8141c-113">Implement silent push data and push scenarios.</span></span>   

## <a name="app-installation"></a><span data-ttu-id="8141c-114">Alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="8141c-114">App installation</span></span>
<span data-ttu-id="8141c-115">Ez az alkalmazás a következő alkalmazás-áruházak érhető el:</span><span class="sxs-lookup"><span data-stu-id="8141c-115">This app is available in the following app stores:</span></span>

* <span data-ttu-id="8141c-116">**Univerzális Windows-bemutatóalkalmazást**:</span><span class="sxs-lookup"><span data-stu-id="8141c-116">**Windows Universal demo app**:</span></span>
  
  * <span data-ttu-id="8141c-117">Töltse le az alkalmazást a [Windows alkalmazás-áruház](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).</span><span class="sxs-lookup"><span data-stu-id="8141c-117">Download the app at the [Windows App store](https://www.microsoft.com/en-us/store/apps/azure-mobile-engagement/9nblggh4qmh2).</span></span>
  * <span data-ttu-id="8141c-118">Az alkalmazás egy univerzális Windows 10-alkalmazásként jött létre.</span><span class="sxs-lookup"><span data-stu-id="8141c-118">The app was developed as a Windows 10 Universal app.</span></span> <span data-ttu-id="8141c-119">A forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).</span><span class="sxs-lookup"><span data-stu-id="8141c-119">The source code is available on [GitHub](https://github.com/Azure/azure-mobile-engagement-app-windows).</span></span>
* <span data-ttu-id="8141c-120">**iOS app bemutató**:</span><span class="sxs-lookup"><span data-stu-id="8141c-120">**iOS demo app**:</span></span>
  
  * <span data-ttu-id="8141c-121">Töltse le az alkalmazást a [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).</span><span class="sxs-lookup"><span data-stu-id="8141c-121">Download the app at the [Apple store](https://itunes.apple.com/us/app/azure%20mobile%20engagement/id1105090090).</span></span>
  * <span data-ttu-id="8141c-122">Az alkalmazás iOS Swift jött létre.</span><span class="sxs-lookup"><span data-stu-id="8141c-122">The app was developed in iOS Swift.</span></span> <span data-ttu-id="8141c-123">A forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).</span><span class="sxs-lookup"><span data-stu-id="8141c-123">The source code is available on [GitHub](https://github.com/Azure/azure-mobile-engagement-app-ios).</span></span>
* <span data-ttu-id="8141c-124">**Android bemutatóalkalmazást**:</span><span class="sxs-lookup"><span data-stu-id="8141c-124">**Android demo app**:</span></span>
  
  * <span data-ttu-id="8141c-125">Töltse le az alkalmazást a [Google Play áruház](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).</span><span class="sxs-lookup"><span data-stu-id="8141c-125">Download the app at the [Google Play store](https://play.google.com/store/apps/details?id=com.microsoft.azure.engagement).</span></span>
  * <span data-ttu-id="8141c-126">A forráskód nem érhető el a [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).</span><span class="sxs-lookup"><span data-stu-id="8141c-126">The source code is available on [GitHub](https://github.com/Azure/azure-mobile-engagement-app-android).</span></span>

![Univerzális Windows-bemutatóalkalmazást][1]

<span data-ttu-id="8141c-128">![iOS bemutató alkalmazás][2]
![Android bemutatóalkalmazást][3]</span><span class="sxs-lookup"><span data-stu-id="8141c-128">![iOS demo app][2]
![Android demo app][3]</span></span>

## <a name="usage"></a><span data-ttu-id="8141c-129">Használat</span><span class="sxs-lookup"><span data-stu-id="8141c-129">Usage</span></span>
<span data-ttu-id="8141c-130">Ez az alkalmazás a következőképpen használhatja:</span><span class="sxs-lookup"><span data-stu-id="8141c-130">You can use this app in the following ways:</span></span>

<span data-ttu-id="8141c-131">**Töltse le az alkalmazást az eszközön az alkalmazás tárolóban található hivatkozásokra (korábbi):**</span><span class="sxs-lookup"><span data-stu-id="8141c-131">**Download the app on your device from the application store links (provided earlier):**</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8141c-132">Nem kell egy Azure-fiók és nem szükséges az alkalmazás csatlakoztatása a háttérből.</span><span class="sxs-lookup"><span data-stu-id="8141c-132">You don't need an Azure account or need to connect the app to a back end.</span></span> <span data-ttu-id="8141c-133">Az alkalmazás függetlenül működik.</span><span class="sxs-lookup"><span data-stu-id="8141c-133">The app works independently.</span></span>
> 
> 

* <span data-ttu-id="8141c-134">Miután az alkalmazást az eszközön, majd lépjen a Mobile Engagement kapcsolatos hasznos forrásokat a bal oldali menü-kapcsolaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="8141c-134">After you have the app on your device, then you can go through the links in the left-side menu to find the useful resources about Mobile Engagement.</span></span>
* <span data-ttu-id="8141c-135">Fel lett véve a [szolgáltatás RSS-hírcsatorna](https://aka.ms/azmerssfeed) az alkalmazásba, hogy még mindig frissítése a legújabb frissítéseket kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="8141c-135">We've added the [service's RSS feed](https://aka.ms/azmerssfeed) into this application so that you're always updated about the latest product updates.</span></span>
* <span data-ttu-id="8141c-136">Tapasztalhat az értesítések a Mobile Engagement által az egyes platformokon támogatott típusa mintaforgatókönyvek értesítési keresztül is végrehajthatja.</span><span class="sxs-lookup"><span data-stu-id="8141c-136">You can also go through the sample notification scenarios to experience the type of notifications that are supported by Mobile Engagement for each platform.</span></span> <span data-ttu-id="8141c-137">Ezek az értesítések is kell észlelte a helyi--, amely esetében a képernyőkön mutatjuk be, az értesítések gyakorlat, amely megegyezik az értesítések küldése a Mobile Engagement platformról gombokra.</span><span class="sxs-lookup"><span data-stu-id="8141c-137">These notifications can be experienced locally--that is, you can click the buttons on the screens to show you the notifications experience, which is identical to sending the notifications from the Mobile Engagement platform.</span></span>

![A Windows App menü][4]

<span data-ttu-id="8141c-139">![Az iOS App menü][5]
![Android App menü][6]</span><span class="sxs-lookup"><span data-stu-id="8141c-139">![App menu for iOS][5]
![App menu for Android][6]</span></span>

<span data-ttu-id="8141c-140">**Töltse le a forráskód a Githubon található hivatkozásokra (korábbi):**</span><span class="sxs-lookup"><span data-stu-id="8141c-140">**Download the source code from the GitHub links (provided earlier):**</span></span>

* <span data-ttu-id="8141c-141">A forráskód letöltése után nyissa meg a megfelelő fejlesztői környezetben – az iOS, Android és a Windowshoz készült Visual Studio Android Studio xcode-ban.</span><span class="sxs-lookup"><span data-stu-id="8141c-141">After you've downloaded the source code, open it in the respective development environment--XCode for iOS, Android Studio for Android, and Visual Studio for Windows.</span></span>
* <span data-ttu-id="8141c-142">Ezután kövesse a [SDK-integráció lépéseken](mobile-engagement-windows-store-dotnet-get-started.md) , hogy Ön az alkalmazás csatlakozni tudjanak a saját a Mobile Engagement háttér-példány.</span><span class="sxs-lookup"><span data-stu-id="8141c-142">You should next follow our [basic SDK integration steps](mobile-engagement-windows-store-dotnet-get-started.md) so that you're able to connect this app to its own Mobile Engagement back-end instance.</span></span>
  * <span data-ttu-id="8141c-143">Kell egy kapcsolati karakterlánc konfigurálása az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8141c-143">You need to configure a connection string in the app.</span></span>
  * <span data-ttu-id="8141c-144">Azt is konfigurálnia kell a leküldéses értesítési platform az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="8141c-144">You also need to configure the push notification platform for your app.</span></span>
* <span data-ttu-id="8141c-145">Láthatja, hogy az alkalmazás maga a Mobile Engagement van-e tagolva.</span><span class="sxs-lookup"><span data-stu-id="8141c-145">You'll notice that the app itself is instrumented with Mobile Engagement.</span></span> <span data-ttu-id="8141c-146">Ezért, azt a háttérben történő kapcsolódás után nyissa meg a alkalmazást, képes lesz a felhasználói munkamenet, tevékenységek, eseményeket és így tovább tekintheti meg a **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="8141c-146">Therefore, as you open the app after connecting it to the back end, you'll be able to see the user session, activities, events, and so on, on the **Monitor** tab.</span></span>
* <span data-ttu-id="8141c-147">Akkor is képes lesz az értesítések küldését az alkalmazás a saját a Mobile Engagement-példányból, helyi értesítések használata helyett.</span><span class="sxs-lookup"><span data-stu-id="8141c-147">You'll also be able to send notifications to this app from your own Mobile Engagement instance, instead of using local notifications.</span></span>
  
  * <span data-ttu-id="8141c-148">Itt is hozzáadhat az eszköz egy vizsgálati eszköz használatával a **az Eszközazonosító lekérése** menüpont az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="8141c-148">Here you can add your device as a test device by using the **Get the Device ID** menu item in the app.</span></span> <span data-ttu-id="8141c-149">Ez lehetővé teszi egy eszköz azonosítója, amely majd regisztrál egy vizsgálati eszköz, a platform háttér-példányát.</span><span class="sxs-lookup"><span data-stu-id="8141c-149">This gives you a device ID that you then register as a test device with your platform back-end instance.</span></span>
    
    ![A Windows eszköz azonosítója][7]
    
    <span data-ttu-id="8141c-151">![Eszközazonosító IOS][8]
    ![az Android eszköz azonosítója][9]</span><span class="sxs-lookup"><span data-stu-id="8141c-151">![Device ID on iOS][8]
![Device ID on Android][9]</span></span>

## <a name="key-features-of-the-demo-app"></a><span data-ttu-id="8141c-152">A bemutató alkalmazás a legfontosabb jellemzők</span><span class="sxs-lookup"><span data-stu-id="8141c-152">Key features of the demo app</span></span>
* <span data-ttu-id="8141c-153">Amint már említettük, az alkalmazással, vannak a legfontosabb erőforrásokat a Mobile Engagement az aktuális.</span><span class="sxs-lookup"><span data-stu-id="8141c-153">As mentioned earlier, with this app, you have all the key resources for Mobile Engagement in your hand.</span></span> <span data-ttu-id="8141c-154">A bal oldali menü Lépkedjen végig a hivatkozásokat.</span><span class="sxs-lookup"><span data-stu-id="8141c-154">You can go through the links on the left menu.</span></span>
* <span data-ttu-id="8141c-155">Alkalmazáson belüli out értesítések az egyes platformokon fedezheti.</span><span class="sxs-lookup"><span data-stu-id="8141c-155">You can experience out-of-app notifications for each platform.</span></span> <span data-ttu-id="8141c-156">Ezek az értesítések kézbesítése **csak értesítés**, ahol az értesítés kattintva egyszerűen megnyitja az alkalmazás natív képernyőt (használatával **mély linking**) – vagy a regisztrációja, mivel egy **webes bejelentés**, amennyiben a Mobile Engagement háttérből az értesítés történő kattintáskor megjelenítendő biztosíthat további HTML-tartalmakat.</span><span class="sxs-lookup"><span data-stu-id="8141c-156">These notifications can be delivered as **Notification only**, where clicking the notification simply opens up a native screen of the application (by using **deep linking**)--or as a **Web announcement**, where you can deliver additional HTML content from the Mobile Engagement back end to be displayed when the notification is clicked.</span></span>
  
    ![Alkalmazáson belüli out értesítések][29]
* <span data-ttu-id="8141c-158">IOS rendszerű eszközökön akkor zárja be az alkalmazás az alkalmazáson belüli out vagy a rendszer leküldéses értesítést.</span><span class="sxs-lookup"><span data-stu-id="8141c-158">On iOS, you have to close the app to see the out-of-app or system push notifications.</span></span> <span data-ttu-id="8141c-159">Megnézheti a megvalósítás itt hozzáadásának **akciógombok**, az alkalmazáson belüli out értesítésre adott megfelelően, például *visszajelzés* és *megosztás* (úgy, hogy a felhasználói is igénybe vehet magát az értesítést a művelet jobbra).</span><span class="sxs-lookup"><span data-stu-id="8141c-159">You can look at the implementation here for adding **Action buttons**, like the ones that are added to this out-of-app notification for *Feedback* and *Share* (so that the user can take action right from the notification itself).</span></span>
  
    ![Alkalmazáson belüli out értesítések iOS][11] ![IOS alkalmazáson belüli out értesítés megjelenítése][14]
* <span data-ttu-id="8141c-162">Az Android, a támogatott beállítások adunk többsoros (**nagy szöveg**) vagy egy értesítés (**nagy vonalakban tekinti**) az értesítésre, valamint a **akciógombok**(mivel az iOS által támogatott).</span><span class="sxs-lookup"><span data-stu-id="8141c-162">On Android, the options that are supported are adding multiline text (**Big Text**) or a notification image (**Big Picture**) to the notification, along with the **Action buttons** (as supported by iOS).</span></span>
  
    ![Alkalmazáson belüli out értesítések Android rendszeren][12] ![Az Android alkalmazáson belüli out értesítés megjelenítése][15]
* <span data-ttu-id="8141c-165">A Windows 10 láthatja, hogyan keresse meg az értesítéseket a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="8141c-165">On Windows 10, you can see how the notifications look on the PC.</span></span> <span data-ttu-id="8141c-166">Ezt az értesítést is megjelennek a Windows 10 **értesítési központba**.</span><span class="sxs-lookup"><span data-stu-id="8141c-166">This notification also shows up in the Windows 10 **Notification Center**.</span></span> <span data-ttu-id="8141c-167">Nem támogatott hozzáadása **akciógombok** jelenleg a Windows SDK-ban.</span><span class="sxs-lookup"><span data-stu-id="8141c-167">There is no support for adding **Action buttons** at the moment in the Windows SDK.</span></span>
  
    ![Alkalmazáson belüli out értesítések Windows rendszeren][10] ![Alkalmazáson belüli out megjelenítési Windows rendszeren][13]
* <span data-ttu-id="8141c-170">Az egyes platformokon alapértelmezett "alkalmazásbeli" értesítések fedezheti.</span><span class="sxs-lookup"><span data-stu-id="8141c-170">You can experience default "in-app" notifications for each platform.</span></span> <span data-ttu-id="8141c-171">Ez az egy kétlépéses élmény ahol egy **értesítési** ablak akkor jelenik meg először.</span><span class="sxs-lookup"><span data-stu-id="8141c-171">This is a two-step experience where a **Notification** window is displayed first.</span></span> <span data-ttu-id="8141c-172">Ha a hivatkozásra tárja fel a teljes képernyős **közlemény**, ahogy az alábbi képernyőképen látható.</span><span class="sxs-lookup"><span data-stu-id="8141c-172">When you click it, it opens up a full screen **Announcement**, as displayed in the following screenshot.</span></span> <span data-ttu-id="8141c-173">A közlemény tartalmát a Mobile Engagement háttér-példány származik.</span><span class="sxs-lookup"><span data-stu-id="8141c-173">The content of this announcement comes from your Mobile Engagement back-end instance.</span></span> <span data-ttu-id="8141c-174">Az SDK-val rendelkezik értesítések és a közlemények a sablonokat.</span><span class="sxs-lookup"><span data-stu-id="8141c-174">The SDK has the templates for both notifications and announcements.</span></span> <span data-ttu-id="8141c-175">Könnyen teste szabhatja őket, ez az embléma és színezés hozzáadását bemutató alkalmazás látható módon.</span><span class="sxs-lookup"><span data-stu-id="8141c-175">You can easily customize them, as shown in this demo app with the addition of our logo and coloring.</span></span>  
  
    ![Az alkalmazásbeli értesítésekben Windows rendszeren][16]
  
    ![Alkalmazáson belüli értesítések iOS][17]  ![Az alkalmazásbeli értesítésekben Android rendszeren][18]
  
    <span data-ttu-id="8141c-179">**iOS**, **Android**</span><span class="sxs-lookup"><span data-stu-id="8141c-179">**iOS**, **Android**</span></span>
* <span data-ttu-id="8141c-180">Használhatja a **kategória** a Mobile Engagement az alapértelmezett testreszabásához szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="8141c-180">You can also use the **Category** feature of Mobile Engagement to customize this default experience.</span></span> <span data-ttu-id="8141c-181">A bemutató alkalmazás azt korábban bemutatott módosítása értesítést élmény két gyakori módjai.</span><span class="sxs-lookup"><span data-stu-id="8141c-181">In the demo app, we've demonstrated two common ways to change the experience of the notifications.</span></span> <span data-ttu-id="8141c-182">Vegye figyelembe, hogy a kategória szolgáltatása még nem támogatott a Windows SDK-t.</span><span class="sxs-lookup"><span data-stu-id="8141c-182">Note that the Category feature is not yet supported in the Windows SDK.</span></span>
  
    <span data-ttu-id="8141c-183">**Teljes képernyős közbeszúrt:**</span><span class="sxs-lookup"><span data-stu-id="8141c-183">**Full-screen interstitial:**</span></span>
  
    ![Az alkalmazáson belüli értesítés - közbeszúrt kategória][30]
  
    ![IOS közbeszúrt kategória][21]     ![Az Android közbeszúrt kategória][22]
  
    <span data-ttu-id="8141c-187">**Előugró értesítések:**</span><span class="sxs-lookup"><span data-stu-id="8141c-187">**Pop-up notification:**</span></span>
  
    ![Az alkalmazáson belüli értesítés - előugró kategória][31]
  
    ![IOS előugró értesítések][19]    ![Az Android előugró értesítések][20]

<span data-ttu-id="8141c-191">**iOS**, **Android**</span><span class="sxs-lookup"><span data-stu-id="8141c-191">**iOS**, **Android**</span></span>

* <span data-ttu-id="8141c-192">A Mobile Engagement is támogat egy speciális típusa az alkalmazáson belüli értesítés nevű **szavazások**.</span><span class="sxs-lookup"><span data-stu-id="8141c-192">Mobile Engagement also supports a specialized type of in-app notification called **Polls**.</span></span> <span data-ttu-id="8141c-193">Ez lehetővé teszi, hogy a szegmentált felhasználók gyors felmérést küldhet.</span><span class="sxs-lookup"><span data-stu-id="8141c-193">This allows you to send out quick surveys to your segmented app users.</span></span> <span data-ttu-id="8141c-194">Kérdések és minden kérdés, ahogy az alábbi képernyőfelvételen a beállítások is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="8141c-194">You can add questions and options for each question as in the following screenshot.</span></span> <span data-ttu-id="8141c-195">Ez fogja majd beolvasása jelenik meg az alkalmazás felhasználó számára az alkalmazáson belüli értesítések.</span><span class="sxs-lookup"><span data-stu-id="8141c-195">This will then get displayed as an in-app notification to the app user.</span></span>   
  
    ![Lekérdezési értesítések][32]
  
    ![Windows felmérése][26]
  
    ![IOS-felmérés][27]   ![Android felmérése][28]

<span data-ttu-id="8141c-200">**iOS**, **Android**</span><span class="sxs-lookup"><span data-stu-id="8141c-200">**iOS**, **Android**</span></span>

* <span data-ttu-id="8141c-201">A Mobile Engagement is támogatja csendes **adatok leküldéses** értesítések.</span><span class="sxs-lookup"><span data-stu-id="8141c-201">Mobile Engagement also supports sending silent **Data Push** notifications.</span></span> <span data-ttu-id="8141c-202">Ezek az értesítések úgy küldhet adatokat a szolgáltatásból (például a JSON-adatokat az alábbi példában), amely az alkalmazás kezelni, és bizonyos művelet végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="8141c-202">With these notifications, you can send data from your service (like the JSON data in the following example), which you can handle in your app and take some action.</span></span> <span data-ttu-id="8141c-203">Példa: hogyan azt épp módosított cikk árának szelektív adatok leküldéses értesítések segítségével.</span><span class="sxs-lookup"><span data-stu-id="8141c-203">An example is how we're changing the price of an item selectively by using data push notifications.</span></span>
  
    ![Adatok leküldéses értesítés][33]
  
    ![Adatok leküldéses értesítés az Windows][23]
  
    ![Adatok leküldéses értesítések IOS rendszerű eszközökön][24]  ![Adatok leküldéses értesítések Android rendszeren][25]

<span data-ttu-id="8141c-208">**iOS**, **Android**</span><span class="sxs-lookup"><span data-stu-id="8141c-208">**iOS**, **Android**</span></span>

> [!NOTE]
> <span data-ttu-id="8141c-209">Ezek az értesítések további részletes útmutatásért kattintva megtekintheti **kattintson ide az értesítések küldése a Mobile Engagement platform útmutatást** bármely minta értesítési képernyőn.</span><span class="sxs-lookup"><span data-stu-id="8141c-209">You can view detailed step-by-step instructions for any of these notifications by clicking **Click here for instructions on how to send these notifications from Mobile Engagement platform** on any sample notification screen.</span></span>
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
