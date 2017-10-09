---
title: "egy Azure futtató fiók aaaConfigure |} Microsoft Docs"
description: "Ez az oktatóanyag bemutatja, hogyan rendszerbiztonsági hitelesítés az Azure Automationben hello létrehozása, tesztelése és példa használatát."
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
redirect_document_id: True
ms.openlocfilehash: 06675d2f6b9d8e7260ffaead4f2b2f61c2b98d13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-runbooks-with-an-azure-run-as-account"></a><span data-ttu-id="2b9be-104">Runbookok hitelesítése Azure-beli futtató fiókkal</span><span class="sxs-lookup"><span data-stu-id="2b9be-104">Authenticate runbooks with an Azure Run As account</span></span>
<span data-ttu-id="2b9be-105">Ez a cikk bemutatja, hogyan tooconfigure egy Azure Automation fiókként hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2b9be-105">This article shows you how tooconfigure an Azure Automation account in hello Azure portal.</span></span> <span data-ttu-id="2b9be-106">toodo Igen, használja a hello Futtatás mint fiók funkció tooauthenticate runbookok Azure Resource Manager vagy az Azure szolgáltatásfelügyelet erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-106">toodo so, you use hello Run As account feature tooauthenticate runbooks managing resources in either Azure Resource Manager or Azure Service Management.</span></span>

<span data-ttu-id="2b9be-107">Automation-fiók létrehozásakor a hello Azure-portál automatikusan két fiókokat hozhat létre:</span><span class="sxs-lookup"><span data-stu-id="2b9be-107">When you create an Automation account in hello Azure portal, you automatically create two accounts:</span></span>

* <span data-ttu-id="2b9be-108">Egy futtató fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-108">A Run As account.</span></span> <span data-ttu-id="2b9be-109">Ez a fiók létrehoz egy egyszerű szolgáltatást az Azure Active Directory-ban (Azure AD), valamint egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-109">This account creates a service principal in Azure Active Directory (Azure AD) and a certificate.</span></span> <span data-ttu-id="2b9be-110">Azt is rendel hello közreműködői szerepköralapú hozzáférés-vezérlést (RBAC), amely kezeli a Resource Managerhez tartozó erőforrások runbookok használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-110">It also assigns hello Contributor role-based access control (RBAC), which manages Resource Manager resources by using runbooks.</span></span>
* <span data-ttu-id="2b9be-111">Egy klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-111">A Classic Run As account.</span></span> <span data-ttu-id="2b9be-112">Ez a fiók feltölt egy felügyeleti tanúsítványt, amely toomanage szolgáltatásfelügyelet vagy hagyományos erőforrások használt runbookok használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-112">This account uploads a management certificate, which is used toomanage Service Management or classic resources by using runbooks.</span></span>

<span data-ttu-id="2b9be-113">Az automatizálási fiók egyszerűbben hello és a segítségével gyorsan indítása létrehozása és telepítése a runbookok toosupport létrehozása az automation kell.</span><span class="sxs-lookup"><span data-stu-id="2b9be-113">Creating an Automation account simplifies hello process for you and helps you quickly start building and deploying runbooks toosupport your automation needs.</span></span>

<span data-ttu-id="2b9be-114">A futtató és klasszikus futtató fiókok segítségével a következőket teheti meg:</span><span class="sxs-lookup"><span data-stu-id="2b9be-114">With Run As and Classic Run As accounts, you can:</span></span>

* <span data-ttu-id="2b9be-115">Azure egy szabványos utat tooauthenticate adja meg, a runbookokat hello Azure-portálon az erőforrás-kezelő vagy a Service Management erőforrások kezelésekor.</span><span class="sxs-lookup"><span data-stu-id="2b9be-115">Provide a standardized way tooauthenticate with Azure when you manage Resource Manager or Service Management resources from runbooks in hello Azure portal.</span></span>
* <span data-ttu-id="2b9be-116">Globális runbookokat, konfigurálhatja a Azure riasztások hello használata automatizálásához.</span><span class="sxs-lookup"><span data-stu-id="2b9be-116">Automate hello use of global runbooks, which you can configure in Azure Alerts.</span></span>

> [!NOTE]
> <span data-ttu-id="2b9be-117">Hello [Azure riasztási integrációs szolgáltatás](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) az Automation szolgáltatásban, globális runbookok Automation-fiók be van állítva egy futtató fiókot és a klasszikus futtató fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b9be-117">hello [Azure Alert integration feature](../monitoring-and-diagnostics/insights-receive-alert-notifications.md) with Automation global runbooks requires an Automation account that's configured with a Run As account and a Classic Run As account.</span></span> <span data-ttu-id="2b9be-118">Automation-fiók, amely már definiálva van futtató és a klasszikus futtató fiókokat is kiválaszthatja, vagy dönthet úgy toocreate új Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-118">You can select an Automation account that already has defined Run As and Classic Run As accounts, or you can choose toocreate a new Automation account.</span></span>
>  

<span data-ttu-id="2b9be-119">Ez a cikk bemutatja, hogyan toocreate hello Azure-portálon az Automation-fiók egy Automation-fiók frissítéséhez, Azure PowerShell használatával, hello fiók konfiguráció kezelése, és a runbookok tudják hitelesíteni magukat.</span><span class="sxs-lookup"><span data-stu-id="2b9be-119">This article shows how toocreate an Automation account from hello Azure portal, update an Automation account by using Azure PowerShell, manage hello account configuration, and authenticate in your runbooks.</span></span>

<span data-ttu-id="2b9be-120">Automation-fiók létrehozása előtt egy jó ötlet toounderstand, és vegye figyelembe a következőket hello:</span><span class="sxs-lookup"><span data-stu-id="2b9be-120">Before you begin creating an Automation account, it's a good idea toounderstand and consider hello following:</span></span>

* <span data-ttu-id="2b9be-121">Automation-fiók létrehozása nincs hatással lehet, hogy már létrehozott vagy a klasszikus hello, vagy a Resource Manager üzembe helyezési modellben az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-121">Creating an Automation account does not affect Automation accounts you might have already created in either hello classic or Resource Manager deployment model.</span></span>
* <span data-ttu-id="2b9be-122">hello folyamat csak a hello Azure-portálon létrehozott Automation-fiók esetén használható.</span><span class="sxs-lookup"><span data-stu-id="2b9be-122">hello process works only for Automation accounts that you create in hello Azure portal.</span></span> <span data-ttu-id="2b9be-123">Kísérlet egy fiókot a klasszikus Azure portálon hello toocreate nem replikálja hello Futtatás mint fiók konfigurálásával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-123">Attempting toocreate an account from hello Azure classic portal does not replicate hello Run As account configuration.</span></span>
* <span data-ttu-id="2b9be-124">Ha már rendelkezik forgatókönyve és eszköze (például ütemezések vagy változók) hely toomanage hagyományos erőforrások, és azt szeretné, hogy a runbookok tooauthenticate hello új klasszikus futtató fiókhoz, hello alábbi módszerekkel:</span><span class="sxs-lookup"><span data-stu-id="2b9be-124">If you already have runbooks and assets (such as schedules or variables) in place toomanage classic resources, and you want runbooks tooauthenticate with hello new Classic Run As account, do either of hello following:</span></span>

  * <span data-ttu-id="2b9be-125">toocreate klasszikus futtató fiókot, hello "Kezelése a futtató fiók" szakasz hello utasításait kövesse.</span><span class="sxs-lookup"><span data-stu-id="2b9be-125">toocreate a Classic Run As account, follow hello instructions in hello "Managing your Run As account" section.</span></span> 
  * <span data-ttu-id="2b9be-126">tooupdate a meglévő fiók használata hello PowerShell parancsfájl-hello "Az Automation-fiók frissítése a PowerShell használatával" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="2b9be-126">tooupdate your existing account, use hello PowerShell script in hello "Update your Automation account by using PowerShell" section.</span></span>
