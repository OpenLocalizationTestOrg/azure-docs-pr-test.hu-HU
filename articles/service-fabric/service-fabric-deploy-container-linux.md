---
title: "Service Fabric és központi telepítése Linux a tárolók |} Microsoft Docs"
description: "A Service Fabric és a Linux-tárolók használatát mikroszolgáltatási alkalmazások központi telepítése. Ez a cikk ismerteti, amely tárolók biztosít a Service Fabric képességeit és a Linux-tároló lemezkép központi telepítése a fürtbe"
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
ms.openlocfilehash: 9dcec753e5f999a1bac07276373c0c25f89ec58d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-a-linux-container-to-service-fabric"></a><span data-ttu-id="0f0a2-104">A Service Fabric központi telepítése egy Linux-tároló</span><span class="sxs-lookup"><span data-stu-id="0f0a2-104">Deploy a Linux container to Service Fabric</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0f0a2-105">Windows-tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="0f0a2-105">Deploy Windows Container</span></span>](service-fabric-deploy-container.md)
> * [<span data-ttu-id="0f0a2-106">Linux-tároló üzembe</span><span class="sxs-lookup"><span data-stu-id="0f0a2-106">Deploy Linux Container</span></span>](service-fabric-deploy-container-linux.md)
>
>

<span data-ttu-id="0f0a2-107">Ez a cikk bemutatja, hogyan indexelése szolgáltatások Docker-tárolókban lévő Linux építve.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-107">This article walks you through building containerized services in Docker containers on Linux.</span></span>

<span data-ttu-id="0f0a2-108">A Service Fabric különböző tároló képességeket, amelyek segítenek a mikroszolgáltatások létrehozására, amelyek indexelése épülnek alkalmazások építéséhez rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-108">Service Fabric has several container capabilities that help you with building applications that are composed of microservices that are containerized.</span></span> <span data-ttu-id="0f0a2-109">Ezek a szolgáltatások indexelése szolgáltatások nevezzük.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-109">These services are called containerized services.</span></span>

<span data-ttu-id="0f0a2-110">A lehetőségek tartalmazzák;</span><span class="sxs-lookup"><span data-stu-id="0f0a2-110">The capabilities include;</span></span>

* <span data-ttu-id="0f0a2-111">Tároló lemezkép-telepítés és az aktiválás</span><span class="sxs-lookup"><span data-stu-id="0f0a2-111">Container image deployment and activation</span></span>
* <span data-ttu-id="0f0a2-112">Erőforrás-irányítás</span><span class="sxs-lookup"><span data-stu-id="0f0a2-112">Resource governance</span></span>
* <span data-ttu-id="0f0a2-113">Tárház-hitelesítés</span><span class="sxs-lookup"><span data-stu-id="0f0a2-113">Repository authentication</span></span>
* <span data-ttu-id="0f0a2-114">A gazdagép porthozzárendelés tárolóportot</span><span class="sxs-lookup"><span data-stu-id="0f0a2-114">Container port to host port mapping</span></span>
* <span data-ttu-id="0f0a2-115">Tároló-tároló felderítése és kommunikáció</span><span class="sxs-lookup"><span data-stu-id="0f0a2-115">Container-to-container discovery and communication</span></span>
* <span data-ttu-id="0f0a2-116">Konfigurálása, és állítsa be a környezeti változók</span><span class="sxs-lookup"><span data-stu-id="0f0a2-116">Ability to configure and set environment variables</span></span>

