---
title: "Ismerkedés az Azure Mobile Engagement Xamarin.Android-alkalmazásokkal való használatával"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement a Xamarin.Android-alkalmazásokhoz kapcsolódó elemzésekkel és leküldéses értesítésekkel."
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
ms.openlocfilehash: 7b3d01b32c2d5a40448fc22861cd45f612238f2f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-xamarinandroid-apps"></a><span data-ttu-id="9a1d0-103">Ismerkedés az Azure Mobile Engagement Xamarin.Android-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="9a1d0-103">Get Started with Azure Mobile Engagement for Xamarin.Android Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="9a1d0-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazás használatának megértéséhez, valamint leküldéses értesítések Xamarin.Android-alkalmazásba történő küldéséhez a szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and how to send push notifications to segmented users of a Xamarin.Android application.</span></span>
<span data-ttu-id="9a1d0-105">Ez az oktatóanyag a Mobile Engagementet használó egyszerű küldési forgatókönyvet mutat be.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="9a1d0-106">Az oktatóanyagban létrehoz egy üres Xamarin.Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-106">In it, you create a blank Xamarin.Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

> [!NOTE]
> <span data-ttu-id="9a1d0-107">Az Azure Mobile Engagement szolgáltatást 2018 márciusától megszüntetjük, és jelenleg csak meglévő ügyfelek számára érhető el.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-107">The Azure Mobile Engagement service will be retired March 2018 and is currently only available to existing customers.</span></span> <span data-ttu-id="9a1d0-108">További információkért lásd: [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="9a1d0-108">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="9a1d0-109">Az oktatóanyaghoz az alábbiakra lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="9a1d0-109">This tutorial requires the following:</span></span>

