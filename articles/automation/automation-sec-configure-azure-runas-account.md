---
title: "Azure-beli futtató fiók konfigurálása | Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti az egyszerű biztonsági hitelesítés létrehozásán, tesztelésén és használatán az Azure Automationben."
services: automation
documentationcenter: 
author: mgoedtel
manager: carmonm
editor: 
keywords: "egyszerű szolgáltatásnév, setspn, azure-hitelesítés"
ms.assetid: 2f783441-15c7-4ea0-ba27-d7daa39b1dd3
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 04/06/2017
ms.author: magoedte
ROBOTS: NOINDEX
redirect_url: /azure/automation/
redirect_document_id: TRUE
ms.openlocfilehash: f88c775eba19bb227d0e206d6420c08b57305b92
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="d08a7-104">Runbookok hitelesítése Azure-beli futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="d08a7-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="d08a7-105">Ebből a cikkből megtudhatja, hogyan konfigurálhat Azure Automation-fiókot az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="d08a7-105">This article shows you how to configure an Azure Automation account in the Azure portal.</span></span> <span data-ttu-id="d08a7-106">Ehhez a Futtató fiók funkció segítségével hitelesítse az Azure Resource Managerben vagy az Azure Service Managerben erőforrásokat kezelő runbookokat.</span><span class="sxs-lookup"><span data-stu-id="d08a7-106">To do so, you use the Run As account feature to authenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="d08a7-107">Amikor egy Automation-fiókot hoz létre az Azure Portalon, automatikusan két fiók jön létre:</span><span class="sxs-lookup"><span data-stu-id="d08a7-107">When you create an Automation account in the Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="d08a7-108">Egy futtató fiók.</span><span class="sxs-lookup"><span data-stu-id="d08a7-108">A Run As account.</span></span> <span data-ttu-id="d08a7-109">Ez a fiók létrehoz egy egyszerű szolgáltatást az Azure Active Directory-ban (Azure AD), valamint egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d08a7-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="d08a7-110">Emellett kiosztja a Közreműködő szerepköralapú hozzáférés-vezérlést (RBAC), amely runbookok használatával kezeli a Resource Manager-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d08a7-110">It also assigns the Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="d08a7-111">Egy klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-111">A Classic Run As account.</span></span> <span data-ttu-id="d08a7-112">Ez a fiók feltölt egy felügyeleti tanúsítványt, amelynek használatával a Szolgáltatásfelügyelet erőforrásai vagy a klasszikus erőforrások kezelhetők runbookokkal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-112">This account uploads a management certificate, which is used to manage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="d08a7-113">Az Automation-fiók létrehozása leegyszerűsíti a folyamatot, valamint segít a tervezés gyors megkezdésében és a runbookok üzembe helyezésében az automatizálási szükségletek támogatására.</span><span class="sxs-lookup"><span data-stu-id="d08a7-113">Creating an Automation account simplifies the process for you and helps you quickly start building and deploying runbooks to support your automation needs.</span></span>

<span data-ttu-id="d08a7-114">A futtató és klasszikus futtató fiókok segítségével a következőket teheti meg:</span><span class="sxs-lookup"><span data-stu-id="d08a7-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="d08a7-115">Szabványos módot biztosíthat az Azure-hitelesítésre abban az esetben, ha runbookokból származó Resource Manager- vagy Azure Service Management-erőforrásokat felügyel az Azure Portal webhelyen.</span><span class="sxs-lookup"><span data-stu-id="d08a7-115">Provide a standardized way to authenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in the Azure portal.</span></span>
* <span data-ttu-id="d08a7-116">Automatizálhatja az Azure Alerts szolgáltatásban konfigurálható globális runbookok használatát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-116">Automate the use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="d08a7-117">Az Automation globális runbookokhoz készült [Azure Alert integrációs funkció](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) használatához szükség van egy futtató és klasszikus futtató fiókokkal konfigurált Automation-fiókra.</span><span class="sxs-lookup"><span data-stu-id="d08a7-117">The [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="d08a7-118">Kiválaszthat egy olyan Automation-fiókot, amelyhez már tartozik futtató és klasszikus futtató fiók, vagy létrehozhat egy új Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose to create a new Automation account.</span></span>
>  

<span data-ttu-id="d08a7-119">A cikkben bemutatjuk, hogyan hozhat létre Automation-fiókot az Azure Portalról, hogyan frissítheti Automation-fiókját az Azure PowerShell-lel, hogyan kezelheti a fiók konfigurációját, illetve hogyan végezhet hitelesítést a runbookokban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-119">This article shows how to create an Automation account from the Azure portal, update an Automation account by using Azure PowerShell, manage the account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="d08a7-120">Az Automation-fiók létrehozása előtt érdemes áttekintenie és megfontolnia a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-120">Before you begin creating an Automation account, it's a good idea to understand and consider the following:</span></span>

