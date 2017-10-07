---
title: aaaCreate Hadoop-sablonokkal - Azure HDInsight clusters |} Microsoft Docs
description: "Ismerje meg, hogyan toocreate fürtöket a HDInsight Resource Manager-sablonok segítségével"
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
ms.openlocfilehash: 92a6c1d888e401a11537dba34f188245ac17f448
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-hadoop-clusters-in-hdinsight-by-using-resource-manager-templates"></a><span data-ttu-id="ee0e2-103">Hadoop-fürtök létrehozása a Hdinsightban Resource Manager-sablonok használatával</span><span class="sxs-lookup"><span data-stu-id="ee0e2-103">Create Hadoop clusters in HDInsight by using Resource Manager templates</span></span>
[!INCLUDE [selector](../../includes/hdinsight-create-linux-cluster-selector.md)]

<span data-ttu-id="ee0e2-104">Ebből a cikkből megismerheti többféleképpen toocreate Azure HDInsight-fürtök az Azure Resource Manager-sablonok.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-104">In this article, you learn several ways toocreate Azure HDInsight clusters with Azure Resource Manager templates.</span></span> <span data-ttu-id="ee0e2-105">További információkért lásd: [Azure Resource Manager-sablon az alkalmazás központi telepítését](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-105">For more information, see [Deploy an application with Azure Resource Manager template](../azure-resource-manager/resource-group-template-deploy.md).</span></span> <span data-ttu-id="ee0e2-106">toolearn más fürt létrehozása eszközöket és szolgáltatásokat, kattintson a hello lapon választó hello legfelső lap vagy a további részletekért lásd a [Fürtlétrehozási módszerekhez](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-106">toolearn about other cluster creation tools and features, click hello tab selector on hello top of this page or see [Cluster creation methods](hdinsight-hadoop-provision-linux-clusters.md#cluster-setup-methods).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ee0e2-107">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ee0e2-107">Prerequisites</span></span>
[!INCLUDE [delete-cluster-warning](../../includes/hdinsight-delete-cluster-warning.md)]

<span data-ttu-id="ee0e2-108">Ez a cikk toofollow hello utasításait, szüksége lesz:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-108">toofollow hello instructions in this article, you'll need:</span></span>

* <span data-ttu-id="ee0e2-109">Egy [Azure-előfizetés](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-109">An [Azure subscription](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="ee0e2-110">Az Azure PowerShell és/vagy Azure CLI-t.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-110">Azure PowerShell and/or Azure CLI.</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-powershell-and-cli.md)]

### <a name="resource-manager-templates"></a><span data-ttu-id="ee0e2-111">Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="ee0e2-111">Resource Manager templates</span></span>
<span data-ttu-id="ee0e2-112">A Resource Manager-sablon lehetővé teszi az alkalmazás egyetlen, koordinált műveletben a következő egyszerű toocreate hello:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-112">A Resource Manager template makes it easy toocreate hello following for your application in a single, coordinated operation:</span></span>
* <span data-ttu-id="ee0e2-113">A HDInsight-fürtök és a tőle függő erőforrások (például hello alapértelmezett tárfiók)</span><span class="sxs-lookup"><span data-stu-id="ee0e2-113">HDInsight clusters and their dependent resources (such as hello default storage account)</span></span>
* <span data-ttu-id="ee0e2-114">További erőforrások (például az Azure SQL Database toouse Apache Sqoop)</span><span class="sxs-lookup"><span data-stu-id="ee0e2-114">Other resources (such as Azure SQL Database toouse Apache Sqoop)</span></span>

<span data-ttu-id="ee0e2-115">Hello sablonban hello alkalmazáshoz szükséges erőforrások hello határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-115">In hello template, you define hello resources that are needed for hello application.</span></span> <span data-ttu-id="ee0e2-116">Telepítési paraméterek tooinput értékeket különböző környezetekben is megadni.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-116">You also specify deployment parameters tooinput values for different environments.</span></span> <span data-ttu-id="ee0e2-117">hello sablon JSON és, hogy a központi telepítés tooconstruct értéket használ kifejezések áll.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-117">hello template consists of JSON and expressions that you use tooconstruct values for your deployment.</span></span>

