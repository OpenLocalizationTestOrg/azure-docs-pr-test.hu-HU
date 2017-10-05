---
title: "Az adatok tudományos feldolgozáshoz Hadoop-fürtök testreszabása |} Microsoft Docs"
description: "Népszerű Python-modulok egyéni Azure HDInsight Hadoop-fürtök elérhetik."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 0c115dca-2565-4e7a-9536-6002af5c786a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: hangzh;bradsev
ms.openlocfilehash: 53ff04ee66b08ae36f3550536c659a547c658fd9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-the-team-data-science-process"></a><span data-ttu-id="b51c1-103">Az Azure HDInsight Hadoop-fürtök testreszabása a csoportos adatelemzési folyamathoz</span><span class="sxs-lookup"><span data-stu-id="b51c1-103">Customize Azure HDInsight Hadoop clusters for the Team Data Science Process</span></span>
<span data-ttu-id="b51c1-104">Ez a cikk ismerteti hogyan szabhatja testre a HDInsight Hadoop-fürt telepítésével, 64 bites Anaconda (Python 2.7) minden egyes csomóponton, amikor a fürt ki van építve a HDInsight-szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="b51c1-104">This article describes how to customize an HDInsight Hadoop cluster by installing 64-bit Anaconda (Python 2.7) on each node when the cluster is provisioned as an HDInsight service.</span></span> <span data-ttu-id="b51c1-105">Azt is bemutatja, hogyan a fürthöz egyéni feladatok küldéséhez headnode eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b51c1-105">It also shows how to access the headnode to submit custom jobs to the cluster.</span></span> <span data-ttu-id="b51c1-106">Ez a beállítás lehetővé teszi számos népszerű Python modult, szereplő Anaconda kényelmesen használható felhasználó által megadott funkciókat (UDF), melyet úgy terveztek, a fürt Hive-rekordok feldolgozásához.</span><span class="sxs-lookup"><span data-stu-id="b51c1-106">This customization makes many popular Python modules, that are included in Anaconda, conveniently available for use in user defined functions (UDFs) that are designed to process Hive records in the cluster.</span></span> <span data-ttu-id="b51c1-107">Ebben a forgatókönyvben használt eljárásokat, lásd: [hogyan elküldeni a Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="b51c1-107">For instructions on the procedures used in this scenario, see [How to submit Hive queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

