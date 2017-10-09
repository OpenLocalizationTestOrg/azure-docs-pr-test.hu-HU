---
title: "aaaFlighting deployment (bétatesztelés) az Azure App Service-ben"
description: "Ismerje meg, hogyan tooflight új szolgáltatásokat az alkalmazásban és a béta a frissítések tesztelése az végpont oktatóanyag. Összegyűjti az App Service szolgáltatások, mint a folyamatos közzététel, tárhelyek, a forgalom útválasztásához és az Application Insights-integráció."
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
ms.openlocfilehash: e83477b1fe46be09e5baa7bc2bd239b840b05cf7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="flighting-deployment-beta-testing-in-azure-app-service"></a><span data-ttu-id="c2e51-104">Flighting deployment (bétatesztelés) az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="c2e51-104">Flighting deployment (beta testing) in Azure App Service</span></span>
<span data-ttu-id="c2e51-105">Az oktatóanyag bemutatja, hogyan toodo *flighting központi telepítések* integrálásával hello különböző lehetőségeinek [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) és [Azure Application Insights](/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="c2e51-105">This tutorial shows you how toodo *flighting deployments* by integrating hello various capabilities of [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) and [Azure Application Insights](/services/application-insights/).</span></span>

<span data-ttu-id="c2e51-106">*Flighting* egy folyamatot, amely egy új szolgáltatás, vagy módosítsa a valódi felhasználók korlátozott számú ellenőrzi, és egy fő tesztelése éles telepítési forgatókönyvhöz.</span><span class="sxs-lookup"><span data-stu-id="c2e51-106">*Flighting* is a deployment process that validates a new feature or change with a limited number of real customers, and is a major testing in production scenario.</span></span> <span data-ttu-id="c2e51-107">Akin toobeta tesztelése, és mint "ellenőrzött teszt repülési" néven is ismert.</span><span class="sxs-lookup"><span data-stu-id="c2e51-107">It is akin toobeta testing and is sometimes known as "controlled test flight".</span></span> <span data-ttu-id="c2e51-108">Egy webes jelenlét sok nagy vállalatok ezt a módszert használja az app frissítéseken a gyakorlatban a korai érvényesítési eléréséhez [gyors fejlesztési](https://en.wikipedia.org/wiki/Agile_software_development).</span><span class="sxs-lookup"><span data-stu-id="c2e51-108">Many large enterprises with a web presence use this approach to get early validation on their app updates in their practice of [agile development](https://en.wikipedia.org/wiki/Agile_software_development).</span></span> <span data-ttu-id="c2e51-109">Az Azure App Service lehetővé teszi toointegrate teszt termelési környezetben, a folyamatos közzététel, és az Application Insights tooimplement hello azonos DevOps-forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="c2e51-109">Azure App Service enables you toointegrate test in production with continous publishing and Application Insights tooimplement hello same DevOps scenario.</span></span> <span data-ttu-id="c2e51-110">Ez a módszer előnyei a következők:</span><span class="sxs-lookup"><span data-stu-id="c2e51-110">Benefits of this approach include:</span></span>

* <span data-ttu-id="c2e51-111">**Valós visszajelzés szerezhet *előtt* frissítései kiadott tooproduction** -hello csak jobb, mint való visszajelzést, amint felszabadít dolog van való visszajelzést, engedélyezés előtt.</span><span class="sxs-lookup"><span data-stu-id="c2e51-111">**Gain real feedback *before* updates are released tooproduction** - hello only thing better than gaining feedback as soon as you release is gaining feedback before you release.</span></span> <span data-ttu-id="c2e51-112">A felhasználó forgalmi és viselkedéshez frissítések korai biztosítják a hello életciklusa tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="c2e51-112">You can test updates with real user traffic and behaviors as early as you desire in hello product life cycle.</span></span>
* <span data-ttu-id="c2e51-113">**Javíthatja [folyamatos teszt adatvezérelt fejlesztési (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)**  - teszt termelési környezetben és integrálásával folyamatos integrációt és az Application insights szolgáltatással instrumentation felhasználói érvényesítési korai és automatikusan történik, a termék életciklusának.</span><span class="sxs-lookup"><span data-stu-id="c2e51-113">**Enhance [continuous test-driven development (CTDD)](https://en.wikipedia.org/wiki/Continuous_test-driven_development)** - By integrating test in production with continuous integration and instrumentation with Application Insights, user validation happens early and automatically in your product life cycle.</span></span> <span data-ttu-id="c2e51-114">Ezzel csökkenthető manuális tesztelési végrehajtási idő befektetések.</span><span class="sxs-lookup"><span data-stu-id="c2e51-114">This helps reduce time investments in manual test execution.</span></span>
* <span data-ttu-id="c2e51-115">**Teszt munkafolyamat optimalizálása** -teszt termelési környezetben, a folyamatos figyelési instrumentation automatizálásával potenciálisan végrehajtható tesztek egyetlen folyamatban, a különféle hello céljai például [integrációs](https://en.wikipedia.org/wiki/Integration_testing), [regressziós](https://en.wikipedia.org/wiki/Regression_testing), [használhatóság](https://en.wikipedia.org/wiki/Usability_testing), kisegítő, honosítási, [teljesítmény](https://en.wikipedia.org/wiki/Software_performance_testing), [biztonsági](https://en.wikipedia.org/wiki/Security_testing), és [ elfogadási](https://en.wikipedia.org/wiki/Acceptance_testing).</span><span class="sxs-lookup"><span data-stu-id="c2e51-115">**Optimize test workflow** - By automating test in production with continuous monitoring instrumentation, you can potentially accomplish hello goals of various kinds of tests in a single process, such as [integration](https://en.wikipedia.org/wiki/Integration_testing), [regression](https://en.wikipedia.org/wiki/Regression_testing), [usability](https://en.wikipedia.org/wiki/Usability_testing), accessibility, localization, [performance](https://en.wikipedia.org/wiki/Software_performance_testing), [security](https://en.wikipedia.org/wiki/Security_testing), and [acceptance](https://en.wikipedia.org/wiki/Acceptance_testing).</span></span>

<span data-ttu-id="c2e51-116">Egy flighting telepítési szinte nem útválasztási élő forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c2e51-116">A flighting deployment is not just about routing live traffic.</span></span> <span data-ttu-id="c2e51-117">Ilyen elrendezés kívánt toogain betekintést lehető leggyorsabban tegye, hogy egy nem várt hiba, a teljesítménycsökkenés, a felhasználói élmény problémák.</span><span class="sxs-lookup"><span data-stu-id="c2e51-117">In such a deployment you want toogain insight as quickly as possible, whether it be an unexpected bug, performance degradation, user experience issues.</span></span> <span data-ttu-id="c2e51-118">Ne feledje, hogy valós ügyfelek foglalkoznak.</span><span class="sxs-lookup"><span data-stu-id="c2e51-118">Remember, you are dealing with real customers.</span></span> <span data-ttu-id="c2e51-119">Igen toodo, jobbra, meg kell győződnie arról, hogy meg van adva a flighting telepítési toogather összes hello adatokat meg kell a rendezés toomake tájékozott döntést a következő lépéshez.</span><span class="sxs-lookup"><span data-stu-id="c2e51-119">So toodo it right, you must make sure that you have set up your flighting deployment toogather all hello data you need in order toomake an informed decision for your next step.</span></span> <span data-ttu-id="c2e51-120">Ez az oktatóanyag toocollect adatok az Application Insights, de új New Relic vagy egyéb technológiák a forgatókönyvéhez leginkább megfelelő használatát.</span><span class="sxs-lookup"><span data-stu-id="c2e51-120">This tutorial shows you how toocollect data with Application Insights, but you can use New Relic or other technologies that suits your scenario.</span></span>

## <a name="what-you-will-do"></a><span data-ttu-id="c2e51-121">Mit fog</span><span class="sxs-lookup"><span data-stu-id="c2e51-121">What you will do</span></span>
<span data-ttu-id="c2e51-122">Ebből az oktatóanyagból megtudhatja, hogyan toobring App Service-alkalmazás, éles környezetben a következő forgatókönyvek együtt tootest hello:</span><span class="sxs-lookup"><span data-stu-id="c2e51-122">In this tutorial, you will learn how toobring hello following scenarios together tootest your App Service app in production:</span></span>

* <span data-ttu-id="c2e51-123">[Éles forgalmat](app-service-web-test-in-production-get-start.md) tooyour beta alkalmazás</span><span class="sxs-lookup"><span data-stu-id="c2e51-123">[Route production traffic](app-service-web-test-in-production-get-start.md) tooyour beta app</span></span>
* <span data-ttu-id="c2e51-124">[Az alkalmazás állíthatnak](../application-insights/app-insights-web-track-usage.md) tooobtain hasznos metrikákat</span><span class="sxs-lookup"><span data-stu-id="c2e51-124">[Instrument your app](../application-insights/app-insights-web-track-usage.md) tooobtain useful metrics</span></span>
* <span data-ttu-id="c2e51-125">Folyamatosan a béta-alkalmazás telepítése, és nyomon követheti az élő app metrikák</span><span class="sxs-lookup"><span data-stu-id="c2e51-125">Continuously deploy your beta app and track live app metrics</span></span>
* <span data-ttu-id="c2e51-126">Hasonlítsa össze metrikák hello éles alkalmazások és hello beta app toosee hogyan változik a kód között tooresults fordítása</span><span class="sxs-lookup"><span data-stu-id="c2e51-126">Compare metrics between hello production app and hello beta app toosee how code changes translate tooresults</span></span>

## <a name="what-you-will-need"></a><span data-ttu-id="c2e51-127">Mit kell</span><span class="sxs-lookup"><span data-stu-id="c2e51-127">What you will need</span></span>
* <span data-ttu-id="c2e51-128">Az Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="c2e51-128">An Azure account</span></span>
* <span data-ttu-id="c2e51-129">A [GitHub](https://github.com/) fiók</span><span class="sxs-lookup"><span data-stu-id="c2e51-129">A [GitHub](https://github.com/) account</span></span>
* <span data-ttu-id="c2e51-130">A Visual Studio 2015 - letöltheti a hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span><span class="sxs-lookup"><span data-stu-id="c2e51-130">Visual Studio 2015 - you can download hello [Community edition](https://www.visualstudio.com/en-us/products/visual-studio-express-vs.aspx).</span></span>
* <span data-ttu-id="c2e51-131">Git rendszerhéj (telepített [GitHub for Windows](https://windows.github.com/)) – Ez lehetővé teszi, hogy Ön toorun hello mindkét hello a Git szoftver, PowerShell parancsok ugyanabban a munkamenetben</span><span class="sxs-lookup"><span data-stu-id="c2e51-131">Git Shell (installed with [GitHub for Windows](https://windows.github.com/)) - this enables you toorun both hello Git and PowerShell commands in hello same session</span></span>
* <span data-ttu-id="c2e51-132">Legújabb [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span><span class="sxs-lookup"><span data-stu-id="c2e51-132">Latest [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/download/v0.9.8-September2015/azure-powershell.0.9.8.msi) bits</span></span>
* <span data-ttu-id="c2e51-133">Hello következő alapvető ismeretekkel:</span><span class="sxs-lookup"><span data-stu-id="c2e51-133">Basic understanding of hello following:</span></span>
  * <span data-ttu-id="c2e51-134">[Az Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) sablon-üzembehelyezés (lásd: [kiszámítható módon tudja az Azure-ban összetett alkalmazást központilag](app-service-deploy-complex-application-predictably.md))</span><span class="sxs-lookup"><span data-stu-id="c2e51-134">[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) template deployment (see [Deploy a complex application predictably in Azure](app-service-deploy-complex-application-predictably.md))</span></span>
  * [<span data-ttu-id="c2e51-135">Git</span><span class="sxs-lookup"><span data-stu-id="c2e51-135">Git</span></span>](http://git-scm.com/documentation)
  * [<span data-ttu-id="c2e51-136">PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2e51-136">PowerShell</span></span>](https://technet.microsoft.com/library/bb978526.aspx)

> [!NOTE]
> <span data-ttu-id="c2e51-137">Ez az oktatóanyag kell egy Azure-fiók toocomplete:</span><span class="sxs-lookup"><span data-stu-id="c2e51-137">You need an Azure account toocomplete this tutorial:</span></span>
>
> * <span data-ttu-id="c2e51-138">Is [szabad nyissa meg az Azure-fiók](https://azure.microsoft.com/pricing/free-trial/) -jóváírásokat kap használhatja ki tootry fizetős Azure-szolgáltatásokkal, és még azok lejárta után hálózati adaptere esetében megtarthatja hello fiókot és használhatja az ingyenes Azure-szolgáltatások, például a Web Apps.</span><span class="sxs-lookup"><span data-stu-id="c2e51-138">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/) - You get credits you can use tootry out paid Azure services, and even after they're used up you can keep hello account and use free Azure services, such as Web Apps.</span></span>
> * <span data-ttu-id="c2e51-139">Is [aktiválhatja a Visual Studio előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) -a Visual Studio-előfizetéssel biztosít Önnek krediteket minden hónapban, fizetős Azure-szolgáltatásokat is használhat.</span><span class="sxs-lookup"><span data-stu-id="c2e51-139">You can [activate Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) - Your Visual Studio subscription gives you credits every month that you can use for paid Azure services.</span></span>
>
> <span data-ttu-id="c2e51-140">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="c2e51-140">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="c2e51-141">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="c2e51-141">No credit cards required; no commitments.</span></span>
>
>

## <a name="set-up-your-production-web-app"></a><span data-ttu-id="c2e51-142">Az éles webes alkalmazás beállítása</span><span class="sxs-lookup"><span data-stu-id="c2e51-142">Set up your production web app</span></span>
> [!NOTE]
> <span data-ttu-id="c2e51-143">Ebben az oktatóanyagban használt hello parancsfájl automatikusan konfigurálja a GitHub-tárház folyamatos közzététel.</span><span class="sxs-lookup"><span data-stu-id="c2e51-143">hello script used in this tutorial will automatically configure continuous publishing from your GitHub repository.</span></span> <span data-ttu-id="c2e51-144">Ehhez az szükséges, hogy a Githubon tartozó hitelesítő adatok már szerepelnek Azure, ellenkező esetben a hello parancsprogram-telepítés sikertelen lesz-e tooconfigure verziókövetési beállítások hello web Apps megkísérlése során.</span><span class="sxs-lookup"><span data-stu-id="c2e51-144">This requires that your GitHub credentials are already stored in Azure, otherwise hello scripted deployment will fail when attempting tooconfigure source control settings for hello web apps.</span></span>
>
> <span data-ttu-id="c2e51-145">a GitHub-felhasználó hitelesítő adatait az Azure toostore egy webalkalmazás létrehozása az hello [Azure Portal](https://portal.azure.com/) és [GitHub-KözpontiTelepítés konfigurálása](app-service-continuous-deployment.md).</span><span class="sxs-lookup"><span data-stu-id="c2e51-145">toostore your GitHub credentials in Azure, create a web app in hello [Azure Portal](https://portal.azure.com/) and [configure GitHub deployment](app-service-continuous-deployment.md).</span></span> <span data-ttu-id="c2e51-146">Csak akkor kell toodo ezen egyszer.</span><span class="sxs-lookup"><span data-stu-id="c2e51-146">You only need toodo this once.</span></span>
>
>

<span data-ttu-id="c2e51-147">Egy tipikus DevOps forgatókönyv valamely alkalmazás az élő Azure-ban futó, és azt szeretné, hogy toomake módosítások tooit folyamatos közzétételen keresztül;.</span><span class="sxs-lookup"><span data-stu-id="c2e51-147">In a typical DevOps scenario, you have an application that’s running live in Azure, and you want toomake changes tooit through continuous publishing.</span></span> <span data-ttu-id="c2e51-148">Ebben a forgatókönyvben a tooproduction egy sablon, amelyet kifejlesztett és tesztelt telepíti.</span><span class="sxs-lookup"><span data-stu-id="c2e51-148">In this scenario, you will deploy tooproduction a template that you have developed and tested.</span></span>

1. <span data-ttu-id="c2e51-149">Hozzon létre saját elágazás a hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) tárházba.</span><span class="sxs-lookup"><span data-stu-id="c2e51-149">Create your own fork of hello [ToDoApp](https://github.com/azure-appservice-samples/ToDoApp) repository.</span></span> <span data-ttu-id="c2e51-150">Létrehozásával kapcsolatos információkat az elágazáshoz, tekintse meg a [oszthatja ketté a tárházban](https://help.github.com/articles/fork-a-repo/).</span><span class="sxs-lookup"><span data-stu-id="c2e51-150">For information on creating your fork, see [Fork a Repo](https://help.github.com/articles/fork-a-repo/).</span></span> <span data-ttu-id="c2e51-151">Ha létrejött az elágazáshoz, tekintheti meg a böngészőben.</span><span class="sxs-lookup"><span data-stu-id="c2e51-151">Once your fork is created, you can see it in your browser.</span></span>

   ![](./media/app-service-agile-software-development/production-1-private-repo.png)
2. <span data-ttu-id="c2e51-152">Nyisson meg egy Git rendszerhéj-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="c2e51-152">Open a Git Shell session.</span></span> <span data-ttu-id="c2e51-153">Ha a Git rendszerhéj még nincs, telepítse [GitHub for Windows](https://windows.github.com/) most.</span><span class="sxs-lookup"><span data-stu-id="c2e51-153">If you don't have Git Shell yet, install [GitHub for Windows](https://windows.github.com/) now.</span></span>
3. <span data-ttu-id="c2e51-154">Az elágazáshoz, helyi másolat létrehozása a következő hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c2e51-154">Create a local clone of your fork by executing hello following command:</span></span>

     <span data-ttu-id="c2e51-155">git-klón https://github.com/<your_fork>/ToDoApp.git</span><span class="sxs-lookup"><span data-stu-id="c2e51-155">git clone https://github.com/<your_fork>/ToDoApp.git</span></span>
4. <span data-ttu-id="c2e51-156">Ha elvégezte a helyi klónozás, keresse meg túl*&lt;repository_root >*\ARMTemplates és futtatási hello deploy.ps1 egyedi utótagot kapjon, ahogy az alábbi parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="c2e51-156">Once you have your local clone, navigate too*&lt;repository_root>*\ARMTemplates, and run hello deploy.ps1 script with a unique suffix, as shown below:</span></span>

     <span data-ttu-id="c2e51-157">.\deploy.ps1 – RepoUrl https://github.com/<your_fork>/todoapp.git - ResourceGroupSuffix < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="c2e51-157">.\deploy.ps1 –RepoUrl https://github.com/<your_fork>/todoapp.git -ResourceGroupSuffix <your_suffix></span></span>
5. <span data-ttu-id="c2e51-158">Amikor a rendszer kéri, írja be hello szükséges felhasználónevet és jelszót az adatbázis eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="c2e51-158">When prompted, type in hello desired username and password for database access.</span></span> <span data-ttu-id="c2e51-159">Ne feledje, az adatbázis hitelesítő adatait, mert toospecify kell őket ismét amikor frissítése hello erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="c2e51-159">Remember your database credentials because you will need toospecify them again when updating hello resource group.</span></span>

   <span data-ttu-id="c2e51-160">Meg kell jelennie a jogosultságkiosztás folyamata különböző Azure-erőforrások hello.</span><span class="sxs-lookup"><span data-stu-id="c2e51-160">You should see hello provisioning progress of various Azure resources.</span></span> <span data-ttu-id="c2e51-161">Központi telepítés befejezése után hello parancsfájl hello böngészőben hello alkalmazást, és adjon meg egy rövid hangjelzés.</span><span class="sxs-lookup"><span data-stu-id="c2e51-161">When deployment completes, hello script will launch hello application in hello browser and give you a friendly beep.</span></span>
   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.1-app-in-browser.png)
6. <span data-ttu-id="c2e51-162">Vissza a Git rendszerhéj-munkamenetben futtassa:</span><span class="sxs-lookup"><span data-stu-id="c2e51-162">Back in your Git Shell session, run:</span></span>

     <span data-ttu-id="c2e51-163">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="c2e51-163">.\swap –Name ToDoApp<your_suffix></span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.2-swap-to-production.png)
7. <span data-ttu-id="c2e51-164">Hello parancsfájl befejezése után térjen vissza toobrowse toohello előtér-cím (http://ToDoApp*&lt;your_suffix >*.azurewebsites.net/) toosee hello alkalmazást éles környezetben.</span><span class="sxs-lookup"><span data-stu-id="c2e51-164">When hello script finishes, go back toobrowse toohello frontend’s address (http://ToDoApp*&lt;your_suffix>*.azurewebsites.net/) toosee hello application running in production.</span></span>
8. <span data-ttu-id="c2e51-165">Jelentkezzen be a hello [Azure Portal](https://portal.azure.com/) és vessen egy pillantást mi jön létre.</span><span class="sxs-lookup"><span data-stu-id="c2e51-165">Log into hello [Azure Portal](https://portal.azure.com/) and take a look at what’s created.</span></span>

   <span data-ttu-id="c2e51-166">Meg kell tudni toosee két webes alkalmazások hello ugyanabban az erőforráscsoportban, amelyen a hello `Api` utótag hello nevében.</span><span class="sxs-lookup"><span data-stu-id="c2e51-166">You should be able toosee two web apps in hello same resource group, one with hello `Api` suffix in hello name.</span></span> <span data-ttu-id="c2e51-167">Hello erőforráscsoport nézet tekinti meg, ha hello SQL-adatbázis és a kiszolgáló, a hello App Service-csomag és a hello átmeneti üzembe helyezési ponti hello web Apps is láthat.</span><span class="sxs-lookup"><span data-stu-id="c2e51-167">If you look at hello resource group view, you will also see hello SQL Database and server, hello App Service plan, and hello staging slots for hello web apps.</span></span> <span data-ttu-id="c2e51-168">Tallózzon a hello különböző erőforrások, és hasonlítsa össze azokat a  *&lt;repository_root >*\ARMTemplates\ProdAndStage.json toosee konfigurációjuktól hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="c2e51-168">Browse through hello different resources and compare them with *&lt;repository_root>*\ARMTemplates\ProdAndStage.json toosee how they are configured in hello template.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/00.3-resource-group-view.png)

<span data-ttu-id="c2e51-169">Hello éles alkalmazások beállítása.</span><span class="sxs-lookup"><span data-stu-id="c2e51-169">You have set up hello production app.</span></span>  <span data-ttu-id="c2e51-170">Most tegyük képzelhető el, hogy megjelenik-e visszajelzést, hogy gyenge hello alkalmazáshoz-e használhatóság.</span><span class="sxs-lookup"><span data-stu-id="c2e51-170">Now, let's imagine that you receive feedback that usability is poor for hello app.</span></span> <span data-ttu-id="c2e51-171">Ezért úgy dönt, hogy tooinvestigate.</span><span class="sxs-lookup"><span data-stu-id="c2e51-171">So you decide tooinvestigate.</span></span> <span data-ttu-id="c2e51-172">Fog tooinstrument az alkalmazás toogive Ön visszajelzését.</span><span class="sxs-lookup"><span data-stu-id="c2e51-172">You're going tooinstrument your app toogive you feedback.</span></span>

## <a name="investigate-instrument-your-client-app-for-monitoringmetrics"></a><span data-ttu-id="c2e51-173">Vizsgálja meg: Állíthatnak be az ügyfél alkalmazás figyelési/metrikák</span><span class="sxs-lookup"><span data-stu-id="c2e51-173">Investigate: Instrument your client app for monitoring/metrics</span></span>
1. <span data-ttu-id="c2e51-174">Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo.sln a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="c2e51-174">Open *&lt;repository_root>*\src\MultiChannelToDo.sln in Visual Studio.</span></span>
2. <span data-ttu-id="c2e51-175">Kattintson a jobb gombbal a megoldás Nuget-csomagok visszaállítása > **NuGet-csomagok kezelése megoldáshoz** > **visszaállítása**.</span><span class="sxs-lookup"><span data-stu-id="c2e51-175">Restore all Nuget packages by right-clicking solution > **Manage NuGet Packages for Solution** > **Restore**.</span></span>
3. <span data-ttu-id="c2e51-176">Kattintson a jobb gombbal **MultiChannelToDo.Web** > **Application Insights Telemetria** > **beállításainak konfigurálása** > erőforrás-csoport módosítása tooToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása tooProject**.</span><span class="sxs-lookup"><span data-stu-id="c2e51-176">Right-click **MultiChannelToDo.Web** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
4. <span data-ttu-id="c2e51-177">Hello Azure portál, nyissa meg hello hello paneljén **MultiChannelToDo.Web** Application Insights-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-177">In hello Azure Portal, open hello blade for hello **MultiChannelToDo.Web** Application Insight resource.</span></span> <span data-ttu-id="c2e51-178">Ezt a hello **alkalmazás állapotának** része, kattintson a **megtudhatja, hogyan toocollect böngésző betöltési adatok** > kód másolása.</span><span class="sxs-lookup"><span data-stu-id="c2e51-178">Then in hello **Application health** part, click **Learn how toocollect browser page load data** > copy code.</span></span>
5. <span data-ttu-id="c2e51-179">Adja hozzá a hello másolta JS instrumentation túl*&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml hello bezárása előtt `<heading>` címke.</span><span class="sxs-lookup"><span data-stu-id="c2e51-179">Add hello copied JS instrumentation code too*&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, just before hello closing `<heading>` tag.</span></span> <span data-ttu-id="c2e51-180">Az Application Insights-erőforrás hello instrumentation egyedi kulcsot kell tartalmaznia.</span><span class="sxs-lookup"><span data-stu-id="c2e51-180">It should contain hello unique instrumentation key of your Application Insight resource.</span></span>

        <script type="text/javascript">
        var appInsights=window.appInsights||function(config){
            function s(config){t[config]=function(){var i=arguments;t.queue.push(function(){t[config].apply(t,i)})}}var t={config:config},r=document,f=window,e="script",o=r.createElement(e),i,u;for(o.src=config.url||"//az416426.vo.msecnd.net/scripts/a/ai.0.js",r.getElementsByTagName(e)[0].parentNode.appendChild(o),t.cookie=r.cookie,t.queue=[],i=["Event","Exception","Metric","PageView","Trace"];i.length;)s("track"+i.pop());return config.disableExceptionTracking||(i="onerror",s("_"+i),u=f[i],f[i]=function(config,r,f,e,o){var s=u&&u(config,r,f,e,o);return s!==!0&&t["_"+i](config,r,f,e,o),s}),t
        }({
            instrumentationKey:"<your_unique_instrumentation_key>"
        });

        window.appInsights=appInsights;
        appInsights.trackPageView();
        </script>
6. <span data-ttu-id="c2e51-181">A kattintások tooApplication Insights az egyéni események küldése adja hozzá a következő kód toohello alsó törzse hello:</span><span class="sxs-lookup"><span data-stu-id="c2e51-181">Send custom events tooApplication Insights for mouse clicks by adding hello following code toohello bottom of body:</span></span>

       <script>
           $(document.body).find("*").click(function(event) {

               appInsights.trackEvent(event.target.tagName + ": " + event.target.className);
           });
       </script>

   <span data-ttu-id="c2e51-182">A JavaScript részlet küld egy egyéni esemény tooApplication Insights minden alkalommal, amikor a felhasználó kattint bárhol hello web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c2e51-182">This JavaScript snippet sends a custom event tooApplication Insights every time a user clicks anywhere in hello web app.</span></span>
7. <span data-ttu-id="c2e51-183">Git rendszerhéj véglegesítse, és küldje le a változások tooyour elágazás a Githubon.</span><span class="sxs-lookup"><span data-stu-id="c2e51-183">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="c2e51-184">Várja meg, majd az ügyfelek toorefresh böngésző.</span><span class="sxs-lookup"><span data-stu-id="c2e51-184">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for client app"
       git push origin master
8. <span data-ttu-id="c2e51-185">Lapozófájl-kapacitás hello telepített alkalmazás módosítások tooproduction:</span><span class="sxs-lookup"><span data-stu-id="c2e51-185">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="c2e51-186">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="c2e51-186">.\swap –Name ToDoApp<your_suffix></span></span>
9. <span data-ttu-id="c2e51-187">Keresse meg a konfigurált toohello Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c2e51-187">Browse toohello Application Insights resource that you configured.</span></span> <span data-ttu-id="c2e51-188">Kattintson az egyéni események.</span><span class="sxs-lookup"><span data-stu-id="c2e51-188">Click Custom events.</span></span>

   ![](./media/app-service-web-test-in-production-controlled-test-flight/01-custom-events.png)

   <span data-ttu-id="c2e51-189">Ha nem látja az egyéni események metrikákat, várjon néhány percet, és kattintson **frissítése**.</span><span class="sxs-lookup"><span data-stu-id="c2e51-189">If you don't see metrics for custom events, wait a few minutes and click **Refresh**.</span></span>

<span data-ttu-id="c2e51-190">Tegyük fel, hogy látja, a diagram például alatt:</span><span class="sxs-lookup"><span data-stu-id="c2e51-190">Suppose you see a chart like below:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/02-custom-events-chart-view.png)

<span data-ttu-id="c2e51-191">És hello esemény rács alá:</span><span class="sxs-lookup"><span data-stu-id="c2e51-191">And hello event grid below it:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/03-custom-event-grid-view.png)

<span data-ttu-id="c2e51-192">Hello tooyour ToDoApp alkalmazáskód szerint, **GOMB** esemény megfelel toohello Küldés gomb és hello **bemeneti** esemény toohello szövegmező felel meg.</span><span class="sxs-lookup"><span data-stu-id="c2e51-192">According tooyour ToDoApp application code, hello **BUTTON** event corresponds toohello submit button, and hello **INPUT** event corresponds toohello textbox.</span></span> <span data-ttu-id="c2e51-193">Az eddigi dolgot értelme.</span><span class="sxs-lookup"><span data-stu-id="c2e51-193">So far, things make sense.</span></span> <span data-ttu-id="c2e51-194">Azonban úgy tűnik, a megfelelő mennyiségű kattintások és nagyon mindössze néhány kattintással a Tennivalólista elemein hello (hello **LI** események).</span><span class="sxs-lookup"><span data-stu-id="c2e51-194">However, it looks like there's a good amount of clicks and very few clicks on hello to-do items (hello **LI** events).</span></span>

<span data-ttu-id="c2e51-195">Alapú az űrlapon, akkor a alapul, hogy egyes felhasználók nem biztos, mely része hello felhasználói felület kattintható és mivel az a kijelölt szöveg stílussal hello kurzor, amikor az hello listaelemek és annak ikonja felett áll.</span><span class="sxs-lookup"><span data-stu-id="c2e51-195">Based on this, you form your hypothesis that some users are confused which part of hello UI is clickable and it is because hello cursor is styled for text selection when it hovers on hello list items and their icons.</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/04-to-do-list-item-ui.png)

<span data-ttu-id="c2e51-196">Ez lehet egy contrived példa.</span><span class="sxs-lookup"><span data-stu-id="c2e51-196">This might be a contrived example.</span></span> <span data-ttu-id="c2e51-197">Ettől függetlenül folyamatos toomake fokozása tooyour alkalmazást, és végezze el a központi telepítési tooget flighting használhatóság visszajelzés élő ügyfelek.</span><span class="sxs-lookup"><span data-stu-id="c2e51-197">Nevertheless, you're going toomake an improvement tooyour app, and then perform a flighting deployment tooget usability feedback from live customers.</span></span>

### <a name="instrument-your-server-app-for-monitoringmetrics"></a><span data-ttu-id="c2e51-198">A kiszolgáló alkalmazás figyelési/metrikáihoz állíthatnak</span><span class="sxs-lookup"><span data-stu-id="c2e51-198">Instrument your server app for monitoring/metrics</span></span>
<span data-ttu-id="c2e51-199">Egy érintő Ez azért, mert az oktatóanyag egy hello forgatókönyv csak hello ügyfélalkalmazás foglalkozik.</span><span class="sxs-lookup"><span data-stu-id="c2e51-199">This is a tangent since hello scenario demonstrated in this tutorial only deals with hello client app.</span></span> <span data-ttu-id="c2e51-200">Azonban a teljesség beállíthat hello kiszolgálóoldali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-200">However, for completeness you will set up hello server-side app.</span></span>

1. <span data-ttu-id="c2e51-201">Kattintson a jobb gombbal **MultiChannelToDo** > **Application Insights Telemetria** > **beállításainak konfigurálása** > erőforrás-csoport módosítása tooToDoApp*&lt;your_suffix >* > **Application Insights hozzáadása tooProject**.</span><span class="sxs-lookup"><span data-stu-id="c2e51-201">Right-click **MultiChannelToDo** > **Add Application Insights Telemetry** > **Configure Settings** > Change resource group tooToDoApp*&lt;your_suffix>* > **Add Application Insights tooProject**.</span></span>
2. <span data-ttu-id="c2e51-202">Git rendszerhéj véglegesítse, és küldje le a változások tooyour elágazás a Githubon.</span><span class="sxs-lookup"><span data-stu-id="c2e51-202">In Git Shell, commit and push your changes tooyour fork in GitHub.</span></span> <span data-ttu-id="c2e51-203">Várja meg, majd az ügyfelek toorefresh böngésző.</span><span class="sxs-lookup"><span data-stu-id="c2e51-203">Then, wait for clients toorefresh browser.</span></span>

       git add -A :/
       git commit -m "add AI configuration for server app"
       git push origin master
3. <span data-ttu-id="c2e51-204">Lapozófájl-kapacitás hello telepített alkalmazás módosítások tooproduction:</span><span class="sxs-lookup"><span data-stu-id="c2e51-204">Swap hello deployed app changes tooproduction:</span></span>

     <span data-ttu-id="c2e51-205">. \swap – name ToDoApp < your_suffix ></span><span class="sxs-lookup"><span data-stu-id="c2e51-205">.\swap –Name ToDoApp<your_suffix></span></span>

<span data-ttu-id="c2e51-206">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="c2e51-206">That's it!</span></span>

## <a name="investigate-add-slot-specific-tags-tooyour-client-app-metrics"></a><span data-ttu-id="c2e51-207">Vizsgálata: Tárolóhely-specifikus címkék tooyour ügyfél app metrikák hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2e51-207">Investigate: Add slot-specific tags tooyour client app metrics</span></span>
<span data-ttu-id="c2e51-208">Ebben a szakaszban hello különböző telepítési üzembe helyezési ponti toosend tárolóhely-specifikus telemetriai toohello konfigurálja ugyanazon Application Insights-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="c2e51-208">In this section, you will configure hello different deployment slots toosend slot-specific telemetry toohello same Application Insights resource.</span></span> <span data-ttu-id="c2e51-209">Ezzel a módszerrel összehasonlíthatja a telemetriai adatok között különböző üzembe helyezési ponti (telepítési környezetekben) tooeasily forgalmát, lásd: hello hatását, hogy az alkalmazások változásairól.</span><span class="sxs-lookup"><span data-stu-id="c2e51-209">This way, you can compare telemetry data between traffic from different slots (deployment environments) tooeasily see hello effect of your app changes.</span></span> <span data-ttu-id="c2e51-210">At hello azonos időben, elkülönítheti hello éles forgalom a hello rest így is toomonitor az éles alkalmazás igény szerint.</span><span class="sxs-lookup"><span data-stu-id="c2e51-210">At hello same time, you can separate hello production traffic from hello rest so you can continue toomonitor your production app as needed.</span></span>

<span data-ttu-id="c2e51-211">Az ügyfél viselkedése az adatgyűjtést van, akkor lesz [adja hozzá a telemetriai adatok inicializáló tooyour JavaScript-kód](../application-insights/app-insights-api-filtering-sampling.md) a index.cshtml.</span><span class="sxs-lookup"><span data-stu-id="c2e51-211">Since you're gathering data on client behavior, you will [add a telemetry initializer tooyour JavaScript code](../application-insights/app-insights-api-filtering-sampling.md) in index.cshtml.</span></span> <span data-ttu-id="c2e51-212">Ha azt szeretné, hogy tootest kiszolgálóoldali teljesítményét, például is elvégezheti hasonlóan a kiszolgáló kódjában található (lásd: [Application Insights API egyéni események és metrikák](../application-insights/app-insights-api-custom-events-metrics.md).</span><span class="sxs-lookup"><span data-stu-id="c2e51-212">If you want tootest server-side performance, for example, you can also do similarly in your server code (see [Application Insights API for custom events and metrics](../application-insights/app-insights-api-custom-events-metrics.md).</span></span>

1. <span data-ttu-id="c2e51-213">Először adja hozzá a hello kód bewteen hello két `//` Megjegyzések alatt hello JavaScript blokkban toohello hozzáadásának `<heading>` korábbi címkét.</span><span class="sxs-lookup"><span data-stu-id="c2e51-213">First, add hello code bewteen hello two `//` comments below in hello JavaScript block that you added toohello `<heading>` tag earlier.</span></span>

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

    <span data-ttu-id="c2e51-214">Az inicializáló hello okozza `appInsights` objektum tooadd hello nevű egyéni tulajdonságot `Environment` tooevery adat, telemetriai adatokat küldi el.</span><span class="sxs-lookup"><span data-stu-id="c2e51-214">This initializer code causes hello `appInsights` object tooadd hello a custom property called `Environment` tooevery piece of telemetry it sends.</span></span>
2. <span data-ttu-id="c2e51-215">Ezután adja hozzá az egyéni tulajdonság egy [beállítás tárolóhely](web-sites-staged-publishing.md#AboutConfiguration) a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="c2e51-215">Next, add this custom property as a [slot setting](web-sites-staged-publishing.md#AboutConfiguration) for your web app in Azure.</span></span> <span data-ttu-id="c2e51-216">toodo, a parancsok a Git rendszerhéj-munkamenetben a következő futtatási hello.</span><span class="sxs-lookup"><span data-stu-id="c2e51-216">toodo this, run hello following commands in your Git Shell session.</span></span>

        $app = Get-AzureWebsite -Name todoapp<your_suffix> -Slot production
        $app.AppSettings.Add("environment", "Production")
        $app.SlotStickyAppSettingNames.Add("environment")
        $app | Set-AzureWebsite -Name todoapp<your_suffix> -Slot production

    <span data-ttu-id="c2e51-217">a projekt Web.config hello már meghatározása hello `environment` Alkalmazásbeállítás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-217">hello Web.config in your project already defines hello `environment` app setting.</span></span> <span data-ttu-id="c2e51-218">Ezzel a beállítással hello alkalmazást helyileg, tesztelésekor a metrikákat címkével fog rendelkezni a `VS Debugger`.</span><span class="sxs-lookup"><span data-stu-id="c2e51-218">With this setting, when you test hello app locally, your metrics will be tagged with `VS Debugger`.</span></span> <span data-ttu-id="c2e51-219">Azonban, amikor a módosítások tooAzure, Azure lesz megkeresheti, hello `environment` alkalmazás hello web app konfigurációban beállítást használja, és a metrikákat címkével rendelkező `Production`.</span><span class="sxs-lookup"><span data-stu-id="c2e51-219">However, when you push your changes tooAzure, Azure will find and use hello `environment` app setting in hello web app's configuration instead, and your metrics will be tagged with `Production`.</span></span>
3. <span data-ttu-id="c2e51-220">Véglegesítse és a kód módosítások tooyour elágazás leküldéses a Githubon, és majd várja meg a felhasználók toouse hello új app (kell toorefresh hello böngésző).</span><span class="sxs-lookup"><span data-stu-id="c2e51-220">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="c2e51-221">Ez körülbelül 15 percet vesz igénybe hello új tulajdonság tooshow az Application insightsban `MultiChannelToDo.Web` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-221">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo.Web` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for client app"
        git push origin master
4. <span data-ttu-id="c2e51-222">Most, lépjen a toohello **egyéni események** újra a panelt és szűrő hello metrikákat a `Environment=Production`.</span><span class="sxs-lookup"><span data-stu-id="c2e51-222">Now, go toohello **Custom events** blade again and filter hello metrics on `Environment=Production`.</span></span> <span data-ttu-id="c2e51-223">Minden hello új egyéni események hello éles környezetben ez a szűrő a tárolóhely képes toosee kell.</span><span class="sxs-lookup"><span data-stu-id="c2e51-223">You should now be able toosee all hello new custom events in hello production slot with this filter.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/05-filter-on-production-environment.png)
5. <span data-ttu-id="c2e51-224">Kattintson a hello **Kedvencek** gomb toosave hello aktuális Metrikaböngésző beállításait toosomething, például **egyéni események: éles**.</span><span class="sxs-lookup"><span data-stu-id="c2e51-224">Click hello **Favorites** button toosave hello current Metrics Explorer settings toosomething like **Custom events: Production**.</span></span> <span data-ttu-id="c2e51-225">Egyszerűen válthat között ez és egy központi telepítési tárolóhely nézetben később.</span><span class="sxs-lookup"><span data-stu-id="c2e51-225">You can easily switch between this view and a deployment slot view later.</span></span>

   > [!TIP]
   > <span data-ttu-id="c2e51-226">Még sokkal hatékonyabb elemzés, érdemes lehet [az Application Insights-erőforrás integrálása a Power BI](../application-insights/app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="c2e51-226">For even more powerful analytics, consider [integrating your Application Insights resource with Power BI](../application-insights/app-insights-export-power-bi.md).</span></span>
   >
   >

### <a name="add-slot-specific-tags-tooyour-server-app-metrics"></a><span data-ttu-id="c2e51-227">Tárolóhely-specifikus címkék tooyour kiszolgálói alkalmazás metrikák hozzáadása</span><span class="sxs-lookup"><span data-stu-id="c2e51-227">Add slot-specific tags tooyour server app metrics</span></span>
<span data-ttu-id="c2e51-228">Ebben az esetben a teljesség beállíthat hello kiszolgálóoldali alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-228">Again, for completeness you will set up hello server-side app.</span></span> <span data-ttu-id="c2e51-229">Hello ügyfélalkalmazást, amely a JavaScript van tagolva, eltérően hello server alkalmazás tárolóhely-specifikus címkék .NET kóddal van tagolva.</span><span class="sxs-lookup"><span data-stu-id="c2e51-229">Unlike hello client app which is instrumented in JavaScript, slot-specific tags for hello server app is instrumented with .NET code.</span></span>

1. <span data-ttu-id="c2e51-230">Nyissa meg  *&lt;repository_root >*\src\MultiChannelToDo\Global.asax.cs.</span><span class="sxs-lookup"><span data-stu-id="c2e51-230">Open *&lt;repository_root>*\src\MultiChannelToDo\Global.asax.cs.</span></span> <span data-ttu-id="c2e51-231">Adja hozzá a hello kódblokkot az alábbi hello névtér kapcsos zárójelet bezárása előtt.</span><span class="sxs-lookup"><span data-stu-id="c2e51-231">Add hello code block below, just before hello closing namespace curly brace.</span></span>

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
2. <span data-ttu-id="c2e51-232">Javítsa ki a hello név feloldása hibákat hello hozzáadásával `using` hello fájl alábbi toohello kezdete utasításokat:</span><span class="sxs-lookup"><span data-stu-id="c2e51-232">Correct hello name resolution errors by adding hello `using` statements below toohello beginning of hello file:</span></span>

        using Microsoft.ApplicationInsights.Channel;
        using Microsoft.ApplicationInsights.Extensibility;
3. <span data-ttu-id="c2e51-233">Adja hozzá hello kódot alább hello toohello elejére `Application_Start()` módszert:</span><span class="sxs-lookup"><span data-stu-id="c2e51-233">Add hello code below toohello beginning of hello `Application_Start()` method:</span></span>

        TelemetryConfiguration.Active.TelemetryInitializers.Add(new ConfigInitializer());
4. <span data-ttu-id="c2e51-234">Véglegesítse és a kód módosítások tooyour elágazás leküldéses a Githubon, és majd várja meg a felhasználók toouse hello új app (kell toorefresh hello böngésző).</span><span class="sxs-lookup"><span data-stu-id="c2e51-234">Commit and push your code changes tooyour fork on GitHub, and then wait for your users toouse hello new app (need toorefresh hello browser).</span></span> <span data-ttu-id="c2e51-235">Ez körülbelül 15 percet vesz igénybe hello új tulajdonság tooshow az Application insightsban `MultiChannelToDo` erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-235">It takes about 15 minutes for hello new property tooshow up in your Application Insights `MultiChannelToDo` resource.</span></span>

        git add -A :/
        git commit -m "add environment property tooAI events for server app"
        git push origin master

## <a name="update-set-up-your-beta-branch"></a><span data-ttu-id="c2e51-236">Frissítés: A béta fiókirodai beállítása</span><span class="sxs-lookup"><span data-stu-id="c2e51-236">Update: Set up your beta branch</span></span>
1. <span data-ttu-id="c2e51-237">Nyissa meg  *&lt;repository_root >*\ARMTemplates\ProdAndStagetest.json és a keresés hello `appsettings` erőforrások (keressen `"name": "appsettings"`).</span><span class="sxs-lookup"><span data-stu-id="c2e51-237">Open *&lt;repository_root>*\ARMTemplates\ProdAndStagetest.json and find hello `appsettings` resources (search for `"name": "appsettings"`).</span></span> <span data-ttu-id="c2e51-238">Nincsenek 4 egyet mindegyik tárolóhely őket.</span><span class="sxs-lookup"><span data-stu-id="c2e51-238">There are 4 of them, one for each slot.</span></span>
2. <span data-ttu-id="c2e51-239">Az egyes `appsettings` erőforrás hozzáadása egy `"environment": "[parameters('slotName')]"` app beállítás toohello vége hello `properties` tömb.</span><span class="sxs-lookup"><span data-stu-id="c2e51-239">For each `appsettings` resource, add an  `"environment": "[parameters('slotName')]"` app setting toohello end of hello `properties` array.</span></span> <span data-ttu-id="c2e51-240">Ne feledje tooend hello előző sor vesszővel válassza el.</span><span class="sxs-lookup"><span data-stu-id="c2e51-240">Don't forget tooend hello previous line with a comma.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/06-arm-app-setting-with-slot-name.png)

    <span data-ttu-id="c2e51-241">Hello hozzáadott `environment` hello üzembe helyezési ponti tooall beállítása hello sablon az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2e51-241">You have just added hello `environment` app setting tooall hello slots in hello template.</span></span>
3. <span data-ttu-id="c2e51-242">A hello ugyanazt a fájlt, megkeresi hello `slotconfignames` erőforrások (keressen `"name": "slotconfignames"`).</span><span class="sxs-lookup"><span data-stu-id="c2e51-242">In hello same file, find hello `slotconfignames` resources (search for `"name": "slotconfignames"`).</span></span> <span data-ttu-id="c2e51-243">Nincsenek 2 valamelyiket, egy, az egyes alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2e51-243">There are 2 of them, one for each app.</span></span>
4. <span data-ttu-id="c2e51-244">Az egyes `slotconfignames` erőforrás hozzáadása `"environment"` hello toohello végét `appSettingNames` tömb.</span><span class="sxs-lookup"><span data-stu-id="c2e51-244">For each `slotconfignames` resource, add `"environment"` toohello end of hello `appSettingNames` array.</span></span> <span data-ttu-id="c2e51-245">Ne feledje tooend hello előző sor vesszővel válassza el.</span><span class="sxs-lookup"><span data-stu-id="c2e51-245">Don't forget tooend hello previous line with a comma.</span></span>

    <span data-ttu-id="c2e51-246">Van szükség hello `environment` app beállítás memóriás tooits megfelelő üzembe helyezési pont mindkét alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2e51-246">You have just made hello `environment` app setting stick tooits respective deployment slot for both apps.</span></span>  
5. <span data-ttu-id="c2e51-247">A Git rendszerhéj-munkamenetben futtassa hello következő parancsok hello azonos erőforrás csoport utótag előtt használt.</span><span class="sxs-lookup"><span data-stu-id="c2e51-247">In your Git Shell session, run hello following commands with hello same resource group suffix that you used before.</span></span>

        git checkout -b beta
        git push origin beta
        .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch beta
6. <span data-ttu-id="c2e51-248">Amikor a rendszer kéri, adja meg, mint korábban hello azonos SQL-adatbázis hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="c2e51-248">When prompted, specify hello same SQL database credentials as before.</span></span> <span data-ttu-id="c2e51-249">Ezt követően amikor ismételt tooupdate hello erőforráscsoport, típus `Y`, majd `ENTER`.</span><span class="sxs-lookup"><span data-stu-id="c2e51-249">Then, when asked tooupdate hello resource group, type `Y`, then `ENTER`.</span></span>

    <span data-ttu-id="c2e51-250">Miután hello parancsfájl befejeződik, hello eredeti erőforráscsoporthoz tartozik, az erőforrások megmaradnak, de "béta" nevű új tárhely hozza létre azt hello azonos hello "Tesztelés" tárolóhely hello elején a létrehozott konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="c2e51-250">Once hello script finishes, all your resources in hello original resource group are retained, but a new slot named "beta" is created in it with hello same configuration as hello "Staging" slot that was created in hello beginning.</span></span>

   > [!NOTE]
   > <span data-ttu-id="c2e51-251">Ez a metódus létrehozásának különböző telepítési környezetekben eltér a hello metódus [gyors szoftverfejlesztői az Azure App Service](app-service-agile-software-development.md).</span><span class="sxs-lookup"><span data-stu-id="c2e51-251">This method of creating different deployment environments is different from hello method in [Agile software development with Azure App Service](app-service-agile-software-development.md).</span></span> <span data-ttu-id="c2e51-252">Itt hoz létre központi telepítési környezetben üzembe helyezési, ahol is vannak hoz létre központi telepítési környezetekben az erőforráscsoportokhoz.</span><span class="sxs-lookup"><span data-stu-id="c2e51-252">Here, you create deployment environments with deployment slots, where as there you create deployment environments with resource groups.</span></span> <span data-ttu-id="c2e51-253">Telepítési környezetekben kezelése az erőforráscsoportok lehetővé teszi tookeep hello éles környezetben off-limits toodevelopers, de nincs tesztelése éles, amelyek könnyen elvégezhető tárolóhelyek könnyen toodo.</span><span class="sxs-lookup"><span data-stu-id="c2e51-253">Managing deployment environments with resource groups enables you tookeep hello production environment off-limits toodevelopers, but it's not easy toodo testing in production, which you can do easily with slots.</span></span>
   >
   >

<span data-ttu-id="c2e51-254">Ha kívánja, is létrehozhat egy alfa alkalmazás futtatásával</span><span class="sxs-lookup"><span data-stu-id="c2e51-254">If you wish, you can also create an alpha app by running</span></span>

    git checkout -b alpha
    git push origin alpha
    .\deploy.ps1 -RepoUrl https://github.com/<your_fork>/ToDoApp.git -ResourceGroupSuffix <your_suffix> -SlotName beta -Branch alpha

<span data-ttu-id="c2e51-255">Ehhez az oktatóanyaghoz akkor csak használja a béta-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2e51-255">For this tutorial, you will just keep using your beta app.</span></span>

## <a name="update-push-your-updates-toohello-beta-app"></a><span data-ttu-id="c2e51-256">Frissítés: A frissítések toohello beta app leküldéses</span><span class="sxs-lookup"><span data-stu-id="c2e51-256">Update: Push your updates toohello beta app</span></span>
<span data-ttu-id="c2e51-257">Biztonsági másolatot, amelyet az tooimprove tooyour alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="c2e51-257">Back tooyour app that you want tooimprove.</span></span>

1. <span data-ttu-id="c2e51-258">Ellenőrizze, hogy van-e már a béta fiókirodai</span><span class="sxs-lookup"><span data-stu-id="c2e51-258">Make sure you're now in your beta branch</span></span>

        git checkout beta
2. <span data-ttu-id="c2e51-259">A  *&lt;repository_root >*\src\MultiChannelToDo.Web\app\Index.cshtml, a keresés hello `<li>` címkét, és adja hozzá a hello `style="cursor:pointer"` attribútum, a lent látható módon.</span><span class="sxs-lookup"><span data-stu-id="c2e51-259">In *&lt;repository_root>*\src\MultiChannelToDo.Web\app\Index.cshtml, find hello `<li>` tag and add hello `style="cursor:pointer"` attribute, as shown below.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/07-change-cursor-style-on-li.png)
3. <span data-ttu-id="c2e51-260">Véglegesítse és leküldéses tooAzure.</span><span class="sxs-lookup"><span data-stu-id="c2e51-260">commit and push tooAzure.</span></span>
4. <span data-ttu-id="c2e51-261">Győződjön meg arról, hogy hello módosítás is most megjelenik hello beta tárolóhely toohttp://todoapp navigálva*&lt;your_suffix >*-beta.azurewebsites.net/.</span><span class="sxs-lookup"><span data-stu-id="c2e51-261">Verify that hello change is now reflected in hello beta slot by navigating toohttp://todoapp*&lt;your_suffix>*-beta.azurewebsites.net/.</span></span> <span data-ttu-id="c2e51-262">Ha nem lát hello módosítása, de a böngésző tooget hello új javascript-kód frissítése.</span><span class="sxs-lookup"><span data-stu-id="c2e51-262">If you don't see hello change yet, refresh your browser tooget hello new javascript code.</span></span>

    ![](./media/app-service-web-test-in-production-controlled-test-flight/08-verify-change-in-beta-site.png)

<span data-ttu-id="c2e51-263">Most, hogy a módosítás hello beta tárolóhelye fut, készen áll a tooperform flighting telepítési áll.</span><span class="sxs-lookup"><span data-stu-id="c2e51-263">Now that you have your change running in hello beta slot, you are ready tooperform a flighting deployment.</span></span>

## <a name="validate-route-traffic-toohello-beta-app"></a><span data-ttu-id="c2e51-264">Ellenőrzése: Útvonal forgalom toohello beta app</span><span class="sxs-lookup"><span data-stu-id="c2e51-264">Validate: Route traffic toohello beta app</span></span>
<span data-ttu-id="c2e51-265">Ebben a szakaszban a forgalom toohello beta app irányítja.</span><span class="sxs-lookup"><span data-stu-id="c2e51-265">In this section, you will route traffic toohello beta app.</span></span> <span data-ttu-id="c2e51-266">Bemutató átláthatóság érdekében for sake of fog tooroute hello felhasználói forgalom tooit jelentős részét.</span><span class="sxs-lookup"><span data-stu-id="c2e51-266">For sake of clarity of demonstration, you're going tooroute a significant portion of hello user traffic tooit.</span></span> <span data-ttu-id="c2e51-267">A valóságban hello kívánt tooroute függ az adott helyzethez forgalom mennyisége.</span><span class="sxs-lookup"><span data-stu-id="c2e51-267">In reality, hello amount of traffic you want tooroute will depend on your specific situation.</span></span> <span data-ttu-id="c2e51-268">Például ha a webhely Microsoft.com hello léptékű, majd szükség lehet a teljes forgalom rendelés toogain hasznos adataiban kevesebb mint egy százalék.</span><span class="sxs-lookup"><span data-stu-id="c2e51-268">For example, if your site is at hello scale of microsoft.com, then you may need less than one percent of your total traffic in order toogain useful data.</span></span>

1. <span data-ttu-id="c2e51-269">A Git rendszerhéj-munkamenetben futtassa a következő parancsok tooroute hello hello éles forgalom toohello beta tárolóhelyre felének:</span><span class="sxs-lookup"><span data-stu-id="c2e51-269">In your Git Shell session, run hello following commands tooroute half of hello production traffic toohello beta slot:</span></span>

        $siteName = "ToDoApp<your suffix>"
        $rule = New-Object Microsoft.WindowsAzure.Commands.Utilities.Websites.Services.WebEntities.RampUpRule
        $rule.ActionHostName = "$siteName-beta.azurewebsites.net"
        $rule.ReroutePercentage = 50
        $rule.Name = "beta"
        Set-AzureWebsite $siteName -Slot Production -RoutingRules $rule

   <span data-ttu-id="c2e51-270">Hello `ReroutePercentage=50` tulajdonság határozza meg, hogy hello éles forgalom 50 %-át lesznek-e irányított toohello beta alkalmazás URL-CÍMÉT (hello által megadott `ActionHostName` tulajdonság).</span><span class="sxs-lookup"><span data-stu-id="c2e51-270">hello `ReroutePercentage=50` property specifies that 50% of hello production traffic will be routed toohello beta app's URL (specified by hello `ActionHostName` property).</span></span>
2. <span data-ttu-id="c2e51-271">Most lépjen toohttp://ToDoApp*&lt;your_suffix >*. azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="c2e51-271">Now navigate toohttp://ToDoApp*&lt;your_suffix>*.azurewebsites.net.</span></span> <span data-ttu-id="c2e51-272">hello forgalom 50 %-át kell átirányított toohello beta tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="c2e51-272">50% of hello traffic should now be redirected toohello beta slot.</span></span>
3. <span data-ttu-id="c2e51-273">Az Application Insights-erőforrást, a szűrést hello metrikák környezet = "béta".</span><span class="sxs-lookup"><span data-stu-id="c2e51-273">In your Application Insights resource, filter hello metrics by environment="beta".</span></span>

   > [!NOTE]
   > <span data-ttu-id="c2e51-274">Ha egy másik kedvencként menti a szűrt nézet, majd egyszerűen váltani hello metrika explorer nézetek éles tárhely és a béta nézetek között.</span><span class="sxs-lookup"><span data-stu-id="c2e51-274">If you save this filtered view as another favorite, then you can easily flip hello metric explorer views between production and beta views.</span></span>
   >
   >

<span data-ttu-id="c2e51-275">Tegyük fel, hogy az Application Insightsban valami hasonló toohello következő látható:</span><span class="sxs-lookup"><span data-stu-id="c2e51-275">Suppose in Application Insights you see something similar toohello following:</span></span>

![](./media/app-service-web-test-in-production-controlled-test-flight/09-test-beta-site-in-production.png)

<span data-ttu-id="c2e51-276">Nem csak a rendszer Ez azt jelzi, hogy vannak-e a hello számos további kattintással `<li>` címkék, de úgy tűnik, toobe kattintással megugrását a `<li>` címkék.</span><span class="sxs-lookup"><span data-stu-id="c2e51-276">Not only is this showing that there are many more clicks on hello `<li>` tags, but there seems toobe a surge of clicks on `<li>` tags.</span></span> <span data-ttu-id="c2e51-277">Majd be, hogy személyek hello új felderített `<li>` címkék kattintható, és most vannak törlésével a korábban szerzett feladataival hello alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="c2e51-277">You can then conclude that people have discovered hello new `<li>` tags are clickable and are now clearing all their previously-completed tasks in hello app.</span></span>

<span data-ttu-id="c2e51-278">A központi telepítés flighting hello adatok alapján, dönthet úgy, hogy az üzemi készen áll-e az új felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="c2e51-278">Based on hello data of your flighting deployment, you decide that your new UI is ready for production.</span></span>

## <a name="go-live-move-your-new-code-into-production"></a><span data-ttu-id="c2e51-279">Nyissa meg az élő: helyezze át az új kódot éles környezetben</span><span class="sxs-lookup"><span data-stu-id="c2e51-279">Go live: Move your new code into production</span></span>
<span data-ttu-id="c2e51-280">Ön éppen most már készen áll a toomove a frissítés tooproduction.</span><span class="sxs-lookup"><span data-stu-id="c2e51-280">You're now ready toomove your update tooproduction.</span></span> <span data-ttu-id="c2e51-281">Mi az az nagy, hogy most már tudja, hogy a frissítés már érvényesítve lett *előtt* tooproduction topológiájával.</span><span class="sxs-lookup"><span data-stu-id="c2e51-281">What's great is that now you know that your update has already been validated *before* it is pushed tooproduction.</span></span> <span data-ttu-id="c2e51-282">Most magabiztosan telepítheti azt.</span><span class="sxs-lookup"><span data-stu-id="c2e51-282">So now you can confidently deploy it.</span></span> <span data-ttu-id="c2e51-283">Végrehajtott frissítés toohello AngularJS ügyfelet alkalmazást, mert csak érvényesítve hello ügyféloldali kódot.</span><span class="sxs-lookup"><span data-stu-id="c2e51-283">Since you made an update toohello AngularJS client app, you only validated hello client-side code.</span></span> <span data-ttu-id="c2e51-284">Ha toomake módosítások toohello háttér-webes API-alkalmazás, érvényesíthető a módosításokat, hasonlóképpen és könnyen.</span><span class="sxs-lookup"><span data-stu-id="c2e51-284">If you were toomake changes toohello back-end Web API app, you could validate your changes similarly and easily.</span></span>

1. <span data-ttu-id="c2e51-285">Git rendszerhéj távolítsa el a hello forgalom-útválasztási szabály hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c2e51-285">In Git Shell, remove hello traffic routing rule by running hello following command:</span></span>

        Set-AzureWebsite $siteName -Slot Production -RoutingRules @()
2. <span data-ttu-id="c2e51-286">Hello Git-parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c2e51-286">Run hello Git commands:</span></span>

        git checkout master
        git pull origin master
        git merge beta
        git push origin master
3. <span data-ttu-id="c2e51-287">Várjon néhány percig, hello új tárhely átmeneti telepített toobe toohello code, majd indítsa el a http://ToDoApp*&lt;your_suffix >*-staging.azurewebsites.net tooverify, amely az új frissítés hello hello átmenetiként tárolóhelyspecifikus tárolóhely.</span><span class="sxs-lookup"><span data-stu-id="c2e51-287">Wait for a few minutes for hello new code toobe deployed toohello staging slot, then launch http://ToDoApp*&lt;your_suffix>*-staging.azurewebsites.net tooverify that hello new update is warmed up in hello staging slot.</span></span> <span data-ttu-id="c2e51-288">Ne feledje, hogy az elágazáshoz fő ág hello kapcsolódó toohello átmeneti tárolóhely az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="c2e51-288">Remember that hello your fork's master branch is linked toohello staging slot of your app.</span></span>
4. <span data-ttu-id="c2e51-289">Most a tárhely átmeneti éles környezetben hello felcserélése</span><span class="sxs-lookup"><span data-stu-id="c2e51-289">Now, swap hello staging slot into production</span></span>

        cd <ROOT>\ToDoApp\ARMTemplates
        .\swap.ps1 -Name todoapp<your_suffix>

## <a name="summary"></a><span data-ttu-id="c2e51-290">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="c2e51-290">Summary</span></span>
<span data-ttu-id="c2e51-291">Az Azure App Service megkönnyíti toomedium-a kisméretű vállalkozások tootest ügyfélkapcsolati alkalmazások éles, valamit a nagyvállalatokhoz hagyományosan elvégzett.</span><span class="sxs-lookup"><span data-stu-id="c2e51-291">Azure App Service makes it easy for small- toomedium-sized businesses tootest their customer-facing apps in production, something that has been traditionally done in big enterprises.</span></span> <span data-ttu-id="c2e51-292">Remélhetőleg, ez az oktatóanyag adott Tudásbázis toobring együtt az App Service és az Application Insights toomake lehetséges flighting telepítési, és akár egyéb tesztelése az éles forgatókönyvek szüksége a DevOps world hello.</span><span class="sxs-lookup"><span data-stu-id="c2e51-292">Hopefully, this tutorial has given you hello knowledge you need toobring together App Service and Application Insights toomake possible flighting deployment, and even other test-in-production scenarios, in your DevOps world.</span></span>

## <a name="more-resources"></a><span data-ttu-id="c2e51-293">További erőforrások</span><span class="sxs-lookup"><span data-stu-id="c2e51-293">More resources</span></span>
* [<span data-ttu-id="c2e51-294">Agilis szoftverfejlesztői az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c2e51-294">Agile software development with Azure App Service</span></span>](app-service-agile-software-development.md)
* [<span data-ttu-id="c2e51-295">Átmeneti környezet az Azure App Service web Apps beállítása</span><span class="sxs-lookup"><span data-stu-id="c2e51-295">Set up staging environments for web apps in Azure App Service</span></span>](web-sites-staged-publishing.md)
* [<span data-ttu-id="c2e51-296">Kiszámítható módon tudja az Azure-ban egy összetett alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="c2e51-296">Deploy a complex application predictably in Azure</span></span>](app-service-deploy-complex-application-predictably.md)
* [<span data-ttu-id="c2e51-297">Az Azure Resource Manager sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="c2e51-297">Authoring Azure Resource Manager Templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="c2e51-298">JSONLint - hello JSON-érvényesítő</span><span class="sxs-lookup"><span data-stu-id="c2e51-298">JSONLint - hello JSON Validator</span></span>](http://jsonlint.com/)
* [<span data-ttu-id="c2e51-299">Git ugorhat – alapvető ugorhat és egyesítése</span><span class="sxs-lookup"><span data-stu-id="c2e51-299">Git Branching – Basic Branching and Merging</span></span>](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
* [<span data-ttu-id="c2e51-300">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2e51-300">Azure PowerShell</span></span>](/powershell/azure/overview)
* [<span data-ttu-id="c2e51-301">Projekt Kudu Wiki</span><span class="sxs-lookup"><span data-stu-id="c2e51-301">Project Kudu Wiki</span></span>](https://github.com/projectkudu/kudu/wiki)
