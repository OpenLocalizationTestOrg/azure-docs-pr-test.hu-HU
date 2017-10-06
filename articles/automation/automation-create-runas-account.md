---
title: "Azure Automation futtató fiókok aaaCreate |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooupdate az automatizálási fiókot és futtató fiókok létrehozása a PowerShell-lel, vagy hello portálról."
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
ms.date: 07/27/2017
ms.author: magoedte
ms.openlocfilehash: 94eb54fa0b518056a726d17146c63411e248273b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-automation-account-authentication-with-run-as-accounts"></a><span data-ttu-id="5c97a-103">Automation-fiók hitelesítésének frissítése futtató fiókokkal</span><span class="sxs-lookup"><span data-stu-id="5c97a-103">Update your Automation account authentication with Run As accounts</span></span> 
<span data-ttu-id="5c97a-104">Hello portálról a meglévő Automation-fiók frissítéséhez, vagy a PowerShell segítségével, ha:</span><span class="sxs-lookup"><span data-stu-id="5c97a-104">You can update your existing Automation account from hello portal or use PowerShell if:</span></span>

* <span data-ttu-id="5c97a-105">Automation-fiók létrehozása, de elutasítása toocreate hello futtató fiókot.</span><span class="sxs-lookup"><span data-stu-id="5c97a-105">You create an Automation account but decline toocreate hello Run As account.</span></span>
* <span data-ttu-id="5c97a-106">Már használja egy automatizálási fiókot toomanage Resource Managerhez tartozó erőforrások és tooupdate hello tooinclude hello futtató fiókok kívánt runbook-hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="5c97a-106">You already use an Automation account toomanage Resource Manager resources and you want tooupdate hello account tooinclude hello Run As account for runbook authentication.</span></span>
* <span data-ttu-id="5c97a-107">Az automatizálási fiók toomanage hagyományos erőforrások már használja, és azt szeretné, hogy tooupdate azt toouse hello klasszikus futtató fiókot új fiók létrehozása és a forgatókönyve és eszköze tooit áttelepítése helyett.</span><span class="sxs-lookup"><span data-stu-id="5c97a-107">You already use an Automation account toomanage classic resources and you want tooupdate it toouse hello Classic Run As account instead of creating a new account and migrating your runbooks and assets tooit.</span></span>   
* <span data-ttu-id="5c97a-108">Érdemes toocreate egy olyan futtató és a klasszikus futtató fiókot a vállalati hitelesítésszolgáltató (CA) által kiadott tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="5c97a-108">You want toocreate a Run As and a Classic Run As account by using a certificate issued by your enterprise certification authority (CA).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5c97a-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5c97a-109">Prerequisites</span></span>

