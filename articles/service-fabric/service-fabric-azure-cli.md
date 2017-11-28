---
title: "az Azure Service Fabric XPlat parancssori felületen lépései aaaGetting"
description: "Ismerkedés az Azure Service Fabric XPlat parancssori felület"
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: c3ec8ff3-3b78-420c-a7ea-0c5e443fb10e
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/9/2017
ms.author: subramar
ms.openlocfilehash: e4baa30536b4d8668d8efad301ed8210eb9c0335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-xplat-cli-toointeract-with-a-service-fabric-cluster"></a><span data-ttu-id="670eb-103">A Service Fabric-fürt hello toointeract XPlat parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="670eb-103">Using hello XPlat CLI toointeract with a Service Fabric cluster</span></span>

<span data-ttu-id="670eb-104">Service Fabric-fürt Linux gépekről Linux hello XPlat parancssori felület használatával kommunikálhat.</span><span class="sxs-lookup"><span data-stu-id="670eb-104">You can interact with Service Fabric cluster from Linux machines using hello XPlat CLI on Linux.</span></span>

<span data-ttu-id="670eb-105">hello első lépése hello hello CLI legújabb verziója van beszerezni hello git rep és a készlet a következő parancsok hello azt fel az elérési út használatával:</span><span class="sxs-lookup"><span data-stu-id="670eb-105">hello first step is get hello latest version of hello CLI from hello git rep and set it up in your path using hello following commands:</span></span>

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

<span data-ttu-id="670eb-106">Az egyes parancsok az támogatja-e, adhatja meg, hogy a parancs hello hello parancs tooobtain hello súgó nevét.</span><span class="sxs-lookup"><span data-stu-id="670eb-106">For each command, it supports, you can type hello name of hello command tooobtain hello help for that command.</span></span>
<span data-ttu-id="670eb-107">Automatikus-végrehajtási hello parancsok esetén támogatott.</span><span class="sxs-lookup"><span data-stu-id="670eb-107">Auto-completion is supported for hello commands.</span></span> <span data-ttu-id="670eb-108">Például hello a következő parancs által biztosított összes hello alkalmazás parancs segítséget.</span><span class="sxs-lookup"><span data-stu-id="670eb-108">For example, hello following command gives you help for all hello application commands.</span></span> 

```sh
 azure servicefabric application 
```

<span data-ttu-id="670eb-109">További szűrheti hello súgó tooa adott parancs, mint hello a következő példa azt mutatja meg:</span><span class="sxs-lookup"><span data-stu-id="670eb-109">You can further filter hello help tooa specific command, as hello following example shows:</span></span>

```sh
 azure servicefabric application  create
```

<span data-ttu-id="670eb-110">tooenable automatikus-kiegészítéshez a hello CLI, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="670eb-110">tooenable auto-completion in hello CLI, run hello following commands:</span></span>

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

<span data-ttu-id="670eb-111">a következő parancsok hello csatlakozás toohello fürt és az Ön hello hello fürt csomópontja:</span><span class="sxs-lookup"><span data-stu-id="670eb-111">hello following commands connect toohello cluster and show you hello nodes in hello cluster:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

<span data-ttu-id="670eb-112">toouse elnevezett paramétereket, és keresse a definíció, beírhatja – Súgó parancs után.</span><span class="sxs-lookup"><span data-stu-id="670eb-112">toouse named parameters, and find what they are, you can type --help after a command.</span></span> <span data-ttu-id="670eb-113">Példa:</span><span class="sxs-lookup"><span data-stu-id="670eb-113">For example:</span></span>

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

<span data-ttu-id="670eb-114">A gép tooa többgépes fürtben kapcsolódáskor **nem részét képező hello fürt**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="670eb-114">When connecting tooa multi-machine cluster from a machine **that is not part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

