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
# <a name="how-toofind-linux-vm-images-in-hello-azure-marketplace-with-hello-azure-cli"></a><span data-ttu-id="b232e-103">Hogyan toofind Linux virtuális gép az Azure piactér hello hello Azure parancssori felület a lemezképek</span><span class="sxs-lookup"><span data-stu-id="b232e-103">How toofind Linux VM images in hello Azure Marketplace with hello Azure CLI</span></span>
<span data-ttu-id="b232e-104">Ez a témakör ismerteti, hogyan toouse hello Azure piactér hello Azure CLI 2.0 toofind VM-lemezképekkel.</span><span class="sxs-lookup"><span data-stu-id="b232e-104">This topic describes how toouse hello Azure CLI 2.0 toofind VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="b232e-105">Ezen információk toospecify Piactéri rendszerkép használata a Linux virtuális gép létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b232e-105">Use this information toospecify a Marketplace image when you create a Linux VM.</span></span>

<span data-ttu-id="b232e-106">Győződjön meg arról, hogy telepítette hello legújabb [Azure CLI 2.0](/cli/azure/install-az-cli2) és tooan Azure-fiókkal van bejelentkezve (`az login`).</span><span class="sxs-lookup"><span data-stu-id="b232e-106">Make sure that you installed hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and are logged in tooan Azure account (`az login`).</span></span>

## <a name="terminology"></a><span data-ttu-id="b232e-107">Terminológia</span><span class="sxs-lookup"><span data-stu-id="b232e-107">Terminology</span></span>

<span data-ttu-id="b232e-108">Piactéren elérhető rendszerkép hello CLI és más Azure-eszközök tooa hierarchia megfelelően azonosítja:</span><span class="sxs-lookup"><span data-stu-id="b232e-108">Marketplace images are identified in hello CLI and other Azure tools according tooa hierarchy:</span></span>

* <span data-ttu-id="b232e-109">**A Publisher** -hello hello lemezképét létrehozó szervezet.</span><span class="sxs-lookup"><span data-stu-id="b232e-109">**Publisher** - hello organization that created hello image.</span></span> <span data-ttu-id="b232e-110">Példa: kanonikus</span><span class="sxs-lookup"><span data-stu-id="b232e-110">Example: Canonical</span></span>
* <span data-ttu-id="b232e-111">**Ajánlat** -közzétevő által létrehozott kapcsolódó képek csoportja.</span><span class="sxs-lookup"><span data-stu-id="b232e-111">**Offer** - A group of related images created by a publisher.</span></span> <span data-ttu-id="b232e-112">Példa: Ubuntu Server</span><span class="sxs-lookup"><span data-stu-id="b232e-112">Example: Ubuntu Server</span></span>
* <span data-ttu-id="b232e-113">**SKU** - ajánlatot, például egy terjesztési jelentős kiadása példányát.</span><span class="sxs-lookup"><span data-stu-id="b232e-113">**SKU** - An instance of an offer, such as a major release of a distribution.</span></span> <span data-ttu-id="b232e-114">Példa: 16.04-es lts verzió</span><span class="sxs-lookup"><span data-stu-id="b232e-114">Example: 16.04-LTS</span></span>
* <span data-ttu-id="b232e-115">**Verzió** -hello SKU lemezkép verziószámát.</span><span class="sxs-lookup"><span data-stu-id="b232e-115">**Version** - hello version number of an image SKU.</span></span> <span data-ttu-id="b232e-116">Hello kép megadásakor lecserélheti hello verziószáma a "legújabb" hello hello terjesztési legújabb verzióját választja ki, amelyek.</span><span class="sxs-lookup"><span data-stu-id="b232e-116">When specifying hello image, you can replace hello version number with "latest", which selects hello latest version of hello distribution.</span></span>

