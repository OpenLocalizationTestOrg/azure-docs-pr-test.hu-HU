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
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Nem interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása
Alkalmazás saját identitással (nem interaktív) vagy a bejelentkezett felhasználó az alkalmazás (interaktív) alatt a .NET Azure HDInsight-alkalmazások futtatása. Az interaktív alkalmazás mintát, lásd: [csatlakozás az Azure HDInsight](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). Ez a cikk bemutatja, hogyan csatlakozzon az Azure-ba, és kezelheti a HDInsight nem interaktív hitelesítés .NET-alkalmazás létrehozása.

A nem interaktív .NET-alkalmazás lesz szüksége:

* Az Azure-előfizetés-bérlőazonosító beszerzése (más néven címtár azonosító). Lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* Az Azure Active Directory alkalmazás ügyfél-azonosítót. Lásd: [hozzon létre egy Azure Active Directory-alkalmazás](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), és [az alkalmazás azonosítót](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)
* Az Azure Active Directory-alkalmazás titkos kulcs. Lásd: [Get alkalmazás hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)

## <a name="prerequisites"></a>Előfeltételek
* HDInsight-fürthöz. Lásd: [használatába bevezető oktatóanyagot](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-azure-ad-application-to-role"></a>Az Azure AD alkalmazás-szerepkör hozzárendelése
Az alkalmazás kell rendelnie egy [szerepkör](../active-directory/role-based-access-built-in-roles.md) műveletekhez tartozó engedélyek megadását. A hatókör szintjén található az előfizetés, erőforráscsoportból vagy erőforrás állíthatja be. Az engedélyek (például egy alkalmazást az olvasó szerepkört erőforráscsoport azt jelenti, hogy olvassa be a az erőforráscsoport hozzáadása és minden olyan erőforrásnál tartalmaz) hatókör alacsonyabb szintre származnak. Ebben az oktatóanyagban a hatókör állítja, az erőforráscsoport szintjén. További információkért lásd: [szerepkör-hozzárendelések segítségével az Azure-előfizetés erőforrásokhoz való hozzáférés kezelése](../active-directory/role-based-access-control-configure.md)

**A tulajdonosi szerepkört az Azure AD-alkalmazás hozzáadása**

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Kattintson a **erőforráscsoport** a bal oldali ablaktáblán.
3. Kattintson az erőforráscsoport, amely tartalmazza a HDInsight-fürt, amelyen futtatja a Hive-lekérdezést az oktatóanyag későbbi részében. Ha túl sok erőforrás-csoportok, használhatja a szűrés.
4. Kattintson a **hozzáférés-vezérlés (IAM)** a erőforrás csoport menüből.
5. Kattintson a **Hozzáadás** a a **felhasználók** panelen.
6. Kövesse a adja hozzá a **tulajdonos** szerepkört az Azure AD-alkalmazás létrehozása az utolsó eljárás. Amikor befejezte az sikeresen, látni fogja az alkalmazást, a felhasználók panelről a tulajdonosi szerepkört, szerepel.

## <a name="develop-hdinsight-client-application"></a>HDInsight-ügyfél alkalmazások fejlesztése

1. Hozzon létre egy C# konzolalkalmazást.
2. Adja hozzá a következő Nuget-csomagok:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. A következő példakód használja:

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

## <a name="next-steps"></a>Következő lépések
* [Azure Active Directory-alkalmazás és a portál használatával egyszerű szolgáltatás létrehozása](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Az Azure Resource Manager egyszerű hitelesítés](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)
