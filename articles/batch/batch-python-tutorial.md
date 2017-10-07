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
# <a name="get-started-with-hello-batch-sdk-for-python"></a>Python hello kötegelt SDK használatába

> [!div class="op_single_selector"]
> * [.NET](batch-dotnet-get-started.md)
> * [Python](batch-python-tutorial.md)
> * [Node.js](batch-nodejs-get-started.md)
>
>

A hello alapvető [Azure Batch] [ azure_batch] és hello [kötegelt Python] [ py_azure_sdk] ügyfél, a kis Batch-alkalmazások pythonban írt arról lesz szó. Úgy tekintünk, hogyan két minta parancsfájlok használata hello Batch szolgáltatás tooprocess egy párhuzamos munkaterhelés hello felhőben lévő Linux virtuális gépeken, és azok működését a [Azure Storage](../storage/common/storage-introduction.md) fájl átmeneti és lekérése. Kell egy közös kötegelt alkalmazás munkafolyamata bemutatása és szerezzen egy alapszintű hello fő összetevőit kötegelt feladatok, feladatok, készletek megértése és számítási csomópontjain.

![Batch-megoldás munkafolyamata (alapszintű)][11]<br/>

## <a name="prerequisites"></a>Előfeltételek
Ez a cikk a Python és a Linux gyakorlati ismeretét feltételezi. Azt is feltételezi, hogy Ön képes toosatisfy hello létrehozása követelmények az alább megadott Azure- és hello Batch-és tárolási szolgáltatások.

