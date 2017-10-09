---
title: "aaaGet lépések az Azure Service Fabric CLI (sfctl)"
description: "Ismerje meg, hogyan toouse hello Azure Service Fabric parancssori felület. Megtudhatja, hogyan tooconnect tooa fürt, és hogyan toomanage alkalmazások."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: f76e8ff65bb38dfb63791da0a23e19b93b337f6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-command-line"></a><span data-ttu-id="7cbdf-104">Azure Service Fabric-parancssor</span><span class="sxs-lookup"><span data-stu-id="7cbdf-104">Azure Service Fabric command line</span></span>

<span data-ttu-id="7cbdf-105">hello Azure Service Fabric CLI (sfctl) parancssori segédprogram, amely használják, és kezelése az Azure Service Fabric entitások.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-105">hello Azure Service Fabric CLI (sfctl) is a command-line utility for interacting and managing Azure Service Fabric entities.</span></span> <span data-ttu-id="7cbdf-106">Az Sfctl Windows- vagy Linux-fürtökön is használható.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-106">Sfctl can be used with either Windows or Linux clusters.</span></span> <span data-ttu-id="7cbdf-107">Az Sfctl bármilyen platformon fut, amely támogatja a Pythont.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-107">Sfctl runs on any platform where python is supported.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7cbdf-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7cbdf-108">Prerequisites</span></span>

<span data-ttu-id="7cbdf-109">Előzetes tooinstallation, ellenőrizze, hogy a környezet python és a pip telepítve rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-109">Prior tooinstallation, make sure your environment has both python and pip installed.</span></span> <span data-ttu-id="7cbdf-110">További információkért tekintse meg hello [pip gyors üzembe helyezési dokumentációja](https://pip.pypa.io/en/latest/quickstart/), és hivatalos [python telepítése dokumentáció](https://wiki.python.org/moin/BeginnersGuide/Download).</span><span class="sxs-lookup"><span data-stu-id="7cbdf-110">For more information, take a look at hello [pip quickstart documentation](https://pip.pypa.io/en/latest/quickstart/), and official [python install documentation](https://wiki.python.org/moin/BeginnersGuide/Download).</span></span>

<span data-ttu-id="7cbdf-111">Mindkét python 2.7-es és 3.6 támogatottak, ajánlott toouse python 3.6.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-111">While both python 2.7 and 3.6 are supported, it is recommended toouse python 3.6.</span></span>

## <a name="install"></a><span data-ttu-id="7cbdf-112">Telepítés</span><span class="sxs-lookup"><span data-stu-id="7cbdf-112">Install</span></span>

<span data-ttu-id="7cbdf-113">hello Azure Service Fabric CLI (sfctl) python-csomagként van csomagolva.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-113">hello Azure Service Fabric CLI (sfctl) is packaged as a python package.</span></span> <span data-ttu-id="7cbdf-114">tooinstall hello legfrissebb verzióját futtassa:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-114">tooinstall hello latest version run:</span></span>

```bash
pip install sfctl
```

<span data-ttu-id="7cbdf-115">A telepítés után futtassa `sfctl -h` elérhető parancsok tooget információt.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-115">After installation, run `sfctl -h` tooget information about available commands.</span></span>

## <a name="cli-syntax"></a><span data-ttu-id="7cbdf-116">A parancssori felület szintaxisa</span><span class="sxs-lookup"><span data-stu-id="7cbdf-116">CLI syntax</span></span>

<span data-ttu-id="7cbdf-117">A parancsok előtt mindig a következő előtag áll: `sfctl`.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-117">Commands are prefixed always with `sfctl`.</span></span> <span data-ttu-id="7cbdf-118">Az összes használható paranccsal kapcsolatos általános információkért használja az `sfctl -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-118">For general information about all commands you can use, use `sfctl -h`.</span></span> <span data-ttu-id="7cbdf-119">Ha egyetlen paranccsal kapcsolatban van szüksége segítségre, használja az `sfctl <command> -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-119">For help with a single command, use `sfctl <command> -h`.</span></span>

<span data-ttu-id="7cbdf-120">Parancsok hajtsa végre egy ismételhető struktúra, hello hello célja az előző hello műveletet vagy művelet parancsot:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-120">Commands follow a repeatable structure, with hello target of hello command preceding hello verb or action:</span></span>

```azurecli
sfctl <object> <action>
```

<span data-ttu-id="7cbdf-121">Ebben a példában `<object>` hello célja a `<action>`.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-121">In this example, `<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="7cbdf-122">Fürt kiválasztása</span><span class="sxs-lookup"><span data-stu-id="7cbdf-122">Select a cluster</span></span>

<span data-ttu-id="7cbdf-123">Mielőtt elvégezné a műveleteket, ki kell választania egy fürt tooconnect számára.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-123">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="7cbdf-124">Például futtassa a következő tooselect hello és csatlakoztassa a toohello fürtöt hello nevű `testcluster.com`.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-124">For example, run hello following tooselect and connect toohello cluster with hello name `testcluster.com`.</span></span>

