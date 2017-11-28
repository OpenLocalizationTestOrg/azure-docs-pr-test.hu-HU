---
title: "aaaGet Baidu segítségével Azure Notification Hubs használatába |} Microsoft Docs"
description: "Ebben az oktatóanyagban elsajátíthatja, hogyan toouse Azure Notification Hubs toopush értesítések tooAndroid eszközök Baidu segítségével."
services: notification-hubs
documentationcenter: android
author: ysxu
manager: erikre
editor: 
ms.assetid: 23bde1ea-f978-43b2-9eeb-bfd7b9edc4c1
ms.service: notification-hubs
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: mobile-baidu
ms.workload: mobile
ms.date: 08/19/2016
ms.author: yuaxu
ms.openlocfilehash: 2767fdd3bb04674e7a531634237cc05cd8c21cb8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-notification-hubs-using-baidu"></a><span data-ttu-id="f7516-103">Ismerkedés a Notification Hubs Baiduval való használatával</span><span class="sxs-lookup"><span data-stu-id="f7516-103">Get started with Notification Hubs using Baidu</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="f7516-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="f7516-104">Overview</span></span>
<span data-ttu-id="f7516-105">Felhőalapú baidu egy kínai felhőszolgáltatás toosend leküldéses értesítések toomobile eszközöket használhatja.</span><span class="sxs-lookup"><span data-stu-id="f7516-105">Baidu cloud push is a Chinese cloud service that you can use toosend push notifications toomobile devices.</span></span> <span data-ttu-id="f7516-106">Ez a szolgáltatás akkor hasznos, kínai, ahol különböző alkalmazás-áruházak és leküldési hello jelenléte miatt tooAndroid összetett leküldéses értesítések kézbesítéséhez szolgáltatásokat, továbbá Android-eszközök, amelyek nincsenek általában csatlakoztatott tooGCM (Google toohello rendelkezésre állása Cloud Messaging).</span><span class="sxs-lookup"><span data-stu-id="f7516-106">This service is useful in China, where delivering push notifications tooAndroid is complex because of hello presence of different app stores and push services, in addition toohello availability of Android devices that are not typically connected tooGCM (Google Cloud Messaging).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f7516-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f7516-107">Prerequisites</span></span>
<span data-ttu-id="f7516-108">Az oktatóanyaghoz a következőkre lesz szükség:</span><span class="sxs-lookup"><span data-stu-id="f7516-108">This tutorial requires:</span></span>

* <span data-ttu-id="f7516-109">Android SDK (feltételezzük, hogy Eclipse használ), amelyre tölthető le: hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android webhelyéről</a></span><span class="sxs-lookup"><span data-stu-id="f7516-109">Android SDK (we assume that you use Eclipse), which you can download from hello <a href="http://go.microsoft.com/fwlink/?LinkId=389797">Android site</a></span></span>
* <span data-ttu-id="f7516-110">[Mobile Services Android SDK]</span><span class="sxs-lookup"><span data-stu-id="f7516-110">[Mobile Services Android SDK]</span></span>
* <span data-ttu-id="f7516-111">[Baidu Push Android SDK]</span><span class="sxs-lookup"><span data-stu-id="f7516-111">[Baidu Push Android SDK]</span></span>

> [!NOTE]
> <span data-ttu-id="f7516-112">toocomplete ebben az oktatóanyagban rendelkeznie kell egy aktív Azure-fiókra.</span><span class="sxs-lookup"><span data-stu-id="f7516-112">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f7516-113">Ha nincs fiókja, néhány perc alatt létrehozhat egy ingyenes próbafiókot.</span><span class="sxs-lookup"><span data-stu-id="f7516-113">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f7516-114">További információkért lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="f7516-114">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-baidu-get-started%2F).</span></span>
> 
> 

## <a name="create-a-baidu-account"></a><span data-ttu-id="f7516-115">Baidu-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7516-115">Create a Baidu account</span></span>
<span data-ttu-id="f7516-116">toouse Baidu, rendelkeznie kell a Baidu-fiók.</span><span class="sxs-lookup"><span data-stu-id="f7516-116">toouse Baidu, you must have a Baidu account.</span></span> <span data-ttu-id="f7516-117">Ha már rendelkezik egy, jelentkezzen be toohello [Baidu portálra] toohello következő lépés kihagyása.</span><span class="sxs-lookup"><span data-stu-id="f7516-117">If you already have one, log in toohello [Baidu portal] and skip toohello next step.</span></span> <span data-ttu-id="f7516-118">Ellenkező esetben tekintse meg az utasításoknak hogyan hello toocreate Baidu-fiók.</span><span class="sxs-lookup"><span data-stu-id="f7516-118">Otherwise, see hello following instructions on how toocreate a Baidu account.</span></span>  

1. <span data-ttu-id="f7516-119">Nyissa meg toohello [Baidu portálra] hello kattintson**登录**(**bejelentkezési**) hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="f7516-119">Go toohello [Baidu portal] and click hello **登录** (**Login**) link.</span></span> <span data-ttu-id="f7516-120">Kattintson a**立即注册**toostart hello fiók regisztrációs folyamat során.</span><span class="sxs-lookup"><span data-stu-id="f7516-120">Click **立即注册** toostart hello account registration process.</span></span>
   
   ![][1]
