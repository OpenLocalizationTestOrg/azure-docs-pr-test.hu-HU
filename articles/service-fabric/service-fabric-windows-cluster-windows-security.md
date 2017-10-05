---
title: "Biztonságos Windows biztonsági segítségével Windows rendszerű fürtre |} Microsoft Docs"
description: "Útmutató a Windows rendszerbiztonság használatával a Windows futtató önálló fürtön található csomópontok és ügyfél-csomópont biztonságának konfigurálása."
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: ce3bf686-ffc4-452f-b15a-3c812aa9e672
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/24/2017
ms.author: dekapur
ms.openlocfilehash: e093a631b0cf81195981a8e3d345504ebce02723
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="dc2d7-103">Biztonságos Windows önálló fürtben, a Windows biztonsági</span><span class="sxs-lookup"><span data-stu-id="dc2d7-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="dc2d7-104">A jogosulatlan hozzáférés elkerülése érdekében a Service Fabric-fürt, biztosítania kell a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-104">To prevent unauthorized access to a Service Fabric cluster, you must secure the cluster.</span></span> <span data-ttu-id="dc2d7-105">Biztonsági különösen fontos, amikor a fürt fut termelési számítási feladatokhoz.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-105">Security is especially important when the cluster runs production workloads.</span></span> <span data-ttu-id="dc2d7-106">Ez a cikk ismerteti, hogyan csomópontok és ügyfél-csomópont biztonsági konfigurálása a Windows biztonsági használatával a *művelet* fájlt.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-106">This article describes how to configure node-to-node and client-to-node security by using Windows security in the *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="dc2d7-107">A folyamat megfelel-e a konfigurálás biztonsági lépés [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-107">The process corresponds to the configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="dc2d7-108">Hogyan használja a Service Fabric a Windows biztonsági kapcsolatos további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="dc2d7-109">Vegye figyelembe a kijelölés-csomópontok biztonsági gondosan, mivel nincs Fürtfrissítés egy biztonsági választás a másikra.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-109">You should consider the selection of node-to-node security carefully because there is no cluster upgrade from one security choice to another.</span></span> <span data-ttu-id="dc2d7-110">A biztonsági rendszer beállításának módosításához be kell, hogy a teljes fürtöt.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-110">To change the security selection, you have to rebuild the full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="dc2d7-111">Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc2d7-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="dc2d7-112">A minta *ClusterConfig.gMSA.Windows.MultiMachine.JSON* együtt letöltött konfigurációs fájlt a [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmaz egy sablon konfigurálásához a Windows biztonsági használatával [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="dc2d7-112">The sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

```  
"security": {  
            "WindowsIdentities": {  
                "ClustergMSAIdentity": "accountname@fqdn"  
                "ClusterSPN": "fqdn"  
                "ClientIdentities": [  
                    {  
                        "Identity": "domain\\username",  
                        "IsAdmin": true  
                    }  
                ]  
            }  
        }  
```  
  
| <span data-ttu-id="dc2d7-113">**Konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="dc2d7-113">**Configuration Setting**</span></span> | <span data-ttu-id="dc2d7-114">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="dc2d7-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="dc2d7-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="dc2d7-115">WindowsIdentities</span></span> |<span data-ttu-id="dc2d7-116">A fürt és az ügyfél identitások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-116">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="dc2d7-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="dc2d7-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="dc2d7-118">Csomópont-csomópont biztonsági konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-118">Configures node-to-node security.</span></span> <span data-ttu-id="dc2d7-119">Egy csoport felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-119">A group managed service account.</span></span> |  
| <span data-ttu-id="dc2d7-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="dc2d7-120">ClusterSPN</span></span> |<span data-ttu-id="dc2d7-121">A csoportosan felügyelt szolgáltatásfiók SPN a teljesen minősített tartománynév</span><span class="sxs-lookup"><span data-stu-id="dc2d7-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="dc2d7-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="dc2d7-122">ClientIdentities</span></span> |<span data-ttu-id="dc2d7-123">Konfigurálja az ügyfél-csomópont biztonsági.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-123">Configures client-to-node security.</span></span> <span data-ttu-id="dc2d7-124">Felhasználói fiókok tömb.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="dc2d7-125">Identitás</span><span class="sxs-lookup"><span data-stu-id="dc2d7-125">Identity</span></span> |<span data-ttu-id="dc2d7-126">Az ügyfelek identitását, a tartományi felhasználók.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-126">The client identity, a domain user.</span></span> |  
| <span data-ttu-id="dc2d7-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="dc2d7-127">IsAdmin</span></span> |<span data-ttu-id="dc2d7-128">Igaz határozza meg, hogy a tartományi felhasználó rendszergazdai hozzáféréssel rendelkezik az ügyfél, a felhasználó ügyfél-hozzáférési hamis.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-128">True specifies that the domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="dc2d7-129">[Csomópont biztonsági csomópont](service-fabric-cluster-security.md#node-to-node-security) úgy, hogy van-e konfigurálva **ClustergMSAIdentity** amikor a service fabric kell futtatni a csoportosan felügyelt szolgáltatásfiók alatt.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-129">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs to run under gMSA.</span></span> <span data-ttu-id="dc2d7-130">Ahhoz, hogy a csomópontok közötti bizalmi kapcsolatok készítéséhez is el minden más kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-130">In order to build trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="dc2d7-131">A két különböző módon lehet elvégezni: Adja meg a csoportosan felügyelt szolgáltatásfiók, amely a fürt összes csomópontján tartalmazza, vagy adja meg a tartományhoz gép, amely a fürt összes csomópontján tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-131">This can be accomplished in two different ways: Specify the Group Managed Service Account that includes all nodes in the cluster or Specify the domain machine group that includes all nodes in the cluster.</span></span> <span data-ttu-id="dc2d7-132">Határozottan javasoljuk a [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) módszert használja, különösen a nagyobb fürtökkel (több mint 10 csomópontok) vagy a fürtök, amelyek valószínűleg növekedhet és csökkenhet.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-132">We strongly recommend using the [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely to grow or shrink.</span></span>  
<span data-ttu-id="dc2d7-133">Ez a megközelítés nem követeli meg, amelynek fürt rendszergazdák rendelkezik hozzáférési jogosultsággal hozzáadhat és eltávolíthat tagokat tartományi csoport létrehozását.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-133">This approach does not require the creation of a domain group for which cluster administrators have been granted access rights to add and remove members.</span></span> <span data-ttu-id="dc2d7-134">Ezek a fiókok automatikus jelszókezelés is hasznosak.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="dc2d7-135">További információkért lásd: [Ismerkedés a csoportosan felügyelt szolgáltatásfiókok](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="dc2d7-136">[A csomópont security ügyfél](service-fabric-cluster-security.md#client-to-node-security) segítségével konfigurálható: **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-136">[Client to node security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="dc2d7-137">Ahhoz, hogy az ügyfél és a fürt közötti megbízhatósági kapcsolat létrehozása, konfigurálnia kell a fürt tudni, hogy melyik ügyfél identitások megbízhatónak is.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-137">In order to establish trust between a client and the cluster, you must configure the cluster to know which client identities that it can trust.</span></span> <span data-ttu-id="dc2d7-138">Ez kétféleképpen végezhető: Adja meg a tartományi felhasználók csoporthoz, amely képes csatlakozni, vagy adja meg a csomópont Tartományfelhasználók csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-138">This can be done in two different ways: Specify the domain group users that can connect or specify the domain node users that can connect.</span></span> <span data-ttu-id="dc2d7-139">Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek csatlakoznak a Service Fabric-fürt: rendszergazdai és felhasználói.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-139">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="dc2d7-140">Hozzáférés-vezérlés lehetővé teszi az a fürt a rendszergazdák korlátozhatják bizonyos típusú, különböző csoportok számára, így nagyobb biztonságot nyújt a fürt a fürt működését.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-140">Access control provides the ability for the cluster administrator to limit access to certain types of cluster operations for different groups of users, making the cluster more secure.</span></span>  <span data-ttu-id="dc2d7-141">Rendszergazdák (beleértve az olvasási/írási képességek) felügyeleti képességek teljes hozzáféréssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-141">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="dc2d7-142">Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáférés kezelési képességek (például lekérdezési lehetőségek), és oldja meg az alkalmazások és szolgáltatások képességét.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-142">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span> <span data-ttu-id="dc2d7-143">A hozzáférés-vezérlést további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="dc2d7-144">Az alábbi példa **biztonsági** szakasz csoportosan felügyelt szolgáltatásfiókot használó Windows biztonsági konfigurálja, és azt jelenti, hogy a gépek *ServiceFabric.clusterA.contoso.com* csoportosan felügyelt szolgáltatásfiók a fürt és a részétképezik. *CONTOSO\usera* felügyeleti ügyfél hozzáfér:</span><span class="sxs-lookup"><span data-stu-id="dc2d7-144">The following example **security** section configures Windows security using gMSA and specifies that the machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of the cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
```  
"security": {  
    "WindowsIdentities": {  
        "ClustergMSAIdentity" : "ServiceFabric.clusterA.contoso.com",  
        "ClusterSPN" : "clusterA.contoso.com",  
        "ClientIdentities": [{  
            "Identity": "CONTOSO\\usera",  
            "IsAdmin": true  
        }]  
    }  
}  
```  
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="dc2d7-145">A számítógép csoport használata Windows biztonságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="dc2d7-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="dc2d7-146">A minta *ClusterConfig.Windows.MultiMachine.JSON* együtt letöltött konfigurációs fájlt a [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmazza a sablon a Windows biztonsági beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-146">The sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with the [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="dc2d7-147">Windows biztonsági konfigurálva van a **tulajdonságok** szakasz:</span><span class="sxs-lookup"><span data-stu-id="dc2d7-147">Windows security is configured in the **Properties** section:</span></span> 

```
"security": {
            "ClusterCredentialType": "Windows",
            "ServerCredentialType": "Windows",
            "WindowsIdentities": {
                "ClusterIdentity" : "[domain\machinegroup]",
                "ClientIdentities": [{
                    "Identity": "[domain\username]",
                    "IsAdmin": true
                }]
            }
        }
```

| <span data-ttu-id="dc2d7-148">**Konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="dc2d7-148">**Configuration setting**</span></span> | <span data-ttu-id="dc2d7-149">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="dc2d7-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="dc2d7-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="dc2d7-150">ClusterCredentialType</span></span> |<span data-ttu-id="dc2d7-151">**ClusterCredentialType** értéke *Windows* Ha ClusterIdentity határoz meg egy Active Directory gép csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-151">**ClusterCredentialType** is set to *Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="dc2d7-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="dc2d7-152">ServerCredentialType</span></span> |<span data-ttu-id="dc2d7-153">Beállítása *Windows* az ügyfelek Windows biztonsági engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-153">Set to *Windows* to enable Windows security for clients.</span></span><br /><br /><span data-ttu-id="dc2d7-154">Ez azt jelzi, hogy az ügyfelek a fürt és egyrészt a fürt Active Directory-tartományban futnak.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-154">This indicates that the clients of the cluster and the cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="dc2d7-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="dc2d7-155">WindowsIdentities</span></span> |<span data-ttu-id="dc2d7-156">A fürt és az ügyfél identitások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-156">Contains the cluster and client identities.</span></span> |  
| <span data-ttu-id="dc2d7-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="dc2d7-157">ClusterIdentity</span></span> |<span data-ttu-id="dc2d7-158">Használja a gép csoportnév domain\machinegroup,-csomópontok biztonsági konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-158">Use a machine group name, domain\machinegroup, to configure node-to-node security.</span></span> |  
| <span data-ttu-id="dc2d7-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="dc2d7-159">ClientIdentities</span></span> |<span data-ttu-id="dc2d7-160">Konfigurálja az ügyfél-csomópont biztonsági.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-160">Configures client-to-node security.</span></span> <span data-ttu-id="dc2d7-161">Felhasználói fiókok tömb.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="dc2d7-162">Identitás</span><span class="sxs-lookup"><span data-stu-id="dc2d7-162">Identity</span></span> |<span data-ttu-id="dc2d7-163">Adja hozzá a tartományi felhasználót, a tartomány\felhasználónév, az ügyfél-azonosító.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-163">Add the domain user, domain\username, for the client identity.</span></span> |  
| <span data-ttu-id="dc2d7-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="dc2d7-164">IsAdmin</span></span> |<span data-ttu-id="dc2d7-165">Adhatja meg, hogy a tartományi felhasználó rendelkezik-e a rendszergazda ügyfélelérési és hamis értéket, a felhasználó ügyfél-hozzáférési igaz értékre állítani.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-165">Set to true to specify that the domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="dc2d7-166">[Csomópont biztonsági csomópont](service-fabric-cluster-security.md#node-to-node-security) konfigurálható beállítás **ClusterIdentity** Ha szeretné használni a számítógép csoport az Active Directory-tartományban.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-166">[Node to node security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want to use a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="dc2d7-167">További információkért lásd: [létrehoz egy gép csoportot az Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="dc2d7-168">[Ügyfél-csomópont biztonsági](service-fabric-cluster-security.md#client-to-node-security) konfigurálható **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="dc2d7-169">Ügyfél és a fürt közötti megbízhatósági kapcsolat létrehozása, konfigurálnia kell a fürt tudni, hogy az ügyfél identitásokat tartalmaz, amelyek a fürt is megbízhat.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-169">To establish trust between a client and the cluster, you must configure the cluster to know the client identities that the cluster can trust.</span></span> <span data-ttu-id="dc2d7-170">Megbízhatósági kapcsolat két különböző módon is létrehozása:</span><span class="sxs-lookup"><span data-stu-id="dc2d7-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="dc2d7-171">Adja meg a tartományi felhasználók csoporthoz, amely képes kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-171">Specify the domain group users that can connect.</span></span>
- <span data-ttu-id="dc2d7-172">Adja meg a csomópont Tartományfelhasználók csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-172">Specify the domain node users that can connect.</span></span>

<span data-ttu-id="dc2d7-173">Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek csatlakoznak a Service Fabric-fürt: rendszergazdai és felhasználói.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-173">Service Fabric supports two different access control types for clients that are connected to a Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="dc2d7-174">Hozzáférés-vezérlés lehetővé teszi, hogy a fürt rendszergazdája korlátozhatják bizonyos típusú fürtműveletekben különböző csoportok számára, amely biztonságosabbá teszi a fürt.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-174">Access control enables the cluster administrator to limit access to certain types of cluster operations for different groups of users, which makes the cluster more secure.</span></span>  <span data-ttu-id="dc2d7-175">Rendszergazdák (beleértve az olvasási/írási képességek) felügyeleti képességek teljes hozzáféréssel rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-175">Administrators have full access to management capabilities (including read/write capabilities).</span></span> <span data-ttu-id="dc2d7-176">Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáférés kezelési képességek (például lekérdezési lehetőségek), és oldja meg az alkalmazások és szolgáltatások képességét.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-176">Users, by default, have only read access to management capabilities (for example, query capabilities), and the ability to resolve applications and services.</span></span>  

<span data-ttu-id="dc2d7-177">Az alábbi példa **biztonsági** szakasz konfigurálja a Windows biztonsági, azt jelenti, hogy a gépek *ServiceFabric/clusterA.contoso.com* a fürt részét képezik, és határozza meg, hogy *CONTOSO\usera* felügyeleti ügyfél hozzáfér:</span><span class="sxs-lookup"><span data-stu-id="dc2d7-177">The following example **security** section configures Windows security, specifies that the machines in *ServiceFabric/clusterA.contoso.com* are part of the cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

```
"security": {
    "ClusterCredentialType": "Windows",
    "ServerCredentialType": "Windows",
    "WindowsIdentities": {
        "ClusterIdentity" : "ServiceFabric/clusterA.contoso.com",
        "ClientIdentities": [{
            "Identity": "CONTOSO\\usera",
            "IsAdmin": true
        }]
    }
},
```

> [!NOTE]
> <span data-ttu-id="dc2d7-178">Service Fabric egy olyan tartományvezérlőre nem telepíthető.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="dc2d7-179">Győződjön meg arról, hogy nincs-e a tartományvezérlő IP-címét tartalmazza, a gép csoport használata esetén művelet, vagy csoportos felügyelt szolgáltatásfiók (gMSA).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-179">Make sure that ClusterConfig.json does not include the IP address of the domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="dc2d7-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="dc2d7-180">Next steps</span></span>
<span data-ttu-id="dc2d7-181">A Windows biztonsági beállításainak megadása után a *művelet* fájlt, az alkalmazás Fürtlétrehozási folyamatának folytatásához [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-181">After configuring Windows security in the *ClusterConfig.JSON* file, resume the cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="dc2d7-182">További információ a hogyan-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés, lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="dc2d7-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="dc2d7-183">Lásd: [Csatlakozás biztonságos fürthöz](service-fabric-connect-to-secure-cluster.md) példák a PowerShell vagy a FabricClient keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="dc2d7-183">See [Connect to a secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
