---
title: "aaaService háló és a Linux tárolók üzembe helyezése |} Microsoft Docs"
description: "A Service Fabric és hello használata Linux tárolók toodeploy mikroszolgáltatási alkalmazások. Ez a cikk ismerteti, amely a Service Fabric-tárolókban, és hogyan toodeploy egy Linux-tároló kép fürtbe hello képességek"
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 4ba99103-6064-429d-ba17-82861b6ddb11
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 6/29/2017
ms.author: msfussell
ms.openlocfilehash: e28f99a145b0594d871b0ec0566233a7ad235ce8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-linux-container-tooservice-fabric"></a><span data-ttu-id="6c490-104">A Linux-tároló tooService háló telepítése</span><span class="sxs-lookup"><span data-stu-id="6c490-104">Deploy a Linux container tooService Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6c490-105">Windows-tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="6c490-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="6c490-106">Linux-tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="6c490-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="6c490-107">Ez a cikk bemutatja, hogyan indexelése szolgáltatások Docker-tárolókban lévő Linux építve.</span><span class="sxs-lookup"><span data-stu-id="6c490-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="6c490-108">A Service Fabric különböző tároló képességeket, amelyek segítenek a mikroszolgáltatások létrehozására, amelyek indexelése épülnek alkalmazások építéséhez rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6c490-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="6c490-109">Ezek a szolgáltatások indexelése szolgáltatások nevezzük.</span><span class="sxs-lookup"><span data-stu-id="6c490-109">These services are called containerized services.</span></span>

<span data-ttu-id="6c490-110">hello lehetőségek tartalmazzák;</span><span class="sxs-lookup"><span data-stu-id="6c490-110">hello capabilities include;</span></span>

