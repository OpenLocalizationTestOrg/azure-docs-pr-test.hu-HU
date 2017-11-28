---
title: "aaaAccessing az alkalmazások bármely eszközről |} Microsoft Docs"
description: "Ismerje meg, hogy mely ügyfelek Azure RemoteApp támogatja, és hogyan tooaccess az alkalmazásokat."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: fb7bd17d-7aa8-43fd-9278-f96e0e9308e4
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 15985b40d870e3155d4132063bf5b9677ff9afed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a><span data-ttu-id="1fd69-103">Az alkalmazások elérése az Azure RemoteAppban</span><span class="sxs-lookup"><span data-stu-id="1fd69-103">Accessing your apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1fd69-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="1fd69-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="1fd69-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="1fd69-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="1fd69-106">Az Azure RemoteApp hello beauties egyike, hogy elérhető alkalmazások bármely eszközén.</span><span class="sxs-lookup"><span data-stu-id="1fd69-106">One of hello beauties of Azure RemoteApp is that you can access apps from any of your devices.</span></span> <span data-ttu-id="1fd69-107">Még jobban dolgozni az eszközöket és zökkenőmentesen átmenet tooa második eszköz és jobb átvételéhez, ahol abbahagyta.</span><span class="sxs-lookup"><span data-stu-id="1fd69-107">Even better, you can start working on one device and then seamlessly transition tooa second device and pick up right where you left off.</span></span> <span data-ttu-id="1fd69-108">tooget lépések akkor toodownload hello megfelelő ügyfél szükséges az eszközt, és jelentkezzen be toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1fd69-108">tooget started you need toodownload hello appropriate client for your device and sign in toohello service.</span></span>

<span data-ttu-id="1fd69-109">Ez a témakör azt áttekinti jelenleg támogatott hello ügyfelek és hogyan toodownload őket előtt I mutatja be az egyes hello ügyfelek tooRemoteApp toosign.</span><span class="sxs-lookup"><span data-stu-id="1fd69-109">In this topic, we'll review hello clients currently supported and how toodownload them before I show you how toosign in tooRemoteApp from each of hello clients.</span></span>

## <a name="supported-clients"></a><span data-ttu-id="1fd69-110">Támogatott kliensek</span><span class="sxs-lookup"><span data-stu-id="1fd69-110">Supported clients</span></span>
<span data-ttu-id="1fd69-111">Az alábbi hello lépéseket követve, ha az eszköz fut a következő operációs rendszerek RemoteApp érhető el:</span><span class="sxs-lookup"><span data-stu-id="1fd69-111">You can access RemoteApp using hello steps below if your device is running one of these operating systems:</span></span>

* <span data-ttu-id="1fd69-112">Windows 10</span><span class="sxs-lookup"><span data-stu-id="1fd69-112">Windows 10</span></span> 
* <span data-ttu-id="1fd69-113">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="1fd69-113">Windows 8.1</span></span>
* <span data-ttu-id="1fd69-114">Windows 8</span><span class="sxs-lookup"><span data-stu-id="1fd69-114">Windows 8</span></span>
* <span data-ttu-id="1fd69-115">Windows 7 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="1fd69-115">Windows 7 Service Pack 1</span></span>
* <span data-ttu-id="1fd69-116">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="1fd69-116">Windows Phone 8.1</span></span>
* <span data-ttu-id="1fd69-117">iOS</span><span class="sxs-lookup"><span data-stu-id="1fd69-117">iOS</span></span>
* <span data-ttu-id="1fd69-118">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="1fd69-118">Mac OS X</span></span>
* <span data-ttu-id="1fd69-119">Android</span><span class="sxs-lookup"><span data-stu-id="1fd69-119">Android</span></span>

 <span data-ttu-id="1fd69-120">Mi a helyzet a vékony ügyfeleket? a következő Windows Embedded vékony ügyfelek támogatottak hello:</span><span class="sxs-lookup"><span data-stu-id="1fd69-120">What about thin clients? hello following Windows Embedded thin clients are supported:</span></span>

