---
title: "aaaGet használatába a Mobile Apps Xamarin.Forms használatával"
description: "Hajtsa végre az ezen oktatóanyag toostart használhatja a Mobile Apps Xamarin.Forms-fejlesztési"
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 5e692220-cc89-4548-96c8-35259722acf5
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/01/2016
ms.author: glenga
ms.openlocfilehash: af6b1c1ce4cf91c397552aa3d8ee40728129238c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-xamarinforms-app"></a><span data-ttu-id="0b7d2-103">Xamarin.Forms-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-103">Create a Xamarin.Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started](../../includes/app-service-mobile-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="0b7d2-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="0b7d2-104">Overview</span></span>
<span data-ttu-id="0b7d2-105">Az oktatóanyag bemutatja, hogyan tooadd egy felhőalapú háttér-szolgáltatás tooa Xamarin.Forms mobilalkalmazás használatával hello hello háttér, Azure App Service Mobile Apps szolgáltatásának.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-105">This tutorial shows you how tooadd a cloud-based back-end service tooa Xamarin.Forms mobile app by using hello Mobile Apps feature of Azure App Service as hello back end.</span></span> <span data-ttu-id="0b7d2-106">Létrehoz egy új Mobile Apps-háttéralkalmazást, illetve egy olyan egyszerű Xamarin.Forms-alapú teendőlista alkalmazást, amely az alkalmazásadatait az Azure-ban tárolja.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-106">You create both a new Mobile Apps back end and a simple to-do list Xamarin.Forms app that stores app data in Azure.</span></span>

<span data-ttu-id="0b7d2-107">Az oktatóanyag végrehajtása feltétele a Mobile Apps Xamarin.Forms-alkalmazásokra vonatkozó összes többi oktatóanyagának elérésének.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-107">Completing this tutorial is a prerequisite for all other Mobile Apps tutorials for Xamarin.Forms.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0b7d2-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="0b7d2-108">Prerequisites</span></span>
<span data-ttu-id="0b7d2-109">toocomplete ebben az oktatóanyagban a következő hello szüksége:</span><span class="sxs-lookup"><span data-stu-id="0b7d2-109">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="0b7d2-110">Aktív Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-110">An active Azure account.</span></span> <span data-ttu-id="0b7d2-111">Ha nincs fiókja, regisztráljon az Azure-próbaverzióra, és létrehozásához too10 szabad mobilalkalmazások a próbaidőszak után is tovább használhat.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-111">If you don't have an account, you can sign up for an Azure trial and get up too10 free mobile apps that you can keep using even after your trial ends.</span></span> <span data-ttu-id="0b7d2-112">További információk: [Ingyenes Azure-próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-112">For more information, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

* <span data-ttu-id="0b7d2-113">Visual Studio és Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-113">Visual Studio with Xamarin.</span></span> <span data-ttu-id="0b7d2-114">Információ: hello [állítsa össze, és telepítse a Visual Studio és Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) lap.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-114">For information, see hello [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) page.</span></span>

* <span data-ttu-id="0b7d2-115">Mac számítógép 7.0 vagy-s újabb verziójú Xcode-dal és Xamarin Studio Communityvel.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-115">A Mac with Xcode v7.0 or later and Xamarin Studio Community installed.</span></span> <span data-ttu-id="0b7d2-116">További információk: [A Visual Studio és a Xamarin beállítása és telepítése](https://msdn.microsoft.com/library/mt613162.aspx) és [Beállítás, telepítés és ellenőrzés Mac felhasználók részére](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-116">For information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx) and [Set up, install, and verify for Mac users](https://msdn.microsoft.com/library/mt488770.aspx) (MSDN).</span></span>

## <a name="create-a-new-mobile-apps-back-end"></a><span data-ttu-id="0b7d2-117">Új Mobile Apps-háttéralkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-117">Create a new Mobile Apps back end</span></span>

<span data-ttu-id="0b7d2-118">vissza egy új Mobile Apps leállításához toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="0b7d2-118">toocreate a new Mobile Apps back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service](../../includes/app-service-mobile-dotnet-backend-create-new-service.md)]

