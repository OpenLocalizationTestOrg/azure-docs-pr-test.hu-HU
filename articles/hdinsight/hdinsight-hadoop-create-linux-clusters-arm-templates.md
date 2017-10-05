---
title: "Sablonokkal - Azure HDInsight Hadoop-fürtök létrehozása |} Microsoft Docs"
description: "Ismerje meg a fürt létrehozása HDInsight Resource Manager-sablonok segítségével"
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: 00a80dea-011f-44f0-92a4-25d09db9d996
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/30/2017
ms.author: jgao
ms.openlocfilehash: b2cdc954530daea2a641599c946ce3787149e762
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a><span data-ttu-id="b3434-103">Hadoop-fürtök létrehozása a Hdinsightban Resource Manager-sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="b3434-103">Create Hadoop clusters in HDInsight by using Resource Manager templates</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="b3434-104">Ebből a cikkből megismerheti az Azure Resource Manager-sablonok Azure HDInsight-fürtök létrehozásának számos módja.</span><span class="sxs-lookup"><span data-stu-id="b3434-104">In this article, you learn several ways to create Azure HDInsight clusters with Azure Resource Manager templates.</span></span> <span data-ttu-id="b3434-105">További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-105">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="b3434-106">Más fürt létrehozása eszközeivel és szolgáltatásaival kapcsolatos információkért kattintson az ezen a lapon lévő lapválasztót, vagy tekintse meg [Fürtlétrehozási módszerekhez](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span><span class="sxs-lookup"><span data-stu-id="b3434-106">To learn about other cluster creation tools and features, click the tab selector on the top of this page or see [Cluster creation methods](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="b3434-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b3434-107">Prerequisites</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="b3434-108">Ez a cikk útmutatását, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="b3434-108">To follow the instructions in this article, you'll need:</span></span>

