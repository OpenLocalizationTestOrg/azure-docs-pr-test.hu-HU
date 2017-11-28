---
title: "aaaDeploy OpenShift származási tooAzure |} Microsoft Docs"
description: "Ismerje meg, hogy toodeploy OpenShift származási tooAzure virtuális gépek."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: jbinder
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: jbinder
ms.openlocfilehash: a67450c46da41134a5f6c669a9e54e14773ac5b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-openshift-origin-tooazure-virtual-machines"></a><span data-ttu-id="d425c-103">OpenShift származási tooAzure virtuális gépek telepítése</span><span class="sxs-lookup"><span data-stu-id="d425c-103">Deploy OpenShift Origin tooAzure Virtual Machines</span></span> 

<span data-ttu-id="d425c-104">[OpenShift származási](https://www.openshift.org/) egy nyílt forráskódú tároló platform épülő [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="d425c-104">[OpenShift Origin](https://www.openshift.org/) is an open source container platform built on [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="d425c-105">Egyszerűbbé teszi a központi telepítését, a méretezés és a több-bérlős alkalmazásokhoz működő hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="d425c-105">It simplifies hello process of deploying, scaling, and operating multi-tenant applications.</span></span> 

<span data-ttu-id="d425c-106">Ez az útmutató ismerteti, hogyan toodeploy OpenShift forrása az Azure virtuális gépek használatával hello Azure CLI és az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="d425c-106">This guide describes how toodeploy OpenShift Origin on Azure Virtual Machines using hello Azure CLI and Azure Resource Manager Templates.</span></span> <span data-ttu-id="d425c-107">Ezen oktatóanyag segítségével megtanulhatja a következőket:</span><span class="sxs-lookup"><span data-stu-id="d425c-107">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d425c-108">A KeyVault toomanage hello OpenShift fürthöz SSH-kulcsok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d425c-108">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="d425c-109">OpenShift-fürt üzembe helyezése Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="d425c-109">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="d425c-110">Telepítse és konfigurálja a hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello fürt.</span><span class="sxs-lookup"><span data-stu-id="d425c-110">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>
> * <span data-ttu-id="d425c-111">Hello OpenShift telepítés testreszabásához.</span><span class="sxs-lookup"><span data-stu-id="d425c-111">Customize hello OpenShift deployment.</span></span>

<span data-ttu-id="d425c-112">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="d425c-112">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

<span data-ttu-id="d425c-113">A gyors üzembe helyezési szükséges hello Azure CLI 2.0.8 verzió vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d425c-113">This quick start requires hello Azure CLI version 2.0.8 or later.</span></span> <span data-ttu-id="d425c-114">toofind hello verzió, futtassa `az --version`.</span><span class="sxs-lookup"><span data-stu-id="d425c-114">toofind hello version, run `az --version`.</span></span> <span data-ttu-id="d425c-115">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d425c-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="log-in-tooazure"></a><span data-ttu-id="d425c-116">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="d425c-116">Log in tooAzure</span></span> 
<span data-ttu-id="d425c-117">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat, vagy kattintson **kipróbálás** toouse felhő rendszerhéj.</span><span class="sxs-lookup"><span data-stu-id="d425c-117">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions or click **Try it** toouse Cloud Shell.</span></span>

```azurecli 
az login
```
## <a name="create-a-resource-group"></a><span data-ttu-id="d425c-118">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="d425c-118">Create a resource group</span></span>

<span data-ttu-id="d425c-119">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d425c-119">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="d425c-120">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="d425c-120">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="d425c-121">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="d425c-121">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-key-vault"></a><span data-ttu-id="d425c-122">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="d425c-122">Create a Key Vault</span></span>
<span data-ttu-id="d425c-123">Hozzon létre egy KeyVault toostore hello hello fürthöz SSH-kulcsok hello [az keyvault létrehozása](/cli/azure/keyvault#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d425c-123">Create a KeyVault toostore hello SSH keys for hello cluster with hello [az keyvault create](/cli/azure/keyvault#create) command.</span></span>  

```azurecli 
az keyvault create --resource-group myResourceGroup --name myKeyVault \
       --enabled-for-template-deployment true \
       --location eastus
```

## <a name="create-an-ssh-key"></a><span data-ttu-id="d425c-124">SSH-kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="d425c-124">Create an SSH key</span></span> 
<span data-ttu-id="d425c-125">SSH-kulcs szükséges toosecure hozzáférés toohello OpenShift forrás fürt.</span><span class="sxs-lookup"><span data-stu-id="d425c-125">An SSH key is needed toosecure access toohello OpenShift Origin cluster.</span></span> <span data-ttu-id="d425c-126">Hozzon létre egy SSH-kulcspár hello segítségével `ssh-keygen` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d425c-126">Create an SSH key-pair using hello `ssh-keygen` command.</span></span> 
 
 ```bash
ssh-keygen -f ~/.ssh/openshift_rsa -t rsa -N ''
```

> [!NOTE]
> <span data-ttu-id="d425c-127">hello hoz létre SSH-kulcspárral nem lehet egy hozzáférési kódot.</span><span class="sxs-lookup"><span data-stu-id="d425c-127">hello SSH key pair you create must not have a passphrase.</span></span>

<span data-ttu-id="d425c-128">További információk a Windows rendszeren az SSH-kulcsok [hogyan toocreate SSH-kulcsok Windows](/azure/virtual-machines/linux/ssh-from-windows).</span><span class="sxs-lookup"><span data-stu-id="d425c-128">For more information on SSH keys on Windows, [How toocreate SSH keys on Windows](/azure/virtual-machines/linux/ssh-from-windows).</span></span>

## <a name="store-ssh-private-key-in-key-vault"></a><span data-ttu-id="d425c-129">Key Vault titkos SSH-kulcs tárolása</span><span class="sxs-lookup"><span data-stu-id="d425c-129">Store SSH private key in Key Vault</span></span>
<span data-ttu-id="d425c-130">hello OpenShift telepítési toohello OpenShift fő létrehozott toosecure hozzáférés hello SSH-kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="d425c-130">hello OpenShift deployment uses hello SSH key you created toosecure access toohello OpenShift master.</span></span> <span data-ttu-id="d425c-131">tooenable hello telepítési toosecurely hello SSH-kulcs beolvasása, hello kulcs tárolása a Key Vault hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="d425c-131">tooenable hello deployment toosecurely retrieve hello SSH key, store hello key in Key Vault using hello following command.</span></span>

# <a name="enabled-for-template-deployment"></a><span data-ttu-id="d425c-132">Sablon-üzembehelyezés engedélyezve</span><span class="sxs-lookup"><span data-stu-id="d425c-132">Enabled for template deployment</span></span>
```azurecli
az keyvault secret set --vault-name KeyVaultName --name OpenShiftKey --file ~/.ssh/openshift.rsa
```

## <a name="create-a-service-principal"></a><span data-ttu-id="d425c-133">Egyszerű szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="d425c-133">Create a service principal</span></span> 
<span data-ttu-id="d425c-134">OpenShift kommunikál az Azure-ban, a felhasználónév és jelszó vagy egy egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d425c-134">OpenShift communicates with Azure using a username and password or a service principal.</span></span> <span data-ttu-id="d425c-135">Egy Azure szolgáltatás egyszerű egy biztonsági azonosító, amely alkalmazások, szolgáltatások és automatizálási eszközökkel, például a OpenShift használható.</span><span class="sxs-lookup"><span data-stu-id="d425c-135">An Azure service principal is a security identity that you can use with apps, services, and automation tools like OpenShift.</span></span> <span data-ttu-id="d425c-136">Szabályozza, és adja meg hello engedélyek, mivel toowhat műveletek hello szolgáltatás egyszerű hajthat végre az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="d425c-136">You control and define hello permissions as toowhat operations hello service principal can perform in Azure.</span></span> <span data-ttu-id="d425c-137">tooimprove biztonsági keresztül csak a felhasználónév és jelszó megadása, ez a példa létrehoz egy alapszintű service egyszerű.</span><span class="sxs-lookup"><span data-stu-id="d425c-137">tooimprove security over just providing a username and password, this example creates a basic service principal.</span></span>

<span data-ttu-id="d425c-138">Az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac) és kimeneti hello hitelesítő adatait, amelyet a OpenShift:</span><span class="sxs-lookup"><span data-stu-id="d425c-138">Create a service principal with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac) and output hello credentials that OpenShift needs:</span></span>