<span data-ttu-id="b51c1-108">A következő menü hivatkozások témakörök azt ismertetik, hogyan állíthatja be a különböző adatok által használt tudományos környezetekben a [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b51c1-108">The following menu links to topics that describe how to set up the various data science environments used by the [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <span data-ttu-id="b51c1-109"><a name="customize"></a>Az Azure HDInsight Hadoop-fürt testreszabása</span><span class="sxs-lookup"><span data-stu-id="b51c1-109"><a name="customize"></a>Customize Azure HDInsight Hadoop Cluster</span></span>
<span data-ttu-id="b51c1-110">Testreszabott HDInsight Hadoop-fürt létrehozása, indítsa el a naplózás [ **a klasszikus Azure portálon**](https://manage.windowsazure.com/), kattintson a **új** bal alsó sarokban, és adja DATA SERVICES -> HDINSIGHT -> **egyéni létrehozás** elindítani a **a fürt részleteinek** ablak.</span><span class="sxs-lookup"><span data-stu-id="b51c1-110">To create a customized HDInsight Hadoop cluster, start by logging on to [**Azure classic portal**](https://manage.windowsazure.com/), click **New** at the left bottom corner, and then select DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** to bring up the **Cluster Details** window.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="b51c1-112">A konfiguráció lapon 1 létrehozni a fürt nevét, és fogadja el a más mezők alapértelmezett értékét.</span><span class="sxs-lookup"><span data-stu-id="b51c1-112">Input the name of the cluster to be created on configuration page 1, and accept default values for the other fields.</span></span> <span data-ttu-id="b51c1-113">Kattintson a nyílra kattintva nyissa meg a következő konfigurációs oldalát.</span><span class="sxs-lookup"><span data-stu-id="b51c1-113">Click the arrow to go to the next configuration page.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="b51c1-115">A konfiguráció lapon 2 bemeneti száma **ADATCSOMÓPONTOKAT**, jelölje be a **régió/virtuális hálózati**, és válassza ki a méretét a **ÁTJÁRÓCSOMÓPONT** és a **ADATCSOMÓPONTON**.</span><span class="sxs-lookup"><span data-stu-id="b51c1-115">On configuration page 2, input the number of **DATA NODES**, select the **REGION/VIRTUAL NETWORK**, and select the sizes of the **HEAD NODE** and the **DATA NODE**.</span></span> <span data-ttu-id="b51c1-116">Kattintson a nyílra kattintva nyissa meg a következő konfigurációs oldalát.</span><span class="sxs-lookup"><span data-stu-id="b51c1-116">Click the arrow to go to the next configuration page.</span></span>

> [!NOTE]
> <span data-ttu-id="b51c1-117">A **régió/virtuális hálózati** lehet ugyanaz, mint a területet a storage-fiók, amelyet a HDInsight Hadoop-fürt használni szeretne.</span><span class="sxs-lookup"><span data-stu-id="b51c1-117">The **REGION/VIRTUAL NETWORK** has to be the same as the region of the storage account that is going to be used for the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="b51c1-118">Ellenkező esetben a negyedik konfigurálása lapon, a tárfiók nem jelennek meg a legördülő listából a **fióknév**.</span><span class="sxs-lookup"><span data-stu-id="b51c1-118">Otherwise, in the fourth configuration page, the storage account will not appear on the dropdown list of **ACCOUNT NAME**.</span></span>
> 
> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

<span data-ttu-id="b51c1-120">A konfiguráció lapon 3 adja meg egy felhasználónevet és jelszót a HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="b51c1-120">On configuration page 3, provide a user name and password for the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="b51c1-121">**Ne** válassza ki a *adja meg a Hive/Oozie Metaadattárhoz*.</span><span class="sxs-lookup"><span data-stu-id="b51c1-121">**Do not** select the *Enter the Hive/Oozie Metastore*.</span></span> <span data-ttu-id="b51c1-122">Kattintson a nyílra kattintva nyissa meg a következő konfigurációs oldalát.</span><span class="sxs-lookup"><span data-stu-id="b51c1-122">Then, click the arrow to go to the next configuration page.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

<span data-ttu-id="b51c1-124">A konfiguráció lapon 4 adja meg a tárfiók nevét, a HDInsight Hadoop-fürt alapértelmezett tárolót.</span><span class="sxs-lookup"><span data-stu-id="b51c1-124">On configuration page 4, specify the storage account name, the default container of the HDInsight Hadoop cluster.</span></span> <span data-ttu-id="b51c1-125">Ha *hozzon létre alapértelmezett tároló* a a **alapértelmezett tároló** legördülő listában, egy azonos nevű tárolót, a fürt jön létre.</span><span class="sxs-lookup"><span data-stu-id="b51c1-125">If you select *Create default container* in the **DEFAULT CONTAINER** dropdown list, a container with the same name as the cluster will be created.</span></span> <span data-ttu-id="b51c1-126">Kattintson a nyílra kattintva nyissa meg az utolsó lap.</span><span class="sxs-lookup"><span data-stu-id="b51c1-126">Click the arrow to go to the last configuration page.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

<span data-ttu-id="b51c1-128">A végső **Parancsfájlműveletek** konfigurációs lapon kattintson a **parancsfájlművelet hozzáadása** gombra, és töltse ki a szövegmezők tartalma a következő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="b51c1-128">On the final **Script Actions** configuration page, click **add script action** button, and fill the text fields with the following values.</span></span>

* <span data-ttu-id="b51c1-129">**NÉV** -bármilyen karakterlánc, mint a parancsfájlművelet nevét</span><span class="sxs-lookup"><span data-stu-id="b51c1-129">**NAME** - any string as the name of this script action</span></span>
* <span data-ttu-id="b51c1-130">**A CSOMÓPONTTÍPUS** – Itt adhatja meg **minden csomópont**</span><span class="sxs-lookup"><span data-stu-id="b51c1-130">**NODE TYPE** - select **All nodes**</span></span>
* <span data-ttu-id="b51c1-131">**PARANCSFÁJL URI azonosítója** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span><span class="sxs-lookup"><span data-stu-id="b51c1-131">**SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span></span> 
  * <span data-ttu-id="b51c1-132">*publicscripts* a tárfiókban lévő nyilvános tárolója</span><span class="sxs-lookup"><span data-stu-id="b51c1-132">*publicscripts* is a public container in the storage account</span></span> 
  * <span data-ttu-id="b51c1-133">*getgoing* megosztása lehetővé teszi a felhasználók munkahelyi az Azure PowerShell-parancsfájlok használatával</span><span class="sxs-lookup"><span data-stu-id="b51c1-133">*getgoing* we use to share PowerShell script files to facilitate users' work in Azure</span></span>
* <span data-ttu-id="b51c1-134">**PARAMÉTEREK** -(hagyja üresen)</span><span class="sxs-lookup"><span data-stu-id="b51c1-134">**PARAMETERS** - (leave blank)</span></span>

<span data-ttu-id="b51c1-135">Végül kattintson a pipa jelre indítsa el a testre szabott HDInsight Hadoop-fürt létrehozását.</span><span class="sxs-lookup"><span data-stu-id="b51c1-135">Finally, click the check mark to start the creation of the customized HDInsight Hadoop cluster.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <span data-ttu-id="b51c1-137"><a name="headnode"></a>Hozzáférés a Hadoop-fürt Átjárócsomópontjához</span><span class="sxs-lookup"><span data-stu-id="b51c1-137"><a name="headnode"></a> Access the Head Node of Hadoop Cluster</span></span>
<span data-ttu-id="b51c1-138">RDP segítségével próbál hozzáférni a Hadoop-fürt átjárócsomópontjához engedélyeznie kell az Azure-ban Hadoop-fürt távoli eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b51c1-138">You must enable remote access to the Hadoop cluster in Azure before you can access the head node of the Hadoop cluster through RDP.</span></span> 

1. <span data-ttu-id="b51c1-139">Jelentkezzen be a [ **a klasszikus Azure portálon**](https://manage.windowsazure.com/), jelölje be **HDInsight** bal oldalán válassza ki a Hadoop-fürt fürtök a listából, kattintson a **konfigurációs** fülre, majd a **távoli engedélyezése** ikonra az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="b51c1-139">Log in to the [**Azure classic portal**](https://manage.windowsazure.com/), select **HDInsight** on the left, select your Hadoop cluster from the list of clusters, click the **CONFIGURATION** tab, and then click the **ENABLE REMOTE** icon at the bottom of the page.</span></span>
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. <span data-ttu-id="b51c1-141">Az a **konfigurálása a távoli asztal** ablak, írja be a FELHASZNÁLÓNEVET és a jelszó mező, és válassza ki a távelérés lejárati dátumát.</span><span class="sxs-lookup"><span data-stu-id="b51c1-141">In the **Configure Remote Desktop** window, enter the USER NAME and PASSWORD fields, and select the expiration date for remote access.</span></span> <span data-ttu-id="b51c1-142">Kattintson a pipa jelre a távoli hozzáférés a Hadoop-fürt átjárócsomópontjához engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="b51c1-142">Then click the check mark to enable the remote access to the head node of the Hadoop cluster.</span></span>
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> <span data-ttu-id="b51c1-144">A felhasználónév és jelszó a távoli hozzáféréshez csak a felhasználónév és a Hadoop-fürt létrehozásakor használt jelszó.</span><span class="sxs-lookup"><span data-stu-id="b51c1-144">The user name and password for the remote access are not the user name and password that you use when you created the Hadoop cluster.</span></span> <span data-ttu-id="b51c1-145">Ez a hitelesítő adatok külön készletét.</span><span class="sxs-lookup"><span data-stu-id="b51c1-145">This is a separate set of credentials.</span></span> <span data-ttu-id="b51c1-146">Is a távelérés lejárati dátuma nem lehet az aktuális 7 napon belül.</span><span class="sxs-lookup"><span data-stu-id="b51c1-146">Also, the expiration date of the remote access has to be within 7 days from the current date.</span></span>
> 
> 

<span data-ttu-id="b51c1-147">Távelérés engedélyezése után kattintson **CONNECT** az átjárócsomópont be távolról a lap alján.</span><span class="sxs-lookup"><span data-stu-id="b51c1-147">After remote access is enabled, click **CONNECT** at the bottom of the page to remote into the head node.</span></span> <span data-ttu-id="b51c1-148">Jelentkezzen be a Hadoop-fürt átjárócsomópontjához a távoli hozzáférés a felhasználó korábban megadott hitelesítő adatok megadásával.</span><span class="sxs-lookup"><span data-stu-id="b51c1-148">You log on to the head node of the Hadoop cluster by entering the credentials for the remote access user that you specified earlier.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

<span data-ttu-id="b51c1-150">A következő lépéseket a speciális elemzési során leképezett a [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , és tartalmazhatnak lépéseket, adatokat áthelyezi HDInsight, majd feldolgozni, és példa, hogy az adatokat az Azure Machine Learning tanulva előkészítésekor.</span><span class="sxs-lookup"><span data-stu-id="b51c1-150">The next steps in the advanced analytics process are mapped in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, then process and sample it there in preparation for learning from the data with Azure Machine Learning.</span></span>

<span data-ttu-id="b51c1-151">Lásd: [hogyan elküldeni a Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit) útmutatást, amely a felhasználói függvény (UDF), amely a fürt tárolt Hive rekordok feldolgozásához használt a fürt átjárócsomópontjához Anaconda szerepel a Python-modulok eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b51c1-151">See [How to submit Hive queries](machine-learning-data-science-move-hive-tables.md#submit) for instructions on how to access the Python modules that are included in Anaconda from the head node of the cluster in user-defined functions (UDFs) that are used to process Hive records stored in the cluster.</span></span>

