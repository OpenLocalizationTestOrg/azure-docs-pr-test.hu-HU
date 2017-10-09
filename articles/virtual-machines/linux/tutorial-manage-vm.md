---
title: "aaaCreate és a Linux virtuális gépek kezelése a hello Azure parancssori Felülettel |} Microsoft Docs"
description: "Az oktatóanyag - létrehozása és kezelése a Linux virtuális gépek a hello Azure parancssori felület"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a>Hozzon létre, és a Linux virtuális gépek kezelése a hello Azure parancssori felület

Az Azure virtuális gépek adjon meg egy teljes mértékben konfigurálhatók és rugalmas számítási környezet. Ez az oktatóanyag ismerteti alapszintű Azure-beli virtuális gép telepítési elemek, például Virtuálisgép-méret kiválasztása, Virtuálisgép-lemezkép kiválasztása és a virtuális gépek telepítése. Az alábbiak végrehajtásának módját ismerheti meg:

> [!div class="checklist"]
> * Hozzon létre, és csatlakozzon a virtuális gép tooa
> * Válassza ki és használja a Virtuálisgép-rendszerképek
> * Megtekintése és használata a megadott Virtuálisgép-méretek
> * Virtuális gép átméretezése
> * Megtekinteni és megérteni a virtuális gép állapota


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-resource-group"></a>Erőforráscsoport létrehozása

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](https://docs.microsoft.com/cli/azure/group#create) parancsot. 

Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat. Egy erőforráscsoportot a virtuális gépek előtt létre kell hozni. Ebben a példában az erőforráscsoport neve *myResourceGroupVM* jön létre az hello *eastus* régióban. 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

hello erőforráscsoport létrehozása vagy módosítása egy virtuális Gépet, amelynek ez az oktatóanyag teljes látható során van megadva.

## <a name="create-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create) parancsot. 

A virtuális gép létrehozásakor több lehetőség is elérhető, például az operációs rendszer lemezképét, a lemez méretezés és a felügyeleti hitelesítő adatokat. Ebben a példában egy virtuális gép létrehozása nevű *myVM* Ubuntu Server operációs rendszert futtató. 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

Egyszer hello a virtuális gép létrehozása, hello Azure CLI kimenete hello VM kapcsolatos információkat. Jegyezze fel a hello `publicIpAddress`, ez a cím lehet használt tooaccess hello virtuális géphez... 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-toovm"></a>Csatlakozás tooVM

Csatlakoztathatja toohello virtuális gép SSH használatával. Cserélje le hello példa IP-cím hello `publicIpAddress` hello előző lépésben feljegyzett.

```bash
ssh 52.174.34.95
```

Miután befejeződött a virtuális gép hello, zárja be a hello SSH-munkamenetet. 

```bash
exit
```

## <a name="understand-vm-images"></a>Virtuálisgép-rendszerképek ismertetése

hello Azure piactér számos rendszerképet, amely használt toocreate virtuális gépeket tartalmaz. Hello előző lépésekben egy virtuális gép létrehozása egy Ubuntu lemezképpel. Ebben a lépésben hello Azure CLI a CentOS képet, majd használt toosearch hello piactérre toodeploy egy második virtuális gép használja.  

hello listája toosee leggyakrabban használt képek, használja a hello [az vm képlistában](/cli/azure/vm/image#list) parancsot.

```azurecli-interactive 
az vm image list --output table
```

hello parancs kimenetében hello legnépszerűbb Virtuálisgép-rendszerképek az Azure ad vissza.

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

Teljes listája látható hello hozzáadásával `--all` argumentum. hello képlistában is alapján szűrhetők `--publisher` vagy `–-offer`. Ebben a példában az ajánlat, amely megfelel az összes képekkel szűrt hello lista *CentOS*. 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

Részleges kimenete:

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

egy virtuális gép egy adott lemezképpel toodeploy hello értéket vegye figyelembe hello *Urn* oszlop. Hello kép megadásakor hello kép verziószám lehet cserélni "legújabb" hello hello terjesztési legújabb verzióját választja ki, amelyek. Ebben a példában hello `--image` argumentum használt toospecify hello legújabb verzióját a CentOS 6.5-lemezkép.  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a>Virtuálisgép-méretek ismertetése

A virtuális gép méretét hello számítási erőforrásokat, elérhető toohello virtuális gépen végrehajtott például a CPU, grafikus Processzor és memória mennyisége határozza meg. Virtuális gépek várt hello munkaterhelés méretének toobe kell. Ha növeli a munkaterhelés, egy meglévő virtuális gépet is átméretezhetők.

### <a name="vm-sizes"></a>Virtuálisgép-méretek

a következő táblázat hello méretek kategorizálja a használati esetek.  

| Típus                     | Méretek           |    Leírás       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [Általános célú](sizes-general.md)         |Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7| Elosztott terhelésű CPU-memória. Épp ezért tökéletes választás a fejlesztési / tesztelési és kis toomedium alkalmazások és adatok megoldások.  |
| [Számításra optimalizált](sizes-compute.md)   | FS, F             | Magas CPU-memória. Jó közepes forgalom alkalmazásokat, hálózati berendezéseket és kötegelt folyamatok.        |
| [Memóriaoptimalizált](../virtual-machines-windows-sizes-memory.md)    | Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D   | Magas memória-core. A relációs adatbázisok, közepes méretű toolarge gyorsítótárak és memórián belüli analytics nagy.                 |
| [Tárolásra optimalizált](../virtual-machines-windows-sizes-storage.md)      | Ls                | Magas lemez-adatátviteli és I/O-műveleti jellemzők. Ideális Big Data-, SQL- és NoSQL-adatbázisok esetén.                                                         |
| [GPU](sizes-gpu.md)          | PORTOK HV, NC            | Nagy mennyiségű grafikus megjelenítési és videó szerkesztése szánt speciális virtuális gépeket.       |
| [Magas teljesítmény](sizes-hpc.md) | H, A8-11          | A leghatékonyabb CPU rendelkező virtuális gépek nagy átviteli további hálózati adapterek (RDMA). 


### <a name="find-available-vm-sizes"></a>Elérhető Virtuálisgép-méretek keresése

virtuális gép listájának toosee méretének egy adott régióban, használja a hello [az vm-méretek listázása](/cli/azure/vm#list-sizes) parancsot. 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

Részleges kimenete:

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a>Meghatározott méretű virtuális gép létrehozása

Hello előző virtuális gép létrehozása a példában a mérete nem adta meg, melyik eredmények alapértelmezett mérete. A Virtuálisgép-méret választható létrehozási ideje az [az virtuális gép létrehozása](/cli/azure/vm#create) és hello `--size` argumentum. 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a>Virtuális gép átméretezése

A virtuális gépek telepítése után az átméretezett tooincrease kell, vagy erőforrás-elosztás csökkentése.

A virtuális gépek átméretezésével előtt ellenőrizze, hogy hello kívánt mérete hello jelenlegi Azure-fürttel érhető el. Hello [az vm-vm-átméretezési-beállításai](/cli/azure/vm#list-vm-resize-options) parancs beolvasása hello méretének megtekintéséhez. 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
Hello szükséges méret érhető el, ha hello VM átméretezhető az alkalmazás bekapcsolja a állapotból, azonban hello művelet során újraindítása után. Használjon hello [az vm átméretezése]( /cli/azure/vm#resize) parancs tooperform hello méretezze át.

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

Hello méretének igény nincs fürtön hello aktuális hello virtuális gép igényeinek felszabadítása előtt hello átméretezése művelet toobe akkor fordulhat elő. Használjon hello [az virtuális gép felszabadítása]( /cli/azure/vm#deallocate) toostop parancsot, és hello VM felszabadítani. Vegye figyelembe, ha hello VM regisztráló vissza, eltávolíthatja a hello ideiglenes lemezen lévő adatokhoz. hello nyilvános IP-cím is megváltozik, kivéve, ha a statikus IP-cím használatban van. 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

Miután felszabadítása. lehetséges, hello átméretezési akkor fordulhat elő. 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

Miután hello átméretezéséhez hello VM indíthatók el.

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a>Virtuális gép energiaállapotokat

Egy Azure virtuális gép sok energiaállapotát egyike lehet. Ebben az állapotban hello hello virtuális gép aktuális állapotát jeleníti meg hello hipervizor hello tükrözik. 

### <a name="power-states"></a>Energiaállapotokat

| Energiaállapot | Leírás
|----|----|
| Indulás alatt | Azt jelzi, hogy hello virtuális gép elindul. |
| Fut | Azt jelzi, hogy hello a virtuális gép futása. |
| Leállítás | Azt jelzi, hogy hello a virtuális gép leáll. | 
| Leállítva | Azt jelzi, hogy hello a virtuális gép le van állítva. Hello leállt állapotban lévő virtuális gépek továbbra is számítási díjakat fel Önnek.  |
| Felszabadítása | Azt jelzi, hogy hello a virtuális gép felszabadítása. |
| Felszabadítása | Azt jelzi, hogy hello a virtuális gép hello hipervizor eltávolították, de továbbra is elérhetők a hello vezérlő vezérlősík. Hello Deallocated állapotban lévő virtuális gépek nem számítunk költségeket. |
| - | Azt jelzi, hogy hello power hello virtuális gép állapota ismeretlen. |

### <a name="find-power-state"></a>Energiaállapot keresése

egy adott virtuális Gépet, használjon hello tooretrieve hello állapotának [az virtuálisgép-példányokat tartalmazó nézet beolvasása](/cli/azure/vm#get-instance-view) parancsot. Lehet, hogy toospecify egy érvényes nevet a virtuális gép és erőforráscsoport. 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

Kimenet:

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a>Felügyeleti feladatok

Során hello életciklus-egy virtuális gép érdemes lehet toorun felügyeleti feladatokhoz, mint az indítása, leállítása vagy egy virtuális gép törlése. Emellett érdemes lehet toocreate parancsfájlok tooautomate ismétlődő vagy összetett feladatokat. Hello Azure parancssori felület használatával, számos gyakori felügyeleti feladatokat is futtatható hello parancssorból vagy parancsfájlokban. 

### <a name="get-ip-address"></a>IP-cím beszerzése

Ez a parancs visszaadja a hello privát és nyilvános IP-címek egy virtuális gép.  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a>Virtuális gép leállítása

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a>Virtuális gép elindítása

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a>Erőforráscsoport törlése

Erőforráscsoport törlésekor a belül található összes erőforrást is törlődnek.

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte alapvető virtuális gép létrehozása és kezelése például hogyan:

> [!div class="checklist"]
> * Hozzon létre, és csatlakozzon a virtuális gép tooa
> * Válassza ki és használja a Virtuálisgép-rendszerképek
> * Megtekintése és használata a megadott Virtuálisgép-méretek
> * Virtuális gép átméretezése
> * Megtekinteni és megérteni a virtuális gép állapota

Virtuális gép lemezeivel kapcsolatos útmutató toolearn következő toohello előzetes.  

> [!div class="nextstepaction"]
> [Hozzon létre és kezelheti a virtuális gépek lemezei](./tutorial-manage-disks.md)