<span data-ttu-id="b232e-117">a Piactéri lemezkép toospecify, általában hello lemezképet használ *URN*.</span><span class="sxs-lookup"><span data-stu-id="b232e-117">toospecify a Marketplace image, you typically use hello image *URN*.</span></span> <span data-ttu-id="b232e-118">hello URN összevonja ezeket az értékeket, hello kettőspont (:) elválasztva: *Publisher*:*kínálnak*:*Sku*:*verzió*.</span><span class="sxs-lookup"><span data-stu-id="b232e-118">hello URN combines these values, separated by hello colon (:) character: *Publisher*:*Offer*:*Sku*:*Version*.</span></span> 


## <a name="list-popular-images"></a><span data-ttu-id="b232e-119">Népszerű lemezképek felsorolása</span><span class="sxs-lookup"><span data-stu-id="b232e-119">List popular images</span></span>

<span data-ttu-id="b232e-120">Hello futtatása [az vm képlistában](/cli/azure/vm/image#list) nélkül hello parancs `--all` beállítás, toosee hello Azure piactér a lemezképek népszerű VM listáját.</span><span class="sxs-lookup"><span data-stu-id="b232e-120">Run hello [az vm image list](/cli/azure/vm/image#list) command, without hello `--all` option, toosee a list of popular VM images in hello Azure Marketplace.</span></span> <span data-ttu-id="b232e-121">Például futtassa a következő parancs toodisplay hello népszerű lemezképek gyorsítótárazott listáját táblázatos formátumú:</span><span class="sxs-lookup"><span data-stu-id="b232e-121">For example, run hello following command toodisplay a cached list of popular images in table format:</span></span>

```azurecli
az vm image list --output table
```

<span data-ttu-id="b232e-122">hello kimenete hello URN (hello értéket hello *Urn* oszlop), amely toospecify hello lemezképet használ.</span><span class="sxs-lookup"><span data-stu-id="b232e-122">hello output includes hello URN (hello value in hello *Urn* column), which you use toospecify hello image.</span></span> <span data-ttu-id="b232e-123">Virtuális gép valamelyik, a népszerű piactér lemezképek létrehozásakor megadhatja hello URN alias, például a *UbuntuLTS*.</span><span class="sxs-lookup"><span data-stu-id="b232e-123">When creating a VM with one of these popular Marketplace images, you can alternatively specify hello URN alias, such as *UbuntuLTS*.</span></span>

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

## <a name="find-specific-images"></a><span data-ttu-id="b232e-124">Adott rendszerképek keresése</span><span class="sxs-lookup"><span data-stu-id="b232e-124">Find specific images</span></span>

<span data-ttu-id="b232e-125">toofind hello piactér, a megadott Virtuálisgép-lemezkép használata hello `az vm image list` hello parancsot `--all` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="b232e-125">toofind a specific VM image in hello Marketplace, use hello `az vm image list` command with hello `--all` option.</span></span> <span data-ttu-id="b232e-126">Hello parancs ezen verziója bizonyos idő toocomplete vesz igénybe, és visszatérhet a hosszadalmas kimeneti, általában szűrheti a listát hello `--publisher` vagy egy másik paraméter.</span><span class="sxs-lookup"><span data-stu-id="b232e-126">This version of hello command takes some time toocomplete and can return lengthy output, so you usually filter hello list by `--publisher` or another parameter.</span></span> 

<span data-ttu-id="b232e-127">Például a következő parancs hello megjeleníti minden Debian ajánlatok (ne feledje, hogy nélkül hello `--all` váltáshoz csak keres a helyi gyorsítótár hello közös képek):</span><span class="sxs-lookup"><span data-stu-id="b232e-127">For example, hello following command displays all Debian offers (remember that without hello `--all` switch, it only searches hello local cache of common images):</span></span>

```azurecli
az vm image list --offer Debian --all --output table 

```

<span data-ttu-id="b232e-128">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="b232e-128">Partial output:</span></span> 
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

<span data-ttu-id="b232e-129">A hello hasonló szűrők `--location`, `--publisher`, és `--sku` beállítások.</span><span class="sxs-lookup"><span data-stu-id="b232e-129">Apply similar filters with hello `--location`, `--publisher`, and `--sku` options.</span></span> <span data-ttu-id="b232e-130">Részleges egyezések is végezhető egy szűrővel – például keresése `--offer Deb` toofind minden Debian lemezképbe.</span><span class="sxs-lookup"><span data-stu-id="b232e-130">You can even perform partial matches on a filter, such as searching for `--offer Deb` toofind all Debian images.</span></span>

<span data-ttu-id="b232e-131">Ha nem adja meg egy adott helyen hello `--location` beállítás, hello értékeinek `westus` alapértelmezett által visszaadott.</span><span class="sxs-lookup"><span data-stu-id="b232e-131">If you don't specify a particular location with hello `--location` option, hello values for `westus` are returned by default.</span></span> <span data-ttu-id="b232e-132">(Egy másik alapértelmezett hely beállítása futtatásával `az configure --defaults location=<location>`.)</span><span class="sxs-lookup"><span data-stu-id="b232e-132">(Set a different default location by running `az configure --defaults location=<location>`.)</span></span>

<span data-ttu-id="b232e-133">Például a következő parancs hello felsorolja a Debian 8 termékváltozatokat a `westeurope`:</span><span class="sxs-lookup"><span data-stu-id="b232e-133">For example, hello following command lists all Debian 8 SKUs in `westeurope`:</span></span>

```azurecli
az vm image list --location westeurope --offer Deb --publisher credativ --sku 8 --all --output table
```

<span data-ttu-id="b232e-134">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="b232e-134">Partial output:</span></span>

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

## <a name="navigate-hello-images"></a><span data-ttu-id="b232e-135">Keresse meg a hello lemezképek</span><span class="sxs-lookup"><span data-stu-id="b232e-135">Navigate hello images</span></span> 
<span data-ttu-id="b232e-136">Egy másik módja toofind helyen lemezkép toorun hello [az méretű kép lista-közzétevők](/cli/azure/vm/image#list-publishers), [az méretű kép lista-ajánlatok](/cli/azure/vm/image#list-offers), és [az méretű kép lista-SKU](/cli/azure/vm/image#list-skus) parancsok sorrendben.</span><span class="sxs-lookup"><span data-stu-id="b232e-136">Another way toofind an image in a location is toorun hello [az vm image list-publishers](/cli/azure/vm/image#list-publishers), [az vm image list-offers](/cli/azure/vm/image#list-offers), and [az vm image list-skus](/cli/azure/vm/image#list-skus) commands in sequence.</span></span> <span data-ttu-id="b232e-137">A következő parancsokkal ezek az értékek határozzák meg:</span><span class="sxs-lookup"><span data-stu-id="b232e-137">With these commands, you determine these values:</span></span>

1. <span data-ttu-id="b232e-138">Lista hello kép közzétevők.</span><span class="sxs-lookup"><span data-stu-id="b232e-138">List hello image publishers.</span></span>
2. <span data-ttu-id="b232e-139">Listázza egy adott közzétevő ajánlatait.</span><span class="sxs-lookup"><span data-stu-id="b232e-139">For a given publisher, list their offers.</span></span>
3. <span data-ttu-id="b232e-140">Listázza egy adott ajánlathoz tartozó termékváltozatokat.</span><span class="sxs-lookup"><span data-stu-id="b232e-140">For a given offer, list their SKUs.</span></span>


<span data-ttu-id="b232e-141">Például hello következő parancs megjeleníti az USA nyugati régiója hely hello hello kép közzétevők:</span><span class="sxs-lookup"><span data-stu-id="b232e-141">For example, hello following command lists hello image publishers in hello West US location:</span></span>

```azurecli
az vm image list-publishers --location westus --output table
```

<span data-ttu-id="b232e-142">Részleges kimenete:</span><span class="sxs-lookup"><span data-stu-id="b232e-142">Partial output:</span></span>

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
<span data-ttu-id="b232e-143">Ezen információk toofind biztosít egy adott közzétételi használja.</span><span class="sxs-lookup"><span data-stu-id="b232e-143">Use this information toofind offers from a specific publisher.</span></span> <span data-ttu-id="b232e-144">Például ha Canonical egy hello USA nyugati régiója helye a kép közzétevőjének, keressék a futtatásával `azure vm image list-offers`.</span><span class="sxs-lookup"><span data-stu-id="b232e-144">For example, if Canonical is an image publisher in hello West US location, find their offers by running `azure vm image list-offers`.</span></span> <span data-ttu-id="b232e-145">Hozzáférési hello hely és hello közzétevő, mint például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="b232e-145">Pass hello location and hello publisher as in hello following example:</span></span>

```azurecli
az vm image list-offers --location westus --publisher Canonical --output table
```

<span data-ttu-id="b232e-146">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b232e-146">Output:</span></span>

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
<span data-ttu-id="b232e-147">Láthatja, hogy hello USA nyugati régiója régióban Canonical közzéteszi a hello **UbuntuServer** kínálnak az Azure-on.</span><span class="sxs-lookup"><span data-stu-id="b232e-147">You see that in hello West US region, Canonical publishes hello **UbuntuServer** offer on Azure.</span></span> <span data-ttu-id="b232e-148">De milyen termékváltozatok? tooget ezeket az értékeket, futtassa `azure vm image list-skus` és hello helyét, kiadó és ajánlat, amely rendelkezik a felderített beállítása:</span><span class="sxs-lookup"><span data-stu-id="b232e-148">But what SKUs? tooget those values, run `azure vm image list-skus` and set hello location, publisher, and offer that you have discovered:</span></span>

```azurecli
az vm image list-skus --location westus --publisher Canonical --offer UbuntuServer --output table
```

<span data-ttu-id="b232e-149">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b232e-149">Output:</span></span>

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

<span data-ttu-id="b232e-150">Végül, használja a hello `az vm image list` parancs toofind hello SKU-t, például egy adott verziójához **16.04-es lts verzió**:</span><span class="sxs-lookup"><span data-stu-id="b232e-150">Finally, use hello `az vm image list` command toofind a specific version of hello SKU you want, for example, **16.04-LTS**:</span></span>

```azurecli
az vm image list --location westus --publisher Canonical --offer UbuntuServer --sku 16.04-LTS --all --output table
```

<span data-ttu-id="b232e-151">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="b232e-151">Output:</span></span>

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
## <a name="next-steps"></a><span data-ttu-id="b232e-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b232e-152">Next steps</span></span>
<span data-ttu-id="b232e-153">Most kiválaszthatja, hogy pontosan hello kép kívánt toouse hello URN érték tudomásul véve.</span><span class="sxs-lookup"><span data-stu-id="b232e-153">Now you can choose precisely hello image you want toouse by taking note of hello URN value.</span></span> <span data-ttu-id="b232e-154">Ezt az értéket hello átadni `--image` paramétert, ha egy virtuális gép létrehozásához hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="b232e-154">Pass this value with hello `--image` parameter when you create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> <span data-ttu-id="b232e-155">Ne feledje, hogy opcionálisan lecserélheti hello verziószám hello URN a "legutóbbi".</span><span class="sxs-lookup"><span data-stu-id="b232e-155">Remember that you can optionally replace hello version number in hello URN with "latest".</span></span> <span data-ttu-id="b232e-156">Ebben a verzióban mindig hello hello terjesztési legújabb verzióját.</span><span class="sxs-lookup"><span data-stu-id="b232e-156">This version is always hello latest version of hello distribution.</span></span> <span data-ttu-id="b232e-157">a virtuális gép hello URN információ segítségével gyorsan toocreate lásd: [létrehozása és kezelése a Linux virtuális gépet az Azure CLI hello](tutorial-manage-vm.md).</span><span class="sxs-lookup"><span data-stu-id="b232e-157">toocreate a virtual machine quickly by using hello URN information, see [Create and Manage Linux VMs with hello Azure CLI](tutorial-manage-vm.md).</span></span>