<span data-ttu-id="670eb-115">Cserélje le hello PublicIPorFQDN címke hello valós IP-cím vagy FQDN szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="670eb-115">Replace hello PublicIPorFQDN tag with hello real IP or FQDN as appropriate.</span></span> <span data-ttu-id="670eb-116">A gép tooa többgépes fürtben kapcsolódáskor **részét képező hello fürt**, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="670eb-116">When connecting tooa multi-machine cluster from a machine **that is part of hello cluster**, use hello following command:</span></span>

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

<span data-ttu-id="670eb-117">A PowerShell segítségével, vagy a Linux Service Fabric-fürt a CLI toointeract létrehozott hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="670eb-117">You can use PowerShell or CLI toointeract with your Linux Service Fabric Cluster created through hello Azure portal.</span></span>

> [!WARNING]
> <span data-ttu-id="670eb-118">Ezeken a fürtökön nem biztonságos, így, akkor előfordulhat, hogy kell megnyitása az egy beépített hello fürtjegyzékben hello nyilvános IP-cím hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="670eb-118">These clusters aren’t secure, thus, you may be opening up your one-box by adding hello public IP address in hello cluster manifest.</span></span>

## <a name="using-hello-xplat-cli-tooconnect-tooa-service-fabric-cluster"></a><span data-ttu-id="670eb-119">Hello tooconnect tooa Service Fabric-fürt XPlat parancssori felület használatával</span><span class="sxs-lookup"><span data-stu-id="670eb-119">Using hello XPlat CLI tooconnect tooa Service Fabric cluster</span></span>

<span data-ttu-id="670eb-120">a következő Azure parancssori felület parancsait hello ismertetik, hogyan tooconnect tooa biztonságos fürt.</span><span class="sxs-lookup"><span data-stu-id="670eb-120">hello following Azure CLI commands describe how tooconnect tooa secure cluster.</span></span> <span data-ttu-id="670eb-121">hello Tanúsítványadatok hello fürtcsomópontokon tanúsítványt meg kell egyeznie.</span><span class="sxs-lookup"><span data-stu-id="670eb-121">hello certificate details must match a certificate on hello cluster nodes.</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

<span data-ttu-id="670eb-122">Ha a tanúsítvány tanúsítványszolgáltatók (CA), tooadd hello – a ca-tanúsítvány-elérési út paraméter a következő példa hello hasonlóan kell:</span><span class="sxs-lookup"><span data-stu-id="670eb-122">If your certificate has Certificate Authorities (CAs), you need tooadd hello --ca-cert-path parameter like hello following example:</span></span> 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

<span data-ttu-id="670eb-123">Ha több ügyfélcím, használják hello elválasztó vessző.</span><span class="sxs-lookup"><span data-stu-id="670eb-123">If you have multiple CAs, use a comma as hello delimiter.</span></span>

<span data-ttu-id="670eb-124">A hello tanúsítvány köznapi neve nem egyezik meg a hello csatlakozási végpont, használhatja hello paraméter `--strict-ssl-false` toobypass hello ellenőrzés, ahogy az a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="670eb-124">If your Common Name in hello certificate does not match hello connection endpoint, you could use hello parameter `--strict-ssl-false` toobypass hello verification as shown in hello following command:</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

<span data-ttu-id="670eb-125">Ha szeretné tooskip hello hitelesítésszolgáltató ellenőrzési, hozzáadhat hello – ahogy az a következő parancs hello utasítsa el a nem engedélyezett hamis paraméter:</span><span class="sxs-lookup"><span data-stu-id="670eb-125">If you would like tooskip hello CA verification, you could add hello --reject-unauthorized-false parameter as shown in hello following command:</span></span> 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

<span data-ttu-id="670eb-126">A kapcsolódás után meg kell tudni toorun más CLI parancsok toointeract a hello fürt.</span><span class="sxs-lookup"><span data-stu-id="670eb-126">After you connect, you should be able toorun other CLI commands toointeract with hello cluster.</span></span>

