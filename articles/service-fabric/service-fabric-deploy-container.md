---
title: "aaaService háló és a tárolók üzembe helyezése |} Microsoft Docs"
description: "A Service Fabric és hello tárolók toodeploy mikroszolgáltatási alkalmazások használatát. Ez a cikk ismerteti, amely a Service Fabric-tárolókban, és hogyan toodeploy egy Windows-tároló kép fürtbe hello képességek."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 799cc9ad-32fd-486e-a6b6-efff6b13622d
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 5/16/2017
ms.author: msfussell
ms.openlocfilehash: 8b6540579641474f21b8712b56049c7d177bec26
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-windows-container-tooservice-fabric"></a><span data-ttu-id="6d47a-104">A Windows-tároló tooService háló telepítése</span><span class="sxs-lookup"><span data-stu-id="6d47a-104">Deploy a Windows container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6d47a-105">Windows-tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="6d47a-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="6d47a-106">Docker-tároló telepítése</span><span class="sxs-lookup"><span data-stu-id="6d47a-106">Deploy Docker Container</span></span>](service-fabric-deploy-container-linux.md)
> 
> 

<span data-ttu-id="6d47a-107">Ez a cikk bemutatja, hogyan indexelése szolgáltatások a Windows-tárolókban lévő hello folyamatán.</span><span class="sxs-lookup"><span data-stu-id="6d47a-107">This article walks you through hello process of building containerized services in Windows containers.</span></span>

<span data-ttu-id="6d47a-108">A Service Fabric különböző képességeket, amelyek segítenek a tárolók belül futó mikroszolgáltatások épülnek alkalmazások építéséhez rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6d47a-108">Service Fabric has several capabilities that help you with building applications that are composed of microservices running inside containers.</span></span> 

<span data-ttu-id="6d47a-109">hello képességei a következők:</span><span class="sxs-lookup"><span data-stu-id="6d47a-109">hello capabilities include:</span></span>

* <span data-ttu-id="6d47a-110">Tároló lemezkép-telepítés és az aktiválás</span><span class="sxs-lookup"><span data-stu-id="6d47a-110">Container image deployment and activation</span></span>
* <span data-ttu-id="6d47a-111">Erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="6d47a-111">Resource governance</span></span>
* <span data-ttu-id="6d47a-112">Tárház-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="6d47a-112">Repository authentication</span></span>
* <span data-ttu-id="6d47a-113">Tároló port-állomás porthozzárendelés</span><span class="sxs-lookup"><span data-stu-id="6d47a-113">Container port-to-host port mapping</span></span>
* <span data-ttu-id="6d47a-114">Tároló-tároló felderítése és kommunikáció</span><span class="sxs-lookup"><span data-stu-id="6d47a-114">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="6d47a-115">Képes tooconfigure és környezeti változók megadása</span><span class="sxs-lookup"><span data-stu-id="6d47a-115">Ability tooconfigure and set environment variables</span></span>

<span data-ttu-id="6d47a-116">Egyes funkciók működése során egy indexelése szolgáltatás toobe bele az alkalmazásba most csomagolására vizsgáljuk meg.</span><span class="sxs-lookup"><span data-stu-id="6d47a-116">Let's look at how each of capabilities works when you're packaging a containerized service toobe included in your application.</span></span>