2. <span data-ttu-id="f7516-121">Írja be a szükséges hello adatait – telefonszám/e-mail-cím, jelszó és Ellenőrzőkód – kattintson **előfizetési**.</span><span class="sxs-lookup"><span data-stu-id="f7516-121">Enter hello required details—phone/email address, password, and verification code—and click **Signup**.</span></span>
   
   ![][2]
3. <span data-ttu-id="f7516-122">Kapnak az e-mailek toohello e-mail címet kell megadni a egy hivatkozás tooactivate a Baidu-fiók.</span><span class="sxs-lookup"><span data-stu-id="f7516-122">You will be sent an email toohello email address that you entered with a link tooactivate your Baidu account.</span></span>
   
   ![][3]
4. <span data-ttu-id="f7516-123">Jelentkezzen be tooyour e-mail fiók, nyissa meg a hello Baidu aktiválási levelét, majd kattintson hello aktiválási hivatkozásra tooactivate a Baidu-fiók.</span><span class="sxs-lookup"><span data-stu-id="f7516-123">Log in tooyour email account, open hello Baidu activation mail, and click hello activation link tooactivate your Baidu account.</span></span>
   
   ![][4]

<span data-ttu-id="f7516-124">Ha elvégezte a Baidu-fiók aktiválása, jelentkezzen be toohello [Baidu portálra].</span><span class="sxs-lookup"><span data-stu-id="f7516-124">Once you have an activated Baidu account, log in toohello [Baidu portal].</span></span>

## <a name="register-as-a-baidu-developer"></a><span data-ttu-id="f7516-125">Regisztráció Baidu-fejlesztőként</span><span class="sxs-lookup"><span data-stu-id="f7516-125">Register as a Baidu developer</span></span>
1. <span data-ttu-id="f7516-126">Ha már bejelentkezett toohello [Baidu portálra], kattintson a**更多 >>** (**további**).</span><span class="sxs-lookup"><span data-stu-id="f7516-126">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="f7516-127">Görgessen le a hello**站长与开发者服务 (gazdáját és fejlesztői szolgáltatások)** szakaszt, és kattintson**百度开放云平台**(**Baidu nyissa meg a felhő platform**).</span><span class="sxs-lookup"><span data-stu-id="f7516-127">Scroll down in hello **站长与开发者服务 (Webmaster and Developer Services)** section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="f7516-128">Hello következő lapon kattintson a**开发者服务**(**fejlesztői szolgáltatások**) hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="f7516-128">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="f7516-129">Hello következő lapon kattintson a**注册开发者**(**regisztrált fejlesztők**) hello jobb felső sarokban hello menüjéből.</span><span class="sxs-lookup"><span data-stu-id="f7516-129">On hello next page, click **注册开发者** (**Registered Developers**) from hello menu on hello top-right corner.</span></span>
   
      ![][8]
5. <span data-ttu-id="f7516-130">Adja meg a nevét, a leírást és a telefonszámot, amelyre meg kívánja kapni az ellenőrző SMS-t, majd kattintson az **送验证码** (**Ellenőrzőkód elküldése**) elemre.</span><span class="sxs-lookup"><span data-stu-id="f7516-130">Enter your name, a description, and a mobile phone number for receiving a verification text message, and then click **送验证码** (**Send Verification Code**).</span></span> <span data-ttu-id="f7516-131">A nemzetközi telefonszám tooenclose hello országhívószám zárójelbe kell.</span><span class="sxs-lookup"><span data-stu-id="f7516-131">For international phone numbers, you need tooenclose hello country code in parentheses.</span></span> <span data-ttu-id="f7516-132">Egyesült államokbeli telefonszámok esetén például a következőképpen: **(1)1234567890**.</span><span class="sxs-lookup"><span data-stu-id="f7516-132">For example, for a United States number, it is **(1)1234567890**.</span></span>
   
      ![][9]
6. <span data-ttu-id="f7516-133">Ezután a rendszer elküld egy szöveges üzenetet t, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="f7516-133">You should then receive a text message with a verification number, as shown in hello following example:</span></span>
   
      ![][10]
7. <span data-ttu-id="f7516-134">Hello ellenőrzési számot írja a üdvözlőüzenetére**验证码**(**megerősítő kód**).</span><span class="sxs-lookup"><span data-stu-id="f7516-134">Enter hello verification number from hello message in **验证码** (**Confirmation code**).</span></span>
8. <span data-ttu-id="f7516-135">Végezetül regisztrálást hello fejlesztői hello Baidu szerződés elfogadását jelző, majd**提交**(**Submit**).</span><span class="sxs-lookup"><span data-stu-id="f7516-135">Finally, complete hello developer registration by accepting hello Baidu agreement and clicking **提交** (**Submit**).</span></span> <span data-ttu-id="f7516-136">Hello oldalon, a regisztráció sikeres befejezését követően jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="f7516-136">You will see hello following page on successful completion of registration:</span></span>
   
      ![][11]

## <a name="create-a-baidu-cloud-push-project"></a><span data-ttu-id="f7516-137">Felhőalapú Baidu-értesítési projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="f7516-137">Create a Baidu cloud push project</span></span>
<span data-ttu-id="f7516-138">Felhőalapú Baidu-értesítési projekt létrehozásakor megkapja az alkalmazásazonosítóját, az API-kulcsot és a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f7516-138">When you create a Baidu cloud push project, you receive your app ID, API key, and secret key.</span></span>

