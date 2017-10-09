---
title: "aaaGet Android alkalmazások Azure Mobile Engagement használatába"
description: "Megtudhatja, hogyan toouse Azure Mobile Engagement az elemzések és leküldéses értesítések Android-alkalmazások."
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
ms.openlocfilehash: e8c92607691104750cdf1c4f7639a041d8a7bcd5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-android-apps"></a><span data-ttu-id="b9984-103">Ismerkedés az Azure Mobile Engagement Android-alkalmazásokkal való használatával</span><span class="sxs-lookup"><span data-stu-id="b9984-103">Get started with Azure Mobile Engagement for Android apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="b9984-104">Ez a témakör bemutatja, hogyan toouse Azure Mobile Engagement toounderstand az alkalmazás használatának és toosend a leküldéses értesítések toosegmented felhasználók Android-alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="b9984-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your app usage and how toosend push notifications toosegmented users of an Android application.</span></span>
<span data-ttu-id="b9984-105">Ez az oktatóanyag bemutatja, hogyan hello használó egyszerű küldési forgatókönyvet a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="b9984-105">This tutorial demonstrates hello simple broadcast scenario using Mobile Engagement.</span></span> <span data-ttu-id="b9984-106">Az oktatóanyagban létrehoz egy üres Android-alkalmazást, amely alapszintű adatokat gyűjt, és leküldéses értesítéseket fogad a Google Cloud Messaging (GCM) használatával.</span><span class="sxs-lookup"><span data-stu-id="b9984-106">In it, you create a blank Android app that collects basic data and receives push notifications using Google Cloud Messaging (GCM).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b9984-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b9984-107">Prerequisites</span></span>
<span data-ttu-id="b9984-108">Az oktatóanyag elvégzéséhez szükség hello [Android Developer Tools](https://developer.android.com/sdk/index.html), amely magában foglalja a hello Android Studio integrált fejlesztőkörnyezetet és hello legújabb Android platformot.</span><span class="sxs-lookup"><span data-stu-id="b9984-108">Completing this tutorial requires hello [Android Developer Tools](https://developer.android.com/sdk/index.html), which includes hello Android Studio integrated development environment, and hello latest Android platform.</span></span>

<span data-ttu-id="b9984-109">Azt is szükséges hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="b9984-109">It also requires hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b9984-110">toocomplete ebben az oktatóanyagban egy aktív Azure-fiókra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="b9984-110">toocomplete this tutorial, you need an active Azure account.</span></span> <span data-ttu-id="b9984-111">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="b9984-111">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="b9984-112">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span><span class="sxs-lookup"><span data-stu-id="b9984-112">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-android-get-started).</span></span>
>
>

## <a name="set-up-mobile-engagement-for-your-android-app"></a><span data-ttu-id="b9984-113">A Mobile Engagement beállítása az Android-alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="b9984-113">Set up Mobile Engagement for your Android app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a name="connect-your-app-toohello-mobile-engagement-backend"></a><span data-ttu-id="b9984-114">Csatlakozás az alkalmazás toohello Mobile Engagement háttérrendszeréhez</span><span class="sxs-lookup"><span data-stu-id="b9984-114">Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="b9984-115">Ez az oktatóanyag egy "alapszintű integrációt" mutat, amely minimális hello beállítása szükséges toocollect adatokat, és leküldéses értesítés küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="b9984-115">This tutorial presents a "basic integration", which is hello minimal set required toocollect data and send a push notification.</span></span> <span data-ttu-id="b9984-116">Létrehozhat egy alapszintű alkalmazást az Android Studio toodemonstrate hello integráció.</span><span class="sxs-lookup"><span data-stu-id="b9984-116">You create a basic app with Android Studio toodemonstrate hello integration.</span></span>

<span data-ttu-id="b9984-117">hello teljes integrációs dokumentáció itt található a hello [Mobile Engagement Android SDK-integráció](mobile-engagement-android-sdk-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b9984-117">hello complete integration documentation can be found in hello [Mobile Engagement Android SDK integration](mobile-engagement-android-sdk-overview.md).</span></span>

### <a name="create-an-android-project"></a><span data-ttu-id="b9984-118">Android-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b9984-118">Create an Android project</span></span>
1. <span data-ttu-id="b9984-119">Start **Android Studio**, és hello előugró ablakban válassza **új Android Studio projekt indítása**.</span><span class="sxs-lookup"><span data-stu-id="b9984-119">Start **Android Studio**, and in hello pop-up, select **Start a new Android Studio project**.</span></span>

    ![][1]
2. <span data-ttu-id="b9984-120">Adjon meg egy alkalmazásnevet és egy vállalati tartományt.</span><span class="sxs-lookup"><span data-stu-id="b9984-120">Provide an app name and company domain.</span></span> <span data-ttu-id="b9984-121">Jegyezze fel a megadott információkat, mert később még szüksége lesz rájuk.</span><span class="sxs-lookup"><span data-stu-id="b9984-121">Make a note of what you are filling, because you need it later.</span></span> <span data-ttu-id="b9984-122">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="b9984-122">Click **Next**.</span></span>

    ![][2]
3. <span data-ttu-id="b9984-123">Válassza ki a hello kívánt méretet és API-szintet, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9984-123">Select hello target form factor and API level, and click **Next**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b9984-124">A Mobile Engagement legalább 10-es API-szintet igényel (Android 2.3.3).</span><span class="sxs-lookup"><span data-stu-id="b9984-124">Mobile Engagement requires API level 10 minimum (Android 2.3.3).</span></span>
   >
   >

    ![][3]
4. <span data-ttu-id="b9984-125">Válassza ki **üres tevékenység** itt, amely egyetlen képernyője hello alkalmazáshoz, majd kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="b9984-125">Select **Blank Activity** here, which is hello only screen for this app and click **Next**.</span></span>

    ![][4]
5. <span data-ttu-id="b9984-126">Végül hagyja az hello alapértelmezett beállításokat, és kattintson **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="b9984-126">Finally, leave hello defaults as is and click **Finish**.</span></span>

    ![][5]

<span data-ttu-id="b9984-127">Az Android Studio létrehozza hello bemutatóalkalmazást, amelybe integrálni a Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="b9984-127">Android Studio now creates hello demo app into which we integrate Mobile Engagement.</span></span>

### <a name="include-hello-sdk-library-in-your-project"></a><span data-ttu-id="b9984-128">A projekt tartalmazza a hello SDK-könyvtár</span><span class="sxs-lookup"><span data-stu-id="b9984-128">Include hello SDK library in your project</span></span>
1. <span data-ttu-id="b9984-129">Töltse le a hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span><span class="sxs-lookup"><span data-stu-id="b9984-129">Download hello [Mobile Engagement Android SDK](https://aka.ms/vq9mfn).</span></span>
2. <span data-ttu-id="b9984-130">Bontsa ki a hello archív fájl tooa mappába a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b9984-130">Extract hello archive file tooa folder in your computer.</span></span>
3. <span data-ttu-id="b9984-131">Ez az SDK aktuális verziójához hello hello .jar könyvtárat határozza meg és toohello vágólapra másolásához.</span><span class="sxs-lookup"><span data-stu-id="b9984-131">Identify hello .jar library for hello current version of this SDK and copy it toohello Clipboard.</span></span>

      ![][6]
4. <span data-ttu-id="b9984-132">Keresse meg a toohello **projekt** szakasz (1) és illessze be a hello .jar hello függvénytárak mappába (2).</span><span class="sxs-lookup"><span data-stu-id="b9984-132">Navigate toohello **Project** section (1) and paste hello .jar in hello libs folder (2).</span></span>

      ![][7]
5. <span data-ttu-id="b9984-133">tooload hello könyvtárban, a szinkronizálási hello projekt.</span><span class="sxs-lookup"><span data-stu-id="b9984-133">tooload hello library, sync hello project .</span></span>

      ![][8]

### <a name="connect-your-app-toomobile-engagement-backend-with-hello-connection-string"></a><span data-ttu-id="b9984-134">Csatlakozás a tooMobile Engagement háttéralkalmazás hello kapcsolati karakterlánc</span><span class="sxs-lookup"><span data-stu-id="b9984-134">Connect your app tooMobile Engagement backend with hello Connection String</span></span>
1. <span data-ttu-id="b9984-135">Másolás hello alábbi kód a hello tevékenység létrehozása (elvégzendő csak egyetlen helyen az alkalmazásba, általában hello fő tevékenységnél).</span><span class="sxs-lookup"><span data-stu-id="b9984-135">Copy hello following lines of code into hello activity creation (must be done only in one place of your application, usually hello main activity).</span></span> <span data-ttu-id="b9984-136">Ezen PéldaAlkalmazás esetén nyisson meg az src MainActivity hello -> main -> java mappában, és adja hozzá a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b9984-136">For this sample app, open up hello MainActivity under src -> main -> java folder and add hello following:</span></span>

        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.setConnectionString("Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}");
        EngagementAgent.getInstance(this).init(engagementConfiguration);
2. <span data-ttu-id="b9984-137">Hárítsa el a hello hivatkozások Alt + Enter megnyomásával, vagy a következő importálási utasításokat hello hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="b9984-137">Resolve hello references by pressing Alt + Enter or adding hello following import statements:</span></span>

        import com.microsoft.azure.engagement.EngagementAgent;
        import com.microsoft.azure.engagement.EngagementConfiguration;
3. <span data-ttu-id="b9984-138">Lépjen vissza toohello klasszikus Azure portálra az alkalmazás **Kapcsolatinformáció** lapjáról, és másolja hello **kapcsolati karakterlánc**.</span><span class="sxs-lookup"><span data-stu-id="b9984-138">Go back toohello Azure Classic Portal in your app's **Connection Info** page and copy hello **Connection String**.</span></span>

      ![][9]
4. <span data-ttu-id="b9984-139">Illessze be hello `setConnectionString` paraméter hello teljes karakterlánc a következő kód hello megjelenő mag cseréje:</span><span class="sxs-lookup"><span data-stu-id="b9984-139">Paste it into hello `setConnectionString` parameter, replacing hello entire string shown in hello following code:</span></span>

        engagementConfiguration.setConnectionString("Endpoint=my-company-name.device.mobileengagement.windows.net;SdkKey=********************;AppId=*********");

### <a name="add-permissions-and-a-service-declaration"></a><span data-ttu-id="b9984-140">Engedélyek és szolgáltatásdeklaráció hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b9984-140">Add permissions and a service declaration</span></span>
1. <span data-ttu-id="b9984-141">Adja hozzá ezek engedélyek toohello a projekt manifest.XML fájljához, azonnal előtt vagy után hello `<application>` címke:</span><span class="sxs-lookup"><span data-stu-id="b9984-141">Add these permissions toohello Manifest.xml of your project immediately before or after hello `<application>` tag:</span></span>

        <uses-permission android:name="android.permission.INTERNET"/>
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION"/>
2. <span data-ttu-id="b9984-142">toodeclare hello ügynökszolgáltatás, adja hozzá a kódot közötti hello `<application>` és `</application>` címkék:</span><span class="sxs-lookup"><span data-stu-id="b9984-142">toodeclare hello agent service, add this code between hello `<application>` and `</application>` tags:</span></span>

        <service
             android:name="com.microsoft.azure.engagement.service.EngagementService"
             android:exported="false"
             android:label="<Your application name>"
             android:process=":Engagement"/>
3. <span data-ttu-id="b9984-143">Hello a beillesztett kódban, cserélje le a `"<Your application name>"` hello címke, amely megjelenik hello **beállítások** menü, ahol láthatja hello eszközön futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="b9984-143">In hello code you pasted, replace `"<Your application name>"` in hello label, which is displayed in hello **Settings** menu where you can see services running on hello device.</span></span> <span data-ttu-id="b9984-144">A label például hozzáadhat hello "Szolgáltatás" szót.</span><span class="sxs-lookup"><span data-stu-id="b9984-144">You can add hello word "Service" in that label for example.</span></span>

### <a name="send-a-screen-toomobile-engagement"></a><span data-ttu-id="b9984-145">A képernyő tooMobile Engagement küldése</span><span class="sxs-lookup"><span data-stu-id="b9984-145">Send a screen tooMobile Engagement</span></span>
<span data-ttu-id="b9984-146">adatküldés toostart, és győződjön meg arról, hogy hello felhasználók aktívak, el kell küldenie a Mobile Engagement háttérrendszeréhez legalább egy képernyőt (tevékenységet) toohello.</span><span class="sxs-lookup"><span data-stu-id="b9984-146">toostart sending data and ensure that hello users are active, you must send at least one screen (Activity) toohello Mobile Engagement backend.</span></span>

<span data-ttu-id="b9984-147">Nyissa meg túl**MainActivity.java** , és adja hozzá a következő tooreplace hello alaposztály hello **MainActivity** túl**EngagementActivity**:</span><span class="sxs-lookup"><span data-stu-id="b9984-147">Go too**MainActivity.java** and add hello following tooreplace hello base class of **MainActivity** too**EngagementActivity**:</span></span>

    public class MainActivity extends EngagementActivity {

> [!NOTE]
> <span data-ttu-id="b9984-148">Ha az alaposztály nem *tevékenység*, tekintse át [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md) arról, hogyan tooinherit a más osztályoktól.</span><span class="sxs-lookup"><span data-stu-id="b9984-148">If your base class is not *Activity*, consult [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md) for how tooinherit from different classes.</span></span>
>
>

<span data-ttu-id="b9984-149">A következő ezen egyszerű forgatókönyv esetén a sor hello megjegyzéssé:</span><span class="sxs-lookup"><span data-stu-id="b9984-149">Comment out hello following line for this simple sample scenario:</span></span>

    // setSupportActionBar(toolbar);

<span data-ttu-id="b9984-150">Ha azt szeretné, hogy tookeep hello `ActionBar` az alkalmazásban, lásd: [speciális Android-jelentéskészítéssel](mobile-engagement-android-advanced-reporting.md).</span><span class="sxs-lookup"><span data-stu-id="b9984-150">If you want tookeep hello `ActionBar` in your app, see [Advanced Android Reporting](mobile-engagement-android-advanced-reporting.md).</span></span>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="b9984-151">Az alkalmazás csatlakoztatása a valós idejű megfigyeléshez</span><span class="sxs-lookup"><span data-stu-id="b9984-151">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

## <a name="enable-push-notifications-and-in-app-messaging"></a><span data-ttu-id="b9984-152">Leküldéses értesítések és alkalmazáson belüli üzenetek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="b9984-152">Enable push notifications and in-app messaging</span></span>
<span data-ttu-id="b9984-153">A Mobile Engagement a kampányok során lehetővé teszi a felhasználókkal folytatott interakciót és a felhasználók ELÉRÉSÉT leküldéses értesítésekkel és alkalmazáson belüli üzenetekkel.</span><span class="sxs-lookup"><span data-stu-id="b9984-153">During a campaign, Mobile Engagement lets you interact with and REACH your users with push notifications and in-app messaging.</span></span> <span data-ttu-id="b9984-154">Ez a modul REACH neve hello a Mobile Engagement portálon.</span><span class="sxs-lookup"><span data-stu-id="b9984-154">This module is called REACH in hello Mobile Engagement portal.</span></span>
<span data-ttu-id="b9984-155">a következő szakasz hello állít be az alkalmazás tooreceive őket.</span><span class="sxs-lookup"><span data-stu-id="b9984-155">hello following section sets up your app tooreceive them.</span></span>

### <a name="copy-sdk-resources-in-your-project"></a><span data-ttu-id="b9984-156">SDK-erőforrások másolása a projektben</span><span class="sxs-lookup"><span data-stu-id="b9984-156">Copy SDK resources in your project</span></span>
1. <span data-ttu-id="b9984-157">Keresse meg a visszafelé tooyour SDK letöltési tartalomhoz, és másolja hello **res** mappát.</span><span class="sxs-lookup"><span data-stu-id="b9984-157">Navigate back tooyour SDK download content and copy hello **res** folder.</span></span>

    ![][10]
2. <span data-ttu-id="b9984-158">Lépjen vissza tooAndroid Studio eszközt, jelölje be hello **fő** könyvtárát a projektfájlok, majd illessze be azt tooadd hello erőforrások tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="b9984-158">Go back tooAndroid Studio, select hello **main** directory of your project files, and then paste it tooadd hello resources tooyour project.</span></span>

    ![][11]

[!INCLUDE [Enable Google Cloud Messaging](../../includes/mobile-engagement-enable-google-cloud-messaging.md)]

[!INCLUDE [Enable in-app messaging](../../includes/mobile-engagement-android-send-push.md)]

[!INCLUDE [Send notification from portal](../../includes/mobile-engagement-android-send-push-from-portal.md)]

## <a name="next-steps"></a><span data-ttu-id="b9984-159">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b9984-159">Next steps</span></span>
<span data-ttu-id="b9984-160">Nyissa meg túl[Android SDK](mobile-engagement-android-sdk-overview.md) tooget részletes hello SDK-integráció ismerete.</span><span class="sxs-lookup"><span data-stu-id="b9984-160">Go too[Android SDK](mobile-engagement-android-sdk-overview.md) tooget detailed knowledge about hello SDK integration.</span></span>

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
