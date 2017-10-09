---
title: "aaaManage Hadoop-fürtök használata az Azure CLI - Azure HDInsight |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure parancssori felület toomanage Hadoop-fürtök az Azure HDInsight. hello Azure CLI Windows, Mac és Linux rendszeren működik."
services: hdinsight
editor: cgronlun
manager: jhubbard
author: mumian
tags: azure-portal
documentationcenter: 
ms.assetid: 4f26c79f-8540-44bd-a470-84722a9e4eca
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ms.openlocfilehash: 03b0cff9331c1c581095b80cc6d1177d843ffa83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a><span data-ttu-id="46fce-104">Hello Azure parancssori felület használatával hdinsight Hadoop-fürtök kezelése</span><span class="sxs-lookup"><span data-stu-id="46fce-104">Manage Hadoop clusters in HDInsight using hello Azure CLI</span></span>
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

<span data-ttu-id="46fce-105">Megtudhatja, hogyan toouse hello [Azure parancssori felület](../cli-install-nodejs.md) toomanage Hadoop-fürtök az Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="46fce-105">Learn how toouse hello [Azure Command-line Interface](../cli-install-nodejs.md) toomanage Hadoop clusters in Azure HDInsight.</span></span> <span data-ttu-id="46fce-106">Node.js hello Azure parancssori felület implementálva van.</span><span class="sxs-lookup"><span data-stu-id="46fce-106">hello Azure CLI is implemented in Node.js.</span></span> <span data-ttu-id="46fce-107">Használható bármilyen platformon, amely támogatja a Node.js-t, beleértve a Windows, Mac és Linux platformokat.</span><span class="sxs-lookup"><span data-stu-id="46fce-107">It can be used on any platform that supports Node.js, including Windows, Mac, and Linux.</span></span>

