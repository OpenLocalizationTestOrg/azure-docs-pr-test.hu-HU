---
title: "hello Azure parancssori felület a lemezképek Linux virtuális gép aaaSelect |} Microsoft Docs"
description: "Ismerje meg, hogyan toouse hello Azure CLI toodetermine hello közzétevő, az ajánlat, SKU és verziója a piactér Virtuálisgép-lemezképek."
services: virtual-machines-linux
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7a858e38-4f17-4e8e-a28a-c7f801101721
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/24/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0b115b8654bc156b5bfadba53a6b002a105acb68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a>Hogyan toofind Linux virtuális gép az Azure piactér hello hello Azure parancssori felület a lemezképek
Ez a témakör ismerteti, hogyan toouse hello Azure piactér hello Azure CLI 2.0 toofind VM-lemezképekkel. Ezen információk toospecify Piactéri rendszerkép használata a Linux virtuális gép létrehozásakor.

Győződjön meg arról, hogy telepítette hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és tooan Azure-fiókkal van bejelentkezve (`az login`).

## <a name="terminology"></a>Terminológia

Piactéren elérhető rendszerkép hello CLI és más Azure-eszközök tooa hierarchia megfelelően azonosítja:

* **A Publisher** -hello hello lemezképét létrehozó szervezet. Példa: kanonikus
* **Ajánlat** -közzétevő által létrehozott kapcsolódó képek csoportja. Példa: Ubuntu Server
* **SKU** - ajánlatot, például egy terjesztési jelentős kiadása példányát. Példa: 16.04-es lts verzió
* **Verzió** -hello SKU lemezkép verziószámát. Hello kép megadásakor lecserélheti hello verziószáma a "legújabb" hello hello terjesztési legújabb verzióját választja ki, amelyek.

a Piactéri lemezkép toospecify, általában hello lemezképet használ *URN*. hello URN összevonja ezeket az értékeket, hello kettőspont (:) elválasztva: *Publisher*:*kínálnak*:*Sku*:*verzió*. 


## <a name="list-popular-images"></a>Népszerű lemezképek felsorolása