1. <span data-ttu-id="f7516-139">Ha már bejelentkezett toohello [Baidu portálra], kattintson a**更多 >>** (**további**).</span><span class="sxs-lookup"><span data-stu-id="f7516-139">Once you have logged in toohello [Baidu portal], click **更多>>** (**more**).</span></span>
   
      ![][5]
2. <span data-ttu-id="f7516-140">Görgessen le a hello**站长与开发者服务**(**gazdáját és fejlesztői szolgáltatások**) szakaszt, és kattintson**百度开放云平台**(**Baidu nyissa meg a felhő platform**).</span><span class="sxs-lookup"><span data-stu-id="f7516-140">Scroll down in hello **站长与开发者服务** (**Webmaster and Developer Services**) section and click **百度开放云平台** (**Baidu open cloud platform**).</span></span>
   
      ![][6]
3. <span data-ttu-id="f7516-141">Hello következő lapon kattintson a**开发者服务**(**fejlesztői szolgáltatások**) hello jobb felső sarokban.</span><span class="sxs-lookup"><span data-stu-id="f7516-141">On hello next page, click **开发者服务** (**Developer Services**) on hello top-right corner.</span></span>
   
      ![][7]
4. <span data-ttu-id="f7516-142">Hello következő lapon kattintson a**云推送**(**felhő leküldéses**) a hello**云服务**(**Felhőszolgáltatások**) szakaszban.</span><span class="sxs-lookup"><span data-stu-id="f7516-142">On hello next page, click **云推送** (**Cloud Push**) from hello **云服务** (**Cloud Services**) section.</span></span>
   
      ![][12]
5. <span data-ttu-id="f7516-143">Ha egy regisztrált fejlesztők, tekintse meg a**管理控制台**(**felügyeleti konzol**): hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="f7516-143">Once you are a registered developer, you see **管理控制台** (**Management Console**) at hello top menu.</span></span> <span data-ttu-id="f7516-144">Kattintson a **开发者服务管理** (**Fejlesztői szolgáltatások kezelése**) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f7516-144">Click **开发者服务管理** (**Developers Service Management**).</span></span>
   
      ![][13]
6. <span data-ttu-id="f7516-145">Hello következő lapon kattintson a**创建工程**(**projekt létrehozása**).</span><span class="sxs-lookup"><span data-stu-id="f7516-145">On hello next page, click **创建工程** (**Create Project**).</span></span>
   
      ![][14]
7. <span data-ttu-id="f7516-146">Adjon meg egy alkalmazásnevet, majd kattintson a **创建** (**Létrehozás**) parancsra.</span><span class="sxs-lookup"><span data-stu-id="f7516-146">Enter an application name and click **创建** (**Create**).</span></span>
   
      ![][15]
8. <span data-ttu-id="f7516-147">A felhőalapú Baidu-értesítési projekt sikeres létrehozása után megjelenő oldalon megtalálja az **alkalmazásazonosítót**, az **API-kulcsot** és a **titkos kulcsot**.</span><span class="sxs-lookup"><span data-stu-id="f7516-147">Upon successful creation of a Baidu cloud push project, you see a page with **AppID**, **API Key**, and **Secret Key**.</span></span> <span data-ttu-id="f7516-148">Jegyezze fel a hello API-kulcsot és titkos kulcsot, mert később fogjuk használni.</span><span class="sxs-lookup"><span data-stu-id="f7516-148">Make a note of hello API key and secret key, which we will use later.</span></span>
   
      ![][16]
9. <span data-ttu-id="f7516-149">Leküldéses értesítések hello projekt konfigurálásához kattintva**云推送**(**felhő leküldéses**) hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="f7516-149">Configure hello project for push notifications by clicking **云推送** (**Cloud Push**) on hello left pane.</span></span>
   
      ![][31]
10. <span data-ttu-id="f7516-150">Hello következő lapon kattintson a hello**推送设置**(**beállítások leküldéses**) gombra.</span><span class="sxs-lookup"><span data-stu-id="f7516-150">On hello next page, click hello **推送设置** (**Push settings**) button.</span></span>
    
    ![][32]  
11. <span data-ttu-id="f7516-151">A hello konfiguráció lapon vegye fel a hello csomag neve, amely az Android projekt a hello fog használni**应用包名**(**alkalmazáscsomag**) mezőben, majd kattintson a**保存设置**() **Mentése**).</span><span class="sxs-lookup"><span data-stu-id="f7516-151">On hello configuration page, add hello package name that you will be using in your Android project in hello **应用包名** (**Application package**) field, and then click **保存设置** (**Save**).</span></span>  
    
    ![][33]

<span data-ttu-id="f7516-152">Megjelenik a hello**保存成功!** (**Sikeresen mentve!**) üzenet.</span><span class="sxs-lookup"><span data-stu-id="f7516-152">You see hello **保存成功！** (**Successfully saved!**) message.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="f7516-153">Az értesítési központ konfigurálása</span><span class="sxs-lookup"><span data-stu-id="f7516-153">Configure your notification hub</span></span>
1. <span data-ttu-id="f7516-154">Jelentkezzen be toohello [klasszikus Azure portál], és kattintson a **+ új** üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="f7516-154">Sign in toohello [Azure Classic Portal], and then click **+NEW** at hello bottom of hello screen.</span></span>
2. <span data-ttu-id="f7516-155">Kattintson az **App Services**, a **Service Bus**, a **Notification Hub**, végül pedig a **Gyors létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="f7516-155">Click **App Services**, click **Service Bus**, click **Notification Hub**, and then click **Quick Create**.</span></span>
3. <span data-ttu-id="f7516-156">Adjon meg egy nevet a **értesítési központ**, jelölje be hello **régió** és hello **Namespace** ahol az értesítési központ jön létre, és kattintson  **Hozzon létre egy új értesítési központ**.</span><span class="sxs-lookup"><span data-stu-id="f7516-156">Provide a name for your **Notification Hub**, select hello **Region** and hello **Namespace** where this notification hub will be created, and then click **Create a New Notification Hub**.</span></span>  
   
      ![][17]
