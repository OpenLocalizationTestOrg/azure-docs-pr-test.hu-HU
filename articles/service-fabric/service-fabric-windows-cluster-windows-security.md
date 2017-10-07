---
title: "a Windows rendszerbiztonság használatával a Windows rendszerű fürtre aaaSecure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure csomópontok és ügyfél-csomópont biztonsági önálló fürtön, a Windows rendszeren futó Windows biztonsági használatával."
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
ms.openlocfilehash: 44f3011eb630357f342052a48d6c852b17dccec4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="secure-a-standalone-cluster-on-windows-by-using-windows-security"></a><span data-ttu-id="10f67-103">Biztonságos Windows önálló fürtben, a Windows biztonsági</span><span class="sxs-lookup"><span data-stu-id="10f67-103">Secure a standalone cluster on Windows by using Windows security</span></span>
<span data-ttu-id="10f67-104">tooprevent jogosulatlan hozzáférés tooa Service Fabric-fürt, biztosítania kell hello fürt.</span><span class="sxs-lookup"><span data-stu-id="10f67-104">tooprevent unauthorized access tooa Service Fabric cluster, you must secure hello cluster.</span></span> <span data-ttu-id="10f67-105">Biztonsági különösen fontos, termelési számítási feladatokhoz hello fürt futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="10f67-105">Security is especially important when hello cluster runs production workloads.</span></span> <span data-ttu-id="10f67-106">Ez a cikk ismerteti, hogyan tooconfigure csomópontok és ügyfél-csomópont biztonsági hello Windows biztonsági használatával *művelet* fájlt.</span><span class="sxs-lookup"><span data-stu-id="10f67-106">This article describes how tooconfigure node-to-node and client-to-node security by using Windows security in hello *ClusterConfig.JSON* file.</span></span>  <span data-ttu-id="10f67-107">hello folyamat toohello felel meg a biztonsági lépés konfigurálható [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="10f67-107">hello process corresponds toohello configure security step of [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span> <span data-ttu-id="10f67-108">Hogyan használja a Service Fabric a Windows biztonsági kapcsolatos további információkért lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="10f67-108">For more information about how Service Fabric uses Windows security, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

> [!NOTE]
> <span data-ttu-id="10f67-109">Érdemes hello kiválasztását-csomópontok biztonsági gondosan, mivel nem egy biztonsági választott tooanother fürt frissítése.</span><span class="sxs-lookup"><span data-stu-id="10f67-109">You should consider hello selection of node-to-node security carefully because there is no cluster upgrade from one security choice tooanother.</span></span> <span data-ttu-id="10f67-110">toochange hello biztonsági kiválasztása, hogy toorebuild hello teljes fürt.</span><span class="sxs-lookup"><span data-stu-id="10f67-110">toochange hello security selection, you have toorebuild hello full cluster.</span></span>
>
>

## <a name="configure-windows-security-using-gmsa"></a><span data-ttu-id="10f67-111">Csoportosan felügyelt szolgáltatásfiókot használó Windows biztonságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="10f67-111">Configure Windows security using gMSA</span></span>  
<span data-ttu-id="10f67-112">hello minta *ClusterConfig.gMSA.Windows.MultiMachine.JSON* konfigurációs fájl hello együtt letöltött [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmaz egy sablon konfigurálásához a Windows biztonsági használatával [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span><span class="sxs-lookup"><span data-stu-id="10f67-112">hello sample *ClusterConfig.gMSA.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security using [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx):</span></span>  

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
  
| <span data-ttu-id="10f67-113">**Konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="10f67-113">**Configuration Setting**</span></span> | <span data-ttu-id="10f67-114">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="10f67-114">**Description**</span></span> |  
| --- | --- |  
| <span data-ttu-id="10f67-115">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="10f67-115">WindowsIdentities</span></span> |<span data-ttu-id="10f67-116">Hello fürt és az ügyfél identitások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="10f67-116">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="10f67-117">ClustergMSAIdentity</span><span class="sxs-lookup"><span data-stu-id="10f67-117">ClustergMSAIdentity</span></span> |<span data-ttu-id="10f67-118">Csomópont-csomópont biztonsági konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="10f67-118">Configures node-to-node security.</span></span> <span data-ttu-id="10f67-119">Egy csoport felügyelt szolgáltatásfiók.</span><span class="sxs-lookup"><span data-stu-id="10f67-119">A group managed service account.</span></span> |  
| <span data-ttu-id="10f67-120">ClusterSPN</span><span class="sxs-lookup"><span data-stu-id="10f67-120">ClusterSPN</span></span> |<span data-ttu-id="10f67-121">A csoportosan felügyelt szolgáltatásfiók SPN a teljesen minősített tartománynév</span><span class="sxs-lookup"><span data-stu-id="10f67-121">Fully qualified domain SPN for gMSA account</span></span>|  
| <span data-ttu-id="10f67-122">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="10f67-122">ClientIdentities</span></span> |<span data-ttu-id="10f67-123">Konfigurálja az ügyfél-csomópont biztonsági.</span><span class="sxs-lookup"><span data-stu-id="10f67-123">Configures client-to-node security.</span></span> <span data-ttu-id="10f67-124">Felhasználói fiókok tömb.</span><span class="sxs-lookup"><span data-stu-id="10f67-124">An array of client user accounts.</span></span> |  
| <span data-ttu-id="10f67-125">Identitás</span><span class="sxs-lookup"><span data-stu-id="10f67-125">Identity</span></span> |<span data-ttu-id="10f67-126">hello ügyfelek identitását, a tartományi felhasználók.</span><span class="sxs-lookup"><span data-stu-id="10f67-126">hello client identity, a domain user.</span></span> |  
| <span data-ttu-id="10f67-127">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="10f67-127">IsAdmin</span></span> |<span data-ttu-id="10f67-128">IGAZ, akkor hello tartományi felhasználónak rendszergazdai hozzáféréssel rendelkezik az ügyfél, a felhasználó ügyfél-hozzáférési hamis megadása</span><span class="sxs-lookup"><span data-stu-id="10f67-128">True specifies that hello domain user has administrator client access, false for user client access.</span></span> |  
  
<span data-ttu-id="10f67-129">[Csomópont toonode biztonsági](service-fabric-cluster-security.md#node-to-node-security) úgy, hogy van-e konfigurálva **ClustergMSAIdentity** Ha szüksége van a service fabric toorun csoportosan felügyelt szolgáltatásfiók alatt.</span><span class="sxs-lookup"><span data-stu-id="10f67-129">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting **ClustergMSAIdentity** when service fabric needs toorun under gMSA.</span></span> <span data-ttu-id="10f67-130">A sorrend toobuild megbízhatósági kapcsolatokat csomópontok között is el kell tisztában legyen egymással.</span><span class="sxs-lookup"><span data-stu-id="10f67-130">In order toobuild trust relationships between nodes, they must be made aware of each other.</span></span> <span data-ttu-id="10f67-131">A két különböző módon lehet elvégezni: Adja meg a csoportosan felügyelt szolgáltatásfiók, amely tartalmazza az összes csomópont hello fürt hello vagy hello tartományhoz gép csoport, amely tartalmazza az összes csomópont hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="10f67-131">This can be accomplished in two different ways: Specify hello Group Managed Service Account that includes all nodes in hello cluster or Specify hello domain machine group that includes all nodes in hello cluster.</span></span> <span data-ttu-id="10f67-132">Határozottan javasoljuk hello [csoportosan felügyelt szolgáltatásfiók (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) módszert használja, különösen a nagyobb fürtökkel (több mint 10 csomópontok) vagy fürtök valószínűleg toogrow vagy zsugorítani.</span><span class="sxs-lookup"><span data-stu-id="10f67-132">We strongly recommend using hello [Group Managed Service Account (gMSA)](https://technet.microsoft.com/library/hh831782.aspx) approach, particularly for larger clusters (more than 10 nodes) or for clusters that are likely toogrow or shrink.</span></span>  
<span data-ttu-id="10f67-133">Ez a módszer nem követeli meg egy tartományi csoport, amelynek a fürt rendszergazdák hozzáférési jogok tooadd rendelkezik, és tagok eltávolítása hello létrehozását.</span><span class="sxs-lookup"><span data-stu-id="10f67-133">This approach does not require hello creation of a domain group for which cluster administrators have been granted access rights tooadd and remove members.</span></span> <span data-ttu-id="10f67-134">Ezek a fiókok automatikus jelszókezelés is hasznosak.</span><span class="sxs-lookup"><span data-stu-id="10f67-134">These accounts are also useful for automatic password management.</span></span> <span data-ttu-id="10f67-135">További információkért lásd: [Ismerkedés a csoportosan felügyelt szolgáltatásfiókok](http://technet.microsoft.com/library/jj128431.aspx).</span><span class="sxs-lookup"><span data-stu-id="10f67-135">For more information, see [Getting Started with Group Managed Service Accounts](http://technet.microsoft.com/library/jj128431.aspx).</span></span>  
 
<span data-ttu-id="10f67-136">[Ügyfél toonode biztonsági](service-fabric-cluster-security.md#client-to-node-security) segítségével konfigurálható: **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="10f67-136">[Client toonode security](service-fabric-cluster-security.md#client-to-node-security) is configured using **ClientIdentities**.</span></span> <span data-ttu-id="10f67-137">A sorrend tooestablish közötti megbízhatósági kapcsolat egy ügyfél és a hello fürt konfigurálnia kell hello fürt tooknow melyik ügyfél identitások megbízhatónak is.</span><span class="sxs-lookup"><span data-stu-id="10f67-137">In order tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow which client identities that it can trust.</span></span> <span data-ttu-id="10f67-138">Ez kétféleképpen végezhető: Adja meg a hello tartományi csoport felhasználók, amelyek csatlakozni, vagy adjon meg hello csomópont Tartományfelhasználók csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="10f67-138">This can be done in two different ways: Specify hello domain group users that can connect or specify hello domain node users that can connect.</span></span> <span data-ttu-id="10f67-139">A Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói.</span><span class="sxs-lookup"><span data-stu-id="10f67-139">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="10f67-140">Hozzáférés-vezérlés segítségével hello hello a fürt rendszergazdai toolimit hozzáférés toocertain típusú különböző csoportok számára, így biztonságosabb hello fürt a fürt működését.</span><span class="sxs-lookup"><span data-stu-id="10f67-140">Access control provides hello ability for hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, making hello cluster more secure.</span></span>  <span data-ttu-id="10f67-141">A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="10f67-141">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="10f67-142">Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="10f67-142">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span> <span data-ttu-id="10f67-143">A hozzáférés-vezérlést további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric-ügyfelek](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="10f67-143">For more information on access controls, see [Role based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>  
 
<span data-ttu-id="10f67-144">hello a következő példa **biztonsági** szakasz csoportosan felügyelt szolgáltatásfiókot használó Windows biztonsági konfigurálja, és megadja a gépek adott hello *ServiceFabric.clusterA.contoso.com* csoportosan felügyelt szolgáltatásfiók hello fürt, és hogy részét képezik. *CONTOSO\usera* felügyeleti ügyfél hozzáfér:</span><span class="sxs-lookup"><span data-stu-id="10f67-144">hello following example **security** section configures Windows security using gMSA and specifies that hello machines in *ServiceFabric.clusterA.contoso.com* gMSA are part of hello cluster and that *CONTOSO\usera* has admin client access:</span></span>  
  
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
  
## <a name="configure-windows-security-using-a-machine-group"></a><span data-ttu-id="10f67-145">A számítógép csoport használata Windows biztonságának konfigurálása</span><span class="sxs-lookup"><span data-stu-id="10f67-145">Configure Windows security using a machine group</span></span>  
<span data-ttu-id="10f67-146">hello minta *ClusterConfig.Windows.MultiMachine.JSON* konfigurációs fájl hello együtt letöltött [Microsoft.Azure.ServiceFabric.WindowsServer.<version>. zip](http://go.microsoft.com/fwlink/?LinkId=730690) önálló fürt csomag tartalmazza a sablon a Windows biztonsági beállításainak megadása.</span><span class="sxs-lookup"><span data-stu-id="10f67-146">hello sample *ClusterConfig.Windows.MultiMachine.JSON* configuration file downloaded with hello [Microsoft.Azure.ServiceFabric.WindowsServer.<version>.zip](http://go.microsoft.com/fwlink/?LinkId=730690) standalone cluster package contains a template for configuring Windows security.</span></span>  <span data-ttu-id="10f67-147">Windows biztonsági beállítások konfigurálása a hello **tulajdonságok** szakasz:</span><span class="sxs-lookup"><span data-stu-id="10f67-147">Windows security is configured in hello **Properties** section:</span></span> 

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

| <span data-ttu-id="10f67-148">**Konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="10f67-148">**Configuration setting**</span></span> | <span data-ttu-id="10f67-149">**Leírás**</span><span class="sxs-lookup"><span data-stu-id="10f67-149">**Description**</span></span> |
| --- | --- |
| <span data-ttu-id="10f67-150">ClusterCredentialType</span><span class="sxs-lookup"><span data-stu-id="10f67-150">ClusterCredentialType</span></span> |<span data-ttu-id="10f67-151">**ClusterCredentialType** értéke túl*Windows* Ha ClusterIdentity határoz meg egy Active Directory gép csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="10f67-151">**ClusterCredentialType** is set too*Windows* if ClusterIdentity specifies an Active Directory Machine Group Name.</span></span> |  
| <span data-ttu-id="10f67-152">ServerCredentialType</span><span class="sxs-lookup"><span data-stu-id="10f67-152">ServerCredentialType</span></span> |<span data-ttu-id="10f67-153">Állítsa be a túl*Windows* tooenable az ügyfelek Windows biztonsági.</span><span class="sxs-lookup"><span data-stu-id="10f67-153">Set too*Windows* tooenable Windows security for clients.</span></span><br /><br /><span data-ttu-id="10f67-154">Ez azt jelzi, hogy hello ügyfelek hello és hello fürtön magát az Active Directory-tartományban futnak.</span><span class="sxs-lookup"><span data-stu-id="10f67-154">This indicates that hello clients of hello cluster and hello cluster itself are running within an Active Directory domain.</span></span> |  
| <span data-ttu-id="10f67-155">WindowsIdentities</span><span class="sxs-lookup"><span data-stu-id="10f67-155">WindowsIdentities</span></span> |<span data-ttu-id="10f67-156">Hello fürt és az ügyfél identitások tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="10f67-156">Contains hello cluster and client identities.</span></span> |  
| <span data-ttu-id="10f67-157">ClusterIdentity</span><span class="sxs-lookup"><span data-stu-id="10f67-157">ClusterIdentity</span></span> |<span data-ttu-id="10f67-158">A számítógép csoport neve, domain\machinegroup, tooconfigure-csomópontok biztonsági használja.</span><span class="sxs-lookup"><span data-stu-id="10f67-158">Use a machine group name, domain\machinegroup, tooconfigure node-to-node security.</span></span> |  
| <span data-ttu-id="10f67-159">ClientIdentities</span><span class="sxs-lookup"><span data-stu-id="10f67-159">ClientIdentities</span></span> |<span data-ttu-id="10f67-160">Konfigurálja az ügyfél-csomópont biztonsági.</span><span class="sxs-lookup"><span data-stu-id="10f67-160">Configures client-to-node security.</span></span> <span data-ttu-id="10f67-161">Felhasználói fiókok tömb.</span><span class="sxs-lookup"><span data-stu-id="10f67-161">An array of client user accounts.</span></span> |  
| <span data-ttu-id="10f67-162">Identitás</span><span class="sxs-lookup"><span data-stu-id="10f67-162">Identity</span></span> |<span data-ttu-id="10f67-163">Adja hozzá a hello tartományi felhasználót, tartomány\felhasználónév, az ügyfél-azonosító hello.</span><span class="sxs-lookup"><span data-stu-id="10f67-163">Add hello domain user, domain\username, for hello client identity.</span></span> |  
| <span data-ttu-id="10f67-164">IsAdmin</span><span class="sxs-lookup"><span data-stu-id="10f67-164">IsAdmin</span></span> |<span data-ttu-id="10f67-165">Set tootrue toospecify, hogy a tartományi felhasználó hello rendszergazda ügyfélelérési és hamis értéket, a felhasználó ügyfél-hozzáférési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="10f67-165">Set tootrue toospecify that hello domain user has administrator client access or false for user client access.</span></span> |  

<span data-ttu-id="10f67-166">[Csomópont toonode biztonsági](service-fabric-cluster-security.md#node-to-node-security) konfigurálható beállítás **ClusterIdentity** Ha azt szeretné, hogy az Active Directory-tartományba tartozó számítógép csoport toouse.</span><span class="sxs-lookup"><span data-stu-id="10f67-166">[Node toonode security](service-fabric-cluster-security.md#node-to-node-security) is configured by setting using **ClusterIdentity** if you want toouse a machine group within an Active Directory Domain.</span></span> <span data-ttu-id="10f67-167">További információkért lásd: [létrehoz egy gép csoportot az Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span><span class="sxs-lookup"><span data-stu-id="10f67-167">For more information, see [Create a Machine Group in Active Directory](https://msdn.microsoft.com/library/aa545347(v=cs.70).aspx).</span></span>

<span data-ttu-id="10f67-168">[Ügyfél-csomópont biztonsági](service-fabric-cluster-security.md#client-to-node-security) konfigurálható **ClientIdentities**.</span><span class="sxs-lookup"><span data-stu-id="10f67-168">[Client-to-node security](service-fabric-cluster-security.md#client-to-node-security) is configured by using **ClientIdentities**.</span></span> <span data-ttu-id="10f67-169">egy ügyfél és a hello fürt közötti megbízhatóság tooestablish hello fürt tooknow hello ügyfél identitásokat tartalmaz, amelyek a fürt hello is megbízhat kell konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="10f67-169">tooestablish trust between a client and hello cluster, you must configure hello cluster tooknow hello client identities that hello cluster can trust.</span></span> <span data-ttu-id="10f67-170">Megbízhatósági kapcsolat két különböző módon is létrehozása:</span><span class="sxs-lookup"><span data-stu-id="10f67-170">You can establish trust in two different ways:</span></span>

- <span data-ttu-id="10f67-171">Adja meg a hello tartományi csoport felhasználók csatlakozhatnak.</span><span class="sxs-lookup"><span data-stu-id="10f67-171">Specify hello domain group users that can connect.</span></span>
- <span data-ttu-id="10f67-172">Adja meg a hello csomópont Tartományfelhasználók csatlakoztatható.</span><span class="sxs-lookup"><span data-stu-id="10f67-172">Specify hello domain node users that can connect.</span></span>

<span data-ttu-id="10f67-173">A Service Fabric két különböző hozzáférést vezérlő típusokat támogatja az ügyfelek, amelyek csatlakoztatott tooa Service Fabric-fürt: rendszergazdai és felhasználói.</span><span class="sxs-lookup"><span data-stu-id="10f67-173">Service Fabric supports two different access control types for clients that are connected tooa Service Fabric cluster: administrator and user.</span></span> <span data-ttu-id="10f67-174">Hozzáférés-vezérlés lehetővé teszi, hogy hello fürt rendszergazdája toolimit hozzáférés toocertain típusú fürtműveletekben különböző csoportok számára, így hello fürt biztonságosabb.</span><span class="sxs-lookup"><span data-stu-id="10f67-174">Access control enables hello cluster administrator toolimit access toocertain types of cluster operations for different groups of users, which makes hello cluster more secure.</span></span>  <span data-ttu-id="10f67-175">A rendszergazdák teljes hozzáféréssel toomanagement képességek (beleértve az olvasási/írási képességek) rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="10f67-175">Administrators have full access toomanagement capabilities (including read/write capabilities).</span></span> <span data-ttu-id="10f67-176">Felhasználók, alapértelmezés szerint rendelkezik csak olvasási hozzáféréssel toomanagement képességek (például lekérdezési lehetőségek), és hello képességét tooresolve alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="10f67-176">Users, by default, have only read access toomanagement capabilities (for example, query capabilities), and hello ability tooresolve applications and services.</span></span>  

<span data-ttu-id="10f67-177">hello a következő példa **biztonsági** szakasz konfigurálja a Windows biztonsági, adja meg a gépek adott hello *ServiceFabric/clusterA.contoso.com* hello fürt részét képezik, és határozza meg, hogy  *CONTOSO\usera* felügyeleti ügyfél hozzáfér:</span><span class="sxs-lookup"><span data-stu-id="10f67-177">hello following example **security** section configures Windows security, specifies that hello machines in *ServiceFabric/clusterA.contoso.com* are part of hello cluster, and specifies that *CONTOSO\usera* has admin client access:</span></span>

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
> <span data-ttu-id="10f67-178">Service Fabric egy olyan tartományvezérlőre nem telepíthető.</span><span class="sxs-lookup"><span data-stu-id="10f67-178">Service Fabric should not be deployed on a domain controller.</span></span> <span data-ttu-id="10f67-179">Győződjön meg arról, hogy nincs-e hello IP-cím hello tartományvezérlő tartalmazhat, ha a gép csoportjával művelet, vagy csoportos felügyelt szolgáltatásfiók (gMSA).</span><span class="sxs-lookup"><span data-stu-id="10f67-179">Make sure that ClusterConfig.json does not include hello IP address of hello domain controller when using a machine group or group Managed Service Account (gMSA).</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="10f67-180">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10f67-180">Next steps</span></span>
<span data-ttu-id="10f67-181">Windows biztonsági hello beállítása után *művelet* hello fürtlétrehozás a memóriakihasználtság, a fájl [létrehozása a Windows rendszert futtató önálló fürtön](service-fabric-cluster-creation-for-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="10f67-181">After configuring Windows security in hello *ClusterConfig.JSON* file, resume hello cluster creation process in [Create a standalone cluster running on Windows](service-fabric-cluster-creation-for-windows-server.md).</span></span>

<span data-ttu-id="10f67-182">További információ a hogyan-csomópontok biztonsági, az ügyfél-csomópont biztonsági és a szerepköralapú hozzáférés-vezérlés, lásd: [fürtök biztonsági forgatókönyveinek](service-fabric-cluster-security.md).</span><span class="sxs-lookup"><span data-stu-id="10f67-182">For more information about how node-to-node security, client-to-node security, and role-based access control, see [Cluster security scenarios](service-fabric-cluster-security.md).</span></span>

<span data-ttu-id="10f67-183">Lásd: [Connect tooa biztonságos fürt](service-fabric-connect-to-secure-cluster.md) példák a PowerShell vagy a FabricClient keresztül kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="10f67-183">See [Connect tooa secure cluster](service-fabric-connect-to-secure-cluster.md) for examples of connecting by using PowerShell or FabricClient.</span></span>
