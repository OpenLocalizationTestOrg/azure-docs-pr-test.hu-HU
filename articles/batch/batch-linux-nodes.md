---
title: "aaaRun Linux virtuális gép számítási csomópontok - Azure Batch |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprocess a párhuzamos számítási feladatok a Linux virtuális gépek az Azure Batch készleteinek."
services: batch
documentationcenter: python
author: tamram
manager: timlt
editor: 
ms.assetid: dc6ba151-1718-468a-b455-2da549225ab2
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: na
ms.date: 05/22/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3daabd5c577baaafd0544f9f7913cb7b116d74d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a><span data-ttu-id="5a0c3-103">Linux számítási csomópontok kötegelt készletek kiépítése</span><span class="sxs-lookup"><span data-stu-id="5a0c3-103">Provision Linux compute nodes in Batch pools</span></span>

<span data-ttu-id="5a0c3-104">A Linux és a Windows virtuális gépek Azure Batch toorun párhuzamos számítási feladatok is használhatja.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-104">You can use Azure Batch toorun parallel compute workloads on both Linux and Windows virtual machines.</span></span> <span data-ttu-id="5a0c3-105">Ez a cikk részletesen hogyan Linux toocreate készleteinek számítási mindkét hello segítségével hello Batch szolgáltatás csomópontjának [kötegelt Python] [ py_batch_package] és [Batch .NET] [ api_net] klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-105">This article details how toocreate pools of Linux compute nodes in hello Batch service by using both hello [Batch Python][py_batch_package] and [Batch .NET][api_net] client libraries.</span></span>

> [!NOTE]
> <span data-ttu-id="5a0c3-106">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-106">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="5a0c3-107">2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-107">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if hello pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="5a0c3-108">Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-108">Batch pools created prior too10 March 2016 do not support application packages.</span></span> <span data-ttu-id="5a0c3-109">Alkalmazás használatával kapcsolatos további információk a toodeploy az alkalmazások tooyour kötegelt csomópontok csomagokat, a következő témakörben: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-109">For more information about using application packages toodeploy your applications tooyour Batch nodes, see [Deploy applications toocompute nodes with Batch application packages](batch-application-packages.md).</span></span>
>
>

## <a name="virtual-machine-configuration"></a><span data-ttu-id="5a0c3-110">Virtuálisgép-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="5a0c3-110">Virtual machine configuration</span></span>
<span data-ttu-id="5a0c3-111">Számítási csomópontok készletét kötegben létrehozásakor mely tooselect hello csomópont méretét és az operációs rendszer két választási lehetősége van: Felhő konfigurálása és a virtuális gép konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-111">When you create a pool of compute nodes in Batch, you have two options from which tooselect hello node size and operating system: Cloud Services Configuration and Virtual Machine Configuration.</span></span>