* <span data-ttu-id="d08a7-121">Az Automation-fiók létrehozása nincs hatással a korábban a klasszikus vagy a Resource Manager-alapú üzemi modell keretében esetleg már létrehozott Automation-fiókokra.</span><span class="sxs-lookup"><span data-stu-id="d08a7-121">Creating an Automation account does not affect Automation accounts you might have already created in either the classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="d08a7-122">Ez a folyamat kizárólag olyan Automation-fiókok esetében működik, amelyeket az Azure Portalon hozott létre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-122">The process works only for Automation accounts that you create in the Azure portal.</span></span> <span data-ttu-id="d08a7-123">Ha a klasszikus Azure portálról hoz létre fiókot, a rendszer nem replikálja a futtató fiók konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="d08a7-123">Attempting to create an account from the Azure classic portal does not replicate the Run As account configuration.</span></span>
* <span data-ttu-id="d08a7-124">Ha jelenleg rendelkezik olyan runbookokkal és objektumokkal (például ütemezésekkel vagy változókkal), amelyeket klasszikus erőforrások felügyeletére hozott létre, és szeretné, hogy ezek a runbookok hitelesítést végezzenek az új klasszikus futtató fiókkal, a következő lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d08a7-124">If you already have runbooks and assets (such as schedules or variables) in place to manage classic resources, and you want runbooks to authenticate with the new Classic Run As account, do either of the following:</span></span>

  * <span data-ttu-id="d08a7-125">Klasszikus futtató fiók létrehozásához kövesse a „Saját futtató fiók kezelése” szakaszban megadott utasításokat.</span><span class="sxs-lookup"><span data-stu-id="d08a7-125">To create a Classic Run As account, follow the instructions in the "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="d08a7-126">Meglévő fiók frissítéséhez használja „Az Automation-fiók frissítése a PowerShell használatával” szakaszban megadott PowerShell-szkriptet.</span><span class="sxs-lookup"><span data-stu-id="d08a7-126">To update your existing account, use the PowerShell script in the "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="d08a7-127">Ha az új futtató fiókkal és a klasszikus futtató Automation-fiókkal szeretne hitelesítést végezni, a [Hitelesítésikód-példák](#authentication-code-examples) szakaszban található példakód segítségével módosítania kell meglévő runbookjait.</span><span class="sxs-lookup"><span data-stu-id="d08a7-127">To authenticate by using the new Run As account and Classic Run As Automation account, you  need to modify your existing runbooks with the example code provided in the section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="d08a7-128">A futtató fiók Resource Manager-erőforrásokon, tanúsítványalapú szolgáltatásnévvel történő hitelesítésre szolgál.</span><span class="sxs-lookup"><span data-stu-id="d08a7-128">The Run As account is for authentication against Resource Manager resources using the certificate-based service principal.</span></span> <span data-ttu-id="d08a7-129">A klasszikus futtató fiók Service Manager-erőforrásokon, felügyeleti tanúsítvánnyal történő hitelesítésére szolgál.</span><span class="sxs-lookup"><span data-stu-id="d08a7-129">The Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-the-azure-portal"></a><span data-ttu-id="d08a7-130">Egy Automation-fiók létrehozása az Azure Portalról</span><span class="sxs-lookup"><span data-stu-id="d08a7-130">Create an Automation account from the Azure portal</span></span>
<span data-ttu-id="d08a7-131">Ebben a szakaszban olyan Azure Automation-fiókot hoz létre az Azure Portalról, amely egy futtató fiókot és egy klasszikus futtató fiókot is létrehoz.</span><span class="sxs-lookup"><span data-stu-id="d08a7-131">In this section, you create an Azure Automation account from the Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="d08a7-132">Automation-fiók létrehozásához a Szolgáltatás-adminisztrátorok szerepkör tagjának, vagy azon előfizetés társadminisztrátorának kell lennie, amely hozzáférést biztosít az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="d08a7-132">To create an Automation account, you must be a member of the Service Admins role or co-administrator of the subscription that is granting access to the subscription.</span></span> <span data-ttu-id="d08a7-133">Ezenfelül felhasználóként hozzá kell legyen rendelve az előfizetés alapértelmezett Active Directory-példányához.</span><span class="sxs-lookup"><span data-stu-id="d08a7-133">You must also be added as a user to that subscription's default Active Directory instance.</span></span> <span data-ttu-id="d08a7-134">A fiókhoz nem szükséges kiemelt szerepkört rendelni.</span><span class="sxs-lookup"><span data-stu-id="d08a7-134">The account does not need to be assigned a privileged role.</span></span>
>
><span data-ttu-id="d08a7-135">Ha nem tagja az előfizetéshez tartozó Active Directory-példánynak, mielőtt hozzáadják az előfizetés társadminisztrátori szerepköréhez, vendégként lesz hozzáadva az Active Directoryhoz.</span><span class="sxs-lookup"><span data-stu-id="d08a7-135">If you are not a member of the subscription’s Active Directory instance before you are added to the co-administrator role of the subscription, you will be added to Active Directory as a guest.</span></span> <span data-ttu-id="d08a7-136">Ez esetben „Nincs engedélye létrehozni…”</span><span class="sxs-lookup"><span data-stu-id="d08a7-136">In this instance, you will receive a “You do not have permissions to create…”</span></span> <span data-ttu-id="d08a7-137">figyelmeztető üzenetet kap az **Automation-fiók hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="d08a7-137">warning on the **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="d08a7-138">Az először a társadminisztrátori szerepkörhöz hozzáadott felhasználók eltávolíthatók az előfizetéshez tartozó Active Directory-példányból, majd újra hozzáadhatók, így teljes jogú felhasználók lehetnek az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-138">Users who were added to the co-administrator role first can be removed from the subscription's Active Directory instance and re-added to make them a full User in Active Directory.</span></span> <span data-ttu-id="d08a7-139">Ez a helyzet úgy ellenőrizhető, ha az Azure Portal **Azure Active Directory** panelén a **Felhasználók és csoportok** és a **Minden felhasználó** elemre kattint, majd a konkrét felhasználó kiválasztása után a **Profil** elemet választja.</span><span class="sxs-lookup"><span data-stu-id="d08a7-139">To verify this situation from the **Azure Active Directory** pane in the Azure portal by selecting **Users and groups**, selecting **All users** and, after you select the specific user, selecting **Profile**.</span></span> <span data-ttu-id="d08a7-140">A felhasználók profilja alatti **Felhasználó típusa** attribútum értéke ne legyen **Guest** (vendég).</span><span class="sxs-lookup"><span data-stu-id="d08a7-140">The value of the **User type** attribute under the users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="d08a7-141">Jelentkezzen be az Azure Portalra egy olyan fiókkal, amely tagja az Előfizetés-adminisztrátorok szerepkörnek, és emellett az előfizetés társadminisztrátorának is számít.</span><span class="sxs-lookup"><span data-stu-id="d08a7-141">Sign in to the Azure portal with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>

2. <span data-ttu-id="d08a7-142">Válassza az **Automation-fiókok** elemet.</span><span class="sxs-lookup"><span data-stu-id="d08a7-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="d08a7-143">Az **Automation-fiókok** panelen kattintson a **Hozzáadás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-143">On the **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="d08a7-144">Ekkor megnyílik az **Automation-fiók hozzáadása** panel.</span><span class="sxs-lookup"><span data-stu-id="d08a7-144">The **Add Automation Account** blade opens.</span></span>

 ![Az „Automation-fiók hozzáadása” panel](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="d08a7-146">Ha a fiók nem tagja az Előfizetés-adminisztrátorok szerepkörnek, illetve nem az előfizetés társadminisztrátora, az **Automation-fiók hozzáadása** panelen a következő figyelmeztetés jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d08a7-146">If your account is not a member of the subscription administrators role and co-administrator of the subscription, the following warning is displayed on the **Add Automation Account** blade:</span></span>
   >
   >![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="d08a7-148">Az **Automation-fiók hozzáadása** panel **Név** mezőjében adjon meg egy nevet az új Automation-fióknak.</span><span class="sxs-lookup"><span data-stu-id="d08a7-148">On the **Add Automation Account** blade, in the **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="d08a7-149">Ha több előfizetéssel rendelkezik, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-149">If you have more than one subscription, do the following:</span></span>

    <span data-ttu-id="d08a7-150">a.</span><span class="sxs-lookup"><span data-stu-id="d08a7-150">a.</span></span> <span data-ttu-id="d08a7-151">Az **Előfizetés** területen adjon meg egy előfizetést az új fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="d08a7-151">Under **Subscription**, specify one for the new account.</span></span>

    <span data-ttu-id="d08a7-152">b.</span><span class="sxs-lookup"><span data-stu-id="d08a7-152">b.</span></span> <span data-ttu-id="d08a7-153">Az **Erőforráscsoport** területen kattintson az **Új létrehozása** vagy a **Meglévő használata** elemre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="d08a7-154">c.</span><span class="sxs-lookup"><span data-stu-id="d08a7-154">c.</span></span> <span data-ttu-id="d08a7-155">A **Hely** területen adjon meg egy Azure-adatközpontot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="d08a7-156">Az **Azure-beli futtató fiók létrehozása** területen válassza az **Igen** lehetőséget, majd kattintson a **Létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="d08a7-157">Ha a **Nem** beállítás kiválasztásával úgy dönt, hogy nem hozza létre a futtató fiókot, egy figyelmeztető üzenet jelenik meg az **Automation-fiók hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="d08a7-157">If you choose not to create the Run As account by selecting **No**, a warning message is displayed the **Add Automation Account** blade.</span></span> <span data-ttu-id="d08a7-158">A fiók az Azure Portalon jön létre, azonban nem kap megfelelő hitelesítési identitást a klasszikus vagy Resource Manager-alapú előfizetés címtárszolgáltatásában.</span><span class="sxs-lookup"><span data-stu-id="d08a7-158">Although the account is created in the Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="d08a7-159">Következésképpen a fiók az előfizetéshez tartozó erőforrásokhoz sem fér hozzá.</span><span class="sxs-lookup"><span data-stu-id="d08a7-159">Consequently, the account has no access to resources in your subscription.</span></span> <span data-ttu-id="d08a7-160">Ez megakadályozza, hogy az erre a fiókra hivatkozó runbookok hitelesítést végezhessenek, illetve feladatokat futtathassanak az ezekben az üzemi modellekben található erőforrások alapján.</span><span class="sxs-lookup"><span data-stu-id="d08a7-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Figyelmeztető üzenet az „Automation-fiók hozzáadása” panelen](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="d08a7-162">Továbbá, mivel nem hozott létre szolgáltatásnevet, a rendszer nem osztja ki a Közreműködő szerepkört.</span><span class="sxs-lookup"><span data-stu-id="d08a7-162">Additionally, because the service principal is not created, the Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="d08a7-163">Amíg az Azure létrehozza az Automation-fiókot, a menü **Értesítések** részén nyomon követheti a folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-163">While Azure creates the Automation account, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="resources"></a><span data-ttu-id="d08a7-164">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="d08a7-164">Resources</span></span>
<span data-ttu-id="d08a7-165">Ha befejeződött az Automation-fiók létrehozása, számos erőforrás automatikusan létrejön.</span><span class="sxs-lookup"><span data-stu-id="d08a7-165">When the Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="d08a7-166">Az erőforrásokat az alábbi két táblázat foglalja össze:</span><span class="sxs-lookup"><span data-stu-id="d08a7-166">The resources are summarized in the following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="d08a7-167">Futtató fiók erőforrásai</span><span class="sxs-lookup"><span data-stu-id="d08a7-167">Run As account resources</span></span>

| <span data-ttu-id="d08a7-168">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="d08a7-168">Resource</span></span> | <span data-ttu-id="d08a7-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="d08a7-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d08a7-170">AzureAutomationTutorial forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d08a7-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="d08a7-171">Grafikus mintarunbook, amely bemutatja, hogyan kell a futtató fiókkal hitelesítést végezni, és megjeleníti az összes Resource Manager-alapú erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d08a7-171">An example graphical runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="d08a7-172">AzureAutomationTutorialScript forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d08a7-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="d08a7-173">PowerShell-mintarunbook, amely bemutatja, hogyan kell a futtató fiókkal hitelesítést végezni, és megjeleníti az összes Resource Manager-alapú erőforrást.</span><span class="sxs-lookup"><span data-stu-id="d08a7-173">An example PowerShell runbook that demonstrates how to authenticate by using the Run As account and gets all the Resource Manager resources.</span></span> |
| <span data-ttu-id="d08a7-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="d08a7-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="d08a7-175">Tanúsítványobjektum, amely automatikusan létrejön, amikor Automation-fiókot hoz létre, vagy futtatja egy meglévő fiókon az alábbi PowerShell-szkriptet.</span><span class="sxs-lookup"><span data-stu-id="d08a7-175">The certificate asset that's automatically created when you create an Automation account or use the following PowerShell script for an existing account.</span></span> <span data-ttu-id="d08a7-176">A tanúsítvány lehetővé teszi az Azure-hitelesítést, így a runbookokból is kezelheti az Azure Resource Manager-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d08a7-176">The certificate allows you to authenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="d08a7-177">A tanúsítvány élettartama egy év.</span><span class="sxs-lookup"><span data-stu-id="d08a7-177">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="d08a7-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="d08a7-178">AzureRunAsConnection</span></span> | <span data-ttu-id="d08a7-179">Kapcsolatobjektum, amely automatikusan létrejön, amikor Automation-fiókot hoz létre, vagy futtatja egy meglévő fiókon a PowerShell-szkriptet.</span><span class="sxs-lookup"><span data-stu-id="d08a7-179">The connection asset that's automatically created when you create an Automation account or use the PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="d08a7-180">Klasszikus futtató fiók erőforrásai</span><span class="sxs-lookup"><span data-stu-id="d08a7-180">Classic Run As account resources</span></span>

| <span data-ttu-id="d08a7-181">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="d08a7-181">Resource</span></span> | <span data-ttu-id="d08a7-182">Leírás</span><span class="sxs-lookup"><span data-stu-id="d08a7-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d08a7-183">AzureClassicAutomationTutorial forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d08a7-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="d08a7-184">Grafikus mintarunbook, amely a klasszikus futtató fiók (tanúsítvány) segítségével lekéri az előfizetéshez tartozó, a klasszikus üzemi modellel létrehozott virtuális gépeket, majd megjeleníti a virtuális gépek nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-184">An example graphical runbook that gets all the VMs that are created using the classic deployment model in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="d08a7-185">AzureClassicAutomationTutorial parancsprogram-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="d08a7-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="d08a7-186">PowerShell-mintarunbook, amely a klasszikus futtató fiók (tanúsítvány) segítségével lekéri az előfizetéshez tartozó klasszikus virtuális gépeket, majd megjeleníti a virtuális gépek nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-186">An example PowerShell runbook that gets all the classic VMs in a subscription by using the Classic Run As account (certificate), and then writes the VM name and status.</span></span> |
| <span data-ttu-id="d08a7-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="d08a7-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="d08a7-188">Automatikusan létrejövő tanúsítványobjektum, amely Azure-hitelesítésre használható, így a klasszikus Azure-erőforrások is kezelhetők a runbookokból.</span><span class="sxs-lookup"><span data-stu-id="d08a7-188">The automatically created certificate asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="d08a7-189">A tanúsítvány élettartama egy év.</span><span class="sxs-lookup"><span data-stu-id="d08a7-189">The certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="d08a7-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="d08a7-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="d08a7-191">Automatikusan létrejövő kapcsolatobjektum, amely Azure-hitelesítésre használható, így a klasszikus Azure-erőforrások is kezelhetők a runbookokból.</span><span class="sxs-lookup"><span data-stu-id="d08a7-191">The automatically created connection asset that you use to authenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="d08a7-192">A futtató fiókos hitelesítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d08a7-192">Verify Run As authentication</span></span>
<span data-ttu-id="d08a7-193">Hajtson végre egy rövid tesztet, amellyel megállapítja, hogy képes-e sikeres hitelesítést végezni az új futtató fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-193">Perform a small test to confirm that you can successfully authenticate by using the new Run As account.</span></span>

1. <span data-ttu-id="d08a7-194">Az Azure Portalon nyissa meg a korábban létrehozott Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-194">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="d08a7-195">Kattintson a **Runbookok** csempére a forgatókönyvek listájának megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d08a7-195">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="d08a7-196">Válassza ki az **AzureAutomationTutorialScript** runbookot, majd a runbook elindításához kattintson az **Indítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d08a7-196">Select the **AzureAutomationTutorialScript** runbook, and then click **Start** to start the runbook.</span></span> <span data-ttu-id="d08a7-197">Az alábbi események történnek:</span><span class="sxs-lookup"><span data-stu-id="d08a7-197">The following events occur:</span></span>
 * <span data-ttu-id="d08a7-198">Létrejön egy [runbookfeladat](automation-runbook-execution.md), megjelenik a **Feladat** panel, a feladat állapota pedig megjelenik a **Feladat összegzése** csempén.</span><span class="sxs-lookup"><span data-stu-id="d08a7-198">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="d08a7-199">A feladat állapota kezdetben **Várólistán**, ami jelzi, hogy egy felhőbeli runbookfeldolgozó elérhetővé válására vár.</span><span class="sxs-lookup"><span data-stu-id="d08a7-199">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="d08a7-200">Az állapot akkor változik **Indítás** értékre, ha egy feldolgozó elvállalja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-200">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="d08a7-201">Amikor a runbook elkezd futni, az állapota **Fut** értékre változik.</span><span class="sxs-lookup"><span data-stu-id="d08a7-201">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="d08a7-202">Ha befejeződik a runbookfeladat, a **Befejezve** állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d08a7-202">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="d08a7-203">Ha szeretné megtekinteni a runbook részletes eredményeit, kattintson a **Kimenet** csempére.</span><span class="sxs-lookup"><span data-stu-id="d08a7-203">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="d08a7-204">Az ekkor megjelenő **Kimenet** panelen látható, hogy a runbook sikeresen elvégezte a hitelesítést, és visszaadta az erőforráscsoportban elérhető összes erőforrás listáját.</span><span class="sxs-lookup"><span data-stu-id="d08a7-204">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all resources available in the resource group.</span></span>

5. <span data-ttu-id="d08a7-205">Zárja be a **Kimenet** panelt, és lépjen vissza a **Feladat összegzése** panelre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-205">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="d08a7-206">Zárja be a **Feladat összegzése** panelt és a megfelelő **AzureAutomationTutorialScript** runbook paneljét.</span><span class="sxs-lookup"><span data-stu-id="d08a7-206">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="d08a7-207">A klasszikus futtató fiókos hitelesítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="d08a7-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="d08a7-208">Hajtson végre egy hasonló rövid tesztet annak ellenőrzéséhez, hogy képes-e sikeres hitelesítést végezni az új klasszikus futtató fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-208">Perform a similar small test to confirm that you can successfully authenticate by using the new Classic Run As account.</span></span>

1. <span data-ttu-id="d08a7-209">Az Azure Portalon nyissa meg a korábban létrehozott Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-209">In the Azure portal, open the Automation account that you created earlier.</span></span>

2. <span data-ttu-id="d08a7-210">Kattintson a **Runbookok** csempére a forgatókönyvek listájának megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="d08a7-210">Click the **Runbooks** tile to open the list of runbooks.</span></span>

3. <span data-ttu-id="d08a7-211">Válassza ki az **AzureClassicAutomationTutorialScript** runbookot, majd a runbook elindításához kattintson az **Indítás** gombra.</span><span class="sxs-lookup"><span data-stu-id="d08a7-211">Select the **AzureClassicAutomationTutorialScript** runbook, and then click **Start** to  start the runbook.</span></span> <span data-ttu-id="d08a7-212">Az alábbi események történnek:</span><span class="sxs-lookup"><span data-stu-id="d08a7-212">The following events occur:</span></span>

 * <span data-ttu-id="d08a7-213">Létrejön egy [runbookfeladat](automation-runbook-execution.md), megjelenik a **Feladat** panel, a feladat állapota pedig megjelenik a **Feladat összegzése** csempén.</span><span class="sxs-lookup"><span data-stu-id="d08a7-213">A [runbook job](automation-runbook-execution.md) is created, the **Job** blade is displayed, and the job status is displayed in the **Job Summary** tile.</span></span>
 * <span data-ttu-id="d08a7-214">A feladat állapota kezdetben **Várólistán**, ami jelzi, hogy egy felhőbeli runbookfeldolgozó elérhetővé válására vár.</span><span class="sxs-lookup"><span data-stu-id="d08a7-214">The job status begins as **Queued**, indicating that it is waiting for a runbook worker in the cloud to become available.</span></span>
 * <span data-ttu-id="d08a7-215">Az állapot akkor változik **Indítás** értékre, ha egy feldolgozó elvállalja a feladatot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-215">The status becomes **Starting** when a worker claims the job.</span></span>
 * <span data-ttu-id="d08a7-216">Amikor a runbook elkezd futni, az állapota **Fut** értékre változik.</span><span class="sxs-lookup"><span data-stu-id="d08a7-216">The status becomes **Running** when the runbook starts running.</span></span>
 * <span data-ttu-id="d08a7-217">Ha befejeződik a runbookfeladat, a **Befejezve** állapot jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d08a7-217">When the runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Rendszerbiztonsági tag forgatókönyvtesztje](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="d08a7-219">Ha szeretné megtekinteni a runbook részletes eredményeit, kattintson a **Kimenet** csempére.</span><span class="sxs-lookup"><span data-stu-id="d08a7-219">To see the detailed results of the runbook, click the **Output** tile.</span></span>  
<span data-ttu-id="d08a7-220">Az ekkor megjelenő **Kimenet** panelen látható, hogy a runbook sikeresen elvégezte a hitelesítést, és visszaadta az előfizetésben elérhető összes klasszikus virtuális gép listáját.</span><span class="sxs-lookup"><span data-stu-id="d08a7-220">The **Output** blade is displayed, showing that the runbook has successfully authenticated and returned a list of all classic VMs in the subscription.</span></span>

5. <span data-ttu-id="d08a7-221">Zárja be a **Kimenet** panelt, és lépjen vissza a **Feladat összegzése** panelre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-221">Close the **Output** blade to return to the **Job Summary** blade.</span></span>

6. <span data-ttu-id="d08a7-222">Zárja be a **Feladat összegzése** panelt és a megfelelő **AzureAutomationTutorialScript** runbook paneljét.</span><span class="sxs-lookup"><span data-stu-id="d08a7-222">Close the **Job Summary** blade and the corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="d08a7-223">Futtató fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="d08a7-223">Managing your Run As account</span></span>
<span data-ttu-id="d08a7-224">Az Automation-fiók lejárata előtt meg kell újítania a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d08a7-224">At some point before your Automation account expires, you will need to renew the certificate.</span></span> <span data-ttu-id="d08a7-225">Ha úgy véli, hogy a futtató fiók biztonsága sérült, akkor törölheti, majd újra létrehozhatja a fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-225">If you believe that the Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="d08a7-226">Ebben a részben ezeknek a műveleteknek a végrehajtását ismertetjük.</span><span class="sxs-lookup"><span data-stu-id="d08a7-226">This section discusses how to perform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="d08a7-227">Önaláírt tanúsítvány megújítása</span><span class="sxs-lookup"><span data-stu-id="d08a7-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="d08a7-228">A futtató fiókhoz létrehozott önaláírt tanúsítvány a létrehozás dátumától számítva egy év múlva jár le.</span><span class="sxs-lookup"><span data-stu-id="d08a7-228">The self-signed certificate that you created for the Run As account expires one year from the date of creation.</span></span> <span data-ttu-id="d08a7-229">A tanúsítványt bármikor meg lehet újítani a lejárata előtt.</span><span class="sxs-lookup"><span data-stu-id="d08a7-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="d08a7-230">A megújításkor a jelenleg érvényes tanúsítványt megőrzi a rendszer, hogy a művelet ne érintse hátrányosan a várólistán lévő vagy aktívan futó runbookokat, amelyek hitelesítést végeznek a futtató fiókkal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-230">When you renew it, the current valid certificate is retained to ensure that any runbooks that are queued up or actively running, and that authenticate with the Run As account, are not negatively affected.</span></span> <span data-ttu-id="d08a7-231">A tanúsítvány a lejárati dátumáig érvényes marad.</span><span class="sxs-lookup"><span data-stu-id="d08a7-231">The certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="d08a7-232">Ha úgy konfigurálta az Automation futtató fiókot, hogy a vállalati hitelesítésszolgáltató által kibocsátott tanúsítványt használja, és ezt a lehetőséget alkalmazza, a vállalati tanúsítvány egy önaláírt tanúsítványra lesz lecserélve.</span><span class="sxs-lookup"><span data-stu-id="d08a7-232">If you have configured your Automation Run As account to use a certificate issued by your enterprise certificate authority and you use this option, the enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="d08a7-233">A tanúsítvány megújításához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-233">To renew the certificate, do the following:</span></span>

1. <span data-ttu-id="d08a7-234">Az Azure Portalon nyissa meg az Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-234">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="d08a7-235">Az **Automation-fiók** panelen, a **Fióktulajdonságok** ablaktábla **Fiókbeállítások** területén válassza a **Futtató fiókok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d08a7-235">On the **Automation Account** blade, in the **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Az Automation-fiók tulajdonságpanelje](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="d08a7-237">A **Futtató fiókok** tulajdonságpaneljén válassza ki azt a futtató fiókot vagy klasszikus futtató fiókot, amelynek a tanúsítványát meg kívánja újítani.</span><span class="sxs-lookup"><span data-stu-id="d08a7-237">On the **Run As Accounts** properties blade, select either the Run As account or the Classic Run As account that you want to renew the certificate for.</span></span>

4. <span data-ttu-id="d08a7-238">A kiválasztott fiók **Tulajdonságok** paneljén kattintson a **Tanúsítvány megújítása** elemre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-238">On the **Properties** blade for the selected account, click **Renew certificate**.</span></span>

    ![Futtató fiók tanúsítványának megújítása](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="d08a7-240">A tanúsítvány megújítása során a menü **Értesítések** részén nyomon követheti a folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-240">While the certificate is being renewed, you can track the progress under **Notifications** from the menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="d08a7-241">Futtató fiók vagy klasszikus futtató fiók törlése</span><span class="sxs-lookup"><span data-stu-id="d08a7-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="d08a7-242">Ez a témakör ismerteti, hogyan törölhet és hozhat újra létre futtató fiókot vagy klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-242">This section describes how to delete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="d08a7-243">A művelet során a rendszer megőrzi az Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-243">When you perform this action, the Automation account is retained.</span></span> <span data-ttu-id="d08a7-244">A törölt futtató fiókot vagy klasszikus futtató fiókot újra létrehozhatja az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="d08a7-244">After you delete a Run As or Classic Run As account, you can re-create it in the Azure portal.</span></span>

1. <span data-ttu-id="d08a7-245">Az Azure Portalon nyissa meg az Automation-fiókot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-245">In the Azure portal, open the Automation account.</span></span>

2. <span data-ttu-id="d08a7-246">Az **Automation-fiók** panelen, a fiók tulajdonságait tartalmazó ablaktáblán válassza a **Futtató fiókok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d08a7-246">On the **Automation account** blade, in the account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="d08a7-247">A **Futtató fiókok** tulajdonságpaneljén válassza ki azt a futtató fiókot vagy klasszikus futtató fiókot, amelyet törölni kíván.</span><span class="sxs-lookup"><span data-stu-id="d08a7-247">On the **Run As Accounts** properties blade, select either the Run As account or Classic Run As account that you want to delete.</span></span> <span data-ttu-id="d08a7-248">Ezután a kiválasztott fiók **Tulajdonságok** paneljén kattintson a **Törlés** elemre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-248">Then, on the **Properties** blade for the selected account, click **Delete**.</span></span>

 ![Futtató fiók törlése](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="d08a7-250">A fiók törlése során a menü **Értesítések** részén nyomon követheti a folyamat állapotát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-250">While the account is being deleted, you can track the progress under **Notifications** from the menu.</span></span>

5. <span data-ttu-id="d08a7-251">A törlés után újra létrehozhatja a fiókot a **Futtató fiókok** tulajdonságpaneljén az **Azure-alapú futtató fiók** lehetőség kiválasztásával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-251">After the account has been deleted, you can re-create it on the **Run As Accounts** properties blade by selecting the create option **Azure Run As Account**.</span></span>

 ![Automation futtató fiók újbóli létrehozása](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="d08a7-253">Hibás konfiguráció</span><span class="sxs-lookup"><span data-stu-id="d08a7-253">Misconfiguration</span></span>
<span data-ttu-id="d08a7-254">Előfordulhat, hogy a futtató fiók vagy klasszikus futtató fiók megfelelő működéséhez szükséges valamelyik konfigurációs elem törlődött, illetve nem megfelelően hozták létre az első beállításkor.</span><span class="sxs-lookup"><span data-stu-id="d08a7-254">Some configuration items necessary for the Run As or Classic Run As account to function properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="d08a7-255">Ilyen elemek például a következők:</span><span class="sxs-lookup"><span data-stu-id="d08a7-255">The items include:</span></span>

* <span data-ttu-id="d08a7-256">Tanúsítványobjektum</span><span class="sxs-lookup"><span data-stu-id="d08a7-256">Certificate asset</span></span>
* <span data-ttu-id="d08a7-257">Kapcsolatobjektum</span><span class="sxs-lookup"><span data-stu-id="d08a7-257">Connection asset</span></span>
* <span data-ttu-id="d08a7-258">A futtató fiók el lett távolítva a közreműködői szerepkörből</span><span class="sxs-lookup"><span data-stu-id="d08a7-258">Run As account has been removed from the contributor role</span></span>
* <span data-ttu-id="d08a7-259">Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d08a7-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="d08a7-260">Az előző és más hibás konfigurációk esetében az Automation-fiók észleli a módosításokat, és *Hiányos* állapotot jelez a fiók **Futtató fiókok** tulajdonságpaneljén.</span><span class="sxs-lookup"><span data-stu-id="d08a7-260">In the preceding and other instances of misconfiguration, the Automation account detects the changes and displays a status of *Incomplete* on the **Run As Accounts** properties blade for the account.</span></span>

![Hiányos futtatófiók-konfigurációs állapot](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="d08a7-262">Ha kiválasztja a futtató fiókot, a fiók **Tulajdonságok** paneljén a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="d08a7-262">When you select the Run As account, the account **Properties** pane displays the following error message:</span></span>

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="d08a7-264">.</span><span class="sxs-lookup"><span data-stu-id="d08a7-264">.</span></span>

<span data-ttu-id="d08a7-265">A futtató fiókkal kapcsolatos hasonló problémákat gyorsan elháríthatja a fiók törlésével és ismételt létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-265">You can quickly resolve these Run As account issues by deleting and re-creating the account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="d08a7-266">Az Automation-fiók frissítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="d08a7-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="d08a7-267">A PowerShell segítségével frissítheti meglévő Automation-fiókját. Erre a következő esetekben lehet szükség:</span><span class="sxs-lookup"><span data-stu-id="d08a7-267">You can use PowerShell to update your existing Automation account if:</span></span>

* <span data-ttu-id="d08a7-268">Létrehozott egy Automation-fiókot, de futtató fiókot nem.</span><span class="sxs-lookup"><span data-stu-id="d08a7-268">You create an Automation account but decline to create the Run As account.</span></span>
* <span data-ttu-id="d08a7-269">Már használ egy, a Resource Manager-erőforrások felügyeletére szolgáló Automation-fiókot, de szeretné frissíteni, hogy runbookos hitelesítést lehetővé tevő futtató fiókot is tartalmazzon.</span><span class="sxs-lookup"><span data-stu-id="d08a7-269">You already use an Automation account to manage Resource Manager resources and you want to update the account to include the Run As account for runbook authentication.</span></span>
* <span data-ttu-id="d08a7-270">Már használ egy, a klasszikus erőforrások felügyeletére szolgáló Automation-fiókot, de szeretné azt frissíteni, és klasszikus futtató fiókként használni, hogy megtakarítsa az új fiók létrehozásához és a runbookok és objektumok áthelyezéséhez szükséges időt.</span><span class="sxs-lookup"><span data-stu-id="d08a7-270">You already use an Automation account to manage classic resources and you want to update it to use the Classic Run As account instead of creating a new account and migrating your runbooks and assets to it.</span></span>   
* <span data-ttu-id="d08a7-271">Egy futtató és egy klasszikus futtató fiókot szeretne létrehozni a vállalati hitelesítésszolgáltató által kibocsátott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-271">You want to create a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="d08a7-272">A szkriptre a következő előfeltételek vonatkoznak:</span><span class="sxs-lookup"><span data-stu-id="d08a7-272">The script has the following prerequisites:</span></span>

* <span data-ttu-id="d08a7-273">Ez a szkript kizárólag Windows 10 és Azure Resource Manager 2.01-es vagy újabb modulokkal rendelkező Windows Server 2016 rendszeren futtatható.</span><span class="sxs-lookup"><span data-stu-id="d08a7-273">The script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="d08a7-274">A korábbi Windows-verziók esetében nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="d08a7-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="d08a7-275">Az Azure PowerShell 1.0-s és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="d08a7-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="d08a7-276">Információk a PowerShell 1.0-s kiadásáról: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="d08a7-276">For information about the PowerShell 1.0 release, see [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="d08a7-277">Olyan Automation-fiók, amelyre az alábbi PowerShell-szkript az *–AutomationAccountName* és az *-ApplicationDisplayName* paraméterek értékeként hivatkozik.</span><span class="sxs-lookup"><span data-stu-id="d08a7-277">An Automation account, which is referenced as the value for the *–AutomationAccountName* and *-ApplicationDisplayName* parameters in the following PowerShell script.</span></span>

<span data-ttu-id="d08a7-278">A szkript végrehajtásához feltétlenül szükséges *SubscriptionID*, *ResourceGroup* és *AutomationAccountName* paraméterek értékének lekéréséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-278">To get the values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for the scripts, do the following:</span></span>
1. <span data-ttu-id="d08a7-279">Az Azure Portalon válassza ki Automation-fiókját az **Automation-fiók** panelen, majd válassza a **Minden beállítás** elemet.</span><span class="sxs-lookup"><span data-stu-id="d08a7-279">In the Azure portal, select your Automation account on the **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="d08a7-280">A **Minden beállítás** panel **Fiókbeállítások** részénél válassza a **Tulajdonságok** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="d08a7-280">On the **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="d08a7-281">Jegyezze fel a **Tulajdonságok** panelen megjelenő értékeket.</span><span class="sxs-lookup"><span data-stu-id="d08a7-281">Note the values on the **Properties** blade.</span></span>

![Az Automation-fiók „Tulajdonságok” panelje](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="d08a7-283">Futtató fiókhoz használható PowerShell-szkript létrehozása</span><span class="sxs-lookup"><span data-stu-id="d08a7-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="d08a7-284">Ez a PowerShell-szkript a következő konfigurációk támogatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="d08a7-284">This PowerShell script includes support for the following configurations:</span></span>

* <span data-ttu-id="d08a7-285">Futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="d08a7-286">Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="d08a7-287">Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="d08a7-288">Futtató fiók és klasszikus futtató fiók létrehozása az Azure Government Cloud egyik önaláírt tanúsítványának használatával.</span><span class="sxs-lookup"><span data-stu-id="d08a7-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud.</span></span>

<span data-ttu-id="d08a7-289">A szkript az alábbi elemeket hozza létre a kiválasztott konfigurációs beállításoktól függően.</span><span class="sxs-lookup"><span data-stu-id="d08a7-289">Depending on the configuration option you select, the script creates the following items.</span></span>

<span data-ttu-id="d08a7-290">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="d08a7-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="d08a7-291">Létrehoz egy Azure AD-alkalmazást, amelynek az exportálása az önaláírt vagy vállalati tanúsítvány nyilvános kulcsával történik, továbbá létrehoz egy egyszerű szolgáltatásfiókot az alkalmazáshoz az Azure AD-ben, és hozzárendeli a Közreműködő szerepkört ehhez a fiókhoz a jelenlegi előfizetésében.</span><span class="sxs-lookup"><span data-stu-id="d08a7-291">Creates an Azure AD application to be exported with either the self-signed or enterprise certificate public key, creates a service principal account for the application in Azure AD, and assigns the Contributor role for the account in your current subscription.</span></span> <span data-ttu-id="d08a7-292">Ezt a beállítást bármikor módosíthatja Tulajdonos értékre vagy bármely egyéb szerepkörre.</span><span class="sxs-lookup"><span data-stu-id="d08a7-292">You can change this setting to Owner or any other role.</span></span> <span data-ttu-id="d08a7-293">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="d08a7-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="d08a7-294">Létrehoz egy *AzureRunAsCertificate* nevű Automation-tanúsítványobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="d08a7-295">Ez a tanúsítványobjektum tartalmazza az Azure AD-alkalmazás által használt titkos tanúsítványkulcsot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-295">The certificate asset holds the certificate private key that's used by the Azure AD application.</span></span>
* <span data-ttu-id="d08a7-296">Létrehoz egy *AzureRunAsConnection* nevű Automation-kapcsolatobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-296">Creates an Automation connection asset named *AzureRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="d08a7-297">Ez a kapcsolatobjektum magában foglalja az alkalmazásazonosítót, a bérlőazonosítót, az előfizetés-azonosítót és a tanúsítvány ujjlenyomatát.</span><span class="sxs-lookup"><span data-stu-id="d08a7-297">The connection asset holds the applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="d08a7-298">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="d08a7-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="d08a7-299">Létrehoz egy *AzureClassicRunAsCertificate* nevű Automation-tanúsítványobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in the specified Automation account.</span></span> <span data-ttu-id="d08a7-300">Ez a tanúsítványobjektum tartalmazza a felügyeleti tanúsítvány által használt titkos tanúsítványkulcsot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-300">The certificate asset holds the certificate private key used by the management certificate.</span></span>
* <span data-ttu-id="d08a7-301">Létrehoz egy *AzureClassicRunAsConnection* nevű Automation-kapcsolatobjektumot a megadott Automation-fiókban.</span><span class="sxs-lookup"><span data-stu-id="d08a7-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in the specified Automation account.</span></span> <span data-ttu-id="d08a7-302">Ez a kapcsolatobjektum tartalmazza az előfizetés nevét, a subscriptionId paramétert, valamint a tanúsítványobjektum nevét.</span><span class="sxs-lookup"><span data-stu-id="d08a7-302">The connection asset holds the subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="d08a7-303">Ha klasszikus futtató fiók létrehozását választja, a szkript végrehajtása után töltse fel a nyilvános tanúsítványt (.cer fájlnévkiterjesztéssel) annak az előfizetésnek a felügyeleti tárába, amelyben az Automation-fiókot létrehozták.</span><span class="sxs-lookup"><span data-stu-id="d08a7-303">If you select either option for creating a Classic Run As account, after the script is executed, upload the public certificate (.cer file name extension) to the management store for the subscription that the Automation account was created in.</span></span>
> 

<span data-ttu-id="d08a7-304">A szkript végrehajtásához és a tanúsítvány feltöltéséhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-304">To execute the script and upload the certificate, do the following:</span></span>

1. <span data-ttu-id="d08a7-305">Mentse el a következő parancsprogramot a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="d08a7-305">Save the following script on your computer.</span></span> <span data-ttu-id="d08a7-306">Ebben a példában mentse a következő fájlnéven: *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="d08a7-306">In this example, save it with the filename *New-RunAsAccount.ps1*.</span></span>

        #Requires -RunAsAdministrator
         Param (
        [Parameter(Mandatory=$true)]
        [String] $ResourceGroup,

        [Parameter(Mandatory=$true)]
        [String] $AutomationAccountName,

        [Parameter(Mandatory=$true)]
        [String] $ApplicationDisplayName,

        [Parameter(Mandatory=$true)]
        [String] $SubscriptionId,

        [Parameter(Mandatory=$true)]
        [Boolean] $CreateClassicRunAsAccount,

        [Parameter(Mandatory=$true)]
        [String] $SelfSignedCertPlainPassword,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPathForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [String] $EnterpriseCertPlainPasswordForClassicRunAsAccount,

        [Parameter(Mandatory=$false)]
        [ValidateSet("AzureCloud","AzureUSGovernment")]
        [string]$EnvironmentName="AzureCloud",

        [Parameter(Mandatory=$false)]
        [int] $SelfSignedCertNoOfMonthsUntilExpired = 12
        )

        function CreateSelfSignedCertificate([string] $keyVaultName, [string] $certificateName, [string] $selfSignedCertPlainPassword,
                                      [string] $certPath, [string] $certPathCer, [string] $selfSignedCertNoOfMonthsUntilExpired ) {
        $Cert = New-SelfSignedCertificate -DnsName $certificateName -CertStoreLocation cert:\LocalMachine\My `
           -KeyExportPolicy Exportable -Provider "Microsoft Enhanced RSA and AES Cryptographic Provider" `
           -NotAfter (Get-Date).AddMonths($selfSignedCertNoOfMonthsUntilExpired)

        $CertPassword = ConvertTo-SecureString $selfSignedCertPlainPassword -AsPlainText -Force
        Export-PfxCertificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPath -Password $CertPassword -Force | Write-Verbose
        Export-Certificate -Cert ("Cert:\localmachine\my\" + $Cert.Thumbprint) -FilePath $certPathCer -Type CERT | Write-Verbose
        }

        function CreateServicePrincipal([System.Security.Cryptography.X509Certificates.X509Certificate2] $PfxCert, [string] $applicationDisplayName) {  
        $CurrentDate = Get-Date
        $keyValue = [System.Convert]::ToBase64String($PfxCert.GetRawCertData())
        $KeyId = (New-Guid).Guid

        $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
        $KeyCredential.StartDate = $CurrentDate
        $KeyCredential.EndDate= [DateTime]$PfxCert.GetExpirationDateString()
        $KeyCredential.EndDate = $KeyCredential.EndDate.AddDays(-1)
        $KeyCredential.KeyId = $KeyId
        $KeyCredential.CertValue  = $keyValue

        # Use key credentials and create an Azure AD application
        $Application = New-AzureRmADApplication -DisplayName $ApplicationDisplayName -HomePage ("http://" + $applicationDisplayName) -IdentifierUris ("http://" + $KeyId) -KeyCredentials $KeyCredential
        $ServicePrincipal = New-AzureRMADServicePrincipal -ApplicationId $Application.ApplicationId
        $GetServicePrincipal = Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id

        # Sleep here for a few seconds to allow the service principal application to become active (ordinarily takes a few seconds)
        Sleep -s 15
        $NewRole = New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
        $Retries = 0;
        While ($NewRole -eq $null -and $Retries -le 6)
        {
           Sleep -s 10
           New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $Application.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
           $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $Application.ApplicationId -ErrorAction SilentlyContinue
           $Retries++;
        }
           return $Application.ApplicationId.ToString();
        }

        function CreateAutomationCertificateAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $certifcateAssetName,[string] $certPath, [string] $certPlainPassword, [Boolean] $Exportable) {
        $CertPassword = ConvertTo-SecureString $certPlainPassword -AsPlainText -Force   
        Remove-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $certifcateAssetName -ErrorAction SilentlyContinue
        New-AzureRmAutomationCertificate -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Path $certPath -Name $certifcateAssetName -Password $CertPassword -Exportable:$Exportable  | write-verbose
        }

        function CreateAutomationConnectionAsset ([string] $resourceGroup, [string] $automationAccountName, [string] $connectionAssetName, [string] $connectionTypeName, [System.Collections.Hashtable] $connectionFieldValues ) {
        Remove-AzureRmAutomationConnection -ResourceGroupName $resourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -Force -ErrorAction SilentlyContinue
        New-AzureRmAutomationConnection -ResourceGroupName $ResourceGroup -AutomationAccountName $automationAccountName -Name $connectionAssetName -ConnectionTypeName $connectionTypeName -ConnectionFieldValues $connectionFieldValues
        }

        Import-Module AzureRM.Profile
        Import-Module AzureRM.Resources

        $AzureRMProfileVersion= (Get-Module AzureRM.Profile).Version
        if (!(($AzureRMProfileVersion.Major -ge 2 -and $AzureRMProfileVersion.Minor -ge 1) -or ($AzureRMProfileVersion.Major -gt 2)))
        {
           Write-Error -Message "Please install the latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
           return
        }

        Login-AzureRmAccount -EnvironmentName $EnvironmentName
        $Subscription = Select-AzureRmSubscription -SubscriptionId $SubscriptionId

        # Create a Run As account by using a service principal
        $CertifcateAssetName = "AzureRunAsCertificate"
        $ConnectionAssetName = "AzureRunAsConnection"
        $ConnectionTypeName = "AzureServicePrincipal"

        if ($EnterpriseCertPathForRunAsAccount -and $EnterpriseCertPlainPasswordForRunAsAccount) {
        $PfxCertPathForRunAsAccount = $EnterpriseCertPathForRunAsAccount
        $PfxCertPlainPasswordForRunAsAccount = $EnterpriseCertPlainPasswordForRunAsAccount
        } else {
          $CertificateName = $AutomationAccountName+$CertifcateAssetName
          $PfxCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".pfx")
          $PfxCertPlainPasswordForRunAsAccount = $SelfSignedCertPlainPassword
          $CerCertPathForRunAsAccount = Join-Path $env:TEMP ($CertificateName + ".cer")
          CreateSelfSignedCertificate $KeyVaultName $CertificateName $PfxCertPlainPasswordForRunAsAccount $PfxCertPathForRunAsAccount $CerCertPathForRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create a service principal
        $PfxCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($PfxCertPathForRunAsAccount, $PfxCertPlainPasswordForRunAsAccount)
        $ApplicationId=CreateServicePrincipal $PfxCert $ApplicationDisplayName

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate the ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload the .cer format of #CERT# to the Management store by following the steps below." + [Environment]::NewLine +
                    "Log in to the Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload the .cer format of #CERT#"

             if ($EnterpriseCertPathForClassicRunAsAccount -and $EnterpriseCertPlainPasswordForClassicRunAsAccount ) {
             $PfxCertPathForClassicRunAsAccount = $EnterpriseCertPathForClassicRunAsAccount
             $PfxCertPlainPasswordForClassicRunAsAccount = $EnterpriseCertPlainPasswordForClassicRunAsAccount
             $UploadMessage = $UploadMessage.Replace("#CERT#", $PfxCertPathForClassicRunAsAccount)
        } else {
             $ClassicRunAsAccountCertificateName = $AutomationAccountName+$ClassicRunAsAccountCertifcateAssetName
             $PfxCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".pfx")
             $PfxCertPlainPasswordForClassicRunAsAccount = $SelfSignedCertPlainPassword
             $CerCertPathForClassicRunAsAccount = Join-Path $env:TEMP ($ClassicRunAsAccountCertificateName + ".cer")
             $UploadMessage = $UploadMessage.Replace("#CERT#", $CerCertPathForClassicRunAsAccount)
             CreateSelfSignedCertificate $KeyVaultName $ClassicRunAsAccountCertificateName $PfxCertPlainPasswordForClassicRunAsAccount $PfxCertPathForClassicRunAsAccount $CerCertPathForClassicRunAsAccount $SelfSignedCertNoOfMonthsUntilExpired
        }

        # Create the Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate the ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in the Automation account. This connection uses the service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="d08a7-307">A számítógépén kattintson a **Start** gombra, majd indítsa el a **Windows PowerShellt** emelt szintű felhasználói jogokkal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="d08a7-308">Az emelt szintű PowerShell parancssori felületből lépjen abba a mappába, amely az 1. lépésben létrehozott szkriptet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="d08a7-308">From the elevated PowerShell command-line shell, go to the folder that contains the script you created in step 1.</span></span>

4. <span data-ttu-id="d08a7-309">Futtassa a szkriptet a kívánt konfigurációhoz szükséges paraméterértékekkel.</span><span class="sxs-lookup"><span data-stu-id="d08a7-309">Execute the script by using the parameter values for the configuration you require.</span></span>

    <span data-ttu-id="d08a7-310">**Futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="d08a7-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="d08a7-311">**Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="d08a7-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="d08a7-312">**Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="d08a7-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="d08a7-313">**Futtató fiók és klasszikus futtató fiók létrehozása az Azure Government Cloud egyik önaláírt tanúsítványának használatával**</span><span class="sxs-lookup"><span data-stu-id="d08a7-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in the Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="d08a7-314">A rendszer az Azure-ral történő hitelesítést fogja kérni a szkript futtatása után.</span><span class="sxs-lookup"><span data-stu-id="d08a7-314">After the script has executed, you will be prompted to authenticate with Azure.</span></span> <span data-ttu-id="d08a7-315">Jelentkezzen be egy olyan fiókkal, amely tagja az Előfizetés-adminisztrátorok szerepkörnek, és emellett az előfizetés társadminisztrátorának is számít.</span><span class="sxs-lookup"><span data-stu-id="d08a7-315">Sign in with an account that is a member of the subscription administrators role and co-administrator of the subscription.</span></span>
    >
    >

<span data-ttu-id="d08a7-316">A szkript sikeres futtatása után jegyezze fel a következőket:</span><span class="sxs-lookup"><span data-stu-id="d08a7-316">After the script has executed successfully, note the following:</span></span>
* <span data-ttu-id="d08a7-317">Ha önaláírt nyilvános tanúsítvánnyal (.cer fájl) ellátott klasszikus futtató fiókot hoz létre, a szkript létrehozza és menti a tanúsítványt a számítógépen a PowerShell-munkamenet végrehajtásához használt *%USERPROFILE%\AppData\Local\Temp* felhasználói profilhoz tartozó ideiglenes mappába.</span><span class="sxs-lookup"><span data-stu-id="d08a7-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), the script creates and saves it to the temporary files folder on your computer under the user profile *%USERPROFILE%\AppData\Local\Temp*, which you used to execute the PowerShell session.</span></span>
* <span data-ttu-id="d08a7-318">Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="d08a7-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="d08a7-319">Kövesse az utasításokat, amelyek bemutatják [a felügyeleti API-tanúsítványok klasszikus Azure portálra való feltöltését](../azure-api-management-certs.md), majd ellenőrizze a hitelesítő adatok konfigurációját a Service Management-erőforrásokkal. Ehhez használja a [Service Management-erőforrásokkal](#sample-code-to-authenticate-with-service-management-resources) való hitelesítéshez használt mintakódot.</span><span class="sxs-lookup"><span data-stu-id="d08a7-319">Follow the instructions for [uploading a management API certificate to the Azure classic portal](../azure-api-management-certs.md), and then validate the credential configuration with Service Management resources by using the [sample code to authenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="d08a7-320">Ha *nem* klasszikus futtató fiókot hozott létre, állítsa be a hitelesítést a Resource Manager-erőforrásokkal, valamint ellenőrizze a hitelesítő adatok konfigurációját. Ehhez használja a [Service Management-erőforrásokkal való hitelesítéshez használt mintakódot.](#sample-code-to-authenticate-with-resource-manager-resources)</span><span class="sxs-lookup"><span data-stu-id="d08a7-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate the credential configuration by using the [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-to-authenticate-with-resource-manager-resources"></a><span data-ttu-id="d08a7-321">Mintakód a Resource Manager-erőforrásokkal történő hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="d08a7-321">Sample code to authenticate with Resource Manager resources</span></span>
<span data-ttu-id="d08a7-322">A Resource Manager-erőforrások runbookkal való kezeléséhez használhatja az alábbi, az *AzureAutomationTutorialScript* mintaforgatókönyvből származó frissített mintakódot a futtató fiók használatával történő hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="d08a7-322">You can use the following updated sample code, taken from the *AzureAutomationTutorialScript* example runbook, to authenticate by using the Run As account to manage Resource Manager resources with your runbooks.</span></span>

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get the connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in to Azure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context to a specific subscription"     
       Set-AzureRmContext -SubscriptionId $SubId              
    }
    catch {
        if (!$servicePrincipalConnection)
        {
           $ErrorMessage = "Connection $connectionName not found."
           throw $ErrorMessage
         } else{
            Write-Error -Message $_.Exception
            throw $_.Exception
         }
    }

<span data-ttu-id="d08a7-323">A szkript tartalmaz két további kódsort az előfizetési környezet hivatkozásának támogatásához, így könnyedén tud több előfizetéssel is dolgozni.</span><span class="sxs-lookup"><span data-stu-id="d08a7-323">To help you to easily work between multiple subscriptions, the script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="d08a7-324">Egy *SubscriptionId* nevű változó objektum tartalmazza az előfizetés azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="d08a7-324">A variable asset named *SubscriptionId* contains the ID of the subscription.</span></span> <span data-ttu-id="d08a7-325">Az `Add-AzureRmAccount` parancsmag-utasítás után a [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) parancsmag a *-SubscriptionId* paraméterkészlettel lesz kiadva.</span><span class="sxs-lookup"><span data-stu-id="d08a7-325">After the `Add-AzureRmAccount` cmdlet statement, the [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with the parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="d08a7-326">Ha a változó neve túl általános, módosíthatja, hogy tartalmazzon egy előtagot vagy igazodjon más elnevezési konvenciókhoz, és könnyebbé tegye az azonosítását.</span><span class="sxs-lookup"><span data-stu-id="d08a7-326">If the variable name is too generic, you can revise it to include a prefix or use another naming convention to make it easier to identify.</span></span> <span data-ttu-id="d08a7-327">Alternatív megoldásként a *-SubscriptionId* helyett használhatja a *-SubscriptionName* paraméterkészletet egy megfelelő változóobjektummal.</span><span class="sxs-lookup"><span data-stu-id="d08a7-327">Alternatively, you can use the parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="d08a7-328">A runbookban használt hitelesítési parancsmag (`Add-AzureRmAccount`) a *ServicePrincipalCertificate* paraméterkészletet használja.</span><span class="sxs-lookup"><span data-stu-id="d08a7-328">The cmdlet that you use for authenticating in the runbook, `Add-AzureRmAccount`, uses the *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="d08a7-329">Ez a megoldás a szolgáltatástanúsítvány segítségével, és nem a felhasználói hitelesítő adatokkal végzi el a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="d08a7-329">It authenticates by using the service principal certificate, not the user credentials.</span></span>

## <a name="sample-code-to-authenticate-with-service-management-resources"></a><span data-ttu-id="d08a7-330">Mintakód a Service Manager-erőforrásokkal történő hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="d08a7-330">Sample code to authenticate with Service Management resources</span></span>
<span data-ttu-id="d08a7-331">Ha a klasszikus futtató fiókkal szeretne hitelesítést végezni a klasszikus erőforrások forgatókönyvekkel való felügyeletéhez, használhatja az alábbi, az *AzureClassicAutomationTutorialScript* mintarunbookból származó frissített mintakódot is.</span><span class="sxs-lookup"><span data-stu-id="d08a7-331">You can use the following updated sample code, which is taken from the *AzureClassicAutomationTutorialScript* example runbook, to authenticate by using the Classic Run As account to manage classic resources with your runbooks.</span></span>

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get the connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate to Azure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in the Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting the certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in the Automation account."
    }

    Write-Verbose "Authenticating to Azure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a><span data-ttu-id="d08a7-332">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d08a7-332">Next steps</span></span>
* [<span data-ttu-id="d08a7-333">Alkalmazás- és egyszerű szolgáltatási objektumok Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="d08a7-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="d08a7-334">Szerepköralapú hozzáférés-vezérlés az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="d08a7-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="d08a7-335">Azure Cloud Services – tanúsítványok áttekintése</span><span class="sxs-lookup"><span data-stu-id="d08a7-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
