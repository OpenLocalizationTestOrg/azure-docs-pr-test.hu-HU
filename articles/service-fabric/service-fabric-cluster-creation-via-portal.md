---
title: "a Service Fabric aaaCreate fürthöz hello Azure-portálon |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan Azure használatával biztonságos Service Fabric fürt tooset hello Azure-portál és az Azure Key Vault."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: vturecek
ms.assetid: 426c3d13-127a-49eb-a54c-6bde7c87a83b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/21/2017
ms.author: chackdan
ms.openlocfilehash: 045f71b491260e741ce7a54a75c440e1b33059a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-in-azure-using-hello-azure-portal"></a><span data-ttu-id="62e07-103">A Service Fabric-fürt létrehozása az Azure-ban hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="62e07-103">Create a Service Fabric cluster in Azure using hello Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="62e07-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="62e07-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="62e07-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="62e07-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
> 
> 

<span data-ttu-id="62e07-106">Ez a cikk részletesen ismerteti, amely végigvezeti Önt az Azure-ban az Azure-portálon hello biztonságos Service Fabric-fürt beállításának lépésein hello.</span><span class="sxs-lookup"><span data-stu-id="62e07-106">This is a step-by-step guide that walks you through hello steps of setting up a secure Service Fabric cluster in Azure using hello Azure portal.</span></span> <span data-ttu-id="62e07-107">Ez az útmutató bemutatja, hogyan hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="62e07-107">This guide walks you through hello following steps:</span></span>

* <span data-ttu-id="62e07-108">Key Vault toomanage kulcsok fürt a biztonság beállítása.</span><span class="sxs-lookup"><span data-stu-id="62e07-108">Set up Key Vault toomanage keys for cluster security.</span></span>
* <span data-ttu-id="62e07-109">Védett fürt létrehozása az Azure-ban hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="62e07-109">Create a secured cluster in Azure through hello Azure portal.</span></span>
* <span data-ttu-id="62e07-110">Rendszergazdák tanúsítványok segítségével hitelesíti.</span><span class="sxs-lookup"><span data-stu-id="62e07-110">Authenticate administrators using certificates.</span></span>

