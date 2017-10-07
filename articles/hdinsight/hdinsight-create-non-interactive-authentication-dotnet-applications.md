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
# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Nem interaktív hitelesítés .NET HDInsight-alkalmazások létrehozása
Alkalmazás saját identitással (nem interaktív) vagy hello identitás hello bejelentkezett felhasználó hello alkalmazás (interaktív) alatt a .NET Azure HDInsight-alkalmazások futtatása. Hello interaktív alkalmazás mintát, lásd: [tooAzure HDInsight csatlakozás](hdinsight-administer-use-dotnet-sdk.md#connect-to-azure-hdinsight). Ez a cikk bemutatja, hogyan toocreate nem interaktív hitelesítés .NET alkalmazás tooconnect tooAzure és HDInsight kezelése.

A nem interaktív .NET-alkalmazás lesz szüksége:

* Az Azure-előfizetés-bérlőazonosító beszerzése (más néven címtár azonosító). Lásd: [-bérlőazonosító beszerzése](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-tenant-id).
* hello Azure Active Directory-alkalmazás ügyfél-azonosító. Lásd: [hozzon létre egy Azure Active Directory-alkalmazás](../azure-resource-manager/resource-group-create-service-principal-portal.md#create-an-azure-active-directory-application), és [az alkalmazás azonosítót](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)
* hello Azure Active Directory alkalmazás titkos kulcs. Lásd: [Get alkalmazás hitelesítési kulcs](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key)

## <a name="prerequisites"></a>Előfeltételek
* HDInsight-fürthöz. Lásd: [használatába bevezető oktatóanyagot](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster).



## <a name="assign-azure-ad-application-toorole"></a>Az Azure AD alkalmazás toorole hozzárendelése
Hozzá kell rendelnie hello alkalmazás tooa [szerepkör](../active-directory/role-based-access-built-in-roles.md) toogrant azt műveleteket engedélyeit. Hello hatókör hello előfizetés, az erőforráscsoportot, vagy az erőforrás hello szinten állíthatja be. hello engedélyek örökölt toolower szintek hatókör (például egy alkalmazás toohello olvasó szerepkört erőforráscsoport azt jelenti, hogy olvassa be a hello erőforráscsoport hozzáadása és erőforrásokat tartalmazza) is. Ebben az oktatóanyagban hello hatókör állítja hello erőforrás csoportok szintjén. További információkért lásd: [szerepkör hozzárendelések toomanage hozzáférés tooyour Azure-előfizetés erőforrásainak használatához](../active-directory/role-based-access-control-configure.md)

**Tulajdonosi szerepkör toohello az Azure AD alkalmazás tooadd hello**

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).
2. Kattintson a **erőforráscsoport** hello bal oldali ablaktáblán.
3. Ha futtatja a Hive-lekérdezést az oktatóanyag későbbi részében hello HDInsight-fürtöt tartalmazó erőforráscsoportot hello kattintson. Ha túl sok erőforrás-csoportok, hello szűrőt használhatja.
4. Kattintson a **hozzáférés-vezérlés (IAM)** hello erőforrás csoport menüből.
5. Kattintson a **Hozzáadás** a hello **felhasználók** panelen.
6. Hajtsa végre a hello utasítás tooadd hello **tulajdonos** szerepkör toohello az Azure AD-alkalmazást eljárásban létrehozott hello utolsó. Amikor befejezte az sikeresen, látni fogja hello alkalmazás hello felhasználók panelről hello tulajdonosi szerepkör szerepel.

## <a name="develop-hdinsight-client-application"></a>HDInsight-ügyfél alkalmazások fejlesztése

1. Hozzon létre egy C# konzolalkalmazást.
2. Adja hozzá a következő Nuget-csomagok hello:

        Install-Package Microsoft.Azure.Common.Authentication -Pre
        Install-Package Microsoft.Azure.Management.HDInsight -Pre
        Install-Package Microsoft.Azure.Management.Resources -Pre

3. A következő példakód hello használata:

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

## <a name="next-steps"></a>Következő lépések
* [Azure Active Directory-alkalmazás és a portál használatával egyszerű szolgáltatás létrehozása](../azure-resource-manager/resource-group-create-service-principal-portal.md)
* [Az Azure Resource Manager egyszerű hitelesítés](../azure-resource-manager/resource-group-authenticate-service-principal.md)
* [Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md)
