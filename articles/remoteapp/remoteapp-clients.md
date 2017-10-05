---
title: "Az alkalmazások bármely eszközről elérése |} Microsoft Docs"
description: "Ismerje meg, milyen ügyfelek használhatók az Azure RemoteApp és az alkalmazások elérését."
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
ms.openlocfilehash: 10a5be6251765b59fac92a33120cedcf8091a677
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="accessing-your-apps-in-azure-remoteapp"></a><span data-ttu-id="0769f-103">Az alkalmazások elérése az Azure RemoteAppban</span><span class="sxs-lookup"><span data-stu-id="0769f-103">Accessing your apps in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="0769f-104">Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik.</span><span class="sxs-lookup"><span data-stu-id="0769f-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="0769f-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="0769f-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="0769f-106">Az Azure RemoteApp beauties egyike, hogy elérhető alkalmazások bármely eszközén.</span><span class="sxs-lookup"><span data-stu-id="0769f-106">One of the beauties of Azure RemoteApp is that you can access apps from any of your devices.</span></span> <span data-ttu-id="0769f-107">Még jobban dolgozni egy eszközön, és majd zökkenőmentesen átmenet egy második eszköz és jobb átvételéhez, ahol abbahagyta.</span><span class="sxs-lookup"><span data-stu-id="0769f-107">Even better, you can start working on one device and then seamlessly transition to a second device and pick up right where you left off.</span></span> <span data-ttu-id="0769f-108">Első lépésként töltse le a megfelelő ügyféloldali az eszközhöz, és jelentkezzen be a szolgáltatás kell.</span><span class="sxs-lookup"><span data-stu-id="0769f-108">To get started you need to download the appropriate client for your device and sign in to the service.</span></span>

<span data-ttu-id="0769f-109">Ebben a témakörben azt áttekinti a jelenleg támogatott ügyfelek és letöltése őket, mielőtt I bemutatják az egyes ügyfelek a RemoteApp bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="0769f-109">In this topic, we'll review the clients currently supported and how to download them before I show you how to sign in to RemoteApp from each of the clients.</span></span>

## <a name="supported-clients"></a><span data-ttu-id="0769f-110">Támogatott kliensek</span><span class="sxs-lookup"><span data-stu-id="0769f-110">Supported clients</span></span>
<span data-ttu-id="0769f-111">Használja az alábbi lépéseket, ha az eszköz fut a következő operációs rendszerek RemoteApp érhető el:</span><span class="sxs-lookup"><span data-stu-id="0769f-111">You can access RemoteApp using the steps below if your device is running one of these operating systems:</span></span>

* <span data-ttu-id="0769f-112">Windows 10</span><span class="sxs-lookup"><span data-stu-id="0769f-112">Windows 10</span></span> 
* <span data-ttu-id="0769f-113">Windows 8.1</span><span class="sxs-lookup"><span data-stu-id="0769f-113">Windows 8.1</span></span>
* <span data-ttu-id="0769f-114">Windows 8</span><span class="sxs-lookup"><span data-stu-id="0769f-114">Windows 8</span></span>
* <span data-ttu-id="0769f-115">Windows 7 Service Pack 1</span><span class="sxs-lookup"><span data-stu-id="0769f-115">Windows 7 Service Pack 1</span></span>
* <span data-ttu-id="0769f-116">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="0769f-116">Windows Phone 8.1</span></span>
* <span data-ttu-id="0769f-117">iOS</span><span class="sxs-lookup"><span data-stu-id="0769f-117">iOS</span></span>
* <span data-ttu-id="0769f-118">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="0769f-118">Mac OS X</span></span>
* <span data-ttu-id="0769f-119">Android</span><span class="sxs-lookup"><span data-stu-id="0769f-119">Android</span></span>

 <span data-ttu-id="0769f-120">Mi a helyzet a vékony ügyfeleket?</span><span class="sxs-lookup"><span data-stu-id="0769f-120">What about thin clients?</span></span> <span data-ttu-id="0769f-121">A következő Windows Embedded vékony ügyfelek támogatottak:</span><span class="sxs-lookup"><span data-stu-id="0769f-121">The following Windows Embedded thin clients are supported:</span></span>

