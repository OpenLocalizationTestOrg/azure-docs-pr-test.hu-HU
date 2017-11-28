---
title: "aaaCreate és egy Windows virtuális Gépet az Azure-ban, Python kezelése |} Microsoft Docs"
description: "Ismerje meg, toouse Python toocreate és kezelése a Windows Azure-ban."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/22/2017
ms.author: davidmu
ms.openlocfilehash: c5553e4e7361e6b9a7183cd935be382f967160cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a><span data-ttu-id="5a683-103">Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban, Python</span><span class="sxs-lookup"><span data-stu-id="5a683-103">Create and manage Windows VMs in Azure using Python</span></span>

<span data-ttu-id="5a683-104">Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell.</span><span class="sxs-lookup"><span data-stu-id="5a683-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="5a683-105">Ez a cikk ismerteti a létrehozása, kezelése és Python Virtuálisgép-erőforrások törlése.</span><span class="sxs-lookup"><span data-stu-id="5a683-105">This article covers creating, managing, and deleting VM resources using Python.</span></span> <span data-ttu-id="5a683-106">Az alábbiak végrehajtásának módját ismerheti meg:</span><span class="sxs-lookup"><span data-stu-id="5a683-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5a683-107">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="5a683-108">Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="5a683-108">Install packages</span></span>
> * <span data-ttu-id="5a683-109">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-109">Create credentials</span></span>
> * <span data-ttu-id="5a683-110">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-110">Create resources</span></span>
> * <span data-ttu-id="5a683-111">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="5a683-111">Perform management tasks</span></span>
> * <span data-ttu-id="5a683-112">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="5a683-112">Delete resources</span></span>
> * <span data-ttu-id="5a683-113">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="5a683-113">Run hello application</span></span>

<span data-ttu-id="5a683-114">Körülbelül 20 percet toodo szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5a683-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="5a683-115">Visual Studio-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="5a683-116">Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="5a683-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="5a683-117">Válassza ki **Python fejlesztői** hello munkaterhelések lap, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="5a683-117">Select **Python development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="5a683-118">Az összefoglaló hello, láthatja, hogy **Python 3 64 bites (3.6.0)** automatikusan ki van jelölve meg.</span><span class="sxs-lookup"><span data-stu-id="5a683-118">In hello summary, you can see that **Python 3 64-bit (3.6.0)** is automatically selected for you.</span></span> <span data-ttu-id="5a683-119">Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello Python munkaterhelés is hozzáadhat.</span><span class="sxs-lookup"><span data-stu-id="5a683-119">If you have already installed Visual Studio, you can add hello Python workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="5a683-120">Miután telepíti, és a Visual Studio indítja, kattintson a **fájl** > **új** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="5a683-120">After installing and starting Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="5a683-121">Kattintson a **sablonok** > **Python** > **Python alkalmazás**, adja meg *myPythonProject* hello neve hello projekt, jelölje ki a hello projekt hello helyét, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="5a683-121">Click **Templates** > **Python** > **Python Application**, enter *myPythonProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-packages"></a><span data-ttu-id="5a683-122">Csomagok telepítése</span><span class="sxs-lookup"><span data-stu-id="5a683-122">Install packages</span></span>

1. <span data-ttu-id="5a683-123">A Megoldáskezelőben a *myPythonProject*, kattintson a jobb gombbal **Python-környezetek**, majd válassza ki **virtuális környezet hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="5a683-123">In Solution Explorer, under *myPythonProject*, right-click **Python Environments**, and then select **Add virtual environment**.</span></span>
2. <span data-ttu-id="5a683-124">A virtuális környezet hozzáadása hello képernyőn fogadja el hello alapértelmezett *env*, győződjön meg arról, hogy *Python 3.6 (64 bites)* hello alapszintű értelmezőt van jelölve, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="5a683-124">On hello Add Virtual Environment screen, accept hello default name of *env*, make sure that *Python 3.6 (64-bit)* is selected for hello base interpreter, and then click **Create**.</span></span>
3. <span data-ttu-id="5a683-125">Kattintson a jobb gombbal hello *env* környezetben létrehozott, kattintson a **Python-csomag telepítése**, adja meg *azure* hello a keresési mezőbe, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="5a683-125">Right-click hello *env* environment that you created, click **Install Python Package**, enter *azure* in hello search box, and then press Enter.</span></span>