```azurecli
az ad sp create-for-rbac --name openshiftsp \
          --role Contributor --password {strong password} \
          --scopes $(az group show --name myResourceGroup --query id)
```
<span data-ttu-id="d425c-139">Jegyezze fel a hello parancs által visszaadott hello appId tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="d425c-139">Take note of hello appId property returned from hello command.</span></span>
```json
{
  "appId": "a487e0c1-82af-47d9-9a0b-af184eb87646d",
  "displayName": "openshiftsp",
  "name": "http://openshiftsp",
  "password": {strong password},
  "tenant": "XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
}
```
 > [!WARNING] 
 > <span data-ttu-id="d425c-140">Ne hozzon létre nem biztonságos jelszót.</span><span class="sxs-lookup"><span data-stu-id="d425c-140">Don't create an insecure password.</span></span>  <span data-ttu-id="d425c-141">Kövesse az alábbi cikkben ismertetett útmutatást: [Az Azure AD-jelszavakra vonatkozó szabályok és korlátozások](/azure/active-directory/active-directory-passwords-policy).</span><span class="sxs-lookup"><span data-stu-id="d425c-141">Follow the [Azure AD password rules and restrictions](/azure/active-directory/active-directory-passwords-policy) guidance.</span></span>

<span data-ttu-id="d425c-142">A szolgáltatás rendszerbiztonsági tagok további információkért lásd: [hozzon létre egy Azure szolgáltatás egyszerű Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span><span class="sxs-lookup"><span data-stu-id="d425c-142">For more information on service principals, see [Create an Azure service principal with Azure CLI 2.0](/cli/azure/create-an-azure-service-principal-azure-cli)</span></span>

## <a name="deploy-hello-openshift-origin-template"></a><span data-ttu-id="d425c-143">Hello OpenShift eredeti sablon telepítése</span><span class="sxs-lookup"><span data-stu-id="d425c-143">Deploy hello OpenShift Origin template</span></span>
<span data-ttu-id="d425c-144">Ezután telepítése Azure Resource Manager-sablonnal OpenShift forrása.</span><span class="sxs-lookup"><span data-stu-id="d425c-144">Next deploy OpenShift Origin using an Azure Resource Manager template.</span></span> 

> [!NOTE] 
> <span data-ttu-id="d425c-145">hello következő parancshoz szükséges az CLI 2.0.8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="d425c-145">hello following command requires az CLI 2.0.8 or later.</span></span> <span data-ttu-id="d425c-146">Ellenőrizheti az CLI hello hello verziójával `az --version` parancsot.</span><span class="sxs-lookup"><span data-stu-id="d425c-146">You can verify hello az CLI version with hello `az --version` command.</span></span> <span data-ttu-id="d425c-147">tooupdate hello CLI verzió, lásd: [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d425c-147">tooupdate hello CLI version, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="d425c-148">Használjon hello `appId` hello szolgáltatás egyszerű hello korábban létrehozott értéket `aadClientId` paraméter.</span><span class="sxs-lookup"><span data-stu-id="d425c-148">Use hello `appId` value from hello service principal you created earlier for hello `aadClientId` parameter.</span></span>

```azurecli 
az group deployment create --name myOpenShiftCluster \
      --template-uri https://raw.githubusercontent.com/Microsoft/openshift-origin/master/azuredeploy.json \
      --params \ 
        openshiftMasterPublicIpDnsLabel=myopenshiftmaster \
        infraLbPublicIpDnsLabel=myopenshiftlb \
        openshiftPassword=Pass@word!
        sshPublicKey=~/.ssh/openshift_rsa.pub \
        keyVaultResourceGroup=myResourceGroup \
        keyVaultName=myKeyVault \
        keyVaultSecret=OpenShiftKey \
        aadClientId={appId} \
        aadClientSecret={strong password} 
```
<span data-ttu-id="d425c-149">hello telepítési too20 perc toocomplete is tarthat.</span><span class="sxs-lookup"><span data-stu-id="d425c-149">hello deployment may take up too20 minutes toocomplete.</span></span> <span data-ttu-id="d425c-150">hello hello OpenShift konzol URL-CÍMÉT és DNS-neve hello főkiszolgáló OpenShift nyomtatott toohello Terminálszolgáltatások hello telepítés befejezéséről.</span><span class="sxs-lookup"><span data-stu-id="d425c-150">hello URL of hello OpenShift console and DNS name of hello OpenShift master is printed toohello terminal when hello deployment completes.</span></span>

```json
{
  "OpenShift Console Uri": "http://openshiftlb.cloudapp.azure.com:8443/console",
  "OpenShift Master SSH": "ocpadmin@myopenshiftmaster.cloudapp.azure.com"
}
```
## <a name="connect-toohello-openshift-cluster"></a><span data-ttu-id="d425c-151">Csatlakoztassa toohello OpenShift fürtöt</span><span class="sxs-lookup"><span data-stu-id="d425c-151">Connect toohello OpenShift cluster</span></span>
<span data-ttu-id="d425c-152">Hello központi telepítés befejezése után csatlakoztassa toohello OpenShift konzolt hello segítségével hello böngészővel `OpenShift Console Uri`.</span><span class="sxs-lookup"><span data-stu-id="d425c-152">When hello deployment completes, connect toohello OpenShift console using hello browser using hello `OpenShift Console Uri`.</span></span> <span data-ttu-id="d425c-153">Másik lehetőségként toohello OpenShift fő hello a következő parancs használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="d425c-153">Alternatively, you can connect toohello OpenShift master using hello following command.</span></span>

```bash
$ ssh ocpadmin@myopenshiftmaster.cloudapp.azure.com
```

## <a name="clean-up-resources"></a><span data-ttu-id="d425c-154">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="d425c-154">Clean up resources</span></span>
<span data-ttu-id="d425c-155">Ha már nincs szükség, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, OpenShift fürt és az összes kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="d425c-155">When no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, OpenShift cluster, and all related resources.</span></span>

```azurecli 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="d425c-156">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d425c-156">Next steps</span></span>

<span data-ttu-id="d425c-157">Az ezen oktatóanyag, megismert hogyan:</span><span class="sxs-lookup"><span data-stu-id="d425c-157">In this tutorial, learned how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d425c-158">A KeyVault toomanage hello OpenShift fürthöz SSH-kulcsok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d425c-158">Create a KeyVault toomanage SSH keys for hello OpenShift cluster.</span></span>
> * <span data-ttu-id="d425c-159">OpenShift-fürt üzembe helyezése Azure virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="d425c-159">Deploy an OpenShift cluster on Azure VMs.</span></span> 
> * <span data-ttu-id="d425c-160">Telepítse és konfigurálja a hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello fürt.</span><span class="sxs-lookup"><span data-stu-id="d425c-160">Install and configure hello [OpenShift CLI](https://docs.openshift.org/latest/cli_reference/index.html#cli-reference-index) toomanage hello cluster.</span></span>

<span data-ttu-id="d425c-161">Most, hogy OpenShift forrás fürt telepítése.</span><span class="sxs-lookup"><span data-stu-id="d425c-161">Now that OpenShift Origin cluster is deployed.</span></span> <span data-ttu-id="d425c-162">Hogyan követheti OpenShift oktatóanyagok toolearn toodeploy az első és használati hello OpenShift eszközök.</span><span class="sxs-lookup"><span data-stu-id="d425c-162">You can follow OpenShift tutorials toolearn how toodeploy your first application and use hello OpenShift tools.</span></span> <span data-ttu-id="d425c-163">Lásd: [OpenShift származási első lépések](https://docs.openshift.org/latest/getting_started/index.html) tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="d425c-163">See [Getting Started with OpenShift Origin](https://docs.openshift.org/latest/getting_started/index.html) tooget started.</span></span> 
