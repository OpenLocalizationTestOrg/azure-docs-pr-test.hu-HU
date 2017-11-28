---
title: aaaDeploy IDES EHP7 SP3 SAP SAP ERP 6.0 az Azure-on |} Microsoft Docs
description: "SAP ERP 6.0 Azure SAP IDES EHP7 SP3 telepítése"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a><span data-ttu-id="a3637-103">SAP ERP 6.0 Azure SAP IDES EHP7 SP3 telepítése</span><span class="sxs-lookup"><span data-stu-id="a3637-103">Deploy SAP IDES EHP7 SP3 for SAP ERP 6.0 on Azure</span></span>
<span data-ttu-id="a3637-104">Ez a cikk ismerteti, hogyan toodeploy egy SAP hello Windows operációs rendszerrel és az SQL Server fut az Azure-on keresztül hello SAP felhő készülék könyvtára (SAP CAL) 3.0 IDES rendszer.</span><span class="sxs-lookup"><span data-stu-id="a3637-104">This article describes how toodeploy an SAP IDES system running with SQL Server and hello Windows operating system on Azure via hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="a3637-105">hello képernyőképeket hello részletes folyamat megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="a3637-105">hello screenshots show hello step-by-step process.</span></span> <span data-ttu-id="a3637-106">egy másik megoldás toodeploy kövesse hello ugyanazokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a3637-106">toodeploy a different solution, follow hello same steps.</span></span>