* <span data-ttu-id="0769f-122">Windows Embedded Standard 7</span><span class="sxs-lookup"><span data-stu-id="0769f-122">Windows Embedded Standard 7</span></span>
* <span data-ttu-id="0769f-123">Windows Embedded 8 Standard</span><span class="sxs-lookup"><span data-stu-id="0769f-123">Windows Embedded 8 Standard</span></span>
* <span data-ttu-id="0769f-124">Windows Embedded 8.1 Industry Pro</span><span class="sxs-lookup"><span data-stu-id="0769f-124">Windows Embedded 8.1 Industry Pro</span></span>
* <span data-ttu-id="0769f-125">Windows 10 IoT Enterprise</span><span class="sxs-lookup"><span data-stu-id="0769f-125">Windows 10 IoT Enterprise</span></span>

## <a name="downloading-the-client"></a><span data-ttu-id="0769f-126">Az ügyfél letöltése</span><span class="sxs-lookup"><span data-stu-id="0769f-126">Downloading the client</span></span>
<span data-ttu-id="0769f-127">Függetlenül attól, milyen platformot használ, az ügyfél RemoteApp eléréséhez szükséges található meg a [távoli asztali ügyfél letöltési](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="0769f-127">No matter what platform you are using, the client you need to access RemoteApp can be found on the [Remote Desktop client download](https://www.remoteapp.windowsazure.com/ClientDownload/AllClients.aspx) page.</span></span>

<span data-ttu-id="0769f-128">A különböző hivatkozásokra kattintva fog vagy közvetlenül az ügyfél letöltése vagy elküld az ügyfélnek letöltése az app Store-ból a platformhoz való lap.</span><span class="sxs-lookup"><span data-stu-id="0769f-128">Clicking the different links will either directly start downloading the client or will send you to the client download page in the app store for that platform.</span></span> <span data-ttu-id="0769f-129">A képernyőn megjelenő utasításokat követve telepítse az ügyfelet.</span><span class="sxs-lookup"><span data-stu-id="0769f-129">Install the client by following the instructions on the screen.</span></span>

<span data-ttu-id="0769f-130">Miután telepítette az ügyfelet az eszközön, és elindítja azt, hogyan RemoteApp, hogy az ügyfél jelentkezzen be az alábbi megfelelő szakaszra ugorhat.</span><span class="sxs-lookup"><span data-stu-id="0769f-130">Once you have installed the client on your device and launched it, jump to the corresponding section below to learn how to sign in to RemoteApp from that client.</span></span>

## <a name="android"></a><span data-ttu-id="0769f-131">Android</span><span class="sxs-lookup"><span data-stu-id="0769f-131">Android</span></span>
<span data-ttu-id="0769f-132">A Google Play áruházból a Microsoft távoli asztal alkalmazás telepítése után megtalálja az alkalmazáslistában alatt **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="0769f-132">Once you have installed the Microsoft Remote Desktop app from the Google Play store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="0769f-133">Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0769f-133">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="0769f-134">Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** koppintson **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="0769f-134">To get started with Azure RemoteApp, tap the add button **""+""** and tap **Azure RemoteApp**.</span></span>    
   
     ![Üres csatlakozási központ](./media/remoteapp-clients/Android1.png)
2. <span data-ttu-id="0769f-136">Jelentkezzen be az e-mail címét, a szolgáltatás eléréséhez szükség.</span><span class="sxs-lookup"><span data-stu-id="0769f-136">You need to sign in with your email address to access the service.</span></span> <span data-ttu-id="0769f-137">Koppintson a **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="0769f-137">Tap **Get started**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Android2.png)
3. <span data-ttu-id="0769f-139">A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-139">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="0769f-140">Ezzel megkezdődik a bejelentkezési folyamat az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="0769f-140">This begins the sign-in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/Android3.png)
4. <span data-ttu-id="0769f-142">Kövesse a képernyőn jelentkezzen be a Microsoft-fiók (korábbi nevén a "Live ID") vagy a szervezeti azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="0769f-142">Follow the instructions on the screen to sign in with your Microsoft account (previously called "LiveID") or organization ID.</span></span> <span data-ttu-id="0769f-143">Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot.</span><span class="sxs-lookup"><span data-stu-id="0769f-143">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="0769f-144">Ha, válassza ki a megbízható, és koppintson a meghívókat **végzett**.</span><span class="sxs-lookup"><span data-stu-id="0769f-144">If you are, select the invitations you trust and tap **Done**.</span></span>    
   
    ![Meghívókat lap](./media/remoteapp-clients/Android4.png)
5. <span data-ttu-id="0769f-146">Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center.</span><span class="sxs-lookup"><span data-stu-id="0769f-146">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="0769f-147">Koppintson az alkalmazások indításához használja azt.</span><span class="sxs-lookup"><span data-stu-id="0769f-147">Tap one of the apps to start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Android5.png)
6. <span data-ttu-id="0769f-149">Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja.</span><span class="sxs-lookup"><span data-stu-id="0769f-149">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="0769f-150">Ehhez koppintson **ingyenes Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="0769f-150">To do so, tap **Go to free trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Android6.png)
7. <span data-ttu-id="0769f-152">Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0769f-152">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Android7.png)

