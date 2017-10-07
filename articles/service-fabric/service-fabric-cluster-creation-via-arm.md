---
title: "egy Azure Service Fabric aaaCreate fürt olyan sablon alapján |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooset másolatot egy biztonságos Service Fabric fürt az Azure-ban Azure Resource Manager, az Azure Key Vault és az Azure Active Directory (Azure AD) segítségével az ügyfél-hitelesítéshez."
services: service-fabric
documentationcenter: .net
author: chackdan
manager: timlt
editor: chackdan
ms.assetid: 15d0ab67-fc66-4108-8038-3584eeebabaa
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2017
ms.author: chackdan
ms.openlocfilehash: a4563c58a68127720a8290c3be0df9d026833eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-service-fabric-cluster-by-using-azure-resource-manager"></a><span data-ttu-id="544e7-103">A Service Fabric-fürt létrehozása az Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="544e7-103">Create a Service Fabric cluster by using Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="544e7-104">Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="544e7-104">Azure Resource Manager</span></span>](service-fabric-cluster-creation-via-arm.md)
> * [<span data-ttu-id="544e7-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="544e7-105">Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
>
>

<span data-ttu-id="544e7-106">Ez az útmutató lépésről lépésre bemutatja, hogyan állítja be a biztonságos Azure Service Fabric-fürt az Azure-ban Azure Resource Manager használatával.</span><span class="sxs-lookup"><span data-stu-id="544e7-106">This step-by-step guide walks you through setting up a secure Azure Service Fabric cluster in Azure by using Azure Resource Manager.</span></span> <span data-ttu-id="544e7-107">A Microsoft tudomásul hello cikkben hosszú.</span><span class="sxs-lookup"><span data-stu-id="544e7-107">We acknowledge that hello article is long.</span></span> <span data-ttu-id="544e7-108">Mindazonáltal kivéve, ha már alaposan tisztában hello tartalommal, kell meg arról, hogy toofollow minden lépés gondosan.</span><span class="sxs-lookup"><span data-stu-id="544e7-108">Nevertheless, unless you are already thoroughly familiar with hello content, be sure toofollow each step carefully.</span></span>

<span data-ttu-id="544e7-109">hello útmutató hello az alábbi eljárásokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="544e7-109">hello guide covers hello following procedures:</span></span>

* <span data-ttu-id="544e7-110">Az az Azure key vault tooupload tanúsítványok fürt- és a biztonság beállítása</span><span class="sxs-lookup"><span data-stu-id="544e7-110">Setting up an Azure key vault tooupload certificates for cluster and application security</span></span>
* <span data-ttu-id="544e7-111">Védett fürt létrehozása az Azure-ban Azure Resource Manager használatával</span><span class="sxs-lookup"><span data-stu-id="544e7-111">Creating a secured cluster in Azure by using Azure Resource Manager</span></span>
* <span data-ttu-id="544e7-112">Felhasználók hitelesítése az Azure Active Directory (Azure AD) segítségével a kiszolgálófürt-felügyelet</span><span class="sxs-lookup"><span data-stu-id="544e7-112">Authenticating users by using Azure Active Directory (Azure AD) for cluster management</span></span>

<span data-ttu-id="544e7-113">A biztonságos fürt olyan fürt, amely megakadályozza a jogosulatlan hozzáférés toomanagement műveletek.</span><span class="sxs-lookup"><span data-stu-id="544e7-113">A secure cluster is a cluster that prevents unauthorized access toomanagement operations.</span></span> <span data-ttu-id="544e7-114">Ez magában foglalja az üzembe helyezés, a frissítés és törlése alkalmazások, szolgáltatások és hello adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="544e7-114">This includes deploying, upgrading, and deleting applications, services, and hello data they contain.</span></span> <span data-ttu-id="544e7-115">Egy nem biztonságos fürt olyan fürt, hogy bárki tooat bármikor csatlakozhat, és felügyeleti műveletek.</span><span class="sxs-lookup"><span data-stu-id="544e7-115">An unsecure cluster is a cluster that anyone can connect tooat any time and perform management operations.</span></span> <span data-ttu-id="544e7-116">Bár lehetséges toocreate egy nem biztonságos fürtre, határozottan ajánlott, hogy hozzon létre egy biztonságos fürt hello kezdettől.</span><span class="sxs-lookup"><span data-stu-id="544e7-116">Although it is possible toocreate an unsecure cluster, we highly recommend that you create a secure cluster from hello outset.</span></span> <span data-ttu-id="544e7-117">Egy nem biztonságos fürt később nem védhető, mert létre kell hozni egy új fürtöt.</span><span class="sxs-lookup"><span data-stu-id="544e7-117">Because an unsecure cluster cannot be secured later, a new cluster must be created.</span></span>

<span data-ttu-id="544e7-118">hello fogalma létrehozása biztonságos fürtök van hello azonos, Linux vagy a Windows-fürtök legyenek.</span><span class="sxs-lookup"><span data-stu-id="544e7-118">hello concept of creating secure clusters is hello same, whether they are Linux or Windows clusters.</span></span> <span data-ttu-id="544e7-119">További információk és segítő parancsfájlok biztonságos Linux-fürtök létrehozásához, lásd: [biztonságos fürtök Linux rendszeren létrehozására](#secure-linux-clusters).</span><span class="sxs-lookup"><span data-stu-id="544e7-119">For more information and helper scripts for creating secure Linux clusters, see [Creating secure clusters on Linux](#secure-linux-clusters).</span></span>

## <a name="sign-in-tooyour-azure-account"></a><span data-ttu-id="544e7-120">Jelentkezzen be tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="544e7-120">Sign in tooyour Azure account</span></span>
<span data-ttu-id="544e7-121">Ez az útmutató használ [Azure PowerShell][azure-powershell].</span><span class="sxs-lookup"><span data-stu-id="544e7-121">This guide uses [Azure PowerShell][azure-powershell].</span></span> <span data-ttu-id="544e7-122">Amikor egy új PowerShell-munkamenetet indít el, jelentkezzen be Azure-fiók tooyour, és jelölje ki az előfizetését, Azure parancsok végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="544e7-122">When you start a new PowerShell session, sign in tooyour Azure account and select your subscription before you execute Azure commands.</span></span>

<span data-ttu-id="544e7-123">Bejelentkezés tooyour Azure-fiók:</span><span class="sxs-lookup"><span data-stu-id="544e7-123">Sign in tooyour Azure account:</span></span>

```powershell
Login-AzureRmAccount
```

<span data-ttu-id="544e7-124">Jelölje ki az előfizetését:</span><span class="sxs-lookup"><span data-stu-id="544e7-124">Select your subscription:</span></span>

```powershell
Get-AzureRmSubscription
Set-AzureRmContext -SubscriptionId <guid>
```

## <a name="set-up-a-key-vault"></a><span data-ttu-id="544e7-125">Kulcstároló beállítása</span><span class="sxs-lookup"><span data-stu-id="544e7-125">Set up a key vault</span></span>
<span data-ttu-id="544e7-126">Ez a szakasz ismerteti, és a Service Fabric-alkalmazások számára az Azure Service Fabric-fürt kulcstároló létrehozása.</span><span class="sxs-lookup"><span data-stu-id="544e7-126">This section discusses creating a key vault for a Service Fabric cluster in Azure and for Service Fabric applications.</span></span> <span data-ttu-id="544e7-127">A teljes körű útmutatót tooAzure Key Vault meg toohello [Key Vault – első lépések útmutató][key-vault-get-started].</span><span class="sxs-lookup"><span data-stu-id="544e7-127">For a complete guide tooAzure Key Vault, refer toohello [Key Vault getting started guide][key-vault-get-started].</span></span>

<span data-ttu-id="544e7-128">A Service Fabric X.509 tanúsítvány toosecure egy fürt használja, és adja meg az alkalmazás biztonsági funkciók.</span><span class="sxs-lookup"><span data-stu-id="544e7-128">Service Fabric uses X.509 certificates toosecure a cluster and provide application security features.</span></span> <span data-ttu-id="544e7-129">A Service Fabric-fürtök az Azure Key Vault toomanage tanúsítványokat használ.</span><span class="sxs-lookup"><span data-stu-id="544e7-129">You use Key Vault toomanage certificates for Service Fabric clusters in Azure.</span></span> <span data-ttu-id="544e7-130">A fürt telepítésekor az Azure-ban, hello Azure erőforrás-szolgáltató, amely felelős az Service Fabric-fürtök létrehozására kéri le a tanúsítványokat a Key Vault, és telepíti őket hello fürt virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="544e7-130">When a cluster is deployed in Azure, hello Azure resource provider that's responsible for creating Service Fabric clusters pulls certificates from Key Vault and installs them on hello cluster VMs.</span></span>

<span data-ttu-id="544e7-131">hello alábbi ábrán látható, az Azure Key Vault, a Service Fabric-fürt és a hello Azure erőforrás-szolgáltató, amikor létrehozza a fürtöt a key vaultban tárolt tanúsítványokat használó hello kapcsolatát:</span><span class="sxs-lookup"><span data-stu-id="544e7-131">hello following diagram illustrates hello relationship between Azure Key Vault, a Service Fabric cluster, and hello Azure resource provider that uses certificates stored in a key vault when it creates a cluster:</span></span>

![Tanúsítvány telepítése ábrája][cluster-security-cert-installation]

### <a name="create-a-resource-group"></a><span data-ttu-id="544e7-133">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="544e7-133">Create a resource group</span></span>
<span data-ttu-id="544e7-134">hello első lépés egy erőforráscsoportot kimondottan a key vaultban toocreate.</span><span class="sxs-lookup"><span data-stu-id="544e7-134">hello first step is toocreate a resource group specifically for your key vault.</span></span> <span data-ttu-id="544e7-135">Azt javasoljuk, hogy hello kulcstároló kerüljenek-e a saját erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="544e7-135">We recommend that you put hello key vault into its own resource group.</span></span> <span data-ttu-id="544e7-136">Ez a művelet lehetővé teszi hello számítási és tárolási erőforrás csoportokhoz, beleértve a Service Fabric-fürt, a kulcsok és titkos kulcsok elvesztése tartalmazó erőforráscsoport hello eltávolítását.</span><span class="sxs-lookup"><span data-stu-id="544e7-136">This action lets you remove hello compute and storage resource groups, including hello resource group that contains your Service Fabric cluster, without losing your keys and secrets.</span></span> <span data-ttu-id="544e7-137">a kulcstartót tartalmazó erőforráscsoportot hello _hello kell ugyanabban a régióban_ által használt hello fürtként.</span><span class="sxs-lookup"><span data-stu-id="544e7-137">hello resource group that contains your key vault _must be in hello same region_ as hello cluster that is using it.</span></span>

<span data-ttu-id="544e7-138">Ha több régióba toodeploy fürtök, javasoljuk, hogy hello erőforráscsoport és oly módon, amely azt jelzi, mely régióhoz tartozik, hello kulcstároló neve.</span><span class="sxs-lookup"><span data-stu-id="544e7-138">If you plan toodeploy clusters in multiple regions, we suggest that you name hello resource group and hello key vault in a way that indicates which region it belongs to.</span></span>  

```powershell

    New-AzureRmResourceGroup -Name westus-mykeyvault -Location 'West US'
```
<span data-ttu-id="544e7-139">hello kimeneti kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="544e7-139">hello output should look like this:</span></span>

```powershell

    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.

    ResourceGroupName : westus-mykeyvault
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
    ResourceId        : /subscriptions/<guid>/resourceGroups/westus-mykeyvault

```
<a id="new-key-vault"></a>

### <a name="create-a-key-vault-in-hello-new-resource-group"></a><span data-ttu-id="544e7-140">Hozzon létre egy kulcstartót hello új erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="544e7-140">Create a key vault in hello new resource group</span></span>
<span data-ttu-id="544e7-141">hello kulcstároló _engedélyezni kell a központi telepítési_ tooallow hello számítási erőforrás szolgáltató tooget tanúsítványokat, és telepíti azt a virtuálisgép-példányok:</span><span class="sxs-lookup"><span data-stu-id="544e7-141">hello key vault _must be enabled for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on virtual machine instances:</span></span>

```powershell

    New-AzureRmKeyVault -VaultName 'mywestusvault' -ResourceGroupName 'westus-mykeyvault' -Location 'West US' -EnabledForDeployment

```

<span data-ttu-id="544e7-142">hello kimeneti kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="544e7-142">hello output should look like this:</span></span>

```powershell

    Vault Name                       : mywestusvault
    Resource Group Name              : westus-mykeyvault
    Location                         : West US
    Resource ID                      : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault
    Vault URI                        : https://mywestusvault.vault.azure.net
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
<a id="existing-key-vault"></a>

## <a name="use-an-existing-key-vault"></a><span data-ttu-id="544e7-143">Használjon egy meglévő kulcstároló</span><span class="sxs-lookup"><span data-stu-id="544e7-143">Use an existing key vault</span></span>

<span data-ttu-id="544e7-144">egy meglévő kulcstároló toouse meg _engedélyezni kell a központi telepítési_ tooallow hello számítási erőforrás szolgáltató tooget tanúsítványokat, és telepítse a fürtcsomópontokon:</span><span class="sxs-lookup"><span data-stu-id="544e7-144">toouse an existing key vault, you _must enable it for deployment_ tooallow hello compute resource provider tooget certificates from it and install it on cluster nodes:</span></span>

```powershell

Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -EnabledForDeployment

```

<a id="add-certificate-to-key-vault"></a>

## <a name="add-certificates-tooyour-key-vault"></a><span data-ttu-id="544e7-145">Tanúsítványok tooyour kulcstároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="544e7-145">Add certificates tooyour key vault</span></span>

<span data-ttu-id="544e7-146">A rendszer tanúsítványokat használ a Service Fabric tooprovide hitelesítési és titkosítási toosecure különböző szempontjairól a fürt és az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="544e7-146">Certificates are used in Service Fabric tooprovide authentication and encryption toosecure various aspects of a cluster and its applications.</span></span> <span data-ttu-id="544e7-147">További információ a tanúsítványok használatának módját a Service Fabric: [Service Fabric-fürt biztonsági forgatókönyvek][service-fabric-cluster-security].</span><span class="sxs-lookup"><span data-stu-id="544e7-147">For more information on how certificates are used in Service Fabric, see [Service Fabric cluster security scenarios][service-fabric-cluster-security].</span></span>

### <a name="cluster-and-server-certificate-required"></a><span data-ttu-id="544e7-148">Fürt és a kiszolgálói tanúsítványt (kötelező)</span><span class="sxs-lookup"><span data-stu-id="544e7-148">Cluster and server certificate (required)</span></span>
<span data-ttu-id="544e7-149">Ez a tanúsítvány szükséges toosecure fürt, és megakadályozza a jogosulatlan hozzáférés tooit.</span><span class="sxs-lookup"><span data-stu-id="544e7-149">This certificate is required toosecure a cluster and prevent unauthorized access tooit.</span></span> <span data-ttu-id="544e7-150">Fürt biztonsági két módon tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="544e7-150">It provides cluster security in two ways:</span></span>

* <span data-ttu-id="544e7-151">Fürt hitelesítési: hitelesíti a fürt összevonási csomópontok kommunikációt.</span><span class="sxs-lookup"><span data-stu-id="544e7-151">Cluster authentication: Authenticates node-to-node communication for cluster federation.</span></span> <span data-ttu-id="544e7-152">Csak olyan csomópontot, amely bizonyítja, ezzel a tanúsítvánnyal az identitásukat hello fürt csatlakozhat.</span><span class="sxs-lookup"><span data-stu-id="544e7-152">Only nodes that can prove their identity with this certificate can join hello cluster.</span></span>
* <span data-ttu-id="544e7-153">Kiszolgálóhitelesítés: hitelesíti a hello fürt felügyeleti végpontok tooa felügyeleti ügyfél, így hello felügyeleti ügyfél tudja azt toohello valós fürt van van szó.</span><span class="sxs-lookup"><span data-stu-id="544e7-153">Server authentication: Authenticates hello cluster management endpoints tooa management client, so that hello management client knows it is talking toohello real cluster.</span></span> <span data-ttu-id="544e7-154">Ez a tanúsítvány is biztosít az SSL hello HTTPS felügyeleti API és a Service Fabric Explorer HTTPS-KAPCSOLATON keresztül.</span><span class="sxs-lookup"><span data-stu-id="544e7-154">This certificate also provides an SSL for hello HTTPS management API and for Service Fabric Explorer over HTTPS.</span></span>

<span data-ttu-id="544e7-155">tooserve ezek céljából, hello tanúsítvány hello követelményeknek kell megfelelniük:</span><span class="sxs-lookup"><span data-stu-id="544e7-155">tooserve these purposes, hello certificate must meet hello following requirements:</span></span>

* <span data-ttu-id="544e7-156">hello tanúsítványnak tartalmaznia kell egy titkos kulccsal.</span><span class="sxs-lookup"><span data-stu-id="544e7-156">hello certificate must contain a private key.</span></span>
* <span data-ttu-id="544e7-157">a kulcscseréhez használt, amely exportálható tooa személyes információcsere (.pfx) fájlt hello tanúsítványt kell létrehozni.</span><span class="sxs-lookup"><span data-stu-id="544e7-157">hello certificate must be created for key exchange, which is exportable tooa Personal Information Exchange (.pfx) file.</span></span>
* <span data-ttu-id="544e7-158">hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata.</span><span class="sxs-lookup"><span data-stu-id="544e7-158">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="544e7-159">A megfelelő szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="544e7-159">This matching is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="544e7-160">Hello egy hitelesítésszolgáltatótól (CA) származó nem kaphat az SSL-tanúsítvány. cloudapp.azure.com tartomány.</span><span class="sxs-lookup"><span data-stu-id="544e7-160">You cannot obtain an SSL certificate from a certificate authority (CA) for hello .cloudapp.azure.com domain.</span></span> <span data-ttu-id="544e7-161">Be kell szereznie egy egyéni tartománynevet a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="544e7-161">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="544e7-162">A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="544e7-162">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

### <a name="application-certificates-optional"></a><span data-ttu-id="544e7-163">Alkalmazás-tanúsítványok (nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="544e7-163">Application certificates (optional)</span></span>
<span data-ttu-id="544e7-164">Tetszőleges számú további tanúsítványokat is telepíthető egy fürt biztonsági okokból.</span><span class="sxs-lookup"><span data-stu-id="544e7-164">Any number of additional certificates can be installed on a cluster for application security purposes.</span></span> <span data-ttu-id="544e7-165">Az fürt létrehozása előtt vegye figyelembe a hello alkalmazás biztonsági igénylő forgatókönyvek egy hello csomópont, például a telepített tanúsítvány toobe:</span><span class="sxs-lookup"><span data-stu-id="544e7-165">Before creating your cluster, consider hello application security scenarios that require a certificate toobe installed on hello nodes, such as:</span></span>

* <span data-ttu-id="544e7-166">Titkosítás és visszafejtés az alkalmazáskonfigurációs értékeket.</span><span class="sxs-lookup"><span data-stu-id="544e7-166">Encryption and decryption of application configuration values.</span></span>
* <span data-ttu-id="544e7-167">Replikáció során az adatok csomópontok közötti titkosítása.</span><span class="sxs-lookup"><span data-stu-id="544e7-167">Encryption of data across nodes during replication.</span></span>

### <a name="formatting-certificates-for-azure-resource-provider-use"></a><span data-ttu-id="544e7-168">Az Azure erőforrás-szolgáltató használt tanúsítványok formázás</span><span class="sxs-lookup"><span data-stu-id="544e7-168">Formatting certificates for Azure resource provider use</span></span>
<span data-ttu-id="544e7-169">Adja hozzá, és a titkos kulcs fájlok használata (.pfx) közvetlenül a kulcstartót keresztül.</span><span class="sxs-lookup"><span data-stu-id="544e7-169">You can add and use private key files (.pfx) directly through your key vault.</span></span> <span data-ttu-id="544e7-170">Azonban hello Azure számítási erőforrás-szolgáltató szükséges kulcsokat toobe különleges JavaScript Object Notation (JSON) formátumban tárolja.</span><span class="sxs-lookup"><span data-stu-id="544e7-170">However, hello Azure compute resource provider requires keys toobe stored in a special JavaScript Object Notation (JSON) format.</span></span> <span data-ttu-id="544e7-171">hello formátum base 64 kódolású karakterlánc és titkos kulcs jelszava hello hello .pfx fájl tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="544e7-171">hello format includes hello .pfx file as a base 64-encoded string and hello private key password.</span></span> <span data-ttu-id="544e7-172">tooaccommodate ezeknek a követelményeknek, hello kulcsok kell helyezni egy JSON-karakterláncban és majd tárolt "titkos kulcsainak" hello kulcsot tároló.</span><span class="sxs-lookup"><span data-stu-id="544e7-172">tooaccommodate these requirements, hello keys must be placed in a JSON string and then stored as "secrets" in hello key vault.</span></span>

<span data-ttu-id="544e7-173">a könnyebb, feldolgozni toomake egy [PowerShell modul az elérhető a Githubon][service-fabric-rp-helpers].</span><span class="sxs-lookup"><span data-stu-id="544e7-173">toomake this process easier, a [PowerShell module is available on GitHub][service-fabric-rp-helpers].</span></span> <span data-ttu-id="544e7-174">toouse hello modul, a következő hello:</span><span class="sxs-lookup"><span data-stu-id="544e7-174">toouse hello module, do hello following:</span></span>

1. <span data-ttu-id="544e7-175">Töltse le a hello hello tárház teljes tartalmát egy helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="544e7-175">Download hello entire contents of hello repo into a local directory.</span></span>
2. <span data-ttu-id="544e7-176">Nyissa meg toohello helyi könyvtárba.</span><span class="sxs-lookup"><span data-stu-id="544e7-176">Go toohello local directory.</span></span>
2. <span data-ttu-id="544e7-177">A PowerShell-ablakban hello ServiceFabricRPHelpers modul importálása:</span><span class="sxs-lookup"><span data-stu-id="544e7-177">Import hello ServiceFabricRPHelpers module in your PowerShell window:</span></span>

```powershell

 Import-Module "C:\..\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

```

<span data-ttu-id="544e7-178">Hello `Invoke-AddCertToKeyVault` parancsot a PowerShell modul automatikusan a tanúsítvány titkos kulcsa alakítja JSON karakterláncnak és feltölti azt toohello kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="544e7-178">hello `Invoke-AddCertToKeyVault` command in this PowerShell module automatically formats a certificate private key into a JSON string and uploads it toohello key vault.</span></span> <span data-ttu-id="544e7-179">Hello parancs tooadd hello fürt tanúsítványt, és minden további alkalmazás tanúsítványok toohello kulcstároló használnia.</span><span class="sxs-lookup"><span data-stu-id="544e7-179">Use hello command tooadd hello cluster certificate and any additional application certificates toohello key vault.</span></span> <span data-ttu-id="544e7-180">Ismételje meg ezt a lépést a fürt kívánt tooinstall külön tanúsítványokra.</span><span class="sxs-lookup"><span data-stu-id="544e7-180">Repeat this step for any additional certificates you want tooinstall in your cluster.</span></span>

#### <a name="uploading-an-existing-certificate"></a><span data-ttu-id="544e7-181">Létező tanúsítvány feltöltése</span><span class="sxs-lookup"><span data-stu-id="544e7-181">Uploading an existing certificate</span></span>

```powershell

 Invoke-AddCertToKeyVault -SubscriptionId <guid> -ResourceGroupName westus-mykeyvault -Location "West US" -VaultName mywestusvault -CertificateName mycert -Password "<password>" -UseExistingCertificate -ExistingPfxFilePath "C:\path\to\mycertkey.pfx"

```

<span data-ttu-id="544e7-182">Ha a hiba, például az egyik látható itt, hello Ez általában azt jelenti, hogy rendelkezik-e URL-cím erőforrás-ütközés.</span><span class="sxs-lookup"><span data-stu-id="544e7-182">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="544e7-183">tooresolve hello ütközik, a módosítás hello kulcstároló neve.</span><span class="sxs-lookup"><span data-stu-id="544e7-183">tooresolve hello conflict, change hello key vault name.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="544e7-184">Miután hello ütközés feloldja, hello kimeneti kell néznek ki:</span><span class="sxs-lookup"><span data-stu-id="544e7-184">After hello conflict is resolved, hello output should look like this:</span></span>

```

    Switching context tooSubscriptionId <guid>
    Ensuring ResourceGroup westus-mykeyvault in West US
    WARNING: hello output object type of this cmdlet is going toobe modified in a future release.
    Using existing value mywestusvault in West US
    Reading pfx file from C:\path\to\key.pfx
    Writing secret toomywestusvault in vault mywestusvault


Name  : CertificateThumbprint
Value : E21DBC64B183B5BF355C34C46E03409FEEAEF58D

Name  : SourceVault
Value : /subscriptions/<guid>/resourceGroups/westus-mykeyvault/providers/Microsoft.KeyVault/vaults/mywestusvault

Name  : CertificateURL
Value : https://mywestusvault.vault.azure.net:443/secrets/mycert/4d087088df974e869f1c0978cb100e47

```

>[!NOTE]
><span data-ttu-id="544e7-185">Hello három előző karakterláncok, CertificateThumbprint SourceVault és CertificateURL, a biztonságos Service Fabric-fürt és az tooobtain tooset szükség, amely akkor használhatja az alkalmazás biztonsági alkalmazás tanúsítványokra.</span><span class="sxs-lookup"><span data-stu-id="544e7-185">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="544e7-186">Hello karakterláncok nem menti, ha azok hello kulcs lekérdezésével tároló később nehéz tooretrieve lehet.</span><span class="sxs-lookup"><span data-stu-id="544e7-186">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

<a id="add-self-signed-certificate-to-key-vault"></a>

#### <a name="creating-a-self-signed-certificate-and-uploading-it-toohello-key-vault"></a><span data-ttu-id="544e7-187">Önaláírt tanúsítvány létrehozása, majd ismét feltölteni a toohello kulcstároló</span><span class="sxs-lookup"><span data-stu-id="544e7-187">Creating a self-signed certificate and uploading it toohello key vault</span></span>

<span data-ttu-id="544e7-188">Ha már feltöltött tanúsítványok toohello key vaultban, kihagyhatja ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="544e7-188">If you have already uploaded your certificates toohello key vault, skip this step.</span></span> <span data-ttu-id="544e7-189">Ebben a lépésben egy új önaláírt tanúsítvány létrehozása, majd ismét feltölteni a tooyour kulcstároló szolgál.</span><span class="sxs-lookup"><span data-stu-id="544e7-189">This step is for generating a new self-signed certificate and uploading it tooyour key vault.</span></span> <span data-ttu-id="544e7-190">Módosítsa a következő parancsfájl hello hello paramétereit, és futtassa, miután a rendszer kéri a tanúsítvány jelszót.</span><span class="sxs-lookup"><span data-stu-id="544e7-190">After you change hello parameters in hello following script and then run it, you should be prompted for a certificate password.</span></span>  

```powershell

$ResourceGroup = "chackowestuskv"
$VName = "chackokv2"
$SubID = "6c653126-e4ba-42cd-a1dd-f7bf96ae7a47"
$locationRegion = "westus"
$newCertName = "chackotestcertificate1"
$dnsName = "www.mycluster.westus.mydomain.com" #hello certificate's subject name must match hello domain used tooaccess hello Service Fabric cluster.
$localCertPath = "C:\MyCertificates" # location where you want hello .PFX toobe stored

 Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResourceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath

```

<span data-ttu-id="544e7-191">Ha a hiba, például az egyik látható itt, hello Ez általában azt jelenti, hogy rendelkezik-e URL-cím erőforrás-ütközés.</span><span class="sxs-lookup"><span data-stu-id="544e7-191">If you get an error, such as hello one shown here, it usually means that you have a resource URL conflict.</span></span> <span data-ttu-id="544e7-192">tooresolve hello ütközést, hello kulcstároló nevének módosítása, RG nevét, és így tovább.</span><span class="sxs-lookup"><span data-stu-id="544e7-192">tooresolve hello conflict, change hello key vault name, RG name, and so forth.</span></span>

```
Set-AzureKeyVaultSecret : hello remote name could not be resolved: 'westuskv.vault.azure.net'
At C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1:440 char:11
+ $secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $Certif ...
+           ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : CloseError: (:) [Set-AzureKeyVaultSecret], WebException
    + FullyQualifiedErrorId : Microsoft.Azure.Commands.KeyVault.SetAzureKeyVaultSecret

```

<span data-ttu-id="544e7-193">Miután hello ütközés feloldja, hello kimeneti kell néznek ki:</span><span class="sxs-lookup"><span data-stu-id="544e7-193">After hello conflict is resolved, hello output should look like this:</span></span>

```
PS C:\Users\chackdan\Documents\GitHub\Service-Fabric\Scripts\ServiceFabricRPHelpers> Invoke-AddCertToKeyVault -SubscriptionId $SubID -ResourceGroupName $ResouceGroup -Location $locationRegion -VaultName $VName -CertificateName $newCertName -Password $certPassword -CreateSelfSignedCertificate -DnsName $dnsName -OutputPath $localCertPath
Switching context tooSubscriptionId 6c343126-e4ba-52cd-a1dd-f8bf96ae7a47
Ensuring ResourceGroup chackowestuskv in westus
WARNING: hello output object type of this cmdlet will be modified in a future release.
Creating new vault westuskv1 in westus
Creating new self signed certificate at C:\MyCertificates\chackonewcertificate1.pfx
Reading pfx file from C:\MyCertificates\chackonewcertificate1.pfx
Writing secret toochackonewcertificate1 in vault westuskv1


Name  : CertificateThumbprint
Value : 96BB3CC234F9D43C25D4B547sd8DE7B569F413EE

Name  : SourceVault
Value : /subscriptions/6c653126-e4ba-52cd-a1dd-f8bf96ae7a47/resourceGroups/chackowestuskv/providers/Microsoft.KeyVault/vaults/westuskv1

Name  : CertificateURL
Value : https://westuskv1.vault.azure.net:443/secrets/chackonewcertificate1/ee247291e45d405b8c8bbf81782d12bd

```

>[!NOTE]
><span data-ttu-id="544e7-194">Hello három előző karakterláncok, CertificateThumbprint SourceVault és CertificateURL, a biztonságos Service Fabric-fürt és az tooobtain tooset szükség, amely akkor használhatja az alkalmazás biztonsági alkalmazás tanúsítványokra.</span><span class="sxs-lookup"><span data-stu-id="544e7-194">You need hello three preceding strings, CertificateThumbprint, SourceVault, and CertificateURL, tooset up a secure Service Fabric cluster and tooobtain any application certificates that you might be using for application security.</span></span> <span data-ttu-id="544e7-195">Hello karakterláncok nem menti, ha azok hello kulcs lekérdezésével tároló később nehéz tooretrieve lehet.</span><span class="sxs-lookup"><span data-stu-id="544e7-195">If you do not save hello strings, it can be difficult tooretrieve them by querying hello key vault later.</span></span>

 <span data-ttu-id="544e7-196">Ezen a ponton rendelkeznie kell a következő helyen elemek hello:</span><span class="sxs-lookup"><span data-stu-id="544e7-196">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="544e7-197">hello kulcstároló erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="544e7-197">hello key vault resource group.</span></span>
* <span data-ttu-id="544e7-198">hello kulcstartó és az URL-CÍMÉT (PowerShell kimeneti megelőző hello SourceVault néven).</span><span class="sxs-lookup"><span data-stu-id="544e7-198">hello key vault and its URL (called SourceVault in hello preceding PowerShell output).</span></span>
* <span data-ttu-id="544e7-199">hello fürt kiszolgálói hitelesítési tanúsítványt és annak hello kulcstároló URL-címet.</span><span class="sxs-lookup"><span data-stu-id="544e7-199">hello cluster server authentication certificate and its URL in hello key vault.</span></span>
* <span data-ttu-id="544e7-200">hello alkalmazás tanúsítványokat és azok hello kulcstároló URL-címeit.</span><span class="sxs-lookup"><span data-stu-id="544e7-200">hello application certificates and their URLs in hello key vault.</span></span>


<a id="add-AAD-for-client"></a>

## <a name="set-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="544e7-201">Az ügyfél-hitelesítéshez az Azure Active Directory beállítása</span><span class="sxs-lookup"><span data-stu-id="544e7-201">Set up Azure Active Directory for client authentication</span></span>

<span data-ttu-id="544e7-202">Az Azure AD lehetővé teszi, hogy a szervezetek (úgynevezett bérlők) toomanage felhasználói hozzáférés tooapplications.</span><span class="sxs-lookup"><span data-stu-id="544e7-202">Azure AD enables organizations (known as tenants) toomanage user access tooapplications.</span></span> <span data-ttu-id="544e7-203">Alkalmazások oszthatók rendelkező webalapú bejelentkezési felhasználói felület és az egy natív ügyfél révén azokat.</span><span class="sxs-lookup"><span data-stu-id="544e7-203">Applications are divided into those with a web-based sign-in UI and those with a native client experience.</span></span> <span data-ttu-id="544e7-204">Ebben a cikkben azt feltételezzük, hogy már létrehozta a bérlő.</span><span class="sxs-lookup"><span data-stu-id="544e7-204">In this article, we assume that you have already created a tenant.</span></span> <span data-ttu-id="544e7-205">Ha nem, olvassa el [hogyan tooget egy Azure Active Directory-bérlőazonosítóra][active-directory-howto-tenant].</span><span class="sxs-lookup"><span data-stu-id="544e7-205">If you have not, start by reading [How tooget an Azure Active Directory tenant][active-directory-howto-tenant].</span></span>

<span data-ttu-id="544e7-206">A Service Fabric-fürt biztosít több belépési pontok tooits felügyeleti funkciót, beleértve a webalapú hello [Service Fabric Explorer] [ service-fabric-visualizing-your-cluster] és [Visual Studio] [service-fabric-manage-application-in-visual-studio].</span><span class="sxs-lookup"><span data-stu-id="544e7-206">A Service Fabric cluster offers several entry points tooits management functionality, including hello web-based [Service Fabric Explorer][service-fabric-visualizing-your-cluster] and [Visual Studio][service-fabric-manage-application-in-visual-studio].</span></span> <span data-ttu-id="544e7-207">Ennek eredményeképpen hozzon létre két az Azure AD alkalmazások toocontrol hozzáférés toohello fürt, egy webes alkalmazás és egy natív alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="544e7-207">As a result, you create two Azure AD applications toocontrol access toohello cluster, one web application and one native application.</span></span>

<span data-ttu-id="544e7-208">egyes hello lépések részt az Azure AD konfigurálása a Service Fabric-fürt toosimplify, a Windows PowerShell-parancsfájlokkal létrehoztunk.</span><span class="sxs-lookup"><span data-stu-id="544e7-208">toosimplify some of hello steps involved in configuring Azure AD with a Service Fabric cluster, we have created a set of Windows PowerShell scripts.</span></span>

> [!NOTE]
> <span data-ttu-id="544e7-209">Hello hello fürt létrehozása előtt az alábbi lépések elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="544e7-209">You must complete hello following steps before you create hello cluster.</span></span> <span data-ttu-id="544e7-210">Hello parancsfájlok a fürt nevét és a végpontok várható, mivel a hello értékeket kell megtervezni, és nem az, hogy már létrehozta értékeket.</span><span class="sxs-lookup"><span data-stu-id="544e7-210">Because hello scripts expect cluster names and endpoints, hello values should be planned and not values that you have already created.</span></span>

1. <span data-ttu-id="544e7-211">[Töltse le a hello parancsfájlok] [ sf-aad-ps-script-download] tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="544e7-211">[Download hello scripts][sf-aad-ps-script-download] tooyour computer.</span></span>
2. <span data-ttu-id="544e7-212">Kattintson a jobb gombbal hello zip-fájl, jelölje be **tulajdonságok**, jelölje be hello **Unblock** jelölőnégyzetet, majd kattintson a **alkalmaz**.</span><span class="sxs-lookup"><span data-stu-id="544e7-212">Right-click hello zip file, select **Properties**, select hello **Unblock** check box, and then click **Apply**.</span></span>
3. <span data-ttu-id="544e7-213">Hello zip-fájl kicsomagolásához.</span><span class="sxs-lookup"><span data-stu-id="544e7-213">Extract hello zip file.</span></span>
4. <span data-ttu-id="544e7-214">Futtatás `SetupApplications.ps1`, és adja meg a hello TenantId, fürtnév, és WebApplicationReplyUrl paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="544e7-214">Run `SetupApplications.ps1`, and provide hello TenantId, ClusterName, and WebApplicationReplyUrl as parameters.</span></span> <span data-ttu-id="544e7-215">Példa:</span><span class="sxs-lookup"><span data-stu-id="544e7-215">For example:</span></span>

    ```powershell
    .\SetupApplications.ps1 -TenantId '690ec069-8200-4068-9d01-5aaf188e557a' -ClusterName 'mycluster' -WebApplicationReplyUrl 'https://mycluster.westus.cloudapp.azure.com:19080/Explorer/index.html'
    ```

    <span data-ttu-id="544e7-216">A TenantId találhatja meg hello PowerShell-parancs végrehajtása `Get-AzureSubscription`.</span><span class="sxs-lookup"><span data-stu-id="544e7-216">You can find your TenantId by executing hello PowerShell command `Get-AzureSubscription`.</span></span> <span data-ttu-id="544e7-217">Ez a parancs végrehajtása hello TenantId minden előfizetés jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="544e7-217">Executing this command displays hello TenantId for every subscription.</span></span>

    <span data-ttu-id="544e7-218">ClusterName hello parancsfájl által létrehozott használt tooprefix hello Azure AD-alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="544e7-218">ClusterName is used tooprefix hello Azure AD applications that are created by hello script.</span></span> <span data-ttu-id="544e7-219">Nem kell toomatch hello tényleges fürtnév pontosan.</span><span class="sxs-lookup"><span data-stu-id="544e7-219">It does not need toomatch hello actual cluster name exactly.</span></span> <span data-ttu-id="544e7-220">Tervezett csak toomake az egyszerűbb toomap az Azure AD összetevők toohello Service Fabric-fürt, amely akkor van használatban.</span><span class="sxs-lookup"><span data-stu-id="544e7-220">It is intended only toomake it easier toomap Azure AD artifacts toohello Service Fabric cluster that they're being used with.</span></span>

    <span data-ttu-id="544e7-221">WebApplicationReplyUrl hello alapértelmezett végpont az Azure AD tooyour felhasználók ad vissza, miután a bejelentkezés befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="544e7-221">WebApplicationReplyUrl is hello default endpoint that Azure AD returns tooyour users after they finish signing in.</span></span> <span data-ttu-id="544e7-222">Ez a végpont hello Service Fabric Explorer végpont a fürt, amely alapértelmezés szerint állítja be:</span><span class="sxs-lookup"><span data-stu-id="544e7-222">Set this endpoint as hello Service Fabric Explorer endpoint for your cluster, which by default is:</span></span>

    <span data-ttu-id="544e7-223">https://&lt;cluster_domain&gt;: 19080/Explorer</span><span class="sxs-lookup"><span data-stu-id="544e7-223">https://&lt;cluster_domain&gt;:19080/Explorer</span></span>

    <span data-ttu-id="544e7-224">A hello Azure AD bérlői rendszergazdai jogosultságokkal rendelkező fiók tooan rákérdezéses toosign áll.</span><span class="sxs-lookup"><span data-stu-id="544e7-224">You are prompted toosign in tooan account that has administrative privileges for hello Azure AD tenant.</span></span> <span data-ttu-id="544e7-225">Miután bejelentkezik, hello parancsfájlt hoz létre hello web- és natív alkalmazások toorepresent a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="544e7-225">After you sign in, hello script creates hello web and native applications toorepresent your Service Fabric cluster.</span></span> <span data-ttu-id="544e7-226">Ha megnézi a hello hello bérlői alkalmazások [a klasszikus Azure portálon][azure-classic-portal], két új bejegyzést kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="544e7-226">If you look at hello tenant's applications in hello [Azure classic portal][azure-classic-portal], you should see two new entries:</span></span>

   * <span data-ttu-id="544e7-227">*ClusterName*\_fürt</span><span class="sxs-lookup"><span data-stu-id="544e7-227">*ClusterName*\_Cluster</span></span>
   * <span data-ttu-id="544e7-228">*ClusterName*\_ügyfél</span><span class="sxs-lookup"><span data-stu-id="544e7-228">*ClusterName*\_Client</span></span>

   <span data-ttu-id="544e7-229">hello parancsfájl kinyomtatja hello JSON hello Azure Resource Manager sablonhoz szükséges hello fürt létrehozásakor hello a következő szakaszban, hogy a jó ötlet tookeep hello PowerShell ablak megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="544e7-229">hello script prints hello JSON required by hello Azure Resource Manager template when you create hello cluster in hello next section, so it's a good idea tookeep hello PowerShell window open.</span></span>

```json
"azureActiveDirectory": {
  "tenantId":"<guid>",
  "clusterApplication":"<guid>",
  "clientApplication":"<guid>"
},
```

## <a name="create-a-service-fabric-cluster-resource-manager-template"></a><span data-ttu-id="544e7-230">Service Fabric-fürt Resource Manager sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="544e7-230">Create a Service Fabric cluster Resource Manager template</span></span>
<span data-ttu-id="544e7-231">Az előző hello kimenete hello ebben a szakaszban a Service Fabric fürt Resource Manager-sablon PowerShell-parancsok használhatók.</span><span class="sxs-lookup"><span data-stu-id="544e7-231">In this section, hello outputs of hello preceding PowerShell commands are used in a Service Fabric cluster Resource Manager template.</span></span>

<span data-ttu-id="544e7-232">A minta Resource Manager-sablonok találhatók hello [Azure gyors üzembe helyezési sablon gyűjteménye a Githubon][azure-quickstart-templates].</span><span class="sxs-lookup"><span data-stu-id="544e7-232">Sample Resource Manager templates are available in hello [Azure quick-start template gallery on GitHub][azure-quickstart-templates].</span></span> <span data-ttu-id="544e7-233">Ezek a sablonok kiindulási pontként használható a fürt sablon.</span><span class="sxs-lookup"><span data-stu-id="544e7-233">These templates can be used as a starting point for your cluster template.</span></span>

### <a name="create-hello-resource-manager-template"></a><span data-ttu-id="544e7-234">Hello Resource Manager-sablon létrehozása</span><span class="sxs-lookup"><span data-stu-id="544e7-234">Create hello Resource Manager template</span></span>
<span data-ttu-id="544e7-235">Ez az útmutató használ hello [biztonságos 5-csomópontot tartalmazó fürtben] [ service-fabric-secure-cluster-5-node-1-nodetype] példa sablon és a sablon paramétereit.</span><span class="sxs-lookup"><span data-stu-id="544e7-235">This guide uses hello [5-node secure cluster][service-fabric-secure-cluster-5-node-1-nodetype] example template and template parameters.</span></span> <span data-ttu-id="544e7-236">Töltse le `azuredeploy.json` és `azuredeploy.parameters.json` tooyour számítógépet, majd nyissa meg a fájlt a kedvenc szövegszerkesztőjével.</span><span class="sxs-lookup"><span data-stu-id="544e7-236">Download `azuredeploy.json` and `azuredeploy.parameters.json` tooyour computer and open both files in your favorite text editor.</span></span>

### <a name="add-certificates"></a><span data-ttu-id="544e7-237">Tanúsítványok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="544e7-237">Add certificates</span></span>
<span data-ttu-id="544e7-238">Tanúsítványok tooa fürt Resource Manager-sablon hivatkozik, amely tartalmazza a hello tanúsítvány kulcsait kulcstároló hello hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="544e7-238">You add certificates tooa cluster Resource Manager template by referencing hello key vault that contains hello certificate keys.</span></span> <span data-ttu-id="544e7-239">Azt javasoljuk, hogy hello kulcstároló értékek helyez egy Resource Manager sablon paraméterfájl.</span><span class="sxs-lookup"><span data-stu-id="544e7-239">We recommend that you place hello key-vault values in a Resource Manager template parameters file.</span></span> <span data-ttu-id="544e7-240">Így tartja a Resource Manager hello sablonfájl újrafelhasználható és szabad értékek adott tooa központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="544e7-240">Doing so keeps hello Resource Manager template file reusable and free of values specific tooa deployment.</span></span>

#### <a name="add-all-certificates-toohello-virtual-machine-scale-set-osprofile"></a><span data-ttu-id="544e7-241">Minden tanúsítványok toohello virtuálisgép-méretezési csoport osProfile hozzáadása</span><span class="sxs-lookup"><span data-stu-id="544e7-241">Add all certificates toohello virtual machine scale set osProfile</span></span>
<span data-ttu-id="544e7-242">Minden tanúsítvány hello fürtben telepített hello osProfile szakaszában hello méretezési készlet erőforrás (Microsoft.Compute/virtualMachineScaleSets) kell konfigurálni.</span><span class="sxs-lookup"><span data-stu-id="544e7-242">Every certificate that's installed in hello cluster must be configured in hello osProfile section of hello scale set resource (Microsoft.Compute/virtualMachineScaleSets).</span></span> <span data-ttu-id="544e7-243">Ez a művelet arra utasítja a hello erőforrás szolgáltató tooinstall hello tanúsítvány hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="544e7-243">This action instructs hello resource provider tooinstall hello certificate on hello VMs.</span></span> <span data-ttu-id="544e7-244">A telepítés hello fürt tanúsítvány és a bármely alkalmazás biztonsági tanúsítványok toouse tervezi meg az alkalmazások is tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="544e7-244">This installation includes both hello cluster certificate and any application security certificates that you plan toouse for your applications:</span></span>

```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "osProfile": {
      ...
      "secrets": [
        {
          "sourceVault": {
            "id": "[parameters('sourceVaultValue')]"
          },
          "vaultCertificates": [
            {
              "certificateStore": "[parameters('clusterCertificateStorevalue')]",
              "certificateUrl": "[parameters('clusterCertificateUrlValue')]"
            },
            {
              "certificateStore": "[parameters('applicationCertificateStorevalue')",
              "certificateUrl": "[parameters('applicationCertificateUrlValue')]"
            },
            ...
          ]
        }
      ]
    }
  }
}
```

#### <a name="configure-hello-service-fabric-cluster-certificate"></a><span data-ttu-id="544e7-245">Hello Service Fabric-fürt tanúsítvány konfigurálása</span><span class="sxs-lookup"><span data-stu-id="544e7-245">Configure hello Service Fabric cluster certificate</span></span>
<span data-ttu-id="544e7-246">hello fürt hitelesítési tanúsítványt kell konfigurálni mindkét hello Service Fabric-fürt erőforrás (Microsoft.ServiceFabric/clusters) pedig a Service Fabric-bővítményt a virtuálisgép-méretezési hello állítja be a hello virtuálisgép-méretezési készlet erőforrás.</span><span class="sxs-lookup"><span data-stu-id="544e7-246">hello cluster authentication certificate must be configured in both hello Service Fabric cluster resource (Microsoft.ServiceFabric/clusters) and hello Service Fabric extension for virtual machine scale sets in hello virtual machine scale set resource.</span></span> <span data-ttu-id="544e7-247">Ezzel az elrendezéssel hello Service Fabric erőforrás-szolgáltató tooconfigure lehetővé teszi, hogy a fürt hitelesítési és felügyeleti végpontok kiszolgálóhitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="544e7-247">This arrangement allows hello Service Fabric resource provider tooconfigure it for use for cluster authentication and server authentication for management endpoints.</span></span>

##### <a name="virtual-machine-scale-set-resource"></a><span data-ttu-id="544e7-248">Virtuálisgép-méretezési erőforrás:</span><span class="sxs-lookup"><span data-stu-id="544e7-248">Virtual machine scale set resource:</span></span>
```json
{
  "apiVersion": "2016-03-30",
  "type": "Microsoft.Compute/virtualMachineScaleSets",
  ...
  "properties": {
    ...
    "virtualMachineProfile": {
      "extensionProfile": {
        "extensions": [
          {
            "name": "[concat('ServiceFabricNodeVmExt','_vmNodeType0Name')]",
            "properties": {
              ...
              "settings": {
                ...
                "certificate": {
                  "thumbprint": "[parameters('clusterCertificateThumbprint')]",
                  "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
                },
                ...
              }
            }
          }
        ]
      }
    }
  }
}
```

##### <a name="service-fabric-resource"></a><span data-ttu-id="544e7-249">A Service Fabric-erőforrás:</span><span class="sxs-lookup"><span data-stu-id="544e7-249">Service Fabric resource:</span></span>
```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  "location": "[parameters('clusterLocation')]",
  "dependsOn": [
    "[concat('Microsoft.Storage/storageAccounts/', variables('supportLogStorageAccountName'))]"
  ],
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStoreValue')]"
    },
    ...
  }
}
```

### <a name="insert-azure-ad-configuration"></a><span data-ttu-id="544e7-250">Helyezze be az Azure AD konfigurálása</span><span class="sxs-lookup"><span data-stu-id="544e7-250">Insert Azure AD configuration</span></span>
<span data-ttu-id="544e7-251">korábban létrehozott hello Azure AD-konfigurációs illeszthető közvetlenül a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="544e7-251">hello Azure AD configuration that you created earlier can be inserted directly into your Resource Manager template.</span></span> <span data-ttu-id="544e7-252">Azonban azt javasoljuk, hogy először hello értékek azokat a paramétereket fájl tookeep hello Resource Manager sablon egyszer használatos kiolvasni szabad értékek adott tooa központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="544e7-252">However, we recommended that you first extract hello values into a parameters file tookeep hello Resource Manager template reusable and free of values specific tooa deployment.</span></span>

```json
{
  "apiVersion": "2016-03-01",
  "type": "Microsoft.ServiceFabric/clusters",
  "name": "[parameters('clusterName')]",
  ...
  "properties": {
    "certificate": {
      "thumbprint": "[parameters('clusterCertificateThumbprint')]",
      "x509StoreName": "[parameters('clusterCertificateStorevalue')]"
    },
    ...
    "azureActiveDirectory": {
      "tenantId": "[parameters('aadTenantId')]",
      "clusterApplication": "[parameters('aadClusterApplicationId')]",
      "clientApplication": "[parameters('aadClientApplicationId')]"
    },
    ...
  }
}
```

### <span data-ttu-id="544e7-253">< a "konfigurálása – arm" ></a>Sablonparaméterek erőforrás-kezelő konfigurálása</span><span class="sxs-lookup"><span data-stu-id="544e7-253"><a "configure-arm" ></a>Configure Resource Manager template parameters</span></span>
<!--- Loc Comment: It seems that <a "configure-arm" > must be replaced with <a name="configure-arm"></a> since hello link seems not toobe redirecting correctly --->
<span data-ttu-id="544e7-254">Végezetül hello kimeneti értékeinek hello kulcstartó és az Azure AD PowerShell parancsok toopopulate hello paraméterfájl használata:</span><span class="sxs-lookup"><span data-stu-id="544e7-254">Finally, use hello output values from hello key vault and Azure AD PowerShell commands toopopulate hello parameters file:</span></span>

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        ...
        "clusterCertificateStoreValue": {
            "value": "My"
        },
        "clusterCertificateThumbprint": {
            "value": "<thumbprint>"
        },
        "clusterCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myclustercert/4d087088df974e869f1c0978cb100e47"
        },
        "applicationCertificateStorevalue": {
            "value": "My"
        },
        "applicationCertificateUrlValue": {
            "value": "https://myvault.vault.azure.net:443/secrets/myapplicationcert/2e035058ae274f869c4d0348ca100f08"
        },
        "sourceVaultvalue": {
            "value": "/subscriptions/<guid>/resourceGroups/mycluster-keyvault/providers/Microsoft.KeyVault/vaults/myvault"
        },
        "aadTenantId": {
            "value": "<guid>"
        },
        "aadClusterApplicationId": {
            "value": "<guid>"
        },
        "aadClientApplicationId": {
            "value": "<guid>"
        },
        ...
    }
}
```
<span data-ttu-id="544e7-255">Ezen a ponton rendelkeznie kell a következő helyen elemek hello:</span><span class="sxs-lookup"><span data-stu-id="544e7-255">At this point, you should have hello following elements in place:</span></span>

