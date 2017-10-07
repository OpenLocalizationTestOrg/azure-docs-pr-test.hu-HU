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
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a><span data-ttu-id="fae13-103">Ismerkedés az Azure Mobile Engagement Xamarin.Android-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="fae13-103">Get Started with Azure Mobile Engagement for Xamarin.Android Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="fae13-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Xamarin.Android-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="fae13-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of a Xamarin.Android application.</span></span>
<span data-ttu-id="fae13-105">Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="fae13-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="fae13-106">Az oktatóanyagban létrehoz egy üres Xamarin.Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="fae13-106">In it, you create a blank Xamarin.Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

> [!NOTE]
> <span data-ttu-id="fae13-107">hello Azure Mobile Engagement szolgáltatás március 2018 rendszerből, és jelenleg csak a rendelkezésre álló tooexisting ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="fae13-107">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="fae13-108">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="fae13-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="fae13-109">Ez az oktatóanyag hello következő szükséges:</span><span class="sxs-lookup"><span data-stu-id="fae13-109">This tutorial requires hello following:</span></span>

* <span data-ttu-id="fae13-110">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="fae13-110">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="fae13-111">Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja.</span><span class="sxs-lookup"><span data-stu-id="fae13-111">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="fae13-112">A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="fae13-112">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* [<span data-ttu-id="fae13-113">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="fae13-113">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="fae13-114">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="fae13-114">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="fae13-115">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="fae13-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fae13-116">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="fae13-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="fae13-117"><a id="setup-azme"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="fae13-117"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="fae13-118"><a id="connecting-app"></a>Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="fae13-118"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="fae13-119">Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="fae13-119">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> 

<span data-ttu-id="fae13-120">Létre fogunk hozni egy alapszintű alkalmazást a Xamarin Studio toodemonstrate hello integrációja.</span><span class="sxs-lookup"><span data-stu-id="fae13-120">We will create a basic app with Xamarin Studio toodemonstrate hello integration.</span></span>

### <a name="create-a-new-xamarinandroid-project"></a><span data-ttu-id="fae13-121">Új Xamarin.Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="fae13-121">Create a new Xamarin.Android project</span></span>
1. <span data-ttu-id="fae13-122">Indítsa el **Xamarin Studio** túl Ugrás**fájl** -> **új** -> **megoldás**</span><span class="sxs-lookup"><span data-stu-id="fae13-122">Launch **Xamarin Studio** Go too**File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="fae13-123">Válassza ki **Android-alkalmazás** majd ellenőrizze, hogy a kiválasztott hello nyelv **C#** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="fae13-123">Select **Android App** then make sure hello selected language is **C#** and click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="fae13-124">Adja meg a hello **alkalmazásnév** és hello **Organization Identifier**.</span><span class="sxs-lookup"><span data-stu-id="fae13-124">Fill in hello **App Name** and hello **Organization Identifier**.</span></span> <span data-ttu-id="fae13-125">Győződjön meg arról, hogy toocheckmark **Google Play-szolgáltatások** majd **következő**.</span><span class="sxs-lookup"><span data-stu-id="fae13-125">Make sure toocheckmark **Google Play Services** and then click **Next**.</span></span> 
   
    ![][3]
4. <span data-ttu-id="fae13-126">Frissítés hello **projektnevet**, **megoldás neve** és **hely** Ha szükséges, majd kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="fae13-126">Update hello **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="fae13-127">Xamarin Studio, amelyben integrálni fogjuk a Mobile Engagement hello alkalmazást hoz létre.</span><span class="sxs-lookup"><span data-stu-id="fae13-127">Xamarin Studio will create hello app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="fae13-128">Csatlakozás az alkalmazás tooMobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="fae13-128">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="fae13-129">Kattintson jobb gombbal a hello **csomagok** megoldás a windows hello, és válassza ki a mappát **csomagok hozzáadása...**</span><span class="sxs-lookup"><span data-stu-id="fae13-129">Right click hello **Packages** folder in hello Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="fae13-130">Keresse meg a hello **Microsoft Azure Mobile Engagement Xamarin SDK** , és adja hozzá tooyour megoldás.</span><span class="sxs-lookup"><span data-stu-id="fae13-130">Search for hello **Microsoft Azure Mobile Engagement Xamarin SDK** and add it tooyour solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="fae13-131">Nyissa meg **MainActivity.cs** és adja hozzá hello következő using utasításokat:</span><span class="sxs-lookup"><span data-stu-id="fae13-131">Open **MainActivity.cs** and add hello following using statements:</span></span>
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. <span data-ttu-id="fae13-132">A hello `OnCreate` módszer, adja hozzá a következő tooinitialize hello kapcsolat Mobile Engagement háttérrendszeréhez való hello.</span><span class="sxs-lookup"><span data-stu-id="fae13-132">In hello `OnCreate` method, add hello following tooinitialize hello connection with Mobile Engagement backend.</span></span> <span data-ttu-id="fae13-133">Győződjön meg arról, hogy tooadd a **ConnectionString**.</span><span class="sxs-lookup"><span data-stu-id="fae13-133">Make sure tooadd your **ConnectionString**.</span></span> 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="fae13-134">Engedélyek és szolgáltatásdeklaráció hozzáadása</span><span class="sxs-lookup"><span data-stu-id="fae13-134">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="fae13-135">Nyissa meg hello **Manifest.xml** fájl hello Properties mappában.</span><span class="sxs-lookup"><span data-stu-id="fae13-135">Open up hello **Manifest.xml** file under hello Properties folder.</span></span> <span data-ttu-id="fae13-136">Válassza ki (forrás) lapot, így hello XML-forrás közvetlen frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="fae13-136">Select Source tab so that you directly update hello XML source.</span></span>
2. <span data-ttu-id="fae13-137">Ezen engedélyek toohello Manifest.xml hozzáadása (amely hello található **tulajdonságok** mappa) a projekt azonnal előtt vagy után hello `<application>` címke:</span><span class="sxs-lookup"><span data-stu-id="fae13-137">Add these permissions toohello Manifest.xml (which can be found under hello **Properties** folder) of your project immediately before or after hello `<application>` tag:</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. <span data-ttu-id="fae13-138">Adja hozzá hello következő közötti hello `<application>` és `</application>` toodeclare hello ügynökszolgáltatás címkéket:</span><span class="sxs-lookup"><span data-stu-id="fae13-138">Add hello following between hello `<application>` and `</application>` tags toodeclare hello agent service:</span></span>
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. <span data-ttu-id="fae13-139">Csak a beillesztett hello kódot, cserélje le `"<Your application name>"` hello címke.</span><span class="sxs-lookup"><span data-stu-id="fae13-139">In hello code you just pasted, replace `"<Your application name>"` in hello label.</span></span> <span data-ttu-id="fae13-140">Ez jelenik meg a hello **beállítások** menü, ahol a felhasználók láthatják hello eszközön futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="fae13-140">This is displayed in hello **Settings** menu where users can see services running on hello device.</span></span> <span data-ttu-id="fae13-141">A label például hozzáadhat hello "Szolgáltatás" szót.</span><span class="sxs-lookup"><span data-stu-id="fae13-141">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="fae13-142">A képernyő tooMobile Engagement küldése</span><span class="sxs-lookup"><span data-stu-id="fae13-142">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="fae13-143">A sorrend toostart adatküldés és biztosítása hello felhasználók aktívak legalább egy képernyőt toohello Mobile Engagement háttérrendszeréhez el kell küldenie.</span><span class="sxs-lookup"><span data-stu-id="fae13-143">In order toostart sending data and ensuring hello users are active, you must send at least one screen toohello Mobile Engagement backend.</span></span> <span data-ttu-id="fae13-144">Mindezt-győződjön meg arról, hogy hello `MainActivity` örököl `EngagementActivity` helyett `Activity`.</span><span class="sxs-lookup"><span data-stu-id="fae13-144">For doing this - ensure that hello `MainActivity` inherits from `EngagementActivity` instead of `Activity`.</span></span>

    public class MainActivity : EngagementActivity

<span data-ttu-id="fae13-145">Ha az `EngagementActivity` elemtől nem lehet örökölni, akkor hozzá kell adnia a `.StartActivity` és `.EndActivity` metódust az `OnResume` és `OnPause` elemhez.</span><span class="sxs-lookup"><span data-stu-id="fae13-145">Alternatively, if you cannot inherit from `EngagementActivity` then you must add `.StartActivity` and `.EndActivity` methods in `OnResume` and `OnPause` respectively.</span></span>  

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

## <span data-ttu-id="fae13-146"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="fae13-146"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="fae13-147"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="fae13-147"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="fae13-148">A Mobile Engagement lehetővé teszi a toointeract, és a felhasználók ELÉRÉSÉT a leküldéses értesítések és alkalmazáson belüli üzenetekkel hello kampányok felhasználókat.</span><span class="sxs-lookup"><span data-stu-id="fae13-148">Mobile Engagement allows you toointeract with and REACH your users with push notifications and in-app messaging in hello context of campaigns.</span></span> <span data-ttu-id="fae13-149">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="fae13-149">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="fae13-150">a következő szakaszok hello állít be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="fae13-150">hello following sections sets up your app tooreceive them.</span></span>

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
