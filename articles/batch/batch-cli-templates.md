---
title: "Azure Batch aaaRun végpont feladatok (előzetes verzió) kód írása nélkül |} Microsoft Docs"
description: "Sablon fájlok hello Azure CLI toocreate kötegelt készletek, a feladatok és a feladatok létrehozását."
services: batch
author: mscurrell
manager: timlt
ms.assetid: 
ms.service: batch
ms.devlang: na
ms.topic: article
ms.workload: big-compute
ms.date: 07/20/2017
ms.author: markscu
ms.openlocfilehash: c0434d09766451f6ba516efbad949834711ee819
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-batch-cli-templates-and-file-transfer-preview"></a><span data-ttu-id="d37cd-103">Az Azure Batch parancssori felületi sablonjainak és fájlátviteli funkciójának (előzetes verzió) használata</span><span class="sxs-lookup"><span data-stu-id="d37cd-103">Use Azure Batch CLI Templates and File Transfer (Preview)</span></span>

<span data-ttu-id="d37cd-104">Hello Azure CLI használata is lehetséges toorun kötegelt feladatok kód írása nélkül.</span><span class="sxs-lookup"><span data-stu-id="d37cd-104">Using hello Azure CLI it is possible toorun Batch jobs without writing code.</span></span>

<span data-ttu-id="d37cd-105">Sablon fájlok hozható létre és hello Azure parancssori felület, amelyek lehetővé teszik a Batch-készletek, a feladatok és a létrehozott feladatok toobe együtt.</span><span class="sxs-lookup"><span data-stu-id="d37cd-105">Template files can be created and used with hello Azure CLI that allow Batch pools, jobs, and tasks toobe created.</span></span> <span data-ttu-id="d37cd-106">Feladat bemeneti fájlok könnyen is feltölthetők a hello fiók és a feladat kimenete parancsfájlokat letöltött társított hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="d37cd-106">Job input files can be easily uploaded to hello storage account associated with hello Batch account and job output files downloaded.</span></span>

## <a name="overview"></a><span data-ttu-id="d37cd-107">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="d37cd-107">Overview</span></span>

<span data-ttu-id="d37cd-108">Egy bővítmény toohello Azure parancssori felület lehetővé teszi, hogy a kötegben használt toobe-végpontok felhasználókat, akik nem a fejlesztők által.</span><span class="sxs-lookup"><span data-stu-id="d37cd-108">An extension toohello Azure CLI enables Batch toobe used end-to-end by users who are not developers.</span></span> <span data-ttu-id="d37cd-109">Egy címkészlet is létrehozható, bemeneti adatokat feltölteni, feladatok és a kapcsolódó feladatok létrehozása és hello eredményül kapott kimeneti letöltött adatok – nincs szükség esetén kód hello CLI közvetlenül használja, vagy parancsfájlok történő integrálását.</span><span class="sxs-lookup"><span data-stu-id="d37cd-109">A pool can be created, input data uploaded, jobs and associated tasks created, and hello resulting output data downloaded – no code required, hello CLI being used directly or being integrated into scripts.</span></span>

<span data-ttu-id="d37cd-110">Kötegelt sablonok létrehozása a kiszolgálón hello [hello Azure CLI meglévő kötegelt támogatást](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) , amely lehetővé teszi a JSON-fájlok toospecify tulajdonságértékek készletek, feladatok, feladatok és más elemek hello létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d37cd-110">Batch templates build on hello [existing Batch support in hello Azure CLI](https://docs.microsoft.com/azure/batch/batch-cli-get-started#json-files-for-resource-creation) that allows JSON files toospecify property values for hello creation of pools, jobs, tasks, and other items.</span></span> <span data-ttu-id="d37cd-111">Kötegelt sablonokkal hello alábbi képességeket kerülnek mi hello JSON-fájlokat tesz lehetővé az keresztül:</span><span class="sxs-lookup"><span data-stu-id="d37cd-111">With Batch templates, hello following capabilities are added over what is possible with hello JSON files:</span></span>