4. <span data-ttu-id="f7516-157">Kattintson a hello névtér, amelyben létrehozta az értesítési központot, majd **Notification Hubs** hello tetején.</span><span class="sxs-lookup"><span data-stu-id="f7516-157">Click hello namespace in which you created your notification hub, and then click **Notification Hubs** at hello top.</span></span>
   
      ![][18]
5. <span data-ttu-id="f7516-158">Létrehozott, és kattintson a kijelölés hello értesítési központ **konfigurálása** hello felső menüjében.</span><span class="sxs-lookup"><span data-stu-id="f7516-158">Select hello notification hub that you created, and then click **Configure** from hello top menu.</span></span>
   
      ![][19]
6. <span data-ttu-id="f7516-159">Görgessen lefelé toohello **baidu-értesítési beállításainak** szakaszt, és adja meg a hello API és beszerzett hello Baidu-konzolon korábban a Baidu felhőalapú leküldéses értesítési projektet a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="f7516-159">Scroll down toohello **baidu notification settings** section and enter hello API key and secret key that you obtained from hello Baidu console previously for your Baidu cloud push project.</span></span> <span data-ttu-id="f7516-160">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="f7516-160">Click **Save**.</span></span>
   
      ![][20]
7. <span data-ttu-id="f7516-161">Kattintson a hello **irányítópult** hello felső hello értesítési központ fülre, majd **kapcsolati karakterlánc megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="f7516-161">Click hello **Dashboard** tab at hello top for hello notification hub, and then click **View Connection String**.</span></span>
   
      ![][21]
8. <span data-ttu-id="f7516-162">Jegyezze fel a hello **DefaultListenSharedAccessSignature** és **DefaultFullSharedAccessSignature** a hello **kapcsolati adatok elérése** ablak.</span><span class="sxs-lookup"><span data-stu-id="f7516-162">Make a note of hello **DefaultListenSharedAccessSignature** and **DefaultFullSharedAccessSignature** from hello **Access connection information** window.</span></span>
   
    ![][22]

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="f7516-163">Csatlakozás az alkalmazás toohello értesítési központ</span><span class="sxs-lookup"><span data-stu-id="f7516-163">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="f7516-164">Az Eclipse ADT-ben hozzon létre egy új Android-projektet (**File** (Fájl)  > **New** (Új)  > **Android Application Project** (Android-alkalmazásprojekt)).</span><span class="sxs-lookup"><span data-stu-id="f7516-164">In Eclipse ADT, create a new Android project (**File** > **New** > **Android Application Project**).</span></span>
   
    ![][23]
2. <span data-ttu-id="f7516-165">Adjon meg egy **alkalmazásnév** , és győződjön meg arról, hogy hello **minimálisan szükséges SDK** verzió beállítása túl**API 16: Android 4.1**.</span><span class="sxs-lookup"><span data-stu-id="f7516-165">Enter an **Application Name** and ensure that hello **Minimum Required SDK** version is set too**API 16: Android 4.1**.</span></span>
   
    ![][24]
3. <span data-ttu-id="f7516-166">Kattintson a **következő** , és folytassa a következő hello varázsló amíg hello **tevékenység létrehozása** ablak jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="f7516-166">Click **Next** and continue following hello wizard until hello **Create Activity** window appears.</span></span> <span data-ttu-id="f7516-167">Győződjön meg arról, hogy **üres tevékenység** kiválasztva, és végül válassza **Befejezés** toocreate egy új Android-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f7516-167">Make sure that **Blank Activity** is selected, and finally select **Finish** toocreate a new Android Application.</span></span>
   
    ![][25]
4. <span data-ttu-id="f7516-168">Győződjön meg arról, hogy hello **Project Build Target** megfelelően van beállítva.</span><span class="sxs-lookup"><span data-stu-id="f7516-168">Make sure that hello **Project Build Target** is set correctly.</span></span>
   
    ![][26]
