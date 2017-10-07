---
title: "aaaCustomize Hadoop-fürtök a csapat az tudományos folyamata hello |} Microsoft Docs"
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
ms.openlocfilehash: e192542dd39f71bccbb5163382b4050d0f12ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="customize-azure-hdinsight-hadoop-clusters-for-hello-team-data-science-process"></a><span data-ttu-id="6eea7-103">Hello csapat az tudományos folyamata az Azure HDInsight Hadoop-fürtök testreszabása</span><span class="sxs-lookup"><span data-stu-id="6eea7-103">Customize Azure HDInsight Hadoop clusters for hello Team Data Science Process</span></span>
<span data-ttu-id="6eea7-104">Ez a cikk ismerteti hogyan toocustomize egy HDInsight Hadoop cluster (Python 2.7) 64 bites Anaconda telepítésével minden egyes csomóponton hello fürt létesítése HDInsight szolgáltatásként.</span><span class="sxs-lookup"><span data-stu-id="6eea7-104">This article describes how toocustomize an HDInsight Hadoop cluster by installing 64-bit Anaconda (Python 2.7) on each node when hello cluster is provisioned as an HDInsight service.</span></span> <span data-ttu-id="6eea7-105">Azt is bemutatja, hogyan tooaccess hello headnode toosubmit egyéni feladatok toohello fürt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-105">It also shows how tooaccess hello headnode toosubmit custom jobs toohello cluster.</span></span> <span data-ttu-id="6eea7-106">Kényelmesen használható felhasználói meghatározott funkciókat (UDF), amelyek tervezték tooprocess Hive rekordok hello fürtben, ez a beállítás lehetővé teszi számos népszerű Python modult, Anaconda található.</span><span class="sxs-lookup"><span data-stu-id="6eea7-106">This customization makes many popular Python modules, that are included in Anaconda, conveniently available for use in user defined functions (UDFs) that are designed tooprocess Hive records in hello cluster.</span></span> <span data-ttu-id="6eea7-107">Ebben a forgatókönyvben használt hello eljárásokat, lásd: [hogyan toosubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit).</span><span class="sxs-lookup"><span data-stu-id="6eea7-107">For instructions on hello procedures used in this scenario, see [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit).</span></span>

