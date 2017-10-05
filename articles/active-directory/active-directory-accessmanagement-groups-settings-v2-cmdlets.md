---
title: "A csoportkezelés az Azure Active Directory PowerShell-példák |} Microsoft Docs"
description: "Ezen a lapon biztosít a PowerShell-példák segítséget nyújtanak az Azure Active Directoryban csoportok kezelése"
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
ms.openlocfilehash: a81820bc778c26f6e8051e2817ebd2b9c24b697a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="47283-104">Csoportok kezelése az Azure Active Directory 2-es parancsmagok</span><span class="sxs-lookup"><span data-stu-id="47283-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="47283-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="47283-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="47283-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="47283-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="47283-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="47283-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="47283-108">Ez a cikk tartalmaz példákat az Azure Active Directory (Azure AD) a csoportok kezelése a PowerShell segítségével.</span><span class="sxs-lookup"><span data-stu-id="47283-108">This article contains examples of how to use PowerShell to manage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="47283-109">Azt is bemutatja, hogyan rendszerrel az Azure AD PowerShell modult.</span><span class="sxs-lookup"><span data-stu-id="47283-109">It also tells you how to get set up with the Azure AD PowerShell module.</span></span> <span data-ttu-id="47283-110">Először meg kell [töltse le az Azure AD PowerShell modult](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="47283-110">First, you must [download the Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-the-azure-ad-powershell-module"></a><span data-ttu-id="47283-111">Az Azure AD PowerShell-modul telepítése</span><span class="sxs-lookup"><span data-stu-id="47283-111">Installing the Azure AD PowerShell module</span></span>
<span data-ttu-id="47283-112">Az Azure AD PowerShell-modul telepítéséhez használja a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="47283-112">To install the Azure AD PowerShell module, use the following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="47283-113">Győződjön meg arról, hogy a modul el lett-e telepítve, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="47283-113">To verify that the module was installed, use the following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="47283-114">Most elindíthatja a modulban a parancsmagok használatával.</span><span class="sxs-lookup"><span data-stu-id="47283-114">Now you can start using the cmdlets in the module.</span></span> <span data-ttu-id="47283-115">Az Azure AD modulban a parancsmagok teljes leírása, olvassa el az online dokumentációját [Azure Active Directory PowerShell 2-es verzió](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="47283-115">For a full description of the cmdlets in the Azure AD module, please refer to the online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-to-the-directory"></a><span data-ttu-id="47283-116">A címtár csatlakoztatása</span><span class="sxs-lookup"><span data-stu-id="47283-116">Connecting to the directory</span></span>
<span data-ttu-id="47283-117">Azure AD PowerShell-parancsmagok használatával csoportok kezelése elindításához, csatlakoznia kell a kezelni kívánt könyvtár a PowerShell-munkamenethez.</span><span class="sxs-lookup"><span data-stu-id="47283-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session to the directory you want to manage.</span></span> <span data-ttu-id="47283-118">Használja az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="47283-118">Use the following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="47283-119">A parancsmag felszólítja a könyvtár eléréséhez használni kívánt hitelesítő adataival.</span><span class="sxs-lookup"><span data-stu-id="47283-119">The cmdlet prompts you for the credentials you want to use to access your directory.</span></span> <span data-ttu-id="47283-120">A jelen példában használjuk karen@drumkit.onmicrosoft.com a telepítési könyvtár eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="47283-120">In this example, we are using karen@drumkit.onmicrosoft.com to access the demonstration directory.</span></span> <span data-ttu-id="47283-121">A parancsmag visszaadja a jóváhagyást a munkamenet sikeresen csatlakoztatva lett a címtárhoz megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="47283-121">The cmdlet returns a confirmation to show the session was connected successfully to your directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="47283-122">Most elindíthatja a AzureAD-parancsmagok használatával kezelheti a csoportok a címtárban.</span><span class="sxs-lookup"><span data-stu-id="47283-122">Now you can start using the AzureAD cmdlets to manage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="47283-123">Csoportok beolvasása</span><span class="sxs-lookup"><span data-stu-id="47283-123">Retrieving groups</span></span>
<span data-ttu-id="47283-124">A Get-AzureADGroups parancsmaggal a címtárban lévő meglévő csoportjainak lekérdezésére.</span><span class="sxs-lookup"><span data-stu-id="47283-124">To retrieve existing groups from your directory you can use the Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="47283-125">A címtár összes csoportjainak lekérdezésére, paraméterek nélkül a parancsmagot használja:</span><span class="sxs-lookup"><span data-stu-id="47283-125">To retrieve all groups in the directory, use the cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="47283-126">A parancsmag az összes csoport a csatlakoztatott címtárhoz adja vissza.</span><span class="sxs-lookup"><span data-stu-id="47283-126">The cmdlet returns all groups in the connected directory.</span></span>

<span data-ttu-id="47283-127">A - objectID paraméter segítségével egy adott csoport, amelynek meg a csoport objectID beolvasásához:</span><span class="sxs-lookup"><span data-stu-id="47283-127">You can use the -objectID parameter to retrieve a specific group for which you specify the group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="47283-128">A parancsmag most visszaadja a csoportot, amelynek objectID megegyezik a megadott paraméter értékét:</span><span class="sxs-lookup"><span data-stu-id="47283-128">The cmdlet now returns the group whose objectID matches the value of the parameter you entered:</span></span>

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

<span data-ttu-id="47283-129">Kereshet egy adott csoport használja a - szűrő paramétert.</span><span class="sxs-lookup"><span data-stu-id="47283-129">You can search for a specific group using the -filter parameter.</span></span> <span data-ttu-id="47283-130">Ez a paraméter egy ODATA szűrőfeltétel vesz igénybe, és minden csoport a szűrőnek, az alábbi példában látható módon adja vissza:</span><span class="sxs-lookup"><span data-stu-id="47283-130">This parameter takes an ODATA filter clause and returns all groups that match the filter, as in the following example:</span></span>

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
> <span data-ttu-id="47283-131">A AzureAD PowerShell-parancsmagok valósítja meg az OData-lekérdezés szabványnak.</span><span class="sxs-lookup"><span data-stu-id="47283-131">The AzureAD PowerShell cmdlets implement the OData query standard.</span></span> <span data-ttu-id="47283-132">További információkért lásd: **$filter** a [OData rendszer lekérdezési lehetőségek az OData-végpont használatával](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="47283-132">For more information, see **$filter** in [OData system query options using the OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="47283-133">Csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="47283-133">Creating groups</span></span>
<span data-ttu-id="47283-134">Új csoport létrehozása a könyvtárban, a New-AzureADGroup parancsmag használható.</span><span class="sxs-lookup"><span data-stu-id="47283-134">To create a new group in your directory, use the New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="47283-135">Ez a parancsmag "Marketing" nevű új biztonsági csoportot hoz létre:</span><span class="sxs-lookup"><span data-stu-id="47283-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="47283-136">Csoportok frissítése</span><span class="sxs-lookup"><span data-stu-id="47283-136">Updating groups</span></span>
<span data-ttu-id="47283-137">Meglévő csoport frissítéséhez használja a Set-AzureADGroup parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="47283-137">To update an existing group, use the Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="47283-138">Ebben a példában a Microsoft épp módosított a DisplayName tulajdonsága a következő csoporthoz: "Az Intune-rendszergazdák."</span><span class="sxs-lookup"><span data-stu-id="47283-138">In this example, we’re changing the DisplayName property of the group “Intune Administrators.”</span></span> <span data-ttu-id="47283-139">Először azt még keresése a csoport, a Get-AzureADGroup parancsmaggal, és a DisplayName attribútum szűrés:</span><span class="sxs-lookup"><span data-stu-id="47283-139">First, we’re finding the group using the Get-AzureADGroup cmdlet and filter using the DisplayName attribute:</span></span>

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

<span data-ttu-id="47283-140">Ezt követően a Microsoft épp módosított a Description tulajdonságot az "Intune eszköz-rendszergazdák" új érték:</span><span class="sxs-lookup"><span data-stu-id="47283-140">Next, we’re changing the Description property to the new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="47283-141">Most ismét találtunk a csoportot, ha az új érték a Description tulajdonságot frissíti a rendszer látható:</span><span class="sxs-lookup"><span data-stu-id="47283-141">Now if we find the group again, we see the Description property is updated to reflect the new value:</span></span>

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

## <a name="deleting-groups"></a><span data-ttu-id="47283-142">Csoportok törlése</span><span class="sxs-lookup"><span data-stu-id="47283-142">Deleting groups</span></span>
<span data-ttu-id="47283-143">A címtárban lévő csoport törléséhez használja a Remove-AzureADGroup parancsmag az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="47283-143">To delete groups from your directory, use the Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="47283-144">A csoportok tagjai kezelése</span><span class="sxs-lookup"><span data-stu-id="47283-144">Managing members of groups</span></span>
<span data-ttu-id="47283-145">Új tagok hozzáadása csoporthoz van szüksége, használja az Add-AzureADGroupMember parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="47283-145">If you need to add new members to a group, use the Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="47283-146">Ez a parancs hozzáadja a használtuk az előző példában Intune rendszergazdák csoport tagja:</span><span class="sxs-lookup"><span data-stu-id="47283-146">This command adds a member to the Intune Administrators group we used in the previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="47283-147">-ObjectId paraméter a ObjectID szeretnénk a tag hozzáadása a csoporthoz, és azt szeretné, hogy a csoportba tagként felvenni kívánt felhasználó ObjectID - RefObjectId.</span><span class="sxs-lookup"><span data-stu-id="47283-147">The -ObjectId parameter is the ObjectID of the group to which we want to add a member, and the -RefObjectId is the ObjectID of the user we want to add as a member to the group.</span></span>

<span data-ttu-id="47283-148">A meglévő egy csoport tagjai, amelyet a Get-AzureADGroupMember parancsmag, ebben a példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="47283-148">To get the existing members of a group, use the Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="47283-149">Távolítsa el a korábban hozzáadott a csoport tagja, használja a Remove-AzureADGroupMember parancsmag, az itt látható:</span><span class="sxs-lookup"><span data-stu-id="47283-149">To remove the member we previously added to the group, use the Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="47283-150">A felhasználói csoport membership(s), amellyel a Select-AzureADGroupIdsUserIsMemberOf parancsmag.</span><span class="sxs-lookup"><span data-stu-id="47283-150">To verify the group membership(s) of a user, use the Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="47283-151">Ez a parancsmag paramétereit, vesz igénybe, a ObjectId, amelyre vonatkozóan szeretné ellenőrizni a csoporttagságot a felhasználó és csoportok, amelyre vonatkozóan szeretné ellenőrizni a csoporttagságot listája.</span><span class="sxs-lookup"><span data-stu-id="47283-151">This cmdlet takes as its parameters the ObjectId of the user for which to check the group memberships, and a list of groups for which to check the memberships.</span></span> <span data-ttu-id="47283-152">A csoportok listáját megadott "Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck" típusú összetett változó formájában, így azt először hozzon létre egy változót ezzel a típussal kell lennie:</span><span class="sxs-lookup"><span data-stu-id="47283-152">The list of groups must be provided in the form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="47283-153">A következő nyújtunk a komplex változó a "csoportazonosítókat" attribútumban ellenőrzéséhez csoportazonosítókat értékeit:</span><span class="sxs-lookup"><span data-stu-id="47283-153">Next, we provide values for the groupIds to check in the attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="47283-154">Most elleni $g a csoportok ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea rendelkező felhasználó csoporttagságát ellenőrizni szeretnénk, ha érhetjük el:</span><span class="sxs-lookup"><span data-stu-id="47283-154">Now, if we want to check the group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against the groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="47283-155">A visszaadott érték, amelynek tagja a felhasználói csoportok listája.</span><span class="sxs-lookup"><span data-stu-id="47283-155">The value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="47283-156">A partnerek, csoportok vagy Szolgáltatásnevekről tagsági csoportok használatával válassza AzureADGroupIdsContactIsMemberOf, válasszon-AzureADGroupIdsGroupIsMemberOf megadott listája ellenőrzési módszert is alkalmazhat vagy SELECT-AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="47283-156">You can also apply this method to check Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="47283-157">Csoport tulajdonosainak kezelése</span><span class="sxs-lookup"><span data-stu-id="47283-157">Managing owners of groups</span></span>
<span data-ttu-id="47283-158">Tulajdonosok hozzáadása egy csoporthoz, az Add-AzureADGroupOwner parancsmaggal:</span><span class="sxs-lookup"><span data-stu-id="47283-158">To add owners to a group, use the Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="47283-159">A - ObjectId paraméter nem a csoport tulajdonosa hozzáadandó szeretnénk ObjectID, és - RefObjectId azt szeretnénk, ha szeretne hozzáadni a csoport tulajdonosa felhasználó ObjectID.</span><span class="sxs-lookup"><span data-stu-id="47283-159">The -ObjectId parameter is the ObjectID of the group to which we want to add an owner, and the -RefObjectId is the ObjectID of the user we want to add as an owner of the group.</span></span>

<span data-ttu-id="47283-160">Egy csoport tulajdonosainak lekéréséhez használja a Get-AzureADGroupOwner parancsmagot:</span><span class="sxs-lookup"><span data-stu-id="47283-160">To retrieve the owners of a group, use the Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="47283-161">A parancsmag visszaadja a megadott csoport tulajdonosok listája:</span><span class="sxs-lookup"><span data-stu-id="47283-161">The cmdlet returns the list of owners for the specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="47283-162">Ha szeretne egy olyan tulajdonost eltávolítása egy csoportból, használja a Remove-AzureADGroupOwner parancsmag:</span><span class="sxs-lookup"><span data-stu-id="47283-162">If you want to remove an owner from a group, use the Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="47283-163">Fenntartott aliasok</span><span class="sxs-lookup"><span data-stu-id="47283-163">Reserved aliases</span></span> 
<span data-ttu-id="47283-164">A csoport létrehozásakor egyes végpontokkal, a végfelhasználó számára adjon meg egy mailNickname vagy alias használata az e-mail cím, a csoport részeként.</span><span class="sxs-lookup"><span data-stu-id="47283-164">When a group is created, certain endpoints allow the end user to specify a mailNickname or alias to be used as part of the email address of the group.</span></span> <span data-ttu-id="47283-165">A következő magas szintű jogosultsággal rendelkező e-mail-aliasokkal használt csoportok csak az Azure AD globális rendszergazda hozhatók létre.</span><span class="sxs-lookup"><span data-stu-id="47283-165">Groups with the following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="47283-166">visszaélés</span><span class="sxs-lookup"><span data-stu-id="47283-166">abuse</span></span> 
* <span data-ttu-id="47283-167">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="47283-167">admin</span></span> 
* <span data-ttu-id="47283-168">Rendszergazda</span><span class="sxs-lookup"><span data-stu-id="47283-168">administrator</span></span> 
* <span data-ttu-id="47283-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="47283-169">hostmaster</span></span> 
* <span data-ttu-id="47283-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="47283-170">majordomo</span></span> 
* <span data-ttu-id="47283-171">postamester</span><span class="sxs-lookup"><span data-stu-id="47283-171">postmaster</span></span> 
* <span data-ttu-id="47283-172">legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="47283-172">root</span></span> 
* <span data-ttu-id="47283-173">Biztonságos</span><span class="sxs-lookup"><span data-stu-id="47283-173">secure</span></span> 
* <span data-ttu-id="47283-174">Biztonsági</span><span class="sxs-lookup"><span data-stu-id="47283-174">security</span></span> 
* <span data-ttu-id="47283-175">SSL-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="47283-175">ssl-admin</span></span> 
* <span data-ttu-id="47283-176">gazdáját</span><span class="sxs-lookup"><span data-stu-id="47283-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="47283-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="47283-177">Next steps</span></span>
<span data-ttu-id="47283-178">További Azure Active Directory PowerShell dokumentációját a található [Azure Active Directory-modul parancsmagokkal](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="47283-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="47283-179">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="47283-179">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="47283-180">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="47283-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
