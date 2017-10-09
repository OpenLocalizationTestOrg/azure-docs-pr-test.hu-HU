---
title: "Oktatóanyag: DevOps a hello Azure portálon |} Microsoft Docs"
description: "Ismerje meg a hello Azure portál különböző DevOps munkafolyamatok hello."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a><span data-ttu-id="8110b-103">Oktatóanyag: DevOps a hello Azure portál</span><span class="sxs-lookup"><span data-stu-id="8110b-103">Tutorial: DevOps with hello Azure Portal</span></span>
<span data-ttu-id="8110b-104">hello Azure platformon rugalmas DevOps munkafolyamatok megtelt.</span><span class="sxs-lookup"><span data-stu-id="8110b-104">hello Azure platform is full of flexible DevOps workflows.</span></span> <span data-ttu-id="8110b-105">Ebben az oktatóanyagban elsajátíthatja, hogyan tooleverage hello képességeit hello Azure Portal toodevelop, teszteléséhez, hibaelhárítása, figyelése, és kezelheti a futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8110b-105">In this tutorial, you learn how tooleverage hello capabilities of hello Azure Portal toodevelop, test, deploy, troubleshoot, monitor, and manage running applications.</span></span> <span data-ttu-id="8110b-106">Ez az oktatóanyag következő hello koncentrál:</span><span class="sxs-lookup"><span data-stu-id="8110b-106">This tutorial focuses on hello following:</span></span>

