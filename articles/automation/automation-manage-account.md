---
title: "Azure Automation-fiók aaaManage |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toomanage hello például a tanúsítvány megújításához, törlés és helytelen konfigurálása az Automation-fiók konfigurációját."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 75e41f601a138d4e8853aa79dcbab6696a5e9fb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-automation-account"></a><span data-ttu-id="97ed6-103">Azure Automation-fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="97ed6-103">Manage Azure Automation account</span></span>
<span data-ttu-id="97ed6-104">Egy bizonyos ponton az Automation-fiók lejárta előtt toorenew hello tanúsítványt kell.</span><span class="sxs-lookup"><span data-stu-id="97ed6-104">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="97ed6-105">Ha úgy véli, hogy a Futtatás mint fiók hello feltörték, törlése, és hozza létre újból.</span><span class="sxs-lookup"><span data-stu-id="97ed6-105">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="97ed6-106">Ez a szakasz ismerteti hogyan tooperform ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="97ed6-106">This section discusses how tooperform these operations.</span></span>

## <a name="self-signed-certificate-renewal"></a><span data-ttu-id="97ed6-107">Önaláírt tanúsítvány megújítása</span><span class="sxs-lookup"><span data-stu-id="97ed6-107">Self-signed certificate renewal</span></span>
<span data-ttu-id="97ed6-108">hello önaláírt tanúsítványt, amelyet létrehozott hello futtató fiók létrehozásának dátuma hello egy év lejár.</span><span class="sxs-lookup"><span data-stu-id="97ed6-108">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="97ed6-109">A tanúsítványt bármikor meg lehet újítani a lejárata előtt.</span><span class="sxs-lookup"><span data-stu-id="97ed6-109">You can renew it at any time before it expires.</span></span> <span data-ttu-id="97ed6-110">Amikor megújítja, a hello aktuális érvényes tanúsítványra, hogy legfeljebb vagy aktívan futó ütemezett, és az, hogy azokkal hello Futtatás mint fiókot, runbook érintett nem negatív megtartott tooensure.</span><span class="sxs-lookup"><span data-stu-id="97ed6-110">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="97ed6-111">amíg a lejárati dátum nem érvényes marad hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="97ed6-111">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="97ed6-112">Ha konfigurálta az Automation Futtatás mint fiók toouse a vállalati hitelesítésszolgáltató által kiállított tanúsítványt, és ezt a beállítást használja, hello vállalati tanúsítvány helyébe egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="97ed6-112">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="97ed6-113">toorenew hello tanúsítvány, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="97ed6-113">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="97ed6-114">Hello Azure-portálon nyissa meg a hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="97ed6-114">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="97ed6-115">A hello **Automation-fiók** paneljén, hello **tulajdonságok fiók** ablaktáblán, a **Fiókbeállítások**, jelölje be **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="97ed6-115">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Az Automation-fiók tulajdonságpanelje](media/automation-manage-account/automation-account-properties-pane.png)
3. <span data-ttu-id="97ed6-117">A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy hello klasszikus Futtatás mint fiókot, amelyet toorenew hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="97ed6-117">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="97ed6-118">A hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **tanúsítvány megújítása**.</span><span class="sxs-lookup"><span data-stu-id="97ed6-118">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Futtató fiók tanúsítványának megújítása](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="97ed6-120">Hello tanúsítvány megújítása folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="97ed6-120">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

## <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="97ed6-121">Futtató fiók vagy klasszikus futtató fiók törlése</span><span class="sxs-lookup"><span data-stu-id="97ed6-121">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="97ed6-122">Ez a szakasz ismerteti, hogyan toodelete, majd hozza létre a Futtatás mint vagy a klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="97ed6-122">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="97ed6-123">Ez a művelet végrehajtásakor hello Automation-fiók őrződnek meg.</span><span class="sxs-lookup"><span data-stu-id="97ed6-123">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="97ed6-124">A Futtatás mint vagy a klasszikus futtató fiók törlése után újra létrehozhatja a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="97ed6-124">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="97ed6-125">Hello Azure-portálon nyissa meg a hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="97ed6-125">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="97ed6-126">A hello **Automation-fiók** hello fiók tulajdonságok panelen, jelölje be a panelt **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="97ed6-126">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="97ed6-127">A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy a klasszikus futtató fiókot, amelyet az toodelete.</span><span class="sxs-lookup"><span data-stu-id="97ed6-127">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="97ed6-128">Ezután a hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="97ed6-128">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Futtató fiók törlése](media/automation-manage-account/automation-account-delete-runas.png)

4. <span data-ttu-id="97ed6-130">Hello fiók törlése folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="97ed6-130">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="97ed6-131">Hello fiók törlése után újra létrehozhatja a hello **futtató fiókok** hello kiválasztásával a Tulajdonságok panelére léphet létrehozása beállítást **Azure futtató fiók**.</span><span class="sxs-lookup"><span data-stu-id="97ed6-131">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Hozza létre újból hello Automation futtató fiók](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a><span data-ttu-id="97ed6-133">Hibás konfiguráció</span><span class="sxs-lookup"><span data-stu-id="97ed6-133">Misconfiguration</span></span>
<span data-ttu-id="97ed6-134">Néhány hello futtató vagy a klasszikus futtató fiók toofunction szükséges konfigurációs elemek megfelelő lehet, hogy törölték vagy helytelenül van létrehozva a kezdeti beállítás során.</span><span class="sxs-lookup"><span data-stu-id="97ed6-134">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="97ed6-135">hello elemek a következők:</span><span class="sxs-lookup"><span data-stu-id="97ed6-135">hello items include:</span></span>

* <span data-ttu-id="97ed6-136">Tanúsítványobjektum</span><span class="sxs-lookup"><span data-stu-id="97ed6-136">Certificate asset</span></span>
* <span data-ttu-id="97ed6-137">Kapcsolatobjektum</span><span class="sxs-lookup"><span data-stu-id="97ed6-137">Connection asset</span></span>
* <span data-ttu-id="97ed6-138">A Futtatás mint fiók hello közreműködői szerepkör eltávolították.</span><span class="sxs-lookup"><span data-stu-id="97ed6-138">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="97ed6-139">Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="97ed6-139">Service principal or application in Azure AD</span></span>

<span data-ttu-id="97ed6-140">Az előző hello és helytelen konfigurálása más példányai, hello Automation-fiók észleli hello változik, és egy állapotát jeleníti meg az *hiányos* a hello **futtató fiókok** hello a Tulajdonságok panelen fiók.</span><span class="sxs-lookup"><span data-stu-id="97ed6-140">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Hiányos futtatófiók-konfigurációs állapot](media/automation-manage-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="97ed6-142">Hello Futtatás mint fiók kiválasztásakor hello fiók **tulajdonságok** ablaktáblán jelennek meg a következő hibaüzenet hello:</span><span class="sxs-lookup"><span data-stu-id="97ed6-142">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="97ed6-144">.</span><span class="sxs-lookup"><span data-stu-id="97ed6-144">.</span></span>

<span data-ttu-id="97ed6-145">A Futtatás mint fiók problémák gyorsan megoldható törli, majd újra létrehozza a hello fiók.</span><span class="sxs-lookup"><span data-stu-id="97ed6-145">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="next-steps"></a><span data-ttu-id="97ed6-146">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="97ed6-146">Next steps</span></span>
* <span data-ttu-id="97ed6-147">Szolgáltatásnevekről kapcsolatos további információkért tekintse meg túl[alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="97ed6-147">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="97ed6-148">Szerepköralapú hozzáférés-vezérlés az Azure Automationben kapcsolatos további információkért tekintse meg túl[szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="97ed6-148">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="97ed6-149">Tanúsítványok és az Azure-szolgáltatásokkal kapcsolatos további információkért tekintse meg túl[tanúsítványok áttekintése Azure-szolgáltatásokhoz](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="97ed6-149">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
