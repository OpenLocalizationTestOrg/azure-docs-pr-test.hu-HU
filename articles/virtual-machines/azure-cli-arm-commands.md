---
title: "A Resource Manager módra az Azure parancssori felület parancsait |} Microsoft Docs"
description: "Az Azure parancssori felület (CLI) parancsok a Resource Manager üzembe helyezési modellel-erőforrások kezeléséhez"
services: virtual-machines-linux,virtual-machines-windows,virtual-network,mobile-services,cloud-services
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: be37da5b-72fe-41a1-9fa0-8937b69464ec
ms.service: multiple
ms.workload: multiple
ms.tgt_pltfrm: command-line-interface
ms.devlang: na
ms.topic: article
ms.date: 04/18/2017
ms.author: danlep
ms.openlocfilehash: be957651af78519f678321aec511b71cb18a85f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a><span data-ttu-id="505f7-103">Az Azure parancssori felület parancsait erőforrás-kezelő módban</span><span class="sxs-lookup"><span data-stu-id="505f7-103">Azure CLI commands in Resource Manager mode</span></span>
<span data-ttu-id="505f7-104">Ez a cikk ismerteti a szintaxis és a beállítások létrehozását és kezelését az Azure Resource Manager üzembe helyezési modellel Azure-erőforrások használata általában az Azure parancssori felület (CLI) parancsok.</span><span class="sxs-lookup"><span data-stu-id="505f7-104">This article provides syntax and options for Azure command-line interface (CLI) commands you'd commonly use to create and manage Azure resources in the Azure Resource Manager deployment model.</span></span> <span data-ttu-id="505f7-105">Ezek a parancsok a parancssori felület futtatásával Resource Manager (arm) módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="505f7-105">You access these commands by running the CLI in Resource Manager (arm) mode.</span></span> <span data-ttu-id="505f7-106">Ez nem teljes, és CLI-verziónak előfordulhat, hogy megjelenítése némileg eltérő parancsok vagy paraméterek.</span><span class="sxs-lookup"><span data-stu-id="505f7-106">This is not a complete reference, and your CLI version may show slightly different commands or parameters.</span></span> <span data-ttu-id="505f7-107">Azure-erőforrások és csoportok általános áttekintést, lásd: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="505f7-107">For a general overview of Azure resources and resource groups, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="505f7-108">Ez a cikk bemutatja erőforrás-kezelő módban parancsok az Azure parancssori felület, a más néven Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="505f7-108">This article shows Resource Manager mode commands in the Azure CLI, sometimes called Azure CLI 1.0.</span></span> <span data-ttu-id="505f7-109">Az erőforrás-kezelő modell használatát is megpróbálhatja a [Azure CLI 2.0](/cli/azure/install-az-cli2), a következő generációs többplatformos parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="505f7-109">To work in the Resource Manager model, you can also try the [Azure CLI 2.0](/cli/azure/install-az-cli2), our next generation multi-platform CLI.</span></span>
><span data-ttu-id="505f7-110">Bővebben a [régi és az új Azure CLIs](/cli/azure/old-and-new-clis).</span><span class="sxs-lookup"><span data-stu-id="505f7-110">Find out more about the [old and new Azure CLIs](/cli/azure/old-and-new-clis).</span></span>
>