* <span data-ttu-id="544e7-256">Kulcstároló erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="544e7-256">Key vault resource group</span></span>
  * <span data-ttu-id="544e7-257">Key Vault</span><span class="sxs-lookup"><span data-stu-id="544e7-257">Key vault</span></span>
  * <span data-ttu-id="544e7-258">Fürt kiszolgálói hitelesítési tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="544e7-258">Cluster server authentication certificate</span></span>
  * <span data-ttu-id="544e7-259">Adatok rejtjelezése tanúsítvány</span><span class="sxs-lookup"><span data-stu-id="544e7-259">Data encipherment certificate</span></span>
* <span data-ttu-id="544e7-260">Az Azure Active Directory-bérlő</span><span class="sxs-lookup"><span data-stu-id="544e7-260">Azure Active Directory tenant</span></span>
  * <span data-ttu-id="544e7-261">Az Azure AD-alkalmazást a web-alapú felügyeleti és a Service Fabric Explorerrel</span><span class="sxs-lookup"><span data-stu-id="544e7-261">Azure AD application for web-based management and Service Fabric Explorer</span></span>
  * <span data-ttu-id="544e7-262">Natív ügyfél kezelése az Azure AD-alkalmazás</span><span class="sxs-lookup"><span data-stu-id="544e7-262">Azure AD application for native client management</span></span>
  * <span data-ttu-id="544e7-263">Felhasználók és a hozzárendelt szerepkör</span><span class="sxs-lookup"><span data-stu-id="544e7-263">Users and their assigned roles</span></span>