1. <span data-ttu-id="8110b-107">Webalkalmazás létrehozása és a folyamatos üzembe helyezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8110b-107">Creating a web app and enabling continuous deployment</span></span>
2. <span data-ttu-id="8110b-108">Alkalmazás fejlesztése és tesztelése</span><span class="sxs-lookup"><span data-stu-id="8110b-108">Develop and test an app</span></span>
3. <span data-ttu-id="8110b-109">Alkalmazás figyelése és hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="8110b-109">Monitoring and Troubleshooting an app</span></span>
4. <span data-ttu-id="8110b-110">Általános alkalmazásfelügyeleti feladatok</span><span class="sxs-lookup"><span data-stu-id="8110b-110">General application management tasks</span></span>

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a><span data-ttu-id="8110b-111">Webalkalmazás létrehozása és a folyamatos üzembe helyezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="8110b-111">Creating a web app and enabling continuous deployment</span></span>
<span data-ttu-id="8110b-112">A webalkalmazás létrehozása [Azure App Service](https://azure.microsoft.com/services/app-service/), amely ebben az oktatóanyagban hello többi fogja használni.</span><span class="sxs-lookup"><span data-stu-id="8110b-112">Create a Web app with [Azure App Service](https://azure.microsoft.com/services/app-service/), which you’ll use in hello rest of this tutorial.</span></span> <span data-ttu-id="8110b-113">Először engedélyeznie kell a folyamatos üzembe helyezést a forráskód tárházából a futó Azure-környezetbe.</span><span class="sxs-lookup"><span data-stu-id="8110b-113">You’ll initially enable continuous deployment from your source code repository into our running Azure environment.</span></span>

1. <span data-ttu-id="8110b-114">Jelentkezzen be a hello Azure portál</span><span class="sxs-lookup"><span data-stu-id="8110b-114">Sign into hello Azure Portal</span></span>
2. <span data-ttu-id="8110b-115">Válasszon **alkalmazásszolgáltatások** &gt; **Hozzáadás ikon** , és adjon meg egy nevet, és válassza ki az előfizetés, hozzon létre egy új erőforrás csoport tooserve hello tárolóként hello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8110b-115">Choose **App Services** &gt; **Add icon** and enter a name, choose your subscription, and create a new resource group tooserve as hello container for hello service.</span></span>
   
   <span data-ttu-id="8110b-116">Erőforráscsoportok lehetővé teszik toomanage különféle aspektusaihoz rendelkezésre álló hello megoldás például a számlázást, a központi telepítések és keresztül egyetlen csoportként összes figyelési [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8110b-116">Resource groups allow you toomanage various aspects of hello solution such as billing, deployments and monitoring all as a single group via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span></span>
   
   ![image1][image1]
3. <span data-ttu-id="8110b-118">Pár pillanat múlva létrejön az alkalmazásszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8110b-118">After a few moments, your app service is created.</span></span> <span data-ttu-id="8110b-119">Igénybe vehet néhány percet tooexplore hello hello szolgáltatás különböző menüpontok hello portálon.</span><span class="sxs-lookup"><span data-stu-id="8110b-119">Take a few minutes tooexplore hello various menu options for hello service in hello portal.</span></span>
   
   ![image2][image2]    
4. <span data-ttu-id="8110b-121">Kattintson a hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="8110b-121">Click hello URL.</span></span> <span data-ttu-id="8110b-122">Figyelje meg a rendelkezésre álló lehetőségek a eszközök és a tárolóhelyekkel hello különböző.</span><span class="sxs-lookup"><span data-stu-id="8110b-122">Notice hello variety of available choices for tools and repositories.</span></span> <span data-ttu-id="8110b-123">Hello nyelv és keretrendszer az Ön által választott, beleértve a .NET, Java és Ruby is használható.</span><span class="sxs-lookup"><span data-stu-id="8110b-123">You can also use hello languages and frameworks of your choice including .NET, Java, and Ruby.</span></span>
   
   ![image3][image3]    
5. <span data-ttu-id="8110b-125">hello Azure-portál lehetővé teszi a folyamatos üzembe helyezés az egyszerű folyamat, amely csak néhány egyszerű lépést foglal magában.</span><span class="sxs-lookup"><span data-stu-id="8110b-125">hello Azure portal makes continuous deployment an easy process that involves only a few simple steps.</span></span> <span data-ttu-id="8110b-126">Hello Azure-portálon, a hello ikon az imént létrehozott hello app service beállítások választhat.</span><span class="sxs-lookup"><span data-stu-id="8110b-126">In hello Azure portal, choose settings from hello icon for hello app service you just created.</span></span>
   
   ![image4][image4]
   
   <span data-ttu-id="8110b-128">Hello panelen jobb hello megnyíló görgessen toohello szakasz közzététele.</span><span class="sxs-lookup"><span data-stu-id="8110b-128">From hello blade that opens on hello right, scroll toohello publishing section.</span></span>
   
   ![image5][image5]
6. <span data-ttu-id="8110b-130">Ezután konfigurálja az egyes beállítások tooenable folyamatos üzembe helyezés hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8110b-130">Next, configure some settings tooenable continuous deployment for hello app.</span></span> <span data-ttu-id="8110b-131">Kattintson a Központi telepítés forrása elemre, és kattintson a Forrás kiválasztása elemre.</span><span class="sxs-lookup"><span data-stu-id="8110b-131">Click Deployment Source and then click Choose Source.</span></span> <span data-ttu-id="8110b-132">Figyelje meg hello számos tárház adatforrások beállításait.</span><span class="sxs-lookup"><span data-stu-id="8110b-132">Notice hello variety of options you have for repository sources.</span></span>
   
   ![image6][image6]
7. <span data-ttu-id="8110b-134">Ebben a példában válassza a Githubot.</span><span class="sxs-lookup"><span data-stu-id="8110b-134">For this example choose GitHub.</span></span> <span data-ttu-id="8110b-135">Opcionálisan válassza ki az Ön által választott hello tárház és hello engedélyezési hitelesítő adatok beállítása.</span><span class="sxs-lookup"><span data-stu-id="8110b-135">Optionally choose hello repository of your choice and setup hello authorization credentials.</span></span>
   
   ![image7][image7]
8. <span data-ttu-id="8110b-137">Engedélyezési tooyour tárház, után dönthet a projekt és a fiókiroda toodeploy kívánja.</span><span class="sxs-lookup"><span data-stu-id="8110b-137">After authorization tooyour repository, you can then choose a project and branch you wish toodeploy.</span></span> <span data-ttu-id="8110b-138">Alább több fiktív mintapéldát láthat.</span><span class="sxs-lookup"><span data-stu-id="8110b-138">There are several fictitious sample examples listed below.</span></span>
   
   ![image8][image8]
9. <span data-ttu-id="8110b-140">A projekt és az ág kiválasztása után kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="8110b-140">Once you choose your project and branch, click ok.</span></span> <span data-ttu-id="8110b-141">A központi telepítés toosee értesítések kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="8110b-141">You should start toosee notifications of a deployment.</span></span>
   
   ![image9][image9]
10. <span data-ttu-id="8110b-143">Lépjen vissza tooGitHub toosee hello webhook Azure toointegrate hello forrás vezérlő tárház hoztak létre.</span><span class="sxs-lookup"><span data-stu-id="8110b-143">Navigate back tooGitHub toosee hello webhook that was created toointegrate hello source control repo with Azure.</span></span> <span data-ttu-id="8110b-144">hello Azure portál lehetővé teszi az integrációt a Githubon csak néhány egyszerű lépésben.</span><span class="sxs-lookup"><span data-stu-id="8110b-144">hello Azure Portal enables integration with GitHub with only a few simple steps.</span></span>
    
    ![image10][image10]
11. <span data-ttu-id="8110b-146">toodemonstrate folyamatos üzembe helyezés, gyorsan hozzá néhány tartalom toohello tárházba.</span><span class="sxs-lookup"><span data-stu-id="8110b-146">toodemonstrate continuous deployment, you quickly add some content toohello repository.</span></span> <span data-ttu-id="8110b-147">Egy egyszerű példa adjon hozzá egy minta szöveges fájl tooa GitHub-tárház.</span><span class="sxs-lookup"><span data-stu-id="8110b-147">For a simple example, add a sample text file tooa GitHub repo.</span></span> <span data-ttu-id="8110b-148">Szabad toouse .NET, Ruby, Python vagy egyéb típusú alkalmazás az App Service áll.</span><span class="sxs-lookup"><span data-stu-id="8110b-148">You are free toouse .NET, Ruby, Python, or some other type of application with App Service.</span></span> <span data-ttu-id="8110b-149">Szabad tooadd egy szövegfájlt érzi, hogy az ASP.NET MVC, Java vagy Ruby alkalmazás toohello tárház az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="8110b-149">Feel free tooadd a text file, ASP.NET MVC, Java, or Ruby application toohello repo of your choice.</span></span>
    
    ![image11][image11]
12. <span data-ttu-id="8110b-151">Után tooyour tárház módosítások véglegesítése, megjelenik egy új központi telepítési hello portál értesítési területen lévő kezdeményezni.</span><span class="sxs-lookup"><span data-stu-id="8110b-151">After committing changes tooyour repository, you see a new deployment initiate in hello portal notifications area.</span></span> <span data-ttu-id="8110b-152">Ha gyorsan jelennek meg módosítások tooyour tárház véglegesítése után, kattintson a szinkronizálás.</span><span class="sxs-lookup"><span data-stu-id="8110b-152">Click Sync if you do not quickly see changes after committing tooyour repository.</span></span>
    
    ![image12][image12]
13. <span data-ttu-id="8110b-154">Ezen a ponton Ha próbálja és hello app Service hello lap betöltése, a 403-as hiba jelenhet.</span><span class="sxs-lookup"><span data-stu-id="8110b-154">At this point, if you try and load hello page for hello app service, you may receive a 403 error.</span></span> <span data-ttu-id="8110b-155">Ebben a példában ez azért, mert nincs alapértelmezett dokumentum beállítása például egy fájl például index.HTML vagy default.html hello lap.</span><span class="sxs-lookup"><span data-stu-id="8110b-155">In this example, it is because there is no typical default document setup for hello page such as a file like index.htm or default.html.</span></span> <span data-ttu-id="8110b-156">Gyorsan megoldhatja ezt a tooling eszköz az Azure portál hello hello.</span><span class="sxs-lookup"><span data-stu-id="8110b-156">You can quickly remedy this with hello tooling in hello Azure Portal.</span></span>  <span data-ttu-id="8110b-157">A hello Azure portálon válassza a beállítások &gt; alkalmazás beállításait.</span><span class="sxs-lookup"><span data-stu-id="8110b-157">In hello Azure Portal choose Settings &gt; Application Settings.</span></span>
    
     ![image13][image13]
14. <span data-ttu-id="8110b-159">Megjelenik egy panel az alkalmazásbeállításokkal.</span><span class="sxs-lookup"><span data-stu-id="8110b-159">A blade opens for application settings.</span></span> <span data-ttu-id="8110b-160">Hello lap "SamplePage.html" hello nevét adja meg, és kattintson a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="8110b-160">Enter hello name of hello page “SamplePage.html” and click Save.</span></span> <span data-ttu-id="8110b-161">Igénybe vehet néhány percet tooexplore hello egyéb beállításokat.</span><span class="sxs-lookup"><span data-stu-id="8110b-161">Take a few minutes tooexplore hello other settings.</span></span>
    
    ![image14][image14]
15. <span data-ttu-id="8110b-163">Opcionálisan frissítse a böngésző URL-cím tooensure várt hello módosításokat láthatják.</span><span class="sxs-lookup"><span data-stu-id="8110b-163">Optionally refresh your browser URL tooensure you see hello expected changes.</span></span> <span data-ttu-id="8110b-164">Ebben az esetben van néhány egyszerű szöveg most feltöltése hello lap.</span><span class="sxs-lookup"><span data-stu-id="8110b-164">In this case, there is some simple text now populating hello page.</span></span> <span data-ttu-id="8110b-165">Minden további módosítása toohello tárház egy új automatikus központi telepítési eredményezne.</span><span class="sxs-lookup"><span data-stu-id="8110b-165">Each additional change toohello repository would result in a new automatic deployment.</span></span>
    
    ![image15][image15]
    
    <span data-ttu-id="8110b-167">Folyamatos üzembe helyezés az Azure portál hello engedélyezése egy egyszerű élményt.</span><span class="sxs-lookup"><span data-stu-id="8110b-167">Enabling continuous deployment with hello Azure Portal is an easy experience.</span></span> <span data-ttu-id="8110b-168">Összetettebb kiadás folyamatok létrehozhatja és sok más módszerek használata meglévő verziókezelő és folyamatos integrációt rendszerek toodeploy tooAzure, például az automatikus build és kiadás felügyeleti rendszerekkel is.</span><span class="sxs-lookup"><span data-stu-id="8110b-168">You can also build more complex release pipelines and use many other techniques with existing source control and continuous integration systems toodeploy tooAzure, such as leveraging automated build and release management systems.</span></span>

## <a name="develop-and-test-an-app"></a><span data-ttu-id="8110b-169">Alkalmazás fejlesztése és tesztelése</span><span class="sxs-lookup"><span data-stu-id="8110b-169">Develop and test an app</span></span>
<span data-ttu-id="8110b-170">A következő néhány módosítást toohello kód alap, és gyorsan telepítheti ezeket a módosításokat.</span><span class="sxs-lookup"><span data-stu-id="8110b-170">Next, make some changes toohello code base and rapidly deploy those changes.</span></span> <span data-ttu-id="8110b-171">Rendszer is telepítenie néhány teljesítménytesztelési hello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8110b-171">You will also setup up some performance testing for hello Web app.</span></span>

1. <span data-ttu-id="8110b-172">Az Azure portál hello alkalmazásszolgáltatások hello navigációs ablakból válassza, és keresse meg az App Service.</span><span class="sxs-lookup"><span data-stu-id="8110b-172">In hello Azure Portal choose App Services from hello navigation pane, and locate your App Service.</span></span>
   
   ![image16][image16]
2. <span data-ttu-id="8110b-174">Kattintson az Eszközök elemre.</span><span class="sxs-lookup"><span data-stu-id="8110b-174">Click Tools</span></span>
   
   ![image17][image17]
3. <span data-ttu-id="8110b-176">Figyelje meg a fejlesztés alatt eszközök kategória hello.</span><span class="sxs-lookup"><span data-stu-id="8110b-176">Notice hello develop category under Tools.</span></span> <span data-ttu-id="8110b-177">Nincsenek számos hasznos eszközök itt, amelyek lehetővé teszik a számunkra alkalmazásokkal toowork hello Azure Portal maradjanak.</span><span class="sxs-lookup"><span data-stu-id="8110b-177">There are several useful tools here that allow us toowork with apps without leaving hello Azure Portal.</span></span> <span data-ttu-id="8110b-178">Kattintson a Konzol elemre.</span><span class="sxs-lookup"><span data-stu-id="8110b-178">Click on Console.</span></span>
   
   ![image18][image18]
4. <span data-ttu-id="8110b-180">Hello konzolablakban élő parancsok adhat ki az alkalmazásra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="8110b-180">In hello console window, you can issue live commands for your app.</span></span> <span data-ttu-id="8110b-181">Típus hello dir parancs, és nyomja le a következő adatokat adja meg.</span><span class="sxs-lookup"><span data-stu-id="8110b-181">Type hello dir command and hit enter.</span></span> <span data-ttu-id="8110b-182">Megjegyzendő, hogy az emelt szintű jogosultságokat igénylő parancsok nem működnek.</span><span class="sxs-lookup"><span data-stu-id="8110b-182">Note that commands requiring elevated privileges do not work.</span></span>
   
   ![image19][image19]
5. <span data-ttu-id="8110b-184">Helyezze vissza toohello Develop kategória, és válassza ki a Visual Studio online-hoz.</span><span class="sxs-lookup"><span data-stu-id="8110b-184">Move back toohello Develop category and choose Visual Studio Online.</span></span> <span data-ttu-id="8110b-185">Megjegyzés: A Visual Studio Online új neve Visual Studio Team Services.</span><span class="sxs-lookup"><span data-stu-id="8110b-185">Note: Visual Studio Online is now named Visual Studio Team Services.</span></span>
   
   ![image20][image20]
6. <span data-ttu-id="8110b-187">Váltás a hello böngészőn belüli szerkesztési funkciót alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="8110b-187">Toggle on hello in-browser editing experience for your App.</span></span>
   
   ![image21][image21]
7. <span data-ttu-id="8110b-189">Egy webes bővítmény települ az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8110b-189">A web extension installs for your app.</span></span> <span data-ttu-id="8110b-190">Bővítmények gyorsan és könnyen egészíthessék funkció tooapps az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8110b-190">Extensions quickly and easily add functionality tooapps in Azure.</span></span> <span data-ttu-id="8110b-191">Figyelje meg, néhány hello más érhető el az alábbi képernyőképen hello bővítménytípusoknak.</span><span class="sxs-lookup"><span data-stu-id="8110b-191">Notice some of hello other extension types available in hello screenshot below.</span></span>
   
   ![image22][image22]
8. <span data-ttu-id="8110b-193">Miután telepíti a Visual Studio Online-bővítmény hello, Ugrás gombra.</span><span class="sxs-lookup"><span data-stu-id="8110b-193">Once hello Visual Studio Online extension installs, click Go.</span></span>
   
   ![image23][image23]
9. <span data-ttu-id="8110b-195">Egy böngészőben lapon válaszoknál láthatja a fejlesztési IDE közvetlenül hello böngészőben megnyílik.</span><span class="sxs-lookup"><span data-stu-id="8110b-195">A browser tab opens where you see a development IDE directly in hello browser.</span></span> <span data-ttu-id="8110b-196">Értesítés hello az alábbi szolgáltatás a Chrome-ban.</span><span class="sxs-lookup"><span data-stu-id="8110b-196">Notice hello experience below is in Chrome.</span></span>
   
   ![image24][image24]
10. <span data-ttu-id="8110b-198">Hajtsa végre több tevékenységeket, például a Szerkesztés fájlok, fájlok és mappák hozzáadása, és a tartalom letöltése a hello élő webhelyet.</span><span class="sxs-lookup"><span data-stu-id="8110b-198">You can perform several activities such as edit files, add files and folders, and download content from hello live site.</span></span> <span data-ttu-id="8110b-199">Készítsen egy gyors Szerkesztés toohello SamplePage.html fájlt.</span><span class="sxs-lookup"><span data-stu-id="8110b-199">Make a quick edit toohello SamplePage.html file.</span></span>
    
    ![image25][image25]
11. <span data-ttu-id="8110b-201">Néhány perc múlva hello módosítások automatikusan megtörténik.</span><span class="sxs-lookup"><span data-stu-id="8110b-201">In a few moments, hello changes are automatically saved.</span></span> <span data-ttu-id="8110b-202">Lépjen vissza toohello lapon, ha hello módosítások tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="8110b-202">If you navigate back toohello page, you can see hello changes.</span></span> <span data-ttu-id="8110b-203">Ne feledje, hogy az ehhez hasonló élő szerkesztések minden valószínűség szerint nem alkalmazhatók az éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="8110b-203">Keep in mind live edits like these are most likely not suitable for production environments.</span></span> <span data-ttu-id="8110b-204">Azonban hello eszközök gyors módosításokat végezhet, nagyon könnyen toomake a fejlesztési és tesztelési környezetben.</span><span class="sxs-lookup"><span data-stu-id="8110b-204">However, hello tools make it very easy toomake quick changes for dev and test environments.</span></span>
    
    ![image26][image26]
    
    ![image27][image27]
12. <span data-ttu-id="8110b-207">Helyezze vissza toohello Eszközök panelen, majd a hello Develop kategória, kattintson a vizsgálat.</span><span class="sxs-lookup"><span data-stu-id="8110b-207">Move back toohello tools blade and under hello Develop category, click on Performance Test.</span></span>
    
    ![image28][image28]
13. <span data-ttu-id="8110b-209">Tooset team services-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="8110b-209">You need tooset a team services account.</span></span> <span data-ttu-id="8110b-210">További információt itt talál: [Team Services-fiók létrehozása](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).</span><span class="sxs-lookup"><span data-stu-id="8110b-210">See here for more details: [Create a Team Services Account](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)</span></span>
14. <span data-ttu-id="8110b-211">Kattintson az új toocreate teljesítményének teszteléséhez.</span><span class="sxs-lookup"><span data-stu-id="8110b-211">Click on New toocreate a performance test.</span></span>
    
    ![image29][image29]
    
    <span data-ttu-id="8110b-213">Konfigurálása hello különböző értékeket, majd kattintson a hello aljához hello párbeszéd tooinitiate teljesítményének teszteléséhez futtassa tesztelése.</span><span class="sxs-lookup"><span data-stu-id="8110b-213">Configure hello various values and click Run Test at hello bottom of hello dialogue tooinitiate a performance test.</span></span>
    
    ![image30][image30]
    
    ![image31][image31]
15. <span data-ttu-id="8110b-216">Hello teszt futásának indításakor, miután hello állapot figyelheti meg.</span><span class="sxs-lookup"><span data-stu-id="8110b-216">Once hello test starts running, you can monitor hello state.</span></span>
    
    ![image32][image32]
    
    <span data-ttu-id="8110b-218">Hello teszt befejezése után kattintson a hello eredmény jeleníti meg a további részleteket.</span><span class="sxs-lookup"><span data-stu-id="8110b-218">Once hello test finishes, clicking on hello result shows more details.</span></span>
    
    ![image33][image33]
16. <span data-ttu-id="8110b-220">Ebben a példában létrehozott egy kis teszt futtatásakor, így korlátozott az tooanalyze, de akkor is tekintse meg a különböző metrikák, valamint futtassa újra a teszt ebben a nézetben.</span><span class="sxs-lookup"><span data-stu-id="8110b-220">In this example, you created a small test run, so there is limited data tooanalyze, but you can see various metrics as well as rerun your test from this view.</span></span> <span data-ttu-id="8110b-221">hello Azure Portal létrehozása, végrehajtása és elemzésével webes teljesítménytesztek az egyszerű folyamat teszi.</span><span class="sxs-lookup"><span data-stu-id="8110b-221">hello Azure Portal makes creating, executing, and analyzing web performance tests an easy process.</span></span> <span data-ttu-id="8110b-222">az alábbi képernyőképek hello hello teljesítményadatait jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8110b-222">hello screenshots below display hello performance data.</span></span>
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a><span data-ttu-id="8110b-226">Alkalmazás figyelése és hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="8110b-226">Monitoring and troubleshooting an app</span></span>
<span data-ttu-id="8110b-227">Az Azure számos funkciót kínál a futó alkalmazások figyeléséhez és hibaelhárításához.</span><span class="sxs-lookup"><span data-stu-id="8110b-227">Azure provides many capabilities for monitoring and troubleshooting running applications.</span></span>

1. <span data-ttu-id="8110b-228">Válassza ki a webalkalmazás Azure Portal hello eszközök.</span><span class="sxs-lookup"><span data-stu-id="8110b-228">In hello Azure Portal for our Web app choose Tools.</span></span>
   
   ![image37][image37]
2. <span data-ttu-id="8110b-230">A hello kapcsolatos problémák elhárítása kategória futó alkalmazáshoz eszközök tootroubleshoot lehetséges problémák használatára vonatkozó különböző döntések hello figyelje.</span><span class="sxs-lookup"><span data-stu-id="8110b-230">Under hello Troubleshoot category, notice hello various choices for using tools tootroubleshoot potential issues with a running app.</span></span> <span data-ttu-id="8110b-231">Lehetőség van például az élő HTTP-forgalom figyelésére, az önjavítás engedélyezésére, a naplók megtekintésére stb.</span><span class="sxs-lookup"><span data-stu-id="8110b-231">You can do things like monitor Live HTTP traffic, enable self-healing, view logs, and more.</span></span>
   
   ![image38][image38]
3. <span data-ttu-id="8110b-233">Válassza a Webhelymetrikák tooquickly get egy nézet egy HTTP-kódját.</span><span class="sxs-lookup"><span data-stu-id="8110b-233">Choose Site Metrics tooquickly get a view of some HTTP codes.</span></span>
   
   ![image39][image39]
4. <span data-ttu-id="8110b-235">Válassza a Diagnosztika lehetőséget szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="8110b-235">Choose Diagnostics as a Service.</span></span> <span data-ttu-id="8110b-236">Válassza ki az alkalmazás típusát, majd kattintson a Futtatás gombra.</span><span class="sxs-lookup"><span data-stu-id="8110b-236">Choose your application type, then choose Run.</span></span>
   
   ![image40][image40]
   
   <span data-ttu-id="8110b-238">Elkezdődik az adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="8110b-238">A collection begins.</span></span>
   
   ![image41][image41]
5. <span data-ttu-id="8110b-240">Úgy is dönthet, hello megfelelő napló toodiagnose lehetséges problémák kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="8110b-240">You may choose hello appropriate log toodiagnose potential issues.</span></span> <span data-ttu-id="8110b-241">Meg kell tooenable naplózási toosee hello rendelkezésre álló adatok például a HTTP-naplók beállításai közül.</span><span class="sxs-lookup"><span data-stu-id="8110b-241">You need tooenable logging toosee all of hello available data options such as HTTP Logs.</span></span>
   
   ![image42][image42]
   
   <span data-ttu-id="8110b-243">Hello memóriakép fájl letöltése és elemezheti a debugdiag lehetőséget kattintva elemző jelentés toohelp található lehetséges problémák kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="8110b-243">By clicking on hello Memory Dump file you can download and analyze a DebugDiag analysis report toohelp find potential issues.</span></span>
   
   ![image43][image43]
6. <span data-ttu-id="8110b-245">tooview több adatot tooenable további naplózás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="8110b-245">tooview more data, you need tooenable additional logging.</span></span> <span data-ttu-id="8110b-246">Az hello Azure portál keresse meg a toohello webalkalmazást, és válassza a beállítások.</span><span class="sxs-lookup"><span data-stu-id="8110b-246">In hello Azure Portal, navigate toohello Web app and choose Settings.</span></span>
   
   ![image44][image44]
7. <span data-ttu-id="8110b-248">Görgessen lefelé toohello szolgáltatások kategóriát, és válassza a diagnosztikai naplókat.</span><span class="sxs-lookup"><span data-stu-id="8110b-248">Scroll down toohello features category, and choose Diagnostic logs.</span></span>
   
      ![image45][image45]
8. <span data-ttu-id="8110b-250">Figyelje meg, hello különféle naplózási beállításai.</span><span class="sxs-lookup"><span data-stu-id="8110b-250">Notice hello various options for logging.</span></span> <span data-ttu-id="8110b-251">Kapcsolja be a webkiszolgálók naplózását, és kattintson a Mentés gombra.</span><span class="sxs-lookup"><span data-stu-id="8110b-251">Toggle on Web server logging and click save.</span></span>
   
   ![image46][image46]
9. <span data-ttu-id="8110b-253">Helyezze vissza toohello eszközök terület hello alkalmazáshoz, és válassza ki a diagnosztika szolgáltatásként, és kattintson a Futtatás toorerun hello adatgyűjtés.</span><span class="sxs-lookup"><span data-stu-id="8110b-253">Move back toohello tools area for hello app and choose Diagnostics as a service and click Run toorerun hello data collection.</span></span>
   
   ![image47][image47]
10. <span data-ttu-id="8110b-255">Hello HTTP-naplózás beállítás engedélyezése esetén a HTTP-naplók gyűjtött adatok ekkor megjelenik.</span><span class="sxs-lookup"><span data-stu-id="8110b-255">With hello HTTP logging setting enabled, you now see data collected for HTTP Logs.</span></span>
    
    ![image48][image48]
11. <span data-ttu-id="8110b-257">Hello HTML-fájl napló kattintva további vizsgálatok gazdag böngészőalapú jelentést készít.</span><span class="sxs-lookup"><span data-stu-id="8110b-257">By clicking hello HTML file log, you produce a rich browser-based report for further investigation.</span></span>
    
    ![image49][image49]
12. <span data-ttu-id="8110b-259">Visszalépés toohello eszközök szakasz hello Azure Portal hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8110b-259">Move back toohello tools section in hello Azure Portal for hello app.</span></span> <span data-ttu-id="8110b-260">Görgessen toohello eszközök szakaszban, és válassza ki a Process Explorer.</span><span class="sxs-lookup"><span data-stu-id="8110b-260">Scroll toohello Tools section and choose Process Explorer.</span></span>
    
    ![image50][image50]
13. <span data-ttu-id="8110b-262">A Process Explorer kiválasztásával megtekintheti a futó folyamatok adatait.</span><span class="sxs-lookup"><span data-stu-id="8110b-262">By choosing Process Explorer, you can view details about running processes.</span></span> <span data-ttu-id="8110b-263">Figyelje meg, az alábbi folyamatok elemezze, és még kill minden hello Azure-portál a folyamatokat.</span><span class="sxs-lookup"><span data-stu-id="8110b-263">Notice below you can drill into processes and even kill processes all from hello Azure Portal.</span></span>
    
    ![image51][image51]
    
    ![image52][image52]
14. <span data-ttu-id="8110b-266">Visszalépés toohello beállítások panel hello bal oldalon.</span><span class="sxs-lookup"><span data-stu-id="8110b-266">Move back toohello Settings blade on hello left.</span></span> <span data-ttu-id="8110b-267">Kattintson az Új támogatási kérelem elemre.</span><span class="sxs-lookup"><span data-stu-id="8110b-267">Click New support request.</span></span>
    
    ![image53][image53]
15. <span data-ttu-id="8110b-269">Hello panelen a jobb oldali hello töltse ki a hello hibákkal kapcsolatos információkat, adja meg a kapcsolattartási adatokat, és még a diagnosztikai adatok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="8110b-269">From hello blade on hello right, you can fill out details about hello issues, enter contact information, and even upload diagnostic data.</span></span> <span data-ttu-id="8110b-270">hello Azure Portal lehetővé teszi, hogy a Microsoft támogatási szolgálatához zökkenőmentes élményt használata.</span><span class="sxs-lookup"><span data-stu-id="8110b-270">hello Azure Portal enables working with Microsoft support a seamless experience.</span></span>
    
    ![image54][image54]
    
    ![image55][image55]
    
    <span data-ttu-id="8110b-273">hello Azure Portal biztosítanak a hatékony és ismerős lép toohelp figyelő tooling eszköz segítségével, és hibaelhárítása a futó alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8110b-273">hello Azure Portal helps provide powerful and familiar tooling experiences toohelp monitor and troubleshoot our running applications.</span></span> <span data-ttu-id="8110b-274">Biztosan is képes tootake művelet gyorsan folyamatok újrahasznosítása, engedélyezése és letiltása különböző adatok gyűjtemények és a Microsoft professzionális támogatás még integrálása feladatok végrehajtásával.</span><span class="sxs-lookup"><span data-stu-id="8110b-274">You are also able tootake action quickly by performing tasks such as recycling processes, enabling and disabling various data collections, and even integrating with Microsoft professional support.</span></span>

## <a name="general-application-management"></a><span data-ttu-id="8110b-275">Általános alkalmazásfelügyelet</span><span class="sxs-lookup"><span data-stu-id="8110b-275">General Application Management</span></span>
<span data-ttu-id="8110b-276">Alkalmazások kezelése, ha gyakran kell tooperform tevékenységek, például a biztonsági mentési stratégia, végrehajtási és identitás-szolgáltatóktól kezelése és a szerepköralapú hozzáférés-vezérlés konfigurálása széles választékában.</span><span class="sxs-lookup"><span data-stu-id="8110b-276">When managing applications, you often need tooperform a broad variety of activities such as configuring backup strategies, implementing and managing identity providers, and configuring Role-based access control.</span></span> <span data-ttu-id="8110b-277">A hello más DevOps lép, mivel hello Azure platformon integrálódik ezeket a feladatokat közvetlenül hello portálon.</span><span class="sxs-lookup"><span data-stu-id="8110b-277">As with hello other DevOps experiences, hello Azure platform integrates these tasks directly into hello portal.</span></span>

1. <span data-ttu-id="8110b-278">amelyek megőrzésével tooensure hello webalkalmazás tooconfigure biztonsági mentések szüksége biztonságos adatvesztés ellen.</span><span class="sxs-lookup"><span data-stu-id="8110b-278">tooensure you are keeping hello Web App safe from data loss you need tooconfigure backups.</span></span> <span data-ttu-id="8110b-279">Keresse meg a webalkalmazás toohello beállítások területen.</span><span class="sxs-lookup"><span data-stu-id="8110b-279">Navigate toohello Settings area for your Web app.</span></span>
   
   ![image56][image56]
2. <span data-ttu-id="8110b-281">Hello panelen a jobb oldali hello görgetve toohello szolgáltatásai kategóriában.</span><span class="sxs-lookup"><span data-stu-id="8110b-281">In hello blade on hello right, scroll down toohello Features category.</span></span>
   
    ![image57][image57]
3. <span data-ttu-id="8110b-283">Válassza ki a biztonsági mentések; a jobb oldali hello megnyílik egy panel.</span><span class="sxs-lookup"><span data-stu-id="8110b-283">Choose Backups; a blade opens on hello right.</span></span>
   
   ![image58][image58]
4. <span data-ttu-id="8110b-285">Kattintson a Konfigurálás gombra, válasszon tárfiókot hello panelen a jobb oldali hello.</span><span class="sxs-lookup"><span data-stu-id="8110b-285">Click Configure, choose a storage account from hello blade on hello right.</span></span>
   
   ![image59][image59]
5. <span data-ttu-id="8110b-287">Most hozzon létre, és válassza ki a tárolási tároló toohold a biztonsági másolatok.</span><span class="sxs-lookup"><span data-stu-id="8110b-287">Now create and choose a storage container toohold your backups.</span></span> <span data-ttu-id="8110b-288">Kattintson a létrehozás hello hello panel alsó részén.</span><span class="sxs-lookup"><span data-stu-id="8110b-288">Click create at hello bottom of hello blade.</span></span> <span data-ttu-id="8110b-289">Válassza ki a hello tároló.</span><span class="sxs-lookup"><span data-stu-id="8110b-289">Then select hello container.</span></span>
   
   ![image60][image60]
6. <span data-ttu-id="8110b-291">Miután kiválasztotta a hello tároló, az adatbázisok a ütemezéseket, valamint a telepítés biztonsági mentések állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="8110b-291">Once you have chosen hello container, you can configure schedules, as well as setup backups for your databases.</span></span> <span data-ttu-id="8110b-292">Ebben az esetben kattintson a mentés ikon hello.</span><span class="sxs-lookup"><span data-stu-id="8110b-292">For this scenario, click hello save icon.</span></span>
   
    ![image61][image61]
7. <span data-ttu-id="8110b-294">A mentés után görgessen hello bal oldali hátsó toohello panel a biztonsági mentésekhez.</span><span class="sxs-lookup"><span data-stu-id="8110b-294">After saving, scroll back toohello blade on hello left for Backups.</span></span> <span data-ttu-id="8110b-295">Kattintson a biztonsági mentés most tooback hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8110b-295">Click Backup Now tooback hello application.</span></span>
   
    ![image62][image62]
8. <span data-ttu-id="8110b-297">Rövidesen múlva megjelenik a létrehozott biztonsági másolat.</span><span class="sxs-lookup"><span data-stu-id="8110b-297">In a few moments, you see a backup created.</span></span> <span data-ttu-id="8110b-298">Értesítés hello visszaállítás most az alábbi képernyőfelvételen hello lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="8110b-298">Notice hello Restore Now option on hello screen-shot below.</span></span>
   
    ![image63][image63]
9. <span data-ttu-id="8110b-300">Kattintson a visszaállítás most, és vizsgálja meg jobb hello hello beállítások toohello paneljét.</span><span class="sxs-lookup"><span data-stu-id="8110b-300">Click on Restore Now and examine hello options toohello blade on hello right.</span></span> <span data-ttu-id="8110b-301">Lehetősége van egy megfelelő biztonsági mentés és egyszerű visszaállítás tooan korábbi állapot szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="8110b-301">You can choose an appropriate backup and easily restore tooan earlier state as necessary.</span></span> <span data-ttu-id="8110b-302">hello Azure-portálon hozzájárult, könnyen engedélyezheti egy egyszerű vész-helyreállítási stratégiát hello alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="8110b-302">hello Azure portal has helped us easily enable a simple disaster recovery strategy for hello app.</span></span>
   
    ![image64][image64]
10. <span data-ttu-id="8110b-304">Visszalépés toohello beállítások panel hello bal oldali és a szolgáltatások, és válassza ki a hitelesítési/engedélyezési.</span><span class="sxs-lookup"><span data-stu-id="8110b-304">Move back toohello Settings blade on hello left, and under Features and choose Authentication/Authorization.</span></span>
    
     ![image65][image65]
11. <span data-ttu-id="8110b-306">Hello panelen a jobb oldali hello válassza ki az App Service hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="8110b-306">In hello blade on hello right choose App Service Authentication.</span></span> <span data-ttu-id="8110b-307">Figyelje meg hello a számos népszerű szolgáltatók konfigurálható.</span><span class="sxs-lookup"><span data-stu-id="8110b-307">Notice hello variety of options you can configure with popular providers.</span></span>
    
     ![image66][image66]
12. <span data-ttu-id="8110b-309">Válassza ki azt az Ön által választott hello szolgáltatót, és figyelje meg hello hatókör hello beállításait.</span><span class="sxs-lookup"><span data-stu-id="8110b-309">Choose hello provider of your choice and notice hello options for hello scope.</span></span> <span data-ttu-id="8110b-310">Adjon meg egy alkalmazás Azonosítóját és egy alkalmazás titkos kulcs, és engedélyezheti a Facebook-hitelesítési hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="8110b-310">You can provide an App ID and App Secret and quickly enable Facebook authentication for hello app.</span></span> <span data-ttu-id="8110b-311">hello Azure portál lehetővé teszi a hitelesítést az alkalmazások azonnal használható megoldás.</span><span class="sxs-lookup"><span data-stu-id="8110b-311">hello Azure Portal enables authentication as a turnkey solution for apps.</span></span>
    
     ![image67][image67]
13. <span data-ttu-id="8110b-313">Helyezze vissza toohello beállítások panelen, és válassza ki a felhasználók hello erőforrás kezelése kategóriához tartozó.</span><span class="sxs-lookup"><span data-stu-id="8110b-313">Move back toohello Settings blade and choose Users under hello Resource Management category.</span></span>
    
     ![image68][image68]
14. <span data-ttu-id="8110b-315">Vizsgálja meg a megfelelő hello hello paneljét hello különböző beállítások a szerepkörök és a felhasználók hozzáadásához.</span><span class="sxs-lookup"><span data-stu-id="8110b-315">In hello blade on hello right examine hello various options for adding roles and users.</span></span> <span data-ttu-id="8110b-316">hello Azure portál segítségével szabályozhatja könnyen RBAC (szerepköralapú hozzáférés-vezérlés) hello alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="8110b-316">hello Azure Portal lets you easily control RBAC (Role-based access control) for hello application.</span></span>
    
     ![image69][image69]

## <a name="summary"></a><span data-ttu-id="8110b-318">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="8110b-318">Summary</span></span>
<span data-ttu-id="8110b-319">Ez az oktatóanyag bemutatott egyes hello power hello Azure platformon, gyorsan engedélyezése az webalkalmazáshoz a folyamatos üzembe helyezés, hajt végre a különböző fejlesztési és tesztelési tevékenységeket, figyelés és hibaelhárítás élő app, végül és kezelhet, és kulcs például a vész-helyreállítási, identitáskezelési és szerepköralapú hozzáférés-vezérlés stratégiák.</span><span class="sxs-lookup"><span data-stu-id="8110b-319">This tutorial demonstrated some of hello power with hello Azure platform by quickly enabling continuous deployment for a web app, performing various development and testing activities, monitoring and troubleshooting a live app, and finally managing key strategies such as disaster recovery, identity, and role-based access control.</span></span> <span data-ttu-id="8110b-320">hello Azure platform lehetővé teszi, hogy a DevOps munkafolyamatok integrált megoldást, és dolgozhat hatékonyan által az aktuális hello feladathoz tartozó környezet marad.</span><span class="sxs-lookup"><span data-stu-id="8110b-320">hello Azure platform enables an integrated experience for these DevOps workflows, and you can work efficiently by staying in context for hello task at hand.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8110b-321">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8110b-321">Next steps</span></span>
* <span data-ttu-id="8110b-322">Az Azure Resource Manager fontos DevOps engedélyezését hello Azure platformon.</span><span class="sxs-lookup"><span data-stu-id="8110b-322">Azure Resource Manager is important for enabling DevOps on hello Azure platform.</span></span>  <span data-ttu-id="8110b-323">több látogasson el az toolearn [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="8110b-323">toolearn more visit [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>
* <span data-ttu-id="8110b-324">Azure App Service telepítési olvashat toolearn [az alkalmazás tooAzure App Service telepítése](../app-service-web/web-sites-deploy.md)</span><span class="sxs-lookup"><span data-stu-id="8110b-324">toolearn more about Azure App Service deployment visit [Deploy your app tooAzure App Service](../app-service-web/web-sites-deploy.md)</span></span>

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
