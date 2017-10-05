---
title: "Ismerkedés az Azure Service Fabric XPlat parancssori felület"
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
ms.openlocfilehash: ddf881f6c202a82a3f64773639aa29b660057f8d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-xplat-cli-to-interact-with-a-service-fabric-cluster"></a><span data-ttu-id="cf9dc-103">A XPlat parancssori felület használatával kommunikál a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="cf9dc-103">Using the XPlat CLI to interact with a Service Fabric cluster</span></span>

<span data-ttu-id="cf9dc-104">Service Fabric-fürt Linux gépekről Linux rendszeren a XPlat parancssori felület használatával kommunikálhat.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-104">You can interact with Service Fabric cluster from Linux machines using the XPlat CLI on Linux.</span></span>

<span data-ttu-id="cf9dc-105">Az első lépés get a git-rep a CLI legújabb verzióját, és állítsa be az elérési úthoz a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-105">The first step is get the latest version of the CLI from the git rep and set it up in your path using the following commands:</span></span>

```sh
 git clone https://github.com/Azure/azure-xplat-cli.git
 cd azure-xplat-cli
 npm install
 sudo ln -s \$(pwd)/bin/azure /usr/bin/azure
 azure servicefabric
```

<span data-ttu-id="cf9dc-106">Az egyes parancsok az támogatja-e, adhatja meg az adott parancshoz tartozó súgó a parancs nevét.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-106">For each command, it supports, you can type the name of the command to obtain the help for that command.</span></span>
<span data-ttu-id="cf9dc-107">Automatikus-kiegészítéshez a parancsok használata támogatott.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-107">Auto-completion is supported for the commands.</span></span> <span data-ttu-id="cf9dc-108">Például a következő parancs lehetővé teszi az súgó összes alkalmazás parancs.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-108">For example, the following command gives you help for all the application commands.</span></span> 

```sh
 azure servicefabric application 
```

<span data-ttu-id="cf9dc-109">További szűrheti a Súgó gombra egy adott parancs az alábbi példában látható módon:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-109">You can further filter the help to a specific command, as the following example shows:</span></span>

```sh
 azure servicefabric application  create
```

<span data-ttu-id="cf9dc-110">Ahhoz, hogy az automatikus-kiegészítés a CLI-t, a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-110">To enable auto-completion in the CLI, run the following commands:</span></span>

```sh
azure --completion >> ~/azure.completion.sh
echo 'source ~/azure.completion.sh' >> ~/.sh\_profile
source ~/azure.completion.sh
```

<span data-ttu-id="cf9dc-111">A következő parancsok csatlakozzon a fürthöz, és jelenjen meg a fürt a csomópontok:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-111">The following commands connect to the cluster and show you the nodes in the cluster:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric node show
```

<span data-ttu-id="cf9dc-112">Az elnevezett paramétert használja, és keresse a definíció, beírhatja--súgójának parancs után.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-112">To use named parameters, and find what they are, you can type --help after a command.</span></span> <span data-ttu-id="cf9dc-113">Példa:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-113">For example:</span></span>

```sh
 azure servicefabric node show --help
 azure servicefabric application create --help
```

<span data-ttu-id="cf9dc-114">A gépről egy többgépes fürtben való csatlakozáskor **, amely nem része a fürtnek**, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-114">When connecting to a multi-machine cluster from a machine **that is not part of the cluster**, use the following command:</span></span>

```sh
 azure servicefabric cluster connect http://PublicIPorFQDN:19080
```

<span data-ttu-id="cf9dc-115">Cserélje le a PublicIPorFQDN címke a valódi IP vagy FQDN szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-115">Replace the PublicIPorFQDN tag with the real IP or FQDN as appropriate.</span></span> <span data-ttu-id="cf9dc-116">A gépről egy többgépes fürtben való csatlakozáskor **részét képező a fürt**, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-116">When connecting to a multi-machine cluster from a machine **that is part of the cluster**, use the following command:</span></span>

```sh
 azure servicefabric cluster connect --connection-endpoint http://localhost:19080 --client-connection-endpoint PublicIPorFQDN:19000