<span data-ttu-id="6eea7-108">hello következő menü hivatkozik, amely ismerteti, hogyan tooset hello be különböző adatok tudományos környezetekben által használt hello tootopics [Team adatok tudományos folyamat (TDSP)](data-science-process-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6eea7-108">hello following menu links tootopics that describe how tooset up hello various data science environments used by hello [Team Data Science Process (TDSP)](data-science-process-overview.md).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

## <span data-ttu-id="6eea7-109"><a name="customize"></a>Az Azure HDInsight Hadoop-fürt testreszabása</span><span class="sxs-lookup"><span data-stu-id="6eea7-109"><a name="customize"></a>Customize Azure HDInsight Hadoop Cluster</span></span>
<span data-ttu-id="6eea7-110">toocreate testreszabott HDInsight Hadoop-fürt első lépésként túl bejelentkezés[**a klasszikus Azure portálon**](https://manage.windowsazure.com/), kattintson a **új** : hello bal alsó sarokban, és válassza a DATA SERVICES HDINSIGHT -> -> **egyéni létrehozás** mentése hello toobring **a fürt részleteinek** ablak.</span><span class="sxs-lookup"><span data-stu-id="6eea7-110">toocreate a customized HDInsight Hadoop cluster, start by logging on too[**Azure classic portal**](https://manage.windowsazure.com/), click **New** at hello left bottom corner, and then select DATA SERVICES -> HDINSIGHT -> **CUSTOM CREATE** toobring up hello **Cluster Details** window.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="6eea7-112">Adjon meg az 1-konfiguráció lapon létre hello fürt toobe hello nevét, és fogadja el a hello tartozó alapértelmezett értékeket más mezők.</span><span class="sxs-lookup"><span data-stu-id="6eea7-112">Input hello name of hello cluster toobe created on configuration page 1, and accept default values for hello other fields.</span></span> <span data-ttu-id="6eea7-113">Kattintson a hello nyíl toogo toohello következő konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="6eea7-113">Click hello arrow toogo toohello next configuration page.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img1.png)

<span data-ttu-id="6eea7-115">A konfiguráció lapon 2 bemeneti hello száma **ADATCSOMÓPONTOKAT**, jelölje be hello **régió/virtuális hálózati**, és válassza ki a hello hello méretű **ÁTJÁRÓCSOMÓPONT** és hello **ADATCSOMÓPONTON**.</span><span class="sxs-lookup"><span data-stu-id="6eea7-115">On configuration page 2, input hello number of **DATA NODES**, select hello **REGION/VIRTUAL NETWORK**, and select hello sizes of hello **HEAD NODE** and hello **DATA NODE**.</span></span> <span data-ttu-id="6eea7-116">Kattintson a hello nyíl toogo toohello következő konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="6eea7-116">Click hello arrow toogo toohello next configuration page.</span></span>

> [!NOTE]
> <span data-ttu-id="6eea7-117">Hello **régió/virtuális hálózati** hello ugyanaz, mint hello terület lesz használatos a HDInsight Hadoop-fürt hello toobe hello tárfiók toobe rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6eea7-117">hello **REGION/VIRTUAL NETWORK** has toobe hello same as hello region of hello storage account that is going toobe used for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="6eea7-118">Ellenkező esetben hello negyedik konfigurálása lapon hello tárfiók nem jelennek meg hello legördülő listája **fióknév**.</span><span class="sxs-lookup"><span data-stu-id="6eea7-118">Otherwise, in hello fourth configuration page, hello storage account will not appear on hello dropdown list of **ACCOUNT NAME**.</span></span>
> 
> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img3.png)

<span data-ttu-id="6eea7-120">A konfiguráció lapon 3 adja meg egy felhasználónevet és jelszót hello HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-120">On configuration page 3, provide a user name and password for hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="6eea7-121">**Ne** válassza hello *Enter hello Hive/Oozie Metaadattárhoz*.</span><span class="sxs-lookup"><span data-stu-id="6eea7-121">**Do not** select hello *Enter hello Hive/Oozie Metastore*.</span></span> <span data-ttu-id="6eea7-122">Kattintson a hello nyíl toogo toohello következő konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="6eea7-122">Then, click hello arrow toogo toohello next configuration page.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img4.png)

<span data-ttu-id="6eea7-124">Konfiguráció lapon adja meg, 4 hello tárfióknév, hello alapértelmezett tároló hello HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-124">On configuration page 4, specify hello storage account name, hello default container of hello HDInsight Hadoop cluster.</span></span> <span data-ttu-id="6eea7-125">Ha *hozzon létre alapértelmezett tároló* a hello **alapértelmezett tároló** legördülő listából, a tárolóhoz hello azonos nevet hello fürt jön létre.</span><span class="sxs-lookup"><span data-stu-id="6eea7-125">If you select *Create default container* in hello **DEFAULT CONTAINER** dropdown list, a container with hello same name as hello cluster will be created.</span></span> <span data-ttu-id="6eea7-126">Kattintson a hello nyíl toogo toohello utolsó konfiguráció lapon.</span><span class="sxs-lookup"><span data-stu-id="6eea7-126">Click hello arrow toogo toohello last configuration page.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/customize-cluster-img5.png)

<span data-ttu-id="6eea7-128">A végső hello **Parancsfájlműveletek** konfigurációs lapon kattintson a **parancsfájlművelet hozzáadása** gombra, és beírja a következő értékek hello hello szövegmezők.</span><span class="sxs-lookup"><span data-stu-id="6eea7-128">On hello final **Script Actions** configuration page, click **add script action** button, and fill hello text fields with hello following values.</span></span>

* <span data-ttu-id="6eea7-129">**NÉV** -bármilyen karakterlánc hello neveként a parancsfájl művelet</span><span class="sxs-lookup"><span data-stu-id="6eea7-129">**NAME** - any string as hello name of this script action</span></span>
* <span data-ttu-id="6eea7-130">**A CSOMÓPONTTÍPUS** – Itt adhatja meg **minden csomópont**</span><span class="sxs-lookup"><span data-stu-id="6eea7-130">**NODE TYPE** - select **All nodes**</span></span>
* <span data-ttu-id="6eea7-131">**PARANCSFÁJL URI azonosítója** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span><span class="sxs-lookup"><span data-stu-id="6eea7-131">**SCRIPT URI** - *http://getgoing.blob.core.windows.net/publicscripts/Azure_HDI_Setup_Windows.ps1*</span></span> 
  * <span data-ttu-id="6eea7-132">*publicscripts* hello tárfiókban lévő nyilvános tárolója</span><span class="sxs-lookup"><span data-stu-id="6eea7-132">*publicscripts* is a public container in hello storage account</span></span> 
  * <span data-ttu-id="6eea7-133">*getgoing* tooshare PowerShell parancsfájl fájlok toofacilitate felhasználók munkahelyi használjuk az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="6eea7-133">*getgoing* we use tooshare PowerShell script files toofacilitate users' work in Azure</span></span>
* <span data-ttu-id="6eea7-134">**PARAMÉTEREK** -(hagyja üresen)</span><span class="sxs-lookup"><span data-stu-id="6eea7-134">**PARAMETERS** - (leave blank)</span></span>

<span data-ttu-id="6eea7-135">Végezetül kattintson hello pipa toostart hello létrehozása testre szabott hello HDInsight Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-135">Finally, click hello check mark toostart hello creation of hello customized HDInsight Hadoop cluster.</span></span> 

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/script-actions.png)