* <span data-ttu-id="9a1d0-110">[Xamarin Studio](http://xamarin.com/studio).</span><span class="sxs-lookup"><span data-stu-id="9a1d0-110">[Xamarin Studio](http://xamarin.com/studio).</span></span> <span data-ttu-id="9a1d0-111">Használhatja a Visual Studiót is a Xamarinhoz, de ez az oktatóanyag a Xamarin Studiót használja.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-111">You can also use Visual Studio with Xamarin but this tutorial uses Xamarin Studio.</span></span> <span data-ttu-id="9a1d0-112">A telepítési útmutatás itt található: [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) (A Visual Studio és a Xamarin beállítása és telepítése).</span><span class="sxs-lookup"><span data-stu-id="9a1d0-112">For installation instructions, see [Setup and Install for Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>
* [<span data-ttu-id="9a1d0-113">Mobile Engagement Xamarin SDK</span><span class="sxs-lookup"><span data-stu-id="9a1d0-113">Mobile Engagement Xamarin SDK</span></span>](https://www.nuget.org/packages/Microsoft.Azure.Engagement.Xamarin/)

> [!NOTE]
> <span data-ttu-id="9a1d0-114">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-114">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="9a1d0-115">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="9a1d0-116">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="9a1d0-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-xamarin-android-get-started).</span></span>
> 
> 

## <span data-ttu-id="9a1d0-117"><a id="setup-azme"></a>A Mobile Engagement beállítása az Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="9a1d0-117"><a id="setup-azme"></a>Setup Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="9a1d0-118"><a id="connecting-app"></a>Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="9a1d0-118"><a id="connecting-app"></a>Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="9a1d0-119">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-119">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> 

<span data-ttu-id="9a1d0-120">Létre fogunk hozni egy alapszintű alkalmazást a Xamarin Studio segítségével az integráció bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-120">We will create a basic app with Xamarin Studio to demonstrate the integration.</span></span>

### <a name="create-a-new-xamarinandroid-project"></a><span data-ttu-id="9a1d0-121">Új Xamarin.Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="9a1d0-121">Create a new Xamarin.Android project</span></span>
1. <span data-ttu-id="9a1d0-122">Indítsa el a **Xamarin Studiót**, lépjen a **File** -> **New** -> **Solution** (Fájl > Új > Megoldás) lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-122">Launch **Xamarin Studio** Go to **File** -> **New** -> **Solution**</span></span> 
   
    ![][1]
2. <span data-ttu-id="9a1d0-123">Válassza az **Android App** (Android-alkalmazás) lehetőséget, majd győződjön meg arról, hogy a választott nyelv a **C#**, és kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-123">Select **Android App** then make sure the selected language is **C#** and click **Next**.</span></span>
   
    ![][2]
3. <span data-ttu-id="9a1d0-124">Töltse ki az **App Name** (Alkalmazás neve) és az **Organization Identifier** (Szervezeti azonosító) mezőt.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-124">Fill in the **App Name** and the **Organization Identifier**.</span></span> <span data-ttu-id="9a1d0-125">Jelölje be a **Google Play Services** (Google Play-szolgáltatások) jelölőnégyzetet, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-125">Make sure to checkmark **Google Play Services** and then click **Next**.</span></span> 
   
    ![][3]
4. <span data-ttu-id="9a1d0-126">Frissítse a **Project Name** (Projekt neve), a **Solution Name** (Megoldás neve) és a **Location** (Hely) értékét, ha szükséges, majd kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-126">Update the **Project Name**, **Solution Name** and **Location** if required and click **Create**.</span></span>
   
    ![][4]

<span data-ttu-id="9a1d0-127">A Xamarin Studio létrehozza az alkalmazást, amelybe integrálni fogjuk a Mobile Engagementet.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-127">Xamarin Studio will create the app in which we will integrate Mobile Engagement.</span></span> 

### <a name="connect-your-app-to-mobile-engagement-backend"></a><span data-ttu-id="9a1d0-128">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="9a1d0-128">Connect your app to Mobile Engagement backend</span></span>
1. <span data-ttu-id="9a1d0-129">Kattintson a jobb gombbal a **Packages** mappára a Solution (Megoldás) ablakban, és válassza az **Add Packages...** (Csomagok hozzáadása...) lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-129">Right click the **Packages** folder in the Solution windows and select **Add Packages...**</span></span>
   
    ![][5]
2. <span data-ttu-id="9a1d0-130">Keresse meg a **Microsoft Azure Mobile Engagement Xamarin SDK**-t, és adja hozzá a megoldásához.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-130">Search for the **Microsoft Azure Mobile Engagement Xamarin SDK** and add it to your solution.</span></span>  
   
    ![][6]
3. <span data-ttu-id="9a1d0-131">Nyissa meg a **MainActivity.cs** fájlt, és adja hozzá a következő using utasításokat:</span><span class="sxs-lookup"><span data-stu-id="9a1d0-131">Open **MainActivity.cs** and add the following using statements:</span></span>
   
        using Microsoft.Azure.Engagement;
        using Microsoft.Azure.Engagement.Activity;
4. <span data-ttu-id="9a1d0-132">Az `OnCreate` metódusban adja hozzá a következőt a Mobile Engagement háttérrendszerhez való csatlakozás inicializálásához.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-132">In the `OnCreate` method, add the following to initialize the connection with Mobile Engagement backend.</span></span> <span data-ttu-id="9a1d0-133">Győződjön meg arról, hogy hozzáadta a **ConnectionString** karakterláncot.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-133">Make sure to add your **ConnectionString**.</span></span> 
   
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.ConnectionString = "YourConnectionStringFromAzurePortal";
        EngagementAgent.Init(engagementConfiguration);

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="9a1d0-134">Engedélyek és szolgáltatásdeklaráció hozzáadása</span><span class="sxs-lookup"><span data-stu-id="9a1d0-134">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="9a1d0-135">Nyissa meg a **Manifest.xml** fájlt a Properties mappában.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-135">Open up the **Manifest.xml** file under the Properties folder.</span></span> <span data-ttu-id="9a1d0-136">Válassza ki a Source (Forrás) lapot az XML-forrás közvetlen frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-136">Select Source tab so that you directly update the XML source.</span></span>
2. <span data-ttu-id="9a1d0-137">Adja hozzá a projekt Manifest.xml fájljához (amely a **Properties** mappában található) a következő engedélyeket, közvetlenül az `<application>` címke előtt vagy után:</span><span class="sxs-lookup"><span data-stu-id="9a1d0-137">Add these permissions to the Manifest.xml (which can be found under the **Properties** folder) of your project immediately before or after the `<application>` tag:</span></span>
   
        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
3. <span data-ttu-id="9a1d0-138">Adja hozzá a következőt az `<application>` és az `</application>` címke között az ügynökszolgáltatás deklarálásához:</span><span class="sxs-lookup"><span data-stu-id="9a1d0-138">Add the following between the `<application>` and `</application>` tags to declare the agent service:</span></span>
   
        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
4. <span data-ttu-id="9a1d0-139">A beillesztett kódban adja meg a megfelelő `"<Your application name>"` nevet a label értékeként.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-139">In the code you just pasted, replace `"<Your application name>"` in the label.</span></span> <span data-ttu-id="9a1d0-140">A név a **Settings** (Beállítások) menüben jelenik meg, amelyben a felhasználók megtekinthetik az eszközön futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-140">This is displayed in the **Settings** menu where users can see services running on the device.</span></span> <span data-ttu-id="9a1d0-141">A label értékeként megadhatja például a „Szolgáltatás” szót.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-141">You can add the word "Service" in that label for example.</span></span>

### <a name="send-a-screen-to-mobile-engagement"></a><span data-ttu-id="9a1d0-142">Képernyő küldése a Mobile Engagement számára</span><span class="sxs-lookup"><span data-stu-id="9a1d0-142">Send a screen to Mobile Engagement</span></span>
<span data-ttu-id="9a1d0-143">Az adatok küldésének megkezdéséhez és annak biztosításához, hogy a felhasználók aktívak, legalább egy képernyőt el kell küldenie a Mobile Engagement háttérrendszere számára.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-143">In order to start sending data and ensuring the users are active, you must send at least one screen to the Mobile Engagement backend.</span></span> <span data-ttu-id="9a1d0-144">Ennek elvégzéséhez biztosítsa, hogy a `MainActivity` az `EngagementActivity` elemtől örököl az `Activity` helyett.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-144">For doing this - ensure that the `MainActivity` inherits from `EngagementActivity` instead of `Activity`.</span></span>

    public class MainActivity : EngagementActivity

<span data-ttu-id="9a1d0-145">Ha az `EngagementActivity` elemtől nem lehet örökölni, akkor hozzá kell adnia a `.StartActivity` és `.EndActivity` metódust az `OnResume` és `OnPause` elemhez.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-145">Alternatively, if you cannot inherit from `EngagementActivity` then you must add `.StartActivity` and `.EndActivity` methods in `OnResume` and `OnPause` respectively.</span></span>  

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

## <span data-ttu-id="9a1d0-146"><a id="monitor"></a>Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="9a1d0-146"><a id="monitor"></a>Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <span data-ttu-id="9a1d0-147"><a id="integrate-push"></a>Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="9a1d0-147"><a id="integrate-push"></a>Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="9a1d0-148">A Mobile Engagement lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók elérését a kampányok részeként megjelenő leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-148">Mobile Engagement allows you to interact with and REACH your users with push notifications and in-app messaging in the context of campaigns.</span></span> <span data-ttu-id="9a1d0-149">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-149">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="9a1d0-150">Az alábbi szakaszok állítják be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="9a1d0-150">The following sections sets up your app to receive them.</span></span>

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
