---
title: "Ismerje meg, a Hadoop védőfalak - emulátor - Azure HDInsight használata |} Microsoft Docs"
description: "A Hadoop ökoszisztémájának használatával megtanulni elindításához állíthat be egy Hadoop védőfal a Hortonworks Azure virtuális géphez. "
keywords: "hadoop-emulátor, hadoop védőfal"
editor: cgronlun
manager: jhubbard
services: hdinsight
author: nitinme
documentationcenter: 
tags: azure-portal
ms.assetid: 6ad5bb58-8215-4e3d-a07f-07fcd8839cc6
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: b701879464205860edd1c097651b532f87bae388
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="05f39-104">Ismerkedés a Hadoop védőfalat, az emulátor egy virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="05f39-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="05f39-105">Ismerje meg, a Hadoop védőfal Hortonworks telepítéséről további információt a Hadoop ökoszisztémájának virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="05f39-105">Learn how to install the Hadoop sandbox from Hortonworks on a virtual machine to learn about the Hadoop ecosystem.</span></span> <span data-ttu-id="05f39-106">A védőfal Hadoop, a Hadoop elosztott fájlrendszerrel (HDFS) és a feladat elküldése helyi fejlesztési környezetet biztosít.</span><span class="sxs-lookup"><span data-stu-id="05f39-106">The sandbox provides a local development environment to learn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="05f39-107">Ha ismeri a Hadoop, megkezdheti a Hadoop használatával az Azure HDInsight-fürtök létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="05f39-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="05f39-108">Első lépések a további információkért lásd: [beolvasása használatába a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="05f39-108">For more information on how to get started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05f39-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="05f39-109">Prerequisites</span></span>
* <span data-ttu-id="05f39-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="05f39-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="05f39-111">Töltse le és telepítse azt [Itt](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="05f39-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-the-virtual-machine"></a><span data-ttu-id="05f39-112">Töltse le és telepítse a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="05f39-112">Download and install the virtual machine</span></span>
1. <span data-ttu-id="05f39-113">Keresse meg a [Hortonworks letölti](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="05f39-113">Browse to the [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="05f39-114">Kattintson a **töltse le a VIRTUALBOX** letölteni a legújabb Hortonworks védőfal a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="05f39-114">Click **DOWNLOAD FOR VIRTUALBOX** to download the latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="05f39-115">A letöltés megkezdése előtt Hortonworks regisztrálása kéri.</span><span class="sxs-lookup"><span data-stu-id="05f39-115">You are prompted to register with Hortonworks before the download begins.</span></span> <span data-ttu-id="05f39-116">Töltse le a hálózat sebességétől függően egy-két órát vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="05f39-116">It takes one to two hours to download depending on your network speed.</span></span>
   
    ![Hivatkozás kép VirtualBox a Hortonworks védőfal letöltése](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="05f39-118">A weblapon, kattintson a **virtuális párbeszédpanel importálás** a virtuális gép telepítési utasításokat tartalmazó PDF-fájl letöltésére mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="05f39-118">From the same web page, click the **Import on Virtual Box** link to download a PDF containing installation instructions for the virtual machine.</span></span>

<span data-ttu-id="05f39-119">Töltse le egy régebbi HDP verzió védőfal, bontsa ki az archív:</span><span class="sxs-lookup"><span data-stu-id="05f39-119">To download an older HDP version sandbox, expand the archive:</span></span>

![Hortonworks védőfal archív](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-the-virtual-machine"></a><span data-ttu-id="05f39-121">A virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="05f39-121">Start the virtual machine</span></span>

1. <span data-ttu-id="05f39-122">Nyissa meg az Oracle VM VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="05f39-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="05f39-123">A a **fájl** menüben kattintson **importálási készülék**, és adja meg a Hortonworks védőfal kép.</span><span class="sxs-lookup"><span data-stu-id="05f39-123">From the **File** menu, click **Import Appliance**, and then specify the Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="05f39-124">Jelölje ki azt a Hortonworks védőfal **Start**, majd **normál Start**.</span><span class="sxs-lookup"><span data-stu-id="05f39-124">Select the Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="05f39-125">Miután a virtuális gép a rendszerindítási folyamat befejeződött, bejelentkezési utasítások jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="05f39-125">Once the virtual machine has finished the boot process, it displays login instructions.</span></span>
   
    ![Normál indítása](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="05f39-127">Nyisson meg egy webböngészőt, és keresse meg az URL-cím jelenik meg (általában http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="05f39-127">Open a web browser and navigate to the URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="05f39-128">A védőfal jelszavak beállítása</span><span class="sxs-lookup"><span data-stu-id="05f39-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="05f39-129">Az a **Ismerkedés** a Hortonworks védőfal lapon jelölje be a lépés **nézet speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="05f39-129">From the **get started** step of the Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="05f39-130">Olvassa el ezen a lapon jelentkezzen be a védőfal SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="05f39-130">Use the information on this page to log in to the sandbox using SSH.</span></span> <span data-ttu-id="05f39-131">Használja a megadott felhasználónévvel és jelszóval.</span><span class="sxs-lookup"><span data-stu-id="05f39-131">Use the name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="05f39-132">Ha nincs telepítve egy SSH-ügyfél, használhatja a webalapú SSH, a virtuális gép által megadott **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="05f39-132">If you do not have an SSH client installed, you can use the web-based SSH provided at by the virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="05f39-133">Az SSH-létesítsen első alkalommal kéri a rendszergazdafiók jelszavának módosítása.</span><span class="sxs-lookup"><span data-stu-id="05f39-133">The first time you connect using SSH, you are prompted to change the password for the root account.</span></span> <span data-ttu-id="05f39-134">Adjon meg egy új jelszót, amelyet használhat, amikor bejelentkezik az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="05f39-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="05f39-135">Miután bejelentkezett, adja meg a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="05f39-135">Once logged in, enter the following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="05f39-136">Amikor a rendszer kéri, adja meg egy jelszót a Ambari rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="05f39-136">When prompted, provide a password for the Ambari admin account.</span></span> <span data-ttu-id="05f39-137">Ez használható az Ambari webes felhasználói felületén elérésekor.</span><span class="sxs-lookup"><span data-stu-id="05f39-137">This is used when you access the Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="05f39-138">Hive-parancsok használata</span><span class="sxs-lookup"><span data-stu-id="05f39-138">Use Hive commands</span></span>

1. <span data-ttu-id="05f39-139">Az SSH-kapcsolatot a védőfal alkalmazás a Hive rendszerhéjat indítsa el a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="05f39-139">From an SSH connection to the sandbox, use the following command to start the Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="05f39-140">Amikor a rendszerhéj elindult, használja a következő a táblák, a védőfal által biztosított megtekintése:</span><span class="sxs-lookup"><span data-stu-id="05f39-140">Once the shell has started, use the following to view the tables that are provided with the sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="05f39-141">Használja a következő 10 sorát beolvasni a `sample_07` tábla:</span><span class="sxs-lookup"><span data-stu-id="05f39-141">Use the following to retrieve 10 rows from the `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="05f39-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="05f39-142">Next steps</span></span>
* [<span data-ttu-id="05f39-143">Ismerje meg a Visual Studio használata a Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="05f39-143">Learn how to use Visual Studio with the Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="05f39-144">Az a Hortonworks védőfal drótkötelek tanulási</span><span class="sxs-lookup"><span data-stu-id="05f39-144">Learning the ropes of the Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="05f39-145">Hadoop oktatóanyag – első lépések HDP</span><span class="sxs-lookup"><span data-stu-id="05f39-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