-   <span data-ttu-id="d37cd-112">Paraméterekkel definiálható.</span><span class="sxs-lookup"><span data-stu-id="d37cd-112">Parameters can be defined.</span></span> <span data-ttu-id="d37cd-113">Hello sablon használatakor csak hello paraméter értékei megadott toocreate hello elem, más elem tulajdonság értékekkel meghatározott hello sablon törzsében.</span><span class="sxs-lookup"><span data-stu-id="d37cd-113">When hello template is used, only hello parameter values are specified toocreate hello item, with other item property values being specified in hello template body.</span></span> <span data-ttu-id="d37cd-114">A felhasználó, aki együttműködik a kötegelt által futtatott kötegelt és az alkalmazások toobe hozhatnak létre, készlet, feladat és egyéb tevékenység tulajdonságértékeket.</span><span class="sxs-lookup"><span data-stu-id="d37cd-114">A user who understands Batch and the applications toobe run by Batch can create templates, specifying pool, job, and task property values.</span></span> <span data-ttu-id="d37cd-115">Kisebb ismerős kötegelt és/vagy az alkalmazásokat a felhasználó csak a meghatározott hello paraméterek kell toospecify hello értékek.</span><span class="sxs-lookup"><span data-stu-id="d37cd-115">A user less familiar with Batch and/or the applications only needs toospecify hello values for hello defined parameters.</span></span>

-   <span data-ttu-id="d37cd-116">Feladat feladat előállítók hozzon létre egyet, vagy egy feladatot, elkerülve a hello társított további feladatokhoz szükséges sok feladat definíciók toobe létrehozott és jelentősen egyszerűsítő feladat elküldése.</span><span class="sxs-lookup"><span data-stu-id="d37cd-116">Job task factories create one or more tasks associated with a job, avoiding hello need for many task definitions toobe created and significantly simplifying job submission.</span></span>


<span data-ttu-id="d37cd-117">Bemeneti adatfájlok kell feladatok megadott toobe és a kimeneti adatok fájlok gyakran.</span><span class="sxs-lookup"><span data-stu-id="d37cd-117">Input data files need toobe supplied for jobs and output data files are often produced.</span></span> <span data-ttu-id="d37cd-118">A storage-fiók hozzá van rendelve, alapértelmezés szerint, az egyes kötegekben fiók és a fájlok könnyen átvitt tooand ezt a tárfiókot nem kódolási a CLI használata az lehet, és tárolás szükséges hitelesítő adatok nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="d37cd-118">A storage account is associated, by default, with each Batch account and files can be easily transferred tooand from this storage account using the CLI, with no coding and no storage credentials required.</span></span>