<span data-ttu-id="a3637-107">az SAP-CAL, nyissa meg toohello hello toostart [felhő készülék teszteléshez](https://cal.sap.com/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="a3637-107">toostart with hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="a3637-108">SAP is rendelkezik egy blog hello kapcsolatos új [SAP felhő készülék könyvtár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="a3637-108">SAP also has a blog about hello new [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span> 

> [!NOTE]
<span data-ttu-id="a3637-109">2017. május 29. hello Azure Resource Manager üzembe helyezési modellben használatával, továbbá toohello kevésbé előnyben részesített klasszikus üzembe helyezési modell toodeploy hello SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="a3637-109">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="a3637-110">Azt javasoljuk, hogy hello új Resource Manager telepítési modellt használja, és figyelmen kívül hagyhatja a hello klasszikus üzembe helyezési modellel.</span><span class="sxs-lookup"><span data-stu-id="a3637-110">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

<span data-ttu-id="a3637-111">Ha már létrehozott egy SAP naptár fiókja hello Klasszikus modell használó *toocreate egy másik SAP CAL-fiókra lesz szüksége*.</span><span class="sxs-lookup"><span data-stu-id="a3637-111">If you already created an SAP CAL account that uses hello classic model, *you need toocreate another SAP CAL account*.</span></span> <span data-ttu-id="a3637-112">Ezt a fiókot kell tooexclusively központi telepítése az Azure Resource Manager modellt hello használatával.</span><span class="sxs-lookup"><span data-stu-id="a3637-112">This account needs tooexclusively deploy into Azure by using hello Resource Manager model.</span></span>

<span data-ttu-id="a3637-113">Miután bejelentkezik toohello SAP-CAL, hello első oldal általában részletes útmutatást toohello **megoldások** lap.</span><span class="sxs-lookup"><span data-stu-id="a3637-113">After you sign in toohello SAP CAL, hello first page usually leads you toohello **Solutions** page.</span></span> <span data-ttu-id="a3637-114">hello SAP CAL kínált hello megoldások folyamatosan növekvő, így tooscroll túl kicsit toofind hello kívánt megoldáshoz szükséges lehet.</span><span class="sxs-lookup"><span data-stu-id="a3637-114">hello solutions offered on hello SAP CAL are steadily increasing, so you might need tooscroll quite a bit toofind hello solution you want.</span></span> <span data-ttu-id="a3637-115">a kijelölt hello SAP IDES Windows-alapú megoldással, kizárólag az Azure hello telepítési folyamatának mutatja be:</span><span class="sxs-lookup"><span data-stu-id="a3637-115">hello highlighted Windows-based SAP IDES solution that is available exclusively on Azure demonstrates hello deployment process:</span></span>

![SAP CAL megoldások](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="a3637-117">Hozzon létre egy fiókot az SAP-CAL hello</span><span class="sxs-lookup"><span data-stu-id="a3637-117">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="a3637-118">az SAP-CAL toohello a hello toosign első alkalommal használja az SAP-felhasználó vagy más SAP regisztrált felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a3637-118">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="a3637-119">Majd adja meg egy SAP CAL hello SAP CAL toodeploy készülékek Azure által használt fiókot.</span><span class="sxs-lookup"><span data-stu-id="a3637-119">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="a3637-120">Hello fiók definícióban kell:</span><span class="sxs-lookup"><span data-stu-id="a3637-120">In hello account definition, you need to:</span></span>

    <span data-ttu-id="a3637-121">a.</span><span class="sxs-lookup"><span data-stu-id="a3637-121">a.</span></span> <span data-ttu-id="a3637-122">(Erőforrás-kezelő vagy klasszikus) Azure hello telepítési modell kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="a3637-122">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="a3637-123">b.</span><span class="sxs-lookup"><span data-stu-id="a3637-123">b.</span></span> <span data-ttu-id="a3637-124">Adja meg az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a3637-124">Enter your Azure subscription.</span></span> <span data-ttu-id="a3637-125">Egy SAP naptár fiókja csak tooone előfizetés rendelhetők hozzá.</span><span class="sxs-lookup"><span data-stu-id="a3637-125">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="a3637-126">Ha több előfizetése van szüksége, toocreate egy másik SAP CAL-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="a3637-126">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>
    
    <span data-ttu-id="a3637-127">c.</span><span class="sxs-lookup"><span data-stu-id="a3637-127">c.</span></span> <span data-ttu-id="a3637-128">Hello SAP CAL engedély toodeploy biztosítják azokat az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a3637-128">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="a3637-129">hello következő lépésekből megtudhatja, hogyan toocreate egy SAP CAL Resource Manager üzembe helyezések fiókot.</span><span class="sxs-lookup"><span data-stu-id="a3637-129">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="a3637-130">Ha már rendelkezik egy SAP naptár fiókja, amely csatolt toohello klasszikus üzembe helyezési modellel, hogy *kell* toofollow ezeket a lépéseket toocreate egy új SAP naptár fiókja.</span><span class="sxs-lookup"><span data-stu-id="a3637-130">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="a3637-131">hello új SAP naptár fiókja toodeploy hello Resource Manager modellben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a3637-131">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="a3637-132">egy új SAP-CAL toocreate fiók, hello **fiókok** lapon látható két választási lehetőség az Azure-bA:</span><span class="sxs-lookup"><span data-stu-id="a3637-132">toocreate a new SAP CAL account, hello **Accounts** page shows two choices for Azure:</span></span> 

    <span data-ttu-id="a3637-133">a.</span><span class="sxs-lookup"><span data-stu-id="a3637-133">a.</span></span> <span data-ttu-id="a3637-134">**A Microsoft Azure (klasszikus)** hello klasszikus üzembe helyezési modell, és már nem elsődleges.</span><span class="sxs-lookup"><span data-stu-id="a3637-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="a3637-135">b.</span><span class="sxs-lookup"><span data-stu-id="a3637-135">b.</span></span> <span data-ttu-id="a3637-136">**A Microsoft Azure** hello új Resource Manager üzembe helyezési modellben van.</span><span class="sxs-lookup"><span data-stu-id="a3637-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    ![SAP CAL fiókok](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    <span data-ttu-id="a3637-138">toodeploy hello Resource Manager modellben, válassza ki **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="a3637-138">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![SAP CAL fiókok](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. <span data-ttu-id="a3637-140">Adja meg a hello Azure **előfizetés-azonosító** , amely hello Azure-portálon található.</span><span class="sxs-lookup"><span data-stu-id="a3637-140">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span> 

    ![SAP CAL előfizetés-azonosító](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. <span data-ttu-id="a3637-142">tooauthorize hello SAP CAL toodeploy hello Azure-előfizetés az Ön által definiált, kattintson a **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="a3637-142">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="a3637-143">a következő lap hello hello böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3637-143">hello following page appears in hello browser tab:</span></span>

    ![Az Internet Explorer cloud services – bejelentkezési](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. <span data-ttu-id="a3637-145">Ha egynél több felhasználó szerepel, válassza ki, amely a kijelölt Azure-előfizetés hello csatolt toobe hello coadministrator hello Microsoft-fiók.</span><span class="sxs-lookup"><span data-stu-id="a3637-145">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="a3637-146">a következő lap hello hello böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3637-146">hello following page appears in hello browser tab:</span></span>

    ![Az Internet Explorer felhőalapú szolgáltatások megerősítése](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. <span data-ttu-id="a3637-148">Kattintson a **elfogadása**.</span><span class="sxs-lookup"><span data-stu-id="a3637-148">Click **Accept**.</span></span> <span data-ttu-id="a3637-149">Sikeres hello engedélyezési esetén hello SAP CAL fiók definition újra jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="a3637-149">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="a3637-150">Rövid idő múlva üzenet jelzi, hogy hello engedélyezési folyamat sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="a3637-150">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="a3637-151">tooassign hello az újonnan létrehozott SAP CAL tooyour felhasználója, adja meg a **Felhasználóazonosító** a szövegmezőben a megfelelő hello hello, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a3637-151">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span> 

    ![Fiók toouser társítás](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. <span data-ttu-id="a3637-153">tooassociate hello felhasználói fiókját toosign használhatja az SAP-CAL, toohello kattintson **felülvizsgálati**.</span><span class="sxs-lookup"><span data-stu-id="a3637-153">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 

9. <span data-ttu-id="a3637-154">toocreate hello társítását a felhasználók és az újonnan létrehozott hello SAP naptár fiókja, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a3637-154">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

    ![Felhasználói tooaccount társítása](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

<span data-ttu-id="a3637-156">Sikeresen létrehozott egy SAP naptár fiókja, amely képes:</span><span class="sxs-lookup"><span data-stu-id="a3637-156">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="a3637-157">Hello Resource Manager telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="a3637-157">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="a3637-158">SAP rendszerek üzembe helyezés az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="a3637-158">Deploy SAP systems into your Azure subscription.</span></span>

> [!NOTE]
<span data-ttu-id="a3637-159">Hello SAP IDES megoldás, amely Windows és az SQL Server telepítése, akkor módosítania kell toosign feliratkozott egy SAP naptár-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a3637-159">Before you can deploy hello SAP IDES solution based on Windows and SQL Server, you might need toosign up for an SAP CAL subscription.</span></span> <span data-ttu-id="a3637-160">Ellenkező esetben hello megoldás lehet, hogy megjelennek, **zárolt** hello áttekintése lapon.</span><span class="sxs-lookup"><span data-stu-id="a3637-160">Otherwise, hello solution might show up as **Locked** on hello overview page.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="a3637-161">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="a3637-161">Deploy a solution</span></span>
1. <span data-ttu-id="a3637-162">Miután beállította az SAP naptár fiókja, válassza ki a **SAP IDES megoldás a Windows és az SQL Server hello** megoldás.</span><span class="sxs-lookup"><span data-stu-id="a3637-162">After you set up an SAP CAL account, select **hello SAP IDES solution on Windows and SQL Server** solution.</span></span> <span data-ttu-id="a3637-163">Kattintson a **példány létrehozása**, és erősítse meg a hello használati és a használati feltételeket.</span><span class="sxs-lookup"><span data-stu-id="a3637-163">Click **Create Instance**, and confirm hello usage and terms conditions.</span></span> 

2. <span data-ttu-id="a3637-164">A hello **alapvető mód: példány létrehozása** lapon kell:</span><span class="sxs-lookup"><span data-stu-id="a3637-164">On hello **Basic Mode: Create Instance** page, you need to:</span></span>

    <span data-ttu-id="a3637-165">a.</span><span class="sxs-lookup"><span data-stu-id="a3637-165">a.</span></span> <span data-ttu-id="a3637-166">Adjon meg egy példány **neve**.</span><span class="sxs-lookup"><span data-stu-id="a3637-166">Enter an instance **Name**.</span></span>

    <span data-ttu-id="a3637-167">b.</span><span class="sxs-lookup"><span data-stu-id="a3637-167">b.</span></span> <span data-ttu-id="a3637-168">Válassza ki az Azure **régió**.</span><span class="sxs-lookup"><span data-stu-id="a3637-168">Select an Azure **Region**.</span></span> <span data-ttu-id="a3637-169">Szükség lehet egy SAP CAL előfizetés tooget több Azure-régiók érhető el.</span><span class="sxs-lookup"><span data-stu-id="a3637-169">You might need an SAP CAL subscription tooget multiple Azure regions offered.</span></span>

    <span data-ttu-id="a3637-170">c.</span><span class="sxs-lookup"><span data-stu-id="a3637-170">c.</span></span>  <span data-ttu-id="a3637-171">Adja meg a hello fő **jelszó** hello megoldás, ahogy:</span><span class="sxs-lookup"><span data-stu-id="a3637-171">Enter hello master **Password** for hello solution, as shown:</span></span>

    ![SAP CAL Basic mód: Példány létrehozása](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. <span data-ttu-id="a3637-173">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="a3637-173">Click **Create**.</span></span> <span data-ttu-id="a3637-174">Némi várakozás után attól függően, hogy hello méretét és összetettségét hello megoldás (hello SAP CAL biztosít becsült érték), hello állapota jelenik meg az aktív és használatra kész:</span><span class="sxs-lookup"><span data-stu-id="a3637-174">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use:</span></span> 

    ![SAP CAL-példányok](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. <span data-ttu-id="a3637-176">toofind hello erőforráscsoport és annak által létrehozott objektumok hello SAP-CAL, nyissa meg az Azure portál toohello.</span><span class="sxs-lookup"><span data-stu-id="a3637-176">toofind hello resource group and all its objects that were created by hello SAP CAL, go toohello Azure portal.</span></span> <span data-ttu-id="a3637-177">hello virtuális gép található, hogy az SAP-CAL hello adott azonos példánynév hello kezdve.</span><span class="sxs-lookup"><span data-stu-id="a3637-177">hello virtual machine can be found starting with hello same instance name that was given in hello SAP CAL.</span></span>

    ![Erőforrás objektumok](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. <span data-ttu-id="a3637-179">Hello SAP CAL Portal, nyissa meg toohello telepített példányok és **Connect**.</span><span class="sxs-lookup"><span data-stu-id="a3637-179">On hello SAP CAL portal, go toohello deployed instances and click **Connect**.</span></span> <span data-ttu-id="a3637-180">a következő megjelenő ablakban hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a3637-180">hello following pop-up window appears:</span></span> 

    ![Csatlakozás toohello példány](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. <span data-ttu-id="a3637-182">Használata előtt hello beállítások tooconnect telepített toohello rendszert, kattintson a **Getting Started Guide**.</span><span class="sxs-lookup"><span data-stu-id="a3637-182">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> <span data-ttu-id="a3637-183">hello dokumentáció nevek felhasználók hello hello csatlakozási módszer.</span><span class="sxs-lookup"><span data-stu-id="a3637-183">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="a3637-184">hello jelszava azoknak a felhasználóknak be van állítva az Ön által definiált toohello fő jelszó hello hello telepítési folyamat elején.</span><span class="sxs-lookup"><span data-stu-id="a3637-184">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="a3637-185">Hello dokumentációban, több működési másoknak szerepel a listában a jelszavukat, amelynek segítségével toosign toohello a telepített rendszer.</span><span class="sxs-lookup"><span data-stu-id="a3637-185">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span>

    ![Üdvözli a SAP-dokumentáció](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

<span data-ttu-id="a3637-187">Néhány órán belül egy kifogástalan SAP IDES rendszer van telepítve az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="a3637-187">Within a few hours, a healthy SAP IDES system is deployed in Azure.</span></span>

<span data-ttu-id="a3637-188">Ha egy SAP naptár-előfizetés vásárolta, SAP teljes mértékben támogatja az SAP CAL hello környezeteit Azure.</span><span class="sxs-lookup"><span data-stu-id="a3637-188">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="a3637-189">hello támogatási várólista BC-VCM-CAL.</span><span class="sxs-lookup"><span data-stu-id="a3637-189">hello support queue is BC-VCM-CAL.</span></span>

