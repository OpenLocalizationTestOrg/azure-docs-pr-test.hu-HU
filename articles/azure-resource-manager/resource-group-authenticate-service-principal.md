---
title: "az Azure PowerShell-alkalmazás identitását aaaCreate |} Microsoft Docs"
description: "Ismerteti, hogyan szabályozza a toouse Azure PowerShell toocreate egy Azure Active Directory-alkalmazás és szolgáltatás egyszerű, és azt keresztül férnek hozzá tooresources szerepkörön alapuló hozzáférés biztosítása. Azt illusztrálja, hogyan tooauthenticate alkalmazás jelszóval vagy tanúsítvánnyal."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: d2caf121-9fbe-4f00-bf9d-8f3d1f00a6ff
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/15/2017
ms.author: tomfitz
ms.openlocfilehash: c534360799b590054a051e4426e5e27dccb559b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toocreate-a-service-principal-tooaccess-resources"></a><span data-ttu-id="2fe8f-104">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate</span><span class="sxs-lookup"><span data-stu-id="2fe8f-104">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>

<span data-ttu-id="2fe8f-105">Ha egy alkalmazás vagy parancsfájlt, amelyet a tooaccess erőforrások, hello alkalmazás identitás beállítása, és hitelesíteni hello alkalmazást a saját hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-105">When you have an app or script that needs tooaccess resources, you can set up an identity for hello app and authenticate hello app with its own credentials.</span></span> <span data-ttu-id="2fe8f-106">Ezzel az identitással egyszerű szolgáltatás néven ismert.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-106">This identity is known as a service principal.</span></span> <span data-ttu-id="2fe8f-107">Ez a megközelítés lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-107">This approach enables you to:</span></span>

* <span data-ttu-id="2fe8f-108">Engedélyek hozzárendelése saját engedélyek eltérő toohello identitását.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-108">Assign permissions toohello app identity that are different than your own permissions.</span></span> <span data-ttu-id="2fe8f-109">Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-109">Typically, these permissions are restricted tooexactly what hello app needs toodo.</span></span>
* <span data-ttu-id="2fe8f-110">A tanúsítvány használata a hitelesítéshez, egy felügyelet nélküli parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="2fe8f-111">Ez a témakör bemutatja, hogyan toouse [Azure PowerShell](/powershell/azure/overview) tooset másolatot minden olyan alkalmazás toorun alatt a saját hitelesítő adatokat és -identitás van szüksége.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-111">This topic shows you how toouse [Azure PowerShell](/powershell/azure/overview) tooset up everything you need for an application toorun under its own credentials and identity.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="2fe8f-112">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="2fe8f-112">Required permissions</span></span>
<span data-ttu-id="2fe8f-113">toocomplete Ez a témakör megfelelő engedélyekkel kell rendelkeznie az Azure Active Directory és az Azure-előfizetésében is.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-113">toocomplete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="2fe8f-114">Pontosabban tudja toocreate hello Azure Active Directory az alkalmazásban, és hello szolgáltatás egyszerű tooa szerepkör hozzárendelése.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-114">Specifically, you must be able toocreate an app in hello Azure Active Directory, and assign hello service principal tooa role.</span></span> 

<span data-ttu-id="2fe8f-115">hello legegyszerűbb módja toocheck hello portálon keresztül van a fiók rendelkezik-e megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-115">hello easiest way toocheck whether your account has adequate permissions is through hello portal.</span></span> <span data-ttu-id="2fe8f-116">Lásd: [szükséges engedély ellenőrzése](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-116">See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="2fe8f-117">A következő lépésben tooa szakasz a hitelesítéséhez:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-117">Now, proceed tooa section for authenticating with:</span></span>