* <span data-ttu-id="1fd69-121">Windows Embedded Standard 7</span><span class="sxs-lookup"><span data-stu-id="1fd69-121">Windows Embedded Standard 7</span></span>
* <span data-ttu-id="1fd69-122">Windows Embedded 8 Standard</span><span class="sxs-lookup"><span data-stu-id="1fd69-122">Windows Embedded 8 Standard</span></span>
* <span data-ttu-id="1fd69-123">Windows Embedded 8.1 Industry Pro</span><span class="sxs-lookup"><span data-stu-id="1fd69-123">Windows Embedded 8.1 Industry Pro</span></span>
* <span data-ttu-id="1fd69-124">Windows 10 IoT Enterprise</span><span class="sxs-lookup"><span data-stu-id="1fd69-124">Windows 10 IoT Enterprise</span></span>

## <a name="downloading-hello-client"></a><span data-ttu-id="1fd69-125">Hello ügyfél letöltése</span><span class="sxs-lookup"><span data-stu-id="1fd69-125">Downloading hello client</span></span>
<span data-ttu-id="1fd69-126">Függetlenül attól, milyen platformot használ, a RemoteApp hello található tooaccess kell hello ügyfél [távoli asztali ügyfél letöltési](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="1fd69-126">No matter what platform you are using, hello client you need tooaccess RemoteApp can be found on hello [Remote Desktop client download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) page.</span></span>

<span data-ttu-id="1fd69-127">Hello gombra kattintva másik hivatkozások fog vagy közvetlenül hello ügyfél letöltése vagy megküldeni toohello ügyfélprogram letöltési oldaláról hello app Store-ból a platformhoz való.</span><span class="sxs-lookup"><span data-stu-id="1fd69-127">Clicking hello different links will either directly start downloading hello client or will send you toohello client download page in hello app store for that platform.</span></span> <span data-ttu-id="1fd69-128">Hello ügyfél telepítése az üdvözlő képernyőt hello utasításokat követve.</span><span class="sxs-lookup"><span data-stu-id="1fd69-128">Install hello client by following hello instructions on hello screen.</span></span>

<span data-ttu-id="1fd69-129">Miután hello ügyfél telepített az eszközön, és akkor indul el, jump toohello megfelelő című szakaszt toolearn hogyan tooRemoteApp az, hogy az ügyfél a toosign.</span><span class="sxs-lookup"><span data-stu-id="1fd69-129">Once you have installed hello client on your device and launched it, jump toohello corresponding section below toolearn how toosign in tooRemoteApp from that client.</span></span>

## <a name="android"></a><span data-ttu-id="1fd69-130">Android</span><span class="sxs-lookup"><span data-stu-id="1fd69-130">Android</span></span>
<span data-ttu-id="1fd69-131">A Google Play áruház hello hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-131">Once you have installed hello Microsoft Remote Desktop app from hello Google Play store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="1fd69-132">Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-132">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="1fd69-133">az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** koppintson **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-133">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Azure RemoteApp**.</span></span>    
   
     ![Üres csatlakozási központ](./media/remoteapp-clients/Android1.png)
2. <span data-ttu-id="1fd69-135">Az e-mail cím tooaccess hello szolgáltatást be kell toosign.</span><span class="sxs-lookup"><span data-stu-id="1fd69-135">You need toosign in with your email address tooaccess hello service.</span></span> <span data-ttu-id="1fd69-136">Koppintson a **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-136">Tap **Get started**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Android2.png)
3. <span data-ttu-id="1fd69-138">Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-138">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="1fd69-139">Ezzel megkezdődik a hello bejelentkezési folyamat az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="1fd69-139">This begins hello sign-in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/Android3.png)
4. <span data-ttu-id="1fd69-141">Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (korábbi nevén a "Live ID") vagy a szervezeti azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="1fd69-141">Follow hello instructions on hello screen toosign in with your Microsoft account (previously called "LiveID") or organization ID.</span></span> <span data-ttu-id="1fd69-142">Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot.</span><span class="sxs-lookup"><span data-stu-id="1fd69-142">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="1fd69-143">Ha, válassza ki a hello meghívókat megbízik, és koppintson **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-143">If you are, select hello invitations you trust and tap **Done**.</span></span>    
   
    ![Meghívókat lap](./media/remoteapp-clients/Android4.png)
5. <span data-ttu-id="1fd69-145">A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello.</span><span class="sxs-lookup"><span data-stu-id="1fd69-145">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="1fd69-146">Koppintson a hello alkalmazások toostart használja azt.</span><span class="sxs-lookup"><span data-stu-id="1fd69-146">Tap one of hello apps toostart using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Android5.png)
6. <span data-ttu-id="1fd69-148">Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-148">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="1fd69-149">toodo tehát koppintson **toofree próbaverzió Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="1fd69-149">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Android6.png)
7. <span data-ttu-id="1fd69-151">Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.</span><span class="sxs-lookup"><span data-stu-id="1fd69-151">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a><span data-ttu-id="1fd69-153">iOS</span><span class="sxs-lookup"><span data-stu-id="1fd69-153">iOS</span></span>
<span data-ttu-id="1fd69-154">Hello App store-ból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztali ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-154">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **RD Client**.</span></span>