## <a name="deploying-your-service-fabric-application"></a><span data-ttu-id="670eb-127">A Service Fabric-alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="670eb-127">Deploying your Service Fabric application</span></span>

<span data-ttu-id="670eb-128">A következő parancsok toocopy, regisztrálja, és indítsa el a service fabric-alkalmazás hello hello hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="670eb-128">Execute hello following commands toocopy, register, and start hello service fabric application:</span></span>

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a><span data-ttu-id="670eb-129">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="670eb-129">Upgrading your application</span></span>

<span data-ttu-id="670eb-130">hello folyamatban a hasonló toohello [folyamatok Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span><span class="sxs-lookup"><span data-stu-id="670eb-130">hello process is similar toohello [process in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span></span>

<span data-ttu-id="670eb-131">Build, másolása, regisztrálja és az alkalmazás létrehozása a projekt gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="670eb-131">Build, copy, register, and create your application from project root directory.</span></span> <span data-ttu-id="670eb-132">Ha az alkalmazás-példány neve `fabric:/MySFApp`, és hello típus MySFApp, hello parancsok a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="670eb-132">If your application instance is named `fabric:/MySFApp`, and hello type is MySFApp, hello commands would be as follows:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

<span data-ttu-id="670eb-133">Ellenőrizze a hello tooyour alkalmazás módosítsa, majd újraépítése a módosított hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="670eb-133">Make hello change tooyour application and rebuild hello modified service.</span></span>  <span data-ttu-id="670eb-134">Frissítés hello módosított szolgáltatás jegyzékfájljában (ServiceManifest.xml) frissítése hello verzióival hello szolgáltatás (és kód vagy Config vagy adatokat szükség szerint).</span><span class="sxs-lookup"><span data-stu-id="670eb-134">Update hello modified service’s manifest file (ServiceManifest.xml) with hello updated versions for hello Service (and Code or Config or Data as appropriate).</span></span> <span data-ttu-id="670eb-135">Is frissíteni hello verziószámú hello alkalmazás hello alkalmazás jegyzékfájlja (ApplicationManifest.xml) módosítása és hello módosított szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="670eb-135">Also modify hello application’s manifest (ApplicationManifest.xml) with hello updated version number for hello application, and hello modified service.</span></span>  <span data-ttu-id="670eb-136">Most másolja, és regisztrálja az frissített alkalmazás hello a következő parancsok használatával:</span><span class="sxs-lookup"><span data-stu-id="670eb-136">Now, copy and register your updated application using hello following commands:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

<span data-ttu-id="670eb-137">Most hello az alkalmazásfrissítés indítható hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="670eb-137">Now, you can start hello application upgrade with hello following command:</span></span>

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

<span data-ttu-id="670eb-138">Az alkalmazásfrissítés hello SFX használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="670eb-138">You can now monitor hello application upgrade using SFX.</span></span> <span data-ttu-id="670eb-139">Néhány perc múlva hello alkalmazás lesz frissítve lett.</span><span class="sxs-lookup"><span data-stu-id="670eb-139">In a few minutes, hello application would have been updated.</span></span> <span data-ttu-id="670eb-140">Is hibával frissített alkalmazást, és ellenőrizze a hello automatikus visszaállítási funkciót az a service fabric.</span><span class="sxs-lookup"><span data-stu-id="670eb-140">You can also try an updated app with an error and check hello auto rollback functionality in service fabric.</span></span>

## <a name="converting-from-pfx-toopem-and-vice-versa"></a><span data-ttu-id="670eb-141">PFX-tooPEM, és ez fordítva is igaz alakítása</span><span class="sxs-lookup"><span data-stu-id="670eb-141">Converting from PFX tooPEM and vice versa</span></span>

<span data-ttu-id="670eb-142">A helyi számítógépen (a Windows vagy Linux) tooaccess biztonságos fürtök különböző környezetben esetlegesen szükség lehet tooinstall egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="670eb-142">You might need tooinstall a certificate in your local machine (with Windows or Linux) tooaccess secure clusters that may be in a different environment.</span></span> <span data-ttu-id="670eb-143">Például a Windows-gépről, és ez fordítva is igaz biztonságos Linux fürt használata közben esetleg tooconvert a tanúsítvány PFX tooPEM, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="670eb-143">For example, while accessing a secured Linux cluster from a Windows machine and vice versa you may need tooconvert your certificate from PFX tooPEM and vice versa.</span></span> 

<span data-ttu-id="670eb-144">PEM fájl tooa PFX-fájlból, a következő parancs használata hello tooconvert:</span><span class="sxs-lookup"><span data-stu-id="670eb-144">tooconvert from a PEM file tooa PFX file, use hello following command:</span></span>

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

<span data-ttu-id="670eb-145">PFX-fájlból egy fájl tooa PEM, a következő parancs használata hello tooconvert:</span><span class="sxs-lookup"><span data-stu-id="670eb-145">tooconvert from a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="670eb-146">Tekintse meg a túl[OpenSSL-dokumentáció](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="670eb-146">Refer too[OpenSSL documentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) for details.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a><span data-ttu-id="670eb-147">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="670eb-147">Troubleshooting</span></span>


### <a name="copying-of-hello-application-package-does-not-succeed"></a><span data-ttu-id="670eb-148">Hello alkalmazás csomag másolása sikertelen</span><span class="sxs-lookup"><span data-stu-id="670eb-148">Copying of hello application package does not succeed</span></span>

<span data-ttu-id="670eb-149">Ellenőrizze, hogy `openssh` telepítve van.</span><span class="sxs-lookup"><span data-stu-id="670eb-149">Check if `openssh` is installed.</span></span> <span data-ttu-id="670eb-150">Alapértelmezés szerint Ubuntu asztali nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="670eb-150">By default, Ubuntu Desktop doesn't have it installed.</span></span> <span data-ttu-id="670eb-151">Telepítse a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="670eb-151">Install it using hello following command:</span></span>

```sh
sudo apt-get install openssh-server openssh-client**
```

<span data-ttu-id="670eb-152">Ha hello a probléma továbbra is fennáll, próbálja letiltása a PAM ssh hello módosításával `sshd_config` fájl a következő parancsok hello használata:</span><span class="sxs-lookup"><span data-stu-id="670eb-152">If hello problem persists, try disabling PAM for ssh by changing hello `sshd_config` file using hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd_config
#Change hello line with UsePAM toohello following: UsePAM no
sudo service sshd reload
```

<span data-ttu-id="670eb-153">Ha hello a probléma továbbra is fennáll, próbálja meg egyre hello több ssh munkamenetek hello a következő parancsok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="670eb-153">If hello problem still persists, try increasing hello number of ssh sessions by executing hello following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd\_config
# Add hello following toolines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

<span data-ttu-id="670eb-154">Billentyűparancsok használata ssh hitelesítés (a megakadályozását toopasswords) még nem támogatott (mivel hello platform használ ssh toocopy csomagok), ezért használja inkább a jelszó-hitelesítés.</span><span class="sxs-lookup"><span data-stu-id="670eb-154">Using keys for ssh authentication (as opposed toopasswords) isn't yet supported (since hello platform uses ssh toocopy packages), so use password authentication instead.</span></span>

## <a name="next-steps"></a><span data-ttu-id="670eb-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="670eb-155">Next steps</span></span>

[<span data-ttu-id="670eb-156">Hello fejlesztési környezet beállítását, és a Service Fabric-alkalmazás tooa Linux-fürt üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="670eb-156">Set up hello development environment and deploy a Service Fabric application tooa Linux cluster.</span></span>](service-fabric-get-started-linux.md)

## <a name="related-articles"></a><span data-ttu-id="670eb-157">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="670eb-157">Related articles</span></span>

* [<span data-ttu-id="670eb-158">A Service Fabric első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="670eb-158">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
