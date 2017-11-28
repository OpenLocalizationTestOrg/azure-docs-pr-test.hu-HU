---
title: "nem interaktív hitelesítés aaaCreate .NET HDInsight applciations - Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan toocreate nem interaktív hitelesítés .NET HDInsight-alkalmazások."
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
ms.openlocfilehash: 5367c160b0146e6b855486b95f363e8fe7f1c98f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a><span data-ttu-id="f3de5-103">Nem interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3de5-103">Create non-interactive authentication .NET HDInsight applications</span></span>
<span data-ttu-id="f3de5-104">Alkalmazás saját identitással (nem interaktív) vagy hello identitás hello bejelentkezett felhasználó hello alkalmazás (interaktív) alatt a .NET Azure HDInsight-alkalmazások futtatása.</span><span class="sxs-lookup"><span data-stu-id="f3de5-104">You can run your .NET Azure HDInsight application either under application's own identity (non-interactive) or under hello identity of hello signed-in user of hello application (interactive).</span></span> <span data-ttu-id="f3de5-105">Hello interaktív alkalmazás mintát, lásd: [tooAzure HDInsight csatlakozás](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span><span class="sxs-lookup"><span data-stu-id="f3de5-105">For a sample of hello interactive application, see [Connect tooAzure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight).</span></span> <span data-ttu-id="f3de5-106">Ez a cikk bemutatja, hogyan toocreate nem interaktív hitelesítés .NET alkalmazás tooconnect tooAzure és HDInsight kezelése.</span><span class="sxs-lookup"><span data-stu-id="f3de5-106">This article shows you how toocreate non-interactive authentication .NET application tooconnect tooAzure and manage HDInsight.</span></span>

<span data-ttu-id="f3de5-107">A nem interaktív .NET-alkalmazás lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="f3de5-107">From your non-interactive .NET application, you need:</span></span>

