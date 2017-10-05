---
title: "WebJobs üzembe helyezése Visual Studióval"
description: "Ismerje meg, üzembe helyezése Azure webjobs-feladatok Azure App Service Web Apps Visual Studio használatával."
services: app-service
documentationcenter: 
author: ggailey777
manager: erikre
editor: jimbe
ms.assetid: a3a9d320-1201-4ac8-9398-b4c9535ba755
ms.service: app-service
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/27/2016
ms.author: glenga
ms.openlocfilehash: 5b0808afdadcf4d86a9a2d07ee6fc63b80b22993
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="3aea4-103">WebJobs üzembe helyezése Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="3aea4-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="3aea4-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="3aea4-104">Overview</span></span>
<span data-ttu-id="3aea4-105">Ez a témakör ismerteti, hogyan egy konzolalkalmazás projekt telepítése egy webalkalmazást, a Visual Studio [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) , egy [Azure webjobs-feladat](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="3aea4-105">This topic explains how to use Visual Studio to deploy a Console Application project to a web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="3aea4-106">A webjobs-feladatok telepítésével kapcsolatos információkat a [Azure Portal](https://portal.azure.com), lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="3aea4-106">For information about how to deploy WebJobs by using the [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="3aea4-107">Ha a Visual Studio telepíti a WebJobs-kompatibilis Konzolalkalmazás projekt, két feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="3aea4-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="3aea4-108">Futásidejű fájlokat másolja a megfelelő mappát a web app alkalmazásban (*App_Data/feladatok/folyamatos* a folyamatos webjobs-feladatok, *App_Data/feladatok/indított* az ütemezett és igény szerinti webjobs-feladatok).</span><span class="sxs-lookup"><span data-stu-id="3aea4-108">Copies runtime files to the appropriate folder in the web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="3aea4-109">Beállítja az [Azure ütemezőjének](#scheduler) az adott időpontokban futtatásra ütemezett webjobs-feladatok.</span><span class="sxs-lookup"><span data-stu-id="3aea4-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled to run at particular times.</span></span> <span data-ttu-id="3aea4-110">(Ez nem szükséges a folyamatos WebJobs.)</span><span class="sxs-lookup"><span data-stu-id="3aea4-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="3aea4-111">A WebJobs-kompatibilis projekt van felvéve a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="3aea4-111">A WebJobs-enabled project has the following items added to it:</span></span>

* <span data-ttu-id="3aea4-112">A [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="3aea4-112">The [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="3aea4-113">A [webjobs-feladat közzététele settings.json](#publishsettings) központi telepítés és az ütemezési beállításokat tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="3aea4-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Mi kerül egy központi telepítésben is webjobs-feladat engedélyezése Konzolalkalmazás bemutató ábra](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="3aea4-115">Ezek az elemek hozzáadása egy meglévő Konzolalkalmazás-projektet, vagy egy sablon használatával létrehozhat egy új WebJobs-kompatibilis Konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="3aea4-115">You can add these items to an existing Console Application project or use a template to create a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="3aea4-116">Projekt telepítése webjobs-feladat, önmagában, vagy hivatkozás egy webes projekt, hogy azt automatikusan telepíti, ha a webes projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="3aea4-116">You can deploy a project as a WebJob by itself, or link it to a web project so that it automatically deploys whenever you deploy the web project.</span></span> <span data-ttu-id="3aea4-117">Projektek csatolásához Visual Studio tartalmazza a WebJobs-kompatibilis projekt nevét a [webjobs-list.json](#webjobslist) a webes projekt fájlban.</span><span class="sxs-lookup"><span data-stu-id="3aea4-117">To link projects, Visual Studio includes the name of the WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in the web project.</span></span>

![Webjobs-feladat project web project csatolása bemutató ábra](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="3aea4-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3aea4-119">Prerequisites</span></span>
<span data-ttu-id="3aea4-120">WebJobs-telepítési szolgáltatások érhetők el a Visual Studióban, ha a .NET-keretrendszerhez készült Azure SDK telepítése:</span><span class="sxs-lookup"><span data-stu-id="3aea4-120">WebJobs deployment features are available in Visual Studio when you install the Azure SDK for .NET:</span></span>

* <span data-ttu-id="3aea4-121">[Az Azure SDK for .NET (a Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="3aea4-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="3aea4-122"><a id="convert"></a>Webjobs-feladatok meglévő Konzolalkalmazás projekt központi telepítésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3aea4-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="3aea4-123">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="3aea4-123">You have two options:</span></span>

* <span data-ttu-id="3aea4-124">[Engedélyezze az automatikus központi telepítési egy webes projekt](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="3aea4-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="3aea4-125">Egy meglévő Konzolalkalmazás-projekt konfigurálása, hogy automatikusan telepíti a webjobs-feladat, amikor egy webes projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="3aea4-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="3aea4-126">Használja ezt a beállítást, ha azt szeretné, a webjobs-feladat futtatása az azonos, a kapcsolódó webalkalmazás futtassa web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3aea4-126">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>
* <span data-ttu-id="3aea4-127">[Központi telepítés nélkül webes projektet engedélyezése](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="3aea4-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="3aea4-128">Webjobs-feladat készítéséhez önmagában, a webes projektet Kapcsolódás meglévő Konzolalkalmazás projekt konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="3aea4-128">Configure an existing Console Application project to deploy as a WebJob by itself, with no link to a web project.</span></span> <span data-ttu-id="3aea4-129">Használja ezt a beállítást, ha azt szeretné, a webjobs-feladat futtatása a webalkalmazásban önmagában nem fut a webes alkalmazás webes alkalmazásokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3aea4-129">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="3aea4-130">Érdemes ehhez ahhoz, hogy a lehetővé válik a webjobs-feladat erőforrások, függetlenül a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="3aea4-130">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="3aea4-131"><a id="convertlink"></a>WebJobs-automatikus központi telepítési egy webes projekt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3aea4-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="3aea4-132">Kattintson a jobb gombbal a webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **Azure webjobs-feladat létező projekt**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-132">Right-click the web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Azure webjobs-feladat létező projekt](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="3aea4-134">A [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3aea4-134">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="3aea4-135">Az a **projektnevet** legördülő listában válassza a Konzolalkalmazás projekt, amelyet a webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="3aea4-135">In the **Project name** drop-down list, select the Console Application project to add as a WebJob.</span></span>
   
    ![Válassza ki a projektet az Azure Webjobs hozzáadása párbeszédpanel](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="3aea4-137">Fejezze be a [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-137">Complete the [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="3aea4-138"><a id="convertnolink"></a>Webjobs-feladatok központi telepítés nélkül webes projektet engedélyezése</span><span class="sxs-lookup"><span data-stu-id="3aea4-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="3aea4-139">Kattintson a jobb gombbal a Konzolalkalmazás-projektre a **Megoldáskezelőben**, és kattintson a **Azure webjobs-feladat közzététel...** .</span><span class="sxs-lookup"><span data-stu-id="3aea4-139">Right-click the Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="3aea4-141">A [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg, a kiválasztott projekthez a **projektnevet** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="3aea4-141">The [Add Azure WebJob](#configure) dialog box appears, with the project selected in the **Project name** box.</span></span>
2. <span data-ttu-id="3aea4-142">Fejezze be a [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-142">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="3aea4-143">A **webhely közzététele** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3aea4-143">The **Publish Web** wizard appears.</span></span>  <span data-ttu-id="3aea4-144">Ha nem szeretné, azonnal közzététele, zárja be a varázslót.</span><span class="sxs-lookup"><span data-stu-id="3aea4-144">If you do not want to publish immediately, close the wizard.</span></span> <span data-ttu-id="3aea4-145">A megadott beállítások a lesznek mentve, ha meg szeretné [a projekt telepítése](#deploy).</span><span class="sxs-lookup"><span data-stu-id="3aea4-145">The settings that you've entered are saved for when you do want to [deploy the project](#deploy).</span></span>

## <span data-ttu-id="3aea4-146"><a id="create"></a>WebJobs-kompatibilis új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3aea4-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="3aea4-147">WebJobs-kompatibilis új projekt létrehozásához, használhatja a Konzolalkalmazás projekt sablont és WebJobs központi telepítés engedélyezése a [az előző szakaszban](#convert).</span><span class="sxs-lookup"><span data-stu-id="3aea4-147">To create a new WebJobs-enabled project, you can use the Console Application project template and enable WebJobs deployment as explained in [the previous section](#convert).</span></span> <span data-ttu-id="3aea4-148">Alternatív megoldásként használhatja a webjobs-feladatok – új projekt sablont:</span><span class="sxs-lookup"><span data-stu-id="3aea4-148">As an alternative, you can use the WebJobs new-project template:</span></span>

* [<span data-ttu-id="3aea4-149">A webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="3aea4-149">Use the WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="3aea4-150">A projekt létrehozása, és konfigurálja úgy, hogy telepítsen önmagában, a webjobs-feladat, a webes projekt nincs hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="3aea4-150">Create a project and configure it to deploy by itself as a WebJob, with no link to a web project.</span></span> <span data-ttu-id="3aea4-151">Használja ezt a beállítást, ha azt szeretné, a webjobs-feladat futtatása a webalkalmazásban önmagában nem fut a webes alkalmazás webes alkalmazásokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="3aea4-151">Use this option when you want to run a WebJob in a web app by itself, with no web application running in the web app.</span></span> <span data-ttu-id="3aea4-152">Érdemes ehhez ahhoz, hogy a lehetővé válik a webjobs-feladat erőforrások, függetlenül a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="3aea4-152">You might want to do this in order to be able to scale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="3aea4-153">A webjobs-feladatok – új projekt sablon használata az egy webes projekt kapcsolódó webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="3aea4-153">Use the WebJobs new-project template for a WebJob linked to a web project</span></span>](#createlink)
  
    <span data-ttu-id="3aea4-154">Egy webes projekt megoldáskezelőben telepítésekor automatikusan webjobs-feladat telepítéséhez konfigurált-projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3aea4-154">Create a project that is configured to deploy automatically as a WebJob when a web project in the same solution is deployed.</span></span> <span data-ttu-id="3aea4-155">Használja ezt a beállítást, ha azt szeretné, a webjobs-feladat futtatása az azonos, a kapcsolódó webalkalmazás futtassa web app alkalmazásban.</span><span class="sxs-lookup"><span data-stu-id="3aea4-155">Use this option when you want to run your WebJob in the same web app in which you run the related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="3aea4-156">A webjobs-feladatok – új projekt sablon automatikusan telepíti a NuGet-csomagok, és magában foglalja a kód *Program.cs* a a [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="3aea4-156">The WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for the [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="3aea4-157">Ha nem szeretné használni a WebJobs SDK, távolítsa el vagy módosítsa a `host.RunAndBlock` utasítás *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="3aea4-157">If you don't want to use the WebJobs SDK, remove or change the `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="3aea4-158"><a id="createnolink"></a>A webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="3aea4-158"><a id="createnolink"></a> Use the WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="3aea4-159">Kattintson a **fájl** > **új projekt**, majd a **új projekt** párbeszédpanelen kattintson **felhő** > **Azure webjobs-feladat (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-159">Click **File** > **New Project**, and then in the **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Webjobs-feladat sablon új projekt párbeszédpanel](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="3aea4-161">Kövesse az utasításokat a korábbi látható [ellenőrizze a Konzolalkalmazás projekt egy független WebJobs-projekt](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="3aea4-161">Follow the directions shown earlier to [make the Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="3aea4-162"><a id="createlink"></a>A webjobs-feladatok – új projekt sablon használata az egy webes projekt kapcsolódó webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="3aea4-162"><a id="createlink"></a> Use the WebJobs new-project template for a WebJob linked to a web project</span></span>
1. <span data-ttu-id="3aea4-163">Kattintson a jobb gombbal a webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **új Azure webjobs-feladat projekt**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-163">Right-click the web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Új Azure webjobs-feladat projekt menübejegyzést](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="3aea4-165">A [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3aea4-165">The [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="3aea4-166">Fejezze be a [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3aea4-166">Complete the [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="3aea4-167"><a id="configure"></a>Az Azure Webjobs hozzáadása párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="3aea4-167"><a id="configure"></a>The Add Azure WebJob dialog</span></span>
<span data-ttu-id="3aea4-168">A **adja hozzá a Azure webjobs-feladat** párbeszédpanelen lehetővé teszi, hogy adja meg a webjobs-feladat nevét, és futtassa a webjobs-feladat mód beállítása.</span><span class="sxs-lookup"><span data-stu-id="3aea4-168">The **Add Azure WebJob** dialog lets you enter the WebJob name and run mode setting for your WebJob.</span></span> 

![Azure webjobs-feladat párbeszédpanel hozzáadása](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="3aea4-170">A mezők ezen a párbeszédpanelen mezőinek felelnek meg a **új feladat** párbeszédpanel Azure-portálról.</span><span class="sxs-lookup"><span data-stu-id="3aea4-170">The fields in this dialog correspond to fields on the **New Job** dialog of the Azure Portal.</span></span> <span data-ttu-id="3aea4-171">További információkért lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="3aea4-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="3aea4-172">Parancssori telepítéssel kapcsolatos információkért lásd: [parancssori engedélyezése vagy folyamatos kézbesítését az Azure webjobs-feladatok](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="3aea4-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="3aea4-173">Ha telepíti a webjobs-feladat, majd döntse el, a webjobs-feladat, és helyezze üzembe újra módosításához, lesz szüksége lehet a webjobs-feladatok közzététele settings.json fájl törléséhez.</span><span class="sxs-lookup"><span data-stu-id="3aea4-173">If you deploy a WebJob and then decide you want to change the type of WebJob and redeploy, you'll need to delete the webjobs-publish-settings.json file.</span></span> <span data-ttu-id="3aea4-174">Ezzel biztosítható a Visual Studio jelenjen meg a közzétételi beállításokat, így módosíthatja a webjobs-feladat típusát.</span><span class="sxs-lookup"><span data-stu-id="3aea4-174">This will make Visual Studio show the publishing options again, so you can change the type of WebJob.</span></span>
> * <span data-ttu-id="3aea4-175">Ha telepíti a webjobs-feladat, a futtatási mód később módosítható a folyamatos nem folytonos vagy fordítva, a Visual Studio a újratelepítésekor a új webjobs-feladat az Azure-hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3aea4-175">If you deploy a WebJob and later change the run mode from continuous to non-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="3aea4-176">Ha további ütemezési beállítások módosítására, de hagyja futtatási mód megegyezik, vagy váltás ütemezett és igény szerinti, Visual Studio frissíti a meglévő feladat helyett hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="3aea4-176">If you change other scheduling settings but leave run mode the same or switch between Scheduled and On Demand, Visual Studio updates the existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="3aea4-177"><a id="publishsettings"></a>webjobs-feladat közzététele settings.json</span><span class="sxs-lookup"><span data-stu-id="3aea4-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="3aea4-178">Amikor konfigurál egy konzolalkalmazás WebJobs központi telepítés, a Visual Studio telepíti a [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet csomag- és ütemezési információkat tárolja egy *webjobs-feladat közzététele settings.json* fájlt a projektben *tulajdonságok* a WebJobs-projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="3aea4-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs the [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in the project *Properties* folder of the WebJobs project.</span></span> <span data-ttu-id="3aea4-179">Íme egy példa, hogy a fájl:</span><span class="sxs-lookup"><span data-stu-id="3aea4-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="3aea4-180">Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít.</span><span class="sxs-lookup"><span data-stu-id="3aea4-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="3aea4-181">A fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) nem lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="3aea4-181">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="3aea4-182"><a id="webjobslist"></a>webjobs-list.json</span><span class="sxs-lookup"><span data-stu-id="3aea4-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="3aea4-183">Ha egy WebJobs-kompatibilis projekt webes projektet, a Visual Studio tárolja a webjobs-feladatok projekt nevét egy *webjobs-list.json* fájl a webes projekt *tulajdonságok* mappa.</span><span class="sxs-lookup"><span data-stu-id="3aea4-183">When you link a WebJobs-enabled project to a web project, Visual Studio stores the name of the WebJobs project in a *webjobs-list.json* file in the web project's *Properties* folder.</span></span> <span data-ttu-id="3aea4-184">A lista tartalmazhat több WebJobs-projektet, a következő példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="3aea4-184">The list might contain multiple WebJobs projects, as shown in the following example:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjobs-list.json",
          "WebJobs": [
            {
              "filePath": "../ConsoleApplication1/ConsoleApplication1.csproj"
            },
            {
              "filePath": "../WebJob1/WebJob1.csproj"
            }
          ]
        }

<span data-ttu-id="3aea4-185">Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít.</span><span class="sxs-lookup"><span data-stu-id="3aea4-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="3aea4-186">A fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) nem lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="3aea4-186">The file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="3aea4-187"><a id="deploy"></a>Webjobs-feladatok projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="3aea4-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="3aea4-188">Webjobs-feladatok projektben lévő nyit meg egy webes projekt automatikusan telepíti a webes projekt.</span><span class="sxs-lookup"><span data-stu-id="3aea4-188">A WebJobs project that you have linked to a web project deploys automatically with the web project.</span></span> <span data-ttu-id="3aea4-189">A webes projekt telepítése kapcsolatos információkért lásd: [webalkalmazások központi telepítése](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="3aea4-189">For information about web project deployment, see [How to deploy to Web Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="3aea4-190">Webjobs-feladatok projekt telepítése önmagában, kattintson a jobb gombbal a projektre a **Megoldáskezelőben** kattintson **Azure webjobs-feladat közzététel...** .</span><span class="sxs-lookup"><span data-stu-id="3aea4-190">To deploy a WebJobs project by itself, right-click the project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="3aea4-192">Egy független webjobs-feladat, ugyanazt a **webhely közzététele** használt webes projektek megjelenik, de kevesebb beállításokkal elérhető módosítása varázsló.</span><span class="sxs-lookup"><span data-stu-id="3aea4-192">For an independent WebJob, the same **Publish Web** wizard that is used for web projects appears, but with fewer settings available to change.</span></span>

## <span data-ttu-id="3aea4-193"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3aea4-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="3aea4-194">Ez a cikk szerint WebJobs központi telepítése a Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="3aea4-194">This article has explained how to deploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="3aea4-195">Azure webjobs-feladatok telepítésével kapcsolatos további információkért lásd: [Azure WebJobs - erőforrások – ajánlott telepítési](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="3aea4-195">For more information about how to deploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

