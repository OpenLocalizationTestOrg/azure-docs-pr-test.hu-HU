---
title: "aaaService egyszerű Azure Kubernetes fürthöz |} Microsoft Docs"
description: "Kubernetes-fürthöz tartozó Azure Active Directory egyszerű szolgáltatás létrehozása és felügyelete az Azure Container Service-ben"
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/08/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 7a01624c5ac3fa717dbcbd570e05ceb4d917c53a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-ad-service-principal-for-a-kubernetes-cluster-in-container-service"></a><span data-ttu-id="c663b-103">Kubernetes-fürthöz tartozó Azure AD egyszerű szolgáltatás beállítása a Container Service-ben</span><span class="sxs-lookup"><span data-stu-id="c663b-103">Set up an Azure AD service principal for a Kubernetes cluster in Container Service</span></span>


<span data-ttu-id="c663b-104">Az Azure Tárolószolgáltatásban, Kubernetes fürt szükséges egy [Azure Active Directory szolgáltatás egyszerű](../../active-directory/develop/active-directory-application-objects.md) toointeract Azure API-khoz.</span><span class="sxs-lookup"><span data-stu-id="c663b-104">In Azure Container Service, a Kubernetes cluster requires an [Azure Active Directory service principal](../../active-directory/develop/active-directory-application-objects.md) toointeract with Azure APIs.</span></span> <span data-ttu-id="c663b-105">hello szolgáltatás egyszerű szükséges toodynamically kezelheti az erőforrásokat például [felhasználó által definiált útvonalak](../../virtual-network/virtual-networks-udr-overview.md) és hello [réteg 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span><span class="sxs-lookup"><span data-stu-id="c663b-105">hello service principal is needed toodynamically manage resources such as [user-defined routes](../../virtual-network/virtual-networks-udr-overview.md) and hello [Layer 4 Azure Load Balancer](../../load-balancer/load-balancer-overview.md).</span></span> 


<span data-ttu-id="c663b-106">Ez a cikk ismerteti a különböző beállítások tooset egy szolgáltatás egyszerű a Kubernetes fürt számára.</span><span class="sxs-lookup"><span data-stu-id="c663b-106">This article shows different options tooset up a service principal for your Kubernetes cluster.</span></span> <span data-ttu-id="c663b-107">Például, ha telepítve, és állítsa be hello [Azure CLI 2.0](/cli/azure/install-az-cli2), futtathatja hello [ `az acs create` ](/cli/azure/acs#create) parancs toocreate hello Kubernetes fürt és hello egyszerű szolgáltatásnév: hello ugyanannyi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c663b-107">For example, if you installed and set up hello [Azure CLI 2.0](/cli/azure/install-az-cli2), you can run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster and hello service principal at hello same time.</span></span>


## <a name="requirements-for-hello-service-principal"></a><span data-ttu-id="c663b-108">Hello szolgáltatás egyszerű követelményei</span><span class="sxs-lookup"><span data-stu-id="c663b-108">Requirements for hello service principal</span></span>

<span data-ttu-id="c663b-109">Használhat egy meglévő Azure AD szolgáltatás egyszerű, hogy megfelel-e hello követelményeknek, vagy hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="c663b-109">You can use an existing Azure AD service principal that meets hello following requirements, or create a new one.</span></span>

* <span data-ttu-id="c663b-110">**Hatókör**: hello erőforráscsoport hello előfizetésben toodeploy hello Kubernetes fürt, vagy (kevésbé korlátozottan) hello előfizetés fel toodeploy hello fürt.</span><span class="sxs-lookup"><span data-stu-id="c663b-110">**Scope**: hello resource group in hello subscription used toodeploy hello Kubernetes cluster, or (less restrictively) hello subscription used toodeploy hello cluster.</span></span>

* <span data-ttu-id="c663b-111">**Szerepkör**: **Közreműködő**</span><span class="sxs-lookup"><span data-stu-id="c663b-111">**Role**: **Contributor**</span></span>

* <span data-ttu-id="c663b-112">**Titkos ügyfélkulcs**: egy jelszónak kell lennie.</span><span class="sxs-lookup"><span data-stu-id="c663b-112">**Client secret**: must be a password.</span></span> <span data-ttu-id="c663b-113">Jelenleg nem használhat egyszerű szolgáltatás beállítást tanúsítvány hitelesítéshez.</span><span class="sxs-lookup"><span data-stu-id="c663b-113">Currently, you can't use a service principal set up for certificate authentication.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="c663b-114">egy egyszerű szolgáltatásnév toocreate, rendelkeznie kell engedélyek tooregister az alkalmazás az Azure AD-bérlő és a tooassign hello alkalmazás tooa szerepkör az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="c663b-114">toocreate a service principal, you must have permissions tooregister an application with your Azure AD tenant, and tooassign hello application tooa role in your subscription.</span></span> <span data-ttu-id="c663b-115">hello szükséges engedélyek, ha toosee [hello Portal beadása](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span><span class="sxs-lookup"><span data-stu-id="c663b-115">toosee if you have hello required permissions, [check in hello Portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md#required-permissions).</span></span> 
>

## <a name="option-1-create-a-service-principal-in-azure-ad"></a><span data-ttu-id="c663b-116">1. lehetőség: Egyszerű szolgáltatás létrehozása az Azure AD-ban</span><span class="sxs-lookup"><span data-stu-id="c663b-116">Option 1: Create a service principal in Azure AD</span></span>

<span data-ttu-id="c663b-117">Ha az Azure AD szolgáltatás egyszerű toocreate Kubernetes fürt központi telepítése előtt, az Azure számos módszert biztosít.</span><span class="sxs-lookup"><span data-stu-id="c663b-117">If you want toocreate an Azure AD service principal before you deploy your Kubernetes cluster, Azure provides several methods.</span></span> 

<span data-ttu-id="c663b-118">a következő Példaparancsok hello mutatja be toodo hello a [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span><span class="sxs-lookup"><span data-stu-id="c663b-118">hello following example commands show you how toodo this with hello [Azure CLI 2.0](../../azure-resource-manager/resource-group-authenticate-service-principal-cli.md).</span></span> <span data-ttu-id="c663b-119">Másik lehetőségként létrehozhat egy szolgáltatás egyszerű használatával [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="c663b-119">You can alternatively create a service principal using [Azure PowerShell](../../azure-resource-manager/resource-group-authenticate-service-principal.md), hello [portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md), or other methods.</span></span>

```azurecli
az login

az account set --subscription "mySubscriptionID"

az group create -n "myResourceGroupName" -l "westus"

az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/mySubscriptionID/resourceGroups/myResourceGroupName"
```

<span data-ttu-id="c663b-120">(Itt látható kivont) hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="c663b-120">Output is similar toohello following (shown here redacted):</span></span>

![Egyszerű szolgáltatás létrehozása](./media/container-service-kubernetes-service-principal/service-principal-creds.png)

<span data-ttu-id="c663b-122">A kijelölt vannak hello **ügyfél-azonosító** (`appId`) és hello **ügyfélkulcs** (`password`) az fürtöt tartalmazó környezetben használt service egyszerű paraméterekként.</span><span class="sxs-lookup"><span data-stu-id="c663b-122">Highlighted are hello **client ID** (`appId`) and hello **client secret** (`password`) that you use as service principal parameters for cluster deployment.</span></span>


### <a name="specify-service-principal-when-creating-hello-kubernetes-cluster"></a><span data-ttu-id="c663b-123">Adja meg a szolgáltatás egyszerű hello Kubernetes fürt létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="c663b-123">Specify service principal when creating hello Kubernetes cluster</span></span>

<span data-ttu-id="c663b-124">Adja meg a hello **ügyfél-azonosító** (más néven hello `appId`, az Alkalmazásazonosító) és **ügyfélkulcs** (`password`) egy meglévő egyszerű szolgáltatásnév paraméter hello létrehozásakor a Kubernetes fürt.</span><span class="sxs-lookup"><span data-stu-id="c663b-124">Provide hello **client ID** (also called hello `appId`, for Application ID) and **client secret** (`password`) of an existing service principal as parameters when you create hello Kubernetes cluster.</span></span> <span data-ttu-id="c663b-125">Ellenőrizze, hogy hello szolgáltatás egyszerű kezdve ez a cikk hello: hello követelményeknek megfelelő.</span><span class="sxs-lookup"><span data-stu-id="c663b-125">Make sure hello service principal meets hello requirements at hello beginning this article.</span></span>

<span data-ttu-id="c663b-126">Ezek a paraméterek hello Kubernetes fürtjéhez hello telepítésekor megadhatja [Azure parancssori felület (CLI) 2.0-s](container-service-kubernetes-walkthrough.md), [Azure-portálon](../dcos-swarm/container-service-deployment.md), vagy más módszerrel.</span><span class="sxs-lookup"><span data-stu-id="c663b-126">You can specify these parameters when deploying hello Kubernetes cluster using hello [Azure Command-Line Interface (CLI) 2.0](container-service-kubernetes-walkthrough.md), [Azure portal](../dcos-swarm/container-service-deployment.md), or other methods.</span></span>

>[!TIP] 
><span data-ttu-id="c663b-127">Hello megadásakor **ügyfél-azonosító**, lehet, hogy toouse hello `appId`, nem hello `ObjectId`, hello szolgáltatás rendszerbiztonsági tag.</span><span class="sxs-lookup"><span data-stu-id="c663b-127">When specifying hello **client ID**, be sure toouse hello `appId`, not hello `ObjectId`, of hello service principal.</span></span>
>

<span data-ttu-id="c663b-128">hello alábbi példában látható egyirányú toopass hello Azure CLI 2.0 hello paramétereket.</span><span class="sxs-lookup"><span data-stu-id="c663b-128">hello following example shows one way toopass hello parameters with hello Azure CLI 2.0.</span></span> <span data-ttu-id="c663b-129">Ez a példa hello [Kubernetes gyorsindítási sablonon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span><span class="sxs-lookup"><span data-stu-id="c663b-129">This example uses hello [Kubernetes quickstart template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-kubernetes).</span></span>

1. <span data-ttu-id="c663b-130">[Töltse le](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello sablon paraméterfájl `azuredeploy.parameters.json` a Githubról.</span><span class="sxs-lookup"><span data-stu-id="c663b-130">[Download](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.parameters.json) hello template parameters file `azuredeploy.parameters.json` from GitHub.</span></span>

2. <span data-ttu-id="c663b-131">toospecify hello szolgáltatás egyszerű, adja meg az értékeket `servicePrincipalClientId` és `servicePrincipalClientSecret` hello fájlban.</span><span class="sxs-lookup"><span data-stu-id="c663b-131">toospecify hello service principal, enter values for `servicePrincipalClientId` and `servicePrincipalClientSecret` in hello file.</span></span> <span data-ttu-id="c663b-132">(Szükség tooprovide a saját értékeit `dnsNamePrefix` és `sshRSAPublicKey`.</span><span class="sxs-lookup"><span data-stu-id="c663b-132">(You also need tooprovide your own values for `dnsNamePrefix` and `sshRSAPublicKey`.</span></span> <span data-ttu-id="c663b-133">Ez utóbbi hello hello SSH nyilvános kulcs tooaccess hello fürt.) Hello fájl mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="c663b-133">hello latter is hello SSH public key tooaccess hello cluster.) Save hello file.</span></span>

    ![Az egyszerű szolgáltatás paramétereinek továbbítása](./media/container-service-kubernetes-service-principal/service-principal-params.png)

3. <span data-ttu-id="c663b-135">A következő parancsot, használatával futtatási hello `--parameters` tooset hello elérési toohello azuredeploy.parameters.json fájlt.</span><span class="sxs-lookup"><span data-stu-id="c663b-135">Run hello following command, using `--parameters` tooset hello path toohello azuredeploy.parameters.json file.</span></span> <span data-ttu-id="c663b-136">Ez a parancs telepíti egy erőforráscsoportban hoz létre a hívott hello fürt `myResourceGroup` hello USA nyugati régiója régióban.</span><span class="sxs-lookup"><span data-stu-id="c663b-136">This command deploys hello cluster in a resource group you create called `myResourceGroup` in hello West US region.</span></span>

    ```azurecli
    az login

    az account set --subscription "mySubscriptionID"

    az group create --name "myResourceGroup" --location "westus" 
    
    az group deployment create -g "myResourceGroup" --template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-acs-kubernetes/azuredeploy.json" --parameters @azuredeploy.parameters.json
    ```


## <a name="option-2-generate-a-service-principal-when-creating-hello-cluster-with-az-acs-create"></a><span data-ttu-id="c663b-137">2. lehetőség: Egy egyszerű szolgáltatás létrehozása, ha hello fürt létrehozása`az acs create`</span><span class="sxs-lookup"><span data-stu-id="c663b-137">Option 2: Generate a service principal when creating hello cluster with `az acs create`</span></span>

<span data-ttu-id="c663b-138">Ha futtatja a hello [ `az acs create` ](/cli/azure/acs#create) parancs toocreate Kubernetes fürt hello telepítette, hello beállítás toogenerate szolgáltatásnevet automatikusan.</span><span class="sxs-lookup"><span data-stu-id="c663b-138">If you run hello [`az acs create`](/cli/azure/acs#create) command toocreate hello Kubernetes cluster, you have hello option toogenerate a service principal automatically.</span></span>

<span data-ttu-id="c663b-139">Hasonlóan mint más Kubernetes-fürt létrehozási lehetőségek esetében, az `az acs create` futtatásakor megadhatja egy meglévő egyszerű szolgáltatás paramétereit.</span><span class="sxs-lookup"><span data-stu-id="c663b-139">As with other Kubernetes cluster creation options, you can specify parameters for an existing service principal when you run `az acs create`.</span></span> <span data-ttu-id="c663b-140">Azonban ha kihagyja ezeket a paramétereket, hello Azure CLI hoz létre egy automatikusan használja a Tárolószolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="c663b-140">However, when you omit these parameters, hello Azure CLI creates one automatically for use with Container Service.</span></span> <span data-ttu-id="c663b-141">Ez akkor történik meg transzparens módon hello üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="c663b-141">This takes place transparently during hello deployment.</span></span> 

<span data-ttu-id="c663b-142">hello következő parancs egy Kubernetes fürtöt hoz létre, és SSH-kulcsok és a szolgáltatás egyszerű hitelesítő adatait:</span><span class="sxs-lookup"><span data-stu-id="c663b-142">hello following command creates a Kubernetes cluster and generates both SSH keys and service principal credentials:</span></span>

```console
az acs create -n myClusterName -d myDNSPrefix -g myResourceGroup --generate-ssh-keys --orchestrator-type kubernetes
```

> [!IMPORTANT]
> <span data-ttu-id="c663b-143">Ha a fiók nem rendelkezik hello Azure AD és az előfizetés engedélyek toocreate egy egyszerű szolgáltatást, hello parancs hibát generál hasonló túl`Insufficient privileges toocomplete hello operation.`</span><span class="sxs-lookup"><span data-stu-id="c663b-143">If your account doesn't have hello Azure AD and subscription permissions toocreate a service principal, hello command generates an error similar too`Insufficient privileges toocomplete hello operation.`</span></span>
> 

## <a name="additional-considerations"></a><span data-ttu-id="c663b-144">Néhány fontos megjegyzés</span><span class="sxs-lookup"><span data-stu-id="c663b-144">Additional considerations</span></span>

* <span data-ttu-id="c663b-145">Ha nincs engedélyek toocreate egyszerű szolgáltatás az előfizetéshez, szükség lehet a tooask az Azure AD vagy az előfizetés rendszergazdája tooassign hello szükséges engedélyekkel, vagy kérje az Azure Tárolószolgáltatás egy szolgáltatás egyszerű toouse.</span><span class="sxs-lookup"><span data-stu-id="c663b-145">If you don't have permissions toocreate a service principal in your subscription, you might need tooask your Azure AD or subscription administrator tooassign hello necessary permissions, or ask them for a service principal toouse with Azure Container Service.</span></span> 

* <span data-ttu-id="c663b-146">a Kubernetes egyszerű hello szolgáltatás hello fürtkonfiguráció része.</span><span class="sxs-lookup"><span data-stu-id="c663b-146">hello service principal for Kubernetes is a part of hello cluster configuration.</span></span> <span data-ttu-id="c663b-147">Azonban nem használható hello identitás toodeploy hello fürt.</span><span class="sxs-lookup"><span data-stu-id="c663b-147">However, don't use hello identity toodeploy hello cluster.</span></span>

* <span data-ttu-id="c663b-148">Minden egyszerű szolgáltatás társítva van egy Azure AD-alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="c663b-148">Every service principal is associated with an Azure AD application.</span></span> <span data-ttu-id="c663b-149">hello szolgáltatás egyszerű Kubernetes fürt társítható bármilyen érvényes Azure AD-alkalmazás neve (például: `https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="c663b-149">hello service principal for a Kubernetes cluster can be associated with any valid Azure AD application name (for example: `https://www.contoso.org/example`).</span></span> <span data-ttu-id="c663b-150">hello alkalmazás URL-címe hello valós végpont nem rendelkezik a toobe.</span><span class="sxs-lookup"><span data-stu-id="c663b-150">hello URL for hello application doesn't have toobe a real endpoint.</span></span>

* <span data-ttu-id="c663b-151">Ha megadó hello szolgáltatás egyszerű **ügyfél-azonosító**, használhatja a hello hello értékének `appId` (amint ez a cikk) vagy hello tartozó egyszerű szolgáltatásnév `name` (például`https://www.contoso.org/example`).</span><span class="sxs-lookup"><span data-stu-id="c663b-151">When specifying hello service principal **Client ID**, you can use hello value of hello `appId` (as shown in this article) or hello corresponding service principal `name` (for example,`https://www.contoso.org/example`).</span></span>

* <span data-ttu-id="c663b-152">Hello master és a virtuális gépek ügynök hello Kubernetes fürt hello fájl /etc/kubernetes/azure.json hello szolgáltatás egyszerű hitelesítő adatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="c663b-152">On hello master and agent VMs in hello Kubernetes cluster, hello service principal credentials are stored in hello file /etc/kubernetes/azure.json.</span></span>

* <span data-ttu-id="c663b-153">Amikor az hello `az acs create` toogenerate hello szolgáltatás egyszerű parancs automatikusan, hello szolgáltatás egyszerű hitelesítő adatait írt toohello fájl ~/.azure/acsServicePrincipal.json hello gépen használt toorun hello parancsot.</span><span class="sxs-lookup"><span data-stu-id="c663b-153">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal credentials are written toohello file ~/.azure/acsServicePrincipal.json on hello machine used toorun hello command.</span></span> 

* <span data-ttu-id="c663b-154">Hello használata esetén `az acs create` toogenerate hello szolgáltatás egyszerű parancs automatikusan, egyszerű hello szolgáltatást is képes hitelesíteni az egy [Azure tároló beállításjegyzék](../../container-registry/container-registry-intro.md) hello létrehozott azonos előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="c663b-154">When you use hello `az acs create` command toogenerate hello service principal automatically, hello service principal can also authenticate with an [Azure container registry](../../container-registry/container-registry-intro.md) created in hello same subscription.</span></span>




## <a name="next-steps"></a><span data-ttu-id="c663b-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c663b-155">Next steps</span></span>

* <span data-ttu-id="c663b-156">[Ismerkedés a Kubernetes-szel](container-service-kubernetes-walkthrough.md) a tárolószolgáltatás-fürtben.</span><span class="sxs-lookup"><span data-stu-id="c663b-156">[Get started with Kubernetes](container-service-kubernetes-walkthrough.md) in your container service cluster.</span></span>

* <span data-ttu-id="c663b-157">tootroubleshoot hello szolgáltatás egyszerű Kubernetes, lásd: hello [ACS motor dokumentáció](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="c663b-157">tootroubleshoot hello service principal for Kubernetes, see hello [ACS Engine documentation](https://github.com/Azure/acs-engine/blob/master/docs/kubernetes.md#troubleshooting).</span></span>


