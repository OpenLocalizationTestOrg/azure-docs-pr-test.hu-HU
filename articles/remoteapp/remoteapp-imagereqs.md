---
title: "aaaAzure RemoteApp kép követelményei |} Microsoft Docs"
description: "További tudnivalók létrehozása az Azure RemoteApp használt képek toobe hello követelményei"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 7cbb90f4-6dc9-462c-a429-088cdb57414e
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 4e35203eb93a866d4e0bd591d42b34746c7ffa4d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="requirements-for-azure-remoteapp-images"></a>Azure RemoteApp-lemezképek követelményei
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Az Azure RemoteApp használja a Windows Server 2012 R2 kép toohost, amelyet meg szeretne tooshare a felhasználók minden hello program. egyéni lemezkép toocreate, megkezdheti a meglévő lemezkép vagy [hozzon létre egy újat](remoteapp-create-custom-image.md).

> [!TIP]
> Tudta, hogy az Azure RemoteApp előfizetés hozzáférést tud biztosítani, hogy tooa Windows Server 2012 R2 rendszerképnél a hello Azure virtuális gép katalógusában, hogy toocreate saját sablon rendszerképet? [Kivétel](remoteapp-image-on-azurevm.md).  
> 
> 

az Azure RemoteApp használata esetén feltölthető hello kép hello követelményei a következők:

* Egyéni alkalmazások helyileg hello rendszerképre ne tároljon adatokat. Ezeket a lemezképeket állapot nélküli és alkalmazások csak tartalmaznia kell.
* hello kép nem tartalmaz adatokat, akkor szakadhat meg.
* hello képméret MB többszörösének kell lennie. Ha egy olyanra, amely nincs egy pontos több tooupload, hello feltöltése sikertelen lesz.
* hello a kép mérete 127 GB méretűnek kell lennie, vagy kisebb.
* A VHD-fájlt kell lennie (VHDX-fájlok jelenleg nem támogatottak).
* hello virtuális merevlemez nem lehet a 2. generációs virtuális gépek.
* rögzített méretű vagy dinamikusan bővülő hello VHD lehet. Dinamikusan bővülő virtuális merevlemez használata ajánlott, mivel kevesebb idő tooupload tooAzure, mint a rögzített méretű VHD-fájl szükséges.
* hello lemez inicializálni kell hello fő rendszertöltő rekord (MBR) particionálási stílus használatával. GUID partíciós tábla (GPT) partícióval hello nem támogatott.
* hello VHD-t tartalmaznia kell a Windows Server 2012 R2 egy telepítés. Több kötet, de csak egy Windows példányának tartalmazó tartalmazhat.
* hello távoli asztali munkamenetgazda (RDSH) szerepkör és hello asztali élmény szolgáltatást telepíteni kell.
* Távoli asztali Kapcsolatszervező szerepkör hello kell *nem* telepíthető.
* hello titkosított fájlrendszer (EFS) le kell tiltani.
* hello képnek kell lennie a Sysprep segédprogrammal előkészített hello paraméterek használatával **/oobe / generalize shutdown** (ne használja az hello **/mode:vm** paraméter).
* A pillanatkép-lánc virtuális merevlemez feltöltése nem támogatott.

Lásd: [hozzon létre egy Azure RemoteApp-rendszerkép](remoteapp-imageoptions.md) olvashat az Azure RemoteApp-lemezképek létrehozása.