* <span data-ttu-id="544e7-264">A Service Fabric-fürt Resource Manager-sablon</span><span class="sxs-lookup"><span data-stu-id="544e7-264">Service Fabric cluster Resource Manager template</span></span>
  * <span data-ttu-id="544e7-265">Kulcstároló keresztül konfigurált tanúsítványok</span><span class="sxs-lookup"><span data-stu-id="544e7-265">Certificates configured through key vault</span></span>
  * <span data-ttu-id="544e7-266">Az Azure Active Directory konfigurálva</span><span class="sxs-lookup"><span data-stu-id="544e7-266">Azure Active Directory configured</span></span>

<span data-ttu-id="544e7-267">hello a következő ábra azt mutatja be, ahol a kulcstartót és az Azure Active Directory beállítása felelnek meg a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="544e7-267">hello following diagram illustrates where your key vault and Azure AD configuration fit into your Resource Manager template.</span></span>

![Erőforrás-kezelő kapcsolatfüggőségi térképének megjelenítéséhez][cluster-security-arm-dependency-map]

## <a name="create-hello-cluster"></a><span data-ttu-id="544e7-269">Hello fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="544e7-269">Create hello cluster</span></span>
<span data-ttu-id="544e7-270">Most már áll készen toocreate hello fürt használatával [Azure-erőforrás sablon-üzembehelyezés][resource-group-template-deploy].</span><span class="sxs-lookup"><span data-stu-id="544e7-270">You are now ready toocreate hello cluster by using [Azure resource template deployment][resource-group-template-deploy].</span></span>