<span data-ttu-id="ee0e2-118">HDInsight sablon minták található [Azure gyors üzembe helyezési sablonokat](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-118">You can find HDInsight template samples at [Azure Quickstart Templates](https://azure.microsoft.com/resources/templates/?term=hdinsight).</span></span> <span data-ttu-id="ee0e2-119">Használja a platformok közötti [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) a hello [erőforrás-kezelő bővítmény](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) vagy szöveges szerkesztő toosave hello sablont egy fájlba a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-119">Use cross-platform [Visual Studio Code](https://code.visualstudio.com/#alt-downloads) with hello [Resource Manager extension](https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools) or a text editor toosave hello template into a file on your workstation.</span></span> <span data-ttu-id="ee0e2-120">Megtudhatja, hogyan toocall hello más módszerekkel sablont.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-120">You learn how toocall hello template by using different methods.</span></span>

<span data-ttu-id="ee0e2-121">Erőforrás-kezelő sablonokkal kapcsolatos további információkért tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-121">For more information about Resource Manager templates, see hello following articles:</span></span>

* [<span data-ttu-id="ee0e2-122">Szerző Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="ee0e2-122">Author Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-authoring-templates.md)
* [<span data-ttu-id="ee0e2-123">Alkalmazás üzembe helyezése az Azure Resource Manager-sablonok</span><span class="sxs-lookup"><span data-stu-id="ee0e2-123">Deploy an application with Azure Resource Manager templates</span></span>](../azure-resource-manager/resource-group-template-deploy.md)

## <a name="generate-templates"></a><span data-ttu-id="ee0e2-124">Sablonok készítése</span><span class="sxs-lookup"><span data-stu-id="ee0e2-124">Generate templates</span></span>

<span data-ttu-id="ee0e2-125">Hello Azure-portál használatával konfigurálja a fürt összes hello tulajdonságait, és mentse hello sablon üzembe helyezése előtt.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-125">By using hello Azure portal, you can configure all hello properties of a cluster and then save hello template before deploying it.</span></span> <span data-ttu-id="ee0e2-126">Ezután újra felhasználhatja hello sablont.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-126">You can then reuse hello template.</span></span>

<span data-ttu-id="ee0e2-127">**toogenerate sablon hello Azure-portál használatával**</span><span class="sxs-lookup"><span data-stu-id="ee0e2-127">**toogenerate a template by using hello Azure portal**</span></span>

1. <span data-ttu-id="ee0e2-128">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-128">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="ee0e2-129">Kattintson a **új** hello bal oldali menüben kattintson **Eszközintelligencia + analitika**, és kattintson a **HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-129">Click **New** on hello left menu, click **Intelligence + analytics**, and then click **HDInsight**.</span></span>
3. <span data-ttu-id="ee0e2-130">Hajtsa végre a hello utasításokat tooenter tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-130">Follow hello instructions tooenter properties.</span></span> <span data-ttu-id="ee0e2-131">Használhatja bármelyik hello **Gyorslétrehozás** vagy hello **egyéni** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-131">You can use either hello **Quick create** or hello **Custom** option.</span></span>
4. <span data-ttu-id="ee0e2-132">A hello **összegzés** lapra, majd **töltse le a sablon és a paraméterek**:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-132">On hello **Summary** tab, click **Download template and parameters**:</span></span>

    ![HDInsight Hadoop létrehozása a fürt Resource Manager sablon letöltése](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download.png)

    <span data-ttu-id="ee0e2-134">Hello sablonfájl paraméterfájl és kód használt minták toodeploy hello sablon listájának megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-134">You see a list of hello template file, parameters file, and code samples used toodeploy hello template:</span></span>

    ![HDInsight Hadoop-fürt létrehozása Resource Manager sablon letöltési beállítások](./media/hdinsight-hadoop-create-linux-clusters-arm-templates/hdinsight-create-cluster-resource-manager-template-download-options.png)

    <span data-ttu-id="ee0e2-136">Itt hello sablon letöltése, mentse tooyour Sablonkönyvtár vagy hello sablon üzembe helyezése.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-136">From here, you can download hello template, save it tooyour template library, or deploy hello template.</span></span>

    <span data-ttu-id="ee0e2-137">tooaccess egy sablont a könyvtárban kattintson **további szolgáltatások** hello bal oldali menüben, majd kattintson a **sablonok** (alatt hello **más** kategória).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-137">tooaccess a template in your library, click **More services** from hello left menu, and then click **Templates** (under hello **Other** category).</span></span>

    > [!Note]
    > <span data-ttu-id="ee0e2-138">hello sablon és a paraméterek fájl együtt kell használni.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-138">hello template and parameters file must be used together.</span></span> <span data-ttu-id="ee0e2-139">Ellenkező esetben előfordulhat, hogy eredményt el nem várt.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-139">Otherwise, you might get unexpected results.</span></span> <span data-ttu-id="ee0e2-140">Például az alapértelmezett hello **clusterKind** tulajdonság értéke mindig **hadoop**, annak ellenére, hogy milyen, adja meg a hello sablon letöltése előtt.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-140">For example, hello default **clusterKind** property value is always **hadoop**, despite what you specify before you download hello template.</span></span>



## <a name="deploy-with-powershell"></a><span data-ttu-id="ee0e2-141">Üzembe helyezés a PowerShell-lel</span><span class="sxs-lookup"><span data-stu-id="ee0e2-141">Deploy with PowerShell</span></span>

<span data-ttu-id="ee0e2-142">Ez az eljárás egy Hadoop-fürt hdinsightban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-142">This procedure creates a Hadoop cluster in HDInsight.</span></span>

1. <span data-ttu-id="ee0e2-143">Hello JSON-fájl mentése hello [függelék](#appx-a-arm-template) tooyour munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-143">Save hello JSON file in hello [Appendix](#appx-a-arm-template) tooyour workstation.</span></span> <span data-ttu-id="ee0e2-144">Hello PowerShell-parancsfájlt, hello fájl neve: `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-144">In hello PowerShell script, hello file name is `C:\HDITutorials-ARM\hdinsight-arm-template.json`.</span></span>
2. <span data-ttu-id="ee0e2-145">Ha szükséges, állítsa a hello paramétereket és változókat.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-145">Set hello parameters and variables if needed.</span></span>
3. <span data-ttu-id="ee0e2-146">Hello sablon futtassa a következő PowerShell-parancsfájl hello használatával:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-146">Run hello template by using hello following PowerShell script:</span></span>

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
        # Connect tooAzure
        ####################################
        #region - Connect tooAzure subscription
        Write-Host "`nConnecting tooyour Azure subscription ..." -ForegroundColor Green
        try{Get-AzureRmContext}
        catch{Login-AzureRmAccount}
        #endregion

        # Create a resource group
        New-AzureRmResourceGroup -Name $resourceGroupName -Location $Location

        # Create cluster and hello dependent storage account
        $parameters = @{clusterName="$hdinsightClusterName"}

        New-AzureRmResourceGroupDeployment `
            -Name $armDeploymentName `
            -ResourceGroupName $resourceGroupName `
            -TemplateFile $templateFile `
            -TemplateParameterObject $parameters

        # List cluster
        Get-AzureRmHDInsightCluster -ResourceGroupName $resourceGroupName -ClusterName $hdinsightClusterName

    <span data-ttu-id="ee0e2-147">PowerShell parancsfájl hello csak hello fürt nevét konfigurálja.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-147">hello PowerShell script configures only hello cluster name.</span></span> <span data-ttu-id="ee0e2-148">hello tárfiók neve nem változtatható hello sablonban.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-148">hello storage account name is hard-coded in hello template.</span></span> <span data-ttu-id="ee0e2-149">Biztosan felszólító tooenter hello fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-149">You are prompted tooenter hello cluster user password.</span></span> <span data-ttu-id="ee0e2-150">(hello alapértelmezett felhasználónév az **admin**.) Biztosan is felszólító tooenter hello SSH felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-150">(hello default username is **admin**.) You are also prompted tooenter hello SSH user password.</span></span> <span data-ttu-id="ee0e2-151">(alapértelmezett SSH-felhasználónév hello **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="ee0e2-151">(hello default SSH username is **sshuser**.)</span></span>  

<span data-ttu-id="ee0e2-152">További információkért lásd: [telepítés a következő PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-152">For more information, see  [Deploy with PowerShell](../azure-resource-manager/resource-group-template-deploy.md#deploy-local-template).</span></span>

## <a name="deploy-with-cli"></a><span data-ttu-id="ee0e2-153">A parancssori felület telepítése</span><span class="sxs-lookup"><span data-stu-id="ee0e2-153">Deploy with CLI</span></span>
<span data-ttu-id="ee0e2-154">a következő minta hello Azure parancssori felület (CLI) használja.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-154">hello following sample uses Azure command-line interface (CLI).</span></span> <span data-ttu-id="ee0e2-155">A fürt és a függő tárfiókot és a tároló létrehoz egy Resource Manager-sablon meghívásával:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-155">It creates a cluster and its dependent storage account and container by calling a Resource Manager template:</span></span>

    azure login
    azure config mode arm
    azure group create -n hdi1229rg -l "East US"
    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "C:\HDITutorials-ARM\hdinsight-arm-template.json"

<span data-ttu-id="ee0e2-156">Rákérdezéses tooenter áll:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-156">You are prompted tooenter:</span></span>
* <span data-ttu-id="ee0e2-157">hello fürt neve.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-157">hello cluster name.</span></span>
* <span data-ttu-id="ee0e2-158">hello fürt felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-158">hello cluster user password.</span></span> <span data-ttu-id="ee0e2-159">(hello alapértelmezett felhasználónév az **admin**.)</span><span class="sxs-lookup"><span data-stu-id="ee0e2-159">(hello default username is **admin**.)</span></span>
* <span data-ttu-id="ee0e2-160">hello SSH felhasználói jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-160">hello SSH user password.</span></span> <span data-ttu-id="ee0e2-161">(alapértelmezett SSH-felhasználónév hello **sshuser**.)</span><span class="sxs-lookup"><span data-stu-id="ee0e2-161">(hello default SSH username is **sshuser**.)</span></span>

<span data-ttu-id="ee0e2-162">a következő kód hello beágyazott paramétereket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-162">hello following code provides inline parameters:</span></span>

    azure group deployment create --resource-group "hdi1229rg" --name "hdi1229" --template-file "c:\Tutorials\HDInsightARM\create-linux-based-hadoop-cluster-in-hdinsight.json" --parameters '{\"clusterName\":{\"value\":\"hdi1229\"},\"clusterLoginPassword\":{\"value\":\"Pass@word1\"},\"sshPassword\":{\"value\":\"Pass@word1\"}}'

## <a name="deploy-with-hello-rest-api"></a><span data-ttu-id="ee0e2-163">Hello REST API-t üzembe helyezéséhez</span><span class="sxs-lookup"><span data-stu-id="ee0e2-163">Deploy with hello REST API</span></span>
<span data-ttu-id="ee0e2-164">Lásd: [Deploy a REST API hello](../azure-resource-manager/resource-group-template-deploy-rest.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-164">See [Deploy with hello REST API](../azure-resource-manager/resource-group-template-deploy-rest.md).</span></span>

## <a name="deploy-with-visual-studio"></a><span data-ttu-id="ee0e2-165">Üzembe helyezés a Visual Studióval</span><span class="sxs-lookup"><span data-stu-id="ee0e2-165">Deploy with Visual Studio</span></span>
 <span data-ttu-id="ee0e2-166">Visual Studio toocreate egy erőforráscsoport-projekt használja, és telepítse azt tooAzure hello felhasználói felületen keresztül.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-166">Use Visual Studio toocreate a resource group project and deploy it tooAzure through hello user interface.</span></span> <span data-ttu-id="ee0e2-167">A projekt hello típusú erőforrások tooinclude lehetőséget választja.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-167">You select hello type of resources tooinclude in your project.</span></span> <span data-ttu-id="ee0e2-168">Ezeket az erőforrásokat automatikusan toohello Resource Manager-sablon.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-168">Those resources are automatically added toohello Resource Manager template.</span></span> <span data-ttu-id="ee0e2-169">hello projekt PowerShell parancsfájl toodeploy hello sablont is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-169">hello project also provides a PowerShell script toodeploy hello template.</span></span>

<span data-ttu-id="ee0e2-170">Egy bevezető toousing Visual Studio az erőforráscsoportokhoz, lásd: [létrehozása és telepítése a Visual Studio használatával Azure erőforráscsoport-sablonok a](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-170">For an introduction toousing Visual Studio with resource groups, see [Creating and deploying Azure resource groups through Visual Studio](../azure-resource-manager/vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="ee0e2-171">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="ee0e2-171">Troubleshoot</span></span>

<span data-ttu-id="ee0e2-172">Ha problémába ütközik a HDInsight-fürtök létrehozása során, tekintse meg [a hozzáférés-vezérlésre vonatkozó követelményeket](hdinsight-administer-use-portal-linux.md#create-clusters).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-172">If you run into issues with creating HDInsight clusters, see [access control requirements](hdinsight-administer-use-portal-linux.md#create-clusters).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ee0e2-173">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ee0e2-173">Next steps</span></span>
<span data-ttu-id="ee0e2-174">Ebben a cikkben megtanulta rendelkezik számos módon toocreate HDInsight-fürtöt.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-174">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="ee0e2-175">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="ee0e2-175">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="ee0e2-176">Például egy keresztül hello .NET ügyféloldali kódtár erőforrásokat üzembe helyezi, lásd: [erőforrások telepíteni a .NET-kódtárakra és egy sablon](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-176">For an example of deploying resources through hello .NET client library, see [Deploy resources by using .NET libraries and a template](../virtual-machines/windows/csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="ee0e2-177">Részletes példa az alkalmazások központi telepítése, lásd: [kiépítése és mikroszolgáltatások kiszámítható módon tudja az Azure-ban telepítheti](../app-service-web/app-service-deploy-complex-application-predictably.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-177">For an in-depth example of deploying an application, see [Provision and deploy microservices predictably in Azure](../app-service-web/app-service-deploy-complex-application-predictably.md).</span></span>
* <span data-ttu-id="ee0e2-178">A megoldás toodifferent környezetei telepítésével kapcsolatos útmutatásért lásd: [a Microsoft Azure-ban fejlesztési és tesztkörnyezetek](../solution-dev-test-environments.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-178">For guidance on deploying your solution toodifferent environments, see [Development and test environments in Microsoft Azure](../solution-dev-test-environments.md).</span></span>
* <span data-ttu-id="ee0e2-179">toolearn hello szakaszai hello Azure Resource Manager-sablon, lásd: [sablonok készítése](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-179">toolearn about hello sections of hello Azure Resource Manager template, see [Authoring templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>
* <span data-ttu-id="ee0e2-180">Az Azure Resource Manager-sablonokban használható hello függvények listáját lásd: [sablonfüggvényei](../azure-resource-manager/resource-group-template-functions.md).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-180">For a list of hello functions you can use in an Azure Resource Manager template, see [Template functions](../azure-resource-manager/resource-group-template-functions.md).</span></span>

## <a name="appendix-resource-manager-template-toocreate-a-hadoop-cluster"></a><span data-ttu-id="ee0e2-181">A függelék: Resource Manager sablon toocreate Hadoop-fürthöz</span><span class="sxs-lookup"><span data-stu-id="ee0e2-181">Appendix: Resource Manager template toocreate a Hadoop cluster</span></span>
<span data-ttu-id="ee0e2-182">hello következő Azure Resource Manager sablonnal hoz létre egy Linux-alapú Hadoop-fürt hello függő Azure storage-fiók.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-182">hello following Azure Resource Manager template creates a Linux-based Hadoop cluster with hello dependent Azure storage account.</span></span>

> [!NOTE]
> <span data-ttu-id="ee0e2-183">Ez a minta Hive metaadattárhoz és az Oozie metaadattárhoz konfigurációs információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-183">This sample includes configuration information for Hive metastore and Oozie metastore.</span></span> <span data-ttu-id="ee0e2-184">Távolítsa el a hello szakaszban, vagy hello szakasz hello sablon használata előtt konfigurálnia.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-184">Remove hello section or configure hello section before using hello template.</span></span>
>
>

    {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "clusterName": {
        "type": "string",
        "metadata": {
            "description": "hello name of hello HDInsight cluster toocreate."
        }
        },
        "clusterLoginUserName": {
        "type": "string",
        "defaultValue": "admin",
        "metadata": {
            "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
        }
        },
        "clusterLoginPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
        }
        },
        "sshUserName": {
        "type": "string",
        "defaultValue": "sshuser",
        "metadata": {
            "description": "These credentials can be used tooremotely access hello cluster."
        }
        },
        "sshPassword": {
        "type": "securestring",
        "metadata": {
            "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
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
            "description": "hello location where all azure resources will be deployed."
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
            "description": "hello type of hello HDInsight cluster toocreate."
        }
        },
        "clusterWorkerNodeCount": {
        "type": "int",
        "defaultValue": 2,
        "metadata": {
            "description": "hello number of nodes in hello HDInsight cluster."
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

## <a name="appendix-resource-manager-template-toocreate-a-spark-cluster"></a><span data-ttu-id="ee0e2-185">A függelék: Resource Manager sablon toocreate Spark-fürt</span><span class="sxs-lookup"><span data-stu-id="ee0e2-185">Appendix: Resource Manager template toocreate a Spark cluster</span></span>

<span data-ttu-id="ee0e2-186">Ez a témakör a Resource Manager-sablon használható toocreate egy HDInsight Spark-fürt.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-186">This section provides a Resource Manager template that you can use toocreate an HDInsight Spark cluster.</span></span> <span data-ttu-id="ee0e2-187">Ez a sablon tartalmazza konfigurációi `spark-defaults` és `spark-thrift-sparkconf` (az 1.6-os Spark-fürtök) és `spark2-defaults` és `spark2-thrift-sparkconf` (a Spark 2-fürtök).</span><span class="sxs-lookup"><span data-stu-id="ee0e2-187">This template includes configurations for `spark-defaults` and `spark-thrift-sparkconf` (for Spark 1.6 clusters) and `spark2-defaults` and `spark2-thrift-sparkconf` (for Spark 2 clusters).</span></span> <span data-ttu-id="ee0e2-188">Ezenkívül toothis, HDInsight számítja ki, és beállítja konfigurációk például `spark.executor.instances`, `spark.executor.memory`, és `spark.executor.cores` hello fürt mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-188">In addition toothis, HDInsight calculates and sets configurations such as `spark.executor.instances`, `spark.executor.memory`, and `spark.executor.cores` based on hello cluster size.</span></span> 

<span data-ttu-id="ee0e2-189">Bármely egy paraméter értéke a szakasz hello sablonba részeként, HDInsight nélkül kiszámításához, hello más paramétereket az hello ugyanabban a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-189">If you set any one parameter in a section as part of hello template itself, HDInsight does not calculate and set hello other parameters of hello same section.</span></span> <span data-ttu-id="ee0e2-190">Például paraméter `spark.executor.instances` hello van `spark-defaults` konfigurációs.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-190">For example, parameter `spark.executor.instances` is in hello  `spark-defaults` configuration.</span></span> <span data-ttu-id="ee0e2-191">Egy másik paraméter értéke (például `spark.yarn.exector.memoryOverhead`) a hello `spark-defaults` konfigurációs, HDInsight nélkül kiszámításához, hello `spark.executor.instances` paramétert is.</span><span class="sxs-lookup"><span data-stu-id="ee0e2-191">If you set another parameter (for example, `spark.yarn.exector.memoryOverhead`) in hello `spark-defaults` configuration, HDInsight does not calculate and set hello `spark.executor.instances` parameter as well.</span></span>

    {
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json#",
    "contentVersion": "0.9.0.0",
    "parameters": {
        "clusterName": {
            "type": "string",
            "metadata": {
                "description": "hello name of hello HDInsight cluster toocreate."
            }
        },
        "clusterLoginUserName": {
            "type": "string",
            "defaultValue": "admin",
            "metadata": {
                "description": "These credentials can be used toosubmit jobs toohello cluster and toolog into cluster dashboards."
            }
        },
        "clusterLoginPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
            }
        },
        "location": {
            "type": "string",
            "defaultValue": "southcentralus",
            "metadata": {
                "description": "hello location where all azure resources will be deployed."
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
                "description": "hello number of nodes in hello HDInsight cluster."
            }
        },
        "clusterKind": {
            "type": "string",
            "defaultValue": "SPARK",
            "metadata": {
                "description": "hello type of hello HDInsight cluster toocreate."
            }
        },
        "sshUserName": {
            "type": "string",
            "defaultValue": "sshuser",
            "metadata": {
                "description": "These credentials can be used tooremotely access hello cluster."
            }
        },
        "sshPassword": {
            "type": "securestring",
            "metadata": {
                "description": "hello password must be at least 10 characters in length and must contain at least one digit, one non-alphanumeric character, and one upper or lower case letter."
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
