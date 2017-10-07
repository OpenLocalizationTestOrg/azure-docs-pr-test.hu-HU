---
title: "az Azure Logic Apps helyszíni kiszolgálón tárolt olyan adatforrások aaaAccess |} Microsoft Docs"
description: "Hello helyszíni adatátjáró beállítása, hogy hozzáférhessen a helyszíni adatforrások a logic Apps alkalmazásokból"
keywords: "adatok, a helyszíni, az adatok átvitele, a titkosítás és a adatforrások eléréséhez"
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: 6cb4449d-e6b8-4c35-9862-15110ae73e6a
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 1d3deaac5a095316ce78e224dab0c08559bc2ff2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-hello-on-premises-data-gateway"></a><span data-ttu-id="7de98-104">Hozzáférés a helyszíni a logic Apps alkalmazásokból hello a helyszíni adatok átjáróval adatforrásokhoz</span><span class="sxs-lookup"><span data-stu-id="7de98-104">Access data sources on premises from logic apps with hello on-premises data gateway</span></span>

<span data-ttu-id="7de98-105">a logic apps a helyi kiszolgálón tárolt olyan adatforrások tooaccess állítson be egy helyszíni adatátjárót, támogatott összekötők logic Apps alkalmazásokat használhat.</span><span class="sxs-lookup"><span data-stu-id="7de98-105">tooaccess data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="7de98-106">hello átjáró működik hídként, amelyen a gyors adatátvitel és a helyszíni adatforrások és a logic Apps alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="7de98-106">hello gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="7de98-107">hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="7de98-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="7de98-108">Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="7de98-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="7de98-109">További információ [hello adatátjáró működése](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="7de98-109">Learn more about [how hello data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="7de98-110">hello átjárót a helyszíni kapcsolatok toothese adatforrások támogatja:</span><span class="sxs-lookup"><span data-stu-id="7de98-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="7de98-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="7de98-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="7de98-112">DB2</span><span class="sxs-lookup"><span data-stu-id="7de98-112">DB2</span></span>  
*   <span data-ttu-id="7de98-113">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="7de98-113">File System</span></span>
*   <span data-ttu-id="7de98-114">Informix</span><span class="sxs-lookup"><span data-stu-id="7de98-114">Informix</span></span>
*   <span data-ttu-id="7de98-115">MQ</span><span class="sxs-lookup"><span data-stu-id="7de98-115">MQ</span></span>
*   <span data-ttu-id="7de98-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="7de98-116">MySQL</span></span>
*   <span data-ttu-id="7de98-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="7de98-117">Oracle Database</span></span>
*   <span data-ttu-id="7de98-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="7de98-118">PostgreSQL</span></span>
*   <span data-ttu-id="7de98-119">SAP-alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="7de98-119">SAP Application Server</span></span> 
*   <span data-ttu-id="7de98-120">SAP üzenet kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="7de98-120">SAP Message Server</span></span>
*   <span data-ttu-id="7de98-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="7de98-121">SharePoint</span></span>
*   <span data-ttu-id="7de98-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="7de98-122">SQL Server</span></span>
*   <span data-ttu-id="7de98-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="7de98-123">Teradata</span></span>

<span data-ttu-id="7de98-124">Ezeket a lépéseket hogyan tooset hello fel a helyszíni adatok átjáró toowork a a logic apps megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7de98-124">These steps show how tooset up hello on-premises data gateway toowork with your logic apps.</span></span> <span data-ttu-id="7de98-125">További információ a támogatott összekötők: [az Azure Logic Apps összekötők](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="7de98-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="7de98-126">Hogyan toouse hello átjáró más szolgáltatásokkal kapcsolatos információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="7de98-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="7de98-127">Microsoft Power BI helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="7de98-128">Az Azure Analysis Services helyi adatátjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="7de98-129">Microsoft Flow helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="7de98-130">Microsoft PowerApps helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="7de98-131">Követelmények</span><span class="sxs-lookup"><span data-stu-id="7de98-131">Requirements</span></span>

* <span data-ttu-id="7de98-132">Már rendelkeznie kell [hello adatátjáró telepítve a helyi számítógép](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7de98-132">You must have already [installed hello data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="7de98-133">Bejelentkezéskor toohello Azure-portálon, hogy rendelkezik-e toouse hello ugyanazzal a munkahelyi vagy iskolai fiókkal, amely túl lett megadva[hello helyszíni adatátjáró telepítése](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="7de98-133">When you sign in toohello Azure portal, you have toouse hello same work or school account that was used too[install hello on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="7de98-134">A bejelentkezési fiók is rendelkeznie kell egy Azure-előfizetés toouse létrehozásakor egy átjáró-erőforráshoz hello Azure-portálon az átjáró telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="7de98-134">Your sign-in account must also have an Azure subscription toouse when you create a gateway resource in hello Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="7de98-135">Az átjáró telepítése már nem engedte, hogy az Azure átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="7de98-136">Az átjáró telepítési tooonly egy Azure átjáró-erőforráshoz lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="7de98-136">You can associate your gateway installation tooonly one Azure gateway resource.</span></span> <span data-ttu-id="7de98-137">Hozza létre a hello átjáró erőforrás hello telepítési nem érhető el az egyéb erőforrások jogcím történik.</span><span class="sxs-lookup"><span data-stu-id="7de98-137">Claim happens when you create hello gateway resource so that hello installation is unavailable for other resources.</span></span>

## <a name="set-up-hello-data-gateway-connection"></a><span data-ttu-id="7de98-138">Hello data gateway kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="7de98-138">Set up hello data gateway connection</span></span>

### <a name="1-install-hello-on-premises-data-gateway"></a><span data-ttu-id="7de98-139">1. Hello helyszíni adatátjáró telepítése</span><span class="sxs-lookup"><span data-stu-id="7de98-139">1. Install hello on-premises data gateway</span></span>

<span data-ttu-id="7de98-140">Ha még nem tette meg, kövesse a hello [lépéseket tooinstall hello helyszíni adatátjáró](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7de98-140">If you haven't already, follow hello [steps tooinstall hello on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="7de98-141">Hello folytatása előtt más lépéseket, győződjön meg arról, hogy a helyi számítógépen telepítve hello adatátjáró.</span><span class="sxs-lookup"><span data-stu-id="7de98-141">Before you continue with hello other steps, make sure that you installed hello data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-hello-on-premises-data-gateway"></a><span data-ttu-id="7de98-142">2. Hozzon létre egy Azure-erőforrás hello a helyszíni adatok átjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-142">2. Create an Azure resource for hello on-premises data gateway</span></span>

<span data-ttu-id="7de98-143">Telepítése után hello átjáró a helyi számítógépen, létre kell hoznia az Ön data gateway, az Azure-ban erőforrásként.</span><span class="sxs-lookup"><span data-stu-id="7de98-143">After you install hello gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="7de98-144">Ebben a lépésben az Azure-előfizetéshez is társítja az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="7de98-145">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com "Azure-portálon").</span><span class="sxs-lookup"><span data-stu-id="7de98-145">Sign in toohello [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="7de98-146">Győződjön meg arról, hogy toouse hello Azure ugyanazzal a munkahelyi vagy iskolai e-mail címét használja a tooinstall hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="7de98-146">Make sure toouse hello same Azure work or school email address used tooinstall hello gateway.</span></span>

2. <span data-ttu-id="7de98-147">Hello bal oldali menüben az Azure-ban, válassza ki a **új** > **vállalati integrációs** > **helyszíni adatátjáró** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="7de98-147">On hello left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   !["A helyszíni adatok gateway" található](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="7de98-149">A hello **kapcsolat átjáró létrehozása** panelen adja meg a részleteket toocreate a data gateway erőforrás:</span><span class="sxs-lookup"><span data-stu-id="7de98-149">On hello **Create connection gateway** blade, provide these details toocreate your data gateway resource:</span></span>

    * <span data-ttu-id="7de98-150">**Név**: Adjon meg egy nevet az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="7de98-151">**Előfizetés**: Válasszon hello Azure-előfizetés tooassociate az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-151">**Subscription**: Select hello Azure subscription tooassociate with your gateway resource.</span></span> 
    <span data-ttu-id="7de98-152">Ehhez az előfizetéshez kell hello a Logic Apps alkalmazást tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="7de98-152">This subscription should be hello same subscription as your logic app.</span></span>
   
      <span data-ttu-id="7de98-153">hello alapértelmezett előfizetés hello Azure-fiókot, amelyet a toosign használt alapul.</span><span class="sxs-lookup"><span data-stu-id="7de98-153">hello default subscription is based on hello Azure account that you used toosign in.</span></span>

    * <span data-ttu-id="7de98-154">**Erőforráscsoport**: hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot üzembe helyezéséhez az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="7de98-155">Erőforráscsoportok segítségével gyűjteményként kapcsolódó Azure eszközöket kezelheti.</span><span class="sxs-lookup"><span data-stu-id="7de98-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="7de98-156">**Hely**: Azure korlátozza a hely toohello ugyanabban a régióban hello átjáró felhőszolgáltatás során kiválasztott [átjáró telepítésének](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="7de98-156">**Location**: Azure restricts this location toohello same region that was selected for hello gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="7de98-157">Győződjön meg arról, hogy hello átjáró erőforrás helye megegyezik-e a hello átjáró felhőalapú szolgáltatás helyét.</span><span class="sxs-lookup"><span data-stu-id="7de98-157">Make sure that hello gateway resource location matches hello gateway cloud service location.</span></span> <span data-ttu-id="7de98-158">Ellenkező esetben az átjáró telepítése nem jelenik meg az Ön tooselect hello telepített átjárók listában hello következő lépésben.</span><span class="sxs-lookup"><span data-stu-id="7de98-158">Otherwise, your gateway installation might not appear in hello installed gateways list for you tooselect in hello next step.</span></span>
      > 
      > <span data-ttu-id="7de98-159">Különböző régiókban is használhatja, az átjáró erőforrás és a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="7de98-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="7de98-160">**Telepítési neve**: Ha nincs kiválasztva, akkor az átjáró telepítésének, válassza ki a korábban telepített hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="7de98-160">**Installation Name**: If your gateway installation isn't already selected, select hello gateway that you previously installed.</span></span> 

    <span data-ttu-id="7de98-161">tooadd hello átjáró erőforrás tooyour Azure irányítópultot, válassza a **PIN-kód toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="7de98-161">tooadd hello gateway resource tooyour Azure dashboard, choose **Pin toodashboard**.</span></span> 
    <span data-ttu-id="7de98-162">Amikor elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7de98-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="7de98-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="7de98-163">For example:</span></span>

    ![Adja meg a részleteket toocreate a helyszíni adatátjáró](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="7de98-165">toofind vagy nézet, az Ön data gateway bármikor hello fő Azure bal oldali menüben lépjen túl **több szolgáltatások** > **vállalati integrációs** > **a helyszíni adatokhoz Átjárók**.</span><span class="sxs-lookup"><span data-stu-id="7de98-165">toofind or view your data gateway at any time,  from hello main Azure left menu, go too  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Nyissa meg túl "Szolgáltatás", "Vállalati integrációs", "A helyszíni Data Gateways"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-toohello-on-premises-data-gateway"></a><span data-ttu-id="7de98-167">3. Csatlakozás a logic app toohello helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="7de98-167">3. Connect your logic app toohello on-premises data gateway</span></span>

<span data-ttu-id="7de98-168">Most, hogy a data gateway erőforrás elkészítette és az Azure-előfizetéshez társított erőforrás, a logic app és hello data gateway közötti kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="7de98-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and hello data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="7de98-169">Az átjáró kapcsolódási helyet már léteznie kell hello azonos régióban legyen, mint a Logic Apps alkalmazást, de használhatja a data gateway már egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="7de98-169">Your gateway connection location must exist in hello same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="7de98-170">Hozzon létre hello Azure-portálon, vagy nyissa meg a Logic Apps alkalmazást a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="7de98-170">In hello Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="7de98-171">Adjon hozzá egy összekötőt, amely támogatja a helyi kapcsolatokat, például az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7de98-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="7de98-172">Válassza ki a következő hello sorrendben, **keresztül, a helyszíni adatátjáró**, adjon meg egy egyedi kapcsolatnevet hello szükséges információkat, és válassza a hello data gateway erőforrás, amelyet az toouse.</span><span class="sxs-lookup"><span data-stu-id="7de98-172">Following hello order shown, select **Connect via on-premises data gateway**, provide a unique connection name and hello required information, and select hello data gateway resource that you want toouse.</span></span> <span data-ttu-id="7de98-173">Amikor elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="7de98-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="7de98-174">Egy egyedi kapcsolatnevet segítségével könnyen később, hogy a kapcsolat azonosítására, különösen akkor, ha több kapcsolatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="7de98-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="7de98-175">Ha alkalmazható, tartalmazzák a felhasználónévhez hello tartománynév.</span><span class="sxs-lookup"><span data-stu-id="7de98-175">If applicable, also include hello qualified domain for your username.</span></span> 

   ![Logic app és az adatok átjáró közötti kapcsolat létrehozása](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="7de98-177">Gratulálunk, az átjáró kapcsolat készen áll a a logic app toouse.</span><span class="sxs-lookup"><span data-stu-id="7de98-177">Congratulations, your gateway connection is now ready for your logic app toouse.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="7de98-178">Az átjáró kapcsolat beállításainak szerkesztése</span><span class="sxs-lookup"><span data-stu-id="7de98-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="7de98-179">Miután gateway-kapcsolatot hoz létre a Logic Apps alkalmazást, érdemes lehet toolater frissítési hello beállításait, hogy adott kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7de98-179">After you create a gateway connection for your logic app, you might want toolater update hello settings for that specific connection.</span></span>

1. <span data-ttu-id="7de98-180">toofind hello gateway-kapcsolatot:</span><span class="sxs-lookup"><span data-stu-id="7de98-180">toofind hello gateway connection:</span></span>

   * <span data-ttu-id="7de98-181">A logic app panelen hello alatt **Fejlesztőeszközök**, jelölje be **API kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="7de98-181">On hello logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="7de98-182">Hello **API kapcsolatok** ablaktábla megjeleníti azokat a Logic Apps alkalmazást, beleértve az átjáró kapcsolatokat társított összes API kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="7de98-182">hello **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Nyissa meg tooyour logikai alkalmazást, jelölje be a "API-kapcsolatok"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="7de98-184">Vagy hello fő Azure bal oldali menüben nyissa meg túl **több szolgáltatások** > **webes & Mobile Services** > **API kapcsolatok** minden API-kapcsolatok esetén többek között a következőket átjárókapcsolatokhoz, az Azure-előfizetéshez társított.</span><span class="sxs-lookup"><span data-stu-id="7de98-184">Or, from hello main Azure left menu, go too **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="7de98-185">Vagy a hello fő Azure bal oldali menüben nyissa meg túl**összes erőforrás** minden API-kommunikációhoz, beleértve az átjáró kapcsolatokat, az Azure-előfizetéshez társított.</span><span class="sxs-lookup"><span data-stu-id="7de98-185">Or, on hello main Azure left menu, go too**All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="7de98-186">Válassza ki a hello gateway-kapcsolatot, hogy szeretné, hogy tooview vagy Szerkesztés, és válassza **szerkesztése API-kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="7de98-186">Select hello gateway connection that you want tooview or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="7de98-187">Ha a frissítések nem lép érvénybe, próbálja meg [hello átjáró Windows-szolgáltatás leállításával és újraindításával](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="7de98-187">If your updates don't take effect, try [stopping and restarting hello gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="7de98-188">Váltás, vagy a helyszíni adatok átjáró erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="7de98-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="7de98-189">toocreate egy másik átjáró-erőforráshoz, rendelje hozzá az átjárót egy másik erőforráscsoportban, vagy távolítsa el a hello átjáró-erőforráshoz, az nem befolyásolja a hello átjáró telepítésének hello átjáró erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="7de98-189">toocreate a different gateway resource, associate your gateway with a different resource, or remove hello gateway resource, you can delete hello gateway resource without affecting hello gateway installation.</span></span> 

1. <span data-ttu-id="7de98-190">Hello fő Azure bal oldali menüben nyissa meg túl**összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="7de98-190">From hello main Azure left menu, go too**All resources**.</span></span> 
2. <span data-ttu-id="7de98-191">Keresse meg és jelölje ki az adatok átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7de98-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="7de98-192">Válasszon **helyszíni Data Gateway**, és a hello erőforrás eszköztárában kattintson **törlése**.</span><span class="sxs-lookup"><span data-stu-id="7de98-192">Choose **On-premises Data Gateway**, and on hello resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="7de98-193">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="7de98-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="7de98-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7de98-194">Next steps</span></span>

* [<span data-ttu-id="7de98-195">A logikai alkalmazások védelme</span><span class="sxs-lookup"><span data-stu-id="7de98-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="7de98-196">Gyakori példák és forgatókönyvek a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="7de98-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