```

<span data-ttu-id="cf9dc-117">A PowerShell segítségével, vagy a parancssori felület, amellyel kommunikálni tud a Linux Service Fabric-fürt létrehozása az Azure portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-117">You can use PowerShell or CLI to interact with your Linux Service Fabric Cluster created through the Azure portal.</span></span>

> [!WARNING]
> <span data-ttu-id="cf9dc-118">Ezeken a fürtökön nem biztonságos, így, akkor előfordulhat, hogy lehet megnyitása az egy beépített adja hozzá a nyilvános IP-cím a fürtjegyzékben.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-118">These clusters aren’t secure, thus, you may be opening up your one-box by adding the public IP address in the cluster manifest.</span></span>

## <a name="using-the-xplat-cli-to-connect-to-a-service-fabric-cluster"></a><span data-ttu-id="cf9dc-119">A XPlat parancssori felület használatával kapcsolódni a Service Fabric-fürt</span><span class="sxs-lookup"><span data-stu-id="cf9dc-119">Using the XPlat CLI to connect to a Service Fabric cluster</span></span>

<span data-ttu-id="cf9dc-120">A következő Azure parancssori felület parancsait azt ismertetik, hogyan kapcsolódik egy biztonságos fürthöz.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-120">The following Azure CLI commands describe how to connect to a secure cluster.</span></span> <span data-ttu-id="cf9dc-121">A tanúsítvány részleteinél meg kell egyeznie a fürtcsomópontokon egy tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-121">The certificate details must match a certificate on the cluster nodes.</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert
```

<span data-ttu-id="cf9dc-122">Ha a tanúsítvány rendelkezik tanúsítványszolgáltatók (CA), adja hozzá a--ca-tanúsítvány-path paramétert, az alábbi példához hasonló szeretné:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-122">If your certificate has Certificate Authorities (CAs), you need to add the --ca-cert-path parameter like the following example:</span></span> 

```sh
 azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --ca-cert-path /tmp/ca1,/tmp/ca2 
```

<span data-ttu-id="cf9dc-123">Ha több hitelesítésszolgáltató, használják az elválasztó vessző.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-123">If you have multiple CAs, use a comma as the delimiter.</span></span>

<span data-ttu-id="cf9dc-124">Ha a tanúsítvány a köznapi név nem egyezik meg a csatlakozási végpont, használhatja a paraméter `--strict-ssl-false` az ellenőrzés elkerülésére, ahogy az az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-124">If your Common Name in the certificate does not match the connection endpoint, you could use the parameter `--strict-ssl-false` to bypass the verification as shown in the following command:</span></span>

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --strict-ssl-false 
```

<span data-ttu-id="cf9dc-125">Ha szeretné a CA-ellenőrzés kihagyása, hozzáadhatja a--utasítsa el a nem engedélyezett hamis paraméter, ahogy az az alábbi parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-125">If you would like to skip the CA verification, you could add the --reject-unauthorized-false parameter as shown in the following command:</span></span> 

```sh
azure servicefabric cluster connect --connection-endpoint http://ip:19080 --client-key-path /tmp/key --client-cert-path /tmp/cert --reject-unauthorized-false 
```

<span data-ttu-id="cf9dc-126">A kapcsolódás után kell tudni futtatni, amellyel kommunikálni tud a fürt más parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-126">After you connect, you should be able to run other CLI commands to interact with the cluster.</span></span>

## <a name="deploying-your-service-fabric-application"></a><span data-ttu-id="cf9dc-127">A Service Fabric-alkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="cf9dc-127">Deploying your Service Fabric application</span></span>

<span data-ttu-id="cf9dc-128">A következő parancsok futtatásával másolja, regisztrálja, és indítsa el a service fabric-alkalmazás hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-128">Execute the following commands to copy, register, and start the service fabric application:</span></span>

```sh
azure servicefabric application package copy [applicationPackagePath] [imageStoreConnectionString] [applicationPathInImageStore]
azure servicefabric application type register [applicationPathinImageStore]
azure servicefabric application create [applicationName] [applicationTypeName] [applicationTypeVersion]
```

## <a name="upgrading-your-application"></a><span data-ttu-id="cf9dc-129">Az alkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="cf9dc-129">Upgrading your application</span></span>

<span data-ttu-id="cf9dc-130">A folyamat hasonlít a [folyamatok Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span><span class="sxs-lookup"><span data-stu-id="cf9dc-130">The process is similar to the [process in Windows](service-fabric-application-upgrade-tutorial-powershell.md)).</span></span>

<span data-ttu-id="cf9dc-131">Build, másolása, regisztrálja és az alkalmazás létrehozása a projekt gyökérkönyvtárában.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-131">Build, copy, register, and create your application from project root directory.</span></span> <span data-ttu-id="cf9dc-132">Ha az alkalmazás-példány neve `fabric:/MySFApp`, és a típus MySFApp, a parancs a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-132">If your application instance is named `fabric:/MySFApp`, and the type is MySFApp, the commands would be as follows:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
 azure servicefabric application create fabric:/MySFApp MySFApp 1.0
```