## <a name="package-a-windows-container"></a><span data-ttu-id="6d47a-117">A csomag egy Windows-tároló</span><span class="sxs-lookup"><span data-stu-id="6d47a-117">Package a Windows container</span></span>
<span data-ttu-id="6d47a-118">Ha csomag tárolója, választhat toouse vagy a Visual Studio-projektsablont vagy [hozzon létre manuálisan hello alkalmazáscsomag](#manually).</span><span class="sxs-lookup"><span data-stu-id="6d47a-118">When you package a container, you can choose toouse either a Visual Studio project template or [create hello application package manually](#manually).</span></span>  <span data-ttu-id="6d47a-119">Visual Studio, alkalmazás-csomag szerkezete hello használata, miután fájlok, a rendszer létrehozza hello új projektsablon által.</span><span class="sxs-lookup"><span data-stu-id="6d47a-119">When you use Visual Studio, hello application package structure and manifest files are created by hello New Project template for you.</span></span>

> [!TIP]
> <span data-ttu-id="6d47a-120">hello legegyszerűbb módja toopackage szolgáltatás meglévő tárolót a képet a Visual Studio toouse.</span><span class="sxs-lookup"><span data-stu-id="6d47a-120">hello easiest way toopackage an existing container image into a service is toouse Visual Studio.</span></span>

## <a name="use-visual-studio-toopackage-an-existing-container-image"></a><span data-ttu-id="6d47a-121">Visual Studio toopackage meglévő tároló lemezkép használata</span><span class="sxs-lookup"><span data-stu-id="6d47a-121">Use Visual Studio toopackage an existing container image</span></span>
<span data-ttu-id="6d47a-122">A Visual Studio biztosít a Service Fabric szolgáltatás sablon toohelp tároló tooa Service Fabric-fürt központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="6d47a-122">Visual Studio provides a Service Fabric service template toohelp you deploy a container tooa Service Fabric cluster.</span></span>

1. <span data-ttu-id="6d47a-123">Válasszon **fájl** > **új projekt**, és a Service Fabric-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6d47a-123">Choose **File** > **New Project**, and create a Service Fabric application.</span></span>
2. <span data-ttu-id="6d47a-124">Válasszon **Vendég tároló** hello szolgáltatássablonként.</span><span class="sxs-lookup"><span data-stu-id="6d47a-124">Choose **Guest Container** as hello service template.</span></span>
3. <span data-ttu-id="6d47a-125">Válasszon **Lemezképnév** , és adja meg a tároló-tárház hello elérési toohello lemezképet.</span><span class="sxs-lookup"><span data-stu-id="6d47a-125">Choose **Image Name** and provide hello path toohello image in your container repository.</span></span> <span data-ttu-id="6d47a-126">Például `myrepo/myimage:v1` a https://hub.docker.com</span><span class="sxs-lookup"><span data-stu-id="6d47a-126">For example, `myrepo/myimage:v1` in https://hub.docker.com</span></span>
4. <span data-ttu-id="6d47a-127">Nevezze el a szolgáltatást, és kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6d47a-127">Give your service a name, and click **OK**.</span></span>
5. <span data-ttu-id="6d47a-128">Ha a tárolóalapú szolgáltatásnak kell a végpont-kommunikációhoz, hozzáadhat hello protokoll, port és típus toohello ServiceManifest.xml fájlt.</span><span class="sxs-lookup"><span data-stu-id="6d47a-128">If your containerized service needs an endpoint for communication, you can now add hello protocol, port, and type toohello ServiceManifest.xml file.</span></span> <span data-ttu-id="6d47a-129">Példa:</span><span class="sxs-lookup"><span data-stu-id="6d47a-129">For example:</span></span> 
     
    `<Endpoint Name="MyContainerServiceEndpoint" Protocol="http" Port="80" UriScheme="http" PathSuffix="myapp/" Type="Input" />`
    
    <span data-ttu-id="6d47a-130">Hello megadásával `UriScheme`, a Service Fabric automatikusan regisztrálja hello tároló végpont hello felderíthetőség a Naming service.</span><span class="sxs-lookup"><span data-stu-id="6d47a-130">By providing hello `UriScheme`, Service Fabric automatically registers hello container endpoint with hello Naming service for discoverability.</span></span> <span data-ttu-id="6d47a-131">hello port kell (a fenti példa hello) rögzített vagy dinamikusan lefoglalt.</span><span class="sxs-lookup"><span data-stu-id="6d47a-131">hello port can either be fixed (as shown in hello preceding example) or dynamically allocated.</span></span> <span data-ttu-id="6d47a-132">Ha nem adja meg a portot, azt dinamikusan lefoglalt az alkalmazás porttartományát hello (ahogyan bármely szolgáltatással).</span><span class="sxs-lookup"><span data-stu-id="6d47a-132">If you don't specify a port, it is dynamically allocated from hello application port range (as would happen with any service).</span></span>
    <span data-ttu-id="6d47a-133">Szükség tooconfigure hello tároló toohost porthozzárendelés megadásával egy `PortBinding` hello alkalmazásjegyzékben házirend.</span><span class="sxs-lookup"><span data-stu-id="6d47a-133">You also need tooconfigure hello container toohost port mapping by specifying a `PortBinding` policy in hello application manifest.</span></span> <span data-ttu-id="6d47a-134">További információkért lásd: [tároló toohost porthozzárendelés konfigurálása](#Portsection).</span><span class="sxs-lookup"><span data-stu-id="6d47a-134">For more information, see [Configure container toohost port mapping](#Portsection).</span></span>
6. <span data-ttu-id="6d47a-135">Ha a tároló erőforrás irányítás kell, majd adja hozzá a `ResourceGovernancePolicy`.</span><span class="sxs-lookup"><span data-stu-id="6d47a-135">If your container needs resource governance then add a `ResourceGovernancePolicy`.</span></span>
8. <span data-ttu-id="6d47a-136">Ha a tároló tooauthenticate a személyes tára, majd adja hozzá `RepositoryCredentials`.</span><span class="sxs-lookup"><span data-stu-id="6d47a-136">If your container needs tooauthenticate with a private repository, then add `RepositoryCredentials`.</span></span>
7. <span data-ttu-id="6d47a-137">Futtatja egy Windows Server 2016-gépnek tároló támogatás engedélyezett, ha hello csomagot használja, és művelet toodeploy tooyour helyi fürt közzététele.</span><span class="sxs-lookup"><span data-stu-id="6d47a-137">If you are running on a Windows Server 2016 machine with container support enabled, you can use hello package and publish action toodeploy tooyour local cluster.</span></span> 
8. <span data-ttu-id="6d47a-138">Ha készen áll, hello alkalmazás tooa távoli fürt közzététele, vagy ellenőrizze hello megoldás toosource vezérlőben.</span><span class="sxs-lookup"><span data-stu-id="6d47a-138">When ready, you can publish hello application tooa remote cluster or check in hello solution toosource control.</span></span> 

<span data-ttu-id="6d47a-139">Például egy kivételt hello [Service Fabric tároló mintakódjainak megtekintése a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="6d47a-139">For an example, checkout hello [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="creating-a-windows-server-2016-cluster"></a><span data-ttu-id="6d47a-140">A Windows Server 2016-os-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="6d47a-140">Creating a Windows Server 2016 cluster</span></span>
<span data-ttu-id="6d47a-141">toodeploy toocreate tároló-támogatással rendelkező Windows Server 2016 rendszerű fürtre engedélyezve van szüksége a tárolóalapú alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6d47a-141">toodeploy your containerized application, you need toocreate a cluster running Windows Server 2016 with container support enabled.</span></span> <span data-ttu-id="6d47a-142">A fürt helyben fut, vagy telepítés keresztül Azure Resource Manager az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="6d47a-142">Your cluster may be running locally, or deployed via Azure Resource Manager in Azure.</span></span> 

<span data-ttu-id="6d47a-143">Azure Resource Manager, a fürt toodeploy válasszon hello **Windows Server 2016 tárolók** rendszerképet az Azure-ban a beállítás.</span><span class="sxs-lookup"><span data-stu-id="6d47a-143">toodeploy a cluster using Azure Resource Manager, choose hello **Windows Server 2016 with Containers** image option in Azure.</span></span> <span data-ttu-id="6d47a-144">Hello cikke [a Service Fabric-fürt létrehozása az Azure Resource Manager használatával](service-fabric-cluster-creation-via-arm.md).</span><span class="sxs-lookup"><span data-stu-id="6d47a-144">See hello article [Create a Service Fabric cluster by using Azure Resource Manager](service-fabric-cluster-creation-via-arm.md).</span></span> <span data-ttu-id="6d47a-145">Győződjön meg arról, hogy hello Azure Resource Manager-beállítások a következő:</span><span class="sxs-lookup"><span data-stu-id="6d47a-145">Ensure that you use hello following Azure Resource Manager settings:</span></span>

```xml
"vmImageOffer": { "type": "string","defaultValue": "WindowsServer"     },
"vmImageSku": { "defaultValue": "2016-Datacenter-with-Containers","type": "string"     },
"vmImageVersion": { "defaultValue": "latest","type": "string"     },  
```
<span data-ttu-id="6d47a-146">Is használhatja a hello [öt csomópont Azure Resource Manager sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate egy fürt.</span><span class="sxs-lookup"><span data-stu-id="6d47a-146">You can also use hello [Five Node Azure Resource Manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/service-fabric-secure-cluster-5-node-1-nodetype) toocreate a cluster.</span></span> <span data-ttu-id="6d47a-147">Másik lehetőségként olvassa el a közösségi [blogbejegyzés](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) használatával a Service Fabric és a Windows-tárolókon.</span><span class="sxs-lookup"><span data-stu-id="6d47a-147">Alternatively read a community [blog post](https://loekd.blogspot.com/2017/01/running-windows-containers-on-azure.html) on using Service Fabric and Windows containers.</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="6d47a-148">Manuálisan csomag és a tároló-lemezkép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6d47a-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="6d47a-149">hello folyamat manuálisan csomagolási indexelése szolgáltatás lépések hello alapul:</span><span class="sxs-lookup"><span data-stu-id="6d47a-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="6d47a-150">Hello tárolók tooyour tárház közzététele.</span><span class="sxs-lookup"><span data-stu-id="6d47a-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="6d47a-151">Hello csomag könyvtárstruktúrát létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6d47a-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="6d47a-152">Hello service manifest-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="6d47a-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="6d47a-153">Hello Alkalmazásjegyzék-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="6d47a-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="6d47a-154">Központi telepítése és a tároló lemezkép aktiválása</span><span class="sxs-lookup"><span data-stu-id="6d47a-154">Deploy and activate a container image</span></span>
<span data-ttu-id="6d47a-155">A Service Fabric hello [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6d47a-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="6d47a-156">toodeploy, és aktiválja a tároló, hello tároló lemezkép a put hello nevét egy `ContainerHost` elem hello szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6d47a-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="6d47a-157">Hello szolgáltatás jegyzékben, vegye fel a `ContainerHost` hello belépési ponthoz.</span><span class="sxs-lookup"><span data-stu-id="6d47a-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="6d47a-158">Majd a készlet hello `ImageName` hello tároló tárház és lemezkép toobe hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6d47a-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="6d47a-159">hello következő részleges jegyzékfájl szemlélteti, hogyan toodeploy hello tároló nevű `myimage:v1` a tárházból nevű `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="6d47a-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

```xml
    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimage:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>
```

<span data-ttu-id="6d47a-160">Megadhatja a választható parancsok toorun hello tárolóhoz tartozó hello elindításakor `Commands` elemet.</span><span class="sxs-lookup"><span data-stu-id="6d47a-160">You can specify optional commands toorun upon starting hello container under hello `Commands` element.</span></span> <span data-ttu-id="6d47a-161">A több parancsok vesszővel-korlátozza őket.</span><span class="sxs-lookup"><span data-stu-id="6d47a-161">For multiple commands, comma-delimit them.</span></span> 

## <a name="understand-resource-governance"></a><span data-ttu-id="6d47a-162">Erőforrás-irányítás ismertetése</span><span class="sxs-lookup"><span data-stu-id="6d47a-162">Understand resource governance</span></span>
<span data-ttu-id="6d47a-163">Erőforrás-irányítás, mert egy olyan képességet, amely korlátozza a tároló hello hello erőforrások hello tároló hello gazdagépen használhatják.</span><span class="sxs-lookup"><span data-stu-id="6d47a-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="6d47a-164">Hello `ResourceGovernancePolicy`, amely hello alkalmazásjegyzékben megadott használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot.</span><span class="sxs-lookup"><span data-stu-id="6d47a-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="6d47a-165">Erőforrás-korlátozások állíthat be a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="6d47a-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="6d47a-166">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="6d47a-166">Memory</span></span>
* <span data-ttu-id="6d47a-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="6d47a-167">MemorySwap</span></span>
* <span data-ttu-id="6d47a-168">CpuShares (CPU relatív súly)</span><span class="sxs-lookup"><span data-stu-id="6d47a-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="6d47a-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="6d47a-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="6d47a-170">BlkioWeight (BlockIO relatív súly).</span><span class="sxs-lookup"><span data-stu-id="6d47a-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="6d47a-171">Támogatja a megadott blokk IO korlátok például iops-érték megadására, olvasási/írási bit/másodperc, megint mások tervbe van véve egy későbbi kiadásban.</span><span class="sxs-lookup"><span data-stu-id="6d47a-171">Support for specifying specific block IO limits such as IOPs, read/write BPS, and others are planned for a future release.</span></span>
> 
> 

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500"
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>
```

## <a name="authenticate-a-repository"></a><span data-ttu-id="6d47a-172">A tárház hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="6d47a-172">Authenticate a repository</span></span>
<span data-ttu-id="6d47a-173">egy tároló toodownload, lehetséges, hogy tooprovide bejelentkezési hitelesítő adatok toohello tároló tárházba.</span><span class="sxs-lookup"><span data-stu-id="6d47a-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="6d47a-174">hello bejelentkezési, hello alkalmazásjegyzékben, a megadott hitelesítő adatok használt toospecify hello bejelentkezési adatait, vagy SSH-kulcsban, a hello tároló lemezkép letöltése hello lemezképtárból.</span><span class="sxs-lookup"><span data-stu-id="6d47a-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="6d47a-175">hello alábbi példa bemutatja nevű *tesztfelhasználó néven* hello a jelszó nyílt szövegként együtt (*nem* ajánlott):</span><span class="sxs-lookup"><span data-stu-id="6d47a-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

<span data-ttu-id="6d47a-176">Azt javasoljuk, hogy titkosítsa a hello jelszó toohello gépen központilag telepített tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="6d47a-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="6d47a-177">hello alábbi példa bemutatja nevű *tesztfelhasználó néven*, ahol hello jelszó nevű tanúsítvánnyal titkosított *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="6d47a-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="6d47a-178">Használhatja a hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello titkos titkosítási parancsszövege hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="6d47a-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="6d47a-179">További információkért lásd: hello cikk [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="6d47a-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="6d47a-180">hello az tanúsítvány titkos kulcsa hello toodecrypt hello jelszó által használt helyi számítógép telepített toohello sávon kívüli metódusban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6d47a-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="6d47a-181">(Az Azure-ban Ez a módszer az Azure Resource Manager.) Majd amikor a Service Fabric hello szolgáltatás csomag toohello számítógépe telepíti, azt vissza tudja fejteni hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6d47a-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="6d47a-182">Hello titkos hello fióknév együtt használva, majd hitelesítheti a hello tároló tárház.</span><span class="sxs-lookup"><span data-stu-id="6d47a-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <span data-ttu-id="6d47a-183"><a name ="Portsection"></a>Tároló toohost porthozzárendelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d47a-183"><a name ="Portsection"></a> Configure container toohost port mapping</span></span>
<span data-ttu-id="6d47a-184">Konfigurálhatja a gazdagépen használt port toocommunicate hello tárolóval megadásával egy `PortBinding` hello alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6d47a-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="6d47a-185">hello port kötés maps hello port toowhich hello szolgáltatást figyel tooa tárolóportot hello hello gazdagépen belül.</span><span class="sxs-lookup"><span data-stu-id="6d47a-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="6d47a-186">Tároló-tároló felderítése és a kommunikáció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d47a-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="6d47a-187">Használhatja a hello `PortBinding` elem toomap a tárolóportot tooan végpont hello szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6d47a-187">You can use hello `PortBinding` element toomap a container port tooan endpoint in hello service manifest.</span></span> <span data-ttu-id="6d47a-188">A következő példa hello, hello végpont `Endpoint1` 8905 egy rögzített portot határozza meg.</span><span class="sxs-lookup"><span data-stu-id="6d47a-188">In hello following example, hello endpoint `Endpoint1` specifies a fixed port, 8905.</span></span> <span data-ttu-id="6d47a-189">Azt is megadhatja, nincs port minden, ebben az esetben az alkalmazás porttartományát hello fürt véletlenszerű port meg van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6d47a-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>


```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>
```
<span data-ttu-id="6d47a-190">Ha megad egy végpontot, használatával hello `Endpoint` címkét hello szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="6d47a-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="6d47a-191">Hello fürtben futó egyéb szolgáltatások így felderíthetők az ebben a tárolóban hello REST lekérdezésekkel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="6d47a-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

<span data-ttu-id="6d47a-192">A Naming service hello regisztrálásával hajthat végre tároló-tároló kommunikációs a tárolóban hello segítségével [fordított proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="6d47a-192">By registering with hello Naming service, you can perform container-to-container communication within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="6d47a-193">Kommunikációs hello fordított proxy http figyelőportja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik.</span><span class="sxs-lookup"><span data-stu-id="6d47a-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="6d47a-194">További információkért lásd a hello következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="6d47a-194">For more information, see hello next section.</span></span> 

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="6d47a-195">Környezeti változók konfigurálása és beállítása</span><span class="sxs-lookup"><span data-stu-id="6d47a-195">Configure and set environment variables</span></span>
<span data-ttu-id="6d47a-196">Környezeti változók minden kódcsomag hello szolgáltatás jegyzékben adható meg.</span><span class="sxs-lookup"><span data-stu-id="6d47a-196">Environment variables can be specified for each code package in hello service manifest.</span></span> <span data-ttu-id="6d47a-197">Ez a funkció az összes szolgáltatáshoz elérhető attól függetlenül, hogy tárolókként, folyamatokként vagy vendég futtatható fájlokként vannak-e üzembe helyezve.</span><span class="sxs-lookup"><span data-stu-id="6d47a-197">This feature is available for all services irrespective of whether they are deployed as containers or processes or guest executables.</span></span> <span data-ttu-id="6d47a-198">Ha szeretné felülbírálni az környezeti változó értékek hello alkalmazás jegyzékfájlja, vagy adja meg azokat az alkalmazás paraméterekként üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="6d47a-198">You can override environment variable values in hello application manifest or specify them during deployment as application parameters.</span></span>

<span data-ttu-id="6d47a-199">hello service manifest következő XML-részletet szemlélteti, hogyan toospecify környezeti változók kód csomag:</span><span class="sxs-lookup"><span data-stu-id="6d47a-199">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>
```

<span data-ttu-id="6d47a-200">Ezek a környezeti változók hello alkalmazás jegyzékének szintjén felülbírálható lesz:</span><span class="sxs-lookup"><span data-stu-id="6d47a-200">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="6d47a-201">Hello előző példában a Microsoft hello explicit érték megadott `HttpGateway` környezeti változó (19000), miközben hivatott hello értéke `BackendServiceName` keresztül hello paraméter `[BackendSvc]` alkalmazás paraméter.</span><span class="sxs-lookup"><span data-stu-id="6d47a-201">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="6d47a-202">Ezek a beállítások lehetővé teszik toospecify hello értéke `BackendServiceName`értéke, ha hello alkalmazás központi telepítése, és nem rendelkezik rögzített érték hello jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6d47a-202">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="configure-isolation-mode"></a><span data-ttu-id="6d47a-203">Az elkülönítési mód konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6d47a-203">Configure isolation mode</span></span>

<span data-ttu-id="6d47a-204">A Windows tárolók - folyamat és a Hyper-V két elkülönítési módot támogat.</span><span class="sxs-lookup"><span data-stu-id="6d47a-204">Windows supports two isolation modes for containers - process and Hyper-V.</span></span>  <span data-ttu-id="6d47a-205">A hello folyamatainak elkülönítési módjának futó összes hello tárolók hello ugyanazon gazdagép gép megosztás hello kernel hello gazdagéphez.</span><span class="sxs-lookup"><span data-stu-id="6d47a-205">With hello process isolation mode, all hello containers running on hello same host machine share hello kernel with hello host.</span></span> <span data-ttu-id="6d47a-206">A Hyper-V hello elkülönítési üzemmódját hello kernelek elkülönítik minden Hyper-V tároló és a tároló-gazdagépen hello között.</span><span class="sxs-lookup"><span data-stu-id="6d47a-206">With hello Hyper-V isolation mode, hello kernels are isolated between each Hyper-V container and hello container host.</span></span> <span data-ttu-id="6d47a-207">hello elkülönítési üzemmódját megadott hello `ContainerHostPolicies` hello Alkalmazásjegyzék-fájl címkét.</span><span class="sxs-lookup"><span data-stu-id="6d47a-207">hello isolation mode is specified in hello `ContainerHostPolicies` tag in hello application manifest file.</span></span>  <span data-ttu-id="6d47a-208">hello elkülönítési módok megadható `process`, `hyperv`, és `default`.</span><span class="sxs-lookup"><span data-stu-id="6d47a-208">hello isolation modes that can be specified are `process`, `hyperv`, and `default`.</span></span> <span data-ttu-id="6d47a-209">Hello `default` elkülönítési üzemmódját alapértelmezés szerint használt érték túl`process` Windows Server rendszeren futtatja, és alapértelmezés szerint használt érték túl`hyperv` Windows 10-gazdagépeken.</span><span class="sxs-lookup"><span data-stu-id="6d47a-209">hello `default` isolation mode defaults too`process` on Windows Server hosts, and defaults too`hyperv` on Windows 10 hosts.</span></span>  <span data-ttu-id="6d47a-210">hello alábbi kódrészletben láthatja, hogyan hello elkülönítési üzemmódját hello Alkalmazásjegyzék-fájl megadott.</span><span class="sxs-lookup"><span data-stu-id="6d47a-210">hello following snippet shows how hello isolation mode is specified in hello application manifest file.</span></span>

```xml
   <ContainerHostPolicies CodePackageRef="NodeService.Code" Isolation="hyperv">
```


## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="6d47a-211">Fejezze be az alkalmazás és szolgáltatás jegyzékfájl példák</span><span class="sxs-lookup"><span data-stu-id="6d47a-211">Complete examples for application and service manifest</span></span>

<span data-ttu-id="6d47a-212">A következő példa alkalmazásjegyzéket:</span><span class="sxs-lookup"><span data-stu-id="6d47a-212">An example application manifest follows:</span></span>

```xml
    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>
```

<span data-ttu-id="6d47a-213">A következő egy példa service jegyzékfájl (alkalmazásjegyzék megelőző hello megadott):</span><span class="sxs-lookup"><span data-stu-id="6d47a-213">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

```xml
    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless front end in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Endpoints>
                <Endpoint Name="Endpoint1" UriScheme="http" Port="80" Protocol="http"/>
            </Endpoints>
        </Resources>
    </ServiceManifest>
```

## <a name="next-steps"></a><span data-ttu-id="6d47a-214">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6d47a-214">Next steps</span></span>
<span data-ttu-id="6d47a-215">Most, hogy indexelése szolgáltatásként telepített, akkor megtudhatja, hogyan toomanage olvasásával életciklus [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="6d47a-215">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="6d47a-216">A Service Fabric és a tárolók áttekintése</span><span class="sxs-lookup"><span data-stu-id="6d47a-216">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* <span data-ttu-id="6d47a-217">Egy vonatkozó példáért kivételt [Service Fabric tároló mintakódjainak megtekintése a Githubon](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="6d47a-217">For an example, checkout [Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>