#### <a name="test-it"></a><span data-ttu-id="544e7-271">Tesztelheti</span><span class="sxs-lookup"><span data-stu-id="544e7-271">Test it</span></span>
<span data-ttu-id="544e7-272">A Resource Manager-sablon az alábbi PowerShell-parancs tootest hello használata a paraméterfájl:</span><span class="sxs-lookup"><span data-stu-id="544e7-272">Use hello following PowerShell command tootest your Resource Manager template with a parameters file:</span></span>

```powershell
Test-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

#### <a name="deploy-it"></a><span data-ttu-id="544e7-273">Telepítés</span><span class="sxs-lookup"><span data-stu-id="544e7-273">Deploy it</span></span>
<span data-ttu-id="544e7-274">Ha hello Resource Manager sablon teszt sikeres, használja a következő PowerShell-parancs toodeploy hello a Resource Manager-sablon paraméterfájl:</span><span class="sxs-lookup"><span data-stu-id="544e7-274">If hello Resource Manager template test passes, use hello following PowerShell command toodeploy your Resource Manager template with a parameters file:</span></span>

```powershell
New-AzureRmResourceGroupDeployment -ResourceGroupName "myresourcegroup" -TemplateFile .\azuredeploy.json -TemplateParameterFile .\azuredeploy.parameters.json
```

<a name="assign-roles"></a>

## <a name="assign-users-tooroles"></a><span data-ttu-id="544e7-275">Felhasználók tooroles hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="544e7-275">Assign users tooroles</span></span>
<span data-ttu-id="544e7-276">A létrehozást követően hello alkalmazások toorepresent a fürthöz, a felhasználók toohello szerepkörök hozzárendelése a Service Fabric által támogatott: olvasási és a rendszergazda segítségét. Hello szerepköröket rendelhet hello segítségével [a klasszikus Azure portálon][azure-classic-portal].</span><span class="sxs-lookup"><span data-stu-id="544e7-276">After you have created hello applications toorepresent your cluster, assign your users toohello roles supported by Service Fabric: read-only and admin. You can assign hello roles by using hello [Azure classic portal][azure-classic-portal].</span></span>

1. <span data-ttu-id="544e7-277">A hello Azure-portálon, válassza a tooyour bérlői, majd válassza ki **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="544e7-277">In hello Azure portal, go tooyour tenant, and then select **Applications**.</span></span>
2. <span data-ttu-id="544e7-278">Válassza ki a hello webalkalmazás, melynek neve például `myTestCluster_Cluster`.</span><span class="sxs-lookup"><span data-stu-id="544e7-278">Select hello web application, which has a name like `myTestCluster_Cluster`.</span></span>
3. <span data-ttu-id="544e7-279">Kattintson a hello **felhasználók** fülre.</span><span class="sxs-lookup"><span data-stu-id="544e7-279">Click hello **Users** tab.</span></span>
4. <span data-ttu-id="544e7-280">Válassza ki a felhasználói tooassign, és kattintson a hello **hozzárendelése** üdvözlő képernyőt hello alján gombra.</span><span class="sxs-lookup"><span data-stu-id="544e7-280">Select a user tooassign, and then click hello **Assign** button at hello bottom of hello screen.</span></span>

    ![Rendelje hozzá a felhasználók tooroles gomb][assign-users-to-roles-button]
5. <span data-ttu-id="544e7-282">Válassza ki a hello szerepkör tooassign toohello felhasználót.</span><span class="sxs-lookup"><span data-stu-id="544e7-282">Select hello role tooassign toohello user.</span></span>

    !["A felhasználók hozzárendelése" párbeszédpanel][assign-users-to-roles-dialog]

> [!NOTE]
> <span data-ttu-id="544e7-284">További információ a szerepkörök a Service Fabric: [szerepköralapú hozzáférés-vezérlés a Service Fabric ügyfelek](service-fabric-cluster-security-roles.md).</span><span class="sxs-lookup"><span data-stu-id="544e7-284">For more information about roles in Service Fabric, see [Role-based access control for Service Fabric clients](service-fabric-cluster-security-roles.md).</span></span>
>
>

 <a name="secure-linux-clusters"></a>
 <!--- Loc Comment: It seems that letter S in cluster was missing, which caused hello wrong redirection of hello link --->

## <a name="create-secure-clusters-on-linux"></a><span data-ttu-id="544e7-285">Hozzon létre a biztonságos fürtök Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="544e7-285">Create secure clusters on Linux</span></span>
<span data-ttu-id="544e7-286">toomake hello folyamat könnyebb adtunk egy [segítő parancsfájl](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span><span class="sxs-lookup"><span data-stu-id="544e7-286">toomake hello process easier, we have provided a [helper script](http://github.com/ChackDan/Service-Fabric/tree/master/Scripts/CertUpload4Linux).</span></span> <span data-ttu-id="544e7-287">A segítő parancsfájl használata előtt győződjön meg arról, hogy már van telepítve az Azure parancssori felület (CLI), és az elérési úthoz.</span><span class="sxs-lookup"><span data-stu-id="544e7-287">Before you use this helper script, ensure that you already have Azure command-line interface (CLI) installed, and it is in your path.</span></span> <span data-ttu-id="544e7-288">Győződjön meg arról, hogy hello parancsfájl engedélyek tooexecute futtatásával `chmod +x cert_helper.py` azt letöltése után.</span><span class="sxs-lookup"><span data-stu-id="544e7-288">Make sure that hello script has permissions tooexecute by running `chmod +x cert_helper.py` after downloading it.</span></span> <span data-ttu-id="544e7-289">hello első lépése az Azure-fiók tooyour toosign hello parancssori felület használatával `azure login` parancsot.</span><span class="sxs-lookup"><span data-stu-id="544e7-289">hello first step is toosign in tooyour Azure account by using CLI with hello `azure login` command.</span></span> <span data-ttu-id="544e7-290">Tooyour Azure-fiókkal történő bejelentkezés után hello segítő parancsfájl használata a CA tanúsítvány aláírására használatos, mint hello parancs azt mutatja be a következő:</span><span class="sxs-lookup"><span data-stu-id="544e7-290">After signing in tooyour Azure account, use hello helper script with your CA signed certificate, as hello following command shows:</span></span>

```sh
./cert_helper.py [-h] CERT_TYPE [-ifile INPUT_CERT_FILE] [-sub SUBSCRIPTION_ID] [-rgname RESOURCE_GROUP_NAME] [-kv KEY_VAULT_NAME] [-sname CERTIFICATE_NAME] [-l LOCATION] [-p PASSWORD]
```

<span data-ttu-id="544e7-291">hello - ifile paramétere lehet egy .pfx fájlba vagy a .pem fájl bemenetként hello tanúsítvány típus (pfx vagy pem, vagy ha az önaláírt tanúsítvány ss).</span><span class="sxs-lookup"><span data-stu-id="544e7-291">hello -ifile parameter can take a .pfx file or a .pem file as input, with hello certificate type (pfx or pem, or ss if it is a self-signed certificate).</span></span>
<span data-ttu-id="544e7-292">hello súgószöveg hello paraméter -h megjeleníti.</span><span class="sxs-lookup"><span data-stu-id="544e7-292">hello parameter -h prints out hello help text.</span></span>


<span data-ttu-id="544e7-293">Ez a parancs visszaadja a következő három karakterláncok hello kimenetként hello:</span><span class="sxs-lookup"><span data-stu-id="544e7-293">This command returns hello following three strings as hello output:</span></span>

* <span data-ttu-id="544e7-294">SourceVaultID, amely hello hello azonosítója hozott létre az Ön új KeyVault ResourceGroup</span><span class="sxs-lookup"><span data-stu-id="544e7-294">SourceVaultID, which is hello ID for hello new KeyVault ResourceGroup it created for you</span></span>
* <span data-ttu-id="544e7-295">CertificateUrl hello tanúsítvány eléréséhez</span><span class="sxs-lookup"><span data-stu-id="544e7-295">CertificateUrl for accessing hello certificate</span></span>
* <span data-ttu-id="544e7-296">CertificateThumbprint, amelyek használják a hitelesítéshez</span><span class="sxs-lookup"><span data-stu-id="544e7-296">CertificateThumbprint, which is used for authentication</span></span>

<span data-ttu-id="544e7-297">hello a következő példa bemutatja, hogyan toouse hello parancsot:</span><span class="sxs-lookup"><span data-stu-id="544e7-297">hello following example shows how toouse hello command:</span></span>

```sh
./cert_helper.py pfx -sub "fffffff-ffff-ffff-ffff-ffffffffffff"  -rgname "mykvrg" -kv "mykevname" -ifile "/home/test/cert.pfx" -sname "mycert" -l "East US" -p "pfxtest"
```
<span data-ttu-id="544e7-298">Az alábbiak szerint hello három karakterláncok parancs által biztosított megelőző hello végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="544e7-298">Executing hello preceding command gives you hello three strings as follows:</span></span>

```sh
SourceVault: /subscriptions/fffffff-ffff-ffff-ffff-ffffffffffff/resourceGroups/mykvrg/providers/Microsoft.KeyVault/vaults/mykvname
CertificateUrl: https://myvault.vault.azure.net/secrets/mycert/00000000000000000000000000000000
CertificateThumbprint: 0xfffffffffffffffffffffffffffffffffffffffff
```

<span data-ttu-id="544e7-299">hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata.</span><span class="sxs-lookup"><span data-stu-id="544e7-299">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="544e7-300">A megfelelés szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="544e7-300">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="544e7-301">Nem szerezze be egy SSL-tanúsítványt egy hitelesítésszolgáltatóból hello `.cloudapp.azure.com` tartomány.</span><span class="sxs-lookup"><span data-stu-id="544e7-301">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="544e7-302">Be kell szereznie egy egyéni tartománynevet a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="544e7-302">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="544e7-303">A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="544e7-303">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="544e7-304">A tulajdonos neve hello bejegyzések toocreate biztonságos Service Fabric-fürt (nélkül az Azure AD), részben ismertetett módon [erőforrás-kezelő konfigurálása Sablonparaméterek](#configure-arm).</span><span class="sxs-lookup"><span data-stu-id="544e7-304">These subject names are hello entries you need toocreate a secure Service Fabric cluster (without Azure AD), as described at [Configure Resource Manager template parameters](#configure-arm).</span></span> <span data-ttu-id="544e7-305">Hello utasításokat követve csatlakozhat toohello biztonságos fürt [ügyfél-hozzáférési tooa fürt hitelesítéséhez](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="544e7-305">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span> <span data-ttu-id="544e7-306">Linux preview fürtök nem támogatják az Azure AD-alapú hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="544e7-306">Linux preview clusters do not support Azure AD authentication.</span></span> <span data-ttu-id="544e7-307">Felügyelet és az ügyfél szerepköröket rendelhet, lásd: hello [szerepkörök toousers hozzárendelése](#assign-roles) szakasz.</span><span class="sxs-lookup"><span data-stu-id="544e7-307">You can assign admin and client roles as described in hello [Assign roles toousers](#assign-roles) section.</span></span> <span data-ttu-id="544e7-308">Felügyelet és az ügyfél Linux preview fürt szerepkörök megadásakor tooprovide tanúsítvány-ujjlenyomatok hitelesítési rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="544e7-308">When you specify admin and client roles for a Linux preview cluster, you have tooprovide certificate thumbprints for authentication.</span></span> <span data-ttu-id="544e7-309">(Nincs megadva hello tulajdonos neve, mert a visszavont tanúsítványok, sem a tanúsítványlánc ellenőrzése zajlik, ebben az előzetes kiadásban.)</span><span class="sxs-lookup"><span data-stu-id="544e7-309">(You do not provide hello subject name, because no chain validation or revocation is being performed in this preview release.)</span></span>

<span data-ttu-id="544e7-310">Ha önaláírt tanúsítványt toouse tesztelési, használhatja ugyanazon parancsfájl toogenerate egy hello.</span><span class="sxs-lookup"><span data-stu-id="544e7-310">If you want toouse a self-signed certificate for testing, you can use hello same script toogenerate one.</span></span> <span data-ttu-id="544e7-311">Majd feltöltheti hello tanúsítvány tooyour kulcstároló hello jelző megadásával `ss` hello tanúsítvány elérési útja és a tanúsítvány nevének megadása helyett.</span><span class="sxs-lookup"><span data-stu-id="544e7-311">You can then upload hello certificate tooyour key vault by providing hello flag `ss` instead of providing hello certificate path and certificate name.</span></span> <span data-ttu-id="544e7-312">Például tekintse meg a következő parancs létrehozása és feltöltése egy önaláírt tanúsítványt hello:</span><span class="sxs-lookup"><span data-stu-id="544e7-312">For example, see hello following command for creating and uploading a self-signed certificate:</span></span>

```sh
./cert_helper.py ss -rgname "mykvrg" -sub "fffffff-ffff-ffff-ffff-ffffffffffff" -kv "mykevname"   -sname "mycert" -l "East US" -p "selftest" -subj "mytest.eastus.cloudapp.net"
```
<span data-ttu-id="544e7-313">Ez a parancs beolvasása hello azonos három karakterláncok: SourceVault CertificateUrl és CertificateThumbprint.</span><span class="sxs-lookup"><span data-stu-id="544e7-313">This command returns hello same three strings: SourceVault, CertificateUrl, and CertificateThumbprint.</span></span> <span data-ttu-id="544e7-314">Biztonságos Linux-fürt és a hely hol helyezkedik el, hello önaláírt tanúsítványt, hello karakterláncok toocreate használhatja.</span><span class="sxs-lookup"><span data-stu-id="544e7-314">You can then use hello strings toocreate both a secure Linux cluster and a location where hello self-signed certificate is placed.</span></span> <span data-ttu-id="544e7-315">Hello önaláírt tanúsítvány tooconnect toohello fürt van szüksége.</span><span class="sxs-lookup"><span data-stu-id="544e7-315">You need hello self-signed certificate tooconnect toohello cluster.</span></span> <span data-ttu-id="544e7-316">Hello utasításokat követve csatlakozhat toohello biztonságos fürt [ügyfél-hozzáférési tooa fürt hitelesítéséhez](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="544e7-316">You can connect toohello secure cluster by following hello instructions for [authenticating client access tooa cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

<span data-ttu-id="544e7-317">hello tanúsítvány tulajdonosának nevét meg kell egyeznie a hello tartomány tooaccess hello Service Fabric-fürt használata.</span><span class="sxs-lookup"><span data-stu-id="544e7-317">hello certificate's subject name must match hello domain that you use tooaccess hello Service Fabric cluster.</span></span> <span data-ttu-id="544e7-318">A megfelelés szükség tooprovide hello fürt HTTPS felügyeleti végpontok az SSL és a Service Fabric Explorerben talál.</span><span class="sxs-lookup"><span data-stu-id="544e7-318">This match is required tooprovide an SSL for hello cluster's HTTPS management endpoints and Service Fabric Explorer.</span></span> <span data-ttu-id="544e7-319">Nem szerezze be egy SSL-tanúsítványt egy hitelesítésszolgáltatóból hello `.cloudapp.azure.com` tartomány.</span><span class="sxs-lookup"><span data-stu-id="544e7-319">You cannot obtain an SSL certificate from a CA for hello `.cloudapp.azure.com` domain.</span></span> <span data-ttu-id="544e7-320">Be kell szereznie egy egyéni tartománynevet a fürt számára.</span><span class="sxs-lookup"><span data-stu-id="544e7-320">You must obtain a custom domain name for your cluster.</span></span> <span data-ttu-id="544e7-321">A hitelesítésszolgáltató tanúsítvány kérése, amikor hello tanúsítvány tulajdonosának nevét meg kell egyeznie a fürt használó hello egyéni tartománynevet.</span><span class="sxs-lookup"><span data-stu-id="544e7-321">When you request a certificate from a CA, hello certificate's subject name must match hello custom domain name that you use for your cluster.</span></span>

<span data-ttu-id="544e7-322">Hello paraméterek az Azure-portálon hello hello segítő parancsfájlból hello leírtak szerint töltheti [fürt létrehozása az Azure-portálon hello](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) szakasz.</span><span class="sxs-lookup"><span data-stu-id="544e7-322">You can fill hello parameters from hello helper script in hello Azure portal, as described in hello [Create a cluster in hello Azure portal](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal) section.</span></span>

## <a name="next-steps"></a><span data-ttu-id="544e7-323">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="544e7-323">Next steps</span></span>
<span data-ttu-id="544e7-324">Ezen a ponton rendelkezik olyan Azure Active Directory-felügyeleti hitelesítés biztonságos fürttel.</span><span class="sxs-lookup"><span data-stu-id="544e7-324">At this point, you have a secure cluster with Azure Active Directory providing management authentication.</span></span> <span data-ttu-id="544e7-325">Ezt követően [csatlakoztassa tooyour fürtöt](service-fabric-connect-to-secure-cluster.md) és megtudhatja, hogyan túl[alkalmazás titkos kulcsok kezelése](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="544e7-325">Next, [connect tooyour cluster](service-fabric-connect-to-secure-cluster.md) and learn how too[manage application secrets](service-fabric-application-secret-management.md).</span></span>

## <a name="troubleshoot-setting-up-azure-active-directory-for-client-authentication"></a><span data-ttu-id="544e7-326">Az ügyfél-hitelesítéshez Azure Active Directory beállításának hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="544e7-326">Troubleshoot setting up Azure Active Directory for client authentication</span></span>
<span data-ttu-id="544e7-327">Futtatása a problémát az ügyfél-hitelesítéshez az Azure AD beállítása közben, tekintse át a lehetséges megoldások hello ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="544e7-327">If you run into an issue while you're setting up Azure AD for client authentication, review hello potential solutions in this section.</span></span>

### <a name="service-fabric-explorer-prompts-you-tooselect-a-certificate"></a><span data-ttu-id="544e7-328">Service Fabric Explorer kér tanúsítvány tooselect</span><span class="sxs-lookup"><span data-stu-id="544e7-328">Service Fabric Explorer prompts you tooselect a certificate</span></span>
#### <a name="problem"></a><span data-ttu-id="544e7-329">Probléma</span><span class="sxs-lookup"><span data-stu-id="544e7-329">Problem</span></span>
<span data-ttu-id="544e7-330">Sikeresen tooAzure AD a Service Fabric Explorerben a bejelentkezést, hello böngésző toohello kezdőlap adja vissza, de az üzenet tooselect tanúsítványt kér.</span><span class="sxs-lookup"><span data-stu-id="544e7-330">After you sign in successfully tooAzure AD in Service Fabric Explorer, hello browser returns toohello home page but a message prompts you tooselect a certificate.</span></span>

![SFX válassza a tanúsítványt párbeszédpanel][sfx-select-certificate-dialog]

#### <a name="reason"></a><span data-ttu-id="544e7-332">Ok</span><span class="sxs-lookup"><span data-stu-id="544e7-332">Reason</span></span>
<span data-ttu-id="544e7-333">hello felhasználói szerepet hello Azure AD-fürt alkalmazást, nincs hozzárendelve.</span><span class="sxs-lookup"><span data-stu-id="544e7-333">hello user isn’t assigned a role in hello Azure AD cluster application.</span></span> <span data-ttu-id="544e7-334">Így az Azure AD-alapú hitelesítés nem sikerül, a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="544e7-334">Thus, Azure AD authentication fails on Service Fabric cluster.</span></span> <span data-ttu-id="544e7-335">Service Fabric Explorer visszavált toocertificate hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="544e7-335">Service Fabric Explorer falls back toocertificate authentication.</span></span>

#### <a name="solution"></a><span data-ttu-id="544e7-336">Megoldás</span><span class="sxs-lookup"><span data-stu-id="544e7-336">Solution</span></span>
<span data-ttu-id="544e7-337">Hello utasítások szerint állíthatja be az Azure AD és a felhasználói szerepkörök hozzárendelésére.</span><span class="sxs-lookup"><span data-stu-id="544e7-337">Follow hello instructions for setting up Azure AD, and assign user roles.</span></span> <span data-ttu-id="544e7-338">Emellett javasoljuk, hogy kapcsolja be a "Felhasználói hozzárendelés szükséges tooaccess alkalmazás," `SetupApplications.ps1` does.</span><span class="sxs-lookup"><span data-stu-id="544e7-338">Also, we recommend that you turn on “User assignment required tooaccess app,” as `SetupApplications.ps1` does.</span></span>

### <a name="connection-with-powershell-fails-with-an-error-hello-specified-credentials-are-invalid"></a><span data-ttu-id="544e7-339">PowerShell-kapcsolatot egy hibaüzenettel meghiúsul: "hello megadva hitelesítő adatok érvénytelenek."</span><span class="sxs-lookup"><span data-stu-id="544e7-339">Connection with PowerShell fails with an error: "hello specified credentials are invalid"</span></span>
#### <a name="problem"></a><span data-ttu-id="544e7-340">Probléma</span><span class="sxs-lookup"><span data-stu-id="544e7-340">Problem</span></span>
<span data-ttu-id="544e7-341">Sikertelen, ha PowerShell tooconnect toohello fürt "AzureActiveDirectory" biztonsági módban, a sikeresen tooAzure AD a bejelentkezést, hello kapcsolódási hiba miatt: "hello megadva hitelesítő adatok érvénytelenek."</span><span class="sxs-lookup"><span data-stu-id="544e7-341">When you use PowerShell tooconnect toohello cluster by using “AzureActiveDirectory” security mode, after you sign in successfully tooAzure AD, hello connection fails with an error: "hello specified credentials are invalid."</span></span>

#### <a name="solution"></a><span data-ttu-id="544e7-342">Megoldás</span><span class="sxs-lookup"><span data-stu-id="544e7-342">Solution</span></span>
<span data-ttu-id="544e7-343">Ez a megoldás azonos hello megelőző egy hello.</span><span class="sxs-lookup"><span data-stu-id="544e7-343">This solution is hello same as hello preceding one.</span></span>

### <a name="service-fabric-explorer-returns-a-failure-when-you-sign-in-aadsts50011"></a><span data-ttu-id="544e7-344">Service Fabric Explorer hibát ad vissza, amikor bejelentkezik: "AADSTS50011"</span><span class="sxs-lookup"><span data-stu-id="544e7-344">Service Fabric Explorer returns a failure when you sign in: "AADSTS50011"</span></span>
#### <a name="problem"></a><span data-ttu-id="544e7-345">Probléma</span><span class="sxs-lookup"><span data-stu-id="544e7-345">Problem</span></span>
<span data-ttu-id="544e7-346">TooAzure AD a Service Fabric Explorerben a toosign használatakor hello lap adja vissza a hiba: "AADSTS50011: hello válaszcímet &lt;URL-cím&gt; nem egyezik meg a hello válasz címek hello alkalmazáshoz konfigurált: &lt;guid&gt;."</span><span class="sxs-lookup"><span data-stu-id="544e7-346">When you try toosign in tooAzure AD in Service Fabric Explorer, hello page returns a failure: "AADSTS50011: hello reply address &lt;url&gt; does not match hello reply addresses configured for hello application: &lt;guid&gt;."</span></span>

![A válaszcím SFX nem egyezik.][sfx-reply-address-not-match]

#### <a name="reason"></a><span data-ttu-id="544e7-348">Ok</span><span class="sxs-lookup"><span data-stu-id="544e7-348">Reason</span></span>
<span data-ttu-id="544e7-349">hello (webalkalmazás) fürt Service Fabric Explorer jelölő kísérletet tesz az Azure AD elleni tooauthenticate, és hello kérelem részeként is biztosít hello átirányítási visszatérési URL-cím.</span><span class="sxs-lookup"><span data-stu-id="544e7-349">hello cluster (web) application that represents Service Fabric Explorer attempts tooauthenticate against Azure AD, and as part of hello request it provides hello redirect return URL.</span></span> <span data-ttu-id="544e7-350">De hello URL-címe nem szerepel a hello Azure AD alkalmazás **válasz URL-CÍMEN** listája.</span><span class="sxs-lookup"><span data-stu-id="544e7-350">But hello URL is not listed in hello Azure AD application **REPLY URL** list.</span></span>

#### <a name="solution"></a><span data-ttu-id="544e7-351">Megoldás</span><span class="sxs-lookup"><span data-stu-id="544e7-351">Solution</span></span>
<span data-ttu-id="544e7-352">A hello **konfigurálása** hello lapján cluster (webalkalmazás), adja hozzá az URL-címet a Service Fabric Explorer toohello hello **válasz URL-CÍMEN** listában, és cserélje le a hello elemek hello listában.</span><span class="sxs-lookup"><span data-stu-id="544e7-352">On hello **Configure** tab of hello cluster (web) application, add hello URL of Service Fabric Explorer toohello **REPLY URL** list or replace one of hello items in hello list.</span></span> <span data-ttu-id="544e7-353">Ha végzett, mentse a módosítást.</span><span class="sxs-lookup"><span data-stu-id="544e7-353">When you have finished, save your change.</span></span>

![Webes alkalmazás válasz URL-címe][web-application-reply-url]

### <a name="connect-hello-cluster-by-using-azure-ad-authentication-via-powershell"></a><span data-ttu-id="544e7-355">Hello fürt csatlakoztatása az Azure AD hitelesítési PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="544e7-355">Connect hello cluster by using Azure AD authentication via PowerShell</span></span>
<span data-ttu-id="544e7-356">tooconnect hello Service Fabric-fürt, a következő PowerShell-parancs például hello használata:</span><span class="sxs-lookup"><span data-stu-id="544e7-356">tooconnect hello Service Fabric cluster, use hello following PowerShell command example:</span></span>

```powershell
Connect-ServiceFabricCluster -ConnectionEndpoint <endpoint> -KeepAliveIntervalInSec 10 -AzureActiveDirectory -ServerCertThumbprint <thumbprint>
```

<span data-ttu-id="544e7-357">toolearn hello Connect-ServiceFabricCluster parancsmaggal kapcsolatban lásd: [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span><span class="sxs-lookup"><span data-stu-id="544e7-357">toolearn about hello Connect-ServiceFabricCluster cmdlet, see [Connect-ServiceFabricCluster](https://msdn.microsoft.com/library/mt125938.aspx).</span></span>

### <a name="can-i-reuse-hello-same-azure-ad-tenant-in-multiple-clusters"></a><span data-ttu-id="544e7-358">Felhasználhatja a több fürt hello ugyanazt az Azure AD-bérlő?</span><span class="sxs-lookup"><span data-stu-id="544e7-358">Can I reuse hello same Azure AD tenant in multiple clusters?</span></span>
<span data-ttu-id="544e7-359">Igen.</span><span class="sxs-lookup"><span data-stu-id="544e7-359">Yes.</span></span> <span data-ttu-id="544e7-360">Ne felejtse tooadd hello URL-címet a Service Fabric Explorer tooyour fürt (webalkalmazás) alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="544e7-360">But remember tooadd hello URL of Service Fabric Explorer tooyour cluster (web) application.</span></span> <span data-ttu-id="544e7-361">Service Fabric Explorer nem működik.</span><span class="sxs-lookup"><span data-stu-id="544e7-361">Otherwise, Service Fabric Explorer doesn’t work.</span></span>

### <a name="why-do-i-still-need-a-server-certificate-while-azure-ad-is-enabled"></a><span data-ttu-id="544e7-362">Miért továbbra is szükséges egy kiszolgálói tanúsítványt az Azure AD be van kapcsolva?</span><span class="sxs-lookup"><span data-stu-id="544e7-362">Why do I still need a server certificate while Azure AD is enabled?</span></span>
<span data-ttu-id="544e7-363">FabricClient és FabricGateway a kölcsönös hitelesítést végezni.</span><span class="sxs-lookup"><span data-stu-id="544e7-363">FabricClient and FabricGateway perform a mutual authentication.</span></span> <span data-ttu-id="544e7-364">Az Azure AD-hitelesítés során az Azure AD-integrációs ügyféloldali toohello identitáskiszolgálók kínál, és hello kiszolgálótanúsítvány használt tooverify hello kiszolgáló identitását.</span><span class="sxs-lookup"><span data-stu-id="544e7-364">During Azure AD authentication, Azure AD integration provides a client identity toohello server, and hello server certificate is used tooverify hello server identity.</span></span> <span data-ttu-id="544e7-365">A Service Fabric-tanúsítványokkal kapcsolatos további információkért lásd: [X.509-tanúsítványokat és a Service Fabric][x509-certificates-and-service-fabric].</span><span class="sxs-lookup"><span data-stu-id="544e7-365">For more information about Service Fabric certificates, see [X.509 certificates and Service Fabric][x509-certificates-and-service-fabric].</span></span>

<!-- Links -->
[azure-powershell]:https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[key-vault-get-started]:../key-vault/key-vault-get-started.md
[aad-graph-api-docs]:https://msdn.microsoft.com/library/azure/ad/graph/api/api-catalog
[azure-classic-portal]: https://manage.windowsazure.com
[service-fabric-rp-helpers]: https://github.com/ChackDan/Service-Fabric/tree/master/Scripts/ServiceFabricRPHelpers
[service-fabric-cluster-security]: service-fabric-cluster-security.md
[active-directory-howto-tenant]: ../active-directory/active-directory-howto-tenant.md
[service-fabric-visualizing-your-cluster]: service-fabric-visualizing-your-cluster.md
[service-fabric-manage-application-in-visual-studio]: service-fabric-manage-application-in-visual-studio.md
[sf-aad-ps-script-download]:http://servicefabricsdkstorage.blob.core.windows.net/publicrelease/MicrosoftAzureServiceFabric-AADHelpers.zip
[azure-quickstart-templates]: https://github.com/Azure/azure-quickstart-templates
[service-fabric-secure-cluster-5-node-1-nodetype]: https://github.com/Azure/azure-quickstart-templates/blob/master/service-fabric-secure-cluster-5-node-1-nodetype/
[resource-group-template-deploy]: https://azure.microsoft.com/documentation/articles/resource-group-template-deploy/
[x509-certificates-and-service-fabric]: service-fabric-cluster-security.md#x509-certificates-and-service-fabric

<!-- Images -->
[cluster-security-arm-dependency-map]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-arm-dependency-map.png
[cluster-security-cert-installation]: ./media/service-fabric-cluster-creation-via-arm/cluster-security-cert-installation.png
[assign-users-to-roles-button]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles-button.png
[assign-users-to-roles-dialog]: ./media/service-fabric-cluster-creation-via-arm/assign-users-to-roles.png
[sfx-select-certificate-dialog]: ./media/service-fabric-cluster-creation-via-arm/sfx-select-certificate-dialog.png
[sfx-reply-address-not-match]: ./media/service-fabric-cluster-creation-via-arm/sfx-reply-address-not-match.png
[web-application-reply-url]: ./media/service-fabric-cluster-creation-via-arm/web-application-reply-url.png

