---
title: "Nem interaktív hitelesítés .NET HDInsight applciations - Azure létrehozása |} Microsoft Docs"
description: "Útmutató a nem interaktív hitelesítés .NET HDInsight-alkalmazások létrehozásához."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 8e32430f-6404-498a-9fcd-f20338d964af
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 7821a9e60e60ff01cff06db2a6f216a260c1c41a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a><span data-ttu-id="d3ffe-103">Nem interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3ffe-103">Create non-interactive authentication .NET HDInsight applications</span></span>
<span data-ttu-id="d3ffe-104">Alkalmazás saját identitással (nem interaktív) vagy a bejelentkezett felhasználó az alkalmazás (interaktív) alatt a .NET Azure HDInsight-alkalmazások futtatása.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-104">You can run your .NET Azure HDInsight application either under application's own identity (non-interactive) or under the identity of the signed-in user of the application (interactive).</span></span> <span data-ttu-id="d3ffe-105">Az interaktív alkalmazás mintát, lásd: [csatlakozás az Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="d3ffe-105">For a sample of the interactive application, see [Connect to Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span></span> <span data-ttu-id="d3ffe-106">Ez a cikk bemutatja, hogyan csatlakozzon az Azure-ba, és kezelheti a HDInsight nem interaktív hitelesítés .NET-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-106">This article shows you how to create non-interactive authentication .NET application to connect to Azure and manage HDInsight.</span></span>

<span data-ttu-id="d3ffe-107">A nem interaktív .NET-alkalmazás lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="d3ffe-107">From your non-interactive .NET application, you need:</span></span>

