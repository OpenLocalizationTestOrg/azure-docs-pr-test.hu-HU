---
title: "A helyszíni adatforrások elérése az Azure Logic Apps |} Microsoft Docs"
description: "A helyszíni adatátjáró beállítása, a helyszíni adatforrások eléréséhez a logic Apps alkalmazásokból"
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
ms.openlocfilehash: 24793b83ca284fe9510fe21bc2d13b0589209d36
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="access-data-sources-on-premises-from-logic-apps-with-the-on-premises-data-gateway"></a><span data-ttu-id="aa019-104">Hozzáférés a helyszíni a logic Apps alkalmazásokból, a helyszíni adatok átjáróval adatforrásokhoz</span><span class="sxs-lookup"><span data-stu-id="aa019-104">Access data sources on premises from logic apps with the on-premises data gateway</span></span>

<span data-ttu-id="aa019-105">A helyszíni adatforrások eléréséhez a logic Apps alkalmazásokat, egy helyszíni adatátjáró, támogatott összekötők logic Apps alkalmazásokat használhat beállítása.</span><span class="sxs-lookup"><span data-stu-id="aa019-105">To access data sources on premises from your logic apps, set up an on-premises data gateway that logic apps can use with supported connectors.</span></span> <span data-ttu-id="aa019-106">Az átjáró működik hídként, amelyen a gyors adatátvitel és a helyszíni adatforrások és a logic Apps alkalmazások között.</span><span class="sxs-lookup"><span data-stu-id="aa019-106">The gateway acts as a bridge that provides quick data transfer and encryption between data sources on premises and your logic apps.</span></span> <span data-ttu-id="aa019-107">Az átjáró továbbítja a titkosított csatornákon keresztül az Azure Service Bus helyszíni forrásból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="aa019-107">The gateway relays data from on-premises sources on encrypted channels through the Azure Service Bus.</span></span> <span data-ttu-id="aa019-108">Az összes forgalom származik, az átjáró ügynök biztonságos kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="aa019-108">All traffic originates as secure outbound traffic from the gateway agent.</span></span> <span data-ttu-id="aa019-109">További információ [az átjáró működése](logic-apps-gateway-install.md#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="aa019-109">Learn more about [how the data gateway works](logic-apps-gateway-install.md#gateway-cloud-service).</span></span> 

<span data-ttu-id="aa019-110">Az átjáró a helyszíni e adatforrásokkal létesített kapcsolatokat is támogatja:</span><span class="sxs-lookup"><span data-stu-id="aa019-110">The gateway supports connections to these data sources on premises:</span></span>

*   <span data-ttu-id="aa019-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="aa019-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="aa019-112">DB2</span><span class="sxs-lookup"><span data-stu-id="aa019-112">DB2</span></span>  
*   <span data-ttu-id="aa019-113">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="aa019-113">File System</span></span>
*   <span data-ttu-id="aa019-114">Informix</span><span class="sxs-lookup"><span data-stu-id="aa019-114">Informix</span></span>
*   <span data-ttu-id="aa019-115">MQ</span><span class="sxs-lookup"><span data-stu-id="aa019-115">MQ</span></span>
*   <span data-ttu-id="aa019-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="aa019-116">MySQL</span></span>
*   <span data-ttu-id="aa019-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="aa019-117">Oracle Database</span></span>
*   <span data-ttu-id="aa019-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="aa019-118">PostgreSQL</span></span>
*   <span data-ttu-id="aa019-119">SAP-alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="aa019-119">SAP Application Server</span></span> 
*   <span data-ttu-id="aa019-120">SAP üzenet kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="aa019-120">SAP Message Server</span></span>
*   <span data-ttu-id="aa019-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="aa019-121">SharePoint</span></span>
*   <span data-ttu-id="aa019-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="aa019-122">SQL Server</span></span>
*   <span data-ttu-id="aa019-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="aa019-123">Teradata</span></span>

<span data-ttu-id="aa019-124">Ezeket a lépéseket a helyszíni adatátjáró beállítása a logic apps használható szemléltetik.</span><span class="sxs-lookup"><span data-stu-id="aa019-124">These steps show how to set up the on-premises data gateway to work with your logic apps.</span></span> <span data-ttu-id="aa019-125">További információ a támogatott összekötők: [az Azure Logic Apps összekötők](../connectors/apis-list.md).</span><span class="sxs-lookup"><span data-stu-id="aa019-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](../connectors/apis-list.md).</span></span> 

<span data-ttu-id="aa019-126">Az átjáró más szolgáltatásokkal való használatával kapcsolatos információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="aa019-126">For information about how to use the gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="aa019-127">Microsoft Power BI helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="aa019-128">Az Azure Analysis Services helyi adatátjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="aa019-129">Microsoft Flow helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="aa019-130">Microsoft PowerApps helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

## <a name="requirements"></a><span data-ttu-id="aa019-131">Követelmények</span><span class="sxs-lookup"><span data-stu-id="aa019-131">Requirements</span></span>

* <span data-ttu-id="aa019-132">Már rendelkeznie kell [az átjáró telepítve a helyi számítógép](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="aa019-132">You must have already [installed the data gateway on a local computer](logic-apps-gateway-install.md).</span></span>

* <span data-ttu-id="aa019-133">Amikor bejelentkezik az Azure portálra, akkor alkalmazza, ugyanazzal a munkahelyi vagy iskolai fiókkal, de a használt [telepítse a helyszíni adatátjáró](logic-apps-gateway-install.md#requirements).</span><span class="sxs-lookup"><span data-stu-id="aa019-133">When you sign in to the Azure portal, you have to use the same work or school account that was used to [install the on-premises data gateway](logic-apps-gateway-install.md#requirements).</span></span> <span data-ttu-id="aa019-134">A bejelentkezési fiók is rendelkeznie kell Azure-előfizetés létrehozásakor egy átjáró-erőforráshoz az Azure-portálon az átjáró telepítéséhez használandó.</span><span class="sxs-lookup"><span data-stu-id="aa019-134">Your sign-in account must also have an Azure subscription to use when you create a gateway resource in the Azure portal for your gateway installation.</span></span>

* <span data-ttu-id="aa019-135">Az átjáró telepítése már nem engedte, hogy az Azure átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-135">Your gateway installation can't already be claimed by an Azure gateway resource.</span></span> <span data-ttu-id="aa019-136">Az átjáró telepítésének csak egy Azure átjáró-erőforráshoz lehet társítani.</span><span class="sxs-lookup"><span data-stu-id="aa019-136">You can associate your gateway installation to only one Azure gateway resource.</span></span> <span data-ttu-id="aa019-137">Jogcím történik, ha az átjáró erőforrás létrehozása, hogy a telepítés nem érhető el az egyéb erőforrások.</span><span class="sxs-lookup"><span data-stu-id="aa019-137">Claim happens when you create the gateway resource so that the installation is unavailable for other resources.</span></span>

## <a name="set-up-the-data-gateway-connection"></a><span data-ttu-id="aa019-138">Az átjáró-kapcsolat beállítása</span><span class="sxs-lookup"><span data-stu-id="aa019-138">Set up the data gateway connection</span></span>

### <a name="1-install-the-on-premises-data-gateway"></a><span data-ttu-id="aa019-139">1. A helyszíni adatátjáró telepítése</span><span class="sxs-lookup"><span data-stu-id="aa019-139">1. Install the on-premises data gateway</span></span>

<span data-ttu-id="aa019-140">Ha még nem tette meg, kövesse a [az helyszíni átjáró telepítésének lépéseit](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="aa019-140">If you haven't already, follow the [steps to install the on-premises data gateway](logic-apps-gateway-install.md).</span></span> <span data-ttu-id="aa019-141">A többi lépés a folytatás előtt győződjön meg arról, hogy az átjáró a helyi számítógépen telepítve.</span><span class="sxs-lookup"><span data-stu-id="aa019-141">Before you continue with the other steps, make sure that you installed the data gateway on a local computer.</span></span>

<a name="create-gateway-resource"></a>
### <a name="2-create-an-azure-resource-for-the-on-premises-data-gateway"></a><span data-ttu-id="aa019-142">2. Hozzon létre egy Azure-erőforrás a helyszíni adatok átjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-142">2. Create an Azure resource for the on-premises data gateway</span></span>

<span data-ttu-id="aa019-143">Miután telepítette az átjárót a helyi számítógépen, létre kell hoznia az Ön data gateway elemmel olyan erőforrásként az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="aa019-143">After you install the gateway on a local computer, you must create your data gateway as a resource in Azure.</span></span> <span data-ttu-id="aa019-144">Ebben a lépésben az Azure-előfizetéshez is társítja az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-144">This step also associates your gateway resource with your Azure subscription.</span></span>

1. <span data-ttu-id="aa019-145">Jelentkezzen be az [Azure Portalra](https://portal.azure.com "Azure Portal")</span><span class="sxs-lookup"><span data-stu-id="aa019-145">Sign in to the [Azure portal](https://portal.azure.com "Azure portal").</span></span> <span data-ttu-id="aa019-146">Győződjön meg arról, hogy az Azure ugyanazzal a munkahelyi vagy iskolai e-mail címet az átjáró telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="aa019-146">Make sure to use the same Azure work or school email address used to install the gateway.</span></span>

2. <span data-ttu-id="aa019-147">Az Azure-ban a bal oldali menüben válassza a **új** > **vállalati integrációs** > **helyszíni adatátjáró** itt látható módon:</span><span class="sxs-lookup"><span data-stu-id="aa019-147">On the left menu in Azure, choose **New** > **Enterprise Integration** > **On-premises data gateway** as shown here:</span></span>

   !["A helyszíni adatok gateway" található](./media/logic-apps-gateway-connection/find-on-premises-data-gateway.png)

3. <span data-ttu-id="aa019-149">Az a **kapcsolat átjáró létrehozása** panelen meg az alábbi adatokat a data gateway erőforrás létrehozása:</span><span class="sxs-lookup"><span data-stu-id="aa019-149">On the **Create connection gateway** blade, provide these details to create your data gateway resource:</span></span>

    * <span data-ttu-id="aa019-150">**Név**: Adjon meg egy nevet az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-150">**Name**: Enter a name for your gateway resource.</span></span> 

    * <span data-ttu-id="aa019-151">**Előfizetés**: válassza ki az Azure-előfizetés társítása az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-151">**Subscription**: Select the Azure subscription to associate with your gateway resource.</span></span> 
    <span data-ttu-id="aa019-152">Ehhez az előfizetéshez kell lennie a Logic Apps alkalmazást tárolóként ugyanazt az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="aa019-152">This subscription should be the same subscription as your logic app.</span></span>
   
      <span data-ttu-id="aa019-153">Az alapértelmezett előfizetés bejelentkezéshez használt Azure-fiók alapul.</span><span class="sxs-lookup"><span data-stu-id="aa019-153">The default subscription is based on the Azure account that you used to sign in.</span></span>

    * <span data-ttu-id="aa019-154">**Erőforráscsoport**: hozzon létre egy erőforráscsoportot, vagy válasszon ki egy meglévő erőforráscsoportot üzembe helyezéséhez az átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-154">**Resource group**: Create a resource group or select an existing resource group for deploying your gateway resource.</span></span> 
    <span data-ttu-id="aa019-155">Erőforráscsoportok segítségével gyűjteményként kapcsolódó Azure eszközöket kezelheti.</span><span class="sxs-lookup"><span data-stu-id="aa019-155">Resource groups help you manage related Azure assets as a collection.</span></span>

    * <span data-ttu-id="aa019-156">**Hely**: Azure korlátozza a hely esetében az átjáró felhőszolgáltatáshoz során kiválasztott ugyanabban a régióban [átjáró telepítésének](logic-apps-gateway-install.md).</span><span class="sxs-lookup"><span data-stu-id="aa019-156">**Location**: Azure restricts this location to the same region that was selected for the gateway cloud service during [gateway installation](logic-apps-gateway-install.md).</span></span> 

      > [!NOTE]
      > <span data-ttu-id="aa019-157">Győződjön meg arról, hogy az átjáró erőforrás helye megegyezik-e az átjáró felhőalapú szolgáltatás helyét.</span><span class="sxs-lookup"><span data-stu-id="aa019-157">Make sure that the gateway resource location matches the gateway cloud service location.</span></span> <span data-ttu-id="aa019-158">Ellenkező esetben az átjáró telepítése nem jelenik meg, hogy a következő lépésben válassza ki a telepített átjárót listájában.</span><span class="sxs-lookup"><span data-stu-id="aa019-158">Otherwise, your gateway installation might not appear in the installed gateways list for you to select in the next step.</span></span>
      > 
      > <span data-ttu-id="aa019-159">Különböző régiókban is használhatja, az átjáró erőforrás és a logikai alkalmazásnak.</span><span class="sxs-lookup"><span data-stu-id="aa019-159">You can use different regions for your gateway resource and for your logic app.</span></span>

    * <span data-ttu-id="aa019-160">**Telepítési neve**: Ha nincs kiválasztva, akkor az átjáró telepítésének, válassza ki a korábban telepített átjárót.</span><span class="sxs-lookup"><span data-stu-id="aa019-160">**Installation Name**: If your gateway installation isn't already selected, select the gateway that you previously installed.</span></span> 

    <span data-ttu-id="aa019-161">Az átjáró erőforrás hozzáadása az Azure irányítópultra, válassza a **rögzítés az irányítópulton**.</span><span class="sxs-lookup"><span data-stu-id="aa019-161">To add the gateway resource to your Azure dashboard, choose **Pin to dashboard**.</span></span> 
    <span data-ttu-id="aa019-162">Amikor elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="aa019-162">When you're done, choose **Create**.</span></span>

    <span data-ttu-id="aa019-163">Példa:</span><span class="sxs-lookup"><span data-stu-id="aa019-163">For example:</span></span>

    ![Adja meg a helyszíni data gateway létrehozásához szükséges adatok](./media/logic-apps-gateway-connection/createblade.png)

    <span data-ttu-id="aa019-165">Található, vagy tekintse meg az Ön data gateway fő Azure bal oldali menüjében, bármikor Ugrás **több szolgáltatások** > **vállalati integrációs** > **a helyszíni adatokhoz Átjárók**.</span><span class="sxs-lookup"><span data-stu-id="aa019-165">To find or view your data gateway at any time,  from the main Azure left menu, go to  **More Services** > **Enterprise Integration** > **On-premises Data Gateways**.</span></span>

    ![Ugrás a "Szolgáltatás", "Vállalati integrációs", "a helyszíni Data Gateways"](./media/logic-apps-gateway-connection/find-on-premises-data-gateway-enterprise-integration.png)

<a name="connect-logic-app-gateway"></a>
### <a name="3-connect-your-logic-app-to-the-on-premises-data-gateway"></a><span data-ttu-id="aa019-167">3. A Logic Apps alkalmazást csatlakozni a helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="aa019-167">3. Connect your logic app to the on-premises data gateway</span></span>

<span data-ttu-id="aa019-168">Most, hogy a data gateway erőforrás elkészítette és az Azure-előfizetéshez társított erőforrás, a Logic Apps alkalmazást és az átjáró közötti kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aa019-168">Now that you've created your data gateway resource and associated your Azure subscription with that resource, create a connection between your logic app and the data gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="aa019-169">Az átjáró kapcsolódási helyet léteznie kell a Logic Apps alkalmazást ugyanabban a régióban, de használhatja a data gateway már egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="aa019-169">Your gateway connection location must exist in the same region as your logic app, but you can use a data gateway that exists in a different region.</span></span>

1. <span data-ttu-id="aa019-170">Az Azure-portálon hozzon létre, vagy nyissa meg a Logic Apps alkalmazást a Logic App tervezőben.</span><span class="sxs-lookup"><span data-stu-id="aa019-170">In the Azure portal, create or open your logic app in Logic App Designer.</span></span>

2. <span data-ttu-id="aa019-171">Adjon hozzá egy összekötőt, amely támogatja a helyi kapcsolatokat, például az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="aa019-171">Add a connector that supports on-premises connections, like SQL Server.</span></span>

3. <span data-ttu-id="aa019-172">Válassza ki a következő sorrendben történik, **keresztül, a helyszíni adatátjáró**, adjon meg egy egyedi kapcsolatnevet és a szükséges adatokat, és válassza ki a használni kívánt adatok átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-172">Following the order shown, select **Connect via on-premises data gateway**, provide a unique connection name and the required information, and select the data gateway resource that you want to use.</span></span> <span data-ttu-id="aa019-173">Amikor elkészült, válassza ki a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="aa019-173">When you're done, choose **Create**.</span></span>

   > [!TIP]
   > <span data-ttu-id="aa019-174">Egy egyedi kapcsolatnevet segítségével könnyen később, hogy a kapcsolat azonosítására, különösen akkor, ha több kapcsolatot is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="aa019-174">A unique connection name helps you easily identify that connection later, especially when you create multiple connections.</span></span> <span data-ttu-id="aa019-175">Ha alkalmazható, tartalmazzák a tartománynevet a felhasználónévhez.</span><span class="sxs-lookup"><span data-stu-id="aa019-175">If applicable, also include the qualified domain for your username.</span></span> 

   ![Logic app és az adatok átjáró közötti kapcsolat létrehozása](./media/logic-apps-gateway-connection/blankconnection.png)

<span data-ttu-id="aa019-177">Gratulálunk, az átjáró kapcsolat készen áll a használatára a Logic Apps alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="aa019-177">Congratulations, your gateway connection is now ready for your logic app to use.</span></span>

## <a name="edit-your-gateway-connection-settings"></a><span data-ttu-id="aa019-178">Az átjáró kapcsolat beállításainak szerkesztése</span><span class="sxs-lookup"><span data-stu-id="aa019-178">Edit your gateway connection settings</span></span>

<span data-ttu-id="aa019-179">Miután egy gateway-kapcsolatot hoz létre a Logic Apps alkalmazást, érdemes később frissíteni a beállításait, hogy adott kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="aa019-179">After you create a gateway connection for your logic app, you might want to later update the settings for that specific connection.</span></span>

1. <span data-ttu-id="aa019-180">Az átjárókapcsolathoz megkeresése:</span><span class="sxs-lookup"><span data-stu-id="aa019-180">To find the gateway connection:</span></span>

   * <span data-ttu-id="aa019-181">A logic app panelen a **Fejlesztőeszközök**, jelölje be **API kapcsolatok**.</span><span class="sxs-lookup"><span data-stu-id="aa019-181">On the logic app blade, under **Development Tools**, select **API Connections**.</span></span> 
   
     <span data-ttu-id="aa019-182">A **API kapcsolatok** ablaktábla megjeleníti azokat a Logic Apps alkalmazást, beleértve az átjáró kapcsolatokat társított összes API kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="aa019-182">The **API Connections** pane shows all API connections associated with your logic app, including gateway connections.</span></span>

     ![Nyissa meg a logikai alkalmazáshoz, jelölje be a "API-kapcsolatok"](./media/logic-apps-gateway-connection/logic-app-find-api-connections.png)

   * <span data-ttu-id="aa019-184">Vagy a fő Azure bal oldali menüben Ugrás **több szolgáltatások** > **webes & Mobile Services** > **API kapcsolatok** minden API-kapcsolatok esetén többek között a következőket átjárókapcsolatokhoz, az Azure-előfizetéshez társított.</span><span class="sxs-lookup"><span data-stu-id="aa019-184">Or, from the main Azure left menu, go to **More Services** > **Web & Mobile Services** > **API Connections** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span> 

   * <span data-ttu-id="aa019-185">Vagy a fő Azure bal oldali menüben Ugrás **összes erőforrás** minden API-kommunikációhoz, beleértve az átjáró kapcsolatokat, az Azure-előfizetéshez társított.</span><span class="sxs-lookup"><span data-stu-id="aa019-185">Or, on the main Azure left menu, go to **All resources** for all API connections, including gateway connections, that are associated with your Azure subscription.</span></span>

2. <span data-ttu-id="aa019-186">Válassza ki a megtekintése, szerkesztése és válassza a kívánt átjáró kapcsolatot **szerkesztése API-kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="aa019-186">Select the gateway connection that you want to view or edit, and choose **Edit API connection**.</span></span>

   > [!TIP]
   > <span data-ttu-id="aa019-187">Ha a frissítések nem lép érvénybe, próbálja meg [leállításával és újraindításával az átjáró Windows-szolgáltatás](./logic-apps-gateway-install.md#restart-gateway).</span><span class="sxs-lookup"><span data-stu-id="aa019-187">If your updates don't take effect, try [stopping and restarting the gateway Windows service](./logic-apps-gateway-install.md#restart-gateway).</span></span>

<a name="change-delete-gateway-resource"></a>
## <a name="switch-or-delete-your-on-premises-data-gateway-resource"></a><span data-ttu-id="aa019-188">Váltás, vagy a helyszíni adatok átjáró erőforrás törlése</span><span class="sxs-lookup"><span data-stu-id="aa019-188">Switch or delete your on-premises data gateway resource</span></span>

<span data-ttu-id="aa019-189">Hozzon létre egy másik átjáró erőforrást, rendelje hozzá az átjárót egy másik erőforráscsoportban, vagy távolítsa el az átjáró-erőforráshoz, törölheti az átjáró-erőforráshoz az átjáró telepítésének befolyásolása nélkül.</span><span class="sxs-lookup"><span data-stu-id="aa019-189">To create a different gateway resource, associate your gateway with a different resource, or remove the gateway resource, you can delete the gateway resource without affecting the gateway installation.</span></span> 

1. <span data-ttu-id="aa019-190">A fő Azure bal oldali menüben Ugrás **összes erőforrás**.</span><span class="sxs-lookup"><span data-stu-id="aa019-190">From the main Azure left menu, go to **All resources**.</span></span> 
2. <span data-ttu-id="aa019-191">Keresse meg és jelölje ki az adatok átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="aa019-191">Find and select your data gateway resource.</span></span>
3. <span data-ttu-id="aa019-192">Válasszon **helyszíni Data Gateway**, és az erőforrás eszköztáron válassza **törlése**.</span><span class="sxs-lookup"><span data-stu-id="aa019-192">Choose **On-premises Data Gateway**, and on the resource toolbar, choose **Delete**.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="aa019-193">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="aa019-193">Frequently asked questions</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

## <a name="next-steps"></a><span data-ttu-id="aa019-194">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa019-194">Next steps</span></span>

* [<span data-ttu-id="aa019-195">A logikai alkalmazások védelme</span><span class="sxs-lookup"><span data-stu-id="aa019-195">Secure your logic apps</span></span>](./logic-apps-securing-a-logic-app.md)
* [<span data-ttu-id="aa019-196">Gyakori példák és forgatókönyvek a logic Apps alkalmazások</span><span class="sxs-lookup"><span data-stu-id="aa019-196">Common examples and scenarios for logic apps</span></span>](./logic-apps-examples-and-scenarios.md)
