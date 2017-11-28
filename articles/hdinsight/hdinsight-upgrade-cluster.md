---
title: "aaaUpgrade HDInsight fürt tooa újabb verziója-Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan tooUpgrade HDInsight fürt tooa újabb verzióra."
services: hdinsight
documentationcenter: 
author: bhanupr
manager: asadk
editor: bhanupr
ms.assetid: 60eb573c-e639-4815-9fc6-ea8b106d8dbc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/04/2017
ms.author: bhanupr
ms.openlocfilehash: 5fff3c9bc88dfbcbc1ccb0188accdfbbec3a62f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="upgrade-hdinsight-cluster-tooa-newer-version"></a><span data-ttu-id="d42af-103">A HDInsight fürt tooa újabb verzióra frissítése</span><span class="sxs-lookup"><span data-stu-id="d42af-103">Upgrade HDInsight cluster tooa newer version</span></span>
<span data-ttu-id="d42af-104">tootake előnyeit hello legújabb HDInsight-funkciókat, azt javasoljuk, hogy a HDInsight-fürtök kell-e a frissített toolatest verziója.</span><span class="sxs-lookup"><span data-stu-id="d42af-104">tootake advantage of hello latest HDInsight features, we recommend that HDInsight clusters be upgraded toolatest version.</span></span> <span data-ttu-id="d42af-105">Kövesse az alábbi irányelvek tooupgrade hello a HDInsight-fürt verziók.</span><span class="sxs-lookup"><span data-stu-id="d42af-105">Follow hello below guidelines tooupgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="d42af-106">HDInsight-fürt 3.2-es és 3.3-as verziója hamarosan kivezetési dátum.</span><span class="sxs-lookup"><span data-stu-id="d42af-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="d42af-107">Információk a HDInsight támogatott verzióján: [HDInsight összetevő verziók](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="d42af-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="d42af-108">Frissítési feladatok</span><span class="sxs-lookup"><span data-stu-id="d42af-108">Upgrade tasks</span></span>
<span data-ttu-id="d42af-109">hello munkafolyamat tooupgrade a HDInsight-fürthöz az alábbiak szerint van.</span><span class="sxs-lookup"><span data-stu-id="d42af-109">hello workflow tooupgrade HDInsight Cluster is as follows.</span></span>

![Frissítési munkafolyamat diagramja](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="d42af-111">Olvassa el a dokumentum minden szakaszát toounderstand módosítások, amelyek szükségesek lehetnek a HDInsight-fürt frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="d42af-111">Read each section of this document toounderstand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="d42af-112">Hozzon létre egy fürtöt, a teszt/minőségi megbízhatósági környezetekben.</span><span class="sxs-lookup"><span data-stu-id="d42af-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="d42af-113">A fürtök létrehozásáról további információk: [megtudhatja, hogyan toocreate Linux-alapú HDInsight-fürtök](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="d42af-113">For more information on creating a cluster, see [Learn how toocreate Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="d42af-114">Másolás meglévő feladatokat, adatforrások, és új környezet toohello fogadók esetében.</span><span class="sxs-lookup"><span data-stu-id="d42af-114">Copy existing jobs, data sources, and sinks toohello new environment.</span></span> <span data-ttu-id="d42af-115">Lásd: [adatok másolása tooTest környezet](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="d42af-115">See [Copy Data tooTest Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="d42af-116">Tesztelési meg arról, hogy a feladatok hello új fürt a várt módon működik-e toomake végez.</span><span class="sxs-lookup"><span data-stu-id="d42af-116">Perform validation testing toomake sure that your jobs work as expected on hello new cluster.</span></span>


<span data-ttu-id="d42af-117">Miután ellenőrizte, hogy minden megfelelően működik-e, tervezze hello áttelepítésre.</span><span class="sxs-lookup"><span data-stu-id="d42af-117">Once you have verified that everything works as expected, schedule downtime for hello migration.</span></span> <span data-ttu-id="d42af-118">Üzemszünet, során hello alábbi műveleteket:</span><span class="sxs-lookup"><span data-stu-id="d42af-118">During this downtime, do hello following actions:</span></span>

1.  <span data-ttu-id="d42af-119">Bármely átmeneti hello fürtcsomópontokon helyben tárolt adatok biztonsági mentését.</span><span class="sxs-lookup"><span data-stu-id="d42af-119">Back up any transient data stored locally on hello cluster nodes.</span></span> <span data-ttu-id="d42af-120">Ha például közvetlenül egy átjárócsomóponttal tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="d42af-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="d42af-121">Hello meglévő fürt törlése.</span><span class="sxs-lookup"><span data-stu-id="d42af-121">Delete hello existing cluster.</span></span>
3.  <span data-ttu-id="d42af-122">Hozzon létre egy fürtöt hello ugyanazon alapértelmezett adattároló hello használt előző fürt azonos virtuális hálózaton a legújabb (vagy a támogatott) alhálózati HDI verzióját használja a hello.</span><span class="sxs-lookup"><span data-stu-id="d42af-122">Create a cluster in hello same VNET subnet with latest (or supported) HDI version using hello same default data store that hello previous cluster used.</span></span> <span data-ttu-id="d42af-123">Ez lehetővé teszi a hello azt az új fürt toocontinue, működik-e a meglévő éles adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="d42af-123">This allows hello new cluster toocontinue working against your existing production data.</span></span>
4.  <span data-ttu-id="d42af-124">Biztonsági másolatot készített az átmeneti adatok importálása.</span><span class="sxs-lookup"><span data-stu-id="d42af-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="d42af-125">Kezdő feladatok/folytatni hello új fürt segítségével.</span><span class="sxs-lookup"><span data-stu-id="d42af-125">Start jobs/continue processing using hello new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d42af-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d42af-126">Next Steps</span></span>
* [<span data-ttu-id="d42af-127">Ismerje meg, hogyan toocreate Linux-alapú HDInsight-fürtök</span><span class="sxs-lookup"><span data-stu-id="d42af-127">Learn how toocreate Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="d42af-128">Csatlakozzon az SSH használatával tooHDInsight</span><span class="sxs-lookup"><span data-stu-id="d42af-128">Connect tooHDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="d42af-129">A Linux-alapú fürt Ambari kezelése</span><span class="sxs-lookup"><span data-stu-id="d42af-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

