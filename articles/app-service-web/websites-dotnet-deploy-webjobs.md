---
title: "aaaDeploy webjobs-feladatok Visual Studio használatával"
description: "Megtudhatja, hogyan toodeploy Azure webjobs-feladatok tooAzure App Service Web Apps Visual Studio használatával."
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
ms.openlocfilehash: 5fc5d9562e8836348f5ab6844fb6c23ff40a321c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-webjobs-using-visual-studio"></a><span data-ttu-id="618dd-103">WebJobs üzembe helyezése Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="618dd-103">Deploy WebJobs using Visual Studio</span></span>
## <a name="overview"></a><span data-ttu-id="618dd-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="618dd-104">Overview</span></span>
<span data-ttu-id="618dd-105">Ez a témakör ismerteti, hogyan toouse Visual Studio toodeploy egy konzolalkalmazás projekt webalkalmazás tooa [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) , egy [Azure webjobs-feladat](http://go.microsoft.com/fwlink/?LinkId=390226).</span><span class="sxs-lookup"><span data-stu-id="618dd-105">This topic explains how toouse Visual Studio toodeploy a Console Application project tooa web app in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) as an [Azure WebJob](http://go.microsoft.com/fwlink/?LinkId=390226).</span></span> <span data-ttu-id="618dd-106">Hogyan toodeploy webjobs-feladatok használatával hello információt [Azure Portal](https://portal.azure.com), lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="618dd-106">For information about how toodeploy WebJobs by using hello [Azure Portal](https://portal.azure.com), see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

<span data-ttu-id="618dd-107">Ha a Visual Studio telepíti a WebJobs-kompatibilis Konzolalkalmazás projekt, két feladatokat hajtja végre:</span><span class="sxs-lookup"><span data-stu-id="618dd-107">When Visual Studio deploys a WebJobs-enabled Console Application project, it performs two tasks:</span></span>

* <span data-ttu-id="618dd-108">Másolatot futásidejű fájlmappa toohello megfelelő hello web app alkalmazásban (*App_Data/feladatok/folyamatos* a folyamatos webjobs-feladatok, *App_Data/feladatok/indított* az ütemezett és igény szerinti webjobs-feladatok).</span><span class="sxs-lookup"><span data-stu-id="618dd-108">Copies runtime files toohello appropriate folder in hello web app (*App_Data/jobs/continuous* for continuous WebJobs, *App_Data/jobs/triggered* for scheduled and on-demand WebJobs).</span></span>
* <span data-ttu-id="618dd-109">Beállítja az [Azure ütemezőjének](#scheduler) a webjobs-feladatok, amelyek ütemezett toorun adott időpontban.</span><span class="sxs-lookup"><span data-stu-id="618dd-109">Sets up [Azure Scheduler jobs](#scheduler) for WebJobs that are scheduled toorun at particular times.</span></span> <span data-ttu-id="618dd-110">(Ez nem szükséges a folyamatos WebJobs.)</span><span class="sxs-lookup"><span data-stu-id="618dd-110">(This is not needed for continuous WebJobs.)</span></span>

<span data-ttu-id="618dd-111">A WebJobs-kompatibilis projekt rendelkezik a következő elemek hozzáadott tooit hello:</span><span class="sxs-lookup"><span data-stu-id="618dd-111">A WebJobs-enabled project has hello following items added tooit:</span></span>

* <span data-ttu-id="618dd-112">Hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet-csomagot.</span><span class="sxs-lookup"><span data-stu-id="618dd-112">hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package.</span></span>
* <span data-ttu-id="618dd-113">A [webjobs-feladat közzététele settings.json](#publishsettings) központi telepítés és az ütemezési beállításokat tartalmazó fájl.</span><span class="sxs-lookup"><span data-stu-id="618dd-113">A [webjob-publish-settings.json](#publishsettings) file that contains deployment and scheduler settings.</span></span> 

![Mi meg van adva egy webjobs-feladat tooa Konzolalkalmazás tooenable központi telepítés ábrája](./media/websites-dotnet-deploy-webjobs/convert.png)

<span data-ttu-id="618dd-115">Adja hozzá a Konzolalkalmazás projekt meglévő elemek tooan, vagy használja a sablon toocreate egy új WebJobs-kompatibilis Konzolalkalmazás-projektet.</span><span class="sxs-lookup"><span data-stu-id="618dd-115">You can add these items tooan existing Console Application project or use a template toocreate a new WebJobs-enabled Console Application project.</span></span> 

<span data-ttu-id="618dd-116">Projekt telepítése webjobs-feladat, önmagában, vagy hivatkozás tooa webes projektet, hogy automatikusan telepíti amikor hello webes projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="618dd-116">You can deploy a project as a WebJob by itself, or link it tooa web project so that it automatically deploys whenever you deploy hello web project.</span></span> <span data-ttu-id="618dd-117">toolink projektek, a Visual Studio hello WebJobs-kompatibilis projekt nevét hello magában foglalja a [webjobs-list.json](#webjobslist) hello webes projekt fájlban.</span><span class="sxs-lookup"><span data-stu-id="618dd-117">toolink projects, Visual Studio includes hello name of hello WebJobs-enabled project in a [webjobs-list.json](#webjobslist) file in hello web project.</span></span>

![Webjobs-feladat projekt tooweb projekt linking bemutató ábra](./media/websites-dotnet-deploy-webjobs/link.png)

## <a name="prerequisites"></a><span data-ttu-id="618dd-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="618dd-119">Prerequisites</span></span>
<span data-ttu-id="618dd-120">Webjobs-feladatok üzembe helyezési funkcióival hello Azure SDK for .NET telepítése esetén a Visual Studio érhetők el:</span><span class="sxs-lookup"><span data-stu-id="618dd-120">WebJobs deployment features are available in Visual Studio when you install hello Azure SDK for .NET:</span></span>

* <span data-ttu-id="618dd-121">[Az Azure SDK for .NET (a Visual Studio)](https://azure.microsoft.com/downloads/).</span><span class="sxs-lookup"><span data-stu-id="618dd-121">[Azure SDK for .NET (Visual Studio)](https://azure.microsoft.com/downloads/).</span></span>

## <span data-ttu-id="618dd-122"><a id="convert"></a>Webjobs-feladatok meglévő Konzolalkalmazás projekt központi telepítésének engedélyezése</span><span class="sxs-lookup"><span data-stu-id="618dd-122"><a id="convert"></a>Enable WebJobs deployment for an existing Console Application project</span></span>
<span data-ttu-id="618dd-123">Erre két lehetősége van:</span><span class="sxs-lookup"><span data-stu-id="618dd-123">You have two options:</span></span>

* <span data-ttu-id="618dd-124">[Engedélyezze az automatikus központi telepítési egy webes projekt](#convertlink).</span><span class="sxs-lookup"><span data-stu-id="618dd-124">[Enable automatic deployment with a web project](#convertlink).</span></span>
  
    <span data-ttu-id="618dd-125">Egy meglévő Konzolalkalmazás-projekt konfigurálása, hogy automatikusan telepíti a webjobs-feladat, amikor egy webes projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="618dd-125">Configure an existing Console Application project so that it automatically deploys as a WebJob when you deploy a web project.</span></span> <span data-ttu-id="618dd-126">Használja ezt a beállítást, ha azt szeretné, toorun a webjobs-feladat a hello hello futtassa azonos webalkalmazás kapcsolódó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="618dd-126">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>
* <span data-ttu-id="618dd-127">[Központi telepítés nélkül webes projektet engedélyezése](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="618dd-127">[Enable deployment without a web project](#convertnolink).</span></span>
  
    <span data-ttu-id="618dd-128">Konfigurálja egy meglévő Konzolalkalmazás projekt toodeploy webjobs-feladat önmagában, nincs hivatkozás tooa webes projekt esetében.</span><span class="sxs-lookup"><span data-stu-id="618dd-128">Configure an existing Console Application project toodeploy as a WebJob by itself, with no link tooa web project.</span></span> <span data-ttu-id="618dd-129">Használja ezt a beállítást, ha azt szeretné, a webes alkalmazás a webjobs-feladat toorun önmagában, nincs hello webalkalmazásban futó webes alkalmazásokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="618dd-129">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="618dd-130">Érdemes lehet toodo Ez a rendezés toobe képes tooscale a webjobs-feladat erőforrásokat, függetlenül a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="618dd-130">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>

### <span data-ttu-id="618dd-131"><a id="convertlink"></a>WebJobs-automatikus központi telepítési egy webes projekt engedélyezése</span><span class="sxs-lookup"><span data-stu-id="618dd-131"><a id="convertlink"></a> Enable automatic WebJobs deployment with a web project</span></span>
1. <span data-ttu-id="618dd-132">Kattintson a jobb gombbal hello webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **Azure webjobs-feladat létező projekt**.</span><span class="sxs-lookup"><span data-stu-id="618dd-132">Right-click hello web project in **Solution Explorer**, and then click **Add** > **Existing Project as Azure WebJob**.</span></span>
   
    ![Azure webjobs-feladat létező projekt](./media/websites-dotnet-deploy-webjobs/eawj.png)
   
    <span data-ttu-id="618dd-134">Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="618dd-134">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="618dd-135">A hello **projektnevet** legördülő listában, jelölje be hello Konzolalkalmazás projekt tooadd, webjobs-feladat.</span><span class="sxs-lookup"><span data-stu-id="618dd-135">In hello **Project name** drop-down list, select hello Console Application project tooadd as a WebJob.</span></span>
   
    ![Válassza ki a projektet az Azure Webjobs hozzáadása párbeszédpanel](./media/websites-dotnet-deploy-webjobs/aaw1.png)
3. <span data-ttu-id="618dd-137">Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="618dd-137">Complete hello [Add Azure WebJob](#configure) dialog, and then click **OK**.</span></span> 

### <span data-ttu-id="618dd-138"><a id="convertnolink"></a>Webjobs-feladatok központi telepítés nélkül webes projektet engedélyezése</span><span class="sxs-lookup"><span data-stu-id="618dd-138"><a id="convertnolink"></a> Enable WebJobs deployment without a web project</span></span>
1. <span data-ttu-id="618dd-139">Kattintson a jobb gombbal hello Konzolalkalmazás projekt **Megoldáskezelőben**, és kattintson a **Azure webjobs-feladat közzététel...** .</span><span class="sxs-lookup"><span data-stu-id="618dd-139">Right-click hello Console Application project in **Solution Explorer**, and then click **Publish as Azure WebJob...**.</span></span> 
   
    ![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)
   
    <span data-ttu-id="618dd-141">Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg, hello kiválasztott hello projekt **projektnevet** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="618dd-141">hello [Add Azure WebJob](#configure) dialog box appears, with hello project selected in hello **Project name** box.</span></span>
2. <span data-ttu-id="618dd-142">Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="618dd-142">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>
   
   <span data-ttu-id="618dd-143">Hello **webhely közzététele** varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="618dd-143">hello **Publish Web** wizard appears.</span></span>  <span data-ttu-id="618dd-144">Ha nem szeretné, hogy toopublish azonnal, hello varázsló bezárásához.</span><span class="sxs-lookup"><span data-stu-id="618dd-144">If you do not want toopublish immediately, close hello wizard.</span></span> <span data-ttu-id="618dd-145">a megadott hello-beállítások a lesznek mentve, ha szeretné, hogy túl[hello projekt telepítése](#deploy).</span><span class="sxs-lookup"><span data-stu-id="618dd-145">hello settings that you've entered are saved for when you do want too[deploy hello project](#deploy).</span></span>

## <span data-ttu-id="618dd-146"><a id="create"></a>WebJobs-kompatibilis új projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="618dd-146"><a id="create"></a>Create a new WebJobs-enabled project</span></span>
<span data-ttu-id="618dd-147">toocreate WebJobs-kompatibilis új projekt, használhatja hello Konzolalkalmazás projekt sablon engedélyezése webjobs-feladatok központi és a [hello az előző szakaszban](#convert).</span><span class="sxs-lookup"><span data-stu-id="618dd-147">toocreate a new WebJobs-enabled project, you can use hello Console Application project template and enable WebJobs deployment as explained in [hello previous section](#convert).</span></span> <span data-ttu-id="618dd-148">Alternatív megoldásként hello webjobs-feladatok – új projekt sablont is használhatja:</span><span class="sxs-lookup"><span data-stu-id="618dd-148">As an alternative, you can use hello WebJobs new-project template:</span></span>

* [<span data-ttu-id="618dd-149">Hello webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="618dd-149">Use hello WebJobs new-project template for an independent WebJob</span></span>](#createnolink)
  
    <span data-ttu-id="618dd-150">A projekt létrehozása és a toodeploy önmagában egy webjobs-feladat, nincs hivatkozás tooa webes projekt esetében.</span><span class="sxs-lookup"><span data-stu-id="618dd-150">Create a project and configure it toodeploy by itself as a WebJob, with no link tooa web project.</span></span> <span data-ttu-id="618dd-151">Használja ezt a beállítást, ha azt szeretné, a webes alkalmazás a webjobs-feladat toorun önmagában, nincs hello webalkalmazásban futó webes alkalmazásokkal együtt.</span><span class="sxs-lookup"><span data-stu-id="618dd-151">Use this option when you want toorun a WebJob in a web app by itself, with no web application running in hello web app.</span></span> <span data-ttu-id="618dd-152">Érdemes lehet toodo Ez a rendezés toobe képes tooscale a webjobs-feladat erőforrásokat, függetlenül a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="618dd-152">You might want toodo this in order toobe able tooscale your WebJob resources independently of your web application resources.</span></span>
* [<span data-ttu-id="618dd-153">A webjobs-feladat csatolt tooa webes projekt hello webjobs-feladatok – új projekt sablon használata</span><span class="sxs-lookup"><span data-stu-id="618dd-153">Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>](#createlink)
  
    <span data-ttu-id="618dd-154">A konfigurált toodeploy automatikusan webjobs-feladat során ugyanazon megoldást már telepítették hello egy webes projekt projekt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="618dd-154">Create a project that is configured toodeploy automatically as a WebJob when a web project in hello same solution is deployed.</span></span> <span data-ttu-id="618dd-155">Használja ezt a beállítást, ha azt szeretné, toorun a webjobs-feladat a hello hello futtassa azonos webalkalmazás kapcsolódó webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="618dd-155">Use this option when you want toorun your WebJob in hello same web app in which you run hello related web application.</span></span>

> [!NOTE]
> <span data-ttu-id="618dd-156">hello webjobs-feladatok – új projekt sablon automatikusan telepíti a NuGet-csomagok, és magában foglalja a kód *Program.cs* a hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span><span class="sxs-lookup"><span data-stu-id="618dd-156">hello WebJobs new-project template automatically installs NuGet packages and includes code in *Program.cs* for hello [WebJobs SDK](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/getting-started-with-windows-azure-webjobs).</span></span> <span data-ttu-id="618dd-157">Ha nem szeretné toouse hello WebJobs SDK, távolítsa el, vagy módosítsa a hello `host.RunAndBlock` utasítás *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="618dd-157">If you don't want toouse hello WebJobs SDK, remove or change hello `host.RunAndBlock` statement in *Program.cs*.</span></span>
> 
> 

### <span data-ttu-id="618dd-158"><a id="createnolink"></a>Hello webjobs-feladatok – új projekt sablon használata az egy független webjobs-feladat</span><span class="sxs-lookup"><span data-stu-id="618dd-158"><a id="createnolink"></a> Use hello WebJobs new-project template for an independent WebJob</span></span>
1. <span data-ttu-id="618dd-159">Kattintson a **fájl** > **új projekt**, majd a hello **új projekt** párbeszédpanelen kattintson **felhő**  >   **Azure webjobs-feladat (.NET-keretrendszer)**.</span><span class="sxs-lookup"><span data-stu-id="618dd-159">Click **File** > **New Project**, and then in hello **New Project** dialog box click **Cloud** > **Azure WebJob (.NET Framework)**.</span></span>
   
    ![Webjobs-feladat sablon új projekt párbeszédpanel](./media/websites-dotnet-deploy-webjobs/np.png)
2. <span data-ttu-id="618dd-161">Kövesse a korábban bemutatott hello irányban túl[ellenőrizze a Konzolalkalmazás-projektet egy független WebJobs-projekt hello](#convertnolink).</span><span class="sxs-lookup"><span data-stu-id="618dd-161">Follow hello directions shown earlier too[make hello Console Application project an independent WebJobs project](#convertnolink).</span></span>

### <span data-ttu-id="618dd-162"><a id="createlink"></a>A webjobs-feladat csatolt tooa webes projekt hello webjobs-feladatok – új projekt sablon használata</span><span class="sxs-lookup"><span data-stu-id="618dd-162"><a id="createlink"></a> Use hello WebJobs new-project template for a WebJob linked tooa web project</span></span>
1. <span data-ttu-id="618dd-163">Kattintson a jobb gombbal hello webes projekt **Megoldáskezelőben**, és kattintson a **Hozzáadás** > **új Azure webjobs-feladat projekt**.</span><span class="sxs-lookup"><span data-stu-id="618dd-163">Right-click hello web project in **Solution Explorer**, and then click **Add** > **New Azure WebJob Project**.</span></span>
   
    ![Új Azure webjobs-feladat projekt menübejegyzést](./media/websites-dotnet-deploy-webjobs/nawj.png)
   
    <span data-ttu-id="618dd-165">Hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="618dd-165">hello [Add Azure WebJob](#configure) dialog box appears.</span></span>
2. <span data-ttu-id="618dd-166">Teljes hello [adja hozzá a Azure webjobs-feladat](#configure) párbeszédpanel, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="618dd-166">Complete hello [Add Azure WebJob](#configure) dialog box, and then click **OK**.</span></span>

## <span data-ttu-id="618dd-167"><a id="configure"></a>hello Azure Webjobs hozzáadása párbeszédpanel</span><span class="sxs-lookup"><span data-stu-id="618dd-167"><a id="configure"></a>hello Add Azure WebJob dialog</span></span>
<span data-ttu-id="618dd-168">Hello **adja hozzá a Azure webjobs-feladat** párbeszédpanelen lehetővé teszi, hogy adja meg a hello webjobs-feladat neve és mód beállítása a webjobs-feladat futtatása.</span><span class="sxs-lookup"><span data-stu-id="618dd-168">hello **Add Azure WebJob** dialog lets you enter hello WebJob name and run mode setting for your WebJob.</span></span> 

![Azure webjobs-feladat párbeszédpanel hozzáadása](./media/websites-dotnet-deploy-webjobs/aaw2.png)

<span data-ttu-id="618dd-170">Ezen a párbeszédpanelen hello mezők felel meg a hello toofields **új feladat** hello Azure Portal párbeszédpanelén.</span><span class="sxs-lookup"><span data-stu-id="618dd-170">hello fields in this dialog correspond toofields on hello **New Job** dialog of hello Azure Portal.</span></span> <span data-ttu-id="618dd-171">További információkért lásd: [háttérfeladatok futtatása a webjobs-feladatok](web-sites-create-web-jobs.md).</span><span class="sxs-lookup"><span data-stu-id="618dd-171">For more information, see [Run Background tasks with WebJobs](web-sites-create-web-jobs.md).</span></span>

> [!NOTE]
> * <span data-ttu-id="618dd-172">Parancssori telepítéssel kapcsolatos információkért lásd: [parancssori engedélyezése vagy folyamatos kézbesítését az Azure webjobs-feladatok](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span><span class="sxs-lookup"><span data-stu-id="618dd-172">For information about command-line deployment, see [Enabling Command-line or Continuous Delivery of Azure WebJobs](https://azure.microsoft.com/blog/2014/08/18/enabling-command-line-or-continuous-delivery-of-azure-webjobs/).</span></span>
> * <span data-ttu-id="618dd-173">Ha telepíti a webjobs-feladat, majd döntse el, toochange hello típusú webjobs-feladat, és helyezze üzembe újra, toodelete hello webjobs-feladatok közzététele settings.json fájl lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="618dd-173">If you deploy a WebJob and then decide you want toochange hello type of WebJob and redeploy, you'll need toodelete hello webjobs-publish-settings.json file.</span></span> <span data-ttu-id="618dd-174">A Visual Studio megjelenítése hello közzétételi beállítások újra, így módosíthatja a webjobs-feladat hello típusú működésképtelenné teszi.</span><span class="sxs-lookup"><span data-stu-id="618dd-174">This will make Visual Studio show hello publishing options again, so you can change hello type of WebJob.</span></span>
> * <span data-ttu-id="618dd-175">Ha egy webjobs-feladat telepíti, később módosíthatja használata a folyamatos toonon folyamatos hello, vagy fordítva, Visual Studio létrehoz egy új webjobs-feladat az Azure-ban újratelepítésekor.</span><span class="sxs-lookup"><span data-stu-id="618dd-175">If you deploy a WebJob and later change hello run mode from continuous toonon-continuous or vice versa, Visual Studio creates a new WebJob in Azure when you redeploy.</span></span> <span data-ttu-id="618dd-176">Ha további ütemezési beállítások módosítására, de hagyja futtatása mód hello azonos, vagy váltás ütemezett és igény szerinti, Visual Studio frissítések meglévő feladat hello helyett hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="618dd-176">If you change other scheduling settings but leave run mode hello same or switch between Scheduled and On Demand, Visual Studio updates hello existing job rather than create a new one.</span></span>
> 
> 

## <span data-ttu-id="618dd-177"><a id="publishsettings"></a>webjobs-feladat közzététele settings.json</span><span class="sxs-lookup"><span data-stu-id="618dd-177"><a id="publishsettings"></a>webjob-publish-settings.json</span></span>
<span data-ttu-id="618dd-178">Egy Konzolalkalmazást a WebJobs-telepítéshez való konfigurálásakor a Visual Studio telepíti hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet csomag- és ütemezési információkat tárolja egy *webjobs-feladat közzététele settings.json*  hello projektben fájl *tulajdonságok* hello WebJobs-projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="618dd-178">When you configure a Console Application for WebJobs deployment, Visual Studio installs hello [Microsoft.Web.WebJobs.Publish](http://www.nuget.org/packages/Microsoft.Web.WebJobs.Publish/) NuGet package and stores scheduling information in a *webjob-publish-settings.json* file in hello project *Properties* folder of hello WebJobs project.</span></span> <span data-ttu-id="618dd-179">Íme egy példa, hogy a fájl:</span><span class="sxs-lookup"><span data-stu-id="618dd-179">Here is an example of that file:</span></span>

        {
          "$schema": "http://schemastore.org/schemas/json/webjob-publish-settings.json",
          "webJobName": "WebJob1",
          "startTime": "null",
          "endTime": "null",
          "jobRecurrenceFrequency": "null",
          "interval": null,
          "runMode": "Continuous"
        }

<span data-ttu-id="618dd-180">Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít.</span><span class="sxs-lookup"><span data-stu-id="618dd-180">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="618dd-181">hello fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) nem lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="618dd-181">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjob-publish-settings.json) and can be viewed there.</span></span>  

## <span data-ttu-id="618dd-182"><a id="webjobslist"></a>webjobs-list.json</span><span class="sxs-lookup"><span data-stu-id="618dd-182"><a id="webjobslist"></a>webjobs-list.json</span></span>
<span data-ttu-id="618dd-183">WebJobs-kompatibilis projekt tooa webes projektet kapcsolódáskor a Visual Studio hello WebJobs projekt hello nevét tárolja egy *webjobs-list.json* hello webes projekt fájl *tulajdonságok* mappát.</span><span class="sxs-lookup"><span data-stu-id="618dd-183">When you link a WebJobs-enabled project tooa web project, Visual Studio stores hello name of hello WebJobs project in a *webjobs-list.json* file in hello web project's *Properties* folder.</span></span> <span data-ttu-id="618dd-184">hello listát tartalmazhat több WebJobs-projektet, ahogy az alábbi példa hello:</span><span class="sxs-lookup"><span data-stu-id="618dd-184">hello list might contain multiple WebJobs projects, as shown in hello following example:</span></span>

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

<span data-ttu-id="618dd-185">Közvetlenül szerkesztheti a fájlt, és a Visual Studio IntelliSense biztosít.</span><span class="sxs-lookup"><span data-stu-id="618dd-185">You can edit this file directly, and Visual Studio provides IntelliSense.</span></span> <span data-ttu-id="618dd-186">hello fájl séma tárolódik [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) nem lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="618dd-186">hello file schema is stored at [http://schemastore.org](http://schemastore.org/schemas/json/webjobs-list.json) and can be viewed there.</span></span>

## <span data-ttu-id="618dd-187"><a id="deploy"></a>Webjobs-feladatok projekt telepítése</span><span class="sxs-lookup"><span data-stu-id="618dd-187"><a id="deploy"></a>Deploy a WebJobs project</span></span>
<span data-ttu-id="618dd-188">A WebJobs-projekt csatolta tooa webes projekt automatikusan hello webes projekt telepíti.</span><span class="sxs-lookup"><span data-stu-id="618dd-188">A WebJobs project that you have linked tooa web project deploys automatically with hello web project.</span></span> <span data-ttu-id="618dd-189">A webes projekt telepítése kapcsolatos információkért lásd: [hogyan toodeploy tooWeb alkalmazások](web-sites-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="618dd-189">For information about web project deployment, see [How toodeploy tooWeb Apps](web-sites-deploy.md).</span></span>

<span data-ttu-id="618dd-190">toodeploy a webjobs-feladatok projekt önmagában, kattintson a jobb gombbal a projekt hello **Megoldáskezelőben** kattintson **Azure webjobs-feladat közzététel...** .</span><span class="sxs-lookup"><span data-stu-id="618dd-190">toodeploy a WebJobs project by itself, right-click hello project in **Solution Explorer** and click **Publish as Azure WebJob...**.</span></span> 

![Közzététel az Azure webjobs-feladat](./media/websites-dotnet-deploy-webjobs/paw.png)

<span data-ttu-id="618dd-192">Egy független webjobs-feladat, az azonos hello **webhely közzététele** webes projektek megjelenik, de kevesebb beállítások elérhető toochange varázslójától.</span><span class="sxs-lookup"><span data-stu-id="618dd-192">For an independent WebJob, hello same **Publish Web** wizard that is used for web projects appears, but with fewer settings available toochange.</span></span>

## <span data-ttu-id="618dd-193"><a id="nextsteps"></a>Következő lépések</span><span class="sxs-lookup"><span data-stu-id="618dd-193"><a id="nextsteps"></a>Next Steps</span></span>
<span data-ttu-id="618dd-194">Ez a cikk szerint hogyan toodeploy webjobs-feladatok Visual Studio használatával.</span><span class="sxs-lookup"><span data-stu-id="618dd-194">This article has explained how toodeploy WebJobs by using Visual Studio.</span></span> <span data-ttu-id="618dd-195">További információ a hogyan toodeploy Azure WebJobs: [Azure WebJobs - erőforrások – ajánlott telepítési](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span><span class="sxs-lookup"><span data-stu-id="618dd-195">For more information about how toodeploy Azure WebJobs, see [Azure WebJobs - Recommended Resources - Deployment](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/azure-webjobs-recommended-resources#deploying).</span></span>