1. <span data-ttu-id="1fd69-155">Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-155">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="1fd69-156">az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** koppintson **adja hozzá az Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-156">tooget started with Azure RemoteApp, tap hello add button **""+""** and tap **Add Azure RemoteApp**.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/IOS1.png)
2. <span data-ttu-id="1fd69-158">Szüksége toosign be az e-mail cím tooaccess hello szolgáltatást, toostart folyamat, írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-158">You need toosign in with your email address tooaccess hello service, toostart that process, type in your **email address** and tap **Continue**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/picture1.png)
3. <span data-ttu-id="1fd69-160">Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="1fd69-160">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="1fd69-161">Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot.</span><span class="sxs-lookup"><span data-stu-id="1fd69-161">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="1fd69-162">Ha, válassza ki a hello meghívókat megbízik, és koppintson **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-162">If you are, select hello invitations you trust and tap **Done**.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/IOS3.png)
4. <span data-ttu-id="1fd69-164">A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello.</span><span class="sxs-lookup"><span data-stu-id="1fd69-164">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="1fd69-165">Koppintson a hello alkalmazások toolaunch egyikére, majd újrakezdi a használja azt.</span><span class="sxs-lookup"><span data-stu-id="1fd69-165">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/IOS4.png)
5. <span data-ttu-id="1fd69-167">Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-167">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="1fd69-168">toodo tehát koppintson **toofree próbaverzió Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="1fd69-168">toodo so, tap **Go toofree trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/IOS5.png)
6. <span data-ttu-id="1fd69-170">Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.</span><span class="sxs-lookup"><span data-stu-id="1fd69-170">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a><span data-ttu-id="1fd69-172">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="1fd69-172">Mac OS X</span></span>
<span data-ttu-id="1fd69-173">Hello App store-ból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **Microsoft távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-173">Once you have installed hello Microsoft Remote Desktop app from hello App store, you can find it in your app list under **Microsoft Remote Desktop**.</span></span>

