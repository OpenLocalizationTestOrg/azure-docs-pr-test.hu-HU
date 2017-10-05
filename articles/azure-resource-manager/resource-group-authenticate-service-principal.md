---
title: "Azure-alkalmazás identitását létrehozása a PowerShell használatával |} Microsoft Docs"
description: "Ismerteti, hogyan hozzon létre egy Azure Active Directory-alkalmazást és egy egyszerű szolgáltatást, és a szerepköralapú hozzáférés-vezérléssel erőforrásokhoz való hozzáférés engedélyezése az Azure PowerShell használatával. Azt mutatja, hogyan alkalmazás jelszóval vagy tanúsítvánnyal hitelesítheti."
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
ms.openlocfilehash: 55e83b0742652abbb42100a11a468bc13a7a8aed
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="use-azure-powershell-to-create-a-service-principal-to-access-resources"></a><span data-ttu-id="420a4-104">Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="420a4-104">Use Azure PowerShell to create a service principal to access resources</span></span>

<span data-ttu-id="420a4-105">Ha egy alkalmazás vagy parancsfájlt, amely erőforrások hozzáférésre van szüksége, állítsa be az alkalmazás identitást, és hitelesítsék az alkalmazást a saját hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="420a4-105">When you have an app or script that needs to access resources, you can set up an identity for the app and authenticate the app with its own credentials.</span></span> <span data-ttu-id="420a4-106">Ezzel az identitással egyszerű szolgáltatás néven ismert.</span><span class="sxs-lookup"><span data-stu-id="420a4-106">This identity is known as a service principal.</span></span> <span data-ttu-id="420a4-107">Ez a megközelítés lehetővé teszi:</span><span class="sxs-lookup"><span data-stu-id="420a4-107">This approach enables you to:</span></span>

* <span data-ttu-id="420a4-108">Engedélyek hozzárendelése saját engedélyek eltérő alkalmazás identitását.</span><span class="sxs-lookup"><span data-stu-id="420a4-108">Assign permissions to the app identity that are different than your own permissions.</span></span> <span data-ttu-id="420a4-109">Ezek az engedélyek általában korlátozódik, hogy mit az alkalmazás kell tennie.</span><span class="sxs-lookup"><span data-stu-id="420a4-109">Typically, these permissions are restricted to exactly what the app needs to do.</span></span>
* <span data-ttu-id="420a4-110">A tanúsítvány használata a hitelesítéshez, egy felügyelet nélküli parancsfájl végrehajtása közben.</span><span class="sxs-lookup"><span data-stu-id="420a4-110">Use a certificate for authentication when executing an unattended script.</span></span>

<span data-ttu-id="420a4-111">Ez a témakör bemutatja, hogyan használható [Azure PowerShell](/powershell/azure/overview) mindent, amire szüksége, az alkalmazás futtatásához a saját hitelesítő adatait és identitás beállítása.</span><span class="sxs-lookup"><span data-stu-id="420a4-111">This topic shows you how to use [Azure PowerShell](/powershell/azure/overview) to set up everything you need for an application to run under its own credentials and identity.</span></span>

## <a name="required-permissions"></a><span data-ttu-id="420a4-112">Szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="420a4-112">Required permissions</span></span>
<span data-ttu-id="420a4-113">Ez a témakör befejezéséhez megfelelő engedélyekkel kell rendelkeznie az Azure Active Directory és az Azure-előfizetésében is.</span><span class="sxs-lookup"><span data-stu-id="420a4-113">To complete this topic, you must have sufficient permissions in both your Azure Active Directory and your Azure subscription.</span></span> <span data-ttu-id="420a4-114">Pontosabban kell lennie az Azure Active Directory-alkalmazás létrehozása, és a szolgáltatás egyszerű hozzárendelése egy szerepkörhöz.</span><span class="sxs-lookup"><span data-stu-id="420a4-114">Specifically, you must be able to create an app in the Azure Active Directory, and assign the service principal to a role.</span></span> 