* [<span data-ttu-id="2fe8f-118">jelszó</span><span class="sxs-lookup"><span data-stu-id="2fe8f-118">password</span></span>](#create-service-principal-with-password)
* [<span data-ttu-id="2fe8f-119">önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="2fe8f-119">self-signed certificate</span></span>](#create-service-principal-with-self-signed-certificate)
* [<span data-ttu-id="2fe8f-120">a hitelesítésszolgáltató tanúsítványa</span><span class="sxs-lookup"><span data-stu-id="2fe8f-120">certificate from Certificate Authority</span></span>](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a><span data-ttu-id="2fe8f-121">PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="2fe8f-121">PowerShell commands</span></span>

<span data-ttu-id="2fe8f-122">egy egyszerű szolgáltatás tooset, használhatja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-122">tooset up a service principal, you use:</span></span>

| <span data-ttu-id="2fe8f-123">Parancs</span><span class="sxs-lookup"><span data-stu-id="2fe8f-123">Command</span></span> | <span data-ttu-id="2fe8f-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="2fe8f-124">Description</span></span> |
| ------- | ----------- | 
| [<span data-ttu-id="2fe8f-125">Új AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="2fe8f-125">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="2fe8f-126">Létrehoz egy Azure Active Directory szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="2fe8f-126">Creates an Azure Active Directory service principal</span></span> |
| [<span data-ttu-id="2fe8f-127">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="2fe8f-127">New-AzureRmRoleAssignment</span></span>](/powershell/module/azurerm.resources/new-azurermroleassignment) | <span data-ttu-id="2fe8f-128">Rendel hello RBAC szerepkör toohello megadott egyszerű megadva, a hello: a megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-128">Assigns hello specified RBAC role toohello specified principal, at hello specified scope.</span></span> |


## <a name="create-service-principal-with-password"></a><span data-ttu-id="2fe8f-129">Egyszerű szolgáltatásnév létrehozása jelszóval</span><span class="sxs-lookup"><span data-stu-id="2fe8f-129">Create service principal with password</span></span>

<span data-ttu-id="2fe8f-130">toocreate hello közreműködő szerepkört az előfizetéséhez, a szolgáltatásnevet használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-130">toocreate a service principal with hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="2fe8f-131">hello példa alvó állapotba kerül 20 másodperc tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-131">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-132">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."</span><span class="sxs-lookup"><span data-stu-id="2fe8f-132">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="2fe8f-133">hello következő parancsfájl lehetővé teszi toospecify eltérő hello alapértelmezett előfizetési hatókört, és az újrapróbálkozások hello szerepkör-hozzárendelés, ha a hiba akkor fordul elő:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-133">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs:</span></span>

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $Password
)

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 
 # Create Service Principal for hello AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="2fe8f-134">Néhány elemet toonote hello parancsfájl kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-134">A few items toonote about hello script:</span></span>

* <span data-ttu-id="2fe8f-135">toogrant hello identitás hozzáférés toohello alapértelmezett előfizetést, akkor nem kell tooprovide ResourceGroup vagy a SubscriptionId paraméterek.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-135">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="2fe8f-136">Adja meg a hello ResourceGroup paraméter csak akkor, ha azt szeretné, hogy toolimit hello hatókör hello szerepkör hozzárendelése tooa erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-136">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
*  <span data-ttu-id="2fe8f-137">Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-137">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="2fe8f-138">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-138">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="2fe8f-139">hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-139">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-140">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."</span><span class="sxs-lookup"><span data-stu-id="2fe8f-140">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="2fe8f-141">Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-141">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>


### <a name="provide-credentials-through-powershell"></a><span data-ttu-id="2fe8f-142">Adjon meg hitelesítő adatokat a PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="2fe8f-142">Provide credentials through PowerShell</span></span>
<span data-ttu-id="2fe8f-143">Most van szüksége a toolog hello alkalmazás tooperform műveletként.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-143">Now, you need toolog in as hello application tooperform operations.</span></span> <span data-ttu-id="2fe8f-144">Hello felhasználónévvel, használja a hello `ApplicationId` hello alkalmazáshoz létrehozott.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-144">For hello user name, use hello `ApplicationId` that you created for hello application.</span></span> <span data-ttu-id="2fe8f-145">Hello jelszót egy hello fiók létrehozásakor megadott hello használata.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-145">For hello password, use hello one you specified when creating hello account.</span></span> 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

<span data-ttu-id="2fe8f-146">hello Bérlőazonosító nincs különbözőnek számítanak, így közvetlenül a parancsfájlban beágyazása.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-146">hello tenant ID is not sensitive, so you can embed it directly in your script.</span></span> <span data-ttu-id="2fe8f-147">Ha tooretrieve hello Bérlőazonosító van szüksége, használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-147">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a><span data-ttu-id="2fe8f-148">Egyszerű szolgáltatásnév létrehozása önaláírt tanúsítvánnyal</span><span class="sxs-lookup"><span data-stu-id="2fe8f-148">Create service principal with self-signed certificate</span></span>

<span data-ttu-id="2fe8f-149">toocreate egy önaláírt tanúsítványt és hello közreműködő szerepkört az előfizetés szolgáltatásnevet használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-149">toocreate a service principal with a self-signed certificate and hello Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="2fe8f-150">hello példa alvó állapotba kerül 20 másodperc tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-150">hello example sleeps for 20 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-151">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."</span><span class="sxs-lookup"><span data-stu-id="2fe8f-151">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>

<span data-ttu-id="2fe8f-152">hello következő parancsfájl lehetővé teszi toospecify eltérő hello alapértelmezett előfizetési hatókört, és az újrapróbálkozások hello szerepkör-hozzárendelés, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-152">hello following script enables you toospecify a scope other than hello default subscription, and retries hello role assignment if an error occurs.</span></span> <span data-ttu-id="2fe8f-153">Azure PowerShell 2.0 a Windows 10 vagy Windows Server 2016 kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-153">You must have Azure PowerShell 2.0 on Windows 10 or Windows Server 2016.</span></span>

```powershell
Param (

 # Use tooset scope tooresource group. If no value is provided, scope is set toosubscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use tooset subscription. If no value is provided, default subscription is used. 
 [Parameter(Mandatory=$false)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources

 if ($SubscriptionId -eq "") 
 {
    $SubscriptionId = (Get-AzureRmContext).Subscription.Id
 }
 else
 {
    Set-AzureRmContext -SubscriptionId $SubscriptionId
 }

 if ($ResourceGroup -eq "")
 {
    $Scope = "/subscriptions/" + $SubscriptionId
 }
 else
 {
    $Scope = (Get-AzureRmResourceGroup -Name $ResourceGroup -ErrorAction Stop).ResourceId
 }

 $cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
 $keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="2fe8f-154">Néhány elemet toonote hello parancsfájl kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-154">A few items toonote about hello script:</span></span>

* <span data-ttu-id="2fe8f-155">toogrant hello identitás hozzáférés toohello alapértelmezett előfizetést, akkor nem kell tooprovide ResourceGroup vagy a SubscriptionId paraméterek.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-155">toogrant hello identity access toohello default subscription, you do not need tooprovide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="2fe8f-156">Adja meg a hello ResourceGroup paraméter csak akkor, ha azt szeretné, hogy toolimit hello hatókör hello szerepkör hozzárendelése tooa erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-156">Specify hello ResourceGroup parameter only when you want toolimit hello scope of hello role assignment tooa resource group.</span></span>
* <span data-ttu-id="2fe8f-157">Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-157">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="2fe8f-158">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-158">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="2fe8f-159">hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-159">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-160">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."</span><span class="sxs-lookup"><span data-stu-id="2fe8f-160">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="2fe8f-161">Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-161">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

<span data-ttu-id="2fe8f-162">Ha Ön **nem rendelkeznek Windows 10 vagy Windows Server 2016 Technical Preview**, toodownload hello kell [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-162">If you **do not have Windows 10 or Windows Server 2016 Technical Preview**, you need toodownload hello [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center.</span></span> <span data-ttu-id="2fe8f-163">Bontsa ki a tartalmát, és hello parancsmag kell importálni.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-163">Extract its contents and import hello cmdlet you need.</span></span>

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
<span data-ttu-id="2fe8f-164">Hello parancsfájlban helyettesítse be a következő két sorok toogenerate hello tanúsítvány hello.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-164">In hello script, substitute hello following two lines toogenerate hello certificate.</span></span>
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="2fe8f-165">Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével</span><span class="sxs-lookup"><span data-stu-id="2fe8f-165">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="2fe8f-166">Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-166">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="2fe8f-167">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-167">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-168">Ha több előfizetéssel rendelkezik, akkor használhatja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-168">If you only have one subscription, you can use:</span></span>

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertSubject,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $Thumbprint = (Get-ChildItem cert:\CurrentUser\My\ | Where-Object {$_.Subject -match $CertSubject }).Thumbprint
 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

<span data-ttu-id="2fe8f-169">hello alkalmazás és bérlő-azonosító nem különbség, így közvetlenül a parancsfájlban beágyazhatók.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-169">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="2fe8f-170">Ha tooretrieve hello Bérlőazonosító van szüksége, használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-170">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="2fe8f-171">Ha tooretrieve hello Alkalmazásazonosítót kell használni:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-171">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a><span data-ttu-id="2fe8f-172">Egyszerű szolgáltatásnév létrehozása hitelesítésszolgáltatótól származó tanúsítvánnyal</span><span class="sxs-lookup"><span data-stu-id="2fe8f-172">Create service principal with certificate from Certificate Authority</span></span>
<span data-ttu-id="2fe8f-173">egy tanúsítványt egy hitelesítésszolgáltatótól toocreate szolgáltatás egyszerű, a következő parancsfájl használata hello toouse:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-173">toouse a certificate issued from a Certificate Authority toocreate service principal, use hello following script:</span></span>

```powershell
Param (
 [Parameter(Mandatory=$true)]
 [String] $ApplicationDisplayName,

 [Parameter(Mandatory=$true)]
 [String] $SubscriptionId,

 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword
 )

 Login-AzureRmAccount
 Import-Module AzureRM.Resources
 Set-AzureRmContext -SubscriptionId $SubscriptionId

 $KeyId = (New-Guid).Guid
 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force

 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $KeyValue = [System.Convert]::ToBase64String($PFXCert.GetRawCertData())

 $KeyCredential = New-Object  Microsoft.Azure.Commands.Resources.Models.ActiveDirectory.PSADKeyCredential
 $KeyCredential.StartDate = $PFXCert.NotBefore
 $KeyCredential.EndDate= $PFXCert.NotAfter
 $KeyCredential.KeyId = $KeyId
 $KeyCredential.CertValue = $KeyValue

 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -KeyCredentials $keyCredential
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

<span data-ttu-id="2fe8f-174">Néhány elemet toonote hello parancsfájl kapcsolatban:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-174">A few items toonote about hello script:</span></span>

* <span data-ttu-id="2fe8f-175">Hozzáférési hatókörrel rendelkező toohello előfizetés.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-175">Access is scoped toohello subscription.</span></span>
* <span data-ttu-id="2fe8f-176">Ebben a példában hello szolgáltatás egyszerű toohello közreműködői szerepkör hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-176">In this example, you add hello service principal toohello Contributor role.</span></span> <span data-ttu-id="2fe8f-177">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-177">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="2fe8f-178">hello parancsfájl alvó állapotban marad, a 15 másodperces tooallow némi időbe, hello új szolgáltatás egyszerű toopropagate egész Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-178">hello script sleeps for 15 seconds tooallow some time for hello new service principal toopropagate throughout Azure Active Directory.</span></span> <span data-ttu-id="2fe8f-179">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} egyszerű hello könyvtárban nem létezik."</span><span class="sxs-lookup"><span data-stu-id="2fe8f-179">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in hello directory."</span></span>
* <span data-ttu-id="2fe8f-180">Ha toogrant hello szolgáltatás egyszerű hozzáférés toomore előfizetések vagy erőforráscsoportok van szüksége, futtassa a hello `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-180">If you need toogrant hello service principal access toomore subscriptions or resource groups, run hello `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="2fe8f-181">Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével</span><span class="sxs-lookup"><span data-stu-id="2fe8f-181">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="2fe8f-182">Amikor jelentkezik be, és egy egyszerű szolgáltatást, az AD-alkalmazás kell tooprovide hello bérlőazonosító hello könyvtár.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-182">Whenever you sign in as a service principal, you need tooprovide hello tenant id of hello directory for your AD app.</span></span> <span data-ttu-id="2fe8f-183">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-183">A tenant is an instance of Azure Active Directory.</span></span>

```powershell
Param (
 
 [Parameter(Mandatory=$true)]
 [String] $CertPath,

 [Parameter(Mandatory=$true)]
 [String] $CertPlainPassword,
 
 [Parameter(Mandatory=$true)]
 [String] $ApplicationId,

 [Parameter(Mandatory=$true)]
 [String] $TenantId
 )

 $CertPassword = ConvertTo-SecureString $CertPlainPassword -AsPlainText -Force
 $PFXCert = New-Object -TypeName System.Security.Cryptography.X509Certificates.X509Certificate2 -ArgumentList @($CertPath, $CertPassword)
 $Thumbprint = $PFXCert.Thumbprint

 Login-AzureRmAccount -ServicePrincipal -CertificateThumbprint $Thumbprint -ApplicationId $ApplicationId -TenantId $TenantId
```

<span data-ttu-id="2fe8f-184">hello alkalmazás és bérlő-azonosító nem különbség, így közvetlenül a parancsfájlban beágyazhatók.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-184">hello application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="2fe8f-185">Ha tooretrieve hello Bérlőazonosító van szüksége, használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-185">If you need tooretrieve hello tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="2fe8f-186">Ha tooretrieve hello Alkalmazásazonosítót kell használni:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-186">If you need tooretrieve hello application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a><span data-ttu-id="2fe8f-187">Hitelesítő adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="2fe8f-187">Change credentials</span></span>

<span data-ttu-id="2fe8f-188">AD-alkalmazás, vagy a biztonsági sérülés vagy a hitelesítő adatok érvényessége miatt toochange hello hitelesítő adatait használja hello [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) és [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-188">toochange hello credentials for an AD app, either because of a security compromise or a credential expiration, use hello [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span></span>

<span data-ttu-id="2fe8f-189">tooremove minden hello hitelesítő alkalmazás esetén használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-189">tooremove all hello credentials for an application, use:</span></span>

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

<span data-ttu-id="2fe8f-190">a jelszó tooadd használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-190">tooadd a password, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

<span data-ttu-id="2fe8f-191">tooadd tanúsítvány értéket, hozzon létre egy önaláírt tanúsítványt, ebben a témakörben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-191">tooadd a certificate value, create a self-signed certificate as shown in this topic.</span></span> <span data-ttu-id="2fe8f-192">Ezután használja:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-192">Then, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-toosimplify-log-in"></a><span data-ttu-id="2fe8f-193">Mentse a hozzáférési token toosimplify jelentkezzen be</span><span class="sxs-lookup"><span data-stu-id="2fe8f-193">Save access token toosimplify log in</span></span>
<span data-ttu-id="2fe8f-194">tooavoid hello szolgáltatás egyszerű hitelesítő adatok minden alkalommal, amikor a toolog szükséges, mentheti hello hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-194">tooavoid providing hello service principal credentials every time it needs toolog in, you can save hello access token.</span></span>

<span data-ttu-id="2fe8f-195">toouse hello aktuális hozzáférési jogkivonat egy újabb munkamenetben hello profilt menthet.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-195">toouse hello current access token in a later session, save hello profile.</span></span>
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
<span data-ttu-id="2fe8f-196">Nyissa meg a hello-profil, és vizsgálja meg annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-196">Open hello profile and examine its contents.</span></span> <span data-ttu-id="2fe8f-197">Figyelje meg, hogy az tartalmazza-e olyan hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-197">Notice that it contains an access token.</span></span> <span data-ttu-id="2fe8f-198">Manuálisan a bejelentkezés újra helyett egyszerűen hello-profil betöltése.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-198">Instead of manually logging in again, simply load hello profile.</span></span>
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> <span data-ttu-id="2fe8f-199">hello hozzáférési jogkivonat lejár, így csak a működik a mentett profil használatával mindaddig, amíg hello lexikális elem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-199">hello access token expires, so using a saved profile only works for as long as hello token is valid.</span></span>
>  

<span data-ttu-id="2fe8f-200">Azt is megteheti hívhat meg a PowerShell toolog REST műveletek.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-200">Alternatively, you can invoke REST operations from PowerShell toolog in.</span></span> <span data-ttu-id="2fe8f-201">Hello hitelesítési választ, a hozzáférési jogkivonat hello használható egyéb műveletek kérheti le.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-201">From hello authentication response, you can retrieve hello access token for use with other operations.</span></span> <span data-ttu-id="2fe8f-202">Például egy hello hozzáférési jogkivonat beolvasása REST műveleteinek figyelőn, lásd: [generálása egy hozzáférési jogkivonat](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-202">For an example of retrieving hello access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="debug"></a><span data-ttu-id="2fe8f-203">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="2fe8f-203">Debug</span></span>

<span data-ttu-id="2fe8f-204">Egy egyszerű szolgáltatás létrehozásakor a következő hibák hello merülhetnek fel:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-204">You may encounter hello following errors when creating a service principal:</span></span>

* <span data-ttu-id="2fe8f-205">**"Authentication_Unauthorized"** vagy **"előfizetést hello a környezetben található."**</span><span class="sxs-lookup"><span data-stu-id="2fe8f-205">**"Authentication_Unauthorized"** or **"No subscription found in hello context."**</span></span> <span data-ttu-id="2fe8f-206">– Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik hello [szükséges engedélyek](#required-permissions) a hello Azure Active Directory tooregister egy alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-206">- You see this error when your account does not have hello [required permissions](#required-permissions) on hello Azure Active Directory tooregister an app.</span></span> <span data-ttu-id="2fe8f-207">Általában ezt a hibát látva az Azure Active Directoryban csak rendszergazda felhasználók regisztrálhatják az alkalmazásokat, és a fiók nincs a rendszergazda segítségét. Kérje a rendszergazda tooeither rendelje hozzá, akkor tooan rendszergazdai szerepkör, illetve tooenable felhasználók tooregister alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-207">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin. Ask your administrator tooeither assign you tooan administrator role, or tooenable users tooregister apps.</span></span>

* <span data-ttu-id="2fe8f-208">A fiók **"Nincs engedély tooperform művelet"Microsoft.Authorization/roleAssignments/write"hatókörben"/Subscriptions/ {guid}"."**  -Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik elegendő engedélyekkel tooassign egy szerepkör tooan identitást.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-208">Your account **"does not have authorization tooperform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions tooassign a role tooan identity.</span></span> <span data-ttu-id="2fe8f-209">Kérje meg az előfizetés rendszergazdája tooadd meg tooUser hozzáférési rendszergazdai szerepkört.</span><span class="sxs-lookup"><span data-stu-id="2fe8f-209">Ask your subscription administrator tooadd you tooUser Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="2fe8f-210">Mintaalkalmazások</span><span class="sxs-lookup"><span data-stu-id="2fe8f-210">Sample applications</span></span>
<span data-ttu-id="2fe8f-211">Különböző platformokon keresztül hello alkalmazásként naplózás kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="2fe8f-211">For information about logging in as hello application through different platforms, see:</span></span>

* [<span data-ttu-id="2fe8f-212">.NET</span><span class="sxs-lookup"><span data-stu-id="2fe8f-212">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="2fe8f-213">Java</span><span class="sxs-lookup"><span data-stu-id="2fe8f-213">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="2fe8f-214">Node.js</span><span class="sxs-lookup"><span data-stu-id="2fe8f-214">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="2fe8f-215">Python</span><span class="sxs-lookup"><span data-stu-id="2fe8f-215">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="2fe8f-216">Ruby</span><span class="sxs-lookup"><span data-stu-id="2fe8f-216">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="2fe8f-217">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2fe8f-217">Next steps</span></span>
* <span data-ttu-id="2fe8f-218">Az alkalmazás integrálása az Azure erőforrások kezeléséhez részletes lépéseiért lásd: [– fejlesztői útmutató tooauthorization hello Azure Resource Manager API-val rendelkező](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-218">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide tooauthorization with hello Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="2fe8f-219">Egy részletes ismertetése az alkalmazások és szolgáltatásnevekről [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-219">For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span> 
* <span data-ttu-id="2fe8f-220">Azure Active Directory-hitelesítéssel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-220">For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span></span>
* <span data-ttu-id="2fe8f-221">Az elérhető műveleteket, lehet megadott vagy toousers megtagadva listájáért lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="2fe8f-221">For a list of available actions that can be granted or denied toousers, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>