1. <span data-ttu-id="1fd69-174">Indító hello app biztosít tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-174">Launching hello app brings you tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="1fd69-175">az Azure RemoteApp lépései tooget kattintson hello **Azure RemoteApp** gombra.</span><span class="sxs-lookup"><span data-stu-id="1fd69-175">tooget started with Azure RemoteApp, click hello **Azure RemoteApp** button.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/Mac1.png)
2. <span data-ttu-id="1fd69-177">Toosign kell be az e-mail cím tooaccess hello szolgáltatás, toostart feldolgozó, koppintson **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-177">You need toosign in with your email address tooaccess hello service, toostart that process, tap **Get Started**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Mac2.png)
3. <span data-ttu-id="1fd69-179">Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-179">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="1fd69-180">Ezzel megkezdődik a hello bejelentkezési folyamat során az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="1fd69-180">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/picture2.png)
4. <span data-ttu-id="1fd69-182">Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="1fd69-182">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="1fd69-183">Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot.</span><span class="sxs-lookup"><span data-stu-id="1fd69-183">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="1fd69-184">Ha, válassza ki a megbízható, és zárja be a párbeszédpanelt hello hello meghívókat.</span><span class="sxs-lookup"><span data-stu-id="1fd69-184">If you are, select hello invitations you trust and close hello dialog.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/Mac4.png)
5. <span data-ttu-id="1fd69-186">A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello.</span><span class="sxs-lookup"><span data-stu-id="1fd69-186">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="1fd69-187">Kattintson duplán a hello alkalmazások toolaunch majd újrakezdi a használja azt.</span><span class="sxs-lookup"><span data-stu-id="1fd69-187">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Mac5.png)
6. <span data-ttu-id="1fd69-189">Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-189">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="1fd69-190">toodo kattintson **toofree próbaverzió Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="1fd69-190">toodo so, click **Go toofree trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Mac6.png)
7. <span data-ttu-id="1fd69-192">Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.</span><span class="sxs-lookup"><span data-stu-id="1fd69-192">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a><span data-ttu-id="1fd69-194">Windows (Windows Phone kivételével az összes támogatott verzió)</span><span class="sxs-lookup"><span data-stu-id="1fd69-194">Windows (All supported versions except Windows Phone)</span></span>
<span data-ttu-id="1fd69-195">hello ügyfél automatikusan elindul telepíti, azonban ha tooaccess kell azt újra később, található az alkalmazáslistában hello néven művelet befejeződése után **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-195">hello client launches automatically after it finishes installing, however when you need tooaccess it again later it can be found in your app list under hello name **Azure RemoteApp**.</span></span>

1. <span data-ttu-id="1fd69-196">Hello ügyfél, hello első képernyőn látható elindításához ésőbb üdvözlőlapjának tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="1fd69-196">Ater launching hello client, hello first page you see welcomes you tooAzure RemoteApp.</span></span> <span data-ttu-id="1fd69-197">tooproceed, kattintson a **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-197">tooproceed, click on **Get Started**.</span></span>
   
    ![Hello Azure RemoteApp-ügyfélen kezdőlapja](./media/remoteapp-clients/Windows1.png)
2. <span data-ttu-id="1fd69-199">hello következő elindul a hello bejelentkezési folyamat során az Azure RemoteApp az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="1fd69-199">hello next page starts hello sign in process for Azure RemoteApp using Azure Active Directory.</span></span> <span data-ttu-id="1fd69-200">Ez a folyamat ismerős, ha van Microsoft-szolgáltatásokkal a múltbeli hello kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="1fd69-200">This process should look familiar if you have used Microsoft services in hello past.</span></span> <span data-ttu-id="1fd69-201">Beírásával indítsa el a **e-mail cím** kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-201">Start by typing your **email address** and click **Continue**.</span></span>
   
    ![Első Azure Active Directory-kérés](./media/remoteapp-clients/Windows2.png)
3. <span data-ttu-id="1fd69-203">Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="1fd69-203">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="1fd69-204">Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot.</span><span class="sxs-lookup"><span data-stu-id="1fd69-204">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="1fd69-205">Ha, válassza ki a hello meghívókat megbízik, és kattintson a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-205">If you are, select hello invitations you trust and click **Done**.</span></span>
   
    ![Hello Azure RemoteApp-ügyfélen meghívókat lapja](./media/remoteapp-clients/Windows3.png)
4. <span data-ttu-id="1fd69-207">A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello.</span><span class="sxs-lookup"><span data-stu-id="1fd69-207">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="1fd69-208">Kattintson duplán a hello alkalmazások toolaunch majd újrakezdi a használja azt.</span><span class="sxs-lookup"><span data-stu-id="1fd69-208">Double-click one of hello apps toolaunch it and start using it.</span></span>
   
    ![Kapcsolat Center hello Azure RemoteApp ügyfél](./media/remoteapp-clients/Windows4.png)
