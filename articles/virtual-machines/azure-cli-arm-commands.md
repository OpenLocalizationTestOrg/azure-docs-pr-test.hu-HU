---
title: "Erőforrás-kezelő módban parancsok aaaAzure CLI |} Microsoft Docs"
description: "Az Azure parancssori felület (CLI) parancsok hello Resource Manager üzembe helyezési modellel toomanage erőforrások"
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
ms.openlocfilehash: 49539655f7b24511e219f982819bcb59c9305d33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cli-commands-in-resource-manager-mode"></a><span data-ttu-id="79efc-103">Az Azure parancssori felület parancsait erőforrás-kezelő módban</span><span class="sxs-lookup"><span data-stu-id="79efc-103">Azure CLI commands in Resource Manager mode</span></span>
<span data-ttu-id="79efc-104">Ez a cikk szintaxis és beállításokat az Azure parancssori felület (CLI) parancsok üzembe helyezése gyakran toocreate használja, és hello Azure Resource Manager üzembe helyezési modellel Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="79efc-104">This article provides syntax and options for Azure command-line interface (CLI) commands you'd commonly use toocreate and manage Azure resources in hello Azure Resource Manager deployment model.</span></span> <span data-ttu-id="79efc-105">Ezek a parancsok hello CLI futtatásával Resource Manager (arm) módban érhető el.</span><span class="sxs-lookup"><span data-stu-id="79efc-105">You access these commands by running hello CLI in Resource Manager (arm) mode.</span></span> <span data-ttu-id="79efc-106">Ez nem teljes, és CLI-verziónak előfordulhat, hogy megjelenítése némileg eltérő parancsok vagy paraméterek.</span><span class="sxs-lookup"><span data-stu-id="79efc-106">This is not a complete reference, and your CLI version may show slightly different commands or parameters.</span></span> <span data-ttu-id="79efc-107">Azure-erőforrások és csoportok általános áttekintést, lásd: [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="79efc-107">For a general overview of Azure resources and resource groups, see [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md).</span></span>  

> [!NOTE]
> <span data-ttu-id="79efc-108">Ez a cikk bemutatja erőforrás-kezelő módban parancsok hello Azure parancssori felület, a más néven Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="79efc-108">This article shows Resource Manager mode commands in hello Azure CLI, sometimes called Azure CLI 1.0.</span></span> <span data-ttu-id="79efc-109">a Resource Manager modellt hello toowork, hello is megpróbálhatja [Azure CLI 2.0](/cli/azure/install-az-cli2), a következő generációs többplatformos parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="79efc-109">toowork in hello Resource Manager model, you can also try hello [Azure CLI 2.0](/cli/azure/install-az-cli2), our next generation multi-platform CLI.</span></span>
><span data-ttu-id="79efc-110">További információk hello [régi és az új Azure CLIs](/cli/azure/old-and-new-clis).</span><span class="sxs-lookup"><span data-stu-id="79efc-110">Find out more about hello [old and new Azure CLIs](/cli/azure/old-and-new-clis).</span></span>
>

