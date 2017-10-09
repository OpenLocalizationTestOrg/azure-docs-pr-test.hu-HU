---
title: "egy Azure Linux virtuális gép toouse sablonként aaaCapture |} Microsoft Docs"
description: "Megtudhatja, hogyan toocapture és a Linux-alapú Azure virtuális gépek (VM) hello Azure Resource Manager üzembe helyezési modellben létre kép generalize."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a>Azure-on futó Linux virtuális gép rögzítése
Kövesse a cikk toogeneralize hello lépéseit, és rögzítheti az Azure Linux virtuális gép (VM) hello Resource Manager üzembe helyezési modellben. Generalize hello VM, amikor eltávolítja a személyes fiók adatait, és készítse elő a hello VM toobe képként használni. Ekkor egy általánosított virtuális merevlemezt (VHD) hello az operációs rendszer, a mellékelt adatok lemez VHD-k a lemezkép rögzítése és egy [Resource Manager-sablon](../../azure-resource-manager/resource-group-overview.md) új virtuális gép központi telepítéséhez. Ez a cikk részletesen, hogyan toocapture egy virtuális gép rendszerkép hello Azure CLI 1.0 a virtuális gépek nem felügyelt lemezekkel. Emellett [rögzíteni a virtuális gépek Azure felügyelt lemezt az Azure CLI 2.0 hello](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Felügyelt lemezek hello Azure platform kezeli, és nem igényelnek bármely előkészítő vagy a hely toostore őket. További információ: [Azure Managed Disks – áttekintés](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). 

toocreate hello lemezképpel, virtuális gépek hálózati erőforrások beállítása az egyes új virtuális gépek, és azt hello rögzített VHD lemezképek hello sablon (JavaScript Object Notation vagy JSON-NÁ, fájl) toodeploy használja. Ezzel a módszerrel a virtuális gép és az aktuális szoftverkonfigurációt, hasonló toohello módon teszi lehetővé a képek a hello Azure piactér replikálhatja.

> [!TIP]
> Ha a meglévő Linux virtuális Gépet, a speciális állapottal másolatát toocreate biztonsági mentését vagy hibakeresés, lásd: [másolatot készít egy Azure-on futó Linux virtuális gép](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Ha azt szeretné, hogy tooupload Linux virtuális merevlemez egy helyszíni virtuális Gépre, tekintse meg és [feltöltése és a Linux virtuális gép létrehozása az egyéni lemezképet](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).  

## <a name="cli-versions-toocomplete-hello-task"></a>Parancssori felület verziók toocomplete hello feladat
Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:

- [Az Azure CLI 1.0](#before-you-begin) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)
- [Az Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell

## <a name="before-you-begin"></a>Előkészületek
Győződjön meg arról, hogy teljesülnek-e a következő előfeltételek hello:

* **Hello Resource Manager üzembe helyezési modellel létrehozott Azure virtuális gép** -Linux virtuális gép még nem hozott létre, ha használható hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vagy [erőforrás-kezelő sablonok](create-ssh-secured-vm-from-template.md). 
  
    Virtuális gép szükség szerint hello konfigurálása. Például [adatok lemezek hozzáadása a](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), frissítések és alkalmazások telepítéséhez. 
* **Az Azure CLI** -telepítés hello [Azure CLI](../../cli-install-nodejs.md) a helyi számítógépen.

## <a name="step-1-remove-hello-azure-linux-agent"></a>1. lépés: Hello Azure Linux ügynök eltávolítása
Először futtassa a hello **waagent** hello parancsot **deprovision** hello Linux virtuális gép paraméter. Ez a parancs törli a fájlokat és adatokat toomake hello virtuális gép készen áll a normalizálása. További információkért lásd: hello [Azure Linux ügynök felhasználói útmutató](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

1. Csatlakozás tooyour Linux virtuális gép SSH-ügyfél.
2. Hello SSH ablakban írja be a következő parancs hello:
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > A parancs egy virtuális gépen, hogy szeretné-e képként toocapture csak fut. Hello lemezkép törlődik a bizalmas információk, vagy alkalmas terjesztési nem garantálja.
 
3. Típus **y** toocontinue. Hozzáadhat hello **-force** paraméter tooavoid a megerősítési lépés.
4. Hello parancs után írja be a **kilépéshez**. Ez a lépés hello SSH-ügyfél bezárása után.

## <a name="step-2-capture-hello-vm"></a>2. lépés: Hello virtuális gép rögzítése
Hello Azure CLI toogeneralize használja, és hello virtuális gép rögzítése. Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit. Példa paraméter nevek a következők **myResourceGroup**, **myVnet**, és **myVM**.

1. A helyi számítógépről nyissa meg a hello Azure CLI és [bejelentkezési tooyour Azure-előfizetés](../../xplat-cli-connect.md). 
2. Győződjön meg arról, hogy az erőforrás-kezelő módban van.
   
    ```azurecli
    azure config mode arm
    ```
3. Állítsa le virtuális Gépet, amely már platformelőfizetés hello a következő parancs használatával hello:
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. A parancs a következő hello VM hello generalize:
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. Most futtassa a hello **azure virtuális gép rögzítése** parancs, mely rögzíti a virtuális gép hello. A következő példa hello, hello kép VHD-k a rendszer rögzíti-tól kezdődően nevek **MyVHDNamePrefix**, és hello **-t** beállítás adja meg egy elérési utat toohello sablon **MyTemplate.json**. 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > Alapértelmezés szerint a ugyanazt a tárfiókot, amely az eredeti virtuális gép hello használt hello hello kép VHD-fájlok létrehozása. Használjon hello *ugyanazt a tárfiókot* toostore hello bármely hello lemezképből hoz létre új virtuális gépek virtuális merevlemezeket. 

6. a rögzített lemezkép, egy szövegszerkesztőben megnyitott hello JSON-sablon toofind hello helye. A hello **storageProfile**, megkeresi hello **uri** a hello **kép** hello található **rendszer** tároló. Például hello URI-jának hello az operációs rendszer lemezképét hasonlít túl`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`

## <a name="step-3-create-a-vm-from-hello-captured-image"></a>3. lépés: Virtuális gép létrehozása hello rögzített lemezképből
Most már használja egy sablon toocreate Linux virtuális gép hello kép. Ezeket a lépéseket bemutatják, hogyan toouse hello Azure CLI-t, és egy új virtuális hálózat a virtuális gép toocreate hello rögzített JSON-fájl sablon hello.

### <a name="create-network-resources"></a>Hálózati erőforrások létrehozása
toouse hello sablon, először a virtuális hálózat és a hálózati adapter tooset az új virtuális gép számára. Javasoljuk, hogy hozzon létre egy erőforráscsoportot ezeket az erőforrásokat, a Virtuálisgép-lemezkép tároló hello helyen. Futtassa a következő, az erőforrások és a megfelelő Azure-beli hely (ezek a parancsok a "centralus" jelöli) tartozó nevek és hasonló toohello parancsokat:

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a>Első hello hello hálózati adapter azonosítója
toodeploy hello lemezkép rögzítése során mentett JSON hello segítségével a virtuális gép, kell hello azonosítója hello hálózati adaptert. Az beszerzése hello a következő parancs futtatásával:

```azurecli
azure network nic show myResourceGroup1 myNIC
```

Hello **azonosító** hello a kimeneti hasonlít túl`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`

### <a name="create-a-vm"></a>Virtuális gép létrehozása
Most futtassa hello következő parancsot a toocreate hello a virtuális gép rögzítése a Virtuálisgép-lemezkép. Használjon hello **-f** paraméter toospecify hello elérési toohello sablon JSON fájl mentése.

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

A hello parancs kimenetében rákérdezéses toosupply egy új virtuális gép nevét, hello rendszergazda felhasználónevet és jelszót, és hello hello korábban létrehozott hálózati adapter azonosítója.

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

a következő minta hello jeleníti meg, a sikeres telepítés lásd:

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-hello-deployment"></a>Hello telepítés ellenőrzése
Most SSH toohello virtuális gép tooverify hello üzembe helyezési és kezdő használatával létrehozott új virtuális gép hello. ssh, tooconnect hello IP-címének a hello hello a következő parancs futtatásával létrehozott virtuális Géphez:

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

hello nyilvános IP-cím szerepel hello parancs kimenetét. Alapértelmezés szerint csatlakozás toohello Linux virtuális gép által SSH 22-es porton.

## <a name="create-additional-vms"></a>További virtuális gépek létrehozása
Használjon hello rögzített rendszerkép és sablon toodeploy szakasz megelőző hello hello lépésekkel további virtuális gépeket. Egyéb beállítások toocreate virtuális gépek hello lemezképből tartalmaz gyors üzembe helyezés sablon használatával, illetve hello futtató **azure virtuális gép létrehozása** parancsot.

### <a name="use-hello-captured-template"></a>Hello rögzített sablon használata
toouse hello rögzített rendszerkép és a sablon, kövesse az alábbi lépéseket (amelynek részleteit a fenti szakaszban hello):

* Győződjön meg arról, hogy a Virtuálisgép-lemezkép hello ugyanazt a tárfiókot, amelyen a virtuális gép virtuális merevlemez.
* Hello sablon JSON-fájlt másolja, és adjon meg egy egyedi nevet az operációsrendszer-lemez hello hello új virtuális gép virtuális merevlemez (vagy VHD-k). Például a hello **storageProfile**a **vhd**, a **uri**, adjon meg egy egyedi nevet hello **osDisk** VHD, hasonló túl`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`
* Hozzon létre egy hálózati adapter vagy hello ugyanabban vagy egy másik virtuális hálózatot.
* Hello módosított sablon JSON-fájl használatával hozhat létre a központi telepítés hello erőforráscsoportban, amelyben beállítása hello virtuális hálózat.

### <a name="use-a-quickstart-template"></a>A következő gyorsindítási sablonon használata
Ha azt szeretné, hogy automatikusan létrehozásakor a virtuális gépek hello lemezképből hello hálózaton, egy sablon adhat meg ezeket az erőforrásokat. Lásd például: hello [101-vm-a-felhasználó-lemezkép sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) a Githubról. Ez a sablon egy virtuális Gépet hoz létre az egyéni lemezképet és a hello szükséges virtuális hálózati, a nyilvános IP-cím és a hálózati erőforrásokhoz. Az Azure-portálon hello hello-sablonnal útmutatást lásd: [hogyan toocreate virtuális gépek létrehozását a Resource Manager-sablon használatával egyéni lemezkép](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).

### <a name="use-hello-azure-vm-create-command"></a>Használja a hello azure virtuális gép létrehozása parancs
Általában azt a legegyszerűbb toouse a Resource Manager sablon toocreate hello lemezképből egy virtuális Gépet. Azonban létrehozhat hello VM *imperatively* hello segítségével **azure virtuális gép létrehozása** hello parancsot **-Q** (**--lemezkép-urn**) paraméter . Ha ezt a módszert használja, akkor is átadhatja hello **-d** (**--operációsrendszer-lemez-vhd**) paraméter toospecify hello hely az operációs rendszer hello .vhd fájl hello új virtuális Gépet. Ez a fájl hello kép VHD-fájlt tároló hello tárfiók hello a VHD-k tárolóban kell lennie. hello másolatok VHD hello hello parancs új virtuális gép automatikusan toohello **VHD-k** tároló.

Futtatása előtt **azure virtuális gép létrehozása** hello lemezképpel, végezze el a következő lépéseket hello:

1. Hozzon létre egy erőforráscsoportot, vagy egy meglévő erőforráscsoportot hello telepítéshez azonosítani.
2. Hozzon létre egy nyilvános IP-cím és a hálózati erőforrás hello új virtuális Gépet. Lépéseket toocreate egy virtuális hálózati, nyilvános IP-cím és hálózati hello CLI, használatával tekintse meg az ebben a cikkben. (**azure virtuális gép létrehozása** is létrehozhat egy hálózati Adaptert, de a virtuális hálózati és alhálózati kell toopass további paraméterek.)

Ezután futtassa a parancsot, kapott URI-azonosítók tooboth hello új operációs rendszer VHD-fájlt és a meglévő lemezkép hello. Ebben a példában a mérete Standard_A1 virtuális gép létrehozása a hello USA keleti régiójában.

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

A további parancssori kapcsolók, futtassa a `azure help vm create`.

## <a name="next-steps"></a>Következő lépések
toomanage hello CLI-t, a virtuális gépek, tekintse meg a hello feladatok [központi telepítése és virtuális gépek Azure Resource Manager-sablonok segítségével kezel, és az Azure parancssori felület hello](create-ssh-secured-vm-from-template.md).

