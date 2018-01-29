---
title: "Cserélje le a fizikai lemez Azure verem |} Microsoft Docs"
description: "Azure-készletben a fizikai lemez cseréje folyamatának ismertetése."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 449ae53e-b951-401a-b2c9-17fee2f491f1
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: a95617a8dd2a8f296164c672e2b4b2628574ce5a
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/11/2017
---
# <a name="replace-a-physical-disk-in-azure-stack"></a>Egy Azure verem fizikai lemezének cseréje

*A következőkre vonatkozik: Azure verem integrált rendszerek és az Azure verem szoftverfejlesztői készlet*

Ez a cikk ismerteti az általános folyamat egy Azure verem fizikai lemezének cseréje. Ha egy fizikai lemez meghibásodik, akkor cserélje le a lehető leghamarabb.

Ez az eljárás használható integrált rendszerek, valamint a közbeni-cserélhető lemezeken rendelkező development kit központi telepítések.

Tényleges lemezcsere lépései eltérőek lesznek a számítógépgyártó (OEM) hardvergyártójához alapján. A rendszer vonatkozó részletes lépéseket a gyártója által biztosított mező cserélhető Cisco egységet (FRU) dokumentációjában talál. 

## <a name="review-disk-alert-information"></a>Lemez riasztási információk áttekintése
Ha egy lemez meghibásodik, akkor figyelmeztetést küld, amely közli, hogy kapcsolat megszakadt a fizikai lemezen. 

 ![Riasztás megjelenítése kapcsolat megszakadt a fizikai lemez](media/azure-stack-replace-disk/DiskAlert.png)

Megnyitja a riasztás, ha a riasztás leírásában a skálázási egység csomópont, és le kell cserélni a lemez fizikai tárolóhely pontos helyét. További Azure verem segítségével azonosíthatja a hibás lemezen LED kijelző képességek segítségével.

 ## <a name="replace-the-disk"></a>A rendszerlemez cseréje

Kövesse a OEM hardver gyártója által biztosított FRU tényleges lemezcsere utasításokat.

Letilthatja az egy nem támogatott lemezt egy integrált rendszerben, a rendszer blokkolja a lemezek, a szállító által nem támogatott. Nem támogatott lemez használata kísérli meg, ha új riasztás tájékoztat, hogy az egy lemezt egy nem támogatott modell vagy a belső vezérlőprogram miatt karanténba-e.

A lemezt cserél ki, Azure verem automatikusan felderíti az új lemezt, és a virtuális lemezek javítási folyamat elindul.  
 
 ## <a name="check-the-status-of-virtual-disk-repair"></a>Tekintse meg a virtuális lemezek javítása
 
 Miután a lemezt cserél ki, a virtuális lemez állapot figyelése, és javítsa a feladat előrehaladását a kiemelt végpont segítségével. Bármely olyan számítógépről, amely rendelkezik hálózati kapcsolattal a kiemelt végpont kövesse az alábbi lépéseket.

1. Nyisson meg egy Windows PowerShell-munkamenetet, és a kiemelt végponthoz kapcsolódni.
    ````PowerShell
        $cred = Get-Credential
        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
    ```` 
  
2. Futtassa a virtuális lemez állapotának megtekintéséhez a következő parancsot:
    ````PowerShell
        Get-VirtualDisk -CimSession s-cluster
    ````
   ![PowerShell Get-VirtualDisk parancs kimenete](media/azure-stack-replace-disk/GetVirtualDiskOutput.png)

3. Futtassa a jelenlegi tárolási feladat állapotának megtekintéséhez a következő parancsot:
    ```PowerShell
        Get-VirtualDisk -CimSession s-cluster | Get-StorageJob
    ````
      ![PowerShell Get-StorageJob parancs kimenete](media/azure-stack-replace-disk/GetStorageJobOutput.png)

## <a name="troubleshoot-virtual-disk-repair"></a>Virtuális lemezek javítási hibaelhárítása

Ha a virtuális lemezek javítása feladat jelenik meg lefagyott, a következő parancsot a feladat újraindításához:
  ````PowerShell
        Get-VirtualDisk -CimSession s-cluster | Repair-VirtualDisk
  ```` 