<span data-ttu-id="5a683-126">Meg kell jelennie a kimeneti windows hello hello azure csomagok telepítése sikeresen megtörtént.</span><span class="sxs-lookup"><span data-stu-id="5a683-126">You should see in hello output windows that hello azure packages were successfully installed.</span></span> 

## <a name="create-credentials"></a><span data-ttu-id="5a683-127">Hitelesítő adatok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-127">Create credentials</span></span>

<span data-ttu-id="5a683-128">Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik egy [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="5a683-128">Before you start this step, make sure that you have an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="5a683-129">Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.</span><span class="sxs-lookup"><span data-stu-id="5a683-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

1. <span data-ttu-id="5a683-130">Nyissa meg *myPythonProject.py* létrehozott fájlt, majd adja hozzá a kódot tooenable az alkalmazás toorun:</span><span class="sxs-lookup"><span data-stu-id="5a683-130">Open *myPythonProject.py* file that was created, and then add this code tooenable your application toorun:</span></span>

    ```python
    if __name__ == "__main__":
    ```

2. <span data-ttu-id="5a683-131">tooimport hello kód szükséges, adja hozzá ezek utasítások toohello felső hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-131">tooimport hello code that is needed, add these statements toohello top of hello .py file:</span></span>

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. <span data-ttu-id="5a683-132">Ezután hello .py fájl, vegyen fel változók hello importálási utasítást toospecify a gyakori értékek használt hello kód után:</span><span class="sxs-lookup"><span data-stu-id="5a683-132">Next in hello .py file, add variables after hello import statements toospecify common values used in hello code:</span></span>
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    <span data-ttu-id="5a683-133">Cserélje le **előfizetés-azonosító** az előfizetés-azonosítóval.</span><span class="sxs-lookup"><span data-stu-id="5a683-133">Replace **subscription-id** with your subscription identifier.</span></span>

4. <span data-ttu-id="5a683-134">toocreate hello Active Directorybeli hitelesítő adatokat, hogy kell-e toomake kérelmeket, ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-134">toocreate hello Active Directory credentials that you need toomake requests, add this function after hello variables in hello .py file:</span></span>

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    <span data-ttu-id="5a683-135">Cserélje le **alkalmazásazonosító**, **hitelesítési kulcs**, és **bérlőazonosító** korábban összegyűjtött létrehozása után az Azure Active Directory hello értékekkel egyszerű szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="5a683-135">Replace **application-id**, **authentication-key**, and **tenant-id** with hello values that you previously collected when you created your Azure Active Directory service principal.</span></span>

5. <span data-ttu-id="5a683-136">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-136">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a><span data-ttu-id="5a683-137">Erőforrások létrehozása</span><span class="sxs-lookup"><span data-stu-id="5a683-137">Create resources</span></span>
 
### <a name="initialize-management-clients"></a><span data-ttu-id="5a683-138">Felügyeleti ügyfelek inicializálása</span><span class="sxs-lookup"><span data-stu-id="5a683-138">Initialize management clients</span></span>

<span data-ttu-id="5a683-139">Felügyeleti ügyfelek szükséges toocreate és Python SDK hello használata az Azure erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="5a683-139">Management clients are needed toocreate and manage resources using hello Python SDK in Azure.</span></span> <span data-ttu-id="5a683-140">toocreate hello ügyfelei, adja hozzá ezt a kódot a hello **Ha** hello .py fájl majd végén utasítást:</span><span class="sxs-lookup"><span data-stu-id="5a683-140">toocreate hello management clients, add this code under hello **if** statement at then end of hello .py file:</span></span>

```python
resource_group_client = ResourceManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
```

### <a name="create-hello-vm-and-supporting-resources"></a><span data-ttu-id="5a683-141">Hello virtuális gép létrehozása és az azt támogató erőforrások</span><span class="sxs-lookup"><span data-stu-id="5a683-141">Create hello VM and supporting resources</span></span>

<span data-ttu-id="5a683-142">Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5a683-142">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="5a683-143">toocreate egy erőforráscsoport, ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-143">toocreate a resource group, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. <span data-ttu-id="5a683-144">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-144">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

<span data-ttu-id="5a683-145">[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="5a683-145">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

1. <span data-ttu-id="5a683-146">rendelkezésre állási toocreate állítva, ez a funkció hozzáadása után hello .py fájl hello változók:</span><span class="sxs-lookup"><span data-stu-id="5a683-146">toocreate an availability set, add this function after hello variables in hello .py file:</span></span>
   
    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. <span data-ttu-id="5a683-147">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-147">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

<span data-ttu-id="5a683-148">A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.</span><span class="sxs-lookup"><span data-stu-id="5a683-148">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

1. <span data-ttu-id="5a683-149">toocreate hello virtuális gép, egy nyilvános IP-címet hello változók hello .py fájl után adja hozzá ezt a funkciót:</span><span class="sxs-lookup"><span data-stu-id="5a683-149">toocreate a public IP address for hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. <span data-ttu-id="5a683-150">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-150">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="5a683-151">Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="5a683-151">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="5a683-152">toocreate egy virtuális hálózatot, ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-152">toocreate a virtual network, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. <span data-ttu-id="5a683-153">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-153">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. <span data-ttu-id="5a683-154">tooadd alhálózati toohello virtuális hálózat, ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-154">tooadd a subnet toohello virtual network, add this function after hello variables in hello .py file:</span></span>
    
    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```
        
4. <span data-ttu-id="5a683-155">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-155">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="5a683-156">A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.</span><span class="sxs-lookup"><span data-stu-id="5a683-156">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

1. <span data-ttu-id="5a683-157">toocreate egy adott hálózati csatoló hello változók hello .py fájl után adja hozzá ezt a funkciót:</span><span class="sxs-lookup"><span data-stu-id="5a683-157">toocreate a network interface, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. <span data-ttu-id="5a683-158">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-158">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="5a683-159">Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="5a683-159">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

1. <span data-ttu-id="5a683-160">toocreate hello a virtuális gép, ez a funkció hozzáadása után hello .py fájl hello változók:</span><span class="sxs-lookup"><span data-stu-id="5a683-160">toocreate hello virtual machine, add this function after hello variables in hello .py file:</span></span>
   
    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': VM_NAME,
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm_parameters
        )
    
        return creation_result.result()
    ```

    > [!NOTE]
    > <span data-ttu-id="5a683-161">Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója.</span><span class="sxs-lookup"><span data-stu-id="5a683-161">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="5a683-162">További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5a683-162">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

2. <span data-ttu-id="5a683-163">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-163">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a><span data-ttu-id="5a683-164">Felügyeleti feladatok végrehajtása</span><span class="sxs-lookup"><span data-stu-id="5a683-164">Perform management tasks</span></span>

<span data-ttu-id="5a683-165">A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése.</span><span class="sxs-lookup"><span data-stu-id="5a683-165">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="5a683-166">Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.</span><span class="sxs-lookup"><span data-stu-id="5a683-166">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="5a683-167">Hello virtuális gép adatainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="5a683-167">Get information about hello VM</span></span>

1. <span data-ttu-id="5a683-168">hello virtuális gép, tooget adatait hello változók hello .py fájl után adja hozzá ezt a funkciót:</span><span class="sxs-lookup"><span data-stu-id="5a683-168">tooget information about hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def get_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME, expand='instanceView')
        print("hardwareProfile")
        print("   vmSize: ", vm.hardware_profile.vm_size)
        print("\nstorageProfile")
        print("  imageReference")
        print("    publisher: ", vm.storage_profile.image_reference.publisher)
        print("    offer: ", vm.storage_profile.image_reference.offer)
        print("    sku: ", vm.storage_profile.image_reference.sku)
        print("    version: ", vm.storage_profile.image_reference.version)
        print("  osDisk")
        print("    osType: ", vm.storage_profile.os_disk.os_type.value)
        print("    name: ", vm.storage_profile.os_disk.name)
        print("    createOption: ", vm.storage_profile.os_disk.create_option.value)
        print("    caching: ", vm.storage_profile.os_disk.caching.value)
        print("\nosProfile")
        print("  computerName: ", vm.os_profile.computer_name)
        print("  adminUsername: ", vm.os_profile.admin_username)
        print("  provisionVMAgent: {0}".format(vm.os_profile.windows_configuration.provision_vm_agent))
        print("  enableAutomaticUpdates: {0}".format(vm.os_profile.windows_configuration.enable_automatic_updates))
        print("\nnetworkProfile")
        for nic in vm.network_profile.network_interfaces:
            print("  networkInterface id: ", nic.id)
        print("\nvmAgent")
        print("  vmAgentVersion", vm.instance_view.vm_agent.vm_agent_version)
        print("    statuses")
        for stat in vm_result.instance_view.vm_agent.statuses:
            print("    code: ", stat.code)
            print("    displayStatus: ", stat.display_status)
            print("    message: ", stat.message)
            print("    time: ", stat.time)
        print("\ndisks");
        for disk in vm.instance_view.disks:
            print("  name: ", disk.name)
            print("  statuses")
            for stat in disk.statuses:
                print("    code: ", stat.code)
                print("    displayStatus: ", stat.display_status)
                print("    time: ", stat.time)
        print("\nVM general status")
        print("  provisioningStatus: ", vm.provisioning_state)
        print("  id: ", vm.id)
        print("  name: ", vm.name)
        print("  type: ", vm.type)
        print("  location: ", vm.location)
        print("\nVM instance status")
        for stat in vm.instance_view.statuses:
            print("  code: ", stat.code)
            print("  displayStatus: ", stat.display_status)
    ```
2. <span data-ttu-id="5a683-169">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-169">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a><span data-ttu-id="5a683-170">Hello VM leállítása</span><span class="sxs-lookup"><span data-stu-id="5a683-170">Stop hello VM</span></span>

<span data-ttu-id="5a683-171">Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani.</span><span class="sxs-lookup"><span data-stu-id="5a683-171">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="5a683-172">Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.</span><span class="sxs-lookup"><span data-stu-id="5a683-172">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

1. <span data-ttu-id="5a683-173">toostop hello virtuális gép felszabadítása, nélkül hello változók hello .py fájl után adja hozzá ezt a funkciót:</span><span class="sxs-lookup"><span data-stu-id="5a683-173">toostop hello virtual machine without deallocating it, add this function after hello variables in hello .py file:</span></span>

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    <span data-ttu-id="5a683-174">Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello power_off hívás toothis kódot:</span><span class="sxs-lookup"><span data-stu-id="5a683-174">If you want toodeallocate hello virtual machine, change hello power_off call toothis code:</span></span>

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="5a683-175">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-175">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a><span data-ttu-id="5a683-176">Indítsa el a virtuális gép hello</span><span class="sxs-lookup"><span data-stu-id="5a683-176">Start hello VM</span></span>

1. <span data-ttu-id="5a683-177">toostart hello a virtuális gép, ez a funkció hozzáadása után hello .py fájl hello változók:</span><span class="sxs-lookup"><span data-stu-id="5a683-177">toostart hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="5a683-178">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-178">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a><span data-ttu-id="5a683-179">Automatikus oszlopszélesség hello méretű VM</span><span class="sxs-lookup"><span data-stu-id="5a683-179">Resize hello VM</span></span>

<span data-ttu-id="5a683-180">Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét.</span><span class="sxs-lookup"><span data-stu-id="5a683-180">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="5a683-181">További információkért lásd: [Virtuálisgép-méretek](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="5a683-181">For more information, see [VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="5a683-182">toochange hello mérete hello virtuális gép, ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-182">toochange hello size of hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def update_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        vm.hardware_profile.vm_size = 'Standard_DS3'
        update_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm
        )

    return update_result.result()
    ```

2. <span data-ttu-id="5a683-183">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-183">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="5a683-184">Adja hozzá a adatok lemez toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="5a683-184">Add a data disk toohello VM</span></span>

<span data-ttu-id="5a683-185">Virtuális gépek is rendelkezhetnek, egy vagy több [adatlemezek](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) merevlemezekként tárolt.</span><span class="sxs-lookup"><span data-stu-id="5a683-185">Virtual machines can have one or more [data disks](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) that are stored as VHDs.</span></span>

1. <span data-ttu-id="5a683-186">tooadd adatok lemez toohello virtuális gépek esetén ez a funkció hozzáadása után hello változók hello .py fájl:</span><span class="sxs-lookup"><span data-stu-id="5a683-186">tooadd a data disk toohello virtual machine, add this function after hello variables in hello .py file:</span></span> 

    ```python
    def add_datadisk(compute_client):
        disk_creation = compute_client.disks.create_or_update(
            GROUP_NAME,
            'myDataDisk1',
            {
                'location': LOCATION,
                'disk_size_gb': 1,
                'creation_data': {
                    'create_option': DiskCreateOption.empty
                }
            }
        )
        data_disk = disk_creation.result()
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        add_result = vm.storage_profile.data_disks.append({
            'lun': 1,
            'name': 'myDataDisk1',
            'create_option': DiskCreateOption.attach,
            'managed_disk': {
                'id': data_disk.id
            }
        })
        add_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME,
            VM_NAME,
            vm)

        return add_result.result()
    ```

2. <span data-ttu-id="5a683-187">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-187">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a><span data-ttu-id="5a683-188">Erőforrások törlése</span><span class="sxs-lookup"><span data-stu-id="5a683-188">Delete resources</span></span>

<span data-ttu-id="5a683-189">Mivel van szó, az Azure-ban használt erőforrásokhoz, még mindig egy célszerű toodelete erőforrásokat, amelyek már nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="5a683-189">Because you are charged for resources used in Azure, it's always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="5a683-190">Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.</span><span class="sxs-lookup"><span data-stu-id="5a683-190">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="5a683-191">toodelete hello erőforráscsoport és az összes erőforrást, vegye fel ezt a funkciót hello változók hello .py fájl után:</span><span class="sxs-lookup"><span data-stu-id="5a683-191">toodelete hello resource group and all resources, add this function after hello variables in hello .py file:</span></span>
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. <span data-ttu-id="5a683-192">toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:</span><span class="sxs-lookup"><span data-stu-id="5a683-192">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    delete_resources(resource_group_client)
    ```

3. <span data-ttu-id="5a683-193">Mentés *myPythonProject.py*.</span><span class="sxs-lookup"><span data-stu-id="5a683-193">Save *myPythonProject.py*.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="5a683-194">Hello alkalmazás futtatása</span><span class="sxs-lookup"><span data-stu-id="5a683-194">Run hello application</span></span>

1. <span data-ttu-id="5a683-195">toorun hello konzolalkalmazást, kattintson a **Start** a Visual Studióban.</span><span class="sxs-lookup"><span data-stu-id="5a683-195">toorun hello console application, click **Start** in Visual Studio.</span></span>

2. <span data-ttu-id="5a683-196">Nyomja le az **Enter** hello állapota az egyes erőforrások visszakapott.</span><span class="sxs-lookup"><span data-stu-id="5a683-196">Press **Enter** after hello status of each resource is returned.</span></span> <span data-ttu-id="5a683-197">A hello állapotinformáció, megjelenik egy **sikeres** üzembe helyezési állapota.</span><span class="sxs-lookup"><span data-stu-id="5a683-197">In hello status information, you should see a **Succeeded** provisioning state.</span></span> <span data-ttu-id="5a683-198">Hello virtuális gép létrehozása után hello lehetőség toodelete az Ön által létrehozott összes hello-erőforrást.</span><span class="sxs-lookup"><span data-stu-id="5a683-198">After hello virtual machine is created, you have hello opportunity toodelete all hello resources that you create.</span></span> <span data-ttu-id="5a683-199">Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc tooverify azok létrehozásáról a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="5a683-199">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify their creation in hello Azure portal.</span></span> <span data-ttu-id="5a683-200">Ha az Azure portál megnyitása rendelkezik hello, lehetséges, hogy toorefresh hello panel toosee új erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="5a683-200">If you have hello Azure portal open, you might have toorefresh hello blade toosee new resources.</span></span>  

    <span data-ttu-id="5a683-201">Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="5a683-201">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> <span data-ttu-id="5a683-202">Hello alkalmazás befejezte előtt minden hello erőforrások és hello erőforráscsoport törlődnek után több percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="5a683-202">It may take several minutes after hello application has finished before all hello resources and hello resource group are deleted.</span></span>


## <a name="next-steps"></a><span data-ttu-id="5a683-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5a683-203">Next steps</span></span>

- <span data-ttu-id="5a683-204">Ha problémák merültek fel hello központi telepítés, a következő lépésben lesz toolook: [hibakeresési erőforrás csoport telepítéshez, amelynek az Azure-portálon](../../resource-manager-troubleshoot-deployments-portal.md)</span><span class="sxs-lookup"><span data-stu-id="5a683-204">If there were issues with hello deployment, a next step would be toolook at [Troubleshooting resource group deployments with Azure portal](../../resource-manager-troubleshoot-deployments-portal.md)</span></span>
- <span data-ttu-id="5a683-205">További tudnivalók hello [Azure Python kódtár](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="5a683-205">Learn more about hello [Azure Python Library](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span></span>

