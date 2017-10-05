---
title: "Flighting deployment (bétatesztelés) az Azure App Service-ben"
description: "Útmutató az alkalmazás új szolgáltatások repülési vagy beta végpont oktatóanyagban a frissítések tesztelése. Összegyűjti az App Service szolgáltatások, mint a folyamatos közzététel, tárhelyek, a forgalom útválasztásához és az Application Insights-integráció."
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: 17953c51-38f8-442d-bb0b-f69c1542f0e9
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/02/2016
ms.author: cephalin
ms.openlocfilehash: 83e3247310461ac148fff3c4ade3aa7216478537
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="84818-104">Flighting deployment (bétatesztelés) az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="84818-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="84818-105">Az oktatóanyag bemutatja, hogyan kell végrehajtani *flighting központi telepítések* különböző lehetőségeinek integrálásával [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="84818-105">This tutorial shows you how to do *flighting deployments* by integrating the various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="84818-106">*Flighting* egy folyamatot, amely egy új szolgáltatás, vagy módosítsa a valódi felhasználók korlátozott számú ellenőrzi, és egy fő tesztelése éles telepítési forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="84818-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="84818-107">Mintha beta tesztelése, és mint "ellenőrzött teszt repülési" néven is ismert.</span><span class="sxs-lookup"><span data-stu-id="84818-107">It is akin to beta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="84818-108">Egy webes jelenlét sok nagy vállalatok ezt a módszert használja az app frissítéseken a gyakorlatban a korai érvényesítési eléréséhez [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="84818-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="84818-109">Az Azure App Service lehetővé teszi a teszt termelési környezetben integrálható a folyamatos közzététele és az Application Insights is az azonos DevOps forgatókönyv megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="84818-109">Azure App Service enables you to integrate test in production with continous publishing and Application Insights to implement the same DevOps scenario.</span></span> <span data-ttu-id="84818-110">Ez a módszer előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="84818-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="84818-111">**Valós visszajelzés szerezhet *előtt* frissítések üzemi** -jobb, mint való visszajelzést, amint felszabadít egyedül van való visszajelzést, engedélyezés előtt.</span><span class="sxs-lookup"><span data-stu-id="84818-111">**Gain real feedback *before* updates are released to production** - The only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="84818-112">A felhasználó forgalmi és viselkedéshez frissítések korai biztosítják a életciklusa tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="84818-112">You can test updates with real user traffic and behaviors as early as you desire in the product life cycle.</span></span>
* <span data-ttu-id="84818-113">**Javíthatja [folyamatos teszt adatvezérelt fejlesztési (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  - teszt termelési környezetben és integrálásával folyamatos integrációt és az Application insights szolgáltatással instrumentation felhasználói érvényesítési korai és automatikusan történik, a termék életciklusának.</span><span class="sxs-lookup"><span data-stu-id="84818-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="84818-114">Ezzel csökkenthető manuális tesztelési végrehajtási idő befektetések.</span><span class="sxs-lookup"><span data-stu-id="84818-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="84818-115">**Teszt munkafolyamat optimalizálása** -teszt termelési környezetben, a folyamatos figyelési instrumentation automatizálásával potenciálisan elvégezhető a célja, különféle típusú tesztek egyetlen folyamat, például a [integrációs](https://en.wikipedia.org/wiki/Integration_testing), [regressziós](https://en.wikipedia.org/wiki/Regression_testing), [használhatóság](https://en.wikipedia.org/wiki/Usability_testing), kisegítő, honosítási, [teljesítmény](https://en.wikipedia.org/wiki/Software_performance_testing), [biztonsági](https://en.wikipedia.org/wiki/Security_testing), és [elfogadási](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="84818-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish the goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="84818-116">Egy flighting telepítési szinte nem útválasztási élő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="84818-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="84818-117">Ilyen elrendezés minél gyorsabban áttekintésével betekintést szeretné hogy legyen egy nem várt hiba, a teljesítménycsökkenés, a felhasználói élmény problémák.</span><span class="sxs-lookup"><span data-stu-id="84818-117">In such a deployment you want to gain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="84818-118">Ne feledje, hogy valós ügyfelek foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="84818-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="84818-119">Ezért ne jobb oldali biztosítsa, hogy állította be a flighting telepítés tájékozott döntést, a következő lépéshez szükséges minden adatgyűjtést.</span><span class="sxs-lookup"><span data-stu-id="84818-119">So to do it right, you must make sure that you have set up your flighting deployment to gather all the data you need in order to make an informed decision for your next step.</span></span> <span data-ttu-id="84818-120">Az oktatóanyag bemutatja, hogyan szeretné az adatgyűjtést az Application insights szolgáltatással, de a New Relic vagy egyéb technológiák, amely megfelel az adott esetben is használhatja.</span><span class="sxs-lookup"><span data-stu-id="84818-120">This tutorial shows you how to collect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="84818-121">Mit fog</span><span class="sxs-lookup"><span data-stu-id="84818-121">What you will do</span></span>
<span data-ttu-id="84818-122">Ebben az oktatóanyagban megtanulhatja, hogyan kell az App Service-alkalmazás tesztelése éles üzemben segítségével a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="84818-122">In this tutorial, you will learn how to bring the following scenarios together to test your App Service app in production:</span></span>

* <span data-ttu-id="84818-123">[Éles forgalmat](app-service-web-test-in-production-get-start.md) a béta-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="84818-123">[Route production traffic](app-service-web-test-in-production-get-start.md) to your beta app</span></span>
* <span data-ttu-id="84818-124">[Az alkalmazás állíthatnak](../application-insights/app-insights-web-track-usage.md) hasznos metrikákat beszerzése</span><span class="sxs-lookup"><span data-stu-id="84818-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) to obtain useful metrics</span></span>
* <span data-ttu-id="84818-125">Folyamatosan a béta-alkalmazás telepítése, és nyomon követheti az élő app metrikák</span><span class="sxs-lookup"><span data-stu-id="84818-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="84818-126">Az éles alkalmazások és a béta alkalmazás láthatja, hogyan kódmódosításokat fordítás eredmények között mérőszámok összehasonlítása</span><span class="sxs-lookup"><span data-stu-id="84818-126">Compare metrics between the production app and the beta app to see how code changes translate to results</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="84818-127">Mit kell</span><span class="sxs-lookup"><span data-stu-id="84818-127">What you will need</span></span>
* <span data-ttu-id="84818-128">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="84818-128">An Azure account</span></span>
* <span data-ttu-id="84818-129">A [GitHub](https://github.com/) fiók</span><span class="sxs-lookup"><span data-stu-id="84818-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="84818-130">A Visual Studio 2015 - letöltheti a [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="84818-130">Visual Studio 2015 - you can download the [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="84818-131">Git rendszerhéj (telepített [GitHub for Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy a Git és a PowerShell-parancsok futtatása ugyanabban a munkamenetben</span><span class="sxs-lookup"><span data-stu-id="84818-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you to run both the Git and PowerShell commands in the same session</span></span>
* <span data-ttu-id="84818-132">Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="84818-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="84818-133">A következő alapvető ismeretekkel:</span><span class="sxs-lookup"><span data-stu-id="84818-133">Basic understanding of the following:</span></span>
  * <span data-ttu-id="84818-134">[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sablon-üzembehelyezés (lásd: [kiszámítható módon tudja az Azure-ban összetett alkalmazást központilag](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="84818-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="84818-135">Git</span><span class="sxs-lookup"><span data-stu-id="84818-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="84818-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="84818-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="84818-137">A jelen oktatóanyag elvégzéséhez Azure-fiókra van szükség:</span><span class="sxs-lookup"><span data-stu-id="84818-137">You need an Azure account to complete this tutorial:</span></span>
>
> * <span data-ttu-id="84818-138">Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -kapott kreditek használatával kipróbálhatja a fizetős Azure-szolgáltatásokat, és még azok lejárta után is megtarthatja a fiókot, és használhatja az ingyenes Azure-szolgáltatások, például webes alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="84818-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use to try out paid Azure services, and even after they're used up you can keep the account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="84818-139">Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -a Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="84818-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="84818-140">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="84818-140">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="84818-141">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="84818-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="84818-142">Az éles webes alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="84818-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="84818-143">Ebben az oktatóanyagban használt parancsfájl automatikusan konfigurálja a GitHub-tárház folyamatos közzététel.</span><span class="sxs-lookup"><span data-stu-id="84818-143">The script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="84818-144">Ehhez az szükséges, hogy a Githubon tartozó hitelesítő adatok már szerepelnek Azure, ellenkező esetben a parancsprogram-alapú központi telepítés sikertelen lesz a verziókövetési beállítások a webes alkalmazások konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="84818-144">This requires that your GitHub credentials are already stored in Azure, otherwise the scripted deployment will fail when attempting to configure source control settings for the web apps.</span></span>
>
> <span data-ttu-id="84818-145">A GitHub hitelesítő adatok tárolását az Azure-ban, a webalkalmazás létrehozása a [Azure Portal](https://portal.azure.com/) és [GitHub-KözpontiTelepítés konfigurálása](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="84818-145">To store your GitHub credentials in Azure, create a web app in the [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="84818-146">Csak akkor kell egyszer.</span><span class="sxs-lookup"><span data-stu-id="84818-146">You only need to do this once.</span></span>
>
>

<span data-ttu-id="84818-147">Egy tipikus DevOps forgatókönyv valamely alkalmazás az élő Azure-ban futó, és szeretné módosítani a folyamatos közzétételen keresztül;.</span><span class="sxs-lookup"><span data-stu-id="84818-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want to make changes to it through continuous publishing.</span></span> <span data-ttu-id="84818-148">Ebben a forgatókönyvben Ha telepíti az üzemi egy sablon, amelyet kifejlesztett és tesztelt.</span><span class="sxs-lookup"><span data-stu-id="84818-148">In this scenario, you will deploy to production a template that you have developed and tested.</span></span>

1. <span data-ttu-id="84818-149">Hozzon létre a saját elágazás a [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházba.</span><span class="sxs-lookup"><span data-stu-id="84818-149">Create your own fork of the [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="84818-150">Létrehozásával kapcsolatos információkat az elágazáshoz, tekintse meg a [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="84818-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="84818-151">Ha létrejött az elágazáshoz, tekintheti meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="84818-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="84818-152">Nyisson meg egy Git rendszerhéj-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="84818-152">Open a Git Shell session.</span></span> <span data-ttu-id="84818-153">Ha a Git rendszerhéj még nincs, telepítse [GitHub for Windows](https://windows.github.com/) most.</span><span class="sxs-lookup"><span data-stu-id="84818-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="84818-154">Klónozhatja helyi az elágazáshoz futtassa a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="84818-154">Create a local clone of your fork by executing the following command:</span></span>

     <span data-ttu-id="84818-155">git-klón https://github.com/<your_fork>/ToDoApp.git</span><span class="sxs-lookup"><span data-stu-id="84818-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="84818-156">Ha elvégezte a helyi klónozás, navigáljon a  *&lt;repository_root >*\ARMTemplates, és futtatnia kell a deploy.ps1 parancsfájlt egyedi utótagot kapjon, az alább látható módon:</span><span class="sxs-lookup"><span data-stu-id="84818-156">Once you have your local clone, navigate to *&lt;repository_root>*\ARMTemplates, and run the deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="84818-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="84818-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="84818-158">Amikor a rendszer kéri, írja be a kívánt felhasználónevet és jelszót az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="84818-158">When prompted, type in the desired username and password for database access.</span></span> <span data-ttu-id="84818-159">Ne felejtse el az adatbázis hitelesítő adatait, mert kell újból megadnia őket az erőforráscsoport frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="84818-159">Remember your database credentials because you will need to specify them again when updating the resource group.</span></span>

   <span data-ttu-id="84818-160">Meg kell jelennie a kiépítési folyamat különböző Azure-erőforrások.</span><span class="sxs-lookup"><span data-stu-id="84818-160">You should see the provisioning progress of various Azure resources.</span></span> <span data-ttu-id="84818-161">Központi telepítés befejezése után a parancsfájl indítani az alkalmazást a böngészőben, és adjon meg egy rövid hangjelzés.</span><span class="sxs-lookup"><span data-stu-id="84818-161">When deployment completes, the script will launch the application in the browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="84818-162">Vissza a Git rendszerhéj-munkamenetben futtassa:</span><span class="sxs-lookup"><span data-stu-id="84818-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="84818-163">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="84818-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="84818-164">A parancsfájl befejezése után térjen vissza az előtér-cím megkeresése tallózással (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) az éles környezetben futó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84818-164">When the script finishes, go back to browse to the frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) to see the application running in production.</span></span>
8. <span data-ttu-id="84818-165">Jelentkezzen be a [Azure Portal](https://portal.azure.com/) és vessen egy pillantást mi jön létre.</span><span class="sxs-lookup"><span data-stu-id="84818-165">Log into the [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="84818-166">Meg kell tudni ugyanazt az erőforráscsoportot, egy-két webes alkalmazások jelennek meg a `Api` utótag nevét.</span><span class="sxs-lookup"><span data-stu-id="84818-166">You should be able to see two web apps in the same resource group, one with the `Api` suffix in the name.</span></span> <span data-ttu-id="84818-167">Az erőforráscsoport nézet tekinti meg, ha az SQL-adatbázis és a kiszolgáló, az App Service-csomag és az átmeneti üzembe helyezési ponti a web Apps is láthat.</span><span class="sxs-lookup"><span data-stu-id="84818-167">If you look at the resource group view, you will also see the SQL Database and server, the App Service plan, and the staging slots for the web apps.</span></span> <span data-ttu-id="84818-168">Tallózzon a különböző erőforrások, és hasonlítsa össze azokat a  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json megtekintéséhez, hogy azok miként vannak konfigurálva a sablonban.</span><span class="sxs-lookup"><span data-stu-id="84818-168">Browse through the different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json to see how they are configured in the template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="84818-169">Az éles alkalmazás beállítása.</span><span class="sxs-lookup"><span data-stu-id="84818-169">You have set up the production app.</span></span>  <span data-ttu-id="84818-170">Most tegyük képzelhető el, hogy megjelenik-e visszajelzést, hogy az alkalmazás gyenge-e használhatóság.</span><span class="sxs-lookup"><span data-stu-id="84818-170">Now, let's imagine that you receive feedback that usability is poor for the app.</span></span> <span data-ttu-id="84818-171">Ezért úgy dönt, hogy kivizsgálja a Microsofttal.</span><span class="sxs-lookup"><span data-stu-id="84818-171">So you decide to investigate.</span></span> <span data-ttu-id="84818-172">Beállíthatják az alkalmazásnak, hogy visszajelzést fog.</span><span class="sxs-lookup"><span data-stu-id="84818-172">You're going to instrument your app to give you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="84818-173">Vizsgálja meg: Állíthatnak be az ügyfél alkalmazás figyelési/metrikák</span><span class="sxs-lookup"><span data-stu-id="84818-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="84818-174">Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo.sln a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="84818-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="84818-175">Kattintson a jobb gombbal a megoldás Nuget-csomagok visszaállítása > **NuGet-csomagok kezelése megoldáshoz** > **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="84818-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="84818-176">Kattintson a jobb gombbal **MultiChannelToDo.Web** > **Application Insights Telemetria** > **beállításainak konfigurálása** > Módosítás erőforráscsoportot ToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása a projekthez**.</span><span class="sxs-lookup"><span data-stu-id="84818-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
4. <span data-ttu-id="84818-177">Az Azure portál panel megnyitásához a **MultiChannelToDo.Web** Application Insights-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="84818-177">In the Azure Portal, open the blade for the **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="84818-178">Ezt a a **alkalmazás állapotának** része, kattintson a **további tudnivalók a böngészők lapbetöltési adatainak gyűjtéséről** > kód másolása.</span><span class="sxs-lookup"><span data-stu-id="84818-178">Then in the **Application health** part, click **Learn how to collect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="84818-179">Adja hozzá a másolt JS instrumentation kódot  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, csak a Bezárás előtt `<heading>` címke.</span><span class="sxs-lookup"><span data-stu-id="84818-179">Add the copied JS instrumentation code to *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before the closing `<heading>` tag.</span></span> <span data-ttu-id="84818-180">Az Application Insights-erőforrás a instrumentation egyedi kulcsot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="84818-180">It should contain the unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="84818-181">Küldése egyéni események az Application Insights az egér gombra kell kattintania törzs alján ad hozzá a következő kódot:</span><span class="sxs-lookup"><span data-stu-id="84818-181">Send custom events to Application Insights for mouse clicks by adding the following code to the bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="84818-182">A JavaScript részlet minden alkalommal, amikor a felhasználó kattint bárhol a web app alkalmazásban küld az Application Insights egyéni esemény.</span><span class="sxs-lookup"><span data-stu-id="84818-182">This JavaScript snippet sends a custom event to Application Insights every time a user clicks anywhere in the web app.</span></span>
7. <span data-ttu-id="84818-183">A Git rendszerhéj véglegesíteni, és továbbítsa a módosításokat a Githubon az elágazáshoz.</span><span class="sxs-lookup"><span data-stu-id="84818-183">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="84818-184">Ezután Várjon, amíg az ügyfelek számára, frissítse a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="84818-184">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="84818-185">A felcserélendő éles üzembe helyezett alkalmazás változásai:</span><span class="sxs-lookup"><span data-stu-id="84818-185">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="84818-186">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="84818-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="84818-187">Tallózással keresse meg az Application Insights-erőforrás konfigurált.</span><span class="sxs-lookup"><span data-stu-id="84818-187">Browse to the Application Insights resource that you configured.</span></span> <span data-ttu-id="84818-188">Kattintson az egyéni események.</span><span class="sxs-lookup"><span data-stu-id="84818-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="84818-189">Ha nem látja az egyéni események metrikákat, várjon néhány percet, és kattintson **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="84818-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="84818-190">Tegyük fel, hogy látja, a diagram például alatt:</span><span class="sxs-lookup"><span data-stu-id="84818-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="84818-191">És az esemény rács alá:</span><span class="sxs-lookup"><span data-stu-id="84818-191">And the event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="84818-192">A ToDoApp alkalmazáskód megfelelően a **GOMB** esemény megfelel-e a Küldés gombra, és a **bemeneti** esemény megfelel-e a szövegmező.</span><span class="sxs-lookup"><span data-stu-id="84818-192">According to your ToDoApp application code, the **BUTTON** event corresponds to the submit button, and the **INPUT** event corresponds to the textbox.</span></span> <span data-ttu-id="84818-193">Az eddigi dolgot értelme.</span><span class="sxs-lookup"><span data-stu-id="84818-193">So far, things make sense.</span></span> <span data-ttu-id="84818-194">Azonban úgy tűnik, a megfelelő mennyiségű kattintások és nagyon mindössze néhány kattintással a a Tennivalólista elemein (a **LI** események).</span><span class="sxs-lookup"><span data-stu-id="84818-194">However, it looks like there's a good amount of clicks and very few clicks on the to-do items (the **LI** events).</span></span>

<span data-ttu-id="84818-195">Ezzel, akkor űrlap alapján a alapul, hogy egyes felhasználók nem biztos, mely a felhasználói felület része kattintható, és mivel a cursor stílussal a kijelölt szöveg, ha azt a listaelemek és annak ikonja felett áll.</span><span class="sxs-lookup"><span data-stu-id="84818-195">Based on this, you form your hypothesis that some users are confused which part of the UI is clickable and it is because the cursor is styled for text selection when it hovers on the list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="84818-196">Ez lehet egy contrived példa.</span><span class="sxs-lookup"><span data-stu-id="84818-196">This might be a contrived example.</span></span> <span data-ttu-id="84818-197">Ettől függetlenül fog az alkalmazás javítása, és hajtsa végre a használhatóság visszajelzés lekérése élő ügyfelek flighting központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="84818-197">Nevertheless, you're going to make an improvement to your app, and then perform a flighting deployment to get usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="84818-198">A kiszolgáló alkalmazás figyelési/metrikáihoz állíthatnak</span><span class="sxs-lookup"><span data-stu-id="84818-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="84818-199">Egy érintő Ez azért, mert a forgatókönyvet, az oktatóanyag egy csak az ügyfélalkalmazás foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="84818-199">This is a tangent since the scenario demonstrated in this tutorial only deals with the client app.</span></span> <span data-ttu-id="84818-200">Azonban a teljesség beállításához nyújt útmutatást a kiszolgálóoldali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="84818-200">However, for completeness you will set up the server-side app.</span></span>

1. <span data-ttu-id="84818-201">Kattintson a jobb gombbal **MultiChannelToDo** > **Application Insights Telemetria** > **beállításainak konfigurálása** > Módosítás erőforráscsoportot ToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása a projekthez**.</span><span class="sxs-lookup"><span data-stu-id="84818-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group to ToDoApp*&lt;your_suffix>* > **Add Application Insights to Project**.</span></span>
2. <span data-ttu-id="84818-202">A Git rendszerhéj véglegesíteni, és továbbítsa a módosításokat a Githubon az elágazáshoz.</span><span class="sxs-lookup"><span data-stu-id="84818-202">In Git Shell, commit and push your changes to your fork in GitHub.</span></span> <span data-ttu-id="84818-203">Ezután Várjon, amíg az ügyfelek számára, frissítse a böngészőt.</span><span class="sxs-lookup"><span data-stu-id="84818-203">Then, wait for clients to refresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="84818-204">A felcserélendő éles üzembe helyezett alkalmazás változásai:</span><span class="sxs-lookup"><span data-stu-id="84818-204">Swap the deployed app changes to production:</span></span>

     <span data-ttu-id="84818-205">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="84818-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="84818-206">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="84818-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-to-your-client-app-metrics"></a><span data-ttu-id="84818-207">Vizsgálja meg: Tárolóhely-specifikus címkék hozzáadása az ügyfél app metrikák</span><span class="sxs-lookup"><span data-stu-id="84818-207">Investigate: Add slot-specific tags to your client app metrics</span></span>
<span data-ttu-id="84818-208">Ebben a szakaszban konfigurálhatja a különböző üzembe helyezési tárhely vonatkozó telemetriai adatokat küldhet ugyanahhoz az Application Insights-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="84818-208">In this section, you will configure the different deployment slots to send slot-specific telemetry to the same Application Insights resource.</span></span> <span data-ttu-id="84818-209">Ezzel a módszerrel összehasonlíthatja telemetriai adatokat különböző üzembe helyezési ponti (központi telepítési környezetben) a forgalom között könnyen Észreveheti, annak hatását, hogy az alkalmazások változásairól.</span><span class="sxs-lookup"><span data-stu-id="84818-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) to easily see the effect of your app changes.</span></span> <span data-ttu-id="84818-210">Egy időben elkülönítheti a termelési forgalmat a többi, továbbra is figyelheti a termelési alkalmazást igény szerint.</span><span class="sxs-lookup"><span data-stu-id="84818-210">At the same time, you can separate the production traffic from the rest so you can continue to monitor your production app as needed.</span></span>

<span data-ttu-id="84818-211">Az ügyfél viselkedése az adatgyűjtést van, akkor lesz [adja hozzá a telemetriai adatok inicializáló a JavaScript-kód](../application-insights/app-insights-api-filtering-sampling.md) a index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="84818-211">Since you're gathering data on client behavior, you will [add a telemetry initializer to your JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="84818-212">Ha azt szeretné, kiszolgálóoldali teljesítményének a tesztelésére, például is elvégezheti hasonlóan a kiszolgáló kódjában található (lásd: [Application Insights API egyéni események és metrikák](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="84818-212">If you want to test server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="84818-213">Első lépésként adja meg a kódot bewteen a két `//` a JavaScript az alábbi megjegyzések blokkolása fel ezeket a `<heading>` korábbi címkét.</span><span class="sxs-lookup"><span data-stu-id="84818-213">First, add the code bewteen the two `//` comments below in the JavaScript block that you added to the `<heading>` tag earlier.</span></span>

        window.appInsights = appInsights;

        // Begin new code
        appInsights.queue.push(function () {
            appInsights.context.addTelemetryInitializer(function (envelope) {
                var telemetryItem = envelope.data.baseData;
                telemetryItem.properties = telemetryItem.properties || {};
                telemetryItem.properties["Environment"] = "@System.Configuration.ConfigurationManager.AppSettings["environment"]";
            });
        });
        // End new code

        appInsights.trackPageView();

    <span data-ttu-id="84818-214">Az inicializáló okozza a `appInsights` objektum hozzáadása az egyéni tulajdonság neve `Environment` minden adat, telemetriai adatokat küld a.</span><span class="sxs-lookup"><span data-stu-id="84818-214">This initializer code causes the `appInsights` object to add the a custom property called `Environment` to every piece of telemetry it sends.</span></span>
2. <span data-ttu-id="84818-215">Ezután adja hozzá az egyéni tulajdonság egy [beállítás tárolóhely](web-sites-staged-publishing.md#AboutConfiguration) a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="84818-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="84818-216">Ehhez futtassa az alábbi parancsokat a Git rendszerhéj-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="84818-216">To do this, run the following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="84818-217">A Web.config fájl, a projekt már határozza meg a `environment` Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="84818-217">The Web.config in your project already defines the `environment` app setting.</span></span> <span data-ttu-id="84818-218">Ezzel a beállítással az alkalmazást helyileg, tesztelésekor a metrikákat címkével fog rendelkezni a `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="84818-218">With this setting, when you test the app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="84818-219">Azonban ha az Azure-bA továbbítsa a módosításokat, Azure lesz megkeresheti, a `environment` alkalmazás inkább a webalkalmazás konfigurációs beállítás, és a metrikákat címkével rendelkező `Production`.</span><span class="sxs-lookup"><span data-stu-id="84818-219">However, when you push your changes to Azure, Azure will find and use the `environment` app setting in the web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="84818-220">Véglegesítse és küldje le a kód módosításokat az elágazáshoz a Githubon, és majd várja meg a felhasználókat, hogy az új alkalmazás (frissítse a böngészőt kell) használatát.</span><span class="sxs-lookup"><span data-stu-id="84818-220">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="84818-221">Az Application Insights megjelennek az új tulajdonság körülbelül 15 percet vesz igénybe `MultiChannelToDo.Web` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="84818-221">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for client app"
        git push origin master
4. <span data-ttu-id="84818-222">Most, lépjen a **egyéni események** újra a panelt és a metrikák szűrést végezni `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="84818-222">Now, go to the **Custom events** blade again and filter the metrics on `Environment=Production`.</span></span> <span data-ttu-id="84818-223">Most kell a szűrőt az összes az új egyéni események az éles ponton látni.</span><span class="sxs-lookup"><span data-stu-id="84818-223">You should now be able to see all the new custom events in the production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="84818-224">Kattintson a **Kedvencek** gombra kattintva mentse az aktuális Metrikaböngésző beállításait hasonlót **egyéni események: éles**.</span><span class="sxs-lookup"><span data-stu-id="84818-224">Click the **Favorites** button to save the current Metrics Explorer settings to something like **Custom events: Production**.</span></span> <span data-ttu-id="84818-225">Egyszerűen válthat között ez és egy központi telepítési tárolóhely nézetben később.</span><span class="sxs-lookup"><span data-stu-id="84818-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="84818-226">Még sokkal hatékonyabb elemzés, érdemes lehet [az Application Insights-erőforrás integrálása a Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="84818-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-to-your-server-app-metrics"></a><span data-ttu-id="84818-227">A server app metrikákat, tárhely-specifikus címkék hozzáadása</span><span class="sxs-lookup"><span data-stu-id="84818-227">Add slot-specific tags to your server app metrics</span></span>
<span data-ttu-id="84818-228">Ebben az esetben a teljesség beállításához nyújt útmutatást a kiszolgálóoldali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="84818-228">Again, for completeness you will set up the server-side app.</span></span> <span data-ttu-id="84818-229">Eltérően az ügyfél-alkalmazást, mert a JavaScript tagolva van a tárhely-specifikus címkék a server app tagolva van a .NET-kódot.</span><span class="sxs-lookup"><span data-stu-id="84818-229">Unlike the client app which is instrumented in JavaScript, slot-specific tags for the server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="84818-230">Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="84818-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="84818-231">Adja hozzá a lezáró előtt az alábbi kódrészletet névtér kapcsos zárójelet.</span><span class="sxs-lookup"><span data-stu-id="84818-231">Add the code block below, just before the closing namespace curly brace.</span></span>

        namespace MultiChannelToDo
        {
                ...

                // Begin new code
            public class ConfigInitializer
            : ITelemetryInitializer
            {
                void ITelemetryInitializer.Initialize(ITelemetry telemetry)
                {
                    telemetry.Context.Properties["Environment"] = System.Configuration.ConfigurationManager.AppSettings["environment"];
                }
            }
                // End new code
        }
2. <span data-ttu-id="84818-232">Javítsa ki a név feloldása hibákat hozzáadásával a `using` utasítás alatt elején a fájl:</span><span class="sxs-lookup"><span data-stu-id="84818-232">Correct the name resolution errors by adding the `using` statements below to the beginning of the file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="84818-233">Adja hozzá az alábbi kódot a elejéhez a `Application_Start()` módszert:</span><span class="sxs-lookup"><span data-stu-id="84818-233">Add the code below to the beginning of the `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="84818-234">Véglegesítse és küldje le a kód módosításokat az elágazáshoz a Githubon, és majd várja meg a felhasználókat, hogy az új alkalmazás (frissítse a böngészőt kell) használatát.</span><span class="sxs-lookup"><span data-stu-id="84818-234">Commit and push your code changes to your fork on GitHub, and then wait for your users to use the new app (need to refresh the browser).</span></span> <span data-ttu-id="84818-235">Az Application Insights megjelennek az új tulajdonság körülbelül 15 percet vesz igénybe `MultiChannelToDo` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="84818-235">It takes about 15 minutes for the new property to show up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property to AI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="84818-236">Frissítés: A béta fiókirodai beállítása</span><span class="sxs-lookup"><span data-stu-id="84818-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="84818-237">Nyissa meg  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json és a Keresés a `appsettings` erőforrások (keressen `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="84818-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find the `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="84818-238">Nincsenek 4 egyet mindegyik tárolóhely őket.</span><span class="sxs-lookup"><span data-stu-id="84818-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="84818-239">Az egyes `appsettings` erőforrás, adja hozzá egy `"environment": "[parameters('slotName')]"` Alkalmazásbeállítás végén a `properties` tömb.</span><span class="sxs-lookup"><span data-stu-id="84818-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting to the end of the `properties` array.</span></span> <span data-ttu-id="84818-240">Ne feledje, vesszővel válassza el az előző sor végére.</span><span class="sxs-lookup"><span data-stu-id="84818-240">Don't forget to end the previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="84818-241">Hozzáadott a `environment` a sablon összes üzembe helyezési ponti Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="84818-241">You have just added the `environment` app setting to all the slots in the template.</span></span>
3. <span data-ttu-id="84818-242">Ugyanebben a fájlban található a `slotconfignames` erőforrások (keressen `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="84818-242">In the same file, find the `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="84818-243">Nincsenek 2 valamelyiket, egy, az egyes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="84818-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="84818-244">Az egyes `slotconfignames` erőforrás hozzáadása `"environment"` végének a `appSettingNames` tömb.</span><span class="sxs-lookup"><span data-stu-id="84818-244">For each `slotconfignames` resource, add `"environment"` to the end of the `appSettingNames` array.</span></span> <span data-ttu-id="84818-245">Ne feledje, vesszővel válassza el az előző sor végére.</span><span class="sxs-lookup"><span data-stu-id="84818-245">Don't forget to end the previous line with a comma.</span></span>

    <span data-ttu-id="84818-246">Van szükség a `environment` memóriás beállítást a megfelelő üzembe helyezési pont mindkét alkalmazások app.</span><span class="sxs-lookup"><span data-stu-id="84818-246">You have just made the `environment` app setting stick to its respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="84818-247">A Git rendszerhéj-munkamenetben futtassa a következő parancsokat a azonos erőforrás csoport utótaggal előtt használt.</span><span class="sxs-lookup"><span data-stu-id="84818-247">In your Git Shell session, run the following commands with the same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="84818-248">Amikor a rendszer kéri, adja meg az azonos SQL adatbázis-hitelesítő adatok szerint előtt.</span><span class="sxs-lookup"><span data-stu-id="84818-248">When prompted, specify the same SQL database credentials as before.</span></span> <span data-ttu-id="84818-249">Az erőforráscsoport frissítése kérdésnél írja be, `Y`, majd `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="84818-249">Then, when asked to update the resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="84818-250">Miután a parancsfájl befejeződik, az eredeti erőforráscsoporthoz tartozik, az erőforrások megmaradnak, de új tárhely "béta" nevű hozza létre azt a "Tesztelés" tárolóhely elején a létrehozott azonos konfigurációjú.</span><span class="sxs-lookup"><span data-stu-id="84818-250">Once the script finishes, all your resources in the original resource group are retained, but a new slot named "beta" is created in it with the same configuration as the "Staging" slot that was created in the beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="84818-251">Ez a metódus létrehozásának különböző telepítési környezetekben eltér metódust [gyors szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="84818-251">This method of creating different deployment environments is different from the method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="84818-252">Itt hoz létre központi telepítési környezetben üzembe helyezési, ahol is vannak hoz létre központi telepítési környezetekben az erőforráscsoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="84818-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="84818-253">Telepítési környezetekben kezelése az erőforráscsoportokhoz tesz lehetővé az éles környezetben off-limits a fejlesztők számára, de nincs könnyen éles, amelyek könnyen elvégezhető üzembe helyezési ponti tesztelés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="84818-253">Managing deployment environments with resource groups enables you to keep the production environment off-limits to developers, but it's not easy to do testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="84818-254">Ha kívánja, is létrehozhat egy alfa alkalmazás futtatásával</span><span class="sxs-lookup"><span data-stu-id="84818-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="84818-255">Ehhez az oktatóanyaghoz akkor csak használja a béta-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="84818-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-to-the-beta-app"></a><span data-ttu-id="84818-256">Frissítés: A frissítések leküldése a béta-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="84818-256">Update: Push your updates to the beta app</span></span>
<span data-ttu-id="84818-257">Vissza a az alkalmazás, amelyet javítása érdekében.</span><span class="sxs-lookup"><span data-stu-id="84818-257">Back to your app that you want to improve.</span></span>

1. <span data-ttu-id="84818-258">Ellenőrizze, hogy van-e már a béta fiókirodai</span><span class="sxs-lookup"><span data-stu-id="84818-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="84818-259">A  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, keresse a `<li>` címkét, és adja hozzá a `style="cursor:pointer"` attribútumot, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="84818-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find the `<li>` tag and add the `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="84818-260">Véglegesítse és leküldéses az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="84818-260">commit and push to Azure.</span></span>
4. <span data-ttu-id="84818-261">Győződjön meg arról, hogy a módosítás is most megjelenik a béta tárolóhely http://todoapp útvonalon*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="84818-261">Verify that the change is now reflected in the beta slot by navigating to http://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="84818-262">Ha még nem látja a módosítást, frissítse a böngészőt, hogy az új javascript-kód beolvasása.</span><span class="sxs-lookup"><span data-stu-id="84818-262">If you don't see the change yet, refresh your browser to get the new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="84818-263">Most, hogy a módosítás a béta tárolóhelye fut, készen áll a flighting központi telepítés végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="84818-263">Now that you have your change running in the beta slot, you are ready to perform a flighting deployment.</span></span>

## <a name="validate-route-traffic-to-the-beta-app"></a><span data-ttu-id="84818-264">Ellenőrzése: Irányíthatja a forgalmat a béta-alkalmazásba</span><span class="sxs-lookup"><span data-stu-id="84818-264">Validate: Route traffic to the beta app</span></span>
<span data-ttu-id="84818-265">Ebben a szakaszban a béta alkalmazásba irányítja a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="84818-265">In this section, you will route traffic to the beta app.</span></span> <span data-ttu-id="84818-266">Bemutató átláthatóság érdekében for sake of fog jelentős részét a felhasználói forgalom átirányítása.</span><span class="sxs-lookup"><span data-stu-id="84818-266">For sake of clarity of demonstration, you're going to route a significant portion of the user traffic to it.</span></span> <span data-ttu-id="84818-267">A valóságban kívánt útválasztó forgalom mennyisége adott helyzettől függ.</span><span class="sxs-lookup"><span data-stu-id="84818-267">In reality, the amount of traffic you want to route will depend on your specific situation.</span></span> <span data-ttu-id="84818-268">Például ha a webhely Microsoft.com léptékű, majd szükség lehet a teljes forgalom kevesebb mint egy százalék a hasznos adatok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="84818-268">For example, if your site is at the scale of microsoft.com, then you may need less than one percent of your total traffic in order to gain useful data.</span></span>

1. <span data-ttu-id="84818-269">A Git rendszerhéj-munkamenetben, a béta tárolóhely-hez irányítandó fele a termelési forgalmat a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="84818-269">In your Git Shell session, run the following commands to route half of the production traffic to the beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="84818-270">A `ReroutePercentage=50` tulajdonság határozza meg, hogy a rendszer 50 %-a termelési forgalom irányítsa a béta-alkalmazás URL-CÍMÉT (által a `ActionHostName` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="84818-270">The `ReroutePercentage=50` property specifies that 50% of the production traffic will be routed to the beta app's URL (specified by the `ActionHostName` property).</span></span>
2. <span data-ttu-id="84818-271">Most lépjen http://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="84818-271">Now navigate to http://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="84818-272">50 %-a forgalmat a most már a rendszer átirányítja a béta-tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="84818-272">50% of the traffic should now be redirected to the beta slot.</span></span>
3. <span data-ttu-id="84818-273">Az Application Insights-erőforrást, a szűrést a metrikák környezet = "béta".</span><span class="sxs-lookup"><span data-stu-id="84818-273">In your Application Insights resource, filter the metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="84818-274">Ha egy másik kedvencként menti a szűrt nézet, majd egyszerűen váltani a metrika explorer nézetek éles tárhely és a béta nézetek között.</span><span class="sxs-lookup"><span data-stu-id="84818-274">If you save this filtered view as another favorite, then you can easily flip the metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="84818-275">Tegyük fel, hogy az Application Insightsban ehhez a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="84818-275">Suppose in Application Insights you see something similar to the following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="84818-276">Nem csak a rendszer Ez azt jelzi, hogy nincsenek-e számos további gombra kell kattintania a a `<li>` címkék, de úgy tűnik, hogy a kattintással megugrását `<li>` címkék.</span><span class="sxs-lookup"><span data-stu-id="84818-276">Not only is this showing that there are many more clicks on the `<li>` tags, but there seems to be a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="84818-277">Majd be, hogy személyek felderített az új `<li>` címkék kattintható, és most vannak törlésével a korábban szerzett feladataival az alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="84818-277">You can then conclude that people have discovered the new `<li>` tags are clickable and are now clearing all their previously-completed tasks in the app.</span></span>

<span data-ttu-id="84818-278">A központi telepítés flighting adatokon alapulnak, dönthet úgy, hogy az üzemi készen áll-e az új felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="84818-278">Based on the data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="84818-279">Nyissa meg az élő: helyezze át az új kódot éles környezetben</span><span class="sxs-lookup"><span data-stu-id="84818-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="84818-280">Most már készen áll a frissítés áthelyezése éles.</span><span class="sxs-lookup"><span data-stu-id="84818-280">You're now ready to move your update to production.</span></span> <span data-ttu-id="84818-281">Mi az az nagy, hogy most már tudja, hogy a frissítés már érvényesítve lett *előtt* üzemi topológiájával.</span><span class="sxs-lookup"><span data-stu-id="84818-281">What's great is that now you know that your update has already been validated *before* it is pushed to production.</span></span> <span data-ttu-id="84818-282">Most magabiztosan telepítheti azt.</span><span class="sxs-lookup"><span data-stu-id="84818-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="84818-283">Az AngularJS ügyfélalkalmazás végzett frissítés, mert az ügyféloldali kódot csak érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="84818-283">Since you made an update to the AngularJS client app, you only validated the client-side code.</span></span> <span data-ttu-id="84818-284">Ha módosítja a háttér-webes API-alkalmazás, tudta érvényesíteni a módosításokat, hasonlóképpen és könnyen.</span><span class="sxs-lookup"><span data-stu-id="84818-284">If you were to make changes to the back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="84818-285">A Git rendszerhéj távolítsa el a forgalom-útválasztási szabály a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="84818-285">In Git Shell, remove the traffic routing rule by running the following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="84818-286">A Git-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="84818-286">Run the Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="84818-287">Várjon néhány percet, amíg az új kódot az előkészítési pont üzembe helyezni, majd indítsa el a http://ToDoApp*&lt;your_suffix >*-ellenőrzése, hogy az új frissítés az átmeneti helyet a tárolóhelyspecifikus staging.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="84818-287">Wait for a few minutes for the new code to be deployed to the staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net to verify that the new update is warmed up in the staging slot.</span></span> <span data-ttu-id="84818-288">Ne feledje, hogy a az elágazáshoz főághoz kapcsolódik az alkalmazás az átmeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="84818-288">Remember that the your fork's master branch is linked to the staging slot of your app.</span></span>
4. <span data-ttu-id="84818-289">Most felcserélni az átmeneti éles környezetben</span><span class="sxs-lookup"><span data-stu-id="84818-289">Now, swap the staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="84818-290">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="84818-290">Summary</span></span>
<span data-ttu-id="84818-291">Az Azure App Service megkönnyíti a kis - és közepes méretű vállalkozások számára ügyfélkapcsolati alkalmazások tesztelése éles üzemben, valami által hagyományosan elvégzett a nagyvállalatokhoz.</span><span class="sxs-lookup"><span data-stu-id="84818-291">Azure App Service makes it easy for small- to medium-sized businesses to test their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="84818-292">Remélhetőleg Ez az oktatóanyag adott Önnek kell összefogni App Service és az Application Insights, hogy a lehetséges flighting telepítési, és akár egyéb tesztelése az éles forgatókönyvek a DevOps világ Tudásbázis.</span><span class="sxs-lookup"><span data-stu-id="84818-292">Hopefully, this tutorial has given you the knowledge you need to bring together App Service and Application Insights to make possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="84818-293">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="84818-293">More resources</span></span>
* [<span data-ttu-id="84818-294">Agilis szoftverfejlesztői az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="84818-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="84818-295">Átmeneti környezet az Azure App Service web Apps beállítása</span><span class="sxs-lookup"><span data-stu-id="84818-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="84818-296">Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="84818-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="84818-297">Az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="84818-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="84818-298">JSONLint - a JSON-érvényesítő</span><span class="sxs-lookup"><span data-stu-id="84818-298">JSONLint - The JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="84818-299">Git ugorhat – alapvető ugorhat és egyesítése</span><span class="sxs-lookup"><span data-stu-id="84818-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="84818-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="84818-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="84818-301">Projekt Kudu Wiki</span><span class="sxs-lookup"><span data-stu-id="84818-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