<span data-ttu-id="cf9dc-133">Végezze el a módosítást az alkalmazáson, és építse újra a módosított szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-133">Make the change to your application and rebuild the modified service.</span></span>  <span data-ttu-id="cf9dc-134">A módosított service manifest-fájl (ServiceManifest.xml) frissítése a frissített verziókat a szolgáltatás (és kód vagy Config vagy adatokat szükség szerint).</span><span class="sxs-lookup"><span data-stu-id="cf9dc-134">Update the modified service’s manifest file (ServiceManifest.xml) with the updated versions for the Service (and Code or Config or Data as appropriate).</span></span> <span data-ttu-id="cf9dc-135">Az alkalmazás és a módosított szolgáltatást a frissített verziószámú is módosíthatja a az alkalmazás jegyzékében (ApplicationManifest.xml).</span><span class="sxs-lookup"><span data-stu-id="cf9dc-135">Also modify the application’s manifest (ApplicationManifest.xml) with the updated version number for the application, and the modified service.</span></span>  <span data-ttu-id="cf9dc-136">Most másolja, és regisztrálja a frissített alkalmazás a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-136">Now, copy and register your updated application using the following commands:</span></span>

```sh
 azure servicefabric cluster connect http://localhost:19080>
 azure servicefabric application package copy MySFApp fabric:ImageStore
 azure servicefabric application type register MySFApp
```

<span data-ttu-id="cf9dc-137">Az alkalmazásfrissítés most már elindíthatja a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-137">Now, you can start the application upgrade with the following command:</span></span>

```sh
 azure servicefabric application upgrade start -–application-name fabric:/MySFApp -–target-application-type-version 2.0 --rolling-upgrade-mode UnmonitoredAuto
```

<span data-ttu-id="cf9dc-138">Az alkalmazásfrissítés SFX használatával figyelheti.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-138">You can now monitor the application upgrade using SFX.</span></span> <span data-ttu-id="cf9dc-139">Az alkalmazás néhány percen belül frissült volna.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-139">In a few minutes, the application would have been updated.</span></span> <span data-ttu-id="cf9dc-140">Hiba történt a frissített alkalmazást is, és ellenőrizze az automatikus visszagörgetésre, ha a service fabric.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-140">You can also try an updated app with an error and check the auto rollback functionality in service fabric.</span></span>

## <a name="converting-from-pfx-to-pem-and-vice-versa"></a><span data-ttu-id="cf9dc-141">PEM, és ez fordítva is igaz PFX-ból</span><span class="sxs-lookup"><span data-stu-id="cf9dc-141">Converting from PFX to PEM and vice versa</span></span>

<span data-ttu-id="cf9dc-142">Szükség lehet a tanúsítvány telepítése a helyi számítógépen (a Windows vagy Linux), amely a másik lehetséges, hogy nem biztonságos fürtök eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-142">You might need to install a certificate in your local machine (with Windows or Linux) to access secure clusters that may be in a different environment.</span></span> <span data-ttu-id="cf9dc-143">Például egy Windows-gépen, és ez fordítva is igaz biztonságos Linux fürt használata közben: szükség lehet a tanúsítvány próbaverzióról PFX PEM, és ez fordítva is igaz.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-143">For example, while accessing a secured Linux cluster from a Windows machine and vice versa you may need to convert your certificate from PFX to PEM and vice versa.</span></span> 

