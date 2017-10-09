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
# <a name="provision-linux-compute-nodes-in-batch-pools"></a>Linux számítási csomópontok kötegelt készletek kiépítése

A Linux és a Windows virtuális gépek Azure Batch toorun párhuzamos számítási feladatok is használhatja. Ez a cikk részletesen hogyan Linux toocreate készleteinek számítási mindkét hello segítségével hello Batch szolgáltatás csomópontjának [kötegelt Python] [ py_batch_package] és [Batch .NET] [ api_net] klienskódtárak segítségével.

> [!NOTE]
> Az alkalmazáscsomagok az összes 2017. július 5. után létrehozott Batch-készleten támogatottak. 2016. március 10. és 5. július 2017 között létrehozott, csak akkor, ha hello készlet lett létrehozva egy felhőalapú szolgáltatás konfigurációja kötegelt készletek azok támogatottak. Kötegelt készletek létrehozott előzetes too10 2016. március nem támogatják az alkalmazáscsomagok. Alkalmazás használatával kapcsolatos további információk a toodeploy az alkalmazások tooyour kötegelt csomópontok csomagokat, a következő témakörben: [központi telepítése az alkalmazások toocompute csomópontokat a kötegelt alkalmazáscsomagok](batch-application-packages.md).
>
>

## <a name="virtual-machine-configuration"></a>Virtuálisgép-konfiguráció
Számítási csomópontok készletét kötegben létrehozásakor mely tooselect hello csomópont méretét és az operációs rendszer két választási lehetősége van: Felhő konfigurálása és a virtuális gép konfigurációját.

A **Cloud Services-konfiguráció** *kizárólag* windowsos számítási csomópontok létrehozására használható. Elérhető számítási csomópont méretek szereplő [Felhőszolgáltatások mérete](../cloud-services/cloud-services-sizes-specs.md), és a rendelkezésre álló operációs rendszeren hello szereplő [Azure vendég operációs rendszereinek kiadásait és SDK-kompatibilitási mátrixát](../cloud-services/cloud-services-guestos-update-matrix.md). Azure Cloud Services csomópontok tartalmazó készletet hoz létre, amikor hello csomópont méretet ad meg, és hello operációsrendszer-család hello ismerteti a korábban említett cikkeket. A Windows készleteinek számítási csomópontokat, a Cloud Services leggyakrabban szolgál.

**Virtuálisgép-konfiguráció** biztosít a Linux és a Windows lemezképek számítási csomópontjain. Elérhető számítási csomópont méretek szereplő [az Azure virtuális gépek méretei](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Linux) és [az Azure virtuális gépek méretei](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Windows). Amikor virtuálisgép-konfiguráció csomópontot tartalmazó készletet hoz létre, meg kell adnia hello mérete hello csomópontok hello a virtuális gép Képhivatkozás és hello kötegelt csomópont ügynök SKU toobe hello csomópontjára telepítve.

### <a name="virtual-machine-image-reference"></a>Virtuális gép Képhivatkozás
Batch szolgáltatás által használt hello [virtuálisgép-méretezési csoportok](../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) tooprovide Linux számítási csomópontjain. Megadhat egy lemezképét hello [Azure piactér][vm_marketplace], vagy adjon meg egy előkészített egyéni lemezképet. További részletek az egyéni rendszerképekről: [Nagy léptékű párhuzamos számítási megoldások fejlesztése a Batch segítségével](batch-api-basics.md#pool).

A virtuális gép Képhivatkozás konfigurálásakor meg kell adnia hello virtuálisgép-lemezkép hello tulajdonságait. következő tulajdonságai hello szükség, amikor egy virtuális gép Képhivatkozás létrehoz:

| **Kép hivatkozás tulajdonságai** | **Példa** |
| --- | --- |
| Közzétevő |Canonical |
| Ajánlat |UbuntuServer |
| SKU |14.04.4-LTS |
| Verzió |legújabb |

> [!TIP]
> Ezeket a tulajdonságokat, és hogyan toolist piactér a kép részletesebb [keresse meg és jelölje be Linux virtuális gép képfájljait az Azure-ban a parancssori felületen vagy a PowerShell](../virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Vegye figyelembe, hogy nem minden piactéren elérhető rendszerkép kompatibilisek jelenleg kötegelt. További információkért lásd: [csomópont ügynök SKU](#node-agent-sku).
>
>

### <a name="node-agent-sku"></a>Csomópont ügynök SKU
hello kötegelt csomópont ügynök egy olyan program, hello készlet minden egyes csomópontján fut, és hello parancs vezérlő és felületet hello csomópont és hello Batch szolgáltatás között. Nincsenek hello csomópont ügynök SKU, úgynevezett különböző operációs rendszerek különböző implementációja. Alapvetően egy virtuálisgép-konfiguráció létrehozásakor hello a virtuális gép Képhivatkozás először adja meg, és hello rendszerképre hello csomópont ügynök tooinstall adja. Minden csomópont ügynök SKU általában több virtuálisgép-lemezkép kompatibilis. Íme néhány példa a csomópont ügynök SKU:

* 14.04 Batch.node.ubuntu
* Batch.node.centos 7
* Batch.node.Windows amd64

> [!IMPORTANT]
> Nem minden hello piactéren elérhető virtuálisgép-rendszerképek hello jelenleg elérhető kötegelt csomópontjainak ügynökeit kompatibilisek. Hello kötegelt SDK-k toolist hello rendelkezésre álló csomópont ügynök SKU használja, és a virtuálisgép-lemezképeket, amelyekhez kompatibilis hello. Lásd: hello [lista a virtuálisgép-lemezképeket](#list-of-virtual-machine-images) további információt és példákat az ebben a cikkben később tooretrieve futási időben érvényes lemezképek listáját.
>
>

## <a name="create-a-linux-pool-batch-python"></a>Linux-készlet létrehozása: kötegelt Python
hello következő kódrészletet szemlélteti, hogyan toouse hello [Microsoft Azure Batch ügyféloldali kódtára a Pythonhoz] [ py_batch_package] toocreate Ubuntu Server készlet számítási csomópontjain. A kötegelt Python modul technológiáról: hello dokumentáció [azure.batch csomag] [ py_batch_docs] az olvasási hello Docs.

Ezt a kódrészletet hoz létre egy [ImageReference] [ py_imagereference] explicit módon meghatározza az egyes tulajdonságát (közzétevő, offer, SKU, verziója). Az éles kódban, azonban javasoljuk, hogy használjon hello [list_node_agent_skus] [ py_list_skus] metódus toodetermine és válassza ki a hello elérhető rendszerkép és csomópont ügynök SKU kombinációit futásidőben.

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

Ahogy korábban említettük, azt javasoljuk, hogy hello létrehozása helyett [ImageReference] [ py_imagereference] explicit módon, használja a hello [list_node_agent_skus] [ py_list_skus] metódus toodynamically válassza ki a hello támogatott csomópont ügynök/Piactéri lemezkép kiegészítve. Hogyan Python kódrészletet mutat be a következő hello toouse ezt a módszert.

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

## <a name="create-a-linux-pool-batch-net"></a>Linux-készlet létrehozása: Batch .NET
hello következő kódrészletet szemlélteti, hogyan toouse hello [Batch .NET] [ nuget_batch_net] ügyfél könyvtár toocreate Ubuntu Server egyes számítási csomópontjain. Hello található [Batch .NET referenciadokumentációt] [ api_net] az MSDN Webhelyén.

hello következő kódrészletet használja hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] metódus tooselect jelenleg támogatott Piactéri lemezkép és a csomópont ügynök SKU kombinációk hello listája. Ezzel a módszerrel nem kívánatos, mert a támogatott kombinációk hello listája idő tootime változhat. A leggyakrabban a támogatott kombinációk kerülnek.

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

Bár hello előző részlet hello [PoolOperations][net_pool_ops].[ ListNodeAgentSkus] [ net_list_skus] módszer toodynamically listából, és válassza ki a támogatott kép és a csomópont ügynök SKU kombinációk (ajánlott), beállíthatja úgy is egy [ImageReference] [ net_imagereference] explicit módon:

```csharp
ImageReference imageReference = new ImageReference(
    publisher: "Canonical",
    offer: "UbuntuServer",
    sku: "14.04.2-LTS",
    version: "latest");
```

## <a name="list-of-virtual-machine-images"></a>Virtuálisgép-rendszerképek listája
hello következő táblázatban hello piactér virtuálisgép-lemezképeket, amelyek kompatibilisek a hello elérhető kötegelt csomópontjainak ügynökeit, ez a cikk legutóbbi frissítésekor. Fontos, hogy a lista létrehozási nem végleges mert képek és csomópontjainak ügynökeit előfordulhat, hogy hozzáadására vagy eltávolítására bármikor toonote. Javasoljuk, hogy a Batch-alkalmazások és szolgáltatások mindig [list_node_agent_skus] [ py_list_skus] (Python) és [ListNodeAgentSkus] [ net_list_skus] (Batch .NET) toodetermine, és válassza ki a hello jelenleg elérhető termékváltozatok.

> [!WARNING]
> a következő lista hello bármikor módosíthatja. Mindig használjon hello **lista csomópont ügynök SKU** módszer áll rendelkezésre a hello kötegelt API-k toolist hello kompatibilis virtuális gép és a csomópont ügynök SKU a kötegelt feladatok futtatásakor.
>
>

| **Közzétevő** | **Az ajánlat** | **Kép Termékváltozat** | **Verzió** | **Csomópont ügynök SKU-azonosítója** |
| ------------- | --------- | ------------- | ----------- | --------------------- |
| Canonical | UbuntuServer | 14.04.5-LTS | legújabb | 14.04 Batch.node.ubuntu |
| Canonical | UbuntuServer | 16.04.0-LTS | legújabb | Batch.node.ubuntu 16.04 |
| Credativ | Debian | 8 | legújabb | 8 Batch.node.debian |
| OpenLogic | CentOS | 7.0 | legújabb | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.1 | legújabb | Batch.node.centos 7 |
| OpenLogic | A HPC-centOS | 7.1 | legújabb | Batch.node.centos 7 |
| OpenLogic | CentOS | 7.2 | legújabb | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.0 | legújabb | Batch.node.centos 7 |
| Oracle | Oracle Linux | 7.2 | legújabb | Batch.node.centos 7 |
| SUSE | openSUSE | 13.2 | legújabb | Batch.node.opensuse 13.2 |
| SUSE | openSUSE-termékek | 42.1 | legújabb | Batch.node.opensuse 42.1 |
| SUSE | SLES | 12-SP1 | legújabb | Batch.node.opensuse 42.1 |
| SUSE | SLES HPC | 12-SP1 | legújabb | Batch.node.opensuse 42.1 |
| Microsoft-ads | Linux-adatok-tudományos-vm | linuxdsvm | legújabb | Batch.node.centos 7 |
| Microsoft-ads | Standard-adatok-tudományos-vm | Standard-adatok-tudományos-vm | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2008 R2 SP1 CSOMAG | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-Datacenter | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2012-R2-Datacenter | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-Datacenter | legújabb | Batch.node.Windows amd64 |
| MicrosoftWindowsServer | WindowsServer | 2016-adatközpont-az-tárolók | legújabb | Batch.node.Windows amd64 |

## <a name="connect-toolinux-nodes-using-ssh"></a>Csatlakozzon az SSH használatával tooLinux csomópontok
A fejlesztés során, vagy a hibaelhárítás során azt tapasztalhatja, szükséges toosign toohello csomópontok a készlethez. Windows számítási csomópontokat, ellentétben a távoli asztal protokoll (RDP) tooconnect tooLinux csomópontok nem használhat. Ehelyett hello Batch szolgáltatás lehetővé teszi, hogy a távoli kapcsolat minden egyes csomóponton SSH-elérést.

hello következő Python kódrészletet hoz létre a felhasználó a készletbe, amely pedig szükséges a távoli kapcsolat minden egyes csomóponton. Majd megjeleníti a minden csomópont-hello secure shell (SSH) kapcsolódási információt.

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

Itt egy minta kimenet hello előző kód készlet, amely négy Linux csomópontokat tartalmazza:

```
Password:
tvm-1219235766_1-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50000
tvm-1219235766_2-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50003
tvm-1219235766_3-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50002
tvm-1219235766_4-20160414t192511z | ComputeNodeState.idle | 13.91.7.57 | 50001
```

A jelszó helyett nyilvános SSH-kulcs a felhasználó létrehozásakor, a csomópont is megadhat. A Python SDK hello, használja a hello **ssh_public_key** paraméter [ComputeNodeUser][py_computenodeuser]. A .NET, használja a hello [ComputeNodeUser][net_computenodeuser].[ Az SshPublicKey] [ net_ssh_key] tulajdonság.

## <a name="pricing"></a>Díjszabás
Az Azure Batch Azure felhőalapú szolgáltatásairól és az Azure virtuális gépek technológiára épül. hello Batch szolgáltatás tartományregisztráció ingyenesen, ami azt jelenti, hogy van szó, csak a hello számítási erőforrásokat, amelyek a kötegelt megoldások felhasználását. Ha úgy dönt, **Felhőszolgáltatások konfigurálása**, van szó, hello alapján [Felhőszolgáltatások árképzési] [ cloud_services_pricing] struktúra. Ha úgy dönt, **virtuálisgép-konfiguráció**, van szó, hello alapján [virtuális gépek díjszabása] [ vm_pricing] struktúra. 

Ha alkalmazások tooyour kötegelt csomópontok használatával telepít [alkalmazáscsomagok](batch-application-packages.md), van is szó, a hello Azure Storage-erőforrások, hogy az alkalmazáscsomagok felhasználását. Általában hello Azure tárolási költségek minimálisak. 

## <a name="next-steps"></a>Következő lépések
### <a name="batch-python-tutorial"></a>Python-útmutató a Batchhez
Kapcsolatos részletesebb oktatóanyagért Python, kivételének használatával kötegelt toowork [Ismerkedés az Azure Batch Python ügyfél hello](batch-python-tutorial.md). A kiegészítő [kódminta] [ github_samples_pyclient] egy segítő függvényt tartalmaz `get_vm_config_for_distro`, amely egy másik módszerrel tooobtain egy virtuálisgép-konfigurációját mutatja be.

### <a name="batch-python-code-samples"></a>Kötegelt Python-Kódminták
Hello [Python Kódminták] [ github_samples_py] a hello [azure-köteg-minták] [ github_samples] GitHub tárházából tartalmaz olyan parancsfájlok, amelyek bemutatják, hogyan tooperform közös kötegelt műveletek, például a készletbe, a feladat és a feladat létrehozása. Hello [információs] [ github_py_readme] hello Python minták kíséri, hogy hogyan tooinstall hello szükséges csomagok kapcsolatos részleteket tartalmaz.

### <a name="batch-forum"></a>A Batch fóruma
Hello [Azure Batch fórum] [ forum] MSDN nagyszerű toodiscuss kötegelt helyezze, és hello szolgáltatás kérdése. Olvasási hasznos "rögzített" küldi, és a kötegelt megoldások létrehozása során felmerülő kérdéseit.

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