<span data-ttu-id="5a0c3-112">A **Cloud Services-konfiguráció** *kizárólag* windowsos számítási csomópontok létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-112">**Cloud Services Configuration** provides Windows compute nodes *only*.</span></span> <span data-ttu-id="5a0c3-113">Elérhető számítási csomópont méretek szereplő [Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md), és a rendelkezésre álló operációs rendszeren hello szereplő [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-113">Available compute node sizes are listed in [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md), and available operating systems are listed in hello [Azure Guest OS releases and SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> <span data-ttu-id="5a0c3-114">Azure Cloud Services csomópontok tartalmazó készletet hoz létre, amikor hello csomópont méretet ad meg, és hello operációsrendszer-család hello ismerteti a korábban említett cikkeket.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-114">When you create a pool that contains Azure Cloud Services nodes, you specify hello node size and hello OS family, which are described in hello previously mentioned articles.</span></span> <span data-ttu-id="5a0c3-115">A Windows készleteinek számítási csomópontokat, a Cloud Services leggyakrabban szolgál.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-115">For pools of Windows compute nodes, Cloud Services is most commonly used.</span></span>

<span data-ttu-id="5a0c3-116">**Virtuálisgép-konfiguráció** biztosít a Linux és a Windows lemezképek számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-116">**Virtual Machine Configuration** provides both Linux and Windows images for compute nodes.</span></span> <span data-ttu-id="5a0c3-117">Elérhető számítási csomópont méretek szereplő [az Azure virtuális gépek méretei](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) és [az Azure virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-117">Available compute node sizes are listed in [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) and [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span></span> <span data-ttu-id="5a0c3-118">Amikor virtuálisgép-konfiguráció csomópontot tartalmazó készletet hoz létre, meg kell adnia hello mérete hello csomópontok hello a virtuális gép Képhivatkozás és hello kötegelt csomópont ügynök SKU toobe hello csomópontjára telepítve.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-118">When you create a pool that contains Virtual Machine Configuration nodes, you must specify hello size of hello nodes, hello virtual machine image reference, and hello Batch node agent SKU toobe installed on hello nodes.</span></span>

### <a name="virtual-machine-image-reference"></a><span data-ttu-id="5a0c3-119">Virtuális gép Képhivatkozás</span><span class="sxs-lookup"><span data-stu-id="5a0c3-119">Virtual machine image reference</span></span>
<span data-ttu-id="5a0c3-120">Batch szolgáltatás által használt hello [virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-120">hello Batch service uses [virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux compute nodes.</span></span> <span data-ttu-id="5a0c3-121">Megadhat egy lemezképét hello [Azure piactér][vm_marketplace], vagy adjon meg egy előkészített egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-121">You can specify an image from hello [Azure Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span> <span data-ttu-id="5a0c3-122">További részletek az egyéni rendszerképekről: [Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-122">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

<span data-ttu-id="5a0c3-123">A virtuális gép Képhivatkozás konfigurálásakor meg kell adnia hello virtuálisgép-lemezkép hello tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-123">When you configure a virtual machine image reference, you specify hello properties of hello virtual machine image.</span></span> <span data-ttu-id="5a0c3-124">következő tulajdonságai hello szükség, amikor egy virtuális gép Képhivatkozás létrehoz:</span><span class="sxs-lookup"><span data-stu-id="5a0c3-124">hello following properties are required when you create a virtual machine image reference:</span></span>

| <span data-ttu-id="5a0c3-125">**Kép hivatkozás tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-125">**Image reference properties**</span></span> | <span data-ttu-id="5a0c3-126">**Példa**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-126">**Example**</span></span> |
| --- | --- |
| <span data-ttu-id="5a0c3-127">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="5a0c3-127">Publisher</span></span> |<span data-ttu-id="5a0c3-128">Canonical</span><span class="sxs-lookup"><span data-stu-id="5a0c3-128">Canonical</span></span> |
| <span data-ttu-id="5a0c3-129">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="5a0c3-129">Offer</span></span> |<span data-ttu-id="5a0c3-130">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-130">UbuntuServer</span></span> |
| <span data-ttu-id="5a0c3-131">SKU</span><span class="sxs-lookup"><span data-stu-id="5a0c3-131">SKU</span></span> |<span data-ttu-id="5a0c3-132">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-132">14.04.4-LTS</span></span> |
| <span data-ttu-id="5a0c3-133">Verzió</span><span class="sxs-lookup"><span data-stu-id="5a0c3-133">Version</span></span> |<span data-ttu-id="5a0c3-134">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-134">latest</span></span> |

> [!TIP]
> <span data-ttu-id="5a0c3-135">Ezeket a tulajdonságokat, és hogyan toolist piactér a kép részletesebb [keresse meg és jelölje be Linux virtuális gép képfájljait az Azure-ban a parancssori felületen vagy a PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-135">You can learn more about these properties and how toolist Marketplace images in [Navigate and select Linux virtual machine images in Azure with CLI or PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="5a0c3-136">Vegye figyelembe, hogy nem minden piactéren elérhető rendszerkép kompatibilisek jelenleg kötegelt.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-136">Note that not all Marketplace images are currently compatible with Batch.</span></span> <span data-ttu-id="5a0c3-137">További információkért lásd: [csomópont ügynök SKU](#node-agent-sku).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-137">For more information, see [Node agent SKU](#node-agent-sku).</span></span>
>
>

### <a name="node-agent-sku"></a><span data-ttu-id="5a0c3-138">Csomópont ügynök SKU</span><span class="sxs-lookup"><span data-stu-id="5a0c3-138">Node agent SKU</span></span>
<span data-ttu-id="5a0c3-139">hello kötegelt csomópont ügynök egy olyan program, hello készlet minden egyes csomópontján fut, és hello parancs vezérlő és felületet hello csomópont és hello Batch szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-139">hello Batch node agent is a program that runs on each node in hello pool and provides hello command-and-control interface between hello node and hello Batch service.</span></span> <span data-ttu-id="5a0c3-140">Nincsenek hello csomópont ügynök SKU, úgynevezett különböző operációs rendszerek különböző implementációja.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-140">There are different implementations of hello node agent, known as SKUs, for different operating systems.</span></span> <span data-ttu-id="5a0c3-141">Alapvetően egy virtuálisgép-konfiguráció létrehozásakor hello a virtuális gép Képhivatkozás először adja meg, és hello rendszerképre hello csomópont ügynök tooinstall adja.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-141">Essentially, when you create a Virtual Machine Configuration, you first specify hello virtual machine image reference, and then you specify hello node agent tooinstall on hello image.</span></span> <span data-ttu-id="5a0c3-142">Minden csomópont ügynök SKU általában több virtuálisgép-lemezkép kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-142">Typically, each node agent SKU is compatible with multiple virtual machine images.</span></span> <span data-ttu-id="5a0c3-143">Íme néhány példa a csomópont ügynök SKU:</span><span class="sxs-lookup"><span data-stu-id="5a0c3-143">Here are a few examples of node agent SKUs:</span></span>

* <span data-ttu-id="5a0c3-144">14.04 Batch.node.ubuntu</span><span class="sxs-lookup"><span data-stu-id="5a0c3-144">batch.node.ubuntu 14.04</span></span>
* <span data-ttu-id="5a0c3-145">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-145">batch.node.centos 7</span></span>
* <span data-ttu-id="5a0c3-146">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-146">batch.node.windows amd64</span></span>

> [!IMPORTANT]
> <span data-ttu-id="5a0c3-147">Nem minden hello piactéren elérhető virtuálisgép-rendszerképek hello jelenleg elérhető kötegelt csomópontjainak ügynökeit kompatibilisek.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-147">Not all virtual machine images that are available in hello Marketplace are compatible with hello currently available Batch node agents.</span></span> <span data-ttu-id="5a0c3-148">Hello kötegelt SDK-k toolist hello rendelkezésre álló csomópont ügynök SKU használja, és a virtuálisgép-lemezképeket, amelyekhez kompatibilis hello.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-148">Use hello Batch SDKs toolist hello available node agent SKUs and hello virtual machine images with which they are compatible.</span></span> <span data-ttu-id="5a0c3-149">Lásd: hello [lista a virtuálisgép-lemezképeket](#list-of-virtual-machine-images) további információt és példákat az ebben a cikkben később tooretrieve futási időben érvényes lemezképek listáját.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-149">See hello [List of Virtual Machine images](#list-of-virtual-machine-images) later in this article for more information and examples of how tooretrieve a list of valid images at runtime.</span></span>
>
>

## <a name="create-a-linux-pool-batch-python"></a><span data-ttu-id="5a0c3-150">Linux-készlet létrehozása: kötegelt Python</span><span class="sxs-lookup"><span data-stu-id="5a0c3-150">Create a Linux pool: Batch Python</span></span>
<span data-ttu-id="5a0c3-151">hello következő kódrészletet szemlélteti, hogyan toouse hello [Microsoft Azure Batch ügyféloldali kódtára a Pythonhoz] [ py_batch_package] toocreate Ubuntu Server készlet számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-151">hello following code snippet shows an example of how toouse hello [Microsoft Azure Batch Client Library for Python][py_batch_package] toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="5a0c3-152">A kötegelt Python modul technológiáról: hello dokumentáció [azure.batch csomag] [ py_batch_docs] az olvasási hello Docs.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-152">Reference documentation for hello Batch Python module can be found at [azure.batch package][py_batch_docs] on Read hello Docs.</span></span>

<span data-ttu-id="5a0c3-153">Ezt a kódrészletet hoz létre egy [ImageReference] [ py_imagereference] explicit módon meghatározza az egyes tulajdonságát (közzétevő, offer, SKU, verziója).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-153">This snippet creates an [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version).</span></span> <span data-ttu-id="5a0c3-154">Az éles kódban, azonban javasoljuk, hogy használjon hello [list_node_agent_skus] [ py_list_skus] metódus toodetermine és válassza ki a hello elérhető rendszerkép és csomópont ügynök SKU kombinációit futásidőben.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-154">In production code, however, we recommend that you use hello [list_node_agent_skus][py_list_skus] method toodetermine and select from hello available image and node agent SKU combinations at runtime.</span></span>

```python
# Import hello required modules from the
# Azure Batch Client Library for Python
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify Batch account credentials
account = "<batch-account-name>"
key = "<batch-account-key>"
batch_url = "<batch-account-url>"

# Pool settings
pool_id = "LinuxNodesSamplePoolPython"
vm_size = "STANDARD_A1"
node_count = 1

# Initialize hello Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create hello unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure hello start task for hello pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies hello Marketplace
# virtual machine image tooinstall on hello nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create hello VirtualMachineConfiguration, specifying
# hello VM image reference and hello Batch node agent to
# be installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign hello virtual machine configuration toohello pool
new_pool.virtual_machine_configuration = vmc

# Create pool in hello Batch service
client.pool.add(new_pool)
```

<span data-ttu-id="5a0c3-155">Ahogy korábban említettük, azt javasoljuk, hogy hello létrehozása helyett [ImageReference] [ py_imagereference] explicit módon, használja a hello [list_node_agent_skus] [ py_list_skus] metódus toodynamically válassza ki a hello támogatott csomópont ügynök/Piactéri lemezkép kiegészítve.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-155">As mentioned previously, we recommend that instead of creating hello [ImageReference][py_imagereference] explicitly, you use hello [list_node_agent_skus][py_list_skus] method toodynamically select from hello currently supported node agent/Marketplace image combinations.</span></span> <span data-ttu-id="5a0c3-156">Hogyan Python kódrészletet mutat be a következő hello toouse ezt a módszert.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-156">hello following Python snippet shows how toouse this method.</span></span>

```python
# Get hello list of node agents from hello Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain hello desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick hello first image reference from hello list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create hello VirtualMachineConfiguration, specifying hello VM image
# reference and hello Batch node agent toobe installed on hello node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a><span data-ttu-id="5a0c3-157">Linux-készlet létrehozása: Batch .NET</span><span class="sxs-lookup"><span data-stu-id="5a0c3-157">Create a Linux pool: Batch .NET</span></span>
<span data-ttu-id="5a0c3-158">hello következő kódrészletet szemlélteti, hogyan toouse hello [Batch .NET] [ nuget_batch_net] ügyfél könyvtár toocreate Ubuntu Server egyes számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-158">hello following code snippet shows an example of how toouse hello [Batch .NET][nuget_batch_net] client library toocreate a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="5a0c3-159">Hello található [Batch .NET referenciadokumentációt] [ api_net] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-159">You can find hello [Batch .NET reference documentation][api_net] on MSDN.</span></span>

<span data-ttu-id="5a0c3-160">hello következő kódrészletet használja hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metódus tooselect jelenleg támogatott Piactéri lemezkép és a csomópont ügynök SKU kombinációk hello listája.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-160">hello following code snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method tooselect from hello list of currently supported Marketplace image and node agent SKU combinations.</span></span> <span data-ttu-id="5a0c3-161">Ezzel a módszerrel nem kívánatos, mert a támogatott kombinációk hello listája idő tootime változhat.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-161">This technique is desirable because hello list of supported combinations may change from time tootime.</span></span> <span data-ttu-id="5a0c3-162">A leggyakrabban a támogatott kombinációk kerülnek.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-162">Most commonly, supported combinations are added.</span></span>

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us tooselect from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of hello VM image
// that we wish toouse.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain hello first node agent SKU in hello collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create hello VirtualMachineConfiguration for use when actually
// creating hello pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create hello unbound pool object using hello VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit hello pool toohello Batch service
await pool.CommitAsync();
```

<span data-ttu-id="5a0c3-163">Bár hello előző részlet hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] módszer toodynamically listából, és válassza ki a támogatott kép és a csomópont ügynök SKU kombinációk (ajánlott), beállíthatja úgy is egy [ImageReference] [ net_imagereference] explicit módon:</span><span class="sxs-lookup"><span data-stu-id="5a0c3-163">Although hello previous snippet uses hello [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method toodynamically list and select from supported image and node agent SKU combinations (recommended), you can also configure an [ImageReference][net_imagereference] explicitly:</span></span>

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a><span data-ttu-id="5a0c3-164">Virtuálisgép-rendszerképek listája</span><span class="sxs-lookup"><span data-stu-id="5a0c3-164">List of virtual machine images</span></span>
<span data-ttu-id="5a0c3-165">hello következő táblázatban hello piactér virtuálisgép-lemezképeket, amelyek kompatibilisek a hello elérhető kötegelt csomópontjainak ügynökeit, ez a cikk legutóbbi frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-165">hello following table lists hello Marketplace virtual machine images that are compatible with hello available Batch node agents when this article was last updated.</span></span> <span data-ttu-id="5a0c3-166">Fontos, hogy a lista létrehozási nem végleges mert képek és csomópontjainak ügynökeit előfordulhat, hogy hozzáadására vagy eltávolítására bármikor toonote.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-166">It is important toonote that this list is not definitive because images and node agents may be added or removed at any time.</span></span> <span data-ttu-id="5a0c3-167">Javasoljuk, hogy a Batch-alkalmazások és szolgáltatások mindig [list_node_agent_skus] [ py_list_skus] (Python) és [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) toodetermine, és válassza ki a hello jelenleg elérhető termékváltozatok.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-167">We recommend that your Batch applications and services always use [list_node_agent_skus][py_list_skus] (Python) and [ListNodeAgentSkus][net_list_skus] (Batch .NET) toodetermine and select from hello currently available SKUs.</span></span>

> [!WARNING]
> <span data-ttu-id="5a0c3-168">a következő lista hello bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-168">hello following list may change at any time.</span></span> <span data-ttu-id="5a0c3-169">Mindig használjon hello **lista csomópont ügynök SKU** módszer áll rendelkezésre a hello kötegelt API-k toolist hello kompatibilis virtuális gép és a csomópont ügynök SKU a kötegelt feladatok futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-169">Always use hello **list node agent SKU** methods available in hello Batch APIs toolist hello compatible virtual machine and node agent SKUs when you run your Batch jobs.</span></span>
>
>

| <span data-ttu-id="5a0c3-170">**Közzétevő**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-170">**Publisher**</span></span> | <span data-ttu-id="5a0c3-171">**Az ajánlat**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-171">**Offer**</span></span> | <span data-ttu-id="5a0c3-172">**Kép Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-172">**Image SKU**</span></span> | <span data-ttu-id="5a0c3-173">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-173">**Version**</span></span> | <span data-ttu-id="5a0c3-174">**Csomópont ügynök SKU-azonosítója**</span><span class="sxs-lookup"><span data-stu-id="5a0c3-174">**Node agent SKU ID**</span></span> |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| <span data-ttu-id="5a0c3-175">Canonical</span><span class="sxs-lookup"><span data-stu-id="5a0c3-175">Canonical</span></span> | <span data-ttu-id="5a0c3-176">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-176">UbuntuServer</span></span> | <span data-ttu-id="5a0c3-177">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-177">14.04.5-LTS</span></span> | <span data-ttu-id="5a0c3-178">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-178">latest</span></span> | <span data-ttu-id="5a0c3-179">14.04 Batch.node.ubuntu</span><span class="sxs-lookup"><span data-stu-id="5a0c3-179">batch.node.ubuntu 14.04</span></span> |
| <span data-ttu-id="5a0c3-180">Canonical</span><span class="sxs-lookup"><span data-stu-id="5a0c3-180">Canonical</span></span> | <span data-ttu-id="5a0c3-181">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-181">UbuntuServer</span></span> | <span data-ttu-id="5a0c3-182">16.04.0-LTS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-182">16.04.0-LTS</span></span> | <span data-ttu-id="5a0c3-183">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-183">latest</span></span> | <span data-ttu-id="5a0c3-184">Batch.node.ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="5a0c3-184">batch.node.ubuntu 16.04</span></span> |
| <span data-ttu-id="5a0c3-185">Credativ</span><span class="sxs-lookup"><span data-stu-id="5a0c3-185">Credativ</span></span> | <span data-ttu-id="5a0c3-186">Debian</span><span class="sxs-lookup"><span data-stu-id="5a0c3-186">Debian</span></span> | <span data-ttu-id="5a0c3-187">8</span><span class="sxs-lookup"><span data-stu-id="5a0c3-187">8</span></span> | <span data-ttu-id="5a0c3-188">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-188">latest</span></span> | <span data-ttu-id="5a0c3-189">8 Batch.node.debian</span><span class="sxs-lookup"><span data-stu-id="5a0c3-189">batch.node.debian 8</span></span> |
| <span data-ttu-id="5a0c3-190">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="5a0c3-190">OpenLogic</span></span> | <span data-ttu-id="5a0c3-191">CentOS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-191">CentOS</span></span> | <span data-ttu-id="5a0c3-192">7.0</span><span class="sxs-lookup"><span data-stu-id="5a0c3-192">7.0</span></span> | <span data-ttu-id="5a0c3-193">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-193">latest</span></span> | <span data-ttu-id="5a0c3-194">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-194">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-195">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="5a0c3-195">OpenLogic</span></span> | <span data-ttu-id="5a0c3-196">CentOS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-196">CentOS</span></span> | <span data-ttu-id="5a0c3-197">7.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-197">7.1</span></span> | <span data-ttu-id="5a0c3-198">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-198">latest</span></span> | <span data-ttu-id="5a0c3-199">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-199">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-200">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="5a0c3-200">OpenLogic</span></span> | <span data-ttu-id="5a0c3-201">A HPC-centOS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-201">CentOS-HPC</span></span> | <span data-ttu-id="5a0c3-202">7.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-202">7.1</span></span> | <span data-ttu-id="5a0c3-203">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-203">latest</span></span> | <span data-ttu-id="5a0c3-204">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-204">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-205">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="5a0c3-205">OpenLogic</span></span> | <span data-ttu-id="5a0c3-206">CentOS</span><span class="sxs-lookup"><span data-stu-id="5a0c3-206">CentOS</span></span> | <span data-ttu-id="5a0c3-207">7.2</span><span class="sxs-lookup"><span data-stu-id="5a0c3-207">7.2</span></span> | <span data-ttu-id="5a0c3-208">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-208">latest</span></span> | <span data-ttu-id="5a0c3-209">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-209">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-210">Oracle</span><span class="sxs-lookup"><span data-stu-id="5a0c3-210">Oracle</span></span> | <span data-ttu-id="5a0c3-211">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="5a0c3-211">Oracle-Linux</span></span> | <span data-ttu-id="5a0c3-212">7.0</span><span class="sxs-lookup"><span data-stu-id="5a0c3-212">7.0</span></span> | <span data-ttu-id="5a0c3-213">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-213">latest</span></span> | <span data-ttu-id="5a0c3-214">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-214">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-215">Oracle</span><span class="sxs-lookup"><span data-stu-id="5a0c3-215">Oracle</span></span> | <span data-ttu-id="5a0c3-216">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="5a0c3-216">Oracle-Linux</span></span> | <span data-ttu-id="5a0c3-217">7.2</span><span class="sxs-lookup"><span data-stu-id="5a0c3-217">7.2</span></span> | <span data-ttu-id="5a0c3-218">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-218">latest</span></span> | <span data-ttu-id="5a0c3-219">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-219">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-220">SUSE</span><span class="sxs-lookup"><span data-stu-id="5a0c3-220">SUSE</span></span> | <span data-ttu-id="5a0c3-221">openSUSE</span><span class="sxs-lookup"><span data-stu-id="5a0c3-221">openSUSE</span></span> | <span data-ttu-id="5a0c3-222">13.2</span><span class="sxs-lookup"><span data-stu-id="5a0c3-222">13.2</span></span> | <span data-ttu-id="5a0c3-223">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-223">latest</span></span> | <span data-ttu-id="5a0c3-224">Batch.node.opensuse 13.2</span><span class="sxs-lookup"><span data-stu-id="5a0c3-224">batch.node.opensuse 13.2</span></span> |
| <span data-ttu-id="5a0c3-225">SUSE</span><span class="sxs-lookup"><span data-stu-id="5a0c3-225">SUSE</span></span> | <span data-ttu-id="5a0c3-226">openSUSE-termékek</span><span class="sxs-lookup"><span data-stu-id="5a0c3-226">openSUSE-Leap</span></span> | <span data-ttu-id="5a0c3-227">42.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-227">42.1</span></span> | <span data-ttu-id="5a0c3-228">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-228">latest</span></span> | <span data-ttu-id="5a0c3-229">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-229">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="5a0c3-230">SUSE</span><span class="sxs-lookup"><span data-stu-id="5a0c3-230">SUSE</span></span> | <span data-ttu-id="5a0c3-231">SLES</span><span class="sxs-lookup"><span data-stu-id="5a0c3-231">SLES</span></span> | <span data-ttu-id="5a0c3-232">12-SP1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-232">12-SP1</span></span> | <span data-ttu-id="5a0c3-233">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-233">latest</span></span> | <span data-ttu-id="5a0c3-234">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-234">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="5a0c3-235">SUSE</span><span class="sxs-lookup"><span data-stu-id="5a0c3-235">SUSE</span></span> | <span data-ttu-id="5a0c3-236">SLES HPC</span><span class="sxs-lookup"><span data-stu-id="5a0c3-236">SLES-HPC</span></span> | <span data-ttu-id="5a0c3-237">12-SP1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-237">12-SP1</span></span> | <span data-ttu-id="5a0c3-238">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-238">latest</span></span> | <span data-ttu-id="5a0c3-239">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="5a0c3-239">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="5a0c3-240">Microsoft-ads</span><span class="sxs-lookup"><span data-stu-id="5a0c3-240">microsoft-ads</span></span> | <span data-ttu-id="5a0c3-241">Linux-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="5a0c3-241">linux-data-science-vm</span></span> | <span data-ttu-id="5a0c3-242">linuxdsvm</span><span class="sxs-lookup"><span data-stu-id="5a0c3-242">linuxdsvm</span></span> | <span data-ttu-id="5a0c3-243">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-243">latest</span></span> | <span data-ttu-id="5a0c3-244">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="5a0c3-244">batch.node.centos 7</span></span> |
| <span data-ttu-id="5a0c3-245">Microsoft-ads</span><span class="sxs-lookup"><span data-stu-id="5a0c3-245">microsoft-ads</span></span> | <span data-ttu-id="5a0c3-246">Standard-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="5a0c3-246">standard-data-science-vm</span></span> | <span data-ttu-id="5a0c3-247">Standard-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="5a0c3-247">standard-data-science-vm</span></span> | <span data-ttu-id="5a0c3-248">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-248">latest</span></span> | <span data-ttu-id="5a0c3-249">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-249">batch.node.windows amd64</span></span> |
| <span data-ttu-id="5a0c3-250">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-250">MicrosoftWindowsServer</span></span> | <span data-ttu-id="5a0c3-251">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-251">WindowsServer</span></span> | <span data-ttu-id="5a0c3-252">2008 R2 SP1 CSOMAG</span><span class="sxs-lookup"><span data-stu-id="5a0c3-252">2008-R2-SP1</span></span> | <span data-ttu-id="5a0c3-253">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-253">latest</span></span> | <span data-ttu-id="5a0c3-254">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-254">batch.node.windows amd64</span></span> |
| <span data-ttu-id="5a0c3-255">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-255">MicrosoftWindowsServer</span></span> | <span data-ttu-id="5a0c3-256">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-256">WindowsServer</span></span> | <span data-ttu-id="5a0c3-257">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="5a0c3-257">2012-Datacenter</span></span> | <span data-ttu-id="5a0c3-258">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-258">latest</span></span> | <span data-ttu-id="5a0c3-259">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-259">batch.node.windows amd64</span></span> |
| <span data-ttu-id="5a0c3-260">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-260">MicrosoftWindowsServer</span></span> | <span data-ttu-id="5a0c3-261">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-261">WindowsServer</span></span> | <span data-ttu-id="5a0c3-262">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="5a0c3-262">2012-R2-Datacenter</span></span> | <span data-ttu-id="5a0c3-263">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-263">latest</span></span> | <span data-ttu-id="5a0c3-264">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-264">batch.node.windows amd64</span></span> |
| <span data-ttu-id="5a0c3-265">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-265">MicrosoftWindowsServer</span></span> | <span data-ttu-id="5a0c3-266">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-266">WindowsServer</span></span> | <span data-ttu-id="5a0c3-267">2016-Datacenter</span><span class="sxs-lookup"><span data-stu-id="5a0c3-267">2016-Datacenter</span></span> | <span data-ttu-id="5a0c3-268">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-268">latest</span></span> | <span data-ttu-id="5a0c3-269">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-269">batch.node.windows amd64</span></span> |
| <span data-ttu-id="5a0c3-270">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-270">MicrosoftWindowsServer</span></span> | <span data-ttu-id="5a0c3-271">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="5a0c3-271">WindowsServer</span></span> | <span data-ttu-id="5a0c3-272">2016-adatközpont-az-tárolók</span><span class="sxs-lookup"><span data-stu-id="5a0c3-272">2016-Datacenter-with-Containers</span></span> | <span data-ttu-id="5a0c3-273">legújabb</span><span class="sxs-lookup"><span data-stu-id="5a0c3-273">latest</span></span> | <span data-ttu-id="5a0c3-274">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="5a0c3-274">batch.node.windows amd64</span></span> |

## <a name="connect-toolinux-nodes-using-ssh"></a><span data-ttu-id="5a0c3-275">Csatlakozzon az SSH használatával tooLinux csomópontok</span><span class="sxs-lookup"><span data-stu-id="5a0c3-275">Connect tooLinux nodes using SSH</span></span>
<span data-ttu-id="5a0c3-276">A fejlesztés során, vagy a hibaelhárítás során azt tapasztalhatja, szükséges toosign toohello csomópontok a készlethez.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-276">During development or while troubleshooting, you may find it necessary toosign in toohello nodes in your pool.</span></span> <span data-ttu-id="5a0c3-277">Windows számítási csomópontokat, ellentétben a távoli asztal protokoll (RDP) tooconnect tooLinux csomópontok nem használhat.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-277">Unlike Windows compute nodes, you cannot use Remote Desktop Protocol (RDP) tooconnect tooLinux nodes.</span></span> <span data-ttu-id="5a0c3-278">Ehelyett hello Batch szolgáltatás lehetővé teszi, hogy a távoli kapcsolat minden egyes csomóponton SSH-elérést.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-278">Instead, hello Batch service enables SSH access on each node for remote connection.</span></span>

<span data-ttu-id="5a0c3-279">hello következő Python kódrészletet hoz létre a felhasználó a készletbe, amely pedig szükséges a távoli kapcsolat minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-279">hello following Python code snippet creates a user on each node in a pool, which is required for remote connection.</span></span> <span data-ttu-id="5a0c3-280">Majd megjeleníti a minden csomópont-hello secure shell (SSH) kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-280">It then prints hello secure shell (SSH) connection information for each node.</span></span>

```python
import datetime
import getpass
import azure.batch.batch_service_client as batch
import azure.batch.batch_auth as batchauth
import azure.batch.models as batchmodels

# Specify your own account credentials
batch_account_name = ''
batch_account_key = ''
batch_account_url = ''

# Specify hello ID of an existing pool containing Linux nodes
# currently in hello 'idle' state
pool_id = ''

# Specify hello username and prompt for a password
username = 'linuxuser'
password = getpass.getpass()

# Create a BatchClient
credentials = batchauth.SharedKeyCredentials(
    batch_account_name,
    batch_account_key
)
batch_client = batch.BatchServiceClient(
        credentials,
        base_url=batch_account_url
)

# Create hello user that will be added tooeach node in hello pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get hello list of nodes in hello pool
nodes = batch_client.compute_node.list(pool_id)

# Add hello user tooeach node in hello pool and print
# hello connection information for hello node
for node in nodes:
    # Add hello user toohello node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for hello node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print hello connection info for hello node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

<span data-ttu-id="5a0c3-281">Itt egy minta kimenet hello előző kód készlet, amely négy Linux csomópontokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="5a0c3-281">Here is sample output for hello previous code for a pool that contains four Linux nodes:</span></span>

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

<span data-ttu-id="5a0c3-282">A jelszó helyett nyilvános SSH-kulcs a felhasználó létrehozásakor, a csomópont is megadhat.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-282">Instead of a password, you can specify an SSH public key when you create a user on a node.</span></span> <span data-ttu-id="5a0c3-283">A Python SDK hello, használja a hello **ssh_public_key** paraméter [ComputeNodeUser][py_computenodeuser].</span><span class="sxs-lookup"><span data-stu-id="5a0c3-283">In hello Python SDK, use hello **ssh_public_key** parameter on [ComputeNodeUser][py_computenodeuser].</span></span> <span data-ttu-id="5a0c3-284">A .NET, használja a hello [ComputeNodeUser][net_computenodeuser].[ Az SshPublicKey] [ net_ssh_key] tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-284">In .NET, use hello [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] property.</span></span>

## <a name="pricing"></a><span data-ttu-id="5a0c3-285">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="5a0c3-285">Pricing</span></span>
<span data-ttu-id="5a0c3-286">Az Azure Batch Azure felhőalapú szolgáltatásairól és az Azure virtuális gépek technológiára épül.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-286">Azure Batch is built on Azure Cloud Services and Azure Virtual Machines technology.</span></span> <span data-ttu-id="5a0c3-287">hello Batch szolgáltatás tartományregisztráció ingyenesen, ami azt jelenti, hogy van szó, csak a hello számítási erőforrásokat, amelyek a kötegelt megoldások felhasználását.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-287">hello Batch service itself is offered at no cost, which means you are charged only for hello compute resources that your Batch solutions consume.</span></span> <span data-ttu-id="5a0c3-288">Ha úgy dönt, **Felhőszolgáltatások konfigurálása**, van szó, hello alapján [Felhőszolgáltatások árképzési] [ cloud_services_pricing] struktúra.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-288">When you choose **Cloud Services Configuration**, you are charged based on hello [Cloud Services pricing][cloud_services_pricing] structure.</span></span> <span data-ttu-id="5a0c3-289">Ha úgy dönt, **virtuálisgép-konfiguráció**, van szó, hello alapján [virtuális gépek díjszabása] [ vm_pricing] struktúra.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-289">When you choose **Virtual Machine Configuration**, you are charged based on hello [Virtual Machines pricing][vm_pricing] structure.</span></span> 

<span data-ttu-id="5a0c3-290">Ha alkalmazások tooyour kötegelt csomópontok használatával telepít [alkalmazáscsomagok](batch-application-packages.md), van is szó, a hello Azure Storage-erőforrások, hogy az alkalmazáscsomagok felhasználását.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-290">If you deploy applications tooyour Batch nodes using [application packages](batch-application-packages.md), you are also charged for hello Azure Storage resources that your application packages consume.</span></span> <span data-ttu-id="5a0c3-291">Általában hello Azure tárolási költségek minimálisak.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-291">In general, hello Azure Storage costs are minimal.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="5a0c3-292">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a0c3-292">Next steps</span></span>
### <a name="batch-python-tutorial"></a><span data-ttu-id="5a0c3-293">Python-útmutató a Batchhez</span><span class="sxs-lookup"><span data-stu-id="5a0c3-293">Batch Python tutorial</span></span>
<span data-ttu-id="5a0c3-294">Kapcsolatos részletesebb oktatóanyagért Python, kivételének használatával kötegelt toowork [Ismerkedés az Azure Batch Python ügyfél hello](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="5a0c3-294">For a more in-depth tutorial about how toowork with Batch by using Python, check out [Get started with hello Azure Batch Python client](batch-python-tutorial.md).</span></span> <span data-ttu-id="5a0c3-295">A kiegészítő [kódminta] [ github_samples_pyclient] egy segítő függvényt tartalmaz `get_vm_config_for_distro`, amely egy másik módszerrel tooobtain egy virtuálisgép-konfigurációját mutatja be.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-295">Its companion [code sample][github_samples_pyclient] includes a helper function, `get_vm_config_for_distro`, that shows another technique tooobtain a virtual machine configuration.</span></span>

### <a name="batch-python-code-samples"></a><span data-ttu-id="5a0c3-296">Kötegelt Python-Kódminták</span><span class="sxs-lookup"><span data-stu-id="5a0c3-296">Batch Python code samples</span></span>
<span data-ttu-id="5a0c3-297">Hello [Python Kódminták] [ github_samples_py] a hello [azure-köteg-minták] [ github_samples] GitHub tárházából tartalmaz olyan parancsfájlok, amelyek bemutatják, hogyan tooperform közös kötegelt műveletek, például a készletbe, a feladat és a feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-297">hello [Python code samples][github_samples_py] in hello [azure-batch-samples][github_samples] repository on GitHub contain scripts that show you how tooperform common Batch operations, such as pool, job, and task creation.</span></span> <span data-ttu-id="5a0c3-298">Hello [információs] [ github_py_readme] hello Python minták kíséri, hogy hogyan tooinstall hello szükséges csomagok kapcsolatos részleteket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-298">hello [README][github_py_readme] that accompanies hello Python samples has details about how tooinstall hello required packages.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="5a0c3-299">A Batch fóruma</span><span class="sxs-lookup"><span data-stu-id="5a0c3-299">Batch forum</span></span>
<span data-ttu-id="5a0c3-300">Hello [Azure Batch fórum] [ forum] MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-300">hello [Azure Batch Forum][forum] on MSDN is a great place toodiscuss Batch and ask questions about hello service.</span></span> <span data-ttu-id="5a0c3-301">Olvasási hasznos "rögzített" küldi, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.</span><span class="sxs-lookup"><span data-stu-id="5a0c3-301">Read helpful "pinned" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

[api_net]: http://msdn.microsoft.com/library/azure/mt348682.aspx
[api_net_mgmt]: https://msdn.microsoft.com/library/azure/mt463120.aspx
[api_rest]: http://msdn.microsoft.com/library/azure/dn820158.aspx
[cloud_services_pricing]: https://azure.microsoft.com/pricing/details/cloud-services/
[forum]: https://social.msdn.microsoft.com/forums/azure/en-US/home?forum=azurebatch
[github_py_readme]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/README.md
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_samples_py]: https://github.com/Azure/azure-batch-samples/tree/master/Python/Batch
[github_samples_pyclient]: https://github.com/Azure/azure-batch-samples/blob/master/Python/Batch/article_samples/python_tutorial_client.py
[portal]: https://portal.azure.com
[net_cloudpool]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.aspx
[net_computenodeuser]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.aspx
[net_imagereference]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.imagereference.aspx
[net_list_skus]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.listnodeagentskus.aspx
[net_pool_ops]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.aspx
[net_ssh_key]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenodeuser.sshpublickey.aspx
[nuget_batch_net]: https://www.nuget.org/packages/Azure.Batch/
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[py_account_ops]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations
[py_azure_sdk]: https://pypi.python.org/pypi/azure
[py_batch_docs]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.html
[py_batch_package]: https://pypi.python.org/pypi/azure-batch
[py_computenodeuser]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ComputeNodeUser
[py_imagereference]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.models.html#azure.batch.models.ImageReference
[py_list_skus]: http://azure-sdk-for-python.readthedocs.org/en/dev/ref/azure.batch.operations.html#azure.batch.operations.AccountOperations.list_node_agent_skus
[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/
[vm_pricing]: https://azure.microsoft.com/pricing/details/virtual-machines/
