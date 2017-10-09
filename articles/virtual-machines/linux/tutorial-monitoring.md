---
title: "aaaMonitor Linux virtuális gépek Azure-ban |} Microsoft Docs"
description: "Ismerje meg, hogyan toomonitor rendszerindítási diagnosztika és a metrikák egy Linux virtuális gép az Azure-ban"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: davidmu
ms.custom: mvc
ms.openlocfilehash: 282da0f03ab0bf37bd44750c22813ca8d1c89560
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-a-linux-virtual-machine-in-azure"></a>Hogyan toomonitor egy Linux virtuális gép az Azure-ban

tooensure megfelelően fut a virtuális gépek (VM) az Azure-ban, a rendszerindítási diagnosztika és a metrikák tudja ellenőrizni. Eben az oktatóanyagban az alábbiakkal fog megismerkedni:

> [!div class="checklist"]
> * Rendszerindítási diagnosztika a virtuális gép hello engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Gazdagép-metrikák megtekintése
> * A virtuális gép hello diagnosztika bővítmény engedélyezése
> * Nézet VM metrikák
> * A diagnosztikai mérőszámok alapján figyelmeztetések létrehozása
> * Speciális figyelés beállítása


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, ez az oktatóanyag van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

## <a name="create-vm"></a>Virtuális gép létrehozása

toosee diagnosztika és a művelet a metrikák kell egy virtuális Gépet. Először hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create). hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroupMonitor* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroupMonitor --location eastus
```

Most létrehozza a virtuális gép és [az virtuális gép létrehozása](https://docs.microsoft.com/cli/azure/vm#create). hello alábbi példakód létrehozza a virtuális gépek nevű *myVM*:

```azurecli-interactive 
az vm create \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

## <a name="enable-boot-diagnostics"></a>Rendszerindítási diagnosztika engedélyezése

Linux virtuális gépek rendszerindító, mivel a hello rendszerindítási diagnosztikai bővítmény rendszerindító kimeneti rögzíti, és azt az Azure storage tárolja. Ezek az adatok használt tootroubleshoot virtuális gép rendszerindítási problémák lehetnek. Rendszerindítási diagnosztika nem automatikusan engedélyezve lesznek hello Azure CLI használata Linux virtuális gép létrehozása.

