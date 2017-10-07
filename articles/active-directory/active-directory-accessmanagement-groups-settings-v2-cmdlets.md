---
title: "csoportkezelés az Azure Active Directory aaaPowerShell példák |} Microsoft Docs"
description: "Ezen a lapon a PowerShell-példák toohelp biztosít az Azure Active Directoryban csoportok kezelése"
keywords: "Az Azure Active Directory, Azure Active Directory PowerShell, csoportok, csoportok kezelése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: ba049babc436e99a290f20899b3a87bcfa811d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="ab529-104">Csoportok kezelése az Azure Active Directory 2-es parancsmagok</span><span class="sxs-lookup"><span data-stu-id="ab529-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ab529-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ab529-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="ab529-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="ab529-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="ab529-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ab529-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="ab529-108">Ebben a cikkben található példák bemutatják, hogyan toouse PowerShell toomanage a csoportok az Azure Active Directory (Azure AD).</span><span class="sxs-lookup"><span data-stu-id="ab529-108">This article contains examples of how toouse PowerShell toomanage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="ab529-109">Azt is megtudhatja, hogyan tooget állítsa be a hello Azure AD PowerShell modult.</span><span class="sxs-lookup"><span data-stu-id="ab529-109">It also tells you how tooget set up with hello Azure AD PowerShell module.</span></span> <span data-ttu-id="ab529-110">Először meg kell [hello Azure AD PowerShell-modul letöltése](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="ab529-110">First, you must [download hello Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-hello-azure-ad-powershell-module"></a><span data-ttu-id="ab529-111">Hello Azure AD PowerShell-modul telepítése</span><span class="sxs-lookup"><span data-stu-id="ab529-111">Installing hello Azure AD PowerShell module</span></span>
<span data-ttu-id="ab529-112">tooinstall hello Azure AD PowerShell modult, a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="ab529-112">tooinstall hello Azure AD PowerShell module, use hello following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="ab529-113">amely modul hello tooverify lett telepítve, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="ab529-113">tooverify that hello module was installed, use hello following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="ab529-114">Most elindíthatja hello modul hello-parancsmagokkal.</span><span class="sxs-lookup"><span data-stu-id="ab529-114">Now you can start using hello cmdlets in hello module.</span></span> <span data-ttu-id="ab529-115">A hello Azure AD modulban hello parancsmagok teljes leírása, tekintse meg az toohello online dokumentációját [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="ab529-115">For a full description of hello cmdlets in hello Azure AD module, please refer toohello online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-toohello-directory"></a><span data-ttu-id="ab529-116">Csatlakozás toohello könyvtár</span><span class="sxs-lookup"><span data-stu-id="ab529-116">Connecting toohello directory</span></span>
<span data-ttu-id="ab529-117">Azure AD PowerShell-parancsmagok használatával csoportok kezelése előtt a PowerShell-munkamenet toohello directory kívánt toomanage kell csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="ab529-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session toohello directory you want toomanage.</span></span> <span data-ttu-id="ab529-118">A következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="ab529-118">Use hello following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="ab529-119">hello parancsmag kéri a hello hitelesítő adatok toouse tooaccess szeretné, hogy a címtárban.</span><span class="sxs-lookup"><span data-stu-id="ab529-119">hello cmdlet prompts you for hello credentials you want toouse tooaccess your directory.</span></span> <span data-ttu-id="ab529-120">A jelen példában használjuk karen@drumkit.onmicrosoft.com tooaccess hello bemutató könyvtár.</span><span class="sxs-lookup"><span data-stu-id="ab529-120">In this example, we are using karen@drumkit.onmicrosoft.com tooaccess hello demonstration directory.</span></span> <span data-ttu-id="ab529-121">hello parancsmag ad vissza egy megerősítő tooshow hello munkamenet sikeresen csatlakoztatva lett tooyour könyvtár:</span><span class="sxs-lookup"><span data-stu-id="ab529-121">hello cmdlet returns a confirmation tooshow hello session was connected successfully tooyour directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="ab529-122">Most elindíthatja a címtárban hello AzureAD parancsmagok toomanage csoportok használatával.</span><span class="sxs-lookup"><span data-stu-id="ab529-122">Now you can start using hello AzureAD cmdlets toomanage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="ab529-123">Csoportok beolvasása</span><span class="sxs-lookup"><span data-stu-id="ab529-123">Retrieving groups</span></span>
<span data-ttu-id="ab529-124">meglévő csoportok tooretrieve a könyvtárból, használhatja a Get-AzureADGroups parancsmag hello.</span><span class="sxs-lookup"><span data-stu-id="ab529-124">tooretrieve existing groups from your directory you can use hello Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="ab529-125">minden tooretrieve csoportok hello könyvtárban, paraméterek nélkül használja a hello parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="ab529-125">tooretrieve all groups in hello directory, use hello cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="ab529-126">hello parancsmag az összes csoport hello csatlakoztatott címtárhoz adja vissza.</span><span class="sxs-lookup"><span data-stu-id="ab529-126">hello cmdlet returns all groups in hello connected directory.</span></span>

<span data-ttu-id="ab529-127">Hello - objectID paraméter tooretrieve is használhat, amelynek hello csoport objectID megadott adott csoporthoz:</span><span class="sxs-lookup"><span data-stu-id="ab529-127">You can use hello -objectID parameter tooretrieve a specific group for which you specify hello group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="ab529-128">hello parancsmag most hello csoport, amelynek objectID megegyezik-e hello hello paraméter megadott adja vissza:</span><span class="sxs-lookup"><span data-stu-id="ab529-128">hello cmdlet now returns hello group whose objectID matches hello value of hello parameter you entered:</span></span>

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

<span data-ttu-id="ab529-129">Kereshet egy adott csoport használatával hello - szűrő paraméter.</span><span class="sxs-lookup"><span data-stu-id="ab529-129">You can search for a specific group using hello -filter parameter.</span></span> <span data-ttu-id="ab529-130">Ez a paraméter egy ODATA szűrőfeltétel vesz igénybe, és adja vissza az összes csoport szűrőnek hello, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="ab529-130">This parameter takes an ODATA filter clause and returns all groups that match hello filter, as in hello following example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> <span data-ttu-id="ab529-131">hello AzureAD PowerShell-parancsmagok hello szabványos OData-lekérdezés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="ab529-131">hello AzureAD PowerShell cmdlets implement hello OData query standard.</span></span> <span data-ttu-id="ab529-132">További információkért lásd: **$filter** a [OData rendszer lekérdezési lehetőségek hello OData-végpont használatával](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="ab529-132">For more information, see **$filter** in [OData system query options using hello OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="ab529-133">Csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="ab529-133">Creating groups</span></span>
<span data-ttu-id="ab529-134">a címtárban, használja a New-AzureADGroup hello parancsmagot az új csoport toocreate.</span><span class="sxs-lookup"><span data-stu-id="ab529-134">toocreate a new group in your directory, use hello New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="ab529-135">Ez a parancsmag "Marketing" nevű új biztonsági csoportot hoz létre:</span><span class="sxs-lookup"><span data-stu-id="ab529-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="ab529-136">Csoportok frissítése</span><span class="sxs-lookup"><span data-stu-id="ab529-136">Updating groups</span></span>
<span data-ttu-id="ab529-137">tooupdate egy meglévő csoportot, használja a Set-AzureADGroup hello parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="ab529-137">tooupdate an existing group, use hello Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="ab529-138">Ebben a példában a Microsoft épp módosított hello DisplayName tulajdonságát hello csoport "Intune-rendszergazdák."</span><span class="sxs-lookup"><span data-stu-id="ab529-138">In this example, we’re changing hello DisplayName property of hello group “Intune Administrators.”</span></span> <span data-ttu-id="ab529-139">Először igazolnia fogjuk megtalálni hello csoport hello Get-AzureADGroup parancsmag és a szűrő hello DisplayName attribútum használatával:</span><span class="sxs-lookup"><span data-stu-id="ab529-139">First, we’re finding hello group using hello Get-AzureADGroup cmdlet and filter using hello DisplayName attribute:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

<span data-ttu-id="ab529-140">A következő azt épp módosított hello leírás toohello új tulajdonságérték "Intune eszköz-rendszergazdák":</span><span class="sxs-lookup"><span data-stu-id="ab529-140">Next, we’re changing hello Description property toohello new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="ab529-141">Most ismét találtunk hello csoport, ha látható hello Description tulajdonságot frissített tooreflect van hello új érték:</span><span class="sxs-lookup"><span data-stu-id="ab529-141">Now if we find hello group again, we see hello Description property is updated tooreflect hello new value:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a><span data-ttu-id="ab529-142">Csoportok törlése</span><span class="sxs-lookup"><span data-stu-id="ab529-142">Deleting groups</span></span>
<span data-ttu-id="ab529-143">a címtárban lévő toodelete csoportok parancsmaggal hello Remove-AzureADGroup az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="ab529-143">toodelete groups from your directory, use hello Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="ab529-144">A csoportok tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="ab529-144">Managing members of groups</span></span>
<span data-ttu-id="ab529-145">Ha tooadd új tagok tooa csoport van szüksége, használja az Add-AzureADGroupMember hello parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="ab529-145">If you need tooadd new members tooa group, use hello Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="ab529-146">Ez a parancs az előző példában hello használtuk tag toohello az Intune-rendszergazdák csoport hozzáadása:</span><span class="sxs-lookup"><span data-stu-id="ab529-146">This command adds a member toohello Intune Administrators group we used in hello previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ab529-147">hello - ObjectId paraméter hello ObjectID a hello csoport toowhich szeretnénk tooadd tagja, és hello - RefObjectId hello ObjectID hello felhasználó szeretnénk tooadd tag toohello csoportként.</span><span class="sxs-lookup"><span data-stu-id="ab529-147">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd a member, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as a member toohello group.</span></span>

<span data-ttu-id="ab529-148">tooget hello meglévő egy csoport tagjai, parancsmaggal hello Get-AzureADGroupMember, ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="ab529-148">tooget hello existing members of a group, use hello Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="ab529-149">tooremove hello tag korábban hozzáadott toohello csoport, használja a Remove-AzureADGroupMember hello parancsmagot, ahogy az itt látható:</span><span class="sxs-lookup"><span data-stu-id="ab529-149">tooremove hello member we previously added toohello group, use hello Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ab529-150">tooverify hello csoport membership(s) egy olyan felhasználó hello válassza-AzureADGroupIdsUserIsMemberOf parancsmag használható.</span><span class="sxs-lookup"><span data-stu-id="ab529-150">tooverify hello group membership(s) of a user, use hello Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="ab529-151">Ez a parancsmag paramétereit hello hello felhasználó mely toocheck hello csoporttagságok ObjectId tesz és mely toocheck hello tagságát csoportok listája.</span><span class="sxs-lookup"><span data-stu-id="ab529-151">This cmdlet takes as its parameters hello ObjectId of hello user for which toocheck hello group memberships, and a list of groups for which toocheck hello memberships.</span></span> <span data-ttu-id="ab529-152">csoportok listájának hello megadott formában hello "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" típusú összetett változó, úgy azt először hozzon létre egy változót ezzel a típussal kell lennie:</span><span class="sxs-lookup"><span data-stu-id="ab529-152">hello list of groups must be provided in hello form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="ab529-153">A következő általunk értékeket a hello csoportazonosítókat toocheck hello attribútum "Csoportazonosítókat" a komplex változó:</span><span class="sxs-lookup"><span data-stu-id="ab529-153">Next, we provide values for hello groupIds toocheck in hello attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="ab529-154">Mostantól Ha azt szeretné, hogy toocheck hello csoporttagságait elleni $g hello csoportok ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea rendelkező felhasználó, érhetjük el:</span><span class="sxs-lookup"><span data-stu-id="ab529-154">Now, if we want toocheck hello group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against hello groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="ab529-155">hello visszaadott érték, amelynek tagja a felhasználói csoportok listája.</span><span class="sxs-lookup"><span data-stu-id="ab529-155">hello value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="ab529-156">A metódus toocheck névjegyek, csoportok vagy Szolgáltatásnevekről tagsági csoportok használatával válassza AzureADGroupIdsContactIsMemberOf, válasszon-AzureADGroupIdsGroupIsMemberOf adott listáját is alkalmazhat vagy SELECT-AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="ab529-156">You can also apply this method toocheck Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="ab529-157">Csoport tulajdonosainak kezelése</span><span class="sxs-lookup"><span data-stu-id="ab529-157">Managing owners of groups</span></span>
<span data-ttu-id="ab529-158">tooadd tulajdonosok tooa csoport, használja a hello Add-AzureADGroupOwner parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="ab529-158">tooadd owners tooa group, use hello Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="ab529-159">hello - ObjectId paraméter hello ObjectID a hello csoport toowhich szeretnénk tooadd egy olyan tulajdonost, és hello - RefObjectId hello ObjectID hello felhasználó hello csoport tulajdonos szeretnénk tooadd.</span><span class="sxs-lookup"><span data-stu-id="ab529-159">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd an owner, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as an owner of hello group.</span></span>

<span data-ttu-id="ab529-160">egy csoport tulajdonosainak tooretrieve hello hello Get-AzureADGroupOwner parancsmagokat használhatja:</span><span class="sxs-lookup"><span data-stu-id="ab529-160">tooretrieve hello owners of a group, use hello Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="ab529-161">hello parancsmag hello megadott csoport tulajdonosainak hello listáját adja vissza:</span><span class="sxs-lookup"><span data-stu-id="ab529-161">hello cmdlet returns hello list of owners for hello specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="ab529-162">Ha azt szeretné, hogy tooremove tulajdonost egy csoportból, használja a hello Remove-AzureADGroupOwner parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="ab529-162">If you want tooremove an owner from a group, use hello Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="ab529-163">Fenntartott aliasok</span><span class="sxs-lookup"><span data-stu-id="ab529-163">Reserved aliases</span></span> 
<span data-ttu-id="ab529-164">Egy csoport jön létre, amikor bizonyos végpontokkal hello végfelhasználói toospecify egy mailNickname vagy alias toobe hello címre küldi hello csoport részeként.</span><span class="sxs-lookup"><span data-stu-id="ab529-164">When a group is created, certain endpoints allow hello end user toospecify a mailNickname or alias toobe used as part of hello email address of hello group.</span></span> <span data-ttu-id="ab529-165">A következő magas szintű jogosultsággal rendelkező e-mail-aliasok hello csoportok csak az Azure AD globális rendszergazda hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="ab529-165">Groups with hello following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="ab529-166">visszaélés</span><span class="sxs-lookup"><span data-stu-id="ab529-166">abuse</span></span> 
* <span data-ttu-id="ab529-167">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="ab529-167">admin</span></span> 
* <span data-ttu-id="ab529-168">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="ab529-168">administrator</span></span> 
* <span data-ttu-id="ab529-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="ab529-169">hostmaster</span></span> 
* <span data-ttu-id="ab529-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="ab529-170">majordomo</span></span> 
* <span data-ttu-id="ab529-171">postamester</span><span class="sxs-lookup"><span data-stu-id="ab529-171">postmaster</span></span> 
* <span data-ttu-id="ab529-172">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="ab529-172">root</span></span> 
* <span data-ttu-id="ab529-173">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="ab529-173">secure</span></span> 
* <span data-ttu-id="ab529-174">Biztonsági</span><span class="sxs-lookup"><span data-stu-id="ab529-174">security</span></span> 
* <span data-ttu-id="ab529-175">SSL-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="ab529-175">ssl-admin</span></span> 
* <span data-ttu-id="ab529-176">gazdáját</span><span class="sxs-lookup"><span data-stu-id="ab529-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ab529-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ab529-177">Next steps</span></span>
<span data-ttu-id="ab529-178">További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="ab529-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="ab529-179">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ab529-179">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="ab529-180">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="ab529-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
