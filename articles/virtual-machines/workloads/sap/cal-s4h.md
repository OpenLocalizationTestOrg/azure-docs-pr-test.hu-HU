---
title: "SAP S/4HANA vagy egy Azure virtuális gépen BW/4HANA aaaDeploy |} Microsoft Docs"
description: "SAP S/4HANA vagy BW/4HANA egy Azure virtuális Gépen lévő telepítése"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a><span data-ttu-id="60a6a-103">SAP S/4HANA vagy BW/4HANA Azure telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="60a6a-103">Deploy SAP S/4HANA or BW/4HANA on Azure</span></span>
<span data-ttu-id="60a6a-104">Ez a cikk ismerteti, hogyan toodeploy S/4HANA Azure használatával hello SAP felhő készülék könyvtára (SAP CAL) 3.0.</span><span class="sxs-lookup"><span data-stu-id="60a6a-104">This article describes how toodeploy S/4HANA on Azure by using hello SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="60a6a-105">toodeploy más SAP HANA-alapú megoldások, például a BW/4HANA, hajtsa végre a hello ugyanazokat a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="60a6a-105">toodeploy other SAP HANA-based solutions, such as BW/4HANA, follow hello same steps.</span></span>

> [!NOTE]
<span data-ttu-id="60a6a-106">A SAP CAL hello kapcsolatos további információkért lásd a toohello [felhő készülék teszteléshez](https://cal.sap.com/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="60a6a-106">For more information about hello SAP CAL, go toohello [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="60a6a-107">SAP is rendelkezik kapcsolatos hello blog [SAP felhő készülék könyvtár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="60a6a-107">SAP also has a blog about hello [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span>

> [!NOTE]
<span data-ttu-id="60a6a-108">2017. május 29. hello Azure Resource Manager üzembe helyezési modellben használatával, továbbá toohello kevésbé előnyben részesített klasszikus üzembe helyezési modell toodeploy hello SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="60a6a-108">As of May 29, 2017, you can use hello Azure Resource Manager deployment model in addition toohello less-preferred classic deployment model toodeploy hello SAP CAL.</span></span> <span data-ttu-id="60a6a-109">Azt javasoljuk, hogy hello új Resource Manager telepítési modellt használja, és figyelmen kívül hagyhatja a hello klasszikus üzembe helyezési modellel.</span><span class="sxs-lookup"><span data-stu-id="60a6a-109">We recommend that you use hello new Resource Manager deployment model and disregard hello classic deployment model.</span></span>

## <a name="step-by-step-process-toodeploy-hello-solution"></a><span data-ttu-id="60a6a-110">Részletes folyamat toodeploy hello megoldás</span><span class="sxs-lookup"><span data-stu-id="60a6a-110">Step-by-step process toodeploy hello solution</span></span>

<span data-ttu-id="60a6a-111">a következő feladatütemezési a képernyőfelvételek hello bemutatja, hogyan toodeploy S/4HANA Azure használatával hello SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="60a6a-111">hello following sequence of screenshots shows you how toodeploy S/4HANA on Azure by using hello SAP CAL.</span></span> <span data-ttu-id="60a6a-112">hello folyamat hello működik ugyanúgy más megoldások, például a BW/4HANA.</span><span class="sxs-lookup"><span data-stu-id="60a6a-112">hello process works hello same way for other solutions, such as BW/4HANA.</span></span>

<span data-ttu-id="60a6a-113">Hello **megoldások** lapon látható néhány hello SAP HANA-CAL-alapú megoldások érhető el az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="60a6a-113">hello **Solutions** page shows some of hello SAP CAL HANA-based solutions available on Azure.</span></span> <span data-ttu-id="60a6a-114">**SAP S/4HANA 1610 FPS01, Fully-Activated készülék** hello középső sor:</span><span class="sxs-lookup"><span data-stu-id="60a6a-114">**SAP S/4HANA 1610 FPS01, Fully-Activated Appliance** is in hello middle row:</span></span>

![SAP CAL megoldások](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a><span data-ttu-id="60a6a-116">Hozzon létre egy fiókot az SAP-CAL hello</span><span class="sxs-lookup"><span data-stu-id="60a6a-116">Create an account in hello SAP CAL</span></span>
1. <span data-ttu-id="60a6a-117">az SAP-CAL toohello a hello toosign első alkalommal használja az SAP-felhasználó vagy más SAP regisztrált felhasználó.</span><span class="sxs-lookup"><span data-stu-id="60a6a-117">toosign in toohello SAP CAL for hello first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="60a6a-118">Majd adja meg egy SAP CAL hello SAP CAL toodeploy készülékek Azure által használt fiókot.</span><span class="sxs-lookup"><span data-stu-id="60a6a-118">Then define an SAP CAL account that is used by hello SAP CAL toodeploy appliances on Azure.</span></span> <span data-ttu-id="60a6a-119">Hello fiók definícióban kell:</span><span class="sxs-lookup"><span data-stu-id="60a6a-119">In hello account definition, you need to:</span></span>

    <span data-ttu-id="60a6a-120">a.</span><span class="sxs-lookup"><span data-stu-id="60a6a-120">a.</span></span> <span data-ttu-id="60a6a-121">(Erőforrás-kezelő vagy klasszikus) Azure hello telepítési modell kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="60a6a-121">Select hello deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="60a6a-122">b.</span><span class="sxs-lookup"><span data-stu-id="60a6a-122">b.</span></span> <span data-ttu-id="60a6a-123">Adja meg az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="60a6a-123">Enter your Azure subscription.</span></span> <span data-ttu-id="60a6a-124">Egy SAP naptár fiókja csak tooone előfizetés rendelhetők hozzá.</span><span class="sxs-lookup"><span data-stu-id="60a6a-124">An SAP CAL account can be assigned tooone subscription only.</span></span> <span data-ttu-id="60a6a-125">Ha több előfizetése van szüksége, toocreate egy másik SAP CAL-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="60a6a-125">If you need more than one subscription, you need toocreate another SAP CAL account.</span></span>

    <span data-ttu-id="60a6a-126">c.</span><span class="sxs-lookup"><span data-stu-id="60a6a-126">c.</span></span> <span data-ttu-id="60a6a-127">Hello SAP CAL engedély toodeploy biztosítják azokat az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="60a6a-127">Give hello SAP CAL permission toodeploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="60a6a-128">hello következő lépésekből megtudhatja, hogyan toocreate egy SAP CAL Resource Manager üzembe helyezések fiókot.</span><span class="sxs-lookup"><span data-stu-id="60a6a-128">hello next steps show how toocreate an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="60a6a-129">Ha már rendelkezik egy SAP naptár fiókja, amely csatolt toohello klasszikus üzembe helyezési modellel, hogy *kell* toofollow ezeket a lépéseket toocreate egy új SAP naptár fiókja.</span><span class="sxs-lookup"><span data-stu-id="60a6a-129">If you already have an SAP CAL account that is linked toohello classic deployment model, you *need* toofollow these steps toocreate a new SAP CAL account.</span></span> <span data-ttu-id="60a6a-130">hello új SAP naptár fiókja toodeploy hello Resource Manager modellben van szüksége.</span><span class="sxs-lookup"><span data-stu-id="60a6a-130">hello new SAP CAL account needs toodeploy in hello Resource Manager model.</span></span>

2. <span data-ttu-id="60a6a-131">Hozzon létre egy új SAP naptár fiókja.</span><span class="sxs-lookup"><span data-stu-id="60a6a-131">Create a new SAP CAL account.</span></span> <span data-ttu-id="60a6a-132">Hello **fiókok** lapon látható három lehetősége az Azure-bA:</span><span class="sxs-lookup"><span data-stu-id="60a6a-132">hello **Accounts** page shows three choices for Azure:</span></span> 

    <span data-ttu-id="60a6a-133">a.</span><span class="sxs-lookup"><span data-stu-id="60a6a-133">a.</span></span> <span data-ttu-id="60a6a-134">**A Microsoft Azure (klasszikus)** hello klasszikus üzembe helyezési modell, és már nem elsődleges.</span><span class="sxs-lookup"><span data-stu-id="60a6a-134">**Microsoft Azure (classic)** is hello classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="60a6a-135">b.</span><span class="sxs-lookup"><span data-stu-id="60a6a-135">b.</span></span> <span data-ttu-id="60a6a-136">**A Microsoft Azure** hello új Resource Manager üzembe helyezési modellben van.</span><span class="sxs-lookup"><span data-stu-id="60a6a-136">**Microsoft Azure** is hello new Resource Manager deployment model.</span></span>

    <span data-ttu-id="60a6a-137">c.</span><span class="sxs-lookup"><span data-stu-id="60a6a-137">c.</span></span> <span data-ttu-id="60a6a-138">**Windows Azure 21Vianet által működtetett** hello klasszikus üzembe helyezési modellt használ Kínában beállítás.</span><span class="sxs-lookup"><span data-stu-id="60a6a-138">**Windows Azure operated by 21Vianet** is an option in China that uses hello classic deployment model.</span></span>

    <span data-ttu-id="60a6a-139">toodeploy hello Resource Manager modellben, válassza ki **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-139">toodeploy in hello Resource Manager model, select **Microsoft Azure**.</span></span>

    ![SAP CAL fiókadatok](./media/cal-s4h/s4h-pic-2a.png)

3. <span data-ttu-id="60a6a-141">Adja meg a hello Azure **előfizetés-azonosító** , amely hello Azure-portálon található.</span><span class="sxs-lookup"><span data-stu-id="60a6a-141">Enter hello Azure **Subscription ID** that can be found on hello Azure portal.</span></span>

   ![SAP CAL fiókok](./media/cal-s4h/s4h-pic3c.png)

4. <span data-ttu-id="60a6a-143">tooauthorize hello SAP CAL toodeploy hello Azure-előfizetés az Ön által definiált, kattintson a **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-143">tooauthorize hello SAP CAL toodeploy into hello Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="60a6a-144">a következő lap hello hello böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="60a6a-144">hello following page appears in hello browser tab:</span></span>

   ![Az Internet Explorer cloud services – bejelentkezési](./media/cal-s4h/s4h-pic4c.png)

5. <span data-ttu-id="60a6a-146">Ha egynél több felhasználó szerepel, válassza ki, amely a kijelölt Azure-előfizetés hello csatolt toobe hello coadministrator hello Microsoft-fiók.</span><span class="sxs-lookup"><span data-stu-id="60a6a-146">If more than one user is listed, choose hello Microsoft account that is linked toobe hello coadministrator of hello Azure subscription you selected.</span></span> <span data-ttu-id="60a6a-147">a következő lap hello hello böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="60a6a-147">hello following page appears in hello browser tab:</span></span>

   ![Az Internet Explorer felhőalapú szolgáltatások megerősítése](./media/cal-s4h/s4h-pic5a.png)

6. <span data-ttu-id="60a6a-149">Kattintson a **elfogadása**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-149">Click **Accept**.</span></span> <span data-ttu-id="60a6a-150">Sikeres hello engedélyezési esetén hello SAP CAL fiók definition újra jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="60a6a-150">If hello authorization is successful, hello SAP CAL account definition displays again.</span></span> <span data-ttu-id="60a6a-151">Rövid idő múlva üzenet jelzi, hogy hello engedélyezési folyamat sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="60a6a-151">After a short time, a message confirms that hello authorization process was successful.</span></span>

7. <span data-ttu-id="60a6a-152">tooassign hello az újonnan létrehozott SAP CAL tooyour felhasználója, adja meg a **Felhasználóazonosító** a szövegmezőben a megfelelő hello hello, és kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-152">tooassign hello newly created SAP CAL account tooyour user, enter your **User ID** in hello text box on hello right and click **Add**.</span></span>

   ![Fiók toouser társítás](./media/cal-s4h/s4h-pic8a.png)

8. <span data-ttu-id="60a6a-154">tooassociate hello felhasználói fiókját toosign használhatja az SAP-CAL, toohello kattintson **felülvizsgálati**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-154">tooassociate your account with hello user that you use toosign in toohello SAP CAL, click **Review**.</span></span> 
 
9. <span data-ttu-id="60a6a-155">toocreate hello társítását a felhasználók és az újonnan létrehozott hello SAP naptár fiókja, kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-155">toocreate hello association between your user and hello newly created SAP CAL account, click **Create**.</span></span>

   ![Felhasználói tooSAP CAL fiók társítása](./media/cal-s4h/s4h-pic9b.png)

<span data-ttu-id="60a6a-157">Sikeresen létrehozott egy SAP naptár fiókja, amely képes:</span><span class="sxs-lookup"><span data-stu-id="60a6a-157">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="60a6a-158">Hello Resource Manager telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="60a6a-158">Use hello Resource Manager deployment model.</span></span>
- <span data-ttu-id="60a6a-159">SAP rendszerek üzembe helyezés az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="60a6a-159">Deploy SAP systems into your Azure subscription.</span></span>

<span data-ttu-id="60a6a-160">Most elindíthatja toodeploy S/4HANA az Azure-ban, a felhasználó előfizetéssé.</span><span class="sxs-lookup"><span data-stu-id="60a6a-160">Now you can start toodeploy S/4HANA into your user subscription in Azure.</span></span>

> [!NOTE]
<span data-ttu-id="60a6a-161">A folytatás előtt megállapításához, hogy rendelkezik-e az Azure core kvóták Azure H sorozatú virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="60a6a-161">Before you continue, determine whether you have Azure core quotas for Azure H-Series VMs.</span></span> <span data-ttu-id="60a6a-162">Hello pillanatban SAP CAL hello Azure virtuális gépek H-sorozat toodeploy használ egyes hello SAP HANA-alapú megoldások.</span><span class="sxs-lookup"><span data-stu-id="60a6a-162">At hello moment, hello SAP CAL uses H-Series VMs of Azure toodeploy some of hello SAP HANA-based solutions.</span></span> <span data-ttu-id="60a6a-163">Az Azure-előfizetéshez esetleg nincs bármely H-sorozat core kvóták a H-adatsorozathoz.</span><span class="sxs-lookup"><span data-stu-id="60a6a-163">Your Azure subscription might not have any H-Series core quotas for H-Series.</span></span> <span data-ttu-id="60a6a-164">Ha igen, szükség lehet a toocontact az Azure támogatási tooget legalább 16 H-sorozat magszámra vonatkozó kvóta.</span><span class="sxs-lookup"><span data-stu-id="60a6a-164">If so, you might need toocontact Azure support tooget a quota of at least 16 H-Series cores.</span></span>

> [!NOTE]
<span data-ttu-id="60a6a-165">SAP CAL hello Azure megoldást telepítésekor előfordulhat, hogy csak egy Azure-régió, választhat.</span><span class="sxs-lookup"><span data-stu-id="60a6a-165">When you deploy a solution on Azure in hello SAP CAL, you might find that you can choose only one Azure region.</span></span> <span data-ttu-id="60a6a-166">az Azure-régiók eltérő hello toodeploy egy által javasolt hello SAP-CAL, toopurchase egy SAP CAL előfizetés szükséges.</span><span class="sxs-lookup"><span data-stu-id="60a6a-166">toodeploy into Azure regions other than hello one suggested by hello SAP CAL, you need toopurchase a CAL subscription from SAP.</span></span> <span data-ttu-id="60a6a-167">Is szükség lehet tooopen SAP toohave üzenet a naptár fiók engedélyezett toodeliver az Azure-régiók eltérő hello eredetileg javasolt néhányat a meglévők közül.</span><span class="sxs-lookup"><span data-stu-id="60a6a-167">You also might need tooopen a message with SAP toohave your CAL account enabled toodeliver into Azure regions other than hello ones initially suggested.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="60a6a-168">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="60a6a-168">Deploy a solution</span></span>

<span data-ttu-id="60a6a-169">Helyezzünk üzembe egy megoldást hello **megoldások** hello SAP CAL oldalán.</span><span class="sxs-lookup"><span data-stu-id="60a6a-169">Let's deploy a solution from hello **Solutions** page of hello SAP CAL.</span></span> <span data-ttu-id="60a6a-170">hello SAP CAL két sorozatok toodeploy rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="60a6a-170">hello SAP CAL has two sequences toodeploy:</span></span>

- <span data-ttu-id="60a6a-171">Egy alapszintű feladatütemezési egy lap toodefine hello rendszer toobe telepített használó</span><span class="sxs-lookup"><span data-stu-id="60a6a-171">A basic sequence that uses one page toodefine hello system toobe deployed</span></span>
- <span data-ttu-id="60a6a-172">Egy speciális részeként, amely lehetővé teszi az egyes lehetőségek a Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="60a6a-172">An advanced sequence that gives you certain choices on VM sizes</span></span> 

<span data-ttu-id="60a6a-173">Itt hello alapvető elérési út toodeployment bemutatjuk.</span><span class="sxs-lookup"><span data-stu-id="60a6a-173">We demonstrate hello basic path toodeployment here.</span></span>

1. <span data-ttu-id="60a6a-174">A hello **fiókadatok** lapon kell:</span><span class="sxs-lookup"><span data-stu-id="60a6a-174">On hello **Account Details** page, you need to:</span></span>

    <span data-ttu-id="60a6a-175">a.</span><span class="sxs-lookup"><span data-stu-id="60a6a-175">a.</span></span> <span data-ttu-id="60a6a-176">Válassza ki a SAP CAL fiókot.</span><span class="sxs-lookup"><span data-stu-id="60a6a-176">Select an SAP CAL account.</span></span> <span data-ttu-id="60a6a-177">(Használja a társított toodeploy hello erőforrás-kezelő telepítési modellel rendelkező fiókkal.)</span><span class="sxs-lookup"><span data-stu-id="60a6a-177">(Use an account that is associated toodeploy with hello Resource Manager deployment model.)</span></span>

    <span data-ttu-id="60a6a-178">b.</span><span class="sxs-lookup"><span data-stu-id="60a6a-178">b.</span></span> <span data-ttu-id="60a6a-179">Adjon meg egy példány **neve**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-179">Enter an instance **Name**.</span></span>

    <span data-ttu-id="60a6a-180">c.</span><span class="sxs-lookup"><span data-stu-id="60a6a-180">c.</span></span> <span data-ttu-id="60a6a-181">Válassza ki az Azure **régió**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-181">Select an Azure **Region**.</span></span> <span data-ttu-id="60a6a-182">hello SAP CAL javasol egy régiót.</span><span class="sxs-lookup"><span data-stu-id="60a6a-182">hello SAP CAL suggests a region.</span></span> <span data-ttu-id="60a6a-183">Ha egy másik Azure-régióban van szüksége, és nem kell egy SAP naptár-előfizetés, SAP tooorder Ügyféllicenccel előfizetés kell.</span><span class="sxs-lookup"><span data-stu-id="60a6a-183">If you need another Azure region and you don't have an SAP CAL subscription, you need tooorder a CAL subscription with SAP.</span></span>

    <span data-ttu-id="60a6a-184">d.</span><span class="sxs-lookup"><span data-stu-id="60a6a-184">d.</span></span> <span data-ttu-id="60a6a-185">Adjon meg egy fő **jelszó** hello megoldás nyolc vagy kilenc karaktereket.</span><span class="sxs-lookup"><span data-stu-id="60a6a-185">Enter a master **Password** for hello solution of eight or nine characters.</span></span> <span data-ttu-id="60a6a-186">hello jelszót hello rendszergazdák hello különböző összetevőt használja.</span><span class="sxs-lookup"><span data-stu-id="60a6a-186">hello password is used for hello administrators of hello different components.</span></span>

   ![SAP CAL Basic mód: Példány létrehozása](./media/cal-s4h/s4h-pic10a.png)

2. <span data-ttu-id="60a6a-188">Kattintson a **létrehozása**, és a hello megjelenő, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-188">Click **Create**, and in hello message box that appears, click **OK**.</span></span>

   ![SAP CAL támogatott Virtuálisgép-méretek](./media/cal-s4h/s4h-pic10b.png)

3. <span data-ttu-id="60a6a-190">A hello **titkos kulcs** párbeszédpanel, kattintson a **tároló** toostore hello lévő titkos kulcs hello SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="60a6a-190">In hello **Private Key** dialog box, click **Store** toostore hello private key in hello SAP CAL.</span></span> <span data-ttu-id="60a6a-191">jelszavas védelem toouse hello titkos kulcs, kattintson a **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-191">toouse password protection for hello private key, click **Download**.</span></span> 

   ![SAP CAL titkos kulcs](./media/cal-s4h/s4h-pic10c.png)

4. <span data-ttu-id="60a6a-193">Olvasási hello SAP CAL **figyelmeztetés** üzenet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-193">Read hello SAP CAL **Warning** message, and click **OK**.</span></span>

   ![SAP CAL figyelmeztetés](./media/cal-s4h/s4h-pic10d.png)

    <span data-ttu-id="60a6a-195">Most hello központi telepítés akkor történik meg.</span><span class="sxs-lookup"><span data-stu-id="60a6a-195">Now hello deployment takes place.</span></span> <span data-ttu-id="60a6a-196">Némi várakozás után attól függően, hogy hello méretét és összetettségét hello megoldás (hello SAP CAL biztosít becsült érték), hello állapota jelenik meg az aktív és használatra kész.</span><span class="sxs-lookup"><span data-stu-id="60a6a-196">After some time, depending on hello size and complexity of hello solution (hello SAP CAL provides an estimate), hello status is shown as active and ready for use.</span></span>

5. <span data-ttu-id="60a6a-197">toofind hello virtuális gépek gyűjtik hello más kapcsolódó erőforrás egy erőforráscsoportban, nyissa meg az Azure portál toohello:</span><span class="sxs-lookup"><span data-stu-id="60a6a-197">toofind hello virtual machines collected with hello other associated resources in one resource group, go toohello Azure portal:</span></span> 

   ![SAP CAL objektumok hello új portál telepítése](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. <span data-ttu-id="60a6a-199">Hello SAP CAL-portál hello állapot jelenik meg **aktív**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-199">On hello SAP CAL portal, hello status appears as **Active**.</span></span> <span data-ttu-id="60a6a-200">tooconnect toohello megoldás, kattintson a **Connect**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-200">tooconnect toohello solution, click **Connect**.</span></span> <span data-ttu-id="60a6a-201">Különböző beállítások tooconnect toohello különböző összetevői ebben a megoldásban belül vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="60a6a-201">Different options tooconnect toohello different components are deployed within this solution.</span></span>

   ![SAP CAL-példányok](./media/cal-s4h/active_solution.png)

7. <span data-ttu-id="60a6a-203">Használata előtt hello beállítások tooconnect telepített toohello rendszert, kattintson a **Getting Started Guide**.</span><span class="sxs-lookup"><span data-stu-id="60a6a-203">Before you can use one of hello options tooconnect toohello deployed systems, click **Getting Started Guide**.</span></span> 

   ![Csatlakozás toohello példány](./media/cal-s4h/connect_to_solution.png)

    <span data-ttu-id="60a6a-205">hello dokumentáció nevek felhasználók hello hello csatlakozási módszer.</span><span class="sxs-lookup"><span data-stu-id="60a6a-205">hello documentation names hello users for each of hello connectivity methods.</span></span> <span data-ttu-id="60a6a-206">hello jelszava azoknak a felhasználóknak be van állítva az Ön által definiált toohello fő jelszó hello hello telepítési folyamat elején.</span><span class="sxs-lookup"><span data-stu-id="60a6a-206">hello passwords for those users are set toohello master password you defined at hello beginning of hello deployment process.</span></span> <span data-ttu-id="60a6a-207">Hello dokumentációban, több működési másoknak szerepel a listában a jelszavukat, amelynek segítségével toosign toohello a telepített rendszer.</span><span class="sxs-lookup"><span data-stu-id="60a6a-207">In hello documentation, other more functional users are listed with their passwords, which you can use toosign in toohello deployed system.</span></span> 

    <span data-ttu-id="60a6a-208">Például használatakor hello SAP grafikus felhasználói felület, amely előre telepítve van a Windows távoli asztali gépen hello hello S/4 rendszer nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="60a6a-208">For example, if you use hello SAP GUI that's preinstalled on hello Windows Remote Desktop machine, hello S/4 system might look like this:</span></span>

   ![A hello SM50 előtelepített SAP grafikus felhasználói felületen](./media/cal-s4h/gui_sm50.png)

    <span data-ttu-id="60a6a-210">Vagy ha hello DBACockpit használatához hello példány nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="60a6a-210">Or if you use hello DBACockpit, hello instance might look like this:</span></span>

   ![A hello DBACockpit SAP grafikus felhasználói Felülettel SM50](./media/cal-s4h/dbacockpit.png)

<span data-ttu-id="60a6a-212">Néhány órán belül egy megfelelő SAP S/4 készülék a rendszer az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="60a6a-212">Within a few hours, a healthy SAP S/4 appliance is deployed in Azure.</span></span>

<span data-ttu-id="60a6a-213">Ha egy SAP naptár-előfizetés vásárolta, SAP teljes mértékben támogatja az SAP CAL hello környezeteit Azure.</span><span class="sxs-lookup"><span data-stu-id="60a6a-213">If you bought an SAP CAL subscription, SAP fully supports deployments through hello SAP CAL on Azure.</span></span> <span data-ttu-id="60a6a-214">hello támogatási várólista BC-VCM-CAL.</span><span class="sxs-lookup"><span data-stu-id="60a6a-214">hello support queue is BC-VCM-CAL.</span></span>