<span data-ttu-id="79efc-111">tooget használatához először [hello Azure parancssori felület telepítése](../cli-install-nodejs.md) és [csatlakozás Azure-előfizetés tooyour](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="79efc-111">tooget started, first [install hello Azure CLI](../cli-install-nodejs.md) and [connect tooyour Azure subscription](../xplat-cli-connect.md).</span></span>

<span data-ttu-id="79efc-112">Aktuális parancs szintaxisa és parancssorból hello erőforrás-kezelő módban beállítások megtekintéséhez írja be a következőt `azure help` vagy egy adott parancs toodisplay súgó `azure help [command]`.</span><span class="sxs-lookup"><span data-stu-id="79efc-112">For current command syntax and options at hello command line in Resource Manager mode, type `azure help` or, toodisplay help for a specific command, `azure help [command]`.</span></span> <span data-ttu-id="79efc-113">Is találhat CLI példák hello dokumentációjának létrehozását és kezelését az adott Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="79efc-113">Also find CLI examples in hello documentation for creating and managing specific Azure services.</span></span>

<span data-ttu-id="79efc-114">Választható paraméterek: szögletes zárójelben jelennek meg (például `[parameter]`).</span><span class="sxs-lookup"><span data-stu-id="79efc-114">Optional parameters are shown in square brackets (for example, `[parameter]`).</span></span> <span data-ttu-id="79efc-115">Más paraméterek szükségesek.</span><span class="sxs-lookup"><span data-stu-id="79efc-115">All other parameters are required.</span></span>

<span data-ttu-id="79efc-116">Továbbá toocommand-specifikus választható paraméterek: részletes ismertetését lásd itt nincsenek három választható paraméterek: használható használt toodisplay részletes például lehetőségek és állapotkódok eredményét.</span><span class="sxs-lookup"><span data-stu-id="79efc-116">In addition toocommand-specific optional parameters documented here, there are three optional parameters that can be used toodisplay detailed output such as request options and status codes.</span></span> <span data-ttu-id="79efc-117">Hello `-v` paraméter biztosít részletes kimenet és hello `-vv` paraméter részletesebb még részletes kimenet.</span><span class="sxs-lookup"><span data-stu-id="79efc-117">hello `-v` parameter provides verbose output, and hello `-vv` parameter provides even more detailed verbose output.</span></span> <span data-ttu-id="79efc-118">Hello `--json` beállítás kimenete hello eredménye nyers json formátumban.</span><span class="sxs-lookup"><span data-stu-id="79efc-118">hello `--json` option outputs hello result in raw json format.</span></span>

## <a name="setting-hello-resource-manager-mode"></a><span data-ttu-id="79efc-119">A beállítás hello Resource Manager módra</span><span class="sxs-lookup"><span data-stu-id="79efc-119">Setting hello Resource Manager mode</span></span>
<span data-ttu-id="79efc-120">A következő parancs tooenable Azure CLI erőforrás-kezelő módban parancsok hello használata.</span><span class="sxs-lookup"><span data-stu-id="79efc-120">Use hello following command tooenable Azure CLI Resource Manager mode commands.</span></span>

    azure config mode arm

> [!NOTE]
> <span data-ttu-id="79efc-121">hello CLI Azure Resource Manager módra és Azure szolgáltatásfelügyeleti módban kölcsönösen kizárják egymást.</span><span class="sxs-lookup"><span data-stu-id="79efc-121">hello CLI's Azure Resource Manager mode and Azure Service Management mode are mutually exclusive.</span></span> <span data-ttu-id="79efc-122">Ez azt jelenti, hogy egy módban létrehozott erőforrások nem felügyelhetők a hello más módot.</span><span class="sxs-lookup"><span data-stu-id="79efc-122">That is, resources created in one mode cannot be managed from hello other mode.</span></span>
> 
> 

## <a name="azure-account-manage-your-account-information"></a><span data-ttu-id="79efc-123">Azure-fiók: a fiók adatainak kezelése</span><span class="sxs-lookup"><span data-stu-id="79efc-123">azure account: Manage your account information</span></span>
<span data-ttu-id="79efc-124">Azure-előfizetés adatainak hello eszköz tooconnect tooyour fiókot használja.</span><span class="sxs-lookup"><span data-stu-id="79efc-124">Your Azure subscription information is used by hello tool tooconnect tooyour account.</span></span>

<span data-ttu-id="79efc-125">**Lista hello importált előfizetések**</span><span class="sxs-lookup"><span data-stu-id="79efc-125">**List hello imported subscriptions**</span></span>

    account list [options]

<span data-ttu-id="79efc-126">**Előfizetés részleteinek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="79efc-126">**Show details about a subscription**</span></span>  

    account show [options] [subscriptionNameOrId]

<span data-ttu-id="79efc-127">**A jelenlegi előfizetés hello beállítása**</span><span class="sxs-lookup"><span data-stu-id="79efc-127">**Set hello current subscription**</span></span>

    account set [options] <subscriptionNameOrId>

<span data-ttu-id="79efc-128">**Távolítsa el a előfizetés vagy a környezet, vagy törölje az összes tárolt hello fiók és a környezet adatait**</span><span class="sxs-lookup"><span data-stu-id="79efc-128">**Remove a subscription or environment, or clear all of hello stored account and environment info**</span></span>  

    account clear [options]

<span data-ttu-id="79efc-129">**Parancsok toomanage a környezet**</span><span class="sxs-lookup"><span data-stu-id="79efc-129">**Commands toomanage your account environment**</span></span>  

    account env list [options]
    account env show [options] [environment]
    account env add [options] [environment]
    account env set [options] [environment]
    account env delete [options] [environment]

## <a name="azure-ad-commands-toodisplay-active-directory-objects"></a><span data-ttu-id="79efc-130">az Azure ad: parancsok toodisplay Active Directory-objektumok</span><span class="sxs-lookup"><span data-stu-id="79efc-130">azure ad: Commands toodisplay Active Directory objects</span></span>
<span data-ttu-id="79efc-131">**Parancsok toodisplay active directory-alkalmazások**</span><span class="sxs-lookup"><span data-stu-id="79efc-131">**Commands toodisplay active directory applications**</span></span>

    ad app create [options]
    ad app delete [options] <object-id>

<span data-ttu-id="79efc-132">**Parancsok toodisplay active directory-csoportok**</span><span class="sxs-lookup"><span data-stu-id="79efc-132">**Commands toodisplay active directory groups**</span></span>

    ad group list [options]
    ad group show [options]

<span data-ttu-id="79efc-133">**Parancsok tooprovide egy active directory sub csoport vagy a tag adatai**</span><span class="sxs-lookup"><span data-stu-id="79efc-133">**Commands tooprovide an active directory sub group or member info**</span></span>

    ad group member list [options] [objectId]

<span data-ttu-id="79efc-134">**Parancsok toodisplay active directory szolgáltatás rendszerbiztonsági tagok**</span><span class="sxs-lookup"><span data-stu-id="79efc-134">**Commands toodisplay active directory service principals**</span></span>

    ad sp list [options]
    ad sp show [options]
    ad sp create [options] <application-id>
    ad sp delete [options] <object-id>

<span data-ttu-id="79efc-135">**Parancsok toodisplay active directory-felhasználók**</span><span class="sxs-lookup"><span data-stu-id="79efc-135">**Commands toodisplay active directory users**</span></span>

    ad user list [options]
    ad user show [options]

## <a name="azure-availset-commands-toomanage-your-availability-sets"></a><span data-ttu-id="79efc-136">az Azure availset: parancsok toomanage a rendelkezésre állási készletek</span><span class="sxs-lookup"><span data-stu-id="79efc-136">azure availset: commands toomanage your availability sets</span></span>
<span data-ttu-id="79efc-137">**Létrehozza a rendelkezésre állási készlet erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-137">**Creates an availability set within a resource group**</span></span>

    availset create [options] <resource-group> <name> <location> [tags]

<span data-ttu-id="79efc-138">**Listák hello rendelkezésre állási készletek erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-138">**Lists hello availability sets within a resource group**</span></span>

    availset list [options] <resource-group>

<span data-ttu-id="79efc-139">**Lekérdezi a rendelkezésre állási csoporthoz erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-139">**Gets one availability set within a resource group**</span></span>

    availset show [options] <resource-group> <name>

<span data-ttu-id="79efc-140">**Törli a rendelkezésre állási csoporthoz erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-140">**Deletes one availability set within a resource group**</span></span>

    availset delete [options] <resource-group> <name>

## <a name="azure-config-commands-toomanage-your-local-settings"></a><span data-ttu-id="79efc-141">az Azure config: parancsok toomanage a helyi beállításokat</span><span class="sxs-lookup"><span data-stu-id="79efc-141">azure config: commands toomanage your local settings</span></span>
<span data-ttu-id="79efc-142">**Az Azure parancssori felület konfigurációs beállítások**</span><span class="sxs-lookup"><span data-stu-id="79efc-142">**List Azure CLI configuration settings**</span></span>

    config list [options]

<span data-ttu-id="79efc-143">**Egy konfigurációs beállítás törlése**</span><span class="sxs-lookup"><span data-stu-id="79efc-143">**Delete a config setting**</span></span>

    config delete [options] <name>

<span data-ttu-id="79efc-144">**Egy konfigurációs beállítás**</span><span class="sxs-lookup"><span data-stu-id="79efc-144">**Update a config setting**</span></span>

    config set <name> <value>

<span data-ttu-id="79efc-145">**Beállítása az Azure parancssori felület használata módot tooeither hello `arm` vagy`asm`**</span><span class="sxs-lookup"><span data-stu-id="79efc-145">**Sets hello Azure CLI working mode tooeither `arm` or `asm`**</span></span>

    config mode [options] <modename>


## <a name="azure-feature-commands-toomanage-account-features"></a><span data-ttu-id="79efc-146">az Azure-funkciót: toomanage fiók szolgáltatások parancsok</span><span class="sxs-lookup"><span data-stu-id="79efc-146">azure feature: commands toomanage account features</span></span>
<span data-ttu-id="79efc-147">**Az előfizetéshez tartozó összes szolgáltatásához felsorolása**</span><span class="sxs-lookup"><span data-stu-id="79efc-147">**List all features available for your subscription**</span></span>

    feature list [options]

<span data-ttu-id="79efc-148">**Megjeleníti a szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="79efc-148">**Shows a feature**</span></span>

    feature show [options] <providerName> <featureName>

<span data-ttu-id="79efc-149">**Egy előzetesen megtekintett funkciója egy erőforrás-szolgáltató regisztrálása**</span><span class="sxs-lookup"><span data-stu-id="79efc-149">**Registers a previewed feature of a resource provider**</span></span>

    feature register [options] <providerName> <featureName>

## <a name="azure-group-commands-toomanage-your-resource-groups"></a><span data-ttu-id="79efc-150">Azure-csoportok: parancsok toomanage az erőforrás-csoportokat</span><span class="sxs-lookup"><span data-stu-id="79efc-150">azure group: Commands toomanage your resource groups</span></span>
<span data-ttu-id="79efc-151">**Létrehoz egy erőforráscsoportot**</span><span class="sxs-lookup"><span data-stu-id="79efc-151">**Creates a resource group**</span></span>

    group create [options] <name> <location>

<span data-ttu-id="79efc-152">**Címkék tooa erőforrás-csoport beállítása**</span><span class="sxs-lookup"><span data-stu-id="79efc-152">**Set tags tooa resource group**</span></span>

    group set [options] <name> <tags>

<span data-ttu-id="79efc-153">**Egy erőforrás-csoport törlése**</span><span class="sxs-lookup"><span data-stu-id="79efc-153">**Deletes a resource group**</span></span>

    group delete [options] <name>

<span data-ttu-id="79efc-154">**Listázza az erőforráscsoportokat hello az előfizetéséhez**</span><span class="sxs-lookup"><span data-stu-id="79efc-154">**Lists hello resource groups for your subscription**</span></span>

    group list [options]

<span data-ttu-id="79efc-155">**Megjeleníti az előfizetéshez tartozó erőforráscsoport**</span><span class="sxs-lookup"><span data-stu-id="79efc-155">**Shows a resource group for your subscription**</span></span>

    group show [options] <name>

<span data-ttu-id="79efc-156">**Parancsok toomanage erőforrás csoport naplók**</span><span class="sxs-lookup"><span data-stu-id="79efc-156">**Commands toomanage resource group logs**</span></span>

    group log show [options] [name]

<span data-ttu-id="79efc-157">**Parancsok toomanage a központi telepítés egy erőforráscsoportban található**</span><span class="sxs-lookup"><span data-stu-id="79efc-157">**Commands toomanage your deployment in a resource group**</span></span>

    group deployment create [options] [resource-group] [name]
    group deployment list [options] <resource-group> [state]
    group deployment show [options] <resource-group> [deployment-name]
    group deployment stop [options] <resource-group> [deployment-name]

<span data-ttu-id="79efc-158">**Parancsok toomanage a helyi vagy gallery resource csoport sablon**</span><span class="sxs-lookup"><span data-stu-id="79efc-158">**Commands toomanage your local or gallery resource group template**</span></span>

    group template list [options]
    group template show [options] <name>
    group template download [options] [name] [file]
    group template validate [options] <resource-group>

## <a name="azure-hdinsight-commands-toomanage-your-hdinsight-clusters"></a><span data-ttu-id="79efc-159">az Azure hdinsight: parancsok toomanage a HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="79efc-159">azure hdinsight: Commands toomanage your HDInsight clusters</span></span>
<span data-ttu-id="79efc-160">**Parancsok toocreate vagy tooa fürt konfigurációs fájl felvétele**</span><span class="sxs-lookup"><span data-stu-id="79efc-160">**Commands toocreate or add tooa cluster configuration file**</span></span>

    hdinsight config create [options] <configFilePath> <overwrite>
    hdinsight config add-config-values [options] <configFilePath>
    hdinsight config add-script-action [options] <configFilePath>

<span data-ttu-id="79efc-161">Példa: A fürt létrehozásakor egy parancsfájl művelet toorun tartalmazó konfigurációs fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79efc-161">Example: Create a configuration file that contains a script action toorun when creating a cluster.</span></span>

    hdinsight config create "C:\myFiles\configFile.config"
    hdinsight config add-script-action --configFilePath "C:\myFiles\configFile.config" --nodeType HeadNode --uri <scriptActionURI> --name myScriptAction --parameters "-param value"

<span data-ttu-id="79efc-162">**A parancs toocreate a fürt egy erőforráscsoportot**</span><span class="sxs-lookup"><span data-stu-id="79efc-162">**Command toocreate a cluster in a resource group**</span></span>

    hdinsight cluster create [options] <clusterName>

<span data-ttu-id="79efc-163">Példa: Egy Storm létrehozása Linux-fürt</span><span class="sxs-lookup"><span data-stu-id="79efc-163">Example: Create a Storm on Linux cluster</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Storm --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="79efc-164">Példa: Hozzon létre egy fürtöt egy parancsfájlművelet</span><span class="sxs-lookup"><span data-stu-id="79efc-164">Example: Create a cluster with a script action</span></span>

    azure hdinsight cluster create -g myarmgroup -l westus -y Linux --clusterType Hadoop --version 3.2 --defaultStorageAccountName mystorageaccount --defaultStorageAccountKey <defaultStorageAccountKey> --defaultStorageContainer mycontainer --userName admin --password <clusterPassword> --sshUserName sshuser --sshPassword <sshPassword> --workerNodeCount 1 –configurationPath "C:\myFiles\configFile.config" myNewCluster01

    info:    Executing command hdinsight cluster create
    + Submitting hello request toocreate cluster...
    info:    hdinsight cluster create command OK

<span data-ttu-id="79efc-165">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-165">Parameter options:</span></span>

    -h, --help                                                 output usage information
    -v, --verbose                                              use verbose output
    -vv                                                        more verbose with debug output
    --json                                                     use json output
    -g --resource-group <resource-group>                       hello name of hello resource group
    -c, --clusterName <clusterName>                            HDInsight cluster name
    -l, --location <location>                                  Data center location for hello cluster
    -y, --osType <osType>                                      HDInsight cluster operating system
    'Windows' or 'Linux'
    --version <version>                                        HDInsight cluster version
    --clusterType <clusterType>                                HDInsight cluster type.
    Hadoop | HBase | Spark | Storm
    --defaultStorageAccountName <storageAccountName>           Storage account url toouse for default HDInsight storage
    --defaultStorageAccountKey <storageAccountKey>             Key toohello storage account toouse for default HDInsight storage
    --defaultStorageContainer <storageContainer>               Container in hello storage account toouse for HDInsight default storage
    --headNodeSize <headNodeSize>                              (Optional) Head node size for hello cluster
    --workerNodeCount <workerNodeCount>                        Number of worker nodes toouse for hello cluster
    --workerNodeSize <workerNodeSize>                          (Optional) Worker node size for hello cluster)
    --zookeeperNodeSize <zookeeperNodeSize>                    (Optional) Zookeeper node size for hello cluster
    --userName <userName>                                      Cluster username
    --password <password>                                      Cluster password
    --sshUserName <sshUserName>                                SSH username (only for Linux clusters)
    --sshPassword <sshPassword>                                SSH password (only for Linux clusters)
    --sshPublicKey <sshPublicKey>                              SSH public key (only for Linux clusters)
    --rdpUserName <rdpUserName>                                RDP username (only for Windows clusters)
    --rdpPassword <rdpPassword>                                RDP password (only for Windows clusters)
    --rdpAccessExpiry <rdpAccessExpiry>                        RDP access expiry.
    For example 12/12/2015 (only for Windows clusters)
    --virtualNetworkId <virtualNetworkId>                      (Optional) Virtual network ID for hello cluster.
    Value is a GUID for Windows cluster and ARM resource ID for Linux cluster)
    --subnetName <subnetName>                                  (Optional) Subnet for hello cluster
    --additionalStorageAccounts <additionalStorageAccounts>    (Optional) Additional storage accounts.
    Can be multiple.
    In hello format of 'accountName#accountKey'.
    For example, --additionalStorageAccounts "acc1#key1;acc2#key2"
    --hiveMetastoreServerName <hiveMetastoreServerName>        (Optional) SQL Server name for hello external metastore for Hive
    --hiveMetastoreDatabaseName <hiveMetastoreDatabaseName>    (Optional) Database name for hello external metastore for Hive
    --hiveMetastoreUserName <hiveMetastoreUserName>            (Optional) Database username for hello external metastore for Hive
    --hiveMetastorePassword <hiveMetastorePassword>            (Optional) Database password for hello external metastore for Hive
    --oozieMetastoreServerName <oozieMetastoreServerName>      (Optional) SQL Server name for hello external metastore for Oozie
    --oozieMetastoreDatabaseName <oozieMetastoreDatabaseName>  (Optional) Database name for hello external metastore for Oozie
    --oozieMetastoreUserName <oozieMetastoreUserName>          (Optional) Database username for hello external metastore for Oozie
    --oozieMetastorePassword <oozieMetastorePassword>          (Optional) Database password for hello external metastore for Oozie
    --configurationPath <configurationPath>                    (Optional) HDInsight cluster configuration file path
    -s, --subscription <id>                                    hello subscription id
    --tags <tags>                                              Tags tooset toohello cluster.
    Can be multiple.
    In hello format of 'name=value'.
    Name is required and value is optional.
    For example, --tags tag1=value1;tag2