<span data-ttu-id="0b7d2-119">Ezennel beállított egy Mobile Apps-háttéralkalmazást, amelyet a mobilos ügyfélalkalmazásai használhatnak.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-119">You have now set up a Mobile Apps back end that your mobile client applications can use.</span></span> <span data-ttu-id="0b7d2-120">Ezt követően töltse le egy kiszolgálóprojektet egy egyszerű Tennivalólista lista háttér és tooAzure közzététele.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-120">Next, you download a server project for a simple to-do list back end and then publish it tooAzure.</span></span>

## <a name="configure-hello-server-project"></a><span data-ttu-id="0b7d2-121">Hello server projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-121">Configure hello server project</span></span>

<span data-ttu-id="0b7d2-122">tooconfigure hello server projekt toouse hello Node.js vagy .NET háttér, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="0b7d2-122">tooconfigure hello server project toouse either hello Node.js or .NET back end, do hello following:</span></span>

[!INCLUDE [app-service-mobile-configure-new-backend](../../includes/app-service-mobile-configure-new-backend.md)]

## <a name="download-and-run-hello-xamarinforms-solution"></a><span data-ttu-id="0b7d2-123">Hello Xamarin.Forms-megoldás letöltése és futtatása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-123">Download and run hello Xamarin.Forms solution</span></span>

<span data-ttu-id="0b7d2-124">Hello megoldás két módon töltheti le.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-124">You can download hello solution in either of two ways.</span></span> <span data-ttu-id="0b7d2-125">Tooa Mac töltse le és nyissa meg a Xamarin Studio vagy tooa Windows rendszerű számítógépen töltse le és nyissa meg a Visual Studio egy hálózati Mac használatával történő megtervezésével hello iOS-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-125">Download it tooa Mac and open it in Xamarin Studio, or download it tooa Windows computer and open it in Visual Studio by using a networked Mac for building hello iOS app.</span></span> <span data-ttu-id="0b7d2-126">További információk: [A Visual Studio és a Xamarin beállítása és telepítése](https://msdn.microsoft.com/library/mt613162.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-126">For more information, see [Set up and install Visual Studio and Xamarin](https://msdn.microsoft.com/library/mt613162.aspx).</span></span>

<span data-ttu-id="0b7d2-127">Az hello a Mac vagy Windows-számítógépen a következő:</span><span class="sxs-lookup"><span data-stu-id="0b7d2-127">On a Mac or Windows computer, do hello following:</span></span>

1. <span data-ttu-id="0b7d2-128">Nyissa meg toohello [Azure-portálon].</span><span class="sxs-lookup"><span data-stu-id="0b7d2-128">Go toohello [Azure portal].</span></span>

2. <span data-ttu-id="0b7d2-129">A hello **beállítások** mobil alkalmazás, a **Mobile**, jelölje be **Ismerkedés** > **Xamarin.Forms**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-129">On hello **Settings** blade for your mobile app, under **Mobile**, select **Get Started** > **Xamarin.Forms**.</span></span> <span data-ttu-id="0b7d2-130">A **3. lépésben** válassza az **Új alkalmazás létrehozása**, majd a **Letöltés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-130">Under **step 3**, select  **Create a new app**, and then select **Download**.</span></span>

   <span data-ttu-id="0b7d2-131">Ez a művelet egy projekt, amely tartalmaz egy ügyfélalkalmazást, amely csatlakoztatott tooyour mobilalkalmazás tölti le.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-131">This action downloads a project that contains a client application that's connected tooyour mobile app.</span></span> <span data-ttu-id="0b7d2-132">Mentse a hello tömörített projekt fájl tooyour helyi számítógépen, és jegyezze fel a mentési helyét.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-132">Save hello compressed project file tooyour local computer, and make a note of where you save it.</span></span>

3. <span data-ttu-id="0b7d2-133">Bontsa ki a letöltött hello projektet, és nyissa meg a Xamarin Studio (Mac) vagy a Visual Studio (Windows).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-133">Extract hello project that you downloaded, and then open it in Xamarin Studio (Mac) or Visual Studio (Windows).</span></span>

   ![Kibontott projekt a Xamarin Studióban][9]

   ![Kibontott projekt a Visual Studióban][8]

## <a name="optional-run-hello-ios-project"></a><span data-ttu-id="0b7d2-136">(Választható) Hello iOS-projekt futtatása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-136">(Optional) Run hello iOS project</span></span>
<span data-ttu-id="0b7d2-137">Ebben a szakaszban hello Xamarin iOS-projektet az iOS-eszközök futtatása.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-137">In this section, you run hello Xamarin iOS project for iOS devices.</span></span> <span data-ttu-id="0b7d2-138">Kihagyhatja ezt a részt, ha nem dolgozik iOS-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-138">You can skip this section if you are not working with iOS devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="0b7d2-139">Xamarin Studióban</span><span class="sxs-lookup"><span data-stu-id="0b7d2-139">In Xamarin Studio</span></span>
1. <span data-ttu-id="0b7d2-140">Kattintson a jobb gombbal a hello iOS-projektre, majd válassza ki **beállítása szerint Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-140">Right-click hello iOS project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="0b7d2-141">A hello **futtatása** menüjében válassza **Start Debugging** toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-141">On hello **Run** menu, select **Start Debugging** toobuild hello project and start hello app in hello iPhone emulator.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0b7d2-142">Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="0b7d2-142">In Visual Studio</span></span>
1. <span data-ttu-id="0b7d2-143">Kattintson a jobb gombbal a hello iOS-projektre, majd válassza ki **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-143">Right-click hello iOS project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0b7d2-144">A hello **Build** menü **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-144">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0b7d2-145">A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello iOS-projektet.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-145">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello iOS project.</span></span>

4. <span data-ttu-id="0b7d2-146">toobuild hello projektet, és indítsa el a hello alkalmazást hello iPhone-emulátorban, jelölje be hello **F5** kulcs.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-146">toobuild hello project and start hello app in hello iPhone emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0b7d2-147">Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-147">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="0b7d2-148">Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-148">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0b7d2-149">Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-149">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][10]

    <span data-ttu-id="0b7d2-150">Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-150">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0b7d2-151">Hello kérelemből adatok bekerülnek hello TodoItem tábla.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-151">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="0b7d2-152">Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-152">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>

    > [!NOTE]
    > <span data-ttu-id="0b7d2-153">Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-153">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-android-project"></a><span data-ttu-id="0b7d2-154">(Választható) Hello Android-projekt futtatása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-154">(Optional) Run hello Android project</span></span>