## <span data-ttu-id="6eea7-137"><a name="headnode"></a>Hozzáférés hello Head csomópont a Hadoop-fürt</span><span class="sxs-lookup"><span data-stu-id="6eea7-137"><a name="headnode"></a> Access hello Head Node of Hadoop Cluster</span></span>
<span data-ttu-id="6eea7-138">Távelérés toohello Hadoop-fürt az Azure-ban engedélyeznie kell, csak hello hello Hadoop-fürt átjárócsomópontjához RDP érheti el.</span><span class="sxs-lookup"><span data-stu-id="6eea7-138">You must enable remote access toohello Hadoop cluster in Azure before you can access hello head node of hello Hadoop cluster through RDP.</span></span> 

1. <span data-ttu-id="6eea7-139">Jelentkezzen be toohello [ **a klasszikus Azure portálon**](https://manage.windowsazure.com/), jelölje be **HDInsight** hello bal oldalon válassza ki a Hadoop-fürt a hello fürtök, hello kattintson  **KONFIGURÁCIÓS** fülre, majd hello **távoli engedélyezése** alján hello hello ikonra.</span><span class="sxs-lookup"><span data-stu-id="6eea7-139">Log in toohello [**Azure classic portal**](https://manage.windowsazure.com/), select **HDInsight** on hello left, select your Hadoop cluster from hello list of clusters, click hello **CONFIGURATION** tab, and then click hello **ENABLE REMOTE** icon at hello bottom of hello page.</span></span>
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-1.png)
2. <span data-ttu-id="6eea7-141">A hello **konfigurálása a távoli asztal** ablakban hello felhasználónév és jelszó mezők megadása, valamint távelérés hello lejárati dátuma.</span><span class="sxs-lookup"><span data-stu-id="6eea7-141">In hello **Configure Remote Desktop** window, enter hello USER NAME and PASSWORD fields, and select hello expiration date for remote access.</span></span> <span data-ttu-id="6eea7-142">Kattintson a hello pipa tooenable hello távelérési toohello átjárócsomópontjához hello Hadoop-fürt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-142">Then click hello check mark tooenable hello remote access toohello head node of hello Hadoop cluster.</span></span>
   
    ![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-2.png)

> [!NOTE]
> <span data-ttu-id="6eea7-144">hello felhasználónevet és jelszót hello táveléréshez nincsenek hello felhasználónevét és jelszavát, hello Hadoop-fürt létrehozásakor használt.</span><span class="sxs-lookup"><span data-stu-id="6eea7-144">hello user name and password for hello remote access are not hello user name and password that you use when you created hello Hadoop cluster.</span></span> <span data-ttu-id="6eea7-145">Ez a hitelesítő adatok külön készletét.</span><span class="sxs-lookup"><span data-stu-id="6eea7-145">This is a separate set of credentials.</span></span> <span data-ttu-id="6eea7-146">Hello lejárati dátuma hello távelérési rendszer is toobe aktuális dátum hello számított 7 napon belül.</span><span class="sxs-lookup"><span data-stu-id="6eea7-146">Also, hello expiration date of hello remote access has toobe within 7 days from hello current date.</span></span>
> 
> 

<span data-ttu-id="6eea7-147">Távelérés engedélyezése után kattintson **CONNECT** hello lap tooremote hello központi csomópontjába hello alján.</span><span class="sxs-lookup"><span data-stu-id="6eea7-147">After remote access is enabled, click **CONNECT** at hello bottom of hello page tooremote into hello head node.</span></span> <span data-ttu-id="6eea7-148">Hello hitelesítő adatokat adjon meg a korábban megadott hello távelérési felhasználói bejelentkezés toohello hello Hadoop-fürt átjárócsomópontjához.</span><span class="sxs-lookup"><span data-stu-id="6eea7-148">You log on toohello head node of hello Hadoop cluster by entering hello credentials for hello remote access user that you specified earlier.</span></span>

![Munkaterület létrehozása](./media/machine-learning-data-science-customize-hadoop-cluster/enable-remote-access-3.png)

<span data-ttu-id="6eea7-150">hello hello advanced analytics folyamat a következő lépések vannak leképezve a hello [Team adatok tudományos folyamat (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , és tartalmazhatnak lépéseket, amely adatok áthelyezi HDInsight, majd feldolgozást és minták ott előkészítésekor hello adatok alapján az Azure Machine Learning segítségével.</span><span class="sxs-lookup"><span data-stu-id="6eea7-150">hello next steps in hello advanced analytics process are mapped in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) and may include steps that move data into HDInsight, then process and sample it there in preparation for learning from hello data with Azure Machine Learning.</span></span>

<span data-ttu-id="6eea7-151">Lásd: [hogyan toosubmit Hive-lekérdezések](machine-learning-data-science-move-hive-tables.md#submit) kapcsolatban, hogyan tooaccess hello hello felhasználói függvény (UDF), amelyek használt tooprocess hello fürt átjárócsomópontjához Anaconda szerepel Python-modulok Hive tárolt rekordok hello fürtben.</span><span class="sxs-lookup"><span data-stu-id="6eea7-151">See [How toosubmit Hive queries](machine-learning-data-science-move-hive-tables.md#submit) for instructions on how tooaccess hello Python modules that are included in Anaconda from hello head node of hello cluster in user-defined functions (UDFs) that are used tooprocess Hive records stored in hello cluster.</span></span>

