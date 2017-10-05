---
title: "Egy Visual Studio használatával a Service Fabric-fürt beállítása |} Microsoft Docs"
description: "Ismerteti, hogyan lehet egy Azure erőforráscsoport-projektet a Visual Studio által létrehozott Azure Resource Manager-sablon használatával a Service Fabric-fürt beállítása"
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
ms.openlocfilehash: c43145b96cdbdfaa7e1893e50d027321fe4c0510
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-a-service-fabric-cluster-by-using-visual-studio"></a><span data-ttu-id="d932c-103">A Service Fabric-fürt beállítása a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="d932c-103">Set up a Service Fabric cluster by using Visual Studio</span></span>
<span data-ttu-id="d932c-104">A cikkből megtudhatja, hogyan állíthat be egy Azure Service Fabric-fürt Visual Studio és az Azure Resource Manager-sablon használatával.</span><span class="sxs-lookup"><span data-stu-id="d932c-104">This article describes how to set up an Azure Service Fabric cluster by using Visual Studio and an Azure Resource Manager template.</span></span> <span data-ttu-id="d932c-105">A Visual Studio Azure erőforráscsoport-projekt használjuk a sablon létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d932c-105">We will use a Visual Studio Azure resource group project to create the template.</span></span> <span data-ttu-id="d932c-106">A sablon létrehozása után telepíthető közvetlenül az Azure-bA a Visual Studio eszközből.</span><span class="sxs-lookup"><span data-stu-id="d932c-106">After the template has been created, it can be deployed directly to Azure from Visual Studio.</span></span> <span data-ttu-id="d932c-107">Azt is használható egy parancsfájlból származó, vagy a folyamatos integrációt (CI) létesítmény részeként.</span><span class="sxs-lookup"><span data-stu-id="d932c-107">It can also be used from a script, or as part of continuous integration (CI) facility.</span></span>

## <a name="create-a-service-fabric-cluster-template-by-using-an-azure-resource-group-project"></a><span data-ttu-id="d932c-108">A Service Fabric-fürt sablon az Azure erőforráscsoport-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="d932c-108">Create a Service Fabric cluster template by using an Azure resource group project</span></span>
<span data-ttu-id="d932c-109">Első lépésként nyissa meg a Visual Studio, és hozzon létre egy Azure erőforráscsoport-projekt (érhető el a **felhő** mappa):</span><span class="sxs-lookup"><span data-stu-id="d932c-109">To get started, open Visual Studio and create an Azure resource group project (it is available in the **Cloud** folder):</span></span>

![Új projekt párbeszédpanel kijelölt Azure erőforráscsoport-projekt][1]

<span data-ttu-id="d932c-111">Hozzon létre egy új Visual Studio megoldás ebben a projektben, vagy adja hozzá egy meglévő megoldás.</span><span class="sxs-lookup"><span data-stu-id="d932c-111">You can create a new Visual Studio solution for this project or add it to an existing solution.</span></span>