* <span data-ttu-id="6c490-111">Tároló lemezkép-telepítés és az aktiválás</span><span class="sxs-lookup"><span data-stu-id="6c490-111">Container image deployment and activation</span></span>
* <span data-ttu-id="6c490-112">Erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="6c490-112">Resource governance</span></span>
* <span data-ttu-id="6c490-113">Tárház-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="6c490-113">Repository authentication</span></span>
* <span data-ttu-id="6c490-114">Tároló-porthozzárendelést, toohost port</span><span class="sxs-lookup"><span data-stu-id="6c490-114">Container port toohost port mapping</span></span>
* <span data-ttu-id="6c490-115">Tároló-tároló felderítése és kommunikáció</span><span class="sxs-lookup"><span data-stu-id="6c490-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="6c490-116">Képes tooconfigure és környezeti változók megadása</span><span class="sxs-lookup"><span data-stu-id="6c490-116">Ability tooconfigure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="6c490-117">Egy docker-tároló csomagolására rendelkező yeoman</span><span class="sxs-lookup"><span data-stu-id="6c490-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="6c490-118">Egy tároló Linux csomagolás, beállíthatja vagy toouse yeoman sablon vagy [hozzon létre manuálisan hello alkalmazáscsomag](#manually).</span><span class="sxs-lookup"><span data-stu-id="6c490-118">When packaging a container on Linux, you can choose either toouse a yeoman template or [create hello application package manually](#manually).</span></span>

<span data-ttu-id="6c490-119">A Service Fabric-alkalmazás tartalmazhat egy vagy több tárolóban, az egy adott szerepkör a postai hello alkalmazás működését.</span><span class="sxs-lookup"><span data-stu-id="6c490-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering hello application's functionality.</span></span> <span data-ttu-id="6c490-120">Service Fabric SDK Linux hello tartalmaz egy [Yeoman](http://yeoman.io/) , így könnyen toocreate generátor az alkalmazás, és adja hozzá a tároló-lemezkép.</span><span class="sxs-lookup"><span data-stu-id="6c490-120">hello Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy toocreate your application and add a container image.</span></span> <span data-ttu-id="6c490-121">Most használja az alkalmazás egy Docker-tároló nevű Yeoman toocreate *SimpleContainerApp*.</span><span class="sxs-lookup"><span data-stu-id="6c490-121">Let's use Yeoman toocreate an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="6c490-122">Hozzáadhat további szolgáltatások később hello létrehozott fájlok szerkesztésével.</span><span class="sxs-lookup"><span data-stu-id="6c490-122">You can add more services later by editing hello generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="6c490-123">A fejlesztési mezőben Docker telepítése</span><span class="sxs-lookup"><span data-stu-id="6c490-123">Install Docker on your development box</span></span>

<span data-ttu-id="6c490-124">Futtatási hello következő parancsokat a Linux-fejlesztési be tooinstall docker (hello vagrant lemezkép használatakor az os x docker már telepítve van):</span><span class="sxs-lookup"><span data-stu-id="6c490-124">Run hello following commands tooinstall docker on your Linux development box (if you are using hello vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-hello-application"></a><span data-ttu-id="6c490-125">Hello alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="6c490-125">Create hello application</span></span>
1. <span data-ttu-id="6c490-126">Írja be a terminálba a következőt: `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="6c490-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="6c490-127">Az alkalmazás - például mycontainerap neve</span><span class="sxs-lookup"><span data-stu-id="6c490-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="6c490-128">Adja meg hello tároló lemezképének egy DockerHub tárházból hello URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="6c490-128">Provide hello URL for hello container image from a DockerHub repo.</span></span> <span data-ttu-id="6c490-129">hello kép paraméter vesz hello űrlap [tárház] / [lemezkép neve]</span><span class="sxs-lookup"><span data-stu-id="6c490-129">hello image parameter takes hello form [repo]/[image name]</span></span>
4. <span data-ttu-id="6c490-130">Ha hello kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor meg kell tooexplicitly adja meg a bemeneti parancsokat parancsok toorun belül hello tároló, amely közli a hello tároló indítás után fut vesszővel tagolt állítja be.</span><span class="sxs-lookup"><span data-stu-id="6c490-130">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="deploy-hello-application"></a><span data-ttu-id="6c490-132">Hello alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6c490-132">Deploy hello application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="6c490-133">Az XPlat CLI használatával</span><span class="sxs-lookup"><span data-stu-id="6c490-133">Using XPlat CLI</span></span>
<span data-ttu-id="6c490-134">Miután hello alkalmazás épül, ezután telepítheti azt toohello helyi fürt hello Azure parancssori felület használatával.</span><span class="sxs-lookup"><span data-stu-id="6c490-134">Once hello application is built, you can deploy it toohello local cluster using hello Azure CLI.</span></span>

1. <span data-ttu-id="6c490-135">Csatlakozás helyi Service Fabric-fürt toohello.</span><span class="sxs-lookup"><span data-stu-id="6c490-135">Connect toohello local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="6c490-136">Használjon hello telepítési parancsfájlját hello sablon toocopy hello alkalmazás csomag toohello fürt lemezképtárolóhoz, hello alkalmazástípus regisztrálása, és a hello alkalmazás példányt létrehozni.</span><span class="sxs-lookup"><span data-stu-id="6c490-136">Use hello install script provided in hello template toocopy hello application package toohello cluster's image store, register hello application type, and create an instance of hello application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="6c490-137">Nyisson meg egy böngészőt, és keresse meg a Fabric Explorer tooService: 19080/Explorer (a név felülírandó localhost a hello privát IP-címe hello VM Vagrant használatakor a Mac OS x).</span><span class="sxs-lookup"><span data-stu-id="6c490-137">Open a browser and navigate tooService Fabric Explorer at http://localhost:19080/Explorer (replace localhost with hello private IP of hello VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="6c490-138">Hello alkalmazások csomópontot, és vegye figyelembe, hogy most egy bejegyzést, az alkalmazás típusának és egy másikat a hello adott típus első példányát.</span><span class="sxs-lookup"><span data-stu-id="6c490-138">Expand hello Applications node and note that there is now an entry for your application type and another for hello first instance of that type.</span></span>
5. <span data-ttu-id="6c490-139">Hello eltávolítás parancsfájl használata hello sablon toodelete hello alkalmazáspéldány és a hello alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="6c490-139">Use hello uninstall script provided in hello template toodelete hello application instance and unregister hello application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="6c490-140">Az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="6c490-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="6c490-141">Lásd: hello hivatkozás doc kezeléséről egy [életciklus használó hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="6c490-141">See hello reference doc on managing an [application life cycle using hello Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="6c490-142">A mintaalkalmazás [kivételt hello Service Fabric tároló kódja a Githubon található – minták](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="6c490-142">For an example application, [checkout hello Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-tooan-existing-application"></a><span data-ttu-id="6c490-143">További szolgáltatások tooan meglévő alkalmazás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="6c490-143">Adding more services tooan existing application</span></span>

<span data-ttu-id="6c490-144">tooadd egy másik tárolóban szolgáltatásalkalmazás tooan már létrehozott `yo`, hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="6c490-144">tooadd another container service tooan application already created using `yo`, perform hello following steps:</span></span>

1. <span data-ttu-id="6c490-145">Hello meglévő alkalmazás gyökérkönyvtárának toohello módosítása.</span><span class="sxs-lookup"><span data-stu-id="6c490-145">Change directory toohello root of hello existing application.</span></span>  <span data-ttu-id="6c490-146">Például `cd ~/YeomanSamples/MyApplication`, ha `MyApplication` Yeoman által létrehozott hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="6c490-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is hello application created by Yeoman.</span></span>
2. <span data-ttu-id="6c490-147">Futtassa a `yo azuresfcontainer:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6c490-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="6c490-148">Manuálisan csomag és a tároló-lemezkép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="6c490-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="6c490-149">hello folyamat manuálisan csomagolási indexelése szolgáltatás lépések hello alapul:</span><span class="sxs-lookup"><span data-stu-id="6c490-149">hello process of manually packaging a containerized service is based on hello following steps:</span></span>

1. <span data-ttu-id="6c490-150">Hello tárolók tooyour tárház közzététele.</span><span class="sxs-lookup"><span data-stu-id="6c490-150">Publish hello containers tooyour repository.</span></span>
2. <span data-ttu-id="6c490-151">Hello csomag könyvtárstruktúrát létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6c490-151">Create hello package directory structure.</span></span>
3. <span data-ttu-id="6c490-152">Hello service manifest-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="6c490-152">Edit hello service manifest file.</span></span>
4. <span data-ttu-id="6c490-153">Hello Alkalmazásjegyzék-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="6c490-153">Edit hello application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="6c490-154">Központi telepítése és a tároló lemezkép aktiválása</span><span class="sxs-lookup"><span data-stu-id="6c490-154">Deploy and activate a container image</span></span>
<span data-ttu-id="6c490-155">A Service Fabric hello [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek.</span><span class="sxs-lookup"><span data-stu-id="6c490-155">In hello Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="6c490-156">toodeploy, és aktiválja a tároló, hello tároló lemezkép a put hello nevét egy `ContainerHost` elem hello szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6c490-156">toodeploy and activate a container, put hello name of hello container image into a `ContainerHost` element in hello service manifest.</span></span>

<span data-ttu-id="6c490-157">Hello szolgáltatás jegyzékben, vegye fel a `ContainerHost` hello belépési ponthoz.</span><span class="sxs-lookup"><span data-stu-id="6c490-157">In hello service manifest, add a `ContainerHost` for hello entry point.</span></span> <span data-ttu-id="6c490-158">Majd a készlet hello `ImageName` hello tároló tárház és lemezkép toobe hello nevét.</span><span class="sxs-lookup"><span data-stu-id="6c490-158">Then set hello `ImageName` toobe hello name of hello container repository and image.</span></span> <span data-ttu-id="6c490-159">hello következő részleges jegyzékfájl szemlélteti, hogyan toodeploy hello tároló nevű `myimage:v1` a tárházból nevű `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="6c490-159">hello following partial manifest shows an example of how toodeploy hello container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="6c490-160">Megadhat bemeneti parancsok választható hello megadásával `Commands` parancsok toorun belül hello tároló vesszővel tagolt számú elemet.</span><span class="sxs-lookup"><span data-stu-id="6c490-160">You can provide input commands by specifying hello optional `Commands` element with a comma-delimited set of commands toorun inside hello container.</span></span>

> [!NOTE]
> <span data-ttu-id="6c490-161">Ha hello kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor meg kell tooexplicitly adja meg a bemeneti parancsok belül `Commands` parancsok toorun belül hello tároló, amely közli a hello tároló futtatása után vesszővel tagolt számú elem indítási.</span><span class="sxs-lookup"><span data-stu-id="6c490-161">If hello image does not have a workload entry-point defined, then you need tooexplicitly specify input commands inside `Commands` element with a comma-delimited set of commands toorun inside hello container, which will keep hello container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="6c490-162">Erőforrás-irányítás ismertetése</span><span class="sxs-lookup"><span data-stu-id="6c490-162">Understand resource governance</span></span>
<span data-ttu-id="6c490-163">Erőforrás-irányítás, mert egy olyan képességet, amely korlátozza a tároló hello hello erőforrások hello tároló hello gazdagépen használhatják.</span><span class="sxs-lookup"><span data-stu-id="6c490-163">Resource governance is a capability of hello container that restricts hello resources that hello container can use on hello host.</span></span> <span data-ttu-id="6c490-164">Hello `ResourceGovernancePolicy`, amely hello alkalmazásjegyzékben megadott használt toodeclare erőforrás korlátai a szolgáltatáscsomagot a kódot.</span><span class="sxs-lookup"><span data-stu-id="6c490-164">hello `ResourceGovernancePolicy`, which is specified in hello application manifest is used toodeclare resource limits for a service code package.</span></span> <span data-ttu-id="6c490-165">Erőforrás-korlátozások állíthat be a következő erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="6c490-165">Resource limits can be set for hello following resources:</span></span>

* <span data-ttu-id="6c490-166">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="6c490-166">Memory</span></span>
* <span data-ttu-id="6c490-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="6c490-167">MemorySwap</span></span>
* <span data-ttu-id="6c490-168">CpuShares (CPU relatív súly)</span><span class="sxs-lookup"><span data-stu-id="6c490-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="6c490-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="6c490-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="6c490-170">BlkioWeight (BlockIO relatív súly).</span><span class="sxs-lookup"><span data-stu-id="6c490-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="6c490-171">Egy későbbi kiadásban támogatja a megadott blokk IO korlátok iops-érték, például megadása olvasási/írási bit/másodperc, és másokkal is fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="6c490-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="6c490-172">A tárház hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="6c490-172">Authenticate a repository</span></span>
<span data-ttu-id="6c490-173">egy tároló toodownload, lehetséges, hogy tooprovide bejelentkezési hitelesítő adatok toohello tároló tárházba.</span><span class="sxs-lookup"><span data-stu-id="6c490-173">toodownload a container, you might have tooprovide sign-in credentials toohello container repository.</span></span> <span data-ttu-id="6c490-174">hello bejelentkezési, hello alkalmazásjegyzékben, a megadott hitelesítő adatok használt toospecify hello bejelentkezési adatait, vagy SSH-kulcsban, a hello tároló lemezkép letöltése hello lemezképtárból.</span><span class="sxs-lookup"><span data-stu-id="6c490-174">hello sign-in credentials, specified in hello application manifest, are used toospecify hello sign-in information, or SSH key, for downloading hello container image from hello image repository.</span></span> <span data-ttu-id="6c490-175">hello alábbi példa bemutatja nevű *tesztfelhasználó néven* hello a jelszó nyílt szövegként együtt (*nem* ajánlott):</span><span class="sxs-lookup"><span data-stu-id="6c490-175">hello following example shows an account called *TestUser* along with hello password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="6c490-176">Azt javasoljuk, hogy titkosítsa a hello jelszó toohello gépen központilag telepített tanúsítvánnyal.</span><span class="sxs-lookup"><span data-stu-id="6c490-176">We recommend that you encrypt hello password by using a certificate that's deployed toohello machine.</span></span>

<span data-ttu-id="6c490-177">hello alábbi példa bemutatja nevű *tesztfelhasználó néven*, ahol hello jelszó nevű tanúsítvánnyal titkosított *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="6c490-177">hello following example shows an account called *TestUser*, where hello password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="6c490-178">Használhatja a hello `Invoke-ServiceFabricEncryptText` PowerShell toocreate hello titkos titkosítási parancsszövege hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="6c490-178">You can use hello `Invoke-ServiceFabricEncryptText` PowerShell command toocreate hello secret cipher text for hello password.</span></span> <span data-ttu-id="6c490-179">További információkért lásd: hello cikk [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="6c490-179">For more information, see hello article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="6c490-180">hello az tanúsítvány titkos kulcsa hello toodecrypt hello jelszó által használt helyi számítógép telepített toohello sávon kívüli metódusban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="6c490-180">hello private key of hello certificate that's used toodecrypt hello password must be deployed toohello local machine in an out-of-band method.</span></span> <span data-ttu-id="6c490-181">(Az Azure-ban Ez a módszer az Azure Resource Manager.) Majd amikor a Service Fabric hello szolgáltatás csomag toohello számítógépe telepíti, azt vissza tudja fejteni hello titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="6c490-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys hello service package toohello machine, it can decrypt hello secret.</span></span> <span data-ttu-id="6c490-182">Hello titkos hello fióknév együtt használva, majd hitelesítheti a hello tároló tárház.</span><span class="sxs-lookup"><span data-stu-id="6c490-182">By using hello secret along with hello account name, it can then authenticate with hello container repository.</span></span>

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

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="6c490-183">Tároló port-állomás porthozzárendelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c490-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="6c490-184">Konfigurálhatja a gazdagépen használt port toocommunicate hello tárolóval megadásával egy `PortBinding` hello alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6c490-184">You can configure a host port used toocommunicate with hello container by specifying a `PortBinding` in hello application manifest.</span></span> <span data-ttu-id="6c490-185">hello port kötés maps hello port toowhich hello szolgáltatást figyel tooa tárolóportot hello hello gazdagépen belül.</span><span class="sxs-lookup"><span data-stu-id="6c490-185">hello port binding maps hello port toowhich hello service is listening inside hello container tooa port on hello host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="6c490-186">Tároló-tároló felderítése és a kommunikáció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="6c490-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="6c490-187">Hello segítségével `PortBinding` házirend, leképezheti a tárolóportot tooan `Endpoint` hello szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6c490-187">By using hello `PortBinding` policy, you can map a container port tooan `Endpoint` in hello service manifest.</span></span> <span data-ttu-id="6c490-188">végpont hello `Endpoint1` adhat meg egy rögzített port (például a 80-as port).</span><span class="sxs-lookup"><span data-stu-id="6c490-188">hello endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="6c490-189">Azt is megadhatja, nincs port minden, ebben az esetben az alkalmazás porttartományát hello fürt véletlenszerű port meg van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="6c490-189">It can also specify no port at all, in which case a random port from hello cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="6c490-190">Ha megad egy végpontot, használatával hello `Endpoint` címkét hello szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont toohello Naming service.</span><span class="sxs-lookup"><span data-stu-id="6c490-190">If you specify an endpoint, using hello `Endpoint` tag in hello service manifest of a guest container, Service Fabric can automatically publish this endpoint toohello Naming service.</span></span> <span data-ttu-id="6c490-191">Hello fürtben futó egyéb szolgáltatások így felderíthetők az ebben a tárolóban hello REST lekérdezésekkel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="6c490-191">Other services that are running in hello cluster can thus discover this container using hello REST queries for resolving.</span></span>

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

<span data-ttu-id="6c490-192">Ha regisztrálja a Naming service hello, egyszerűen elvégezhető tároló-tároló kommunikációs hello kódban a tárolóban hello segítségével [fordított proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="6c490-192">By registering with hello Naming service, you can easily do container-to-container communication in hello code within your container by using hello [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="6c490-193">Kommunikációs hello fordított proxy http figyelőportja és hello szolgáltatások kiszolgálóként használni kívánt toocommunicate a környezeti változók neve hello megadásával történik.</span><span class="sxs-lookup"><span data-stu-id="6c490-193">Communication is performed by providing hello reverse proxy http listening port and hello name of hello services that you want toocommunicate with as environment variables.</span></span> <span data-ttu-id="6c490-194">További információkért lásd a hello következő szakaszt.</span><span class="sxs-lookup"><span data-stu-id="6c490-194">For more information, see hello next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="6c490-195">Környezeti változók konfigurálása és beállítása</span><span class="sxs-lookup"><span data-stu-id="6c490-195">Configure and set environment variables</span></span>
<span data-ttu-id="6c490-196">Környezeti változók minden hello szolgáltatás jegyzékben, mind a tárolók üzembe helyezett szolgáltatások vagy szolgáltatások, folyamatok/Vendég végrehajtható fájlok telepített kódcsomag adható meg.</span><span class="sxs-lookup"><span data-stu-id="6c490-196">Environment variables can be specified for each code package in hello service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="6c490-197">A környezeti változók értékeinek kifejezetten hello alkalmazásjegyzék felülbírálva, vagy alkalmazás paraméterekként telepítés során megadott.</span><span class="sxs-lookup"><span data-stu-id="6c490-197">These environment variable values can be overridden specifically in hello application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="6c490-198">hello service manifest következő XML-részletet szemlélteti, hogyan toospecify környezeti változók kód csomag:</span><span class="sxs-lookup"><span data-stu-id="6c490-198">hello following service manifest XML snippet shows an example of how toospecify environment variables for a code package:</span></span>

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

<span data-ttu-id="6c490-199">Ezek a környezeti változók hello alkalmazás jegyzékének szintjén felülbírálható lesz:</span><span class="sxs-lookup"><span data-stu-id="6c490-199">These environment variables can be overridden at hello application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="6c490-200">Hello előző példában a Microsoft hello explicit érték megadott `HttpGateway` környezeti változó (19000), miközben hivatott hello értéke `BackendServiceName` keresztül hello paraméter `[BackendSvc]` alkalmazás paraméter.</span><span class="sxs-lookup"><span data-stu-id="6c490-200">In hello previous example, we specified an explicit value for hello `HttpGateway` environment variable (19000), while we set hello value for `BackendServiceName` parameter via hello `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="6c490-201">Ezek a beállítások lehetővé teszik toospecify hello értéke `BackendServiceName`értéke, ha hello alkalmazás központi telepítése, és nem rendelkezik rögzített érték hello jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="6c490-201">These settings enable you toospecify hello value for `BackendServiceName`value when you deploy hello application and not have a fixed value in hello manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="6c490-202">Fejezze be az alkalmazás és szolgáltatás jegyzékfájl példák</span><span class="sxs-lookup"><span data-stu-id="6c490-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="6c490-203">A következő példa alkalmazásjegyzéket:</span><span class="sxs-lookup"><span data-stu-id="6c490-203">An example application manifest follows:</span></span>

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

<span data-ttu-id="6c490-204">A következő egy példa service jegyzékfájl (alkalmazásjegyzék megelőző hello megadott):</span><span class="sxs-lookup"><span data-stu-id="6c490-204">An example service manifest (specified in hello preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="6c490-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6c490-205">Next steps</span></span>
<span data-ttu-id="6c490-206">Most, hogy indexelése szolgáltatásként telepített, akkor megtudhatja, hogyan toomanage olvasásával életciklus [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="6c490-206">Now that you have deployed a containerized service, learn how toomanage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="6c490-207">A Service Fabric és a tárolók áttekintése</span><span class="sxs-lookup"><span data-stu-id="6c490-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="6c490-208">Service Fabric-fürtök hello Azure parancssori felület használatával való interakció</span><span class="sxs-lookup"><span data-stu-id="6c490-208">Interacting with Service Fabric clusters using hello Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="6c490-209">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="6c490-209">Related articles</span></span>

* [<span data-ttu-id="6c490-210">A Service Fabric első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="6c490-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="6c490-211">Első lépések a Service Fabric XPlat CLI használatával</span><span class="sxs-lookup"><span data-stu-id="6c490-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
