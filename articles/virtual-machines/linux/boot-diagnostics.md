---
title: "a Linux virtuális gépek Azure-ban aaaBoot diagnosztika |} Microsoft Doc"
description: "Hello két hibakeresési funkcióinak a Linux virtuális gépek Azure-ban áttekintése"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: Deland-Han
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: delhan
ms.openlocfilehash: d355d512de09d2f1d7a2718e3db3fb99c9dd9e24
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-boot-diagnostics-tootroubleshoot-linux-virtual-machines-in-azure"></a>Hogyan toouse rendszerindítási diagnosztika tootroubleshoot Linux virtuális gépek Azure

Mostantól két hibakereső szolgáltatás is elérhető az Azure-ban: konzolkimenet és képernyőkép is támogatott az Azure virtuális gépeken a Resource Manager-alapú üzemi modellben. 

Amikor a saját kép tooAzure vagy hello platform képek is rendszerindítás egyike, miért egy virtuális gép nem indítható állapotba lekérdezi számos oka lehet. E szolgáltatások engedélyezése meg tooeasily diagnosztizálhatja és a virtuális gépek helyreállítás rendszerindítási hibák esetén.

A Linux virtuális gépek megtekintéséhez a konzol napló, a portál hello hello kimeneti:

![Azure Portal](./media/boot-diagnostics/screenshot1.png)
 
Azonban a Windows és Linux rendszerű virtuális gépek Azure is lehetővé teszi, hogy egy képernyőfelvétel a hello VM hello hipervizorról toosee:

![Hiba](./media/boot-diagnostics/screenshot2.png)

Mindkét szolgáltatás támogatott az Azure virtuális gépeken minden régióban. Vegye figyelembe, a pillanatképek és a kimeneti too10 perc tooappear a tárfiókban lévő is eltarthat.

## <a name="common-boot-errors"></a>Gyakori rendszerindítási hibák

- [Kapcsolatos problémák](https://blogs.msdn.microsoft.com/linuxonazure/2016/09/13/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck-inodes/)
- [Kernel-problémák](https://blogs.msdn.microsoft.com/linuxonazure/2016/10/09/linux-recovery-manually-fixing-non-boot-issues-related-to-kernel-problems/)
- [FSTAB hibák](https://blogs.msdn.microsoft.com/linuxonazure/2016/07/21/cannot-ssh-to-linux-vm-after-adding-data-disk-to-etcfstab-and-rebooting/ )

## <a name="enable-diagnostics-on-a-new-virtual-machine"></a>Diagnosztika engedélyezése egy új virtuális gépen
1. Ha egy új virtuális gép létrehozása a betekintő portálon hello, válassza ki a hello **Azure Resource Manager** hello telepítési modell legördülő listából:
 
    ![Resource Manager](./media/boot-diagnostics/screenshot3.jpg)

2. A diagnosztikai fájlok hello figyelés beállítás tooselect hello tárfiók tooplace helyének megadása
 
    ![Virtuális gép létrehozása](./media/boot-diagnostics/screenshot4.jpg)

3. Ha telepíti az Azure Resource Manager sablon alapján, keresse meg a virtuálisgép-erőforrás tooyour, és a hozzáfűző hello diagnosztikai profil szakasz. Ne felejtse el toouse hello "2015-06-15" API version fejlécnek.

    ```json
    {
          "apiVersion": "2015-06-15",
          "type": "Microsoft.Compute/virtualMachines",
          … 
    ```

4. hello diagnosztikai profiljának lehetővé teszi tooselect hello tárfiók, amelybe tooput ezek a naplók.

    ```json
            "diagnosticsProfile": {
                "bootDiagnostics": {
                "enabled": true,
                "storageUri": "[concat('http://', parameters('newStorageAccountName'), '.blob.core.windows.net')]"
                }
            }
            }
        }
    ```

## <a name="update-an-existing-virtual-machine"></a>Meglévő virtuális gép frissítése

hello portálon keresztül tooenable a rendszerindítási diagnosztika, is frissítheti egy meglévő virtuális gép hello portálon keresztül. Válassza ki a rendszerindítási diagnosztika lehetőséget, majd mentse hello. Hello VM tootake léptetéséhez indítsa újra.

![Létező virtuális gép frissítése](./media/boot-diagnostics/screenshot5.png)