<span data-ttu-id="cf9dc-144">A PEM-fájl át az PFX-fájlba, a következő paranccsal:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-144">To convert from a PEM file to a PFX file, use the following command:</span></span>

```bash
openssl pkcs12 -export -out certificate.pfx -inkey mycert.pem -in mycert.pem -certfile mycert.pem
```

<span data-ttu-id="cf9dc-145">PFX-fájl a következő paranccsal konvertálható PEM fájllá:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-145">To convert from a PFX file to a PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="cf9dc-146">Részletes információkat az [OpenSSL-dokumentációban](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) talál.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-146">Refer to [OpenSSL documentation](https://www.openssl.org/docs/man1.0.1/apps/pkcs12.html) for details.</span></span>

<a id="troubleshooting"></a>

## <a name="troubleshooting"></a><span data-ttu-id="cf9dc-147">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="cf9dc-147">Troubleshooting</span></span>


### <a name="copying-of-the-application-package-does-not-succeed"></a><span data-ttu-id="cf9dc-148">Az alkalmazás csomag másolása sikertelen</span><span class="sxs-lookup"><span data-stu-id="cf9dc-148">Copying of the application package does not succeed</span></span>

<span data-ttu-id="cf9dc-149">Ellenőrizze, hogy `openssh` telepítve van.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-149">Check if `openssh` is installed.</span></span> <span data-ttu-id="cf9dc-150">Alapértelmezés szerint Ubuntu asztali nincs telepítve.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-150">By default, Ubuntu Desktop doesn't have it installed.</span></span> <span data-ttu-id="cf9dc-151">Telepítse a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-151">Install it using the following command:</span></span>

```sh
sudo apt-get install openssh-server openssh-client**
```

<span data-ttu-id="cf9dc-152">Ha a probléma továbbra is fennáll, próbálja letiltása a PAM ssh módosításával a `sshd_config` fájlt a következő parancsokkal:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-152">If the problem persists, try disabling PAM for ssh by changing the `sshd_config` file using the following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd_config
#Change the line with UsePAM to the following: UsePAM no
sudo service sshd reload
```

<span data-ttu-id="cf9dc-153">Ha a probléma továbbra is fennáll, próbálja növelni a száma ssh munkamenetek a következő parancsok végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="cf9dc-153">If the problem still persists, try increasing the number of ssh sessions by executing the following commands:</span></span>

```sh
sudo vi /etc/ssh/sshd\_config
# Add the following to lines:
# MaxSessions 500
# MaxStartups 300:30:500
sudo service sshd reload
```

<span data-ttu-id="cf9dc-154">Kulcsok használata az ssh hitelesítés (jelszó) szemben még nem támogatott (mivel a platform használatával ssh csomagok másolása), ezért használja inkább a jelszó-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-154">Using keys for ssh authentication (as opposed to passwords) isn't yet supported (since the platform uses ssh to copy packages), so use password authentication instead.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cf9dc-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cf9dc-155">Next steps</span></span>

[<span data-ttu-id="cf9dc-156">Állítsa be a fejlesztési környezetet, és a Service Fabric-alkalmazás egy Linux-fürt telepítése.</span><span class="sxs-lookup"><span data-stu-id="cf9dc-156">Set up the development environment and deploy a Service Fabric application to a Linux cluster.</span></span>](service-fabric-get-started-linux.md)

## <a name="related-articles"></a><span data-ttu-id="cf9dc-157">Kapcsolódó cikkek</span><span class="sxs-lookup"><span data-stu-id="cf9dc-157">Related articles</span></span>

* [<span data-ttu-id="cf9dc-158">A Service Fabric első lépései az Azure CLI 2.0 használatával</span><span class="sxs-lookup"><span data-stu-id="cf9dc-158">Getting started with Service Fabric and Azure CLI 2.0</span></span>](service-fabric-azure-cli-2-0.md)
