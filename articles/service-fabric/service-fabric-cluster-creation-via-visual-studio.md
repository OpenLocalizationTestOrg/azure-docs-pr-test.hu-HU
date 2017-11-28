---
title: "Visual Studio használatával a Service Fabric fürt aaaSetting |} Microsoft Docs"
description: "Ismerteti, hogyan tooset mentése a Service Fabric fürt egy Azure erőforráscsoport-projektet a Visual Studio által létrehozott Azure Resource Manager-sablon használatával"
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: adegeo
editor: 
ms.assetid: bd2c0511-36c9-4828-8dc3-69e4b6a70567
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/21/2017
ms.author: mikhegn
redirect_url: /azure/service-fabric/service-fabric-cluster-creation-via-arm
ms.openlocfilehash: adb0dd2169a28b46e832c6f06c998cbed0c473f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a><span data-ttu-id="aa3a1-103">A Service Fabric-fürt beállítása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="aa3a1-103">Set up a Service Fabric cluster by using Visual Studio</span></span>
<span data-ttu-id="aa3a1-104">Ez a cikk ismerteti, hogyan tooset mentése az Azure Service Fabric fürt Visual Studio és az Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-104">This article describes how tooset up an Azure Service Fabric cluster by using Visual Studio and an Azure Resource Manager template.</span></span> <span data-ttu-id="aa3a1-105">A Visual Studio Azure resource csoport toocreate hello projektsablon használjuk.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-105">We will use a Visual Studio Azure resource group project toocreate hello template.</span></span> <span data-ttu-id="aa3a1-106">Hello sablon létrehozását követően központilag telepíthető közvetlenül a Visual Studio eszközből tooAzure.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-106">After hello template has been created, it can be deployed directly tooAzure from Visual Studio.</span></span> <span data-ttu-id="aa3a1-107">Azt is használható egy parancsfájlból származó, vagy a folyamatos integrációt (CI) létesítmény részeként.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-107">It can also be used from a script, or as part of continuous integration (CI) facility.</span></span>

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a><span data-ttu-id="aa3a1-108">A Service Fabric-fürt sablon az Azure erőforráscsoport-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="aa3a1-108">Create a Service Fabric cluster template by using an Azure resource group project</span></span>
<span data-ttu-id="aa3a1-109">tooget elindult, nyissa meg a Visual Studio és az Azure erőforráscsoport-projekt létrehozása (hello elérhető **felhő** mappa):</span><span class="sxs-lookup"><span data-stu-id="aa3a1-109">tooget started, open Visual Studio and create an Azure resource group project (it is available in hello **Cloud** folder):</span></span>

![Új projekt párbeszédpanel kijelölt Azure erőforráscsoport-projekt][1]

<span data-ttu-id="aa3a1-111">Hozzon létre egy új Visual Studio megoldás ebben a projektben, vagy adja hozzá tooan meglévő megoldással.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-111">You can create a new Visual Studio solution for this project or add it tooan existing solution.</span></span>

