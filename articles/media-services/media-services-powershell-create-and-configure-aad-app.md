---
title: "aaaUse PowerShell toocreate egy Azure AD alkalmazás tooaccess hello Azure Media Services API |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse PowerShell toocreate egy Azure Active Directory (Azure AD) alkalmazást, majd tooaccess hello Azure Media Services API."
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 1a8b4a53ad10b559f6ee4242b95c5bd15ee8e903
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a><span data-ttu-id="888cf-103">Az Azure AD alkalmazás toouse PowerShell toocreate használata hello Azure Media Services API</span><span class="sxs-lookup"><span data-stu-id="888cf-103">Use PowerShell toocreate an Azure AD app toouse with hello Azure Media Services API</span></span>

<span data-ttu-id="888cf-104">Ismerje meg, hogyan toouse a PowerShell parancsfájl-e toocreate egy Azure Active Directory (Azure AD) alkalmazás és szolgáltatás egyszerű tooaccess Azure Media Services-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="888cf-104">Learn how toouse a PowerShell script toocreate an Azure Active Directory (Azure AD) application and service principal tooaccess Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="888cf-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="888cf-105">Prerequisites</span></span>

- <span data-ttu-id="888cf-106">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="888cf-106">An Azure account.</span></span> <span data-ttu-id="888cf-107">Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="888cf-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="888cf-108">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="888cf-108">A Media Services account.</span></span> <span data-ttu-id="888cf-109">További információkért lásd: [Azure Media Services-fiók létrehozása az Azure-portálon hello](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="888cf-109">For more information, see [Create an Azure Media Services account in hello Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="888cf-110">Az Azure PowerShell-verzió 0.8.8 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="888cf-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="888cf-111">További információkért lásd: [hogyan toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="888cf-111">For more information, see [How toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="888cf-112">Az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="888cf-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="888cf-113">Az Azure AD-alkalmazás létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="888cf-113">Create an Azure AD app by using PowerShell</span></span>  

```powershell
Login-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds tooallow hello service principal application toobecome active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

<span data-ttu-id="888cf-114">További információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="888cf-114">For more information, see hello following articles:</span></span>

- [<span data-ttu-id="888cf-115">A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate</span><span class="sxs-lookup"><span data-stu-id="888cf-115">Use Azure PowerShell toocreate a service principal tooaccess resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="888cf-116">Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="888cf-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="888cf-117">Hogyan toomanually démon alkalmazások konfigurálása tanúsítványok használatával</span><span class="sxs-lookup"><span data-stu-id="888cf-117">How toomanually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="888cf-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="888cf-118">Next steps</span></span>

<span data-ttu-id="888cf-119">Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="888cf-119">Get started with [uploading files tooyour account](media-services-portal-upload-files.md).</span></span>