<span data-ttu-id="505f7-111">Első lépésként először [az Azure parancssori felület telepítése](../cli-install-nodejs.md) és [csatlakozni az Azure-előfizetéshez](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="505f7-111">To get started, first [install the Azure CLI](../cli-install-nodejs.md) and [connect to your Azure subscription](../xplat-cli-connect.md).</span></span>

<span data-ttu-id="505f7-112">Aktuális parancs szintaxisa és a Resource Manager módra a parancssorban beállítások megtekintéséhez írja be a következőt `azure help` vagy egy adott parancs súgó megjelenítéséhez a `azure help [command]`.</span><span class="sxs-lookup"><span data-stu-id="505f7-112">For current command syntax and options at the command line in Resource Manager mode, type `azure help` or, to display help for a specific command, `azure help [command]`.</span></span> <span data-ttu-id="505f7-113">Is találhat CLI példák a dokumentáció létrehozása és kezelése az adott Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="505f7-113">Also find CLI examples in the documentation for creating and managing specific Azure services.</span></span>

<span data-ttu-id="505f7-114">Választható paraméterek: szögletes zárójelben jelennek meg (például `[parameter]`).</span><span class="sxs-lookup"><span data-stu-id="505f7-114">Optional parameters are shown in square brackets (for example, `[parameter]`).</span></span> <span data-ttu-id="505f7-115">Más paraméterek szükségesek.</span><span class="sxs-lookup"><span data-stu-id="505f7-115">All other parameters are required.</span></span>

<span data-ttu-id="505f7-116">Parancs-specifikus választható paraméterek: itt dokumentált lehetőségektől kívül három választható paraméterek: használható például lehetőségek és állapotkódok részletes kimenet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="505f7-116">In addition to command-specific optional parameters documented here, there are three optional parameters that can be used to display detailed output such as request options and status codes.</span></span> <span data-ttu-id="505f7-117">A `-v` paraméter biztosít részletes kimenet és a `-vv` paraméter részletesebb még részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="505f7-117">The `-v` parameter provides verbose output, and the `-vv` parameter provides even more detailed verbose output.</span></span> <span data-ttu-id="505f7-118">A `--json` beállítás kiírja a eredménye nyers json formátumban.</span><span class="sxs-lookup"><span data-stu-id="505f7-118">The `--json` option outputs the result in raw json format.</span></span>

## <a name="setting-the-resource-manager-mode"></a><span data-ttu-id="505f7-119">Az erőforrás-kezelő mód beállítása</span><span class="sxs-lookup"><span data-stu-id="505f7-119">Setting the Resource Manager mode</span></span>
<span data-ttu-id="505f7-120">A következő paranccsal Azure CLI erőforrás-kezelő módban parancsok engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="505f7-120">Use the following command to enable Azure CLI Resource Manager mode commands.</span></span>

    azure config mode arm

> [!NOTE]
> <span data-ttu-id="505f7-121">A CLI Azure Resource Manager módra és Azure szolgáltatásfelügyeleti módban kölcsönösen kizárják egymást.</span><span class="sxs-lookup"><span data-stu-id="505f7-121">The CLI's Azure Resource Manager mode and Azure Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="505f7-122">Ez azt jelenti, hogy egy módban létrehozott erőforrások nem tudja felügyelni a üzemmódról.</span><span class="sxs-lookup"><span data-stu-id="505f7-122">That is, resources created in one mode cannot be managed from the other mode.</span></span>
> 
> 

## <a name="azure-account-manage-your-account-information"></a><span data-ttu-id="505f7-123">Azure-fiók: a fiók adatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="505f7-123">azure account: Manage your account information</span></span>
<span data-ttu-id="505f7-124">Az Azure-előfizetés adatok az eszköz által a fiókjához.</span><span class="sxs-lookup"><span data-stu-id="505f7-124">Your Azure subscription information is used by the tool to connect to your account.</span></span>

<span data-ttu-id="505f7-125">**Az importált előfizetések felsorolása**</span><span class="sxs-lookup"><span data-stu-id="505f7-125">**List the imported subscriptions**</span></span>

    account list [options]

<span data-ttu-id="505f7-126">**Előfizetés részleteinek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="505f7-126">**Show details about a subscription**</span></span>  

    account show [options] [subscriptionNameOrId]

<span data-ttu-id="505f7-127">**A jelenlegi előfizetés beállítása**</span><span class="sxs-lookup"><span data-stu-id="505f7-127">**Set the current subscription**</span></span>

    account set [options] <subscriptionNameOrId>

<span data-ttu-id="505f7-128">**Távolítson el egy előfizetés vagy a környezet, vagy törölje az összes a tárolt fiók és a környezet adatait**</span><span class="sxs-lookup"><span data-stu-id="505f7-128">**Remove a subscription or environment, or clear all of the stored account and environment info**</span></span>  

    account clear [options]

<span data-ttu-id="505f7-129">**A fiók környezete kezeléséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-129">**Commands to manage your account environment**</span></span>  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-to-display-active-directory-objects"></a><span data-ttu-id="505f7-130">az Azure ad: Active Directory-objektumok megjelenítéséhez parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-130">azure ad: Commands to display Active Directory objects</span></span>
<span data-ttu-id="505f7-131">**Megtekintheti az active directory alkalmazásokat parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-131">**Commands to display active directory applications**</span></span>

    ad app create [options]
    ad app delete [options] <object-id>

<span data-ttu-id="505f7-132">**Az active directory-csoportok megjelenítéséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-132">**Commands to display active directory groups**</span></span>

    ad group list [options]
    ad group show [options]

<span data-ttu-id="505f7-133">**A parancsokat az active directory sub csoport vagy a tag adatainak megadása**</span><span class="sxs-lookup"><span data-stu-id="505f7-133">**Commands to provide an active directory sub group or member info**</span></span>

    ad group member list [options] [objectId]

<span data-ttu-id="505f7-134">**Parancsok az active directory szolgáltatás rendszerbiztonsági tagok megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="505f7-134">**Commands to display active directory service principals**</span></span>

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

<span data-ttu-id="505f7-135">**Parancsok megjelenítéséhez az active directory-felhasználók**</span><span class="sxs-lookup"><span data-stu-id="505f7-135">**Commands to display active directory users**</span></span>

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-to-manage-your-availability-sets"></a><span data-ttu-id="505f7-136">az Azure availset: a rendelkezésre állási készletek kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-136">azure availset: commands to manage your availability sets</span></span>
<span data-ttu-id="505f7-137">**Létrehozza a rendelkezésre állási készlet erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-137">**Creates an availability set within a resource group**</span></span>

    availset create [options] <resource-group> <name> <location> [tags]

<span data-ttu-id="505f7-138">**Felsorolja a rendelkezésre állási készletek erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-138">**Lists the availability sets within a resource group**</span></span>

    availset list [options] <resource-group>

<span data-ttu-id="505f7-139">**Lekérdezi a rendelkezésre állási csoporthoz erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-139">**Gets one availability set within a resource group**</span></span>

    availset show [options] <resource-group> <name>

<span data-ttu-id="505f7-140">**Törli a rendelkezésre állási csoporthoz erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-140">**Deletes one availability set within a resource group**</span></span>

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-to-manage-your-local-settings"></a><span data-ttu-id="505f7-141">az Azure config: a helyi beállításokat kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-141">azure config: commands to manage your local settings</span></span>
<span data-ttu-id="505f7-142">**Az Azure parancssori felület konfigurációs beállítások**</span><span class="sxs-lookup"><span data-stu-id="505f7-142">**List Azure CLI configuration settings**</span></span>

    config list [options]

<span data-ttu-id="505f7-143">**Egy konfigurációs beállítás törlése**</span><span class="sxs-lookup"><span data-stu-id="505f7-143">**Delete a config setting**</span></span>

    config delete [options] <name>

<span data-ttu-id="505f7-144">**Egy konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="505f7-144">**Update a config setting**</span></span>

    config set <name> <value>

<span data-ttu-id="505f7-145">**Az Azure parancssori felület használatának mód beállítása vagy `arm` vagy`asm`**</span><span class="sxs-lookup"><span data-stu-id="505f7-145">**Sets the Azure CLI working mode to either `arm` or `asm`**</span></span>

    config mode [options] <modename>


## <a name="azure-feature-commands-to-manage-account-features"></a><span data-ttu-id="505f7-146">az Azure-funkciót: fiók szolgáltatások kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-146">azure feature: commands to manage account features</span></span>
<span data-ttu-id="505f7-147">**Az előfizetéshez tartozó összes szolgáltatásához felsorolása**</span><span class="sxs-lookup"><span data-stu-id="505f7-147">**List all features available for your subscription**</span></span>

    feature list [options]

<span data-ttu-id="505f7-148">**Megjeleníti a szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="505f7-148">**Shows a feature**</span></span>

    feature show [options] <providerName> <featureName>

<span data-ttu-id="505f7-149">**Egy előzetesen megtekintett funkciója egy erőforrás-szolgáltató regisztrálása**</span><span class="sxs-lookup"><span data-stu-id="505f7-149">**Registers a previewed feature of a resource provider**</span></span>

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-to-manage-your-resource-groups"></a><span data-ttu-id="505f7-150">Azure-csoportok: a erőforráscsoportok kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-150">azure group: Commands to manage your resource groups</span></span>
<span data-ttu-id="505f7-151">**Létrehoz egy erőforráscsoportot**</span><span class="sxs-lookup"><span data-stu-id="505f7-151">**Creates a resource group**</span></span>

    group create [options] <name> <location>

<span data-ttu-id="505f7-152">**Az erőforráscsoporthoz címkék beállítása**</span><span class="sxs-lookup"><span data-stu-id="505f7-152">**Set tags to a resource group**</span></span>

    group set [options] <name> <tags>

<span data-ttu-id="505f7-153">**Egy erőforrás-csoport törlése**</span><span class="sxs-lookup"><span data-stu-id="505f7-153">**Deletes a resource group**</span></span>

    group delete [options] <name>

<span data-ttu-id="505f7-154">**Az előfizetéshez tartozó erőforráscsoportok listája**</span><span class="sxs-lookup"><span data-stu-id="505f7-154">**Lists the resource groups for your subscription**</span></span>

    group list [options]

<span data-ttu-id="505f7-155">**Megjeleníti az előfizetéshez tartozó erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="505f7-155">**Shows a resource group for your subscription**</span></span>

    group show [options] <name>

<span data-ttu-id="505f7-156">**Erőforrás naplóiban kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-156">**Commands to manage resource group logs**</span></span>

    group log show [options] [name]

<span data-ttu-id="505f7-157">**A központi telepítés egy erőforráscsoportban kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-157">**Commands to manage your deployment in a resource group**</span></span>

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

<span data-ttu-id="505f7-158">**A helyi vagy gallery resource csoport sablon kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-158">**Commands to manage your local or gallery resource group template**</span></span>

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-to-manage-your-hdinsight-clusters"></a><span data-ttu-id="505f7-159">az Azure hdinsight: parancsokat küld a HDInsight-fürtök kezelése</span><span class="sxs-lookup"><span data-stu-id="505f7-159">azure hdinsight: Commands to manage your HDInsight clusters</span></span>
<span data-ttu-id="505f7-160">**Parancsok létrehozásához vagy a fürt konfigurációs fájl felvétele**</span><span class="sxs-lookup"><span data-stu-id="505f7-160">**Commands to create or add to a cluster configuration file**</span></span>

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

<span data-ttu-id="505f7-161">Példa: A futtatást, ha a fürt létrehozása a parancsfájlművelet tartalmazó konfigurációs fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="505f7-161">Example: Create a configuration file that contains a script action to run when creating a cluster.</span></span>

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

<span data-ttu-id="505f7-162">**A parancs egy erőforráscsoportot a fürt létrehozása**</span><span class="sxs-lookup"><span data-stu-id="505f7-162">**Command to create a cluster in a resource group**</span></span>

    hdinsight cluster create [options] <clusterName>

<span data-ttu-id="505f7-163">Példa: Egy Storm létrehozása Linux-fürt</span><span class="sxs-lookup"><span data-stu-id="505f7-163">Example: Create a Storm on Linux cluster</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="505f7-164">Példa: Hozzon létre egy fürtöt egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="505f7-164">Example: Create a cluster with a script action</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting the request to create cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="505f7-165">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-165">Parameter options:</span></span>

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       The name of the resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for the cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url to use for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key to the storage account to use for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in the storage account to use for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for the cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes to use for the cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for the cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for the cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for the cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for the cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In the format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for the external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for the external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for the external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for the external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for the external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for the external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for the external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for the external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    The subscription id
    --tags <tags>                                              Tags to set to the cluster.
    Can be multiple.
    In the format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


<span data-ttu-id="505f7-166">**Parancsot a fürt törlése**</span><span class="sxs-lookup"><span data-stu-id="505f7-166">**Command to delete a cluster**</span></span>

    hdinsight cluster delete [options] <clusterName>

<span data-ttu-id="505f7-167">**Parancsot a fürt részleteinek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="505f7-167">**Command to show cluster details**</span></span>

    hdinsight cluster show [options] <clusterName>

<span data-ttu-id="505f7-168">**Paranccsal listát készíthet az összes fürtök (a megadott erőforráscsoport, ha a megadott)**</span><span class="sxs-lookup"><span data-stu-id="505f7-168">**Command to list all clusters (in a specific resource group, if provided)**</span></span>

    hdinsight cluster list [options]

<span data-ttu-id="505f7-169">**A fürt átméretezése parancs**</span><span class="sxs-lookup"><span data-stu-id="505f7-169">**Command to resize a cluster**</span></span>

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

<span data-ttu-id="505f7-170">**A parancs a fürt HTTP-hozzáférés engedélyezéséhez**</span><span class="sxs-lookup"><span data-stu-id="505f7-170">**Command to enable HTTP access for a cluster**</span></span>

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

<span data-ttu-id="505f7-171">**Parancsot a fürt HTTP-hozzáférés letiltása**</span><span class="sxs-lookup"><span data-stu-id="505f7-171">**Command to disable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-http-access [options] <clusterName>

<span data-ttu-id="505f7-172">**A parancs a fürt RDP-hozzáférés engedélyezéséhez**</span><span class="sxs-lookup"><span data-stu-id="505f7-172">**Command to enable RDP access for a cluster**</span></span>

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

<span data-ttu-id="505f7-173">**Parancsot a fürt HTTP-hozzáférés letiltása**</span><span class="sxs-lookup"><span data-stu-id="505f7-173">**Command to disable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-to-monitoring-insights-events-alert-rules-autoscale-settings-metrics"></a><span data-ttu-id="505f7-174">az Azure insights: parancsok kapcsolódó elemzések (eseményeket, a riasztási szabályok, automatikus skálázási beállításokat, metrikák) figyelése</span><span class="sxs-lookup"><span data-stu-id="505f7-174">azure insights: Commands related to monitoring Insights (events, alert rules, autoscale settings, metrics)</span></span>
<span data-ttu-id="505f7-175">**Az előfizetés, a correlationId, egy erőforráscsoport, erőforrás vagy erőforrás-szolgáltató műveletnaplók beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-175">**Retrieve operation logs for a subscription, a correlationId, a resource group, resource, or resource provider**</span></span>

    insights logs list [options]

## <a name="azure-location-commands-to-get-the-available-locations-for-all-resource-types"></a><span data-ttu-id="505f7-176">Azure-beli hely: parancsokat minden erőforrástípus esetén elérhető helyeinek lekéréséhez.</span><span class="sxs-lookup"><span data-stu-id="505f7-176">azure location: Commands to get the available locations for all resource types</span></span>
<span data-ttu-id="505f7-177">**A helyeket felsorolása**</span><span class="sxs-lookup"><span data-stu-id="505f7-177">**List the available locations**</span></span>

    location list [options]

## <a name="azure-network-commands-to-manage-network-resources"></a><span data-ttu-id="505f7-178">Azure-hálózat: hálózati erőforrások kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-178">azure network: Commands to manage network resources</span></span>
<span data-ttu-id="505f7-179">**Virtuális hálózatok kezeléséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-179">**Commands to manage virtual networks**</span></span>

    network vnet create [options] <resource-group> <name> <location>
<span data-ttu-id="505f7-180">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="505f7-180">Creates a virtual network.</span></span> <span data-ttu-id="505f7-181">Az alábbi példa azt newvnet nevű erőforrás csoport myresourcegroup USA nyugati régiója régióban a virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="505f7-181">In the following example we create a virtual network named newvnet for resource group myresourcegroup in the West US region.</span></span>

    azure network vnet create myresourcegroup newvnet "west us"
    info:    Executing command network vnet create
    + Looking up virtual network "newvnet"
    + Creating virtual network "newvnet"
     Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet create command OK


<span data-ttu-id="505f7-182">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-182">Parameter options:</span></span>

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      the name of the resource group
     -n, --name <name>                          the name of the virtual network
     -l, --location <location>                  the location
     -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            the comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          the tags set on this virtual network.
      Can be multiple. In the format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

<span data-ttu-id="505f7-183">Frissíti a virtuális hálózati konfiguráció erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="505f7-183">Updates a virtual network configuration within a resource group.</span></span>

    azure network vnet set myresourcegroup newvnet

    info:    Executing command network vnet set
    + Looking up virtual network "newvnet"
    + Updating virtual network "newvnet"
    + Loading virtual network state
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet set command OK

<span data-ttu-id="505f7-184">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-184">Parameter options:</span></span>

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      the name of the resource group
       -n, --name <name>                          the name of the virtual network
       -a, --address-prefixes <address-prefixes>  the comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended to the current list of address prefixes.
        The address prefixes in this list should not overlap between them.
        The address prefixes in this list should not overlap with existing address prefixes in the vnet.

       -d, --dns-servers [dns-servers]            the comma separated list of DNS servers IP addresses.
        This list will be appended to the current list of DNS server IP addresses.

       -t, --tags <tags>                          the tags set on this virtual network.
        Can be multiple. In the format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended to the current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          the subscription identifier
<BR>

    network vnet list [options] <resource-group>

<span data-ttu-id="505f7-185">A parancs felsorolja egy erőforráscsoportban található összes virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="505f7-185">The command lists all virtual networks in a resource group.</span></span>

    C:\>azure network vnet list myresourcegroup

    info:    Executing command network vnet list
    + Listing virtual networks
        data:    ID
       Name      Location  Address prefixes  DNS servers
    data:    -------------------------------------------------------------------
    ------  --------  --------  ----------------  -----------
    data:    /subscriptions/###############################/resourceGroups/
    wvnet   newvnet   westus    10.0.0.0/8
    info:    network vnet list command OK

<span data-ttu-id="505f7-186">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-186">Parameter options:</span></span>

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  the name of the resource group
      -s, --subscription <subscription>      the subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
<span data-ttu-id="505f7-187">A parancs megjeleníti a virtuális hálózati tulajdonságok erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="505f7-187">The command shows the virtual network properties in a resource group.</span></span>

    azure network vnet show -g myresourcegroup -n newvnet

    info:    Executing command network vnet show
    + Looking up virtual network "newvnet"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet
    data:    Name:                 newvnet
    data:    Type:                 Microsoft.Network/virtualNetworks
    data:    Location:             westus
    data:    Tags:
    data:    Provisioning state:   Succeeded
    data:    Address prefixes:
    data:     10.0.0.0/8
    data:    DNS servers:
    data:    Subnets:
    data:
    info:    network vnet show command OK
<BR>

    network vnet delete [options] <resource-group> <name>
<span data-ttu-id="505f7-188">A parancs eltávolítja a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="505f7-188">The command removes a virtual network.</span></span>

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

<span data-ttu-id="505f7-189">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-189">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier


<span data-ttu-id="505f7-190">**Parancsok futtatásával kezelheti a virtuális hálózati alhálózat**</span><span class="sxs-lookup"><span data-stu-id="505f7-190">**Commands to manage virtual network subnets**</span></span>

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="505f7-191">Egy másik alhálózat hozzáadása egy meglévő virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="505f7-191">Adds another subnet to an existing virtual network.</span></span>

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up the subnet "subnet"
    + Creating subnet "subnet"
    + Looking up the subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

<span data-ttu-id="505f7-192">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-192">Parameter options:</span></span>

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            the name of the resource group
     -e, --vnet-name <vnet-name>                                      the name of the virtual network
     -n, --name <name>                                                the name of the subnet
     -a, --address-prefix <address-prefix>                            the address prefix
     -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  the network security group name
     -s, --subscription <subscription>                                the subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="505f7-193">Beállítja egy adott virtuális hálózati alhálózat erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="505f7-193">Sets a specific virtual network subnet within a resource group.</span></span>

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

<span data-ttu-id="505f7-194">Megjeleníti egy adott virtuális hálózati erőforráscsoporton belül minden virtuális hálózati alhálózat.</span><span class="sxs-lookup"><span data-stu-id="505f7-194">Lists all the virtual network subnets for a specific virtual network within a resource group.</span></span>

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up the subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
<span data-ttu-id="505f7-195">Virtuális hálózati alhálózat tulajdonságainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="505f7-195">Displays virtual network subnet properties</span></span>

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up the subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

<span data-ttu-id="505f7-196">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-196">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -e, --vnet-name <vnet-name>            the name of the virtual network
    -n, --name <name>                      the name of the subnet
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
<span data-ttu-id="505f7-197">Egy alhálózat eltávolítja a meglévő virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="505f7-197">Removes a subnet from an existing virtual network.</span></span>

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up the subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

<span data-ttu-id="505f7-198">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-198">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -e, --vnet-name <vnet-name>            the name of the virtual network
     -n, --name <name>                      the subnet name
     -s, --subscription <subscription>      the subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

<span data-ttu-id="505f7-199">**A terheléselosztók kezeléséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-199">**Commands to manage load balancers**</span></span>

    network lb create [options] <resource-group> <name> <location>
<span data-ttu-id="505f7-200">Jönnek létre a load balancer.</span><span class="sxs-lookup"><span data-stu-id="505f7-200">Creates a load balancer set.</span></span>

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up the load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

<span data-ttu-id="505f7-201">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-201">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -l, --location <location>              the location
    -t, --tags <tags>                      the list of tags.
     Can be multiple. In the format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb list [options] <resource-group>
<span data-ttu-id="505f7-202">Felsorolja a Load balancer erőforrások erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="505f7-202">Lists Load balancer resources within a resource group.</span></span>

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting the load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

<span data-ttu-id="505f7-203">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-203">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

<span data-ttu-id="505f7-204">Megjeleníti az erőforráscsoporton belül meghatározott típusú terheléselosztójának terheléselosztó adatok betöltésekor</span><span class="sxs-lookup"><span data-stu-id="505f7-204">Displays load balancer information of a specific load balancer within a resource group</span></span>

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

<span data-ttu-id="505f7-205">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-205">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

<span data-ttu-id="505f7-206">Törölje a load balancer erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="505f7-206">Delete load balancer resources.</span></span>

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up the load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

<span data-ttu-id="505f7-207">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-207">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -n, --name <name>                      the name of the load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="505f7-208">**Terheléselosztó mintavételt kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-208">**Commands to manage probes of a load balancer**</span></span>

    network lb probe create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-209">A mintavétel konfiguráció állapotadatok létrehozása a terheléselosztóban.</span><span class="sxs-lookup"><span data-stu-id="505f7-209">Create the probe configuration for health status in the load balancer.</span></span> <span data-ttu-id="505f7-210">Vegye figyelembe a parancs futtatásához, a terheléselosztó előtérbeli ip-erőforrás (kivételét parancs "azure-hálózat előtér-IP-címe" IP-címet a terheléselosztóhoz,) van szükség.</span><span class="sxs-lookup"><span data-stu-id="505f7-210">Keep in mind to run this command, your load balancer requires a frontend-ip resource (Check out command "azure network frontend-ip" to assign an ip address to load balancer).</span></span>

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

<span data-ttu-id="505f7-211">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-211">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -p, --protocol <protocol>              the probe protocol
    -o, --port <port>                      the probe port
    -f, --path <path>                      the probe path
    -i, --interval <interval>              the probe interval in seconds
    -c, --count <count>                    the number of probes
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-212">Egy meglévő terheléselosztói mintavétel frissítése az új értékeivel.</span><span class="sxs-lookup"><span data-stu-id="505f7-212">Updates an existing load balancer probe with new values for it.</span></span>

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

<span data-ttu-id="505f7-213">A paraméter-beállítások</span><span class="sxs-lookup"><span data-stu-id="505f7-213">Parameter options</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the probe
    -e, --new-probe-name <new-probe-name>  the new name of the probe
    -p, --protocol <protocol>              the new value for probe protocol
    -o, --port <port>                      the new value for probe port
    -f, --path <path>                      the new value for probe path
    -i, --interval <interval>              the new value for probe interval in seconds
    -c, --count <count>                    the new value for number of probes
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

<span data-ttu-id="505f7-214">A mintavétel tulajdonságok load balancer készletének listázásához.</span><span class="sxs-lookup"><span data-stu-id="505f7-214">List the probe properties for a load balancer set.</span></span>

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up the load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

<span data-ttu-id="505f7-215">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-215">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-216">Eltávolítja a mintavétel létrehozása a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="505f7-216">Removes the probe created for the load balancer.</span></span>

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up the load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

<span data-ttu-id="505f7-217">**Parancsok futtatásával kezelheti a terheléselosztó előtérbeli IP-konfigurációk**</span><span class="sxs-lookup"><span data-stu-id="505f7-217">**Commands to manage frontend ip configurations of a load balancer**</span></span>

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-218">A load balancer készlet előtérbeli IP-konfiguráció létrehozása</span><span class="sxs-lookup"><span data-stu-id="505f7-218">Creates a frontend IP configuration to an existing load balancer set.</span></span>

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up the load balancer "mylb"
    + Looking up the subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/Myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    /frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:           10.0.1.4
    data:    Subnet:                       id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Public IP address:
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip create command OK

<BR>

    network lb frontend-ip set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-219">Frissíti egy előtérbeli IP-cím egy meglévő konfigurációját. Az alábbi parancs hozzáadja egy meglévő terheléselosztási terheléselosztó előtérbeli IP-myfrontendip nevű mypubip5 nevű nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="505f7-219">Updates an existing configuration of a frontend IP.The command below adds a public IP called mypubip5 to an existing load balancer frontend IP named myfrontendip.</span></span>

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up the load balancer "mylb"
    + Looking up the public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Name:                         myfrontendip
    data:    Type:                         Microsoft.Network/loadBalancers/frontendIPConfigurations
    data:    Provisioning state:           Succeeded
    data:    Private IP allocation method: Dynamic
    data:    Private IP address:
    data:    Subnet:
    data:    Public IP address:            id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mypubip5
    data:    Inbound NAT rules
    data:    Outbound NAT rules
    data:    Load balancing rules
    data:
    info:    network lb frontend-ip set command OK

<span data-ttu-id="505f7-220">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-220">Parameter options:</span></span>

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              the name of the resource group
    -l, --lb-name <lb-name>                                            the name of the load balancer
    -n, --name <name>                                                  the name of the frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      the private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  the private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  the public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              the public ip name.
    This public ip must exist in the same resource group as the lb.
    Please use public-ip-id if that is not the case.
    -b, --subnet-id <subnet-id>                                        the subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    the subnet name
    -m, --vnet-name <vnet-name>                                        the virtual network name.
    This virtual network must exist in the same resource group as the lb.
    Please use subnet-id if that is not the case.
    -s, --subscription <subscription>                                  the subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

<span data-ttu-id="505f7-221">Felsorolja az összes az előtérbeli IP-erőforrás a terheléselosztó konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="505f7-221">Lists all the frontend IP resources configured for the load balancer.</span></span>

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

<span data-ttu-id="505f7-222">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-222">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-223">A terheléselosztóhoz társított előtérbeli IP-objektum törlése</span><span class="sxs-lookup"><span data-stu-id="505f7-223">Deletes the frontend IP object associated to load balancer</span></span>

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up the load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

<span data-ttu-id="505f7-224">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-224">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="505f7-225">**Háttércímkészletek terheléselosztó kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-225">**Commands to manage backend address pools of a load balancer**</span></span>

    network lb address-pool create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-226">Hozzon létre egy háttér címkészletet egy terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="505f7-226">Create a backend address pool for a load balancer.</span></span>

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

<span data-ttu-id="505f7-227">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-227">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -s, --subscription <subscription>      the subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

<span data-ttu-id="505f7-228">Az adott erőforráscsoport lista készlet háttérbeli IP-címtartomány</span><span class="sxs-lookup"><span data-stu-id="505f7-228">List backend IP address pool range for a specific resource group</span></span>

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up the load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

<span data-ttu-id="505f7-229">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-229">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  the name of the resource group
     -l, --lb-name <lb-name>                the name of the load balancer
     -s, --subscription <subscription>      the subscription identifier

<BR>
    <span data-ttu-id="505f7-230">hálózati terheléselosztó-címkészlet törlése [kapcsolók] < erőforráscsoport >< lb-neve ><name></span><span class="sxs-lookup"><span data-stu-id="505f7-230">network lb address-pool delete [options] <resource-group> <lb-name> <name></span></span>

<span data-ttu-id="505f7-231">Eltávolítja a háttér IP-címkészlet tartománya erőforrás terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="505f7-231">Removes the backend IP pool range resource from load balancer.</span></span>

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up the load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

<span data-ttu-id="505f7-232">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-232">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="505f7-233">**Parancsok futtatásával kezelheti szabály betöltése**</span><span class="sxs-lookup"><span data-stu-id="505f7-233">**Commands to manage load balancer rules**</span></span>

    network lb rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-234">Terheléselosztási szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="505f7-234">Create load balancer rules.</span></span>

<span data-ttu-id="505f7-235">Létrehozhat olyan terheléselosztó szabályhoz konfigurálása az előtér-végpont a terheléselosztó és a háttér-készlet címtartomány a bejövő hálózati forgalom fogadására.</span><span class="sxs-lookup"><span data-stu-id="505f7-235">You can create a load balancer rule configuring the frontend endpoint for the load balancer and the backend address pool range to receive the incoming network traffic.</span></span> <span data-ttu-id="505f7-236">Beállítások is az előtérbeli IP-végponton által használt portokat és a háttér-készlet címtartomány.</span><span class="sxs-lookup"><span data-stu-id="505f7-236">Settings also include the ports for frontend IP endpoint and ports for the backend address pool range.</span></span>

<span data-ttu-id="505f7-237">A következő példa bemutatja, hogyan hozzon létre olyan terheléselosztó szabályhoz, az előtér-végpont 80 TCP porton és történik a hálózati forgalom terheléselosztása elküldését a háttér-készlet címtartomány a 8080-as porton figyel.</span><span class="sxs-lookup"><span data-stu-id="505f7-237">The following example shows how to create a load balancer rule,  the frontend endpoint listening to port 80 TCP and load balancing network traffic sending to port 8080 for the backend address pool range.</span></span>

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mylbrule
    data:    Name:                      mylbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule create command OK

<BR>

    network lb rule set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-238">Egy meglévő terheléselosztási szabály egy adott erőforráscsoportban beállítása frissíti.</span><span class="sxs-lookup"><span data-stu-id="505f7-238">Updates an existing load balancer rule set in a specific resource group.</span></span> <span data-ttu-id="505f7-239">A következő példában azt állapotról a szabály neve mylbrule mynewlbrule.</span><span class="sxs-lookup"><span data-stu-id="505f7-239">In the following example, we changed the rule name from mylbrule to mynewlbrule.</span></span>

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Loading rule state
    data:    Id:                        /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/loadBalancingRules/mynewlbrule
    data:    Name:                      mynewlbrule
    data:    Type:                      Microsoft.Network/loadBalancers/loadBalancingRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP configuration: /subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend address pool:      id=/subscriptions/###############################/resourceGroups/yresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    data:    Protocol:                  Tcp
    data:    Frontend port:             80
    data:    Backend port:              8080
    data:    Enable floating IP:        false
    data:    Idle timeout in minutes:   10
    data:    Probes
    data:
    info:    network lb rule set command OK

<span data-ttu-id="505f7-240">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-240">Parameter options:</span></span>

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              the name of the resource group
    -l, --lb-name <lb-name>                            the name of the load balancer
    -n, --name <name>                                  the name of the rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          the rule protocol
    -f, --frontend-port <frontend-port>                the frontend port
    -b, --backend-port <backend-port>                  the backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  the idle timeout in minutes
    -a, --probe-name [probe-name]                      the name of the probe defined in the same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          the name of the frontend ip configuration in the same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of the backend address pool defined in the same load balancer
    -s, --subscription <subscription>                  the subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

<span data-ttu-id="505f7-241">Felsorolja az összes terheléselosztó szabály egy adott erőforráscsoportban terheléselosztó beállítva betölteni.</span><span class="sxs-lookup"><span data-stu-id="505f7-241">Lists all load balancer rules configured for a load balancer in a specific resource group.</span></span>

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up the load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

<span data-ttu-id="505f7-242">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-242">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-243">A terheléselosztási szabály törlése.</span><span class="sxs-lookup"><span data-stu-id="505f7-243">Deletes a load balancer rule.</span></span>

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up the load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

<span data-ttu-id="505f7-244">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-244">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="505f7-245">**Parancsok futtatásával kezelheti a terheléselosztó bejövő forgalmat kezelő NAT-szabályok**</span><span class="sxs-lookup"><span data-stu-id="505f7-245">**Commands to manage load balancer inbound NAT rules**</span></span>

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-246">Terheléselosztó bejövő NAT-szabályban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="505f7-246">Creates an inbound NAT rule for load balancer.</span></span>

<span data-ttu-id="505f7-247">A következő példa egy NAT-szabály (amely a korábban már definiálva lett az "azure-hálózat előtér-IP-címe" paranccsal) előtérbeli IP-címről egy figyelőportja bejövő és kimenő port, amelyeket a hálózati forgalom elküldése használ a load balancer létrehozott.</span><span class="sxs-lookup"><span data-stu-id="505f7-247">In the following example  we created a NAT rule from frontend IP (which was previously defined using the "azure network frontend-ip" command) with an inbound listening port and outbound port that the load balancer uses to send the network traffic.</span></span>

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              80
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule create command OK

<span data-ttu-id="505f7-248">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-248">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id <vm-id>                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.This VM must exist in the same resource group as the lb.
    Please use vm-id if that is not the case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
<span data-ttu-id="505f7-249">Egy meglévő bejövő forgalmat kezelő nat-szabály frissítése.</span><span class="sxs-lookup"><span data-stu-id="505f7-249">Updates an existing inbound nat rule.</span></span> <span data-ttu-id="505f7-250">A következő példában változtattuk a bejövő figyelő port a 80-as a 81-es.</span><span class="sxs-lookup"><span data-stu-id="505f7-250">In the following example, we changed the inbound listening port from 80 to 81.</span></span>

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up the load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up the load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/inboundNatRules/myinboundnat
    data:    Name:                      myinboundnat
    data:    Type:                      Microsoft.Network/loadBalancers/inboundNatRules
    data:    Provisioning state:        Succeeded
    data:    Frontend IP Configuration: id=/subscriptions/###############################/resourceGroups/group-1/providers/Microsoft.Network/loadBalancers/mylb/frontendIPConfigurations/myfrontendip
    data:    Backend IP configuration
    data:    Protocol                   Tcp
    data:    Frontend port              81
    data:    Backend port               8080
    data:    Enable floating IP         false
    info:    network lb inbound-nat-rule set command OK

<span data-ttu-id="505f7-251">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-251">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          the name of the resource group
    -l, --lb-name <lb-name>                        the name of the load balancer
    -n, --name <name>                              the name of the inbound NAT rule
    -p, --protocol <protocol>                      the rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            the frontend port [0-65535]
    -b, --backend-port <backend-port>              the backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                the name of the frontend ip configuration
    -m, --vm-id [vm-id]                            the VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        the VM name.
    This virtual machine must exist in the same resource group as the lb.
    Please use vm-id if that is not the case
    -s, --subscription <subscription>              the subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

<span data-ttu-id="505f7-252">Felsorolja az összes bejövő nat-szabályok a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="505f7-252">Lists all inbound nat rules for load balancer.</span></span>

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up the load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

<span data-ttu-id="505f7-253">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-253">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -s, --subscription <subscription>      the subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="505f7-254">A load balancer egy adott erőforráscsoportban található NAT-szabály törlése.</span><span class="sxs-lookup"><span data-stu-id="505f7-254">Deletes NAT rule for the load balancer in a specific resource group.</span></span>

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up the load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

<span data-ttu-id="505f7-255">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-255">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -l, --lb-name <lb-name>                the name of the load balancer
    -n, --name <name>                      the name of the inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier

<span data-ttu-id="505f7-256">**Nyilvános IP-címek kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-256">**Commands to manage public ip addresses**</span></span>

    network public-ip create [options] <resource-group> <name> <location>
<span data-ttu-id="505f7-257">Létrehoz egy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="505f7-257">Creates a public ip resource.</span></span> <span data-ttu-id="505f7-258">A nyilvános IP-cím erőforrás létrehoz és rendelheti hozzá az egy tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="505f7-258">You will create the public ip resource and associate to a domain name.</span></span>

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up the public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Dynamic
    data:    Idle timeout:         4
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip create command OK


<span data-ttu-id="505f7-259">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-259">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -l, --location <location>                    the location
    -d, --domain-name-label <domain-name-label>  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            the subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
<span data-ttu-id="505f7-260">Frissíti a meglévő nyilvános IP-cím erőforrás tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="505f7-260">Updates the properties of an existing public ip resource.</span></span> <span data-ttu-id="505f7-261">Az alábbi példa azt változása a nyilvános IP-cím dinamikus statikus.</span><span class="sxs-lookup"><span data-stu-id="505f7-261">In the following example we changed the public IP address from Dynamic to Static.</span></span>

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up the public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip1
    data:    Name:                 mytestpublicip1
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip set command OK

<span data-ttu-id="505f7-262">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-262">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        the name of the resource group
    -n, --name <name>                            the name of the public ip
    -d, --domain-name-label [domain-name-label]  the domain name label.
    This set DNS to <domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  the allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              the idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            the reverse fqdn
    -t, --tags <tags>                            the list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            the subscription identifier

<br>
    <span data-ttu-id="505f7-263">hálózati nyilvános ip-[kapcsolók] < erőforráscsoport > lista minden nyilvános IP-erőforrások erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="505f7-263">network public-ip list [options] <resource-group> Lists all public IP resources within a resource group.</span></span>

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting the public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

<span data-ttu-id="505f7-264">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-264">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -s, --subscription <subscription>      the subscription identifier
<BR>
    <span data-ttu-id="505f7-265">hálózati nyilvános ip-megjelenítése [kapcsolók] < erőforráscsoport ><name></span><span class="sxs-lookup"><span data-stu-id="505f7-265">network public-ip show [options] <resource-group> <name></span></span>

<span data-ttu-id="505f7-266">A nyilvános IP-cím erőforrás egy erőforráscsoportban nyilvános IP-cím tulajdonságainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="505f7-266">Displays public ip properties for a public ip resource within a resource group.</span></span>

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up the public ip "mytestpublicip1"
    data:    Id:                   /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/publicIPAddresses/mytestpublicip
    data:    Name:                 mytestpublicip
    data:    Type:                 Microsoft.Network/publicIPAddresses
    data:    Location:             eastus
    data:    Provisioning state:   Succeeded
    data:    Allocation method:    Static
    data:    Idle timeout:         4
    data:    IP Address:           (static IP address)
    data:    Domain name label:    azureclitest
    data:    FQDN:                 azureclitest.eastus.cloudapp.azure.com
    info:    network public-ip show command OK

<span data-ttu-id="505f7-267">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-267">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -s, --subscription <subscription>      the subscription identifier


    network public-ip delete [options] <resource-group> <name>

<span data-ttu-id="505f7-268">Nyilvános IP-cím erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="505f7-268">Deletes public ip resource.</span></span>

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up the public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

<span data-ttu-id="505f7-269">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-269">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  the name of the resource group
    -n, --name <name>                      the name of the public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      the subscription identifier


<span data-ttu-id="505f7-270">**Parancsok futtatásával kezelheti a hálózati illesztők**</span><span class="sxs-lookup"><span data-stu-id="505f7-270">**Commands to manage network interfaces**</span></span>

    network nic create [options] <resource-group> <name> <location>
<span data-ttu-id="505f7-271">Létrehoz egy hálózati adapter (NIC), amely nem használható terheléselosztók, vagy rendeljen hozzá egy virtuális géphez nevű erőforrást.</span><span class="sxs-lookup"><span data-stu-id="505f7-271">Creates a resource called network interface (NIC) which can be used for load balancers or associate to a Virtual Machine.</span></span>

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up the network interface "testnic1"
    + Looking up the subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up the network interface "testnic1"
    data:    Id:                     /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/networkInterfaces/testnic1
    data:    Name:                   testnic1
    data:    Type:                   Microsoft.Network/networkInterfaces
    data:    Location:               eastus
    data:    Provisioning state:     Succeeded
    data:    IP configurations:
    data:       Name:                         NIC-config
    data:       Provisioning state:           Succeeded
    data:       Private IP address:           10.0.0.5
    data:       Private IP Allocation Method: Dynamic
    data:       Subnet:                       /subscriptions/c4a17ddf-aa84-491c-b6f9-b90d882299f7/resourceGroups/group-1/providers/Microsoft.Network/virtualNetworks/myVNET/subnets/Subnet-1

<span data-ttu-id="505f7-272">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="505f7-272">Parameter options:</span></span>

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            the name of the resource group
    -n, --name <name>                                                the name of the network interface
    -l, --location <location>                                        the location
    -w, --network-security-group-id <network-security-group-id>      the network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  the network security group name.
    This network security group must exist in the same resource group as the nic.
    Please use network-security-group-id if that is not the case.
    -i, --public-ip-id <public-ip-id>                                the public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            the public IP name.
    This public ip must exist in the same resource group as the nic.
    Please use public-ip-id if that is not the case.
    -a, --private-ip-address <private-ip-address>                    the private IP address
    -u, --subnet-id <subnet-id>                                      the subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  the subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        the vnet name under which subnet-name exists
    -t, --tags <tags>                                                the comma seperated list of tags.
    Can be multiple. In the format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                the subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

<span data-ttu-id="505f7-273">**Parancsok futtatásával kezelheti a hálózati biztonsági csoportok**</span><span class="sxs-lookup"><span data-stu-id="505f7-273">**Commands to manage network security groups**</span></span>

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

<span data-ttu-id="505f7-274">**Parancsok futtatásával kezelheti a hálózati biztonsági csoportszabályok**</span><span class="sxs-lookup"><span data-stu-id="505f7-274">**Commands to manage network security group rules**</span></span>

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

<span data-ttu-id="505f7-275">**Traffic manager-profil kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-275">**Commands to manage traffic manager profile**</span></span>

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

<span data-ttu-id="505f7-276">**Traffic manager-végpontokat kezelheti a parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-276">**Commands to manage traffic manager endpoints**</span></span>

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

<span data-ttu-id="505f7-277">**Parancsok futtatásával kezelheti a virtuális hálózati átjáró**</span><span class="sxs-lookup"><span data-stu-id="505f7-277">**Commands to manage virtual network gateways**</span></span>

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-to-manage-resource-provider-registrations"></a><span data-ttu-id="505f7-278">az Azure szolgáltatói: erőforrás-szolgáltató regisztrációk kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-278">azure provider: Commands to manage resource provider registrations</span></span>
<span data-ttu-id="505f7-279">**Az erőforrás-kezelőben jelenleg regisztrált szolgáltatók felsorolása**</span><span class="sxs-lookup"><span data-stu-id="505f7-279">**List currently registered providers in Resource Manager**</span></span>

    provider list [options]

<span data-ttu-id="505f7-280">**A kért szolgáltató névterének részleteinek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="505f7-280">**Show details about the requested provider namespace**</span></span>

    provider show [options] <namespace>

<span data-ttu-id="505f7-281">**Regisztrálja a szolgáltatót az előfizetéshez**</span><span class="sxs-lookup"><span data-stu-id="505f7-281">**Register provider with the subscription**</span></span>

    provider register [options] <namespace>

<span data-ttu-id="505f7-282">**Az előfizetés-szolgáltató regisztrációjának törlése**</span><span class="sxs-lookup"><span data-stu-id="505f7-282">**Unregister provider with the subscription**</span></span>

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-to-manage-your-resources"></a><span data-ttu-id="505f7-283">Azure-erőforrás: parancsok futtatásával kezelheti az erőforrásokat</span><span class="sxs-lookup"><span data-stu-id="505f7-283">azure resource: Commands to manage your resources</span></span>
<span data-ttu-id="505f7-284">**Létrehoz egy erőforrás egy erőforráscsoportban található**</span><span class="sxs-lookup"><span data-stu-id="505f7-284">**Creates a resource in a resource group**</span></span>

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

<span data-ttu-id="505f7-285">**Frissíti az erőforrás egy erőforráscsoportban található sablonok vagy paraméterek nélkül**</span><span class="sxs-lookup"><span data-stu-id="505f7-285">**Updates a resource in a resource group without any templates or parameters**</span></span>

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

<span data-ttu-id="505f7-286">**Az erőforrások listája**</span><span class="sxs-lookup"><span data-stu-id="505f7-286">**Lists the resources**</span></span>

    resource list [options] [resource-group]

<span data-ttu-id="505f7-287">**Lekérdezi egy erőforrást egy erőforráscsoportba vagy előfizetésbe belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-287">**Gets one resource within a resource group or subscription**</span></span>

    resource show [options] <resource-group> <name> <resource-type> <api-version>

<span data-ttu-id="505f7-288">**Töröl egy erőforrást egy erőforráscsoportban található**</span><span class="sxs-lookup"><span data-stu-id="505f7-288">**Deletes a resource in a resource group**</span></span>

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-to-manage-your-azure-roles"></a><span data-ttu-id="505f7-289">az Azure szerepkör: parancsok futtatásával kezelheti az Azure szerepkörök</span><span class="sxs-lookup"><span data-stu-id="505f7-289">azure role: Commands to manage your Azure roles</span></span>
<span data-ttu-id="505f7-290">**Az összes rendelkezésre álló szerepkör-definíciók beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-290">**Get all available role definitions**</span></span>

    role list [options]

<span data-ttu-id="505f7-291">**Egy szerepkör definíciójának beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-291">**Get an available role definition**</span></span>

    role show [options] [name]

<span data-ttu-id="505f7-292">**A szerepkör-hozzárendelés kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-292">**Commands to manage your role assignment**</span></span>

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-to-manage-your-storage-objects"></a><span data-ttu-id="505f7-293">az Azure storage: a tárolási objektum kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-293">azure storage: Commands to manage your Storage objects</span></span>
<span data-ttu-id="505f7-294">**A Storage-fiókok kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-294">**Commands to manage your Storage accounts**</span></span>

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

<span data-ttu-id="505f7-295">**A Tárfiók kulcsait kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-295">**Commands to manage your Storage account keys**</span></span>

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

<span data-ttu-id="505f7-296">**A tárolási kapcsolati karakterlánc megjelenítendő parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-296">**Commands to show your Storage connection string**</span></span>

    storage account connectionstring show [options] <name>

<span data-ttu-id="505f7-297">**A tárolókban kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-297">**Commands to manage your Storage containers**</span></span>

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

<span data-ttu-id="505f7-298">**Parancsok futtatásával kezelheti a megosztott hozzáférési aláírásokkal, a tároló**</span><span class="sxs-lookup"><span data-stu-id="505f7-298">**Commands to manage shared access signatures of your Storage container**</span></span>

    storage container sas create [options] [container] [permissions] [expiry]

<span data-ttu-id="505f7-299">**Parancsok futtatásával kezelheti tárolt hozzáférési házirendek, a tároló**</span><span class="sxs-lookup"><span data-stu-id="505f7-299">**Commands to manage stored access policies of your Storage container**</span></span>

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

<span data-ttu-id="505f7-300">**A tárolási BLOB kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-300">**Commands to manage your Storage blobs**</span></span>

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

<span data-ttu-id="505f7-301">**Parancsok futtatásával kezelheti a blob másolási művelet**</span><span class="sxs-lookup"><span data-stu-id="505f7-301">**Commands to manage your blob copy operations**</span></span>

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

<span data-ttu-id="505f7-302">**Parancsok futtatásával kezelheti a megosztott hozzáférési aláírása a Storage-blobba**</span><span class="sxs-lookup"><span data-stu-id="505f7-302">**Commands to manage shared access signature of your Storage blob**</span></span>

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

<span data-ttu-id="505f7-303">**A tárolófájl-megosztások kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-303">**Commands to manage your Storage file shares**</span></span>

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

<span data-ttu-id="505f7-304">**A tárolási fájlok kezelését parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-304">**Commands to manage your Storage files**</span></span>

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

<span data-ttu-id="505f7-305">**A tárolási könyvtárának kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-305">**Commands to manage your Storage file directory**</span></span>

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

<span data-ttu-id="505f7-306">**A tárolási sorok kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-306">**Commands to manage your Storage queues**</span></span>

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

<span data-ttu-id="505f7-307">**Parancsok futtatásával kezelheti a megosztott hozzáférési aláírást a tároló várólista**</span><span class="sxs-lookup"><span data-stu-id="505f7-307">**Commands to manage shared access signatures of your Storage queue**</span></span>

    storage queue sas create [options] [queue] [permissions] [expiry]

<span data-ttu-id="505f7-308">**Parancsok futtatásával kezelheti tárolt hozzáférési házirendek a tároló várólista**</span><span class="sxs-lookup"><span data-stu-id="505f7-308">**Commands to manage stored access policies of your Storage queue**</span></span>

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

<span data-ttu-id="505f7-309">**A tárolási naplózási tulajdonságait: kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-309">**Commands to manage your Storage logging properties**</span></span>

    storage logging show [options]
    storage logging set [options]

<span data-ttu-id="505f7-310">**A tárolási metrikák tulajdonságainak kezeléséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-310">**Commands to manage your Storage metrics properties**</span></span>

    storage metrics show [options]
    storage metrics set [options]

<span data-ttu-id="505f7-311">**A Storage-táblákat kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-311">**Commands to manage your Storage tables**</span></span>

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

<span data-ttu-id="505f7-312">**Parancsok futtatásával kezelheti a megosztott hozzáférési aláírást a tárolási tábla**</span><span class="sxs-lookup"><span data-stu-id="505f7-312">**Commands to manage shared access signatures of your Storage table**</span></span>

    storage table sas create [options] [table] [permissions] [expiry]

<span data-ttu-id="505f7-313">**Parancsok futtatásával kezelheti tárolt hozzáférési házirendek a tárolási tábla**</span><span class="sxs-lookup"><span data-stu-id="505f7-313">**Commands to manage stored access policies of your Storage table**</span></span>

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-to-manage-your-resource-manager-tag"></a><span data-ttu-id="505f7-314">az Azure címke: parancsok futtatásával kezelheti a resource manager címke</span><span class="sxs-lookup"><span data-stu-id="505f7-314">azure tag: Commands to manage your resource manager tag</span></span>
<span data-ttu-id="505f7-315">**Címke hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="505f7-315">**Add a tag**</span></span>

    tag create [options] <name> <value>

<span data-ttu-id="505f7-316">**Egy teljes tag vagy egy címke eltávolítása**</span><span class="sxs-lookup"><span data-stu-id="505f7-316">**Remove an entire tag or a tag value**</span></span>

    tag delete [options] <name> <value>

<span data-ttu-id="505f7-317">**A címke adatait**</span><span class="sxs-lookup"><span data-stu-id="505f7-317">**Lists the tag information**</span></span>

    tag list [options]

<span data-ttu-id="505f7-318">**A címke beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-318">**Get a tag**</span></span>

    tag show [options] [name]

## <a name="azure-vm-commands-to-manage-your-azure-virtual-machines"></a><span data-ttu-id="505f7-319">Azure virtuális gép: az Azure virtuális gépek kezelésére szolgáló parancsok</span><span class="sxs-lookup"><span data-stu-id="505f7-319">azure vm: Commands to manage your Azure Virtual Machines</span></span>
<span data-ttu-id="505f7-320">**Virtuális gép létrehozása**</span><span class="sxs-lookup"><span data-stu-id="505f7-320">**Create a VM**</span></span>

    vm create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="505f7-321">**Az alapértelmezett erőforrásokat a virtuális gép létrehozása**</span><span class="sxs-lookup"><span data-stu-id="505f7-321">**Create a VM with default resources**</span></span>

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> <span data-ttu-id="505f7-322">Parancssori felület 0.10-es verziójától kezdve megadhat egy rövid alias, például "UbuntuLTS" vagy "Win2012R2Datacenter" a `image-urn` az egyes népszerű piactéren elérhető rendszerkép.</span><span class="sxs-lookup"><span data-stu-id="505f7-322">Starting with CLI version 0.10, you can provide a short alias such as "UbuntuLTS" or "Win2012R2Datacenter" as the `image-urn` for some popular Marketplace images.</span></span> <span data-ttu-id="505f7-323">Futtatás `azure help vm quick-create` a beállítások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="505f7-323">Run `azure help vm quick-create` for options.</span></span> <span data-ttu-id="505f7-324">Emellett 0.10,-es verziójától kezdve `azure vm quick-create` alapértelmezés szerint prémium szintű storage használja, amennyiben az rendelkezésre áll a kiválasztott régióban.</span><span class="sxs-lookup"><span data-stu-id="505f7-324">Additionally, starting with version 0.10, `azure vm quick-create` uses premium storage by default if it's available in the selected region.</span></span>
> 
> 

<span data-ttu-id="505f7-325">**A virtuális gépek, a partner felsorolása**</span><span class="sxs-lookup"><span data-stu-id="505f7-325">**List the virtual machines within an account**</span></span>

    vm list [options]

<span data-ttu-id="505f7-326">**Egy virtuális gép erőforráscsoporton belül beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-326">**Get one virtual machine within a resource group**</span></span>

    vm show [options] <resource-group> <name>

<span data-ttu-id="505f7-327">**Erőforráscsoporton belül egy virtuális gép törlése**</span><span class="sxs-lookup"><span data-stu-id="505f7-327">**Delete one virtual machine within a resource group**</span></span>

    vm delete [options] <resource-group> <name>

<span data-ttu-id="505f7-328">**Egy virtuális gép leállítási erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-328">**Shutdown one virtual machine within a resource group**</span></span>

    vm stop [options] <resource-group> <name>

<span data-ttu-id="505f7-329">**Indítsa újra az erőforráscsoporton belül egy virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="505f7-329">**Restart one virtual machine within a resource group**</span></span>

    vm restart [options] <resource-group> <name>

<span data-ttu-id="505f7-330">**Indítson el egy virtuális gépet erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="505f7-330">**Start one virtual machine within a resource group**</span></span>

    vm start [options] <resource-group> <name>

<span data-ttu-id="505f7-331">**Egy virtuális gép leállítási belül az erőforráscsoportot és felszabadítja a számítási erőforrásokat**</span><span class="sxs-lookup"><span data-stu-id="505f7-331">**Shutdown one virtual machine within a resource group and releases the compute resources**</span></span>

    vm deallocate [options] <resource-group> <name>

<span data-ttu-id="505f7-332">**Elérhető virtuálisgép-méretek listázása**</span><span class="sxs-lookup"><span data-stu-id="505f7-332">**List available virtual machine sizes**</span></span>

    vm sizes [options]

<span data-ttu-id="505f7-333">**A virtuális gép operációs rendszer lemezképét vagy Virtuálisgép-lemezkép rögzítése**</span><span class="sxs-lookup"><span data-stu-id="505f7-333">**Capture the VM as OS Image or VM Image**</span></span>

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

<span data-ttu-id="505f7-334">**A virtuális gép állapotának Generalized beállítása**</span><span class="sxs-lookup"><span data-stu-id="505f7-334">**Set the state of the VM to Generalized**</span></span>

    vm generalize [options] <resource-group> <name>

<span data-ttu-id="505f7-335">**A virtuális gép példányait tartalmazó nézet beolvasása**</span><span class="sxs-lookup"><span data-stu-id="505f7-335">**Get instance view of the VM**</span></span>

    vm get-instance-view [options] <resource-group> <name>

<span data-ttu-id="505f7-336">**Lehetővé teszi a virtuális gépen a távoli asztal eléréséhez és az SSH beállítások alaphelyzetbe állítása és a jelszót a fiókhoz, amelyen a rendszergazda vagy a sudo hatóság**</span><span class="sxs-lookup"><span data-stu-id="505f7-336">**Enable you to reset Remote Desktop Access or SSH settings on a Virtual Machine and to reset the password for the account that has administrator or sudo authority**</span></span>

    vm reset-access [options] <resource-group> <name>

<span data-ttu-id="505f7-337">**Frissítse a virtuális gép új adatokkal**</span><span class="sxs-lookup"><span data-stu-id="505f7-337">**Update VM with new data**</span></span>

    vm set [options] <resource-group> <name>

<span data-ttu-id="505f7-338">**A virtuális gép adatlemezek kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-338">**Commands to manage your Virtual Machine data disks**</span></span>

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

<span data-ttu-id="505f7-339">**Parancsok futtatásával kezelheti a virtuális gép erőforrás-bővítményt**</span><span class="sxs-lookup"><span data-stu-id="505f7-339">**Commands to manage VM resource extensions**</span></span>

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

<span data-ttu-id="505f7-340">**A Docker virtuális gépek kezelésére szolgáló parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-340">**Commands to manage your Docker Virtual Machine**</span></span>

    vm docker create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="505f7-341">**A Virtuálisgép-lemezképek kezeléséhez parancsok**</span><span class="sxs-lookup"><span data-stu-id="505f7-341">**Commands to manage VM images**</span></span>

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