* <span data-ttu-id="5c97a-110">hello parancsfájl futtatása csak Windows 10 és Windows Server 2016 az Azure Resource Manager modulok 3.0.0 és későbbi lehet.</span><span class="sxs-lookup"><span data-stu-id="5c97a-110">hello script can be run only on Windows 10 and Windows Server 2016 with Azure Resource Manager modules 3.0.0 and later.</span></span> <span data-ttu-id="5c97a-111">A korábbi Windows-verziók esetében nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="5c97a-111">It is not supported on earlier versions of Windows.</span></span>
* <span data-ttu-id="5c97a-112">Az Azure PowerShell 1.0-s és újabb verziói.</span><span class="sxs-lookup"><span data-stu-id="5c97a-112">Azure PowerShell 1.0 and later.</span></span> <span data-ttu-id="5c97a-113">További információk hello PowerShell 1.0-s kiadásról: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="5c97a-113">For information about hello PowerShell 1.0 release, see [How tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>
* <span data-ttu-id="5c97a-114">Automation-fiók, hello hello értékként hivatkozott *– AutomationAccountName* és *- ApplicationDisplayName* paramétereket a PowerShell-parancsfájl a következő hello.</span><span class="sxs-lookup"><span data-stu-id="5c97a-114">An Automation account, which is referenced as hello value for hello *–AutomationAccountName* and *-ApplicationDisplayName* parameters in hello following PowerShell script.</span></span>

<span data-ttu-id="5c97a-115">tooget hello értékei *SubscriptionID*, *ResourceGroup*, és *AutomationAccountName*, amely hello parancsfájl kötelező paraméterek tartoznak, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="5c97a-115">tooget hello values for *SubscriptionID*, *ResourceGroup*, and *AutomationAccountName*, which are required parameters for hello script, do hello following:</span></span>

1. <span data-ttu-id="5c97a-116">A hello Azure-portálon, válassza ki az Automation-fiókban lévő hello **Automation-fiók** panelt, és válassza **összes beállítás**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-116">In hello Azure portal, select your Automation account on hello **Automation account** blade, and then select **All settings**.</span></span>  
2. <span data-ttu-id="5c97a-117">A hello **összes beállítás** panel alatt **Fiókbeállítások**, jelölje be **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-117">On hello **All settings** blade, under **Account Settings**, select **Properties**.</span></span> 
3. <span data-ttu-id="5c97a-118">Vegye figyelembe a hello hello értékek **tulajdonságok** panelen.</span><span class="sxs-lookup"><span data-stu-id="5c97a-118">Note hello values on hello **Properties** blade.</span></span><br><br> <span data-ttu-id="5c97a-119">![hello Automation-fiók "Tulajdonságok" panelen](media/automation-create-runas-account/automation-account-properties.png)</span><span class="sxs-lookup"><span data-stu-id="5c97a-119">![hello Automation account "Properties" blade](media/automation-create-runas-account/automation-account-properties.png)</span></span>  

### <a name="required-permissions-tooupdate-your-automation-account"></a><span data-ttu-id="5c97a-120">Engedélyek tooupdate az Automation-fiók szükséges</span><span class="sxs-lookup"><span data-stu-id="5c97a-120">Required permissions tooupdate your Automation account</span></span>
<span data-ttu-id="5c97a-121">tooupdate Automation-fiók, rendelkeznie kell a következő bizonyos jogokat hello és engedélyek szükséges toocomplete ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="5c97a-121">tooupdate an Automation account, you must have hello following specific privileges and permissions required toocomplete this topic.</span></span>   
 
* <span data-ttu-id="5c97a-122">Az AD-felhasználói fiókot Microsoft.Automation erőforrások kell toobe hozzáadott tooa szerepkör engedélyek egyenértékű toohello közreműködő szerepkörrel rendelkező, a cikkben leírt módon történő [szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md#contributor-role-permissions).</span><span class="sxs-lookup"><span data-stu-id="5c97a-122">Your AD user account needs toobe added tooa role with permissions equivalent toohello Contributor role for Microsoft.Automation resources as outlined in article [Role-based access control in Azure Automation](automation-role-based-access-control.md#contributor-role-permissions).</span></span>  
* <span data-ttu-id="5c97a-123">Az Azure AD-bérlő a nem rendszergazda felhasználók is [AD-alkalmazásokat regisztrálni](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) Ha hello App regisztrációk beállítás értéke túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-123">Non-admin users in your Azure AD tenant can [register AD applications](../azure-resource-manager/resource-group-create-service-principal-portal.md#check-azure-subscription-permissions) if hello App registrations setting is set too**Yes**.</span></span>  <span data-ttu-id="5c97a-124">Ha hello app regisztrációk beállítás értéke túl**nem**, a művelet végrehajtása hello felhasználói globális rendszergazdának kell lennie az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="5c97a-124">If hello app registrations setting is set too**No**, hello user performing this action must be a global administrator in Azure AD.</span></span> 

<span data-ttu-id="5c97a-125">Ha nem tagja Active Directory-példányban hello előfizetés toohello globális rendszergazda/járművezető-administrator szerepkör hello előfizetés vannak adása előtt, akkor tooActive Directory vendégként kerülnek.</span><span class="sxs-lookup"><span data-stu-id="5c97a-125">If you are not a member of hello subscription’s Active Directory instance before you are added toohello global administrator/co-administrator role of hello subscription, you are added tooActive Directory as a guest.</span></span> <span data-ttu-id="5c97a-126">Ebben az esetben a megjelenik a "nem rendelkezik engedélyekkel toocreate..."</span><span class="sxs-lookup"><span data-stu-id="5c97a-126">In this situation, you receive a “You do not have permissions toocreate…”</span></span> <span data-ttu-id="5c97a-127">Figyelmeztetés-hello **Automation-fiók hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="5c97a-127">warning on hello **Add Automation Account** blade.</span></span> <span data-ttu-id="5c97a-128">Felhasználók, akik toohello globális rendszergazda/járművezető-administrator szerepkör először eltávolíthatja hello előfizetés Active Directory-példányban, és újból hozzáadták toomake hozzáadták azokat a teljes felhasználói az Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="5c97a-128">Users who were added toohello global administrator/co-administrator role first can be removed from hello subscription's Active Directory instance and re-added toomake them a full User in Active Directory.</span></span> <span data-ttu-id="5c97a-129">tooverify ebben az esetben a hello **Azure Active Directory** hello Azure portálon, válassza a panelen **felhasználók és csoportok**, jelölje be **minden felhasználó** és hello kiválasztása után adott felhasználó, jelölje be **profil**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-129">tooverify this situation, from hello **Azure Active Directory** pane in hello Azure portal, select **Users and groups**, select **All users** and, after you select hello specific user, select **Profile**.</span></span> <span data-ttu-id="5c97a-130">hello értékének hello **felhasználótípust** attribútum hello felhasználó profil alapján nem értékűnek kell lennie **vendég**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-130">hello value of hello **User type** attribute under hello users profile should not equal **Guest**.</span></span>

## <a name="create-run-as-account-from-hello-portal"></a><span data-ttu-id="5c97a-131">Futtató fiók létrehozása hello portálról</span><span class="sxs-lookup"><span data-stu-id="5c97a-131">Create Run As account from hello portal</span></span>
<span data-ttu-id="5c97a-132">Ebben a részben hajtható végre a következő lépéseket tooupdate hello az Azure Automation-fiók hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5c97a-132">In this section, perform hello following steps tooupdate your Azure Automation account from  hello Azure portal.</span></span>  <span data-ttu-id="5c97a-133">Létrehozhat hello futtató és a klasszikus futtató fiókok külön-külön, és ha a klasszikus Azure portálon hello toomanage erőforrások nem szükséges, létrehozhat hello Azure-beli futtató fiókot csak.</span><span class="sxs-lookup"><span data-stu-id="5c97a-133">You create hello Run As and Classic Run As accounts individually, and if you don't need toomanage resources in hello classic Azure portal, you can just create hello Azure Run As account.</span></span>  

<span data-ttu-id="5c97a-134">hello folyamat a következő elemeket az Automation-fiókban hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5c97a-134">hello process creates hello following items in your Automation account.</span></span>

<span data-ttu-id="5c97a-135">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="5c97a-135">**For Run As accounts:**</span></span>

* <span data-ttu-id="5c97a-136">Egy Azure AD-alkalmazást hoz létre egy önaláírt tanúsítványt hoz létre egy egyszerű szolgáltatásfiók hello alkalmazás az Azure AD és hello közreműködő szerepkört az aktuális előfizetésben hello fiókhoz rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="5c97a-136">Creates an Azure AD application with a self-signed certificate, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="5c97a-137">Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="5c97a-137">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="5c97a-138">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5c97a-138">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5c97a-139">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-139">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-140">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-140">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="5c97a-141">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-141">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-142">hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.</span><span class="sxs-lookup"><span data-stu-id="5c97a-142">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="5c97a-143">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="5c97a-143">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="5c97a-144">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-144">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-145">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-145">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="5c97a-146">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-146">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-147">hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-147">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

1. <span data-ttu-id="5c97a-148">Bejelentkezés toohello egy olyan fiókkal, amely hello előfizetés-Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5c97a-148">Sign in toohello Azure portal with an account that is a member of hello Subscription Admins role and co-administrator of hello subscription.</span></span>
2. <span data-ttu-id="5c97a-149">Hello Automation-fiók panelen válassza ki **futtató fiókok** hello szakaszban **Fiókbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="5c97a-149">From hello Automation account blade, select **Run As Accounts** under hello section **Account Settings**.</span></span>  
3. <span data-ttu-id="5c97a-150">Attól függően, hogy melyik fiókra van szüksége, válassza az **Azure-alapú futtató fiók** vagy a **Klasszikus Azure-alapú futtató fiók** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="5c97a-150">Depending on which account you require, select either **Azure Run As Account** or **Azure Classic Run As Account**.</span></span>  <span data-ttu-id="5c97a-151">Vagy hello kijelölése után **adja hozzá Azure-beli futtató** vagy **hozzáadása Azure klasszikus futtató fiók** panel jelenik meg, és hello áttekintő információkat áttekintése után kattintson **létrehozása** a futtató fiók létrehozása tooproceed.</span><span class="sxs-lookup"><span data-stu-id="5c97a-151">After selecting either hello **Add Azure Run As** or **Add Azure Classic Run As Account** blade appears and after reviewing hello overview information, click **Create** tooproceed with Run As account creation.</span></span>  
4. <span data-ttu-id="5c97a-152">Azure hello Futtatás mint fiókot hoz létre, amíg előrehaladásának hello alatt **értesítések** a hello menüre, és a fejléc megjelenik hello fiók létrehozása folyamatban van megjelölve.</span><span class="sxs-lookup"><span data-stu-id="5c97a-152">While Azure creates hello Run As account, you can track hello progress under **Notifications** from hello menu and a banner is displayed stating hello account is being created.</span></span>  <span data-ttu-id="5c97a-153">A folyamat eltarthat néhány percig toocomplete.</span><span class="sxs-lookup"><span data-stu-id="5c97a-153">This process can take a few minutes toocomplete.</span></span>  

## <a name="create-run-as-account-using-powershell-script"></a><span data-ttu-id="5c97a-154">Futtató fiók létrehozása PowerShell-szkripttel</span><span class="sxs-lookup"><span data-stu-id="5c97a-154">Create Run As account using PowerShell script</span></span>
<span data-ttu-id="5c97a-155">A PowerShell-parancsfájl a következő konfigurációk hello támogatását tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5c97a-155">This PowerShell script includes support for hello following configurations:</span></span>

* <span data-ttu-id="5c97a-156">Futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="5c97a-156">Create a Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="5c97a-157">Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="5c97a-157">Create a Run As account and a Classic Run As account by using a self-signed certificate.</span></span>
* <span data-ttu-id="5c97a-158">Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="5c97a-158">Create a Run As account and a Classic Run As account by using an enterprise certificate.</span></span>
* <span data-ttu-id="5c97a-159">Hozzon létre egy futtató fiókot és egy klasszikus futtató fiókot egy önaláírt tanúsítványt a hello Azure Government felhő.</span><span class="sxs-lookup"><span data-stu-id="5c97a-159">Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud.</span></span>

<span data-ttu-id="5c97a-160">Attól függően, hogy hello konfigurációs beállítást választja a hello parancsfájl a következő elemek hello hoz létre.</span><span class="sxs-lookup"><span data-stu-id="5c97a-160">Depending on hello configuration option you select, hello script creates hello following items.</span></span>

<span data-ttu-id="5c97a-161">**Futtató fiókok esetén:**</span><span class="sxs-lookup"><span data-stu-id="5c97a-161">**For Run As accounts:**</span></span>

* <span data-ttu-id="5c97a-162">Létrehoz egy Azure AD alkalmazás toobe exportálni vagy hello önaláírt vagy vállalati tanúsítvány nyilvános kulcsát, az Azure AD-hoz létre egy egyszerű szolgáltatásfiók hello alkalmazáshoz, és rendel hello közreműködő szerepkört az aktuális hello fiókhoz előfizetés.</span><span class="sxs-lookup"><span data-stu-id="5c97a-162">Creates an Azure AD application toobe exported with either hello self-signed or enterprise certificate public key, creates a service principal account for hello application in Azure AD, and assigns hello Contributor role for hello account in your current subscription.</span></span> <span data-ttu-id="5c97a-163">Ez a beállítás tooOwner vagy bármilyen más szerepkör módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="5c97a-163">You can change this setting tooOwner or any other role.</span></span> <span data-ttu-id="5c97a-164">További információk: [Szerepköralapú hozzáférés-vezérlés az Azure Automationben](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="5c97a-164">For more information, see [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="5c97a-165">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-165">Creates an Automation certificate asset named *AzureRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-166">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello Azure AD-alkalmazás által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-166">hello certificate asset holds hello certificate private key that's used by hello Azure AD application.</span></span>
* <span data-ttu-id="5c97a-167">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-167">Creates an Automation connection asset named *AzureRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-168">hello kapcsolódási eszköz rendelkezik hello applicationId, tenantId, előfizetés-azonosító és tanúsítvány-ujjlenyomatot.</span><span class="sxs-lookup"><span data-stu-id="5c97a-168">hello connection asset holds hello applicationId, tenantId, subscriptionId, and certificate thumbprint.</span></span>

<span data-ttu-id="5c97a-169">**Klasszikus futtatófiókok esetében:**</span><span class="sxs-lookup"><span data-stu-id="5c97a-169">**For Classic Run As accounts:**</span></span>

* <span data-ttu-id="5c97a-170">Létrehoz egy nevű Automation szolgáltatásbeli tanúsítványeszköz *AzureClassicRunAsCertificate* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-170">Creates an Automation certificate asset named *AzureClassicRunAsCertificate* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-171">hello tanúsítványeszköz hello tanúsítvány titkos kulcsa hello felügyeleti tanúsítvány által használt tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-171">hello certificate asset holds hello certificate private key used by hello management certificate.</span></span>
* <span data-ttu-id="5c97a-172">Létrehoz egy nevű Automation szolgáltatásbeli kapcsolateszköz *AzureClassicRunAsConnection* a hello megadva az Automation-fiók.</span><span class="sxs-lookup"><span data-stu-id="5c97a-172">Creates an Automation connection asset named *AzureClassicRunAsConnection* in hello specified Automation account.</span></span> <span data-ttu-id="5c97a-173">hello kapcsolódási eszköz hello előfizetés nevét, a subscriptionId és a tanúsítvány eszköz neve tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5c97a-173">hello connection asset holds hello subscription name, subscriptionId, and certificate asset name.</span></span>

>[!NOTE]
> <span data-ttu-id="5c97a-174">Ha a beállítást vagy a klasszikus futtató fiók létrehozásához, hello parancsfájl végrehajtása után, feltöltés hello nyilvános tanúsítványtároló (.cer kiterjesztésű) toohello felügyeleti hello előfizetés adott hello Automation-fiók hozták létre.</span><span class="sxs-lookup"><span data-stu-id="5c97a-174">If you select either option for creating a Classic Run As account, after hello script is executed, upload hello public certificate (.cer file name extension) toohello management store for hello subscription that hello Automation account was created in.</span></span>
> 

1. <span data-ttu-id="5c97a-175">Mentse a parancsfájlt a számítógépen a következő hello.</span><span class="sxs-lookup"><span data-stu-id="5c97a-175">Save hello following script on your computer.</span></span> <span data-ttu-id="5c97a-176">Ebben a példában mentse hello Fájlnév *New-RunAsAccount.ps1*.</span><span class="sxs-lookup"><span data-stu-id="5c97a-176">In this example, save it with hello filename *New-RunAsAccount.ps1*.</span></span>

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
        $KeyCredential.EndDate = Get-Date $PfxCert.GetExpirationDateString()
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
        if (!(($AzureRMProfileVersion.Major -ge 3 -and $AzureRMProfileVersion.Minor -ge 0) -or ($AzureRMProfileVersion.Major -gt 3)))
        {
            Write-Error -Message "Please install hello latest Azure PowerShell and retry. Relevant doc url : https://docs.microsoft.com/powershell/azureps-cmdlets-docs/ "
            return
        }

        Login-AzureRmAccount -Environment $EnvironmentName 
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
        $SubscriptionName = $subscription.Subscription.Name
        $ClassicRunAsAccountConnectionFieldValues = @{"SubscriptionName" = $SubscriptionName; "SubscriptionId" = $SubscriptionId; "CertificateAssetName" = $ClassicRunAsAccountCertifcateAssetName}

        # Create an Automation connection asset named AzureRunAsConnection in hello Automation account. This connection uses hello service principal.
        CreateAutomationConnectionAsset $ResourceGroup $AutomationAccountName $ClassicRunAsAccountConnectionAssetName $ClassicRunAsAccountConnectionTypeName $ClassicRunAsAccountConnectionFieldValues

        Write-Host -ForegroundColor red $UploadMessage
        }

2. <span data-ttu-id="5c97a-177">A számítógépen indítása **Windows PowerShell** a hello **Start** képernyő emelt szintű felhasználói jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="5c97a-177">On your computer, start **Windows PowerShell** from hello **Start** screen with elevated user rights.</span></span>
3. <span data-ttu-id="5c97a-178">A hello emelt szintű parancssori rendszerhéj, az 1. lépésben létrehozott hello parancsfájl tartalmazó lépjen toohello mappát.</span><span class="sxs-lookup"><span data-stu-id="5c97a-178">From hello elevated command-line shell, go toohello folder that contains hello script you created in step 1.</span></span>  
4. <span data-ttu-id="5c97a-179">Hajtsa végre a hello parancsfájlt hello paraméter értékének használatával hello konfiguráció szükséges.</span><span class="sxs-lookup"><span data-stu-id="5c97a-179">Execute hello script by using hello parameter values for hello configuration you require.</span></span>

    <span data-ttu-id="5c97a-180">**Futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="5c97a-180">**Create a Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $false`

    <span data-ttu-id="5c97a-181">**Futtató fiók és klasszikus futtató fiók létrehozása önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="5c97a-181">**Create a Run As account and a Classic Run As account by using a self-signed certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true`

    <span data-ttu-id="5c97a-182">**Futtató fiók és klasszikus futtató fiók létrehozása vállalati tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="5c97a-182">**Create a Run As account and a Classic Run As account by using an enterprise certificate**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication>  -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true -EnterpriseCertPathForRunAsAccount <EnterpriseCertPfxPathForRunAsAccount> -EnterpriseCertPlainPasswordForRunAsAccount <StrongPassword> -EnterpriseCertPathForClassicRunAsAccount <EnterpriseCertPfxPathForClassicRunAsAccount> -EnterpriseCertPlainPasswordForClassicRunAsAccount <StrongPassword>`

    <span data-ttu-id="5c97a-183">**Hozzon létre egy futtató fiókot és egy klasszikus Futtatás mint fiók hello Azure Government felhő önaláírt tanúsítvány használatával**</span><span class="sxs-lookup"><span data-stu-id="5c97a-183">**Create a Run As account and a Classic Run As account by using a self-signed certificate in hello Azure Government cloud**</span></span>  
    `.\New-RunAsAccount.ps1 -ResourceGroup <ResourceGroupName> -AutomationAccountName <NameofAutomationAccount> -SubscriptionId <SubscriptionId> -ApplicationDisplayName <DisplayNameofAADApplication> -SelfSignedCertPlainPassword <StrongPassword> -CreateClassicRunAsAccount $true  -EnvironmentName AzureUSGovernment`

    > [!NOTE]
    > <span data-ttu-id="5c97a-184">Hello parancsfájl által végrehajtott, miután az Azure-ral felszólító tooauthenticate fogja.</span><span class="sxs-lookup"><span data-stu-id="5c97a-184">After hello script has executed, you will be prompted tooauthenticate with Azure.</span></span> <span data-ttu-id="5c97a-185">Olyan fiókkal jelentkezzen be, amely hello előfizetés Rendszergazdák szerepkör tagja, és hello előfizetés társadminisztrátoraként.</span><span class="sxs-lookup"><span data-stu-id="5c97a-185">Sign in with an account that is a member of hello subscription administrators role and co-administrator of hello subscription.</span></span>
    >
    >

<span data-ttu-id="5c97a-186">Miután hello parancsfájl végrehajtása sikeres, vegye figyelembe a hello következőket:</span><span class="sxs-lookup"><span data-stu-id="5c97a-186">After hello script has executed successfully, note hello following:</span></span>
* <span data-ttu-id="5c97a-187">Nyilvános önaláírt tanúsítvány (.cer-fájl) klasszikus futtató fiókot hozta létre, ha hello parancsfájl hoz létre, és a számítógép hello felhasználói profil toohello ideiglenes fájlok mappába menti *%USERPROFILE%\AppData\Local\Temp*, használt tooexecute hello PowerShell-munkamenetben.</span><span class="sxs-lookup"><span data-stu-id="5c97a-187">If you created a Classic Run As account with a self-signed public certificate (.cer file), hello script creates and saves it toohello temporary files folder on your computer under hello user profile *%USERPROFILE%\AppData\Local\Temp*, which you used tooexecute hello PowerShell session.</span></span>
* <span data-ttu-id="5c97a-188">Ha egy (.cer formátumú) vállalati tanúsítvánnyal rendelkező klasszikus futtató fiókot hozott létre, használja ezt a tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="5c97a-188">If you created a Classic Run As account with an enterprise public certificate (.cer file), use this certificate.</span></span> <span data-ttu-id="5c97a-189">Hello utasításokat követve [feltöltése a felügyeleti API tanúsítvány toohello a klasszikus Azure portálon](../azure-api-management-certs.md), és ezután ellenőrizze a klasszikus üzembe helyezési erőforrások hello hitelesítőadat-konfiguráció hello segítségével [példakód az Azure klasszikus telepítési erőforrásai tooauthenticate](automation-verify-runas-authentication.md#classic-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="5c97a-189">Follow hello instructions for [uploading a management API certificate toohello Azure classic portal](../azure-api-management-certs.md), and then validate hello credential configuration with classic deployment resources by using hello [sample code tooauthenticate with Azure Classic Deployment Resources](automation-verify-runas-authentication.md#classic-run-as-authentication).</span></span> 
* <span data-ttu-id="5c97a-190">Ha elvégezte *nem* klasszikus futtató fiók létrehozása, erőforrás-kezelő erőforrások a hitelesítést és hello hitelesítőadat-konfiguráció érvényesítése hello segítségével [példakód azonosítja a felügyeleti szolgáltatás erőforrások](automation-verify-runas-authentication.md#automation-run-as-authentication).</span><span class="sxs-lookup"><span data-stu-id="5c97a-190">If you did *not* create a Classic Run As account, authenticate with Resource Manager resources and validate hello credential configuration by using hello [sample code for authenticating with Service Management resources](automation-verify-runas-authentication.md#automation-run-as-authentication).</span></span>

## <a name="next-steps"></a><span data-ttu-id="5c97a-191">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5c97a-191">Next steps</span></span>
* <span data-ttu-id="5c97a-192">Szolgáltatásnevekről kapcsolatos további információkért tekintse meg túl[alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="5c97a-192">For more information about Service Principals, refer too[Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span>
* <span data-ttu-id="5c97a-193">Tanúsítványok és az Azure-szolgáltatásokkal kapcsolatos további információkért tekintse meg túl[tanúsítványok áttekintése Azure-szolgáltatásokhoz](../cloud-services/cloud-services-certs-create.md).</span><span class="sxs-lookup"><span data-stu-id="5c97a-193">For more information about certificates and Azure services, refer too[Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md).</span></span>