<span data-ttu-id="0b7d2-155">Ebben a szakaszban a hello Xamarin droid-projekt az Android futtatja.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-155">In this section, you run hello Xamarin droid project for Android.</span></span> <span data-ttu-id="0b7d2-156">Kihagyhatja ezt a részt, ha nem dolgozik Android-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-156">You can skip this section if you are not working with Android devices.</span></span>

#### <a name="in-xamarin-studio"></a><span data-ttu-id="0b7d2-157">Xamarin Studióban</span><span class="sxs-lookup"><span data-stu-id="0b7d2-157">In Xamarin Studio</span></span>

1. <span data-ttu-id="0b7d2-158">Kattintson a jobb gombbal a hello Android-projekt, majd válassza ki **beállítása szerint Startup Project**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-158">Right-click hello Android project, and then select **Set As Startup Project**.</span></span>

2. <span data-ttu-id="0b7d2-159">toobuild hello projektet, és indítsa el a hello alkalmazást egy Android-emulátorban, a hello **futtatása** menü **Start Debugging**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-159">toobuild hello project and start hello app in an Android emulator, on hello **Run** menu, select **Start Debugging**.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0b7d2-160">Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="0b7d2-160">In Visual Studio</span></span>

1. <span data-ttu-id="0b7d2-161">Kattintson a jobb gombbal a hello (Droid) Android-projektet, majd válassza ki **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-161">Right-click hello Android (Droid) project, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0b7d2-162">A hello **Build** menü **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-162">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0b7d2-163">A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello Androidos projekt.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-163">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Android project.</span></span>