Ahhoz, hogy a rendszerindítási diagnosztika, egy tárfiókot meg kell toobe létrehozott rendszerindító naplók tárolásához. Storage-fiókok 3 és 24 karakter között lehet globálisan egyedi névvel kell rendelkeznie, és csak számokat és kisbetűket tartalmazhat. Hozzon létre egy tárfiókot hello [az storage-fiók létrehozása](/cli/azure/storage/account#create) parancsot. Ebben a példában a véletlenszerű karakterlánc használt toocreate egy egyedi tárfiók neve. 

```azurecli-interactive 
storageacct=mydiagdata$RANDOM

az storage account create \
  --resource-group myResourceGroupMonitor \
  --name $storageacct \
  --sku Standard_LRS \
  --location eastus
```

Ha engedélyezve van a rendszerindítási diagnosztika, hello URI toohello blob storage tárolóban van szükség. hello következő parancs lekérdezések hello tárolási fiók tooreturn ezt az URI. hello URI azonosítóját tárolja a változók nevében *bloburi*, amellyel hello következő lépésben.

```azurecli-interactive 
bloburi=$(az storage account show --resource-group myResourceGroupMonitor --name $storageacct --query 'primaryEndpoints.blob' -o tsv)
```

Most már engedélyezheti a rendszerindítási diagnosztika a [az virtuális gép rendszerindítási-diagnosztika engedélyezése](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#enable). Hello `--storage` értéke hello blob URI gyűjtött hello előző lépésben.

```azurecli-interactive 
az vm boot-diagnostics enable \
  --resource-group myResourceGroupMonitor \
  --name myVM \
  --storage $bloburi
```


## <a name="view-boot-diagnostics"></a>Rendszerindítási diagnosztika megtekintése

Rendszerindítási diagnosztika, ha engedélyezve vannak minden alkalommal, állítsa le és indítsa el a virtuális gép, hello hello rendszerindítási folyamat információ íródik tooa naplófájlt. Ebben a példában az első felszabadítani a hello a virtuális gép hello [az virtuális gép felszabadítása](/cli/azure/vm#deallocate) parancsot a következőképpen:

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupMonitor --name myVM
```

Most hello VM kezdődnie hello [az vm indítása]( /cli/azure/vm#stop) parancsot a következőképpen:

```azurecli-interactive 
az vm start --resource-group myResourceGroupMonitor --name myVM
```

Hello rendszerindítási diagnosztikai adatokat a kaphat *myVM* hello a [az virtuális gép rendszerindítási-diagnosztika get-rendszerindítási-naplófájl](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics#get-boot-log) parancsot a következőképpen:

```azurecli-interactive 
az vm boot-diagnostics get-boot-log --resource-group myResourceGroupMonitor --name myVM
```


## <a name="view-host-metrics"></a>Gazdagép-metrikák megtekintése

A Linux virtuális gépek dedikált-állomással rendelkezik, amely hatással van az Azure-ban. Metrikák automatikusan begyűjtött hello állomás, és megtekinthetők az Azure-portálon hello az alábbiak szerint:

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroupMonitor**, majd válassza ki **myVM** hello erőforrás listában.
1. toosee hogyan működik-e hello állomást a virtuális Gépet, kattintson a **metrikák** hello VM panelen, majd válassza ki valamelyik hello *[állomás]* mérőszámok alapján **elérhető**.

    ![Gazdagép-metrikák megtekintése](./media/tutorial-monitoring/monitor-host-metrics.png)


## <a name="install-diagnostics-extension"></a>Diagnosztika-kiterjesztés telepítése

> [!IMPORTANT]
> Ez a dokumentum ismerteti a hello Linux diagnosztikai bővítmény, amely elavult 2.3 verzióját. 2.3 verziója lesz támogatott 2018. június 30-ig.
>
> Linux diagnosztikai bővítmény helyette engedélyezhető hello 3.0-s verziója. További információkért lásd: [dokumentáció hello](./diagnostic-extension.md).

hello alapvető gazdagép metrikák érhetők el, de a részletesebb toosee, és a Virtuálisgép-specifikus metrika, akkor tooneed tooinstall hello Azure diagnostics bővítményt a virtuális gép hello. hello Azure diagnostics bővítmény lehetővé teszi, hogy további felügyeleti és diagnosztikai adatok toobe hello VM lekért. A metrikák megtekintheti, és hozzon létre a riasztások alapján hogyan hello VM hajt végre. hello diagnosztikai telepítve van a bővítmény hello Azure-portálon keresztül az alábbiak szerint:

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
1. Kattintson a **diagnosztikai beállítások**. hello lista azt mutatja, hogy *rendszerindítási diagnosztika* már engedélyezve van az előző szakasz hello. Kattintson a megfelelő jelölőnégyzet hello *alapvető metrikák*.
1. A hello *tárfiók* területen válassza hello tooand Tallózás *mydiagdata [1234]* hello előző szakaszban létrehozott fiók.
1. Kattintson a hello **mentése** gombra.

    ![Nézet diagnosztikai metrikák](./media/tutorial-monitoring/enable-diagnostics-extension.png)


## <a name="view-vm-metrics"></a>Nézet VM metrikák

Hello VM metrikák hello megtekintheti, hogy megtekinthetők-e hello gazdagép VM metrikák ugyanúgy:

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
1. toosee hogyan hello VM hajt végre, kattintson a **metrikák** a hello VM panelen, majd válassza ki bármelyik hello diagnosztika mérőszámok alapján **elérhető**.

    ![Nézet VM metrikák](./media/tutorial-monitoring/monitor-vm-metrics.png)


## <a name="create-alerts"></a>Riasztások létrehozása

Riasztások adott mérőszámok alapján hozhat létre. Riasztás például akkor, amikor az átlagos CPU-használat meghaladja az egy bizonyos küszöb vagy a rendelkezésre álló szabad lemezterületet egy adott értékre, alá süllyed használt toonotify. Riasztások jelennek meg hello Azure-portálon, vagy e-mailben küldhetők el. A válasz tooalerts generálása Azure Automation-forgatókönyveket vagy Azure Logic Apps is kiválthatják.

hello alábbi példa riasztást hoz létre az átlagos CPU-használat.

1. Hello Azure-portálon, kattintson **erőforráscsoportok**, jelölje be **myResourceGroup**, majd válassza ki **myVM** hello erőforrás listában.
2. Kattintson a **riasztási szabályok** hello VM panelen, majd kattintson a **metrika riasztás hozzáadása** keresztül hello hello riasztások panel tetején.
4. Adjon meg egy **neve** a riasztás például *myAlertRule*
5. tootrigger riasztást, amikor a processzor 1.0 meghaladja 5 percig, hagyja összes hello többi kijelölt alapértelmezett értéket.
6. Szükség esetén jelölje be a hello jelölőnégyzetet *E-mail-tulajdonosok, közreműködőknek és olvasóknak* toosend e-mailben értesítést. hello alapértelmezett művelete toopresent hello portálon értesítést.
7. Kattintson a hello **OK** gombra.


## <a name="advanced-monitoring"></a>Speciális figyelés 

Fejlettebb, figyelés, a virtuális gép segítségével teheti [Operations Management Suite](https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview). Ha még nem tette meg, akkor regisztrálhatnak az egy [ingyenes próbaverzió](https://www.microsoft.com/en-us/cloud-platform/operations-management-suite-trial) az Operations Management Suite szolgáltatásban.

Ha a hozzáférési toohello OMS-portálon, hello munkaterület kulcs és a munkaterület-azonosítót a hello-beállítások panelen található. A név felülírandó < munkaterület-kulcs > és < munkaterület-azonosítója > értékekkel hello az OMS a munkaterületet, és ezután használhatja **az virtuálisgép-bővítmény készlet** tooadd hello OMS bővítmény toohello VM:

```azurecli-interactive 
az vm extension set \
  --resource-group myResourceGroupMonitor \
  --vm-name myVM \
  --name OmsAgentForLinux \
  --publisher Microsoft.EnterpriseCloud.Monitoring \
  --version 1.3 \
  --protected-settings '{"workspaceKey": "<workspace-key>"}' \
  --settings '{"workspaceId": "<workspace-id>"}'
```

Hello OMS-portálon hello napló keresése paneljén láthatja *myVM* például hello az alábbi képen látható:

![OMS panel](./media/tutorial-monitoring/tutorial-monitor-oms.png)

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban konfigurálva, és tekintse át a virtuális gépek az Azure Security Center. Megismerte, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Rendszerindítási diagnosztika a virtuális gép hello engedélyezése
> * Rendszerindítási diagnosztika megtekintése
> * Gazdagép-metrikák megtekintése
> * A virtuális gép hello diagnosztika bővítmény engedélyezése
> * Nézet VM metrikák
> * A diagnosztikai mérőszámok alapján figyelmeztetések létrehozása
> * Speciális figyelés beállítása

Az Azure security Centerrel kapcsolatos útmutató toolearn következő toohello előzetes.

> [!div class="nextstepaction"]
> [A virtuális gépek biztonságának kezelése](./tutorial-azure-security.md)