---
title: "Értékek lekérése app hitelesítés – az Azure SQL Database |} Microsoft Docs"
description: "Hozzon létre egy egyszerű szolgáltatást kód SQL-adatbázis eléréséhez."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
tags: 
ms.assetid: b43e43bb-6660-49e6-b069-abde97eb5770
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/30/2016
ms.author: sstein
ms.openlocfilehash: ec6256e9c5bb0d9c8d15d0f673cea70b3915eb34
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a><span data-ttu-id="c4a06-103">Az alkalmazás kódjában SQL-adatbázis eléréséhez szükséges értékek beolvasása</span><span class="sxs-lookup"><span data-stu-id="c4a06-103">Get the required values for authenticating an application to access SQL Database from code</span></span>
<span data-ttu-id="c4a06-104">Létrehozásához és kezeléséhez az SQL-adatbázis kódból regisztrálnia kell az az alkalmazás hol létrejöttek-e az Azure-erőforrások Azure Active Directory (AAD) tartományban az előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c4a06-104">To create and manage SQL Database from code you must register your app in the Azure Active Directory (AAD) domain  in the subscription where your Azure resources have been created.</span></span>

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a><span data-ttu-id="c4a06-105">Egy egyszerű szolgáltatásnév létrehozása hozzáférését az erőforrásokhoz az adott alkalmazásból</span><span class="sxs-lookup"><span data-stu-id="c4a06-105">Create a service principal to access resources from an application</span></span>
<span data-ttu-id="c4a06-106">Telepíteni kell a legújabb [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) telepíteni és futtatni.</span><span class="sxs-lookup"><span data-stu-id="c4a06-106">You need to have the latest [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) installed and running.</span></span> <span data-ttu-id="c4a06-107">Részletes információk: [Az Azure PowerShell telepítése és konfigurálása](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c4a06-107">For detailed information, see [How to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span>

<span data-ttu-id="c4a06-108">A következő PowerShell-parancsprogram létrehozza az Active Directory (AD)-alkalmazást, és a C#-alkalmazás hitelesítéséhez szükséges egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="c4a06-108">The following PowerShell script creates the Active Directory (AD) application and the service principal that we need to authenticate our C# app.</span></span> <span data-ttu-id="c4a06-109">A parancsfájl kimenetének értékeire szükség lesz az előbb említett C#-mintához.</span><span class="sxs-lookup"><span data-stu-id="c4a06-109">The script outputs values we need for the preceding C# sample.</span></span> <span data-ttu-id="c4a06-110">Részletes információk: [Egyszerű szolgáltatás létrehozása erőforrások eléréséhez az Azure PowerShell használatával](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span><span class="sxs-lookup"><span data-stu-id="c4a06-110">For detailed information, see [Use Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md).</span></span>

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a><span data-ttu-id="c4a06-111">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="c4a06-111">See also</span></span>
* [<span data-ttu-id="c4a06-112">SQL-adatbázis létrehozása a C#</span><span class="sxs-lookup"><span data-stu-id="c4a06-112">Create a SQL database with C#</span></span>](sql-database-get-started-csharp.md)
* [<span data-ttu-id="c4a06-113">Csatlakozás az SQL Database az Azure Active Directory-hitelesítés használatával</span><span class="sxs-lookup"><span data-stu-id="c4a06-113">Connecting to SQL Database By Using Azure Active Directory Authentication</span></span>](sql-database-aad-authentication.md)