<span data-ttu-id="46fce-108">Ez a cikk ismerteti, hogy csak hello Azure CLI használata a HDInsight.</span><span class="sxs-lookup"><span data-stu-id="46fce-108">This article covers only using hello Azure CLI with HDInsight.</span></span> <span data-ttu-id="46fce-109">Egy általános útmutató az Azure CLI toouse lásd: [telepítése és konfigurálása az Azure parancssori felület][azure-command-line-tools].</span><span class="sxs-lookup"><span data-stu-id="46fce-109">For a general guide on how toouse Azure CLI, see [Install and configure Azure CLI][azure-command-line-tools].</span></span>

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a><span data-ttu-id="46fce-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="46fce-110">Prerequisites</span></span>
<span data-ttu-id="46fce-111">Ez a cikk elkezdéséhez hello következő kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="46fce-111">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="46fce-112">**Azure-előfizetés**.</span><span class="sxs-lookup"><span data-stu-id="46fce-112">**An Azure subscription**.</span></span> <span data-ttu-id="46fce-113">Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="46fce-113">See [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="46fce-114">**Az Azure CLI** -lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) telepítési és konfigurációs információt.</span><span class="sxs-lookup"><span data-stu-id="46fce-114">**Azure CLI** - See [Install and configure hello Azure CLI](../cli-install-nodejs.md) for installation and configuration information.</span></span>
* <span data-ttu-id="46fce-115">**Csatlakozás tooAzure**használatával hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="46fce-115">**Connect tooAzure**, using hello following command:</span></span>
  
        azure login
  
    <span data-ttu-id="46fce-116">Munkahelyi vagy iskolai fiókkal hitelesítésről további információkért lásd: [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="46fce-116">For more information on authenticating using a work or school account, see [Connect tooan Azure subscription from hello Azure CLI](../xplat-cli-connect.md).</span></span>
* <span data-ttu-id="46fce-117">**Kapcsoló toohello Azure Resource Manager módra**használatával hello a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="46fce-117">**Switch toohello Azure Resource Manager mode**, using hello following command:</span></span>
  
        azure config mode arm

<span data-ttu-id="46fce-118">tooget segítségre van szüksége, használja a hello **-h** váltani.</span><span class="sxs-lookup"><span data-stu-id="46fce-118">tooget help, use hello **-h** switch.</span></span>  <span data-ttu-id="46fce-119">Példa:</span><span class="sxs-lookup"><span data-stu-id="46fce-119">For example:</span></span>

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a><span data-ttu-id="46fce-120">Létre fürtöket hello parancssori felület</span><span class="sxs-lookup"><span data-stu-id="46fce-120">Create clusters with hello CLI</span></span>
<span data-ttu-id="46fce-121">Lásd: [hozzon létre fürt a Hdinsightban az Azure CLI hello](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="46fce-121">See [Create clusters in HDInsight using hello Azure CLI](hdinsight-hadoop-create-linux-clusters-azure-cli.md).</span></span>

## <a name="list-and-show-cluster-details"></a><span data-ttu-id="46fce-122">Listáról, és a fürt részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="46fce-122">List and show cluster details</span></span>
<span data-ttu-id="46fce-123">A következő parancsok toolist hello használja, és a fürt részleteinek megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="46fce-123">Use hello following commands toolist and show cluster details:</span></span>

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

<span data-ttu-id="46fce-124">![A fürt listájának parancssori megtekintése][image-cli-clusterlisting]</span><span class="sxs-lookup"><span data-stu-id="46fce-124">![Command-line view of cluster list][image-cli-clusterlisting]</span></span>

## <a name="delete-clusters"></a><span data-ttu-id="46fce-125">Fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="46fce-125">Delete clusters</span></span>
<span data-ttu-id="46fce-126">A következő parancs toodelete fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="46fce-126">Use hello following command toodelete a cluster:</span></span>

    azure hdinsight cluster delete <Cluster Name>

<span data-ttu-id="46fce-127">A fürt hello fürt tartalmazó erőforráscsoportot hello törlésével is törli.</span><span class="sxs-lookup"><span data-stu-id="46fce-127">You can also delete a cluster by deleting hello resource group that contains hello cluster.</span></span> <span data-ttu-id="46fce-128">Vegye figyelembe, többek között az alapértelmezett tárfiók hello hello csoport minden hello erőforrásának törölni szeretné.</span><span class="sxs-lookup"><span data-stu-id="46fce-128">Please note, this will delete all hello resources in hello group including hello default storage account.</span></span>

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a><span data-ttu-id="46fce-129">Fürtök méretezése</span><span class="sxs-lookup"><span data-stu-id="46fce-129">Scale clusters</span></span>
<span data-ttu-id="46fce-130">toochange hello Hadoop fürtméret:</span><span class="sxs-lookup"><span data-stu-id="46fce-130">toochange hello Hadoop cluster size:</span></span>

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a><span data-ttu-id="46fce-131">Fürt HTTP-hozzáférés engedélyezése vagy letiltása</span><span class="sxs-lookup"><span data-stu-id="46fce-131">Enable/disable HTTP access for a cluster</span></span>
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a><span data-ttu-id="46fce-132">Engedélyezi/letiltja az RDP-hozzáférést a fürthöz</span><span class="sxs-lookup"><span data-stu-id="46fce-132">Enable/disable RDP access for a cluster</span></span>
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a><span data-ttu-id="46fce-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="46fce-133">Next steps</span></span>
<span data-ttu-id="46fce-134">Ebben a cikkben megtanulta, hogyan tooperform különböző HDInsight fürt felügyeleti feladatokat.</span><span class="sxs-lookup"><span data-stu-id="46fce-134">In this article, you have learned how tooperform different HDInsight cluster administrative tasks.</span></span> <span data-ttu-id="46fce-135">toolearn több, tekintse meg a következő cikkek hello:</span><span class="sxs-lookup"><span data-stu-id="46fce-135">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="46fce-136">[HDInsight felügyelete hello Azure portál használatával][hdinsight-admin-portal]</span><span class="sxs-lookup"><span data-stu-id="46fce-136">[Administer HDInsight by using hello Azure Portal][hdinsight-admin-portal]</span></span>
* <span data-ttu-id="46fce-137">[HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]</span><span class="sxs-lookup"><span data-stu-id="46fce-137">[Administer HDInsight by using Azure PowerShell][hdinsight-admin-powershell]</span></span>
* <span data-ttu-id="46fce-138">[Azure HDInsight – első lépések][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="46fce-138">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="46fce-139">[Hogyan toouse hello Azure parancssori felület][azure-command-line-tools]</span><span class="sxs-lookup"><span data-stu-id="46fce-139">[How toouse hello Azure CLI][azure-command-line-tools]</span></span>

[azure-command-line-tools]: ../cli-install-nodejs.md
[azure-create-storageaccount]:../storage/common/storage-create-storage-account.md
[azure-purchase-options]: http://azure.microsoft.com/pricing/purchase-options/
[azure-member-offers]: http://azure.microsoft.com/pricing/member-offers/
[azure-free-trial]: http://azure.microsoft.com/pricing/free-trial/


[hdinsight-admin-portal]: hdinsight-administer-use-management-portal.md
[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md

[image-cli-account-download-import]: ./media/hdinsight-administer-use-command-line/HDI.CLIAccountDownloadImport.png
[image-cli-clustercreation]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreation.png
[image-cli-clustercreation-config]: ./media/hdinsight-administer-use-command-line/HDI.CLIClusterCreationConfig.png
[image-cli-clusterlisting]: ./media/hdinsight-administer-use-command-line/command-line-list-of-clusters.png "Listában, és a fürt megjelenítése"
