---
title: "jelentéskészítési lehetőségek az Azure Mobile Engagement Android SDK aaaAdvanced"
description: "Ismerteti, hogyan toodo speciális az Azure Mobile Engagement Android SDK jelentéskészítési toocapture elemzés"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
ms.openlocfilehash: 5c8f4ea36c54715f4e09fd43c96132c15019a71b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="advanced-reporting-with-engagement-on-android"></a><span data-ttu-id="334f8-103">Speciális hibajelentési az Engagement Android rendszeren</span><span class="sxs-lookup"><span data-stu-id="334f8-103">Advanced Reporting with Engagement on Android</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="334f8-104">Univerzális Windows</span><span class="sxs-lookup"><span data-stu-id="334f8-104">Universal Windows</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="334f8-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="334f8-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="334f8-106">iOS</span><span class="sxs-lookup"><span data-stu-id="334f8-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="334f8-107">Android</span><span class="sxs-lookup"><span data-stu-id="334f8-107">Android</span></span>](mobile-engagement-android-advanced-reporting.md)
> 
> 

<span data-ttu-id="334f8-108">Ez a témakör az Android alkalmazás további jelentéskészítési forgatókönyvekhez.</span><span class="sxs-lookup"><span data-stu-id="334f8-108">This topic describes additional reporting scenarios in your Android application.</span></span> <span data-ttu-id="334f8-109">E beállítások toohello app hello létrehozott alkalmazhat [bevezetés](mobile-engagement-android-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="334f8-109">You can apply these options toohello app created in hello [Getting Started](mobile-engagement-android-get-started.md) tutorial.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="334f8-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="334f8-110">Prerequisites</span></span>
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

<span data-ttu-id="334f8-111">hello oktatóanyag elvégzésének szándékosan közvetlen és egyszerű, de nincs speciális lehetősége választható.</span><span class="sxs-lookup"><span data-stu-id="334f8-111">hello tutorial you completed was deliberately direct and simple, but there are advanced options you can choose.</span></span>

## <a name="modifying-your-activity-classes"></a><span data-ttu-id="334f8-112">Módosítja a `Activity` osztályok</span><span class="sxs-lookup"><span data-stu-id="334f8-112">Modifying your `Activity` classes</span></span>
<span data-ttu-id="334f8-113">A hello [bevezető oktatóanyag](mobile-engagement-android-get-started.md), minden toodo kellett toomake volt a `*Activity` alosztályok öröklik a megfelelő hello `Engagement*Activity` osztályok.</span><span class="sxs-lookup"><span data-stu-id="334f8-113">In hello [Getting Started tutorial](mobile-engagement-android-get-started.md), all you had toodo was toomake your `*Activity` subclasses inherit from hello corresponding `Engagement*Activity` classes.</span></span> <span data-ttu-id="334f8-114">Például, ha az örökölt tevékenység kiterjesztett `ListActivity`, hogy könnyebben kiterjesztése `EngagementListActivity`.</span><span class="sxs-lookup"><span data-stu-id="334f8-114">For example, if your legacy activity extended `ListActivity`, you would make it extend `EngagementListActivity`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="334f8-115">Használata esetén `EngagementListActivity` vagy `EngagementExpandableListActivity`, győződjön meg arról, hogy semmilyen hívásban túl`requestWindowFeature(...);` túl a hello hívása előtt végrehajtott`super.onCreate(...);`, különben a összeomlási történik.</span><span class="sxs-lookup"><span data-stu-id="334f8-115">When using `EngagementListActivity` or `EngagementExpandableListActivity`, make sure any call too`requestWindowFeature(...);` is made before hello call too`super.onCreate(...);`, otherwise a crash occurs.</span></span>
> 
> 

<span data-ttu-id="334f8-116">Ezeket az osztályokat az hello található `src` mappa, és a projektben másolja őket.</span><span class="sxs-lookup"><span data-stu-id="334f8-116">You can find these classes in hello `src` folder, and can copy them into your project.</span></span> <span data-ttu-id="334f8-117">hello osztályokat is az hello **JavaDoc**.</span><span class="sxs-lookup"><span data-stu-id="334f8-117">hello classes are also in hello **JavaDoc**.</span></span>

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a><span data-ttu-id="334f8-118">Másodlagos módszer: hívja `startActivity()` és `endActivity()` manuálisan</span><span class="sxs-lookup"><span data-stu-id="334f8-118">Alternate method: call `startActivity()` and `endActivity()` manually</span></span>
<span data-ttu-id="334f8-119">Ha nem, vagy nem toooverload a `Activity` osztályokat, akkor is inkább kezdő és záró a tevékenységek hello meghívásával `EngagementAgent`tartozó módszerek közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="334f8-119">If you cannot or do not want toooverload your `Activity` classes, you can instead start and end your activities by calling hello `EngagementAgent`'s methods directly.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="334f8-120">Android SDK hello soha nem hívja a hello `endActivity()` metódust, akkor is, ha hello alkalmazás bezárása (Android, az alkalmazások soha nem Lezáródnak).</span><span class="sxs-lookup"><span data-stu-id="334f8-120">hello Android SDK never calls hello `endActivity()` method, even when hello application is closed (on Android, applications are never closed).</span></span> <span data-ttu-id="334f8-121">Így *magas* toocall hello ajánlott `startActivity()` hello metódust `onResume` a visszahívási *minden* tevékenységek, és hello `endActivity()` hello metódus `onPause()` a visszahívási *összes* a tevékenységeket.</span><span class="sxs-lookup"><span data-stu-id="334f8-121">Thus, it is *HIGHLY* recommended toocall hello `startActivity()` method in hello `onResume` callback of *ALL* your activities, and hello `endActivity()` method in hello `onPause()` callback of *ALL* your activities.</span></span> <span data-ttu-id="334f8-122">Ez a hello csak úgy toobe meg arról, hogy munkamenetek nem szivárognak.</span><span class="sxs-lookup"><span data-stu-id="334f8-122">This is hello only way toobe sure that sessions are not leaked.</span></span> <span data-ttu-id="334f8-123">Ha a munkamenet kiszivárgott, hello Engagement szolgáltatás soha nem bontja a kapcsolatot hello Engagement háttérrendszeréhez (mivel hello szolgáltatás továbbra is csatlakoztatva marad mindaddig, amíg függőben egy munkamenet).</span><span class="sxs-lookup"><span data-stu-id="334f8-123">If a session is leaked, hello Engagement service never disconnects from hello Engagement backend (since hello service stays connected as long as a session is pending).</span></span>
> 
> 

<span data-ttu-id="334f8-124">Például:</span><span class="sxs-lookup"><span data-stu-id="334f8-124">Here is an example:</span></span>

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at hello end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

<span data-ttu-id="334f8-125">Ebben a példában az hasonló toohello `EngagementActivity` osztály és annak változataihoz, amelynek forráskód megadott hello `src` mappát.</span><span class="sxs-lookup"><span data-stu-id="334f8-125">This example is similar toohello `EngagementActivity` class and its variants, whose source code is provided in hello `src` folder.</span></span>

## <a name="using-applicationoncreate"></a><span data-ttu-id="334f8-126">Application.onCreate() használatával</span><span class="sxs-lookup"><span data-stu-id="334f8-126">Using Application.onCreate()</span></span>
<span data-ttu-id="334f8-127">A kód helyez `Application.onCreate()` és más alkalmazásban visszahívások az alkalmazás folyamatok, többek között a hello Engagement szolgáltatás futtatásához.</span><span class="sxs-lookup"><span data-stu-id="334f8-127">Any code you place in `Application.onCreate()` and in other application callbacks is run for all your application's processes, including hello Engagement service.</span></span> <span data-ttu-id="334f8-128">Nem kívánt hatásai, nem szükséges memória-kiosztásokat és hello bevonási folyamat, például lehet, vagy ismétlődő szórási fogadók vagy szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="334f8-128">It may have unwanted side effects, like unneeded memory allocations and threads in hello Engagement's process, or duplicate broadcast receivers or services.</span></span>

<span data-ttu-id="334f8-129">Ha Ön felülbírálása `Application.onCreate()`, azt javasoljuk, hogy a következő kódrészletet a hello elején hello a `Application.onCreate()` függvény:</span><span class="sxs-lookup"><span data-stu-id="334f8-129">If you override `Application.onCreate()`, we recommend adding hello following code snippet at hello beginning of your `Application.onCreate()` function:</span></span>

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

<span data-ttu-id="334f8-130">Mindent ugyanaz a hello `Application.onTerminate()`, `Application.onLowMemory()`, és `Application.onConfigurationChanged(...)`.</span><span class="sxs-lookup"><span data-stu-id="334f8-130">You can do hello same thing for `Application.onTerminate()`, `Application.onLowMemory()`, and `Application.onConfigurationChanged(...)`.</span></span>

<span data-ttu-id="334f8-131">Emellett kibővítheti `EngagementApplication` helyett kiterjesztése `Application`: hello visszahívási `Application.onCreate()` hello folyamat ellenőrzése és hívások `Application.onApplicationProcessCreate()` csak ha hello aktuális folyamat nem hello egy üzemeltetési hello Engagement szolgáltatás, hello ugyanazok a szabályok vonatkoznak a hello más visszahívások.</span><span class="sxs-lookup"><span data-stu-id="334f8-131">You can also extend `EngagementApplication` instead of extending `Application`: hello callback `Application.onCreate()` does hello process check and calls `Application.onApplicationProcessCreate()` only if hello current process is not hello one hosting hello Engagement service, hello same rules apply for hello other callbacks.</span></span>

## <a name="tags-in-hello-androidmanifestxml-file"></a><span data-ttu-id="334f8-132">Címkék hello AndroidManifest.xml fájlban</span><span class="sxs-lookup"><span data-stu-id="334f8-132">Tags in hello AndroidManifest.xml file</span></span>
<span data-ttu-id="334f8-133">Hello szolgáltatás címkében hello AndroidManifest.xml fájlban hello `android:label` attribútum lehetővé teszi a toochoose hello neve hello Engagement szolgáltatás tooend felhasználók telefont hello "Futó szolgáltatások" képernyőjén megjelenő.</span><span class="sxs-lookup"><span data-stu-id="334f8-133">In hello service tag in hello AndroidManifest.xml file, hello `android:label` attribute allows you toochoose hello name of hello Engagement service as it appears tooend users in hello "Running services" screen of their phone.</span></span> <span data-ttu-id="334f8-134">Javasoljuk, hogy ha az attribútum túl`"<Your application name>Service"` (például `"AcmeFunGameService"`).</span><span class="sxs-lookup"><span data-stu-id="334f8-134">We recommended setting this attribute too`"<Your application name>Service"` (for example, `"AcmeFunGameService"`).</span></span>

<span data-ttu-id="334f8-135">Adja meg hello `android:process` attribútum biztosítja, hogy hello Engagement szolgáltatásának futtatásakor a saját folyamat (Engagement futtató ugyanezt a folyamatot, az alkalmazás lehetővé teszi a fő/felhasználói felület szálán potenciálisan kevésbé rugalmas hello).</span><span class="sxs-lookup"><span data-stu-id="334f8-135">Specifying hello `android:process` attribute ensures that hello Engagement service runs in its own process (running Engagement in hello same process as your application makes your main/UI thread potentially less responsive).</span></span>

## <a name="building-with-proguard"></a><span data-ttu-id="334f8-136">A ProGuard felépítése</span><span class="sxs-lookup"><span data-stu-id="334f8-136">Building with ProGuard</span></span>
<span data-ttu-id="334f8-137">Ha ProGuard az alkalmazáscsomag, kell tookeep bizonyos osztályokat.</span><span class="sxs-lookup"><span data-stu-id="334f8-137">If you build your application package with ProGuard, you need tookeep some classes.</span></span> <span data-ttu-id="334f8-138">A következő konfigurációs részlet hello használhatja:</span><span class="sxs-lookup"><span data-stu-id="334f8-138">You can use hello following configuration snippet:</span></span>

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }
