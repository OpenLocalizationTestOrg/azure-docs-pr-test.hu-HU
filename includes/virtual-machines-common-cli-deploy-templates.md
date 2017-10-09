
* [Virtuális gép gyors létrehozása az Azure-ban](#quick-create-a-vm-in-azure)
* [Virtuális gép központi telepítése sablonból az Azure-ban](#deploy-a-vm-in-azure-from-a-template)
* [Virtuális gép létrehozása egyéni rendszerképből](#create-a-custom-vm-image)
* [Virtuális hálózatot és terheléselosztót használó virtuális gép központi telepítése](#deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer)
* [Erőforráscsoport eltávolítása](#remove-a-resource-group)
* [Egy erőforrás-csoport központi telepítésének hello naplóban megjelenítése](#show-the-log-for-a-resource-group-deployment)
* [Virtuális gépre vonatkozó információk megjelenítése](#display-information-about-a-virtual-machine)
* [Csatlakoztassa tooa Linux-alapú virtuális gépet](#log-on-to-a-linux-based-virtual-machine)
* [Virtuális gép leállítása](#stop-a-virtual-machine)
* [Virtuális gép elindítása](#start-a-virtual-machine)
* [Adatlemez csatolása](#attach-a-data-disk)

## <a name="getting-ready"></a>Előkészületek
Azure erőforráscsoport-sablonok hello Azure CLI használata előtt kell toohave hello Azure CLI verzió és az Azure-fiók. Ha még nem rendelkezik Azure CLI hello [telepítse](../articles/cli-install-nodejs.md).

### <a name="update-your-azure-cli-version-too090-or-later"></a>Az Azure CLI verzió too0.9.0 frissítése vagy újabb verzió
Típus `azure --version` fel kell-e már telepített 0.9.0-s verziója toosee vagy újabb.

```azurecli
azure --version
0.9.0 (node: 0.10.25)
```

Ha a verziószáma nem 0.9.0-s vagy újabb rendszert kell tooupdate azt egyikével hello natív telepítők vagy **npm** beírásával `npm update -g azure-cli`.

Is futtathatja, az Azure parancssori felület egy Docker-tároló által hello alábbi [Docker kép](https://registry.hub.docker.com/u/microsoft/azure-cli/). Docker gazdagépről futtassa a következő parancs hello:

```bash
docker run -it microsoft/azure-cli
```

### <a name="set-your-azure-account-and-subscription"></a>Az Azure-fiók és -előfizetés beállítása
Ha még nincs Azure-előfizetése, de van MSDN-előfizetése, aktiválhatja [MSDN-előfizetői előnyeit](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Regisztrálhat [ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/) is.

Most [interaktív jelentkezzen be Azure-fiók tooyour](../articles/xplat-cli-connect.md#scenario-1-azure-login-with-interactive-login) beírásával `azure login` és a következő hello kér egy interaktív bejelentkezési élmény tooyour Azure-fiók. 

> [!NOTE]
> Ha rendelkezik munkahelyi vagy iskolai azonosítója, és nem rendelkezik kéttényezős hitelesítést, akkor tudja **is** használja `azure login -u` együtt hello munkahelyi vagy iskolai azonosító toolog a *nélkül* egy interaktív munkamenet. Ha nem rendelkezik munkahelyi vagy iskolai azonosítója, akkor [munkahelyi vagy iskolai azonosító létrehozása a személyes Microsoft-fiókhoz tartozó](../articles/virtual-machines/windows/create-aad-work-id.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) toolog hello a megszokott módon.
>
>

A fiókjához tartozhat több előfizetés is. Az előfizetések listájának megjelenítéséhez írja be az `azure account list` karakterláncot. Az eredmény a következőhöz hasonló lesz:

```azurecli
azure account list
info:    Executing command account list
data:    Name                              Id                                    Tenant Id                            Current
data:    --------------------------------  ------------------------------------  ------------------------------------  -------
data:    Contoso Admin                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  true
data:    Fabrikam dev                      xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Fabrikam test                     xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
data:    Contoso production                xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx  false  
```

Hello jelenlegi Azure-előfizetés hello következő állíthatja be. Hello nevét vagy hello Előfizetésazonosító toomanage kívánt hello erőforrásokat tartalmazó használja.

```azurecli
azure account set <subscription name or ID> true
```

### <a name="switch-toohello-azure-cli-resource-group-mode"></a>Váltás toohello Azure CLI erőforrás mód
Alapértelmezés szerint az Azure parancssori felület hello hello szolgáltatás felügyeleti üzemmódban indul (**asm** módot). Írja be a következő tooswitch tooresource mód hello.

```azurecli
azure config mode arm
```

## <a name="understanding-azure-resource-templates-and-resource-groups"></a>Az Azure-erőforrássablonok és -erőforráscsoportok ismertetése
A legtöbb alkalmazás különböző erőforrástípusokból épül fel (például egy vagy több virtuális gép és tárfiók, egy SQL-adatbázis, egy virtuális hálózat vagy egy tartalomkézbesítési hálózat). alapértelmezett Azure service management API hello és az hello a klasszikus Azure portálon ezeket az elemeket a szolgáltatások által módszer használatával. Ez a megközelítés toodeploy meg és hello egyéni szolgáltatások kezelése (vagy találhatók, így más eszközök), nem pedig a központi telepítés egyetlen logikai egységbe.

*Az Azure Resource Manager-sablonok*, azonban Ön toodeploy lehetővé teszik, és a különböző erőforrások kezelését egy logikai telepítési egységként deklaratív módon. Helyett imperatively szólítja fel Azure milyen toodeploy egy parancs egymás után, egy JSON-fájl – összes hello erőforrások és a társított konfigurációs és üzembe helyezéshez megadott paraméterek – a teljes telepítését írja le, és mondja el ezeket az erőforrásokat, mint az Azure toodeploy a csoport.

Hello kezelhetők teljes életciklusát hello csoport erőforrások az Azure parancssori felület erőforrás felügyeleti parancsok használatával:

* Állítsa le, elindításához, vagy törölje az összes hello erőforrások hello csoporton belüli egyszerre.
* Szerepköralapú hozzáférés-vezérlés (RBAC) szabályok toolock le a biztonsági engedélyek alkalmazza őket.
* Auditálási műveletek.
* Erőforrások címkézése további metaadatokkal a jobb nyomon követés érdekében.

Sok Azure erőforráscsoport-sablonok és mit tehet meg a hello többet tudhat [Azure Resource Manager áttekintése](../articles/azure-resource-manager/resource-group-overview.md). A sablonok készítésével kapcsolatos további információk: [Azure Resource Manager-sablonok készítése](../articles/resource-group-authoring-templates.md).

## <a id="quick-create-a-vm-in-azure"></a>Feladat: Virtuális gép gyors létrehozása az Azure-ban
Néha ismernie kell, képet és van szüksége a virtuális gépek erről a képről most nem túlságosan érdeklik hello infrastruktúra – lehet, hogy rendelkezik tootest valami egy tiszta virtuális gépen. Amely megadása, ha azt szeretné, hogy toouse hello `azure vm quick-create` parancsot, és adja át hello argumentum szükséges toocreate, a virtuális gépek és az infrastruktúra.

Először is hozzon létre egy erőforráscsoportot.

```azurecli
azure group create coreos-quick westus
info:    Executing command group create
+ Getting resource group coreos-quick
+ Creating resource group coreos-quick
info:    Created resource group coreos-quick
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick
data:    Name:                coreos-quick
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Ezután szüksége lesz egy rendszerképre. hello Azure parancssori felület, a lemezkép egy toofind lásd [Navigating és a PowerShell és az Azure parancssori felület hello Azure virtuálisgép-rendszerképek kiválasztásáról](../articles/virtual-machines/linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ebben a cikkben azonban az alábbiakban látható egy rövid lista a népszerű rendszerképekről. A gyors létrehozáshoz a CoreOS Stable rendszerképét fogjuk használni.

> [!NOTE]
> A ComputeImageVersion egyszerűen is megadhat "legújabb" módon paraméter mindkét hello sablon nyelven, és az Azure CLI hello hello. Ez lehetővé teszi a tooalways hello legújabb és javított verzióját használja hello kép toomodify nélkül, a parancsfájlok vagy sablonok. Ez az alábbiakban látható.
>
>

| Közzétevő neve | Ajánlat | SKU | Verzió |
|:--- |:--- |:--- |:--- |
| OpenLogic |CentOS |7 |7.0.201503 |
| OpenLogic |CentOS |7.1 |7.1.201504 |
| CoreOS |CoreOS |Beta |647.0.0 |
| CoreOS |CoreOS |Stable |633.1.0 |
| MicrosoftDynamicsNAV |DynamicsNAV |2015 |8.0.40459 |
| MicrosoftSharePoint |MicrosoftSharePointServer |2013 |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Standard |1.0.0 |
| msopentech |Oracle-Database-12c-Weblogic-Server-12c |Enterprise |1.0.0 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-DW |12.0.2430 |
| MicrosoftSQLServer |SQL2014-WS2012R2 |Enterprise-Optimized-for-OLTP |12.0.2430 |
| Canonical |UbuntuServer |12.04.5-LTS |12.04.201504230 |
| Canonical |UbuntuServer |14.04.2-LTS |14.04.201503090 |
| MicrosoftWindowsServer |WindowsServer |2012-Datacenter |3.0.201503 |
| MicrosoftWindowsServer |WindowsServer |2012-R2-Datacenter |4.0.201503 |
| MicrosoftWindowsServer |WindowsServer |Windows-Server-Technical-Preview |5.0.201504 |
| MicrosoftWindowsServerEssentials |WindowsServerEssentials |WindowsServerEssentials |1.0.141204 |
| MicrosoftWindowsServerHPCPack |WindowsServerHPCPack |2012R2 |4.3.4665 |

Csak a virtuális gép létrehozása az hello megadásával `azure vm quick-create` parancsot, és készen áll a hello megjelenő utasításokat. A következőhöz hasonló eredményt kell kapnia:

```azurecli
azure vm quick-create
info:    Executing command vm quick-create
Resource group name: coreos-quick
Virtual machine name: coreos
Location name: westus
Operating system Type [Windows, Linux]: linux
ImageURN (format: "publisherName:offer:skus:version"): coreos:coreos:stable:latest
User name: ops
Password: *********
Confirm password: *********
+ Looking up hello VM "coreos"
info:    Using hello VM Size "Standard_A1"
info:    hello [OS, Data] Disk or image configuration requires storage account
+ Retrieving storage accounts
info:    Could not find any storage accounts in hello region "westus", trying toocreate new one
+ Creating storage account "cli9fd3fce49e9a9b3d14302" in "westus"
+ Looking up hello storage account cli9fd3fce49e9a9b3d14302
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
info:    An nic with given name "coreo-westu-1430261891570-nic" not found, creating a new one
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
info:    Preparing toocreate new virtual network and subnet
/ Creating a new virtual network "coreo-westu-1430261891570-vnet" [address prefix: "10.0.0.0/16"] with subnet "coreo-westu-1430261891570-sne+" [address prefix: "10.0.1.0/24"]
+ Looking up hello virtual network "coreo-westu-1430261891570-vnet"
+ Looking up hello subnet "coreo-westu-1430261891570-snet" under hello virtual network "coreo-westu-1430261891570-vnet"
info:    Found public ip parameters, trying toosetup PublicIP profile
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
info:    PublicIP with given name "coreo-westu-1430261891570-pip" not found, creating a new one
+ Creating public ip "coreo-westu-1430261891570-pip"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
+ Creating NIC "coreo-westu-1430261891570-nic"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Creating VM "coreos"
+ Looking up hello VM "coreos"
+ Looking up hello NIC "coreo-westu-1430261891570-nic"
+ Looking up hello public ip "coreo-westu-1430261891570-pip"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Compute/virtualMachines/coreos
data:    ProvisioningState               :Succeeded
data:    Name                            :coreos
data:    Location                        :westus
data:    FQDN                            :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :coreos
data:        Offer                       :coreos
data:        Sku                         :stable
data:        Version                     :633.1.0
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :cli9fd3fce49e9a9b3d-os-1430261892283
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://cli9fd3fce49e9a9b3d14302.blob.core.windows.net/vhds/cli9fd3fce49e9a9b3d-os-1430261892283.vhd
data:
data:    OS Profile:
data:      Computer Name                 :coreos
data:      User Name                     :ops
data:      Linux Configuration:
data:        Disable Password Auth       :false
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/coreos-quick/providers/Microsoft.Network/networkInterfaces/coreo-westu-1430261891570-nic
data:          Primary                   :true
data:          MAC Address               :00-0D-3A-30-72-E3
data:          Provisioning State        :Succeeded
data:          Name                      :coreo-westu-1430261891570-nic
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.1.4
data:            Public IP address       :104.40.24.124
data:            FQDN                    :coreo-westu-1430261891570-pip.westus.cloudapp.azure.com
info:    vm quick-create command OK
```

És már kész is az új virtuális gép.

## <a id="deploy-a-vm-in-azure-from-a-template"></a>Feladat: Virtuális gép központi telepítése sablonból az Azure-ban
Használja a hello utasításokat a szakaszok toodeploy egy új Azure virtuális Gépet a sablon hello Azure parancssori felület használatával. Ez a sablon egyetlen virtuális gép létrehoz egy új virtuális hálózatot egyetlen alhálózattal, és eltérően a `azure vm quick-create`, lehetővé teszi, hogy Ön toodescribe mit pontosan, és ismételje meg a hiba nélkül betöltődtek. Itt látható, amit a sablon létrehoz:

![](./media/virtual-machines-common-cli-deploy-templates/new-vm.png)

### <a name="step-1-examine-hello-json-file-for-hello-template-parameters"></a>1. lépés:, Vizsgálja meg hello JSON-fájl hello Sablonparaméterek
Az alábbiakban hello hello hello sablon JSON-fájl tartalmát. (hello sablon is található [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json).)

Sablonok rugalmasak, így előfordulhat, hogy a kiválasztott toogive nagy mennyiségű paraméterek vagy toooffer csak hozzon létre egy sablont, amely több rögzített néhány kiválasztott hello designer. A toocollect hello rendelésinformációkat toopass hello sablon paraméterként van szüksége, nyissa meg a hello sablonfájl (Ez a témakör tartalmaz egy sablon beágyazott alább), és vizsgálja meg a hello **paraméterek** értékeket.

Ebben az esetben az alábbi hello sablon kéri:

* Egyedi tárfióknév.
* Egy virtuális gép hello rendszergazda felhasználó felhasználóneve.
* Jelszó.
* Hello world toouse kívül tartomány nevét.
* Ubuntu Server-verziószám – de a rendszer csak egyet fogad el egy listáról.

További információk a [felhasználónév- és jelszókövetelményekről](../articles/virtual-machines/linux/faq.md#what-are-the-username-requirements-when-creating-a-vm).

Ha eldöntötte, ezeket az értékeket, Ön készen toocreate egy csoportot, és ez a sablon üzembe helyezés az Azure-előfizetéshez.

```json
{
"$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
"contentVersion": "1.0.0.0",
"parameters": {
    "newStorageAccountName": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello storage account where hello virtual machine's disks will be placed."
    }
    },
    "adminUsername": {
    "type": "string",
    "metadata": {
        "description": "User name for hello virtual machine."
    }
    },
    "adminPassword": {
    "type": "securestring",
    "metadata": {
        "description": "Password for hello virtual machine."
    }
    },
    "dnsNameForPublicIP": {
    "type": "string",
    "metadata": {
        "description": "Unique DNS name for hello public IP used tooaccess hello virtual machine."
    }
    },
    "ubuntuOSVersion": {
    "type": "string",
    "defaultValue": "14.04.2-LTS",
    "allowedValues": [
        "12.04.5-LTS",
        "14.04.2-LTS",
        "15.04"
    ],
    "metadata": {
        "description": "hello Ubuntu version for hello VM. This will pick a fully patched image of this given Ubuntu version. Allowed values: 12.04.5-LTS, 14.04.2-LTS, 15.04."
    }
    }
},
"variables": {
    "location": "West US",
    "imagePublisher": "Canonical",
    "imageOffer": "UbuntuServer",
    "OSDiskName": "osdiskforlinuxsimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyUbuntuVM",
    "vmSize": "Standard_D1",
    "virtualNetworkName": "MyVNET",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
},
"resources": [
    {
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[parameters('newStorageAccountName')]",
    "apiVersion": "2015-05-01-preview",
    "location": "[variables('location')]",
    "properties": {
        "accountType": "[variables('storageAccountType')]"
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/publicIPAddresses",
    "name": "[variables('publicIPAddressName')]",
    "location": "[variables('location')]",
    "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
        "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
        }
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/virtualNetworks",
    "name": "[variables('virtualNetworkName')]",
    "location": "[variables('location')]",
    "properties": {
        "addressSpace": {
        "addressPrefixes": [
            "[variables('addressPrefix')]"
        ]
        },
        "subnets": [
        {
            "name": "[variables('subnetName')]",
            "properties": {
            "addressPrefix": "[variables('subnetPrefix')]"
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Network/networkInterfaces",
    "name": "[variables('nicName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
    ],
    "properties": {
        "ipConfigurations": [
        {
            "name": "ipconfig1",
            "properties": {
            "privateIPAllocationMethod": "Dynamic",
            "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
            },
            "subnet": {
                "id": "[variables('subnetRef')]"
            }
            }
        }
        ]
    }
    },
    {
    "apiVersion": "2015-05-01-preview",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[variables('location')]",
    "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', parameters('newStorageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {
        "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
        "computername": "[variables('vmName')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
        "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('ubuntuOSVersion')]",
            "version": "latest"
        },
        "osDisk": {
            "name": "osdisk",
            "vhd": {
            "uri": "[concat('http://',parameters('newStorageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
        }
        },
        "networkProfile": {
        "networkInterfaces": [
            {
            "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
        ]
        }
    }
    }
]
}
```

### <a name="step-2-create-hello-virtual-machine-by-using-hello-template"></a>2. lépés: Hello virtuális gép létrehozása hello sablon használatával
Ha már készen áll a paraméterértékek, hozzon létre egy erőforráscsoportot a sablon üzembe helyezésére, és majd a hello sablon üzembe helyezése.

toocreate hello erőforráscsoportot, a típus `azure group create <group name> <location>` hello nevű hello csoportot, és szeretné toodeploy hello adatközpont helye. Ez gyorsan megtörténik:

```azurecli
azure group create myResourceGroup westus
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Most toocreate hello telepítés, a hívás `azure group deployment create` , és adja át:

* hello sablonfájl (Ha mentett JSON-sablon tooa helyi fájl fent hello).
* A sablon URI (Ha azt szeretné, hogy a hello fájlt a Githubon vagy valamilyen más webcímet toopoint).
* szeretné toodeploy hello erőforráscsoportot.
* A központi telepítés nevét (nem kötelező).

Rákérdezéses toosupply hello értékeket a paraméterek hello "paraméterek" részében hello JSON-fájl lesz. Minden hello paraméterértékek megadását, megkezdődik a telepítés.

Például:

```azurecli
azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-linux/azuredeploy.json myResourceGroup firstDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
newStorageAccountName: storageaccount
adminUsername: ops
adminPassword: password
dnsNameForPublicIP: newdomainname
```

A következő típusú információkat hello fog kapni:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "firstDeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
data:    DeploymentName     : firstDeployment
data:    ResourceGroupName  : myResourceGroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T07:53:55.1828878Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-simple-linux-vm/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  -------------
data:    newStorageAccountName  String        storageaccount
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameForPublicIP     String        newdomainname
data:    ubuntuOSVersion        String        14.10
info:    group deployment create command OK
```


## <a id="create-a-custom-vm-image"></a>Feladat: Egyéni virtuális gép rendszerképének létrehozása
Most láthatta, a fenti sablonok hello alapvető használatát, így most is használunk hasonló utasításokat toocreate egyéni virtuális gép egy adott .vhd fájl az Azure-ban sablonnal keresztül hello Azure parancssori felület. hello különbség itt az, hogy ez a sablon a megadott virtuális merevlemezről (VHD) egyetlen virtuális gép létrehoz.

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>1. lépés:, Vizsgálja meg hello hello sablon JSON-fájl
Az alábbiakban hello JSON-fájl hello sablonból, amely ebben a szakaszban példaként hello tartalmát. (hello sablon is található [GitHub](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json).)

Ebben az esetben szüksége lesz toofind hello értékeket a tooenter hello paraméterek, amely nem rendelkezik alapértelmezett értékekkel. Hello futtatásakor `azure group deployment create` parancs hello Azure parancssori felület felszólítja, tooenter ezeket az értékeket.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters" : {
        "userImageStorageAccountName": {
            "type": "string",
            "defaultValue" : "userImageStorageAccountName"
        },
        "userImageStorageContainerName" : {
            "type" : "string",
            "defaultValue" : "userImageStorageContainerName"
        },
        "userImageVhdName" : {
            "type" : "string",
            "defaultValue" : "userImageVhdName"
        },
        "dnsNameForPublicIP" : {
            "type" : "string",
            "defaultValue": "uniqueDnsNameForPublicIP"
        },
        "adminUserName": {
            "type": "string"
        },
        "adminPassword": {
            "type": "securestring"
        },
        "osType" : {
            "type" : "string",
            "allowedValues" : [
                "windows",
                "linux"
            ]
        },
        "subscriptionId":{
            "type" : "string"
        },
        "location": {
            "type": "String",
            "defaultValue" : "West US"
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A2"
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue" : "myPublicIP"
        },
        "vmName": {
            "type": "string",
            "defaultValue" : "myVM"
        },
        "virtualNetworkName":{
            "type" : "string",
            "defaultValue" : "myVNET"
        },
        "nicName":{
            "type" : "string",
            "defaultValue":"myNIC"
        }
    },
    "variables": {
        "addressPrefix":"10.0.0.0/16",
        "subnet1Name": "Subnet-1",
        "subnet2Name": "Subnet-2",
        "subnet1Prefix" : "10.0.0.0/24",
        "subnet2Prefix" : "10.0.1.0/24",
        "publicIPAddressType" : "Dynamic",
        "vnetID":"[resourceId('Microsoft.Network/virtualNetworks',parameters('virtualNetworkName'))]",
        "subnet1Ref" : "[concat(variables('vnetID'),'/subnets/',variables('subnet1Name'))]",
        "userImageName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('userImageVhdName'))]",
        "osDiskVhdName" : "[concat('http://',parameters('userImageStorageAccountName'),'.blob.core.windows.net/',parameters('userImageStorageContainerName'),'/',parameters('vmName'),'osDisk.vhd')]"
    },
    "resources": [
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/publicIPAddresses",
        "name": "[parameters('publicIPAddressName')]",
        "location": "[parameters('location')]",
        "properties": {
            "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
            "dnsSettings": {
                "domainNameLabel": "[parameters('dnsNameForPublicIP')]"
            }
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/virtualNetworks",
        "name": "[parameters('virtualNetworkName')]",
        "location": "[parameters('location')]",
        "properties": {
        "addressSpace": {
            "addressPrefixes": [
            "[variables('addressPrefix')]"
            ]
        },
        "subnets": [
            {
            "name": "[variables('subnet1Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet1Prefix')]"
            }
            },
            {
            "name": "[variables('subnet2Name')]",
            "properties" : {
                "addressPrefix": "[variables('subnet2Prefix')]"
            }
            }
        ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Network/networkInterfaces",
        "name": "[parameters('nicName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]",
            "[concat('Microsoft.Network/virtualNetworks/', parameters('virtualNetworkName'))]"
        ],
        "properties": {
            "ipConfigurations": [
            {
                "name": "ipconfig1",
                "properties": {
                    "privateIPAllocationMethod": "Dynamic",
                    "publicIPAddress": {
                        "id": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]"
                    },
                    "subnet": {
                        "id": "[variables('subnet1Ref')]"
                    }
                }
            }
            ]
        }
    },
    {
        "apiVersion": "2014-12-01-preview",
        "type": "Microsoft.Compute/virtualMachines",
        "name": "[parameters('vmName')]",
        "location": "[parameters('location')]",
        "dependsOn": [
            "[concat('Microsoft.Network/networkInterfaces/', parameters('nicName'))]"
        ],
        "properties": {
            "hardwareProfile": {
                "vmSize": "[parameters('vmSize')]"
            },
            "osProfile": {
                "computername": "[parameters('vmName')]",
                "adminUsername": "[parameters('adminUsername')]",
                "adminPassword": "[parameters('adminPassword')]"
            },
            "storageProfile": {
                "osDisk" : {
                    "name" : "[concat(parameters('vmName'),'-osDisk')]",
                    "osType" : "[parameters('osType')]",
                    "caching" : "ReadWrite",
                    "image" : {
                        "uri" : "[variables('userImageName')]"
                    },
                    "vhd" : {
                        "uri" : "[variables('osDiskVhdName')]"
                    }
                }
            },
            "networkProfile": {
                "networkInterfaces" : [
                {
                    "id": "[resourceId('Microsoft.Network/networkInterfaces',parameters('nicName'))]"
                }
                ]
            }
        }
    }
    ]
}
```

### <a name="step-2-obtain-hello-vhd"></a>2. lépés: Virtuális merevlemez hello beszerzése
Ehhez természetesen szükség van egy .vhd fájlra. Ha már rendelkezik eggyel az Azure-ban, használhatja azt, vagy feltölthet egyet.

Windows-alapú virtuális gép esetén lásd: [létrehozása és feltöltése a Windows Server VHD tooAzure](../articles/virtual-machines/windows/classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

Linux-alapú virtuális gép esetén lásd: [létrehozása és feltöltése hello Linux operációs rendszert tartalmazó virtuális merevlemez](../articles/virtual-machines/linux/classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

### <a name="step-3-create-hello-virtual-machine-by-using-hello-template"></a>3. lépés: Hello virtuális gép létrehozása hello sablon használatával
Most már készen áll a toocreate hello .vhd alapuló új virtuális gép most. Hozzon létre egy csoport toodeploy, `azure group create <location>`:

```azurecli
azure group create myResourceGroupUser eastus
info:    Executing command group create
+ Getting resource group myResourceGroupUser
+ Creating resource group myResourceGroupUser
info:    Created resource group myResourceGroupUser
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/myResourceGroupUser
data:    Name:                myResourceGroupUser
data:    Location:            eastus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Majd hello segítségével a hello központi telepítés létrehozásához `--template-uri` beállítás toocall közvetlenül hello sablonban (vagy használhatja a hello `--template-file` beállítás toouse helyileg mentett fájl). Jegyezze fel, mert hello szolgáltatássablonra megadott alapértelmezett értéke, meg kell adni csak néhány dolgot. Ha hello sablon különböző helyeken telepít, előfordulhat, hogy néhány elnevezési ütközések hello alapértelmezett értékükön (különösen hello DNS-név létrehozása) történik-e.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json \
> myResourceGroup \
> customVhdDeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
adminUserName: ops
adminPassword: password
osType: linux
subscriptionId: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

Kimeneti alábbihoz hasonló hello:

```azurecli
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "customVhdDeployment"
+ Registering providers
info:    Registering provider microsoft.network
info:    Registering provider microsoft.compute
+ Waiting for deployment toocomplete
error:   Deployment provisioning state was not successful
data:    DeploymentName     : customVhdDeployment
data:    ResourceGroupName  : myResourceGroupUser
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T14:55:48.0963829Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-from-user-image/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                           Type          Value
data:    -----------------------------  ------------  ------------------------------------
data:    userImageStorageAccountName    String        userImageStorageAccountName
data:    userImageStorageContainerName  String        userImageStorageContainerName
data:    userImageVhdName               String        userImageVhdName
data:    dnsNameForPublicIP             String        uniqueDnsNameForPublicIP
data:    adminUserName                  String        ops
data:    adminPassword                  SecureString  undefined
data:    osType                         String        linux
data:    subscriptionId                 String        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
data:    location                       String        West US
data:    vmSize                         String        Standard_A2
data:    publicIPAddressName            String        myPublicIP
data:    vmName                         String        myVM
data:    virtualNetworkName             String        myVNET
data:    nicName                        String        myNIC
info:    group deployment create command OK
```

## <a id="deploy-a-multi-vm-application-that-uses-a-virtual-network-and-an-external-load-balancer"></a>Feladat: Virtuális hálózatot és külső terheléselosztót használó több virtuális gépes alkalmazás üzembe helyezése
Ez a sablon lehetővé teszi a toocreate két virtuális gép egy terheléselosztó alatt, és konfiguráljon egy terheléselosztási szabályt a 80-as Port. Emellett ez a sablon üzembe helyez egy tárfiókot, egy virtuális hálózatot, és nyilvános IP-címet, egy rendelkezésre állási csoportot és hálózati adaptereket is.

![](./media/virtual-machines-common-cli-deploy-templates/multivmextlb.png)

Kövesse ezeket a lépéseket toodeploy egy virtuális Gépre kiterjedő alkalmazás, amely egy virtuális hálózatot és egy terheléselosztót használ hello GitHub sablon tárházban Azure PowerShell-parancsok segítségével a Resource Manager-sablon használatával.

### <a name="step-1-examine-hello-json-file-for-hello-template"></a>1. lépés:, Vizsgálja meg hello hello sablon JSON-fájl
Az alábbiakban hello hello hello sablon JSON-fájl tartalmát. Ha azt szeretné, hogy hello legújabb verziója, akkor található [hello GitHub-tárházban sablonok](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json). Ez a témakör használ hello `--template-uri` kapcsoló toocall hello sablon, de használhatja hello `--template-file` toopass egy helyi verzióra váltani.

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2014-04-01-preview/deploymentTemplate.json",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "metadata": {
                "description": "Location of resources"
            }
        },
        "storageAccountName": {
            "type": "string",
            "metadata": {
                "description": "Name of storage account"
            }
        },
        "adminUsername": {
            "type": "string",
            "metadata": {
                "description": "Admin user name"
            }
        },
        "adminPassword": {
            "type": "securestring",
            "metadata": {
                "description": "Admin password"
            }
        },
        "dnsNameforLBIP": {
            "type": "string",
            "metadata": {
                "description": "DNS for load balancer IP"
            }
        },
        "backendPort": {
            "type": "int",
            "defaultValue": 3389,
            "metadata": {
                "description": "Back-end port"
            }
        },
        "vmNamePrefix": {
            "type": "string",
            "defaultValue": "myVM",
            "metadata": {
                "description": "Prefix toouse for VM names"
            }
        },
        "vmSourceImageName": {
            "type": "string",
            "defaultValue": "a699494373c04fc0bc8f2bb1389d6106__Windows-Server-2012-R2-201412.01-en.us-127GB.vhd"
        },
        "lbName": {
            "type": "string",
            "defaultValue": "myLB",
            "metadata": {
                "description": "Load balancer name"
            }
        },
        "nicNamePrefix": {
            "type": "string",
            "defaultValue": "nic",
            "metadata": {
                "description": "Network interface name prefix"
            }
        },
        "publicIPAddressName": {
            "type": "string",
            "defaultValue": "myPublicIP",
            "metadata": {
                "description": "Public IP name"
            }
        },
        "vnetName": {
            "type": "string",
            "defaultValue": "myVNET",
            "metadata": {
                "description": "Virtual network name"
            }
        },
        "vmSize": {
            "type": "string",
            "defaultValue": "Standard_A1",
            "metadata": {
                "description": "Size of hello VM"
            }
        }
    },
    "variables": {
        "storageAccountType": "Standard_LRS",
        "vmStorageAccountContainerName": "vhds",
        "availabilitySetName": "myAvSet",
        "addressPrefix": "10.0.0.0/16",
        "subnetName": "Subnet-1",
        "subnetPrefix": "10.0.0.0/24",
        "publicIPAddressType": "Dynamic",
        "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',parameters('vnetName'))]",
        "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables ('subnetName'))]",
        "publicIPAddressID": "[resourceId('Microsoft.Network/publicIPAddresses',parameters('publicIPAddressName'))]",
        "lbID": "[resourceId('Microsoft.Network/loadBalancers',parameters('lbName'))]",
        "numberOfInstances": 2,
        "nicId1": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 0))]",
        "nicId2": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'), 1))]",
        "frontEndIPConfigID": "[concat(variables('lbID'),'/frontendIPConfigurations/LBFE')]",
        "backEndIPConfigID1": "[concat(variables('nicId1'),'/ipConfigurations/ipconfig1')]",
        "backEndIPConfigID2": "[concat(variables('nicId2'),'/ipConfigurations/ipconfig1')]",
        "sourceImageName": "[concat('/', subscription().subscriptionId,'/services/images/',parameters('vmSourceImageName'))]",
        "lbPoolID": "[concat(variables('lbID'),'/backendAddressPools/LBBE')]",
        "lbProbeID": "[concat(variables('lbID'),'/probes/tcpProbe')]"
    },
    "resources": [
        {
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[parameters('storageAccountName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": {
                "accountType": "[variables('storageAccountType')]"
            }
        },
        {
            "type": "Microsoft.Compute/availabilitySets",
            "name": "[variables('availabilitySetName')]",
            "apiVersion": "2015-05-01-preview",
            "location": "[parameters('location')]",
            "properties": { }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/publicIPAddresses",
            "name": "[parameters('publicIPAddressName')]",
            "location": "[parameters('location')]",
            "properties": {
                "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
                "dnsSettings": {
                    "domainNameLabel": "[parameters('dnsNameforLBIP')]"
                }
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/virtualNetworks",
            "name": "[parameters('vnetName')]",
            "location": "[parameters('location')]",
            "properties": {
                "addressSpace": {
                    "addressPrefixes": [
                        "[variables('addressPrefix')]"
                    ]
                },
                "subnets": [
                    {
                        "name": "[variables('subnetName')]",
                        "properties": {
                            "addressPrefix": "[variables('subnetPrefix')]"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Network/networkInterfaces",
            "name": "[concat(parameters('nicNamePrefix'), copyindex())]",
            "location": "[parameters('location')]",
            "copy": {
                "name": "nicLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "dependsOn": [
                "[concat('Microsoft.Network/virtualNetworks/', parameters('vnetName'))]"
            ],
            "properties": {
                "ipConfigurations": [
                    {
                        "name": "ipconfig1",
                        "properties": {
                            "privateIPAllocationMethod": "Dynamic",
                            "subnet": {
                                "id": "[variables('subnetRef')]"
                            }
                        },
                        "loadBalancerBackendAddressPools": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/backendAddressPools/LBBE')]"
                            }
                        ],
                        "loadBalancerInboundNatRules": [
                            {
                                "id": "[concat('Microsoft.Network/loadBalancers/',parameters('lbName'),'/inboundNatRule/RDP-VM', copyindex())]"
                            }
                        ]


                    }
                ]

            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "name": "[parameters('lbName')]",
            "type": "Microsoft.Network/loadBalancers",
            "location": "[parameters('location')]",
            "dependsOn": [
                "nicLoop",
                "[concat('Microsoft.Network/publicIPAddresses/', parameters('publicIPAddressName'))]"
            ],
            "properties": {
                "frontendIPConfigurations": [
                    {
                        "name": "LBFE",
                        "properties": {
                            "publicIPAddress": {
                                "id": "[variables('publicIPAddressID')]"
                            }
                        }
                    }
                ],
                "backendAddressPools": [
                    {
                        "name": "LBBE"

                    }
                ],
                "inboundNatRules": [
                    {
                        "name": "RDP-VM1",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50001,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    },
                    {
                        "name": "RDP-VM2",
                        "properties": {
                            "frontendIPConfiguration":
                                {
                                    "id": "[variables('frontEndIPConfigID')]"
                                },
                            "protocol": "tcp",
                            "frontendPort": 50002,
                            "backendPort": 3389,
                            "enableFloatingIP": false
                        }
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "LBRule",
                        "properties": {
                            "frontendIPConfiguration": {
                                "id": "[variables('frontEndIPConfigID')]"
                            },
                            "backendAddressPool": {
                                "id": "[variables('lbPoolID')]"
                            },
                            "protocol": "tcp",
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 5,
                            "probe": {
                                "id": "[variables('lbProbeID')]"
                            }
                        }
                    }
                ],
                "probes": [
                    {
                        "name": "tcpProbe",
                        "properties": {
                            "protocol": "tcp",
                            "port": 80,
                            "intervalInSeconds": "5",
                            "numberOfProbes": "2"
                        }
                    }
                ]
            }
        },
        {
            "apiVersion": "2015-05-01-preview",
            "type": "Microsoft.Compute/virtualMachines",
            "name": "[concat(parameters('vmNamePrefix'), copyindex())]",
            "copy": {
                "name": "virtualMachineLoop",
                "count": "[variables('numberOfInstances')]"
            },
            "location": "[parameters('location')]",
            "dependsOn": [
                "[concat('Microsoft.Storage/storageAccounts/', parameters('storageAccountName'))]",
                "[concat('Microsoft.Network/networkInterfaces/', parameters('nicNamePrefix'), copyindex())]",
                "[concat('Microsoft.Compute/availabilitySets/', variables('availabilitySetName'))]"
            ],
            "properties": {
                "availabilitySet": {
                    "id": "[resourceId('Microsoft.Compute/availabilitySets',variables('availabilitySetName'))]"
                },
                "hardwareProfile": {
                    "vmSize": "[parameters('vmSize')]"
                },
                "osProfile": {
                    "computername": "[concat(parameters('vmNamePrefix'), copyIndex())]",
                    "adminUsername": "[parameters('adminUsername')]",
                    "adminPassword": "[parameters('adminPassword')]"
                },
                "storageProfile": {
                    "sourceImage": {
                        "id": "[variables('sourceImageName')]"
                    },
                    "destinationVhdsContainer": "[concat('http://',parameters('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/')]"
                },
                "networkProfile": {
                    "networkInterfaces": [
                        {
                            "id": "[resourceId('Microsoft.Network/networkInterfaces',concat(parameters('nicNamePrefix'),copyindex()))]"
                        }
                    ]
                }
            }
        }
    ]
}
```

### <a name="step-2-create-hello-deployment-by-using-hello-template"></a>2. lépés: Hello központi telepítés létrehozásához hello sablon használatával
Hozzon létre egy erőforráscsoportot hello sablon használatával `azure group create <location>`. Ezután hozzon létre egy erőforráscsoport szolgáltatássablonjaikat használatával `azure group deployment create` hello erőforráscsoport sikeres, majd átadja a központi telepítés nevét, és a hello kérdések megválaszolásával hello sablont, amely nem volt alapértelmezett értékeket a paraméterek.

```azurecli
azure group create lbgroup westus
info:    Executing command group create
+ Getting resource group lbgroup
+ Creating resource group lbgroup
info:    Created resource group lbgroup
data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/lbgroup
data:    Name:                lbgroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags:
data:
info:    group create command OK
```

Most már használja a hello `azure group deployment create` parancs és hello `--template-uri` beállítás toodeploy hello sablont. Készítse elő a paraméterértékeket, hogy megadja őket, amikor a rendszer kéri, ahogyan az az alábbiakban látható.

```azurecli
azure group deployment create \
> --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json \
> lbgroup \
> newdeployment
info:    Executing command group deployment create
info:    Supply values for hello following parameters
location: westus
newStorageAccountName: storagename
adminUsername: ops
adminPassword: password
dnsNameforLBIP: lbdomainname
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "newdeployment"
+ Registering providers
info:    Registering provider microsoft.storage
info:    Registering provider microsoft.compute
info:    Registering provider microsoft.network
+ Waiting for deployment toocomplete
data:    DeploymentName     : newdeployment
data:    ResourceGroupName  : lbgroup
data:    ProvisioningState  : Succeeded
data:    Timestamp          : 2015-04-28T20:58:40.1678876Z
data:    Mode               : Incremental
data:    TemplateLink       : https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-2-vms-loadbalancer-lbrules/azuredeploy.json
data:    ContentVersion     : 1.0.0.0
data:    Name                   Type          Value
data:    ---------------------  ------------  ----------------------
data:    location               String        westus
data:    newStorageAccountName  String        storagename
data:    adminUsername          String        ops
data:    adminPassword          SecureString  undefined
data:    dnsNameforLBIP         String        lbdomainname
data:    backendPort            Int           3389
data:    vmNamePrefix           String        myVM
data:    imagePublisher         String        MicrosoftWindowsServer
data:    imageOffer             String        WindowsServer
data:    imageSKU               String        2012-R2-Datacenter
data:    lbName                 String        myLB
data:    nicNamePrefix          String        nic
data:    publicIPAddressName    String        myPublicIP
data:    vnetName               String        myVNET
data:    vmSize                 String        Standard_A1
info:    group deployment create command OK
```

Ez a sablon egy Windows Server rendszerképet telepít, de könnyen helyettesíthető lenne bármilyen Linux rendszerképpel is. Szeretné, hogy több swarm felügyelőkkel rendelkező toocreate egy Docker-fürtöt? [Megteheti](https://azure.microsoft.com/documentation/templates/docker-swarm-cluster/).

## <a id="remove-a-resource-group"></a>Feladat: Erőforráscsoport eltávolítása
Ne feledje, hogy tooa erőforráscsoport központilag telepítheti, de ha elkészült, az egyik, törölheti azt használatával `azure group delete <group name>`.

```azurecli
azure group delete myResourceGroup
info:    Executing command group delete
Delete resource group myResourceGroup? [y/n] y
+ Deleting resource group myResourceGroup
info:    group delete command OK
```

## <a id="show-the-log-for-a-resource-group-deployment"></a>Feladat: Hello keresse meg a központi telepítés erőforrás csoport megjelenítése
Ez azonos a sablonok létrehozásakor és használatakor. hello hívás toodisplay hello telepítési naplói a csoport `azure group log show <groupname>`, amely jeleníti meg, amely akkor hasznos, ha ismertetése, hogy valami történt –, vagy nem elég a bit. (Az üzemelő példányok hibaelhárításával, illetve általánosságban a hibákkal kapcsolatban az [Azure-telepítések gyakori hibáinak elhárítása az Azure Resource Managerrel](../articles/azure-resource-manager/resource-manager-common-deployment-errors.md) témájú cikkben talál további információt.)

tootarget vonatkozó hibákat, például használhatja a hasonló eszközök **jq** tooquery dolgot egy kicsit nagyobb pontosan, mely az egyes hibák például szüksége toocorrect. hello alábbi példában **jq** a központi telepítés keresse meg a tooparse **lbgroup**, akik szeretnének hibák.

```azurecli
azure group log show lbgroup -l --json | jq '.[] | select(.status.value == "Failed") | .properties'
```
A problémát gyorsan felismerheti, elháríthatja, majd megpróbálkozhat a kérdéses művelet újbóli végrehajtásával. A következő esetben hello, hello sablon kellett lett létrehozása két virtuális gépek hello zárolást létre hello .vhd egy időben. (Hello sablon módosítása azt, után hello sikeres telepítés gyorsan.)

```json
{
    "statusCode": "Conflict",
    "statusMessage": "{\"status\":\"Failed\",\"error\":{\"code\":\"ResourceDeploymentFailure\",\"message\":\"hello resource operation completed with terminal provisioning state 'Failed'.\",\"details\":[{\"code\":\"AcquireDiskLeaseFailed\",\"message\":\"Failed tooacquire lease while creating disk 'osdisk' using blob with URI http://storage.blob.core.windows.net/vhds/osdisk.vhd.\"}]}}"
}
```

## <a id="display-information-about-a-virtual-machine"></a>Feladat: Virtuális gépre vonatkozó információk megjelenítése
Megtekintheti adott virtuális gépek kapcsolatos információkat az erőforráscsoportban hello segítségével `azure vm show <groupname> <vmname>` parancsot. Ha egynél több virtuális gép a csoportban található, először meg kell egy csoport a virtuális gépek toolist hello segítségével `azure vm list <groupname>`.

```azurecli
azure vm list zoo
info:    Executing command vm list
+ Getting virtual machines
data:    Name   ProvisioningState  Location  Size
data:    -----  -----------------  --------  -----------
data:    myVM0  Succeeded          westus    Standard_A1
data:    myVM1  Failed             westus    Standard_A1
```

Ezután keresheti meg a myVM1-et:

```azurecli
azure vm show zoo myVM1
info:    Executing command vm show
+ Looking up hello VM "myVM1"
+ Looking up hello NIC "nic1"
data:    Id                              :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/virtualMachines/myVM1
data:    ProvisioningState               :Failed
data:    Name                            :myVM1
data:    Location                        :westus
data:    Type                            :Microsoft.Compute/virtualMachines
data:
data:    Hardware Profile:
data:      Size                          :Standard_A1
data:
data:    Storage Profile:
data:      Image reference:
data:        Publisher                   :MicrosoftWindowsServer
data:        Offer                       :WindowsServer
data:        Sku                         :2012-R2-Datacenter
data:        Version                     :latest
data:
data:      OS Disk:
data:        OSType                      :Windows
data:        Name                        :osdisk
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :http://zoostorageralph.blob.core.windows.net/vhds/osdisk.vhd
data:
data:    OS Profile:
data:      Computer Name                 :myVM1
data:      User Name                     :ops
data:      Windows Configuration:
data:        Provision VM Agent          :true
data:        Enable automatic updates    :true
data:
data:    Network Profile:
data:      Network Interfaces:
data:        Network Interface #1:
data:          Id                        :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Network/networkInterfaces/nic1
data:          Primary                   :false
data:          Provisioning State        :Succeeded
data:          Name                      :nic1
data:          Location                  :westus
data:            Private IP alloc-method :Dynamic
data:            Private IP address      :10.0.0.5
data:
data:    AvailabilitySet:
data:      Id                            :/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/zoo/providers/Microsoft.Compute/availabilitySets/MYAVSET
info:    vm show command OK
```

> [!NOTE]
> Ha szeretné, hogy tooprogrammatically tároló, és kezelheti a konzol parancsok hello kimenetét, érdemes lehet például egy JSON-elemző eszköz toouse  **[jq](https://github.com/stedolan/jq)**  vagy  **[jsawk](https://github.com/micha/jsawk)** , vagy nyelvi szalagtár szerepel, amely jó hello feladathoz.
>
>

## <a id="log-on-to-a-linux-based-virtual-machine"></a>Feladat: Bejelentkezés tooa Linux-alapú virtuális gépek
Linux-gépek általában csatlakoztatott toothrough SSH. További információkért lásd: [hogyan toouse Azure Linux SSH](../articles/virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a id="stop-a-virtual-machine"></a>Feladat: Virtuális gép leállítása
Futtassa ezt a parancsot:

```azurecli
azure vm stop <group name> <virtual machine name>
```

> [!IMPORTANT]
> Ez a paraméter tookeep hello virtuális IP-cím (VIP) hello vnet használja abban az esetben, ha az utolsó virtuális gép a vnetben hello. <br><br> Ha hello `StayProvisioned` paraméter, akkor fogjuk továbbra is számlázni hello virtuális gép.
>
>

## <a id="start-a-virtual-machine"></a>Feladat: Virtuális gép indítása
Futtassa ezt a parancsot:

```azurecli
azure vm start <group name> <virtual machine name>
```

## <a id="attach-a-data-disk"></a>Feladat: Adatlemez csatolása
Meg kell toodecide e tooattach új lemez vagy egy tartalmazó adatokat. Egy új lemez hello parancs hello .vhd fájlt hoz létre, és csatolja a hello ugyanezt a parancsot.

egy új lemezt tooattach futtassa ezt a parancsot:

```azurecli
    azure vm disk attach-new <resource-group> <vm-name> <size-in-gb>
```

egy meglévő adatlemez tooattach futtassa ezt a parancsot:

```azurecli
azure vm disk attach <resource-group> <vm-name> [vhd-url]
```

Majd kell toomount hello lemez, a szokásos módon a Linux.

## <a name="next-steps"></a>Következő lépések
Amennyiben további példák a hello az Azure parancssori felület használatának **arm** mód, lásd: [Using hello Azure parancssori felület Mac, Linux és a Windows Azure Resource Manager](../articles/xplat-cli-azure-resource-manager.md). További információ az Azure-erőforrások és a fogalmakat toolearn lásd [Azure Resource Manager áttekintése](../articles/azure-resource-manager/resource-group-overview.md).

További felhasználható sablonokat az [Azure-gyorssablonok](https://azure.microsoft.com/documentation/templates/) és a [Sablonokat használó alkalmazás-keretrendszerek](../articles/virtual-machines/linux/app-frameworks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) között talál.
