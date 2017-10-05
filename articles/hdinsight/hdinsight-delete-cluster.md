---
title: "HDInsight-fürtök - Azure törlése |} Microsoft Docs"
description: "Információ a különböző módszereket, hogy egy HDInsight-fürt törlése."
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
ms.openlocfilehash: 65dac529df15d2dd43eec17673d82a2832f7692e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-the-azure-cli"></a><span data-ttu-id="915dc-103">A böngésző, PowerShell vagy az Azure parancssori felület használatával HDInsight-fürtök törlése</span><span class="sxs-lookup"><span data-stu-id="915dc-103">Delete an HDInsight cluster using your browser, PowerShell, or the Azure CLI</span></span>

<span data-ttu-id="915dc-104">A HDInsight fürt számlázás követően egy fürt jön létre, és leállítja a fürt törlésekor indul.</span><span class="sxs-lookup"><span data-stu-id="915dc-104">HDInsight cluster billing starts once a cluster is created and stops when the cluster is deleted.</span></span> <span data-ttu-id="915dc-105">Számlázási, egyenletesen percenként történik, így mindig törölnie kell a fürthöz, amikor már nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="915dc-105">Billing is pro-rated per minute, so you should always delete your cluster when it is no longer in use.</span></span> <span data-ttu-id="915dc-106">Ebből a dokumentumból megismerheti, hogyan törlése az Azure portál, Azure PowerShell és az Azure CLI 1.0 egy fürtöt.</span><span class="sxs-lookup"><span data-stu-id="915dc-106">In this document, you learn how to delete a cluster using the Azure portal, Azure PowerShell, and the Azure CLI 1.0.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="915dc-107">HDInsight-fürtök törlése nem érinti az Azure Storage-fiókokat, vagy a Data Lake Store a fürthöz rendelt.</span><span class="sxs-lookup"><span data-stu-id="915dc-107">Deleting an HDInsight cluster does not delete the Azure Storage accounts or Data Lake Store associated with the cluster.</span></span> <span data-ttu-id="915dc-108">Ezek a szolgáltatások a jövőben tárolt adatokat is felhasználhatja.</span><span class="sxs-lookup"><span data-stu-id="915dc-108">You can reuse data stored in those services in the future.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="915dc-109">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="915dc-109">Azure portal</span></span>

1. <span data-ttu-id="915dc-110">Jelentkezzen be a [Azure-portálon](https://portal.azure.com) válassza ki a HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="915dc-110">Log in to the [Azure portal](https://portal.azure.com) and select your HDInsight cluster.</span></span> <span data-ttu-id="915dc-111">Ha a HDInsight-fürt nincs rögzítve az irányítópulton, kereshet, használja a keresőmezőt név szerint.</span><span class="sxs-lookup"><span data-stu-id="915dc-111">If your HDInsight cluster is not pinned to the dashboard, you can search for it by name using the search field.</span></span>
   
    ![portál keresési](./media/hdinsight-delete-cluster/navbar.png)

2. <span data-ttu-id="915dc-113">Ha a panel nyit a fürt számára, jelölje ki a **törlése** ikonra.</span><span class="sxs-lookup"><span data-stu-id="915dc-113">Once the blade opens for the cluster, select the **Delete** icon.</span></span> <span data-ttu-id="915dc-114">Amikor a rendszer kéri, válassza ki a **Igen** törölni a fürtöt.</span><span class="sxs-lookup"><span data-stu-id="915dc-114">When prompted, select **Yes** to delete the cluster.</span></span>
   
    ![Törlés ikonja](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a><span data-ttu-id="915dc-116">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="915dc-116">Azure PowerShell</span></span>

<span data-ttu-id="915dc-117">Egy PowerShell-parancssorból a következő parancs segítségével törölheti a fürtöt:</span><span class="sxs-lookup"><span data-stu-id="915dc-117">From a PowerShell prompt, use the following command to delete the cluster:</span></span>

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

<span data-ttu-id="915dc-118">Cserélje le a **CLUSTERNAME** kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="915dc-118">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

## <a name="azure-cli-10"></a><span data-ttu-id="915dc-119">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="915dc-119">Azure CLI 1.0</span></span>

<span data-ttu-id="915dc-120">A parancssorba használja a következő törölni a fürtöt:</span><span class="sxs-lookup"><span data-stu-id="915dc-120">From a prompt, use the following to delete the cluster:</span></span>

    azure hdinsight cluster delete CLUSTERNAME

<span data-ttu-id="915dc-121">Cserélje le a **CLUSTERNAME** kifejezést a HDInsight-fürt nevére.</span><span class="sxs-lookup"><span data-stu-id="915dc-121">Replace **CLUSTERNAME** with the name of your HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="915dc-122">Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök törlése most (2017. július 31.).</span><span class="sxs-lookup"><span data-stu-id="915dc-122">Azure CLI 2.0 does not support deleting HDInsight clusters at this time (July 31, 2017).</span></span>