* <span data-ttu-id="f3de5-108">Az Azure-előfizetés-bérlőazonosító beszerzése (más néven címtár azonosító).</span><span class="sxs-lookup"><span data-stu-id="f3de5-108">Your Azure subscription tenant ID (A.K.A directory ID).</span></span> <span data-ttu-id="f3de5-109">Lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span><span class="sxs-lookup"><span data-stu-id="f3de5-109">See [Get tenant ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).</span></span>
* <span data-ttu-id="f3de5-110">hello Azure Active Directory-alkalmazás ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="f3de5-110">hello Azure Active Directory application client ID.</span></span> <span data-ttu-id="f3de5-111">Lásd: [hozzon létre egy Azure Active Directory-alkalmazás](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), és [az alkalmazás azonosítót](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="f3de5-111">See [Create an Azure Active Directory application](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), and [Get an application ID](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>
* <span data-ttu-id="f3de5-112">hello Azure Active Directory alkalmazás titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="f3de5-112">hello Azure Active Directory application secret key.</span></span> <span data-ttu-id="f3de5-113">Lásd: [Get alkalmazás hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span><span class="sxs-lookup"><span data-stu-id="f3de5-113">See [Get application authentication key](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f3de5-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="f3de5-114">Prerequisites</span></span>
* <span data-ttu-id="f3de5-115">HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="f3de5-115">HDInsight cluster.</span></span> <span data-ttu-id="f3de5-116">Lásd: [használatába bevezető oktatóanyagot](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span><span class="sxs-lookup"><span data-stu-id="f3de5-116">See [getting started tutorial](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).</span></span>



## <a name="assign-azure-ad-application-toorole"></a><span data-ttu-id="f3de5-117">Az Azure AD alkalmazás toorole hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="f3de5-117">Assign Azure AD application toorole</span></span>
<span data-ttu-id="f3de5-118">Hozzá kell rendelnie hello alkalmazás tooa [szerepkör](../active-directory/role-based-access-built-in-roles.md) toogrant azt műveleteket engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="f3de5-118">You must assign hello application tooa [role](../active-directory/role-based-access-built-in-roles.md) toogrant it permissions for performing actions.</span></span> <span data-ttu-id="f3de5-119">Hello hatókör hello előfizetés, az erőforráscsoportot, vagy az erőforrás hello szinten állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="f3de5-119">You can set hello scope at hello level of hello subscription, resource group, or resource.</span></span> <span data-ttu-id="f3de5-120">hello engedélyek örökölt toolower szintek hatókör (például egy alkalmazás toohello olvasó szerepkört erőforráscsoport azt jelenti, hogy olvassa be a hello erőforráscsoport hozzáadása és erőforrásokat tartalmazza) is.</span><span class="sxs-lookup"><span data-stu-id="f3de5-120">hello permissions are inherited toolower levels of scope (for example, adding an application toohello Reader role for a resource group means it can read hello resource group and any resources it contains).</span></span> <span data-ttu-id="f3de5-121">Ebben az oktatóanyagban hello hatókör állítja hello erőforrás csoportok szintjén.</span><span class="sxs-lookup"><span data-stu-id="f3de5-121">In this tutorial, you will set hello scope at hello resource group level.</span></span> <span data-ttu-id="f3de5-122">További információkért lásd: [szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](../active-directory/role-based-access-control-configure.md)</span><span class="sxs-lookup"><span data-stu-id="f3de5-122">For more information, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md)</span></span>

<span data-ttu-id="f3de5-123">**Tulajdonosi szerepkör toohello az Azure AD alkalmazás tooadd hello**</span><span class="sxs-lookup"><span data-stu-id="f3de5-123">**tooadd hello Owner role toohello Azure AD application**</span></span>

1. <span data-ttu-id="f3de5-124">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="f3de5-124">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="f3de5-125">Kattintson a **erőforráscsoport** hello bal oldali ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="f3de5-125">Click **Resource Group** from hello left pane.</span></span>
3. <span data-ttu-id="f3de5-126">Ha futtatja a Hive-lekérdezést az oktatóanyag későbbi részében hello HDInsight-fürtöt tartalmazó erőforráscsoportot hello kattintson.</span><span class="sxs-lookup"><span data-stu-id="f3de5-126">Click hello resource group that contains hello HDInsight cluster where you will run your Hive query later in this tutorial.</span></span> <span data-ttu-id="f3de5-127">Ha túl sok erőforrás-csoportok, hello szűrőt használhatja.</span><span class="sxs-lookup"><span data-stu-id="f3de5-127">If there are too many resource groups, you can use hello filter.</span></span>
4. <span data-ttu-id="f3de5-128">Kattintson a **hozzáférés-vezérlés (IAM)** hello erőforrás csoport menüből.</span><span class="sxs-lookup"><span data-stu-id="f3de5-128">Click **Access control (IAM)** from hello resource group menu.</span></span>
5. <span data-ttu-id="f3de5-129">Kattintson a **Hozzáadás** a hello **felhasználók** panelen.</span><span class="sxs-lookup"><span data-stu-id="f3de5-129">Click **Add** from hello **Users** blade.</span></span>
6. <span data-ttu-id="f3de5-130">Hajtsa végre a hello utasítás tooadd hello **tulajdonos** szerepkör toohello az Azure AD-alkalmazást eljárásban létrehozott hello utolsó.</span><span class="sxs-lookup"><span data-stu-id="f3de5-130">Follow hello instruction tooadd hello **Owner** role toohello Azure AD application you created in hello last procedure.</span></span> <span data-ttu-id="f3de5-131">Amikor befejezte az sikeresen, látni fogja hello alkalmazás hello felhasználók panelről hello tulajdonosi szerepkör szerepel.</span><span class="sxs-lookup"><span data-stu-id="f3de5-131">When you complete it successfully, you shall see hello application listed in hello Users blade with hello Owner role.</span></span>

## <a name="develop-hdinsight-client-application"></a><span data-ttu-id="f3de5-132">HDInsight-ügyfél alkalmazások fejlesztése</span><span class="sxs-lookup"><span data-stu-id="f3de5-132">Develop HDInsight client application</span></span>

1. <span data-ttu-id="f3de5-133">Hozzon létre egy C# konzolalkalmazást.</span><span class="sxs-lookup"><span data-stu-id="f3de5-133">Create a C# console application.</span></span>
2. <span data-ttu-id="f3de5-134">Adja hozzá a következő Nuget-csomagok hello:</span><span class="sxs-lookup"><span data-stu-id="f3de5-134">Add hello following Nuget packages:</span></span>

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. <span data-ttu-id="f3de5-135">A következő példakód hello használata:</span><span class="sxs-lookup"><span data-stu-id="f3de5-135">Use hello following code sample:</span></span>

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
                private static string secretKey = "<Enter hello Application Secret Key>";
        
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
                    Console.WriteLine("Press Enter toocontinue");
                    Console.ReadLine();
                }

                /// Get hello access token for a service principal and provided key                
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

## <a name="next-steps"></a><span data-ttu-id="f3de5-136">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f3de5-136">Next steps</span></span>
* [<span data-ttu-id="f3de5-137">Azure Active Directory-alkalmazás és a portál használatával egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="f3de5-137">Create Azure Active Directory application and service principal using portal</span></span>](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [<span data-ttu-id="f3de5-138">Az Azure Resource Manager egyszerű hitelesítés</span><span class="sxs-lookup"><span data-stu-id="f3de5-138">Authenticate service principal with Azure Resource Manager</span></span>](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [<span data-ttu-id="f3de5-139">Azure szerepköralapú hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="f3de5-139">Azure Role-Based Access Control</span></span>](../active-directory/role-based-access-control-configure.md)