## <a name="packaging-a-docker-container-with-yeoman"></a><span data-ttu-id="0f0a2-117">Egy docker-tároló csomagolására rendelkező yeoman</span><span class="sxs-lookup"><span data-stu-id="0f0a2-117">Packaging a docker container with yeoman</span></span>
<span data-ttu-id="0f0a2-118">Egy tároló Linux-csomagban, amikor Ön választhatja yeoman sablon használata vagy [hozzon létre manuálisan az alkalmazáscsomag](#manually).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-118">When packaging a container on Linux, you can choose either to use a yeoman template or [create the application package manually](#manually).</span></span>

<span data-ttu-id="0f0a2-119">A Service Fabric-alkalmazás tartalmazhat egy vagy több tárolóban, az egy adott szerepkör a postai az alkalmazás működését.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-119">A Service Fabric application can contain one or more containers, each with a specific role in delivering the application's functionality.</span></span> <span data-ttu-id="0f0a2-120">A Linux Service Fabric SDK tartalmaz egy [Yeoman](http://yeoman.io/)-generátort, amely megkönnyíti az alkalmazás létrehozását és egy tárolórendszerkép hozzáadását.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-120">The Service Fabric SDK for Linux includes a [Yeoman](http://yeoman.io/) generator that makes it easy to create your application and add a container image.</span></span> <span data-ttu-id="0f0a2-121">Hozzunk létre egy alkalmazást, amely egyetlen, *SimpleContainerApp* nevű Docker-tárolóval rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-121">Let's use Yeoman to create an application with a single Docker container called *SimpleContainerApp*.</span></span> <span data-ttu-id="0f0a2-122">Hozzáadhat további szolgáltatások később a létrehozott szerkesztésével manifest fájlt.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-122">You can add more services later by editing the generated manifest files.</span></span>

## <a name="install-docker-on-your-development-box"></a><span data-ttu-id="0f0a2-123">A fejlesztési mezőben Docker telepítése</span><span class="sxs-lookup"><span data-stu-id="0f0a2-123">Install Docker on your development box</span></span>

<span data-ttu-id="0f0a2-124">Docker telepítését a Linux-fejlesztési mezőben a következő parancsokat (a vagrant lemezkép használatakor az os x docker már telepítve van):</span><span class="sxs-lookup"><span data-stu-id="0f0a2-124">Run the following commands to install docker on your Linux development box (if you are using the vagrant image on OSX, docker is already installed):</span></span>

```bash
    sudo apt-get install wget
    wget -qO- https://get.docker.io/ | sh
```

## <a name="create-the-application"></a><span data-ttu-id="0f0a2-125">Az alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="0f0a2-125">Create the application</span></span>
1. <span data-ttu-id="0f0a2-126">Írja be a terminálba a következőt: `yo azuresfcontainer`.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-126">In a terminal, type `yo azuresfcontainer`.</span></span>
2. <span data-ttu-id="0f0a2-127">Az alkalmazás - például mycontainerap neve</span><span class="sxs-lookup"><span data-stu-id="0f0a2-127">Name your application - for example, mycontainerap</span></span>
3. <span data-ttu-id="0f0a2-128">Adja meg az URL-címet a tároló lemezképének egy DockerHub tárházból.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-128">Provide the URL for the container image from a DockerHub repo.</span></span> <span data-ttu-id="0f0a2-129">A kép paraméter [tárház] formájában történik [kép neve]</span><span class="sxs-lookup"><span data-stu-id="0f0a2-129">The image parameter takes the form [repo]/[image name]</span></span>
4. <span data-ttu-id="0f0a2-130">Ha a kép nem rendelkezik a munkaterhelés belépési ponttal definiált, explicit módon adja meg a parancsok futtatásához a tároló, amely akkor is megtartja a tároló indítás után fut belül vesszővel tagolt számú bemeneti parancsokat kell használnia.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-130">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

![Tárolókhoz készült Service Fabric Yeoman-generátor][sf-yeoman]

## <a name="deploy-the-application"></a><span data-ttu-id="0f0a2-132">Az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0f0a2-132">Deploy the application</span></span>

### <a name="using-xplat-cli"></a><span data-ttu-id="0f0a2-133">Az XPlat CLI használatával</span><span class="sxs-lookup"><span data-stu-id="0f0a2-133">Using XPlat CLI</span></span>
<span data-ttu-id="0f0a2-134">Az alkalmazást a létrehozása után az Azure parancssori felülettel telepítheti a helyi fürtben.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-134">Once the application is built, you can deploy it to the local cluster using the Azure CLI.</span></span>

1. <span data-ttu-id="0f0a2-135">Csatlakozzon a helyi Service Fabric-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-135">Connect to the local Service Fabric cluster.</span></span>

    ```bash
    azure servicefabric cluster connect
    ```

2. <span data-ttu-id="0f0a2-136">Használja a sablonban megadott telepítési szkriptet az alkalmazáscsomag a fürt lemezképtárolójába való másolásához, regisztrálja az alkalmazás típusát, és hozza létre az alkalmazás egy példányát.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-136">Use the install script provided in the template to copy the application package to the cluster's image store, register the application type, and create an instance of the application.</span></span>

    ```bash
    ./install.sh
    ```

3. <span data-ttu-id="0f0a2-137">Nyisson meg egy böngészőt, és keresse fel a Service Fabric Explorert a következő címen: http://localhost:19080/Explorer (a Vagrant Mac OS X rendszeren való használata esetében a localhost helyett használja a virtuális gép magánhálózati IP-címét).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-137">Open a browser and navigate to Service Fabric Explorer at http://localhost:19080/Explorer (replace localhost with the private IP of the VM if using Vagrant on Mac OS X).</span></span>
4. <span data-ttu-id="0f0a2-138">Bontsa ki az Alkalmazások csomópontot, és figyelje meg, hogy most már megjelenik benne egy bejegyzés az alkalmazása típusához, és egy másik a típus első példányához.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-138">Expand the Applications node and note that there is now an entry for your application type and another for the first instance of that type.</span></span>
5. <span data-ttu-id="0f0a2-139">A sablonban megadott eltávolítása parancsfájl segítségével törölje az alkalmazáspéldány, és az alkalmazástípus regisztrációjának törlése.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-139">Use the uninstall script provided in the template to delete the application instance and unregister the application type.</span></span>

    ```bash
    ./uninstall.sh
    ```

### <a name="using-azure-cli-20"></a><span data-ttu-id="0f0a2-140">Az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="0f0a2-140">Using Azure CLI 2.0</span></span>

<span data-ttu-id="0f0a2-141">Lásd: a hivatkozási doc kezeléséről egy [alkalmazás életciklusa az Azure CLI 2.0 használatával](service-fabric-application-lifecycle-azure-cli-2-0.md).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-141">See the reference doc on managing an [application life cycle using the Azure CLI 2.0](service-fabric-application-lifecycle-azure-cli-2-0.md).</span></span>

<span data-ttu-id="0f0a2-142">A mintaalkalmazás [kivételt a Service Fabric-tároló kódja a Githubon található – minták](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span><span class="sxs-lookup"><span data-stu-id="0f0a2-142">For an example application, [checkout the Service Fabric container code samples on GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-containers)</span></span>

## <a name="adding-more-services-to-an-existing-application"></a><span data-ttu-id="0f0a2-143">További szolgáltatások hozzáadása meglévő alkalmazáshoz</span><span class="sxs-lookup"><span data-stu-id="0f0a2-143">Adding more services to an existing application</span></span>

<span data-ttu-id="0f0a2-144">Egy másik tárolószolgáltatás hozzáadása egy már létrehozott alkalmazás `yo`, hajtsa végre a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-144">To add another container service to an application already created using `yo`, perform the following steps:</span></span>

1. <span data-ttu-id="0f0a2-145">Lépjen a meglevő alkalmazás gyökérkönyvtárába.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-145">Change directory to the root of the existing application.</span></span>  <span data-ttu-id="0f0a2-146">Például `cd ~/YeomanSamples/MyApplication`, ha a `MyApplication` a Yeoman által létrehozott alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-146">For example, `cd ~/YeomanSamples/MyApplication`, if `MyApplication` is the application created by Yeoman.</span></span>
2. <span data-ttu-id="0f0a2-147">Futtassa a `yo azuresfcontainer:AddService` parancsot.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-147">Run `yo azuresfcontainer:AddService`</span></span>

<a id="manually"></a>

## <a name="manually-package-and-deploy-a-container-image"></a><span data-ttu-id="0f0a2-148">Manuálisan csomag és a tároló-lemezkép központi telepítése</span><span class="sxs-lookup"><span data-stu-id="0f0a2-148">Manually package and deploy a container image</span></span>
<span data-ttu-id="0f0a2-149">A folyamat manuálisan csomagolási indexelése szolgáltatás alapul az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-149">The process of manually packaging a containerized service is based on the following steps:</span></span>

1. <span data-ttu-id="0f0a2-150">A tárolók közzététele a tárházhoz.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-150">Publish the containers to your repository.</span></span>
2. <span data-ttu-id="0f0a2-151">Hozza létre a csomag könyvtárstruktúrát.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-151">Create the package directory structure.</span></span>
3. <span data-ttu-id="0f0a2-152">A service manifest fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-152">Edit the service manifest file.</span></span>
4. <span data-ttu-id="0f0a2-153">Az Alkalmazásjegyzék-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-153">Edit the application manifest file.</span></span>

## <a name="deploy-and-activate-a-container-image"></a><span data-ttu-id="0f0a2-154">Központi telepítése és a tároló lemezkép aktiválása</span><span class="sxs-lookup"><span data-stu-id="0f0a2-154">Deploy and activate a container image</span></span>
<span data-ttu-id="0f0a2-155">A Service Fabric a [alkalmazásmodell](service-fabric-application-model.md), a tároló jelöli egy alkalmazásgazda, mely több szolgáltatásban replikák kerülnek.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-155">In the Service Fabric [application model](service-fabric-application-model.md), a container represents an application host in which multiple service replicas are placed.</span></span> <span data-ttu-id="0f0a2-156">Üzembe helyezését, és aktiválja a tároló, helyezze be a tároló lemezkép nevét egy `ContainerHost` elem a szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-156">To deploy and activate a container, put the name of the container image into a `ContainerHost` element in the service manifest.</span></span>

<span data-ttu-id="0f0a2-157">A szolgáltatás jegyzékfájlban hozzáadása egy `ContainerHost` a belépési pont számára.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-157">In the service manifest, add a `ContainerHost` for the entry point.</span></span> <span data-ttu-id="0f0a2-158">Utána állítsa be a `ImageName` kell lennie a tároló tárházhoz és a lemezkép nevét.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-158">Then set the `ImageName` to be the name of the container repository and image.</span></span> <span data-ttu-id="0f0a2-159">A következő részleges jegyzékfájl látható példa központi telepítése a tároló neve `myimage:v1` a tárházból nevű `myrepo`:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-159">The following partial manifest shows an example of how to deploy the container called `myimage:v1` from a repository called `myrepo`:</span></span>

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

<span data-ttu-id="0f0a2-160">Bemeneti parancsok megadhatja a választható megadásával `Commands` parancsok futtatásához a tároló belső vesszővel tagolt számú elemet.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-160">You can provide input commands by specifying the optional `Commands` element with a comma-delimited set of commands to run inside the container.</span></span>

> [!NOTE]
> <span data-ttu-id="0f0a2-161">Ha a kép nem rendelkezik egy munkaterhelés belépési pont definiálva, akkor explicit módon adja meg a bemeneti parancsokat belül kell `Commands` elem futhat a tároló, amely akkor is megtartja a tároló indítás után fut, parancsokat tartalmazó, vesszővel elválasztott készletével.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-161">If the image does not have a workload entry-point defined, then you need to explicitly specify input commands inside `Commands` element with a comma-delimited set of commands to run inside the container, which will keep the container running after startup.</span></span>

## <a name="understand-resource-governance"></a><span data-ttu-id="0f0a2-162">Erőforrás-irányítás ismertetése</span><span class="sxs-lookup"><span data-stu-id="0f0a2-162">Understand resource governance</span></span>
<span data-ttu-id="0f0a2-163">Erőforrás-irányítás egy olyan képességet, a tároló, amely korlátozza az erőforrásokat, amelyek a tároló a gazdagépen.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-163">Resource governance is a capability of the container that restricts the resources that the container can use on the host.</span></span> <span data-ttu-id="0f0a2-164">A `ResourceGovernancePolicy`, amely az alkalmazás jegyzékében meghatározott szolgál a szolgáltatáscsomagot a kód erőforrás korlátairól deklarálható.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-164">The `ResourceGovernancePolicy`, which is specified in the application manifest is used to declare resource limits for a service code package.</span></span> <span data-ttu-id="0f0a2-165">Erőforrás-korlátozások állíthat be a következőket:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-165">Resource limits can be set for the following resources:</span></span>

* <span data-ttu-id="0f0a2-166">Memory (Memória)</span><span class="sxs-lookup"><span data-stu-id="0f0a2-166">Memory</span></span>
* <span data-ttu-id="0f0a2-167">MemorySwap</span><span class="sxs-lookup"><span data-stu-id="0f0a2-167">MemorySwap</span></span>
* <span data-ttu-id="0f0a2-168">CpuShares (CPU relatív súly)</span><span class="sxs-lookup"><span data-stu-id="0f0a2-168">CpuShares (CPU relative weight)</span></span>
* <span data-ttu-id="0f0a2-169">MemoryReservationInMB</span><span class="sxs-lookup"><span data-stu-id="0f0a2-169">MemoryReservationInMB</span></span>  
* <span data-ttu-id="0f0a2-170">BlkioWeight (BlockIO relatív súly).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-170">BlkioWeight (BlockIO relative weight).</span></span>

> [!NOTE]
> <span data-ttu-id="0f0a2-171">Egy későbbi kiadásban támogatja a megadott blokk IO korlátok iops-érték, például megadása olvasási/írási bit/másodperc, és másokkal is fog szerepelni.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-171">In a future release, support for specifying specific block IO limits such as IOPs, read/write BPS, and others will be included.</span></span>
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

## <a name="authenticate-a-repository"></a><span data-ttu-id="0f0a2-172">A tárház hitelesítéséhez</span><span class="sxs-lookup"><span data-stu-id="0f0a2-172">Authenticate a repository</span></span>
<span data-ttu-id="0f0a2-173">Egy tároló letöltéséhez, lehetséges, hogy bejelentkezési hitelesítő adatok a tároló tárházba.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-173">To download a container, you might have to provide sign-in credentials to the container repository.</span></span> <span data-ttu-id="0f0a2-174">Adja meg a bejelentkezési adatait, vagy az SSH-kulcsot a tároló lemezkép letöltése a lemezképtárból a bejelentkezéshez megadott hitelesítő adatokat, az alkalmazás jegyzékében szolgálnak.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-174">The sign-in credentials, specified in the application manifest, are used to specify the sign-in information, or SSH key, for downloading the container image from the image repository.</span></span> <span data-ttu-id="0f0a2-175">A következő példa bemutatja nevű *tesztfelhasználó néven* és a jelszó nyílt szövegként (*nem* ajánlott):</span><span class="sxs-lookup"><span data-stu-id="0f0a2-175">The following example shows an account called *TestUser* along with the password in clear text (*not* recommended):</span></span>

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

<span data-ttu-id="0f0a2-176">Azt javasoljuk, hogy a gépre telepített tanúsítvány használatával titkosítja a jelszót.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-176">We recommend that you encrypt the password by using a certificate that's deployed to the machine.</span></span>

<span data-ttu-id="0f0a2-177">A következő példa bemutatja nevű *tesztfelhasználó néven*, ahol a jelszó nevű tanúsítvánnyal titkosított *MyCert*.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-177">The following example shows an account called *TestUser*, where the password was encrypted by using a certificate called *MyCert*.</span></span> <span data-ttu-id="0f0a2-178">Használhatja a `Invoke-ServiceFabricEncryptText` PowerShell-parancsot a jelszót a titkos titkosítási szöveg létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-178">You can use the `Invoke-ServiceFabricEncryptText` PowerShell command to create the secret cipher text for the password.</span></span> <span data-ttu-id="0f0a2-179">További információkért lásd: a cikk [Service Fabric-alkalmazások a titkos kulcsok kezelése](service-fabric-application-secret-management.md).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-179">For more information, see the article [Managing secrets in Service Fabric applications](service-fabric-application-secret-management.md).</span></span>

<span data-ttu-id="0f0a2-180">A tanúsítvány használatával fejthetők vissza a jelszót a titkos kulcsot egy sávon kívüli módszert a helyi gépre kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-180">The private key of the certificate that's used to decrypt the password must be deployed to the local machine in an out-of-band method.</span></span> <span data-ttu-id="0f0a2-181">(Az Azure-ban Ez a módszer az Azure Resource Manager.) Majd amikor a Service Fabric telepíti a szolgáltatáscsomagnak a gép, azt vissza tudja fejteni a titkos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-181">(In Azure, this method is Azure Resource Manager.) Then, when Service Fabric deploys the service package to the machine, it can decrypt the secret.</span></span> <span data-ttu-id="0f0a2-182">A fiók nevét és a titkos kulcs használatával, majd hitelesítheti a tároló tárházban.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-182">By using the secret along with the account name, it can then authenticate with the container repository.</span></span>

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

## <a name="configure-container-port-to-host-port-mapping"></a><span data-ttu-id="0f0a2-183">Tároló port-állomás porthozzárendelés konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0f0a2-183">Configure container port-to-host port mapping</span></span>
<span data-ttu-id="0f0a2-184">Egy megadásával a tárolóhoz való kommunikációhoz használt állomás port is beállítható egy `PortBinding` az alkalmazásjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-184">You can configure a host port used to communicate with the container by specifying a `PortBinding` in the application manifest.</span></span> <span data-ttu-id="0f0a2-185">A port kötés leképezi a portot, amelyre a szolgáltatás figyeli-e a tárolót, hogy a portot a gazdagépen belül.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-185">The port binding maps the port to which the service is listening inside the container to a port on the host.</span></span>

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

## <a name="configure-container-to-container-discovery-and-communication"></a><span data-ttu-id="0f0a2-186">Tároló-tároló felderítése és a kommunikáció konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0f0a2-186">Configure container-to-container discovery and communication</span></span>
<span data-ttu-id="0f0a2-187">Használatával a `PortBinding` házirend, a tároló port hozzárendelhető egy `Endpoint` a szolgáltatás jegyzékben.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-187">By using the `PortBinding` policy, you can map a container port to an `Endpoint` in the service manifest.</span></span> <span data-ttu-id="0f0a2-188">A végpont `Endpoint1` adhat meg egy rögzített port (például a 80-as port).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-188">The endpoint `Endpoint1` can specify a fixed port (for example, port 80).</span></span> <span data-ttu-id="0f0a2-189">Azt is megadhatja, nincs port minden, ebben az esetben a fürt alkalmazás porttartományát a véletlenszerű port meg van kiválasztva.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-189">It can also specify no port at all, in which case a random port from the cluster's application port range is chosen for you.</span></span>

<span data-ttu-id="0f0a2-190">Ha megad egy végpontot, használja a `Endpoint` címkét a szolgáltatás jegyzékfájlban Vendég-tároló, a Service Fabric automatikusan közzéteheti a végpont a Naming service.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-190">If you specify an endpoint, using the `Endpoint` tag in the service manifest of a guest container, Service Fabric can automatically publish this endpoint to the Naming service.</span></span> <span data-ttu-id="0f0a2-191">A fürtben futó egyéb szolgáltatások így képes felderíteni a tárolót a REST-lekérdezésekkel kapcsolatos.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-191">Other services that are running in the cluster can thus discover this container using the REST queries for resolving.</span></span>

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

<span data-ttu-id="0f0a2-192">Ha regisztrálja a Naming Service, egyszerűen elvégezhető tároló-tároló kommunikációs a kódban a tárolóban használatával a [fordított proxy](service-fabric-reverseproxy.md).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-192">By registering with the Naming service, you can easily do container-to-container communication in the code within your container by using the [reverse proxy](service-fabric-reverseproxy.md).</span></span> <span data-ttu-id="0f0a2-193">Kommunikáció a fordított proxy figyelő http-port és a szolgáltatások környezeti változóként folytatott kommunikációhoz használni kívánt név megadásával történik.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-193">Communication is performed by providing the reverse proxy http listening port and the name of the services that you want to communicate with as environment variables.</span></span> <span data-ttu-id="0f0a2-194">További információkért tekintse meg a következő szakaszban.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-194">For more information, see the next section.</span></span>

## <a name="configure-and-set-environment-variables"></a><span data-ttu-id="0f0a2-195">Környezeti változók konfigurálása és beállítása</span><span class="sxs-lookup"><span data-stu-id="0f0a2-195">Configure and set environment variables</span></span>
<span data-ttu-id="0f0a2-196">Környezeti változók minden szolgáltatás jegyzékben, mind a tárolók üzembe helyezett szolgáltatások vagy szolgáltatások, folyamatok/Vendég végrehajtható fájlok telepített kódcsomag adható meg.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-196">Environment variables can be specified for each code package in the service manifest, both for services that are deployed in containers or for services that are deployed as processes/guest executables.</span></span> <span data-ttu-id="0f0a2-197">A környezeti változók értékeinek kifejezetten az alkalmazásjegyzékben szereplő felül, vagy alkalmazás paraméterekként telepítés során megadott.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-197">These environment variable values can be overridden specifically in the application manifest or specified during deployment as application parameters.</span></span>

<span data-ttu-id="0f0a2-198">A következő szolgáltatásjegyzékbeli XML-kódrészlet arra mutat be egy példát, hogyan adhat meg környezeti változókat egy kódcsomaghoz:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-198">The following service manifest XML snippet shows an example of how to specify environment variables for a code package:</span></span>

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

<span data-ttu-id="0f0a2-199">Ezek a környezeti változók az alkalmazás jegyzékének szintjén felülbírálható lesz:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-199">These environment variables can be overridden at the application manifest level:</span></span>

```xml
    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>
```

<span data-ttu-id="0f0a2-200">Az előző példában azt meg explicit érték a `HttpGateway` környezeti változó (19000), miközben hivatott értéke `BackendServiceName` keresztül paraméter a `[BackendSvc]` alkalmazás paraméter.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-200">In the previous example, we specified an explicit value for the `HttpGateway` environment variable (19000), while we set the value for `BackendServiceName` parameter via the `[BackendSvc]` application parameter.</span></span> <span data-ttu-id="0f0a2-201">Ezek a beállítások lehetővé teszik az értéket adja meg `BackendServiceName`értéke, ha az alkalmazás központi telepítése, és nem rendelkezik rögzített érték a jegyzékfájlban.</span><span class="sxs-lookup"><span data-stu-id="0f0a2-201">These settings enable you to specify the value for `BackendServiceName`value when you deploy the application and not have a fixed value in the manifest.</span></span>

## <a name="complete-examples-for-application-and-service-manifest"></a><span data-ttu-id="0f0a2-202">Fejezze be az alkalmazás és szolgáltatás jegyzékfájl példák</span><span class="sxs-lookup"><span data-stu-id="0f0a2-202">Complete examples for application and service manifest</span></span>

<span data-ttu-id="0f0a2-203">A következő példa alkalmazásjegyzéket:</span><span class="sxs-lookup"><span data-stu-id="0f0a2-203">An example application manifest follows:</span></span>

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

<span data-ttu-id="0f0a2-204">A következő egy példa service jegyzékfájl (az előző alkalmazásjegyzékben megadott):</span><span class="sxs-lookup"><span data-stu-id="0f0a2-204">An example service manifest (specified in the preceding application manifest) follows:</span></span>

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

## <a name="next-steps"></a><span data-ttu-id="0f0a2-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0f0a2-205">Next steps</span></span>
<span data-ttu-id="0f0a2-206">Most, hogy indexelése szolgáltatásként telepített, akkor útmutató elolvasásával életciklus kezeléséhez [Service Fabric-alkalmazás életciklusa](service-fabric-application-lifecycle.md).</span><span class="sxs-lookup"><span data-stu-id="0f0a2-206">Now that you have deployed a containerized service, learn how to manage its lifecycle by reading [Service Fabric application lifecycle](service-fabric-application-lifecycle.md).</span></span>

* [<span data-ttu-id="0f0a2-207">A Service Fabric és a tárolók áttekintése</span><span class="sxs-lookup"><span data-stu-id="0f0a2-207">Overview of Service Fabric and containers</span></span>](service-fabric-containers-overview.md)
* [<span data-ttu-id="0f0a2-208">Service Fabric-fürtökkel folytatott interakció az Azure parancssori felületének használatával</span><span class="sxs-lookup"><span data-stu-id="0f0a2-208">Interacting with Service Fabric clusters using the Azure CLI</span></span>](service-fabric-azure-cli.md)

<!-- Images -->
[sf-yeoman]: ./media/service-fabric-deploy-container-linux/sf-container-yeoman1.png

## <a name="related-articles"></a><span data-ttu-id="0f0a2-209">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="0f0a2-209">Related articles</span></span>

* [<span data-ttu-id="0f0a2-210">A Service Fabric első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="0f0a2-210">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
* [<span data-ttu-id="0f0a2-211">Első lépések a Service Fabric XPlat CLI használatával</span><span class="sxs-lookup"><span data-stu-id="0f0a2-211">Getting started with Service Fabric XPlat CLI</span></span>](service-fabric-azure-cli.md)
