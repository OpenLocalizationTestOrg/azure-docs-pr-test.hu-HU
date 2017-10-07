---
title: "HDInsight-fürtök - Azure aaaHow toodelete |} Microsoft Docs"
description: "HDInsight-fürtök törlése különböző módokon hello információkat."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 55f7838b-9786-47ff-96db-1b64437bd0bb
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/31/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 5b9d9a09eecfdcfaed7a1f5ebab440e13bd358b0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a><span data-ttu-id="d6f84-103">A böngészőben, a PowerShell vagy az hello Azure parancssori felület használatával HDInsight-fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="d6f84-103">Delete an HDInsight cluster using your browser, PowerShell, or hello Azure CLI</span></span>

<span data-ttu-id="d6f84-104">HDInsight-fürt számlázási elindul, miután a fürt jön létre, és leállítja a hello fürt törlésekor.</span><span class="sxs-lookup"><span data-stu-id="d6f84-104">HDInsight cluster billing starts once a cluster is created and stops when hello cluster is deleted.</span></span> <span data-ttu-id="d6f84-105">Számlázási, egyenletesen percenként történik, így mindig törölnie kell a fürthöz, amikor már nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="d6f84-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="d6f84-106">Ebből a dokumentumból megismerheti, hogyan egy fürt használt toodelete hello Azure-portál, Azure PowerShell és hello Azure CLI 1.0.</span><span class="sxs-lookup"><span data-stu-id="d6f84-106">In this document, you learn how toodelete a cluster using hello Azure portal, Azure PowerShell, and hello Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d6f84-107">HDInsight-fürtök törlése nem érinti hello Azure Storage-fiókokat, vagy a Data Lake Store társított hello fürt.</span><span class="sxs-lookup"><span data-stu-id="d6f84-107">Deleting an HDInsight cluster does not delete hello Azure Storage accounts or Data Lake Store associated with hello cluster.</span></span> <span data-ttu-id="d6f84-108">A jövőbeli hello e szolgáltatásokban tárolt adatokat is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="d6f84-108">You can reuse data stored in those services in hello future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="d6f84-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d6f84-109">Azure portal</span></span>

1. <span data-ttu-id="d6f84-110">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d6f84-110">Log in toohello [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="d6f84-111">Ha a HDInsight-fürt nem rögzített toohello irányítópult, kereshet az hello keresési mező nevét.</span><span class="sxs-lookup"><span data-stu-id="d6f84-111">If your HDInsight cluster is not pinned toohello dashboard, you can search for it by name using hello search field.</span></span>
   
    ![portál keresési](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="d6f84-113">Ha hello panel hello fürt nyit, válassza ki a hello **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="d6f84-113">Once hello blade opens for hello cluster, select hello **Delete** icon.</span></span> <span data-ttu-id="d6f84-114">Amikor a rendszer kéri, válassza ki a **Igen** toodelete hello fürt.</span><span class="sxs-lookup"><span data-stu-id="d6f84-114">When prompted, select **Yes** toodelete hello cluster.</span></span>
   
    ![Törlés ikonja](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="d6f84-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="d6f84-116">Azure PowerShell</span></span>

<span data-ttu-id="d6f84-117">Egy PowerShell-parancssorból a következő parancs toodelete hello fürt hello használata:</span><span class="sxs-lookup"><span data-stu-id="d6f84-117">From a PowerShell prompt, use hello following command toodelete hello cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="d6f84-118">Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d6f84-118">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="d6f84-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="d6f84-119">Azure CLI 1.0</span></span>

<span data-ttu-id="d6f84-120">A parancssorba használja a következő toodelete hello fürt hello:</span><span class="sxs-lookup"><span data-stu-id="d6f84-120">From a prompt, use hello following toodelete hello cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="d6f84-121">Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="d6f84-121">Replace **CLUSTERNAME** with hello name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="d6f84-122">Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök törlése most (2017. július 31.).</span><span class="sxs-lookup"><span data-stu-id="d6f84-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>