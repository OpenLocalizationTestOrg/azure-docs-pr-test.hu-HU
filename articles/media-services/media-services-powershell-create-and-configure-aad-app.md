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
# <a name="use-powershell-toocreate-an-azure-ad-app-toouse-with-hello-azure-media-services-api"></a>Az Azure AD alkalmazás toouse PowerShell toocreate használata hello Azure Media Services API

Ismerje meg, hogyan toouse a PowerShell parancsfájl-e toocreate egy Azure Active Directory (Azure AD) alkalmazás és szolgáltatás egyszerű tooaccess Azure Media Services-erőforrásokat.  

## <a name="prerequisites"></a>Előfeltételek

- Egy Azure-fiók. Ha nincs fiókja, kezdje egy [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/). 
- Egy Media Services-fiók. További információkért lásd: [Azure Media Services-fiók létrehozása az Azure-portálon hello](media-services-portal-create-account.md).
- Az Azure PowerShell-verzió 0.8.8 vagy újabb verziója. További információkért lásd: [hogyan toouse Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Az Azure Resource Manager parancsmagok.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Az Azure AD-alkalmazás létrehozása a PowerShell használatával  

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

További információkért tekintse meg a következő cikkek hello:

- [A szolgáltatás egyszerű tooaccess erőforrásainak használatához az Azure PowerShell toocreate](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Szerepköralapú hozzáférés-vezérlés kezelése az Azure PowerShell használatával](../active-directory/role-based-access-control-manage-access-powershell.md)
- [Hogyan toomanually démon alkalmazások konfigurálása tanúsítványok használatával](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Következő lépések

Ismerkedés a [fájlok feltöltése a tooyour fiók](media-services-portal-upload-files.md).