> [!WARNING]
> <span data-ttu-id="7cbdf-125">Éles környezetben ne használjon nem védett Service Fabric-fürtöket.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-125">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="7cbdf-126">a fürt végpontja hello kell szerepelnie, hogy `http` vagy `https`.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-126">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="7cbdf-127">Hello port hello HTTP átjáró tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-127">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="7cbdf-128">hello portok és hello azonos a Service Fabric Explorer URL-cím hello történik.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-128">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="7cbdf-129">A tanúsítvánnyal védett fürtök esetében megadhat egy PEM-kódolású tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-129">For clusters that are secured with a certificate, you can specify a PEM encoded certificate.</span></span> <span data-ttu-id="7cbdf-130">egyetlen fájl vagy tanúsítvány-és kulcspárt hello tanúsítvány adható meg.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-130">hello certificate can be specified as a single file or cert and key pair.</span></span>

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="7cbdf-131">További információkért lásd: [Connect tooa biztonságos Azure Service Fabric-fürt](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="7cbdf-131">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

## <a name="basic-operations"></a><span data-ttu-id="7cbdf-132">Alapszintű műveletek</span><span class="sxs-lookup"><span data-stu-id="7cbdf-132">Basic operations</span></span>

<span data-ttu-id="7cbdf-133">A fürt kapcsolatadatai több sfctl-munkamenetben is megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-133">Cluster connection information persists across multiple sfctl sessions.</span></span> <span data-ttu-id="7cbdf-134">Miután kiválasztotta a Service Fabric-fürt, a Service Fabric-parancs hello fürtön is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-134">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="7cbdf-135">Tooget hello Service Fabric-fürt állapotát, például a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-135">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
sfctl cluster health
```

<span data-ttu-id="7cbdf-136">hello parancs hello a következő kimenetet eredményezi:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-136">hello command results in hello following output:</span></span>

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="7cbdf-137">Tippek és hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="7cbdf-137">Tips and troubleshooting</span></span>

<span data-ttu-id="7cbdf-138">Néhány javaslat és tipp a gyakori problémák megoldásához.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-138">Some suggestions and tips for solving common issues.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="7cbdf-139">Alakítsa át a tanúsítvány PFX tooPEM formátumból</span><span class="sxs-lookup"><span data-stu-id="7cbdf-139">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="7cbdf-140">Service Fabric CLI hello ügyféloldali tanúsítványokkal PEM (PEM kiterjesztésű) fájlokat támogatja.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-140">hello Service Fabric CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="7cbdf-141">A Windows a PFX-fájlokkal, ha ezen tanúsítványok tooPEM formátumban kell konvertálnia.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-141">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="7cbdf-142">tooconvert a PFX fájl tooa PEM-fájl, használja a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-142">tooconvert a PFX file tooa PEM file, use the following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="7cbdf-143">További információkért lásd: hello [OpenSSL-dokumentáció](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="7cbdf-143">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="7cbdf-144">Kapcsolódási problémák</span><span class="sxs-lookup"><span data-stu-id="7cbdf-144">Connection issues</span></span>

<span data-ttu-id="7cbdf-145">Egyes műveletek hozhat létre a következő üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-145">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="7cbdf-146">Győződjön meg arról, hogy hello megadva, a fürt végpontja rendelkezésre áll és figyelő.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-146">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="7cbdf-147">Azt is ellenőrizze, hogy hello Service Fabric Explorer felhasználói felület érhető el, amely állomás és port.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-147">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="7cbdf-148">tooupdate hello végpont használatát `sfctl cluster select`.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-148">tooupdate hello endpoint, use `sfctl cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="7cbdf-149">Részletes naplók</span><span class="sxs-lookup"><span data-stu-id="7cbdf-149">Detailed logs</span></span>

<span data-ttu-id="7cbdf-150">A részletes naplók gyakran hasznosak a hibák javításához vagy jelentéséhez.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-150">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="7cbdf-151">Nincs a globális `--debug` jelzőt, amely növeli a naplófájlok hello részletességi.</span><span class="sxs-lookup"><span data-stu-id="7cbdf-151">There is a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="7cbdf-152">Parancsok súgója és szintaxisa</span><span class="sxs-lookup"><span data-stu-id="7cbdf-152">Command help and syntax</span></span>

<span data-ttu-id="7cbdf-153">Egy adott parancs vagy parancsok egész csoportját, használatáról a hello `-h` jelző:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
sfctl application -h
```

<span data-ttu-id="7cbdf-154">Egy másik példa:</span><span class="sxs-lookup"><span data-stu-id="7cbdf-154">Another example:</span></span>

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a><span data-ttu-id="7cbdf-155">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="7cbdf-155">Next steps</span></span>

* [<span data-ttu-id="7cbdf-156">Hello Azure Service Fabric CLI az alkalmazás központi telepítését</span><span class="sxs-lookup"><span data-stu-id="7cbdf-156">Deploy an application with hello Azure Service Fabric CLI</span></span>](service-fabric-application-lifecycle-sfctl.md)
* [<span data-ttu-id="7cbdf-157">A Service Fabric használatának első lépései Linuxon</span><span class="sxs-lookup"><span data-stu-id="7cbdf-157">Get started with Service Fabric on Linux</span></span>](service-fabric-get-started-linux.md)
