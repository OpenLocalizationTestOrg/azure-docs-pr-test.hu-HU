---
title: "aaaMove fájlok, a szolgáltatáskapcsolódási pont Azure Linux virtuális gépekről érkező tooand |} Microsoft Docs"
description: "Fájlok tooand biztonságosan áthelyezése az Azure-ban a szolgáltatáskapcsolódási pont és egy SSH-kulcspárral Linux virtuális gép."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: dlepow
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: danlep
ms.openlocfilehash: 9e4dce667aa581df74aec3d45a6ba5e52f777d1c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="move-files-tooand-from-a-linux-vm-using-scp"></a>Linux virtuális gépet a szolgáltatáskapcsolódási pont fájlok tooand áthelyezése

Ez a cikk bemutatja, hogyan toomove fájlok a munkaállomáson tooan Azure Linux virtuális gép mentése, vagy egy Azure Linux virtuális gép tooyour munkaállomásra, leállt biztonságos másolása (SCP). Fájlok a munkaállomáson és a Linux virtuális gép közötti áthelyezése, gyorsan és biztonságosan, fontos az Azure-infrastruktúra felügyeletéhez. 

Ez a cikk a Linux virtuális gép üzembe helyezett Azure használatával kell [SSH nyilvános és titkos kulcs fájlok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Szükség egy SCP-ügyfél a helyi számítógépen. SSH épül, és alapértelmezett rendszerhéjakba hello legtöbb Linux és Mac számítógépek és az egyes Windows ismertetése szerepel.

## <a name="quick-commands"></a>Gyors parancsok

Másolja át a fájlt toohello Linux virtuális gép mentése

```bash
scp file azureuser@azurehost:directory/targetfile
```

Másolja át a fájlt a Linux virtuális gép hello le

```bash
scp azureuser@azurehost:directory/file targetfile
```

## <a name="detailed-walkthrough"></a>Részletes bemutató

Példaként, azt egy Azure-alapú konfigurációs fájl mentése tooa Linux virtuális gép áthelyezése, és húzza a naplófájl könyvtárának, le is Szolgáltatáskapcsolódási pontjait és SSH kulcs használatával.   

## <a name="ssh-key-pair-authentication"></a>SSH-kulcspár hitelesítés

Szolgáltatáskapcsolódási pont hello átviteli réteg SSH használ. SSH leírók hello hitelesítési hello cél gazdagépen, és az SSH alapértelmezés szerint egy titkosított csatornán hello fájl áthelyezi azt. SSH hitelesítés a felhasználónevek és jelszavak használható. Ajánlott biztonsági eljárásként azonban SSH nyilvános és titkos kulcsos hitelesítés használata ajánlott. SSH hitelesítette hello kapcsolat, ha a szolgáltatáskapcsolódási pont majd megkezdi hello fájl másolása. Használja a megfelelően konfigurált `~/.ssh/config` és az SSH nyilvános és titkos kulcsokat, hello SCP-kapcsolat csak a kiszolgáló nevét (vagy IP-cím) segítségével hozhatók létre. Ha csak egy SSH-kulcsot, SCP megkeresi azt hello `~/.ssh/` könyvtár, és használja ezt a szolgáltatást a virtuális gép toohello alapértelmezett toolog által.

Konfigurálásáról további információt a `~/.ssh/config` és az SSH nyilvános és titkos kulcsok, lásd: [hozzon létre SSH-kulcsok](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="scp-a-file-tooa-linux-vm"></a>Szolgáltatáskapcsolódási pont egy fájl tooa Linux virtuális gép

Például hello első azt tooa Linux virtuális gép által használt toodeploy automation be Azure-alapú konfigurációs fájl másolása. Ez a fájl tartalmazza az Azure API hitelesítő adatait, amely tartalmazza a titkos kulcsok, mert a biztonsági fontos. hello titkosított alagút SSH által biztosított védelmet nyújt a hello hello fájl tartalmát.

parancs másolatok hello helyi következő hello *.azure/config* tooan Azure virtuális gép teljes tartománynévvel fájl *myserver.eastus.cloudapp.azure.com*. hello Azure virtuális gép hello rendszergazdai felhasználónév *azureuser* . hello fájl megcélzott toohello */home/azureuser/* könyvtár. A parancs a saját értékeit helyettesítse.

```bash
scp ~/.azure/config azureuser@myserver.eastus.cloudapp.com:/home/azureuser/config
```

## <a name="scp-a-directory-from-a-linux-vm"></a>Szolgáltatáskapcsolódási pont a könyvtárat, a Linux virtuális gépek

Ebben a példában a Linux virtuális gép hello tooyour munkaállomás le egy könyvtárat a naplófájlok másolja azt. Egy naplófájlt is, vagy nem tartalmazhatnak bizalmas vagy titkos adatok. Azonban a szolgáltatáskapcsolódási pont biztosítja a Titkosított hello naplófájlok hello tartalmát. Szolgáltatáskapcsolódási pont tootransfer hello fájlok használata hello legegyszerűbb módja tooget naplókönyvtár és tooyour munkaállomás le fájlok hello miközben lehetőség van a biztonságos.

hello alábbi a parancs átmásolja a terjesztendő fájlok hello */otthoni/azureuser/logs/* könyvtárába hello Azure virtuális gép toohello helyi könyvtárban:

```bash
scp -r azureuser@myserver.eastus.cloudapp.com:/home/azureuser/logs/. /tmp/
```

Hello `-r` cli jelző arra utasítja a szolgáltatáskapcsolódási pont toorecursively másolási hello fájlok és könyvtárak hello pontról hello könyvtár hello parancs szerepel.  Figyelje meg, hogy a parancssori szintaxist hello a rendszer hasonló tooa `cp` -parancs másolásával.

## <a name="next-steps"></a>Következő lépések

* [Kezelheti a felhasználókat, az SSH és az ellenőrzése vagy javítása lemezek Azure Linux virtuális gépek használatával hello VMAccess bővítmény](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