> [!NOTE]
> <span data-ttu-id="d932c-112">Ha nem látja az Azure erőforráscsoport-projekt felhő csomópontja alatt, nincs az Azure SDK telepítése.</span><span class="sxs-lookup"><span data-stu-id="d932c-112">If you do not see the Azure resource group project under the Cloud node, you do not have the Azure SDK installed.</span></span> <span data-ttu-id="d932c-113">Indítsa el a Webplatform-telepítővel ([most telepíteni](http://www.microsoft.com/web/downloads/platform.aspx) Ha még nem tette meg), és keressen a "Azure SDK a .NET", és telepítse a Visual Studio verziója kompatibilis verziót.</span><span class="sxs-lookup"><span data-stu-id="d932c-113">Launch Web Platform Installer ([install it now](http://www.microsoft.com/web/downloads/platform.aspx) if you have not already), and then search for "Azure SDK for .NET" and install the version that is compatible with your version of Visual Studio.</span></span>
> 
> 

<span data-ttu-id="d932c-114">Miután az OK gombra kattint, a Visual Studio felszólítja, hogy válassza ki az erőforrás-kezelő sablont szeretne létrehozni:</span><span class="sxs-lookup"><span data-stu-id="d932c-114">After you hit the OK button, Visual Studio will ask you to select the Resource Manager template you want to create:</span></span>

![Azure-sablon párbeszédpanelen válassza ki a kijelölt Service Fabric-fürt sablonnal][2]

<span data-ttu-id="d932c-116">Válassza ki a Service Fabric-fürt sablont, majd ismét kattintson az OK gombra.</span><span class="sxs-lookup"><span data-stu-id="d932c-116">Select the Service Fabric Cluster template and hit the OK button again.</span></span> <span data-ttu-id="d932c-117">Most már létrejött a projekt és a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="d932c-117">The project and the Resource Manager template have now been created.</span></span>

## <a name="prepare-the-template-for-deployment"></a><span data-ttu-id="d932c-118">A sablon üzembe helyezésének előkészítése</span><span class="sxs-lookup"><span data-stu-id="d932c-118">Prepare the template for deployment</span></span>
<span data-ttu-id="d932c-119">A sablon telepítése a fürt létrehozásához, előtt a sablonhoz szükséges paraméterek értékeket kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="d932c-119">Before the template is deployed to create the cluster, you must provide values for the required template parameters.</span></span> <span data-ttu-id="d932c-120">A paraméterértékek pedig olvassa a rendszer a `ServiceFabricCluster.parameters.json` fájl, amely a `Templates` az erőforráscsoport-projekt mappájából.</span><span class="sxs-lookup"><span data-stu-id="d932c-120">These parameter values are read from the `ServiceFabricCluster.parameters.json` file, which is in the `Templates` folder of the resource group project.</span></span> <span data-ttu-id="d932c-121">Nyissa meg a fájlt, és adja meg a következő értékeket:</span><span class="sxs-lookup"><span data-stu-id="d932c-121">Open the file and provide the following values:</span></span>

| <span data-ttu-id="d932c-122">Paraméter neve</span><span class="sxs-lookup"><span data-stu-id="d932c-122">Parameter name</span></span> | <span data-ttu-id="d932c-123">Leírás</span><span class="sxs-lookup"><span data-stu-id="d932c-123">Description</span></span> |
| --- | --- |
| <span data-ttu-id="d932c-124">adminUserName</span><span class="sxs-lookup"><span data-stu-id="d932c-124">adminUserName</span></span> |<span data-ttu-id="d932c-125">A Service Fabric gépek (csomópontok) a rendszergazdai fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="d932c-125">The name of the administrator account for Service Fabric machines (nodes).</span></span> |
| <span data-ttu-id="d932c-126">certificateThumbprint</span><span class="sxs-lookup"><span data-stu-id="d932c-126">certificateThumbprint</span></span> |<span data-ttu-id="d932c-127">Biztonságossá teszi a fürt tanúsítvány ujjlenyomata.</span><span class="sxs-lookup"><span data-stu-id="d932c-127">The thumbprint of the certificate that secures the cluster.</span></span> |
| <span data-ttu-id="d932c-128">sourceVaultResourceId</span><span class="sxs-lookup"><span data-stu-id="d932c-128">sourceVaultResourceId</span></span> |<span data-ttu-id="d932c-129">A *erőforrás-azonosító* a kulcstároló, a tanúsítványt, amely biztosítja a fürt tárolására.</span><span class="sxs-lookup"><span data-stu-id="d932c-129">The *resource ID* of the key vault where the certificate that secures the cluster is stored.</span></span> |
| <span data-ttu-id="d932c-130">certificateUrlValue</span><span class="sxs-lookup"><span data-stu-id="d932c-130">certificateUrlValue</span></span> |<span data-ttu-id="d932c-131">A fürt biztonsági tanúsítvány URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="d932c-131">The URL of the cluster security certificate.</span></span> |

<span data-ttu-id="d932c-132">A Visual Studio Service Fabric Resource Manager-sablon védi egy tanúsítványt egy biztonságos fürtöt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d932c-132">The Visual Studio Service Fabric Resource Manager template creates a secure cluster that is protected by a certificate.</span></span> <span data-ttu-id="d932c-133">Ez a tanúsítvány az utolsó három Sablonparaméterek azonosít (`certificateThumbprint`, `sourceVaultValue`, és `certificateUrlValue`), és azt már léteznie kell egy **Azure Key Vault**.</span><span class="sxs-lookup"><span data-stu-id="d932c-133">This certificate is identified by the last three template parameters (`certificateThumbprint`, `sourceVaultValue`, and `certificateUrlValue`), and it must exist in an **Azure Key Vault**.</span></span> <span data-ttu-id="d932c-134">A fürt biztonsági tanúsítvány létrehozásával kapcsolatos további információkért lásd: [Service Fabric-fürt biztonsági forgatókönyvek](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) cikk.</span><span class="sxs-lookup"><span data-stu-id="d932c-134">For more information on how to create the cluster security certificate, see [Service Fabric cluster security scenarios](service-fabric-cluster-security.md#x509-certificates-and-service-fabric) article.</span></span>

## <a name="optional-change-the-cluster-name"></a><span data-ttu-id="d932c-135">Választható lehetőség: a fürt nevének módosítása</span><span class="sxs-lookup"><span data-stu-id="d932c-135">Optional: change the cluster name</span></span>
<span data-ttu-id="d932c-136">Minden Service Fabric-fürt neve van.</span><span class="sxs-lookup"><span data-stu-id="d932c-136">Every Service Fabric cluster has a name.</span></span> <span data-ttu-id="d932c-137">A Fabric-fürt létrehozása az Azure-fürt nevét határozza meg (együtt az Azure-régió) a tartománynévrendszer (DNS) a fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="d932c-137">When a Fabric cluster is created in Azure, cluster name determines (together with the Azure region) the Domain Name System (DNS) name for the cluster.</span></span> <span data-ttu-id="d932c-138">Például, ha a fürt nevezze el `myBigCluster`, és a hely (Azure-régió), amely az új fürt üzemelteti az erőforráscsoportban USA keleti régiója, és a fürt DNS-neve lesz `myBigCluster.eastus.cloudapp.azure.com`.</span><span class="sxs-lookup"><span data-stu-id="d932c-138">For example, if you name your cluster `myBigCluster`, and the location (Azure region) of the resource group that will host the new cluster is East US, the DNS name of the cluster will be `myBigCluster.eastus.cloudapp.azure.com`.</span></span>

<span data-ttu-id="d932c-139">Alapértelmezés szerint a fürt neve automatikusan létrehozza és egyedi tett egy véletlenszerű utótagot csatolása egy "fürt" előtag.</span><span class="sxs-lookup"><span data-stu-id="d932c-139">By default the cluster name is generated automatically and made unique by attaching a random suffix to a "cluster" prefix.</span></span> <span data-ttu-id="d932c-140">Ez megkönnyíti a sablon használata részeként egy **folyamatos integrációt** (CI) rendszer.</span><span class="sxs-lookup"><span data-stu-id="d932c-140">This makes it very easy to use the template as part of a **continuous integration** (CI) system.</span></span> <span data-ttu-id="d932c-141">Ha azt szeretné, a fürt számára megadott név, amelyet kifejező, állítsa a `clusterName` változó a Resource Manager sablon fájlban (`ServiceFabricCluster.json`) a kiválasztott névre.</span><span class="sxs-lookup"><span data-stu-id="d932c-141">If you want to use a specific name for your cluster, one that is meaningful to you, set the value of the `clusterName` variable in the Resource Manager template file (`ServiceFabricCluster.json`) to your chosen name.</span></span> <span data-ttu-id="d932c-142">Az első, az adott fájlban definiált változó.</span><span class="sxs-lookup"><span data-stu-id="d932c-142">It is the first variable defined in that file.</span></span>

## <a name="optional-add-public-application-ports"></a><span data-ttu-id="d932c-143">Választható lehetőség: nyilvános alkalmazás-portok hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d932c-143">Optional: add public application ports</span></span>
<span data-ttu-id="d932c-144">Érdemes módosítani a nyilvános alkalmazás által használt portokat a fürt telepítése előtt is.</span><span class="sxs-lookup"><span data-stu-id="d932c-144">You may also want to change the public application ports for the cluster before you deploy it.</span></span> <span data-ttu-id="d932c-145">Alapértelmezés szerint a sablon csak két nyilvános TCP-portot (80-as és 8081) megnyílik.</span><span class="sxs-lookup"><span data-stu-id="d932c-145">By default, the template opens up just two public TCP ports (80 and 8081).</span></span> <span data-ttu-id="d932c-146">Ha több, az alkalmazások, módosítsa a sablon az Azure Load Balancer-definíciójában.</span><span class="sxs-lookup"><span data-stu-id="d932c-146">If you need more for your applications, modify the Azure Load Balancer definition in the template.</span></span> <span data-ttu-id="d932c-147">A fő sablon fájl tárolja a definition (`ServiceFabricCluster.json`).</span><span class="sxs-lookup"><span data-stu-id="d932c-147">The definition is stored in the main template file (`ServiceFabricCluster.json`).</span></span> <span data-ttu-id="d932c-148">Nyissa meg a fájlt, és keresse meg `loadBalancedAppPort`.</span><span class="sxs-lookup"><span data-stu-id="d932c-148">Open that file and search for `loadBalancedAppPort`.</span></span> <span data-ttu-id="d932c-149">Minden port társítva három összetevők:</span><span class="sxs-lookup"><span data-stu-id="d932c-149">Each port is associated with three artifacts:</span></span>

1. <span data-ttu-id="d932c-150">Egy sablon változó, amely meghatározza a port, az TCP-port értékének:</span><span class="sxs-lookup"><span data-stu-id="d932c-150">A template variable that defines the TCP port value for the port:</span></span>
   
    ```json
    "loadBalancedAppPort1": "80"
    ```
2. <span data-ttu-id="d932c-151">A *mintavételi* , mely meghatározza, hogy milyen gyakran és mennyi ideig a Azure Load terheléselosztó próbálja meg használni egy adott Service Fabric-csomópont másikra feladatátvételét előtt.</span><span class="sxs-lookup"><span data-stu-id="d932c-151">A *probe* that defines how frequently and for how long the Azure load balancer attempts to use a specific Service Fabric node before failing over to another one.</span></span> <span data-ttu-id="d932c-152">A mintavételt a Load Balancer erőforrás részét képezik.</span><span class="sxs-lookup"><span data-stu-id="d932c-152">The probes are part of the Load Balancer resource.</span></span> <span data-ttu-id="d932c-153">Ez az első alapértelmezett portja mintavételi definíciója:</span><span class="sxs-lookup"><span data-stu-id="d932c-153">Here is the probe definition for the first default application port:</span></span>
   
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
3. <span data-ttu-id="d932c-154">A *terheléselosztási szabály* , amely kötelékek együtt a port és a mintavételi, mely lehetővé teszi, hogy terheléselosztás meg a Service Fabric-fürt csomópontok között:</span><span class="sxs-lookup"><span data-stu-id="d932c-154">A *load-balancing rule* that ties together the port and the probe, which enables load balancing across a set of Service Fabric cluster nodes:</span></span>
   
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
   <span data-ttu-id="d932c-155">Alkalmazásokat szeretne telepíteni a fürt további portokat kell, adhat hozzá további mintavétel létrehozása és szabálydefiníciók terheléselosztás.</span><span class="sxs-lookup"><span data-stu-id="d932c-155">If the applications that you plan to deploy to the cluster need more ports, you can add them by creating additional probe and load-balancing rule definitions.</span></span> <span data-ttu-id="d932c-156">A Resource Manager-sablonok segítségével az Azure terheléselosztó munkavégzés további információkért lásd: [létrehozásához egy belső terheléselosztó-sablon használatával](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span><span class="sxs-lookup"><span data-stu-id="d932c-156">For more information on how to work with Azure Load Balancer through Resource Manager templates, see [Get started creating an internal load balancer using a template](../load-balancer/load-balancer-get-started-ilb-arm-template.md).</span></span>

## <a name="deploy-the-template-by-using-visual-studio"></a><span data-ttu-id="d932c-157">A sablon telepítése a Visual Studio használatával</span><span class="sxs-lookup"><span data-stu-id="d932c-157">Deploy the template by using Visual Studio</span></span>
<span data-ttu-id="d932c-158">Az összes kötelező paraméter értékét mentése után a`ServiceFabricCluster.param.dev.json` fájl, készen áll a sablon telepítéséhez és a Service Fabric-fürt létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d932c-158">After you have saved all the required parameter values in the`ServiceFabricCluster.param.dev.json` file, you are ready to deploy the template and create your Service Fabric cluster.</span></span> <span data-ttu-id="d932c-159">Kattintson a jobb gombbal az erőforráscsoport-projekt a Visual Studio Solution Explorerben, és válassza a **telepítés |} Új központi telepítési...** .</span><span class="sxs-lookup"><span data-stu-id="d932c-159">Right-click the resource group project in Visual Studio Solution Explorer and choose **Deploy | New Deployment...**.</span></span> <span data-ttu-id="d932c-160">Ha szükséges, a Visual Studio megjeleníti a **telepítés erőforráscsoportra** párbeszédpanelen kéri, hogy hitelesítésre:</span><span class="sxs-lookup"><span data-stu-id="d932c-160">If necessary, Visual Studio will show the **Deploy to Resource Group** dialog box, asking you to authenticate to Azure:</span></span>

![Erőforráscsoport párbeszédpanel telepítése][3]

<span data-ttu-id="d932c-162">A párbeszédpanelen kiválaszthatja egy erőforrás-kezelő meglévő erőforráscsoportot, a fürt, és felajánlja a hozzon létre egy újat.</span><span class="sxs-lookup"><span data-stu-id="d932c-162">The dialog box lets you choose an existing Resource Manager resource group for the cluster and gives you the option to create a new one.</span></span> <span data-ttu-id="d932c-163">Általában érdemes külön erőforráscsoport használata a Service Fabric-fürt.</span><span class="sxs-lookup"><span data-stu-id="d932c-163">It normally makes sense to use a separate resource group for a Service Fabric cluster.</span></span>

<span data-ttu-id="d932c-164">Miután a telepítés gombra kattint, a Visual Studio kérni fogja, hogy erősítse meg a sablon-paraméterértékek.</span><span class="sxs-lookup"><span data-stu-id="d932c-164">After you hit the Deploy button, Visual Studio will prompt you to confirm the template parameter values.</span></span> <span data-ttu-id="d932c-165">Elérte a **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="d932c-165">Hit the **Save** button.</span></span> <span data-ttu-id="d932c-166">Egy paraméter nincs a megőrzött értéke: a fürt rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="d932c-166">One parameter does not have a persisted value: the administrative account password for the cluster.</span></span> <span data-ttu-id="d932c-167">Adjon meg egy jelszót értéket, ha a Visual Studio kéri egy kell.</span><span class="sxs-lookup"><span data-stu-id="d932c-167">You need to provide a password value when Visual Studio prompts you for one.</span></span>

> [!NOTE]
> <span data-ttu-id="d932c-168">Azure SDK 2.9-től kezdődően Visual Studio támogatja az olvasást jelszavakat **Azure Key Vault** üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="d932c-168">Starting with Azure SDK 2.9, Visual Studio supports reading passwords from **Azure Key Vault** during deployment.</span></span> <span data-ttu-id="d932c-169">Figyelje meg, hogy a sablon paraméterek párbeszédpanelen a `adminPassword` paraméter szövegmező rendelkezik egy kis "kulcsot" ikonra a jobb oldali.</span><span class="sxs-lookup"><span data-stu-id="d932c-169">In the template parameters dialog notice that the `adminPassword` parameter text box has a little "key" icon on the right.</span></span> <span data-ttu-id="d932c-170">Ez az ikon jelöljön ki egy meglévő kulcstároló titkos kulcsot, a fürt rendszergazdai jelszavának teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="d932c-170">This icon allows you to select an existing key vault secret as the administrative password for the cluster.</span></span> <span data-ttu-id="d932c-171">Ne feledje első engedélyezése a kulcstartót sablon-üzembehelyezés a speciális hozzáférési házirendek az Azure Resource Manager hozzáférését.</span><span class="sxs-lookup"><span data-stu-id="d932c-171">Just make sure to first enable Azure Resource Manager access for template deployment in the Advanced Access Policies of your key vault.</span></span> 
> 
> 

<span data-ttu-id="d932c-172">Megfigyelheti, hogy a Visual Studio kimeneti ablakában a telepítési folyamat előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="d932c-172">You can monitor the progress of the deployment process in the Visual Studio output window.</span></span> <span data-ttu-id="d932c-173">Ha a sablon telepítése befejeződött, az új fürt használatra készen áll!</span><span class="sxs-lookup"><span data-stu-id="d932c-173">Once the template deployment is completed, your new cluster is ready to use!</span></span>

> [!NOTE]
> <span data-ttu-id="d932c-174">Ha a PowerShell soha nem szerepel a számítógépről, most használt Azure felügyeletéhez, kell tennie egy kis housekeeping.</span><span class="sxs-lookup"><span data-stu-id="d932c-174">If PowerShell was never used to administer Azure from the machine that you are using now, you need to do a little housekeeping.</span></span>
> 
> 1. <span data-ttu-id="d932c-175">Engedélyezze a PowerShell-parancsprogramok futtatásával a [ `Set-ExecutionPolicy` ](https://technet.microsoft.com/library/hh849812.aspx) parancsot.</span><span class="sxs-lookup"><span data-stu-id="d932c-175">Enable PowerShell scripting by running the [`Set-ExecutionPolicy`](https://technet.microsoft.com/library/hh849812.aspx) command.</span></span> <span data-ttu-id="d932c-176">A fejlesztési gépek "korlátlan" házirend általában elfogadható.</span><span class="sxs-lookup"><span data-stu-id="d932c-176">For development machines, "unrestricted" policy is usually acceptable.</span></span>
> 2. <span data-ttu-id="d932c-177">Határozza meg, hogy az Azure PowerShell-parancsok diagnosztikai adatok gyűjtését, és futtassa [ `Enable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619303.aspx) vagy [ `Disable-AzureRmDataCollection` ](https://msdn.microsoft.com/library/mt619236.aspx) szükség szerint.</span><span class="sxs-lookup"><span data-stu-id="d932c-177">Decide whether to allow diagnostic data collection from Azure PowerShell commands, and run [`Enable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619303.aspx) or [`Disable-AzureRmDataCollection`](https://msdn.microsoft.com/library/mt619236.aspx) as necessary.</span></span> <span data-ttu-id="d932c-178">Ezzel elkerüli a szükségtelen kér sablon üzembe helyezése során.</span><span class="sxs-lookup"><span data-stu-id="d932c-178">This will avoid unnecessary prompts during template deployment.</span></span>
> 
> 

<span data-ttu-id="d932c-179">Ha hiba történik, akkor folytassa a [Azure-portálon](https://portal.azure.com/) , és nyissa meg az erőforráscsoport számára központilag telepített.</span><span class="sxs-lookup"><span data-stu-id="d932c-179">If there are any errors, go to the [Azure portal](https://portal.azure.com/) and open the resource group that you deployed to.</span></span> <span data-ttu-id="d932c-180">Kattintson a **összes beállítás**, majd kattintson a **központi telepítések** a beállítások panelen.</span><span class="sxs-lookup"><span data-stu-id="d932c-180">Click **All settings**, then click **Deployments** on the settings blade.</span></span> <span data-ttu-id="d932c-181">Sikertelen erőforráscsoport-telepítési hiba elhagyja részletes diagnosztikai adatokat.</span><span class="sxs-lookup"><span data-stu-id="d932c-181">A failed resource-group deployment leaves detailed diagnostic information there.</span></span>

> [!NOTE]
> <span data-ttu-id="d932c-182">Service Fabric-fürtök rendelkezésre álljon, és állapot - néven "kvórum fenntartása." megőrzése be kell csomópontok bizonyos számú megkövetelése</span><span class="sxs-lookup"><span data-stu-id="d932c-182">Service Fabric clusters require a certain number of nodes to be up to maintain availability and preserve state - referred to as "maintaining quorum."</span></span> <span data-ttu-id="d932c-183">Ezért már nem biztonságos állítsa le a gépeket a fürt összes, kivéve, ha először elvégezte a [teljes biztonsági mentés a állapot](service-fabric-reliable-services-backup-restore.md).</span><span class="sxs-lookup"><span data-stu-id="d932c-183">Therefore, it is not safe to shut down all of the machines in the cluster unless you have first performed a [full backup of your state](service-fabric-reliable-services-backup-restore.md).</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d932c-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d932c-184">Next steps</span></span>
* [<span data-ttu-id="d932c-185">További tudnivalók az Azure portál használatával a Service Fabric-fürt beállítása</span><span class="sxs-lookup"><span data-stu-id="d932c-185">Learn about setting up Service Fabric cluster using the Azure portal</span></span>](service-fabric-cluster-creation-via-portal.md)
* [<span data-ttu-id="d932c-186">Ismerje meg, hogyan kezelheti és a Visual Studio használatával a Service Fabric-alkalmazások központi telepítése</span><span class="sxs-lookup"><span data-stu-id="d932c-186">Learn how to manage and deploy Service Fabric applications using Visual Studio</span></span>](service-fabric-manage-application-in-visual-studio.md)

<!--Image references-->
[1]: ./media/service-fabric-cluster-creation-via-visual-studio/azure-resource-group-project-creation.png
[2]: ./media/service-fabric-cluster-creation-via-visual-studio/selecting-azure-template.png
[3]: ./media/service-fabric-cluster-creation-via-visual-studio/deploy-to-azure.png