> [!NOTE]
> <span data-ttu-id="62e07-111">A speciális biztonsági beállítások, például a felhasználói hitelesítés az Azure Active Directory és az alkalmazás biztonsági tanúsítványok beállítását [Azure Resource Manager használatával a fürt létrehozása][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="62e07-111">For more advanced security options, such as user authentication with Azure Active Directory and setting up certificates for application security, [create your cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

<span data-ttu-id="62e07-112">A biztonságos fürtre a fürt, amely megakadályozza a jogosulatlan hozzáférés toomanagement műveletek, beleértve telepítése, frissítése és törlése, alkalmazások, szolgáltatások és bennük hello adatok.</span><span class="sxs-lookup"><span data-stu-id="62e07-112">A secure cluster is a cluster that prevents unauthorized access toomanagement operations, which includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="62e07-113">Egy nem biztonságos fürt olyan fürt, hogy bárki tooat bármikor csatlakozhat, és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="62e07-113">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="62e07-114">Bár lehetséges toocreate egy nem biztonságos fürt, akkor **erősen ajánlott toocreate biztonságos fürt**.</span><span class="sxs-lookup"><span data-stu-id="62e07-114">Although it is possible toocreate an unsecure cluster, it is **highly recommended toocreate a secure cluster**.</span></span> <span data-ttu-id="62e07-115">Egy nem biztonságos fürt **később nem védhető** -léteznie kell egy új fürtöt.</span><span class="sxs-lookup"><span data-stu-id="62e07-115">An unsecure cluster **cannot be secured later** - a new cluster must be created.</span></span>

<span data-ttu-id="62e07-116">hello fogalmak vannak hello ugyanazt a biztonságos fürtök létrehozásához, hogy hello fürtök Linux-fürtök és a Windows-fürtök.</span><span class="sxs-lookup"><span data-stu-id="62e07-116">hello concepts are hello same for creating secure clusters, whether hello clusters are Linux clusters or Windows clusters.</span></span> <span data-ttu-id="62e07-117">További információk és segítő parancsfájlok biztonságos Linux-fürtök létrehozásához, ellenőrizze a [biztonságos fürtök Linux rendszeren létrehozására](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="62e07-117">For more information and helper scripts for creating secure Linux clusters, please see [Creating secure clusters on Linux](service-fabric-cluster-creation-via-arm.md#secure-linux-clusters).</span></span> <span data-ttu-id="62e07-118">hello révén hello segítő parancsfájl a megadott paraméterek Igen közvetlenül be hello portálra hello szakaszban leírt módon [fürt létrehozása az Azure-portálon hello](#create-cluster-portal).</span><span class="sxs-lookup"><span data-stu-id="62e07-118">hello parameters obtained by hello helper script provided can be input directly into hello portal as described in hello section [Create a cluster in hello Azure portal](#create-cluster-portal).</span></span>

## <a name="log-in-tooazure"></a><span data-ttu-id="62e07-119">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="62e07-119">Log in tooAzure</span></span>
<span data-ttu-id="62e07-120">Ez az útmutató használ [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="62e07-120">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="62e07-121">Új PowerShell-munkamenet indításakor be tooyour Azure-fiókra, és jelölje ki az előfizetését az Azure parancsok végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="62e07-121">When starting a new PowerShell session, log in tooyour Azure account and select your subscription before executing Azure commands.</span></span>

<span data-ttu-id="62e07-122">Jelentkezzen be tooyour azure fiók:</span><span class="sxs-lookup"><span data-stu-id="62e07-122">Log in tooyour azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="62e07-123">Jelölje ki az előfizetését:</span><span class="sxs-lookup"><span data-stu-id="62e07-123">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-key-vault"></a><span data-ttu-id="62e07-124">A Key Vault beállítása</span><span class="sxs-lookup"><span data-stu-id="62e07-124">Set up Key Vault</span></span>
<span data-ttu-id="62e07-125">Ez a kijelző hello útmutató végigvezeti a kulcstároló, és a Service Fabric-alkalmazások számára az Azure Service Fabric-fürtök létrehozása.</span><span class="sxs-lookup"><span data-stu-id="62e07-125">This part of hello guide walks you through creating a Key Vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="62e07-126">A Key Vault teljes körű, lásd: hello [Key Vault – első lépések útmutató][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="62e07-126">For a complete guide on Key Vault, see hello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="62e07-127">A Service Fabric X.509 tanúsítvány toosecure egy fürt használja.</span><span class="sxs-lookup"><span data-stu-id="62e07-127">Service Fabric uses X.509 certificates toosecure a cluster.</span></span> <span data-ttu-id="62e07-128">Az Azure Key Vault az Azure Service Fabric-fürtök használt toomanage tanúsítványait.</span><span class="sxs-lookup"><span data-stu-id="62e07-128">Azure Key Vault is used toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="62e07-129">A fürt telepítésekor az Azure-ban, a Service Fabric-fürtök létrehozásáért felelős hello Azure erőforrás-szolgáltató kéri le a tanúsítványokat a Key Vault, és telepíti azokat a virtuális gépek hello fürtön.</span><span class="sxs-lookup"><span data-stu-id="62e07-129">When a cluster is deployed in Azure, hello Azure resource provider responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="62e07-130">hello következő diagram azt ábrázolja, hello kapcsolat kulcstároló, a Service Fabric-fürt és a hello Azure erőforrás-szolgáltató által használt tanúsítványok Key Vault tárolódnak, amikor létrehozza a fürt között:</span><span class="sxs-lookup"><span data-stu-id="62e07-130">hello following diagram illustrates hello relationship between Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in Key Vault when it creates a cluster:</span></span>

![Tanúsítvány telepítése][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="62e07-132">Erőforráscsoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="62e07-132">Create a Resource Group</span></span>
<span data-ttu-id="62e07-133">hello első lépés egy új erőforráscsoportot kimondottan a Key Vault toocreate.</span><span class="sxs-lookup"><span data-stu-id="62e07-133">hello first step is toocreate a new resource group specifically for Key Vault.</span></span> <span data-ttu-id="62e07-134">Key Vault üzembe a saját erőforráscsoport ajánlott, hogy a számítási és tárolási erőforráscsoportok – például a Service Fabric-fürt rendelkező hello erőforráscsoport - eltávolíthatja a kulcsok és titkos elvesztése nélkül.</span><span class="sxs-lookup"><span data-stu-id="62e07-134">Putting Key Vault into its own resource group is recommended so that you can remove compute and storage resource groups - such as hello resource group that has your Service Fabric cluster - without losing your keys and secrets.</span></span> <span data-ttu-id="62e07-135">a Key Vault rendelkező hello erőforráscsoport hello kell lennie és hello fürt által használt ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="62e07-135">hello resource group that has your Key Vault must be in hello same region as hello cluster that is using it.</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmResourceGroup -Name mycluster-keyvault -Location 'West US'
    WARNING: hello output object type of this cmdlet will be modified in a future release.

    ResourceGroupName : mycluster-keyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/mycluster-keyvault

```

### <a name="create-key-vault"></a><span data-ttu-id="62e07-136">Kulcstároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="62e07-136">Create Key Vault</span></span>
<span data-ttu-id="62e07-137">A kulcstároló hello új erőforráscsoport létrehozása.</span><span class="sxs-lookup"><span data-stu-id="62e07-137">Create a Key Vault in hello new resource group.</span></span> <span data-ttu-id="62e07-138">Key Vault hello **engedélyezni kell a központi telepítési** tooallow hello Service Fabric erőforrás szolgáltató tooget tanúsítványokat, és telepítse az fürtcsomóponton:</span><span class="sxs-lookup"><span data-stu-id="62e07-138">hello Key Vault **must be enabled for deployment** tooallow hello Service Fabric resource provider tooget certificates from it and install on cluster nodes:</span></span>

```powershell

    PS C:\Users\vturecek> New-AzureRmKeyVault -VaultName 'myvault' -ResourceGroupName 'mycluster-keyvault' -Location 'West US' -EnabledForDeployment


    Vault Name                       : myvault
    Resource Group Name              : mycluster-keyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault
    Vault URI                        : https://myvault.vault.azure.net
    Tenant ID                        : <guid>
    SKU                              : Standard
    Enabled For Deployment?          : False
    Enabled For Template Deployment? : False
    Enabled For Disk Encryption?     : False
    Access Policies                  :
                                       Tenant ID                :    <guid>
                                       Object ID                :    <guid>
                                       Application ID           :
                                       Display Name             :    
                                       Permissions tooKeys      :    get, create, delete, list, update, import, backup, restore
                                       Permissions tooSecrets   :    all


    Tags                             :
```

<span data-ttu-id="62e07-139">Ha egy meglévő kulcstároló, a központi telepítés Azure parancssori felület használatával engedélyezheti azt:</span><span class="sxs-lookup"><span data-stu-id="62e07-139">If you have an existing Key Vault, you can enable it for deployment using Azure CLI:</span></span>

```cli
> azure login
> azure account set "your account"
> azure config mode arm 
> azure keyvault list
> azure keyvault set-policy --vault-name "your vault name" --enabled-for-deployment true
```


## <a name="add-certificates-tookey-vault"></a><span data-ttu-id="62e07-140">Tanúsítványok tooKey tároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="62e07-140">Add certificates tooKey Vault</span></span>
<span data-ttu-id="62e07-141">A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="62e07-141">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="62e07-142">További információ a tanúsítványok használatának módját a Service Fabric: [Service Fabric-fürt biztonsági forgatókönyvek][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="62e07-142">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="62e07-143">Fürt és a kiszolgálói tanúsítványt (kötelező)</span><span class="sxs-lookup"><span data-stu-id="62e07-143">Cluster and server certificate (required)</span></span>
<span data-ttu-id="62e07-144">Ez a tanúsítvány szükséges toosecure fürt, és megakadályozza a jogosulatlan hozzáférés tooit.</span><span class="sxs-lookup"><span data-stu-id="62e07-144">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="62e07-145">Fürt biztonsági néhány módon tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="62e07-145">It provides cluster security in a couple ways:</span></span>

* <span data-ttu-id="62e07-146">**Fürt hitelesítési:** hitelesíti a fürt összevonási csomópontok kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="62e07-146">**Cluster authentication:** Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="62e07-147">Csak olyan csomópontot, amely bizonyítja, ezzel a tanúsítvánnyal az identitásukat hello fürt csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="62e07-147">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="62e07-148">**Kiszolgálóhitelesítés:** hitelesíti hello fürt felügyeleti végpontok tooa felügyeleti ügyfél, így hello felügyeleti ügyfél tudja azt toohello valós fürt van van szó.</span><span class="sxs-lookup"><span data-stu-id="62e07-148">**Server authentication:** Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="62e07-149">Ez a tanúsítvány is biztosít SSL hello HTTPS felügyeleti API és a Service Fabric Explorer HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="62e07-149">This certificate also provides SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="62e07-150">tooserve ezek céljából, hello tanúsítvány hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="62e07-150">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="62e07-151">hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="62e07-151">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="62e07-152">a kulcscsere, exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="62e07-152">hello certificate must be created for key exchange, exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="62e07-153">hello tanúsítvány tulajdonosának nevét meg kell egyeznie hello használt tartomány tooaccess hello Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="62e07-153">hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="62e07-154">Ez azért szükség tooprovide SSL hello fürt HTTPS felügyeleti végpontok és a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="62e07-154">This is required tooprovide SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="62e07-155">Hello egy hitelesítésszolgáltatótól (CA) származó nem kaphat az SSL-tanúsítvány `.cloudapp.azure.com` tartomány.</span><span class="sxs-lookup"><span data-stu-id="62e07-155">You cannot obtain an SSL certificate from a certificate authority (CA) for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="62e07-156">Szerezzen be egy egyéni tartománynevet a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="62e07-156">Acquire a custom domain name for your cluster.</span></span> <span data-ttu-id="62e07-157">Amikor kér le tanúsítványt a CA hello tanúsítvány tulajdonosának neve meg kell egyeznie hello egyéni tartománynevet használja a fürthöz.</span><span class="sxs-lookup"><span data-stu-id="62e07-157">When you request a certificate from a CA hello certificate's subject name must match hello custom domain name used for your cluster.</span></span>

### <a name="client-authentication-certificates"></a><span data-ttu-id="62e07-158">Ügyfél-hitelesítési tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="62e07-158">Client authentication certificates</span></span>
<span data-ttu-id="62e07-159">További ügyfél-tanúsítványok fürt felügyeleti feladatok rendszergazdák hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="62e07-159">Additional client certificates authenticate administrators for cluster management tasks.</span></span> <span data-ttu-id="62e07-160">A Service Fabric két olyan hozzáférési szintje van: **admin** és **írásvédett felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="62e07-160">Service Fabric has two access levels: **admin** and **read-only user**.</span></span> <span data-ttu-id="62e07-161">Legalább a rendszergazdai hozzáférést a rendszer egy tanúsítványt kell használni.</span><span class="sxs-lookup"><span data-stu-id="62e07-161">At minimum, a single certificate for administrative access should be used.</span></span> <span data-ttu-id="62e07-162">A további felhasználói szintű hozzáférés érdekében egy külön tanúsítványt kell megadni.</span><span class="sxs-lookup"><span data-stu-id="62e07-162">For additional user-level access, a separate certificate must be provided.</span></span> <span data-ttu-id="62e07-163">A hozzáférés szerepkörök további információkért lásd: [szerepköralapú hozzáférés-vezérlés a Service Fabric ügyfelek][service-fabric-cluster-security-roles].</span><span class="sxs-lookup"><span data-stu-id="62e07-163">For more information on access roles, see [role-based access control for Service Fabric clients][service-fabric-cluster-security-roles].</span></span>

<span data-ttu-id="62e07-164">Nem kell a Service Fabric tooupload ügyfél hitelesítési tanúsítványok tooKey tároló toowork.</span><span class="sxs-lookup"><span data-stu-id="62e07-164">You do not need tooupload Client authentication certificates tooKey Vault toowork with Service Fabric.</span></span> <span data-ttu-id="62e07-165">Ezek a tanúsítványok csak a megadott toousers, akik jogosultak a kiszolgálófürt-felügyelet toobe van szükség.</span><span class="sxs-lookup"><span data-stu-id="62e07-165">These certificates only need toobe provided toousers who are authorized for cluster management.</span></span> 

> [!NOTE]
> <span data-ttu-id="62e07-166">Az Azure Active Directory rendszer hello ajánlott módja tooauthenticate ügyfelek fürt felügyeleti műveleteket.</span><span class="sxs-lookup"><span data-stu-id="62e07-166">Azure Active Directory is hello recommended way tooauthenticate clients for cluster management operations.</span></span> <span data-ttu-id="62e07-167">Azure Active Directory toouse, kell [hozzon létre egy fürtöt, Azure Resource Manager használatával][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="62e07-167">toouse Azure Active Directory, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span>
> 
> 

### <a name="application-certificates-optional"></a><span data-ttu-id="62e07-168">Alkalmazás-tanúsítványok (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="62e07-168">Application certificates (optional)</span></span>
<span data-ttu-id="62e07-169">Tetszőleges számú további tanúsítványokat is telepíthető egy fürt biztonsági okokból.</span><span class="sxs-lookup"><span data-stu-id="62e07-169">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="62e07-170">Az fürt létrehozása előtt vegye figyelembe a hello alkalmazás biztonsági igénylő forgatókönyvek egy hello csomópont, például a telepített tanúsítvány toobe:</span><span class="sxs-lookup"><span data-stu-id="62e07-170">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="62e07-171">Az alkalmazás konfigurációs értékeit titkosítását és visszafejtését</span><span class="sxs-lookup"><span data-stu-id="62e07-171">Encryption and decryption of application configuration values</span></span>
* <span data-ttu-id="62e07-172">Replikáció során az adatok csomópontok közötti titkosítása</span><span class="sxs-lookup"><span data-stu-id="62e07-172">Encryption of data across nodes during replication</span></span> 

<span data-ttu-id="62e07-173">Tanúsítványok nem állítható be, amikor hello Azure-portálon keresztül egy fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="62e07-173">Application certificates cannot be configured when creating a cluster through hello Azure portal.</span></span> <span data-ttu-id="62e07-174">fürt telepítéskor tooconfigure tanúsítványok, kell [hozzon létre egy fürtöt, Azure Resource Manager használatával][create-cluster-arm].</span><span class="sxs-lookup"><span data-stu-id="62e07-174">tooconfigure application certificates at cluster setup time, you must [create a cluster using Azure Resource Manager][create-cluster-arm].</span></span> <span data-ttu-id="62e07-175">Alkalmazás tanúsítványok toohello fürt létrehozása után is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="62e07-175">You can also add application certificates toohello cluster after it has been created.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="62e07-176">Az Azure erőforrás-szolgáltató használt tanúsítványok formázás</span><span class="sxs-lookup"><span data-stu-id="62e07-176">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="62e07-177">Titkos kulcs fájlok (.pfx) is felvehető és használható közvetlenül a Key Vault keresztül.</span><span class="sxs-lookup"><span data-stu-id="62e07-177">Private key files (.pfx) can be added and used directly through Key Vault.</span></span> <span data-ttu-id="62e07-178">Azonban hello Azure erőforrás-szolgáltató szükséges kulcsokat toobe tartalmaz, mint a base-64 kódolású karakterlánc és hello titkos kulcs jelszava hello .pfx különleges JSON formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="62e07-178">However, hello Azure resource provider requires keys toobe stored in a special JSON format that includes hello .pfx as a base-64 encoded string and hello private key password.</span></span> <span data-ttu-id="62e07-179">Ezek a követelmények, a kulcsok kell helyezni egy JSON-karakterláncban és majd tárolt tooaccommodate *titkok* a Key Vault.</span><span class="sxs-lookup"><span data-stu-id="62e07-179">tooaccommodate these requirements, keys must be placed in a JSON string and then stored as *secrets* in Key Vault.</span></span>

<span data-ttu-id="62e07-180">Ez a könnyebb feldolgozni toomake, egy PowerShell-modul van [elérhető a Githubon][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="62e07-180">toomake this process easier, a PowerShell module is [available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="62e07-181">Kövesse az alábbi lépéseket toouse hello modul:</span><span class="sxs-lookup"><span data-stu-id="62e07-181">Follow these steps toouse hello module:</span></span>

1. <span data-ttu-id="62e07-182">Töltse le a hello hello tárház teljes tartalmát egy helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="62e07-182">Download hello entire contents of hello repo into a local directory.</span></span> 
2. <span data-ttu-id="62e07-183">A PowerShell-ablakban hello modul importálása:</span><span class="sxs-lookup"><span data-stu-id="62e07-183">Import hello module in your PowerShell window:</span></span>

```powershell
  PS C:\Users\vturecek> Import-Module "C:\users\vturecek\Documents\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"
```

<span data-ttu-id="62e07-184">Hello `Invoke-AddCertToKeyVault` parancsot a PowerShell modul automatikusan a tanúsítvány titkos kulcsa alakítja JSON karakterláncnak és feltölti azt tooKey tárolóban.</span><span class="sxs-lookup"><span data-stu-id="62e07-184">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it tooKey Vault.</span></span> <span data-ttu-id="62e07-185">Ezzel tooadd hello fürt tanúsítvány, és minden további alkalmazás tanúsítványok tooKey tárolóban.</span><span class="sxs-lookup"><span data-stu-id="62e07-185">Use it tooadd hello cluster certificate and any additional application certificates tooKey Vault.</span></span> <span data-ttu-id="62e07-186">Ismételje meg ezt a lépést a fürt kívánt tooinstall külön tanúsítványokra.</span><span class="sxs-lookup"><span data-stu-id="62e07-186">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

```powershell
PS C:\Users\vturecek> Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName mycluster-keyvault -Location "West US" -VaultName myvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup mycluster-keyvault in West US
    WARNING: hello output object type of this cmdlet will be modified in a future release.
    Using existing valut myvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomyvault in vault myvault


Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

<span data-ttu-id="62e07-187">Ezek a hello Key Vault szükséges előfeltételek maradéktalanul a Service Fabric fürt Resource Manager-sablon, amely telepíti a csomópont hitelesítési, felügyeleti végpont biztonsági és hitelesítési és minden további alkalmazásbiztonságot tanúsítványok konfigurálása X.509-tanúsítványokat használó szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="62e07-187">These are all hello Key Vault prerequisites for configuring a Service Fabric cluster Resource Manager template that installs certificates for node authentication, management endpoint security and authentication, and any additional application security features that use X.509 certificates.</span></span> <span data-ttu-id="62e07-188">Ezen a ponton hello telepítése az Azure-ban most már rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="62e07-188">At this point, you should now have hello following setup in Azure:</span></span>

* <span data-ttu-id="62e07-189">Key Vault erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="62e07-189">Key Vault resource group</span></span>
  * <span data-ttu-id="62e07-190">Key Vault</span><span class="sxs-lookup"><span data-stu-id="62e07-190">Key Vault</span></span>
    * <span data-ttu-id="62e07-191">Fürt kiszolgálói hitelesítési tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="62e07-191">Cluster server authentication certificate</span></span>

<span data-ttu-id="62e07-192">< /a "létrehozása, fürt, portal" ></a></span><span class="sxs-lookup"><span data-stu-id="62e07-192"></a "create-cluster-portal" ></a></span></span>

## <a name="create-cluster-in-hello-azure-portal"></a><span data-ttu-id="62e07-193">Hello Azure-portálon a fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="62e07-193">Create cluster in hello Azure portal</span></span>
### <a name="search-for-hello-service-fabric-cluster-resource"></a><span data-ttu-id="62e07-194">Keresse meg a Service Fabric-fürt erőforrás hello</span><span class="sxs-lookup"><span data-stu-id="62e07-194">Search for hello Service Fabric cluster resource</span></span>
![Keresse meg a Service Fabric fürt sablonjának hello Azure-portálon.][SearchforServiceFabricClusterTemplate]

1. <span data-ttu-id="62e07-196">Jelentkezzen be toohello [Azure-portálon][azure-portal].</span><span class="sxs-lookup"><span data-stu-id="62e07-196">Sign in toohello [Azure portal][azure-portal].</span></span>
2. <span data-ttu-id="62e07-197">Kattintson a **új** tooadd egy új erőforrás-sablont.</span><span class="sxs-lookup"><span data-stu-id="62e07-197">Click **New** tooadd a new resource template.</span></span> <span data-ttu-id="62e07-198">Keresse meg a Service Fabric-fürt sablon hello hello **piactér** alatt **mindent**.</span><span class="sxs-lookup"><span data-stu-id="62e07-198">Search for hello Service Fabric Cluster template in hello **Marketplace** under **Everything**.</span></span>
3. <span data-ttu-id="62e07-199">Válassza ki **Service Fabric-fürt** hello listából.</span><span class="sxs-lookup"><span data-stu-id="62e07-199">Select **Service Fabric Cluster** from hello list.</span></span>
4. <span data-ttu-id="62e07-200">Keresse meg a toohello **Service Fabric-fürt** panelen kattintson a **létrehozása**,</span><span class="sxs-lookup"><span data-stu-id="62e07-200">Navigate toohello **Service Fabric Cluster** blade, click **Create**,</span></span>
5. <span data-ttu-id="62e07-201">Hello **létrehozása a Service Fabric-fürt** panelen a következő négy lépést hello rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="62e07-201">hello **Create Service Fabric cluster** blade has hello following four steps.</span></span>

#### <a name="1-basics"></a><span data-ttu-id="62e07-202">1. Alapvető beállítások</span><span class="sxs-lookup"><span data-stu-id="62e07-202">1. Basics</span></span>
![Képernyőfelvétel az új erőforráscsoport létrehozásához.][CreateRG]

<span data-ttu-id="62e07-204">Hello alapvető beállítások panel kell tooprovide hello alapvető adatokat a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="62e07-204">In hello Basics blade you need tooprovide hello basic details for your cluster.</span></span>

1. <span data-ttu-id="62e07-205">Adja meg a fürt hello neve.</span><span class="sxs-lookup"><span data-stu-id="62e07-205">Enter hello name of your cluster.</span></span>
2. <span data-ttu-id="62e07-206">Adjon meg egy **felhasználónév** és **jelszó** a távoli asztal hello virtuális gépek számára.</span><span class="sxs-lookup"><span data-stu-id="62e07-206">Enter a **user name** and **password** for Remote Desktop for hello VMs.</span></span>
3. <span data-ttu-id="62e07-207">Győződjön meg arról, hogy tooselect hello **előfizetés** , amelyet a fürt toobe hatályba léptetni, különösen akkor, ha több előfizetéssel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="62e07-207">Make sure tooselect hello **Subscription** that you want your cluster toobe deployed to, especially if you have multiple subscriptions.</span></span>
4. <span data-ttu-id="62e07-208">Hozzon létre egy **új erőforráscsoport**.</span><span class="sxs-lookup"><span data-stu-id="62e07-208">Create a **new resource group**.</span></span> <span data-ttu-id="62e07-209">Hello neve megegyezik a hello fürtön, mivel segíti a keresés, azokat később, különösen akkor, ha toomake módosítások tooyour telepítési próbált, vagy törölje a fürtöt, ajánlott toogive.</span><span class="sxs-lookup"><span data-stu-id="62e07-209">It is best toogive it hello same name as hello cluster, since it helps in finding them later, especially when you are trying toomake changes tooyour deployment or delete your cluster.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="62e07-210">Bár eldöntheti, hogy egy meglévő erőforráscsoportot toouse, továbbra is egy célszerű toocreate egy új erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="62e07-210">Although you can decide toouse an existing resource group, it is a good practice toocreate a new resource group.</span></span> <span data-ttu-id="62e07-211">Így nem kell könnyen toodelete fürtöket.</span><span class="sxs-lookup"><span data-stu-id="62e07-211">This makes it easy toodelete clusters that you do not need.</span></span>
   > 
   > 
5. <span data-ttu-id="62e07-212">Jelölje be hello **régió** kívánt toocreate hello fürt.</span><span class="sxs-lookup"><span data-stu-id="62e07-212">Select hello **region** in which you want toocreate hello cluster.</span></span> <span data-ttu-id="62e07-213">Ugyanabban a régióban, amely a kulcsot tároló van hello kell használnia.</span><span class="sxs-lookup"><span data-stu-id="62e07-213">You must use hello same region that your Key Vault is in.</span></span>

#### <a name="2-cluster-configuration"></a><span data-ttu-id="62e07-214">2. Fürtkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="62e07-214">2. Cluster configuration</span></span>
![Csomópont-típus létrehozása][CreateNodeType]

<span data-ttu-id="62e07-216">Konfigurálja a fürtcsomópontokat.</span><span class="sxs-lookup"><span data-stu-id="62e07-216">Configure your cluster nodes.</span></span> <span data-ttu-id="62e07-217">Csomóponttípusok hello Virtuálisgép-méretek, a virtuális gépek hello számát és azok tulajdonságait határozza meg.</span><span class="sxs-lookup"><span data-stu-id="62e07-217">Node types define hello VM sizes, hello number of VMs, and their properties.</span></span> <span data-ttu-id="62e07-218">A fürt rendelkezhet egynél több csomóponttípus, de hello elsődleges csomóponttípushoz (hello elsőt hello Portal meghatározó) kell rendelkeznie legalább öt virtuális gépeket, ez ugyanis hello csomóponttípus ahol a Service Fabric rendszerszolgáltatások kerülnek.</span><span class="sxs-lookup"><span data-stu-id="62e07-218">Your cluster can have more than one node type, but hello primary node type (hello first one that you define on hello portal) must have at least five VMs, as this is hello node type where Service Fabric system services are placed.</span></span> <span data-ttu-id="62e07-219">Ne konfiguráljon **elhelyezési tulajdonságok** , mert a rendszer automatikusan hozzáadja egy alapértelmezett elhelyezési "NodeTypeName" tulajdonságát.</span><span class="sxs-lookup"><span data-stu-id="62e07-219">Do not configure **Placement Properties** because a default placement property of "NodeTypeName" is added automatically.</span></span>

> [!NOTE]
> <span data-ttu-id="62e07-220">Egy általános forgatókönyv több csomópont esetében az előtér-szolgáltatás és a háttér-szolgáltatás tartalmazó alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="62e07-220">A common scenario for multiple node types is an application that contains a front-end service and a back-end service.</span></span> <span data-ttu-id="62e07-221">Azt szeretné, hogy tooput hello előtér-szolgáltatást a kisebb rendelkező virtuális gépek (VM-méretek D2 beállításaihoz hasonlóan) portok nyitva toohello Internet, de arra tooput hello háttér-szolgáltatást a nagyobb virtuális gépek (VM-méretek például D4 D6, D15 és így tovább) internetre portot.</span><span class="sxs-lookup"><span data-stu-id="62e07-221">You want tooput hello front-end service on smaller VMs (VM sizes like D2) with ports open toohello Internet, but you want tooput hello back-end service on larger VMs (with VM sizes like D4, D6, D15, and so on) with no Internet-facing ports open.</span></span>
> 
> 

1. <span data-ttu-id="62e07-222">Adjon nevet a csomóponttípus (1 too12 karakter csak betűket és számokat tartalmazó).</span><span class="sxs-lookup"><span data-stu-id="62e07-222">Choose a name for your node type (1 too12 characters containing only letters and numbers).</span></span>
2. <span data-ttu-id="62e07-223">minimális hello **mérete** hello elsődleges csomóponthoz tartozó virtuális gépek típus célja a hello **tartóssági** réteg hello fürt választja.</span><span class="sxs-lookup"><span data-stu-id="62e07-223">hello minimum **size** of VMs for hello primary node type is driven by hello **durability** tier you choose for hello cluster.</span></span> <span data-ttu-id="62e07-224">hello hello tartóssági szint alapértelmezés szerint bronz.</span><span class="sxs-lookup"><span data-stu-id="62e07-224">hello default for hello durability tier is bronze.</span></span> <span data-ttu-id="62e07-225">A durability további információkért lásd: [hogyan toochoose hello Service Fabric fürt megbízhatóság és a tartós][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="62e07-225">For more information on durability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
3. <span data-ttu-id="62e07-226">Hello Virtuálisgép-méretet és tarifacsomagjának kiválasztása.</span><span class="sxs-lookup"><span data-stu-id="62e07-226">Select hello VM size and pricing tier.</span></span> <span data-ttu-id="62e07-227">D sorozatú virtuális gépek SSD meghajtók rendelkeznek, és erősen ajánlott állapotalapú alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="62e07-227">D-series VMs have SSD drives and are highly recommended for stateful applications.</span></span> <span data-ttu-id="62e07-228">Nem használja a virtuális gép SKU részleges maggal rendelkező vagy a rendelkezésre álló szabad kapacitás 7 GB-nál kisebb.</span><span class="sxs-lookup"><span data-stu-id="62e07-228">Do not use any VM SKU that has partial cores or have less than 7 GB of available disk capacity.</span></span> 
4. <span data-ttu-id="62e07-229">minimális hello **szám** hello elsődleges csomóponthoz tartozó virtuális gépek típus célja a hello **megbízhatóság** réteg választja.</span><span class="sxs-lookup"><span data-stu-id="62e07-229">hello minimum **number** of VMs for hello primary node type is driven by hello **reliability** tier you choose.</span></span> <span data-ttu-id="62e07-230">hello hello megbízhatósági szint alapértelmezés szerint ezüst.</span><span class="sxs-lookup"><span data-stu-id="62e07-230">hello default for hello reliability tier is Silver.</span></span> <span data-ttu-id="62e07-231">A megbízhatóság további információkért lásd: [hogyan toochoose hello Service Fabric fürt megbízhatóság és a tartós][service-fabric-cluster-capacity].</span><span class="sxs-lookup"><span data-stu-id="62e07-231">For more information on reliability, see [how toochoose hello Service Fabric cluster reliability and durability][service-fabric-cluster-capacity].</span></span>
5. <span data-ttu-id="62e07-232">Válassza ki a hello hello csomóponttípus virtuális gépeinek számával.</span><span class="sxs-lookup"><span data-stu-id="62e07-232">Choose hello number of VMs for hello node type.</span></span> <span data-ttu-id="62e07-233">Méretezheti felfelé vagy lefelé hello virtuális gépek számát csomóponttípus meg, de hello elsődleges csomóponttípushoz, a minimális hello célja a választott hello megbízhatóság szintje.</span><span class="sxs-lookup"><span data-stu-id="62e07-233">You can scale up or down hello number of VMs in a node type later on, but on hello primary node type, hello minimum is driven by hello reliability level that you have chosen.</span></span> <span data-ttu-id="62e07-234">Egyéb legalább 1 virtuális gép is.</span><span class="sxs-lookup"><span data-stu-id="62e07-234">Other node types can have a minimum of 1 VM.</span></span>
6. <span data-ttu-id="62e07-235">Egyéni végpontokat konfigurálhat.</span><span class="sxs-lookup"><span data-stu-id="62e07-235">Configure custom endpoints.</span></span> <span data-ttu-id="62e07-236">Ez a mező tooenter, amelyet az tooexpose keresztül hello Azure Load Balancer toohello portok vesszővel tagolt listája lehetővé teszi az alkalmazások a nyilvános internethez.</span><span class="sxs-lookup"><span data-stu-id="62e07-236">This field allows you tooenter a comma-separated list of ports that you want tooexpose through hello Azure Load Balancer toohello public Internet for your applications.</span></span> <span data-ttu-id="62e07-237">Például, ha azt tervezi, hogy egy webes alkalmazás tooyour fürt toodeploy, adja meg az "80" Itt tooallow forgalom 80-as porton a fürtbe.</span><span class="sxs-lookup"><span data-stu-id="62e07-237">For example, if you plan toodeploy a web application tooyour cluster, enter "80" here tooallow traffic on port 80 into your cluster.</span></span> <span data-ttu-id="62e07-238">A végpontok további információkért lásd: [alkalmazások folytatott kommunikáció][service-fabric-connect-and-communicate-with-services]</span><span class="sxs-lookup"><span data-stu-id="62e07-238">For more information on endpoints, see [communicating with applications][service-fabric-connect-and-communicate-with-services]</span></span>
7. <span data-ttu-id="62e07-239">Fürt konfigurálása **diagnosztika**.</span><span class="sxs-lookup"><span data-stu-id="62e07-239">Configure cluster **diagnostics**.</span></span> <span data-ttu-id="62e07-240">Alapértelmezés szerint a diagnosztika engedélyezve vannak a a fürt tooassist a problémák elhárításához.</span><span class="sxs-lookup"><span data-stu-id="62e07-240">By default, diagnostics are enabled on your cluster tooassist with troubleshooting issues.</span></span> <span data-ttu-id="62e07-241">Ha azt szeretné, hogy a toodisable diagnosztika módosítása hello **állapot** túl váltása**ki**.</span><span class="sxs-lookup"><span data-stu-id="62e07-241">If you want toodisable diagnostics change hello **Status** toggle too**Off**.</span></span> <span data-ttu-id="62e07-242">Diagnosztika kikapcsolása van **nem** ajánlott.</span><span class="sxs-lookup"><span data-stu-id="62e07-242">Turning off diagnostics is **not** recommended.</span></span>
8. <span data-ttu-id="62e07-243">Válassza ki a háló frissítési mód beállítása a fürt kívánt hello.</span><span class="sxs-lookup"><span data-stu-id="62e07-243">Select hello Fabric upgrade mode you want set your cluster to.</span></span> <span data-ttu-id="62e07-244">Válassza ki **automatikus**, ha szeretné, hogy hello rendszer tooautomatically felvételével kapcsolatban hello elérhető legújabb verzióra, majd próbálja tooupgrade a fürt tooit.</span><span class="sxs-lookup"><span data-stu-id="62e07-244">Select **Automatic**, if you want hello system tooautomatically pick up hello latest available version and try tooupgrade your cluster tooit.</span></span> <span data-ttu-id="62e07-245">Hello mód beállítása túl**manuális**, ha azt szeretné, hogy toochoose valamelyik támogatott verzióra.</span><span class="sxs-lookup"><span data-stu-id="62e07-245">Set hello mode too**Manual**, if you want toochoose a supported version.</span></span>

> [!NOTE]
> <span data-ttu-id="62e07-246">Csak a service Fabric támogatott verzióit futtató fürtök támogatott.</span><span class="sxs-lookup"><span data-stu-id="62e07-246">We support only clusters that are running supported versions of service Fabric.</span></span> <span data-ttu-id="62e07-247">Hello kiválasztásával **manuális** mód, készítésének hello felelősségi tooupgrade meg a fürt tooa támogatott verziója.</span><span class="sxs-lookup"><span data-stu-id="62e07-247">By selecting hello **Manual** mode, you are taking on hello responsibility tooupgrade your cluster tooa supported version.</span></span> <span data-ttu-id="62e07-248">További információ a hello háló frissítési módot: hello [service fabric-fürt-frissítési dokumentum.][service-fabric-cluster-upgrade]</span><span class="sxs-lookup"><span data-stu-id="62e07-248">For more details on hello Fabric upgrade mode see hello [service-fabric-cluster-upgrade document.][service-fabric-cluster-upgrade]</span></span>
> 
> 

#### <a name="3-security"></a><span data-ttu-id="62e07-249">3. Biztonság</span><span class="sxs-lookup"><span data-stu-id="62e07-249">3. Security</span></span>
![Képernyőfelvétel az Azure-portál biztonsági beállításokkal.][SecurityConfigs]

<span data-ttu-id="62e07-251">utolsó lépésként hello tooprovide tanúsítvány információk toosecure hello fürt hello Key Vault és információk a korábban létrehozott tanúsítvány használatával.</span><span class="sxs-lookup"><span data-stu-id="62e07-251">hello final step is tooprovide certificate information toosecure hello cluster using hello Key Vault and certificate information created earlier.</span></span>

* <span data-ttu-id="62e07-252">Hello elsődleges tanúsítvány mezők feltöltése hello töltsenek kapott hello kimenet **fürt tanúsítvány** tooKey tárolót használó hello `Invoke-AddCertToKeyVault` PowerShell-parancsot.</span><span class="sxs-lookup"><span data-stu-id="62e07-252">Populate hello primary certificate fields with hello output obtained from uploading hello **cluster certificate** tooKey Vault using hello `Invoke-AddCertToKeyVault` PowerShell command.</span></span>

```powershell
Name  : CertificateThumbprint
Value : <value>

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault

Name  : CertificateURL
Value : https://myvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47
```

* <span data-ttu-id="62e07-253">Ellenőrizze a hello **speciális beállítások konfigurálása** ügyféltanúsítványainak tooenter mezőben **rendszergazdai ügyfél** és **írásvédett ügyfél**.</span><span class="sxs-lookup"><span data-stu-id="62e07-253">Check hello **Configure advanced settings** box tooenter client certificates for **admin client** and **read-only client**.</span></span> <span data-ttu-id="62e07-254">Adja meg ezeket a mezőket az admin ügyféltanúsítvány ujjlenyomata hello és az írásvédett felhasználó ügyféltanúsítvány ujjlenyomata hello egységeit.</span><span class="sxs-lookup"><span data-stu-id="62e07-254">In these fields, enter hello thumbprint of your admin client certificate and hello thumbprint of your read-only user client certificate, if applicable.</span></span> <span data-ttu-id="62e07-255">Amikor a rendszergazdák próbálnak tooconnect toohello fürt, kapnak, az itt megadott hozzáférés csak akkor, ha rendelkeznek a megfelelő hello ujjlenyomat értékek ujjlenyomattal rendelkező tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="62e07-255">When administrators attempt tooconnect toohello cluster, they are granted access only if they have a certificate with a thumbprint that matches hello thumbprint values entered here.</span></span>  

#### <a name="4-summary"></a><span data-ttu-id="62e07-256">4. Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="62e07-256">4. Summary</span></span>
![<span data-ttu-id="62e07-257">Képernyőfelvétel a hello start Bizottsága megjelenítése "A Service Fabric-fürt."</span><span class="sxs-lookup"><span data-stu-id="62e07-257">Screen shot of hello start board displaying "Deploying Service Fabric Cluster."</span></span> ][Notifications]

<span data-ttu-id="62e07-258">toocomplete hello fürt létrehozása, kattintson a **összegzés** toosee hello konfigurációkat megadott, vagy töltse le hello toodeploy a fürt, amely használt Azure Resource Manager sablon.</span><span class="sxs-lookup"><span data-stu-id="62e07-258">toocomplete hello cluster creation, click **Summary** toosee hello configurations that you have provided, or download hello Azure Resource Manager template that that used toodeploy your cluster.</span></span> <span data-ttu-id="62e07-259">Hello kötelező beállítások megadása után hello **OK** gomb akkor válik zöld és hello Fürtlétrehozási folyamat indításához kattintson az.</span><span class="sxs-lookup"><span data-stu-id="62e07-259">After you have provided hello mandatory settings, hello **OK** button becomes green and you can start hello cluster creation process by clicking it.</span></span>

<span data-ttu-id="62e07-260">Hello létrehozásának folyamata hello értesítések tekintheti meg.</span><span class="sxs-lookup"><span data-stu-id="62e07-260">You can see hello creation progress in hello notifications.</span></span> <span data-ttu-id="62e07-261">(Kattintson a hello "harang" ikonra hello állapotsorban hello jobb felső sarkában a képernyőn közelében.) Kattintott **PIN-kód tooStartboard** hello fürt létrehozásakor látni fogja **Fabric-fürt üzembe helyezése** toohello rögzítve **Start** tábla.</span><span class="sxs-lookup"><span data-stu-id="62e07-261">(Click hello "Bell" icon near hello status bar at hello upper right of your screen.) If you clicked **Pin tooStartboard** while creating hello cluster, you will see **Deploying Service Fabric Cluster** pinned toohello **Start** board.</span></span>

### <a name="view-your-cluster-status"></a><span data-ttu-id="62e07-262">A fürt állapotának megtekintése</span><span class="sxs-lookup"><span data-stu-id="62e07-262">View your cluster status</span></span>
![Képernyőfelvétel a hello irányítópulton fürt adatait.][ClusterDashboard]

<span data-ttu-id="62e07-264">A fürt létrehozása után vizsgálhatja meg a fürt hello portálon:</span><span class="sxs-lookup"><span data-stu-id="62e07-264">Once your cluster is created, you can inspect your cluster in hello portal:</span></span>

1. <span data-ttu-id="62e07-265">Nyissa meg túl**Tallózás** kattintson **Service Fabric-fürtök**.</span><span class="sxs-lookup"><span data-stu-id="62e07-265">Go too**Browse** and click **Service Fabric Clusters**.</span></span>
2. <span data-ttu-id="62e07-266">Keresse meg a fürthöz, és kattintson rá.</span><span class="sxs-lookup"><span data-stu-id="62e07-266">Locate your cluster and click it.</span></span>
3. <span data-ttu-id="62e07-267">Most már megtekintheti a fürt hello irányítópulton, beleértve a nyilvános végpontot hello fürt és a hivatkozás tooService Fabric Explorer hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="62e07-267">You can now see hello details of your cluster in hello dashboard, including hello cluster's public endpoint and a link tooService Fabric Explorer.</span></span>

<span data-ttu-id="62e07-268">Hello **csomópont Monitor** szakasz hello fürt irányítópult panelen, amelyek a megfelelő és nem kifogástalan állapotú virtuális gépek hello számát jelzi.</span><span class="sxs-lookup"><span data-stu-id="62e07-268">hello **Node Monitor** section on hello cluster's dashboard blade indicates hello number of VMs that are healthy and not healthy.</span></span> <span data-ttu-id="62e07-269">Hello fürt állapotát, további információkat is talál [Service Fabric rendszerállapot-modell bemutatása][service-fabric-health-introduction].</span><span class="sxs-lookup"><span data-stu-id="62e07-269">You can find more details about hello cluster's health at [Service Fabric health model introduction][service-fabric-health-introduction].</span></span>

> [!NOTE]
> <span data-ttu-id="62e07-270">Service Fabric-fürtök bizonyos számú csomópontok toobe mindig toomaintain rendelkezésre állási be kell, és állapot - hivatkozott tooas "kvórum fenntartása" megőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="62e07-270">Service Fabric clusters require a certain number of nodes toobe up always toomaintain availability and preserve state - referred tooas "maintaining quorum".</span></span> <span data-ttu-id="62e07-271">Therfore, nincs általában le hello fürt összes gépeket biztonságos tooshut kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot][service-fabric-reliable-services-backup-restore].</span><span class="sxs-lookup"><span data-stu-id="62e07-271">Therfore, it is typically not safe tooshut down all machines in hello cluster unless you have first performed a [full backup of your state][service-fabric-reliable-services-backup-restore].</span></span>
> 
> 

## <a name="remote-connect-tooa-virtual-machine-scale-set-instance-or-a-cluster-node"></a><span data-ttu-id="62e07-272">Távoli kapcsolódás tooa virtuálisgép-méretezési csoport példányt vagy egy fürt csomópontja</span><span class="sxs-lookup"><span data-stu-id="62e07-272">Remote connect tooa Virtual Machine Scale Set instance or a cluster node</span></span>
<span data-ttu-id="62e07-273">A NodeType tulajdonságok értéke hello mindegyikének ad meg a fürt eredmény egy virtuálisgép-méretezési csoportban lévő első beállításról.</span><span class="sxs-lookup"><span data-stu-id="62e07-273">Each of hello NodeTypes you specify in your cluster results in a Virtual Machine Scale Set getting set-up.</span></span> <span data-ttu-id="62e07-274">Lásd: [távoli csatlakozás tooa virtuálisgép-méretezési csoport példány] [ remote-connect-to-a-vm-scale-set] részleteiről.</span><span class="sxs-lookup"><span data-stu-id="62e07-274">See [Remote connect tooa Virtual Machine Scale Set instance][remote-connect-to-a-vm-scale-set] for details.</span></span>

## <a name="next-steps"></a><span data-ttu-id="62e07-275">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="62e07-275">Next steps</span></span>
<span data-ttu-id="62e07-276">Ezen a ponton hogy a biztonságos fürt felügyeleti hitelesítési tanúsítványokat használnak.</span><span class="sxs-lookup"><span data-stu-id="62e07-276">At this point, you have a secure cluster using certificates for management authentication.</span></span> <span data-ttu-id="62e07-277">Ezt követően [csatlakoztassa tooyour fürtöt](service-fabric-connect-to-secure-cluster.md) és megtudhatja, hogyan túl[alkalmazás titkos kulcsok kezelése](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="62e07-277">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>  <span data-ttu-id="62e07-278">Emellett megismerése [Service Fabric támogatási lehetőségek](service-fabric-support.md).</span><span class="sxs-lookup"><span data-stu-id="62e07-278">Also, learn about [Service Fabric support options](service-fabric-support.md).</span></span>

<!-- Links -->
[azure-powershell]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[azure-portal]: https://portal.azure.com/
[key-vault-get-started]: ../key-vault/key-vault-get-started.md
[create-cluster-arm]: service-fabric-cluster-creation-via-arm.md
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[service-fabric-cluster-security-roles]: service-fabric-cluster-security-roles.md
[service-fabric-cluster-capacity]: service-fabric-cluster-capacity.md
[service-fabric-connect-and-communicate-with-services]: service-fabric-connect-and-communicate-with-services.md
[service-fabric-health-introduction]: service-fabric-health-introduction.md
[service-fabric-reliable-services-backup-restore]: service-fabric-reliable-services-backup-restore.md
[remote-connect-to-a-vm-scale-set]: service-fabric-cluster-nodetypes.md#remote-connect-to-a-vm-scale-set-instance-or-a-cluster-node
[service-fabric-cluster-upgrade]: service-fabric-cluster-upgrade.md

<!--Image references-->
[SearchforServiceFabricClusterTemplate]: ./media/service-fabric-cluster-creation-via-portal/SearchforServiceFabricClusterTemplate.png
[CreateRG]: ./media/service-fabric-cluster-creation-via-portal/CreateRG.png
[CreateNodeType]: ./media/service-fabric-cluster-creation-via-portal/NodeType.png
[SecurityConfigs]: ./media/service-fabric-cluster-creation-via-portal/SecurityConfigs.png
[Notifications]: ./media/service-fabric-cluster-creation-via-portal/notifications.png
[ClusterDashboard]: ./media/service-fabric-cluster-creation-via-portal/ClusterDashboard.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
