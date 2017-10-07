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
# <a name="delete-an-hdinsight-cluster-using-your-browser-powershell-or-hello-azure-cli"></a>A böngészőben, a PowerShell vagy az hello Azure parancssori felület használatával HDInsight-fürtök törlése

HDInsight-fürt számlázási elindul, miután a fürt jön létre, és leállítja a hello fürt törlésekor. Számlázási, egyenletesen percenként történik, így mindig törölnie kell a fürthöz, amikor már nincs használatban. Ebből a dokumentumból megismerheti, hogyan egy fürt használt toodelete hello Azure-portál, Azure PowerShell és hello Azure CLI 1.0.

> [!IMPORTANT]
> HDInsight-fürtök törlése nem érinti hello Azure Storage-fiókokat, vagy a Data Lake Store társított hello fürt. A jövőbeli hello e szolgáltatásokban tárolt adatokat is felhasználhatja.

## <a name="azure-portal"></a>Azure Portal

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) válassza ki a HDInsight-fürthöz. Ha a HDInsight-fürt nem rögzített toohello irányítópult, kereshet az hello keresési mező nevét.
   
    ![portál keresési](./media/hdinsight-delete-cluster/navbar.png)

2. Ha hello panel hello fürt nyit, válassza ki a hello **törlése** ikonra. Amikor a rendszer kéri, válassza ki a **Igen** toodelete hello fürt.
   
    ![Törlés ikonja](./media/hdinsight-delete-cluster/deletecluster.png)

## <a name="azure-powershell"></a>Azure PowerShell

Egy PowerShell-parancssorból a következő parancs toodelete hello fürt hello használata:

    Remove-AzureRmHDInsightCluster -ClusterName CLUSTERNAME

Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.

## <a name="azure-cli-10"></a>Azure CLI 1.0

A parancssorba használja a következő toodelete hello fürt hello:

    azure hdinsight cluster delete CLUSTERNAME

Cserélje le **CLUSTERNAME** hello nevet, a HDInsight-fürthöz.

> [!NOTE]
> Az Azure CLI 2.0 nem támogatja a HDInsight-fürtök törlése most (2017. július 31.).