## <a name="ios"></a><span data-ttu-id="0769f-154">iOS</span><span class="sxs-lookup"><span data-stu-id="0769f-154">iOS</span></span>
<span data-ttu-id="0769f-155">Miután telepítette a Microsoft távoli asztal alkalmazás az App store-ból, megtalálhatja az alkalmazáslistában alatt **távoli asztali ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="0769f-155">Once you have installed the Microsoft Remote Desktop app from the App store, you can find it in your app list under **RD Client**.</span></span>

1. <span data-ttu-id="0769f-156">Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0769f-156">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="0769f-157">Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** koppintson **adja hozzá az Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="0769f-157">To get started with Azure RemoteApp, tap the add button **""+""** and tap **Add Azure RemoteApp**.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/IOS1.png)
2. <span data-ttu-id="0769f-159">Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-159">You need to sign in with your email address to access the service, to start that process, type in your **email address** and tap **Continue**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/picture1.png)
3. <span data-ttu-id="0769f-161">Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="0769f-161">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="0769f-162">Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot.</span><span class="sxs-lookup"><span data-stu-id="0769f-162">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="0769f-163">Ha, válassza ki a megbízható, és koppintson a meghívókat **végzett**.</span><span class="sxs-lookup"><span data-stu-id="0769f-163">If you are, select the invitations you trust and tap **Done**.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/IOS3.png)
4. <span data-ttu-id="0769f-165">Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center.</span><span class="sxs-lookup"><span data-stu-id="0769f-165">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="0769f-166">Koppintson az alkalmazás elindításához és használatba egyikére.</span><span class="sxs-lookup"><span data-stu-id="0769f-166">Tap one of the apps to launch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/IOS4.png)
5. <span data-ttu-id="0769f-168">Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja.</span><span class="sxs-lookup"><span data-stu-id="0769f-168">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="0769f-169">Ehhez koppintson **ingyenes Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="0769f-169">To do so, tap **Go to free trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/IOS5.png)
6. <span data-ttu-id="0769f-171">Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0769f-171">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/IOS6.png)

## <a name="mac-os-x"></a><span data-ttu-id="0769f-173">Mac OS X</span><span class="sxs-lookup"><span data-stu-id="0769f-173">Mac OS X</span></span>
<span data-ttu-id="0769f-174">Miután telepítette a Microsoft távoli asztal alkalmazás az App store-ból, megtalálhatja az alkalmazáslistában alatt **Microsoft távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="0769f-174">Once you have installed the Microsoft Remote Desktop app from the App store, you can find it in your app list under **Microsoft Remote Desktop**.</span></span>

1. <span data-ttu-id="0769f-175">Elindítani az alkalmazást számos lehetőséget kínál, egy üres kapcsolat középre, kivéve, ha már használja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0769f-175">Launching the app brings you to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="0769f-176">Ismerkedés az Azure RemoteApp, kattintson a **Azure RemoteApp** gombra.</span><span class="sxs-lookup"><span data-stu-id="0769f-176">To get started with Azure RemoteApp, click the **Azure RemoteApp** button.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/Mac1.png)
2. <span data-ttu-id="0769f-178">Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez koppintson **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="0769f-178">You need to sign in with your email address to access the service, to start that process, tap **Get Started**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/Mac2.png)
3. <span data-ttu-id="0769f-180">A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-180">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="0769f-181">Ezzel megkezdődik a bejelentkezési folyamat során az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="0769f-181">This begins the sign in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/picture2.png)
4. <span data-ttu-id="0769f-183">Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="0769f-183">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="0769f-184">Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot.</span><span class="sxs-lookup"><span data-stu-id="0769f-184">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="0769f-185">Ha, válassza ki a meghívót, megbízható, és zárja be a párbeszédpanelt.</span><span class="sxs-lookup"><span data-stu-id="0769f-185">If you are, select the invitations you trust and close the dialog.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/Mac4.png)
5. <span data-ttu-id="0769f-187">Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center.</span><span class="sxs-lookup"><span data-stu-id="0769f-187">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="0769f-188">Kattintson duplán az alkalmazás elindításához és használatba egyikét.</span><span class="sxs-lookup"><span data-stu-id="0769f-188">Double-click one of the apps to launch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/Mac5.png)
6. <span data-ttu-id="0769f-190">Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja.</span><span class="sxs-lookup"><span data-stu-id="0769f-190">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="0769f-191">Ehhez kattintson **ingyenes Ugrás** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="0769f-191">To do so, click **Go to free trial** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/Mac6.png)
7. <span data-ttu-id="0769f-193">Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0769f-193">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Mac7.png)

