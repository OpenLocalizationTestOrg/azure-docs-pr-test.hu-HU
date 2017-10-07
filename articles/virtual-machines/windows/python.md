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
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a>Létrehozása és kezelése Windows-alapú virtuális gépek az Azure-ban, Python

Egy [Azure virtuális gép](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) több támogató Azure-erőforrások kell. Ez a cikk ismerteti a létrehozása, kezelése és Python Virtuálisgép-erőforrások törlése. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Visual Studio-projekt létrehozása
> * Csomagok telepítése
> * Hitelesítő adatok létrehozása
> * Erőforrások létrehozása
> * Felügyeleti feladatok végrehajtása
> * Erőforrások törlése
> * Hello alkalmazás futtatása

Körülbelül 20 percet toodo szükséges lépéseket.

## <a name="create-a-visual-studio-project"></a>Visual Studio-projekt létrehozása

1. Ha még nem tette meg, telepítse [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Válassza ki **Python fejlesztői** hello munkaterhelések lap, és kattintson a **telepítése**. Az összefoglaló hello, láthatja, hogy **Python 3 64 bites (3.6.0)** automatikusan ki van jelölve meg. Ha már telepítette a Visual Studio, a Visual Studio indítója hello segítségével hello Python munkaterhelés is hozzáadhat.
2. Miután telepíti, és a Visual Studio indítja, kattintson a **fájl** > **új** > **projekt**.
3. Kattintson a **sablonok** > **Python** > **Python alkalmazás**, adja meg *myPythonProject* hello neve hello projekt, jelölje ki a hello projekt hello helyét, és kattintson **OK**.

## <a name="install-packages"></a>Csomagok telepítése

1. A Megoldáskezelőben a *myPythonProject*, kattintson a jobb gombbal **Python-környezetek**, majd válassza ki **virtuális környezet hozzáadása**.
2. A virtuális környezet hozzáadása hello képernyőn fogadja el hello alapértelmezett *env*, győződjön meg arról, hogy *Python 3.6 (64 bites)* hello alapszintű értelmezőt van jelölve, és kattintson a **létrehozása**.
3. Kattintson a jobb gombbal hello *env* környezetben létrehozott, kattintson a **Python-csomag telepítése**, adja meg *azure* hello a keresési mezőbe, és nyomja le az ENTER billentyűt.

Meg kell jelennie a kimeneti windows hello hello azure csomagok telepítése sikeresen megtörtént. 

## <a name="create-credentials"></a>Hitelesítő adatok létrehozása

Ez a lépés megkezdése előtt győződjön meg arról, hogy rendelkezik egy [Active Directory szolgáltatás egyszerű](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Is rögzíteni kell hello Alkalmazásazonosító, hello hitelesítési kulcs és hello Bérlőazonosító, amelyekre szüksége van egy későbbi lépésben.

1. Nyissa meg *myPythonProject.py* létrehozott fájlt, majd adja hozzá a kódot tooenable az alkalmazás toorun:

    ```python
    if __name__ == "__main__":
    ```

2. tooimport hello kód szükséges, adja hozzá ezek utasítások toohello felső hello .py fájl:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. Ezután hello .py fájl, vegyen fel változók hello importálási utasítást toospecify a gyakori értékek használt hello kód után:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    Cserélje le **előfizetés-azonosító** az előfizetés-azonosítóval.

4. toocreate hello Active Directorybeli hitelesítő adatokat, hogy kell-e toomake kérelmeket, ez a funkció hozzáadása után hello változók hello .py fájl:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    Cserélje le **alkalmazásazonosító**, **hitelesítési kulcs**, és **bérlőazonosító** korábban összegyűjtött létrehozása után az Azure Active Directory hello értékekkel egyszerű szolgáltatást.

5. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Erőforrások létrehozása
 
### <a name="initialize-management-clients"></a>Felügyeleti ügyfelek inicializálása

Felügyeleti ügyfelek szükséges toocreate és Python SDK hello használata az Azure erőforrások kezeléséhez. toocreate hello ügyfelei, adja hozzá ezt a kódot a hello **Ha** hello .py fájl majd végén utasítást:

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

### <a name="create-hello-vm-and-supporting-resources"></a>Hello virtuális gép létrehozása és az azt támogató erőforrások

Minden erőforrás tartalmaznia kell egy [erőforráscsoport](../../azure-resource-manager/resource-group-overview.md).

1. toocreate egy erőforráscsoport, ez a funkció hozzáadása után hello változók hello .py fájl:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

[Rendelkezésre állási készletek](tutorial-availability-sets.md) könnyebben meg az alkalmazás által használt toomaintain hello virtuális gépeket.

1. rendelkezésre állási toocreate állítva, ez a funkció hozzáadása után hello .py fájl hello változók:
   
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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

A [nyilvános IP-cím](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) szükséges toocommunicate hello virtuális gép van.

1. toocreate hello virtuális gép, egy nyilvános IP-címet hello változók hello .py fájl után adja hozzá ezt a funkciót:

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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Az alhálózat szerepelnie kell egy virtuális gépet egy [virtuális hálózati](../../virtual-network/virtual-networks-overview.md).

1. toocreate egy virtuális hálózatot, ez a funkció hozzáadása után hello változók hello .py fájl:

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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. tooadd alhálózati toohello virtuális hálózat, ez a funkció hozzáadása után hello változók hello .py fájl:
    
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
        
4. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

A virtuális gép egy hálózati illesztő toocommunicate hello virtuális hálózaton kell.

1. toocreate egy adott hálózati csatoló hello változók hello .py fájl után adja hozzá ezt a funkciót:

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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Most, hogy a létrehozott összes hello erőforrások támogatása, létrehozhat egy virtuális gépet.

1. toocreate hello a virtuális gép, ez a funkció hozzáadása után hello .py fájl hello változók:
   
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
    > Ebben az oktatóanyagban létrehoz egy virtuális gépet, hello Windows Server operációs rendszer verziója. További információ az egyéb rendszerképek kiválasztásáról toolearn lásd: [keresse meg és válassza ki azokat a Windows PowerShell és az Azure parancssori felület hello Azure virtuális gép lemezképeket](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a>Felügyeleti feladatok végrehajtása

A virtuális gépek hello életciklusa során szükség lehet a toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése. Emellett érdemes lehet toocreate kód tooautomate ismétlődő vagy összetett feladatokat.

### <a name="get-information-about-hello-vm"></a>Hello virtuális gép adatainak beolvasása

1. hello virtuális gép, tooget adatait hello változók hello .py fájl után adja hozzá ezt a funkciót:

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
2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a>Hello VM leállítása

Állítsa le a virtuális gépet és a beállítások megtartásához, de továbbra is toobe felszámított, vagy állítsa le a virtuális gépet, és azt felszabadítani. Ha egy virtuális gép fel van szabadítva, vele társított összes erőforrás is felszabadított és számlázási végpontjainak.

1. toostop hello virtuális gép felszabadítása, nélkül hello változók hello .py fájl után adja hozzá ezt a funkciót:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Ha azt szeretné, hogy toodeallocate hello virtuális gép, módosítsa a hello power_off hívás toothis kódot:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a>Indítsa el a virtuális gép hello

1. toostart hello a virtuális gép, ez a funkció hozzáadása után hello .py fájl hello változók:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a>Automatikus oszlopszélesség hello méretű VM

Telepítési sok szempontját figyelembe kell venni, amikor eldönti, a virtuális gép méretét. További információkért lásd: [Virtuálisgép-méretek](sizes.md).

1. toochange hello mérete hello virtuális gép, ez a funkció hozzáadása után hello változók hello .py fájl:

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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a>Adja hozzá a adatok lemez toohello méretű VM

Virtuális gépek is rendelkezhetnek, egy vagy több [adatlemezek](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) merevlemezekként tárolt.

1. tooadd adatok lemez toohello virtuális gépek esetén ez a funkció hozzáadása után hello változók hello .py fájl: 

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

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a>Erőforrások törlése

Mivel van szó, az Azure-ban használt erőforrásokhoz, még mindig egy célszerű toodelete erőforrásokat, amelyek már nem szükséges. Ha azt szeretné, hogy toodelete hello virtuális gépek és erőforrások támogató összes hello, minden toodo van hello erőforrás csoport törlése.

1. toodelete hello erőforráscsoport és az összes erőforrást, vegye fel ezt a funkciót hello változók hello .py fájl után:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. toocall hello függvény korábban hozzáadott, adja hozzá ezt a kódot a hello **Ha** utasításban, hely: hello hello .py fájl végéhez:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. Mentés *myPythonProject.py*.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

1. toorun hello konzolalkalmazást, kattintson a **Start** a Visual Studióban.

2. Nyomja le az **Enter** hello állapota az egyes erőforrások visszakapott. A hello állapotinformáció, megjelenik egy **sikeres** üzembe helyezési állapota. Hello virtuális gép létrehozása után hello lehetőség toodelete az Ön által létrehozott összes hello-erőforrást. Ahhoz, hogy nyomja le az ENTER **Enter** toostart törlése erőforrásokat, készíthet néhány perc tooverify azok létrehozásáról a hello Azure-portálon. Ha az Azure portál megnyitása rendelkezik hello, lehetséges, hogy toorefresh hello panel toosee új erőforrásokat.  

    Akkor kell a konzol alkalmazás toorun teljesen a start toofinish körülbelül öt percet is igénybe vehet. Hello alkalmazás befejezte előtt minden hello erőforrások és hello erőforráscsoport törlődnek után több percig is eltarthat.


## <a name="next-steps"></a>Következő lépések

- Ha problémák merültek fel hello központi telepítés, a következő lépésben lesz toolook: [hibakeresési erőforrás csoport telepítéshez, amelynek az Azure-portálon](../../resource-manager-troubleshoot-deployments-portal.md)
- További tudnivalók hello [Azure Python kódtár](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)

