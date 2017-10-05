---
title: "Futtatási Linux virtuális gép számítási csomópontok - Azure Batch |} Microsoft Docs"
description: "Útmutató: a Linux virtuális gépek az Azure Batch készleteinek a párhuzamos számítási feladatok feldolgozásához."
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
ms.openlocfilehash: 9b2257917e2368478beb75957677de23d4157865
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="provision-linux-compute-nodes-in-batch-pools"></a><span data-ttu-id="f8a6f-103">Linux számítási csomópontok kötegelt készletek kiépítése</span><span class="sxs-lookup"><span data-stu-id="f8a6f-103">Provision Linux compute nodes in Batch pools</span></span>

<span data-ttu-id="f8a6f-104">Azure Batch segítségével párhuzamos számítási feladatok futtatásához a Linux és a Windows virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-104">You can use Azure Batch to run parallel compute workloads on both Linux and Windows virtual machines.</span></span> <span data-ttu-id="f8a6f-105">Ez a cikk részletesen létrehozása Linux számítási csomópontok készleteinek a Batch szolgáltatás használatával is a [kötegelt Python] [ py_batch_package] és [Batch .NET] [ api_net]klienskódtárak segítségével.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-105">This article details how to create pools of Linux compute nodes in the Batch service by using both the [Batch Python][py_batch_package] and [Batch .NET][api_net] client libraries.</span></span>

> [!NOTE]
> <span data-ttu-id="f8a6f-106">Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-106">Application packages are supported on all Batch pools created after 5 July 2017.</span></span> <span data-ttu-id="f8a6f-107">A 2016. március 10. és 2017. július 5. között létrehozott Batch-készletek esetében csak akkor támogatottak, ha a készlet felhőszolgáltatás-konfigurációval lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-107">They are supported on Batch pools created between 10 March 2016 and 5 July 2017 only if the pool was created using a Cloud Service configuration.</span></span> <span data-ttu-id="f8a6f-108">A 2016. március 10. előtt létrehozott Batch-készletek nem támogatják az alkalmazáscsomagokat.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-108">Batch pools created prior to 10 March 2016 do not support application packages.</span></span> <span data-ttu-id="f8a6f-109">További információkat az alkalmazások a Batch-csomópontokon alkalmazáscsomagok használatával történő központi telepítéséről a [Batch-alkalmazáscsomagokkal számítási csomópontokra végzett alkalmazástelepítést](batch-application-packages.md) ismertető cikkben talál.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-109">For more information about using application packages to deploy your applications to your Batch nodes, see [Deploy applications to compute nodes with Batch application packages](batch-application-packages.md).</span></span>
>
>

## <a name="virtual-machine-configuration"></a><span data-ttu-id="f8a6f-110">Virtuálisgép-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="f8a6f-110">Virtual machine configuration</span></span>
<span data-ttu-id="f8a6f-111">Számítási csomópontok készletét kötegben létrehozásakor, amelyből a csomópont méretét és az operációs rendszer két választási lehetősége van: Felhő konfigurálása és a virtuális gép konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-111">When you create a pool of compute nodes in Batch, you have two options from which to select the node size and operating system: Cloud Services Configuration and Virtual Machine Configuration.</span></span>

<span data-ttu-id="f8a6f-112">A **Cloud Services-konfiguráció** *kizárólag* windowsos számítási csomópontok létrehozására használható.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-112">**Cloud Services Configuration** provides Windows compute nodes *only*.</span></span> <span data-ttu-id="f8a6f-113">Elérhető számítási csomópont méretek szereplő [Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md), és a rendelkezésre álló operációs rendszeren jelennek meg a [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](../cloud-services/cloud-services-guestos-update-matrix.md).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-113">Available compute node sizes are listed in [Sizes for Cloud Services](../cloud-services/cloud-services-sizes-specs.md), and available operating systems are listed in the [Azure Guest OS releases and SDK compatibility matrix](../cloud-services/cloud-services-guestos-update-matrix.md).</span></span> <span data-ttu-id="f8a6f-114">Azure Cloud Services csomópontok tartalmazó készletet hoz létre, amikor a csomópont méretének és az operációsrendszer-család a korábban említett cikkekben leírt adja meg.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-114">When you create a pool that contains Azure Cloud Services nodes, you specify the node size and the OS family, which are described in the previously mentioned articles.</span></span> <span data-ttu-id="f8a6f-115">A Windows készleteinek számítási csomópontokat, a Cloud Services leggyakrabban szolgál.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-115">For pools of Windows compute nodes, Cloud Services is most commonly used.</span></span>

<span data-ttu-id="f8a6f-116">**Virtuálisgép-konfiguráció** biztosít a Linux és a Windows lemezképek számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-116">**Virtual Machine Configuration** provides both Linux and Windows images for compute nodes.</span></span> <span data-ttu-id="f8a6f-117">Elérhető számítási csomópont méretek szereplő [az Azure virtuális gépek méretei](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) és [az Azure virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-117">Available compute node sizes are listed in [Sizes for virtual machines in Azure](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) and [Sizes for virtual machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows).</span></span> <span data-ttu-id="f8a6f-118">Amikor virtuálisgép-konfiguráció csomópontot tartalmazó készletet hoz létre, meg kell adnia a csomópontokat, a virtuális gép Képhivatkozás és a kötegelt csomópont ügynök SKU csomópontjain telepítendő méretét.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-118">When you create a pool that contains Virtual Machine Configuration nodes, you must specify the size of the nodes, the virtual machine image reference, and the Batch node agent SKU to be installed on the nodes.</span></span>

