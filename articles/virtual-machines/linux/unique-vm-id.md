---
title: "egy Azure Linux virtuális gép azonosítója aaaGet |} Microsoft Docs"
description: "Ismerteti, hogyan tooget és használja az Azure Linux virtuális gép egyedi azonosítóját."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: kmouss
manager: timlt
editor: 
ms.assetid: 136c5d28-ff6b-4466-b27f-7a29785b5d27
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 01/23/2017
ms.author: kmouss
ms.openlocfilehash: 4c8ddfc2e892824581e77649285ee8adbccd5def
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="accessing-and-using-azure-vm-unique-id"></a>Elérésére és használatára az Azure virtuális gép egyedi azonosítója
Az Azure virtuális gép egyedi azonosítója egy 128 azonosító kódolású, minden Azure IaaS virtuális gép SMBIOS tárolja, és jelenleg olvasható platform BIOS-parancsok használatával.

Az Azure virtuális gép egyedi azonosítója egy csak olvasható tulajdonság értéke. Virtuális gép Azure egyedi azonosítója nem változik, újraindítás leállásakor (tervezett vagy nem tervezett), indítása/leállítása deszerializálni lefoglalni, szolgáltatás a javítás vagy a hely visszaállítása. Azonban ha hello VM pillanatfelvételt és másolt toocreate egy új példányát, új Azure virtuális gép azonosítója van konfigurálva.

> [!NOTE]
> Ha a régebbi virtuális gépek létrehozása és mivel ezen új szolgáltatás lett megkezdődött (2014. szeptember 18) fut, indítsa újra a virtuális gép tooautomatically első Azure egyedi azonosítója.
> 
> 

tooaccess Azure egyedi virtuális gép azonosítója a virtuális gép hello belül:

## <a name="create-a-vm"></a>Virtuális gép létrehozása
További információkért lásd: [virtuális gép létrehozása](../windows/creation-choices.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="connect-toohello-vm"></a>Csatlakozás a virtuális gép toohello
További információkért lásd: [SSH a Linux](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="query-vm-unique-id"></a>Lekérdezés virtuális gép egyedi azonosítója
A parancs (a példa **Ubuntu**):

```bash
sudo dmidecode | grep UUID
```

Példa várt eredmény:

```bash
UUID: 090556DA-D4FA-764F-A9F1-63614EDA019A
```

Lejáró tooBig Endian bit rendezés hello tényleges egyedi virtuális gép azonosítója ebben az esetben lesz:

```bash
DA 56 05 09 – FA D4 – 4f 76 - A9F1-63614EDA019A
```

Azure virtuális gép egyedi azonosítója használható különböző helyzetekben, hogy hello VM Azure-on futó vagy a helyszíni és segít az engedélyezési, jelentés vagy általános követési követelményeit, előfordulhat, hogy az Azure infrastruktúra-szolgáltatási üzemelő példányok esetében. Alkalmazások létrehozásához, és igazoló őket az Azure számos független szoftvergyártók tooidentify egy Azure virtuális Gépen egész annak életciklusa és tootell lehet szükség, ha hello virtuális gép fut az Azure, a helyszíni vagy más szolgáltatók. A platform-azonosító például ha hello megfelelően szoftvert vagy súgó toocorrelate bármely virtuális gép tooits adatforrás például tooassist hello jobb platform és tootrack jobb metrikáját hello beállításával, és segítséget összefüggéseket többek között a metrikák egyéb felhasználásra.