4. <span data-ttu-id="0b7d2-164">toobuild hello projektet, és indítsa el a hello alkalmazást egy Android-emulátorban, jelölje be hello **F5** kulcs.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-164">toobuild hello project and start hello app in an Android emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0b7d2-165">Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-165">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="0b7d2-166">Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-166">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0b7d2-167">Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-167">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    ![][11]
    
    <span data-ttu-id="0b7d2-168">Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-168">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0b7d2-169">Hello kérelemből adatok bekerülnek hello TodoItem tábla.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-169">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="0b7d2-170">Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-170">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    > [!NOTE]
    > <span data-ttu-id="0b7d2-171">Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-171">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="optional-run-hello-windows-project"></a><span data-ttu-id="0b7d2-172">(Választható) Futtassa a Windows hello-projekt</span><span class="sxs-lookup"><span data-stu-id="0b7d2-172">(Optional) Run hello Windows project</span></span>

<span data-ttu-id="0b7d2-173">Ebben a szakaszban hello Xamarin WinApp-projektjét, amely a Windows-eszközök futtatása.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-173">In this section, you run hello Xamarin WinApp project for Windows devices.</span></span> <span data-ttu-id="0b7d2-174">Kihagyhatja ezt a részt, ha nem dolgozik Windows-eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-174">You can skip this section if you are not working with Windows devices.</span></span>

#### <a name="in-visual-studio"></a><span data-ttu-id="0b7d2-175">Visual Studióban</span><span class="sxs-lookup"><span data-stu-id="0b7d2-175">In Visual Studio</span></span>

1. <span data-ttu-id="0b7d2-176">Kattintson a jobb gombbal bármelyik Windows hello-projektek egyikére, majd válassza ki **beállítás kezdőprojektként**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-176">Right-click any of hello Windows projects, and then select **Set as StartUp Project**.</span></span>

2. <span data-ttu-id="0b7d2-177">A hello **Build** menü **Configuration Manager**.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-177">On hello **Build** menu, select **Configuration Manager**.</span></span>

3. <span data-ttu-id="0b7d2-178">A hello **Configuration Manager** párbeszédpanel megnyitásához, jelölje be hello **Build** és **telepítés** jelölőnégyzetek következő toohello Windows-projekt választott.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-178">In hello **Configuration Manager** dialog box, select hello **Build** and **Deploy** check boxes next toohello Windows project that you chose.</span></span>

4. <span data-ttu-id="0b7d2-179">toobuild hello projektet, és indítsa el a hello alkalmazást egy Windows-emulátorban, jelölje be hello **F5** kulcs.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-179">toobuild hello project and start hello app in a Windows emulator, select hello **F5** key.</span></span>

   > [!NOTE]
   > <span data-ttu-id="0b7d2-180">Ha problémába ütközik hello projekt, futtassa a hello NuGet package manager és a frissítés toohello legújabb verziójának hello Xamarin támogatási csomag.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-180">If you have problems building hello project, run hello NuGet package manager and update toohello latest version of hello Xamarin support packages.</span></span> <span data-ttu-id="0b7d2-181">Előfordulhat, hogy a gyorssablonra épülő projektek lassú tooupdate toohello legújabb verziói.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-181">Quickstart projects might be slow tooupdate toohello latest versions.</span></span>    
   >
   >