5. <span data-ttu-id="1fd69-210">Ha nem küldött meghívót még, ne aggódjon van, a kezelt!</span><span class="sxs-lookup"><span data-stu-id="1fd69-210">If no one has sent you an invitation yet, don't worry we've got you covered!</span></span> <span data-ttu-id="1fd69-211">Hozzáférés tooa bemutató gyűjtemény, amellyel tesztelheti, hello szolgáltatás továbbra is kell.</span><span class="sxs-lookup"><span data-stu-id="1fd69-211">You'll still have access tooa demo collection so you can test out hello service.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a><span data-ttu-id="1fd69-213">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="1fd69-213">Windows Phone 8.1</span></span>
<span data-ttu-id="1fd69-214">Windows Phone 8.1 hello áruházból hello Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-214">Once you have installed hello Microsoft Remote Desktop app from hello Windows Phone 8.1 store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="1fd69-215">Indító hello alkalmazás számos lehetőséget kínál, közvetlenül tooan üres csatlakozási központ, kivéve, ha már eddig használt hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-215">Launching hello app brings you directly tooan empty Connection Center, unless you've already been using hello app.</span></span> <span data-ttu-id="1fd69-216">az Azure RemoteApp lépései tooget, koppintson a hello Hozzáadás gomb **"" +""** üdvözlő képernyőt hello alján.</span><span class="sxs-lookup"><span data-stu-id="1fd69-216">tooget started with Azure RemoteApp, tap hello add button **""+""** at hello bottom of hello screen.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/WinPhone1.png)
2. <span data-ttu-id="1fd69-218">A következő koppintson **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-218">Next, tap on **Azure RemoteApp**.</span></span>
   
    ![Konfigurációelem-weblap hozzáadása](./media/remoteapp-clients/WinPhone2.png)
3. <span data-ttu-id="1fd69-220">Toosign kell be az e-mail cím tooaccess hello szolgáltatás, toostart feldolgozó, koppintson **csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-220">You need toosign in with your email address tooaccess hello service, toostart that process, tap **connect**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/WinPhone3.png)
4. <span data-ttu-id="1fd69-222">Hello következő lapon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-222">On hello next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="1fd69-223">Ezzel megkezdődik a hello bejelentkezési folyamat során az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="1fd69-223">This begins hello sign in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/WinPhone4.png)
5. <span data-ttu-id="1fd69-225">Hello utasításokat kövesse a megjelenő hello képernyő toosign be a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával.</span><span class="sxs-lookup"><span data-stu-id="1fd69-225">Follow hello instructions on hello screen toosign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="1fd69-226">Miután bejelentkezett, típusától felsoroló összes hello meghívót kapott lapot.</span><span class="sxs-lookup"><span data-stu-id="1fd69-226">Once signed in, you may be presented with a page listing all hello invitations you have received.</span></span> <span data-ttu-id="1fd69-227">Ha, válassza ki a hello meghívókat megbízik, és koppintson **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1fd69-227">If you are, select hello invitations you trust and tap **save**.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/WinPhone5.png)
6. <span data-ttu-id="1fd69-229">A meghívót, az alkalmazások listájának hello elfogadása után hozzáférést toowill letöltött tooyour eszköz kell rendelkeznie, és azt a csatlakozási központ hello.</span><span class="sxs-lookup"><span data-stu-id="1fd69-229">After accepting your invitations, hello list of apps you have access toowill be downloaded tooyour device and made available in hello Connection Center.</span></span> <span data-ttu-id="1fd69-230">Koppintson a hello alkalmazások toolaunch egyikére, majd újrakezdi a használja azt.</span><span class="sxs-lookup"><span data-stu-id="1fd69-230">Tap one of hello apps toolaunch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/WinPhone6.png)
7. <span data-ttu-id="1fd69-232">Ha nem rendelkezik még meghívót, továbbra is kipróbálása hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="1fd69-232">If you do not have an invitation yet, you can still try out hello service.</span></span> <span data-ttu-id="1fd69-233">Igen, koppintson toodo **Igen** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="1fd69-233">toodo so, tap **yes** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/WinPhone7.png)
8. <span data-ttu-id="1fd69-235">Ekkor kap hozzáférés tooa alapvető alkalmazások tooget RemoteApp-t elindította.</span><span class="sxs-lookup"><span data-stu-id="1fd69-235">This will give you access tooa basic set of apps tooget you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/WinPhone8.png)

