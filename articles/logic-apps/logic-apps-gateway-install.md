---
title: "aaaInstall helyszíni adatátjáró - Azure Logic Apps |} Microsoft Docs"
description: "A helyszíni adatforrások eléréséhez előtt telepítse a hello helyszíni adatátjáró gyors adatátvitel és a titkosításhoz a helyszíni adatforrások és a logic Apps alkalmazások között"
keywords: "adatok, a helyszíni, az adatok átvitele, a titkosítás és a adatforrások eléréséhez"
services: logic-apps
documentationcenter: 
author: jeffhollan
manager: anneta
editor: 
ms.assetid: 47e3024e-88a0-4017-8484-8f392faec89d
ms.service: logic-apps
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 07/13/2017
ms.author: LADocs; dimazaid; estfan
ms.openlocfilehash: 01386a904d856ff545f2eca8eb1b008dcdc08574
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-hello-on-premises-data-gateway-for-azure-logic-apps"></a><span data-ttu-id="cda83-104">Az Azure Logic Apps hello helyszíni adatátjáró telepítése</span><span class="sxs-lookup"><span data-stu-id="cda83-104">Install hello on-premises data gateway for Azure Logic Apps</span></span>

<span data-ttu-id="cda83-105">A logic apps a helyszíni adatforrások eléréséhez, telepítésekor, és a helyszíni adatátjáró hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="cda83-105">Before your logic apps can access data sources on premises, you must install and set up hello on-premises data gateway.</span></span> <span data-ttu-id="cda83-106">hello átjáró működik, amely gyors adatátvitel és a titkosítás a helyszínen és a logic Apps alkalmazások közötti hídként.</span><span class="sxs-lookup"><span data-stu-id="cda83-106">hello gateway acts as a bridge that provides quick data transfer and encryption between on-premises systems and your logic apps.</span></span> <span data-ttu-id="cda83-107">hello átjáró továbbítja a titkosított csatornákon keresztül hello Azure Service Bus helyszíni forrásból származó adatokat.</span><span class="sxs-lookup"><span data-stu-id="cda83-107">hello gateway relays data from on-premises sources on encrypted channels through hello Azure Service Bus.</span></span> <span data-ttu-id="cda83-108">Az összes forgalom származik, mint hello átjáró ügynök biztonságos kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="cda83-108">All traffic originates as secure outbound traffic from hello gateway agent.</span></span> <span data-ttu-id="cda83-109">További információ [hello adatátjáró működése](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="cda83-109">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

<span data-ttu-id="cda83-110">hello átjárót a helyszíni kapcsolatok toothese adatforrások támogatja:</span><span class="sxs-lookup"><span data-stu-id="cda83-110">hello gateway supports connections toothese data sources on premises:</span></span>

*   <span data-ttu-id="cda83-111">BizTalk Server 2016</span><span class="sxs-lookup"><span data-stu-id="cda83-111">BizTalk Server 2016</span></span>
*   <span data-ttu-id="cda83-112">DB2</span><span class="sxs-lookup"><span data-stu-id="cda83-112">DB2</span></span>  
*   <span data-ttu-id="cda83-113">Fájlrendszer</span><span class="sxs-lookup"><span data-stu-id="cda83-113">File System</span></span>
*   <span data-ttu-id="cda83-114">Informix</span><span class="sxs-lookup"><span data-stu-id="cda83-114">Informix</span></span>
*   <span data-ttu-id="cda83-115">MQ</span><span class="sxs-lookup"><span data-stu-id="cda83-115">MQ</span></span>
*   <span data-ttu-id="cda83-116">MySQL</span><span class="sxs-lookup"><span data-stu-id="cda83-116">MySQL</span></span>
*   <span data-ttu-id="cda83-117">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="cda83-117">Oracle Database</span></span>
*   <span data-ttu-id="cda83-118">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="cda83-118">PostgreSQL</span></span>
*   <span data-ttu-id="cda83-119">SAP-alkalmazáskiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cda83-119">SAP Application Server</span></span> 
*   <span data-ttu-id="cda83-120">SAP üzenet kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="cda83-120">SAP Message Server</span></span>
*   <span data-ttu-id="cda83-121">SharePoint</span><span class="sxs-lookup"><span data-stu-id="cda83-121">SharePoint</span></span>
*   <span data-ttu-id="cda83-122">SQL Server</span><span class="sxs-lookup"><span data-stu-id="cda83-122">SQL Server</span></span>
*   <span data-ttu-id="cda83-123">Teradata</span><span class="sxs-lookup"><span data-stu-id="cda83-123">Teradata</span></span>

<span data-ttu-id="cda83-124">Ezeket a lépéseket megjelenítése hogyan toofirst telepítés hello helyszíni adatátjáró előtt [hello átjáró és a logic Apps alkalmazások közötti kapcsolat beállítása](./logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="cda83-124">These steps show how toofirst install hello on-premises data gateway before you [set up a connection between hello gateway and your logic apps](./logic-apps-gateway-connection.md).</span></span> <span data-ttu-id="cda83-125">További információ a támogatott összekötők: [az Azure Logic Apps összekötők](https://docs.microsoft.com/azure/connectors/apis-list).</span><span class="sxs-lookup"><span data-stu-id="cda83-125">For more information about supported connectors, see [Connectors for Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list).</span></span> 

<span data-ttu-id="cda83-126">Hogyan toouse hello átjáró más szolgáltatásokkal kapcsolatos információkért lásd: ezek a cikkek:</span><span class="sxs-lookup"><span data-stu-id="cda83-126">For information about how toouse hello gateway with other services, see these articles:</span></span>

*   [<span data-ttu-id="cda83-127">Microsoft Power BI helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="cda83-127">Microsoft Power BI on-premises data gateway</span></span>](https://powerbi.microsoft.com/documentation/powerbi-gateway-onprem/)
*   [<span data-ttu-id="cda83-128">Az Azure Analysis Services helyi adatátjáró</span><span class="sxs-lookup"><span data-stu-id="cda83-128">Azure Analysis Services on-premises data gateway</span></span>](../analysis-services/analysis-services-gateway.md)
*   [<span data-ttu-id="cda83-129">Microsoft Flow helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="cda83-129">Microsoft Flow on-premises data gateway</span></span>](https://flow.microsoft.com/documentation/gateway-manage/)
*   [<span data-ttu-id="cda83-130">Microsoft PowerApps helyszíni adatátjáró</span><span class="sxs-lookup"><span data-stu-id="cda83-130">Microsoft PowerApps on-premises data gateway</span></span>](https://powerapps.microsoft.com/tutorials/gateway-management/)

<a name="requirements"></a>
## <a name="requirements"></a><span data-ttu-id="cda83-131">Követelmények</span><span class="sxs-lookup"><span data-stu-id="cda83-131">Requirements</span></span>

<span data-ttu-id="cda83-132">**Minimális**:</span><span class="sxs-lookup"><span data-stu-id="cda83-132">**Minimum**:</span></span>

* <span data-ttu-id="cda83-133">.NET 4.5 keretrendszer</span><span class="sxs-lookup"><span data-stu-id="cda83-133">.NET 4.5 Framework</span></span>
* <span data-ttu-id="cda83-134">64 bites Windows 7 vagy Windows Server 2008 R2 (vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="cda83-134">64-bit version of Windows 7 or Windows Server 2008 R2 (or later)</span></span>

<span data-ttu-id="cda83-135">**Ajánlott**:</span><span class="sxs-lookup"><span data-stu-id="cda83-135">**Recommended**:</span></span>

* <span data-ttu-id="cda83-136">8 mag Processzor</span><span class="sxs-lookup"><span data-stu-id="cda83-136">8 Core CPU</span></span>
* <span data-ttu-id="cda83-137">8 GB memória</span><span class="sxs-lookup"><span data-stu-id="cda83-137">8 GB Memory</span></span>
* <span data-ttu-id="cda83-138">64 bites Windows 2012 R2 (vagy újabb)</span><span class="sxs-lookup"><span data-stu-id="cda83-138">64-bit version of Windows 2012 R2 (or later)</span></span>

<span data-ttu-id="cda83-139">**Fontos tudnivalók találhatók**:</span><span class="sxs-lookup"><span data-stu-id="cda83-139">**Important considerations**:</span></span>

* <span data-ttu-id="cda83-140">Hello helyszíni adatátjáró telepítése csak a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="cda83-140">Install hello on-premises data gateway only on a local computer.</span></span>
<span data-ttu-id="cda83-141">Hello átjáró nem telepíthet tartományvezérlőre.</span><span class="sxs-lookup"><span data-stu-id="cda83-141">You can't install hello gateway on a domain controller.</span></span>

   > [!TIP]
   > <span data-ttu-id="cda83-142">Nincs a hello tooinstall hello átjáró ugyanazon a számítógépen a adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="cda83-142">You don't have tooinstall hello gateway on hello same computer as your data source.</span></span> <span data-ttu-id="cda83-143">toominimize késés, a legközelebb lehetséges tooyour adatforrásként, vagy a hello azonos hello átjárót telepítheti számítógép, feltéve, hogy Ön jogosult.</span><span class="sxs-lookup"><span data-stu-id="cda83-143">toominimize latency, you can install hello gateway as close as possible tooyour data source, or on hello same computer, assuming that you have permissions.</span></span>

* <span data-ttu-id="cda83-144">Hello átjáró nem telepíthető olyan számítógépre, kikapcsolja, toosleep kerül vagy toohello Internet nem csatlakoztatható, mert a hello átjáró ilyen körülmények nem futtatható.</span><span class="sxs-lookup"><span data-stu-id="cda83-144">Don't install hello gateway on a computer that turns off, goes toosleep, or doesn't connect toohello Internet because hello gateway can't run under those circumstances.</span></span> <span data-ttu-id="cda83-145">Emellett átjáró teljesítménye csökkenhet, vezeték nélküli hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="cda83-145">Also, gateway performance might suffer over a wireless network.</span></span>

* <span data-ttu-id="cda83-146">A telepítés során be kell jelentkeznie a egy [munkahelyi vagy iskolai fiók](https://docs.microsoft.com/azure/active-directory/sign-up-organization) , amely az Azure Active Directory (Azure AD), nem Microsoft-fiók felügyeli.</span><span class="sxs-lookup"><span data-stu-id="cda83-146">During installation, you must sign in with a [work or school account](https://docs.microsoft.com/azure/active-directory/sign-up-organization) that's managed by Azure Active Directory (Azure AD), not a Microsoft account.</span></span> 

  <span data-ttu-id="cda83-147">Rendelkezik toouse hello ugyanazzal a munkahelyi vagy iskolai fiók később a hello Azure portal létrehozásakor, és rendelje hozzá az átjáró telepítése egy átjáró-erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="cda83-147">You have toouse hello same work or school account later in hello Azure portal when you create and associate a gateway resource with your gateway installation.</span></span> <span data-ttu-id="cda83-148">Az átjáró-erőforráshoz, majd válassza ki, a logic app és hello helyszíni adatforrás között hello kapcsolat létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="cda83-148">You then select this gateway resource when you create hello connection between your logic app and hello on-premises data source.</span></span> [<span data-ttu-id="cda83-149">Miért kell használni az Azure AD munkahelyi vagy iskolai fiókkal?</span><span class="sxs-lookup"><span data-stu-id="cda83-149">Why must I use an Azure AD work or school account?</span></span>](#why-azure-work-school-account)

  > [!TIP]
  > <span data-ttu-id="cda83-150">Ha regisztrált az Office 365 ajánlat, és nem adja meg a tényleges munkahelyi e-mail címét, a bejelentkezési címe nézhet jeff@contoso.onmicrosoft.com.</span><span class="sxs-lookup"><span data-stu-id="cda83-150">If you signed up for an Office 365 offering and didn't supply your actual work email, your sign-in address might look like jeff@contoso.onmicrosoft.com.</span></span> 

* <span data-ttu-id="cda83-151">Ha egy meglévő átjárót, a korábbi verziónál 14.16.6317.4 telepítő beállított, az átjáró helyen futó hello legújabb telepítő által nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="cda83-151">If you have an existing gateway that you set up with an installer that's earlier than version 14.16.6317.4, you can't change your gateway's location by running hello latest installer.</span></span> <span data-ttu-id="cda83-152">Azonban ehelyett kívánt hello hellyel rendelkező hello legújabb telepítő tooset be egy új átjárót is használhatja.</span><span class="sxs-lookup"><span data-stu-id="cda83-152">However, you can use hello latest installer tooset up a new gateway with hello location that you want instead.</span></span>
  
  <span data-ttu-id="cda83-153">Ha egy átjáró installer korábbi verziója 14.16.6317.4, de még nem telepítette az átjárót, letöltheti és hello legújabb telepítővel.</span><span class="sxs-lookup"><span data-stu-id="cda83-153">If you have a gateway installer that's earlier than version 14.16.6317.4, but you haven't installed your gateway yet, you can download and use hello latest installer.</span></span>

<a name="install-gateway"></a>

## <a name="install-hello-data-gateway"></a><span data-ttu-id="cda83-154">Hello adatátjáró telepítése</span><span class="sxs-lookup"><span data-stu-id="cda83-154">Install hello data gateway</span></span>

1.  <span data-ttu-id="cda83-155">[Töltse le és futtassa a hello gateway installer a helyi számítógép](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="cda83-155">[Download and run hello gateway installer on a local computer](http://go.microsoft.com/fwlink/?LinkID=820931&clcid=0x409).</span></span>

2. <span data-ttu-id="cda83-156">Tekintse át és fogadja el hello használati feltételei és adatvédelmi nyilatkozata.</span><span class="sxs-lookup"><span data-stu-id="cda83-156">Review and accept hello terms of use and privacy statement.</span></span>

3. <span data-ttu-id="cda83-157">Hello utat adjon meg a helyi számítógépre, amelyre tooinstall hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="cda83-157">Specify hello path on your local computer where you want tooinstall hello gateway.</span></span>

4. <span data-ttu-id="cda83-158">Amikor a rendszer kéri, jelentkezzen be a a Azure munkahelyi vagy iskolai fiókkal, nem Microsoft-fiókkal.</span><span class="sxs-lookup"><span data-stu-id="cda83-158">When prompted, sign in with your Azure work or school account, not a Microsoft account.</span></span>

   ![Jelentkezzen be Azure munkahelyi vagy iskolai fiók](./media/logic-apps-gateway-install/sign-in-gateway-install.png)

5. <span data-ttu-id="cda83-160">Most regisztrálja a telepített átjárót hello [átjáró felhőszolgáltatáshoz](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="cda83-160">Now register your installed gateway with hello [gateway cloud service](#gateway-cloud-service).</span></span> <span data-ttu-id="cda83-161">Válasszon **ezen a számítógépen egy új átjáró regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="cda83-161">Choose **Register a new gateway on this computer**.</span></span>

   <span data-ttu-id="cda83-162">hello átjáró felhőszolgáltatáshoz titkosítja, és tárolja az adatforrás hitelesítő adatai és az átjáró beállításai között.</span><span class="sxs-lookup"><span data-stu-id="cda83-162">hello gateway cloud service encrypts and stores your data source credentials and gateway details.</span></span> 
   <span data-ttu-id="cda83-163">hello szolgáltatást is irányítja a lekérdezések és az eredmények között a Logic Apps alkalmazást, a helyszíni adatátjáró hello és az adatforrás a helyszínen.</span><span class="sxs-lookup"><span data-stu-id="cda83-163">hello service also routes queries and their results between your logic app, hello on-premises data gateway, and your data source on premises.</span></span>

6. <span data-ttu-id="cda83-164">Adjon meg egy nevet az átjáró telepítése.</span><span class="sxs-lookup"><span data-stu-id="cda83-164">Provide a name for your gateway installation.</span></span> <span data-ttu-id="cda83-165">Hozzon létre helyreállítási kulcsot, majd erősítse meg a helyreállítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cda83-165">Create a recovery key, then confirm your recovery key.</span></span> 

   > [!IMPORTANT] 
   > <span data-ttu-id="cda83-166">A helyreállítási kulcs legalább nyolc karakterből kell állnia.</span><span class="sxs-lookup"><span data-stu-id="cda83-166">Your recovery key must contain at least eight characters.</span></span> <span data-ttu-id="cda83-167">Győződjön meg arról, hogy mentse el, és hello kulcs tartsa biztonságos helyen.</span><span class="sxs-lookup"><span data-stu-id="cda83-167">Make sure that you save and keep hello key in a safe place.</span></span> <span data-ttu-id="cda83-168">Ha azt szeretné, hogy toomigrate, is kell a kulcs visszaállítása, vagy vegye át egy meglévő átjáróhoz.</span><span class="sxs-lookup"><span data-stu-id="cda83-168">You also need this key when you want toomigrate, restore, or take over an existing gateway.</span></span>

   1. <span data-ttu-id="cda83-169">Válassza ki a toochange hello alapértelmezett régió hello átjáró felhőalapú szolgáltatás és az átjáró telepítése által használt Azure Service Bus **régió módosítása**.</span><span class="sxs-lookup"><span data-stu-id="cda83-169">toochange hello default region for hello gateway cloud service and Azure Service Bus used by your gateway installation, choose **Change Region**.</span></span>

      ![Régió módosítása](./media/logic-apps-gateway-install/change-region-gateway-install.png)

      <span data-ttu-id="cda83-171">hello alapértelmezett terület az Azure AD bérlőhöz társított hello régióban.</span><span class="sxs-lookup"><span data-stu-id="cda83-171">hello default region is hello region associated with your Azure AD tenant.</span></span>

   2. <span data-ttu-id="cda83-172">A következő ablaktábla hello, nyissa meg a hello **válasszon régiót** túl válasszon egy másik régióban.</span><span class="sxs-lookup"><span data-stu-id="cda83-172">On hello next pane, open hello **Select Region** too choose a different region.</span></span>

      ![Válasszon ki egy másik régiót](./media/logic-apps-gateway-install/select-region-gateway-install.png)

      <span data-ttu-id="cda83-174">Például akkor lehet, hogy válassza ki a Logic Apps alkalmazást és ugyanabban a régióban hello, vagy válassza hello régió legközelebbi tooyour helyszíni adatforrás, a késés csökkentése érdekében.</span><span class="sxs-lookup"><span data-stu-id="cda83-174">For example, you might select hello same region as your logic app, or select hello region closest tooyour on-premises data source so you can reduce latency.</span></span> <span data-ttu-id="cda83-175">Az átjáró-erőforráshoz és logikai alkalmazás különböző helyen is rendelkezhetnek.</span><span class="sxs-lookup"><span data-stu-id="cda83-175">Your gateway resource and logic app can have different locations.</span></span>

      > [!IMPORTANT]
      > <span data-ttu-id="cda83-176">Ebben a régióban a telepítés után nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="cda83-176">You can't change this region after installation.</span></span> <span data-ttu-id="cda83-177">Ebben a régióban is határozza meg, és korlátozza a hello helyre, ahol az átjáró létrehozhat hello Azure-erőforrás.</span><span class="sxs-lookup"><span data-stu-id="cda83-177">This region also determines and restricts hello location where you can create hello Azure resource for your gateway.</span></span> <span data-ttu-id="cda83-178">Ezért az átjáró erőforrás létrehozásakor az Azure-ban győződjön meg arról, hogy hello erőforrás helye megegyezik-e az átjáró telepítésekor kiválasztott hello régióban.</span><span class="sxs-lookup"><span data-stu-id="cda83-178">So when you create your gateway resource in Azure, make sure that hello resource location matches hello region that you selected during gateway installation.</span></span>
      > 
      > <span data-ttu-id="cda83-179">Ha szeretne egy másik régióban toouse az átjáró később, be kell állítania egy új átjárót.</span><span class="sxs-lookup"><span data-stu-id="cda83-179">If you want toouse a different region for your gateway later, you must set up a new gateway.</span></span>

   3. <span data-ttu-id="cda83-180">Ha elkészült, válassza ki a **végzett**.</span><span class="sxs-lookup"><span data-stu-id="cda83-180">When you're ready, choose **Done**.</span></span>

7. <span data-ttu-id="cda83-181">Most tegye a következőket a hello Azure-portálon, így [hozzon létre egy Azure-erőforrás az átjáró](../logic-apps/logic-apps-gateway-connection.md).</span><span class="sxs-lookup"><span data-stu-id="cda83-181">Now follow these steps in hello Azure portal so you can [create an Azure resource for your gateway](../logic-apps/logic-apps-gateway-connection.md).</span></span> 

<span data-ttu-id="cda83-182">További információ [hello adatátjáró működése](#gateway-cloud-service).</span><span class="sxs-lookup"><span data-stu-id="cda83-182">Learn more about [how hello data gateway works](#gateway-cloud-service).</span></span>

## <a name="migrate-restore-or-take-over-an-existing-gateway"></a><span data-ttu-id="cda83-183">Telepítse át, állítsa vissza, vagy vegye át egy meglévő átjáróhoz</span><span class="sxs-lookup"><span data-stu-id="cda83-183">Migrate, restore, or take over an existing gateway</span></span>

<span data-ttu-id="cda83-184">Ezek a feladatok tooperform, hello helyreállítási kulcs hello átjáró telepítésekor megadott rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="cda83-184">tooperform these tasks, you must have hello recovery key that was specified when hello gateway was installed.</span></span>

1. <span data-ttu-id="cda83-185">A számítógép Start menüből **helyszíni adatátjáró**.</span><span class="sxs-lookup"><span data-stu-id="cda83-185">From your computer's Start menu, choose **On-premises data gateway**.</span></span>

2. <span data-ttu-id="cda83-186">Miután hello telepítő megnyílik, jelentkezzen be az hello azonos Azure munkahelyi vagy iskolai fiókkal, amely korábban használt tooinstall hello átjáró.</span><span class="sxs-lookup"><span data-stu-id="cda83-186">After hello installer opens, sign in with hello same Azure work or school account that was previously used tooinstall hello gateway.</span></span>

3. <span data-ttu-id="cda83-187">Válasszon **áttelepítése, visszaállítása vagy átvétele egy meglévő átjáróhoz**.</span><span class="sxs-lookup"><span data-stu-id="cda83-187">Choose **Migrate, restore, or take over an existing gateway**.</span></span>

4. <span data-ttu-id="cda83-188">Adja meg a kívánt toomigrate, visszaállítása vagy hajtsa végre a megfelelő hello átjáró nevét és a helyreállítási kulcs hello.</span><span class="sxs-lookup"><span data-stu-id="cda83-188">Provide hello name and recovery key for hello gateway that you want toomigrate, restore, or take over.</span></span>

<a name="restart-gateway"></a>
## <a name="restart-hello-gateway"></a><span data-ttu-id="cda83-189">Indítsa újra a hello átjáró</span><span class="sxs-lookup"><span data-stu-id="cda83-189">Restart hello gateway</span></span>

<span data-ttu-id="cda83-190">hello átjáró fut a Windows-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="cda83-190">hello gateway runs as a Windows service.</span></span> <span data-ttu-id="cda83-191">Mint bármely más Windows-szolgáltatás indítsa el, és többféle módon hello szolgáltatás leállítása.</span><span class="sxs-lookup"><span data-stu-id="cda83-191">Like any other Windows service, you can start and stop hello service in multiple ways.</span></span> <span data-ttu-id="cda83-192">Például nyisson meg egy parancssort emelt jogosultsági szintű hello hello átjárót futtató számítógépen, és futtassa az alábbi parancsokat vagy:</span><span class="sxs-lookup"><span data-stu-id="cda83-192">For example, you can open a command prompt with elevated permissions on hello computer where hello gateway is running, and run either these commands:</span></span>

* <span data-ttu-id="cda83-193">toostop hello szolgáltatást, futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="cda83-193">toostop hello service, run this command:</span></span>
  
    `net stop PBIEgwService`

* <span data-ttu-id="cda83-194">toostart hello szolgáltatást, futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="cda83-194">toostart hello service, run this command:</span></span>
  
    `net start PBIEgwService`

### <a name="windows-service-account"></a><span data-ttu-id="cda83-195">Windows-szolgáltatásfiók</span><span class="sxs-lookup"><span data-stu-id="cda83-195">Windows service account</span></span>

<span data-ttu-id="cda83-196">hello helyszíni adatátjáró toouse be van állítva `NT SERVICE\PBIEgwService` Windows hello szolgáltatás bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="cda83-196">hello on-premises data gateway is set up toouse `NT SERVICE\PBIEgwService` for hello Windows service logon credentials.</span></span> <span data-ttu-id="cda83-197">Alapértelmezés szerint hello átjáró hello "Bejelentkezés szolgáltatásként" jogosultsággal rendelkezik hello gép hello átjáró telepítéséhez használt.</span><span class="sxs-lookup"><span data-stu-id="cda83-197">By default, hello gateway has hello "Log on as a service" right for hello machine where you install hello gateway.</span></span>

> [!NOTE]
> <span data-ttu-id="cda83-198">szolgáltatásfiók Windows hello tooon helyi adatforrások csatlakozáshoz használt hello fiókból, illetve a hello Azure munkahelyi értéke vagy iskolai fiókjával használt toosign toocloud szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="cda83-198">hello Windows service account differs from hello account used for connecting tooon-premises data sources, and from hello Azure work or school account used toosign in toocloud services.</span></span>

## <a name="configure-a-firewall-or-proxy"></a><span data-ttu-id="cda83-199">Egy tűzfal vagy a proxy konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cda83-199">Configure a firewall or proxy</span></span>

<span data-ttu-id="cda83-200">hello átjáró létrehoz egy kimenő kapcsolat túl [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="cda83-200">hello gateway creates an outbound connection too [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="cda83-201">Tekintse meg a proxyadatokat tooprovide az átjáró [konfigurálja a proxybeállításokat](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span><span class="sxs-lookup"><span data-stu-id="cda83-201">tooprovide proxy information for your gateway, see [Configure proxy settings](https://powerbi.microsoft.com/documentation/powerbi-gateway-proxy/).</span></span>

<span data-ttu-id="cda83-202">toocheck, hogy a tűzfal, vagy a proxy, előfordulhat, hogy kapcsolat tiltása, győződjön meg róla hogy a gép ténylegesen kapcsolódhat toohello internet és hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span><span class="sxs-lookup"><span data-stu-id="cda83-202">toocheck whether your firewall, or proxy, might block connections, confirm whether your machine can actually connect toohello internet and hello [Azure Service Bus](https://azure.microsoft.com/services/service-bus/).</span></span> <span data-ttu-id="cda83-203">Futtassa ezt a parancsot egy PowerShell-parancssorba:</span><span class="sxs-lookup"><span data-stu-id="cda83-203">From a PowerShell prompt, run this command:</span></span>

`Test-NetConnection -ComputerName watchdog.servicebus.windows.net -Port 9350`

> [!NOTE]
> <span data-ttu-id="cda83-204">A parancs csak a hálózati kapcsolatot, valamint kapcsolat toohello Azure Service Bus teszteli.</span><span class="sxs-lookup"><span data-stu-id="cda83-204">This command only tests network connectivity and connectivity toohello Azure Service Bus.</span></span> <span data-ttu-id="cda83-205">Így hello parancs nem kell semmi toodo hello átjáróval vagy hello átjáró felhőszolgáltatás, amely titkosítja, és tárolja a hitelesítő adatait, és az átjáró beállításai között.</span><span class="sxs-lookup"><span data-stu-id="cda83-205">So hello command doesn't have anything toodo with hello gateway or hello gateway cloud service that encrypts and stores your credentials and gateway details.</span></span> 
>
> <span data-ttu-id="cda83-206">Ez a parancs is csak a Windows Server 2012 R2 vagy későbbi, és a Windows 8.1 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="cda83-206">Also, this command is only available on Windows Server 2012 R2 or later, and Windows 8.1 or later.</span></span> <span data-ttu-id="cda83-207">Az operációs rendszer korábbi verzióiban, használhatja a Telnet túl a kapcsolat tesztelése.</span><span class="sxs-lookup"><span data-stu-id="cda83-207">On earlier OS versions, you can use Telnet too test connectivity.</span></span> <span data-ttu-id="cda83-208">További információ [Azure Service Bus vagy hibrid megoldások](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="cda83-208">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

<span data-ttu-id="cda83-209">Az eredmények alábbihoz hasonló toothis példa:</span><span class="sxs-lookup"><span data-stu-id="cda83-209">Your results should look similar toothis example:</span></span>

```text
ComputerName           : watchdog.servicebus.windows.net
RemoteAddress          : 70.37.104.240
RemotePort             : 5672
InterfaceAlias         : vEthernet (Broadcom NetXtreme Gigabit Ethernet - Virtual Switch)
SourceAddress          : 10.120.60.105
PingSucceeded          : False
PingReplyDetails (RTT) : 0 ms
TcpTestSucceeded       : True
```

<span data-ttu-id="cda83-210">Ha **TcpTestSucceeded** értéke túl**igaz**, előfordulhat, hogy blokkolja tűzfal.</span><span class="sxs-lookup"><span data-stu-id="cda83-210">If **TcpTestSucceeded** is not set too**True**, you might be blocked by a firewall.</span></span> <span data-ttu-id="cda83-211">Ha azt szeretné, toobe átfogó, helyettesítse hello **számítógépnév** és **Port** felsorolt értékek hello értékekkel [portok konfigurálása](#configure-ports) ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="cda83-211">If you want toobe comprehensive, substitute hello **ComputerName** and **Port** values with hello values listed under [Configure ports](#configure-ports) in this topic.</span></span>

<span data-ttu-id="cda83-212">hello tűzfal is megakadályozhatja, hogy kapcsolatok adott hello Azure Service Bus révén toohello Azure adatközpontjaiban.</span><span class="sxs-lookup"><span data-stu-id="cda83-212">hello firewall might also block connections that hello Azure Service Bus makes toohello Azure datacenters.</span></span> <span data-ttu-id="cda83-213">Ha ebben a forgatókönyvben, jóváhagyása (feloldása) összes hello ezeket a saját régiójában adatközpontok IP-címet.</span><span class="sxs-lookup"><span data-stu-id="cda83-213">If this scenario happens, approve (unblock) all hello IP addresses for those datacenters in your region.</span></span> <span data-ttu-id="cda83-214">Azon IP-címek [get hello Azure IP-címek listája itt](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="cda83-214">For those IP addresses, [get hello Azure IP addresses list here](https://www.microsoft.com/download/details.aspx?id=41653).</span></span>

## <a name="configure-ports"></a><span data-ttu-id="cda83-215">Portok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cda83-215">Configure ports</span></span>

<span data-ttu-id="cda83-216">hello átjáró létrehoz egy kimenő kapcsolat túl[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) és kimenő portok folytat: TCP 443-as (alapértelmezett), 5671, 5672, 9350 – 9354-es.</span><span class="sxs-lookup"><span data-stu-id="cda83-216">hello gateway creates an outbound connection too[Azure Service Bus](https://azure.microsoft.com/services/service-bus/) and communicates on outbound ports: TCP 443 (default), 5671, 5672, 9350 through 9354.</span></span> <span data-ttu-id="cda83-217">hello átjáró nincs szükség a bejövő portra.</span><span class="sxs-lookup"><span data-stu-id="cda83-217">hello gateway doesn't require inbound ports.</span></span> <span data-ttu-id="cda83-218">További információ [Azure Service Bus vagy hibrid megoldások](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="cda83-218">Learn more about [Azure Service Bus and hybrid solutions](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md).</span></span>

| <span data-ttu-id="cda83-219">TARTOMÁNYNEVEK</span><span class="sxs-lookup"><span data-stu-id="cda83-219">DOMAIN NAMES</span></span> | <span data-ttu-id="cda83-220">KIMENŐ PORTOK</span><span class="sxs-lookup"><span data-stu-id="cda83-220">OUTBOUND PORTS</span></span> | <span data-ttu-id="cda83-221">LEÍRÁS</span><span class="sxs-lookup"><span data-stu-id="cda83-221">DESCRIPTION</span></span> |
| --- | --- | --- |
| <span data-ttu-id="cda83-222">*. analysis.windows.net</span><span class="sxs-lookup"><span data-stu-id="cda83-222">*.analysis.windows.net</span></span> | <span data-ttu-id="cda83-223">443</span><span class="sxs-lookup"><span data-stu-id="cda83-223">443</span></span> | <span data-ttu-id="cda83-224">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cda83-224">HTTPS</span></span> | 
| <span data-ttu-id="cda83-225">*. login.windows.net</span><span class="sxs-lookup"><span data-stu-id="cda83-225">*.login.windows.net</span></span> | <span data-ttu-id="cda83-226">443</span><span class="sxs-lookup"><span data-stu-id="cda83-226">443</span></span> | <span data-ttu-id="cda83-227">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cda83-227">HTTPS</span></span> | 
| <span data-ttu-id="cda83-228">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="cda83-228">*.servicebus.windows.net</span></span> | <span data-ttu-id="cda83-229">5671-5672</span><span class="sxs-lookup"><span data-stu-id="cda83-229">5671-5672</span></span> | <span data-ttu-id="cda83-230">Speciális üzenetsor-kezelési protokoll (AMQP)</span><span class="sxs-lookup"><span data-stu-id="cda83-230">Advanced Message Queuing Protocol (AMQP)</span></span> | 
| <span data-ttu-id="cda83-231">*. servicebus.windows.net</span><span class="sxs-lookup"><span data-stu-id="cda83-231">*.servicebus.windows.net</span></span> | <span data-ttu-id="cda83-232">443, 9350-9354</span><span class="sxs-lookup"><span data-stu-id="cda83-232">443, 9350-9354</span></span> | <span data-ttu-id="cda83-233">A Service Bus Relay (a 443-as kér a hozzáférés-vezérlés jogkivonat beszerzése) TCP-n keresztül figyelői</span><span class="sxs-lookup"><span data-stu-id="cda83-233">Listeners on Service Bus Relay over TCP (requires 443 for Access Control token acquisition)</span></span> | 
| <span data-ttu-id="cda83-234">*. frontend.clouddatahub.net</span><span class="sxs-lookup"><span data-stu-id="cda83-234">*.frontend.clouddatahub.net</span></span> | <span data-ttu-id="cda83-235">443</span><span class="sxs-lookup"><span data-stu-id="cda83-235">443</span></span> | <span data-ttu-id="cda83-236">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cda83-236">HTTPS</span></span> | 
| <span data-ttu-id="cda83-237">*. core.windows.net</span><span class="sxs-lookup"><span data-stu-id="cda83-237">*.core.windows.net</span></span> | <span data-ttu-id="cda83-238">443</span><span class="sxs-lookup"><span data-stu-id="cda83-238">443</span></span> | <span data-ttu-id="cda83-239">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cda83-239">HTTPS</span></span> | 
| <span data-ttu-id="cda83-240">login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="cda83-240">login.microsoftonline.com</span></span> | <span data-ttu-id="cda83-241">443</span><span class="sxs-lookup"><span data-stu-id="cda83-241">443</span></span> | <span data-ttu-id="cda83-242">HTTPS</span><span class="sxs-lookup"><span data-stu-id="cda83-242">HTTPS</span></span> | 
| <span data-ttu-id="cda83-243">*. msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="cda83-243">*.msftncsi.com</span></span> | <span data-ttu-id="cda83-244">443</span><span class="sxs-lookup"><span data-stu-id="cda83-244">443</span></span> | <span data-ttu-id="cda83-245">Hello átjáró nem érhető el Power BI-ban hello által használt tootest internetkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="cda83-245">Used tootest internet connectivity when hello gateway is unreachable by hello Power BI service.</span></span> | 

<span data-ttu-id="cda83-246">Ha tooapprove IP-címek hello tartomány helyett, töltse le és használja hello [Microsoft Azure Datacenter IP-címtartományok lista](https://www.microsoft.com/download/details.aspx?id=41653).</span><span class="sxs-lookup"><span data-stu-id="cda83-246">If you have tooapprove IP addresses instead of hello domains, you can download and use hello [Microsoft Azure Datacenter IP ranges list](https://www.microsoft.com/download/details.aspx?id=41653).</span></span> <span data-ttu-id="cda83-247">Bizonyos esetekben hello Azure Service Bus-kapcsolatokat válnak, teljes tartománynevek helyett IP-címet.</span><span class="sxs-lookup"><span data-stu-id="cda83-247">In some cases, hello Azure Service Bus connections are made with IP Address rather than fully qualified domain names.</span></span>

<a name="gateway-cloud-service"></a>
## <a name="how-does-hello-data-gateway-work"></a><span data-ttu-id="cda83-248">Hogyan működik a hello az adatátjárót?</span><span class="sxs-lookup"><span data-stu-id="cda83-248">How does hello data gateway work?</span></span>

<span data-ttu-id="cda83-249">hello adatátjáró elősegíti a Logic Apps alkalmazást, hello átjáró felhőalapú szolgáltatás és a helyszíni adatforrás között gyors és biztonságos kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="cda83-249">hello data gateway facilitates quick and secure communication between your logic app, hello gateway cloud service, and your on-premises data source.</span></span> 

![diagram-for-on-premises-Data-Gateway-flow](./media/logic-apps-gateway-install/how-on-premises-data-gateway-works-flow-diagram.png)

<span data-ttu-id="cda83-251">Ezért ha hello felhőben hello felhasználó csatlakozott tooyour elemen kommunikációja a helyszíni adatforrás:</span><span class="sxs-lookup"><span data-stu-id="cda83-251">So when hello user in hello cloud interacts with an element that's connected tooyour on-premises data source:</span></span>

1. <span data-ttu-id="cda83-252">hello átjáró felhőszolgáltatáshoz lekérdezést, hello titkosított hitelesítő adatok hello adatforrás, valamint hoz létre, és elküldi a hello lekérdezés toohello várólista hello átjáró tooprocess.</span><span class="sxs-lookup"><span data-stu-id="cda83-252">hello gateway cloud service creates a query, along with hello encrypted credentials for hello data source, and sends hello query toohello queue for hello gateway tooprocess.</span></span>

2. <span data-ttu-id="cda83-253">hello átjáró felhőszolgáltatáshoz hello lekérdezés elemzi, és leküldéses értesítések hello kérelem toohello Azure Service Bus.</span><span class="sxs-lookup"><span data-stu-id="cda83-253">hello gateway cloud service analyzes hello query and pushes hello request toohello Azure Service Bus.</span></span>

3. <span data-ttu-id="cda83-254">hello helyszíni adatátjáró hello Azure Service Bus a függőben lévő kérelmek kérdezi le.</span><span class="sxs-lookup"><span data-stu-id="cda83-254">hello on-premises data gateway polls hello Azure Service Bus for pending requests.</span></span>

4. <span data-ttu-id="cda83-255">hello átjáró hello lekérdezés lekérdezi, visszafejti hello hitelesítő adatokat, és toohello adatforrás összekapcsolja ezeket a hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="cda83-255">hello gateway gets hello query, decrypts hello credentials, and connects toohello data source with those credentials.</span></span>

5. <span data-ttu-id="cda83-256">hello átjáró küldi hello lekérdezési toohello adatforrás végrehajtásra.</span><span class="sxs-lookup"><span data-stu-id="cda83-256">hello gateway sends hello query toohello data source for execution.</span></span>

6. <span data-ttu-id="cda83-257">hello eredmények küldi hello adatforrás hátsó toohello átjáró és toohello átjáró felhőszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="cda83-257">hello results are sent from hello data source, back toohello gateway, and then toohello gateway cloud service.</span></span> <span data-ttu-id="cda83-258">hello átjáró felhőszolgáltatáshoz ezután hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="cda83-258">hello gateway cloud service then uses hello results.</span></span>

<a name="faq"></a>
## <a name="frequently-asked-questions"></a><span data-ttu-id="cda83-259">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="cda83-259">Frequently asked questions</span></span>

### <a name="general"></a><span data-ttu-id="cda83-260">Általános kérdések</span><span class="sxs-lookup"><span data-stu-id="cda83-260">General</span></span>

<span data-ttu-id="cda83-261">**A Q**: van egy átjáró hello felhőben, például az SQL Azure adatforrások?</span><span class="sxs-lookup"><span data-stu-id="cda83-261">**Q**: Do I need a gateway for data sources in hello cloud, such as SQL Azure?</span></span> <br/><span data-ttu-id="cda83-262">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="cda83-262">
**A**: No.</span></span> <span data-ttu-id="cda83-263">Egy átjáró tooon helyi adatforrások csak csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="cda83-263">A gateway connects tooon-premises data sources only.</span></span>

<span data-ttu-id="cda83-264">**A Q**: rendelkezik hello átjáró hello hello adatforrásként azonos számítógépre telepített toobe?</span><span class="sxs-lookup"><span data-stu-id="cda83-264">**Q**: Does hello gateway have toobe installed on hello same machine as hello data source?</span></span> <br/><span data-ttu-id="cda83-265">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="cda83-265">
**A**: No.</span></span> <span data-ttu-id="cda83-266">hello átjáró toohello az adatforráshoz megadott hello kapcsolat adatainak felhasználásával csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="cda83-266">hello gateway connects toohello data source using hello connection information that was provided.</span></span> <span data-ttu-id="cda83-267">Vegye figyelembe a hello átjáró abban az értelemben, ügyfél-alkalmazásként.</span><span class="sxs-lookup"><span data-stu-id="cda83-267">Consider hello gateway as a client application in this sense.</span></span> <span data-ttu-id="cda83-268">hello csak regisztrálnia kell az hello funkció tooconnect toohello kiszolgáló megadott tartománynévvel.</span><span class="sxs-lookup"><span data-stu-id="cda83-268">hello gateway just needs hello capability tooconnect toohello server name that was provided.</span></span>

<a name="why-azure-work-school-account"></a>

<span data-ttu-id="cda83-269">**A Q**: Miért kell I használja az Azure munkahelyi vagy iskolai fiók toosign a?</span><span class="sxs-lookup"><span data-stu-id="cda83-269">**Q**: Why must I use an Azure work or school account toosign in?</span></span> <br/><span data-ttu-id="cda83-270">
**A**: csak használja az Azure munkahelyi vagy iskolai fiókkal, hello helyszíni az átjáró telepítésekor.</span><span class="sxs-lookup"><span data-stu-id="cda83-270">
**A**: You can only use an Azure work or school account when you install hello on-premises data gateway.</span></span> <span data-ttu-id="cda83-271">A bejelentkezési fiók egy Azure Active Directory (Azure AD) által felügyelt bérlői tárolja.</span><span class="sxs-lookup"><span data-stu-id="cda83-271">Your sign-in account is stored in a tenant that's managed by Azure Active Directory (Azure AD).</span></span> <span data-ttu-id="cda83-272">Az Azure AD-fiókot egyszerű felhasználónév (UPN) általában hello e-mail cím megfelelő.</span><span class="sxs-lookup"><span data-stu-id="cda83-272">Usually, your Azure AD account's user principal name (UPN) matches hello email address.</span></span>

<span data-ttu-id="cda83-273">**A Q**: a hitelesítő adataimat tároló?</span><span class="sxs-lookup"><span data-stu-id="cda83-273">**Q**: Where are my credentials stored?</span></span> <br/><span data-ttu-id="cda83-274">
**A**: hello adatforráshoz beírt hitelesítő adatok titkosítottak és a hello átjáró felhőszolgáltatásban tárolt.</span><span class="sxs-lookup"><span data-stu-id="cda83-274">
**A**: hello credentials that you enter for a data source are encrypted and stored in hello gateway cloud service.</span></span> <span data-ttu-id="cda83-275">hello hitelesítő adatok lesznek visszafejtve hello a helyszíni adatok átjáróra.</span><span class="sxs-lookup"><span data-stu-id="cda83-275">hello credentials are decrypted at hello on-premises data gateway.</span></span>

<span data-ttu-id="cda83-276">**A Q**: vannak-e hálózati sávszélesség vonatkozó követelmények?</span><span class="sxs-lookup"><span data-stu-id="cda83-276">**Q**: Are there any requirements for network bandwidth?</span></span> <br/><span data-ttu-id="cda83-277">
**A**: javasoljuk, hogy rendelkezik-e a hálózati kapcsolat megfelelő teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="cda83-277">
**A**: We recommend that your network connection has good throughput.</span></span> <span data-ttu-id="cda83-278">Minden környezet más, és elküldött adatok mennyisége hello hello eredményekre van hatással.</span><span class="sxs-lookup"><span data-stu-id="cda83-278">Every environment is different, and hello amount of data being sent affects hello results.</span></span> <span data-ttu-id="cda83-279">ExpressRoute használata segíthet a helyszíni és az Azure adatközpontjaiban hello közötti átviteli szintű tooguarantee.</span><span class="sxs-lookup"><span data-stu-id="cda83-279">Using ExpressRoute could help tooguarantee a level of throughput between on-premises and hello Azure datacenters.</span></span>
<span data-ttu-id="cda83-280">Hello külső eszköz Azure sebesség teszt app toohelp mérőműszer használhatja a teljesítményt.</span><span class="sxs-lookup"><span data-stu-id="cda83-280">You can use hello third-party tool Azure Speed Test app toohelp gauge your throughput.</span></span>

<span data-ttu-id="cda83-281">**A Q**: Mi az az hello késés futó lekérdezések tooa adatforrás hello átjáró?</span><span class="sxs-lookup"><span data-stu-id="cda83-281">**Q**: What is hello latency for running queries tooa data source from hello gateway?</span></span> <span data-ttu-id="cda83-282">Mi az a legjobb architektúra hello?</span><span class="sxs-lookup"><span data-stu-id="cda83-282">What is hello best architecture?</span></span> <br/><span data-ttu-id="cda83-283">
**A**: tooreduce hálózati késés, telepítés hello átjáró lehető Bezárás toohello adatforrásként.</span><span class="sxs-lookup"><span data-stu-id="cda83-283">
**A**: tooreduce network latency, install hello gateway as close toohello data source as possible.</span></span> <span data-ttu-id="cda83-284">Hello tényleges adatforrás hello átjáró telepíthető, ha a közelségi kapcsolat minimálisra hello késés rendszerben jelent meg.</span><span class="sxs-lookup"><span data-stu-id="cda83-284">If you can install hello gateway on hello actual data source, this proximity minimizes hello latency introduced.</span></span> <span data-ttu-id="cda83-285">Érdemes lehet túl hello adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="cda83-285">Consider hello datacenters too.</span></span> <span data-ttu-id="cda83-286">Például ha a szolgáltatás hello USA nyugati régiója datacenter használ, és egy Azure virtuális Gépen található SQL-kiszolgálóval rendelkezik, az Azure virtuális gép kell hello USA nyugati régiója túl.</span><span class="sxs-lookup"><span data-stu-id="cda83-286">For example, if your service uses hello West US datacenter, and you have SQL Server hosted in an Azure VM, your Azure VM should be in hello West US too.</span></span> <span data-ttu-id="cda83-287">A közelségi kapcsolat minimálisra csökkenti a késést, és ezzel elkerülheti a kimenő forgalom költségek hello Azure virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cda83-287">This proximity minimizes latency and avoids egress charges on hello Azure VM.</span></span>

<span data-ttu-id="cda83-288">**A Q**: hogyan történik az eredmények küldött vissza toohello felhő?</span><span class="sxs-lookup"><span data-stu-id="cda83-288">**Q**: How are results sent back toohello cloud?</span></span> <br/><span data-ttu-id="cda83-289">
**A**: eredmények hello Azure Service Bus keresztül zajlik.</span><span class="sxs-lookup"><span data-stu-id="cda83-289">
**A**: Results are sent through hello Azure Service Bus.</span></span>

<span data-ttu-id="cda83-290">**A Q**: vannak-e a bejövő kapcsolatok toohello átjáró hello felhőből?</span><span class="sxs-lookup"><span data-stu-id="cda83-290">**Q**: Are there any inbound connections toohello gateway from hello cloud?</span></span> <br/><span data-ttu-id="cda83-291">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="cda83-291">
**A**: No.</span></span> <span data-ttu-id="cda83-292">hello átjáró kimenő kapcsolatok tooAzure Service Bus használja.</span><span class="sxs-lookup"><span data-stu-id="cda83-292">hello gateway uses outbound connections tooAzure Service Bus.</span></span>

<span data-ttu-id="cda83-293">**A Q**: Mi történik, ha az általam letiltottak kimenő kapcsolatokat?</span><span class="sxs-lookup"><span data-stu-id="cda83-293">**Q**: What if I block outbound connections?</span></span> <span data-ttu-id="cda83-294">Mire van szükségem az tooopen?</span><span class="sxs-lookup"><span data-stu-id="cda83-294">What do I need tooopen?</span></span> <br/><span data-ttu-id="cda83-295">
**A**: hello portok és a gazdagépekhez, amelyek hello átjárót használja.</span><span class="sxs-lookup"><span data-stu-id="cda83-295">
**A**: See hello ports and hosts that hello gateway uses.</span></span>

<span data-ttu-id="cda83-296">**A Q**: hello tényleges Windows-szolgáltatás úgynevezett?</span><span class="sxs-lookup"><span data-stu-id="cda83-296">**Q**: What is hello actual Windows service called?</span></span><br/><span data-ttu-id="cda83-297">
**A**: A szolgáltatások a hello átjáró a Power BI Enterprise Gateway szolgáltatás nevezik.</span><span class="sxs-lookup"><span data-stu-id="cda83-297">
**A**: In Services, hello gateway is called Power BI Enterprise Gateway Service.</span></span>

<span data-ttu-id="cda83-298">**A Q**: is hello átjáró Windows-szolgáltatás Azure Active Directory-fiókkal való futtatása?</span><span class="sxs-lookup"><span data-stu-id="cda83-298">**Q**: Can hello gateway Windows service run with an Azure Active Directory account?</span></span> <br/><span data-ttu-id="cda83-299">
**A**: nem.</span><span class="sxs-lookup"><span data-stu-id="cda83-299">
**A**: No.</span></span> <span data-ttu-id="cda83-300">Windows-szolgáltatás hello rendelkeznie kell egy érvényes Windows-fiókot.</span><span class="sxs-lookup"><span data-stu-id="cda83-300">hello Windows service must have a valid Windows account.</span></span> <span data-ttu-id="cda83-301">Alapértelmezés szerint hello szolgáltatás SID NT SERVICE\PBIEgwService hello szolgáltatást futtatja.</span><span class="sxs-lookup"><span data-stu-id="cda83-301">By default, hello service runs with hello Service SID, NT SERVICE\PBIEgwService.</span></span>

### <a name="high-availability-and-disaster-recovery"></a><span data-ttu-id="cda83-302">Magas rendelkezésre állás és vészhelyreállítás</span><span class="sxs-lookup"><span data-stu-id="cda83-302">High availability and disaster recovery</span></span>

<span data-ttu-id="cda83-303">**A Q**: milyen beállítások érhetők el a vész-helyreállítási?</span><span class="sxs-lookup"><span data-stu-id="cda83-303">**Q**: What options are available for disaster recovery?</span></span> <br/><span data-ttu-id="cda83-304">
**A**: hello helyreállítási kulcs toorestore használhatja, vagy helyezze át az átjárót.</span><span class="sxs-lookup"><span data-stu-id="cda83-304">
**A**: You can use hello recovery key toorestore or move a gateway.</span></span> <span data-ttu-id="cda83-305">Hello átjáró telepítésekor adja meg a hello helyreállítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="cda83-305">When you install hello gateway, specify hello recovery key.</span></span>

<span data-ttu-id="cda83-306">**A Q**: Miért érdemes hello hello helyreállítási kulcs?</span><span class="sxs-lookup"><span data-stu-id="cda83-306">**Q**: What is hello benefit of hello recovery key?</span></span> <br/><span data-ttu-id="cda83-307">
**A**: hello helyreállítási kulcs tartalmaz egy módja toomigrate, vagy az átjáró beállításainak katasztrófa utáni helyreállításhoz.</span><span class="sxs-lookup"><span data-stu-id="cda83-307">
**A**: hello recovery key provides a way toomigrate or recover your gateway settings after a disaster.</span></span>

<span data-ttu-id="cda83-308">**A Q**: vannak-e a magas rendelkezésre állás elérésére hello átjáróval engedélyező tervekről?</span><span class="sxs-lookup"><span data-stu-id="cda83-308">**Q**: Are there any plans for enabling high availability scenarios with hello gateway?</span></span> <br/><span data-ttu-id="cda83-309">
**A**: ezek a forgatókönyvek szerepelnek a hello terv, de még nincs ütemterv.</span><span class="sxs-lookup"><span data-stu-id="cda83-309">
**A**: These scenarios are on hello roadmap, but we don't have a timeline yet.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="cda83-310">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="cda83-310">Troubleshooting</span></span>

[!INCLUDE [existing-gateway-location-changed](../../includes/logic-apps-existing-gateway-location-changed.md)]

<span data-ttu-id="cda83-311">**A Q**: hogyan láthatja meg, hogy milyen lekérdezések küldési toohello helyszíni adatforrás?</span><span class="sxs-lookup"><span data-stu-id="cda83-311">**Q**: How can I see what queries are being sent toohello on-premises data source?</span></span> <br/><span data-ttu-id="cda83-312">
**A**: engedélyezheti a lekérdezés nyomon követése, beleértve a küldött hello lekérdezések.</span><span class="sxs-lookup"><span data-stu-id="cda83-312">
**A**: You can enable query tracing, which includes hello queries that are sent.</span></span> <span data-ttu-id="cda83-313">Ne felejtse el vissza toohello eredeti érték Miután befejezte a hibakeresési nyomkövetés toochange lekérdezés.</span><span class="sxs-lookup"><span data-stu-id="cda83-313">Remember toochange query tracing back toohello original value when done troubleshooting.</span></span> <span data-ttu-id="cda83-314">A lekérdezés nyomon követése engedélyezve van a Kilépés hoz létre a nagyobb naplókat.</span><span class="sxs-lookup"><span data-stu-id="cda83-314">Leaving query tracing turned on creates larger logs.</span></span>

<span data-ttu-id="cda83-315">Is megtekintheti, hogy az adatforrás rendelkezik a nyomkövetési lekérdezések eszközök.</span><span class="sxs-lookup"><span data-stu-id="cda83-315">You can also look at tools that your data source has for tracing queries.</span></span> <span data-ttu-id="cda83-316">Például használhatja bővített eseményektől vagy SQL Profiler az SQL Server és az Analysis Services.</span><span class="sxs-lookup"><span data-stu-id="cda83-316">For example, you can use Extended Events or SQL Profiler for SQL Server and Analysis Services.</span></span>

<span data-ttu-id="cda83-317">**A Q**: Ha az hello átjáró naplói nem?</span><span class="sxs-lookup"><span data-stu-id="cda83-317">**Q**: Where are hello gateway logs?</span></span> <br/><span data-ttu-id="cda83-318">
**A**: eszközök lásd a témakör későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="cda83-318">
**A**: See Tools later in this topic.</span></span>

### <a name="update-toohello-latest-version"></a><span data-ttu-id="cda83-319">Frissítés toohello legújabb verziója</span><span class="sxs-lookup"><span data-stu-id="cda83-319">Update toohello latest version</span></span>

<span data-ttu-id="cda83-320">Számos problémájának is surface, amikor hello átjáró verziója elavult.</span><span class="sxs-lookup"><span data-stu-id="cda83-320">Many issues can surface when hello gateway version becomes outdated.</span></span> <span data-ttu-id="cda83-321">Jó általános gyakorlat győződjön meg arról, hogy hello legújabb verzióját használja-e.</span><span class="sxs-lookup"><span data-stu-id="cda83-321">As good general practice, make sure that you use hello latest version.</span></span> <span data-ttu-id="cda83-322">Ha hello átjáró havonta vagy hosszabb ideig nem frissítette, akkor előfordulhat, hogy hello hello átjáró legújabb verzióját telepítse, és ha Reprodukálja hello probléma.</span><span class="sxs-lookup"><span data-stu-id="cda83-322">If you haven't updated hello gateway for a month or longer, you might consider installing hello latest version of hello gateway, and see if you can reproduce hello issue.</span></span>

### <a name="error-failed-tooadd-user-toogroup--2147463168-pbiegwservice-performance-log-users"></a><span data-ttu-id="cda83-323">Hiba: Nem sikerült tooadd felhasználói toogroup.</span><span class="sxs-lookup"><span data-stu-id="cda83-323">Error: Failed tooadd user toogroup.</span></span> <span data-ttu-id="cda83-324">(Felhasználói-2147463168 PBIEgwService teljesítmény)</span><span class="sxs-lookup"><span data-stu-id="cda83-324">(-2147463168 PBIEgwService Performance Log Users)</span></span>

<span data-ttu-id="cda83-325">Ez a hiba kaphat, ha tooinstall hello átjáró tartományvezérlőn, amely nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="cda83-325">You might get this error if you try tooinstall hello gateway on a domain controller, which isn't supported.</span></span> <span data-ttu-id="cda83-326">Győződjön meg arról, hogy egy számítógép, amely a tartományvezérlő nem hello-átjáró üzembe helyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="cda83-326">Make sure that you deploy hello gateway on a machine that isn't a domain controller.</span></span>

## <a name="tools"></a><span data-ttu-id="cda83-327">Eszközök</span><span class="sxs-lookup"><span data-stu-id="cda83-327">Tools</span></span>

### <a name="collect-logs-from-hello-gateway-configurer"></a><span data-ttu-id="cda83-328">Hello átjáró configurer naplóinak gyűjtése</span><span class="sxs-lookup"><span data-stu-id="cda83-328">Collect logs from hello gateway configurer</span></span>

<span data-ttu-id="cda83-329">Hello átjáró több naplók hozhatja létre.</span><span class="sxs-lookup"><span data-stu-id="cda83-329">You can collect several logs for hello gateway.</span></span> <span data-ttu-id="cda83-330">Always start hello naplók!</span><span class="sxs-lookup"><span data-stu-id="cda83-330">Always start with hello logs!</span></span>

#### <a name="installer-logs"></a><span data-ttu-id="cda83-331">Telepítési naplókat</span><span class="sxs-lookup"><span data-stu-id="cda83-331">Installer logs</span></span>

`%localappdata%\Temp\Power_BI_Gateway_–Enterprise.log`

#### <a name="configuration-logs"></a><span data-ttu-id="cda83-332">Konfigurációs naplók</span><span class="sxs-lookup"><span data-stu-id="cda83-332">Configuration logs</span></span>

`%localappdata%\Microsoft\Power BI Enterprise Gateway\GatewayConfigurator.log`

#### <a name="enterprise-gateway-service-logs"></a><span data-ttu-id="cda83-333">Vállalati átjáró service naplóit</span><span class="sxs-lookup"><span data-stu-id="cda83-333">Enterprise gateway service logs</span></span>

`C:\Users\PBIEgwService\AppData\Local\Microsoft\Power BI Enterprise Gateway\EnterpriseGateway.log`

#### <a name="event-logs"></a><span data-ttu-id="cda83-334">Eseménynaplók</span><span class="sxs-lookup"><span data-stu-id="cda83-334">Event logs</span></span>

<span data-ttu-id="cda83-335">Hello az adatkezelési átjáró és PowerBIGateway naplók alapján is megtalálhatja **alkalmazás- és szolgáltatásnaplók**.</span><span class="sxs-lookup"><span data-stu-id="cda83-335">You can find hello Data Management Gateway and PowerBIGateway logs under **Application and Services Logs**.</span></span>

### <a name="fiddler-trace"></a><span data-ttu-id="cda83-336">Fiddler nyomkövetési</span><span class="sxs-lookup"><span data-stu-id="cda83-336">Fiddler Trace</span></span>

<span data-ttu-id="cda83-337">[Fiddler](http://www.telerik.com/fiddler) van egy ingyenes eszközt, amely figyeli a HTTP-forgalom Telerik.</span><span class="sxs-lookup"><span data-stu-id="cda83-337">[Fiddler](http://www.telerik.com/fiddler) is a free tool from Telerik that monitors HTTP traffic.</span></span> <span data-ttu-id="cda83-338">A Power BI-ban hello ügyfél gépről hello forgalom tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="cda83-338">You can see this traffic with hello Power BI service from hello client machine.</span></span> <span data-ttu-id="cda83-339">Ez a szolgáltatás előfordulhat, hogy a hibák és egyéb kapcsolódó információk megjelenítése</span><span class="sxs-lookup"><span data-stu-id="cda83-339">This service might show errors and other related information.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cda83-340">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cda83-340">Next steps</span></span>
    
* [<span data-ttu-id="cda83-341">Csatlakozás tooon helyszíni adatokhoz a logic Apps alkalmazásokból</span><span class="sxs-lookup"><span data-stu-id="cda83-341">Connect tooon-premises data from logic apps</span></span>](../logic-apps/logic-apps-gateway-connection.md)
* [<span data-ttu-id="cda83-342">Vállalati integrációs szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="cda83-342">Enterprise integration features</span></span>](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [<span data-ttu-id="cda83-343">Az Azure Logic Apps összekötők</span><span class="sxs-lookup"><span data-stu-id="cda83-343">Connectors for Azure Logic Apps</span></span>](../connectors/apis-list.md)