### <a name="accounts"></a>Fiókok
* **Azure-fiók**: Ha még nincs Azure-előfizetése, [hozzon létre egy ingyenes Azure-fiókot][azure_free_account].
* **Batch-fiók**: Ha már rendelkezik Azure-előfizetéssel, [hozzon létre egy Azure Batch-fiókot](batch-account-create-portal.md).
* **Tárfiók**: Lásd a [Tudnivalók az Azure Storage-fiókokról](../storage/common/storage-create-storage-account.md) cikk [Tárfiók létrehozása](../storage/common/storage-create-storage-account.md#create-a-storage-account) szakaszát.

### <a name="code-sample"></a>Kódminta
hello Python oktatóanyag [kódminta] [ github_article_samples] hello sok kötegelt mintakódok hello található egyik [azure-köteg-minták] [ github_samples] tárházából GitHub. Összes hello minta kattintva letöltheti **Klónozás vagy letöltési > töltse le a ZIP-** hello tárház kezdőlapján, vagy kattintson a hello [azure-köteg-minták-master.zip] [ github_samples_zip]közvetlen letöltési hivatkozását. Miután kibontotta már hello hello ZIP-fájl tartalmát, ebben az oktatóanyagban hello két parancsfájlok hello találhatók `article_samples` könyvtár:

`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_client.py`<br/>
`/azure-batch-samples/Python/Batch/article_samples/python_tutorial_task.py`

### <a name="python-environment"></a>Python-környezet
toorun hello *python_tutorial_client.py* kell a helyi munkaállomáson mintaparancsfájl egy **Python parancsértelmező** verziójával kompatibilis **2.7** vagy **3.3 +**. a Linux és a Windows hello parancsfájl tesztelték.

### <a name="cryptography-dependencies"></a>titkosítási függőségek
Telepítenie kell a hello hello függőségeinek [titkosítás] [ crypto] könyvtár hello által igényelt `azure-batch` és `azure-storage` Python-csomagokat. Hajtsa végre a következő műveletek a platformhoz megfelelő hello, vagy tekintse meg a toohello [titkosítás telepítési] [ crypto_install] részleteiben talál további információt:

* Ubuntu

    `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython-dev python-dev`
* CentOS

    `yum update && yum install -y gcc openssl-devel libffi-devel python-devel`
* SLES/OpenSUSE

    `zypper ref && zypper -n in libopenssl-dev libffi48-devel python-devel`
* Windows

    `pip install cryptography`

> [!NOTE]
> Ha telepíti a Python 3.3 + Linux, használja a hello python3 alakokat hello Python függőségek. Például az Ubuntu rendszeren: `apt-get update && apt-get install -y build-essential libssl-dev libffi-dev libpython3-dev python3-dev`
>
>

### <a name="azure-packages"></a>Azure-csomagok
Következő lépésként telepítse a hello **Azure Batch** és **Azure Storage** Python-csomagokat. Mindkét csomagot segítségével telepítheti **pip** és hello *requirements.txt* itt található:

`/azure-batch-samples/Python/Batch/requirements.txt`

A probléma következő **pip** tooinstall hello kötegelt és a tárolási csomagok parancssori:

`pip install -r requirements.txt`

Vagy telepítse hello [azure-köteg] [ pypi_batch] és [azure-tároló] [ pypi_storage] Python-csomagok manuálisan:

`pip install azure-batch`<br/>
`pip install azure-storage`

> [!TIP]
> Ha olyan nem rendszerjogosultságú fiókot használ, szükség lehet tooprefix a utasítással `sudo`. Például: `sudo pip install -r requirements.txt`. Ha további információt szeretne kapni a Python-csomagok telepítésével kapcsolatban, olvassa el a [csomagok telepítését][pypi_install] ismertető cikket a python.org webhelyen.
>
>

## <a name="batch-python-tutorial-code-sample"></a>Batch Python-oktatóprogram kódmintája
hello kötegelt Python oktatóanyag példakód két Python-parancsfájl és néhány adatfájlok áll.

* **python_tutorial_client.PY**: egy párhuzamos munkaterhelés, a számítási csomópontok (virtuális gépek) hello kötegelt és tárolási szolgáltatások tooexecute kommunikál. Hello *python_tutorial_client.py* parancsprogram lefut a helyi munkaállomáson.
* **python_tutorial_task.PY**: hello futó parancsfájl számítási csomópontok az Azure tooperform hello tényleges munkát. Hello mintában *python_tutorial_task.py* elemez hello (hello bemeneti fájl) Azure Storage-ból letöltött fájlok szöveget. Ezután azt szöveges fájlt hoz létre (hello kimeneti fájl), amely hello a három legfontosabb szereplő szavakkal hello bemeneti fájl listáját tartalmazza. Miután létrehoz hello kimeneti fájlt, *python_tutorial_task.py* feltöltések hello fájl tooAzure tároló. Ez lehetővé teszi a letöltési toohello ügyféloldali parancsprogram futtatása a munkaállomáson. Hello *python_tutorial_task.py* parancsfájl futtatása párhuzamosan több számítási csomópontjain a Batch szolgáltatás hello.
* **./Data/taskdata\*.txt**: ezek három szövegfájlok hello bemeneti adjon meg, a hello feladatok hello futó számítási csomópontjain.

a következő diagram hello hello elsődleges műveletek hello ügyfél és a feladat parancsfájlok által végrehajtott mutatja be. Ez az alapvető munkafolyamat számos, a Batch használatával létrehozott számítási megoldásra jellemző. Minden elérhető, a Batch szolgáltatás hello szolgáltatást nem azt mutatják, amíg a szinte minden kötegelt az eset tartalmazza a munkafolyamat részeit.

![Példa Batch-munkafolyamat][8]<br/>

[**1. lépés**](#step-1-create-storage-containers) **Tárolók** létrehozása az Azure Blob Storage-ban.<br/>
[**2. lépés**](#step-2-upload-task-script-and-data-files) A feladat parancsfájlt és a bemeneti fájlok toocontainers feltöltése.<br/>
[**3. lépés**](#step-3-create-batch-pool) Batch-**készlet** létrehozása.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**3a.** készlet hello **StartTask** letöltések hello feladat parancsfájl (python_tutorial_task.py) toonodes, mivel azok csatlakoztatását hello készlet.<br/>
[**4. lépés**](#step-4-create-batch-job) Batch-**feladat** létrehozása.<br/>
[**5. lépés**](#step-5-add-tasks-to-job) Adja hozzá **feladatok** toohello feladat.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**5a.** hello feladatok ütemezett tooexecute csomópontján.<br/>
    &nbsp;&nbsp;&nbsp;&nbsp;**5b.** Mindegyik tevékenység letölti a bemeneti adatait az Azure Storage-ból, majd elkezdi a végrehajtást.<br/>
[**6. lépés**](#step-6-monitor-tasks) Tevékenységek figyelése.<br/>
  &nbsp;&nbsp;&nbsp;&nbsp;**6a.** Feladatok befejezésekor, ezek a kimeneti adatok tooAzure tárolási feltöltése.<br/>
[**7. lépés**](#step-7-download-task-output) Tevékenység kimenetének letöltése a Storage-ból.

Ahogy említettük, nem minden Batch-megoldás végzi el pontosan ezeket a lépéseket, és sok más lépést is tartalmazhatnak, de ez a minta bemutatja a Batch-megoldások általános folyamatait.

## <a name="prepare-client-script"></a>Az ügyfélparancsfájl előkészítése
Hello minta futtatása előtt hozzáadása a kötegelt és a tárolási fiók hitelesítő adataival túl*python_tutorial_client.py*. Ha még nem tette meg, nyissa meg a következő sorokat a hitelesítő adataival kedvenc szerkesztő és a frissítés hello hello fájlt.

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

A kötegelt és a tárolási fiók hitelesítő adataival belül minden szolgáltatás hello fiók panelen található hello [Azure-portálon][azure_portal]:

![A Batch-hitelesítő adatok hello portálon][9]
![hello portal tároló hitelesítő adatait][10]<br/>

A következő részekben hello, azt elemzése által használt hello lépéseket hello parancsfájlok, a munkaterhelés, a Batch szolgáltatás hello tooprocess. Javasoljuk, toorefer rendszeresen toohello parancsfájlok közben a szerkesztőben hello cikk többi hello útján a módon működnek.

Keresse meg a következő sort a toohello **python_tutorial_client.py** toostart az 1. lépés:

```python
if __name__ == '__main__':
```

## <a name="step-1-create-storage-containers"></a>1. lépés: Storage-tárolók létrehozása
![Tárolók létrehozása az Azure Storage-ban][1]
<br/>

A Batch beépített támogatást tartalmaz az Azure Storage használatához. A tárfiók a tárolók hello feladatok futtatása a Batch-fiók szükséges hello fájlokat ad meg. hello tárolók nagy előnye, egy hely toostore hello kimeneti adatok, amelyek hello feladatok egymással. először thing hello hello *python_tutorial_client.py* parancsprogram hozható létre a három tároló [Azure Blob Storage](../storage/common/storage-introduction.md#blob-storage):

* **alkalmazás**: Ebben a tárolóban tárolni fogja hello Python-parancsfájl futtatása hello feladatok, *python_tutorial_task.py*.
* **bemeneti**: feladatok töltik le hello adatok fájlok tooprocess hello *bemeneti* tároló.
* **kimeneti**: a bemeneti fájl feldolgozása végeztével a feladatok hello eredmények toohello feltölti *kimeneti* tároló.

Egy tároló rendelés toointeract a fiókot, és tárolók, hozzon létre hello használjuk [azure-tároló] [ pypi_storage] toocreate csomag egy [BlockBlobService] [ py_blockblobservice] objektum – hello "blob ügyfél." Majd létrehozhatunk három tároló hello tárfiókban hello blob ügyfél használatával.

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

Hello tárolók létrehozása után, hello alkalmazás most már feltöltheti hello feladatok által használt hello fájlokat.

> [!TIP]
> [Hogyan toouse Azure Blob storage-ának Python](../storage/blobs/storage-python-how-to-use-blob-storage.md) jó áttekintést nyújt az Azure Storage tárolók és blobok használata. Meg kell az olvasási lista hello tetején, az Batch használatának megkezdése.
>
>

## <a name="step-2-upload-task-script-and-data-files"></a>2. lépés: Feladatparancsfájl és adatfájlok feltöltése
![Feltöltési feladat alkalmazás- és bemeneti (adatok) fájlok toocontainers][2]
<br/>

Hello fájl feltöltése a művelet, *python_tutorial_client.py* először határozza meg a gyűjteményei **alkalmazás** és **bemeneti** elérési utak fájlt a helyi számítógépen hello léteznek. Majd ezen fájlok toohello tárolók hello előző lépésben létrehozott feltölti azt.

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

Lista szövegértést használva hello `upload_file_to_container` függvény hívása esetén hello gyűjtemények lévő összes fájlhoz, és két [ResourceFile] [ py_resource_file] gyűjtemények fel van töltve. Hello `upload_file_to_container` függvény alatt jelenik meg:

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

### <a name="resourcefiles"></a>ResourceFiles
A [ResourceFile] [ py_resource_file] biztosít hello URL-cím tooa fájl, amely letöltött tooa Azure Storage a kötegelt feladatok számítási csomópont e feladat futása előtt. Hello [ResourceFile][py_resource_file]. **blob_source** tulajdonság hello teljes URL-címet hello fájl határozza meg, az Azure Storage található. hello URL-címet is a közös hozzáférésű jogosultságkód (SAS), amely toohello fájlt biztonságos hozzáférést biztosít. A Batch legtöbb feladattípusa tartalmaz *ResourceFiles* tulajdonságot, beleértve a következőket:

* [CloudTask][py_task]
* [StartTask][py_starttask]
* [JobPreparationTask][py_jobpreptask]
* [JobReleaseTask][py_jobreltask]

Ez a minta hello JobPreparationTask vagy JobReleaseTask tevékenységtípusok nem használható, de további a rájuk vonatkozó [Futtatás feladat előkészítése és a befejezési feladatok Azure Batch számítási csomópontjain](batch-job-prep-release.md).

### <a name="shared-access-signature-sas"></a>Közös hozzáférésű jogosultságkód (SAS)
Közös hozzáférésű jogosultságkód karakterláncok, amelyek biztonságos hozzáférést toocontainers és az Azure Storage blobs. Hello *python_tutorial_client.py* parancsfájl mindkét blob és tároló közös hozzáférésű jogosultságkód, és bemutatja, hogyan ezek megosztott tooobtain elérje aláírás karakterláncok hello tároló szolgáltatást.

* **A BLOB megosztott hozzáférési aláírásokkal**: hello készlet StartTask által használt megosztott hozzáférési aláírásokkal blob-tárolóból hello feladat parancsfájlt és a bemeneti adatok fájlok letöltésekor (lásd: [#3. lépés](#step-3-create-batch-pool) alatt). Hello `upload_file_to_container` működni *python_tutorial_client.py* hello kódot tartalmaz, amely minden egyes blob megosztott hozzáférési aláírást beolvassa. Meghívásával ilyeneket [BlockBlobService.make_blob_url] [ py_make_blob_url] hello tárolási modulban.
* **Tároló közös hozzáférésű jogosultságkódot**: minden tevékenység befejezése hello számítási csomóponton teendőit, mivel azt feltölti a kimeneti fájl toohello *kimeneti* az Azure Storage tárolóban. toodo, *python_tutorial_task.py* egy tároló közös hozzáférésű jogosultságkódot, amely írási hozzáférés toohello tárolót használ. Hello `get_container_sas_token` működni *python_tutorial_client.py* hello tároló közös hozzáférésű jogosultságkódot, amelyet majd, a parancssori argumentum toohello tevékenységként beolvassa. #5. lépés [Hozzáadás feladatok tooa feladat](#step-5-add-tasks-to-job), hello tároló SAS hello használatát ismerteti.

> [!TIP]
> A közös hozzáférésű jogosultságkódokról hello kétrészes adatsorozat kivételének [1. rész: ismertetése hello SAS-modell](../storage/common/storage-dotnet-shared-access-signature-part-1.md) és [2. rész: létrehozása és SAS-kód használata hello Blob szolgáltatás](../storage/blobs/storage-dotnet-shared-access-signature-part-2.md), toolearn további információk a biztonságos hozzáférés biztosítása a tárfiókban lévő toodata.
>
>

## <a name="step-3-create-batch-pool"></a>3. lépés: Batch-készlet létrehozása
![Batch-készlet létrehozása][3]
<br/>

A Batch-**készletek** olyan számítási csomópontok (virtuális gépek) gyűjteményei, amelyeken a Batch a feladatok tevékenységeit végzi el.

Miután feltölti a hello feladat parancsfájl és adatok fájlok toohello tárfiókot, *python_tutorial_client.py* elindítja a Batch szolgáltatás hello interakcióba hello kötegelt Python modul használatával. toodo, egy [BatchServiceClient] [ py_batchserviceclient] jön létre:

```python
# Create a Batch service client. We'll now be interacting with hello Batch
# service in addition tooStorage.
credentials = batchauth.SharedKeyCredentials(BATCH_ACCOUNT_NAME,
                                             BATCH_ACCOUNT_KEY)

batch_client = batch.BatchServiceClient(
    credentials,
    base_url=BATCH_ACCOUNT_URL)
```

A következő számítási csomópontok készletét jön létre a Batch-fiók hívással hello túl`create_pool`.

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

Egy készlet létrehozásakor megadhatja a [PoolAddParameter] [ py_pooladdparam] , amely megadja, hogy hello készlet több tulajdonságok:

* **Azonosító** hello készlet (*azonosító* - szükséges)<p/>A Batch legtöbb entitásához hasonlóan az új készletnek egyedi azonosítóval kell rendelkeznie a Batch-fiókban. A kódot az azonosítójával toothis készlet hivatkozik, amely hello Azure hello készletébe azonosítására [portal][azure_portal].
* **Számítási csomópontok száma** (*target_dedicated* – kötelező)<p/>Ez a tulajdonság határozza meg, hány virtuális gépek hello készletben kell telepíteni. Fontos, hogy minden Batch-fiókok esetében az alapértelmezett toonote **kvóta** , hogy a korlátozások száma hello **magok** (és így a számítási csomópontok) a Batch-fiók. Hello alapértelmezett kvóták és találhat útmutatást hogyan túl[a kvóta növeléséhez](batch-quota-limit.md#increase-a-quota) (például hello magok maximális száma a Batch-fiók) a [kvótái és korlátai hello Azure Batch szolgáltatás](batch-quota-limit.md). Ha azt kérdezi magától, hogy „Miért nem ér el a készletem X-nél több csomópontot?”, a core kvóta hello oka lehet.
* Csomópontok **operációs rendszere** (*virtual_machine_configuration* **vagy** *cloud_service_configuration* – kötelező)<p/>A *python_tutorial_client.py* fájlban létrehozzuk a Linux-csomópontok egy készletét a [VirtualMachineConfiguration][py_vm_config] osztállyal. Hello `select_latest_verified_vm_image_with_node_agent_sku` működni `common.helpers` egyszerűbbé teszi a munkát [Azure virtuális gépek piactér] [ vm_marketplace] képek. További információ a piactérről származó rendszerképek használatával kapcsolatban: [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux számítási csomópontok létrehozása Azure Batch-készletekben).
* **Számítási csomópontok mérete** (*vm_size* – kötelező)<p/>Mivel Linux-csomópontokat határozunk meg a [VirtualMachineConfiguration][py_vm_config] számára, megadunk egy virtuálisgép-méretet (`STANDARD_A1` ebben a példában) [az Azure-ban található virtuális gépek méreteivel](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) foglalkozó szakaszban. Lásd ismét: [Provision Linux compute nodes in Azure Batch pools](batch-linux-nodes.md) (Linux számítási csomópontok létrehozása Azure Batch-készletekben).
* **Tevékenység indítása** (*start_task* – nem kötelező)<p/>Hello fent fizikai csomópont tulajdonságait, valamint megadhat egy [StartTask] [ py_starttask] hello készlet (kitöltése nem kötelező). hello StartTask végrehajtása minden egyes csomóponton, hogy a csomópont csatlakozik hello készletet, és minden alkalommal, amikor újraindítja a csomópontot. hello StartTask különösen fontos a számítási csomópontok előkészítése hello végrehajtásának feladatokat, mint például a feladatok futó hello alkalmazások telepítése.<p/>A mintaalkalmazás hello StartTask hello tárolásból tölt le fájlokat másolja át. (amely hello StartTask használatával megadott **resource_files** tulajdonság) a hello StartTask *Munkakönyvtár* toohello *megosztott* hello csomóponton futó összes feladatot hozzáférő könyvtár. Alapvetően, ennek `python_tutorial_task.py` toohello, megosztott directory minden egyes csomóponton hello csomópont csatlakozik hello készlet, mivel így hello csomóponton futó minden feladatot-e hozzáférési engedélye.

Előfordulhat, hogy hello hívás toohello `wrap_commands_in_shell` segítő függvény. Ez a függvény különálló parancsok gyűjteményéből hoz létre a tevékenység parancssori tulajdonságának megfelelő egyetlen parancssort.

A fenti hello kódrészletet lényeges hello hello két környezeti változók használatát a rendszer **parancssor** hello StartTask tulajdonsága: `AZ_BATCH_TASK_WORKING_DIR` és `AZ_BATCH_NODE_SHARED_DIR`. A Batch-készlet belül minden számítási csomópont automatikusan a több környezeti változókat, amelyek adott tooBatch van konfigurálva. E feladat által végrehajtott folyamat rendelkezik hozzáférési toothese környezeti változókat.

> [!TIP]
> További információk a Batch-készlet, mint a feladat működő könyvtárak, a számítási csomópontok elérhető hello környezeti változók toofind lásd: **környezeti beállítások feladatok** és **fájlok és könyvtárak**  a hello [Azure Batch funkcióinak áttekintése](batch-api-basics.md).
>
>

## <a name="step-4-create-batch-job"></a>4. lépés: Batch-feladat létrehozása
![Batch-feladat létrehozása][4]<br/>

A Batch-**feladatok** számítási csomópontok készletéhez társított tevékenységek gyűjteményei. hello feladatot a feladatok végrehajtása tartozó hello készlet számítási csomópontok.

Egy feladat nem csak rendszerezése és nyomon követni a feladatokat a kapcsolódó munkaterheléseknél, hanem is előíró bizonyos megkötéseknek – például a maximális futási hello hello feladat (és -kiterjesztés, a feladatok által) használhatja, és a kapcsolat tooother feladatok a feladat prioritása hello Batch-fiókhoz. Ebben a példában azonban hello feladat tartozik csak a #3. lépésben létrehozott alkalmazáskészlet hello. Nincsenek konfigurálva további tulajdonságok.

Mindegyik Batch-feladat egy adott készlettel van társítva. Ezt a társítást azt jelzi, hogy mely csomópontok hello feladat feladatokat hajt végre. Hello használatával megadja a hello készlet [PoolInformation] [ py_poolinfo] tulajdonság, ahogy az alábbi hello kódrészlet.

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

Most, hogy a feladat létrehozása után feladatok tooperform hello munkahelyi lettek hozzáadva.

## <a name="step-5-add-tasks-toojob"></a>5. lépés: A feladatok toojob hozzáadása
![Feladatok toojob hozzáadása][5]<br/>
*(1) feladatok toohello feladat kerülnek, (2) hello feladatok ütemezett toorun csomópontján, és (3) hello feladatok hello adatok fájlok tooprocess letöltése*

Kötegelt **feladatok** hello egyes Munkaegységek hello hajt végre, a számítási csomópontok. A feladatot a parancssor és hello parancsfájlok vagy végrehajtható fájlok, amely megadja, hogy a parancssor futtatja.

tooactually feladatok végrehajtására, a feladatok tooa feladat hozzá kell adni. Minden egyes [CloudTask] [ py_task] parancssori tulajdonsággal van konfigurálva, és [ResourceFiles] [ py_resource_file] (például hello készlet StartTask), hogy hello a feladat toohello csomópont tölti le, a parancssor automatikusan végrehajtása előtt. Minden tevékenység hello mintában dolgozza fel a csak egy fájl. A ResourceFiles gyűjtemény így egyetlen elemet tartalmaz.

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
> Ha környezeti változók érik, például a `$AZ_BATCH_NODE_SHARED_DIR` vagy egy alkalmazás nem található a hello csomópont végrehajtása `PATH`, feladat parancssorokat kell elindítania a hello héjas explicit módon, mint például a `/bin/sh -c MyTaskApplication $MY_ENV_VAR`. Ez a követelmény nem szükséges, ha a feladatok végrehajtása egy alkalmazás hello csomópontban `PATH` , és nem hivatkozik a környezeti változók.
>
>

Hello belül `for` hurok a fenti hello kódrészletet, láthatja, hogy az öt túl számára továbbított parancssori argumentumokat hello feladat parancssorának hello összeállított*python_tutorial_task.py*:

1. **FilePath**: Ez az hello helyi elérési út toohello fájlt, mert létezik hello csomóponton. Ha hello ResourceFile objektum `upload_file_to_container` jött létre ehhez a tulajdonsághoz használt hello fájl nevét a 2, (hello `file_path` paraméter hello ResourceFile konstruktorban). Ez azt jelzi, hogy hello fájl található hello azonos hello csomópontban könyvtárába *python_tutorial_task.py*.
2. **NUMWORDS**: hello felső *N* szavak toohello kimeneti fájl kell írni.
3. **storageaccount**: hello hello tároló toowhich hello feladat kimenetének birtokló tárfiók neve hello fel kell tölteni.
4. **storagecontainer**: hello tárolási tároló toowhich hello hello nevét a kimeneti fájlokat fel kell tölteni.
5. **sastoken**: hello közös hozzáférésű jogosultságkód (SAS), amely írási toohello **kimeneti** az Azure Storage tárolóban. Hello *python_tutorial_task.py* parancsfájl a következő megosztott hozzáférési aláírást amikor létrehozza a BlockBlobService hivatkozás. Így lehetővé teszi írási toohello tároló anélkül, hogy az elérési kulcs hello tárfiók.

```python
# NOTE: Taken from python_tutorial_task.py

# Create hello blob client using hello container's SAS token.
# This allows us toocreate a client that provides write
# access only toohello container.
blob_client = azureblob.BlockBlobService(account_name=args.storageaccount,
                                         sas_token=args.sastoken)
```

## <a name="step-6-monitor-tasks"></a>6. lépés: Tevékenységek figyelése
![Tevékenységek figyelése][6]<br/>
*hello parancsfájl (1) figyelők hello feladatok befejezési állapotának, és (2) hello feladatok feltöltése eredmény adatok tooAzure tárolási*

Feladatok tooa feladatot ad hozzá, amikor ezeket automatikusan aszinkron és számítási csomóponton belül hello hello feladattal társított végrehajtásra ütemezni. Hello megadott beállítások alapján, kötegelt kezeli az összes feladat queuing, ütemezés, újrapróbálkozás és más tevékenység felügyeleti feladatokat meg.

Nincsenek számos különböző módszer toomonitoring feladat végrehajtása. Hello `wait_for_tasks_to_complete` működni *python_tutorial_client.py* biztosít egy egyszerű példa felügyeleti feladatok egy bizonyos állapothoz, ebben az esetben az hello [befejeződött] [ py_taskstate] állapot.

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

## <a name="step-7-download-task-output"></a>7. lépés: Feladat kimenetének letöltése
![Tevékenység kimenetének letöltése a Storage-ból][7]<br/>

Most, hogy hello feladat befejezését követően hello feladatok kimenete hello Azure Storage-ból letölthető. Ez a lépés hívása túl`download_blobs_from_container` a *python_tutorial_client.py*:

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
> hívás túl hello`download_blobs_from_container` a *python_tutorial_client.py* megadhatja, hogy hello fájlok letöltött tooyour kezdőkönyvtárát. Szabad toomodify érzi, hogy a kimeneti helyet.
>
>

## <a name="step-8-delete-containers"></a>8. lépés: Tárolók törlése
Mivel van szó, az Azure-tárolóban lévő adatok, még mindig egy jó ötlet tooremove bármely blobok, amelyek már nem szükséges a kötegelt feladatok. A *python_tutorial_client.py*, ebben az esetben három hívások túl[BlockBlobService.delete_container][py_delete_container]:

```python
# Clean up storage resources
print('Deleting containers...')
blob_client.delete_container(app_container_name)
blob_client.delete_container(input_container_name)
blob_client.delete_container(output_container_name)
```

## <a name="step-9-delete-hello-job-and-hello-pool"></a>9. lépés: Hello feladat és hello készlet törlése
Hello utolsó lépést, a rendszer felszólító toodelete hello feladat és hello készlet hello által létrehozott *python_tutorial_client.py* parancsfájl. Bár magukért a munkákért és feladatokért nem kell fizetnie, a számítási csomópontokért *igen*. Ezért ajánlott csak szükség szerint lefoglalni a csomópontokat. A nem használt készletek törlése a karbantartási folyamat része lehet.

hello BatchServiceClient [JobOperations] [ py_job] és [PoolOperations] [ py_pool] egyaránt rendelkezik a megfelelő törlésre, módszerek Ha a törlés jóváhagyásához nevű:

```python
# Clean up Batch resources (if hello user so chooses).
if query_yes_no('Delete job?') == 'yes':
    batch_client.job.delete(_JOB_ID)

if query_yes_no('Delete pool?') == 'yes':
    batch_client.pool.delete(_POOL_ID)
```

> [!IMPORTANT]
> Ne feledje, hogy a számítási erőforrások díjkötelesek – a nem használt készletek törlése minimalizálja a költségeket. Emellett vegye figyelembe, hogy törli a készlet törlése belül, hogy minden számítási csomópontokat, és, hogy bármely hello csomópontok adatainak lesz helyreállíthatatlan hello készlet törlése után.
>
>

## <a name="run-hello-sample-script"></a>Hello minta parancsprogram futtatása
Hello futtatásakor *python_tutorial_client.py* hello oktatóanyagot parancsfájl [kódminta][github_article_samples], hello konzol kimenete hasonló toohello következő. Nincs a szünetelést `Monitoring all tasks for 'Completed' state, timeout in 0:20:00...` hello készlet számítási csomópontok jönnek létre, elindult, és hello készlet kezdő tevékenység hello parancsok végrehajtása. Használjon hello [Azure-portálon] [ azure_portal] toomonitor a készlet, a számítási csomópontok, a feladat és a feladatok során, és végrehajtása után. Használjon hello [Azure-portálon] [ azure_portal] vagy hello [Microsoft Azure Tártallózó] [ storage_explorer] tooview hello tárolási erőforrások (tárolók és blobok) hello alkalmazás által létrehozott.

> [!TIP]
> Futtassa a hello *python_tutorial_client.py* belül hello parancsfájl `azure-batch-samples/Python/Batch/article_samples` könyvtár. A hello relatív elérési utat használ `common.helpers` importálás, így előfordulhat, hogy `ImportError: No module named 'common'` Ha hello parancsfájlt a címtáron belül nem lehet futtatni.
>
>

Tipikus végrehajtási idő **körülbelül 5 – 7 percet** futtatásakor hello minta alapértelmezett konfigurációval.

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

## <a name="next-steps"></a>Következő lépések
Szabad toomake módosítások érzi, hogy túl*python_tutorial_client.py* és *python_tutorial_task.py* tooexperiment másik számítási forgatókönyvek. Például, próbálja meg a túl hozzáadni egy végrehajtási késedelem*python_tutorial_task.py* toosimulate hosszan futó feladatokat, és figyelheti azokat hello portálon. Próbálja meg további feladatok hozzáadása vagy módosítása a számítási csomópontok száma hello. Adja hozzá a logikai toocheck, és egy meglévő készlet toospeed végrehajtási idő hello használatának engedélyezése.

Most, hogy ismeri a kötegelt megoldás hello alapvető munkafolyamattal, idő toodig a Batch szolgáltatás hello toohello további szolgáltatásait is.

* Felülvizsgálati hello [áttekintés az Azure Batch funkcióinak](batch-api-basics.md) cikk, amely ajánlott, ha még új toohello szolgáltatás.
* A Start hello egyéb kötegelt fejlesztési cikkek alapján **részletes fejlesztési** a hello [kötegelt képzési terv][batch_learning_path].
* Tekintse meg a "legfontosabb N szavak" hello munkaterhelés-es hello feldolgozása különböző végrehajtásának [TopNWords] [ github_topnwords] minta.

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