## <a name="windows-all-supported-versions-except-windows-phone"></a><span data-ttu-id="0769f-195">Windows (Windows Phone kivételével az összes támogatott verzió)</span><span class="sxs-lookup"><span data-stu-id="0769f-195">Windows (All supported versions except Windows Phone)</span></span>
<span data-ttu-id="0769f-196">Az ügyfél automatikusan elindul a telepítés, azonban ha való hozzáférésre van szüksége később azt található neve alatt az alkalmazáslistában művelet befejeződése után **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="0769f-196">The client launches automatically after it finishes installing, however when you need to access it again later it can be found in your app list under the name **Azure RemoteApp**.</span></span>

1. <span data-ttu-id="0769f-197">Az ügyfél, az első képernyőn látható elindításához ésőbb Üdvözli az Azure Remoteapphoz.</span><span class="sxs-lookup"><span data-stu-id="0769f-197">Ater launching the client, the first page you see welcomes you to Azure RemoteApp.</span></span> <span data-ttu-id="0769f-198">A folytatáshoz kattintson a **Ismerkedés**.</span><span class="sxs-lookup"><span data-stu-id="0769f-198">To proceed, click on **Get Started**.</span></span>
   
    ![Az Azure RemoteApp-ügyfélen kezdőlapja](./media/remoteapp-clients/Windows1.png)
2. <span data-ttu-id="0769f-200">A következő oldalon indítja el a bejelentkezési folyamat az Azure RemoteApp az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="0769f-200">The next page starts the sign in process for Azure RemoteApp using Azure Active Directory.</span></span> <span data-ttu-id="0769f-201">Ez a folyamat ismerős, ha van Microsoft-szolgáltatásokkal a múltban kell kinéznie.</span><span class="sxs-lookup"><span data-stu-id="0769f-201">This process should look familiar if you have used Microsoft services in the past.</span></span> <span data-ttu-id="0769f-202">Beírásával indítsa el a **e-mail cím** kattintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-202">Start by typing your **email address** and click **Continue**.</span></span>
   
    ![Első Azure Active Directory-kérés](./media/remoteapp-clients/Windows2.png)
3. <span data-ttu-id="0769f-204">Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="0769f-204">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="0769f-205">Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot.</span><span class="sxs-lookup"><span data-stu-id="0769f-205">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="0769f-206">Ha, válassza ki a megbízható, és kattintson a meghívókat **végzett**.</span><span class="sxs-lookup"><span data-stu-id="0769f-206">If you are, select the invitations you trust and click **Done**.</span></span>
   
    ![Az Azure RemoteApp-ügyfélen meghívókat lapja](./media/remoteapp-clients/Windows3.png)
4. <span data-ttu-id="0769f-208">Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center.</span><span class="sxs-lookup"><span data-stu-id="0769f-208">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="0769f-209">Kattintson duplán az alkalmazás elindításához és használatba egyikét.</span><span class="sxs-lookup"><span data-stu-id="0769f-209">Double-click one of the apps to launch it and start using it.</span></span>
   
    ![Az Azure RemoteApp ügyfél csatlakozási központ](./media/remoteapp-clients/Windows4.png)
5. <span data-ttu-id="0769f-211">Ha nem küldött meghívót még, ne aggódjon van, a kezelt!</span><span class="sxs-lookup"><span data-stu-id="0769f-211">If no one has sent you an invitation yet, don't worry we've got you covered!</span></span> <span data-ttu-id="0769f-212">Ön továbbra is elérheti a bemutató gyűjteményhez, amellyel tesztelheti, a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="0769f-212">You'll still have access to a demo collection so you can test out the service.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/Windows5.png)

## <a name="windows-phone-81"></a><span data-ttu-id="0769f-214">Windows Phone 8.1</span><span class="sxs-lookup"><span data-stu-id="0769f-214">Windows Phone 8.1</span></span>
<span data-ttu-id="0769f-215">Miután telepítette a Microsoft távoli asztal alkalmazással a Windows Phone 8.1-tárolóból, megtalálhatja az alkalmazáslistában alatt **távoli asztal**.</span><span class="sxs-lookup"><span data-stu-id="0769f-215">Once you have installed the Microsoft Remote Desktop app from the Windows Phone 8.1 store, you can find it in your app list under **Remote Desktop**.</span></span>