Hello futtatása [az vm képlistában](/cli/azure/vm/image#list) nélkül hello parancs `--all` beállítás, toosee hello Azure piactér a lemezképek népszerű VM listáját. Például futtassa a következő parancs toodisplay hello népszerű lemezképek gyorsítótárazott listáját táblázatos formátumú:

```azurecli
az vm image list --output table
```

hello kimenete hello URN (hello értéket hello *Urn* oszlop), amely toospecify hello lemezképet használ. Virtuális gép valamelyik, a népszerű piactér lemezképek létrehozásakor megadhatja hello URN alias, például a *UbuntuLTS*.

```
You are viewing an offline list of images, use --all tooretrieve an up-to-date list
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
...
```

## <a name="find-specific-images"></a>Adott rendszerképek keresése

toofind hello piactér, a megadott Virtuálisgép-lemezkép használata hello `az vm image list` hello parancsot `--all` lehetőséget. Hello parancs ezen verziója bizonyos idő toocomplete vesz igénybe, és visszatérhet a hosszadalmas kimeneti, általában szűrheti a listát hello `--publisher` vagy egy másik paraméter. 

Például a következő parancs hello megjeleníti minden Debian ajánlatok (ne feledje, hogy nélkül hello `--all` váltáshoz csak keres a helyi gyorsítótár hello közös képek):

```azurecli
az vm image list --offer Debian --all --output table 

```

Részleges kimenete: 
```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  --------------
Debian   credativ     7                  credativ:Debian:7:7.0.201602010                  7.0.201602010
Debian   credativ     7                  credativ:Debian:7:7.0.201603020                  7.0.201603020
Debian   credativ     7                  credativ:Debian:7:7.0.201604050                  7.0.201604050
Debian   credativ     7                  credativ:Debian:7:7.0.201604200                  7.0.201604200
Debian   credativ     7                  credativ:Debian:7:7.0.201606280                  7.0.201606280
Debian   credativ     7                  credativ:Debian:7:7.0.201609120                  7.0.201609120
Debian   credativ     7                  credativ:Debian:7:7.0.201611020                  7.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
Debian   credativ     8                  credativ:Debian:8:8.0.201708040                  8.0.201708040
...
```

A hello hasonló szűrők `--location`, `--publisher`, és `--sku` beállítások. Részleges egyezések is végezhető egy szűrővel – például keresése `--offer Deb` toofind minden Debian lemezképbe.

Ha nem adja meg egy adott helyen hello `--location` beállítás, hello értékeinek `westus` alapértelmezett által visszaadott. (Egy másik alapértelmezett hely beállítása futtatásával `az configure --defaults location=<location>`.)

Például a következő parancs hello felsorolja a Debian 8 termékváltozatokat a `westeurope`:

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

Részleges kimenete:

```
Offer    Publisher    Sku                Urn                                              Version
-------  -----------  -----------------  -----------------------------------------------  -------------
Debian   credativ     8                  credativ:Debian:8:8.0.201602010                  8.0.201602010
Debian   credativ     8                  credativ:Debian:8:8.0.201603020                  8.0.201603020
Debian   credativ     8                  credativ:Debian:8:8.0.201604050                  8.0.201604050
Debian   credativ     8                  credativ:Debian:8:8.0.201604200                  8.0.201604200
Debian   credativ     8                  credativ:Debian:8:8.0.201606280                  8.0.201606280
Debian   credativ     8                  credativ:Debian:8:8.0.201609120                  8.0.201609120
Debian   credativ     8                  credativ:Debian:8:8.0.201611020                  8.0.201611020
Debian   credativ     8                  credativ:Debian:8:8.0.201701180                  8.0.201701180
Debian   credativ     8                  credativ:Debian:8:8.0.201703150                  8.0.201703150
Debian   credativ     8                  credativ:Debian:8:8.0.201704110                  8.0.201704110
Debian   credativ     8                  credativ:Debian:8:8.0.201704180                  8.0.201704180
Debian   credativ     8                  credativ:Debian:8:8.0.201706190                  8.0.201706190
Debian   credativ     8                  credativ:Debian:8:8.0.201706210                  8.0.201706210
...
```

## <a name="navigate-hello-images"></a>Keresse meg a hello lemezképek 
Egy másik módja toofind helyen lemezkép toorun hello [az méretű kép lista-közzétevők](/cli/azure/vm/image#list-publishers), [az méretű kép lista-ajánlatok](/cli/azure/vm/image#list-offers), és [az méretű kép lista-SKU](/cli/azure/vm/image#list-skus) parancsok sorrendben. A következő parancsokkal ezek az értékek határozzák meg:

1. Lista hello kép közzétevők.
2. Listázza egy adott közzétevő ajánlatait.
3. Listázza egy adott ajánlathoz tartozó termékváltozatokat.


Például hello következő parancs megjeleníti az USA nyugati régiója hely hello hello kép közzétevők:

```azurecli
az vm image list-publishers --location westus --output table
```

Részleges kimenete:

```
Location    Name
----------  ----------------------------------------------------
westus      1e
westus      4psa
westus      7isolutions
westus      a10networks
westus      abiquo
westus      accellion
westus      Acronis
westus      Acronis.Backup
westus      actian_matrix
westus      actifio
westus      activeeon
westus      adatao
...
```
Ezen információk toofind biztosít egy adott közzétételi használja. Például ha Canonical egy hello USA nyugati régiója helye a kép közzétevőjének, keressék a futtatásával `azure vm image list-offers`. Hozzáférési hello hely és hello közzétevő, mint például a következő hello:

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

Kimenet:

```
Location    Name
----------  -------------------------
westus      Ubuntu15.04Snappy
westus      Ubuntu15.04SnappyDocker
westus      UbunturollingSnappy
westus      UbuntuServer
westus      Ubuntu_Core
westus      Ubuntu_Snappy_Core
westus      Ubuntu_Snappy_Core_Docker
```
Láthatja, hogy hello USA nyugati régiója régióban Canonical közzéteszi a hello **UbuntuServer** kínálnak az Azure-on. De milyen termékváltozatok? tooget ezeket az értékeket, futtassa `azure vm image list-skus` és hello helyét, kiadó és ajánlat, amely rendelkezik a felderített beállítása:

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

Kimenet:

```
Location    Name
----------  -----------------
westus      12.04.3-LTS
westus      12.04.4-LTS
westus      12.04.5-DAILY-LTS
westus      12.04.5-LTS
westus      12.10
westus      14.04.0-LTS
westus      14.04.1-LTS
westus      14.04.2-LTS
westus      14.04.3-LTS
westus      14.04.4-LTS
westus      14.04.5-DAILY-LTS
westus      14.04.5-LTS
westus      16.04-beta
westus      16.04-DAILY-LTS
westus      16.04-LTS
westus      16.04.0-LTS
westus      16.10
westus      16.10-DAILY
westus      17.04
westus      17.04-DAILY
westus      17.10-DAILY
```

Végül, használja a hello `az vm image list` parancs toofind hello SKU-t, például egy adott verziójához **16.04-es lts verzió**:

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

Kimenet:

```
Offer         Publisher    Sku        Urn                                               Version
------------  -----------  ---------  ------------------------------------------------  ---------------
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611220  16.04.201611220
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201611300  16.04.201611300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612050  16.04.201612050
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612140  16.04.201612140
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201612210  16.04.201612210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201701130  16.04.201701130
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702020  16.04.201702020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702200  16.04.201702200
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702210  16.04.201702210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201702240  16.04.201702240
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703020  16.04.201703020
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703030  16.04.201703030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703070  16.04.201703070
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703270  16.04.201703270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703280  16.04.201703280
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201703300  16.04.201703300
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705080  16.04.201705080
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201705160  16.04.201705160
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706100  16.04.201706100
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201706191  16.04.201706191
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707210  16.04.201707210
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201707270  16.04.201707270
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708030  16.04.201708030
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708110  16.04.201708110
UbuntuServer  Canonical    16.04-LTS  Canonical:UbuntuServer:16.04-LTS:16.04.201708151  16.04.201708151
```
## <a name="next-steps"></a>Következő lépések
Most kiválaszthatja, hogy pontosan hello kép kívánt toouse hello URN érték tudomásul véve. Ezt az értéket hello átadni `--image` paramétert, ha egy virtuális gép létrehozásához hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. Ne feledje, hogy opcionálisan lecserélheti hello verziószám hello URN a "legutóbbi". Ebben a verzióban mindig hello hello terjesztési legújabb verzióját. a virtuális gép hello URN információ segítségével gyorsan toocreate lásd: [létrehozása és kezelése a Linux virtuális gépet az Azure CLI hello](tutorial-manage-vm.md).
