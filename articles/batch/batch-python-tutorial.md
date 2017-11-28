---
title: "aaaTutorial - használata hello Azure Batch Python SDK |} Microsoft Docs"
description: "Ismerje meg az Azure Batch hello alapvető fogalmait, és egy egyszerű megoldás pythonos környezetekben."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: 42cae157-d43d-47f8-88f5-486ccfd334f4
ms.service: batch
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c4d5152aeef31848c50a7f2aae5e7a7e0e1e9535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-batch-sdk-for-python"></a><span data-ttu-id="90219-103">Python hello kötegelt SDK használatába</span><span class="sxs-lookup"><span data-stu-id="90219-103">Get started with hello Batch SDK for Python</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="90219-104">.NET</span><span class="sxs-lookup"><span data-stu-id="90219-104">.NET</span></span>](batch-dotnet-get-started.md)
> * [<span data-ttu-id="90219-105">Python</span><span class="sxs-lookup"><span data-stu-id="90219-105">Python</span></span>](batch-python-tutorial.md)
> * [<span data-ttu-id="90219-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="90219-106">Node.js</span></span>](batch-nodejs-get-started.md)
>
>

<span data-ttu-id="90219-107">A hello alapvető [Azure Batch] [ azure_batch] és hello [kötegelt Python] [ py_azure_sdk] ügyfél, a kis Batch-alkalmazások pythonban írt arról lesz szó.</span><span class="sxs-lookup"><span data-stu-id="90219-107">Learn hello basics of [Azure Batch][azure_batch] and hello [Batch Python][py_azure_sdk] client as we discuss a small Batch application written in Python.</span></span> <span data-ttu-id="90219-108">Úgy tekintünk, hogyan két minta parancsfájlok használata hello Batch szolgáltatás tooprocess egy párhuzamos munkaterhelés hello felhőben lévő Linux virtuális gépeken, és azok működését a [Azure Storage](../storage/common/storage-introduction.md) fájl átmeneti és lekérése.</span><span class="sxs-lookup"><span data-stu-id="90219-108">We look at how two sample scripts use hello Batch service tooprocess a parallel workload on Linux virtual machines in hello cloud, and how they interact with [Azure Storage](../storage/common/storage-introduction.md) for file staging and retrieval.</span></span> <span data-ttu-id="90219-109">Kell egy közös kötegelt alkalmazás munkafolyamata bemutatása és szerezzen egy alapszintű hello fő összetevőit kötegelt feladatok, feladatok, készletek megértése és számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="90219-109">You'll learn a common Batch application workflow and gain a base understanding of hello major components of Batch such as jobs, tasks, pools, and compute nodes.</span></span>

<span data-ttu-id="90219-110">![Batch-megoldás munkafolyamata (alapszintű)][11]</span><span class="sxs-lookup"><span data-stu-id="90219-110">![Batch solution workflow (basic)][11]</span></span><br/>

## <a name="prerequisites"></a><span data-ttu-id="90219-111">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="90219-111">Prerequisites</span></span>
<span data-ttu-id="90219-112">Ez a cikk a Python és a Linux gyakorlati ismeretét feltételezi.</span><span class="sxs-lookup"><span data-stu-id="90219-112">This article assumes that you have a working knowledge of Python and familiarity with Linux.</span></span> <span data-ttu-id="90219-113">Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az alább megadott Azure- és hello Batch-és tárolási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="90219-113">It also assumes that you're able toosatisfy hello account creation requirements that are specified below for Azure and hello Batch and Storage services.</span></span>

### <a name="accounts"></a><span data-ttu-id="90219-114">Fiókok</span><span class="sxs-lookup"><span data-stu-id="90219-114">Accounts</span></span>
* <span data-ttu-id="90219-115">**Azure-fiók**: Ha még nincs Azure-előfizetése, [hozzon létre egy ingyenes Azure-fiókot][azure_free_account].</span><span class="sxs-lookup"><span data-stu-id="90219-115">**Azure account**: If you don't already have an Azure subscription, [create a free Azure account][azure_free_account].</span></span>
* <span data-ttu-id="90219-116">**Batch-fiók**: Ha már rendelkezik Azure-előfizetéssel, [hozzon létre egy Azure Batch-fiókot](batch-account-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="90219-116">**Batch account**: Once you have an Azure subscription, [create an Azure Batch account](batch-account-create-portal.md).</span></span>
* <span data-ttu-id="90219-117">**Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) cikk [Tárfiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account) szakaszát.</span><span class="sxs-lookup"><span data-stu-id="90219-117">**Storage account**: See [Create a storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account) in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

### <a name="code-sample"></a><span data-ttu-id="90219-118">Kódminta</span><span class="sxs-lookup"><span data-stu-id="90219-118">Code sample</span></span>
<span data-ttu-id="90219-119">hello Python oktatóanyag [kódminta] [ github_article_samples] hello sok kötegelt mintakódok hello található egyik [azure-köteg-minták] [ github_samples] tárházából GitHub.</span><span class="sxs-lookup"><span data-stu-id="90219-119">hello Python tutorial [code sample][github_article_samples] is one of hello many Batch code samples found in hello [azure-batch-samples][github_samples] repository on GitHub.</span></span> <span data-ttu-id="90219-120">Összes hello minta kattintva letöltheti **Klónozás vagy letöltési > töltse le a ZIP-** hello tárház kezdőlapján, vagy kattintson a hello [azure-köteg-minták-master.zip] [ github_samples_zip]közvetlen letöltési hivatkozását.</span><span class="sxs-lookup"><span data-stu-id="90219-120">You can download all hello samples by clicking **Clone or download > Download ZIP** on hello repository home page, or by clicking hello [azure-batch-samples-master.zip][github_samples_zip] direct download link.</span></span> <span data-ttu-id="90219-121">Miután kibontotta már hello hello ZIP-fájl tartalmát, ebben az oktatóanyagban hello két parancsfájlok hello találhatók `article_samples` könyvtár:</span><span class="sxs-lookup"><span data-stu-id="90219-121">Once you've extracted hello contents of hello ZIP file, hello two scripts for this tutorial are found in hello `article_samples` directory:</span></span>

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a><span data-ttu-id="90219-122">Python-környezet</span><span class="sxs-lookup"><span data-stu-id="90219-122">Python environment</span></span>
<span data-ttu-id="90219-123">toorun hello *python_tutorial_client.py* kell a helyi munkaállomáson mintaparancsfájl egy **Python parancsértelmező** verziójával kompatibilis **2.7** vagy **3.3 +**.</span><span class="sxs-lookup"><span data-stu-id="90219-123">toorun hello *python_tutorial_client.py* sample script on your local workstation, you need a **Python interpreter** compatible with version **2.7** or **3.3+**.</span></span> <span data-ttu-id="90219-124">a Linux és a Windows hello parancsfájl tesztelték.</span><span class="sxs-lookup"><span data-stu-id="90219-124">hello script has been tested on both Linux and Windows.</span></span>

### <a name="cryptography-dependencies"></a><span data-ttu-id="90219-125">titkosítási függőségek</span><span class="sxs-lookup"><span data-stu-id="90219-125">cryptography dependencies</span></span>
<span data-ttu-id="90219-126">Telepítenie kell a hello hello függőségeinek [titkosítás] [ crypto] könyvtár hello által igényelt `azure-batch` és `azure-storage` Python-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="90219-126">You must install hello dependencies for hello [cryptography][crypto] library, required by hello `azure-batch` and `azure-storage` Python packages.</span></span> <span data-ttu-id="90219-127">Hajtsa végre a következő műveletek a platformhoz megfelelő hello, vagy tekintse meg a toohello [titkosítás telepítési] [ crypto_install] részleteiben talál további információt:</span><span class="sxs-lookup"><span data-stu-id="90219-127">Perform one of hello following operations appropriate for your platform, or refer toohello [cryptography installation][crypto_install] details for more information:</span></span>

* <span data-ttu-id="90219-128">Ubuntu</span><span class="sxs-lookup"><span data-stu-id="90219-128">Ubuntu</span></span>

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* <span data-ttu-id="90219-129">CentOS</span><span class="sxs-lookup"><span data-stu-id="90219-129">CentOS</span></span>

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* <span data-ttu-id="90219-130">SLES/OpenSUSE</span><span class="sxs-lookup"><span data-stu-id="90219-130">SLES/OpenSUSE</span></span>

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* <span data-ttu-id="90219-131">Windows</span><span class="sxs-lookup"><span data-stu-id="90219-131">Windows</span></span>

    `pip install cryptography`

> [!NOTE]
> <span data-ttu-id="90219-132">Ha telepíti a Python 3.3 + Linux, használja a hello python3 alakokat hello Python függőségek.</span><span class="sxs-lookup"><span data-stu-id="90219-132">If installing for Python 3.3+ on Linux, use hello python3 equivalents for hello Python dependencies.</span></span> <span data-ttu-id="90219-133">Például az Ubuntu rendszeren: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span><span class="sxs-lookup"><span data-stu-id="90219-133">For example, on Ubuntu: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`</span></span>
>
>

### <a name="azure-packages"></a><span data-ttu-id="90219-134">Azure-csomagok</span><span class="sxs-lookup"><span data-stu-id="90219-134">Azure packages</span></span>
<span data-ttu-id="90219-135">Következő lépésként telepítse a hello **Azure Batch** és **Azure Storage** Python-csomagokat.</span><span class="sxs-lookup"><span data-stu-id="90219-135">Next, install hello **Azure Batch** and **Azure Storage** Python packages.</span></span> <span data-ttu-id="90219-136">Mindkét csomagot segítségével telepítheti **pip** és hello *requirements.txt* itt található:</span><span class="sxs-lookup"><span data-stu-id="90219-136">You can install both packages by using **pip** and hello *requirements.txt* found here:</span></span>

`/azure-batch-samples/Python/Batch/requirements.txt`

<span data-ttu-id="90219-137">A probléma következő **pip** tooinstall hello kötegelt és a tárolási csomagok parancssori:</span><span class="sxs-lookup"><span data-stu-id="90219-137">Issue following **pip** command tooinstall hello Batch and Storage packages:</span></span>

`pip install -r requirements.txt`

<span data-ttu-id="90219-138">Vagy telepítse hello [azure-köteg] [ pypi_batch] és [azure-tároló] [ pypi_storage] Python-csomagok manuálisan:</span><span class="sxs-lookup"><span data-stu-id="90219-138">Or, you can install hello [azure-batch][pypi_batch] and [azure-storage][pypi_storage] Python packages manually:</span></span>

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> <span data-ttu-id="90219-139">Ha olyan nem rendszerjogosultságú fiókot használ, szükség lehet tooprefix a utasítással `sudo`.</span><span class="sxs-lookup"><span data-stu-id="90219-139">If you are using an unprivileged account, you may need tooprefix your commands with `sudo`.</span></span> <span data-ttu-id="90219-140">Például: `sudo pip install -r requirements.txt`.</span><span class="sxs-lookup"><span data-stu-id="90219-140">For example, `sudo pip install -r requirements.txt`.</span></span> <span data-ttu-id="90219-141">Ha további információt szeretne kapni a Python-csomagok telepítésével kapcsolatban, olvassa el a [csomagok telepítését][pypi_install] ismertető cikket a python.org webhelyen.</span><span class="sxs-lookup"><span data-stu-id="90219-141">For more information on installing Python packages, see [Installing Packages][pypi_install] on python.org.</span></span>
>
>

## <a name="batch-python-tutorial-code-sample"></a><span data-ttu-id="90219-142">Batch Python-oktatóprogram kódmintája</span><span class="sxs-lookup"><span data-stu-id="90219-142">Batch Python tutorial code sample</span></span>
<span data-ttu-id="90219-143">hello kötegelt Python oktatóanyag példakód két Python-parancsfájl és néhány adatfájlok áll.</span><span class="sxs-lookup"><span data-stu-id="90219-143">hello Batch Python tutorial code sample consists of two Python scripts and a few data files.</span></span>

* <span data-ttu-id="90219-144">**python_tutorial_client.PY**: egy párhuzamos munkaterhelés, a számítási csomópontok (virtuális gépek) hello kötegelt és tárolási szolgáltatások tooexecute kommunikál.</span><span class="sxs-lookup"><span data-stu-id="90219-144">**python_tutorial_client.py**: Interacts with hello Batch and Storage services tooexecute a parallel workload on compute nodes (virtual machines).</span></span> <span data-ttu-id="90219-145">Hello *python_tutorial_client.py* parancsprogram lefut a helyi munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="90219-145">hello *python_tutorial_client.py* script runs on your local workstation.</span></span>
* <span data-ttu-id="90219-146">**python_tutorial_task.PY**: hello futó parancsfájl számítási csomópontok az Azure tooperform hello tényleges munkát.</span><span class="sxs-lookup"><span data-stu-id="90219-146">**python_tutorial_task.py**: hello script that runs on compute nodes in Azure tooperform hello actual work.</span></span> <span data-ttu-id="90219-147">Hello mintában *python_tutorial_task.py* elemez hello (hello bemeneti fájl) Azure Storage-ból letöltött fájlok szöveget.</span><span class="sxs-lookup"><span data-stu-id="90219-147">In hello sample, *python_tutorial_task.py* parses hello text in a file downloaded from Azure Storage (hello input file).</span></span> <span data-ttu-id="90219-148">Ezután azt szöveges fájlt hoz létre (hello kimeneti fájl), amely hello a három legfontosabb szereplő szavakkal hello bemeneti fájl listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="90219-148">Then it produces a text file (hello output file) that contains a list of hello top three words that appear in hello input file.</span></span> <span data-ttu-id="90219-149">Miután létrehoz hello kimeneti fájlt, *python_tutorial_task.py* feltöltések hello fájl tooAzure tároló.</span><span class="sxs-lookup"><span data-stu-id="90219-149">After it creates hello output file, *python_tutorial_task.py* uploads hello file tooAzure Storage.</span></span> <span data-ttu-id="90219-150">Ez lehetővé teszi a letöltési toohello ügyféloldali parancsprogram futtatása a munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="90219-150">This makes it available for download toohello client script running on your workstation.</span></span> <span data-ttu-id="90219-151">Hello *python_tutorial_task.py* parancsfájl futtatása párhuzamosan több számítási csomópontjain a Batch szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="90219-151">hello *python_tutorial_task.py* script runs in parallel on multiple compute nodes in hello Batch service.</span></span>
* <span data-ttu-id="90219-152">**./Data/taskdata\*.txt**: ezek három szövegfájlok hello bemeneti adjon meg, a hello feladatok hello futó számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="90219-152">**./data/taskdata\*.txt**: These three text files provide hello input for hello tasks that run on hello compute nodes.</span></span>

<span data-ttu-id="90219-153">a következő diagram hello hello elsődleges műveletek hello ügyfél és a feladat parancsfájlok által végrehajtott mutatja be.</span><span class="sxs-lookup"><span data-stu-id="90219-153">hello following diagram illustrates hello primary operations that are performed by hello client and task scripts.</span></span> <span data-ttu-id="90219-154">Ez az alapvető munkafolyamat számos, a Batch használatával létrehozott számítási megoldásra jellemző.</span><span class="sxs-lookup"><span data-stu-id="90219-154">This basic workflow is typical of many compute solutions that are created with Batch.</span></span> <span data-ttu-id="90219-155">Minden elérhető, a Batch szolgáltatás hello szolgáltatást nem azt mutatják, amíg a szinte minden kötegelt az eset tartalmazza a munkafolyamat részeit.</span><span class="sxs-lookup"><span data-stu-id="90219-155">While it does not demonstrate every feature available in hello Batch service, nearly every Batch scenario includes portions of this workflow.</span></span>

<span data-ttu-id="90219-156">![Példa Batch-munkafolyamat][8]</span><span class="sxs-lookup"><span data-stu-id="90219-156">![Batch example workflow][8]</span></span><br/>

[<span data-ttu-id="90219-157">**1. lépés**</span><span class="sxs-lookup"><span data-stu-id="90219-157">**Step 1.**</span></span>](#step-1-create-storage-containers) <span data-ttu-id="90219-158">**Tárolók** létrehozása az Azure Blob Storage-ban.</span><span class="sxs-lookup"><span data-stu-id="90219-158">Create **containers** in Azure Blob Storage.</span></span><br/><span data-ttu-id="90219-159">
[**2. lépés**](#step-2-upload-task-script-and-data-files)</span><span class="sxs-lookup"><span data-stu-id="90219-159">
[**Step 2.**](#step-2-upload-task-script-and-data-files)</span></span> <span data-ttu-id="90219-160">A feladat parancsfájlt és a bemeneti fájlok toocontainers feltöltése.</span><span class="sxs-lookup"><span data-stu-id="90219-160">Upload task script and input files toocontainers.</span></span><br/><span data-ttu-id="90219-161">
[**3. lépés**](#step-3-create-batch-pool)</span><span class="sxs-lookup"><span data-stu-id="90219-161">
[**Step 3.**](#step-3-create-batch-pool)</span></span> <span data-ttu-id="90219-162">Batch-**készlet** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90219-162">Create a Batch **pool**.</span></span><br/>
  <span data-ttu-id="90219-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span><span class="sxs-lookup"><span data-stu-id="90219-163">&nbsp;&nbsp;&nbsp;&nbsp;**3a.**</span></span> <span data-ttu-id="90219-164">készlet hello **StartTask** letöltések hello feladat parancsfájl (python_tutorial_task.py) toonodes, mivel azok csatlakoztatását hello készlet.</span><span class="sxs-lookup"><span data-stu-id="90219-164">hello pool **StartTask** downloads hello task script (python_tutorial_task.py) toonodes as they join hello pool.</span></span><br/><span data-ttu-id="90219-165">
[**4. lépés**](#step-4-create-batch-job)</span><span class="sxs-lookup"><span data-stu-id="90219-165">
[**Step 4.**](#step-4-create-batch-job)</span></span> <span data-ttu-id="90219-166">Batch-**feladat** létrehozása.</span><span class="sxs-lookup"><span data-stu-id="90219-166">Create a Batch **job**.</span></span><br/><span data-ttu-id="90219-167">
[**5. lépés**](#step-5-add-tasks-to-job)</span><span class="sxs-lookup"><span data-stu-id="90219-167">
[**Step 5.**](#step-5-add-tasks-to-job)</span></span> <span data-ttu-id="90219-168">Adja hozzá **feladatok** toohello feladat.</span><span class="sxs-lookup"><span data-stu-id="90219-168">Add **tasks** toohello job.</span></span><br/>
  <span data-ttu-id="90219-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span><span class="sxs-lookup"><span data-stu-id="90219-169">&nbsp;&nbsp;&nbsp;&nbsp;**5a.**</span></span> <span data-ttu-id="90219-170">hello feladatok ütemezett tooexecute csomópontján.</span><span class="sxs-lookup"><span data-stu-id="90219-170">hello tasks are scheduled tooexecute on nodes.</span></span><br/>
    <span data-ttu-id="90219-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span><span class="sxs-lookup"><span data-stu-id="90219-171">&nbsp;&nbsp;&nbsp;&nbsp;**5b.**</span></span> <span data-ttu-id="90219-172">Mindegyik tevékenység letölti a bemeneti adatait az Azure Storage-ból, majd elkezdi a végrehajtást.</span><span class="sxs-lookup"><span data-stu-id="90219-172">Each task downloads its input data from Azure Storage, then begins execution.</span></span><br/><span data-ttu-id="90219-173">
[**6. lépés**](#step-6-monitor-tasks)</span><span class="sxs-lookup"><span data-stu-id="90219-173">
[**Step 6.**](#step-6-monitor-tasks)</span></span> <span data-ttu-id="90219-174">Tevékenységek figyelése.</span><span class="sxs-lookup"><span data-stu-id="90219-174">Monitor tasks.</span></span><br/>
  <span data-ttu-id="90219-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span><span class="sxs-lookup"><span data-stu-id="90219-175">&nbsp;&nbsp;&nbsp;&nbsp;**6a.**</span></span> <span data-ttu-id="90219-176">Feladatok befejezésekor, ezek a kimeneti adatok tooAzure tárolási feltöltése.</span><span class="sxs-lookup"><span data-stu-id="90219-176">As tasks are completed, they upload their output data tooAzure Storage.</span></span><br/><span data-ttu-id="90219-177">
[**7. lépés**](#step-7-download-task-output)</span><span class="sxs-lookup"><span data-stu-id="90219-177">
[**Step 7.**](#step-7-download-task-output)</span></span> <span data-ttu-id="90219-178">Tevékenység kimenetének letöltése a Storage-ból.</span><span class="sxs-lookup"><span data-stu-id="90219-178">Download task output from Storage.</span></span>

<span data-ttu-id="90219-179">Ahogy említettük, nem minden Batch-megoldás végzi el pontosan ezeket a lépéseket, és sok más lépést is tartalmazhatnak, de ez a minta bemutatja a Batch-megoldások általános folyamatait.</span><span class="sxs-lookup"><span data-stu-id="90219-179">As mentioned, not every Batch solution performs these exact steps, and may include many more, but this sample demonstrates common processes found in a Batch solution.</span></span>

## <a name="prepare-client-script"></a><span data-ttu-id="90219-180">Az ügyfélparancsfájl előkészítése</span><span class="sxs-lookup"><span data-stu-id="90219-180">Prepare client script</span></span>
<span data-ttu-id="90219-181">Hello minta futtatása előtt hozzáadása a kötegelt és a tárolási fiók hitelesítő adataival túl*python_tutorial_client.py*.</span><span class="sxs-lookup"><span data-stu-id="90219-181">Before you run hello sample, add your Batch and Storage account credentials too*python_tutorial_client.py*.</span></span> <span data-ttu-id="90219-182">Ha még nem tette meg, nyissa meg a következő sorokat a hitelesítő adataival kedvenc szerkesztő és a frissítés hello hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="90219-182">If you have not done so already, open hello file in your favorite editor and update hello following lines with your credentials.</span></span>

```python
# Update hello Batch and Storage account credential strings below with hello values
# unique tooyour accounts. These are used when constructing connection strings
# for hello Batch and Storage client objects.

# Batch account credentials
BATCH_ACCOUNT_NAME = ""
BATCH_ACCOUNT_KEY = ""
BATCH_ACCOUNT_URL = ""

# Storage account credentials
STORAGE_ACCOUNT_NAME = ""
STORAGE_ACCOUNT_KEY = ""
```

<span data-ttu-id="90219-183">A kötegelt és a tárolási fiók hitelesítő adataival belül minden szolgáltatás hello fiók panelen található hello [Azure-portálon][azure_portal]:</span><span class="sxs-lookup"><span data-stu-id="90219-183">You can find your Batch and Storage account credentials within hello account blade of each service in hello [Azure portal][azure_portal]:</span></span>

<span data-ttu-id="90219-184">![A Batch-hitelesítő adatok hello portálon][9]
![hello portal tároló hitelesítő adatait][10]</span><span class="sxs-lookup"><span data-stu-id="90219-184">![Batch credentials in hello portal][9]
![Storage credentials in hello portal][10]</span></span><br/>

<span data-ttu-id="90219-185">A következő részekben hello, azt elemzése által használt hello lépéseket hello parancsfájlok, a munkaterhelés, a Batch szolgáltatás hello tooprocess.</span><span class="sxs-lookup"><span data-stu-id="90219-185">In hello following sections, we analyze hello steps used by hello scripts tooprocess a workload in hello Batch service.</span></span> <span data-ttu-id="90219-186">Javasoljuk, toorefer rendszeresen toohello parancsfájlok közben a szerkesztőben hello cikk többi hello útján a módon működnek.</span><span class="sxs-lookup"><span data-stu-id="90219-186">We encourage you toorefer regularly toohello scripts in your editor while you work your way through hello rest of hello article.</span></span>

<span data-ttu-id="90219-187">Keresse meg a következő sort a toohello **python_tutorial_client.py** toostart az 1. lépés:</span><span class="sxs-lookup"><span data-stu-id="90219-187">Navigate toohello following line in **python_tutorial_client.py** toostart with Step 1:</span></span>

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a><span data-ttu-id="90219-188">1. lépés: Storage-tárolók létrehozása</span><span class="sxs-lookup"><span data-stu-id="90219-188">Step 1: Create Storage containers</span></span>
<span data-ttu-id="90219-189">![Tárolók létrehozása az Azure Storage-ban][1]
</span><span class="sxs-lookup"><span data-stu-id="90219-189">![Create containers in Azure Storage][1]
</span></span><br/>

<span data-ttu-id="90219-190">A Batch beépített támogatást tartalmaz az Azure Storage használatához.</span><span class="sxs-lookup"><span data-stu-id="90219-190">Batch includes built-in support for interacting with Azure Storage.</span></span> <span data-ttu-id="90219-191">A tárfiók a tárolók hello feladatok futtatása a Batch-fiók szükséges hello fájlokat ad meg.</span><span class="sxs-lookup"><span data-stu-id="90219-191">Containers in your Storage account will provide hello files needed by hello tasks that run in your Batch account.</span></span> <span data-ttu-id="90219-192">hello tárolók nagy előnye, egy hely toostore hello kimeneti adatok, amelyek hello feladatok egymással.</span><span class="sxs-lookup"><span data-stu-id="90219-192">hello containers also provide a place toostore hello output data that hello tasks produce.</span></span> <span data-ttu-id="90219-193">először thing hello hello *python_tutorial_client.py* parancsprogram hozható létre a három tároló [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span><span class="sxs-lookup"><span data-stu-id="90219-193">hello first thing hello *python_tutorial_client.py* script does is create three containers in [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):</span></span>

* <span data-ttu-id="90219-194">**alkalmazás**: Ebben a tárolóban tárolni fogja hello Python-parancsfájl futtatása hello feladatok, *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="90219-194">**application**: This container will store hello Python script run by hello tasks, *python_tutorial_task.py*.</span></span>
* <span data-ttu-id="90219-195">**bemeneti**: feladatok töltik le hello adatok fájlok tooprocess hello *bemeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="90219-195">**input**: Tasks will download hello data files tooprocess from hello *input* container.</span></span>
* <span data-ttu-id="90219-196">**kimeneti**: a bemeneti fájl feldolgozása végeztével a feladatok hello eredmények toohello feltölti *kimeneti* tároló.</span><span class="sxs-lookup"><span data-stu-id="90219-196">**output**: When tasks complete input file processing, they will upload hello results toohello *output* container.</span></span>

<span data-ttu-id="90219-197">Egy tároló rendelés toointeract a fiókot, és tárolók, hozzon létre hello használjuk [azure-tároló] [ pypi_storage] toocreate csomag egy [BlockBlobService] [ py_blockblobservice] objektum – hello "blob ügyfél."</span><span class="sxs-lookup"><span data-stu-id="90219-197">In order toointeract with a Storage account and create containers, we use hello [azure-storage][pypi_storage] package toocreate a [BlockBlobService][py_blockblobservice] object--hello "blob client."</span></span> <span data-ttu-id="90219-198">Majd létrehozhatunk három tároló hello tárfiókban hello blob ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="90219-198">We then create three containers in hello Storage account using hello blob client.</span></span>

```python
import azure.storage.blob as azureblob

# Create hello blob client, for use in obtaining references to
# blob storage containers and uploading files toocontainers.
blob_client = azureblob.BlockBlobService(
    account_name=STORAGE_ACCOUNT_NAME,
    account_key=STORAGE_ACCOUNT_KEY)

# Use hello blob client toocreate hello containers in Azure Storage if they
# don't yet exist.
APP_CONTAINER_NAME = 'application'
INPUT_CONTAINER_NAME = 'input'
OUTPUT_CONTAINER_NAME = 'output'
blob_client.create_container(APP_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(INPUT_CONTAINER_NAME, fail_on_exist=False)
blob_client.create_container(OUTPUT_CONTAINER_NAME, fail_on_exist=False)
```

<span data-ttu-id="90219-199">Hello tárolók létrehozása után, hello alkalmazás most már feltöltheti hello feladatok által használt hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="90219-199">Once hello containers have been created, hello application can now upload hello files that will be used by hello tasks.</span></span>

> [!TIP]
> <span data-ttu-id="90219-200">[Hogyan toouse Azure Blob storage-ának Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) jó áttekintést nyújt az Azure Storage tárolók és blobok használata.</span><span class="sxs-lookup"><span data-stu-id="90219-200">[How toouse Azure Blob storage from Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) provides a good overview of working with Azure Storage containers and blobs.</span></span> <span data-ttu-id="90219-201">Meg kell az olvasási lista hello tetején, az Batch használatának megkezdése.</span><span class="sxs-lookup"><span data-stu-id="90219-201">It should be near hello top of your reading list as you start working with Batch.</span></span>
>
>

## <a name="step-2-upload-task-script-and-data-files"></a><span data-ttu-id="90219-202">2. lépés: Feladatparancsfájl és adatfájlok feltöltése</span><span class="sxs-lookup"><span data-stu-id="90219-202">Step 2: Upload task script and data files</span></span>
<span data-ttu-id="90219-203">![Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers][2]
</span><span class="sxs-lookup"><span data-stu-id="90219-203">![Upload task application and input (data) files toocontainers][2]
</span></span><br/>

<span data-ttu-id="90219-204">Hello fájl feltöltése a művelet, *python_tutorial_client.py* először határozza meg a gyűjteményei **alkalmazás** és **bemeneti** elérési utak fájlt a helyi számítógépen hello léteznek.</span><span class="sxs-lookup"><span data-stu-id="90219-204">In hello file upload operation, *python_tutorial_client.py* first defines collections of **application** and **input** file paths as they exist on hello local machine.</span></span> <span data-ttu-id="90219-205">Majd ezen fájlok toohello tárolók hello előző lépésben létrehozott feltölti azt.</span><span class="sxs-lookup"><span data-stu-id="90219-205">Then it uploads these files toohello containers that you created in hello previous step.</span></span>

```python
# Paths toohello task script. This script will be executed by hello tasks that
# run on hello compute nodes.
application_file_paths = [os.path.realpath('python_tutorial_task.py')]

# hello collection of data files that are toobe processed by hello tasks.
input_file_paths = [os.path.realpath('./data/taskdata1.txt'),
                    os.path.realpath('./data/taskdata2.txt'),
                    os.path.realpath('./data/taskdata3.txt')]

# Upload hello application script tooAzure Storage. This is hello script that
# will process hello data files, and is executed by each of hello tasks on the
# compute nodes.
application_files = [
    upload_file_to_container(blob_client, APP_CONTAINER_NAME, file_path)
    for file_path in application_file_paths]

# Upload hello data files. This is hello data that will be processed by each of
# hello tasks executed on hello compute nodes in hello pool.
input_files = [
    upload_file_to_container(blob_client, INPUT_CONTAINER_NAME, file_path)
    for file_path in input_file_paths]
```

<span data-ttu-id="90219-206">Lista szövegértést használva hello `upload_file_to_container` függvény hívása esetén hello gyűjtemények lévő összes fájlhoz, és két [ResourceFile] [ py_resource_file] gyűjtemények fel van töltve.</span><span class="sxs-lookup"><span data-stu-id="90219-206">Using list comprehension, hello `upload_file_to_container` function is called for each file in hello collections, and two [ResourceFile][py_resource_file] collections are populated.</span></span> <span data-ttu-id="90219-207">Hello `upload_file_to_container` függvény alatt jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="90219-207">hello `upload_file_to_container` function appears below:</span></span>

```python
def upload_file_to_container(block_blob_client, container_name, path):
    """
    Uploads a local file tooan Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param str container_name: hello name of hello Azure Blob storage container.
    :param str file_path: hello local path toohello file.
    :rtype: `azure.batch.models.ResourceFile`
    :return: A ResourceFile initialized with a SAS URL appropriate for Batch
    tasks.
    """

    import datetime
    import azure.storage.blob as azureblob
    import azure.batch.models as batchmodels

    blob_name = os.path.basename(path)

    print('Uploading file {} toocontainer [{}]...'.format(path,
                                                          container_name))

    block_blob_client.create_blob_from_path(container_name,
                                            blob_name,
                                            file_path)

    sas_token = block_blob_client.generate_blob_shared_access_signature(
        container_name,
        blob_name,
        permission=azureblob.BlobPermissions.READ,
        expiry=datetime.datetime.utcnow() + datetime.timedelta(hours=2))

    sas_url = block_blob_client.make_blob_url(container_name,
                                              blob_name,
                                              sas_token=sas_token)

    return batchmodels.ResourceFile(file_path=blob_name,
                                    blob_source=sas_url)
```

### <a name="resourcefiles"></a><span data-ttu-id="90219-208">ResourceFiles</span><span class="sxs-lookup"><span data-stu-id="90219-208">ResourceFiles</span></span>
<span data-ttu-id="90219-209">A [ResourceFile] [ py_resource_file] biztosít hello URL-cím tooa fájl, amely letöltött tooa Azure Storage a kötegelt feladatok számítási csomópont e feladat futása előtt.</span><span class="sxs-lookup"><span data-stu-id="90219-209">A [ResourceFile][py_resource_file] provides tasks in Batch with hello URL tooa file in Azure Storage that is downloaded tooa compute node before that task is run.</span></span> <span data-ttu-id="90219-210">Hello [ResourceFile][py_resource_file]. **blob_source** tulajdonság hello teljes URL-címet hello fájl határozza meg, az Azure Storage található.</span><span class="sxs-lookup"><span data-stu-id="90219-210">hello [ResourceFile][py_resource_file].**blob_source** property specifies hello full URL of hello file as it exists in Azure Storage.</span></span> <span data-ttu-id="90219-211">hello URL-címet is a közös hozzáférésű jogosultságkód (SAS), amely toohello fájlt biztonságos hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="90219-211">hello URL may also include a shared access signature (SAS) that provides secure access toohello file.</span></span> <span data-ttu-id="90219-212">A Batch legtöbb feladattípusa tartalmaz *ResourceFiles* tulajdonságot, beleértve a következőket:</span><span class="sxs-lookup"><span data-stu-id="90219-212">Most task types in Batch include a *ResourceFiles* property, including:</span></span>

* <span data-ttu-id="90219-213">[CloudTask][py_task]</span><span class="sxs-lookup"><span data-stu-id="90219-213">[CloudTask][py_task]</span></span>
* <span data-ttu-id="90219-214">[StartTask][py_starttask]</span><span class="sxs-lookup"><span data-stu-id="90219-214">[StartTask][py_starttask]</span></span>
* <span data-ttu-id="90219-215">[JobPreparationTask][py_jobpreptask]</span><span class="sxs-lookup"><span data-stu-id="90219-215">[JobPreparationTask][py_jobpreptask]</span></span>
* <span data-ttu-id="90219-216">[JobReleaseTask][py_jobreltask]</span><span class="sxs-lookup"><span data-stu-id="90219-216">[JobReleaseTask][py_jobreltask]</span></span>

<span data-ttu-id="90219-217">Ez a minta hello JobPreparationTask vagy JobReleaseTask tevékenységtípusok nem használható, de további a rájuk vonatkozó [Futtatás feladat előkészítése és a befejezési feladatok Azure Batch számítási csomópontjain](batch-job-prep-release.md).</span><span class="sxs-lookup"><span data-stu-id="90219-217">This sample does not use hello JobPreparationTask or JobReleaseTask task types, but you can read more about them in [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md).</span></span>

### <a name="shared-access-signature-sas"></a><span data-ttu-id="90219-218">Közös hozzáférésű jogosultságkód (SAS)</span><span class="sxs-lookup"><span data-stu-id="90219-218">Shared access signature (SAS)</span></span>
<span data-ttu-id="90219-219">Közös hozzáférésű jogosultságkód karakterláncok, amelyek biztonságos hozzáférést toocontainers és az Azure Storage blobs.</span><span class="sxs-lookup"><span data-stu-id="90219-219">Shared access signatures are strings that provide secure access toocontainers and blobs in Azure Storage.</span></span> <span data-ttu-id="90219-220">Hello *python_tutorial_client.py* parancsfájl mindkét blob és tároló közös hozzáférésű jogosultságkód, és bemutatja, hogyan ezek megosztott tooobtain elérje aláírás karakterláncok hello tároló szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="90219-220">hello *python_tutorial_client.py* script uses both blob and container shared access signatures, and demonstrates how tooobtain these shared access signature strings from hello Storage service.</span></span>

* <span data-ttu-id="90219-221">**A BLOB megosztott hozzáférési aláírásokkal**: hello készlet StartTask által használt megosztott hozzáférési aláírásokkal blob-tárolóból hello feladat parancsfájlt és a bemeneti adatok fájlok letöltésekor (lásd: [#3. lépés](#step-3-create-batch-pool) alatt).</span><span class="sxs-lookup"><span data-stu-id="90219-221">**Blob shared access signatures**: hello pool's StartTask uses blob shared access signatures when it downloads hello task script and input data files from Storage (see [Step #3](#step-3-create-batch-pool) below).</span></span> <span data-ttu-id="90219-222">Hello `upload_file_to_container` működni *python_tutorial_client.py* hello kódot tartalmaz, amely minden egyes blob megosztott hozzáférési aláírást beolvassa.</span><span class="sxs-lookup"><span data-stu-id="90219-222">hello `upload_file_to_container` function in *python_tutorial_client.py* contains hello code that obtains each blob's shared access signature.</span></span> <span data-ttu-id="90219-223">Meghívásával ilyeneket [BlockBlobService.make_blob_url] [ py_make_blob_url] hello tárolási modulban.</span><span class="sxs-lookup"><span data-stu-id="90219-223">It does so by calling [BlockBlobService.make_blob_url][py_make_blob_url] in hello Storage module.</span></span>
* <span data-ttu-id="90219-224">**Tároló közös hozzáférésű jogosultságkódot**: minden tevékenység befejezése hello számítási csomóponton teendőit, mivel azt feltölti a kimeneti fájl toohello *kimeneti* az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="90219-224">**Container shared access signature**: As each task finishes its work on hello compute node, it uploads its output file toohello *output* container in Azure Storage.</span></span> <span data-ttu-id="90219-225">toodo, *python_tutorial_task.py* egy tároló közös hozzáférésű jogosultságkódot, amely írási hozzáférés toohello tárolót használ.</span><span class="sxs-lookup"><span data-stu-id="90219-225">toodo so, *python_tutorial_task.py* uses a container shared access signature that provides write access toohello container.</span></span> <span data-ttu-id="90219-226">Hello `get_container_sas_token` működni *python_tutorial_client.py* hello tároló közös hozzáférésű jogosultságkódot, amelyet majd, a parancssori argumentum toohello tevékenységként beolvassa.</span><span class="sxs-lookup"><span data-stu-id="90219-226">hello `get_container_sas_token` function in *python_tutorial_client.py* obtains hello container's shared access signature, which is then passed as a command-line argument toohello tasks.</span></span> <span data-ttu-id="90219-227">#5. lépés [Hozzáadás feladatok tooa feladat](#step-5-add-tasks-to-job), hello tároló SAS hello használatát ismerteti.</span><span class="sxs-lookup"><span data-stu-id="90219-227">Step #5, [Add tasks tooa job](#step-5-add-tasks-to-job), discusses hello usage of hello container SAS.</span></span>

> [!TIP]
> <span data-ttu-id="90219-228">A közös hozzáférésű jogosultságkódokról hello kétrészes adatsorozat kivételének [1. rész: ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) és [2. rész: létrehozása és SAS-kód használata hello Blob szolgáltatás](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn további információk a biztonságos hozzáférés biztosítása a tárfiókban lévő toodata.</span><span class="sxs-lookup"><span data-stu-id="90219-228">Check out hello two-part series on shared access signatures, [Part 1: Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) and [Part 2: Create and use a SAS with hello Blob service](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn more about providing secure access toodata in your Storage account.</span></span>
>
>

## <a name="step-3-create-batch-pool"></a><span data-ttu-id="90219-229">3. lépés: Batch-készlet létrehozása</span><span class="sxs-lookup"><span data-stu-id="90219-229">Step 3: Create Batch pool</span></span>
<span data-ttu-id="90219-230">![Batch-készlet létrehozása][3]
</span><span class="sxs-lookup"><span data-stu-id="90219-230">![Create a Batch pool][3]
</span></span><br/>

<span data-ttu-id="90219-231">A Batch-**készletek** olyan számítási csomópontok (virtuális gépek) gyűjteményei, amelyeken a Batch a feladatok tevékenységeit végzi el.</span><span class="sxs-lookup"><span data-stu-id="90219-231">A Batch **pool** is a collection of compute nodes (virtual machines) on which Batch executes a job's tasks.</span></span>

<span data-ttu-id="90219-232">Miután feltölti a hello feladat parancsfájl és adatok fájlok toohello tárfiókot, *python_tutorial_client.py* elindítja a Batch szolgáltatás hello interakcióba hello kötegelt Python modul használatával.</span><span class="sxs-lookup"><span data-stu-id="90219-232">After it uploads hello task script and data files toohello Storage account, *python_tutorial_client.py* starts its interaction with hello Batch service by using hello Batch Python module.</span></span> <span data-ttu-id="90219-233">toodo, egy [BatchServiceClient] [ py_batchserviceclient] jön létre:</span><span class="sxs-lookup"><span data-stu-id="90219-233">toodo so, a [BatchServiceClient][py_batchserviceclient] is created:</span></span>

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

<span data-ttu-id="90219-234">A következő számítási csomópontok készletét jön létre a Batch-fiók hívással hello túl`create_pool`.</span><span class="sxs-lookup"><span data-stu-id="90219-234">Next, a pool of compute nodes is created in hello Batch account with a call too`create_pool`.</span></span>

```python
def create_pool(batch_service_client, pool_id,
                resource_files, publisher, offer, sku):
    """
    Creates a pool of compute nodes with hello specified OS settings.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str pool_id: An ID for hello new pool.
    :param list resource_files: A collection of resource files for hello pool's
    start task.
    :param str publisher: Marketplace image publisher
    :param str offer: Marketplace image offer
    :param str sku: Marketplace image sku
    """
    print('Creating pool [{}]...'.format(pool_id))

    # Create a new pool of Linux compute nodes using an Azure Virtual Machines
    # Marketplace image. For more information about creating pools of Linux
    # nodes, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/

    # Specify hello commands for hello pool's start task. hello start task is run
    # on each node as it joins hello pool, and when it's rebooted or re-imaged.
    # We use hello start task tooprep hello node for running our task script.
    task_commands = [
        # Copy hello python_tutorial_task.py script toohello "shared" directory
        # that all tasks that run on hello node have access to.
        'cp -r $AZ_BATCH_TASK_WORKING_DIR/* $AZ_BATCH_NODE_SHARED_DIR',
        # Install pip and hello dependencies for cryptography
        'apt-get update',
        'apt-get -y install python-pip',
        'apt-get -y install build-essential libssl-dev libffi-dev python-dev',
        # Install hello azure-storage module so that hello task script can access
        # Azure Blob storage
        'pip install azure-storage']

    # Get hello node agent SKU and image reference for hello virtual machine
    # configuration.
    # For more information about hello virtual machine configuration, see:
    # https://azure.microsoft.com/documentation/articles/batch-linux-nodes/
    sku_to_use, image_ref_to_use = \
        common.helpers.select_latest_verified_vm_image_with_node_agent_sku(
            batch_service_client, publisher, offer, sku)

    new_pool = batch.models.PoolAddParameter(
        id=pool_id,
        virtual_machine_configuration=batchmodels.VirtualMachineConfiguration(
            image_reference=image_ref_to_use,
            node_agent_sku_id=sku_to_use),
        vm_size=_POOL_VM_SIZE,
        target_dedicated=_POOL_NODE_COUNT,
        start_task=batch.models.StartTask(
            command_line=
            common.helpers.wrap_commands_in_shell('linux', task_commands),
            run_elevated=True,
            wait_for_success=True,
            resource_files=resource_files),
        )

    try:
        batch_service_client.pool.add(new_pool)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

<span data-ttu-id="90219-235">Egy készlet létrehozásakor megadhatja a [PoolAddParameter] [ py_pooladdparam] , amely megadja, hogy hello készlet több tulajdonságok:</span><span class="sxs-lookup"><span data-stu-id="90219-235">When you create a pool, you define a [PoolAddParameter][py_pooladdparam] that specifies several properties for hello pool:</span></span>

* <span data-ttu-id="90219-236">**Azonosító** hello készlet (*azonosító* - szükséges)</span><span class="sxs-lookup"><span data-stu-id="90219-236">**ID** of hello pool (*id* - required)</span></span><p/><span data-ttu-id="90219-237">A Batch legtöbb entitásához hasonlóan az új készletnek egyedi azonosítóval kell rendelkeznie a Batch-fiókban.</span><span class="sxs-lookup"><span data-stu-id="90219-237">As with most entities in Batch, your new pool must have a unique ID within your Batch account.</span></span> <span data-ttu-id="90219-238">A kódot az azonosítójával toothis készlet hivatkozik, amely hello Azure hello készletébe azonosítására [portal][azure_portal].</span><span class="sxs-lookup"><span data-stu-id="90219-238">Your code refers toothis pool using its ID, and it's how you identify hello pool in hello Azure [portal][azure_portal].</span></span>
* <span data-ttu-id="90219-239">**Számítási csomópontok száma** (*target_dedicated* – kötelező)</span><span class="sxs-lookup"><span data-stu-id="90219-239">**Number of compute nodes** (*target_dedicated* - required)</span></span><p/><span data-ttu-id="90219-240">Ez a tulajdonság határozza meg, hány virtuális gépek hello készletben kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="90219-240">This property specifies how many VMs should be deployed in hello pool.</span></span> <span data-ttu-id="90219-241">Fontos, hogy minden Batch-fiókok esetében az alapértelmezett toonote **kvóta** , hogy a korlátozások száma hello **magok** (és így a számítási csomópontok) a Batch-fiók.</span><span class="sxs-lookup"><span data-stu-id="90219-241">It is important toonote that all Batch accounts have a default **quota** that limits hello number of **cores** (and thus, compute nodes) in a Batch account.</span></span> <span data-ttu-id="90219-242">Hello alapértelmezett kvóták és találhat útmutatást hogyan túl[a kvóta növeléséhez](batch-quota-limit.md#increase-a-quota) (például hello magok maximális száma a Batch-fiók) a [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md).</span><span class="sxs-lookup"><span data-stu-id="90219-242">You can find hello default quotas and instructions on how too[increase a quota](batch-quota-limit.md#increase-a-quota) (such as hello maximum number of cores in your Batch account) in [Quotas and limits for hello Azure Batch service](batch-quota-limit.md).</span></span> <span data-ttu-id="90219-243">Ha azt kérdezi magától, hogy „Miért nem ér el a készletem X-nél több csomópontot?”,</span><span class="sxs-lookup"><span data-stu-id="90219-243">If you find yourself asking "Why won't my pool reach more than X nodes?"</span></span> <span data-ttu-id="90219-244">a core kvóta hello oka lehet.</span><span class="sxs-lookup"><span data-stu-id="90219-244">this core quota may be hello cause.</span></span>
* <span data-ttu-id="90219-245">Csomópontok **operációs rendszere** (*virtual_machine_configuration* **vagy** *cloud_service_configuration* – kötelező)</span><span class="sxs-lookup"><span data-stu-id="90219-245">**Operating system** for nodes (*virtual_machine_configuration* **or** *cloud_service_configuration* - required)</span></span><p/><span data-ttu-id="90219-246">A *python_tutorial_client.py* fájlban létrehozzuk a Linux-csomópontok egy készletét a [VirtualMachineConfiguration][py_vm_config] osztállyal.</span><span class="sxs-lookup"><span data-stu-id="90219-246">In *python_tutorial_client.py*, we create a pool of Linux nodes using a [VirtualMachineConfiguration][py_vm_config].</span></span> <span data-ttu-id="90219-247">Hello `select_latest_verified_vm_image_with_node_agent_sku` működni `common.helpers` egyszerűbbé teszi a munkát [Azure virtuális gépek piactér] [ vm_marketplace] képek.</span><span class="sxs-lookup"><span data-stu-id="90219-247">hello `select_latest_verified_vm_image_with_node_agent_sku` function in `common.helpers` simplifies working with [Azure Virtual Machines Marketplace][vm_marketplace] images.</span></span> <span data-ttu-id="90219-248">További információ a piactérről származó rendszerképek használatával kapcsolatban: [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux számítási csomópontok létrehozása Azure Batch-készletekben).</span><span class="sxs-lookup"><span data-stu-id="90219-248">See [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information about using Marketplace images.</span></span>
* <span data-ttu-id="90219-249">**Számítási csomópontok mérete** (*vm_size* – kötelező)</span><span class="sxs-lookup"><span data-stu-id="90219-249">**Size of compute nodes** (*vm_size* - required)</span></span><p/><span data-ttu-id="90219-250">Mivel Linux-csomópontokat határozunk meg a [VirtualMachineConfiguration][py_vm_config] számára, megadunk egy virtuálisgép-méretet (`STANDARD_A1` ebben a példában) [az Azure-ban található virtuális gépek méreteivel](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) foglalkozó szakaszban.</span><span class="sxs-lookup"><span data-stu-id="90219-250">Since we're specifying Linux nodes for our [VirtualMachineConfiguration][py_vm_config], we specify a VM size (`STANDARD_A1` in this sample) from [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="90219-251">Lásd ismét: [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux számítási csomópontok létrehozása Azure Batch-készletekben).</span><span class="sxs-lookup"><span data-stu-id="90219-251">Again, see [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) for more information.</span></span>
* <span data-ttu-id="90219-252">**Tevékenység indítása** (*start_task* – nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="90219-252">**Start task** (*start_task* - not required)</span></span><p/><span data-ttu-id="90219-253">Hello fent fizikai csomópont tulajdonságait, valamint megadhat egy [StartTask] [ py_starttask] hello készlet (kitöltése nem kötelező).</span><span class="sxs-lookup"><span data-stu-id="90219-253">Along with hello above physical node properties, you may also specify a [StartTask][py_starttask] for hello pool (it is not required).</span></span> <span data-ttu-id="90219-254">hello StartTask végrehajtása minden egyes csomóponton, hogy a csomópont csatlakozik hello készletet, és minden alkalommal, amikor újraindítja a csomópontot.</span><span class="sxs-lookup"><span data-stu-id="90219-254">hello StartTask executes on each node as that node joins hello pool, and each time a node is restarted.</span></span> <span data-ttu-id="90219-255">hello StartTask különösen fontos a számítási csomópontok előkészítése hello végrehajtásának feladatokat, mint például a feladatok futó hello alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="90219-255">hello StartTask is especially useful for preparing compute nodes for hello execution of tasks, such as installing hello applications that your tasks run.</span></span><p/><span data-ttu-id="90219-256">A mintaalkalmazás hello StartTask hello tárolásból tölt le fájlokat másolja át. (amely hello StartTask használatával megadott **resource_files** tulajdonság) a hello StartTask *Munkakönyvtár* toohello *megosztott* hello csomóponton futó összes feladatot hozzáférő könyvtár.</span><span class="sxs-lookup"><span data-stu-id="90219-256">In this sample application, hello StartTask copies hello files that it downloads from Storage (which are specified by using hello StartTask's **resource_files** property) from hello StartTask *working directory* toohello *shared* directory that all tasks running on hello node can access.</span></span> <span data-ttu-id="90219-257">Alapvetően, ennek `python_tutorial_task.py` toohello, megosztott directory minden egyes csomóponton hello csomópont csatlakozik hello készlet, mivel így hello csomóponton futó minden feladatot-e hozzáférési engedélye.</span><span class="sxs-lookup"><span data-stu-id="90219-257">Essentially, this copies `python_tutorial_task.py` toohello shared directory on each node as hello node joins hello pool, so that any tasks that run on hello node can access it.</span></span>

<span data-ttu-id="90219-258">Előfordulhat, hogy hello hívás toohello `wrap_commands_in_shell` segítő függvény.</span><span class="sxs-lookup"><span data-stu-id="90219-258">You may notice hello call toohello `wrap_commands_in_shell` helper function.</span></span> <span data-ttu-id="90219-259">Ez a függvény különálló parancsok gyűjteményéből hoz létre a tevékenység parancssori tulajdonságának megfelelő egyetlen parancssort.</span><span class="sxs-lookup"><span data-stu-id="90219-259">This function takes a collection of separate commands and creates a single command line appropriate for a task's command-line property.</span></span>

<span data-ttu-id="90219-260">A fenti hello kódrészletet lényeges hello hello két környezeti változók használatát a rendszer **parancssor** hello StartTask tulajdonsága: `AZ_BATCH_TASK_WORKING_DIR` és `AZ_BATCH_NODE_SHARED_DIR`.</span><span class="sxs-lookup"><span data-stu-id="90219-260">Also notable in hello code snippet above is hello use of two environment variables in hello **command_line** property of hello StartTask: `AZ_BATCH_TASK_WORKING_DIR` and `AZ_BATCH_NODE_SHARED_DIR`.</span></span> <span data-ttu-id="90219-261">A Batch-készlet belül minden számítási csomópont automatikusan a több környezeti változókat, amelyek adott tooBatch van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="90219-261">Each compute node within a Batch pool is automatically configured with several environment variables that are specific tooBatch.</span></span> <span data-ttu-id="90219-262">E feladat által végrehajtott folyamat rendelkezik hozzáférési toothese környezeti változókat.</span><span class="sxs-lookup"><span data-stu-id="90219-262">Any process that is executed by a task has access toothese environment variables.</span></span>

> [!TIP]
> <span data-ttu-id="90219-263">További információk a Batch-készlet, mint a feladat működő könyvtárak, a számítási csomópontok elérhető hello környezeti változók toofind lásd: **környezeti beállítások feladatok** és **fájlok és könyvtárak**  a hello [Azure Batch funkcióinak áttekintése](batch-api-basics.md).</span><span class="sxs-lookup"><span data-stu-id="90219-263">toofind out more about hello environment variables that are available on compute nodes in a Batch pool, as well as information on task working directories, see **Environment settings for tasks** and **Files and directories** in hello [overview of Azure Batch features](batch-api-basics.md).</span></span>
>
>

## <a name="step-4-create-batch-job"></a><span data-ttu-id="90219-264">4. lépés: Batch-feladat létrehozása</span><span class="sxs-lookup"><span data-stu-id="90219-264">Step 4: Create Batch job</span></span>
<span data-ttu-id="90219-265">![Batch-feladat létrehozása][4]</span><span class="sxs-lookup"><span data-stu-id="90219-265">![Create Batch job][4]</span></span><br/>

<span data-ttu-id="90219-266">A Batch-**feladatok** számítási csomópontok készletéhez társított tevékenységek gyűjteményei.</span><span class="sxs-lookup"><span data-stu-id="90219-266">A Batch **job** is a collection of tasks, and is associated with a pool of compute nodes.</span></span> <span data-ttu-id="90219-267">hello feladatot a feladatok végrehajtása tartozó hello készlet számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="90219-267">hello tasks in a job execute on hello associated pool's compute nodes.</span></span>

<span data-ttu-id="90219-268">Egy feladat nem csak rendszerezése és nyomon követni a feladatokat a kapcsolódó munkaterheléseknél, hanem is előíró bizonyos megkötéseknek – például a maximális futási hello hello feladat (és -kiterjesztés, a feladatok által) használhatja, és a kapcsolat tooother feladatok a feladat prioritása hello Batch-fiókhoz.</span><span class="sxs-lookup"><span data-stu-id="90219-268">You can use a job not only for organizing and tracking tasks in related workloads, but also for imposing certain constraints--such as hello maximum runtime for hello job (and by extension, its tasks) and job priority in relation tooother jobs in hello Batch account.</span></span> <span data-ttu-id="90219-269">Ebben a példában azonban hello feladat tartozik csak a #3. lépésben létrehozott alkalmazáskészlet hello.</span><span class="sxs-lookup"><span data-stu-id="90219-269">In this example, however, hello job is associated only with hello pool that was created in step #3.</span></span> <span data-ttu-id="90219-270">Nincsenek konfigurálva további tulajdonságok.</span><span class="sxs-lookup"><span data-stu-id="90219-270">No additional properties are configured.</span></span>

<span data-ttu-id="90219-271">Mindegyik Batch-feladat egy adott készlettel van társítva.</span><span class="sxs-lookup"><span data-stu-id="90219-271">All Batch jobs are associated with a specific pool.</span></span> <span data-ttu-id="90219-272">Ezt a társítást azt jelzi, hogy mely csomópontok hello feladat feladatokat hajt végre.</span><span class="sxs-lookup"><span data-stu-id="90219-272">This association indicates which nodes hello job's tasks execute on.</span></span> <span data-ttu-id="90219-273">Hello használatával megadja a hello készlet [PoolInformation] [ py_poolinfo] tulajdonság, ahogy az alábbi hello kódrészlet.</span><span class="sxs-lookup"><span data-stu-id="90219-273">You specify hello pool by using hello [PoolInformation][py_poolinfo] property, as shown in hello code snippet below.</span></span>

```python
def create_job(batch_service_client, job_id, pool_id):
    """
    Creates a job with hello specified ID, associated with hello specified pool.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID for hello job.
    :param str pool_id: hello ID for hello pool.
    """
    print('Creating job [{}]...'.format(job_id))

    job = batch.models.JobAddParameter(
        job_id,
        batch.models.PoolInformation(pool_id=pool_id))

    try:
        batch_service_client.job.add(job)
    except batchmodels.batch_error.BatchErrorException as err:
        print_batch_exception(err)
        raise
```

<span data-ttu-id="90219-274">Most, hogy a feladat létrehozása után feladatok tooperform hello munkahelyi lettek hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="90219-274">Now that a job has been created, tasks are added tooperform hello work.</span></span>

## <a name="step-5-add-tasks-toojob"></a><span data-ttu-id="90219-275">5. lépés: A feladatok toojob hozzáadása</span><span class="sxs-lookup"><span data-stu-id="90219-275">Step 5: Add tasks toojob</span></span>
<span data-ttu-id="90219-276">![Feladatok toojob hozzáadása][5]</span><span class="sxs-lookup"><span data-stu-id="90219-276">![Add tasks toojob][5]</span></span><br/><span data-ttu-id="90219-277">
*(1) feladatok toohello feladat kerülnek, (2) hello feladatok ütemezett toorun csomópontján, és (3) hello feladatok hello adatok fájlok tooprocess letöltése*</span><span class="sxs-lookup"><span data-stu-id="90219-277">
*(1) Tasks are added toohello job, (2) hello tasks are scheduled toorun on nodes, and (3) hello tasks download hello data files tooprocess*</span></span>

<span data-ttu-id="90219-278">Kötegelt **feladatok** hello egyes Munkaegységek hello hajt végre, a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="90219-278">Batch **tasks** are hello individual units of work that execute on hello compute nodes.</span></span> <span data-ttu-id="90219-279">A feladatot a parancssor és hello parancsfájlok vagy végrehajtható fájlok, amely megadja, hogy a parancssor futtatja.</span><span class="sxs-lookup"><span data-stu-id="90219-279">A task has a command line and runs hello scripts or executables that you specify in that command line.</span></span>

<span data-ttu-id="90219-280">tooactually feladatok végrehajtására, a feladatok tooa feladat hozzá kell adni.</span><span class="sxs-lookup"><span data-stu-id="90219-280">tooactually perform work, tasks must be added tooa job.</span></span> <span data-ttu-id="90219-281">Minden egyes [CloudTask] [ py_task] parancssori tulajdonsággal van konfigurálva, és [ResourceFiles] [ py_resource_file] (például hello készlet StartTask), hogy hello a feladat toohello csomópont tölti le, a parancssor automatikusan végrehajtása előtt.</span><span class="sxs-lookup"><span data-stu-id="90219-281">Each [CloudTask][py_task] is configured with a command-line property and [ResourceFiles][py_resource_file] (as with hello pool's StartTask) that hello task downloads toohello node before its command line is automatically executed.</span></span> <span data-ttu-id="90219-282">Minden tevékenység hello mintában dolgozza fel a csak egy fájl.</span><span class="sxs-lookup"><span data-stu-id="90219-282">In hello sample, each task processes only one file.</span></span> <span data-ttu-id="90219-283">A ResourceFiles gyűjtemény így egyetlen elemet tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="90219-283">Thus, its ResourceFiles collection contains a single element.</span></span>

```python
def add_tasks(batch_service_client, job_id, input_files,
              output_container_name, output_container_sas_token):
    """
    Adds a task for each input file in hello collection toohello specified job.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello ID of hello job toowhich tooadd hello tasks.
    :param list input_files: A collection of input files. One task will be
     created for each input file.
    :param output_container_name: hello ID of an Azure Blob storage container to
    which hello tasks will upload their results.
    :param output_container_sas_token: A SAS token granting write access to
    hello specified Azure Blob storage container.
    """

    print('Adding {} tasks toojob [{}]...'.format(len(input_files), job_id))

    tasks = list()

    for input_file in input_files:

        command = ['python $AZ_BATCH_NODE_SHARED_DIR/python_tutorial_task.py '
                   '--filepath {} --numwords {} --storageaccount {} '
                   '--storagecontainer {} --sastoken "{}"'.format(
                    input_file.file_path,
                    '3',
                    _STORAGE_ACCOUNT_NAME,
                    output_container_name,
                    output_container_sas_token)]

        tasks.append(batch.models.TaskAddParameter(
                'topNtask{}'.format(input_files.index(input_file)),
                wrap_commands_in_shell('linux', command),
                resource_files=[input_file]
                )
        )

    batch_service_client.task.add_collection(job_id, tasks)
```

> [!IMPORTANT]
> <span data-ttu-id="90219-284">Ha környezeti változók érik, például a `$AZ_BATCH_NODE_SHARED_DIR` vagy egy alkalmazás nem található a hello csomópont végrehajtása `PATH`, feladat parancssorokat kell elindítania a hello héjas explicit módon, mint például a `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span><span class="sxs-lookup"><span data-stu-id="90219-284">When they access environment variables such as `$AZ_BATCH_NODE_SHARED_DIR` or execute an application not found in hello node's `PATH`, task command lines must invoke hello shell explicitly, such as with `/bin/sh -c MyTaskApplication $MY_ENV_VAR`.</span></span> <span data-ttu-id="90219-285">Ez a követelmény nem szükséges, ha a feladatok végrehajtása egy alkalmazás hello csomópontban `PATH` , és nem hivatkozik a környezeti változók.</span><span class="sxs-lookup"><span data-stu-id="90219-285">This requirement is unnecessary if your tasks execute an application in hello node's `PATH` and do not reference any environment variables.</span></span>
>
>

<span data-ttu-id="90219-286">Hello belül `for` hurok a fenti hello kódrészletet, láthatja, hogy az öt túl számára továbbított parancssori argumentumokat hello feladat parancssorának hello összeállított*python_tutorial_task.py*:</span><span class="sxs-lookup"><span data-stu-id="90219-286">Within hello `for` loop in hello code snippet above, you can see that hello command line for hello task is constructed with five command-line arguments that are passed too*python_tutorial_task.py*:</span></span>

1. <span data-ttu-id="90219-287">**FilePath**: Ez az hello helyi elérési út toohello fájlt, mert létezik hello csomóponton.</span><span class="sxs-lookup"><span data-stu-id="90219-287">**filepath**: This is hello local path toohello file as it exists on hello node.</span></span> <span data-ttu-id="90219-288">Ha hello ResourceFile objektum `upload_file_to_container` jött létre ehhez a tulajdonsághoz használt hello fájl nevét a 2, (hello `file_path` paraméter hello ResourceFile konstruktorban).</span><span class="sxs-lookup"><span data-stu-id="90219-288">When hello ResourceFile object in `upload_file_to_container` was created in Step 2 above, hello file name was used for this property (hello `file_path` parameter in hello ResourceFile constructor).</span></span> <span data-ttu-id="90219-289">Ez azt jelzi, hogy hello fájl található hello azonos hello csomópontban könyvtárába *python_tutorial_task.py*.</span><span class="sxs-lookup"><span data-stu-id="90219-289">This indicates that hello file can be found in hello same directory on hello node as *python_tutorial_task.py*.</span></span>
2. <span data-ttu-id="90219-290">**NUMWORDS**: hello felső *N* szavak toohello kimeneti fájl kell írni.</span><span class="sxs-lookup"><span data-stu-id="90219-290">**numwords**: hello top *N* words should be written toohello output file.</span></span>
3. <span data-ttu-id="90219-291">**storageaccount**: hello hello tároló toowhich hello feladat kimenetének birtokló tárfiók neve hello fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="90219-291">**storageaccount**: hello name of hello Storage account that owns hello container toowhich hello task output should be uploaded.</span></span>
4. <span data-ttu-id="90219-292">**storagecontainer**: hello tárolási tároló toowhich hello hello nevét a kimeneti fájlokat fel kell tölteni.</span><span class="sxs-lookup"><span data-stu-id="90219-292">**storagecontainer**: hello name of hello Storage container toowhich hello output files should be uploaded.</span></span>
5. <span data-ttu-id="90219-293">**sastoken**: hello közös hozzáférésű jogosultságkód (SAS), amely írási toohello **kimeneti** az Azure Storage tárolóban.</span><span class="sxs-lookup"><span data-stu-id="90219-293">**sastoken**: hello shared access signature (SAS) that provides write access toohello **output** container in Azure Storage.</span></span> <span data-ttu-id="90219-294">Hello *python_tutorial_task.py* parancsfájl a következő megosztott hozzáférési aláírást amikor létrehozza a BlockBlobService hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="90219-294">hello *python_tutorial_task.py* script uses this shared access signature when creates its BlockBlobService reference.</span></span> <span data-ttu-id="90219-295">Így lehetővé teszi írási toohello tároló anélkül, hogy az elérési kulcs hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="90219-295">This provides write access toohello container without requiring an access key for hello storage account.</span></span>

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a><span data-ttu-id="90219-296">6. lépés: Tevékenységek figyelése</span><span class="sxs-lookup"><span data-stu-id="90219-296">Step 6: Monitor tasks</span></span>
<span data-ttu-id="90219-297">![Tevékenységek figyelése][6]</span><span class="sxs-lookup"><span data-stu-id="90219-297">![Monitor tasks][6]</span></span><br/><span data-ttu-id="90219-298">
*hello parancsfájl (1) figyelők hello feladatok befejezési állapotának, és (2) hello feladatok feltöltése eredmény adatok tooAzure tárolási*</span><span class="sxs-lookup"><span data-stu-id="90219-298">
*hello script (1) monitors hello tasks for completion status, and (2) hello tasks upload result data tooAzure Storage*</span></span>

<span data-ttu-id="90219-299">Feladatok tooa feladatot ad hozzá, amikor ezeket automatikusan aszinkron és számítási csomóponton belül hello hello feladattal társított végrehajtásra ütemezni.</span><span class="sxs-lookup"><span data-stu-id="90219-299">When tasks are added tooa job, they are automatically queued and scheduled for execution on compute nodes within hello pool associated with hello job.</span></span> <span data-ttu-id="90219-300">Hello megadott beállítások alapján, kötegelt kezeli az összes feladat queuing, ütemezés, újrapróbálkozás és más tevékenység felügyeleti feladatokat meg.</span><span class="sxs-lookup"><span data-stu-id="90219-300">Based on hello settings you specify, Batch handles all task queuing, scheduling, retrying, and other task administration duties for you.</span></span>

<span data-ttu-id="90219-301">Nincsenek számos különböző módszer toomonitoring feladat végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="90219-301">There are many approaches toomonitoring task execution.</span></span> <span data-ttu-id="90219-302">Hello `wait_for_tasks_to_complete` működni *python_tutorial_client.py* biztosít egy egyszerű példa felügyeleti feladatok egy bizonyos állapothoz, ebben az esetben az hello [befejeződött] [ py_taskstate] állapot.</span><span class="sxs-lookup"><span data-stu-id="90219-302">hello `wait_for_tasks_to_complete` function in *python_tutorial_client.py* provides a simple example of monitoring tasks for a certain state, in this case, hello [completed][py_taskstate] state.</span></span>

```python
def wait_for_tasks_to_complete(batch_service_client, job_id, timeout):
    """
    Returns when all tasks in hello specified job reach hello Completed state.

    :param batch_service_client: A Batch service client.
    :type batch_service_client: `azure.batch.BatchServiceClient`
    :param str job_id: hello id of hello job whose tasks should be toomonitored.
    :param timedelta timeout: hello duration toowait for task completion. If all
    tasks in hello specified job do not reach Completed state within this time
    period, an exception will be raised.
    """
    timeout_expiration = datetime.datetime.now() + timeout

    print("Monitoring all tasks for 'Completed' state, timeout in {}..."
          .format(timeout), end='')

    while datetime.datetime.now() < timeout_expiration:
        print('.', end='')
        sys.stdout.flush()
        tasks = batch_service_client.task.list(job_id)

        incomplete_tasks = [task for task in tasks if
                            task.state != batchmodels.TaskState.completed]
        if not incomplete_tasks:
            print()
            return True
        else:
            time.sleep(1)

    print()
    raise RuntimeError("ERROR: Tasks did not reach 'Completed' state within "
                       "timeout period of " + str(timeout))
```

## <a name="step-7-download-task-output"></a><span data-ttu-id="90219-303">7. lépés: Feladat kimenetének letöltése</span><span class="sxs-lookup"><span data-stu-id="90219-303">Step 7: Download task output</span></span>
<span data-ttu-id="90219-304">![Tevékenység kimenetének letöltése a Storage-ból][7]</span><span class="sxs-lookup"><span data-stu-id="90219-304">![Download task output from Storage][7]</span></span><br/>

<span data-ttu-id="90219-305">Most, hogy hello feladat befejezését követően hello feladatok kimenete hello Azure Storage-ból letölthető.</span><span class="sxs-lookup"><span data-stu-id="90219-305">Now that hello job is completed, hello output from hello tasks can be downloaded from Azure Storage.</span></span> <span data-ttu-id="90219-306">Ez a lépés hívása túl`download_blobs_from_container` a *python_tutorial_client.py*:</span><span class="sxs-lookup"><span data-stu-id="90219-306">This is done with a call too`download_blobs_from_container` in *python_tutorial_client.py*:</span></span>

```python
def download_blobs_from_container(block_blob_client,
                                  container_name, directory_path):
    """
    Downloads all blobs from hello specified Azure Blob storage container.

    :param block_blob_client: A blob service client.
    :type block_blob_client: `azure.storage.blob.BlockBlobService`
    :param container_name: hello Azure Blob storage container from which to
     download files.
    :param directory_path: hello local directory toowhich toodownload hello files.
    """
    print('Downloading all files from container [{}]...'.format(
        container_name))

    container_blobs = block_blob_client.list_blobs(container_name)

    for blob in container_blobs.items:
        destination_file_path = os.path.join(directory_path, blob.name)

        block_blob_client.get_blob_to_path(container_name,
                                           blob.name,
                                           destination_file_path)

        print('  Downloaded blob [{}] from container [{}] too{}'.format(
            blob.name,
            container_name,
            destination_file_path))

    print('  Download complete!')
```

> [!NOTE]
> <span data-ttu-id="90219-307">hívás túl hello`download_blobs_from_container` a *python_tutorial_client.py* megadhatja, hogy hello fájlok letöltött tooyour kezdőkönyvtárát.</span><span class="sxs-lookup"><span data-stu-id="90219-307">hello call too`download_blobs_from_container` in *python_tutorial_client.py* specifies that hello files should be downloaded tooyour home directory.</span></span> <span data-ttu-id="90219-308">Szabad toomodify érzi, hogy a kimeneti helyet.</span><span class="sxs-lookup"><span data-stu-id="90219-308">Feel free toomodify this output location.</span></span>
>
>

## <a name="step-8-delete-containers"></a><span data-ttu-id="90219-309">8. lépés: Tárolók törlése</span><span class="sxs-lookup"><span data-stu-id="90219-309">Step 8: Delete containers</span></span>
<span data-ttu-id="90219-310">Mivel van szó, az Azure-tárolóban lévő adatok, még mindig egy jó ötlet tooremove bármely blobok, amelyek már nem szükséges a kötegelt feladatok.</span><span class="sxs-lookup"><span data-stu-id="90219-310">Because you are charged for data that resides in Azure Storage, it is always a good idea tooremove any blobs that are no longer needed for your Batch jobs.</span></span> <span data-ttu-id="90219-311">A *python_tutorial_client.py*, ebben az esetben három hívások túl[BlockBlobService.delete_container][py_delete_container]:</span><span class="sxs-lookup"><span data-stu-id="90219-311">In *python_tutorial_client.py*, this is done with three calls too[BlockBlobService.delete_container][py_delete_container]:</span></span>

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a><span data-ttu-id="90219-312">9. lépés: Hello feladat és hello készlet törlése</span><span class="sxs-lookup"><span data-stu-id="90219-312">Step 9: Delete hello job and hello pool</span></span>
<span data-ttu-id="90219-313">Hello utolsó lépést, a rendszer felszólító toodelete hello feladat és hello készlet hello által létrehozott *python_tutorial_client.py* parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="90219-313">In hello final step, you are prompted toodelete hello job and hello pool that were created by hello *python_tutorial_client.py* script.</span></span> <span data-ttu-id="90219-314">Bár magukért a munkákért és feladatokért nem kell fizetnie, a számítási csomópontokért *igen*.</span><span class="sxs-lookup"><span data-stu-id="90219-314">Although you are not charged for jobs and tasks themselves, you *are* charged for compute nodes.</span></span> <span data-ttu-id="90219-315">Ezért ajánlott csak szükség szerint lefoglalni a csomópontokat.</span><span class="sxs-lookup"><span data-stu-id="90219-315">Thus, we recommend that you allocate nodes only as needed.</span></span> <span data-ttu-id="90219-316">A nem használt készletek törlése a karbantartási folyamat része lehet.</span><span class="sxs-lookup"><span data-stu-id="90219-316">Deleting unused pools can be part of your maintenance process.</span></span>

<span data-ttu-id="90219-317">hello BatchServiceClient [JobOperations] [ py_job] és [PoolOperations] [ py_pool] egyaránt rendelkezik a megfelelő törlésre, módszerek Ha a törlés jóváhagyásához nevű:</span><span class="sxs-lookup"><span data-stu-id="90219-317">hello BatchServiceClient's [JobOperations][py_job] and [PoolOperations][py_pool] both have corresponding deletion methods, which are called if you confirm deletion:</span></span>

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> <span data-ttu-id="90219-318">Ne feledje, hogy a számítási erőforrások díjkötelesek – a nem használt készletek törlése minimalizálja a költségeket.</span><span class="sxs-lookup"><span data-stu-id="90219-318">Keep in mind that you are charged for compute resources--deleting unused pools will minimize cost.</span></span> <span data-ttu-id="90219-319">Emellett vegye figyelembe, hogy törli a készlet törlése belül, hogy minden számítási csomópontokat, és, hogy bármely hello csomópontok adatainak lesz helyreállíthatatlan hello készlet törlése után.</span><span class="sxs-lookup"><span data-stu-id="90219-319">Also, be aware that deleting a pool deletes all compute nodes within that pool, and that any data on hello nodes will be unrecoverable after hello pool is deleted.</span></span>
>
>

## <a name="run-hello-sample-script"></a><span data-ttu-id="90219-320">Hello minta parancsprogram futtatása</span><span class="sxs-lookup"><span data-stu-id="90219-320">Run hello sample script</span></span>
<span data-ttu-id="90219-321">Hello futtatásakor *python_tutorial_client.py* hello oktatóanyagot parancsfájl [kódminta][github_article_samples], hello konzol kimenete hasonló toohello következő.</span><span class="sxs-lookup"><span data-stu-id="90219-321">When you run hello *python_tutorial_client.py* script from hello tutorial [code sample][github_article_samples], hello console output is similar toohello following.</span></span> <span data-ttu-id="90219-322">Nincs a szünetelést `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` hello készlet számítási csomópontok jönnek létre, elindult, és hello készlet kezdő tevékenység hello parancsok végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="90219-322">There is a pause at `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` while hello pool's compute nodes are created, started, and hello commands in hello pool's start task are executed.</span></span> <span data-ttu-id="90219-323">Használjon hello [Azure-portálon] [ azure_portal] toomonitor a készlet, a számítási csomópontok, a feladat és a feladatok során, és végrehajtása után.</span><span class="sxs-lookup"><span data-stu-id="90219-323">Use hello [Azure portal][azure_portal] toomonitor your pool, compute nodes, job, and tasks during and after execution.</span></span> <span data-ttu-id="90219-324">Használjon hello [Azure-portálon] [ azure_portal] vagy hello [Microsoft Azure Tártallózó] [ storage_explorer] tooview hello tárolási erőforrások (tárolók és blobok) hello alkalmazás által létrehozott.</span><span class="sxs-lookup"><span data-stu-id="90219-324">Use hello [Azure portal][azure_portal] or hello [Microsoft Azure Storage Explorer][storage_explorer] tooview hello Storage resources (containers and blobs) that are created by hello application.</span></span>

> [!TIP]
> <span data-ttu-id="90219-325">Futtassa a hello *python_tutorial_client.py* belül hello parancsfájl `azure-batch-samples/Python/Batch/article_samples` könyvtár.</span><span class="sxs-lookup"><span data-stu-id="90219-325">Run hello *python_tutorial_client.py* script from within hello `azure-batch-samples/Python/Batch/article_samples` directory.</span></span> <span data-ttu-id="90219-326">A hello relatív elérési utat használ `common.helpers` importálás, így előfordulhat, hogy `ImportError: No module named 'common'` Ha hello parancsfájlt a címtáron belül nem lehet futtatni.</span><span class="sxs-lookup"><span data-stu-id="90219-326">It uses a relative path for hello `common.helpers` module import, so you might see `ImportError: No module named 'common'` if you don't run hello script from within this directory.</span></span>
>
>

<span data-ttu-id="90219-327">Tipikus végrehajtási idő **körülbelül 5 – 7 percet** futtatásakor hello minta alapértelmezett konfigurációval.</span><span class="sxs-lookup"><span data-stu-id="90219-327">Typical execution time is **approximately 5-7 minutes** when you run hello sample in its default configuration.</span></span>

```
Sample start: 2016-05-20 22:47:10

Uploading file /home/user/py_tutorial/python_tutorial_task.py toocontainer [application]...
Uploading file /home/user/py_tutorial/data/taskdata1.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata2.txt toocontainer [input]...
Uploading file /home/user/py_tutorial/data/taskdata3.txt toocontainer [input]...
Creating pool [PythonTutorialPool]...
Creating job [PythonTutorialJob]...
Adding 3 tasks toojob [PythonTutorialJob]...
Monitoring all tasks for 'Completed' state, timeout in 0:20:00..........................................................................
  Success! All tasks reached hello 'Completed' state within hello specified timeout period.
Downloading all files from container [output]...
  Downloaded blob [taskdata1_OUTPUT.txt] from container [output] too/home/user/taskdata1_OUTPUT.txt
  Downloaded blob [taskdata2_OUTPUT.txt] from container [output] too/home/user/taskdata2_OUTPUT.txt
  Downloaded blob [taskdata3_OUTPUT.txt] from container [output] too/home/user/taskdata3_OUTPUT.txt
  Download complete!
Deleting containers...

Sample end: 2016-05-20 22:53:12
Elapsed time: 0:06:02

Delete job? [Y/n]
Delete pool? [Y/n]

Press ENTER tooexit...
```

## <a name="next-steps"></a><span data-ttu-id="90219-328">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="90219-328">Next steps</span></span>
<span data-ttu-id="90219-329">Szabad toomake módosítások érzi, hogy túl*python_tutorial_client.py* és *python_tutorial_task.py* tooexperiment másik számítási forgatókönyvek.</span><span class="sxs-lookup"><span data-stu-id="90219-329">Feel free toomake changes too*python_tutorial_client.py* and *python_tutorial_task.py* tooexperiment with different compute scenarios.</span></span> <span data-ttu-id="90219-330">Például, próbálja meg a túl hozzáadni egy végrehajtási késedelem*python_tutorial_task.py* toosimulate hosszan futó feladatokat, és figyelheti azokat hello portálon.</span><span class="sxs-lookup"><span data-stu-id="90219-330">For example, try adding an execution delay too*python_tutorial_task.py* toosimulate long-running tasks and monitor them in hello portal.</span></span> <span data-ttu-id="90219-331">Próbálja meg további feladatok hozzáadása vagy módosítása a számítási csomópontok száma hello.</span><span class="sxs-lookup"><span data-stu-id="90219-331">Try adding more tasks or adjusting hello number of compute nodes.</span></span> <span data-ttu-id="90219-332">Adja hozzá a logikai toocheck, és egy meglévő készlet toospeed végrehajtási idő hello használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="90219-332">Add logic toocheck for and allow hello use of an existing pool toospeed execution time.</span></span>

<span data-ttu-id="90219-333">Most, hogy ismeri a kötegelt megoldás hello alapvető munkafolyamattal, idő toodig a Batch szolgáltatás hello toohello további szolgáltatásait is.</span><span class="sxs-lookup"><span data-stu-id="90219-333">Now that you're familiar with hello basic workflow of a Batch solution, it's time toodig in toohello additional features of hello Batch service.</span></span>

* <span data-ttu-id="90219-334">Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="90219-334">Review hello [Overview of Azure Batch features](batch-api-basics.md) article, which we recommend if you're new toohello service.</span></span>
* <span data-ttu-id="90219-335">A Start hello egyéb kötegelt fejlesztési cikkek alapján **részletes fejlesztési** a hello [kötegelt képzési terv][batch_learning_path].</span><span class="sxs-lookup"><span data-stu-id="90219-335">Start on hello other Batch development articles under **Development in-depth** in hello [Batch learning path][batch_learning_path].</span></span>
* <span data-ttu-id="90219-336">Tekintse meg a "legfontosabb N szavak" hello munkaterhelés-es hello feldolgozása különböző végrehajtásának [TopNWords] [ github_topnwords] minta.</span><span class="sxs-lookup"><span data-stu-id="90219-336">Check out a different implementation of processing hello "top N words" workload with Batch in hello [TopNWords][github_topnwords] sample.</span></span>

[azure_batch]: https://azure.microsoft.com/services/batch/
[azure_free_account]: https://azure.microsoft.com/free/
[azure_portal]: https://portal.azure.com
[batch_learning_path]: https://azure.microsoft.com/documentation/learning-paths/batch/
[blog_linux]: http://blogs.technet.com/b/windowshpc/archive/2016/03/30/introducing-linux-support-on-azure-batch.aspx
[crypto]: https://cryptography.io/en/latest/
[crypto_install]: https://cryptography.io/en/latest/installation/
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_zip]: https://github.com/Azure/azure-batch-samples/archive/master.zip
[github_topnwords]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/TopNWords
[github_article_samples]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/article_samples

[nuget_packagemgr]: https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c
[nuget_restore]: https://docs.nuget.org/consume/package-restore/msbuild-integrated#enabling-package-restore-during-build

[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_batchserviceclient]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.html#azure.batch.BatchServiceClient
[py_blockblobservice]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.blockblobservice.html#azure.storage.blob.blockblobservice.BlockBlobService
[py_cloudtask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_cs_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudServiceConfiguration
[py_delete_container]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.delete_container
[py_gen_blob_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_blob_shared_access_signature
[py_gen_container_sas]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.generate_container_shared_access_signature
[py_image_ref]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/latest/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_job]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.JobOperations
[py_jobpreptask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobPreparationTask
[py_jobreltask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.JobReleaseTask
[py_list_skus]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[py_make_blob_url]: http://azure.github.io/azure-storage-python/ref/azure.storage.blob.baseblobservice.html#azure.storage.blob.baseblobservice.BaseBlobService.make_blob_url
[py_pool]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.operations.html#azure.batch.operations.PoolOperations
[py_pooladdparam]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolAddParameter
[py_poolinfo]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.PoolInformation
[py_resource_file]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.ResourceFile
[py_samples_github]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch/
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_starttask]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.StartTask
[py_task]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.CloudTask
[py_taskstate]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.TaskState
[py_vm_config]: http://azure-sdk-for-python.readthedocs.io/en/latest/ref/azure.batch.models.html#azure.batch.models.VirtualMachineConfiguration
[pypi_batch]: https://pypi.python.org/pypi/azure-batch
[pypi_storage]: https://pypi.python.org/pypi/azure-storage
[pypi_install]: https://packaging.python.org/installing/
[storage_explorer]: http://storageexplorer.com/
[visual_studio]: https://www.visualstudio.com/products/vs-2015-product-editions
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/

[1]: ./media/batch-python-tutorial/batch_workflow_01_sm.png "Tárolók létrehozása az Azure Storage-ban"
[2]: ./media/batch-python-tutorial/batch_workflow_02_sm.png "Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers"
[3]: ./media/batch-python-tutorial/batch_workflow_03_sm.png "Batch-készlet létrehozása"
[4]: ./media/batch-python-tutorial/batch_workflow_04_sm.png "Batch-feladat létrehozása"
[5]: ./media/batch-python-tutorial/batch_workflow_05_sm.png "Feladatok toojob hozzáadása"
[6]: ./media/batch-python-tutorial/batch_workflow_06_sm.png "Tevékenységek figyelése"
[7]: ./media/batch-python-tutorial/batch_workflow_07_sm.png "Tevékenység kimenetének letöltése a Storage-ból"
[8]: ./media/batch-python-tutorial/batch_workflow_sm.png "Batch-megoldás munkafolyamata (teljes diagram)"
[9]: ./media/batch-python-tutorial/credentials_batch_sm.png "A Batch hitelesítő adatai a portálon"
[10]: ./media/batch-python-tutorial/credentials_storage_sm.png "A Storage hitelesítő adatai a portálon"
[11]: ./media/batch-python-tutorial/batch_workflow_minimal_sm.png "Batch-megoldás munkafolyamata (minimális diagram)"