* <span data-ttu-id="d3ffe-108">Az Azure-előfizetés-bérlőazonosító beszerzése (más néven címtár azonosító).</span><span class="sxs-lookup"><span data-stu-id="d3ffe-108">Your Azure subscription tenant ID (A.K.A directory ID).</span></span> <span data-ttu-id="d3ffe-109">Lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span><span class="sxs-lookup"><span data-stu-id="d3ffe-109">See [Get tenant ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span></span>
* <span data-ttu-id="d3ffe-110">Az Azure Active Directory alkalmazás ügyfél-azonosítót.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-110">The Azure Active Directory application client ID.</span></span> <span data-ttu-id="d3ffe-111">Lásd: [hozzon létre egy Azure Active Directory-alkalmazás](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), és [az alkalmazás azonosítót](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="d3ffe-111">See [Create an Azure Active Directory application](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), and [Get an application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>
* <span data-ttu-id="d3ffe-112">Az Azure Active Directory-alkalmazás titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-112">The Azure Active Directory application secret key.</span></span> <span data-ttu-id="d3ffe-113">Lásd: [Get alkalmazás hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="d3ffe-113">See [Get application authentication key](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d3ffe-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="d3ffe-114">Prerequisites</span></span>
* <span data-ttu-id="d3ffe-115">HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-115">HDInsight cluster.</span></span> <span data-ttu-id="d3ffe-116">Lásd: [használatába bevezető oktatóanyagot](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="d3ffe-116">See [getting started tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span>



## <a name="assign-azure-ad-application-to-role"></a><span data-ttu-id="d3ffe-117">Az Azure AD alkalmazás-szerepkör hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="d3ffe-117">Assign Azure AD application to role</span></span>
<span data-ttu-id="d3ffe-118">Az alkalmazás kell rendelnie egy [szerepkör](../active-directory/role-based-access-built-in-roles.md) műveletekhez tartozó engedélyek megadását.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-118">You must assign the application to a [role](../active-directory/role-based-access-built-in-roles.md) to grant it permissions for performing actions.</span></span> <span data-ttu-id="d3ffe-119">A hatókör szintjén található az előfizetés, erőforráscsoportból vagy erőforrás állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-119">You can set the scope at the level of the subscription, resource group, or resource.</span></span> <span data-ttu-id="d3ffe-120">Az engedélyek (például egy alkalmazást az olvasó szerepkört erőforráscsoport azt jelenti, hogy olvassa be a az erőforráscsoport hozzáadása és minden olyan erőforrásnál tartalmaz) hatókör alacsonyabb szintre származnak.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-120">The permissions are inherited to lower levels of scope (for example, adding an application to the Reader role for a resource group means it can read the resource group and any resources it contains).</span></span> <span data-ttu-id="d3ffe-121">Ebben az oktatóanyagban a hatókör állítja, az erőforráscsoport szintjén.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-121">In this tutorial, you will set the scope at the resource group level.</span></span> <span data-ttu-id="d3ffe-122">További információkért lásd: [szerepkör-hozzárendelések segítségével az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="d3ffe-122">For more information, see [Use role assignments to manage access to your Azure subscription resources](../active-directory/role-based-access-control-configure.md)</span></span>

<span data-ttu-id="d3ffe-123">**A tulajdonosi szerepkört az Azure AD-alkalmazás hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="d3ffe-123">**To add the Owner role to the Azure AD application**</span></span>

1. <span data-ttu-id="d3ffe-124">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d3ffe-124">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d3ffe-125">Kattintson a **erőforráscsoport** a bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-125">Click **Resource Group** from the left pane.</span></span>
3. <span data-ttu-id="d3ffe-126">Kattintson az erőforráscsoport, amely tartalmazza a HDInsight-fürt, amelyen futtatja a Hive-lekérdezést az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-126">Click the resource group that contains the HDInsight cluster where you will run your Hive query later in this tutorial.</span></span> <span data-ttu-id="d3ffe-127">Ha túl sok erőforrás-csoportok, használhatja a szűrés.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-127">If there are too many resource groups, you can use the filter.</span></span>
4. <span data-ttu-id="d3ffe-128">Kattintson a **hozzáférés-vezérlés (IAM)** a erőforrás csoport menüből.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-128">Click **Access control (IAM)** from the resource group menu.</span></span>
5. <span data-ttu-id="d3ffe-129">Kattintson a **Hozzáadás** a a **felhasználók** panelen.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-129">Click **Add** from the **Users** blade.</span></span>
6. <span data-ttu-id="d3ffe-130">Kövesse a adja hozzá a **tulajdonos** szerepkört az Azure AD-alkalmazás létrehozása az utolsó eljárás.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-130">Follow the instruction to add the **Owner** role to the Azure AD application you created in the last procedure.</span></span> <span data-ttu-id="d3ffe-131">Amikor befejezte az sikeresen, látni fogja az alkalmazást, a felhasználók panelről a tulajdonosi szerepkört, szerepel.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-131">When you complete it successfully, you shall see the application listed in the Users blade with the Owner role.</span></span>

## <a name="develop-hdinsight-client-application"></a><span data-ttu-id="d3ffe-132">HDInsight-ügyfél alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="d3ffe-132">Develop HDInsight client application</span></span>

1. <span data-ttu-id="d3ffe-133">Hozzon létre egy C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="d3ffe-133">Create a C# console application.</span></span>
2. <span data-ttu-id="d3ffe-134">Adja hozzá a következő Nuget-csomagok:</span><span class="sxs-lookup"><span data-stu-id="d3ffe-134">Add the following Nuget packages:</span></span>

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. <span data-ttu-id="d3ffe-135">A következő példakód használja:</span><span class="sxs-lookup"><span data-stu-id="d3ffe-135">Use the following code sample:</span></span>

        using System;
        using System.Security;
        using Microsoft.Azure;
        using Microsoft.Azure.Common.Authentication;
        using Microsoft.Azure.Common.Authentication.Factories;
        using Microsoft.Azure.Common.Authentication.Models;
        using Microsoft.Azure.Management.Resources;
        using Microsoft.Azure.Management.HDInsight;
        
        namespace CreateHDICluster
        {
            internal class Program
            {
                private static HDInsightManagementClient _hdiManagementClient;
        
                private static Guid SubscriptionId = new Guid("<Enter Your Azure Subscription ID>");
                private static string tenantID = "<Enter Your Tenant ID (A.K.A. Directory ID)>";
                private static string applicationID = "<Enter Your Application ID>";
                private static string secretKey = "<Enter the Application Secret Key>";
        
                private static void Main(string[] args)
                {
                    var key = new SecureString();
                    foreach (char c in secretKey) { key.AppendChar(c); }

                    var tokenCreds = GetTokenCloudCredentials(tenantID, applicationID, key);
                    var subCloudCredentials = GetSubscriptionCloudCredentials(tokenCreds, SubscriptionId);
        
                    var resourceManagementClient = new ResourceManagementClient(subCloudCredentials);
                    resourceManagementClient.Providers.Register("Microsoft.HDInsight");
        
                    _hdiManagementClient = new HDInsightManagementClient(subCloudCredentials);
        
                    var results = _hdiManagementClient.Clusters.List();
                    foreach (var name in results.Clusters)
                    {
                        Console.WriteLine("Cluster Name: " + name.Name);
                        Console.WriteLine("\t Cluster type: " + name.Properties.ClusterDefinition.ClusterType);
                        Console.WriteLine("\t Cluster location: " + name.Location);
                        Console.WriteLine("\t Cluster version: " + name.Properties.ClusterVersion);
                    }
                    Console.WriteLine("Press Enter to continue");
                    Console.ReadLine();
                }

                /// Get the access token for a service principal and provided key                
                public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
                {
                    var authFactory = new AuthenticationFactory();
                    var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };
                    var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];
                    var accessToken =
                        authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never).AccessToken;
        
                    return new TokenCloudCredentials(accessToken);
                }
        
                public static SubscriptionCloudCredentials GetSubscriptionCloudCredentials(SubscriptionCloudCredentials creds, Guid subId)
                {
                    return new TokenCloudCredentials(subId.ToString(), ((TokenCloudCredentials)creds).Token);
                }
            }
        }

## <a name="next-steps"></a><span data-ttu-id="d3ffe-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3ffe-136">Next steps</span></span>
* [<span data-ttu-id="d3ffe-137">Azure Active Directory-alkalmazás és a portál használatával egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d3ffe-137">Create Azure Active Directory application and service principal using portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="d3ffe-138">Az Azure Resource Manager egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="d3ffe-138">Authenticate service principal with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="d3ffe-139">Azure szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="d3ffe-139">Azure Role-Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
