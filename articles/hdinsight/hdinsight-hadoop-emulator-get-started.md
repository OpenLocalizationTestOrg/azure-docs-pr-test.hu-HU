---
title: "-emulátor - Azure HDInsight Hadoop védőfalat használatával aaaLearn |} Microsoft Docs"
description: "learning használatáról toostart hello Hadoop ökoszisztémájának, állíthatja be a Hadoop védőfalak Hortonworks az Azure virtuális géphez. "
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
ms.openlocfilehash: 91e74f0823fd02e9bb812155a7d09357a77b0736
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-a-hadoop-sandbox-an-emulator-on-a-virtual-machine"></a><span data-ttu-id="57ca7-104">Ismerkedés a Hadoop védőfalat, az emulátor egy virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="57ca7-104">Get started with a Hadoop sandbox, an emulator on a virtual machine</span></span>

<span data-ttu-id="57ca7-105">Ismerje meg, hogyan tooinstall hello Hadoop védőfal a Hortonworks meg a virtuális gép toolearn hello Hadoop ökoszisztémájának kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="57ca7-105">Learn how tooinstall hello Hadoop sandbox from Hortonworks on a virtual machine toolearn about hello Hadoop ecosystem.</span></span> <span data-ttu-id="57ca7-106">hello védőfal biztosít egy helyi fejlesztési környezet toolearn Hadoop, a Hadoop elosztott fájlrendszerrel (HDFS) és a feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="57ca7-106">hello sandbox provides a local development environment toolearn about Hadoop, Hadoop Distributed File System (HDFS), and job submission.</span></span> <span data-ttu-id="57ca7-107">Ha ismeri a Hadoop, megkezdheti a Hadoop használatával az Azure HDInsight-fürtök létrehozásával.</span><span class="sxs-lookup"><span data-stu-id="57ca7-107">Once you are familiar with Hadoop, you can start using Hadoop on Azure by creating an HDInsight cluster.</span></span> <span data-ttu-id="57ca7-108">A tooget indításának további információkért lásd: [beolvasása használatába a HDInsight Hadoop](hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="57ca7-108">For more information on how tooget started, see [Get started with Hadoop on HDInsight](hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="57ca7-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="57ca7-109">Prerequisites</span></span>
* <span data-ttu-id="57ca7-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span><span class="sxs-lookup"><span data-stu-id="57ca7-110">[Oracle VirtualBox](https://www.virtualbox.org/).</span></span> <span data-ttu-id="57ca7-111">Töltse le és telepítse azt [Itt](https://www.virtualbox.org/wiki/Downloads).</span><span class="sxs-lookup"><span data-stu-id="57ca7-111">Download and install it from [here](https://www.virtualbox.org/wiki/Downloads).</span></span>



## <a name="download-and-install-hello-virtual-machine"></a><span data-ttu-id="57ca7-112">Töltse le és telepítse a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="57ca7-112">Download and install hello virtual machine</span></span>
1. <span data-ttu-id="57ca7-113">Keresse meg a toohello [Hortonworks letölti](http://hortonworks.com/downloads/#sandbox).</span><span class="sxs-lookup"><span data-stu-id="57ca7-113">Browse toohello [Hortonworks downloads](http://hortonworks.com/downloads/#sandbox).</span></span>

2. <span data-ttu-id="57ca7-114">Kattintson a **töltse le a VIRTUALBOX** toodownload hello legújabb Hortonworks védőfal a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="57ca7-114">Click **DOWNLOAD FOR VIRTUALBOX** toodownload hello latest Hortonworks Sandbox on a VM.</span></span> <span data-ttu-id="57ca7-115">A Hortonworks rákérdezéses tooregister hello letöltési megkezdése előtt áll.</span><span class="sxs-lookup"><span data-stu-id="57ca7-115">You are prompted tooregister with Hortonworks before hello download begins.</span></span> <span data-ttu-id="57ca7-116">A hálózat sebességétől függően egy tootwo óra toodownload vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="57ca7-116">It takes one tootwo hours toodownload depending on your network speed.</span></span>
   
    ![Hivatkozás kép VirtualBox a Hortonworks védőfal letöltése](./media/hdinsight-hadoop-emulator-get-started/download-sandbox.png)
3. <span data-ttu-id="57ca7-118">Az azonos weblapon hello, kattintson a hello **importálás virtuális mezőbe az** toodownload hello virtuális gép telepítési utasításokat tartalmazó PDF-fájl csatolása.</span><span class="sxs-lookup"><span data-stu-id="57ca7-118">From hello same web page, click hello **Import on Virtual Box** link toodownload a PDF containing installation instructions for hello virtual machine.</span></span>

<span data-ttu-id="57ca7-119">egy régebbi HDP verzió védőfal toodownload bontsa ki a hello archív:</span><span class="sxs-lookup"><span data-stu-id="57ca7-119">toodownload an older HDP version sandbox, expand hello archive:</span></span>

![Hortonworks védőfal archív](./media/hdinsight-hadoop-emulator-get-started/hortonworks-sandbox-archive.png)


## <a name="start-hello-virtual-machine"></a><span data-ttu-id="57ca7-121">Hello virtuális gép elindítása</span><span class="sxs-lookup"><span data-stu-id="57ca7-121">Start hello virtual machine</span></span>

1. <span data-ttu-id="57ca7-122">Nyissa meg az Oracle VM VirtualBox.</span><span class="sxs-lookup"><span data-stu-id="57ca7-122">Open Oracle VM VirtualBox.</span></span>
2. <span data-ttu-id="57ca7-123">A hello **fájl** menüben kattintson a **importálási készülék**, majd adja meg a hello Hortonworks védőfal kép.</span><span class="sxs-lookup"><span data-stu-id="57ca7-123">From hello **File** menu, click **Import Appliance**, and then specify hello Hortonworks Sandbox image.</span></span>
1. <span data-ttu-id="57ca7-124">Jelölje be hello Hortonworks védőfal, kattintson a **Start**, majd **normál Start**.</span><span class="sxs-lookup"><span data-stu-id="57ca7-124">Select hello Hortonworks Sandbox, click **Start**, and then **Normal Start**.</span></span> <span data-ttu-id="57ca7-125">Miután hello virtuális gép hello rendszerindítási folyamat befejeződött, bejelentkezési utasítások jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="57ca7-125">Once hello virtual machine has finished hello boot process, it displays login instructions.</span></span>
   
    ![Normál indítása](./media/hdinsight-hadoop-emulator-get-started/normal-start.png)
2. <span data-ttu-id="57ca7-127">Nyisson meg egy webböngészőt, és keresse meg a toohello URL-cím jelenik meg (általában http://127.0.0.1:8888).</span><span class="sxs-lookup"><span data-stu-id="57ca7-127">Open a web browser and navigate toohello URL displayed (usually http://127.0.0.1:8888).</span></span>

## <a name="set-sandbox-passwords"></a><span data-ttu-id="57ca7-128">A védőfal jelszavak beállítása</span><span class="sxs-lookup"><span data-stu-id="57ca7-128">Set Sandbox passwords</span></span>

1. <span data-ttu-id="57ca7-129">A hello **Ismerkedés** hello Hortonworks védőfal lapon válassza ki a lépés **nézet speciális beállítások**.</span><span class="sxs-lookup"><span data-stu-id="57ca7-129">From hello **get started** step of hello Hortonworks Sandbox page, select **View Advanced Options**.</span></span> <span data-ttu-id="57ca7-130">A lap toolog az SSH használatával toohello védőfal hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="57ca7-130">Use hello information on this page toolog in toohello sandbox using SSH.</span></span> <span data-ttu-id="57ca7-131">Hello nevet és jelszót adott meg használja.</span><span class="sxs-lookup"><span data-stu-id="57ca7-131">Use hello name and password provided.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="57ca7-132">Ha nincs telepítve egy SSH-ügyfél, használhatja a hello a virtuális gép a megadott webes SSH hello **http://localhost:4200 /**.</span><span class="sxs-lookup"><span data-stu-id="57ca7-132">If you do not have an SSH client installed, you can use hello web-based SSH provided at by hello virtual machine at **http://localhost:4200/**.</span></span>
   > 
   
    <span data-ttu-id="57ca7-133">hello először létesítsen SSH-áll felszólító toochange hello hello root fiókjának jelszavát.</span><span class="sxs-lookup"><span data-stu-id="57ca7-133">hello first time you connect using SSH, you are prompted toochange hello password for hello root account.</span></span> <span data-ttu-id="57ca7-134">Adjon meg egy új jelszót, amelyet használhat, amikor bejelentkezik az SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="57ca7-134">Enter a new password, which you use when you log in using SSH.</span></span>

2. <span data-ttu-id="57ca7-135">Miután bejelentkezett, adja meg a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="57ca7-135">Once logged in, enter hello following command:</span></span>
   
        ambari-admin-password-reset
   
    <span data-ttu-id="57ca7-136">Amikor a rendszer kéri, adja meg hello Ambari rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="57ca7-136">When prompted, provide a password for hello Ambari admin account.</span></span> <span data-ttu-id="57ca7-137">Ez használható hello Ambari webes felhasználói felületén elérésekor.</span><span class="sxs-lookup"><span data-stu-id="57ca7-137">This is used when you access hello Ambari Web UI.</span></span>

## <a name="use-hive-commands"></a><span data-ttu-id="57ca7-138">Hive-parancsok használata</span><span class="sxs-lookup"><span data-stu-id="57ca7-138">Use Hive commands</span></span>

1. <span data-ttu-id="57ca7-139">Egy SSH kapcsolat toohello védőfalak használja a következő parancs toostart hello Hive rendszerhéjat hello:</span><span class="sxs-lookup"><span data-stu-id="57ca7-139">From an SSH connection toohello sandbox, use hello following command toostart hello Hive shell:</span></span>
   
        hive
2. <span data-ttu-id="57ca7-140">Amikor hello rendszerhéj elindult, használja a következő tooview hello táblák hello védőfal által biztosított hello:</span><span class="sxs-lookup"><span data-stu-id="57ca7-140">Once hello shell has started, use hello following tooview hello tables that are provided with hello sandbox:</span></span>
   
        show tables;
3. <span data-ttu-id="57ca7-141">Tooretrieve 10 sorok követően – hello használata hello `sample_07` tábla:</span><span class="sxs-lookup"><span data-stu-id="57ca7-141">Use hello following tooretrieve 10 rows from hello `sample_07` table:</span></span>
   
        select * from sample_07 limit 10;

## <a name="next-steps"></a><span data-ttu-id="57ca7-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57ca7-142">Next steps</span></span>
* [<span data-ttu-id="57ca7-143">Megtudhatja, hogyan hello Hortonworks védőfal a Visual Studio toouse</span><span class="sxs-lookup"><span data-stu-id="57ca7-143">Learn how toouse Visual Studio with hello Hortonworks Sandbox</span></span>](hdinsight-hadoop-emulator-visual-studio.md)
* [<span data-ttu-id="57ca7-144">Learning hello drótkötelek a hello Hortonworks védőfal</span><span class="sxs-lookup"><span data-stu-id="57ca7-144">Learning hello ropes of hello Hortonworks Sandbox</span></span>](http://hortonworks.com/hadoop-tutorial/learning-the-ropes-of-the-hortonworks-sandbox/)
* [<span data-ttu-id="57ca7-145">Hadoop oktatóanyag – első lépések HDP</span><span class="sxs-lookup"><span data-stu-id="57ca7-145">Hadoop tutorial - Getting started with HDP</span></span>](http://hortonworks.com/hadoop-tutorial/hello-world-an-introduction-to-hadoop-hcatalog-hive-and-pig/)