5. <span data-ttu-id="f7516-169">Töltse le hello notification-hubs-0.4.jar fájlt hello **fájlok** hello lapján [Notification-Hubs-Android-SDK a Files](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span><span class="sxs-lookup"><span data-stu-id="f7516-169">Download hello notification-hubs-0.4.jar file from hello **Files** tab of hello [Notification-Hubs-Android-SDK on Bintray](https://bintray.com/microsoftazuremobile/SDK/Notification-Hubs-Android-SDK/0.4).</span></span> <span data-ttu-id="f7516-170">Adja hozzá a hello fájl toohello **függvénytárak** mappa az Eclipse-projekt, és a frissítési hello *függvénytárak* mappát.</span><span class="sxs-lookup"><span data-stu-id="f7516-170">Add hello file toohello **libs** folder of your Eclipse project, and refresh hello *libs* folder.</span></span>
6. <span data-ttu-id="f7516-171">Töltse le és csomagolja ki a hello [Baidu Push Android SDK], nyissa meg hello **függvénytárak** mappát, majd a Másolás hello **pushservice-x.y.z** fájl- és hello jar **armeabi**  &  **mips** hello mappák **függvénytárak** az Android-alkalmazás mappájában.</span><span class="sxs-lookup"><span data-stu-id="f7516-171">Download and unzip hello [Baidu Push Android SDK], open hello **libs** folder, and then copy hello **pushservice-x.y.z** jar file and hello **armeabi** & **mips** folders in hello **libs** folder of your Android application.</span></span>
7. <span data-ttu-id="f7516-172">Nyissa meg hello **AndroidManifest.xml** fájl az Android projektre, majd adja hozzá a hello Baidu SDK által igényelt hello engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="f7516-172">Open hello **AndroidManifest.xml** file of your Android project and add hello permissions that are required by hello Baidu SDK.</span></span>
   
        <uses-permission android:name="android.permission.INTERNET" />
        <uses-permission android:name="android.permission.READ_PHONE_STATE" />
        <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
        <uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />
        <uses-permission android:name="android.permission.WRITE_SETTINGS" />
        <uses-permission android:name="android.permission.VIBRATE" />
        <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
        <uses-permission android:name="android.permission.DISABLE_KEYGUARD" />
        <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
        <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
        <uses-permission android:name="android.permission.ACCESS_DOWNLOAD_MANAGER" />
        <uses-permission android:name="android.permission.DOWNLOAD_WITHOUT_NOTIFICATION" />
8. <span data-ttu-id="f7516-173">Adja hozzá a hello **android: name** tulajdonság tooyour **alkalmazás** elemében **AndroidManifest.xml**, ahol *com.example.baidutest* (a a példában **com.example.BaiduTest**).</span><span class="sxs-lookup"><span data-stu-id="f7516-173">Add hello **android:name** property tooyour **application** element in **AndroidManifest.xml**, replacing *yourprojectname* (for example, **com.example.BaiduTest**).</span></span> <span data-ttu-id="f7516-174">Győződjön meg arról, hogy ez a projektnév megegyezik-e a hello egy hello Baidu-konzolon konfigurált.</span><span class="sxs-lookup"><span data-stu-id="f7516-174">Make sure that this project name matches hello one that you configured in hello Baidu console.</span></span>
   
        <application android:name="yourprojectname.DemoApplication"
9. <span data-ttu-id="f7516-175">Adja hozzá a következő konfigurációs hello alkalmazás elemen belül hello után hello **. MainActivity** tevékenységelem cseréje *com.example.baidutest* (például **com.example.BaiduTest**):</span><span class="sxs-lookup"><span data-stu-id="f7516-175">Add hello following configuration within hello application element after hello **.MainActivity** activity element, replacing *yourprojectname* (for example, **com.example.BaiduTest**):</span></span>
   
        <receiver android:name="yourprojectname.MyPushMessageReceiver">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.MESSAGE" />
                <action android:name="com.baidu.android.pushservice.action.RECEIVE" />
                <action android:name="com.baidu.android.pushservice.action.notification.CLICK" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.PushServiceReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="android.intent.action.BOOT_COMPLETED" />
                <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
                <action android:name="com.baidu.android.pushservice.action.notification.SHOW" />
            </intent-filter>
        </receiver>
   
        <receiver android:name="com.baidu.android.pushservice.RegistrationReceiver"
            android:process=":bdservice_v1">
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.METHOD" />
                <action android:name="com.baidu.android.pushservice.action.BIND_SYNC" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.PACKAGE_REMOVED"/>
                <data android:scheme="package" />
            </intent-filter>
        </receiver>
   
        <service
            android:name="com.baidu.android.pushservice.PushService"
            android:exported="true"
            android:process=":bdservice_v1"  >
            <intent-filter>
                <action android:name="com.baidu.android.pushservice.action.PUSH_SERVICE" />
            </intent-filter>
        </service>
10. <span data-ttu-id="f7516-176">Adja hozzá egy új osztályt **ConfigurationSettings.java** toohello projekt.</span><span class="sxs-lookup"><span data-stu-id="f7516-176">Add a new class called **ConfigurationSettings.java** toohello project.</span></span>
    
     ![][28]
    
     ![][29]
11. <span data-ttu-id="f7516-177">Adja hozzá a következő kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="f7516-177">Add hello following code tooit:</span></span>
    
        public class ConfigurationSettings {
                public static String API_KEY = "...";
                public static String NotificationHubName = "...";
                public static String NotificationHubConnectionString = "...";
            }
    
    <span data-ttu-id="f7516-178">Állítsa be hello **API_KEY** lekért hello Baidu felhőalapú projektet korábban, a **NotificationHubName** hello klasszikus Azure portálról származó értesítésiközpont-nevet a és  **NotificationHubConnectionString** és a klasszikus Azure portál hello a defaultlistensharedaccesssignature értéket.</span><span class="sxs-lookup"><span data-stu-id="f7516-178">Set hello value of **API_KEY** with what you retrieved from hello Baidu cloud project earlier, **NotificationHubName** with your notification hub name from hello Azure Classic Portal and **NotificationHubConnectionString** with DefaultListenSharedAccessSignature from hello Azure Classic Portal.</span></span>
12. <span data-ttu-id="f7516-179">Adja hozzá egy új osztályt **DemoApplication.java**, és adja hozzá a következő kód tooit hello:</span><span class="sxs-lookup"><span data-stu-id="f7516-179">Add a new class called **DemoApplication.java**, and add hello following code tooit:</span></span>
    
        import com.baidu.frontia.FrontiaApplication;
    
        public class DemoApplication extends FrontiaApplication {
            @Override
            public void onCreate() {
                super.onCreate();
            }
        }
13. <span data-ttu-id="f7516-180">Adja hozzá egy másik új osztályt **MyPushMessageReceiver.java**, és adja hozzá a következő kód tooit hello.</span><span class="sxs-lookup"><span data-stu-id="f7516-180">Add another new class called **MyPushMessageReceiver.java**, and add hello following code tooit.</span></span> <span data-ttu-id="f7516-181">Leírók hello hello Baidu leküldési kiszolgálóról kapott leküldéses értesítések hello osztály.</span><span class="sxs-lookup"><span data-stu-id="f7516-181">It is hello class that handles hello push notifications that are received from hello Baidu push server.</span></span>
    
        import java.util.List;
        import android.content.Context;
        import android.os.AsyncTask;
        import android.util.Log;
        import com.baidu.frontia.api.FrontiaPushMessageReceiver;
        import com.microsoft.windowsazure.messaging.NotificationHub;
    
        public class MyPushMessageReceiver extends FrontiaPushMessageReceiver {
            /** TAG tooLog */
            public static NotificationHub hub = null;
            public static String mChannelId, mUserId;
            public static final String TAG = MyPushMessageReceiver.class
                    .getSimpleName();
    
            @Override
            public void onBind(Context context, int errorCode, String appid,
                    String userId, String channelId, String requestId) {
                String responseString = "onBind errorCode=" + errorCode + " appid="
                        + appid + " userId=" + userId + " channelId=" + channelId
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
                mChannelId = channelId;
                mUserId = userId;
    
                try {
                    if (hub == null) {
                        hub = new NotificationHub(
                                ConfigurationSettings.NotificationHubName,
                                ConfigurationSettings.NotificationHubConnectionString,
                                context);
                        Log.i(TAG, "Notification hub initialized");
                    }
                } catch (Exception e) {
                   Log.e(TAG, e.getMessage());
                }
    
                registerWithNotificationHubs();
            }
    
            private void registerWithNotificationHubs() {
               new AsyncTask<Void, Void, Void>() {
                  @Override
                  protected Void doInBackground(Void... params) {
                     try {
                         hub.registerBaidu(mUserId, mChannelId);
                         Log.i(TAG, "Registered with Notification Hub - '"
                                 + ConfigurationSettings.NotificationHubName + "'"
                                 + " with UserId - '"
                                 + mUserId + "' and Channel Id - '"
                                 + mChannelId + "'");
                     } catch (Exception e) {
                         Log.e(TAG, e.getMessage());
                     }
                     return null;
                 }
               }.execute(null, null, null);
            }
    
            @Override
            public void onSetTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onSetTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onDelTags(Context context, int errorCode,
                    List<String> sucessTags, List<String> failTags, String requestId) {
                String responseString = "onDelTags errorCode=" + errorCode
                        + " sucessTags=" + sucessTags + " failTags=" + failTags
                        + " requestId=" + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onListTags(Context context, int errorCode, List<String> tags,
                    String requestId) {
                String responseString = "onListTags errorCode=" + errorCode + " tags="
                        + tags;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onUnbind(Context context, int errorCode, String requestId) {
                String responseString = "onUnbind errorCode=" + errorCode
                        + " requestId = " + requestId;
                Log.d(TAG, responseString);
            }
    
            @Override
            public void onNotificationClicked(Context context, String title,
                    String description, String customContentString) {
                String notifyString = "title=\"" + title + "\" description=\""
                        + description + "\" customContent=" + customContentString;
                Log.d(TAG, notifyString);
            }
    
            @Override
            public void onMessage(Context context, String message,
                    String customContentString) {
                String messageString = "message=\"" + message + "\" customContentString=" + customContentString;
                Log.d(TAG, messageString);
            }
        }
14. <span data-ttu-id="f7516-182">Nyissa meg **MainActivity.java**, és adja hozzá a következő toohello hello **onCreate** módszert:</span><span class="sxs-lookup"><span data-stu-id="f7516-182">Open **MainActivity.java**, and add hello following toohello **onCreate** method:</span></span>
    
            PushManager.startWork(getApplicationContext(),
                    PushConstants.LOGIN_TYPE_API_KEY, ConfigurationSettings.API_KEY);
15. <span data-ttu-id="f7516-183">Nyissa meg a következő importálási utasításokat hello felső hello:</span><span class="sxs-lookup"><span data-stu-id="f7516-183">Open hello following import statements at hello top:</span></span>
    
            import com.baidu.android.pushservice.PushConstants;
            import com.baidu.android.pushservice.PushManager;

## <a name="send-notifications-tooyour-app"></a><span data-ttu-id="f7516-184">Értesítések tooyour app küldése</span><span class="sxs-lookup"><span data-stu-id="f7516-184">Send notifications tooyour app</span></span>
<span data-ttu-id="f7516-185">Gyorsan tesztelheti értesítések fogadásának az alkalmazásban való értesítések a hello [Azure-portálon](https://portal.azure.com/) hello segítségével **küldése** hello értesítési központot, ahogy az a következő képernyő hello gombra:</span><span class="sxs-lookup"><span data-stu-id="f7516-185">You can quickly test receiving notifications in your app by sending notifications in hello [Azure portal](https://portal.azure.com/) using hello **Send** button on hello notification hub, as shown in hello following screen:</span></span>

![](./media/notification-hubs-baidu-get-started/notification-hub-test-send-baidu.png)

<span data-ttu-id="f7516-186">A leküldéses értesítések küldése általában olyan háttérszolgáltatásokon keresztül történik egy kompatibilis kódtár használatával, mint a Mobile Services vagy az ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f7516-186">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="f7516-187">Ha a szalagtár nem érhető el a háttér-, hello REST API-t használhatja közvetlenül a toosend értesítési üzeneteket.</span><span class="sxs-lookup"><span data-stu-id="f7516-187">If a library is not available for your back-end, you can use hello REST API directly toosend notification messages .</span></span>

<span data-ttu-id="f7516-188">Az oktatóanyag azt legyen egyszerű és csak az ügyfélalkalmazás tesztelését küldött értesítésekkel hello .NET SDK használatával egy háttér-szolgáltatás helyett egy konzolalkalmazás értesítési központjának bemutatása.</span><span class="sxs-lookup"><span data-stu-id="f7516-188">In this tutorial, we keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="f7516-189">Azt javasoljuk, hogy hello [Notification Hubs használata toopush értesítések toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) oktatóanyagot hello következő lépés az értesítéseknek ASP.NET-háttérrendszerből történő küldéséhez.</span><span class="sxs-lookup"><span data-stu-id="f7516-189">We recommend hello [Use Notification Hubs toopush notifications toousers](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="f7516-190">A következő módszerekkel hello azonban az értesítések küldésével használhatók:</span><span class="sxs-lookup"><span data-stu-id="f7516-190">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="f7516-191">**REST-felület**: bármely háttér platformon hello segítségével támogathatja az értesítéseket [REST-felület](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="f7516-191">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="f7516-192">**A Microsoft Azure Notification Hubs .NET SDK**: hello Nuget-Csomagkezelőt a Visual Studio, a futtasson [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="f7516-192">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="f7516-193">**NODE.js**: [hogyan toouse Notification Hubs Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f7516-193">**Node.js**: [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="f7516-194">**Mobile Apps**: A példa bemutatja, hogyan toosend értesítések a Notification Hubs szolgáltatással integrált Azure App Service Mobile Apps-háttérrendszerből lásd [Hozzáadás leküldéses értesítések tooyour mobilalkalmazás](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="f7516-194">**Mobile Apps**: For an example of how toosend notifications from an Azure App Service Mobile Apps backend that's integrated with Notification Hubs, see [Add push notifications tooyour mobile app](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="f7516-195">**Java / PHP**: hogyan toosend értesítések használatával hello REST API-k példát lásd: "hogyan toouse Notification Hubs Java/php-ből" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="f7516-195">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-net-console-app"></a><span data-ttu-id="f7516-196">(Nem kötelező) Értesítések küldése .NET-konzolalkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="f7516-196">(Optional) Send notifications from a .NET console app.</span></span>
<span data-ttu-id="f7516-197">Ebben a szakaszban az értesítések .NET-konzolalkalmazásból történő küldését mutatjuk be.</span><span class="sxs-lookup"><span data-stu-id="f7516-197">In this section, we show sending a notification using a .NET console app.</span></span>

1. <span data-ttu-id="f7516-198">Hozzon létre egy új Visual C#-konzolalkalmazást:</span><span class="sxs-lookup"><span data-stu-id="f7516-198">Create a new Visual C# console application:</span></span>
   
    ![][30]
2. <span data-ttu-id="f7516-199">Hello Package Manager Console ablakban, állítson be hello **alapértelmezett projekt** tooyour új projekt konzolról, és majd hello konzolablakban hajtható végre a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="f7516-199">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="f7516-200">Ezeket az utasításokat ad a hivatkozás toohello Azure Notification Hubs SDK használatával hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-csomag</a>.</span><span class="sxs-lookup"><span data-stu-id="f7516-200">This instruction adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
3. <span data-ttu-id="f7516-201">Megnyitás hello fájl **Program.cs** és adja hozzá hello következő using utasítást:</span><span class="sxs-lookup"><span data-stu-id="f7516-201">Open hello file **Program.cs** and add hello following using statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
4. <span data-ttu-id="f7516-202">Az a `Program` osztályban adja hozzá a következő metódus hello és cserélje le *DefaultFullSharedAccessSignatureSASConnectionString* és *NotificationHubName* hello értékekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="f7516-202">In your `Program` class, add hello following method and replace *DefaultFullSharedAccessSignatureSASConnectionString* and *NotificationHubName* with hello values that you have.</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString("DefaultFullSharedAccessSignatureSASConnectionString", "NotificationHubName");
            string message = "{\"title\":\"((Notification title))\",\"description\":\"Hello from Azure\"}";
            var result = await hub.SendBaiduNativeNotificationAsync(message);
        }
5. <span data-ttu-id="f7516-203">Adja hozzá az alábbi hello a **fő** módszert:</span><span class="sxs-lookup"><span data-stu-id="f7516-203">Add hello following lines in your **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();

## <a name="test-your-app"></a><span data-ttu-id="f7516-204">Az alkalmazás tesztelése</span><span class="sxs-lookup"><span data-stu-id="f7516-204">Test your app</span></span>
<span data-ttu-id="f7516-205">Ez az alkalmazás egy valódi telefonon, egyszerűen csatlakoztassa tootest hello phone tooyour számítógép USB-kábelen keresztül.</span><span class="sxs-lookup"><span data-stu-id="f7516-205">tootest this app with an actual phone, just connect hello phone tooyour computer by using a USB cable.</span></span> <span data-ttu-id="f7516-206">Ez a művelet betölti az alkalmazás feltöltődik a csatlakoztatott hello phone.</span><span class="sxs-lookup"><span data-stu-id="f7516-206">This action loads your app onto hello attached phone.</span></span>

<span data-ttu-id="f7516-207">tootest hello emulátorral hello Eclipse felső eszköztárán, az alkalmazás kattintson **futtatása**, majd válassza ki az alkalmazást: hello emulátor, terhelés esetén kezdődik, és futtat hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="f7516-207">tootest this app with hello emulator, on hello Eclipse top toolbar, click **Run**, and then select your app: it starts hello emulator, loads, and runs hello app.</span></span>

<span data-ttu-id="f7516-208">hello az alkalmazás lekéri hello "userId" és a "channelId" hello Baidu leküldéses értesítéseket kezelő szolgáltatása, és regisztrálja hello értesítési központban.</span><span class="sxs-lookup"><span data-stu-id="f7516-208">hello app retrieves hello 'userId' and 'channelId' from hello Baidu Push notification service and registers with hello notification hub.</span></span>

<span data-ttu-id="f7516-209">tesztértesítés toosend, hello klasszikus Azure portál hibakeresési lapjáról hello is használhatja.</span><span class="sxs-lookup"><span data-stu-id="f7516-209">toosend a test notification, you can use hello debug tab of hello Azure Classic Portal.</span></span> <span data-ttu-id="f7516-210">Ha a hello .NET konzolalkalmazást a Visual Studio, csak Entert kell hello F5 billentyűt a Visual Studio toorun hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="f7516-210">If you built hello .NET console application for Visual Studio, just press hello F5 key in Visual Studio toorun hello application.</span></span> <span data-ttu-id="f7516-211">hello alkalmazás hello az eszköz vagy az emulátor felső értesítési területén megjelenő értesítést küld.</span><span class="sxs-lookup"><span data-stu-id="f7516-211">hello application sends a notification that appears in hello top notification area of your device or emulator.</span></span>

<!-- Images. -->
[1]: ./media/notification-hubs-baidu-get-started/BaiduRegistration.png
[2]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationInput.png
[3]: ./media/notification-hubs-baidu-get-started/BaiduConfirmation.png
[4]: ./media/notification-hubs-baidu-get-started/BaiduActivationEmail.png
[5]: ./media/notification-hubs-baidu-get-started/BaiduRegistrationMore.png
[6]: ./media/notification-hubs-baidu-get-started/BaiduOpenCloudPlatform.png
[7]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperServices.png
[8]: ./media/notification-hubs-baidu-get-started/BaiduDeveloperRegistration.png
[9]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationInput.png
[10]: ./media/notification-hubs-baidu-get-started/BaiduDevRegistrationConfirmation.png
[11]: ./media/notification-hubs-baidu-get-started/BaiduDevConfirmationFinal.png
[12]: ./media/notification-hubs-baidu-get-started/BaiduCloudPush.png
[13]: ./media/notification-hubs-baidu-get-started/BaiduDevSvcMgmt.png
[14]: ./media/notification-hubs-baidu-get-started/BaiduCreateProject.png
[15]: ./media/notification-hubs-baidu-get-started/BaiduCreateProjectInput.png
[16]: ./media/notification-hubs-baidu-get-started/BaiduProjectKeys.png
[17]: ./media/notification-hubs-baidu-get-started/AzureNHCreation.png
[18]: ./media/notification-hubs-baidu-get-started/NotificationHubs.png
[19]: ./media/notification-hubs-baidu-get-started/NotificationHubsConfigure.png
[20]: ./media/notification-hubs-baidu-get-started/NotificationHubBaiduConfigure.png
[21]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionStringView.png
[22]: ./media/notification-hubs-baidu-get-started/NotificationHubsConnectionString.png
[23]: ./media/notification-hubs-baidu-get-started/EclipseNewProject.png
[24]: ./media/notification-hubs-baidu-get-started/EclipseProjectCreation.png
[25]: ./media/notification-hubs-baidu-get-started/EclipseBlankActivity.png
[26]: ./media/notification-hubs-baidu-get-started/EclipseProjectBuildProperty.png
[27]: ./media/notification-hubs-baidu-get-started/EclipseBaiduReferences.png
[28]: ./media/notification-hubs-baidu-get-started/EclipseNewClass.png
[29]: ./media/notification-hubs-baidu-get-started/EclipseConfigSettingsClass.png
[30]: ./media/notification-hubs-baidu-get-started/ConsoleProject.png
[31]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig1.png
[32]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig2.png
[33]: ./media/notification-hubs-baidu-get-started/BaiduPushConfig3.png

<!-- URLs. -->
[Mobile Services Android SDK]: https://go.microsoft.com/fwLink/?LinkID=280126&clcid=0x409
[Baidu Push Android SDK]: http://developer.baidu.com/wiki/index.php?title=docs/cplat/push/sdk/clientsdk
[klasszikus Azure portál]: https://manage.windowsazure.com/
[Baidu portálra]: http://www.baidu.com/