* <span data-ttu-id="2b9be-127">a meglévő runbookok hello hello szakaszban megadott példakódot toomodify szüksége tooauthenticate hello új futtató fiók és a klasszikus Futtatás mint Automation-fiók használatával, [hitelesítésikód-példák](#authentication-code-examples).</span><span class="sxs-lookup"><span data-stu-id="2b9be-127">tooauthenticate by using hello new Run As account and Classic Run As Automation account, you  need toomodify your existing runbooks with hello example code provided in hello section [Authentication code examples](#authentication-code-examples).</span></span>

    >[!NOTE]
    ><span data-ttu-id="2b9be-128">a Futtatás mint fiók hello erőforrás-kezelő erőforrásoknak hello-alapú szolgáltatás egyszerű hitelesítést szolgál.</span><span class="sxs-lookup"><span data-stu-id="2b9be-128">hello Run As account is for authentication against Resource Manager resources using hello certificate-based service principal.</span></span> <span data-ttu-id="2b9be-129">hello klasszikus Futtatás mint fiók felügyeleti tanúsítvánnyal szolgáltatásfelügyelet erőforrásokon hitelesítéséhez van.</span><span class="sxs-lookup"><span data-stu-id="2b9be-129">hello Classic Run As account is for authenticating against Service Management resources with a management certificate.</span></span>

## <a name="create-an-automation-account-from-hello-azure-portal"></a><span data-ttu-id="2b9be-130">Hello Azure-portálon az Automation-fiók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b9be-130">Create an Automation account from hello Azure portal</span></span>
<span data-ttu-id="2b9be-131">Ebben a szakaszban hoz létre egy Azure Automation-fiók hello Azure-portálon, ami viszont hoz létre egy futtató fiókot és a klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-131">In this section, you create an Azure Automation account from hello Azure portal, which in turn creates both a Run As account and a Classic Run As account.</span></span>

>[!NOTE]
><span data-ttu-id="2b9be-132">Automation-fiók toocreate, a hello szolgáltatás-Rendszergazdák szerepkör tagjának vagy hello előfizetést biztosít hozzáférést toohello előfizetés társadminisztrátoraként kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2b9be-132">toocreate an Automation account, you must be a member of hello Service Admins role or co-administrator of hello subscription that is granting access toohello subscription.</span></span> <span data-ttu-id="2b9be-133">Meg kell adni egy felhasználói toothat előfizetés alapértelmezett Active Directory-példányként.</span><span class="sxs-lookup"><span data-stu-id="2b9be-133">You must also be added as a user toothat subscription's default Active Directory instance.</span></span> <span data-ttu-id="2b9be-134">hello fióknak nem szükséges egy kiemelt szerepkörhöz hozzárendelt toobe.</span><span class="sxs-lookup"><span data-stu-id="2b9be-134">hello account does not need toobe assigned a privileged role.</span></span>
>
><span data-ttu-id="2b9be-135">Ha nem tagja Active Directory-példányban hello előfizetés hello előfizetés társadminisztrátoraként szerepe toohello vannak adása előtt, hogy megkapja tooActive Directory vendégként.</span><span class="sxs-lookup"><span data-stu-id="2b9be-135">If you are not a member of hello subscription’s Active Directory instance before you are added toohello co-administrator role of hello subscription, you will be added tooActive Directory as a guest.</span></span> <span data-ttu-id="2b9be-136">Ebben a példában kapni fog egy "nem rendelkezik engedélyekkel toocreate..."</span><span class="sxs-lookup"><span data-stu-id="2b9be-136">In this instance, you will receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="2b9be-137">Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-137">warning on hello **Add Automation Account** blade.</span></span>
>
><span data-ttu-id="2b9be-138">Felhasználók, akik toohello közös rendszergazdai szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újból hozzáadták toomake hozzáadták azokat a teljes felhasználói az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2b9be-138">Users who were added toohello co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="2b9be-139">tooverify ebben a helyzetben a hello **Azure Active Directory** hello Azure-portálon kiválasztásával ablaktábláján **felhasználók és csoportok**, kiválasztásával **minden felhasználó** és kiválasztása után adott felhasználó hello kiválasztásával **profil**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-139">tooverify this situation from hello **Azure Active Directory** pane in hello Azure portal by selecting **Users and groups**, selecting **All users** and, after you select hello specific user, selecting **Profile**.</span></span> <span data-ttu-id="2b9be-140">hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-140">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>
>

1. <span data-ttu-id="2b9be-141">Jelentkezzen be az Azure portál egy olyan fiókkal, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként toohello.</span><span class="sxs-lookup"><span data-stu-id="2b9be-141">Sign in toohello Azure portal with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>

2. <span data-ttu-id="2b9be-142">Válassza az **Automation-fiókok** elemet.</span><span class="sxs-lookup"><span data-stu-id="2b9be-142">Select **Automation Accounts**.</span></span>

3. <span data-ttu-id="2b9be-143">A hello **Automation-fiók** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-143">On hello **Automation Accounts** blade, click **Add**.</span></span>
<span data-ttu-id="2b9be-144">Hello **Automation-fiók hozzáadása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2b9be-144">hello **Add Automation Account** blade opens.</span></span>

 ![hello "Hozzáadása Automation-fiók" panel](media/automation-sec-configure-azure-runas-account/create-automation-account-properties-b.png)

   > [!NOTE]
   > <span data-ttu-id="2b9be-146">Ha a fiók nem hello előfizetés Rendszergazdák szerepkör tagja és hello előfizetés társadminisztrátoraként, hello a következő figyelmeztetés jelenik meg hello **Automation-fiók hozzáadása** panel:</span><span class="sxs-lookup"><span data-stu-id="2b9be-146">If your account is not a member of hello subscription administrators role and co-administrator of hello subscription, hello following warning is displayed on hello **Add Automation Account** blade:</span></span>
   >
   >![Az Automation-fiókhoz kapcsolódó figyelmeztetés hozzáadása](media/automation-sec-configure-azure-runas-account/create-account-without-perms.png)
   >
   >

4. <span data-ttu-id="2b9be-148">A hello **Automation-fiók hozzáadása** paneljén, hello **neve** mezőbe írja be az új Automation-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="2b9be-148">On hello **Add Automation Account** blade, in hello **Name** box, type a name for your new Automation account.</span></span>

5. <span data-ttu-id="2b9be-149">Ha egynél több előfizetéssel rendelkezik, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="2b9be-149">If you have more than one subscription, do hello following:</span></span>

    <span data-ttu-id="2b9be-150">a.</span><span class="sxs-lookup"><span data-stu-id="2b9be-150">a.</span></span> <span data-ttu-id="2b9be-151">A **előfizetés**, adjon meg egy új fiókot hello.</span><span class="sxs-lookup"><span data-stu-id="2b9be-151">Under **Subscription**, specify one for hello new account.</span></span>

    <span data-ttu-id="2b9be-152">b.</span><span class="sxs-lookup"><span data-stu-id="2b9be-152">b.</span></span> <span data-ttu-id="2b9be-153">Az **Erőforráscsoport** területen kattintson az **Új létrehozása** vagy a **Meglévő használata** elemre.</span><span class="sxs-lookup"><span data-stu-id="2b9be-153">Under **Resource Group**, click **Create new** or **Use existing**.</span></span>

    <span data-ttu-id="2b9be-154">c.</span><span class="sxs-lookup"><span data-stu-id="2b9be-154">c.</span></span> <span data-ttu-id="2b9be-155">A **Hely** területen adjon meg egy Azure-adatközpontot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-155">Under **Location**, specify an Azure datacenter.</span></span>

6. <span data-ttu-id="2b9be-156">Az **Azure-beli futtató fiók létrehozása** területen válassza az **Igen** lehetőséget, majd kattintson a **Létrehozás** elemre.</span><span class="sxs-lookup"><span data-stu-id="2b9be-156">Under **Create Azure Run As account**, select **Yes**, and then click **Create**.</span></span>

   > [!NOTE]
   > <span data-ttu-id="2b9be-157">Ha úgy dönt, nem toocreate hello Futtatás mint fiók kiválasztásával **nem**, megjelenik egy figyelmeztető üzenet hello **Automation-fiók hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-157">If you choose not toocreate hello Run As account by selecting **No**, a warning message is displayed hello **Add Automation Account** blade.</span></span> <span data-ttu-id="2b9be-158">Bár a hello-fiók létrehozása az Azure-portálon hello nem rendelkezik a megfelelő hitelesítési identitását a klasszikus vagy erőforrás-kezelő előfizetési címtárszolgáltatás belül.</span><span class="sxs-lookup"><span data-stu-id="2b9be-158">Although hello account is created in hello Azure portal, it does not have a corresponding authentication identity within your classic or Resource Manager subscription directory service.</span></span> <span data-ttu-id="2b9be-159">Következésképpen hello fióknak nincs hozzáférési tooresources az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="2b9be-159">Consequently, hello account has no access tooresources in your subscription.</span></span> <span data-ttu-id="2b9be-160">Ez megakadályozza, hogy az erre a fiókra hivatkozó runbookok hitelesítést végezhessenek, illetve feladatokat futtathassanak az ezekben az üzemi modellekben található erőforrások alapján.</span><span class="sxs-lookup"><span data-stu-id="2b9be-160">This scenario prevents any runbooks that reference this account from authenticating and performing tasks against resources in those deployment models.</span></span>
   >
   > ![Figyelmeztető üzenet hello "Hozzáadása Automation-fiók" panelen](media/automation-sec-configure-azure-runas-account/create-account-decline-create-runas-msg.png)
   >
   > <span data-ttu-id="2b9be-162">Emellett egyszerű hello szolgáltatást nem jön létre, mert hello közreműködői szerepkör nincs hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="2b9be-162">Additionally, because hello service principal is not created, hello Contributor role is not assigned.</span></span>
   >

7. <span data-ttu-id="2b9be-163">Azure Automation-fiók hello hoz létre, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="2b9be-163">While Azure creates hello Automation account, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="resources"></a><span data-ttu-id="2b9be-164">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="2b9be-164">Resources</span></span>
<span data-ttu-id="2b9be-165">Ha hello Automation-fiók sikeresen létrejött, a több erőforrás automatikusan létrejönnek meg.</span><span class="sxs-lookup"><span data-stu-id="2b9be-165">When hello Automation account is successfully created, several resources are automatically created for you.</span></span> <span data-ttu-id="2b9be-166">a következő két tábla hello hello erőforrások foglalja össze:</span><span class="sxs-lookup"><span data-stu-id="2b9be-166">hello resources are summarized in hello following two tables:</span></span>

#### <a name="run-as-account-resources"></a><span data-ttu-id="2b9be-167">Futtató fiók erőforrásai</span><span class="sxs-lookup"><span data-stu-id="2b9be-167">Run As account resources</span></span>

| <span data-ttu-id="2b9be-168">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="2b9be-168">Resource</span></span> | <span data-ttu-id="2b9be-169">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b9be-169">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2b9be-170">AzureAutomationTutorial forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2b9be-170">AzureAutomationTutorial Runbook</span></span> | <span data-ttu-id="2b9be-171">Grafikus példaforgatókönyv, amely bemutatja, hogyan használatával tooauthenticate hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-171">An example graphical runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="2b9be-172">AzureAutomationTutorialScript forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2b9be-172">AzureAutomationTutorialScript Runbook</span></span> | <span data-ttu-id="2b9be-173">PowerShell példaforgatókönyv, amely bemutatja, hogyan használatával tooauthenticate hello Futtatás mint fiókot, és minden hello Resource Managerhez tartozó erőforrások lekérése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-173">An example PowerShell runbook that demonstrates how tooauthenticate by using hello Run As account and gets all hello Resource Manager resources.</span></span> |
| <span data-ttu-id="2b9be-174">AzureRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="2b9be-174">AzureRunAsCertificate</span></span> | <span data-ttu-id="2b9be-175">automatikusan létrejön egy Automation-fiók létrehozásakor, vagy használja a következő PowerShell-parancsfájl egy létező fiók hello hello tanúsítványeszköz.</span><span class="sxs-lookup"><span data-stu-id="2b9be-175">hello certificate asset that's automatically created when you create an Automation account or use hello following PowerShell script for an existing account.</span></span> <span data-ttu-id="2b9be-176">hello tanúsítvány teszi lehetővé az Azure-ral tooauthenticate, hogy a runbookok Azure Resource Manager-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-176">hello certificate allows you tooauthenticate with Azure so that you can manage Azure Resource Manager resources from runbooks.</span></span> <span data-ttu-id="2b9be-177">a tanúsítványnak hello egyéves-élettartamot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-177">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="2b9be-178">AzureRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="2b9be-178">AzureRunAsConnection</span></span> | <span data-ttu-id="2b9be-179">hello kapcsolódási eszköz, amely automatikusan létrejön egy Automation-fiók létrehozásakor vagy hello PowerShell-parancsfájl használata a meglévő fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-179">hello connection asset that's automatically created when you create an Automation account or use hello PowerShell script for an existing account.</span></span> |

#### <a name="classic-run-as-account-resources"></a><span data-ttu-id="2b9be-180">Klasszikus futtató fiók erőforrásai</span><span class="sxs-lookup"><span data-stu-id="2b9be-180">Classic Run As account resources</span></span>

| <span data-ttu-id="2b9be-181">Erőforrás</span><span class="sxs-lookup"><span data-stu-id="2b9be-181">Resource</span></span> | <span data-ttu-id="2b9be-182">Leírás</span><span class="sxs-lookup"><span data-stu-id="2b9be-182">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2b9be-183">AzureClassicAutomationTutorial forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2b9be-183">AzureClassicAutomationTutorial Runbook</span></span> | <span data-ttu-id="2b9be-184">Grafikus példaforgatókönyv, amely lekérdezi a hello gépek hello klasszikus telepítési modell használatával egy előfizetésben hello klasszikus futtató fiók (tanúsítvány) használatával hozhatók létre, és ezután ír hello virtuális gép nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="2b9be-184">An example graphical runbook that gets all hello VMs that are created using hello classic deployment model in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="2b9be-185">AzureClassicAutomationTutorial parancsprogram-forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="2b9be-185">AzureClassicAutomationTutorial Script Runbook</span></span> | <span data-ttu-id="2b9be-186">PowerShell példaforgatókönyv, amely lekérdezi az összes előfizetést a klasszikus virtuális gépeinek hello hello klasszikus futtató fiók (tanúsítvány) használatával, és majd hello az írási műveletek a virtuális gép nevét és állapotát.</span><span class="sxs-lookup"><span data-stu-id="2b9be-186">An example PowerShell runbook that gets all hello classic VMs in a subscription by using hello Classic Run As account (certificate), and then writes hello VM name and status.</span></span> |
| <span data-ttu-id="2b9be-187">AzureClassicRunAsCertificate</span><span class="sxs-lookup"><span data-stu-id="2b9be-187">AzureClassicRunAsCertificate</span></span> | <span data-ttu-id="2b9be-188">automatikusan létrehozott hello tanúsítványeszköz használt tooauthenticate az Azure-ral, hogy a runbookok klasszikus Azure-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-188">hello automatically created certificate asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> <span data-ttu-id="2b9be-189">a tanúsítványnak hello egyéves-élettartamot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-189">hello certificate has a one-year lifespan.</span></span> |
| <span data-ttu-id="2b9be-190">AzureClassicRunAsConnection</span><span class="sxs-lookup"><span data-stu-id="2b9be-190">AzureClassicRunAsConnection</span></span> | <span data-ttu-id="2b9be-191">hello automatikusan létrehozott kapcsolódási eszköz használata tooauthenticate az Azure-ral, hogy a runbookok klasszikus Azure-erőforrások kezelése.</span><span class="sxs-lookup"><span data-stu-id="2b9be-191">hello automatically created connection asset that you use tooauthenticate with Azure so that you can manage Azure classic resources from runbooks.</span></span> |

## <a name="verify-run-as-authentication"></a><span data-ttu-id="2b9be-192">A futtató fiókos hitelesítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2b9be-192">Verify Run As authentication</span></span>
<span data-ttu-id="2b9be-193">Hajtsa végre egy kis teszt tooconfirm, amely képes sikeresen hitelesíteni hello új futtató fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-193">Perform a small test tooconfirm that you can successfully authenticate by using hello new Run As account.</span></span>

1. <span data-ttu-id="2b9be-194">Hello Azure-portálon nyissa meg a korábban létrehozott hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-194">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="2b9be-195">Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.</span><span class="sxs-lookup"><span data-stu-id="2b9be-195">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="2b9be-196">Jelölje be hello **AzureAutomationTutorialScript** runbookot, és kattintson a **Start** toostart hello runbook.</span><span class="sxs-lookup"><span data-stu-id="2b9be-196">Select hello **AzureAutomationTutorialScript** runbook, and then click **Start** toostart hello runbook.</span></span> <span data-ttu-id="2b9be-197">a következő események hello fordulhat elő:</span><span class="sxs-lookup"><span data-stu-id="2b9be-197">hello following events occur:</span></span>
 * <span data-ttu-id="2b9be-198">A [runbook-feladat](automation-runbook-execution.md) jön létre, hello **feladat** panel jelenik meg, és hello feladat állapota látható hello **feladat összegzése** csempére.</span><span class="sxs-lookup"><span data-stu-id="2b9be-198">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="2b9be-199">hello feladatállapot jön létre **várakozik**, azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.</span><span class="sxs-lookup"><span data-stu-id="2b9be-199">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="2b9be-200">hello állapota lesz **indítása** Ha egy munkavégző jogcímek hello feladat.</span><span class="sxs-lookup"><span data-stu-id="2b9be-200">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="2b9be-201">hello állapota lesz **futtató** amikor hello runbook elindul.</span><span class="sxs-lookup"><span data-stu-id="2b9be-201">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="2b9be-202">Amikor hello runbook-feladat futása befejeződött, megjelenik egy állapotának **befejezve**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-202">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

       ![Security Principal Runbook Test](media/automation-sec-configure-azure-runas-account/job-summary-automationtutorialscript.png)
4. <span data-ttu-id="2b9be-203">toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.</span><span class="sxs-lookup"><span data-stu-id="2b9be-203">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="2b9be-204">Hello **kimeneti** panel jelenik meg, melyen hello runbook sikeresen hitelesített és hello erőforráscsoportban elérhető összes erőforrás listáját adja vissza.</span><span class="sxs-lookup"><span data-stu-id="2b9be-204">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all resources available in hello resource group.</span></span>

5. <span data-ttu-id="2b9be-205">Bezárás hello **kimeneti** panel tooreturn toohello **feladat összegzése** panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-205">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="2b9be-206">Bezárás hello **feladat összegzése** panelen, a megfelelő hello **AzureAutomationTutorialScript** runbook panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-206">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="verify-classic-run-as-authentication"></a><span data-ttu-id="2b9be-207">A klasszikus futtató fiókos hitelesítés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="2b9be-207">Verify Classic Run As authentication</span></span>
<span data-ttu-id="2b9be-208">Hajtsa végre a hasonló kis tooconfirm, amely képes sikeresen hitelesíteni hello új klasszikus futtató fiók segítségével tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="2b9be-208">Perform a similar small test tooconfirm that you can successfully authenticate by using hello new Classic Run As account.</span></span>

1. <span data-ttu-id="2b9be-209">Hello Azure-portálon nyissa meg a korábban létrehozott hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-209">In hello Azure portal, open hello Automation account that you created earlier.</span></span>

2. <span data-ttu-id="2b9be-210">Kattintson a hello **Runbookok** csempe tooopen hello forgatókönyvek listája.</span><span class="sxs-lookup"><span data-stu-id="2b9be-210">Click hello **Runbooks** tile tooopen hello list of runbooks.</span></span>

3. <span data-ttu-id="2b9be-211">Jelölje be hello **AzureClassicAutomationTutorialScript** runbookot, és kattintson a **Start** túl a hello runbook indítása.</span><span class="sxs-lookup"><span data-stu-id="2b9be-211">Select hello **AzureClassicAutomationTutorialScript** runbook, and then click **Start** too start hello runbook.</span></span> <span data-ttu-id="2b9be-212">a következő események hello fordulhat elő:</span><span class="sxs-lookup"><span data-stu-id="2b9be-212">hello following events occur:</span></span>

 * <span data-ttu-id="2b9be-213">A [runbook-feladat](automation-runbook-execution.md) jön létre, hello **feladat** panel jelenik meg, és hello feladat állapota látható hello **feladat összegzése** csempére.</span><span class="sxs-lookup"><span data-stu-id="2b9be-213">A [runbook job](automation-runbook-execution.md) is created, hello **Job** blade is displayed, and hello job status is displayed in hello **Job Summary** tile.</span></span>
 * <span data-ttu-id="2b9be-214">hello feladatállapot jön létre **várakozik**, azt jelzi, hogy a forgatókönyv-feldolgozók hello felhő toobecome érhető el a vár.</span><span class="sxs-lookup"><span data-stu-id="2b9be-214">hello job status begins as **Queued**, indicating that it is waiting for a runbook worker in hello cloud toobecome available.</span></span>
 * <span data-ttu-id="2b9be-215">hello állapota lesz **indítása** Ha egy munkavégző jogcímek hello feladat.</span><span class="sxs-lookup"><span data-stu-id="2b9be-215">hello status becomes **Starting** when a worker claims hello job.</span></span>
 * <span data-ttu-id="2b9be-216">hello állapota lesz **futtató** amikor hello runbook elindul.</span><span class="sxs-lookup"><span data-stu-id="2b9be-216">hello status becomes **Running** when hello runbook starts running.</span></span>
 * <span data-ttu-id="2b9be-217">Amikor hello runbook-feladat futása befejeződött, megjelenik egy állapotának **befejezve**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-217">When hello runbook job has finished running, you should see a status of **Completed**.</span></span>

    ![Rendszerbiztonsági tag forgatókönyvtesztje](media/automation-sec-configure-azure-runas-account/job-summary-automationclassictutorialscript.png)<br>
4. <span data-ttu-id="2b9be-219">toosee hello hello runbook részletes eredményét, kattintson a hello **kimeneti** csempére.</span><span class="sxs-lookup"><span data-stu-id="2b9be-219">toosee hello detailed results of hello runbook, click hello **Output** tile.</span></span>  
<span data-ttu-id="2b9be-220">Hello **kimeneti** panel jelenik meg, melyen hello runbook sikeresen hitelesített, és visszahelyezi a klasszikus virtuális gépeinek listáját hello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2b9be-220">hello **Output** blade is displayed, showing that hello runbook has successfully authenticated and returned a list of all classic VMs in hello subscription.</span></span>

5. <span data-ttu-id="2b9be-221">Bezárás hello **kimeneti** panel tooreturn toohello **feladat összegzése** panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-221">Close hello **Output** blade tooreturn toohello **Job Summary** blade.</span></span>

6. <span data-ttu-id="2b9be-222">Bezárás hello **feladat összegzése** panelen, a megfelelő hello **AzureAutomationTutorialScript** runbook panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-222">Close hello **Job Summary** blade and hello corresponding **AzureAutomationTutorialScript** runbook blade.</span></span>

## <a name="managing-your-run-as-account"></a><span data-ttu-id="2b9be-223">Futtató fiók kezelése</span><span class="sxs-lookup"><span data-stu-id="2b9be-223">Managing your Run As account</span></span>
<span data-ttu-id="2b9be-224">Egy bizonyos ponton az Automation-fiók lejárta előtt toorenew hello tanúsítványt kell.</span><span class="sxs-lookup"><span data-stu-id="2b9be-224">At some point before your Automation account expires, you will need toorenew hello certificate.</span></span> <span data-ttu-id="2b9be-225">Ha úgy véli, hogy a Futtatás mint fiók hello feltörték, törlése, és hozza létre újból.</span><span class="sxs-lookup"><span data-stu-id="2b9be-225">If you believe that hello Run As account has been compromised, you can delete and re-create it.</span></span> <span data-ttu-id="2b9be-226">Ez a szakasz ismerteti hogyan tooperform ezeket a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="2b9be-226">This section discusses how tooperform these operations.</span></span>

### <a name="self-signed-certificate-renewal"></a><span data-ttu-id="2b9be-227">Önaláírt tanúsítvány megújítása</span><span class="sxs-lookup"><span data-stu-id="2b9be-227">Self-signed certificate renewal</span></span>
<span data-ttu-id="2b9be-228">hello önaláírt tanúsítványt, amelyet létrehozott hello futtató fiók létrehozásának dátuma hello egy év lejár.</span><span class="sxs-lookup"><span data-stu-id="2b9be-228">hello self-signed certificate that you created for hello Run As account expires one year from hello date of creation.</span></span> <span data-ttu-id="2b9be-229">A tanúsítványt bármikor meg lehet újítani a lejárata előtt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-229">You can renew it at any time before it expires.</span></span> <span data-ttu-id="2b9be-230">Amikor megújítja, a hello aktuális érvényes tanúsítványra, hogy legfeljebb vagy aktívan futó ütemezett, és az, hogy azokkal hello Futtatás mint fiókot, runbook érintett nem negatív megtartott tooensure.</span><span class="sxs-lookup"><span data-stu-id="2b9be-230">When you renew it, hello current valid certificate is retained tooensure that any runbooks that are queued up or actively running, and that authenticate with hello Run As account, are not negatively affected.</span></span> <span data-ttu-id="2b9be-231">amíg a lejárati dátum nem érvényes marad hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-231">hello certificate remains valid until its expiration date.</span></span>

> [!NOTE]
> <span data-ttu-id="2b9be-232">Ha konfigurálta az Automation Futtatás mint fiók toouse a vállalati hitelesítésszolgáltató által kiállított tanúsítványt, és ezt a beállítást használja, hello vállalati tanúsítvány helyébe egy önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-232">If you have configured your Automation Run As account toouse a certificate issued by your enterprise certificate authority and you use this option, hello enterprise certificate will be replaced by a self-signed certificate.</span></span>

<span data-ttu-id="2b9be-233">toorenew hello tanúsítvány, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2b9be-233">toorenew hello certificate, do hello following:</span></span>

1. <span data-ttu-id="2b9be-234">Hello Azure-portálon nyissa meg a hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-234">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="2b9be-235">A hello **Automation-fiók** paneljén, hello **tulajdonságok fiók** ablaktáblán, a **Fiókbeállítások**, jelölje be **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-235">On hello **Automation Account** blade, in hello **Account properties** pane, under **Account Settings**, select **Run As Accounts**.</span></span>

    ![Az Automation-fiók tulajdonságpanelje](media/automation-sec-configure-azure-runas-account/automation-account-properties-pane.png)
3. <span data-ttu-id="2b9be-237">A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy hello klasszikus Futtatás mint fiókot, amelyet toorenew hello tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-237">On hello **Run As Accounts** properties blade, select either hello Run As account or hello Classic Run As account that you want toorenew hello certificate for.</span></span>

4. <span data-ttu-id="2b9be-238">A hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **tanúsítvány megújítása**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-238">On hello **Properties** blade for hello selected account, click **Renew certificate**.</span></span>

    ![Futtató fiók tanúsítványának megújítása](media/automation-sec-configure-azure-runas-account/automation-account-renew-runas-certificate.png)

5. <span data-ttu-id="2b9be-240">Hello tanúsítvány megújítása folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="2b9be-240">While hello certificate is being renewed, you can track hello progress under **Notifications** from hello menu.</span></span>

### <a name="delete-a-run-as-or-classic-run-as-account"></a><span data-ttu-id="2b9be-241">Futtató fiók vagy klasszikus futtató fiók törlése</span><span class="sxs-lookup"><span data-stu-id="2b9be-241">Delete a Run As or Classic Run As account</span></span>
<span data-ttu-id="2b9be-242">Ez a szakasz ismerteti, hogyan toodelete, majd hozza létre a Futtatás mint vagy a klasszikus futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-242">This section describes how toodelete and re-create a Run As or Classic Run As account.</span></span> <span data-ttu-id="2b9be-243">Ez a művelet végrehajtásakor hello Automation-fiók őrződnek meg.</span><span class="sxs-lookup"><span data-stu-id="2b9be-243">When you perform this action, hello Automation account is retained.</span></span> <span data-ttu-id="2b9be-244">A Futtatás mint vagy a klasszikus futtató fiók törlése után újra létrehozhatja a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="2b9be-244">After you delete a Run As or Classic Run As account, you can re-create it in hello Azure portal.</span></span>

1. <span data-ttu-id="2b9be-245">Hello Azure-portálon nyissa meg a hello Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-245">In hello Azure portal, open hello Automation account.</span></span>

2. <span data-ttu-id="2b9be-246">A hello **Automation-fiók** hello fiók tulajdonságok panelen, jelölje be a panelt **futtató fiókok**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-246">On hello **Automation account** blade, in hello account properties pane, select **Run As Accounts**.</span></span>

3. <span data-ttu-id="2b9be-247">A hello **futtató fiókok** tulajdonságok panelére léphet, válassza ki bármelyik hello futtató fiók vagy a klasszikus futtató fiókot, amelyet az toodelete.</span><span class="sxs-lookup"><span data-stu-id="2b9be-247">On hello **Run As Accounts** properties blade, select either hello Run As account or Classic Run As account that you want toodelete.</span></span> <span data-ttu-id="2b9be-248">Ezután a hello **tulajdonságok** hello panelen kiválasztott fiókot, kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-248">Then, on hello **Properties** blade for hello selected account, click **Delete**.</span></span>

 ![Futtató fiók törlése](media/automation-sec-configure-azure-runas-account/automation-account-delete-runas.png)

4. <span data-ttu-id="2b9be-250">Hello fiók törlése folyamatban van, amíg előrehaladásának hello alatt **értesítések** hello menüből.</span><span class="sxs-lookup"><span data-stu-id="2b9be-250">While hello account is being deleted, you can track hello progress under **Notifications** from hello menu.</span></span>

5. <span data-ttu-id="2b9be-251">Hello fiók törlése után újra létrehozhatja a hello **futtató fiókok** hello kiválasztásával a Tulajdonságok panelére léphet létrehozása beállítást **Azure futtató fiók**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-251">After hello account has been deleted, you can re-create it on hello **Run As Accounts** properties blade by selecting hello create option **Azure Run As Account**.</span></span>

 ![Hozza létre újból hello Automation futtató fiók](media/automation-sec-configure-azure-runas-account/automation-account-create-runas.png)

### <a name="misconfiguration"></a><span data-ttu-id="2b9be-253">Hibás konfiguráció</span><span class="sxs-lookup"><span data-stu-id="2b9be-253">Misconfiguration</span></span>
<span data-ttu-id="2b9be-254">Néhány hello futtató vagy a klasszikus futtató fiók toofunction szükséges konfigurációs elemek megfelelő lehet, hogy törölték vagy helytelenül van létrehozva a kezdeti beállítás során.</span><span class="sxs-lookup"><span data-stu-id="2b9be-254">Some configuration items necessary for hello Run As or Classic Run As account toofunction properly might have been deleted or created improperly during initial setup.</span></span> <span data-ttu-id="2b9be-255">hello elemek a következők:</span><span class="sxs-lookup"><span data-stu-id="2b9be-255">hello items include:</span></span>

* <span data-ttu-id="2b9be-256">Tanúsítványobjektum</span><span class="sxs-lookup"><span data-stu-id="2b9be-256">Certificate asset</span></span>
* <span data-ttu-id="2b9be-257">Kapcsolatobjektum</span><span class="sxs-lookup"><span data-stu-id="2b9be-257">Connection asset</span></span>
* <span data-ttu-id="2b9be-258">A Futtatás mint fiók hello közreműködői szerepkör eltávolították.</span><span class="sxs-lookup"><span data-stu-id="2b9be-258">Run As account has been removed from hello contributor role</span></span>
* <span data-ttu-id="2b9be-259">Egyszerű szolgáltatás vagy alkalmazás az Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2b9be-259">Service principal or application in Azure AD</span></span>

<span data-ttu-id="2b9be-260">Az előző hello és helytelen konfigurálása más példányai, hello Automation-fiók észleli hello változik, és egy állapotát jeleníti meg az *hiányos* a hello **futtató fiókok** hello a Tulajdonságok panelen fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-260">In hello preceding and other instances of misconfiguration, hello Automation account detects hello changes and displays a status of *Incomplete* on hello **Run As Accounts** properties blade for hello account.</span></span>

![Hiányos futtatófiók-konfigurációs állapot](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config.png)

<span data-ttu-id="2b9be-262">Hello Futtatás mint fiók kiválasztásakor hello fiók **tulajdonságok** ablaktáblán jelennek meg a következő hibaüzenet hello:</span><span class="sxs-lookup"><span data-stu-id="2b9be-262">When you select hello Run As account, hello account **Properties** pane displays hello following error message:</span></span>

![Hiányos futtatófiók-konfigurációra figyelmeztető üzenet](media/automation-sec-configure-azure-runas-account/automation-account-runas-incomplete-config-msg.png)<span data-ttu-id="2b9be-264">.</span><span class="sxs-lookup"><span data-stu-id="2b9be-264">.</span></span>

<span data-ttu-id="2b9be-265">A Futtatás mint fiók problémák gyorsan megoldható törli, majd újra létrehozza a hello fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-265">You can quickly resolve these Run As account issues by deleting and re-creating hello account.</span></span>

## <a name="update-your-automation-account-by-using-powershell"></a><span data-ttu-id="2b9be-266">Az Automation-fiók frissítése a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="2b9be-266">Update your Automation account by using PowerShell</span></span>
<span data-ttu-id="2b9be-267">Akkor használhatja PowerShell tooupdate a meglévő Automation-fiókot:</span><span class="sxs-lookup"><span data-stu-id="2b9be-267">You can use PowerShell tooupdate your existing Automation account if:</span></span>

* <span data-ttu-id="2b9be-268">Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-268">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="2b9be-269">Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="2b9be-269">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="2b9be-270">Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.</span><span class="sxs-lookup"><span data-stu-id="2b9be-270">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="2b9be-271">Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="2b9be-271">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

<span data-ttu-id="2b9be-272">hello parancsfájl a következő előfeltételek hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="2b9be-272">hello script has hello following prerequisites:</span></span>

* <span data-ttu-id="2b9be-273">hello parancsfájl csak a Windows 10 és Windows Server 2016 futtatható Azure Resource Manager modulok 2.01 és újabb verziók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-273">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 2.01 and later.</span></span> <span data-ttu-id="2b9be-274">A korábbi Windows-verziók esetében nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="2b9be-274">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="2b9be-275">Az Azure PowerShell 1.0-s és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="2b9be-275">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="2b9be-276">További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="2b9be-276">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>
* <span data-ttu-id="2b9be-277">Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.</span><span class="sxs-lookup"><span data-stu-id="2b9be-277">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="2b9be-278">tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájlok kötelező paraméterek tartoznak, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2b9be-278">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello scripts, do hello following:</span></span>
1. <span data-ttu-id="2b9be-279">A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-279">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span> 
2. <span data-ttu-id="2b9be-280">A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="2b9be-280">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="2b9be-281">Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.</span><span class="sxs-lookup"><span data-stu-id="2b9be-281">Note hello values on hello **Properties** blade.</span></span>

![hello Automation-fiók "Tulajdonságok" panelen](media/automation-sec-configure-azure-runas-account/automation-account-properties.png)  

### <a name="create-a-run-as-account-powershell-script"></a><span data-ttu-id="2b9be-283">Futtató fiókhoz használható PowerShell-szkript létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b9be-283">Create a Run As account PowerShell script</span></span>
<span data-ttu-id="2b9be-284">A PowerShell-parancsfájl a következő konfigurációk hello támogatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="2b9be-284">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="2b9be-285">Futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-285">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="2b9be-286">Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-286">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="2b9be-287">Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="2b9be-287">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="2b9be-288">Hozzon létre egy futtató fiókot és egy klasszikus futtató fiókot egy önaláírt tanúsítványt a hello Azure Government felhő.</span><span class="sxs-lookup"><span data-stu-id="2b9be-288">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="2b9be-289">Attól függően, hogy hello konfigurációs beállítást választja a hello parancsfájl a következő elemek hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="2b9be-289">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="2b9be-290">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="2b9be-290">**For Run As accounts:**</span></span>

* <span data-ttu-id="2b9be-291">Létrehoz egy Azure AD alkalmazás toobe exportálni vagy hello önaláírt vagy vállalati tanúsítvány nyilvános kulcsát, az Azure AD-hoz létre egy egyszerű szolgáltatásfiók hello alkalmazáshoz, és rendel hello közreműködő szerepkört az aktuális hello fiókhoz előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2b9be-291">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="2b9be-292">Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="2b9be-292">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="2b9be-293">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="2b9be-293">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="2b9be-294">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-294">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="2b9be-295">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b9be-295">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="2b9be-296">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-296">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="2b9be-297">hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.</span><span class="sxs-lookup"><span data-stu-id="2b9be-297">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="2b9be-298">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="2b9be-298">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="2b9be-299">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-299">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="2b9be-300">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b9be-300">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="2b9be-301">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="2b9be-301">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="2b9be-302">hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b9be-302">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="2b9be-303">Ha a beállítást vagy a klasszikus futtató fiók létrehozásához, hello parancsfájl végrehajtása után, feltöltés hello nyilvános tanúsítványtároló (.cer kiterjesztésű) toohello felügyeleti hello előfizetés adott hello Automation-fiók hozták létre.</span><span class="sxs-lookup"><span data-stu-id="2b9be-303">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

<span data-ttu-id="2b9be-304">tooexecute parancsfájl hello és hello-tanúsítvány feltöltése, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2b9be-304">tooexecute hello script and upload hello certificate, do hello following:</span></span>

1. <span data-ttu-id="2b9be-305">Mentse a parancsfájlt a számítógépen a következő hello.</span><span class="sxs-lookup"><span data-stu-id="2b9be-305">Save hello following script on your computer.</span></span> <span data-ttu-id="2b9be-306">Ebben a példában mentse hello Fájlnév *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="2b9be-306">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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

        # Sleep here for a few seconds tooallow hello service principal application toobecome active (ordinarily takes a few seconds)
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
           Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
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

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $CertifcateAssetName $PfxCertPathForRunAsAccount $PfxCertPlainPasswordForRunAsAccount $true

        # Populate hello ConnectionFieldValues
        $SubscriptionInfo = Get-AzureRmSubscription -SubscriptionId $SubscriptionId
        $TenantID = $SubscriptionInfo | Select TenantId -First 1
        $Thumbprint = $PfxCert.Thumbprint
        $ConnectionFieldValues = @{"ApplicationId" = $ApplicationId; "TenantId" = $TenantID.TenantId; "CertificateThumbprint" = $Thumbprint; "SubscriptionId" = $SubscriptionId}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ConnectionAssetName $ConnectionTypeName $ConnectionFieldValues

        if ($CreateClassicRunAsAccount) {
            # Create a Run As account by using a service principal
            $ClassicRunAsAccountCertifcateAssetName = "AzureClassicRunAsCertificate"
            $ClassicRunAsAccountConnectionAssetName = "AzureClassicRunAsConnection"
            $ClassicRunAsAccountConnectionTypeName = "AzureClassicCertificate "
            $UploadMessage = "Please upload hello .cer format of #CERT# toohello Management store by following hello steps below." + [Environment]::NewLine +
                    "Log in toohello Microsoft Azure Management portal (https://manage.windowsazure.com) and select Settings -> Management Certificates." + [Environment]::NewLine +
                    "Then click Upload and upload hello .cer format of #CERT#"

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

        # Create hello Automation certificate asset
        CreateAutomationCertificateAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountCertifcateAssetName $PfxCertPathForClassicRunAsAccount $PfxCertPlainPasswordForClassicRunAsAccount $false

        # Populate hello ConnectionFieldValues
        $SubscriptionName = $subscription.Subscription.SubscriptionName
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="2b9be-307">A számítógépén kattintson a **Start** gombra, majd indítsa el a **Windows PowerShellt** emelt szintű felhasználói jogokkal.</span><span class="sxs-lookup"><span data-stu-id="2b9be-307">On your computer, click **Start**, and then start **Windows PowerShell** with elevated user rights.</span></span>

3. <span data-ttu-id="2b9be-308">A hello emelt szintű PowerShell parancssori felület, az 1. lépésben létrehozott hello parancsfájl tartalmazó lépjen toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="2b9be-308">From hello elevated PowerShell command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>

4. <span data-ttu-id="2b9be-309">Hajtsa végre a hello parancsfájlt hello paraméter értékének használatával hello konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="2b9be-309">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="2b9be-310">**Futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="2b9be-310">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="2b9be-311">**Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="2b9be-311">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="2b9be-312">**Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="2b9be-312">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="2b9be-313">**Hozzon létre egy futtató fiókot és egy klasszikus Futtatás mint fiók hello Azure Government felhő önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="2b9be-313">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="2b9be-314">Hello parancsfájl által végrehajtott, miután az Azure-ral felszólító tooauthenticate fogja.</span><span class="sxs-lookup"><span data-stu-id="2b9be-314">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="2b9be-315">Olyan fiókkal jelentkezzen be, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="2b9be-315">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="2b9be-316">Miután hello parancsfájl végrehajtása sikeres, vegye figyelembe a hello következőket:</span><span class="sxs-lookup"><span data-stu-id="2b9be-316">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="2b9be-317">Nyilvános önaláírt tanúsítvány (.cer-fájl) klasszikus futtató fiókot hozta létre, ha hello parancsfájl hoz létre, és a számítógép hello felhasználói profil toohello ideiglenes fájlok mappába menti *%USERPROFILE%\AppData\Local\Temp*, használt tooexecute hello PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="2b9be-317">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="2b9be-318">Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="2b9be-318">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="2b9be-319">Kövesse az utasításokat hello [feltöltése a felügyeleti API tanúsítvány toohello a klasszikus Azure portálon](../azure-api-management-certs.md), és szolgáltatásfelügyelet erőforrások hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód a szolgáltatás felügyeleti erőforrásról tooauthenticate](#sample-code-to-authenticate-with-service-management-resources).</span><span class="sxs-lookup"><span data-stu-id="2b9be-319">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with Service Management resources by using hello [sample code tooauthenticate with Service Management Resources](#sample-code-to-authenticate-with-service-management-resources).</span></span> 
* <span data-ttu-id="2b9be-320">Ha elvégezte *nem* klasszikus futtató fiók létrehozása, erőforrás-kezelő erőforrások a hitelesítést és hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód azonosítja a felügyeleti szolgáltatás erőforrások](#sample-code-to-authenticate-with-resource-manager-resources).</span><span class="sxs-lookup"><span data-stu-id="2b9be-320">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](#sample-code-to-authenticate-with-resource-manager-resources).</span></span>

## <a name="sample-code-tooauthenticate-with-resource-manager-resources"></a><span data-ttu-id="2b9be-321">A minta kód tooauthenticate a Resource Managerhez tartozó erőforrások</span><span class="sxs-lookup"><span data-stu-id="2b9be-321">Sample code tooauthenticate with Resource Manager resources</span></span>
<span data-ttu-id="2b9be-322">Frissített hello alábbi példakód hello vett *AzureAutomationTutorialScript* példa runbook, a runbookokat hello Futtatás mint fiók toomanage Resource Managerhez tartozó erőforrások használatával tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="2b9be-322">You can use hello following updated sample code, taken from hello *AzureAutomationTutorialScript* example runbook, tooauthenticate by using hello Run As account toomanage Resource Manager resources with your runbooks.</span></span>

    $connectionName = "AzureRunAsConnection"
    $SubId = Get-AutomationVariable -Name 'SubscriptionId'
    try
    {
       # Get hello connection "AzureRunAsConnection "
       $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

       "Signing in tooAzure..."
       Add-AzureRmAccount `
         -ServicePrincipal `
         -TenantId $servicePrincipalConnection.TenantId `
         -ApplicationId $servicePrincipalConnection.ApplicationId `
         -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
       "Setting context tooa specific subscription"     
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

<span data-ttu-id="2b9be-323">több előfizetéssel is működnek, tooeasily toohelp, hello parancsfájl két további sornyi kód, amely támogatja a hivatkozó egy előfizetési kontextust is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b9be-323">toohelp you tooeasily work between multiple subscriptions, hello script includes two additional lines of code that support referencing a subscription context.</span></span> <span data-ttu-id="2b9be-324">Egy változó eszköz nevű *SubscriptionId* hello Azonosítóját hello előfizetés tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="2b9be-324">A variable asset named *SubscriptionId* contains hello ID of hello subscription.</span></span> <span data-ttu-id="2b9be-325">Hello után `Add-AzureRmAccount` parancsmag utasítás hello [ `Set-AzureRmContext` ](/powershell/module/azurerm.profile/set-azurermcontext) parancsmag van megadva a hello paraméterhalmaz *- SubscriptionId*.</span><span class="sxs-lookup"><span data-stu-id="2b9be-325">After hello `Add-AzureRmAccount` cmdlet statement, hello [`Set-AzureRmContext`](/powershell/module/azurerm.profile/set-azurermcontext) cmdlet is stated with hello parameter set *-SubscriptionId*.</span></span> <span data-ttu-id="2b9be-326">Ha hello változó neve túl általános, javítsa ki a tooinclude előtag, vagy használja egy másik elnevezési egyezmény toomake azt könnyebb tooidentify.</span><span class="sxs-lookup"><span data-stu-id="2b9be-326">If hello variable name is too generic, you can revise it tooinclude a prefix or use another naming convention toomake it easier tooidentify.</span></span> <span data-ttu-id="2b9be-327">Másik lehetőségként használhatja a hello paraméterkészletet alakítanak *- SubscriptionName* helyett *- SubscriptionId* a megfelelő változóeszköz.</span><span class="sxs-lookup"><span data-stu-id="2b9be-327">Alternatively, you can use hello parameter set *-SubscriptionName* instead of *-SubscriptionId* with a corresponding variable asset.</span></span>

<span data-ttu-id="2b9be-328">a hello runbook hitelesítéséhez használt parancsmag hello `Add-AzureRmAccount`, használ hello *ServicePrincipalCertificate* paraméterhalmaz.</span><span class="sxs-lookup"><span data-stu-id="2b9be-328">hello cmdlet that you use for authenticating in hello runbook, `Add-AzureRmAccount`, uses hello *ServicePrincipalCertificate* parameter set.</span></span> <span data-ttu-id="2b9be-329">Hello szolgáltatás egyszerű tanúsítvány, nem a hello felhasználói hitelesítő adatok használatával hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="2b9be-329">It authenticates by using hello service principal certificate, not hello user credentials.</span></span>

## <a name="sample-code-tooauthenticate-with-service-management-resources"></a><span data-ttu-id="2b9be-330">A minta kód tooauthenticate szolgáltatásfelügyelet erőforrásokkal</span><span class="sxs-lookup"><span data-stu-id="2b9be-330">Sample code tooauthenticate with Service Management resources</span></span>
<span data-ttu-id="2b9be-331">Használhatja a következő frissített mintakód, amely hello forrása hello *AzureClassicAutomationTutorialScript* példa runbook, tooauthenticate klasszikus futtató fiókhoz, toomanage a hagyományos erőforrások hello segítségével a runbookok.</span><span class="sxs-lookup"><span data-stu-id="2b9be-331">You can use hello following updated sample code, which is taken from hello *AzureClassicAutomationTutorialScript* example runbook, tooauthenticate by using hello Classic Run As account toomanage classic resources with your runbooks.</span></span>

    $ConnectionAssetName = "AzureClassicRunAsConnection"
    # Get hello connection
    $connection = Get-AutomationConnection -Name $connectionAssetName        

    # Authenticate tooAzure with certificate
    Write-Verbose "Get connection asset: $ConnectionAssetName" -Verbose
    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
    if ($Conn -eq $null)
    {
       throw "Could not retrieve connection asset: $ConnectionAssetName. Assure that this asset exists in hello Automation account."
    }

    $CertificateAssetName = $Conn.CertificateAssetName
    Write-Verbose "Getting hello certificate: $CertificateAssetName" -Verbose
    $AzureCert = Get-AutomationCertificate -Name $CertificateAssetName
    if ($AzureCert -eq $null)
    {
       throw "Could not retrieve certificate asset: $CertificateAssetName. Assure that this asset exists in hello Automation account."
    }

    Write-Verbose "Authenticating tooAzure with certificate." -Verbose
    Set-AzureSubscription -SubscriptionName $Conn.SubscriptionName -SubscriptionId $Conn.SubscriptionID -Certificate $AzureCert
    Select-AzureSubscription -SubscriptionId $Conn.SubscriptionID

## <a name="next-steps"></a><span data-ttu-id="2b9be-332">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b9be-332">Next steps</span></span>
* [<span data-ttu-id="2b9be-333">Alkalmazás- és egyszerű szolgáltatási objektumok Azure AD-ben</span><span class="sxs-lookup"><span data-stu-id="2b9be-333">Application and service principal objects in Azure AD</span></span>](../active-directory/active-directory-application-objects.md)
* [<span data-ttu-id="2b9be-334">Szerepköralapú hozzáférés-vezérlés az Azure Automationben</span><span class="sxs-lookup"><span data-stu-id="2b9be-334">Role-based access control in Azure Automation</span></span>](automation-role-based-access-control.md)
* [<span data-ttu-id="2b9be-335">Azure Cloud Services – tanúsítványok áttekintése</span><span class="sxs-lookup"><span data-stu-id="2b9be-335">Certificates overview for Azure Cloud Services</span></span>](../cloud-services/cloud-services-certs-create.md)