<span data-ttu-id="d37cd-119">Például [ffmpeg](http://ffmpeg.org/) népszerű alkalmazás, amely feldolgozza a hang- és fájlokat.</span><span class="sxs-lookup"><span data-stu-id="d37cd-119">For example, [ffmpeg](http://ffmpeg.org/) is a popular application that processes audio and video files.</span></span> <span data-ttu-id="d37cd-120">Azure Batch CLI hello használt tooinvoke ffmpeg tootranscode forrás videofájlok toodifferent megoldások is lehet.</span><span class="sxs-lookup"><span data-stu-id="d37cd-120">hello Azure Batch CLI can be used tooinvoke ffmpeg tootranscode source video files toodifferent resolutions.</span></span>

-   <span data-ttu-id="d37cd-121">Egy készlet sablon jön létre.</span><span class="sxs-lookup"><span data-stu-id="d37cd-121">A pool template is created.</span></span> <span data-ttu-id="d37cd-122">hello sablonok létrehozásának hello felhasználó tudja, hogyan toocall hello ffmpeg alkalmazás és a követelmények vonatkoznak; hello adnia megfelelő operációs rendszer, a virtuális gép mérete, hogyan ffmpeg telepítve (az alkalmazás csomagot vagy a Csomagkezelő például használata), és más tárolókészlet tulajdonság értékével.</span><span class="sxs-lookup"><span data-stu-id="d37cd-122">hello user creating hello template knows how toocall hello ffmpeg application and its requirements; they specify hello appropriate OS, VM size, how ffmpeg is installed (from an application package or using a package manager, for example), and other pool property values.</span></span> <span data-ttu-id="d37cd-123">Paraméterek jönnek létre, ezért hello sablon használatakor csak a hello alkalmazáskészlet-azonosító és a virtuális gépek száma kell-e a megadott toobe.</span><span class="sxs-lookup"><span data-stu-id="d37cd-123">Parameters are created so when hello template is used, only hello pool id and number of VMs need toobe specified.</span></span>

-   <span data-ttu-id="d37cd-124">Egy feladat sablon jön létre.</span><span class="sxs-lookup"><span data-stu-id="d37cd-124">A job template is created.</span></span> <span data-ttu-id="d37cd-125">hello felhasználói létrehozni hello sablont tudja, hogyan ffmpeg igényeinek toobe meghívott tootranscode forrás videó tooa különböző feloldásához, és adja meg a feladat parancssori hello; tudják is, hogy van-e a mappát, amely tartalmazza a hello forrás videofájlok lejátszását, a bemeneti fájl szükséges feladatokkal.</span><span class="sxs-lookup"><span data-stu-id="d37cd-125">hello user creating hello template knows how ffmpeg needs toobe invoked tootranscode source video tooa different resolution and specifies hello task command line; they also know that there is a folder containing hello source video files, with a task required per input file.</span></span>

-   <span data-ttu-id="d37cd-126">Videofájlok tootranscode állítja be a felhasználó először létrehoz egy alkalmazáskészlet hello készlet sablonnal, csak hello alkalmazáskészlet-azonosító és egyéb szükséges virtuális gépek száma.</span><span class="sxs-lookup"><span data-stu-id="d37cd-126">An end user with a set of video files tootranscode first creates a pool using hello pool template, specifying only hello pool id and number of VMs required.</span></span> <span data-ttu-id="d37cd-127">Hello forrás fájlok tootranscode majd feltöltheti teheti.</span><span class="sxs-lookup"><span data-stu-id="d37cd-127">They can then upload hello source files tootranscode.</span></span> <span data-ttu-id="d37cd-128">Egy feladat majd küldheti el hello feladat sablont használja, csak a hello alkalmazáskészlet-azonosító és a feltöltött hello forrásfájljainak helyét.</span><span class="sxs-lookup"><span data-stu-id="d37cd-128">A job can then be submitted using hello job template, specifying only hello pool id and location of hello source files uploaded.</span></span> <span data-ttu-id="d37cd-129">hello kötegelt hozza létre, egy feladathoz egy bemeneti fájl létrehozása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="d37cd-129">hello Batch job is created, with one task per input file being generated.</span></span> <span data-ttu-id="d37cd-130">Végezetül hello engedélyezi az átalakítását kimeneti fájlok letöltési lehet.</span><span class="sxs-lookup"><span data-stu-id="d37cd-130">Finally, hello transcoded output files can be download.</span></span>

## <a name="installation"></a><span data-ttu-id="d37cd-131">Telepítés</span><span class="sxs-lookup"><span data-stu-id="d37cd-131">Installation</span></span>

<span data-ttu-id="d37cd-132">hello sablon és a fájl-átviteli lehetőségeket egy bővítmény toobe telepítése szükséges.</span><span class="sxs-lookup"><span data-stu-id="d37cd-132">hello template and file transfer capabilities require an extension toobe installed.</span></span>

<span data-ttu-id="d37cd-133">Hogyan tooinstall hello Azure CLI: kapcsolatos utasításokat [Azure CLI 2.0 telepítése](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="d37cd-133">For instructions on how tooinstall hello Azure CLI see [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="d37cd-134">Egyszer hello Azure parancssori felület telepítve van, hello kötegelt bővítmény telepítheti a következő parancssori parancsot:</span><span class="sxs-lookup"><span data-stu-id="d37cd-134">Once hello Azure CLI has been installed, hello Batch extension can be installed using the following CLI command:</span></span>

```azurecli
az component update --add batch-extensions --allow-third-party
```

<span data-ttu-id="d37cd-135">Hello kötegelt bővítmény kapcsolatos további információkért lásd: [Microsoft Azure Batch CLI Extensions for Windows, Mac és Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span><span class="sxs-lookup"><span data-stu-id="d37cd-135">For more information about hello Batch extension, see [Microsoft Azure Batch CLI Extensions for Windows, Mac and Linux](https://github.com/Azure/azure-batch-cli-extensions#microsoft-azure-batch-cli-extensions-for-windows-mac-and-linux).</span></span>

## <a name="templates"></a><span data-ttu-id="d37cd-136">Sablonok</span><span class="sxs-lookup"><span data-stu-id="d37cd-136">Templates</span></span>

<span data-ttu-id="d37cd-137">hello Azure Batch parancssori felület lehetővé teszi, hogy az elemek, például készletek, feladatok és feladatok toobe hozta létre a JSON-fájl nevét és értékeit tartalmazó megadása.</span><span class="sxs-lookup"><span data-stu-id="d37cd-137">hello Azure Batch CLI allows items such as pools, jobs, and tasks toobe created by specifying a JSON file containing property names and values.</span></span> <span data-ttu-id="d37cd-138">Példa:</span><span class="sxs-lookup"><span data-stu-id="d37cd-138">For example:</span></span>

```azurecli
az batch pool create –-json-file AppPool.json
```

<span data-ttu-id="d37cd-139">Azure Batch-sablonok olyan hasonló tooAzure Resource Manager-sablonok, funkcióit és szintaxist.</span><span class="sxs-lookup"><span data-stu-id="d37cd-139">Azure Batch templates are similar tooAzure Resource Manager templates, in functionality and syntax.</span></span> <span data-ttu-id="d37cd-140">Azok az elemek nevét és értékeit tartalmazza, de vegye fel a következő fő alapelv hello JSON-fájlok:</span><span class="sxs-lookup"><span data-stu-id="d37cd-140">They are JSON files that contain item property names and values, but add hello following main concepts:</span></span>

-   <span data-ttu-id="d37cd-141">**Paraméterek**</span><span class="sxs-lookup"><span data-stu-id="d37cd-141">**Parameters**</span></span>

    -   <span data-ttu-id="d37cd-142">A tulajdonság értékek toobe csak megadni, ha a sablont hello toobe kellene paraméterértékekkel egy szervezet szakaszában megadott engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="d37cd-142">Allow property values toobe specified in a body section, with only parameter values needing toobe supplied when hello template is used.</span></span> <span data-ttu-id="d37cd-143">Például hello teljes definition készlet sikerült elhelyezni a hello törzs és csak az egyik paraméter definiálva a következő alkalmazáskészlet-azonosító; csak egy alkalmazáskészlet azonosító karakterláncot ezért szükséges megadott toobe toocreate készlet.</span><span class="sxs-lookup"><span data-stu-id="d37cd-143">For example, hello complete definition for a pool could be placed in hello body and only one parameter defined for pool id; only a pool id string therefore needs toobe supplied toocreate a pool.</span></span>
        
    -   <span data-ttu-id="d37cd-144">Kötegelt ismerete rendelkező bármely személy hello sablon törzs hozhat létre, és hello alkalmazások toobe futtathatja kötegelt; csak hello szerző által megadott paraméterek értékét meg kell adni, ha hello sablont használ.</span><span class="sxs-lookup"><span data-stu-id="d37cd-144">hello template body can be authored by someone with knowledge of Batch and hello applications toobe run by Batch; only values for hello author-defined parameters must be supplied when hello template is used.</span></span> <span data-ttu-id="d37cd-145">A felhasználó nélkül hello részletes kötegelt és/vagy alkalmazás Tudásbázis ezért a sablonok használatával.</span><span class="sxs-lookup"><span data-stu-id="d37cd-145">A user without hello in-depth Batch and/or application knowledge can therefore use the templates.</span></span>

-   <span data-ttu-id="d37cd-146">**Változók**</span><span class="sxs-lookup"><span data-stu-id="d37cd-146">**Variables**</span></span>

    -   <span data-ttu-id="d37cd-147">Engedélyezze az egyszerű vagy összetett paraméter értékek toobe egy helyen vannak megadva, és egy vagy több helyen hello sablon törzsében használja.</span><span class="sxs-lookup"><span data-stu-id="d37cd-147">Allow simple or complex parameter values toobe specified in one place and used in one or more places in hello template body.</span></span> <span data-ttu-id="d37cd-148">Változók is egyszerűbbé és hello sablon hello méretének csökkentésére, valamint teszik több fenntarthatóvá azzal, hogy egy hely toochange tulajdonságai, amelynek az értéke módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="d37cd-148">Variables can simplify and reduce hello size of hello template, as well as make it more maintainable by having one location toochange properties whose value may change.</span></span>

-   <span data-ttu-id="d37cd-149">**Magasabb szintű szerkezeteket**</span><span class="sxs-lookup"><span data-stu-id="d37cd-149">**Higher-level constructs**</span></span>

    -   <span data-ttu-id="d37cd-150">Néhány magasabb szintű szerkezeteket hello sablont, amely még nem állnak rendelkezésre a hello kötegelt API-k érhetők el.</span><span class="sxs-lookup"><span data-stu-id="d37cd-150">Some higher-level constructs are available in hello template that are not yet available in hello Batch APIs.</span></span> <span data-ttu-id="d37cd-151">Például egy feladat gyári adható meg a feladat-sablont, amely használatával a közös feladatdefiníció hello feladat több feladatot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="d37cd-151">For example, a task factory can be defined in a job template that creates multiple tasks for hello job using a common task definition.</span></span> <span data-ttu-id="d37cd-152">A fentiek elkerülése érdekében hello kell toocode dinamikusan hozzon létre több JSON-fájlt, például egy fájl feladat, valamint parancsfájl fájlok tooinstall alkalmazások létrehozása a Csomagkezelő keresztül például.</span><span class="sxs-lookup"><span data-stu-id="d37cd-152">These constructs avoid hello need toocode to dynamically create multiple JSON files, such as one file per task, as well as create script files tooinstall applications via a package manager, for example.</span></span>

    -   <span data-ttu-id="d37cd-153">Néhány pont, ahol alkalmazható, lehet, hogy a fentiek hozzáadott toothe Batch szolgáltatás és a rendelkezésre álló hello kötegelt API-k, UI, stb.</span><span class="sxs-lookup"><span data-stu-id="d37cd-153">At some point, where applicable, these constructs may be added toothe Batch service and available in hello Batch APIs, UIs, etc.</span></span>

### <a name="pool-templates"></a><span data-ttu-id="d37cd-154">Alkalmazáskészlet-sablonok</span><span class="sxs-lookup"><span data-stu-id="d37cd-154">Pool templates</span></span>

<span data-ttu-id="d37cd-155">Ezenkívül toohello standard sablon képességek paraméterek és változók, a következő magasabb szintű szerkezeteket hello hello készlet sablon támogatja:</span><span class="sxs-lookup"><span data-stu-id="d37cd-155">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello pool template:</span></span>

-   <span data-ttu-id="d37cd-156">**Csomaghivatkozásokhoz**</span><span class="sxs-lookup"><span data-stu-id="d37cd-156">**Package references**</span></span>

    -   <span data-ttu-id="d37cd-157">Opcionálisan lehetővé teszi a szoftver másolása toobe toopool csomópontok csomag kezelők.</span><span class="sxs-lookup"><span data-stu-id="d37cd-157">Optionally allows software toobe copied toopool nodes by using package managers.</span></span> <span data-ttu-id="d37cd-158">hello Csomagkezelő és csomagazonosító meg van adva.</span><span class="sxs-lookup"><span data-stu-id="d37cd-158">hello package manager and package id are specified.</span></span> <span data-ttu-id="d37cd-159">Képes toodeclare éppen egy vagy több csomagot elkerülhető hello kell toocreate olyan parancsfájlt, amely lekérdezi a szükséges hello csomagok hello script telepítése és hello parancsprogrammal minden alkalmazáskészlet-csomóponton.</span><span class="sxs-lookup"><span data-stu-id="d37cd-159">Being able toodeclare one or more packages avoids hello need toocreate a script that gets hello required packages, install hello script, and run hello script on each pool node.</span></span>

<span data-ttu-id="d37cd-160">hello az alábbiakban látható egy példa a sablont, amely ffmpeg hoz létre egy Linux virtuális gépek készletét telepítve, és csak számot kell megadni készlet azonosítója karakterlánc és hello a virtuális gépek megadott toobe toouse:</span><span class="sxs-lookup"><span data-stu-id="d37cd-160">hello following is an example of a template that creates a pool of Linux VMs with ffmpeg installed and only requires a pool id string and hello number of VMs toobe supplied toouse:</span></span>

```json
{
    "parameters": {
        "nodeCount": {
            "type": "int",
            "metadata": {
                "description": "hello number of pool nodes"
            }
        },
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello pool id "
            }
        }
    },
    "pool": {
        "type": "Microsoft.Batch/batchAccounts/pools",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('poolId')]",
            "virtualMachineConfiguration": {
                "imageReference": {
                    "publisher": "Canonical",
                    "offer": "UbuntuServer",
                    "sku": "16.04.0-LTS",
                    "version": "latest"
                },
                "nodeAgentSKUId": "batch.node.ubuntu 16.04"
            },
            "vmSize": "STANDARD_D3_V2",
            "targetDedicatedNodes": "[parameters('nodeCount')]",
            "enableAutoScale": false,
            "maxTasksPerNode": 1,
            "packageReferences": [
                {
                    "type": "aptPackage",
                    "id": "ffmpeg"
                }
            ]
        }
    }
}
```

<span data-ttu-id="d37cd-161">Ha hello sablonfájl nevet kapta _alkalmazáskészlet-ffmpeg.json_, majd hello sablon volna hívható meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d37cd-161">If hello template file was named _pool-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch pool create --template pool-ffmpeg.json
```

### <a name="job-templates"></a><span data-ttu-id="d37cd-162">Feladatsablonok</span><span class="sxs-lookup"><span data-stu-id="d37cd-162">Job templates</span></span>

<span data-ttu-id="d37cd-163">Ezenkívül toohello standard sablon képességek paraméterek és változók, a következő magasabb szintű szerkezeteket hello hello feladatsablonhoz támogatja:</span><span class="sxs-lookup"><span data-stu-id="d37cd-163">In addition toohello standard template capabilities of parameters and variables, hello following higher-level constructs are supported by hello job template:</span></span>

-   <span data-ttu-id="d37cd-164">**A feladat gyári**</span><span class="sxs-lookup"><span data-stu-id="d37cd-164">**Task factory**</span></span>

    -   <span data-ttu-id="d37cd-165">Egy feladat meghatározása a feladat több feladat jön létre.</span><span class="sxs-lookup"><span data-stu-id="d37cd-165">Creates multiple tasks for a job from one task definition.</span></span> <span data-ttu-id="d37cd-166">Három típusú feladatot gyári támogatja – paraméteres sweep, feladathoz egy fájlt, és tevékenység-gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="d37cd-166">Three types of task factory are supported – parametric sweep, task per file, and task collection.</span></span>

<span data-ttu-id="d37cd-167">hello az alábbiakban látható egy példa a sablont, amely létrehoz egy feladatot, amely egy feladat minden forrás videó fájlban létrehozott ffmpeg átkódolására MP4 videofájlok tooone a két alsó megoldások, hogy használ:</span><span class="sxs-lookup"><span data-stu-id="d37cd-167">hello following is an example of a template that creates a job that uses ffmpeg to transcode MP4 video files tooone of two lower resolutions, with one task created per source video file:</span></span>

```json
{
    "parameters": {
        "poolId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch pool which runs hello job"
            }
        },
        "jobId": {
            "type": "string",
            "metadata": {
                "description": "hello name of Azure Batch job"
            }
        },
        "resolution": {
            "type": "string",
            "defaultValue": "428x240",
            "allowedValues": [
                "428x240",
                "854x480"
            ],
            "metadata": {
                "description": "Target video resolution"
            }
        }
    },
    "job": {
        "type": "Microsoft.Batch/batchAccounts/jobs",
        "apiVersion": "2016-12-01",
        "properties": {
            "id": "[parameters('jobId')]",
            "constraints": {
                "maxWallClockTime": "PT5H",
                "maxTaskRetryCount": 1
            },
            "poolInfo": {
                "poolId": "[parameters('poolId')]"
            },
            "taskFactory": {
                "type": "taskPerFile",
                "source": { 
                    "fileGroup": "ffmpeg-input"
                },
                "repeatTask": {
                    "commandLine": "ffmpeg -i {fileName} -y -s [parameters('resolution')] -strict -2 {fileNameWithoutExtension}_[parameters('resolution')].mp4",
                    "resourceFiles": [
                        {
                            "blobSource": "{url}",
                            "filePath": "{fileName}"
                        }
                    ],
                    "outputFiles": [
                        {
                            "filePattern": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                            "destination": {
                                "autoStorage": {
                                    "path": "{fileNameWithoutExtension}_[parameters('resolution')].mp4",
                                    "fileGroup": "ffmpeg-output"
                                }
                            },
                            "uploadOptions": {
                                "uploadCondition": "TaskSuccess"
                            }
                        }
                    ]
                }
            },
            "onAllTasksComplete": "terminatejob"
        }
    }
}
```

<span data-ttu-id="d37cd-168">Ha hello sablonfájl nevet kapta _feladat-ffmpeg.json_, majd hello sablon volna hívható meg az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="d37cd-168">If hello template file was named _job-ffmpeg.json_, then hello template would be invoked as follows:</span></span>

```azurecli
az batch job create --template job-ffmpeg.json
```

## <a name="file-groups-and-file-transfer"></a><span data-ttu-id="d37cd-169">Fájlcsoportok és fájlátvitel</span><span class="sxs-lookup"><span data-stu-id="d37cd-169">File groups and file transfer</span></span>

<span data-ttu-id="d37cd-170">A legtöbb feladatot és feladatok esetében a bemeneti fájlokat, és a kimeneti fájlok előállításához.</span><span class="sxs-lookup"><span data-stu-id="d37cd-170">Most jobs and tasks require input files and produce output files.</span></span> <span data-ttu-id="d37cd-171">Mind a bemeneti fájlokból, és a kimeneti fájlok általában kell áthelyezéshez hello ügyfél toohello csomópont, vagy hello csomópont toohello ügyfél toobe.</span><span class="sxs-lookup"><span data-stu-id="d37cd-171">Both input files and output files typically need toobe transferred, either from hello client toohello node, or from hello node toohello client.</span></span> <span data-ttu-id="d37cd-172">hello Azure Batch CLI kiterjesztés kötelező fájlátvitel kivonatolja, és használja a hello storage-fiók, amely alapértelmezés szerint minden Batch-fiók jön létre.</span><span class="sxs-lookup"><span data-stu-id="d37cd-172">hello Azure Batch CLI extension abstracts away file transfer and utilizes hello storage account that is created by default for each Batch account.</span></span>

<span data-ttu-id="d37cd-173">Fájlcsoport csatlakozás az Azure storage-fiók hello létrehozott tooa tároló.</span><span class="sxs-lookup"><span data-stu-id="d37cd-173">A file group equates tooa container that is created in hello Azure storage account.</span></span> <span data-ttu-id="d37cd-174">hello fájlcsoport almappákat is.</span><span class="sxs-lookup"><span data-stu-id="d37cd-174">hello file group may have subfolders.</span></span>

<span data-ttu-id="d37cd-175">hello kötegelt CLI bővítmény parancsokat biztosít tooa megadott fájl ügyfélcsoportból feltöltése fájlok és a fájlok letöltése hello megadott fájl csoport tooa ügyfélről.</span><span class="sxs-lookup"><span data-stu-id="d37cd-175">hello Batch CLI extension provides commands for uploading files from client tooa specified file group and downloading files from hello specified file group tooa client.</span></span>

```azurecli
az batch file upload --local-path c:\source_videos\*.mp4 
    --file-group ffmpeg-input

az batch file download --file-group ffmpeg-output --local-path
    c:\output_lowres_videos
```

<span data-ttu-id="d37cd-176">Készlet és a feladat a sablonokkal fájl csoportok toobe megadott készlet csomópontok alakzatot másolási tárolt fájlok, vagy ki készlet csomópontok biztonsági tooa fájlcsoport.</span><span class="sxs-lookup"><span data-stu-id="d37cd-176">Pool and job templates allow files stored in file groups toobe specified for copy onto pool nodes or off pool nodes back tooa file group.</span></span> <span data-ttu-id="d37cd-177">Például a korábban megadott feladat sablon hello fájl csoport "ffmpeg-bevitel" beállítása hello feladat beépített hello hello videó forrásfájljainak helyét az átkódolás; hello csomópont másolható hello fájl csoport "ffmpeg-kimeneti" hello hello engedélyezi az átalakítását kimeneti fájlok esetén helyre másolt toofrom hello csomópont minden feladatot használatos.</span><span class="sxs-lookup"><span data-stu-id="d37cd-177">For example, in the job template specified previously, hello file group “ffmpeg-input” is specified for hello task factory as hello location of hello source video files copied down onto hello node for transcoding; hello file group “ffmpeg-output” is used as hello location where hello transcoded output files are copied toofrom hello node running each task.</span></span>

## <a name="summary"></a><span data-ttu-id="d37cd-178">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="d37cd-178">Summary</span></span>

<span data-ttu-id="d37cd-179">Sablon és a fájl adatátviteli támogatása jelenleg váltak elérhetővé, csak az Azure parancssori felület toohello.</span><span class="sxs-lookup"><span data-stu-id="d37cd-179">Template and file transfer support have currently been added only toohello Azure CLI.</span></span> <span data-ttu-id="d37cd-180">hello célja tooexpand hello célközönségnek juttathatja el, használhatja a kötegelt toousers, akik nem szükséges toodevelop kód megadása a hello kötegelt API-k, például a kutatók, az informatikusok stb.</span><span class="sxs-lookup"><span data-stu-id="d37cd-180">hello goal is tooexpand hello audience that can use Batch toousers who do not need toodevelop code using hello Batch APIs, such as researchers, IT users, and so on.</span></span> <span data-ttu-id="d37cd-181">Nélkül kódolási, az Azure Batch és hello alkalmazások toobe kötegelt által futtatott ismerete rendelkező felhasználók hozhatnak létre a készlet és a feladat létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="d37cd-181">Without coding, users with knowledge of Azure, Batch, and hello applications toobe run by Batch can create templates for pool and job creation.</span></span> <span data-ttu-id="d37cd-182">A sablon paraméterekkel kötegelt és hello alkalmazások részletes ismerete nélkül a felhasználók a hello sablonok.</span><span class="sxs-lookup"><span data-stu-id="d37cd-182">With template parameters, users without detailed knowledge of Batch and hello applications can use hello templates.</span></span>

<span data-ttu-id="d37cd-183">Próbálja ki hello kötegelt kiterjesztése hello Azure CLI és meg kell adnia a visszajelzést vagy a javaslatok, vagy a hello megjegyzésekkel ebben a cikkben vagy hello segítségével [Azure Batch fórum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span><span class="sxs-lookup"><span data-stu-id="d37cd-183">Try out hello Batch extension for hello Azure CLI and provide us with any feedback or suggestions, either in hello comments for this article or via hello [Azure Batch forum](https://social.msdn.microsoft.com/forums/azure/home?forum=azurebatch).</span></span>

## <a name="next-steps"></a><span data-ttu-id="d37cd-184">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d37cd-184">Next steps</span></span>

- <span data-ttu-id="d37cd-185">Hello kötegelt sablonok blogbejegyzésben található: [használó Azure Batch futó feladatok hello Azure CLI-t – nem szükséges kód](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span><span class="sxs-lookup"><span data-stu-id="d37cd-185">See hello Batch templates blog post: [Running Azure Batch jobs using hello Azure CLI – no code required](https://azure.microsoft.com/en-us/blog/running-azure-batch-jobs-using-the-azure-cli-no-code-required/).</span></span>
- <span data-ttu-id="d37cd-186">Részletes telepítési és használati dokumentációt, példák és forráskód találhatók hello [Azure GitHub-tárházban](https://github.com/Azure/azure-batch-cli-extensions).</span><span class="sxs-lookup"><span data-stu-id="d37cd-186">Detailed installation and usage documentation, samples, and source code are available in hello [Azure GitHub repository](https://github.com/Azure/azure-batch-cli-extensions).</span></span>
