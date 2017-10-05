---
title: "Az Azure Media Services API eléréséhez az Azure AD-alkalmazás létrehozása a PowerShell használatával |} Microsoft Docs"
description: "Tudnivalók a PowerShell segítségével hozzon létre egy Azure Active Directory (Azure AD) alkalmazást, és állítsa be a hozzáférés az Azure Media Services API."
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
ms.openlocfilehash: eea0f3a03dd77ce56484f32b192299bd97c05300
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a><span data-ttu-id="00fe3-103">Az Azure Media Services API használata az Azure AD-alkalmazás létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="00fe3-103">Use PowerShell to create an Azure AD app to use with the Azure Media Services API</span></span>

<span data-ttu-id="00fe3-104">Útmutató az Azure Media Services-erőforrások eléréséhez Azure Active Directory (Azure AD) alkalmazás és egyszerű szolgáltatás létrehozása a PowerShell parancsfájl segítségével.</span><span class="sxs-lookup"><span data-stu-id="00fe3-104">Learn how to use a PowerShell script to create an Azure Active Directory (Azure AD) application and service principal to access Azure Media Services resources.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="00fe3-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="00fe3-105">Prerequisites</span></span>

- <span data-ttu-id="00fe3-106">Egy Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="00fe3-106">An Azure account.</span></span> <span data-ttu-id="00fe3-107">Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="00fe3-107">If you don't have an account, start with an [Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
- <span data-ttu-id="00fe3-108">Egy Media Services-fiók.</span><span class="sxs-lookup"><span data-stu-id="00fe3-108">A Media Services account.</span></span> <span data-ttu-id="00fe3-109">További információkért lásd: [Azure Media Services-fiók létrehozása az Azure portálon](media-services-portal-create-account.md).</span><span class="sxs-lookup"><span data-stu-id="00fe3-109">For more information, see [Create an Azure Media Services account in the Azure portal](media-services-portal-create-account.md).</span></span>
- <span data-ttu-id="00fe3-110">Az Azure PowerShell-verzió 0.8.8 vagy újabb verziója.</span><span class="sxs-lookup"><span data-stu-id="00fe3-110">Azure PowerShell version 0.8.8 or a later version.</span></span> <span data-ttu-id="00fe3-111">További információkért lásd: [Azure PowerShell használata](https://docs.microsoft.com/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="00fe3-111">For more information, see [How to use Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).</span></span>
- <span data-ttu-id="00fe3-112">Az Azure Resource Manager parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="00fe3-112">Azure Resource Manager cmdlets.</span></span>  

## <a name="create-an-azure-ad-app-by-using-powershell"></a><span data-ttu-id="00fe3-113">Az Azure AD-alkalmazás létrehozása a PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="00fe3-113">Create an Azure AD app by using PowerShell</span></span>  

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
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

<span data-ttu-id="00fe3-114">További információkért tekintse át a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="00fe3-114">For more information, see the following articles:</span></span>

- [<span data-ttu-id="00fe3-115">Szolgáltatásnév létrehozása erőforrások eléréséhez az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="00fe3-115">Use Azure PowerShell to create a service principal to access resources</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [<span data-ttu-id="00fe3-116">Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="00fe3-116">Manage Role-Based Access Control by using Azure PowerShell</span></span>](../active-directory/role-based-access-control-manage-access-powershell.md)
- [<span data-ttu-id="00fe3-117">Saját kezű konfigurálásáról démon alkalmazások tanúsítványok használatával</span><span class="sxs-lookup"><span data-stu-id="00fe3-117">How to manually configure daemon apps by using certificates</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a><span data-ttu-id="00fe3-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="00fe3-118">Next steps</span></span>

<span data-ttu-id="00fe3-119">Ismerkedés a [fájlok feltöltése a fiókjához](media-services-portal-upload-files.md).</span><span class="sxs-lookup"><span data-stu-id="00fe3-119">Get started with [uploading files to your account](media-services-portal-upload-files.md).</span></span>
