---
title: "Egy újabb verzióra frissítés HDInsight-fürt-Azure |} Microsoft Docs"
description: "Megtudhatja, hogyan kell frissíteni a HDInsight-fürt egy újabb verzióra."
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
ms.openlocfilehash: fa2e37bd922690322ccc3d8f68128180d013b701
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="upgrade-hdinsight-cluster-to-a-newer-version"></a><span data-ttu-id="d798d-103">HDInsight-fürt frissítése újabb verzióra</span><span class="sxs-lookup"><span data-stu-id="d798d-103">Upgrade HDInsight cluster to a newer version</span></span>
<span data-ttu-id="d798d-104">A legújabb HDInsight-funkciók előnyeit, azt javasoljuk, hogy a HDInsight-fürtök legújabb verzióra frissíteni.</span><span class="sxs-lookup"><span data-stu-id="d798d-104">To take advantage of the latest HDInsight features, we recommend that HDInsight clusters be upgraded to latest version.</span></span> <span data-ttu-id="d798d-105">Kövesse az útmutatást frissítése a HDInsight alatt fürt verziók.</span><span class="sxs-lookup"><span data-stu-id="d798d-105">Follow the below guidelines to upgrade your HDInsight cluster versions.</span></span>

> [!NOTE]
> <span data-ttu-id="d798d-106">HDInsight-fürt 3.2-es és 3.3-as verziója hamarosan kivezetési dátum.</span><span class="sxs-lookup"><span data-stu-id="d798d-106">HDInsight clusters version 3.2 and 3.3 are nearing retirement date.</span></span> <span data-ttu-id="d798d-107">Információk a HDInsight támogatott verzióján: [HDInsight összetevő verziók](hdinsight-component-versioning.md#supported-hdinsight-versions).</span><span class="sxs-lookup"><span data-stu-id="d798d-107">For information on supported version of HDInsight, see [HDInsight component versions](hdinsight-component-versioning.md#supported-hdinsight-versions).</span></span>
>
>

## <a name="upgrade-tasks"></a><span data-ttu-id="d798d-108">Frissítési feladatok</span><span class="sxs-lookup"><span data-stu-id="d798d-108">Upgrade tasks</span></span>
<span data-ttu-id="d798d-109">A munkafolyamat HDInsight-fürt frissítése a következőképpen történik.</span><span class="sxs-lookup"><span data-stu-id="d798d-109">The workflow to upgrade HDInsight Cluster is as follows.</span></span>

![Frissítési munkafolyamat diagramja](./media/hdinsight-upgrade-cluster/upgrade-workflow.png)

1. <span data-ttu-id="d798d-111">Olvassa el a módosításokat, amelyek szükségesek lehetnek a HDInsight-fürt frissítésekor megérteni a dokumentum minden szakaszát.</span><span class="sxs-lookup"><span data-stu-id="d798d-111">Read each section of this document to understand changes that may be required when upgrading your HDInsight cluster.</span></span>
2. <span data-ttu-id="d798d-112">Hozzon létre egy fürtöt, a teszt/minőségi megbízhatósági környezetekben.</span><span class="sxs-lookup"><span data-stu-id="d798d-112">Create a cluster as a test/quality assurance environment.</span></span> <span data-ttu-id="d798d-113">A fürtök létrehozásáról további információk: [útmutató Linux-alapú HDInsight-fürtök létrehozása](hdinsight-hadoop-provision-linux-clusters.md)</span><span class="sxs-lookup"><span data-stu-id="d798d-113">For more information on creating a cluster, see [Learn how to create Linux-based HDInsight clusters](hdinsight-hadoop-provision-linux-clusters.md)</span></span>
3. <span data-ttu-id="d798d-114">Másolja a meglévő feladatokat, az adatforrások és mosdók az új környezetbe.</span><span class="sxs-lookup"><span data-stu-id="d798d-114">Copy existing jobs, data sources, and sinks to the new environment.</span></span> <span data-ttu-id="d798d-115">Lásd: [másolási adatok a tesztkörnyezet](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) további részleteket.</span><span class="sxs-lookup"><span data-stu-id="d798d-115">See [Copy Data To Test Environment](hdinsight-migrate-from-windows-to-linux.md#copy-data-to-the-test-environment) for more details.</span></span>
4. <span data-ttu-id="d798d-116">Hajtsa végre az ellenőrzés alá vonni, győződjön meg arról, hogy a feladatok az új fürt a várt módon működik-e.</span><span class="sxs-lookup"><span data-stu-id="d798d-116">Perform validation testing to make sure that your jobs work as expected on the new cluster.</span></span>


<span data-ttu-id="d798d-117">Miután ellenőrizte, hogy minden megfelelően működik-e, az áttelepítés tervezze.</span><span class="sxs-lookup"><span data-stu-id="d798d-117">Once you have verified that everything works as expected, schedule downtime for the migration.</span></span> <span data-ttu-id="d798d-118">Üzemszünet, során a következő műveleteket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="d798d-118">During this downtime, do the following actions:</span></span>

1.  <span data-ttu-id="d798d-119">Készítsen biztonsági másolatot a fürtcsomópontokon helyileg tárolt átmeneti adatok.</span><span class="sxs-lookup"><span data-stu-id="d798d-119">Back up any transient data stored locally on the cluster nodes.</span></span> <span data-ttu-id="d798d-120">Ha például közvetlenül egy átjárócsomóponttal tárolt adatokat.</span><span class="sxs-lookup"><span data-stu-id="d798d-120">For example, if you have data stored directly on a head node.</span></span>
2.  <span data-ttu-id="d798d-121">Törölje a meglévő fürtből.</span><span class="sxs-lookup"><span data-stu-id="d798d-121">Delete the existing cluster.</span></span>
3.  <span data-ttu-id="d798d-122">Hozzon létre egy fürtöt ugyanazon virtuális hálózat alhálózatában legújabb (vagy nem támogatott) HDI-verziója alapértelmezett ugyanazon adattár, amelyeket az előző használt használatával.</span><span class="sxs-lookup"><span data-stu-id="d798d-122">Create a cluster in the same VNET subnet with latest (or supported) HDI version using the same default data store that the previous cluster used.</span></span> <span data-ttu-id="d798d-123">Ez lehetővé teszi, hogy az új fürtön való további munkához a meglévő éles adatok alapján.</span><span class="sxs-lookup"><span data-stu-id="d798d-123">This allows the new cluster to continue working against your existing production data.</span></span>
4.  <span data-ttu-id="d798d-124">Biztonsági másolatot készített az átmeneti adatok importálása.</span><span class="sxs-lookup"><span data-stu-id="d798d-124">Import any transient data you backed up.</span></span>
5.  <span data-ttu-id="d798d-125">Kezdő feladatok/folytatni az új fürt segítségével.</span><span class="sxs-lookup"><span data-stu-id="d798d-125">Start jobs/continue processing using the new cluster.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d798d-126">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d798d-126">Next Steps</span></span>
* [<span data-ttu-id="d798d-127">Ismerje meg a Linux-alapú HDInsight-fürtök létrehozása</span><span class="sxs-lookup"><span data-stu-id="d798d-127">Learn how to create Linux-based HDInsight clusters</span></span>](hdinsight-hadoop-provision-linux-clusters.md)
* [<span data-ttu-id="d798d-128">HDInsight SSH használatával kapcsolódni</span><span class="sxs-lookup"><span data-stu-id="d798d-128">Connect to HDInsight using SSH</span></span>](hdinsight-hadoop-linux-use-ssh-unix.md)
* [<span data-ttu-id="d798d-129">A Linux-alapú fürt Ambari kezelése</span><span class="sxs-lookup"><span data-stu-id="d798d-129">Manage a Linux-based cluster using Ambari</span></span>](hdinsight-hadoop-manage-ambari.md)