<span data-ttu-id="79efc-166">**A parancs toodelete fürt**</span><span class="sxs-lookup"><span data-stu-id="79efc-166">**Command toodelete a cluster**</span></span>

    hdinsight cluster delete [options] <clusterName>

<span data-ttu-id="79efc-167">**Parancs tooshow fürt részleteinek megadása**</span><span class="sxs-lookup"><span data-stu-id="79efc-167">**Command tooshow cluster details**</span></span>

    hdinsight cluster show [options] <clusterName>

<span data-ttu-id="79efc-168">**Az összes parancs toolist clusters (a megadott erőforráscsoport, ha a megadott)**</span><span class="sxs-lookup"><span data-stu-id="79efc-168">**Command toolist all clusters (in a specific resource group, if provided)**</span></span>

    hdinsight cluster list [options]

<span data-ttu-id="79efc-169">**A parancs tooresize fürt**</span><span class="sxs-lookup"><span data-stu-id="79efc-169">**Command tooresize a cluster**</span></span>

    hdinsight cluster resize [options] <clusterName> <targetInstanceCount>

<span data-ttu-id="79efc-170">**Parancsot a fürt tooenable HTTP-hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="79efc-170">**Command tooenable HTTP access for a cluster**</span></span>

    hdinsight cluster enable-http-access [options] <clusterName> <userName> <password>

<span data-ttu-id="79efc-171">**Parancsot a fürt toodisable HTTP-hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="79efc-171">**Command toodisable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-http-access [options] <clusterName>

<span data-ttu-id="79efc-172">**A parancs tooenable RDP-hozzáférést a fürt**</span><span class="sxs-lookup"><span data-stu-id="79efc-172">**Command tooenable RDP access for a cluster**</span></span>

    hdinsight cluster enable-rdp-access [options] <clusterName> <rdpUserName> <rdpPassword> <rdpExpiryDate>

<span data-ttu-id="79efc-173">**Parancsot a fürt toodisable HTTP-hozzáférés**</span><span class="sxs-lookup"><span data-stu-id="79efc-173">**Command toodisable HTTP access for a cluster**</span></span>

    hdinsight cluster disable-rdp-access [options] <clusterName>

## <a name="azure-insights-commands-related-toomonitoring-insights-events-alert-rules-autoscale-settings-metrics"></a><span data-ttu-id="79efc-174">az Azure insights: parancsok kapcsolódó toomonitoring Insights (eseményeket, a riasztási szabályok, automatikus skálázási beállításokat, metrikák)</span><span class="sxs-lookup"><span data-stu-id="79efc-174">azure insights: Commands related toomonitoring Insights (events, alert rules, autoscale settings, metrics)</span></span>
<span data-ttu-id="79efc-175">**Az előfizetés, a correlationId, egy erőforráscsoport, erőforrás vagy erőforrás-szolgáltató műveletnaplók beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-175">**Retrieve operation logs for a subscription, a correlationId, a resource group, resource, or resource provider**</span></span>

    insights logs list [options]

## <a name="azure-location-commands-tooget-hello-available-locations-for-all-resource-types"></a><span data-ttu-id="79efc-176">Azure-beli hely: tooget hello elérhető helyét az összes erőforrástípus-parancsok</span><span class="sxs-lookup"><span data-stu-id="79efc-176">azure location: Commands tooget hello available locations for all resource types</span></span>
<span data-ttu-id="79efc-177">**Lista hello elérhető helyről**</span><span class="sxs-lookup"><span data-stu-id="79efc-177">**List hello available locations**</span></span>

    location list [options]

## <a name="azure-network-commands-toomanage-network-resources"></a><span data-ttu-id="79efc-178">Azure-hálózat: toomanage hálózati erőforrások parancsok</span><span class="sxs-lookup"><span data-stu-id="79efc-178">azure network: Commands toomanage network resources</span></span>
<span data-ttu-id="79efc-179">**Parancsok toomanage virtuális hálózatok**</span><span class="sxs-lookup"><span data-stu-id="79efc-179">**Commands toomanage virtual networks**</span></span>

    network vnet create [options] <resource-group> <name> <location>
<span data-ttu-id="79efc-180">Virtuális hálózat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79efc-180">Creates a virtual network.</span></span> <span data-ttu-id="79efc-181">Hello az alábbi példa azt a virtuális hálózat létrehozása nevű newvnet erőforrás csoport myresourcegroup hello USA nyugati régiója régióban.</span><span class="sxs-lookup"><span data-stu-id="79efc-181">In hello following example we create a virtual network named newvnet for resource group myresourcegroup in hello West US region.</span></span>

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


<span data-ttu-id="79efc-182">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-182">Parameter options:</span></span>

     -h, --help                                 output usage information
     -v, --verbose                              use verbose output
    --json                                     use json output
     -g, --resource-group <resource-group>      hello name of hello resource group
     -n, --name <name>                          hello name of hello virtual network
     -l, --location <location>                  hello location
     -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network
      For example -a 10.0.0.0/24,10.0.1.0/24.
      Default value is 10.0.0.0/8

    -d, --dns-servers <dns-servers>            hello comma separated list of DNS servers IP addresses
     -t, --tags <tags>                          hello tags set on this virtual network.
      Can be multiple. In hello format of "name=value".
      Name is required and value is optional.
      For example, -t tag1=value1;tag2
     -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet set [options] <resource-group> <name>

<span data-ttu-id="79efc-183">Frissíti a virtuális hálózati konfiguráció erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="79efc-183">Updates a virtual network configuration within a resource group.</span></span>

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

<span data-ttu-id="79efc-184">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-184">Parameter options:</span></span>

       -h, --help                                 output usage information
       -v, --verbose                              use verbose output
       --json                                     use json output
       -g, --resource-group <resource-group>      hello name of hello resource group
       -n, --name <name>                          hello name of hello virtual network
       -a, --address-prefixes <address-prefixes>  hello comma separated list of address prefixes for this virtual network.
        For example -a 10.0.0.0/24,10.0.1.0/24.
        This list will be appended toohello current list of address prefixes.
        hello address prefixes in this list should not overlap between them.
        hello address prefixes in this list should not overlap with existing address prefixes in hello vnet.

       -d, --dns-servers [dns-servers]            hello comma separated list of DNS servers IP addresses.
        This list will be appended toohello current list of DNS server IP addresses.

       -t, --tags <tags>                          hello tags set on this virtual network.
        Can be multiple. In hello format of "name=value".
        Name is required and value is optional. For example, -t tag1=value1;tag2.
        This list will be appended toohello current list of tags

       --no-tags                                  remove all existing tags
       -s, --subscription <subscription>          hello subscription identifier
<BR>

    network vnet list [options] <resource-group>

<span data-ttu-id="79efc-185">hello parancs felsorolja egy erőforráscsoportban található összes virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="79efc-185">hello command lists all virtual networks in a resource group.</span></span>

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

<span data-ttu-id="79efc-186">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-186">Parameter options:</span></span>

      -h, --help                             output usage information
      -v, --verbose                          use verbose output
      --json                                 use json output
      -g, --resource-group <resource-group>  hello name of hello resource group
      -s, --subscription <subscription>      hello subscription identifier

<BR>

    network vnet show [options] <resource-group> <name>
<span data-ttu-id="79efc-187">hello parancs erőforráscsoportban hello virtuális hálózati tulajdonságok láthatók.</span><span class="sxs-lookup"><span data-stu-id="79efc-187">hello command shows hello virtual network properties in a resource group.</span></span>

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
<span data-ttu-id="79efc-188">hello parancs eltávolítja a virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="79efc-188">hello command removes a virtual network.</span></span>

    azure network vnet delete myresourcegroup newvnetX

    info:    Executing command network vnet delete
    + Looking up virtual network "newvnetX"
    Delete virtual network newvnetX? [y/n] y
    + Deleting virtual network "newvnetX"
    info:    network vnet delete command OK

<span data-ttu-id="79efc-189">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-189">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello virtual network
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier


<span data-ttu-id="79efc-190">**Parancsok toomanage virtuális alhálózatok**</span><span class="sxs-lookup"><span data-stu-id="79efc-190">**Commands toomanage virtual network subnets**</span></span>

    network vnet subnet create [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="79efc-191">Egy másik alhálózati tooan meglévő virtuális hálózat hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="79efc-191">Adds another subnet tooan existing virtual network.</span></span>

    azure network vnet subnet create -g myresourcegroup --vnet-name newvnet -n subnet --address-prefix 10.0.1.0/24

    info:    Executing command network vnet subnet create
    + Looking up hello subnet "subnet"
    + Creating subnet "subnet"
    + Looking up hello subnet "subnet"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet
    data:    Name:                      subnet
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet create command OK

<span data-ttu-id="79efc-192">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-192">Parameter options:</span></span>

     -h, --help                                                       output usage information
     -v, --verbose                                                    use verbose output
         --json                                                           use json output
     -g, --resource-group <resource-group>                            hello name of hello resource group
     -e, --vnet-name <vnet-name>                                      hello name of hello virtual network
     -n, --name <name>                                                hello name of hello subnet
     -a, --address-prefix <address-prefix>                            hello address prefix
     -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
           e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
     -o, --network-security-group-name <network-security-group-name>  hello network security group name
     -s, --subscription <subscription>                                hello subscription identifier

<BR>

    network vnet subnet set [options] <resource-group> <vnet-name> <name>

<span data-ttu-id="79efc-193">Beállítja egy adott virtuális hálózati alhálózat erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="79efc-193">Sets a specific virtual network subnet within a resource group.</span></span>

    C:\>azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet list [options] <resource-group> <vnet-name>

<span data-ttu-id="79efc-194">Felsorolja az összes hello virtuális hálózati alhálózat virtuális hálózaton erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="79efc-194">Lists all hello virtual network subnets for a specific virtual network within a resource group.</span></span>

    azure network vnet subnet set -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet set
    + Looking up hello subnet "subnet1"
    + Setting subnet "subnet1"
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet set command OK
<BR>

    network vnet subnet show [options] <resource-group> <vnet-name> <name>
<span data-ttu-id="79efc-195">Virtuális hálózati alhálózat tulajdonságainak megjelenítése</span><span class="sxs-lookup"><span data-stu-id="79efc-195">Displays virtual network subnet properties</span></span>

    azure network vnet subnet show -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet show
    + Looking up hello subnet "subnet1"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft
    .Network/virtualNetworks/newvnet/subnets/subnet1
    data:    Name:                      subnet1
    data:    Type:                      Microsoft.Network/virtualNetworks/subnets
    data:    Provisioning state:        Succeeded
    data:    Address prefix:            10.0.1.0/24
    info:    network vnet subnet show command OK

<span data-ttu-id="79efc-196">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-196">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -e, --vnet-name <vnet-name>            hello name of hello virtual network
    -n, --name <name>                      hello name of hello subnet
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network vnet subnet delete [options] <resource-group> <vnet-name> <subnet-name>
<span data-ttu-id="79efc-197">Egy alhálózat eltávolítja a meglévő virtuális hálózat.</span><span class="sxs-lookup"><span data-stu-id="79efc-197">Removes a subnet from an existing virtual network.</span></span>

    azure network vnet subnet delete -g myresourcegroup --vnet-name newvnet -n subnet1

    info:    Executing command network vnet subnet delete
    + Looking up hello subnet "subnet1"
    Delete subnet "subnet1"? [y/n] y
    + Deleting subnet "subnet1"
    info:    network vnet subnet delete command OK

<span data-ttu-id="79efc-198">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-198">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -e, --vnet-name <vnet-name>            hello name of hello virtual network
     -n, --name <name>                      hello subnet name
     -s, --subscription <subscription>      hello subscription identifier
     -q, --quiet                            quiet mode, do not ask for delete confirmation

<span data-ttu-id="79efc-199">**Parancsok toomanage terheléselosztók**</span><span class="sxs-lookup"><span data-stu-id="79efc-199">**Commands toomanage load balancers**</span></span>

    network lb create [options] <resource-group> <name> <location>
<span data-ttu-id="79efc-200">Jönnek létre a load balancer.</span><span class="sxs-lookup"><span data-stu-id="79efc-200">Creates a load balancer set.</span></span>

    azure network lb create -g myresourcegroup -n mylb -l westus

    info:    Executing command network lb create
    + Looking up hello load balancer "mylb"
    + Creating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb create command OK

<span data-ttu-id="79efc-201">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-201">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -l, --location <location>              hello location
    -t, --tags <tags>                      hello list of tags.
     Can be multiple. In hello format of "name=value".
     Name is required and value is optional. For example, -t tag1=value1;tag2
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb list [options] <resource-group>
<span data-ttu-id="79efc-202">Felsorolja a Load balancer erőforrások erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="79efc-202">Lists Load balancer resources within a resource group.</span></span>

    azure network lb list myresourcegroup

    info:    Executing command network lb list
    + Getting hello load balancers
    data:    Name  Location
    data:    ----  --------
    data:    mylb  westus
    info:    network lb list command OK

<span data-ttu-id="79efc-203">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-203">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb show [options] <resource-group> <name>

<span data-ttu-id="79efc-204">Megjeleníti az erőforráscsoporton belül meghatározott típusú terheléselosztójának terheléselosztó adatok betöltésekor</span><span class="sxs-lookup"><span data-stu-id="79efc-204">Displays load balancer information of a specific load balancer within a resource group</span></span>

    azure network lb show myresourcegroup mylb -v

    info:    Executing command network lb show
    verbose: Looking up hello load balancer "mylb"
    data:    Id:                           /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb
    data:    Name:                         mylb
    data:    Type:                         Microsoft.Network/loadBalancers
    data:    Location:                     westus
    data:    Provisioning state:           Succeeded
    info:    network lb show command OK

<span data-ttu-id="79efc-205">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-205">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb delete [options] <resource-group> <name>

<span data-ttu-id="79efc-206">Törölje a load balancer erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="79efc-206">Delete load balancer resources.</span></span>

    azure network lb delete  myresourcegroup mylb

    info:    Executing command network lb delete
    + Looking up hello load balancer "mylb"
    Delete load balancer "mylb"? [y/n] y
    + Deleting load balancer "mylb"
    info:    network lb delete command OK

<span data-ttu-id="79efc-207">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-207">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -n, --name <name>                      hello name of hello load balancer
     -q, --quiet                            quiet mode, do not ask for delete confirmation
     -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="79efc-208">**Parancsok toomanage mintavételt terheléselosztó**</span><span class="sxs-lookup"><span data-stu-id="79efc-208">**Commands toomanage probes of a load balancer**</span></span>

    network lb probe create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="79efc-209">Hozzon létre hello mintavételi konfigurációs állapotadatok hello terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="79efc-209">Create hello probe configuration for health status in hello load balancer.</span></span> <span data-ttu-id="79efc-210">Tartsa szem előtt tartva toorun ezt a parancsot, a terheléselosztó előtérbeli ip-erőforrás (parancs "azure-hálózat előtér-IP-címe" tooassign egy ip-cím tooload terheléselosztó kivételének) van szükség.</span><span class="sxs-lookup"><span data-stu-id="79efc-210">Keep in mind toorun this command, your load balancer requires a frontend-ip resource (Check out command "azure network frontend-ip" tooassign an ip address tooload balancer).</span></span>

    azure network lb probe create -g myresourcegroup --lb-name mylb -n mylbprobe --protocol tcp --port 80 -i 300

    info:    Executing command network lb probe create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe create command OK

<span data-ttu-id="79efc-211">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-211">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -p, --protocol <protocol>              hello probe protocol
    -o, --port <port>                      hello probe port
    -f, --path <path>                      hello probe path
    -i, --interval <interval>              hello probe interval in seconds
    -c, --count <count>                    hello number of probes
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb probe set [options] <resource-group> <lb-name> <name>

<span data-ttu-id="79efc-212">Egy meglévő terheléselosztói mintavétel frissítése az új értékeivel.</span><span class="sxs-lookup"><span data-stu-id="79efc-212">Updates an existing load balancer probe with new values for it.</span></span>

    azure network lb probe set -g myresourcegroup -l mylb -n mylbprobe -p mylbprobe1 -p TCP -o 443 -i 300

    info:    Executing command network lb probe set
        + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    info:    network lb probe set command OK

<span data-ttu-id="79efc-213">A paraméter-beállítások</span><span class="sxs-lookup"><span data-stu-id="79efc-213">Parameter options</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello probe
    -e, --new-probe-name <new-probe-name>  hello new name of hello probe
    -p, --protocol <protocol>              hello new value for probe protocol
    -o, --port <port>                      hello new value for probe port
    -f, --path <path>                      hello new value for probe path
    -i, --interval <interval>              hello new value for probe interval in seconds
    -c, --count <count>                    hello new value for number of probes
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb probe list [options] <resource-group> <lb-name>

<span data-ttu-id="79efc-214">Lista hello mintavételi tulajdonságai load balancer állítja be.</span><span class="sxs-lookup"><span data-stu-id="79efc-214">List hello probe properties for a load balancer set.</span></span>

    C:\>azure network lb probe list -g myresourcegroup -l mylb

    info:    Executing command network lb probe list
    + Looking up hello load balancer "mylb"
    data:    Name       Protocol  Port  Path  Interval  Count
    data:    ---------  --------  ----  ----  --------  -----
    data:    mylbprobe  Tcp       443         300       2
    info:    network lb probe list command OK

<span data-ttu-id="79efc-215">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-215">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier


    network lb probe delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-216">Eltávolítja a hello mintavételi hello terheléselosztóhoz készült.</span><span class="sxs-lookup"><span data-stu-id="79efc-216">Removes hello probe created for hello load balancer.</span></span>

    azure network lb probe delete -g myresourcegroup -l mylb -n mylbprobe

    info:    Executing command network lb probe delete
    + Looking up hello load balancer "mylb"
    Delete a probe "mylbprobe?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb probe delete command OK

<span data-ttu-id="79efc-217">**Parancsok toomanage előtérbeli IP-konfigurációk terheléselosztó**</span><span class="sxs-lookup"><span data-stu-id="79efc-217">**Commands toomanage frontend ip configurations of a load balancer**</span></span>

    network lb frontend-ip create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-218">Létrehoz egy előtérbeli IP konfigurációs tooan meglévő load balancer beállítása.</span><span class="sxs-lookup"><span data-stu-id="79efc-218">Creates a frontend IP configuration tooan existing load balancer set.</span></span>

    azure network lb frontend-ip create -g myresourcegroup --lb-name mylb -n myfrontendip -o Dynamic -e subnet -m newvnet

    info:    Executing command network lb frontend-ip create
    + Looking up hello load balancer "mylb"
    + Looking up hello subnet "subnet"
    + Creating frontend IP configuration "myfrontendip"
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-219">A frissítés egy meglévő konfigurációt az alábbi parancs IP.hello időtúllépést ad egy mypubip5 tooan meglévő terheléselosztási terheléselosztó előtér-IP-myfrontendip nevű nevű nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="79efc-219">Updates an existing configuration of a frontend IP.hello command below adds a public IP called mypubip5 tooan existing load balancer frontend IP named myfrontendip.</span></span>

    azure network lb frontend-ip set -g myresourcegroup --lb-name mylb -n myfrontendip -i mypubip5

    info:    Executing command network lb frontend-ip set
    + Looking up hello load balancer "mylb"
    + Looking up hello public ip "mypubip5"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-220">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-220">Parameter options:</span></span>

    -h, --help                                                         output usage information
    -v, --verbose                                                      use verbose output
    --json                                                             use json output
    -g, --resource-group <resource-group>                              hello name of hello resource group
    -l, --lb-name <lb-name>                                            hello name of hello load balancer
    -n, --name <name>                                                  hello name of hello frontend ip configuration
    -a, --private-ip-address <private-ip-address>                      hello private ip address
    -o, --private-ip-allocation-method <private-ip-allocation-method>  hello private ip allocation method [Static, Dynamic]
    -u, --public-ip-id <public-ip-id>                                  hello public ip identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -i, --public-ip-name <public-ip-name>                              hello public ip name.
    This public ip must exist in hello same resource group as hello lb.
    Please use public-ip-id if that is not hello case.
    -b, --subnet-id <subnet-id>                                        hello subnet id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/VirtualNetworks/<vnet-name>/subnets/<subnet-name>
    -e, --subnet-name <subnet-name>                                    hello subnet name
    -m, --vnet-name <vnet-name>                                        hello virtual network name.
    This virtual network must exist in hello same resource group as hello lb.
    Please use subnet-id if that is not hello case.
    -s, --subscription <subscription>                                  hello subscription identifier

<BR>

    network lb frontend-ip list [options] <resource-group> <lb-name>

<span data-ttu-id="79efc-221">Minden hello előtérbeli IP-erőforrások hello terheléselosztóhoz konfigurált sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="79efc-221">Lists all hello frontend IP resources configured for hello load balancer.</span></span>

    azure network lb frontend-ip list -g myresourcegroup -l mylb

    info:    Executing command network lb frontend-ip list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Private IP allocation method  Subnet
    data:    -----------  ------------------  ----------------------------  ------
    data:    myprivateip  Succeeded           Dynamic
    info:    network lb frontend-ip list command OK

<span data-ttu-id="79efc-222">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-222">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb frontend-ip delete [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-223">Hello előtérbeli IP tartozó objektum tooload terheléselosztó törlése</span><span class="sxs-lookup"><span data-stu-id="79efc-223">Deletes hello frontend IP object associated tooload balancer</span></span>

    network lb frontend-ip delete -g myresourcegroup -l mylb -n myfrontendip
    info:    Executing command network lb frontend-ip delete
    + Looking up hello load balancer "mylb"
    Delete frontend ip configuration "myfrontendip"? [y/n] y
    + Updating load balancer "mylb"

<span data-ttu-id="79efc-224">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-224">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello frontend ip configuration
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="79efc-225">**Parancsok toomanage háttércímkészletek terheléselosztó**</span><span class="sxs-lookup"><span data-stu-id="79efc-225">**Commands toomanage backend address pools of a load balancer**</span></span>

    network lb address-pool create [options] <resource-group> <lb-name> <name>

<span data-ttu-id="79efc-226">Hozzon létre egy háttér címkészletet egy terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="79efc-226">Create a backend address pool for a load balancer.</span></span>

    azure network lb address-pool create -g myresourcegroup --lb-name mylb -n myaddresspool

    info:    Executing command network lb address-pool create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
    data:    Id:                        /subscriptions/###############################/resourceGroups/myresourgroup/providers/Microso.Network/loadBalancers/mylb/backendAddressPools/myaddresspool
    data:    Name:                      myaddresspool
    data:    Type:                      Microsoft.Network/loadBalancers/backendAddressPools
    data:    Provisioning state:        Succeeded
    data:    Backend IP configurations:
    data:    Load balancing rules:
    data:
    info:    network lb address-pool create command OK

<span data-ttu-id="79efc-227">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-227">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -s, --subscription <subscription>      hello subscription identifier

<BR>

    network lb address-pool list [options] <resource-group> <lb-name>

<span data-ttu-id="79efc-228">Az adott erőforráscsoport lista készlet háttérbeli IP-címtartomány</span><span class="sxs-lookup"><span data-stu-id="79efc-228">List backend IP address pool range for a specific resource group</span></span>

    azure network lb address-pool list -g myresourcegroup -l mylb

    info:    Executing command network lb address-pool list
    + Looking up hello load balancer "mylb"
    data:    Name           Provisioning state
    data:    -------------  ------------------
    data:    mybackendpool  Succeeded
    info:    network lb address-pool list command OK

<span data-ttu-id="79efc-229">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-229">Parameter options:</span></span>

     -h, --help                             output usage information
     -v, --verbose                          use verbose output
     --json                                 use json output
     -g, --resource-group <resource-group>  hello name of hello resource group
     -l, --lb-name <lb-name>                hello name of hello load balancer
     -s, --subscription <subscription>      hello subscription identifier

<BR>
    <span data-ttu-id="79efc-230">hálózati terheléselosztó-címkészlet törlése [kapcsolók] < erőforráscsoport >< lb-neve ><name></span><span class="sxs-lookup"><span data-stu-id="79efc-230">network lb address-pool delete [options] <resource-group> <lb-name> <name></span></span>

<span data-ttu-id="79efc-231">Hello háttérbeli IP-címkészlet tartománya erőforrás eltávolítása a terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="79efc-231">Removes hello backend IP pool range resource from load balancer.</span></span>

    azure network lb address-pool delete -g myresourcegroup -l mylb -n mybackendpool

    info:    Executing command network lb address-pool delete
    + Looking up hello load balancer "mylb"
    Delete backend address pool "mybackendpool"? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb address-pool delete command OK

<span data-ttu-id="79efc-232">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-232">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello backend address pool
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="79efc-233">**Parancsok toomanage load balancer szabályok**</span><span class="sxs-lookup"><span data-stu-id="79efc-233">**Commands toomanage load balancer rules**</span></span>

    network lb rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-234">Terheléselosztási szabály létrehozása.</span><span class="sxs-lookup"><span data-stu-id="79efc-234">Create load balancer rules.</span></span>

<span data-ttu-id="79efc-235">Létrehozhat olyan terheléselosztó szabályhoz hello előtér végpont hello terheléselosztó és hello háttér cím készlet tartomány tooreceive hello bejövő hálózati forgalom konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="79efc-235">You can create a load balancer rule configuring hello frontend endpoint for hello load balancer and hello backend address pool range tooreceive hello incoming network traffic.</span></span> <span data-ttu-id="79efc-236">Beállítások a hello az előtérbeli IP-végponton és portokkal hello háttér-készlet címtartomány is tartalmazzák.</span><span class="sxs-lookup"><span data-stu-id="79efc-236">Settings also include hello ports for frontend IP endpoint and ports for hello backend address pool range.</span></span>

<span data-ttu-id="79efc-237">hello következő példa bemutatja, hogyan toocreate egy terheléselosztó-szabály hello előtér végpont figyel tooport 80-as TCP- és terheléselosztási hálózati forgalom küldése a 8080-as tooport hello háttér-készlet címtartomány.</span><span class="sxs-lookup"><span data-stu-id="79efc-237">hello following example shows how toocreate a load balancer rule,  hello frontend endpoint listening tooport 80 TCP and load balancing network traffic sending tooport 8080 for hello backend address pool range.</span></span>

    azure network lb rule create -g myresourcegroup -l mylb -n mylbrule -p tcp -f 80 -b 8080 -i 10


    info:    Executing command network lb rule create
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-238">Egy meglévő terheléselosztási szabály egy adott erőforráscsoportban beállítása frissíti.</span><span class="sxs-lookup"><span data-stu-id="79efc-238">Updates an existing load balancer rule set in a specific resource group.</span></span> <span data-ttu-id="79efc-239">A következő példa hello mylbrule toomynewlbrule hello szabály neve módosult azt.</span><span class="sxs-lookup"><span data-stu-id="79efc-239">In hello following example, we changed hello rule name from mylbrule toomynewlbrule.</span></span>

    azure network lb rule set -g myresourcegroup -l mylb -n mylbrule -r mynewlbrule -p tcp -f 80 -b 8080 -i 10 -t myfrontendip -o mybackendpool

    info:    Executing command network lb rule set
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-240">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-240">Parameter options:</span></span>

    -h, --help                                         output usage information
    -v, --verbose                                      use verbose output
    --json                                             use json output
    -g, --resource-group <resource-group>              hello name of hello resource group
    -l, --lb-name <lb-name>                            hello name of hello load balancer
    -n, --name <name>                                  hello name of hello rule
    -r, --new-rule-name <new-rule-name>                new rule name
    -p, --protocol <protocol>                          hello rule protocol
    -f, --frontend-port <frontend-port>                hello frontend port
    -b, --backend-port <backend-port>                  hello backend port
    -e, --enable-floating-ip <enable-floating-ip>      enable floating point ip
    -i, --idle-timeout <idle-timeout>                  hello idle timeout in minutes
    -a, --probe-name [probe-name]                      hello name of hello probe defined in hello same load balancer
    -t, --frontend-ip-name <frontend-ip-name>          hello name of hello frontend ip configuration in hello same load balancer
    -o, --backend-address-pool <backend-address-pool>  name of hello backend address pool defined in hello same load balancer
    -s, --subscription <subscription>                  hello subscription identifier


    network lb rule list [options] <resource-group> <lb-name>

<span data-ttu-id="79efc-241">Felsorolja az összes terheléselosztó szabály egy adott erőforráscsoportban terheléselosztó beállítva betölteni.</span><span class="sxs-lookup"><span data-stu-id="79efc-241">Lists all load balancer rules configured for a load balancer in a specific resource group.</span></span>

    azure network lb rule list -g myresourcegroup -l mylb

    info:    Executing command network lb rule list
    + Looking up hello load balancer "mylb"
    data:    Name         Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend address pool  Probe data

    data:    mynewlbrule  Succeeded           Tcp       80             8080          false               10                       /subscriptions/###############################/resourceGroups/myresourcegroup/providers/Microsoft.Network/loadBalancers/mylb/backendAddressPools/mybackendpool
    info:    network lb rule list command OK

<span data-ttu-id="79efc-242">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-242">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier

    network lb rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="79efc-243">A terheléselosztási szabály törlése.</span><span class="sxs-lookup"><span data-stu-id="79efc-243">Deletes a load balancer rule.</span></span>

    azure network lb rule delete -g myresourcegroup -l mylb -n mynewlbrule

    info:    Executing command network lb rule delete
    + Looking up hello load balancer "mylb"
    Delete load balancing rule mynewlbrule? [y/n] y
    + Updating load balancer "mylb"
    info:    network lb rule delete command OK

<span data-ttu-id="79efc-244">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-244">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="79efc-245">**Parancsok toomanage terheléselosztó bejövő forgalmat kezelő NAT-szabályok**</span><span class="sxs-lookup"><span data-stu-id="79efc-245">**Commands toomanage load balancer inbound NAT rules**</span></span>

    network lb inbound-nat-rule create [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-246">Terheléselosztó bejövő NAT-szabályban hoz létre.</span><span class="sxs-lookup"><span data-stu-id="79efc-246">Creates an inbound NAT rule for load balancer.</span></span>

<span data-ttu-id="79efc-247">Létrehoztunk egy NAT-szabály (amely a korábban már definiálva lett hello "azure-hálózat előtér-IP-címe" paranccsal) előtérbeli IP-címről figyelőportja bejövő és kimenő port az adott hello terheléselosztó alábbi példa hello toosend hello hálózati forgalom használ.</span><span class="sxs-lookup"><span data-stu-id="79efc-247">In hello following example  we created a NAT rule from frontend IP (which was previously defined using hello "azure network frontend-ip" command) with an inbound listening port and outbound port that hello load balancer uses toosend hello network traffic.</span></span>

    azure network lb inbound-nat-rule create -g myresourcegroup -l mylb -n myinboundnat -p tcp -f 80 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule create
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-248">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-248">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id <vm-id>                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.This VM must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case.
    this parameter will be ignored if --vm-id is specified
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule set [options] <resource-group> <lb-name> <name>
<span data-ttu-id="79efc-249">Egy meglévő bejövő forgalmat kezelő nat-szabály frissítése.</span><span class="sxs-lookup"><span data-stu-id="79efc-249">Updates an existing inbound nat rule.</span></span> <span data-ttu-id="79efc-250">A következő példa hello, hello változtattuk bejövő 80 too81 a figyelő portja.</span><span class="sxs-lookup"><span data-stu-id="79efc-250">In hello following example, we changed hello inbound listening port from 80 too81.</span></span>

    azure network lb inbound-nat-rule set -g group-1 -l mylb -n myinboundnat -p tcp -f 81 -b 8080 -i myfrontendip

    info:    Executing command network lb inbound-nat-rule set
    + Looking up hello load balancer "mylb"
    + Updating load balancer "mylb"
    + Looking up hello load balancer "mylb"
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

<span data-ttu-id="79efc-251">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-251">Parameter options:</span></span>

    -h, --help                                     output usage information
    -v, --verbose                                  use verbose output
    --json                                         use json output
    -g, --resource-group <resource-group>          hello name of hello resource group
    -l, --lb-name <lb-name>                        hello name of hello load balancer
    -n, --name <name>                              hello name of hello inbound NAT rule
    -p, --protocol <protocol>                      hello rule protocol [tcp,udp]
    -f, --frontend-port <frontend-port>            hello frontend port [0-65535]
    -b, --backend-port <backend-port>              hello backend port [0-65535]
    -e, --enable-floating-ip <enable-floating-ip>  enable floating point ip [true,false]
    -i, --frontend-ip <frontend-ip>                hello name of hello frontend ip configuration
    -m, --vm-id [vm-id]                            hello VM id.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Compute/virtualMachines/<vm-name>
    -a, --vm-name <vm-name>                        hello VM name.
    This virtual machine must exist in hello same resource group as hello lb.
    Please use vm-id if that is not hello case
    -s, --subscription <subscription>              hello subscription identifier
<BR>

    network lb inbound-nat-rule list [options] <resource-group> <lb-name>

<span data-ttu-id="79efc-252">Felsorolja az összes bejövő nat-szabályok a terheléselosztóhoz.</span><span class="sxs-lookup"><span data-stu-id="79efc-252">Lists all inbound nat rules for load balancer.</span></span>

    azure network lb inbound-nat-rule list -g myresourcegroup -l mylb

    info:    Executing command network lb inbound-nat-rule list
    + Looking up hello load balancer "mylb"
    data:    Name          Provisioning state  Protocol  Frontend port  Backend port  Enable floating IP  Idle timeout in minutes  Backend IP configuration
    data:    ------------  ------------------  --------  -------------  ------------  ------------------  -----------------------  ---
    ---------------------
    data:    myinboundnat  Succeeded           Tcp       81             8080          false               4

    info:    network lb inbound-nat-rule list command OK

<span data-ttu-id="79efc-253">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-253">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -s, --subscription <subscription>      hello subscription identifier
<BR>

    network lb inbound-nat-rule delete [options] <resource-group> <lb-name> <name>

<span data-ttu-id="79efc-254">Egy adott erőforráscsoportban hello terheléselosztó NAT-szabály törlése.</span><span class="sxs-lookup"><span data-stu-id="79efc-254">Deletes NAT rule for hello load balancer in a specific resource group.</span></span>

    azure network lb inbound-nat-rule delete -g myresourcegroup -l mylb -n myinboundnat

    info:    Executing command network lb inbound-nat-rule delete
    + Looking up hello load balancer "mylb"
    Delete inbound NAT rule "myinboundnat?" [y/n] y
    + Updating load balancer "mylb"
    info:    network lb inbound-nat-rule delete command OK

<span data-ttu-id="79efc-255">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-255">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -l, --lb-name <lb-name>                hello name of hello load balancer
    -n, --name <name>                      hello name of hello inbound NAT rule
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier

<span data-ttu-id="79efc-256">**Parancsok toomanage nyilvános IP-címek**</span><span class="sxs-lookup"><span data-stu-id="79efc-256">**Commands toomanage public ip addresses**</span></span>

    network public-ip create [options] <resource-group> <name> <location>
<span data-ttu-id="79efc-257">Létrehoz egy nyilvános IP-cím erőforrás.</span><span class="sxs-lookup"><span data-stu-id="79efc-257">Creates a public ip resource.</span></span> <span data-ttu-id="79efc-258">Hello nyilvános IP-cím erőforrás létrehoz, és társítsa tooa tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="79efc-258">You will create hello public ip resource and associate tooa domain name.</span></span>

    azure network public-ip create -g myresourcegroup -n mytestpublicip1 -l eastus -d azureclitest -a "Dynamic"
    info:    Executing command network public-ip create
    + Looking up hello public ip "mytestpublicip1"
    + Creating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
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


<span data-ttu-id="79efc-259">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-259">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -l, --location <location>                    hello location
    -d, --domain-name-label <domain-name-label>  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn <reverse-fqdn>            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>            hello subscription identifier
<br>

    network public-ip set [options] <resource-group> <name>
<span data-ttu-id="79efc-260">Frissíti a meglévő nyilvános IP-cím erőforrás hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="79efc-260">Updates hello properties of an existing public ip resource.</span></span> <span data-ttu-id="79efc-261">Az alábbi példa hello a dinamikus tooStatic úgy módosítottuk hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="79efc-261">In hello following example we changed hello public IP address from Dynamic tooStatic.</span></span>

    azure network public-ip set -g group-1 -n mytestpublicip1 -d azureclitest -a "Static"
    info:    Executing command network public-ip set
    + Looking up hello public ip "mytestpublicip1"
    + Updating public ip address "mytestpublicip1"
    + Looking up hello public ip "mytestpublicip1"
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

<span data-ttu-id="79efc-262">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-262">Parameter options:</span></span>

    -h, --help                                   output usage information
    -v, --verbose                                use verbose output
    --json                                       use json output
    -g, --resource-group <resource-group>        hello name of hello resource group
    -n, --name <name>                            hello name of hello public ip
    -d, --domain-name-label [domain-name-label]  hello domain name label.
    This set DNS too<domain-name-label>.<location>.cloudapp.azure.com
    -a, --allocation-method <allocation-method>  hello allocation method [Static][Dynamic]
    -i, --idletimeout <idletimeout>              hello idle timeout in minutes
    -f, --reverse-fqdn [reverse-fqdn]            hello reverse fqdn
    -t, --tags <tags>                            hello list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    --no-tags                                    remove all existing tags
    -s, --subscription <subscription>            hello subscription identifier

<br>
    <span data-ttu-id="79efc-263">hálózati nyilvános ip-[kapcsolók] < erőforráscsoport > lista minden nyilvános IP-erőforrások erőforráscsoporton belül.</span><span class="sxs-lookup"><span data-stu-id="79efc-263">network public-ip list [options] <resource-group> Lists all public IP resources within a resource group.</span></span>

    azure network public-ip list -g myresourcegroup

    info:    Executing command network public-ip list
    + Getting hello public ip addresses
    data:    Name             Location  Allocation  IP Address    Idle timeout  DNS Name
    data:    ---------------  --------  ----------  ------------  ------------  -------------------------------------------
    data:    mypubip5         westus    Dynamic                   4             "domain name".westus.cloudapp.azure.com
    data:    myPublicIP       eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip   eastus    Dynamic                   4             "domain name".eastus.cloudapp.azure.com
    data:    mytestpublicip1  eastus   Static (Static IP address) 4             azureclitest.eastus.cloudapp.azure.com

<span data-ttu-id="79efc-264">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-264">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -s, --subscription <subscription>      hello subscription identifier
<BR>
    <span data-ttu-id="79efc-265">hálózati nyilvános ip-megjelenítése [kapcsolók] < erőforráscsoport ><name></span><span class="sxs-lookup"><span data-stu-id="79efc-265">network public-ip show [options] <resource-group> <name></span></span>

<span data-ttu-id="79efc-266">A nyilvános IP-cím erőforrás egy erőforráscsoportban nyilvános IP-cím tulajdonságainak megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="79efc-266">Displays public ip properties for a public ip resource within a resource group.</span></span>

    azure network public-ip show -g myresourcegroup -n mytestpublicip

    info:    Executing command network public-ip show
    + Looking up hello public ip "mytestpublicip1"
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

<span data-ttu-id="79efc-267">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-267">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -s, --subscription <subscription>      hello subscription identifier


    network public-ip delete [options] <resource-group> <name>

<span data-ttu-id="79efc-268">Nyilvános IP-cím erőforrás törlése.</span><span class="sxs-lookup"><span data-stu-id="79efc-268">Deletes public ip resource.</span></span>

    azure network public-ip delete -g group-1 -n mypublicipname
    info:    Executing command network public-ip delete
    + Looking up hello public ip "mypublicipname"
    Delete public ip address "mypublicipname"? [y/n] y
    + Deleting public ip address "mypublicipname"
    info:    network public-ip delete command OK

<span data-ttu-id="79efc-269">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-269">Parameter options:</span></span>

    -h, --help                             output usage information
    -v, --verbose                          use verbose output
    --json                                 use json output
    -g, --resource-group <resource-group>  hello name of hello resource group
    -n, --name <name>                      hello name of hello public IP
    -q, --quiet                            quiet mode, do not ask for delete confirmation
    -s, --subscription <subscription>      hello subscription identifier


<span data-ttu-id="79efc-270">**Parancsok toomanage hálózati illesztők**</span><span class="sxs-lookup"><span data-stu-id="79efc-270">**Commands toomanage network interfaces**</span></span>

    network nic create [options] <resource-group> <name> <location>
<span data-ttu-id="79efc-271">Létrehoz egy hálózati adapter (NIC), amely nem használható terheléselosztók, vagy rendeljen hozzá a virtuális gép tooa nevű erőforrást.</span><span class="sxs-lookup"><span data-stu-id="79efc-271">Creates a resource called network interface (NIC) which can be used for load balancers or associate tooa Virtual Machine.</span></span>

    azure network nic create -g myresourcegroup -l eastus -n testnic1 --subnet-name subnet-1 --subnet-vnet-name myvnet

    info:    Executing command network nic create
    + Looking up hello network interface "testnic1"
    + Looking up hello subnet "subnet-1"
    + Creating network interface "testnic1"
    + Looking up hello network interface "testnic1"
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

<span data-ttu-id="79efc-272">A paraméter beállítások:</span><span class="sxs-lookup"><span data-stu-id="79efc-272">Parameter options:</span></span>

    -h, --help                                                       output usage information
    -v, --verbose                                                    use verbose output
    --json                                                           use json output
    -g, --resource-group <resource-group>                            hello name of hello resource group
    -n, --name <name>                                                hello name of hello network interface
    -l, --location <location>                                        hello location
    -w, --network-security-group-id <network-security-group-id>      hello network security group identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/networkSecurityGroups/<nsg-name>
    -o, --network-security-group-name <network-security-group-name>  hello network security group name.
    This network security group must exist in hello same resource group as hello nic.
    Please use network-security-group-id if that is not hello case.
    -i, --public-ip-id <public-ip-id>                                hello public IP identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/publicIPAddresses/<public-ip-name>
    -p, --public-ip-name <public-ip-name>                            hello public IP name.
    This public ip must exist in hello same resource group as hello nic.
    Please use public-ip-id if that is not hello case.
    -a, --private-ip-address <private-ip-address>                    hello private IP address
    -u, --subnet-id <subnet-id>                                      hello subnet identifier.
    e.g. /subscriptions/<subscription-id>/resourceGroups/<resource-group-name>/providers/Microsoft.Network/virtualNetworks/<vnet-name>/subnets/<subnet-name>
    --subnet-name <subnet-name>                                  hello subnet name
    -m, --subnet-vnet-name <subnet-vnet-name>                        hello vnet name under which subnet-name exists
    -t, --tags <tags>                                                hello comma seperated list of tags.
    Can be multiple. In hello format of "name=value".
    Name is required and value is optional.
    For example, -t tag1=value1;tag2
    -s, --subscription <subscription>                                hello subscription identifier
    data:
    info:    network nic create command OK

<BR>

    network nic set [options] <resource-group> <name>

    network nic list [options] <resource-group>
    network nic show [options] <resource-group> <name>
    network nic delete [options] <resource-group> <name>

<span data-ttu-id="79efc-273">**Parancsok toomanage hálózati biztonsági csoportok**</span><span class="sxs-lookup"><span data-stu-id="79efc-273">**Commands toomanage network security groups**</span></span>

    network nsg create [options] <resource-group> <name> <location>
    network nsg set [options] <resource-group> <name>
    network nsg list [options] <resource-group>
    network nsg show [options] <resource-group> <name>
    network nsg delete [options] <resource-group> <name>

<span data-ttu-id="79efc-274">**Parancsok toomanage hálózati biztonsági csoportszabályok**</span><span class="sxs-lookup"><span data-stu-id="79efc-274">**Commands toomanage network security group rules**</span></span>

    network nsg rule create [options] <resource-group> <nsg-name> <name>
    network nsg rule set [options] <resource-group> <nsg-name> <name>
    network nsg rule list [options] <resource-group> <nsg-name>
    network nsg rule show [options] <resource-group> <nsg-name> <name>
    network nsg rule delete [options] <resource-group> <nsg-name> <name>

<span data-ttu-id="79efc-275">**Parancsok toomanage traffic manager-profil**</span><span class="sxs-lookup"><span data-stu-id="79efc-275">**Commands toomanage traffic manager profile**</span></span>

    network traffic-manager profile create [options] <resource-group> <name>
    network traffic-manager profile set [options] <resource-group> <name>
    network traffic-manager profile list [options] <resource-group>
    network traffic-manager profile show [options] <resource-group> <name>
    network traffic-manager profile delete [options] <resource-group> <name>
    network traffic-manager profile is-dns-available [options] <resource-group> <relative-dns-name>

<span data-ttu-id="79efc-276">**Parancsok toomanage traffic manager-végpontok**</span><span class="sxs-lookup"><span data-stu-id="79efc-276">**Commands toomanage traffic manager endpoints**</span></span>

    network traffic-manager profile endpoint create [options] <resource-group> <profile-name> <name> <endpoint-location>
    network traffic-manager profile endpoint set [options] <resource-group> <profile-name> <name>
    network traffic-manager profile endpoint delete [options] <resource-group> <profile-name> <name>

<span data-ttu-id="79efc-277">**Parancsok toomanage virtuális hálózati átjárók**</span><span class="sxs-lookup"><span data-stu-id="79efc-277">**Commands toomanage virtual network gateways**</span></span>

    network gateway list [options] <resource-group>

## <a name="azure-provider-commands-toomanage-resource-provider-registrations"></a><span data-ttu-id="79efc-278">az Azure szolgáltatói: toomanage erőforrás-szolgáltató regisztrációk parancsok</span><span class="sxs-lookup"><span data-stu-id="79efc-278">azure provider: Commands toomanage resource provider registrations</span></span>
<span data-ttu-id="79efc-279">**Az erőforrás-kezelőben jelenleg regisztrált szolgáltatók felsorolása**</span><span class="sxs-lookup"><span data-stu-id="79efc-279">**List currently registered providers in Resource Manager**</span></span>

    provider list [options]

<span data-ttu-id="79efc-280">**A kért szolgáltatójának névtere hello kapcsolatos részletek megjelenítése**</span><span class="sxs-lookup"><span data-stu-id="79efc-280">**Show details about hello requested provider namespace**</span></span>

    provider show [options] <namespace>

<span data-ttu-id="79efc-281">**Regisztrálja a szolgáltatót hello előfizetéssel**</span><span class="sxs-lookup"><span data-stu-id="79efc-281">**Register provider with hello subscription**</span></span>

    provider register [options] <namespace>

<span data-ttu-id="79efc-282">**Hello előfizetés-szolgáltató regisztrációjának törlése**</span><span class="sxs-lookup"><span data-stu-id="79efc-282">**Unregister provider with hello subscription**</span></span>

    provider unregister [options] <namespace>

## <a name="azure-resource-commands-toomanage-your-resources"></a><span data-ttu-id="79efc-283">Azure-erőforrás: parancsok toomanage az erőforrások</span><span class="sxs-lookup"><span data-stu-id="79efc-283">azure resource: Commands toomanage your resources</span></span>
<span data-ttu-id="79efc-284">**Létrehoz egy erőforrás egy erőforráscsoportban található**</span><span class="sxs-lookup"><span data-stu-id="79efc-284">**Creates a resource in a resource group**</span></span>

    resource create [options] <resource-group> <name> <resource-type> <location> <api-version>

<span data-ttu-id="79efc-285">**Frissíti az erőforrás egy erőforráscsoportban található sablonok vagy paraméterek nélkül**</span><span class="sxs-lookup"><span data-stu-id="79efc-285">**Updates a resource in a resource group without any templates or parameters**</span></span>

    resource set [options] <resource-group> <name> <resource-type> <properties> <api-version>

<span data-ttu-id="79efc-286">**Listák hello erőforrások**</span><span class="sxs-lookup"><span data-stu-id="79efc-286">**Lists hello resources**</span></span>

    resource list [options] [resource-group]

<span data-ttu-id="79efc-287">**Lekérdezi egy erőforrást egy erőforráscsoportba vagy előfizetésbe belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-287">**Gets one resource within a resource group or subscription**</span></span>

    resource show [options] <resource-group> <name> <resource-type> <api-version>

<span data-ttu-id="79efc-288">**Töröl egy erőforrást egy erőforráscsoportban található**</span><span class="sxs-lookup"><span data-stu-id="79efc-288">**Deletes a resource in a resource group**</span></span>

    resource delete [options] <resource-group> <name> <resource-type> <api-version>

## <a name="azure-role-commands-toomanage-your-azure-roles"></a><span data-ttu-id="79efc-289">az Azure szerepkör: parancsok toomanage a Azure szerepkörök</span><span class="sxs-lookup"><span data-stu-id="79efc-289">azure role: Commands toomanage your Azure roles</span></span>
<span data-ttu-id="79efc-290">**Az összes rendelkezésre álló szerepkör-definíciók beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-290">**Get all available role definitions**</span></span>

    role list [options]

<span data-ttu-id="79efc-291">**Egy szerepkör definíciójának beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-291">**Get an available role definition**</span></span>

    role show [options] [name]

<span data-ttu-id="79efc-292">**Parancsok toomanage a szerepkör-hozzárendelés**</span><span class="sxs-lookup"><span data-stu-id="79efc-292">**Commands toomanage your role assignment**</span></span>

    role assignment create [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment list [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]
    role assignment delete [options] [objectId] [upn] [mail] [spn] [role] [scope] [resource-group] [resource-type] [resource-name]

## <a name="azure-storage-commands-toomanage-your-storage-objects"></a><span data-ttu-id="79efc-293">az Azure storage: parancsok toomanage a tárolási objektum</span><span class="sxs-lookup"><span data-stu-id="79efc-293">azure storage: Commands toomanage your Storage objects</span></span>
<span data-ttu-id="79efc-294">**Parancsok toomanage a Storage-fiókok**</span><span class="sxs-lookup"><span data-stu-id="79efc-294">**Commands toomanage your Storage accounts**</span></span>

    storage account list [options]
    storage account show [options] <name>
    storage account create [options] <name>
    storage account set [options] <name>
    storage account delete [options] <name>

<span data-ttu-id="79efc-295">**A Tárfiók kulcsait a toomanage parancsokat**</span><span class="sxs-lookup"><span data-stu-id="79efc-295">**Commands toomanage your Storage account keys**</span></span>

    storage account keys list [options] <name>
    storage account keys renew [options] <name>

<span data-ttu-id="79efc-296">**Parancsok tooshow a tárolási kapcsolati karakterlánc**</span><span class="sxs-lookup"><span data-stu-id="79efc-296">**Commands tooshow your Storage connection string**</span></span>

    storage account connectionstring show [options] <name>

<span data-ttu-id="79efc-297">**Parancsok toomanage a tárolókban**</span><span class="sxs-lookup"><span data-stu-id="79efc-297">**Commands toomanage your Storage containers**</span></span>

    storage container list [options] [prefix]
    storage container show [options] [container]
    storage container create [options] [container]
    storage container delete [options] [container]
    storage container set [options] [container]

<span data-ttu-id="79efc-298">**Parancsok toomanage megosztott hozzáférési aláírásokkal, a tároló**</span><span class="sxs-lookup"><span data-stu-id="79efc-298">**Commands toomanage shared access signatures of your Storage container**</span></span>

    storage container sas create [options] [container] [permissions] [expiry]

<span data-ttu-id="79efc-299">**Parancsok toomanage tárolja a tárolót a hozzáférési házirendek**</span><span class="sxs-lookup"><span data-stu-id="79efc-299">**Commands toomanage stored access policies of your Storage container**</span></span>

    storage container policy create [options] [container] [name]
    storage container policy show [options] [container] [name]
    storage container policy list [options] [container]
    storage container policy set [options] [container] [name]
    storage container policy delete [options] [container] [name]

<span data-ttu-id="79efc-300">**A tárolási BLOB-toomanage parancsok**</span><span class="sxs-lookup"><span data-stu-id="79efc-300">**Commands toomanage your Storage blobs**</span></span>

    storage blob list [options] [container] [prefix]
    storage blob show [options] [container] [blob]
    storage blob delete [options] [container] [blob]
    storage blob upload [options] [file] [container] [blob]
    storage blob download [options] [container] [blob] [destination]

<span data-ttu-id="79efc-301">**Parancsok toomanage a blob másolási műveletek**</span><span class="sxs-lookup"><span data-stu-id="79efc-301">**Commands toomanage your blob copy operations**</span></span>

    storage blob copy start [options] [sourceUri] [destContainer]
    storage blob copy show [options] [container] [blob]
    storage blob copy stop [options] [container] [blob] [copyid]

<span data-ttu-id="79efc-302">**A tárolási blob parancsok toomanage megosztott hozzáférési aláírása**</span><span class="sxs-lookup"><span data-stu-id="79efc-302">**Commands toomanage shared access signature of your Storage blob**</span></span>

    storage blob sas create [options] [container] [blob] [permissions] [expiry]

<span data-ttu-id="79efc-303">**Parancsok toomanage a tárolófájl-megosztások**</span><span class="sxs-lookup"><span data-stu-id="79efc-303">**Commands toomanage your Storage file shares**</span></span>

    storage share create [options] [share]
    storage share show [options] [share]
    storage share delete [options] [share]
    storage share list [options] [prefix]

<span data-ttu-id="79efc-304">**Parancsok toomanage a tárolófájlok**</span><span class="sxs-lookup"><span data-stu-id="79efc-304">**Commands toomanage your Storage files**</span></span>

    storage file list [options] [share] [path]
    storage file delete [options] [share] [path]
    storage file upload [options] [source] [share] [path]
    storage file download [options] [share] [path] [destination]

<span data-ttu-id="79efc-305">**Parancsok toomanage a tárolási könyvtára**</span><span class="sxs-lookup"><span data-stu-id="79efc-305">**Commands toomanage your Storage file directory**</span></span>

    storage directory create [options] [share] [path]
    storage directory delete [options] [share] [path]

<span data-ttu-id="79efc-306">**Parancsok toomanage a tárolási sorok**</span><span class="sxs-lookup"><span data-stu-id="79efc-306">**Commands toomanage your Storage queues**</span></span>

    storage queue create [options] [queue]
    storage queue list [options] [prefix]
    storage queue show [options] [queue]
    storage queue delete [options] [queue]

<span data-ttu-id="79efc-307">**A tároló várólista a parancsok toomanage megosztott hozzáférési aláírásokkal**</span><span class="sxs-lookup"><span data-stu-id="79efc-307">**Commands toomanage shared access signatures of your Storage queue**</span></span>

    storage queue sas create [options] [queue] [permissions] [expiry]

<span data-ttu-id="79efc-308">**Parancsok toomanage tárolja a tároló várólista hozzáférési házirendek**</span><span class="sxs-lookup"><span data-stu-id="79efc-308">**Commands toomanage stored access policies of your Storage queue**</span></span>

    storage queue policy create [options] [queue] [name]
    storage queue policy show [options] [queue] [name]
    storage queue policy list [options] [queue]
    storage queue policy set [options] [queue] [name]
    storage queue policy delete [options] [queue] [name]

<span data-ttu-id="79efc-309">**Parancsok toomanage a tárolási naplózási tulajdonságait:**</span><span class="sxs-lookup"><span data-stu-id="79efc-309">**Commands toomanage your Storage logging properties**</span></span>

    storage logging show [options]
    storage logging set [options]

<span data-ttu-id="79efc-310">**Parancsok toomanage a tárolási metrikák tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="79efc-310">**Commands toomanage your Storage metrics properties**</span></span>

    storage metrics show [options]
    storage metrics set [options]

<span data-ttu-id="79efc-311">**A Storage-táblákat a toomanage parancsokat**</span><span class="sxs-lookup"><span data-stu-id="79efc-311">**Commands toomanage your Storage tables**</span></span>

    storage table create [options] [table]
    storage table list [options] [prefix]
    storage table show [options] [table]
    storage table delete [options] [table]

<span data-ttu-id="79efc-312">**Parancsok toomanage megosztott hozzáférési aláírásokkal a tárolási tábla**</span><span class="sxs-lookup"><span data-stu-id="79efc-312">**Commands toomanage shared access signatures of your Storage table**</span></span>

    storage table sas create [options] [table] [permissions] [expiry]

<span data-ttu-id="79efc-313">**Parancsok toomanage tárolja a tárolási tábla hozzáférési házirendek**</span><span class="sxs-lookup"><span data-stu-id="79efc-313">**Commands toomanage stored access policies of your Storage table**</span></span>

    storage table policy create [options] [table] [name]
    storage table policy show [options] [table] [name]
    storage table policy list [options] [table]
    storage table policy set [options] [table] [name]
    storage table policy delete [options] [table] [name]

## <a name="azure-tag-commands-toomanage-your-resource-manager-tag"></a><span data-ttu-id="79efc-314">az Azure címke: parancsok toomanage a resource manager címke</span><span class="sxs-lookup"><span data-stu-id="79efc-314">azure tag: Commands toomanage your resource manager tag</span></span>
<span data-ttu-id="79efc-315">**Címke hozzáadása**</span><span class="sxs-lookup"><span data-stu-id="79efc-315">**Add a tag**</span></span>

    tag create [options] <name> <value>

<span data-ttu-id="79efc-316">**Egy teljes tag vagy egy címke eltávolítása**</span><span class="sxs-lookup"><span data-stu-id="79efc-316">**Remove an entire tag or a tag value**</span></span>

    tag delete [options] <name> <value>

<span data-ttu-id="79efc-317">**Hello címkeadatokat listája**</span><span class="sxs-lookup"><span data-stu-id="79efc-317">**Lists hello tag information**</span></span>

    tag list [options]

<span data-ttu-id="79efc-318">**A címke beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-318">**Get a tag**</span></span>

    tag show [options] [name]

## <a name="azure-vm-commands-toomanage-your-azure-virtual-machines"></a><span data-ttu-id="79efc-319">Azure virtuális gép: parancsok toomanage az Azure virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="79efc-319">azure vm: Commands toomanage your Azure Virtual Machines</span></span>
<span data-ttu-id="79efc-320">**Virtuális gép létrehozása**</span><span class="sxs-lookup"><span data-stu-id="79efc-320">**Create a VM**</span></span>

    vm create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="79efc-321">**Az alapértelmezett erőforrásokat a virtuális gép létrehozása**</span><span class="sxs-lookup"><span data-stu-id="79efc-321">**Create a VM with default resources**</span></span>

    vm quick-create [options] <resource-group> <name> <location> <os-type> <image-urn> <admin-username> <admin-password

> [!TIP]
> <span data-ttu-id="79efc-322">Parancssori felület 0.10-es verziójától kezdve megadhat egy rövid alias, például "UbuntuLTS" vagy "Win2012R2Datacenter" hello `image-urn` az egyes népszerű piactéren elérhető rendszerkép.</span><span class="sxs-lookup"><span data-stu-id="79efc-322">Starting with CLI version 0.10, you can provide a short alias such as "UbuntuLTS" or "Win2012R2Datacenter" as hello `image-urn` for some popular Marketplace images.</span></span> <span data-ttu-id="79efc-323">Futtatás `azure help vm quick-create` a beállítások megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="79efc-323">Run `azure help vm quick-create` for options.</span></span> <span data-ttu-id="79efc-324">Emellett 0.10,-es verziójától kezdve `azure vm quick-create` alapértelmezés szerint prémium szintű storage használja, amennyiben az rendelkezésre áll hello a kiválasztott régióban.</span><span class="sxs-lookup"><span data-stu-id="79efc-324">Additionally, starting with version 0.10, `azure vm quick-create` uses premium storage by default if it's available in hello selected region.</span></span>
> 
> 

<span data-ttu-id="79efc-325">**Lista hello virtuális gépeit egy fiók**</span><span class="sxs-lookup"><span data-stu-id="79efc-325">**List hello virtual machines within an account**</span></span>

    vm list [options]

<span data-ttu-id="79efc-326">**Egy virtuális gép erőforráscsoporton belül beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-326">**Get one virtual machine within a resource group**</span></span>

    vm show [options] <resource-group> <name>

<span data-ttu-id="79efc-327">**Erőforráscsoporton belül egy virtuális gép törlése**</span><span class="sxs-lookup"><span data-stu-id="79efc-327">**Delete one virtual machine within a resource group**</span></span>

    vm delete [options] <resource-group> <name>

<span data-ttu-id="79efc-328">**Egy virtuális gép leállítási erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-328">**Shutdown one virtual machine within a resource group**</span></span>

    vm stop [options] <resource-group> <name>

<span data-ttu-id="79efc-329">**Indítsa újra az erőforráscsoporton belül egy virtuális gép**</span><span class="sxs-lookup"><span data-stu-id="79efc-329">**Restart one virtual machine within a resource group**</span></span>

    vm restart [options] <resource-group> <name>

<span data-ttu-id="79efc-330">**Indítson el egy virtuális gépet erőforráscsoporton belül**</span><span class="sxs-lookup"><span data-stu-id="79efc-330">**Start one virtual machine within a resource group**</span></span>

    vm start [options] <resource-group> <name>

<span data-ttu-id="79efc-331">**Egy virtuális gép leállítási belül egy erőforrás-csoport és a kiadások hello számítási erőforrásokat**</span><span class="sxs-lookup"><span data-stu-id="79efc-331">**Shutdown one virtual machine within a resource group and releases hello compute resources**</span></span>

    vm deallocate [options] <resource-group> <name>

<span data-ttu-id="79efc-332">**Elérhető virtuálisgép-méretek listázása**</span><span class="sxs-lookup"><span data-stu-id="79efc-332">**List available virtual machine sizes**</span></span>

    vm sizes [options]

<span data-ttu-id="79efc-333">**Hello operációsrendszer-lemezképek, a virtuális gép vagy Virtuálisgép-lemezkép rögzítése**</span><span class="sxs-lookup"><span data-stu-id="79efc-333">**Capture hello VM as OS Image or VM Image**</span></span>

    vm capture [options] <resource-group> <name> <vhd-name-prefix>

<span data-ttu-id="79efc-334">**Hello VM tooGeneralized hello állapotának beállítása**</span><span class="sxs-lookup"><span data-stu-id="79efc-334">**Set hello state of hello VM tooGeneralized**</span></span>

    vm generalize [options] <resource-group> <name>

<span data-ttu-id="79efc-335">**Hello virtuális gép példányait tartalmazó nézet beolvasása**</span><span class="sxs-lookup"><span data-stu-id="79efc-335">**Get instance view of hello VM**</span></span>

    vm get-instance-view [options] <resource-group> <name>

<span data-ttu-id="79efc-336">**Ön tooreset távoli asztal eléréséhez és az SSH-beállítások engedélyezése a virtuális gépen, hello fiók, amely rendelkezik a rendszergazda vagy a sudo hatóság tooreset hello jelszavát**</span><span class="sxs-lookup"><span data-stu-id="79efc-336">**Enable you tooreset Remote Desktop Access or SSH settings on a Virtual Machine and tooreset hello password for hello account that has administrator or sudo authority**</span></span>

    vm reset-access [options] <resource-group> <name>

<span data-ttu-id="79efc-337">**Frissítse a virtuális gép új adatokkal**</span><span class="sxs-lookup"><span data-stu-id="79efc-337">**Update VM with new data**</span></span>

    vm set [options] <resource-group> <name>

<span data-ttu-id="79efc-338">**A virtuális gép adatlemezek-toomanage parancsok**</span><span class="sxs-lookup"><span data-stu-id="79efc-338">**Commands toomanage your Virtual Machine data disks**</span></span>

    vm disk attach-new [options] <resource-group> <vm-name> <size-in-gb> [vhd-name]
    vm disk detach [options] <resource-group> <vm-name> <lun>
    vm disk attach [options] <resource-group> <vm-name> [vhd-url]

<span data-ttu-id="79efc-339">**Parancsok toomanage VM erőforrás-bővítményt**</span><span class="sxs-lookup"><span data-stu-id="79efc-339">**Commands toomanage VM resource extensions**</span></span>

    vm extension set [options] <resource-group> <vm-name> <name> <publisher-name> <version>
    vm extension get [options] <resource-group> <vm-name>

<span data-ttu-id="79efc-340">**A Docker virtuálisgép-toomanage parancsok**</span><span class="sxs-lookup"><span data-stu-id="79efc-340">**Commands toomanage your Docker Virtual Machine**</span></span>

    vm docker create [options] <resource-group> <name> <location> <os-type>

<span data-ttu-id="79efc-341">**Parancsok toomanage Virtuálisgép-rendszerképek**</span><span class="sxs-lookup"><span data-stu-id="79efc-341">**Commands toomanage VM images**</span></span>

    vm image list-publishers [options] <location>
    vm image list-offers [options] <location> <publisher>
    vm image list-skus [options] <location> <publisher> <offer>
    vm image list [options] <location> <publisher> [offer] [sku]