<span data-ttu-id="420a4-115">A legegyszerűbben a portálon ellenőrizheti, hogy rendelkezik-e megfelelő jogosultságokkal.</span><span class="sxs-lookup"><span data-stu-id="420a4-115">The easiest way to check whether your account has adequate permissions is through the portal.</span></span> <span data-ttu-id="420a4-116">Lásd: [szükséges engedély ellenőrzése](resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="420a4-116">See [Check required permission](resource-group-create-service-principal-portal.md#required-permissions).</span></span>

<span data-ttu-id="420a4-117">Most folytassa a szakasz a hitelesítéséhez:</span><span class="sxs-lookup"><span data-stu-id="420a4-117">Now, proceed to a section for authenticating with:</span></span>

* [<span data-ttu-id="420a4-118">jelszó</span><span class="sxs-lookup"><span data-stu-id="420a4-118">password</span></span>](#create-service-principal-with-password)
* [<span data-ttu-id="420a4-119">önaláírt tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="420a4-119">self-signed certificate</span></span>](#create-service-principal-with-self-signed-certificate)
* [<span data-ttu-id="420a4-120">a hitelesítésszolgáltató tanúsítványa</span><span class="sxs-lookup"><span data-stu-id="420a4-120">certificate from Certificate Authority</span></span>](#create-service-principal-with-certificate-from-certificate-authority)

## <a name="powershell-commands"></a><span data-ttu-id="420a4-121">PowerShell-parancsok</span><span class="sxs-lookup"><span data-stu-id="420a4-121">PowerShell commands</span></span>

<span data-ttu-id="420a4-122">Egy egyszerű beállításához használhatja:</span><span class="sxs-lookup"><span data-stu-id="420a4-122">To set up a service principal, you use:</span></span>

| <span data-ttu-id="420a4-123">Parancs</span><span class="sxs-lookup"><span data-stu-id="420a4-123">Command</span></span> | <span data-ttu-id="420a4-124">Leírás</span><span class="sxs-lookup"><span data-stu-id="420a4-124">Description</span></span> |
| ------- | ----------- | 
| [<span data-ttu-id="420a4-125">Új AzureRmADServicePrincipal</span><span class="sxs-lookup"><span data-stu-id="420a4-125">New-AzureRmADServicePrincipal</span></span>](/powershell/module/azurerm.resources/new-azurermadserviceprincipal) | <span data-ttu-id="420a4-126">Létrehoz egy Azure Active Directory szolgáltatás egyszerű</span><span class="sxs-lookup"><span data-stu-id="420a4-126">Creates an Azure Active Directory service principal</span></span> |
| [<span data-ttu-id="420a4-127">New-AzureRmRoleAssignment</span><span class="sxs-lookup"><span data-stu-id="420a4-127">New-AzureRmRoleAssignment</span></span>](/powershell/module/azurerm.resources/new-azurermroleassignment) | <span data-ttu-id="420a4-128">A megadott RBAC szerepkört rendel hozzá a megadott rendszerbiztonsági tag, a megadott hatókörben.</span><span class="sxs-lookup"><span data-stu-id="420a4-128">Assigns the specified RBAC role to the specified principal, at the specified scope.</span></span> |


## <a name="create-service-principal-with-password"></a><span data-ttu-id="420a4-129">Egyszerű szolgáltatásnév létrehozása jelszóval</span><span class="sxs-lookup"><span data-stu-id="420a4-129">Create service principal with password</span></span>

<span data-ttu-id="420a4-130">Hozzon létre egy egyszerű szolgáltatást a közreműködő szerepkört az előfizetés, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-130">To create a service principal with the Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$sp = New-AzureRmADServicePrincipal -DisplayName exampleapp -Password "{provide-password}"
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="420a4-131">A példa alvó állapotban van, várja meg, hogy az új szolgáltatáshoz Azure Active Directory teljes propagálása egyszerű 20 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="420a4-131">The example sleeps for 20 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="420a4-132">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} rendszerbiztonsági tag nem létezik a címtárban."</span><span class="sxs-lookup"><span data-stu-id="420a4-132">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>

<span data-ttu-id="420a4-133">A következő parancsfájl lehetővé teszi az alapértelmezett előfizetés nem határoz meg, és a szerepkör-hozzárendelés újrapróbálkozik, ha a hiba akkor fordul elő:</span><span class="sxs-lookup"><span data-stu-id="420a4-133">The following script enables you to specify a scope other than the default subscription, and retries the role assignment if an error occurs:</span></span>

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
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

 
 # Create Service Principal for the AD app
 $ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password
 Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 

 $NewRole = $null
 $Retries = 0;
 While ($NewRole -eq $null -and $Retries -le 6)
 {
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="420a4-134">Néhány elemet a parancsfájllal kapcsolatos figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="420a4-134">A few items to note about the script:</span></span>

* <span data-ttu-id="420a4-135">Az identitás hozzáférést biztosít az alapértelmezett előfizetés, nem kell megadnia a ResourceGroup vagy a SubscriptionId paraméterek.</span><span class="sxs-lookup"><span data-stu-id="420a4-135">To grant the identity access to the default subscription, you do not need to provide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="420a4-136">Itt adhatja meg, a ResourceGroup paraméter csak korlátozhatja azon szerepkör-hozzárendelés erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="420a4-136">Specify the ResourceGroup parameter only when you want to limit the scope of the role assignment to a resource group.</span></span>
*  <span data-ttu-id="420a4-137">Ebben a példában hozzáadja a közreműködő szerepkört az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="420a4-137">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="420a4-138">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-138">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="420a4-139">A parancsfájl alvó állapotban van, várja meg, hogy az új szolgáltatáshoz Azure Active Directory teljes propagálása egyszerű 15 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="420a4-139">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="420a4-140">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} rendszerbiztonsági tag nem létezik a címtárban."</span><span class="sxs-lookup"><span data-stu-id="420a4-140">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="420a4-141">Ha a szolgáltatás egyszerű hozzáférést biztosít a további előfizetések vagy erőforráscsoportok kell futtatni a `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="420a4-141">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>


### <a name="provide-credentials-through-powershell"></a><span data-ttu-id="420a4-142">Adjon meg hitelesítő adatokat a PowerShell segítségével</span><span class="sxs-lookup"><span data-stu-id="420a4-142">Provide credentials through PowerShell</span></span>
<span data-ttu-id="420a4-143">Most kell jelentkezzen be az alkalmazás műveletek végrehajtásához.</span><span class="sxs-lookup"><span data-stu-id="420a4-143">Now, you need to log in as the application to perform operations.</span></span> <span data-ttu-id="420a4-144">A felhasználó nevét, használja a `ApplicationId` , amelyet az alkalmazás hozott létre.</span><span class="sxs-lookup"><span data-stu-id="420a4-144">For the user name, use the `ApplicationId` that you created for the application.</span></span> <span data-ttu-id="420a4-145">A jelszót a fiók létrehozásakor megadott használja.</span><span class="sxs-lookup"><span data-stu-id="420a4-145">For the password, use the one you specified when creating the account.</span></span> 

```powershell   
$creds = Get-Credential
Login-AzureRmAccount -Credential $creds -ServicePrincipal -TenantId {tenant-id}
```

<span data-ttu-id="420a4-146">A bérlő azonosítója nincs különbözőnek számítanak, így közvetlenül a parancsfájlban beágyazása.</span><span class="sxs-lookup"><span data-stu-id="420a4-146">The tenant ID is not sensitive, so you can embed it directly in your script.</span></span> <span data-ttu-id="420a4-147">Ha szeretné beolvasni a bérlő Azonosítóját, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-147">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

## <a name="create-service-principal-with-self-signed-certificate"></a><span data-ttu-id="420a4-148">Egyszerű szolgáltatásnév létrehozása önaláírt tanúsítvánnyal</span><span class="sxs-lookup"><span data-stu-id="420a4-148">Create service principal with self-signed certificate</span></span>

<span data-ttu-id="420a4-149">Egy szolgáltatásnevet létrehozni az önaláírt tanúsítványt és a közreműködő szerepkört az előfizetés, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-149">To create a service principal with a self-signed certificate and the Contributor role for your subscription, use:</span></span> 

```powershell
Login-AzureRmAccount
$cert = New-SelfSignedCertificate -CertStoreLocation "cert:\CurrentUser\My" -Subject "CN=exampleappScriptCert" -KeySpec KeyExchange
$keyValue = [System.Convert]::ToBase64String($cert.GetRawCertData())

$sp = New-AzureRMADServicePrincipal -DisplayName exampleapp -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
Sleep 20
New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $sp.ApplicationId
```

<span data-ttu-id="420a4-150">A példa alvó állapotban van, várja meg, hogy az új szolgáltatáshoz Azure Active Directory teljes propagálása egyszerű 20 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="420a4-150">The example sleeps for 20 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="420a4-151">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} rendszerbiztonsági tag nem létezik a címtárban."</span><span class="sxs-lookup"><span data-stu-id="420a4-151">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>

<span data-ttu-id="420a4-152">A következő parancsfájl lehetővé teszi az alapértelmezett előfizetés nem határoz meg, és a szerepkör-hozzárendelés újrapróbálkozik, ha a hiba akkor fordul elő.</span><span class="sxs-lookup"><span data-stu-id="420a4-152">The following script enables you to specify a scope other than the default subscription, and retries the role assignment if an error occurs.</span></span> <span data-ttu-id="420a4-153">Azure PowerShell 2.0 a Windows 10 vagy Windows Server 2016 kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="420a4-153">You must have Azure PowerShell 2.0 on Windows 10 or Windows Server 2016.</span></span>

```powershell
Param (

 # Use to set scope to resource group. If no value is provided, scope is set to subscription.
 [Parameter(Mandatory=$false)]
 [String] $ResourceGroup,

 # Use to set subscription. If no value is provided, default subscription is used. 
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
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
```

<span data-ttu-id="420a4-154">Néhány elemet a parancsfájllal kapcsolatos figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="420a4-154">A few items to note about the script:</span></span>

* <span data-ttu-id="420a4-155">Az identitás hozzáférést biztosít az alapértelmezett előfizetés, nem kell megadnia a ResourceGroup vagy a SubscriptionId paraméterek.</span><span class="sxs-lookup"><span data-stu-id="420a4-155">To grant the identity access to the default subscription, you do not need to provide either ResourceGroup or SubscriptionId parameters.</span></span>
* <span data-ttu-id="420a4-156">Itt adhatja meg, a ResourceGroup paraméter csak korlátozhatja azon szerepkör-hozzárendelés erőforráscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="420a4-156">Specify the ResourceGroup parameter only when you want to limit the scope of the role assignment to a resource group.</span></span>
* <span data-ttu-id="420a4-157">Ebben a példában hozzáadja a közreműködő szerepkört az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="420a4-157">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="420a4-158">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-158">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="420a4-159">A parancsfájl alvó állapotban van, várja meg, hogy az új szolgáltatáshoz Azure Active Directory teljes propagálása egyszerű 15 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="420a4-159">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="420a4-160">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} rendszerbiztonsági tag nem létezik a címtárban."</span><span class="sxs-lookup"><span data-stu-id="420a4-160">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="420a4-161">Ha a szolgáltatás egyszerű hozzáférést biztosít a további előfizetések vagy erőforráscsoportok kell futtatni a `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="420a4-161">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

<span data-ttu-id="420a4-162">Ha Ön **nem rendelkeznek Windows 10 vagy Windows Server 2016 Technical Preview**, le kell töltenie a [önaláírt tanúsítvány generátor](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) Microsoft Script Center.</span><span class="sxs-lookup"><span data-stu-id="420a4-162">If you **do not have Windows 10 or Windows Server 2016 Technical Preview**, you need to download the [Self-signed certificate generator](https://gallery.technet.microsoft.com/scriptcenter/Self-signed-certificate-5920a7c6/) from Microsoft Script Center.</span></span> <span data-ttu-id="420a4-163">Bontsa ki a tartalmát, és importálni kell a parancsmagot.</span><span class="sxs-lookup"><span data-stu-id="420a4-163">Extract its contents and import the cmdlet you need.</span></span>

```powershell  
# Only run if you could not use New-SelfSignedCertificate
Import-Module -Name c:\ExtractedModule\New-SelfSignedCertificateEx.ps1
```
  
<span data-ttu-id="420a4-164">A parancsfájl helyettesítse be a tanúsítvány előállításához a következő két sorral.</span><span class="sxs-lookup"><span data-stu-id="420a4-164">In the script, substitute the following two lines to generate the certificate.</span></span>
  
```powershell
New-SelfSignedCertificateEx  -StoreLocation CurrentUser -StoreName My -Subject "CN=exampleapp" -KeySpec "Exchange" -FriendlyName "exampleapp"
$cert = Get-ChildItem -path Cert:\CurrentUser\my | where {$PSitem.Subject -eq 'CN=exampleapp' }
```

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="420a4-165">Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével</span><span class="sxs-lookup"><span data-stu-id="420a4-165">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="420a4-166">Amikor jelentkezik be, és egy egyszerű szolgáltatást, meg kell adnia annak a könyvtárnak a bérlő azonosítója az AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="420a4-166">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="420a4-167">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="420a4-167">A tenant is an instance of Azure Active Directory.</span></span> <span data-ttu-id="420a4-168">Ha több előfizetéssel rendelkezik, akkor használhatja:</span><span class="sxs-lookup"><span data-stu-id="420a4-168">If you only have one subscription, you can use:</span></span>

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

<span data-ttu-id="420a4-169">Az alkalmazás azonosítója és a bérlő azonosítója nem különbség, így közvetlenül a parancsfájlban beágyazhatók.</span><span class="sxs-lookup"><span data-stu-id="420a4-169">The application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="420a4-170">Ha szeretné beolvasni a bérlő Azonosítóját, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-170">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="420a4-171">Ha szeretné beolvasni az alkalmazás Azonosítóját, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-171">If you need to retrieve the application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="create-service-principal-with-certificate-from-certificate-authority"></a><span data-ttu-id="420a4-172">Egyszerű szolgáltatásnév létrehozása hitelesítésszolgáltatótól származó tanúsítvánnyal</span><span class="sxs-lookup"><span data-stu-id="420a4-172">Create service principal with certificate from Certificate Authority</span></span>
<span data-ttu-id="420a4-173">Egy hitelesítésszolgáltató által kiadott tanúsítvánnyal történő egyszerű szolgáltatásnév létrehozása, használja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="420a4-173">To use a certificate issued from a Certificate Authority to create service principal, use the following script:</span></span>

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
    # Sleep here for a few seconds to allow the service principal application to become active (should only take a couple of seconds normally)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
 }
 
 $NewRole
```

<span data-ttu-id="420a4-174">Néhány elemet a parancsfájllal kapcsolatos figyelembe venni:</span><span class="sxs-lookup"><span data-stu-id="420a4-174">A few items to note about the script:</span></span>

* <span data-ttu-id="420a4-175">Az előfizetés hozzáférés hatókörét.</span><span class="sxs-lookup"><span data-stu-id="420a4-175">Access is scoped to the subscription.</span></span>
* <span data-ttu-id="420a4-176">Ebben a példában hozzáadja a közreműködő szerepkört az egyszerű szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="420a4-176">In this example, you add the service principal to the Contributor role.</span></span> <span data-ttu-id="420a4-177">Más szerepköreivel kapcsolatban, tekintse meg a [RBAC: beépített szerepkörök](../active-directory/role-based-access-built-in-roles.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-177">For other roles, see [RBAC: Built-in roles](../active-directory/role-based-access-built-in-roles.md).</span></span>
* <span data-ttu-id="420a4-178">A parancsfájl alvó állapotban van, várja meg, hogy az új szolgáltatáshoz Azure Active Directory teljes propagálása egyszerű 15 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="420a4-178">The script sleeps for 15 seconds to allow some time for the new service principal to propagate throughout Azure Active Directory.</span></span> <span data-ttu-id="420a4-179">Ha a parancsfájl nem elegendő ideig kell várni, megjelenik egy üzenet szerint: "PrincipalNotFound: {azonosítójú} rendszerbiztonsági tag nem létezik a címtárban."</span><span class="sxs-lookup"><span data-stu-id="420a4-179">If your script does not wait long enough, you see an error stating: "PrincipalNotFound: Principal {id} does not exist in the directory."</span></span>
* <span data-ttu-id="420a4-180">Ha a szolgáltatás egyszerű hozzáférést biztosít a további előfizetések vagy erőforráscsoportok kell futtatni a `New-AzureRMRoleAssignment` parancsmagot újra különböző hatóköröket.</span><span class="sxs-lookup"><span data-stu-id="420a4-180">If you need to grant the service principal access to more subscriptions or resource groups, run the `New-AzureRMRoleAssignment` cmdlet again with different scopes.</span></span>

### <a name="provide-certificate-through-automated-powershell-script"></a><span data-ttu-id="420a4-181">Adja meg a tanúsítvány automatikus PowerShell-parancsfájl segítségével</span><span class="sxs-lookup"><span data-stu-id="420a4-181">Provide certificate through automated PowerShell script</span></span>
<span data-ttu-id="420a4-182">Amikor jelentkezik be, és egy egyszerű szolgáltatást, meg kell adnia annak a könyvtárnak a bérlő azonosítója az AD-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="420a4-182">Whenever you sign in as a service principal, you need to provide the tenant id of the directory for your AD app.</span></span> <span data-ttu-id="420a4-183">A bérlő az Azure Active Directory példánya.</span><span class="sxs-lookup"><span data-stu-id="420a4-183">A tenant is an instance of Azure Active Directory.</span></span>

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

<span data-ttu-id="420a4-184">Az alkalmazás azonosítója és a bérlő azonosítója nem különbség, így közvetlenül a parancsfájlban beágyazhatók.</span><span class="sxs-lookup"><span data-stu-id="420a4-184">The application ID and tenant ID are not sensitive, so you can embed them directly in your script.</span></span> <span data-ttu-id="420a4-185">Ha szeretné beolvasni a bérlő Azonosítóját, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-185">If you need to retrieve the tenant ID, use:</span></span>

```powershell
(Get-AzureRmSubscription -SubscriptionName "Contoso Default").TenantId
```

<span data-ttu-id="420a4-186">Ha szeretné beolvasni az alkalmazás Azonosítóját, használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-186">If you need to retrieve the application ID, use:</span></span>

```powershell
(Get-AzureRmADApplication -DisplayNameStartWith {display-name}).ApplicationId
```

## <a name="change-credentials"></a><span data-ttu-id="420a4-187">Hitelesítő adatok módosítása</span><span class="sxs-lookup"><span data-stu-id="420a4-187">Change credentials</span></span>

<span data-ttu-id="420a4-188">Az AD-alkalmazás, vagy a biztonsági sérülés vagy a hitelesítő adatok érvényessége miatt a hitelesítő adatok módosításához használja a [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) és [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="420a4-188">To change the credentials for an AD app, either because of a security compromise or a credential expiration, use the [Remove-AzureRmADAppCredential](/powershell/resourcemanager/azurerm.resources/v3.3.0/remove-azurermadappcredential) and [New-AzureRmADAppCredential](/powershell/module/azurerm.resources/new-azurermadappcredential) cmdlets.</span></span>

<span data-ttu-id="420a4-189">Az alkalmazás hitelesítő adatok eltávolításához használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-189">To remove all the credentials for an application, use:</span></span>

```powershell
Remove-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -All
```

<span data-ttu-id="420a4-190">A jelszó hozzáadásához használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-190">To add a password, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -Password p@ssword!
```

<span data-ttu-id="420a4-191">Tanúsítvány érték hozzáadása, hozzon létre egy önaláírt tanúsítványt, ebben a témakörben ismertetett módon.</span><span class="sxs-lookup"><span data-stu-id="420a4-191">To add a certificate value, create a self-signed certificate as shown in this topic.</span></span> <span data-ttu-id="420a4-192">Ezután használja:</span><span class="sxs-lookup"><span data-stu-id="420a4-192">Then, use:</span></span>

```powershell
New-AzureRmADAppCredential -ApplicationId 8bc80782-a916-47c8-a47e-4d76ed755275 -CertValue $keyValue -EndDate $cert.NotAfter -StartDate $cert.NotBefore
```

## <a name="save-access-token-to-simplify-log-in"></a><span data-ttu-id="420a4-193">Egyszerűbbé teheti a bejelentkezést a hozzáférési token mentése</span><span class="sxs-lookup"><span data-stu-id="420a4-193">Save access token to simplify log in</span></span>
<span data-ttu-id="420a4-194">Adja meg a szolgáltatás egyszerű hitelesítő adatokat, minden alkalommal, amikor jogcímadatokat kell-e jelentkezni elkerüléséhez mentheti a hozzáférési jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="420a4-194">To avoid providing the service principal credentials every time it needs to log in, you can save the access token.</span></span>

<span data-ttu-id="420a4-195">Az aktuális jogkivonat használni egy későbbi munkamenetben, mentse a profilt.</span><span class="sxs-lookup"><span data-stu-id="420a4-195">To use the current access token in a later session, save the profile.</span></span>
   
```powershell
Save-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```
   
<span data-ttu-id="420a4-196">Nyissa meg a profilt, és vizsgálja meg annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="420a4-196">Open the profile and examine its contents.</span></span> <span data-ttu-id="420a4-197">Figyelje meg, hogy az tartalmazza-e olyan hozzáférési jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="420a4-197">Notice that it contains an access token.</span></span> <span data-ttu-id="420a4-198">Manuálisan a bejelentkezés újra helyett egyszerűen betölteni a profilt.</span><span class="sxs-lookup"><span data-stu-id="420a4-198">Instead of manually logging in again, simply load the profile.</span></span>
   
```powershell
Select-AzureRmProfile -Path c:\Users\exampleuser\profile\exampleSP.json
```

> [!NOTE]
> <span data-ttu-id="420a4-199">A hozzáférési jogkivonat lejár, így csak a működik a mentett profil használatával mindaddig, amíg a lexikális elem érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="420a4-199">The access token expires, so using a saved profile only works for as long as the token is valid.</span></span>
>  

<span data-ttu-id="420a4-200">Azt is megteheti hívhat meg REST műveleteinek-e jelentkezni a powershellből.</span><span class="sxs-lookup"><span data-stu-id="420a4-200">Alternatively, you can invoke REST operations from PowerShell to log in.</span></span> <span data-ttu-id="420a4-201">A hitelesítési válaszra kérheti le a hozzáférési jogkivonat más műveletek való használatra.</span><span class="sxs-lookup"><span data-stu-id="420a4-201">From the authentication response, you can retrieve the access token for use with other operations.</span></span> <span data-ttu-id="420a4-202">Lásd a példát a hozzáférési jogkivonat beolvasása REST műveleteinek figyelőn, [generálása egy hozzáférési jogkivonat](resource-manager-rest-api.md#generating-an-access-token).</span><span class="sxs-lookup"><span data-stu-id="420a4-202">For an example of retrieving the access token by invoking REST operations, see [Generating an Access Token](resource-manager-rest-api.md#generating-an-access-token).</span></span>

## <a name="debug"></a><span data-ttu-id="420a4-203">Hibakeresés</span><span class="sxs-lookup"><span data-stu-id="420a4-203">Debug</span></span>

<span data-ttu-id="420a4-204">Egy egyszerű szolgáltatás létrehozása során felmerülő hibák a következők:</span><span class="sxs-lookup"><span data-stu-id="420a4-204">You may encounter the following errors when creating a service principal:</span></span>

* <span data-ttu-id="420a4-205">**"Authentication_Unauthorized"** vagy **"előfizetést az adott környezetben található."**</span><span class="sxs-lookup"><span data-stu-id="420a4-205">**"Authentication_Unauthorized"** or **"No subscription found in the context."**</span></span> <span data-ttu-id="420a4-206">– Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik a [szükséges engedélyek](#required-permissions) az Azure Active Directory regisztrálnia az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="420a4-206">- You see this error when your account does not have the [required permissions](#required-permissions) on the Azure Active Directory to register an app.</span></span> <span data-ttu-id="420a4-207">Általában ezt a hibát látva az Azure Active Directoryban csak rendszergazda felhasználók regisztrálhatják az alkalmazásokat, és a fiók nincs a rendszergazda segítségét.</span><span class="sxs-lookup"><span data-stu-id="420a4-207">Typically, you see this error when only admin users in your Azure Active Directory can register apps, and your account is not an admin.</span></span> <span data-ttu-id="420a4-208">Kérje a rendszergazdától, vagy rendeljen Önhöz egy rendszergazdai szerepkört, vagy lehetővé teszi a felhasználók alkalmazásokat regisztrálni.</span><span class="sxs-lookup"><span data-stu-id="420a4-208">Ask your administrator to either assign you to an administrator role, or to enable users to register apps.</span></span>

* <span data-ttu-id="420a4-209">A fiók **"nem jogosult a műveletre"Microsoft.Authorization/roleAssignments/write"hatókörben"/Subscriptions/ {guid}"."**  -Ezt a hibaüzenetet látja, ha a fiók nem rendelkezik megfelelő engedélyekkel a szerepkör hozzárendelése identitást.</span><span class="sxs-lookup"><span data-stu-id="420a4-209">Your account **"does not have authorization to perform action 'Microsoft.Authorization/roleAssignments/write' over scope '/subscriptions/{guid}'."** - You see this error when your account does not have sufficient permissions to assign a role to an identity.</span></span> <span data-ttu-id="420a4-210">Kérje meg a előfizetési rendszergazda, akkor a felhasználói hozzáférés adminisztrátora szerepkörbe való felvételre.</span><span class="sxs-lookup"><span data-stu-id="420a4-210">Ask your subscription administrator to add you to User Access Administrator role.</span></span>

## <a name="sample-applications"></a><span data-ttu-id="420a4-211">Mintaalkalmazások</span><span class="sxs-lookup"><span data-stu-id="420a4-211">Sample applications</span></span>
<span data-ttu-id="420a4-212">Az alkalmazás a különböző platformokat, a bejelentkezés kapcsolatos információkért lásd:</span><span class="sxs-lookup"><span data-stu-id="420a4-212">For information about logging in as the application through different platforms, see:</span></span>

* [<span data-ttu-id="420a4-213">.NET</span><span class="sxs-lookup"><span data-stu-id="420a4-213">.NET</span></span>](/dotnet/azure/dotnet-sdk-azure-authenticate?view=azure-dotnet)
* [<span data-ttu-id="420a4-214">Java</span><span class="sxs-lookup"><span data-stu-id="420a4-214">Java</span></span>](/java/azure/java-sdk-azure-authenticate)
* [<span data-ttu-id="420a4-215">Node.js</span><span class="sxs-lookup"><span data-stu-id="420a4-215">Node.js</span></span>](/nodejs/azure/node-sdk-azure-get-started?view=azure-node-2.0.0)
* [<span data-ttu-id="420a4-216">Python</span><span class="sxs-lookup"><span data-stu-id="420a4-216">Python</span></span>](/python/azure/python-sdk-azure-authenticate?view=azure-python)
* [<span data-ttu-id="420a4-217">Ruby</span><span class="sxs-lookup"><span data-stu-id="420a4-217">Ruby</span></span>](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)

## <a name="next-steps"></a><span data-ttu-id="420a4-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="420a4-218">Next steps</span></span>
* <span data-ttu-id="420a4-219">Az alkalmazás integrálása az Azure erőforrások kezeléséhez részletes lépéseiért lásd: [fejlesztői útmutató az Azure Resource Manager API-val engedélyezési](resource-manager-api-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-219">For detailed steps on integrating an application into Azure for managing resources, see [Developer's guide to authorization with the Azure Resource Manager API](resource-manager-api-authentication.md).</span></span>
* <span data-ttu-id="420a4-220">Egy részletes ismertetése az alkalmazások és szolgáltatásnevekről [alkalmazás és szolgáltatás egyszerű objektumok](../active-directory/active-directory-application-objects.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-220">For a more detailed explanation of applications and service principals, see [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md).</span></span> 
* <span data-ttu-id="420a4-221">Azure Active Directory-hitelesítéssel kapcsolatos további információkért lásd: [hitelesítési forgatókönyvek az Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-221">For more information about Azure Active Directory authentication, see [Authentication Scenarios for Azure AD](../active-directory/active-directory-authentication-scenarios.md).</span></span>
* <span data-ttu-id="420a4-222">Elérhető műveleteket, lehet megadott vagy megtagadta a felhasználók listáját lásd: [Azure Resource Manager erőforrás-szolgáltató műveletek](../active-directory/role-based-access-control-resource-provider-operations.md).</span><span class="sxs-lookup"><span data-stu-id="420a4-222">For a list of available actions that can be granted or denied to users, see [Azure Resource Manager Resource Provider operations](../active-directory/role-based-access-control-resource-provider-operations.md).</span></span>