1. <span data-ttu-id="0769f-216">Elindítani az alkalmazást biztosítható a az Ön közvetlenül egy üres csatlakozási központ, kivéve, ha már használja az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0769f-216">Launching the app brings you directly to an empty Connection Center, unless you've already been using the app.</span></span> <span data-ttu-id="0769f-217">Ismerkedés az Azure RemoteApp, koppintson a Hozzáadás gombra **"" +""** a képernyő alján.</span><span class="sxs-lookup"><span data-stu-id="0769f-217">To get started with Azure RemoteApp, tap the add button **""+""** at the bottom of the screen.</span></span>
   
    ![Üres csatlakozási központ](./media/remoteapp-clients/WinPhone1.png)
2. <span data-ttu-id="0769f-219">A következő koppintson **Azure RemoteApp**.</span><span class="sxs-lookup"><span data-stu-id="0769f-219">Next, tap on **Azure RemoteApp**.</span></span>
   
    ![Konfigurációelem-weblap hozzáadása](./media/remoteapp-clients/WinPhone2.png)
3. <span data-ttu-id="0769f-221">Meg kell bejelentkezni az e-mail címét, a folyamat elindításához a szolgáltatás eléréséhez koppintson **csatlakozás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-221">You need to sign in with your email address to access the service, to start that process, tap **connect**.</span></span>
   
    ![Jelentkezzen be a parancssorba](./media/remoteapp-clients/WinPhone3.png)
4. <span data-ttu-id="0769f-223">A következő oldalon írja be a **e-mail cím** koppintson **Folytatás**.</span><span class="sxs-lookup"><span data-stu-id="0769f-223">On the next page, type in your **email address** and tap **Continue**.</span></span> <span data-ttu-id="0769f-224">Ezzel megkezdődik a bejelentkezési folyamat során az Azure Active Directoryval.</span><span class="sxs-lookup"><span data-stu-id="0769f-224">This begins the sign in process using Azure Active Directory.</span></span>
   
    ![Első Azure Active Directory lap](./media/remoteapp-clients/WinPhone4.png)
5. <span data-ttu-id="0769f-226">Kövesse a képernyőn a Microsoft-fiók (Live ID) vagy a szervezeti azonosítójával. Jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="0769f-226">Follow the instructions on the screen to sign in with your Microsoft account (LiveID) or Organization ID.</span></span> <span data-ttu-id="0769f-227">Miután bejelentkezett, típusától kapott minden meghívókat felsoroló lapot.</span><span class="sxs-lookup"><span data-stu-id="0769f-227">Once signed in, you may be presented with a page listing all the invitations you have received.</span></span> <span data-ttu-id="0769f-228">Ha, válassza ki a megbízható, és koppintson a meghívókat **mentése**.</span><span class="sxs-lookup"><span data-stu-id="0769f-228">If you are, select the invitations you trust and tap **save**.</span></span>
   
    ![Meghívókat lap](./media/remoteapp-clients/WinPhone5.png)
6. <span data-ttu-id="0769f-230">Miután elfogadja a meghívót, Ön hozzáférhet az alkalmazások listáját az eszközre letöltődik és szeretné elérhetővé tenni a kapcsolódási Center.</span><span class="sxs-lookup"><span data-stu-id="0769f-230">After accepting your invitations, the list of apps you have access to will be downloaded to your device and made available in the Connection Center.</span></span> <span data-ttu-id="0769f-231">Koppintson az alkalmazás elindításához és használatba egyikére.</span><span class="sxs-lookup"><span data-stu-id="0769f-231">Tap one of the apps to launch it and start using it.</span></span>
   
    ![A hírcsatorna csatlakozási központ](./media/remoteapp-clients/WinPhone6.png)
7. <span data-ttu-id="0769f-233">Ha nincs meghívót, de a szolgáltatás továbbra is kipróbálhatja.</span><span class="sxs-lookup"><span data-stu-id="0769f-233">If you do not have an invitation yet, you can still try out the service.</span></span> <span data-ttu-id="0769f-234">Ehhez koppintson **Igen** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="0769f-234">To do so, tap **yes** when prompted.</span></span>
   
    ![Kérdezzen rá hírcsatorna bemutató](./media/remoteapp-clients/WinPhone7.png)
8. <span data-ttu-id="0769f-236">Ez fogja hozzáférést biztosít egy alapvető házirendcsoport első RemoteApp-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="0769f-236">This will give you access to a basic set of apps to get you started with RemoteApp.</span></span>
   
    ![Azure RemoteApp-hírcsatorna bemutató](./media/remoteapp-clients/WinPhone8.png)