> [!NOTE]
> <span data-ttu-id="aa3a1-112">Ha nem látja hello Azure erőforráscsoport-projekt hello felhő csomópontja alatt, nincs hello Azure SDK telepítése.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-112">If you do not see hello Azure resource group project under hello Cloud node, you do not have hello Azure SDK installed.</span></span> <span data-ttu-id="aa3a1-113">Indítsa el a Webplatform-telepítővel ([most telepíteni](http://www.microsoft.com/web/downloads/platform.aspx) Ha még nem tette meg), majd keresse meg a "Azure SDK a .NET" és a telepítés hello verziója, amely kompatibilis a Visual Studio verziójának.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-113">Launch Web Platform Installer ([install it now](http://www.microsoft.com/web/downloads/platform.aspx) if you have not already), and then search for "Azure SDK for .NET" and install hello version that is compatible with your version of Visual Studio.</span></span>
> 
> 

<span data-ttu-id="aa3a1-114">Miután hello OK gombra kattint, a Visual Studio ekkor megkérdezi, hogy tooselect hello Resource Manager sablon toocreate:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-114">After you hit hello OK button, Visual Studio will ask you tooselect hello Resource Manager template you want toocreate:</span></span>

![Azure-sablon párbeszédpanelen válassza ki a kijelölt Service Fabric-fürt sablonnal][2]

<span data-ttu-id="aa3a1-116">Válassza ki újra hello Service Fabric-fürt sablon és a találati hello OK gombra.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-116">Select hello Service Fabric Cluster template and hit hello OK button again.</span></span> <span data-ttu-id="aa3a1-117">hello projekt és hello Resource Manager-sablon már létrejött.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-117">hello project and hello Resource Manager template have now been created.</span></span>

## <a name="prepare-hello-template-for-deployment"></a><span data-ttu-id="aa3a1-118">Hello sablon üzembe helyezésének előkészítése</span><span class="sxs-lookup"><span data-stu-id="aa3a1-118">Prepare hello template for deployment</span></span>
<span data-ttu-id="aa3a1-119">Mielőtt hello sablon telepített toocreate hello fürt, kell értékeket ad meg a szükséges hello Sablonparaméterek.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-119">Before hello template is deployed toocreate hello cluster, you must provide values for hello required template parameters.</span></span> <span data-ttu-id="aa3a1-120">A paraméterértékek pedig olvassa a hello `ServiceFabricCluster.parameters.json` fájl, amely hello `Templates` hello erőforráscsoport-projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-120">These parameter values are read from hello `ServiceFabricCluster.parameters.json` file, which is in hello `Templates` folder of hello resource group project.</span></span> <span data-ttu-id="aa3a1-121">Nyissa meg hello fájlt, és adja meg a következő értékek hello:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-121">Open hello file and provide hello following values:</span></span>

| <span data-ttu-id="aa3a1-122">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="aa3a1-122">Parameter name</span></span> | <span data-ttu-id="aa3a1-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="aa3a1-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="aa3a1-124">adminUserName</span><span class="sxs-lookup"><span data-stu-id="aa3a1-124">adminUserName</span></span> |<span data-ttu-id="aa3a1-125">hello hello rendszergazdai fiók nevét a Service Fabric gépek (csomópontok).</span><span class="sxs-lookup"><span data-stu-id="aa3a1-125">hello name of hello administrator account for Service Fabric machines (nodes).</span></span> |
| <span data-ttu-id="aa3a1-126">certificateThumbprint</span><span class="sxs-lookup"><span data-stu-id="aa3a1-126">certificateThumbprint</span></span> |<span data-ttu-id="aa3a1-127">hello biztosítja a hello fürt hello tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-127">hello thumbprint of hello certificate that secures hello cluster.</span></span> |
| <span data-ttu-id="aa3a1-128">sourceVaultResourceId</span><span class="sxs-lookup"><span data-stu-id="aa3a1-128">sourceVaultResourceId</span></span> |<span data-ttu-id="aa3a1-129">Hello *erőforrás-azonosító* a hello kulcstároló hello tanúsítvány, amely biztosítja a hello fürt tárolására.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-129">hello *resource ID* of hello key vault where hello certificate that secures hello cluster is stored.</span></span> |
| <span data-ttu-id="aa3a1-130">certificateUrlValue</span><span class="sxs-lookup"><span data-stu-id="aa3a1-130">certificateUrlValue</span></span> |<span data-ttu-id="aa3a1-131">hello hello fürt biztonsági tanúsítvány URL-címe</span><span class="sxs-lookup"><span data-stu-id="aa3a1-131">hello URL of hello cluster security certificate.</span></span> |

<span data-ttu-id="aa3a1-132">hello Visual Studio Service Fabric Resource Manager-sablon védi egy tanúsítványt egy biztonságos fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-132">hello Visual Studio Service Fabric Resource Manager template creates a secure cluster that is protected by a certificate.</span></span> <span data-ttu-id="aa3a1-133">Ez a tanúsítvány hello azonosíthatók az utolsó három Sablonparaméterek (`certificateThumbprint`, `sourceVaultValue`, és `certificateUrlValue`), és azt már léteznie kell egy **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-133">This certificate is identified by hello last three template parameters (`certificateThumbprint`, `sourceVaultValue`, and `certificateUrlValue`), and it must exist in an **Azure Key Vault**.</span></span> <span data-ttu-id="aa3a1-134">Hogyan toocreate hello fürt biztonsági tanúsítvány további információkért lásd: [Service Fabric-fürt biztonsági forgatókönyvek](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) cikk.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-134">For more information on how toocreate hello cluster security certificate, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) article.</span></span>

## <a name="optional-change-hello-cluster-name"></a><span data-ttu-id="aa3a1-135">Választható lehetőség: hello fürt nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="aa3a1-135">Optional: change hello cluster name</span></span>
<span data-ttu-id="aa3a1-136">Minden Service Fabric-fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-136">Every Service Fabric cluster has a name.</span></span> <span data-ttu-id="aa3a1-137">A Fabric-fürt létrehozása az Azure-ban fürt nevét határozza meg (együtt hello Azure-régió) hello fürt hello tartománynévrendszer (DNS) nevét.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-137">When a Fabric cluster is created in Azure, cluster name determines (together with hello Azure region) hello Domain Name System (DNS) name for hello cluster.</span></span> <span data-ttu-id="aa3a1-138">Például, ha a fürt nevezze el `myBigCluster`, és hello helyét (Azure-régió), amely helyt hello új fürt hello erőforráscsoport USA keleti régiója, hello fürt hello DNS-neve lesz `myBigCluster.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-138">For example, if you name your cluster `myBigCluster`, and hello location (Azure region) of hello resource group that will host hello new cluster is East US, hello DNS name of hello cluster will be `myBigCluster.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="aa3a1-139">Alapértelmezés szerint hello fürtnév automatikusan jönnek létre, és egyedi végzett véletlenszerű utótag tooa "fürt" előtag csatolásával.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-139">By default hello cluster name is generated automatically and made unique by attaching a random suffix tooa "cluster" prefix.</span></span> <span data-ttu-id="aa3a1-140">Ez teszi, hogy rendkívül könnyen toouse hello sablon részeként egy **folyamatos integrációt** (CI) rendszer.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-140">This makes it very easy toouse hello template as part of a **continuous integration** (CI) system.</span></span> <span data-ttu-id="aa3a1-141">Ha azt szeretné, hogy toouse egyedi nevet a fürtnek, ki, amely jelentéssel bíró tooyou, állítsa be hello hello `clusterName` változó hello Resource Manager sablon fájlban (`ServiceFabricCluster.json`) kiválasztott tooyour nevét.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-141">If you want toouse a specific name for your cluster, one that is meaningful tooyou, set hello value of hello `clusterName` variable in hello Resource Manager template file (`ServiceFabricCluster.json`) tooyour chosen name.</span></span> <span data-ttu-id="aa3a1-142">Hello első változót, az adott fájlban definiálva.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-142">It is hello first variable defined in that file.</span></span>

## <a name="optional-add-public-application-ports"></a><span data-ttu-id="aa3a1-143">Választható lehetőség: nyilvános alkalmazás-portok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="aa3a1-143">Optional: add public application ports</span></span>
<span data-ttu-id="aa3a1-144">Is érdemes lehet toochange hello nyilvános alkalmazás portok hello fürt telepítése előtt.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-144">You may also want toochange hello public application ports for hello cluster before you deploy it.</span></span> <span data-ttu-id="aa3a1-145">Alapértelmezés szerint hello sablon csak két nyilvános TCP-portot (80-as és 8081) megnyílik.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-145">By default, hello template opens up just two public TCP ports (80 and 8081).</span></span> <span data-ttu-id="aa3a1-146">Ha több, az alkalmazások, módosítsa a hello Azure Load Balancer definition hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-146">If you need more for your applications, modify hello Azure Load Balancer definition in hello template.</span></span> <span data-ttu-id="aa3a1-147">hello definition hello fő sablon fájl tárolja (`ServiceFabricCluster.json`).</span><span class="sxs-lookup"><span data-stu-id="aa3a1-147">hello definition is stored in hello main template file (`ServiceFabricCluster.json`).</span></span> <span data-ttu-id="aa3a1-148">Nyissa meg a fájlt, és keresse meg `loadBalancedAppPort`.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-148">Open that file and search for `loadBalancedAppPort`.</span></span> <span data-ttu-id="aa3a1-149">Minden port társítva három összetevők:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-149">Each port is associated with three artifacts:</span></span>

1. <span data-ttu-id="aa3a1-150">Hello port hello TCP port értékét meghatározó sablonváltozó:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-150">A template variable that defines hello TCP port value for hello port:</span></span>
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. <span data-ttu-id="aa3a1-151">A *mintavételi* , amely meghatározza, milyen gyakran és mennyi ideig hello Azure terheléselosztó toouse egy adott Service Fabric-csomópont mielőtt hibát jelentene megpróbál egy tooanother keresztül.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-151">A *probe* that defines how frequently and for how long hello Azure load balancer attempts toouse a specific Service Fabric node before failing over tooanother one.</span></span> <span data-ttu-id="aa3a1-152">hello mintavételt a Load Balancer erőforrás hello részét képezik.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-152">hello probes are part of hello Load Balancer resource.</span></span> <span data-ttu-id="aa3a1-153">Íme hello mintavételi definíciója hello első alapértelmezett portja:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-153">Here is hello probe definition for hello first default application port:</span></span>
   
    ```json
    {
        "name": "AppPortProbe1",
        "properties": {
            "intervalInSeconds": 5,
            "numberOfProbes": 2,
            "port": "[variables('loadBalancedAppPort1')]",
            "protocol": "Tcp"
        }
    }
    ```
3. <span data-ttu-id="aa3a1-154">A *terheléselosztási szabály* , amely kötelékek együtt hello port és hello mintavételi, mely lehetővé teszi, hogy terheléselosztás meg a Service Fabric-fürt csomópontok között:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-154">A *load-balancing rule* that ties together hello port and hello probe, which enables load balancing across a set of Service Fabric cluster nodes:</span></span>
   
    ```json
    {
        "name": "AppPortLBRule1",
        "properties": {
            "backendAddressPool": {
                "id": "[variables('lbPoolID0')]"
            },
            "backendPort": "[variables('loadBalancedAppPort1')]",
            "enableFloatingIP": false,
            "frontendIPConfiguration": {
                "id": "[variables('lbIPConfig0')]"
            },
            "frontendPort": "[variables('loadBalancedAppPort1')]",
            "idleTimeoutInMinutes": 5,
            "probe": {
                "id": "[concat(variables('lbID0'),'/probes/AppPortProbe1')]"
            },
            "protocol": "Tcp"
        }
    }
    ```
   <span data-ttu-id="aa3a1-155">Hello megtervezni toodeploy toohello fürt szükséges további portok, adhat hozzá további mintavétel létrehozása és szabálydefiníciók terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-155">If hello applications that you plan toodeploy toohello cluster need more ports, you can add them by creating additional probe and load-balancing rule definitions.</span></span> <span data-ttu-id="aa3a1-156">További tájékoztatást a Resource Manager-sablonok segítségével az Azure terheléselosztó toowork lásd: [létrehozásához egy belső terheléselosztó-sablon használatával](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="aa3a1-156">For more information on how toowork with Azure Load Balancer through Resource Manager templates, see [Get started creating an internal load balancer using a template](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span></span>

## <a name="deploy-hello-template-by-using-visual-studio"></a><span data-ttu-id="aa3a1-157">Hello sablon telepítése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="aa3a1-157">Deploy hello template by using Visual Studio</span></span>
<span data-ttu-id="aa3a1-158">Mentése után minden hello-e a szükséges paraméterértékeket a`ServiceFabricCluster.param.dev.json` fájl, készen áll a toodeploy hello sablon és a Service Fabric-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-158">After you have saved all hello required parameter values in the`ServiceFabricCluster.param.dev.json` file, you are ready toodeploy hello template and create your Service Fabric cluster.</span></span> <span data-ttu-id="aa3a1-159">Kattintson a jobb gombbal a hello erőforráscsoport-projekt a Visual Studio Solution Explorerben, és válassza a **telepítés |} Új központi telepítési...** . Ha szükséges, a Visual Studio megjeleníti hello **tooResource csoport telepítése** párbeszédpanelen tooauthenticate tooAzure kéri:</span><span class="sxs-lookup"><span data-stu-id="aa3a1-159">Right-click hello resource group project in Visual Studio Solution Explorer and choose **Deploy | New Deployment...**. If necessary, Visual Studio will show hello **Deploy tooResource Group** dialog box, asking you tooauthenticate tooAzure:</span></span>

![Központi telepítése tooResource csoport párbeszédpanel][3]

<span data-ttu-id="aa3a1-161">hello párbeszédpanelen kiválaszthatja hello fürt-vagy hello beállítás toocreate egy új ad egy meglévő Resource Manager erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-161">hello dialog box lets you choose an existing Resource Manager resource group for hello cluster and gives you hello option toocreate a new one.</span></span> <span data-ttu-id="aa3a1-162">Általában teszi logika toouse egy külön erőforráscsoportot a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-162">It normally makes sense toouse a separate resource group for a Service Fabric cluster.</span></span>

<span data-ttu-id="aa3a1-163">Miután hello telepítés gombra kattint, a Visual Studio kérni fogja, tooconfirm hello sablon-paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-163">After you hit hello Deploy button, Visual Studio will prompt you tooconfirm hello template parameter values.</span></span> <span data-ttu-id="aa3a1-164">Találati hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-164">Hit hello **Save** button.</span></span> <span data-ttu-id="aa3a1-165">Egy paraméter nincs a megőrzött értéke: hello fürt hello rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-165">One parameter does not have a persisted value: hello administrative account password for hello cluster.</span></span> <span data-ttu-id="aa3a1-166">Ha a Visual Studio kéri egy szüksége tooprovide jelszó értéket.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-166">You need tooprovide a password value when Visual Studio prompts you for one.</span></span>

> [!NOTE]
> <span data-ttu-id="aa3a1-167">Azure SDK 2.9-től kezdődően Visual Studio támogatja az olvasást jelszavakat **Azure Key Vault** üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-167">Starting with Azure SDK 2.9, Visual Studio supports reading passwords from **Azure Key Vault** during deployment.</span></span> <span data-ttu-id="aa3a1-168">Hello sablon paraméterek párbeszédpanelen figyelje meg, hogy hello `adminPassword` paraméter beviteli mező jobb hello rendelkezik egy kis "kulcsot" ikonra.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-168">In hello template parameters dialog notice that hello `adminPassword` parameter text box has a little "key" icon on hello right.</span></span> <span data-ttu-id="aa3a1-169">Erre az ikonra lehetővé teszi egy meglévő kulcstároló titkos tooselect hello fürt hello felügyeleti jelszóként.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-169">This icon allows you tooselect an existing key vault secret as hello administrative password for hello cluster.</span></span> <span data-ttu-id="aa3a1-170">Csak győződjön meg arról, hogy engedélyezze a hozzáférést a kulcstartót sablon-üzembehelyezés hello speciális hozzáférési házirendek az Azure Resource Manager toofirst.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-170">Just make sure toofirst enable Azure Resource Manager access for template deployment in hello Advanced Access Policies of your key vault.</span></span> 
> 
> 

<span data-ttu-id="aa3a1-171">Kísérheti hello hello Visual Studio kimeneti ablakában hello központi telepítési folyamata során.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-171">You can monitor hello progress of hello deployment process in hello Visual Studio output window.</span></span> <span data-ttu-id="aa3a1-172">Ha hello sablon telepítése befejeződött, az új fürthöz toouse készen áll!</span><span class="sxs-lookup"><span data-stu-id="aa3a1-172">Once hello template deployment is completed, your new cluster is ready toouse!</span></span>

> [!NOTE]
> <span data-ttu-id="aa3a1-173">Ha PowerShell soha nem volt használt tooadminister Azure most használt hello gépről, akkor toodo egy kis housekeeping.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-173">If PowerShell was never used tooadminister Azure from hello machine that you are using now, you need toodo a little housekeeping.</span></span>
> 
> 1. <span data-ttu-id="aa3a1-174">Engedélyezze a PowerShell parancsfájl-kezelési hello futtatásával [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) parancsot.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-174">Enable PowerShell scripting by running hello [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) command.</span></span> <span data-ttu-id="aa3a1-175">A fejlesztési gépek "korlátlan" házirend általában elfogadható.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-175">For development machines, "unrestricted" policy is usually acceptable.</span></span>
> 2. <span data-ttu-id="aa3a1-176">Döntse el, hogy Azure PowerShell-parancsokat, majd futtassa a diagnosztikai adatok gyűjtése tooallow [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) vagy [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-176">Decide whether tooallow diagnostic data collection from Azure PowerShell commands, and run [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) or [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) as necessary.</span></span> <span data-ttu-id="aa3a1-177">Ezzel elkerüli a szükségtelen kér sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-177">This will avoid unnecessary prompts during template deployment.</span></span>
> 
> 

<span data-ttu-id="aa3a1-178">Ha hiba történik, lépjen a toohello [Azure-portálon](https://portal.azure.com/) és a nyitott hello erőforráscsoport számára központilag telepített.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-178">If there are any errors, go toohello [Azure portal](https://portal.azure.com/) and open hello resource group that you deployed to.</span></span> <span data-ttu-id="aa3a1-179">Kattintson a **összes beállítás**, majd kattintson a **központi telepítések** a hello-beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-179">Click **All settings**, then click **Deployments** on hello settings blade.</span></span> <span data-ttu-id="aa3a1-180">Sikertelen erőforráscsoport-telepítési hiba elhagyja részletes diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="aa3a1-180">A failed resource-group deployment leaves detailed diagnostic information there.</span></span>

> [!NOTE]
> <span data-ttu-id="aa3a1-181">Service Fabric-fürtök bizonyos számú csomópontok toobe toomaintain rendelkezésre állási be kell, és megőrzi állapotát - hivatkozott tooas "kvórum fenntartása."</span><span class="sxs-lookup"><span data-stu-id="aa3a1-181">Service Fabric clusters require a certain number of nodes toobe up toomaintain availability and preserve state - referred tooas "maintaining quorum."</span></span> <span data-ttu-id="aa3a1-182">Ezért nem biztonságos tooshut összes hello fürt hello gépet le kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot](service-fabric-reliable-services-backup-restore.md).</span><span class="sxs-lookup"><span data-stu-id="aa3a1-182">Therefore, it is not safe tooshut down all of hello machines in hello cluster unless you have first performed a [full backup of your state](service-fabric-reliable-services-backup-restore.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="aa3a1-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="aa3a1-183">Next steps</span></span>
* [<span data-ttu-id="aa3a1-184">További tudnivalók: hello Azure-portál használatával a Service Fabric-fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="aa3a1-184">Learn about setting up Service Fabric cluster using hello Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="aa3a1-185">Megtudhatja, hogyan toomanage és a Visual Studio használatával a Service Fabric-alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="aa3a1-185">Learn how toomanage and deploy Service Fabric applications using Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