5. <span data-ttu-id="0b7d2-182">Hello alkalmazásban írjon be egy értelmes szöveget, például *további Xamarin*, és ezután válasszon hello plusz jelre (**+**).</span><span class="sxs-lookup"><span data-stu-id="0b7d2-182">In hello app, type meaningful text, such as *Learn Xamarin*, and then select hello plus sign (**+**).</span></span>

    <span data-ttu-id="0b7d2-183">Ez a művelet elküld egy új Mobile Apps háttér az Azure-ban üzemeltetett post kérelem toohello.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-183">This action sends a post request toohello new Mobile Apps back end that's hosted in Azure.</span></span> <span data-ttu-id="0b7d2-184">Hello kérelemből adatok bekerülnek hello TodoItem tábla.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-184">Data from hello request is inserted into hello TodoItem table.</span></span> <span data-ttu-id="0b7d2-185">Hello táblában tárolt elemeket adta vissza a Mobile Apps befejezése és hello hello adatok hello listája jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-185">Items that are stored in hello table are returned by hello Mobile Apps back end, and hello data is displayed in hello list.</span></span>
    
    ![][12]
    
    > [!NOTE]
    > <span data-ttu-id="0b7d2-186">Látni fogja a Mobile Apps háttér hello TodoItemManager.cs C# fájljában hello hordozhatóosztálytár-projektjének a megoldás hozzáférő hello kódot.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-186">You'll find hello code that accesses your Mobile Apps back end in hello TodoItemManager.cs C# file of hello portable class library project of your solution.</span></span>
    >
    >

## <a name="next-steps"></a><span data-ttu-id="0b7d2-187">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0b7d2-187">Next steps</span></span>

* [<span data-ttu-id="0b7d2-188">Hitelesítési tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-188">Add authentication tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-users.md)  
  <span data-ttu-id="0b7d2-189">Ismerje meg, hogy az alkalmazás egy identitásszolgáltatóval tooauthenticate felhasználóit.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-189">Learn how tooauthenticate users of your app with an identity provider.</span></span>

* [<span data-ttu-id="0b7d2-190">Leküldéses értesítések tooyour alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0b7d2-190">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)  
  <span data-ttu-id="0b7d2-191">Ismerje meg, hogyan támogatják a különböző tooadd leküldéses értesítések tooyour alkalmazást, és a Mobile Apps háttér toouse Azure Notification Hubs toosend hello leküldéses értesítések konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-191">Learn how tooadd push notifications support tooyour app and configure your Mobile Apps back end toouse Azure Notification Hubs toosend hello push notifications.</span></span>

* [<span data-ttu-id="0b7d2-192">Az offline szinkronizálás engedélyezése az alkalmazás számára</span><span class="sxs-lookup"><span data-stu-id="0b7d2-192">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  <span data-ttu-id="0b7d2-193">Megtudhatja, hogyan háttér tooadd offline támogatást az alkalmazásához egy Mobile Apps használatával.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-193">Learn how tooadd offline support for your app by using a Mobile Apps back end.</span></span> <span data-ttu-id="0b7d2-194">A kapcsolat nélküli szinkronizálás lehetővé teszi a mobilalkalmazás adatainak megtekintését, hozzáadását és módosítását akkor is, ha nincs hálózati kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-194">With offline sync, you can view, add, or modify your mobile app's data, even when there is no network connection.</span></span>

* [<span data-ttu-id="0b7d2-195">A Mobile Apps hello felügyelt ügyfelek használata</span><span class="sxs-lookup"><span data-stu-id="0b7d2-195">Use hello managed client for Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md)  
  <span data-ttu-id="0b7d2-196">Ismerje meg, hogyan toowork hello való kezelése a Xamarin-alkalmazás ügyfél-SDK.</span><span class="sxs-lookup"><span data-stu-id="0b7d2-196">Learn how toowork with hello managed client SDK in your Xamarin app.</span></span>

<!-- Anchors. -->
[Get started with Mobile Apps back ends]:#getting-started
[Create a new Mobile Apps back end]:#create-new-service
[Next steps]:#next-steps


<!-- Images. -->
[6]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart.png
[8]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-vs.png
[9]: ./media/app-service-mobile-xamarin-forms-get-started/xamarin-forms-quickstart-xs.png
[10]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-ios.png
[11]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-android.png
[12]: ./media/app-service-mobile-xamarin-forms-get-started/mobile-quickstart-startup-windows.png


<!-- URLs. -->
[Visual Studio Professional 2013]: https://go.microsoft.com/fwLink/p/?LinkID=257546
[Mobile app SDK]: http://go.microsoft.com/fwlink/?LinkId=257545
[Azure-portálon]: https://portal.azure.com/
