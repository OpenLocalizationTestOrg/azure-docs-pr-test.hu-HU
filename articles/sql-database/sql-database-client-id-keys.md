---
title: "alkalmazás hitelesítés – az Azure SQL Database aaaGet értékeinek |} Microsoft Docs"
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
ms.openlocfilehash: b57dc075ec9e679da9f2f5fa53e02312539cdf07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-hello-required-values-for-authenticating-an-application-tooaccess-sql-database-from-code"></a>Egy alkalmazás tooaccess SQL-adatbázis kódból hitelesítéséhez szükséges hello értékek lekérése
toocreate és SQL-adatbázis kezeléséhez hello Azure Active Directory (AAD) tartomány hello előfizetésben ahol létrejöttek-e az Azure-erőforrások regisztrálnia kell az alkalmazás kódját.

## <a name="create-a-service-principal-tooaccess-resources-from-an-application"></a>A szolgáltatás egyszerű tooaccess erőforrások létrehozása egy alkalmazás
Legújabb szüksége toohave hello [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) telepíteni és futtatni. Részletes információkért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azureps-cmdlets-docs).

hello következő PowerShell-parancsfájlt hoz létre hello Active Directory (AD) és hello szolgáltatást egyszerű tooauthenticate a C# alkalmazás igazolnia kell. hello parancsfájl kimenetében igazolnia kell a hello megelőző C# minták értékeket. Részletes információkért lásd: [egyszerű szolgáltatás használata az Azure PowerShell toocreate tooaccess erőforrások](../azure-resource-manager/resource-group-authenticate-service-principal.md).

    # Sign in tooAzure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set toohello subscription you want toowork with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is hello display name for your app, must be unique in your directory.
    # $uri does not need toobe a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for hello app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # tooavoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun hello following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output hello values we need for our C# application toosuccessfully authenticate

    Write-Output "Copy these values into hello C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Lásd még:
* [SQL-adatbázis létrehozása a C#](sql-database-get-started-csharp.md)
* [Csatlakozás tooSQL adatbázis által használó Azure Active Directory-hitelesítés](sql-database-aad-authentication.md)

