---
title: "Linux virtuális gép lemezképét aaaCapture |} Microsoft Docs"
description: "Ismerje meg, hogyan toocapture a Linux-alapú Azure virtuális gépek (VM) lemezkép létre hello klasszikus üzembe helyezési modellben."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: 33c4059d5bb919a86bdc3492abca540750f365ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocapture-a-classic-linux-virtual-machine-as-an-image"></a>Hogyan toocapture egy klasszikus Linuxos virtuális gép képként
> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja. Ismerje meg, hogyan túl[hello Resource Manager modellt használja a következő lépésekkel](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Ez a cikk bemutatja, hogyan toocapture a klasszikus Azure virtuális gép (VM) egy kép toocreate Linux futtató más virtuális gépekkel. Ez a rendszerkép tartalmazza hello operációsrendszer-lemez, és adatlemezt csatolni toohello virtuális gép. Hálózati konfiguráció nem tartoznak bele, tooconfigure van szüksége, amely létrehozásakor hello más virtuális gép hello lemezképéről.

Azure tárolók hello lemezkép **képek**, feltöltött képeket együtt. Lemezképek kapcsolatos további információkért lásd: [kapcsolatos virtuálisgép-lemezképeket az Azure-ban][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Előkészületek
Ezek a lépések feltételezik, hogy már létrehozott egy Azure virtuális gép hello klasszikus telepítési modell és a beállított hello operációs rendszer, beleértve a bármely adatlemezt csatolni. Ha egy virtuális gép toocreate van szüksége, olvassa el a [hogyan tooCreate Linux virtuális gépek][How tooCreate a Linux Virtual Machine].

## <a name="capture-hello-virtual-machine"></a>Hello virtuális gép rögzítése
1. [Csatlakozás a virtuális gép toohello](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) egy SSH-ügyfél az Ön által választott használatával.
2. Hello SSH ablak írja be a következő parancs hello. hello kimenetét `waagent` függően eltérőek lehetnek attól függően, hogy ezt a segédprogramot hello verziója:

    ```bash
    sudo waagent -deprovision+user
    ```

    hello előző parancs kísérletek tooclean hello rendszer és a megfelelő reprovisioning. Ez a művelet a következő feladatok hello hajtja végre:

   * SSH-állomások kulcsait eltávolítja (ha Provisioning.RegenerateSshHostKeyPair "y" hello konfigurációs fájlban)
   * Törli a /etc/resolv.conf névkiszolgáló-konfiguráció
   * Eltávolítja a hello `root` a felhasználó jelszava a/etc/árnyékmásolat (ha Provisioning.DeleteRootPassword "y" hello konfigurációs fájlban)
   * A gyorsítótárazott DHCP-ügyfélbérletek eltávolítása
   * Alaphelyzetbe állítását gazdagép neve toolocalhost.localdomain
   * Törli a hello (/var/lib/waagent nyert) utolsó kiépített felhasználói fiókot **és az adatok**.

     > [!NOTE]
     > Megszüntetés törli a fájlokat és adatokat túl "generalize" hello kép. A parancs egy virtuális gépen, hogy szeretné-e új lemezkép sablonként toocapture csak fut. Hello lemezkép minden a bizalmas adatok megszűnik, vagy alkalmas Újraterjesztési toothird felek nem garantálja.

3. Típus **y** toocontinue. Hozzáadhat hello `-force` paraméter tooavoid a megerősítési lépés.
4. Típus **kilépési** tooclose hello SSH-ügyfél.

   > [!NOTE]
   > hello fennmaradó lépések során feltételezzük, hogy már [hello Azure parancssori felület telepítve](../../../cli-install-nodejs.md) az ügyfélszámítógépen. A következő lépéseket az összes hello is elvégezhető a hello [Azure-portálon](http://portal.azure.com).

5. Az ügyfélszámítógépen nyissa meg az Azure parancssori felület és a bejelentkezési tooyour Azure-előfizetés. További információkért olvassa el a [hello Azure CLI Azure-előfizetés tooan kapcsolódó](../../../xplat-cli-connect.md).

   > [!NOTE]
   > Hello Azure-portálon, a bejelentkezés toohello portálon.

6. Ellenőrizze, hogy a szolgáltatás felügyeleti mód:

    ```azurecli
    azure config mode asm
    ```

7. Állítsa le a virtuális Gépet, amely már platformelőfizetés hello. hello alábbi példa leáll hello nevű virtuális gép `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Ha szükséges, az előfizetés használatával létrehozott összes hello virtuális gépek listáját megtekintheti`azure vm list`

   > [!NOTE]
   > Hello Azure portál használata, válassza ki a virtuális gép hello, és kattintson a **leállítása** hello VM leállítása.

8. Hello virtuális gép leállítását követően hello lemezképének rögzítése. a következő példa rögzítésekre hello hello nevű virtuális gép `myVM` , és létrehoz egy általánosított nevű lemezkép `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    Hello `-t` törli az eredeti virtuális gép hello alparancs.

    > [!NOTE]
    > Hello Azure-portálon, rögzítheti képként kiválasztásával **kép** hello hub menüből. A következő hello lemezképének információit toosupply hello van szüksége: neve, a erőforráscsoport, a helye, a operációs rendszer típusa és a tárolási blob elérési útja.

9. hello új lemezkép már csak olyan lemezképkészlet, amellyel lehet hello listában használt tooconfigure bármely új virtuális Gépet. Ez a hello paranccsal tekintheti meg:

   ```azurecli
   azure vm image list
   ```

   A hello [Azure-portálon](http://portal.azure.com), hello jelenik meg hello **Virtuálisgép-rendszerképek (klasszikus)** toohello tartozó **számítási** szolgáltatások. Van-e hozzáférési **Virtuálisgép-rendszerképek (klasszikus)** kattintva _további szolgáltatások_ hello aljához hello Azure szolgáltatást a listában, majd a hello **számítási** szolgáltatások.   

   ![Lemezkép-rögzítési sikeres](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Következő lépések
hello kép készen toobe használt toocreate virtuális gépek. Hello Azure CLI paranccsal `azure vm create` és a létrehozott ellátási hello lemezkép nevét. További információkért lásd: [Using hello Azure CLI klasszikus telepítési modellel rendelkező](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Másik megoldásként használhatja a hello [Azure-portálon](http://portal.azure.com) toocreate hello segítségével egyéni virtuális gép **kép** módszer, majd válassza a hello létrehozott lemezképet. További információkért lásd: [hogyan tooCreate egy egyéni VM][How tooCreate a Custom Virtual Machine].

**További információ:** [Azure Linux ügynök felhasználói útmutatója](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How tooCreate a Custom Virtual Machine]:create-custom.md
[How tooAttach a Data Disk tooa Virtual Machine]:attach-disk.md
[How tooCreate a Linux Virtual Machine]:create-custom.md
