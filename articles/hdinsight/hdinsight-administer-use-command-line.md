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
# <a name="manage-hadoop-clusters-in-hdinsight-using-hello-azure-cli"></a>Hello Azure parancssori felület használatával hdinsight Hadoop-fürtök kezelése
[!INCLUDE [selector](../../includes/hdinsight-portal-management-selector.md)]

Megtudhatja, hogyan toouse hello [Azure parancssori felület](../cli-install-nodejs.md) toomanage Hadoop-fürtök az Azure HDInsight. Node.js hello Azure parancssori felület implementálva van. Használható bármilyen platformon, amely támogatja a Node.js-t, beleértve a Windows, Mac és Linux platformokat.

Ez a cikk ismerteti, hogy csak hello Azure CLI használata a HDInsight. Egy általános útmutató az Azure CLI toouse lásd: [telepítése és konfigurálása az Azure parancssori felület][azure-command-line-tools].

[!INCLUDE [use-latest-version](../../includes/hdinsight-use-latest-cli.md)]

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk elkezdéséhez hello következő kell rendelkeznie:

* **Azure-előfizetés**. Lásd: [Ingyenes Azure-fiók létrehozása](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* **Az Azure CLI** -lásd: [telepítése és konfigurálása az Azure parancssori felület hello](../cli-install-nodejs.md) telepítési és konfigurációs információt.
* **Csatlakozás tooAzure**használatával hello a következő parancsot:
  
        azure login
  
    Munkahelyi vagy iskolai fiókkal hitelesítésről további információkért lásd: [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../xplat-cli-connect.md).
* **Kapcsoló toohello Azure Resource Manager módra**használatával hello a következő parancsot:
  
        azure config mode arm

tooget segítségre van szüksége, használja a hello **-h** váltani.  Példa:

    azure hdinsight cluster create -h

## <a name="create-clusters-with-hello-cli"></a>Létre fürtöket hello parancssori felület
Lásd: [hozzon létre fürt a Hdinsightban az Azure CLI hello](hdinsight-hadoop-create-linux-clusters-azure-cli.md).

## <a name="list-and-show-cluster-details"></a>Listáról, és a fürt részleteinek megjelenítése
A következő parancsok toolist hello használja, és a fürt részleteinek megjelenítése:

    azure hdinsight cluster list
    azure hdinsight cluster show <Cluster Name>

![A fürt listájának parancssori megtekintése][image-cli-clusterlisting]

## <a name="delete-clusters"></a>Fürtök törlése
A következő parancs toodelete fürt hello használata:

    azure hdinsight cluster delete <Cluster Name>

A fürt hello fürt tartalmazó erőforráscsoportot hello törlésével is törli. Vegye figyelembe, többek között az alapértelmezett tárfiók hello hello csoport minden hello erőforrásának törölni szeretné.

    azure group delete <Resource Group Name>

## <a name="scale-clusters"></a>Fürtök méretezése
toochange hello Hadoop fürtméret:

    azure hdinsight cluster resize [options] <clusterName> <Target Instance Count>


## <a name="enabledisable-http-access-for-a-cluster"></a>Fürt HTTP-hozzáférés engedélyezése vagy letiltása
    azure hdinsight cluster enable-http-access [options] <Cluster Name> <userName> <password>
    azure hdinsight cluster disable-http-access [options] <Cluster Name>

## <a name="enabledisable-rdp-access-for-a-cluster"></a>Engedélyezi/letiltja az RDP-hozzáférést a fürthöz
      azure hdinsight cluster enable-rdp-access [options] <Cluster Name> <rdpUserName> <rdpPassword> <rdpExpiryDate>
      azure hdinsight cluster disable-rdp-access [options] <Cluster Name>


## <a name="next-steps"></a>Következő lépések
Ebben a cikkben megtanulta, hogyan tooperform különböző HDInsight fürt felügyeleti feladatokat. toolearn több, tekintse meg a következő cikkek hello:

* [HDInsight felügyelete hello Azure portál használatával][hdinsight-admin-portal]
* [HDInsight felügyelete az Azure PowerShell használatával][hdinsight-admin-powershell]
* [Azure HDInsight – első lépések][hdinsight-get-started]
* [Hogyan toouse hello Azure parancssori felület][azure-command-line-tools]

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
