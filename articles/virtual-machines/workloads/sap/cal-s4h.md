---
title: "SAP S/4HANA vagy egy Azure virtuális gépen BW/4HANA telepítése |} Microsoft Docs"
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
ms.openlocfilehash: 4788fa14a6c49d39b5a3096a69b6738f4a5d8cca
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a><span data-ttu-id="cc54f-103">SAP S/4HANA vagy BW/4HANA Azure telepítéséhez</span><span class="sxs-lookup"><span data-stu-id="cc54f-103">Deploy SAP S/4HANA or BW/4HANA on Azure</span></span>
<span data-ttu-id="cc54f-104">Ez a cikk ismerteti a felhő készülék teszteléshez (SAP CAL) 3.0 telepítését az S/4HANA az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="cc54f-104">This article describes how to deploy S/4HANA on Azure by using the SAP Cloud Appliance Library (SAP CAL) 3.0.</span></span> <span data-ttu-id="cc54f-105">Más SAP HANA-alapú megoldások, például a BW/4HANA, központi telepítéséhez hajtsa végre a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="cc54f-105">To deploy other SAP HANA-based solutions, such as BW/4HANA, follow the same steps.</span></span>

> [!NOTE]
<span data-ttu-id="cc54f-106">Az SAP-CAL kapcsolatos további információkért látogasson el a [felhő készülék teszteléshez](https://cal.sap.com/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="cc54f-106">For more information about the SAP CAL, go to the [SAP Cloud Appliance Library](https://cal.sap.com/) website.</span></span> <span data-ttu-id="cc54f-107">SAP is rendelkezik egy blog a [SAP felhő készülék könyvtár 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span><span class="sxs-lookup"><span data-stu-id="cc54f-107">SAP also has a blog about the [SAP Cloud Appliance Library 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).</span></span>

> [!NOTE]
<span data-ttu-id="cc54f-108">2017. május 29. frissítésétől mellett a kevésbé előnyben részesített klasszikus üzembe helyezési modellel, az Azure Resource Manager telepítési modell segítségével telepítheti az SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="cc54f-108">As of May 29, 2017, you can use the Azure Resource Manager deployment model in addition to the less-preferred classic deployment model to deploy the SAP CAL.</span></span> <span data-ttu-id="cc54f-109">Azt javasoljuk, hogy az új központi telepítési Resource Manager modellt használja, és figyelmen kívül hagyhatja a klasszikus üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="cc54f-109">We recommend that you use the new Resource Manager deployment model and disregard the classic deployment model.</span></span>

## <a name="step-by-step-process-to-deploy-the-solution"></a><span data-ttu-id="cc54f-110">A megoldás részletes folyamata</span><span class="sxs-lookup"><span data-stu-id="cc54f-110">Step-by-step process to deploy the solution</span></span>

<span data-ttu-id="cc54f-111">A képernyőfelvételek a következő folyamat bemutatja, hogyan telepítse az S/4HANA Azure az SAP-CAL segítségével.</span><span class="sxs-lookup"><span data-stu-id="cc54f-111">The following sequence of screenshots shows you how to deploy S/4HANA on Azure by using the SAP CAL.</span></span> <span data-ttu-id="cc54f-112">A folyamat más megoldások, például a BW/4HANA ugyanúgy működik.</span><span class="sxs-lookup"><span data-stu-id="cc54f-112">The process works the same way for other solutions, such as BW/4HANA.</span></span>

<span data-ttu-id="cc54f-113">A **megoldások** lap néhány mutatja be a rendelkezésre álló SAP HANA-CAL-alapú megoldások Azure-on.</span><span class="sxs-lookup"><span data-stu-id="cc54f-113">The **Solutions** page shows some of the SAP CAL HANA-based solutions available on Azure.</span></span> <span data-ttu-id="cc54f-114">**SAP S/4HANA 1610 FPS01, Fully-Activated készülék** a középső sor:</span><span class="sxs-lookup"><span data-stu-id="cc54f-114">**SAP S/4HANA 1610 FPS01, Fully-Activated Appliance** is in the middle row:</span></span>

![SAP CAL megoldások](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-the-sap-cal"></a><span data-ttu-id="cc54f-116">Az SAP-CAL, hozzon létre egy fiókot</span><span class="sxs-lookup"><span data-stu-id="cc54f-116">Create an account in the SAP CAL</span></span>
1. <span data-ttu-id="cc54f-117">Jelentkezzen be az SAP-CAL először, használja az SAP-felhasználó vagy más SAP regisztrált felhasználó.</span><span class="sxs-lookup"><span data-stu-id="cc54f-117">To sign in to the SAP CAL for the first time, use your SAP S-User or other user registered with SAP.</span></span> <span data-ttu-id="cc54f-118">Majd adja meg egy SAP naptár fiókja Azure készülékek telepítendő az SAP-CAL által használt.</span><span class="sxs-lookup"><span data-stu-id="cc54f-118">Then define an SAP CAL account that is used by the SAP CAL to deploy appliances on Azure.</span></span> <span data-ttu-id="cc54f-119">A fiók-definícióban kell:</span><span class="sxs-lookup"><span data-stu-id="cc54f-119">In the account definition, you need to:</span></span>

    <span data-ttu-id="cc54f-120">a.</span><span class="sxs-lookup"><span data-stu-id="cc54f-120">a.</span></span> <span data-ttu-id="cc54f-121">(Erőforrás-kezelő vagy klasszikus) Azure telepítési modell kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="cc54f-121">Select the deployment model on Azure (Resource Manager or classic).</span></span>

    <span data-ttu-id="cc54f-122">b.</span><span class="sxs-lookup"><span data-stu-id="cc54f-122">b.</span></span> <span data-ttu-id="cc54f-123">Adja meg az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="cc54f-123">Enter your Azure subscription.</span></span> <span data-ttu-id="cc54f-124">Egy SAP naptár fiókja csak egyetlen előfizetéssel is hozzárendelhető.</span><span class="sxs-lookup"><span data-stu-id="cc54f-124">An SAP CAL account can be assigned to one subscription only.</span></span> <span data-ttu-id="cc54f-125">Ha egynél több előfizetéssel, hozzon létre egy másik SAP naptár fiókja szeretné.</span><span class="sxs-lookup"><span data-stu-id="cc54f-125">If you need more than one subscription, you need to create another SAP CAL account.</span></span>

    <span data-ttu-id="cc54f-126">c.</span><span class="sxs-lookup"><span data-stu-id="cc54f-126">c.</span></span> <span data-ttu-id="cc54f-127">Az SAP-CAL engedélyt üzembe helyezés az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="cc54f-127">Give the SAP CAL permission to deploy into your Azure subscription.</span></span>

    > [!NOTE]
    <span data-ttu-id="cc54f-128">A következő lépések bemutatják, hogyan a Resource Manager üzembe helyezések SAP CAL-fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cc54f-128">The next steps show how to create an SAP CAL account for Resource Manager deployments.</span></span> <span data-ttu-id="cc54f-129">Ha már rendelkezik egy SAP naptár fiókja, amely csatolva van a klasszikus üzembe helyezési modellel, *kell* lépések SAP CAL új fiók létrehozása.</span><span class="sxs-lookup"><span data-stu-id="cc54f-129">If you already have an SAP CAL account that is linked to the classic deployment model, you *need* to follow these steps to create a new SAP CAL account.</span></span> <span data-ttu-id="cc54f-130">Az új SAP CAL fiókot kell üzembe helyezni a Resource Manager modellt.</span><span class="sxs-lookup"><span data-stu-id="cc54f-130">The new SAP CAL account needs to deploy in the Resource Manager model.</span></span>

2. <span data-ttu-id="cc54f-131">Hozzon létre egy új SAP naptár fiókja.</span><span class="sxs-lookup"><span data-stu-id="cc54f-131">Create a new SAP CAL account.</span></span> <span data-ttu-id="cc54f-132">A **fiókok** lapon látható három lehetősége az Azure-bA:</span><span class="sxs-lookup"><span data-stu-id="cc54f-132">The **Accounts** page shows three choices for Azure:</span></span> 

    <span data-ttu-id="cc54f-133">a.</span><span class="sxs-lookup"><span data-stu-id="cc54f-133">a.</span></span> <span data-ttu-id="cc54f-134">**A Microsoft Azure (klasszikus)** a klasszikus üzembe helyezési modellel, és már nem elsődleges.</span><span class="sxs-lookup"><span data-stu-id="cc54f-134">**Microsoft Azure (classic)** is the classic deployment model and is no longer preferred.</span></span>

    <span data-ttu-id="cc54f-135">b.</span><span class="sxs-lookup"><span data-stu-id="cc54f-135">b.</span></span> <span data-ttu-id="cc54f-136">**A Microsoft Azure** az új erőforrás-kezelő telepítési modell.</span><span class="sxs-lookup"><span data-stu-id="cc54f-136">**Microsoft Azure** is the new Resource Manager deployment model.</span></span>

    <span data-ttu-id="cc54f-137">c.</span><span class="sxs-lookup"><span data-stu-id="cc54f-137">c.</span></span> <span data-ttu-id="cc54f-138">**Windows Azure 21Vianet által működtetett** szabályozó beállítást a kínai, amely a klasszikus üzembe helyezési modellt használ.</span><span class="sxs-lookup"><span data-stu-id="cc54f-138">**Windows Azure operated by 21Vianet** is an option in China that uses the classic deployment model.</span></span>

    <span data-ttu-id="cc54f-139">A Resource Manager modellt a telepítéséhez válassza ki a **Microsoft Azure**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-139">To deploy in the Resource Manager model, select **Microsoft Azure**.</span></span>

    ![SAP CAL fiókadatok](./media/cal-s4h/s4h-pic-2a.png)

3. <span data-ttu-id="cc54f-141">Adja meg az Azure **előfizetés-azonosító** , amely az Azure portálon található.</span><span class="sxs-lookup"><span data-stu-id="cc54f-141">Enter the Azure **Subscription ID** that can be found on the Azure portal.</span></span>

   ![SAP CAL fiókok](./media/cal-s4h/s4h-pic3c.png)

4. <span data-ttu-id="cc54f-143">Az SAP-CAL való üzembe helyezés az Azure-előfizetés az Ön által definiált engedélyezésére, kattintson a **engedélyezés**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-143">To authorize the SAP CAL to deploy into the Azure subscription you defined, click **Authorize**.</span></span> <span data-ttu-id="cc54f-144">A következő lap a böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cc54f-144">The following page appears in the browser tab:</span></span>

   ![Az Internet Explorer cloud services – bejelentkezési](./media/cal-s4h/s4h-pic4c.png)

5. <span data-ttu-id="cc54f-146">Ha egynél több felhasználó szerepel a listán, válassza ki a Microsoft-fiókkal, amely csatolva van a kijelölt Azure-előfizetés coadministrator kell.</span><span class="sxs-lookup"><span data-stu-id="cc54f-146">If more than one user is listed, choose the Microsoft account that is linked to be the coadministrator of the Azure subscription you selected.</span></span> <span data-ttu-id="cc54f-147">A következő lap a böngészőlapon jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="cc54f-147">The following page appears in the browser tab:</span></span>

   ![Az Internet Explorer felhőalapú szolgáltatások megerősítése](./media/cal-s4h/s4h-pic5a.png)

6. <span data-ttu-id="cc54f-149">Kattintson a **elfogadása**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-149">Click **Accept**.</span></span> <span data-ttu-id="cc54f-150">Ha az engedélyezési sikeres, az SAP-CAL fiók definíció újra jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="cc54f-150">If the authorization is successful, the SAP CAL account definition displays again.</span></span> <span data-ttu-id="cc54f-151">Rövid idő múlva üzenet jelzi, hogy az engedélyezési folyamat sikeres volt-e.</span><span class="sxs-lookup"><span data-stu-id="cc54f-151">After a short time, a message confirms that the authorization process was successful.</span></span>

7. <span data-ttu-id="cc54f-152">Az újonnan létrehozott SAP naptár fiókja hozzárendelése a felhasználóhoz, adja meg a **Felhasználóazonosító** jobbra, majd a szövegmezőben **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-152">To assign the newly created SAP CAL account to your user, enter your **User ID** in the text box on the right and click **Add**.</span></span>

   ![Fiók és felhasználói társítása](./media/cal-s4h/s4h-pic8a.png)

8. <span data-ttu-id="cc54f-154">A fiók társítása a felhasználót, hogy jelentkezzen be az SAP-CAL segítségével, kattintson a **felülvizsgálati**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-154">To associate your account with the user that you use to sign in to the SAP CAL, click **Review**.</span></span> 
 
9. <span data-ttu-id="cc54f-155">A felhasználó és az újonnan létrehozott SAP naptár fiókja közötti társítás létrehozásához kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-155">To create the association between your user and the newly created SAP CAL account, click **Create**.</span></span>

   ![Felhasználói és SAP CAL fiók társítása](./media/cal-s4h/s4h-pic9b.png)

<span data-ttu-id="cc54f-157">Sikeresen létrehozott egy SAP naptár fiókja, amely képes:</span><span class="sxs-lookup"><span data-stu-id="cc54f-157">You successfully created an SAP CAL account that is able to:</span></span>

- <span data-ttu-id="cc54f-158">A Resource Manager telepítési modellt használja.</span><span class="sxs-lookup"><span data-stu-id="cc54f-158">Use the Resource Manager deployment model.</span></span>
- <span data-ttu-id="cc54f-159">SAP rendszerek üzembe helyezés az Azure-előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="cc54f-159">Deploy SAP systems into your Azure subscription.</span></span>

<span data-ttu-id="cc54f-160">Most már megkezdheti, az S/4HANA üzembe helyezés felhasználói előfizetését az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cc54f-160">Now you can start to deploy S/4HANA into your user subscription in Azure.</span></span>

> [!NOTE]
<span data-ttu-id="cc54f-161">A folytatás előtt megállapításához, hogy rendelkezik-e az Azure core kvóták Azure H sorozatú virtuális gépekhez.</span><span class="sxs-lookup"><span data-stu-id="cc54f-161">Before you continue, determine whether you have Azure core quotas for Azure H-Series VMs.</span></span> <span data-ttu-id="cc54f-162">Az SAP-CAL időpontjában, Azure virtuális gépek H-sorozat használja a központi telepítésére az SAP HANA-alapú megoldások némelyike.</span><span class="sxs-lookup"><span data-stu-id="cc54f-162">At the moment, the SAP CAL uses H-Series VMs of Azure to deploy some of the SAP HANA-based solutions.</span></span> <span data-ttu-id="cc54f-163">Az Azure-előfizetéshez esetleg nincs bármely H-sorozat core kvóták a H-adatsorozathoz.</span><span class="sxs-lookup"><span data-stu-id="cc54f-163">Your Azure subscription might not have any H-Series core quotas for H-Series.</span></span> <span data-ttu-id="cc54f-164">Ha igen, szükség lehet egy legalább 16 H-sorozat magszámra vonatkozó kvóta eléréséhez Azure ügyfélszolgálatához.</span><span class="sxs-lookup"><span data-stu-id="cc54f-164">If so, you might need to contact Azure support to get a quota of at least 16 H-Series cores.</span></span>

> [!NOTE]
<span data-ttu-id="cc54f-165">Amikor telepít egy megoldást az SAP-CAL az Azure-on, előfordulhat, hogy csak egy Azure-régió, választhat.</span><span class="sxs-lookup"><span data-stu-id="cc54f-165">When you deploy a solution on Azure in the SAP CAL, you might find that you can choose only one Azure region.</span></span> <span data-ttu-id="cc54f-166">Üzembe helyezés az SAP-CAL által javasolt más Azure-régiók, a naptár-előfizetés vásárlása SAP kell.</span><span class="sxs-lookup"><span data-stu-id="cc54f-166">To deploy into Azure regions other than the one suggested by the SAP CAL, you need to purchase a CAL subscription from SAP.</span></span> <span data-ttu-id="cc54f-167">Is szükség lehet a naptár fiók engedélyezve van az Azure-régiók eredetileg javasolt eltérő továbbítására SAP rendelkező.</span><span class="sxs-lookup"><span data-stu-id="cc54f-167">You also might need to open a message with SAP to have your CAL account enabled to deliver into Azure regions other than the ones initially suggested.</span></span>

### <a name="deploy-a-solution"></a><span data-ttu-id="cc54f-168">A megoldás üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="cc54f-168">Deploy a solution</span></span>

<span data-ttu-id="cc54f-169">Helyezzünk üzembe egy megoldást a **megoldások** az SAP-CAL oldalán.</span><span class="sxs-lookup"><span data-stu-id="cc54f-169">Let's deploy a solution from the **Solutions** page of the SAP CAL.</span></span> <span data-ttu-id="cc54f-170">Az SAP-CAL van két feladatütemezések központi telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="cc54f-170">The SAP CAL has two sequences to deploy:</span></span>

- <span data-ttu-id="cc54f-171">Egy alapszintű sorozatot, amely egy lap segítségével határozza meg a rendszer</span><span class="sxs-lookup"><span data-stu-id="cc54f-171">A basic sequence that uses one page to define the system to be deployed</span></span>
- <span data-ttu-id="cc54f-172">Egy speciális részeként, amely lehetővé teszi az egyes lehetőségek a Virtuálisgép-méretek</span><span class="sxs-lookup"><span data-stu-id="cc54f-172">An advanced sequence that gives you certain choices on VM sizes</span></span> 

<span data-ttu-id="cc54f-173">Bemutatjuk a központi telepítési itt alapvető elérési útja.</span><span class="sxs-lookup"><span data-stu-id="cc54f-173">We demonstrate the basic path to deployment here.</span></span>

1. <span data-ttu-id="cc54f-174">Az a **fiókadatok** lapon kell:</span><span class="sxs-lookup"><span data-stu-id="cc54f-174">On the **Account Details** page, you need to:</span></span>

    <span data-ttu-id="cc54f-175">a.</span><span class="sxs-lookup"><span data-stu-id="cc54f-175">a.</span></span> <span data-ttu-id="cc54f-176">Válassza ki a SAP CAL fiókot.</span><span class="sxs-lookup"><span data-stu-id="cc54f-176">Select an SAP CAL account.</span></span> <span data-ttu-id="cc54f-177">(Olyan fiókot használjon, amely a Resource Manager üzembe helyezési modellel telepítését társítva van.)</span><span class="sxs-lookup"><span data-stu-id="cc54f-177">(Use an account that is associated to deploy with the Resource Manager deployment model.)</span></span>

    <span data-ttu-id="cc54f-178">b.</span><span class="sxs-lookup"><span data-stu-id="cc54f-178">b.</span></span> <span data-ttu-id="cc54f-179">Adjon meg egy példány **neve**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-179">Enter an instance **Name**.</span></span>

    <span data-ttu-id="cc54f-180">c.</span><span class="sxs-lookup"><span data-stu-id="cc54f-180">c.</span></span> <span data-ttu-id="cc54f-181">Válassza ki az Azure **régió**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-181">Select an Azure **Region**.</span></span> <span data-ttu-id="cc54f-182">Az SAP-CAL javasol egy régiót.</span><span class="sxs-lookup"><span data-stu-id="cc54f-182">The SAP CAL suggests a region.</span></span> <span data-ttu-id="cc54f-183">Ha egy másik Azure-régióban van szüksége, és egy SAP naptár-előfizetés nem rendelkezik, egy SAP CAL előfizetés sorrendben szeretné.</span><span class="sxs-lookup"><span data-stu-id="cc54f-183">If you need another Azure region and you don't have an SAP CAL subscription, you need to order a CAL subscription with SAP.</span></span>

    <span data-ttu-id="cc54f-184">d.</span><span class="sxs-lookup"><span data-stu-id="cc54f-184">d.</span></span> <span data-ttu-id="cc54f-185">Adjon meg egy fő **jelszó** nyolc vagy kilenc karaktereket a megoldáshoz.</span><span class="sxs-lookup"><span data-stu-id="cc54f-185">Enter a master **Password** for the solution of eight or nine characters.</span></span> <span data-ttu-id="cc54f-186">A jelszót a rendszergazdák a különböző összetevők használja.</span><span class="sxs-lookup"><span data-stu-id="cc54f-186">The password is used for the administrators of the different components.</span></span>

   ![SAP CAL Basic mód: Példány létrehozása](./media/cal-s4h/s4h-pic10a.png)

2. <span data-ttu-id="cc54f-188">Kattintson a **létrehozása**, és kattintson a párbeszédpanelen megjelenik, **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-188">Click **Create**, and in the message box that appears, click **OK**.</span></span>

   ![SAP CAL támogatott Virtuálisgép-méretek](./media/cal-s4h/s4h-pic10b.png)

3. <span data-ttu-id="cc54f-190">Az a **titkos kulcs** párbeszédpanel, kattintson a **tárolására** a titkos kulcs tárolása az SAP-CAL.</span><span class="sxs-lookup"><span data-stu-id="cc54f-190">In the **Private Key** dialog box, click **Store** to store the private key in the SAP CAL.</span></span> <span data-ttu-id="cc54f-191">Jelszavas védelem a titkos kulcs használatához kattintson **letöltése**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-191">To use password protection for the private key, click **Download**.</span></span> 

   ![SAP CAL titkos kulcs](./media/cal-s4h/s4h-pic10c.png)

4. <span data-ttu-id="cc54f-193">Olvassa el az SAP-CAL **figyelmeztetés** üzenet, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-193">Read the SAP CAL **Warning** message, and click **OK**.</span></span>

   ![SAP CAL figyelmeztetés](./media/cal-s4h/s4h-pic10d.png)

    <span data-ttu-id="cc54f-195">Most már a központi telepítés akkor történik meg.</span><span class="sxs-lookup"><span data-stu-id="cc54f-195">Now the deployment takes place.</span></span> <span data-ttu-id="cc54f-196">Némi várakozás után attól függően, hogy méretét és összetettségét (az SAP-CAL biztosít becsült érték) megoldás állapota jelenik meg az aktív és használatra kész.</span><span class="sxs-lookup"><span data-stu-id="cc54f-196">After some time, depending on the size and complexity of the solution (the SAP CAL provides an estimate), the status is shown as active and ready for use.</span></span>

5. <span data-ttu-id="cc54f-197">A virtuális gépek egy erőforráscsoporthoz tartozik, a más társított erőforrásokkal rendelkező gyűjtött megkereséséhez nyissa meg az Azure-portálon:</span><span class="sxs-lookup"><span data-stu-id="cc54f-197">To find the virtual machines collected with the other associated resources in one resource group, go to the Azure portal:</span></span> 

   ![SAP CAL objektumok az új portál telepítése](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. <span data-ttu-id="cc54f-199">Az SAP-CAL-portál, az állapot jelenik meg **aktív**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-199">On the SAP CAL portal, the status appears as **Active**.</span></span> <span data-ttu-id="cc54f-200">A megoldás való kapcsolódáshoz kattintson **Connect**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-200">To connect to the solution, click **Connect**.</span></span> <span data-ttu-id="cc54f-201">Különböző beállítások a különböző összetevők csatlakozni ehhez a megoldáshoz belül vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="cc54f-201">Different options to connect to the different components are deployed within this solution.</span></span>

   ![SAP CAL-példányok](./media/cal-s4h/active_solution.png)

7. <span data-ttu-id="cc54f-203">Kattintson a használata előtt a egyikére kapcsolódni a telepített rendszereken, **Getting Started Guide**.</span><span class="sxs-lookup"><span data-stu-id="cc54f-203">Before you can use one of the options to connect to the deployed systems, click **Getting Started Guide**.</span></span> 

   ![Kapcsolódjon ahhoz a példányhoz](./media/cal-s4h/connect_to_solution.png)

    <span data-ttu-id="cc54f-205">A dokumentáció a felhasználók számára a csatlakozási módszer nevet.</span><span class="sxs-lookup"><span data-stu-id="cc54f-205">The documentation names the users for each of the connectivity methods.</span></span> <span data-ttu-id="cc54f-206">A jelszavak azoknak a felhasználóknak a fő jelszót, amelyet megadott, a telepítési folyamat elején értékre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="cc54f-206">The passwords for those users are set to the master password you defined at the beginning of the deployment process.</span></span> <span data-ttu-id="cc54f-207">A dokumentáció több működési másoknak szerepel a listában a jelszavukat, amelyek segítségével jelentkezzen be a telepített rendszer.</span><span class="sxs-lookup"><span data-stu-id="cc54f-207">In the documentation, other more functional users are listed with their passwords, which you can use to sign in to the deployed system.</span></span> 

    <span data-ttu-id="cc54f-208">Például ha használja a SAP grafikus felhasználói felület, amely előre telepítve van a Windows távoli asztali gépen, a S/4 rendszer nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="cc54f-208">For example, if you use the SAP GUI that's preinstalled on the Windows Remote Desktop machine, the S/4 system might look like this:</span></span>

   ![SM50 előtelepített SAP grafikus felhasználói felületen](./media/cal-s4h/gui_sm50.png)

    <span data-ttu-id="cc54f-210">Vagy ha a DBACockpit használja, a példány nézhet ki:</span><span class="sxs-lookup"><span data-stu-id="cc54f-210">Or if you use the DBACockpit, the instance might look like this:</span></span>

   ![SM50 DBACockpit SAP grafikus felhasználói felületen](./media/cal-s4h/dbacockpit.png)

<span data-ttu-id="cc54f-212">Néhány órán belül egy megfelelő SAP S/4 készülék a rendszer az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cc54f-212">Within a few hours, a healthy SAP S/4 appliance is deployed in Azure.</span></span>

<span data-ttu-id="cc54f-213">Ha egy SAP naptár-előfizetés vásárolta, SAP Azure teljes mértékben támogatja az SAP-CAL környezeteit.</span><span class="sxs-lookup"><span data-stu-id="cc54f-213">If you bought an SAP CAL subscription, SAP fully supports deployments through the SAP CAL on Azure.</span></span> <span data-ttu-id="cc54f-214">A támogatási várólista BC-VCM-CAL.</span><span class="sxs-lookup"><span data-stu-id="cc54f-214">The support queue is BC-VCM-CAL.</span></span>







