---
title: "az Azure Tárolószolgáltatás és Azure tároló beállításjegyzék vázlat aaaUse |} Microsoft Docs"
description: "Az ACS Kubernetes fürt és az Azure-tároló beállításjegyzék toocreate az első alkalmazás létrehozása az Azure-ban Vázlat."
services: container-service
documentationcenter: 
author: squillace
manager: gamonroy
editor: 
tags: draft, helm, acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, Draft, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/31/2017
ms.author: rasquill
ms.custom: mvc
ms.openlocfilehash: f5e21cda01e5e8452bf86a5c8fa458904d89f451
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-draft-with-azure-container-service-and-azure-container-registry-toobuild-and-deploy-an-application-tookubernetes"></a><span data-ttu-id="147b1-104">Azure Tárolószolgáltatás és Azure tároló beállításjegyzék toobuild vázlat használjon, és egy alkalmazás tooKubernetes telepítése</span><span class="sxs-lookup"><span data-stu-id="147b1-104">Use Draft with Azure Container Service and Azure Container Registry toobuild and deploy an application tooKubernetes</span></span>

<span data-ttu-id="147b1-105">[Vázlat](https://aka.ms/draft) egy új nyílt forráskódú eszköz, így könnyen toodevelop tároló-alapú alkalmazások és azok tooKubernetes fürtök nélkül nagy Docker és Kubernetes--ismerete, vagy még akkor is telepíteni kell őket.</span><span class="sxs-lookup"><span data-stu-id="147b1-105">[Draft](https://aka.ms/draft) is a new open-source tool that makes it easy toodevelop container-based applications and deploy them tooKubernetes clusters without knowing much about Docker and Kubernetes -- or even installing them.</span></span> <span data-ttu-id="147b1-106">Vázlat hasonló eszközökkel legyen, és a csoportok fókusz Kubernetes hello alkalmazás felépítése, nem a legtöbb figyelmet tooinfrastructure fizet.</span><span class="sxs-lookup"><span data-stu-id="147b1-106">Using tools like Draft let you and your teams focus on building hello application with Kubernetes, not paying as much attention tooinfrastructure.</span></span>

<span data-ttu-id="147b1-107">A Draft bármely Docker-rendszerképjegyzékkel és Kubernetes-fürttel használható, beleértve a helyi használatot.</span><span class="sxs-lookup"><span data-stu-id="147b1-107">You can use Draft with any Docker image registry and any Kubernetes cluster, including locally.</span></span> <span data-ttu-id="147b1-108">Ez az oktatóanyag bemutatja, hogyan toouse ACS Kubernetes ACR és Azure DNS toocreate az élő CI/CD fejlesztők csővezeték-vázlat használata.</span><span class="sxs-lookup"><span data-stu-id="147b1-108">This tutorial shows how toouse ACS with Kubernetes, ACR, and Azure DNS toocreate a live CI/CD developer pipeline using Draft.</span></span>


## <a name="create-an-azure-container-registry"></a><span data-ttu-id="147b1-109">Azure Container Registry létrehozása</span><span class="sxs-lookup"><span data-stu-id="147b1-109">Create an Azure Container Registry</span></span>
<span data-ttu-id="147b1-110">Egyszerűen [hozzon létre egy új Azure-tároló beállításjegyzék](../../container-registry/container-registry-get-started-azure-cli.md), de hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="147b1-110">You can easily [create a new Azure Container Registry](../../container-registry/container-registry-get-started-azure-cli.md), but hello steps are as follows:</span></span>

1. <span data-ttu-id="147b1-111">Hozzon létre egy Azure-erőforrás csoport toomanage a ACR beállításjegyzék és hello Kubernetes fürtöt az ACS szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="147b1-111">Create a Azure resource group toomanage your ACR registry and hello Kubernetes cluster in ACS.</span></span>
      ```azurecli
      az group create --name draft --location eastus
      ```

2. <span data-ttu-id="147b1-112">Hozzon létre egy ACR-rendszerképjegyzéket az [az acr create](/cli/azure/acr#create) használatával</span><span class="sxs-lookup"><span data-stu-id="147b1-112">Create an ACR image registry using [az acr create](/cli/azure/acr#create)</span></span>
      ```azurecli
      az acr create -g draft -n draftacs --sku Basic --admin-enabled true -l eastus
      ```


## <a name="create-an-azure-container-service-with-kubernetes"></a><span data-ttu-id="147b1-113">Azure Container Service létrehozása a Kubernetes-szel</span><span class="sxs-lookup"><span data-stu-id="147b1-113">Create an Azure Container Service with Kubernetes</span></span>

<span data-ttu-id="147b1-114">Most már készen áll a toouse Ön [az acs létre](/cli/azure/acs#create) Kubernetes használata hello toocreate az ACS-fürt `--orchestrator-type` érték.</span><span class="sxs-lookup"><span data-stu-id="147b1-114">Now you're ready toouse [az acs create](/cli/azure/acs#create) toocreate an ACS cluster using Kubernetes as hello `--orchestrator-type` value.</span></span>
```azurecli
az acs create --resource-group draft --name draft-kube-acs --dns-prefix draft-cluster --orchestrator-type kubernetes
```

> [!NOTE]
> <span data-ttu-id="147b1-115">Kubernetes nincs hello alapértelmezett orchestrator típus, mert kell használnia, hello `--orchestrator-type kubernetes` váltani.</span><span class="sxs-lookup"><span data-stu-id="147b1-115">Because Kubernetes is not hello default orchestrator type, be sure you use hello `--orchestrator-type kubernetes` switch.</span></span>

<span data-ttu-id="147b1-116">hello kimeneti, ha sikeres a következőhöz hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="147b1-116">hello output when successful looks similar toohello following.</span></span>

```json
waiting for AAD role toopropagate.done
{
  "id": "/subscriptions/<guid>/resourceGroups/draft/providers/Microsoft.Resources/deployments/azurecli14904.93snip09",
  "name": "azurecli1496227204.9323909",
  "properties": {
    "correlationId": "<guid>",
    "debugSetting": null,
    "dependencies": [],
    "mode": "Incremental",
    "outputs": null,
    "parameters": {
      "clientSecret": {
        "type": "SecureString"
      }
    },
    "parametersLink": null,
    "providers": [
      {
        "id": null,
        "namespace": "Microsoft.ContainerService",
        "registrationState": null,
        "resourceTypes": [
          {
            "aliases": null,
            "apiVersions": null,
            "locations": [
              "westus"
            ],
            "properties": null,
            "resourceType": "containerServices"
          }
        ]
      }
    ],
    "provisioningState": "Succeeded",
    "template": null,
    "templateLink": null,
    "timestamp": "2017-05-31T10:46:29.434095+00:00"
  },
  "resourceGroup": "draft"
}
```

<span data-ttu-id="147b1-117">Most, hogy a fürt, hello hitelesítő adatokat importálhat hello segítségével [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot.</span><span class="sxs-lookup"><span data-stu-id="147b1-117">Now that you have a cluster, you can import hello credentials by using hello [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) command.</span></span> <span data-ttu-id="147b1-118">Most már rendelkezik a helyi konfigurációs fájlt a fürt, amely milyen Helm és vázlathoz kell tooget a munkájukat.</span><span class="sxs-lookup"><span data-stu-id="147b1-118">Now you have a local configuration file for your cluster, which is what Helm and Draft need tooget their work done.</span></span>

## <a name="install-and-configure-draft"></a><span data-ttu-id="147b1-119">A draft telepítése és konfigurálása</span><span class="sxs-lookup"><span data-stu-id="147b1-119">Install and configure draft</span></span>
<span data-ttu-id="147b1-120">hello telepítési utasításait vázlat szerepelnek hello [vázlat tárház](https://github.com/Azure/draft/blob/master/docs/install.md).</span><span class="sxs-lookup"><span data-stu-id="147b1-120">hello installation instructions for Draft are in hello [Draft repository](https://github.com/Azure/draft/blob/master/docs/install.md).</span></span> <span data-ttu-id="147b1-121">Viszonylag könnyen, de egyéb konfigurációra van szükség, attól függ, mint [Helm](https://aka.ms/helm) toocreate és központi telepítése egy Helm diagram hello Kubernetes fürtbe.</span><span class="sxs-lookup"><span data-stu-id="147b1-121">They are relatively simple, but do require some configuration, as it depends on [Helm](https://aka.ms/helm) toocreate and deploy a Helm chart into hello Kubernetes cluster.</span></span>

1. <span data-ttu-id="147b1-122">[A Helm letöltése és telepítése](https://aka.ms/helm#install).</span><span class="sxs-lookup"><span data-stu-id="147b1-122">[Download and install Helm](https://aka.ms/helm#install).</span></span>
2. <span data-ttu-id="147b1-123">A Helm toosearch használ, és telepíti `stable/traefik`, és a bejövő kérelmek a buildek érkező vezérlő tooenable.</span><span class="sxs-lookup"><span data-stu-id="147b1-123">Use Helm toosearch for and install `stable/traefik`, and ingress controller tooenable inbound requests for your builds.</span></span>
    ```bash
    $ helm search traefik
    NAME            VERSION DESCRIPTION
    stable/traefik  1.3.0   A Traefik based Kubernetes ingress controller w...

    $ helm install stable/traefik --name ingress
    ```
    <span data-ttu-id="147b1-124">A figyelési most be hello `ingress` vezérlő toocapture hello külső IP érték, ha telepítve van.</span><span class="sxs-lookup"><span data-stu-id="147b1-124">Now set a watch on hello `ingress` controller toocapture hello external IP value when it is deployed.</span></span> <span data-ttu-id="147b1-125">Az IP-cím lesz egy hello [tooyour telepítési tartományhoz csatlakoztatott](#wire-up-deployment-domain) hello a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="147b1-125">This IP address will be hello one [mapped tooyour deployment domain](#wire-up-deployment-domain) in hello next section.</span></span>

    ```bash
    kubectl get svc -w
    NAME                          CLUSTER-IP     EXTERNAL-IP     PORT(S)                      AGE
    ingress-traefik               10.0.248.104   13.64.108.240   80:31046/TCP,443:32556/TCP   1h
    kubernetes                    10.0.0.1       <none>          443/TCP                      7h
    ```

    <span data-ttu-id="147b1-126">Ebben az esetben a hello telepítési tartomány: hello külső IP `13.64.108.240`.</span><span class="sxs-lookup"><span data-stu-id="147b1-126">In this case, hello external IP for hello deployment domain is `13.64.108.240`.</span></span> <span data-ttu-id="147b1-127">Most már a toothat IP-Címének is leképezheti.</span><span class="sxs-lookup"><span data-stu-id="147b1-127">Now you can map your domain toothat IP.</span></span>

## <a name="wire-up-deployment-domain"></a><span data-ttu-id="147b1-128">Az üzembe helyezési tartomány beállítása</span><span class="sxs-lookup"><span data-stu-id="147b1-128">Wire up deployment domain</span></span>

<span data-ttu-id="147b1-129">A Draft kiadást hoz létre minden egyes létrehozott Helm-diagramhoz – minden egyes alkalmazáshoz, amin dolgozik.</span><span class="sxs-lookup"><span data-stu-id="147b1-129">Draft creates a release for each Helm chart it creates -- each application you are working on.</span></span> <span data-ttu-id="147b1-130">Mindegyik lekérdezi egy létrehozott nevet, mint a Vázlat által használt egy _altartomány_ hello legfelső szintű funkciók mellett kártevőészlelést _telepítési tartomány_ , amelyek.</span><span class="sxs-lookup"><span data-stu-id="147b1-130">Each one gets a generated name that is used by draft as a _subdomain_ on top of hello root _deployment domain_ that you control.</span></span> <span data-ttu-id="147b1-131">(A jelen példában használjuk `squillace.io` hello telepítési tartomány.) tooenable altartomány a jelenség kell létrehoznia A rekordjainak `'*'` a központi telepítés tartomány, a DNS-bejegyzéseket, hogy minden egyes létrehozott altartomány irányított toohello Kubernetes fürt érkező vezérlő.</span><span class="sxs-lookup"><span data-stu-id="147b1-131">(In this example, we use `squillace.io` as hello deployment domain.) tooenable this subdomain behavior, you must create an A record for `'*'` in your DNS entries for your deployment domain, so that each generated subdomain is routed toohello Kubernetes cluster's ingress controller.</span></span>

<span data-ttu-id="147b1-132">Saját tartomány szolgáltatónak van-e a saját módon tooassign DNS-kiszolgálók; túl[delegálása a tartományi nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), hello lépések végrehajtása:</span><span class="sxs-lookup"><span data-stu-id="147b1-132">Your own domain provider has their own way tooassign DNS servers; too[delegate your domain nameservers tooAzure DNS](../../dns/dns-delegate-domain-azure-dns.md), you take hello following steps:</span></span>

1. <span data-ttu-id="147b1-133">Hozzon létre egy erőforráscsoportot a zóna számára.</span><span class="sxs-lookup"><span data-stu-id="147b1-133">Create a resource group for your zone.</span></span>
    ```azurecli
    az group create --name squillace.io --location eastus
    {
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io",
      "location": "eastus",
      "managedBy": null,
      "name": "zones",
      "properties": {
        "provisioningState": "Succeeded"
      },
      "tags": null
    }
    ```

2. <span data-ttu-id="147b1-134">Hozzon létre DNS-zónát a tartományához.</span><span class="sxs-lookup"><span data-stu-id="147b1-134">Create a DNS zone for your domain.</span></span>
<span data-ttu-id="147b1-135">Használjon hello [az hálózati DNS-zóna létrehozása](/cli/azure/network/dns/zone#create) tooobtain hello nameservers toodelegate DNS szabályozhatja a tartomány DNS tooAzure parancsot.</span><span class="sxs-lookup"><span data-stu-id="147b1-135">Use hello [az network dns zone create](/cli/azure/network/dns/zone#create) command tooobtain hello nameservers toodelegate DNS control tooAzure DNS for your domain.</span></span>
    ```azurecli
    az network dns zone create --resource-group squillace.io --name squillace.io
    {
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/zones/providers/Microsoft.Network/dnszones/squillace.io",
      "location": "global",
      "maxNumberOfRecordSets": 5000,
      "name": "squillace.io",
      "nameServers": [
        "ns1-09.azure-dns.com.",
        "ns2-09.azure-dns.net.",
        "ns3-09.azure-dns.org.",
        "ns4-09.azure-dns.info."
      ],
      "numberOfRecordSets": 2,
      "resourceGroup": "squillace.io",
      "tags": {},
      "type": "Microsoft.Network/dnszones"
    }
    ```
3. <span data-ttu-id="147b1-136">Adja hozzá a hello DNS-kiszolgálók toohello tartomány szolgáltató lehetősége van a központi telepítés tartományba, amely lehetővé teszi, hogy Ön toouse Azure DNS toorepoint a tartomány ahányat csak szeretne.</span><span class="sxs-lookup"><span data-stu-id="147b1-136">Add hello DNS servers you are given toohello domain provider for your deployment domain, which enables you toouse Azure DNS toorepoint your domain as you want.</span></span>
4. <span data-ttu-id="147b1-137">A központi telepítés tartomány leképezése toohello A rekordhalmaz bejegyzés létrehozása `ingress` IP hello előző szakaszban 2. lépés.</span><span class="sxs-lookup"><span data-stu-id="147b1-137">Create an A record-set entry for your deployment domain mapping toohello `ingress` IP from step 2 of hello previous section.</span></span>
    ```azurecli
    az network dns record-set a add-record --ipv4-address 13.64.108.240 --record-set-name '*' -g squillace.io -z squillace.io
    ```
<span data-ttu-id="147b1-138">hello kimeneti alábbihoz hasonló:</span><span class="sxs-lookup"><span data-stu-id="147b1-138">hello output looks something like:</span></span>
    ```json
    {
      "arecords": [
        {
          "ipv4Address": "13.64.108.240"
        }
      ],
      "etag": "<guid>",
      "id": "/subscriptions/<guid>/resourceGroups/squillace.io/providers/Microsoft.Network/dnszones/squillace.io/A/*",
      "metadata": null,
      "name": "*",
      "resourceGroup": "squillace.io",
      "ttl": 3600,
      "type": "Microsoft.Network/dnszones/A"
    }
    ```

5. <span data-ttu-id="147b1-139">Konfigurálja a beállításjegyzéket az Vázlat toouse és altartományok minden Helm diagram hoz létre.</span><span class="sxs-lookup"><span data-stu-id="147b1-139">Configure Draft toouse your registry and create subdomains for each Helm chart it creates.</span></span> <span data-ttu-id="147b1-140">tooconfigure vázlat, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="147b1-140">tooconfigure Draft, you need:</span></span>
  - <span data-ttu-id="147b1-141">Az Azure Container Registry neve (a példában `draft`)</span><span class="sxs-lookup"><span data-stu-id="147b1-141">your Azure Container Registry name (in this example, `draft`)</span></span>
  - <span data-ttu-id="147b1-142">A beállításkulcs vagy jelszó a következőből: `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span><span class="sxs-lookup"><span data-stu-id="147b1-142">your registry key, or password, from `az acr credential show -n <registry name> --output tsv --query "passwords[0].value"`.</span></span>
  - <span data-ttu-id="147b1-143">hello telepítési gyökértartomány, hogy konfigurálta-e az toomap toohello Kubernetes érkező külső IP-cím (Itt `squillace.io`)</span><span class="sxs-lookup"><span data-stu-id="147b1-143">hello root deployment domain that you have configured toomap toohello Kubernetes ingress external IP address (here, `squillace.io`)</span></span>

  <span data-ttu-id="147b1-144">Hívás `draft init` és hello konfigurációs folyamat felajánlja a fenti hello értékek.</span><span class="sxs-lookup"><span data-stu-id="147b1-144">Call `draft init` and hello configuration process prompts you for hello values above.</span></span> <span data-ttu-id="147b1-145">hello folyamat tűnik valami hello következő hello első alkalommal futtatja azt.</span><span class="sxs-lookup"><span data-stu-id="147b1-145">hello process looks something like hello following hello first time you run it.</span></span>
 ```bash
    $ draft init
    Creating pack ruby...
    Creating pack node...
    Creating pack gradle...
    Creating pack maven...
    Creating pack php...
    Creating pack python...
    Creating pack dotnetcore...
    Creating pack golang...
    $DRAFT_HOME has been configured at /Users/ralphsquillace/.draft.

    In order tooinstall Draft, we need a bit more information...

    1. Enter your Docker registry URL (e.g. docker.io, quay.io, myregistry.azurecr.io): draft.azurecr.io
    2. Enter your username: draft
    3. Enter your password:
    4. Enter your org where Draft will push images [draft]: draft
    5. Enter your top-level domain for ingress (e.g. draft.example.com): squillace.io
    Draft has been installed into your Kubernetes Cluster.
    Happy Sailing!
    ```

<span data-ttu-id="147b1-146">Most már készen áll a toodeploy egy alkalmazás most.</span><span class="sxs-lookup"><span data-stu-id="147b1-146">Now you're ready toodeploy an application.</span></span>


## <a name="build-and-deploy-an-application"></a><span data-ttu-id="147b1-147">Alkalmazás fejlesztése és üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="147b1-147">Build and deploy an application</span></span>

<span data-ttu-id="147b1-148">Hello vázlat tárházban vannak [hat egyszerű példa alkalmazások](https://github.com/Azure/draft/tree/master/examples).</span><span class="sxs-lookup"><span data-stu-id="147b1-148">In hello Draft repo are [six simple example applications](https://github.com/Azure/draft/tree/master/examples).</span></span> <span data-ttu-id="147b1-149">Hello tárház klónozása, és most használja az hello [Python példa](https://github.com/Azure/draft/tree/master/examples/python).</span><span class="sxs-lookup"><span data-stu-id="147b1-149">Clone hello repo and let's use hello [Python example](https://github.com/Azure/draft/tree/master/examples/python).</span></span> <span data-ttu-id="147b1-150">Váltson át hello példák/Python könyvtár, és írja be `draft create` toobuild hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="147b1-150">Change into hello examples/Python directory, and type `draft create` toobuild hello application.</span></span> <span data-ttu-id="147b1-151">A következő példa hello kell hasonlítania.</span><span class="sxs-lookup"><span data-stu-id="147b1-151">It should look like hello following example.</span></span>
```bash
$ draft create
--> Python app detected
--> Ready toosail
```

<span data-ttu-id="147b1-152">hello kimenete egy Dockerfile és Helm diagram.</span><span class="sxs-lookup"><span data-stu-id="147b1-152">hello output includes a Dockerfile and a Helm chart.</span></span> <span data-ttu-id="147b1-153">toobuild és telepíteni, csak gépelje `draft up`.</span><span class="sxs-lookup"><span data-stu-id="147b1-153">toobuild and deploy, you just type `draft up`.</span></span> <span data-ttu-id="147b1-154">hello kimeneti túl hosszú, de a következő példa hello például kezdődik.</span><span class="sxs-lookup"><span data-stu-id="147b1-154">hello output is extensive, but begins like hello following example.</span></span>
```bash
$ draft up
--> Building Dockerfile
Step 1 : FROM python:onbuild
onbuild: Pulling from library/python
10a267c67f42: Pulling fs layer
fb5937da9414: Pulling fs layer
9021b2326a1e: Pulling fs layer
dbed9b09434e: Pulling fs layer
ea8a37f15161: Pulling fs layer
<snip>
```

<span data-ttu-id="147b1-155">mikor és a következő példa valami hasonló toohello sikeres multiplicitású.</span><span class="sxs-lookup"><span data-stu-id="147b1-155">and when successful ends with something similar toohello following example.</span></span>
```bash
ab68189731eb: Pushed
53c0ab0341bee12d01be3d3c192fbd63562af7f1: digest: sha256:bb0450ec37acf67ed461c1512ef21f58a500ff9326ce3ec623ce1e4427df9765 size: 2841
--> Deploying tooKubernetes
--> Status: DEPLOYED
--> Notes:

  http://gangly-bronco.squillace.io tooaccess your application

Watching local files for changes...
```

<span data-ttu-id="147b1-156">Függetlenül a diagram neve van, akkor most `curl http://gangly-bronco.squillace.io` tooreceive hello válasz, `Hello World!`.</span><span class="sxs-lookup"><span data-stu-id="147b1-156">Whatever your chart's name is, you can now `curl http://gangly-bronco.squillace.io` tooreceive hello reply, `Hello World!`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="147b1-157">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="147b1-157">Next steps</span></span>

<span data-ttu-id="147b1-158">Most, hogy az ACS Kubernetes fürt, segítségével kivizsgálhatja [Azure tároló beállításjegyzék](../../container-registry/container-registry-intro.md) toocreate további és különböző példányainak ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="147b1-158">Now that you have an ACS Kubernetes cluster, you can investigate using [Azure Container Registry](../../container-registry/container-registry-intro.md) toocreate more and different deployments of this scenario.</span></span> <span data-ttu-id="147b1-159">Például létrehozhat egy draft._basedomain.toplevel_ tartományi DNS-rekordkészletet, amely mélyebb altartományból szabályozza a dolgokat adott ACS-környezetekhez.</span><span class="sxs-lookup"><span data-stu-id="147b1-159">For example, you can create a draft._basedomain.toplevel_ domain DNS record-set that controls things off of a deeper subdomain for specific ACS deployments.</span></span>