* <span data-ttu-id="b3434-109">Egy [Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="b3434-109">An [Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="b3434-110">Az Azure PowerShell és/vagy Azure CLI-t.</span><span class="sxs-lookup"><span data-stu-id="b3434-110">Azure PowerShell and/or Azure CLI.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a><span data-ttu-id="b3434-111">Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="b3434-111">Resource Manager templates</span></span>
<span data-ttu-id="b3434-112">A Resource Manager-sablon megkönnyíti, hogy egyetlen, koordinált műveletben a következő alkalmazás létrehozásához:</span><span class="sxs-lookup"><span data-stu-id="b3434-112">A Resource Manager template makes it easy to create the following for your application in a single, coordinated operation:</span></span>
* <span data-ttu-id="b3434-113">A HDInsight-fürtök és a tőle függő erőforrások (például az alapértelmezett tárfiók)</span><span class="sxs-lookup"><span data-stu-id="b3434-113">HDInsight clusters and their dependent resources (such as the default storage account)</span></span>
* <span data-ttu-id="b3434-114">További erőforrások (például az Azure SQL Database Apache Sqoop használatával)</span><span class="sxs-lookup"><span data-stu-id="b3434-114">Other resources (such as Azure SQL Database to use Apache Sqoop)</span></span>

<span data-ttu-id="b3434-115">A sablon határozza meg az erőforrásokat, amelyek szükségesek az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b3434-115">In the template, you define the resources that are needed for the application.</span></span> <span data-ttu-id="b3434-116">Is meg az üzembe helyezéshez megadott paraméterek a felhasználótól a különböző környezetekhez tartozó értékeket.</span><span class="sxs-lookup"><span data-stu-id="b3434-116">You also specify deployment parameters to input values for different environments.</span></span> <span data-ttu-id="b3434-117">A sablon JSON és az üzemelő példány értékeit összeállításához használt kifejezések áll.</span><span class="sxs-lookup"><span data-stu-id="b3434-117">The template consists of JSON and expressions that you use to construct values for your deployment.</span></span>

<span data-ttu-id="b3434-118">HDInsight sablon minták található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span><span class="sxs-lookup"><span data-stu-id="b3434-118">You can find HDInsight template samples at [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span></span> <span data-ttu-id="b3434-119">Használja a platformok közötti [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) rendelkező a [erőforrás-kezelő bővítmény](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) vagy egy szövegszerkesztőben, hogy menti a sablont egy fájlba a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="b3434-119">Use cross-platform [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) with the [Resource Manager extension](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) or a text editor to save the template into a file on your workstation.</span></span> <span data-ttu-id="b3434-120">Megismerheti, hogyan hívhatja meg a sablont más módszerekkel.</span><span class="sxs-lookup"><span data-stu-id="b3434-120">You learn how to call the template by using different methods.</span></span>

<span data-ttu-id="b3434-121">Erőforrás-kezelő sablonokkal kapcsolatos további információkért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b3434-121">For more information about Resource Manager templates, see the following articles:</span></span>

* [<span data-ttu-id="b3434-122">Szerző Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="b3434-122">Author Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="b3434-123">Alkalmazás üzembe helyezése az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="b3434-123">Deploy an application with Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a><span data-ttu-id="b3434-124">Sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="b3434-124">Generate templates</span></span>

<span data-ttu-id="b3434-125">Az Azure portál használatával állítsa be a fürt összes tulajdonságait, és mentse a sablon üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="b3434-125">By using the Azure portal, you can configure all the properties of a cluster and then save the template before deploying it.</span></span> <span data-ttu-id="b3434-126">A sablon majd felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="b3434-126">You can then reuse the template.</span></span>

<span data-ttu-id="b3434-127">**Egy sablon létrehozása az Azure portál használatával**</span><span class="sxs-lookup"><span data-stu-id="b3434-127">**To generate a template by using the Azure portal**</span></span>

1. <span data-ttu-id="b3434-128">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b3434-128">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="b3434-129">Kattintson a **új** kattintson a bal oldali menü **Eszközintelligencia + analitika**, és kattintson a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="b3434-129">Click **New** on the left menu, click **Intelligence + analytics**, and then click **HDInsight**.</span></span>
3. <span data-ttu-id="b3434-130">Kövesse az útmutatást követve adja meg az tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="b3434-130">Follow the instructions to enter properties.</span></span> <span data-ttu-id="b3434-131">Választhatja a **Gyorslétrehozás** vagy a **egyéni** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b3434-131">You can use either the **Quick create** or the **Custom** option.</span></span>
4. <span data-ttu-id="b3434-132">Az a **összegzés** lapra, majd **töltse le a sablon és a paraméterek**:</span><span class="sxs-lookup"><span data-stu-id="b3434-132">On the **Summary** tab, click **Download template and parameters**:</span></span>

    ![HDInsight Hadoop létrehozása a fürt Resource Manager sablon letöltése](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    <span data-ttu-id="b3434-134">A sablonfájl paraméterfájl és a sablon telepítéséhez használt mintakódok listájának megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="b3434-134">You see a list of the template file, parameters file, and code samples used to deploy the template:</span></span>

    ![HDInsight Hadoop-fürt létrehozása Resource Manager sablon letöltési beállítások](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    <span data-ttu-id="b3434-136">Itt a sablon letöltése, mentse a sablont a könyvtárban, vagy a sablon telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b3434-136">From here, you can download the template, save it to your template library, or deploy the template.</span></span>

    <span data-ttu-id="b3434-137">Egy sablont a könyvtárban eléréséhez kattintson **további szolgáltatások** a bal oldali menüben, majd kattintson a **sablonok** (alatt a **más** kategória).</span><span class="sxs-lookup"><span data-stu-id="b3434-137">To access a template in your library, click **More services** from the left menu, and then click **Templates** (under the **Other** category).</span></span>

    > [!Note]
    > <span data-ttu-id="b3434-138">A sablon és a paraméterek fájl együtt kell használni.</span><span class="sxs-lookup"><span data-stu-id="b3434-138">The template and parameters file must be used together.</span></span> <span data-ttu-id="b3434-139">Ellenkező esetben előfordulhat, hogy eredményt el nem várt.</span><span class="sxs-lookup"><span data-stu-id="b3434-139">Otherwise, you might get unexpected results.</span></span> <span data-ttu-id="b3434-140">Például az alapértelmezett **clusterKind** tulajdonság értéke mindig **hadoop**, annak ellenére, hogy milyen, adja meg a sablon letöltése előtt.</span><span class="sxs-lookup"><span data-stu-id="b3434-140">For example, the default **clusterKind** property value is always **hadoop**, despite what you specify before you download the template.</span></span>



## <a name="deploy-with-powershell"></a><span data-ttu-id="b3434-141">Üzembe helyezés a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="b3434-141">Deploy with PowerShell</span></span>

<span data-ttu-id="b3434-142">Ez az eljárás egy Hadoop-fürt hdinsightban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b3434-142">This procedure creates a Hadoop cluster in HDInsight.</span></span>

1. <span data-ttu-id="b3434-143">A JSON-fájl mentése a [függelék](#appx-a-arm-template) a munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="b3434-143">Save the JSON file in the [Appendix](#appx-a-arm-template) to your workstation.</span></span> <span data-ttu-id="b3434-144">A PowerShell-parancsfájlt, a fájl neve: `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span><span class="sxs-lookup"><span data-stu-id="b3434-144">In the PowerShell script, the file name is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span></span>
2. <span data-ttu-id="b3434-145">Ha szükséges, állítsa a paramétereket és változókat.</span><span class="sxs-lookup"><span data-stu-id="b3434-145">Set the parameters and variables if needed.</span></span>
3. <span data-ttu-id="b3434-146">A sablon futtassa a következő PowerShell-parancsfájl használatával:</span><span class="sxs-lookup"><span data-stu-id="b3434-146">Run the template by using the following PowerShell script:</span></span>

        ####################################
        # Set these variables
        ####################################
        #region - used for creating Azure service names
        $nameToken = "<Enter an Alias>"
        $templateFile = "C:\HDITutorials-ARM\hdinsight-arm-template.json"
        #endregion

        ####################################
        # Service names and variables
        ####################################
        #region - service names
        $namePrefix = $nameToken.ToLower() + (Get-Date -Format "MMdd")

        $resourceGroupName = $namePrefix + "rg"
        $hdinsightClusterName = $namePrefix + "hdi"
        $defaultStorageAccountName = $namePrefix + "store"
        $defaultBlobContainerName = $hdinsightClusterName

        $location = "East US 2"

        $armDeploymentName = $namePrefix
        #endregion

        ####################################
        # Connect to Azure
        ####################################
        #region - Connect to Azure subscription
        Write-Host "`nConnecting to your Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and the dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    <span data-ttu-id="b3434-147">A PowerShell-parancsfájl a csak a fürt nevét konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="b3434-147">The PowerShell script configures only the cluster name.</span></span> <span data-ttu-id="b3434-148">A tárfiók neve nem változtatható a sablonban.</span><span class="sxs-lookup"><span data-stu-id="b3434-148">The storage account name is hard-coded in the template.</span></span> <span data-ttu-id="b3434-149">A fürt felhasználói jelszó megadására kéri.</span><span class="sxs-lookup"><span data-stu-id="b3434-149">You are prompted to enter the cluster user password.</span></span> <span data-ttu-id="b3434-150">(Az alapértelmezett felhasználónév az **admin**.) Is kéri az SSH-felhasználói jelszó.</span><span class="sxs-lookup"><span data-stu-id="b3434-150">(The default username is **admin**.) You are also prompted to enter the SSH user password.</span></span> <span data-ttu-id="b3434-151">(Az alapértelmezett SSH-felhasználónév **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="b3434-151">(The default SSH username is **sshuser**.)</span></span>  

<span data-ttu-id="b3434-152">További információkért lásd: [telepítés a következő PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="b3434-152">For more information, see  [Deploy with PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span>

## <a name="deploy-with-cli"></a><span data-ttu-id="b3434-153">A parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="b3434-153">Deploy with CLI</span></span>
<span data-ttu-id="b3434-154">Az alábbi példában az Azure parancssori felület (CLI).</span><span class="sxs-lookup"><span data-stu-id="b3434-154">The following sample uses Azure command-line interface (CLI).</span></span> <span data-ttu-id="b3434-155">A fürt és a függő tárfiókot és a tároló létrehoz egy Resource Manager-sablon meghívásával:</span><span class="sxs-lookup"><span data-stu-id="b3434-155">It creates a cluster and its dependent storage account and container by calling a Resource Manager template:</span></span>

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

<span data-ttu-id="b3434-156">Adja meg kéri:</span><span class="sxs-lookup"><span data-stu-id="b3434-156">You are prompted to enter:</span></span>
* <span data-ttu-id="b3434-157">A fürt nevét.</span><span class="sxs-lookup"><span data-stu-id="b3434-157">The cluster name.</span></span>
* <span data-ttu-id="b3434-158">A fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b3434-158">The cluster user password.</span></span> <span data-ttu-id="b3434-159">(Az alapértelmezett felhasználónév az **admin**.)</span><span class="sxs-lookup"><span data-stu-id="b3434-159">(The default username is **admin**.)</span></span>
* <span data-ttu-id="b3434-160">Az SSH-felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="b3434-160">The SSH user password.</span></span> <span data-ttu-id="b3434-161">(Az alapértelmezett SSH-felhasználónév **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="b3434-161">(The default SSH username is **sshuser**.)</span></span>

<span data-ttu-id="b3434-162">Az alábbi kód beágyazott paraméterek szolgál:</span><span class="sxs-lookup"><span data-stu-id="b3434-162">The following code provides inline parameters:</span></span>

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-the-rest-api"></a><span data-ttu-id="b3434-163">A REST API-t központi telepítése</span><span class="sxs-lookup"><span data-stu-id="b3434-163">Deploy with the REST API</span></span>
<span data-ttu-id="b3434-164">Lásd: [REST API-val telepítése](../azure-resource-manager/resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-164">See [Deploy with the REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span></span>

## <a name="deploy-with-visual-studio"></a><span data-ttu-id="b3434-165">Üzembe helyezés a Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="b3434-165">Deploy with Visual Studio</span></span>
 <span data-ttu-id="b3434-166">Visual Studio használatával hozzon létre egy erőforráscsoport-projekt, és telepítse az Azure a felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="b3434-166">Use Visual Studio to create a resource group project and deploy it to Azure through the user interface.</span></span> <span data-ttu-id="b3434-167">Milyen típusú erőforrásokat tartalmazza a projekt választja.</span><span class="sxs-lookup"><span data-stu-id="b3434-167">You select the type of resources to include in your project.</span></span> <span data-ttu-id="b3434-168">Ezeket az erőforrásokat a rendszer automatikusan hozzáadja a Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="b3434-168">Those resources are automatically added to the Resource Manager template.</span></span> <span data-ttu-id="b3434-169">A projekt a sablon telepítéséhez PowerShell parancsfájlt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="b3434-169">The project also provides a PowerShell script to deploy the template.</span></span>

<span data-ttu-id="b3434-170">Visual Studio használatával az erőforráscsoportokhoz bemutatása, lásd: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-170">For an introduction to using Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="b3434-171">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="b3434-171">Troubleshoot</span></span>

<span data-ttu-id="b3434-172">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="b3434-172">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="b3434-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b3434-173">Next steps</span></span>
<span data-ttu-id="b3434-174">Ebben a cikkben megtanulta rendelkezik többféle módon hozhat létre HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="b3434-174">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="b3434-175">További tudnivalókért tekintse meg a következő cikkeket:</span><span class="sxs-lookup"><span data-stu-id="b3434-175">To learn more, see the following articles:</span></span>

* <span data-ttu-id="b3434-176">Például a .NET ügyféloldali kódtár erőforrásoknak történő telepítésének, [erőforrások telepíteni a .NET-kódtárakra és egy sablon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b3434-176">For an example of deploying resources through the .NET client library, see [Deploy resources by using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="b3434-177">Részletes példa az alkalmazások központi telepítése, lásd: [kiépítése és mikroszolgáltatások kiszámítható módon tudja az Azure-ban telepítheti](../app-service-web/app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-177">For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span></span>
* <span data-ttu-id="b3434-178">Útmutató a megoldások különböző környezetekben történő telepítéséhez: [Fejlesztési és tesztelési környezetek a Microsoft Azure eszközben](../solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-178">For guidance on deploying your solution to different environments, see [Development and test environments in Microsoft Azure](../solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="b3434-179">A szakaszok az Azure Resource Manager sablon kapcsolatos további tudnivalókért lásd: [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-179">To learn about the sections of the Azure Resource Manager template, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="b3434-180">Az Azure Resource Manager-sablonokban használható függvények listáját lásd: [sablonfüggvényei](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="b3434-180">For a list of the functions you can use in an Azure Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md).</span></span>

## <a name="appendix-resource-manager-template-to-create-a-hadoop-cluster"></a><span data-ttu-id="b3434-181">A függelék: Resource Manager-sablon egy Hadoop-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3434-181">Appendix: Resource Manager template to create a Hadoop cluster</span></span>
<span data-ttu-id="b3434-182">A következő Azure Resource Manager-sablon egy Linux-alapú Hadoop-fürt a függő Azure storage-fiókot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b3434-182">The following Azure Resource Manager template creates a Linux-based Hadoop cluster with the dependent Azure storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="b3434-183">Ez a minta Hive metaadattárhoz és az Oozie metaadattárhoz konfigurációs információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b3434-183">This sample includes configuration information for Hive metastore and Oozie metastore.</span></span> <span data-ttu-id="b3434-184">Távolítsa el a szakasz, vagy konfigurálja a szakasz a sablon használata előtt.</span><span class="sxs-lookup"><span data-stu-id="b3434-184">Remove the section or configure the section before using the template.</span></span>
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "The name of the HDInsight cluster to create."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used to remotely access the cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "location": {
        "type": "string",
        "defaultValue": "East US",
        "allowedValues": [
            "East US",
            "East US 2",
            "North Central US",
            "South Central US",
            "West US",
            "North Europe",
            "West Europe",
            "East Asia",
            "Southeast Asia",
            "Japan East",
            "Japan West",
            "Australia East",
            "Australia Southeast"
        ],
        "metadata": {
            "description": "The location where all azure resources will be deployed."
        }
        },
        "clusterType": {
        "type": "string",
        "defaultValue": "hadoop",
        "allowedValues": [
            "hadoop",
            "hbase",
            "storm",
            "spark"
        ],
        "metadata": {
            "description": "The type of the HDInsight cluster to create."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "The number of nodes in the HDInsight cluster."
        }
        }
    },
    "variables": {
        "defaultApiVersion": "2015-05-01-preview",
        "clusterApiVersion": "2015-03-01-preview",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
        {
        "name": "[parameters('clusterName')]",
        "type": "Microsoft.HDInsight/clusters",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('clusterApiVersion')]",
        "dependsOn": [ "[concat('Microsoft.Storage/storageAccounts/',variables('clusterStorageAccountName'))]" ],
        "tags": {

        },
        "properties": {
            "clusterVersion": "3.4",
            "osType": "Linux",
            "tier": "standard",
            "clusterDefinition": {
            "kind": "[parameters('clusterType')]",
            "configurations": {
                "gateway": {
                "restAuthCredential.isEnabled": true,
                "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                },
                "hive-site": {
                    "javax.jdo.option.ConnectionDriverName": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "javax.jdo.option.ConnectionURL": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "javax.jdo.option.ConnectionUserName": "johndole",
                    "javax.jdo.option.ConnectionPassword": "myPassword$"
                },
                "hive-env": {
                    "hive_database": "Existing MSSQL Server database with SQL authentication",
                    "hive_database_name": "myhive20160901",
                    "hive_database_type": "mssql",
                    "hive_existing_mssql_server_database": "myhive20160901",
                    "hive_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "hive_hostname": "myadla0901dbserver.database.windows.net"
                },
                "oozie-site": {
                    "oozie.service.JPAService.jdbc.driver": "com.microsoft.sqlserver.jdbc.SQLServerDriver",
                    "oozie.service.JPAService.jdbc.url": "jdbc:sqlserver://myadla0901dbserver.database.windows.net;database=myhive20160901;encrypt=true;trustServerCertificate=true;create=false;loginTimeout=300",
                    "oozie.service.JPAService.jdbc.username": "johndole",
                    "oozie.service.JPAService.jdbc.password": "myPassword$",
                    "oozie.db.schema.name": "oozie"
                },
                "oozie-env": {
                    "oozie_database": "Existing MSSQL Server database with SQL authentication",
                    "oozie_database_name": "myhive20160901",
                    "oozie_database_type": "mssql",
                    "oozie_existing_mssql_server_database": "myhive20160901",
                    "oozie_existing_mssql_server_host": "myadla0901dbserver.database.windows.net",
                    "oozie_hostname": "myadla0901dbserver.database.windows.net"
                }            
            }
            },
            "storageProfile": {
            "storageaccounts": [
                {
                "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                "isDefault": true,
                "container": "[parameters('clusterName')]",
                "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                }
            ]
            },
            "computeProfile": {
            "roles": [
                {
                "name": "headnode",
                "targetInstanceCount": "2",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                },
                {
                "name": "workernode",
                "targetInstanceCount": "[parameters('clusterWorkerNodeCount')]",
                "hardwareProfile": {
                    "vmSize": "Standard_D3"
                },
                "osProfile": {
                    "linuxOperatingSystemProfile": {
                    "username": "[parameters('sshUserName')]",
                    "password": "[parameters('sshPassword')]"
                    }
                }
                }
            ]
            }
        }
        }
    ],
    "outputs": {
        "cluster": {
        "type": "object",
        "value": "[reference(resourceId('Microsoft.HDInsight/clusters',parameters('clusterName')))]"
        }
    }
    }

## <a name="appendix-resource-manager-template-to-create-a-spark-cluster"></a><span data-ttu-id="b3434-185">A függelék: Resource Manager-sablon a Spark-fürt létrehozása</span><span class="sxs-lookup"><span data-stu-id="b3434-185">Appendix: Resource Manager template to create a Spark cluster</span></span>

<span data-ttu-id="b3434-186">Ez a témakör a Resource Manager-sablon egy HDInsight Spark-fürt létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="b3434-186">This section provides a Resource Manager template that you can use to create an HDInsight Spark cluster.</span></span> <span data-ttu-id="b3434-187">Ez a sablon tartalmazza konfigurációi `spark-defaults` és `spark-thrift-sparkconf` (az 1.6-os Spark-fürtök) és `spark2-defaults` és `spark2-thrift-sparkconf` (a Spark 2-fürtök).</span><span class="sxs-lookup"><span data-stu-id="b3434-187">This template includes configurations for `spark-defaults` and `spark-thrift-sparkconf` (for Spark 1.6 clusters) and `spark2-defaults` and `spark2-thrift-sparkconf` (for Spark 2 clusters).</span></span> <span data-ttu-id="b3434-188">Továbbá a HDInsight számítja ki, és beállítja, mint a konfigurációk `spark.executor.instances`, `spark.executor.memory`, és `spark.executor.cores` a fürt mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="b3434-188">In addition to this, HDInsight calculates and sets configurations such as `spark.executor.instances`, `spark.executor.memory`, and `spark.executor.cores` based on the cluster size.</span></span> 

<span data-ttu-id="b3434-189">Bármely egy paraméter értéke a szakasz a sablonba részeként, HDInsight nélkül kiszámításához, a többi paraméter szakaszában azonos.</span><span class="sxs-lookup"><span data-stu-id="b3434-189">If you set any one parameter in a section as part of the template itself, HDInsight does not calculate and set the other parameters of the same section.</span></span> <span data-ttu-id="b3434-190">Például paraméter `spark.executor.instances` szerepel a `spark-defaults` konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="b3434-190">For example, parameter `spark.executor.instances` is in the  `spark-defaults` configuration.</span></span> <span data-ttu-id="b3434-191">Egy másik paraméter értéke (például `spark.yarn.exector.memoryOverhead`) található a `spark-defaults` konfigurációs, HDInsight nélkül kiszámításához, a `spark.executor.instances` paramétert is.</span><span class="sxs-lookup"><span data-stu-id="b3434-191">If you set another parameter (for example, `spark.yarn.exector.memoryOverhead`) in the `spark-defaults` configuration, HDInsight does not calculate and set the `spark.executor.instances` parameter as well.</span></span>

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "The name of the HDInsight cluster to create."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used to submit jobs to the cluster and to log into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "The location where all azure resources will be deployed."
            }
        },
        "clusterVersion": {
            "type": "string",
            "defaultValue": "3.5",
            "metadata": {
                "description": "HDInsight cluster version."
            }
        },
        "clusterWorkerNodeCount": {
            "type": "int",
            "defaultValue": 4,
            "metadata": {
                "description": "The number of nodes in the HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "The type of the HDInsight cluster to create."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used to remotely access the cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "The password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        }
    },
    "variables": {
        "defaultApiVersion": "2017-06-01",
        "clusterStorageAccountName": "[concat(parameters('clusterName'),'store')]"
    },
    "resources": [
        {
        "name": "[variables('clusterStorageAccountName')]",
        "type": "Microsoft.Storage/storageAccounts",
        "location": "[parameters('location')]",
        "apiVersion": "[variables('defaultApiVersion')]",
        "dependsOn": [ ],
        "tags": { },
        "properties": {
            "accountType": "Standard_LRS"
        }
        },
    {
            "apiVersion": "2015-03-01-preview",
            "name": "[parameters('clusterName')]",
            "type": "Microsoft.HDInsight/clusters",
            "location": "[parameters('location')]",
            "dependsOn": [],
            "properties": {
                "clusterVersion": "[parameters('clusterVersion')]",
                "osType": "Linux",
                "tier": "standard",
                "clusterDefinition": {
                    "kind": "[parameters('clusterKind')]",
                    "configurations": {
                        "gateway": {
                            "restAuthCredential.isEnabled": true,
                            "restAuthCredential.username": "[parameters('clusterLoginUserName')]",
                            "restAuthCredential.password": "[parameters('clusterLoginPassword')]"
                        },
                        "spark-defaults": {
                            "spark.executor.cores": "2"
                        },
                        "spark-thrift-sparkconf": {
                            "spark.yarn.executor.memoryOverhead": "896"
                        }
                    }
                },
                "storageProfile": {
                    "storageaccounts": [
                        {
                            "name": "[concat(variables('clusterStorageAccountName'),'.blob.core.windows.net')]",
                            "isDefault": true,
                            "container": "[parameters('clusterName')]",
                            "key": "[listKeys(resourceId('Microsoft.Storage/storageAccounts', variables('clusterStorageAccountName')), variables('defaultApiVersion')).keys[0].value]"
                        }
                    ]
                },
                "computeProfile": {
                    "roles": [
                        {
                            "name": "headnode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 2,
                            "hardwareProfile": {
                                "vmSize": "Standard_D12"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                }
                            },
                            "virtualNetworkProfile": null,
                            "scriptActions": []
                        },
                        {
                            "name": "workernode",
                            "minInstanceCount": 1,
                            "targetInstanceCount": 4,
                            "hardwareProfile": {
                                "vmSize": "Standard_D4"
                            },
                            "osProfile": {
                                "linuxOperatingSystemProfile": {
                                    "username": "[parameters('sshUserName')]",
                                    "password": "[parameters('sshPassword')]"
                                    }
                                },
                                "virtualNetworkProfile": null,
                                "scriptActions": []
                            }
                        ]
                    }
                }
            }
        ]
    }
