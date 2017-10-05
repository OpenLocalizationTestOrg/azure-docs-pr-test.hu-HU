---
title: "Ismerkedés az Azure Mobile Engagement Android-alkalmazásokkal való használatával"
description: "Ismerje meg, hogyan használható az Azure Mobile Engagement az Android-alkalmazásokhoz kapcsolódó elemzések és leküldéses értesítések tekintetében."
services: mobile-engagement
documentationcenter: android
author: piyushjo
manager: erikre
editor: 
ms.assetid: 3c286c6d-cfef-4e3e-9b2c-715429fe82db
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: hero-article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: dc255a930bf71e6ef6d964bc5e3472a38ce4e467
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="c213c-103">Ismerkedés az Azure Mobile Engagement Android-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="c213c-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="c213c-104">Ebben a témakörben elsajátíthatja, hogy miként használható az Azure Mobile Engagement az alkalmazás használatának megértéséhez, valamint leküldéses értesítések Android-alkalmazásba történő küldéséhez a szegmentált felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="c213c-104">This topic shows you how to use Azure Mobile Engagement to understand your app usage and how to send push notifications to segmented users of an Android application.</span></span>
<span data-ttu-id="c213c-105">Ez az oktatóanyag a Mobile Engagementet használó egyszerű küldési forgatókönyvet mutat be.</span><span class="sxs-lookup"><span data-stu-id="c213c-105">This tutorial demonstrates the simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="c213c-106">Az oktatóanyagban létrehoz egy üres Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="c213c-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c213c-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="c213c-107">Prerequisites</span></span>
<span data-ttu-id="c213c-108">Az oktatóprogram elvégzéséhez szükség lesz az [Android Developer Tools](https://developer.android.com/sdk/index.html) eszközökre, amelyek az Android Studio integrált fejlesztőkörnyezetet és a legújabb Android platformot foglalják magukban.</span><span class="sxs-lookup"><span data-stu-id="c213c-108">Completing this tutorial requires the [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes the Android Studio integrated development environment, and the latest Android platform.</span></span>

<span data-ttu-id="c213c-109">Szükséges hozzá a [Mobile Engagement Android SDK](https://aka.ms/vq9mfn) is.</span><span class="sxs-lookup"><span data-stu-id="c213c-109">It also requires the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c213c-110">Az oktatóanyag elvégzéséhez egy aktív Azure-fiókra lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="c213c-110">To complete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="c213c-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="c213c-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="c213c-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="c213c-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="c213c-113">A Mobile Engagement beállítása az Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="c213c-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-to-the-mobile-engagement-backend"></a><span data-ttu-id="c213c-114">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="c213c-114">Connect your app to the Mobile Engagement backend</span></span>
<span data-ttu-id="c213c-115">Ez az oktatóanyag egy „alapszintű integrációt” mutat be, ami minimálisan szükséges az adatok gyűjtéséhez és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="c213c-115">This tutorial presents a "basic integration", which is the minimal set required to collect data and send a push notification.</span></span> <span data-ttu-id="c213c-116">Létre fog hozni egy alapszintű alkalmazást az Android Studio segítségével az integráció bemutatásához.</span><span class="sxs-lookup"><span data-stu-id="c213c-116">You create a basic app with Android Studio to demonstrate the integration.</span></span>

<span data-ttu-id="c213c-117">A teljes integrációs dokumentáció itt található: [Mobile Engagement Android SDK-integráció](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c213c-117">The complete integration documentation can be found in the [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="c213c-118">Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="c213c-118">Create an Android project</span></span>
1. <span data-ttu-id="c213c-119">Indítsa el az **Android Studio** alkalmazást, majd az előugró ablakban válassza a **Start a new Android Studio project** (Új Android Studio-projekt indítása) elemet.</span><span class="sxs-lookup"><span data-stu-id="c213c-119">Start **Android Studio**, and in the pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="c213c-120">Adjon meg egy alkalmazásnevet és egy vállalati tartományt.</span><span class="sxs-lookup"><span data-stu-id="c213c-120">Provide an app name and company domain.</span></span> <span data-ttu-id="c213c-121">Jegyezze fel a megadott információkat, mert később még szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="c213c-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="c213c-122">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="c213c-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="c213c-123">Jelölje ki a kívánt méretet és API-szintet, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="c213c-123">Select the target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c213c-124">A Mobile Engagement legalább 10-es API-szintet igényel (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="c213c-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="c213c-125">Válassza a **Blank Activity** (Üres tevékenység) elemet, amely ezen alkalmazás egyetlen képernyője lesz, majd kattintson a **Next** (Tovább) gombra.</span><span class="sxs-lookup"><span data-stu-id="c213c-125">Select **Blank Activity** here, which is the only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="c213c-126">Végül hagyja az alapértelmezett értékeket, és kattintson a **Finish** (Befejezés) gombra.</span><span class="sxs-lookup"><span data-stu-id="c213c-126">Finally, leave the defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="c213c-127">Az Android Studio létrehozza a bemutatóalkalmazást, amelybe integrálni fogjuk a Mobile Engagementet.</span><span class="sxs-lookup"><span data-stu-id="c213c-127">Android Studio now creates the demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-the-sdk-library-in-your-project"></a><span data-ttu-id="c213c-128">Az SDK-könyvtár hozzáadása a projekthez</span><span class="sxs-lookup"><span data-stu-id="c213c-128">Include the SDK library in your project</span></span>
1. <span data-ttu-id="c213c-129">Töltse le a [Mobile Engagement Android SDK](https://aka.ms/vq9mfn)-t.</span><span class="sxs-lookup"><span data-stu-id="c213c-129">Download the [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="c213c-130">Bontsa ki az archívumfájlt a számítógép egyik mappájába.</span><span class="sxs-lookup"><span data-stu-id="c213c-130">Extract the archive file to a folder in your computer.</span></span>
3. <span data-ttu-id="c213c-131">Azonosítsa az SDK aktuális verziójához tartozó .jar könyvtárat, és másolja a vágólapra.</span><span class="sxs-lookup"><span data-stu-id="c213c-131">Identify the .jar library for the current version of this SDK and copy it to the Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="c213c-132">Lépjen a **Project** (Projekt) szakaszhoz (1), és illessze be a .jar fájlt a „libs” mappába (2).</span><span class="sxs-lookup"><span data-stu-id="c213c-132">Navigate to the **Project** section (1) and paste the .jar in the libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="c213c-133">Szinkronizálja a projektet a könyvtár betöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="c213c-133">To load the library, sync the project .</span></span>

      ![][8]

### <a name="connect-your-app-to-mobile-engagement-backend-with-the-connection-string"></a><span data-ttu-id="c213c-134">Az alkalmazás csatlakoztatása a Mobile Engagement háttérrendszeréhez a kapcsolati karakterlánc segítségével</span><span class="sxs-lookup"><span data-stu-id="c213c-134">Connect your app to Mobile Engagement backend with the Connection String</span></span>
1. <span data-ttu-id="c213c-135">Másolja az alábbi kódsorokat a tevékenység létrehozásának helyén (csak egy helyre kell beilleszteni az alkalmazásba, általában a fő tevékenységnél).</span><span class="sxs-lookup"><span data-stu-id="c213c-135">Copy the following lines of code into the activity creation (must be done only in one place of your application, usually the main activity).</span></span> <span data-ttu-id="c213c-136">Ezen példaalkalmazás esetén nyissa meg az src -> main -> java mappában található MainActivity tevékenységet, és adja hozzá a következőket:</span><span class="sxs-lookup"><span data-stu-id="c213c-136">For this sample app, open up the MainActivity under src -> main -> java folder and add the following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="c213c-137">Oldja fel a hivatkozásokat az Alt+Enter billentyűkombináció lenyomásával vagy a következő importálási utasítások hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="c213c-137">Resolve the references by pressing Alt + Enter or adding the following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="c213c-138">Lépjen vissza a klasszikus Azure portálra az alkalmazás **Connection Info** (Kapcsolati adatok) lapjáról, és másolja a **kapcsolati karakterláncot**.</span><span class="sxs-lookup"><span data-stu-id="c213c-138">Go back to the Azure Classic Portal in your app's **Connection Info** page and copy the **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="c213c-139">Illessze be azt a `setConnectionString` paraméterbe az alábbi kódban látható teljes karakterláncot lecserélve:</span><span class="sxs-lookup"><span data-stu-id="c213c-139">Paste it into the `setConnectionString` parameter, replacing the entire string shown in the following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="c213c-140">Engedélyek és szolgáltatásdeklaráció hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c213c-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="c213c-141">Adja hozzá a projekt Manifest.xml fájljához a következő engedélyeket, közvetlenül az `<application>` címke előtt vagy után:</span><span class="sxs-lookup"><span data-stu-id="c213c-141">Add these permissions to the Manifest.xml of your project immediately before or after the `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="c213c-142">Adja hozzá a következő kódot az `<application>` és a `</application>` címke között az ügynökszolgáltatás deklarálásához:</span><span class="sxs-lookup"><span data-stu-id="c213c-142">To declare the agent service, add this code between the `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="c213c-143">A beillesztett kódban cserélje le a `"<Your application name>"` paramétert abban a címkében, amely a **Settings** (Beállítások) menüben jelenik meg, és amelyben a felhasználók megtekinthetik az eszközön futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="c213c-143">In the code you pasted, replace `"<Your application name>"` in the label, which is displayed in the **Settings** menu where you can see services running on the device.</span></span> <span data-ttu-id="c213c-144">A label értékeként megadhatja például a „Szolgáltatás” szót.</span><span class="sxs-lookup"><span data-stu-id="c213c-144">You can add the word "Service" in that label for example.</span></span>

### <a name="send-a-screen-to-mobile-engagement"></a><span data-ttu-id="c213c-145">Képernyő küldése a Mobile Engagement számára</span><span class="sxs-lookup"><span data-stu-id="c213c-145">Send a screen to Mobile Engagement</span></span>
<span data-ttu-id="c213c-146">Az adatok küldésének megkezdéséhez és annak biztosításához, hogy a felhasználók aktívak, legalább egy képernyőt (tevékenységet) el kell küldenie a Mobile Engagement háttérrendszere számára.</span><span class="sxs-lookup"><span data-stu-id="c213c-146">To start sending data and ensure that the users are active, you must send at least one screen (Activity) to the Mobile Engagement backend.</span></span>

<span data-ttu-id="c213c-147">Lépjen a **MainActivity.java** fájlhoz, és adja hozzá a következőt a **MainActivity** alaposztály lecseréléséhez az **EngagementActivity** osztályra:</span><span class="sxs-lookup"><span data-stu-id="c213c-147">Go to **MainActivity.java** and add the following to replace the base class of **MainActivity** to **EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="c213c-148">Ha az alaposztály nem *Activity*, a [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md) foglalkozó témakörben tájékozódhat a más osztályoktól való öröklésről.</span><span class="sxs-lookup"><span data-stu-id="c213c-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how to inherit from different classes.</span></span>
>
>

<span data-ttu-id="c213c-149">Az alábbi sort tegye megjegyzésbe ezen egyszerű forgatókönyv esetén:</span><span class="sxs-lookup"><span data-stu-id="c213c-149">Comment out the following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="c213c-150">Ha meg akarja tartani az alkalmazásban az `ActionBar` értéket, tekintse meg a [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md) foglalkozó témakört.</span><span class="sxs-lookup"><span data-stu-id="c213c-150">If you want to keep the `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="c213c-151">Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="c213c-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="c213c-152">Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="c213c-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="c213c-153">A Mobile Engagement a kampányok során lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók ELÉRÉSÉT leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="c213c-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="c213c-154">Ez a modul REACH (Elérés) néven érhető el a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="c213c-154">This module is called REACH in the Mobile Engagement portal.</span></span>
<span data-ttu-id="c213c-155">Az alábbi szakaszban állíthatja be az alkalmazást a fogadásukra.</span><span class="sxs-lookup"><span data-stu-id="c213c-155">The following section sets up your app to receive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="c213c-156">SDK-erőforrások másolása a projektben</span><span class="sxs-lookup"><span data-stu-id="c213c-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="c213c-157">Lépjen vissza a letöltött SDK-tartalomhoz, és másolja a **res** mappát.</span><span class="sxs-lookup"><span data-stu-id="c213c-157">Navigate back to your SDK download content and copy the **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="c213c-158">Lépjen vissza az Android Studio alkalmazásba, jelölje ki a projektfájlok **main** könyvtárát, majd illessze be az erőforrásoknak a projekthez való hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="c213c-158">Go back to Android Studio, select the **main** directory of your project files, and then paste it to add the resources to your project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="c213c-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c213c-159">Next steps</span></span>
<span data-ttu-id="c213c-160">Az [Android SDK-val](mobile-engagement-android-sdk-overview.md) foglalkozó témakörben tájékozódhat bővebben az SDK integrálásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c213c-160">Go to [Android SDK](mobile-engagement-android-sdk-overview.md) to get detailed knowledge about the SDK integration.</span></span>

<!-- Images. -->
[1]: ./media/mobile-engagement-android-get-started/android-studio-new-project.png
[2]: ./media/mobile-engagement-android-get-started/android-studio-project-props.png
[3]: ./media/mobile-engagement-android-get-started/android-studio-project-props2.png
[4]: ./media/mobile-engagement-android-get-started/android-studio-add-activity.png
[5]: ./media/mobile-engagement-android-get-started/android-studio-activity-name.png
[6]: ./media/mobile-engagement-android-get-started/sdk-content.png
[7]: ./media/mobile-engagement-android-get-started/paste-jar.png
[8]: ./media/mobile-engagement-android-get-started/sync-project.png
[9]: ./media/mobile-engagement-android-get-started/app-connection-info-page.png
[10]: ./media/mobile-engagement-android-get-started/copy-resources.png
[11]: ./media/mobile-engagement-android-get-started/paste-resources.png
