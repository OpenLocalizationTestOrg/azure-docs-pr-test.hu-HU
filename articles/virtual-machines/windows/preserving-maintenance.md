---
title: "aaa VM megóvása karbantartása a Windows-alapú virtuális gépek Azure-ban |} Microsoft Docs"
description: "A helyszíni virtuális gép áttelepítés megőrzi az frissítések memória."
services: virtual-machines-windows
documentationcenter: 
author: zivr
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: zivr
ms.openlocfilehash: b798f0afd9d8dc60ca8a78f7cc77435a0ddc76fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>Virtuális gép megőrzi az karbantartási a helyszíni virtuális gép áttelepítése

Míg a frissítések többségét hello nincs hatása toohosted virtuális gépeket, vannak esetek, ahol frissítések toocomponents vagy szolgáltatások eredményez-e a minimális zavaró tényező toorunning virtuális gépek (nélkül hello virtuális gép teljes újraindítás).

Ezeket a frissítéseket, amely lehetővé teszi a helyszíni élő áttelepítés, más néven "memória-megőrzi az update" technológiával történik. Hello állomás frissítésekor hello virtuális gép el van helyezve egy "felfüggesztett" állapotba kerül, RAM, hello memóriája megőrzi az üzemeltetési környezet (pl. az alapul szolgáló operációs rendszert) hello hello szükséges frissítések és javítások alkalmazása közben.
hello virtuális gép majd folytatódik a felfüggesztés 30 másodpercen belül.
Folytatja a futtatását, miután a hello óra hello virtuális gép automatikus szinkronizálásának engedélyezése.

Nem minden frissítés is telepíthető a mechanizmussal, de a rövid késleltetés időszak alatt a megadott ezen frissítéseinek telepítéséhez módon jelentősen csökkenti a hatás toovirtual gépek.

Többpéldányos (virtuális gépek rendelkezésre állási csoportba) frissítései alkalmazott frissítési tartományok egyszerre.

Egyes alkalmazások negatív hatással lehet több, mint a többire ezeket a frissítéseket. Valós idejű esemény feldolgozása, médiaadatfolyam vagy átkódolás vagy hálózati forgatókönyvek, magas teljesítmény végző alkalmazások például nem lehet tervezett tootolerate egy 30 másodperces szünet. A virtuális gépen futó alkalmazások is információ a jövőbeli frissítések hívó hello [ütemezett események](../virtual-machines-scheduled-events.md) hello API-ja [Azure metaadat-szolgáltatás](../virtual-machines-instancemetadataservice-overview.md).
