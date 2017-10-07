---
title: a Visual Studio - Azure Logic Apps aaaManage a logic apps |} Microsoft Docs
description: "A logic apps és az egyéb Azure eszközök Visual Studio Cloud Explorer kezelése"
author: klam
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 12/19/2016
ms.author: LADocs; klam
ms.openlocfilehash: 419f83eb062b56e4ac2642dea4de1a025f747521
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-your-logic-apps-with-visual-studio-cloud-explorer"></a><span data-ttu-id="3972c-103">A Visual Studio Cloud Explorer a logic Apps alkalmazásokat kezeléséhez</span><span class="sxs-lookup"><span data-stu-id="3972c-103">Manage your logic apps with Visual Studio Cloud Explorer</span></span>

<span data-ttu-id="3972c-104">Bár hello [Azure-portálon](https://portal.azure.com/) nagyszerű lehetőséget nyújt az Ön toodesign és kezelése az Azure Logic Apps, használhatja a Visual Studio Cloud Explorer sok Azure eszközök, beleértve a logic Apps alkalmazásokat kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="3972c-104">Although hello [Azure portal](https://portal.azure.com/) offers a great way for you toodesign and manage Azure Logic Apps, you can use Visual Studio Cloud Explorer for managing many Azure assets, including logic apps.</span></span> <span data-ttu-id="3972c-105">Visual Studio Cloud Explorer lehetővé teszi, hogy a Tallózás gombra, kezeléséhez, szerkesztése, és letöltés a logic apps közzé.</span><span class="sxs-lookup"><span data-stu-id="3972c-105">Visual Studio Cloud Explorer lets you browse, manage, edit, and download published logic apps.</span></span> <span data-ttu-id="3972c-106">Felügyeleti feladatok előzményeinek megtekintése, futtassa, engedélyezése és letiltása.</span><span class="sxs-lookup"><span data-stu-id="3972c-106">Management tasks include enable, disable, and view run history.</span></span> 

<span data-ttu-id="3972c-107">Mielőtt eléréséhez, és a Visual Studio a logic Apps alkalmazásokat kezeléséhez, telepítése és a Visual Studio eszközök konfigurálása az Azure Logic Apps.</span><span class="sxs-lookup"><span data-stu-id="3972c-107">Before you can access and manage your logic apps in Visual Studio, install and configure these Visual Studio tools for Azure Logic Apps.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="3972c-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3972c-108">Prerequisites</span></span>

* [<span data-ttu-id="3972c-109">Visual Studio 2015-öt vagy a Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="3972c-109">Visual Studio 2015 or Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs.aspx)
* <span data-ttu-id="3972c-110">[Legfrissebb Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="3972c-110">[Latest Azure SDK](https://azure.microsoft.com/downloads/) (2.9.1 or greater)</span></span>
* [<span data-ttu-id="3972c-111">A Visual Studio Cloud Explorer</span><span class="sxs-lookup"><span data-stu-id="3972c-111">Visual Studio Cloud Explorer</span></span>](https://marketplace.visualstudio.com/items?itemName=MicrosoftCloudExplorer.CloudExplorerforVisualStudio2015)
* <span data-ttu-id="3972c-112">Hozzáférés toohello webes hello beágyazott designer használata esetén</span><span class="sxs-lookup"><span data-stu-id="3972c-112">Access toohello web when using hello embedded designer</span></span>

## <a name="install-visual-studio-tools-for-logic-apps"></a><span data-ttu-id="3972c-113">A Logic Apps a Visual Studio eszközök telepítése</span><span class="sxs-lookup"><span data-stu-id="3972c-113">Install Visual Studio tools for Logic Apps</span></span>

<span data-ttu-id="3972c-114">Hello előfeltételek telepítése után töltse le, és a Visual Studio hello Azure Logic Apps eszközök telepítése.</span><span class="sxs-lookup"><span data-stu-id="3972c-114">After you install hello prerequisites, download and install hello Azure Logic Apps Tools for Visual Studio.</span></span>

1. <span data-ttu-id="3972c-115">Nyissa meg a Visual Studiót.</span><span class="sxs-lookup"><span data-stu-id="3972c-115">Open Visual Studio.</span></span> <span data-ttu-id="3972c-116">A hello **eszközök** menü **bővítmények és frissítések**.</span><span class="sxs-lookup"><span data-stu-id="3972c-116">On hello **Tools** menu, select **Extensions and Updates**.</span></span>
2. <span data-ttu-id="3972c-117">Bontsa ki a hello **Online** kategória hello Visual Studio galériában online kereséséhez.</span><span class="sxs-lookup"><span data-stu-id="3972c-117">Expand hello **Online** category so you can search online in hello Visual Studio Gallery.</span></span>
3. <span data-ttu-id="3972c-118">Keresse meg **Logic Apps** amíg meg nem látja **Azure Logic Apps Tools for Visual Studio**.</span><span class="sxs-lookup"><span data-stu-id="3972c-118">Browse or search for **Logic Apps** until you find **Azure Logic Apps Tools for Visual Studio**.</span></span>
4. <span data-ttu-id="3972c-119">toodownload és a telepítés hello kiterjesztéssel, kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="3972c-119">toodownload and install hello extension, click **Download**.</span></span>
5. <span data-ttu-id="3972c-120">Indítsa újra a Visual Studio telepítése után.</span><span class="sxs-lookup"><span data-stu-id="3972c-120">Restart Visual Studio after installation.</span></span>

> [!NOTE]
> <span data-ttu-id="3972c-121">toodownload hello Azure Logic Apps Tools for Visual Studio közvetlenül, nyissa meg toohello [Visual Studio piactér](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span><span class="sxs-lookup"><span data-stu-id="3972c-121">toodownload hello Azure Logic Apps Tools for Visual Studio directly, go toohello [Visual Studio Marketplace](https://visualstudiogallery.msdn.microsoft.com/e25ad307-46cf-412e-8ba5-5b555d53d2d9).</span></span>

## <a name="browse-for-logic-apps-in-cloud-explorer"></a><span data-ttu-id="3972c-122">Tallózással keresse meg a logic apps Cloud Explorerben</span><span class="sxs-lookup"><span data-stu-id="3972c-122">Browse for logic apps in Cloud Explorer</span></span>

1.  <span data-ttu-id="3972c-123">tooopen Cloud Explorer, a hello **nézet** menüben válasszon **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="3972c-123">tooopen Cloud Explorer, on hello **View** menu, choose **Cloud Explorer**.</span></span>
2.  <span data-ttu-id="3972c-124">Tallózással keresse meg a Logic Apps alkalmazást, erőforráscsoporthoz vagy erőforrástípus.</span><span class="sxs-lookup"><span data-stu-id="3972c-124">Browse for your logic app, either by resource group or by resource type.</span></span> 

    * <span data-ttu-id="3972c-125">Ha tallózással erőforrástípusok szerint, jelölje ki az Azure-előfizetéshez, bontsa ki a hello **Logic Apps** szakaszt, és válassza ki a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3972c-125">If you browse by resource type, select your Azure subscription, expand hello **Logic Apps** section, and select your logic app.</span></span> 
    * <span data-ttu-id="3972c-126">Ha tallózással erőforráscsoport, bontsa ki a Logic Apps alkalmazást tartalmazó erőforráscsoportot hello, és válassza ki a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="3972c-126">If you browse by resource group, expand hello resource group that has your logic app, and select your logic app.</span></span>

    <span data-ttu-id="3972c-127">tooview parancsok a logikai alkalmazásnak, vagy kattintson a jobb gombbal a Logic Apps alkalmazást, vagy a Cloud Explorer hello alján hello választhat **műveletek** menü.</span><span class="sxs-lookup"><span data-stu-id="3972c-127">tooview commands for your logic app, either right-click your logic app, or at hello bottom of Cloud Explorer, choose from hello **Actions** menu.</span></span>

    ![Tallózással keresse meg a Logic Apps alkalmazást](./media/logic-apps-manage-from-vs/browse.png)

## <a name="edit-your-logic-app-with-logic-apps-designer"></a><span data-ttu-id="3972c-129">A Logic Apps alkalmazást a Logic Apps-tervezővel szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3972c-129">Edit your logic app with Logic Apps Designer</span></span>

<span data-ttu-id="3972c-130">A Cloud Explorer megnyitható a most üzembe helyezett logikai alkalmazást a hello azonos designer, amelyek hello Azure-portál használatával.</span><span class="sxs-lookup"><span data-stu-id="3972c-130">From Cloud Explorer, you can open a currently deployed logic app in hello same designer that you use in hello Azure portal.</span></span> 

* <span data-ttu-id="3972c-131">tooedit a Logic Apps alkalmazást, a Cloud Explorerben (Megoldáskezelőben) kattintson a jobb gombbal a Logic Apps alkalmazást, és válassza ki **nyissa meg a Logic App szerkesztő**.</span><span class="sxs-lookup"><span data-stu-id="3972c-131">tooedit your logic app, in Cloud Explorer, right-click your logic app, and select **Open with Logic App Editor**.</span></span> 

* <span data-ttu-id="3972c-132">a felhő a frissítések toohello toopublish, válassza a **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="3972c-132">toopublish your updates toohello cloud, choose **Publish**.</span></span> 

* <span data-ttu-id="3972c-133">Válasszon egy új futtató toostart **futtatása eseményindító**.</span><span class="sxs-lookup"><span data-stu-id="3972c-133">toostart a new run, choose **Run Trigger**.</span></span>

![Logic Apps-Tervező](./media/logic-apps-manage-from-vs/designer.png)

<span data-ttu-id="3972c-135">Hello designer alkalmazásból is **letöltése** logikai alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3972c-135">From hello designer, you can also **Download** a logic app.</span></span> <span data-ttu-id="3972c-136">Ez a művelet automatikusan parameterizes hello logic app-definíciót, majd hello definition menti az Azure Resource Manager központi telepítési sablont.</span><span class="sxs-lookup"><span data-stu-id="3972c-136">This action automatically parameterizes hello logic app definition, and saves hello definition as an Azure Resource Manager deployment template.</span></span> <span data-ttu-id="3972c-137">A központi telepítési sablon tooyour Azure erőforráscsoport-projekt adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="3972c-137">You can add this deployment template tooyour Azure Resource Group project.</span></span>

## <a name="browse-your-logic-app-run-history"></a><span data-ttu-id="3972c-138">Keresse meg a Logic Apps alkalmazást futtatási előzményei</span><span class="sxs-lookup"><span data-stu-id="3972c-138">Browse your logic app run history</span></span>

<span data-ttu-id="3972c-139">tooview hello futtatási előzményei a logikai alkalmazásnak, kattintson a jobb gombbal a Logic Apps alkalmazást, és válassza ki **nyílt futtatási előzményei**.</span><span class="sxs-lookup"><span data-stu-id="3972c-139">tooview hello run history for your logic app, right-click your logic app, and select **Open run history**.</span></span> <span data-ttu-id="3972c-140">tooreorder a futtatási előzményei alapján hello tulajdonságok látható, jelölje be hello oszlop fejlécére.</span><span class="sxs-lookup"><span data-stu-id="3972c-140">tooreorder your run history based on any of hello properties shown, select hello column header.</span></span>

![futtatási előzményei](media/logic-apps-manage-from-vs/runs.png)

<span data-ttu-id="3972c-142">futtatási előzményei-példányok esetében, áttekintheti az eredményeket, többek között a hello bemenetekhez és kimenetekhez minden lépésben hello tooshow hello kattintson duplán a hello példányán fusson.</span><span class="sxs-lookup"><span data-stu-id="3972c-142">tooshow hello run history for an instance so you can review hello run results, including hello inputs and outputs from each step, double-click one of hello run instances.</span></span>

![Futtassa a előzménylistát, bemenetei és kimenetei lépéseiből](./media/logic-apps-manage-from-vs/history.png)

## <a name="next-steps"></a><span data-ttu-id="3972c-144">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3972c-144">Next steps</span></span>

* [<span data-ttu-id="3972c-145">Az első logikai alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3972c-145">Create your first logic app</span></span>](logic-apps-create-a-logic-app.md)
* [<span data-ttu-id="3972c-146">Tervezési, elkészítéséhez és logikai alkalmazások telepítése a Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3972c-146">Design, build, and deploy logic apps in Visual Studio</span></span>](logic-apps-deploy-from-vs.md)
* [<span data-ttu-id="3972c-147">Gyakori példák és felhasználási helyzetek megtekintése</span><span class="sxs-lookup"><span data-stu-id="3972c-147">View common examples and scenarios</span></span>](logic-apps-examples-and-scenarios.md)
* [<span data-ttu-id="3972c-148">Videó: Az Azure Logic Apps üzleti folyamatok automatizálása</span><span class="sxs-lookup"><span data-stu-id="3972c-148">Video: Automate business processes with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/T694)
* [<span data-ttu-id="3972c-149">Videó: Rendszerintegráció az Azure Logic Apps</span><span class="sxs-lookup"><span data-stu-id="3972c-149">Video: Integrate your systems with Azure Logic Apps</span></span>](http://channel9.msdn.com/Events/Build/2016/P462)