### <a name="virtual-machine-image-reference"></a><span data-ttu-id="f8a6f-119">Virtuális gép Képhivatkozás</span><span class="sxs-lookup"><span data-stu-id="f8a6f-119">Virtual machine image reference</span></span>
<span data-ttu-id="f8a6f-120">A Batch szolgáltatás által használt [virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) biztosításához Linux számítási csomópontjain.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-120">The Batch service uses [virtual machine scale sets](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) to provide Linux compute nodes.</span></span> <span data-ttu-id="f8a6f-121">Megadhatja, hogy a kép a [Azure piactér][vm_marketplace], vagy adjon meg egy előkészített egyéni lemezképet.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-121">You can specify an image from the [Azure Marketplace][vm_marketplace], or provide a custom image that you have prepared.</span></span> <span data-ttu-id="f8a6f-122">További részletek az egyéni rendszerképekről: [Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével](batch-api-basics.md#pool).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-122">For more details about custom images, see [Develop large-scale parallel compute solutions with Batch](batch-api-basics.md#pool).</span></span>

<span data-ttu-id="f8a6f-123">A virtuális gép Képhivatkozás konfigurálásakor adja meg a virtuálisgép-lemezkép tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-123">When you configure a virtual machine image reference, you specify the properties of the virtual machine image.</span></span> <span data-ttu-id="f8a6f-124">A következő tulajdonságok szükség, amikor egy virtuális gép Képhivatkozás létrehoz:</span><span class="sxs-lookup"><span data-stu-id="f8a6f-124">The following properties are required when you create a virtual machine image reference:</span></span>

| <span data-ttu-id="f8a6f-125">**Kép hivatkozás tulajdonságai**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-125">**Image reference properties**</span></span> | <span data-ttu-id="f8a6f-126">**Példa**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-126">**Example**</span></span> |
| --- | --- |
| <span data-ttu-id="f8a6f-127">Közzétevő</span><span class="sxs-lookup"><span data-stu-id="f8a6f-127">Publisher</span></span> |<span data-ttu-id="f8a6f-128">Canonical</span><span class="sxs-lookup"><span data-stu-id="f8a6f-128">Canonical</span></span> |
| <span data-ttu-id="f8a6f-129">Ajánlat</span><span class="sxs-lookup"><span data-stu-id="f8a6f-129">Offer</span></span> |<span data-ttu-id="f8a6f-130">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-130">UbuntuServer</span></span> |
| <span data-ttu-id="f8a6f-131">SKU</span><span class="sxs-lookup"><span data-stu-id="f8a6f-131">SKU</span></span> |<span data-ttu-id="f8a6f-132">14.04.4-LTS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-132">14.04.4-LTS</span></span> |
| <span data-ttu-id="f8a6f-133">Verzió</span><span class="sxs-lookup"><span data-stu-id="f8a6f-133">Version</span></span> |<span data-ttu-id="f8a6f-134">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-134">latest</span></span> |

> [!TIP]
> <span data-ttu-id="f8a6f-135">Ezeket a tulajdonságokat, és hogyan listázhat a piactéren elérhető rendszerkép kapcsolatos részletesebb [keresse meg és jelölje be Linux virtuális gép képfájljait az Azure-ban a parancssori felületen vagy a PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-135">You can learn more about these properties and how to list Marketplace images in [Navigate and select Linux virtual machine images in Azure with CLI or PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="f8a6f-136">Vegye figyelembe, hogy nem minden piactéren elérhető rendszerkép kompatibilisek jelenleg kötegelt.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-136">Note that not all Marketplace images are currently compatible with Batch.</span></span> <span data-ttu-id="f8a6f-137">További információkért lásd: [csomópont ügynök SKU](#node-agent-sku).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-137">For more information, see [Node agent SKU](#node-agent-sku).</span></span>
>
>

### <a name="node-agent-sku"></a><span data-ttu-id="f8a6f-138">Csomópont ügynök SKU</span><span class="sxs-lookup"><span data-stu-id="f8a6f-138">Node agent SKU</span></span>
<span data-ttu-id="f8a6f-139">A kötegelt csomópont ügynök egy olyan program, a készlet minden egyes csomópontján fut, és a parancs-és-ellenőrzés felületet, a csomópont és a Batch szolgáltatás között.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-139">The Batch node agent is a program that runs on each node in the pool and provides the command-and-control interface between the node and the Batch service.</span></span> <span data-ttu-id="f8a6f-140">Nincsenek a csomópont ügynök SKU, úgynevezett különböző operációs rendszerek különböző implementációja.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-140">There are different implementations of the node agent, known as SKUs, for different operating systems.</span></span> <span data-ttu-id="f8a6f-141">Alapvetően egy virtuálisgép-konfiguráció létrehozásakor először adja meg a virtuális gép képhivatkozás, és adja a csomópont ügynök, a lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-141">Essentially, when you create a Virtual Machine Configuration, you first specify the virtual machine image reference, and then you specify the node agent to install on the image.</span></span> <span data-ttu-id="f8a6f-142">Minden csomópont ügynök SKU általában több virtuálisgép-lemezkép kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-142">Typically, each node agent SKU is compatible with multiple virtual machine images.</span></span> <span data-ttu-id="f8a6f-143">Íme néhány példa a csomópont ügynök SKU:</span><span class="sxs-lookup"><span data-stu-id="f8a6f-143">Here are a few examples of node agent SKUs:</span></span>

* <span data-ttu-id="f8a6f-144">14.04 Batch.node.ubuntu</span><span class="sxs-lookup"><span data-stu-id="f8a6f-144">batch.node.ubuntu 14.04</span></span>
* <span data-ttu-id="f8a6f-145">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-145">batch.node.centos 7</span></span>
* <span data-ttu-id="f8a6f-146">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-146">batch.node.windows amd64</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8a6f-147">Nem minden virtuálisgép-rendszerképek a piactéren rendelkezésre álló kompatibilisek a jelenleg rendelkezésre álló kötegben csomópontjainak ügynökeit.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-147">Not all virtual machine images that are available in the Marketplace are compatible with the currently available Batch node agents.</span></span> <span data-ttu-id="f8a6f-148">A kötegelt SDK-k segítségével kilistázhatja a rendelkezésre álló csomópont ügynök SKU és a virtuálisgép-lemezképeket, amelyekhez kompatibilis.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-148">Use the Batch SDKs to list the available node agent SKUs and the virtual machine images with which they are compatible.</span></span> <span data-ttu-id="f8a6f-149">Tekintse meg a [lista a virtuálisgép-rendszerképek](#list-of-virtual-machine-images) további információt és példákat a futási időben érvényes képek listájának beolvasása az ebben a cikkben később.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-149">See the [List of Virtual Machine images](#list-of-virtual-machine-images) later in this article for more information and examples of how to retrieve a list of valid images at runtime.</span></span>
>
>

## <a name="create-a-linux-pool-batch-python"></a><span data-ttu-id="f8a6f-150">Linux-készlet létrehozása: kötegelt Python</span><span class="sxs-lookup"><span data-stu-id="f8a6f-150">Create a Linux pool: Batch Python</span></span>
<span data-ttu-id="f8a6f-151">A következő kódrészletet szemlélteti, hogyan használható a [Microsoft Azure Batch ügyféloldali kódtára a Pythonhoz] [ py_batch_package] Ubuntu Server készlet létrehozása a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-151">The following code snippet shows an example of how to use the [Microsoft Azure Batch Client Library for Python][py_batch_package] to create a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="f8a6f-152">A kötegelt Python modul ismertető dokumentációjában található [azure.batch csomag] [ py_batch_docs] a dokumentumok olvasáskor.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-152">Reference documentation for the Batch Python module can be found at [azure.batch package][py_batch_docs] on Read the Docs.</span></span>

<span data-ttu-id="f8a6f-153">Ezt a kódrészletet hoz létre egy [ImageReference] [ py_imagereference] explicit módon meghatározza az egyes tulajdonságát (közzétevő, offer, SKU, verziója).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-153">This snippet creates an [ImageReference][py_imagereference] explicitly and specifies each of its properties (publisher, offer, SKU, version).</span></span> <span data-ttu-id="f8a6f-154">Az éles kódban, azonban azt javasoljuk, hogy használja a [list_node_agent_skus] [ py_list_skus] metódus használatával állapítsa meg és jelölje ki a rendelkezésre álló kép és a csomópont ügynök SKU kombinációval futásidőben.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-154">In production code, however, we recommend that you use the [list_node_agent_skus][py_list_skus] method to determine and select from the available image and node agent SKU combinations at runtime.</span></span>

```python
# Import the required modules from the
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

# Initialize the Batch client
creds = batchauth.SharedKeyCredentials(account, key)
config = batch.BatchServiceClientConfiguration(creds, base_url = batch_url)
client = batch.BatchServiceClient(config)

# Create the unbound pool
new_pool = batchmodels.PoolAddParameter(id = pool_id, vm_size = vm_size)
new_pool.target_dedicated = node_count

# Configure the start task for the pool
start_task = batchmodels.StartTask()
start_task.run_elevated = True
start_task.command_line = "printenv AZ_BATCH_NODE_STARTUP_DIR"
new_pool.start_task = start_task

# Create an ImageReference which specifies the Marketplace
# virtual machine image to install on the nodes.
ir = batchmodels.ImageReference(
    publisher = "Canonical",
    offer = "UbuntuServer",
    sku = "14.04.2-LTS",
    version = "latest")

# Create the VirtualMachineConfiguration, specifying
# the VM image reference and the Batch node agent to
# be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = "batch.node.ubuntu 14.04")

# Assign the virtual machine configuration to the pool
new_pool.virtual_machine_configuration = vmc

# Create pool in the Batch service
client.pool.add(new_pool)
```

<span data-ttu-id="f8a6f-155">Ahogy korábban említettük, azt javasoljuk, hogy létrehozása helyett a [ImageReference] [ py_imagereference] explicit módon, használhatja a [list_node_agent_skus] [ py_list_skus] dinamikusan választhatnak a jelenleg támogatott csomópont ügynök/Piactéri lemezkép kombinációk metódust.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-155">As mentioned previously, we recommend that instead of creating the [ImageReference][py_imagereference] explicitly, you use the [list_node_agent_skus][py_list_skus] method to dynamically select from the currently supported node agent/Marketplace image combinations.</span></span> <span data-ttu-id="f8a6f-156">A következő Python részlet szemlélteti ezt a módszert használja.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-156">The following Python snippet shows how to use this method.</span></span>

```python
# Get the list of node agents from the Batch service
nodeagents = client.account.list_node_agent_skus()

# Obtain the desired node agent
ubuntu1404agent = next(agent for agent in nodeagents if "ubuntu 14.04" in agent.id)

# Pick the first image reference from the list of verified references
ir = ubuntu1404agent.verified_image_references[0]

# Create the VirtualMachineConfiguration, specifying the VM image
# reference and the Batch node agent to be installed on the node.
vmc = batchmodels.VirtualMachineConfiguration(
    image_reference = ir,
    node_agent_sku_id = ubuntu1404agent.id)
```

## <a name="create-a-linux-pool-batch-net"></a><span data-ttu-id="f8a6f-157">Linux-készlet létrehozása: Batch .NET</span><span class="sxs-lookup"><span data-stu-id="f8a6f-157">Create a Linux pool: Batch .NET</span></span>
<span data-ttu-id="f8a6f-158">A következő kódrészletet szemlélteti, hogyan használható a [Batch .NET] [ nuget_batch_net] ügyféloldali kódtár Ubuntu Server készlet létrehozása a számítási csomópontok.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-158">The following code snippet shows an example of how to use the [Batch .NET][nuget_batch_net] client library to create a pool of Ubuntu Server compute nodes.</span></span> <span data-ttu-id="f8a6f-159">Megtalálhatja az [Batch .NET referenciadokumentációt] [ api_net] az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-159">You can find the [Batch .NET reference documentation][api_net] on MSDN.</span></span>

<span data-ttu-id="f8a6f-160">A következő kódban részlet a [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] mód közül jelenleg támogatott Piactéri lemezkép-csomópont ügynök SKU kombinációkat.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-160">The following code snippet uses the [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method to select from the list of currently supported Marketplace image and node agent SKU combinations.</span></span> <span data-ttu-id="f8a6f-161">Ez a módszer nem kívánatos, mert a lista támogatott kombinációk időnként változhat.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-161">This technique is desirable because the list of supported combinations may change from time to time.</span></span> <span data-ttu-id="f8a6f-162">A leggyakrabban a támogatott kombinációk kerülnek.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-162">Most commonly, supported combinations are added.</span></span>

```csharp
// Pool settings
const string poolId = "LinuxNodesSamplePoolDotNet";
const string vmSize = "STANDARD_A1";
const int nodeCount = 1;

// Obtain a collection of all available node agent SKUs.
// This allows us to select from a list of supported
// VM image/node agent combinations.
List<NodeAgentSku> nodeAgentSkus =
    batchClient.PoolOperations.ListNodeAgentSkus().ToList();

// Define a delegate specifying properties of the VM image
// that we wish to use.
Func<ImageReference, bool> isUbuntu1404 = imageRef =>
    imageRef.Publisher == "Canonical" &&
    imageRef.Offer == "UbuntuServer" &&
    imageRef.Sku.Contains("14.04");

// Obtain the first node agent SKU in the collection that matches
// Ubuntu Server 14.04. Note that there are one or more image
// references associated with this node agent SKU.
NodeAgentSku ubuntuAgentSku = nodeAgentSkus.First(sku =>
    sku.VerifiedImageReferences.Any(isUbuntu1404));

// Select an ImageReference from those available for node agent.
ImageReference imageReference =
    ubuntuAgentSku.VerifiedImageReferences.First(isUbuntu1404);

// Create the VirtualMachineConfiguration for use when actually
// creating the pool
VirtualMachineConfiguration virtualMachineConfiguration =
    new VirtualMachineConfiguration(imageReference, ubuntuAgentSku.Id);

// Create the unbound pool object using the VirtualMachineConfiguration
// created above
CloudPool pool = batchClient.PoolOperations.CreatePool(
    poolId: poolId,
    virtualMachineSize: vmSize,
    virtualMachineConfiguration: virtualMachineConfiguration,
    targetDedicatedComputeNodes: nodeCount);

// Commit the pool to the Batch service
await pool.CommitAsync();
```

<span data-ttu-id="f8a6f-163">Bár az előző részlet használ a [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metódus dinamikus listában, és válassza ki a támogatott kép és a csomópont ügynök SKU kombinációk (ajánlott), beállíthatja úgy is egy [ImageReference] [ net_imagereference] explicit módon:</span><span class="sxs-lookup"><span data-stu-id="f8a6f-163">Although the previous snippet uses the [PoolOperations][net_pool_ops].[ListNodeAgentSkus][net_list_skus] method to dynamically list and select from supported image and node agent SKU combinations (recommended), you can also configure an [ImageReference][net_imagereference] explicitly:</span></span>

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a><span data-ttu-id="f8a6f-164">Virtuálisgép-rendszerképek listája</span><span class="sxs-lookup"><span data-stu-id="f8a6f-164">List of virtual machine images</span></span>
<span data-ttu-id="f8a6f-165">A következő táblázat a piactér virtuálisgép-lemezképeket, amelyek kompatibilisek a rendelkezésre álló kötegben csomópontjainak ügynökeit, ez a cikk legutóbbi frissítésekor.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-165">The following table lists the Marketplace virtual machine images that are compatible with the available Batch node agents when this article was last updated.</span></span> <span data-ttu-id="f8a6f-166">Fontos megjegyezni, hogy a lista létrehozási nem végleges mert képek és csomópontjainak ügynökeit előfordulhat, hogy hozzáadására vagy eltávolítására bármikor.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-166">It is important to note that this list is not definitive because images and node agents may be added or removed at any time.</span></span> <span data-ttu-id="f8a6f-167">Javasoljuk, hogy a Batch-alkalmazások és szolgáltatások mindig [list_node_agent_skus] [ py_list_skus] (Python) és [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) használatával állapítsa meg és válassza ki a jelenleg elérhető termékváltozatok.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-167">We recommend that your Batch applications and services always use [list_node_agent_skus][py_list_skus] (Python) and [ListNodeAgentSkus][net_list_skus] (Batch .NET) to determine and select from the currently available SKUs.</span></span>

> [!WARNING]
> <span data-ttu-id="f8a6f-168">Az alábbi lista bármikor módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-168">The following list may change at any time.</span></span> <span data-ttu-id="f8a6f-169">Mindig a **lista csomópont ügynök SKU** metódusok kompatibilis virtuális gép és csomópont ügynök SKU listáját, amikor a kötegelt feladatok futtatása a kötegelt API-k érhető el.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-169">Always use the **list node agent SKU** methods available in the Batch APIs to list the compatible virtual machine and node agent SKUs when you run your Batch jobs.</span></span>
>
>

| <span data-ttu-id="f8a6f-170">**Közzétevő**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-170">**Publisher**</span></span> | <span data-ttu-id="f8a6f-171">**Az ajánlat**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-171">**Offer**</span></span> | <span data-ttu-id="f8a6f-172">**Kép Termékváltozat**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-172">**Image SKU**</span></span> | <span data-ttu-id="f8a6f-173">**Verzió**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-173">**Version**</span></span> | <span data-ttu-id="f8a6f-174">**Csomópont ügynök SKU-azonosítója**</span><span class="sxs-lookup"><span data-stu-id="f8a6f-174">**Node agent SKU ID**</span></span> |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| <span data-ttu-id="f8a6f-175">Canonical</span><span class="sxs-lookup"><span data-stu-id="f8a6f-175">Canonical</span></span> | <span data-ttu-id="f8a6f-176">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-176">UbuntuServer</span></span> | <span data-ttu-id="f8a6f-177">14.04.5-LTS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-177">14.04.5-LTS</span></span> | <span data-ttu-id="f8a6f-178">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-178">latest</span></span> | <span data-ttu-id="f8a6f-179">14.04 Batch.node.ubuntu</span><span class="sxs-lookup"><span data-stu-id="f8a6f-179">batch.node.ubuntu 14.04</span></span> |
| <span data-ttu-id="f8a6f-180">Canonical</span><span class="sxs-lookup"><span data-stu-id="f8a6f-180">Canonical</span></span> | <span data-ttu-id="f8a6f-181">UbuntuServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-181">UbuntuServer</span></span> | <span data-ttu-id="f8a6f-182">16.04.0-LTS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-182">16.04.0-LTS</span></span> | <span data-ttu-id="f8a6f-183">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-183">latest</span></span> | <span data-ttu-id="f8a6f-184">Batch.node.ubuntu 16.04</span><span class="sxs-lookup"><span data-stu-id="f8a6f-184">batch.node.ubuntu 16.04</span></span> |
| <span data-ttu-id="f8a6f-185">Credativ</span><span class="sxs-lookup"><span data-stu-id="f8a6f-185">Credativ</span></span> | <span data-ttu-id="f8a6f-186">Debian</span><span class="sxs-lookup"><span data-stu-id="f8a6f-186">Debian</span></span> | <span data-ttu-id="f8a6f-187">8</span><span class="sxs-lookup"><span data-stu-id="f8a6f-187">8</span></span> | <span data-ttu-id="f8a6f-188">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-188">latest</span></span> | <span data-ttu-id="f8a6f-189">8 Batch.node.debian</span><span class="sxs-lookup"><span data-stu-id="f8a6f-189">batch.node.debian 8</span></span> |
| <span data-ttu-id="f8a6f-190">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="f8a6f-190">OpenLogic</span></span> | <span data-ttu-id="f8a6f-191">CentOS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-191">CentOS</span></span> | <span data-ttu-id="f8a6f-192">7.0</span><span class="sxs-lookup"><span data-stu-id="f8a6f-192">7.0</span></span> | <span data-ttu-id="f8a6f-193">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-193">latest</span></span> | <span data-ttu-id="f8a6f-194">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-194">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-195">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="f8a6f-195">OpenLogic</span></span> | <span data-ttu-id="f8a6f-196">CentOS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-196">CentOS</span></span> | <span data-ttu-id="f8a6f-197">7.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-197">7.1</span></span> | <span data-ttu-id="f8a6f-198">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-198">latest</span></span> | <span data-ttu-id="f8a6f-199">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-199">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-200">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="f8a6f-200">OpenLogic</span></span> | <span data-ttu-id="f8a6f-201">A HPC-centOS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-201">CentOS-HPC</span></span> | <span data-ttu-id="f8a6f-202">7.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-202">7.1</span></span> | <span data-ttu-id="f8a6f-203">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-203">latest</span></span> | <span data-ttu-id="f8a6f-204">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-204">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-205">OpenLogic</span><span class="sxs-lookup"><span data-stu-id="f8a6f-205">OpenLogic</span></span> | <span data-ttu-id="f8a6f-206">CentOS</span><span class="sxs-lookup"><span data-stu-id="f8a6f-206">CentOS</span></span> | <span data-ttu-id="f8a6f-207">7.2</span><span class="sxs-lookup"><span data-stu-id="f8a6f-207">7.2</span></span> | <span data-ttu-id="f8a6f-208">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-208">latest</span></span> | <span data-ttu-id="f8a6f-209">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-209">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-210">Oracle</span><span class="sxs-lookup"><span data-stu-id="f8a6f-210">Oracle</span></span> | <span data-ttu-id="f8a6f-211">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="f8a6f-211">Oracle-Linux</span></span> | <span data-ttu-id="f8a6f-212">7.0</span><span class="sxs-lookup"><span data-stu-id="f8a6f-212">7.0</span></span> | <span data-ttu-id="f8a6f-213">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-213">latest</span></span> | <span data-ttu-id="f8a6f-214">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-214">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-215">Oracle</span><span class="sxs-lookup"><span data-stu-id="f8a6f-215">Oracle</span></span> | <span data-ttu-id="f8a6f-216">Oracle Linux</span><span class="sxs-lookup"><span data-stu-id="f8a6f-216">Oracle-Linux</span></span> | <span data-ttu-id="f8a6f-217">7.2</span><span class="sxs-lookup"><span data-stu-id="f8a6f-217">7.2</span></span> | <span data-ttu-id="f8a6f-218">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-218">latest</span></span> | <span data-ttu-id="f8a6f-219">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-219">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-220">SUSE</span><span class="sxs-lookup"><span data-stu-id="f8a6f-220">SUSE</span></span> | <span data-ttu-id="f8a6f-221">openSUSE</span><span class="sxs-lookup"><span data-stu-id="f8a6f-221">openSUSE</span></span> | <span data-ttu-id="f8a6f-222">13.2</span><span class="sxs-lookup"><span data-stu-id="f8a6f-222">13.2</span></span> | <span data-ttu-id="f8a6f-223">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-223">latest</span></span> | <span data-ttu-id="f8a6f-224">Batch.node.opensuse 13.2</span><span class="sxs-lookup"><span data-stu-id="f8a6f-224">batch.node.opensuse 13.2</span></span> |
| <span data-ttu-id="f8a6f-225">SUSE</span><span class="sxs-lookup"><span data-stu-id="f8a6f-225">SUSE</span></span> | <span data-ttu-id="f8a6f-226">openSUSE-termékek</span><span class="sxs-lookup"><span data-stu-id="f8a6f-226">openSUSE-Leap</span></span> | <span data-ttu-id="f8a6f-227">42.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-227">42.1</span></span> | <span data-ttu-id="f8a6f-228">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-228">latest</span></span> | <span data-ttu-id="f8a6f-229">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-229">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="f8a6f-230">SUSE</span><span class="sxs-lookup"><span data-stu-id="f8a6f-230">SUSE</span></span> | <span data-ttu-id="f8a6f-231">SLES</span><span class="sxs-lookup"><span data-stu-id="f8a6f-231">SLES</span></span> | <span data-ttu-id="f8a6f-232">12-SP1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-232">12-SP1</span></span> | <span data-ttu-id="f8a6f-233">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-233">latest</span></span> | <span data-ttu-id="f8a6f-234">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-234">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="f8a6f-235">SUSE</span><span class="sxs-lookup"><span data-stu-id="f8a6f-235">SUSE</span></span> | <span data-ttu-id="f8a6f-236">SLES HPC</span><span class="sxs-lookup"><span data-stu-id="f8a6f-236">SLES-HPC</span></span> | <span data-ttu-id="f8a6f-237">12-SP1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-237">12-SP1</span></span> | <span data-ttu-id="f8a6f-238">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-238">latest</span></span> | <span data-ttu-id="f8a6f-239">Batch.node.opensuse 42.1</span><span class="sxs-lookup"><span data-stu-id="f8a6f-239">batch.node.opensuse 42.1</span></span> |
| <span data-ttu-id="f8a6f-240">Microsoft-ads</span><span class="sxs-lookup"><span data-stu-id="f8a6f-240">microsoft-ads</span></span> | <span data-ttu-id="f8a6f-241">Linux-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="f8a6f-241">linux-data-science-vm</span></span> | <span data-ttu-id="f8a6f-242">linuxdsvm</span><span class="sxs-lookup"><span data-stu-id="f8a6f-242">linuxdsvm</span></span> | <span data-ttu-id="f8a6f-243">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-243">latest</span></span> | <span data-ttu-id="f8a6f-244">Batch.node.centos 7</span><span class="sxs-lookup"><span data-stu-id="f8a6f-244">batch.node.centos 7</span></span> |
| <span data-ttu-id="f8a6f-245">Microsoft-ads</span><span class="sxs-lookup"><span data-stu-id="f8a6f-245">microsoft-ads</span></span> | <span data-ttu-id="f8a6f-246">Standard-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="f8a6f-246">standard-data-science-vm</span></span> | <span data-ttu-id="f8a6f-247">Standard-adatok-tudományos-vm</span><span class="sxs-lookup"><span data-stu-id="f8a6f-247">standard-data-science-vm</span></span> | <span data-ttu-id="f8a6f-248">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-248">latest</span></span> | <span data-ttu-id="f8a6f-249">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-249">batch.node.windows amd64</span></span> |
| <span data-ttu-id="f8a6f-250">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-250">MicrosoftWindowsServer</span></span> | <span data-ttu-id="f8a6f-251">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-251">WindowsServer</span></span> | <span data-ttu-id="f8a6f-252">2008 R2 SP1 CSOMAG</span><span class="sxs-lookup"><span data-stu-id="f8a6f-252">2008-R2-SP1</span></span> | <span data-ttu-id="f8a6f-253">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-253">latest</span></span> | <span data-ttu-id="f8a6f-254">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-254">batch.node.windows amd64</span></span> |
| <span data-ttu-id="f8a6f-255">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-255">MicrosoftWindowsServer</span></span> | <span data-ttu-id="f8a6f-256">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-256">WindowsServer</span></span> | <span data-ttu-id="f8a6f-257">2012-Datacenter</span><span class="sxs-lookup"><span data-stu-id="f8a6f-257">2012-Datacenter</span></span> | <span data-ttu-id="f8a6f-258">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-258">latest</span></span> | <span data-ttu-id="f8a6f-259">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-259">batch.node.windows amd64</span></span> |
| <span data-ttu-id="f8a6f-260">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-260">MicrosoftWindowsServer</span></span> | <span data-ttu-id="f8a6f-261">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-261">WindowsServer</span></span> | <span data-ttu-id="f8a6f-262">2012-R2-Datacenter</span><span class="sxs-lookup"><span data-stu-id="f8a6f-262">2012-R2-Datacenter</span></span> | <span data-ttu-id="f8a6f-263">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-263">latest</span></span> | <span data-ttu-id="f8a6f-264">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-264">batch.node.windows amd64</span></span> |
| <span data-ttu-id="f8a6f-265">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-265">MicrosoftWindowsServer</span></span> | <span data-ttu-id="f8a6f-266">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-266">WindowsServer</span></span> | <span data-ttu-id="f8a6f-267">2016-Datacenter</span><span class="sxs-lookup"><span data-stu-id="f8a6f-267">2016-Datacenter</span></span> | <span data-ttu-id="f8a6f-268">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-268">latest</span></span> | <span data-ttu-id="f8a6f-269">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-269">batch.node.windows amd64</span></span> |
| <span data-ttu-id="f8a6f-270">MicrosoftWindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-270">MicrosoftWindowsServer</span></span> | <span data-ttu-id="f8a6f-271">WindowsServer</span><span class="sxs-lookup"><span data-stu-id="f8a6f-271">WindowsServer</span></span> | <span data-ttu-id="f8a6f-272">2016-adatközpont-az-tárolók</span><span class="sxs-lookup"><span data-stu-id="f8a6f-272">2016-Datacenter-with-Containers</span></span> | <span data-ttu-id="f8a6f-273">legújabb</span><span class="sxs-lookup"><span data-stu-id="f8a6f-273">latest</span></span> | <span data-ttu-id="f8a6f-274">Batch.node.Windows amd64</span><span class="sxs-lookup"><span data-stu-id="f8a6f-274">batch.node.windows amd64</span></span> |

## <a name="connect-to-linux-nodes-using-ssh"></a><span data-ttu-id="f8a6f-275">Csatlakozzon SSH használt Linux-csomópontok</span><span class="sxs-lookup"><span data-stu-id="f8a6f-275">Connect to Linux nodes using SSH</span></span>
<span data-ttu-id="f8a6f-276">A fejlesztés során, vagy a hibaelhárítás során szükség lehet arra a készlet csomópontjain bejelentkezéshez szükséges.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-276">During development or while troubleshooting, you may find it necessary to sign in to the nodes in your pool.</span></span> <span data-ttu-id="f8a6f-277">Windows számítási csomópontokat, eltérően Remote Desktop Protocol (RDP) nem használható Linux csomópontok való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-277">Unlike Windows compute nodes, you cannot use Remote Desktop Protocol (RDP) to connect to Linux nodes.</span></span> <span data-ttu-id="f8a6f-278">Ehelyett a Batch szolgáltatás lehetővé teszi, hogy a távoli kapcsolat minden egyes csomóponton SSH-elérést.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-278">Instead, the Batch service enables SSH access on each node for remote connection.</span></span>

<span data-ttu-id="f8a6f-279">A következő Python kódrészletet felhasználót hoz létre a készletbe, amely pedig szükséges a távoli kapcsolat minden egyes csomóponton.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-279">The following Python code snippet creates a user on each node in a pool, which is required for remote connection.</span></span> <span data-ttu-id="f8a6f-280">Hogy majd kiírja a secure shell (SSH) minden csomópont-kapcsolódási információt.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-280">It then prints the secure shell (SSH) connection information for each node.</span></span>

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

# Specify the ID of an existing pool containing Linux nodes
# currently in the 'idle' state
pool_id = ''

# Specify the username and prompt for a password
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

# Create the user that will be added to each node in the pool
user = batchmodels.ComputeNodeUser(username)
user.password = password
user.is_admin = True
user.expiry_time = \
    (datetime.datetime.today() + datetime.timedelta(days=30)).isoformat()

# Get the list of nodes in the pool
nodes = batch_client.compute_node.list(pool_id)

# Add the user to each node in the pool and print
# the connection information for the node
for node in nodes:
    # Add the user to the node
    batch_client.compute_node.add_user(pool_id, node.id, user)

    # Obtain SSH login information for the node
    login = batch_client.compute_node.get_remote_login_settings(pool_id,
                                                                node.id)

    # Print the connection info for the node
    print("{0} | {1} | {2} | {3}".format(node.id,
                                         node.state,
                                         login.remote_login_ip_address,
                                         login.remote_login_port))
```

<span data-ttu-id="f8a6f-281">Itt egy minta kimenet az előző kód készlet, amely négy Linux csomópontokat tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="f8a6f-281">Here is sample output for the previous code for a pool that contains four Linux nodes:</span></span>

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

<span data-ttu-id="f8a6f-282">A jelszó helyett nyilvános SSH-kulcs a felhasználó létrehozásakor, a csomópont is megadhat.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-282">Instead of a password, you can specify an SSH public key when you create a user on a node.</span></span> <span data-ttu-id="f8a6f-283">A Python SDK használatára a **ssh_public_key** paraméter [ComputeNodeUser][py_computenodeuser].</span><span class="sxs-lookup"><span data-stu-id="f8a6f-283">In the Python SDK, use the **ssh_public_key** parameter on [ComputeNodeUser][py_computenodeuser].</span></span> <span data-ttu-id="f8a6f-284">A .NET, használja a [ComputeNodeUser][net_computenodeuser].[ Az SshPublicKey] [ net_ssh_key] tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-284">In .NET, use the [ComputeNodeUser][net_computenodeuser].[SshPublicKey][net_ssh_key] property.</span></span>

## <a name="pricing"></a><span data-ttu-id="f8a6f-285">Díjszabás</span><span class="sxs-lookup"><span data-stu-id="f8a6f-285">Pricing</span></span>
<span data-ttu-id="f8a6f-286">Az Azure Batch Azure felhőalapú szolgáltatásairól és az Azure virtuális gépek technológiára épül.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-286">Azure Batch is built on Azure Cloud Services and Azure Virtual Machines technology.</span></span> <span data-ttu-id="f8a6f-287">A Batch szolgáltatás tartományregisztráció ingyenesen, ami azt jelenti, hogy van szó, csak a számítási erőforrások, hogy a kötegelt megoldások felhasználását.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-287">The Batch service itself is offered at no cost, which means you are charged only for the compute resources that your Batch solutions consume.</span></span> <span data-ttu-id="f8a6f-288">Ha úgy dönt, **Felhőszolgáltatások konfigurálása**, alapján van szó a [Felhőszolgáltatások árképzési] [ cloud_services_pricing] struktúra.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-288">When you choose **Cloud Services Configuration**, you are charged based on the [Cloud Services pricing][cloud_services_pricing] structure.</span></span> <span data-ttu-id="f8a6f-289">Ha úgy dönt, **virtuálisgép-konfiguráció**, alapján van szó a [virtuális gépek díjszabása] [ vm_pricing] struktúra.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-289">When you choose **Virtual Machine Configuration**, you are charged based on the [Virtual Machines pricing][vm_pricing] structure.</span></span> 

<span data-ttu-id="f8a6f-290">Ha a központi telepítés a Batch-csomópontokat használja [alkalmazáscsomagok](batch-application-packages.md), van is szó, az Azure Storage-erőforrások, hogy az alkalmazáscsomagok felhasználását.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-290">If you deploy applications to your Batch nodes using [application packages](batch-application-packages.md), you are also charged for the Azure Storage resources that your application packages consume.</span></span> <span data-ttu-id="f8a6f-291">Általában az Azure Storage-költségeket minimálisak.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-291">In general, the Azure Storage costs are minimal.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="f8a6f-292">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="f8a6f-292">Next steps</span></span>
### <a name="batch-python-tutorial"></a><span data-ttu-id="f8a6f-293">Python-útmutató a Batchhez</span><span class="sxs-lookup"><span data-stu-id="f8a6f-293">Batch Python tutorial</span></span>
<span data-ttu-id="f8a6f-294">Részletesebb oktatóanyag használatával kötegelt Python használatával kapcsolatban, tekintse meg [Ismerkedés az Azure Batch Python-ügyfél a](batch-python-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="f8a6f-294">For a more in-depth tutorial about how to work with Batch by using Python, check out [Get started with the Azure Batch Python client](batch-python-tutorial.md).</span></span> <span data-ttu-id="f8a6f-295">A kiegészítő [kódminta] [ github_samples_pyclient] egy segítő függvényt tartalmaz `get_vm_config_for_distro`, amely bemutatja, hogy egy másik módszer a virtuális gép konfigurációjának.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-295">Its companion [code sample][github_samples_pyclient] includes a helper function, `get_vm_config_for_distro`, that shows another technique to obtain a virtual machine configuration.</span></span>

### <a name="batch-python-code-samples"></a><span data-ttu-id="f8a6f-296">Kötegelt Python-Kódminták</span><span class="sxs-lookup"><span data-stu-id="f8a6f-296">Batch Python code samples</span></span>
<span data-ttu-id="f8a6f-297">A [Python Kódminták] [ github_samples_py] a a [azure-köteg-minták] [ github_samples] GitHub tárházából parancsfájlok, amelyek bemutatják, hogyan hajthat végre tartalmaz közös kötegelt műveletek, például a készletbe, a feladat és a feladat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-297">The [Python code samples][github_samples_py] in the [azure-batch-samples][github_samples] repository on GitHub contain scripts that show you how to perform common Batch operations, such as pool, job, and task creation.</span></span> <span data-ttu-id="f8a6f-298">A [információs] [ github_py_readme] , amely a Python kísérik minták rendelkezik a szükséges csomagokat telepítésével kapcsolatos adatokat.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-298">The [README][github_py_readme] that accompanies the Python samples has details about how to install the required packages.</span></span>

### <a name="batch-forum"></a><span data-ttu-id="f8a6f-299">A Batch fóruma</span><span class="sxs-lookup"><span data-stu-id="f8a6f-299">Batch forum</span></span>
<span data-ttu-id="f8a6f-300">A [Azure Batch fórum] [ forum] az MSDN webhelyen van remek kötegelt tárgyalja, és kérdése van a szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-300">The [Azure Batch Forum][forum] on MSDN is a great place to discuss Batch and ask questions about the service.</span></span> <span data-ttu-id="f8a6f-301">Olvasási hasznos "rögzített" küldi, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.</span><span class="sxs-lookup"><span data-stu-id="f8a6f-301">Read helpful "pinned" posts, and post your questions as they arise while you build your Batch solutions.</span></span>

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
