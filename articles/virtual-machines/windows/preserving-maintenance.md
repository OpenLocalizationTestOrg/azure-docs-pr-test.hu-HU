---
title: "Virtuális gép megőrzi az karbantartási Windows virtuális gépek Azure-ban |} Microsoft Docs"
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
ms.openlocfilehash: 8888bafbc3aba24168312b611a9b4fbde25f376d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="vm-preserving-maintenance-with-in-place-vm-migration"></a>Virtuális gép megőrzi az karbantartási a helyszíni virtuális gép áttelepítése

Amíg frissítések többségét üzemeltetett virtuális gépek nincs hatása, vannak esetek, ahol az összetevők vagy a szolgáltatások frissítéseket futó virtuális gépek (nélkül a virtuális gép teljes újraindítás) minimális zavaró tényező eredményez.

Ezeket a frissítéseket, amely lehetővé teszi a helyszíni élő áttelepítés, más néven "memória-megőrzi az update" technológiával történik. Amikor frissíti a gazdagép, a virtuális gép el van helyezve egy "felfüggesztett" állapotba kerül, RAM, a memória megőrzi, amíg az üzemeltetési környezet (pl. az alapul szolgáló operációs rendszert) alkalmazza a szükséges frissítések és javítások.
A virtuális gép majd folytatódik a felfüggesztés 30 másodpercen belül.
A virtuális gép órája a folytatás után automatikusan szinkronizálódik.

Nem minden frissítés helyezhető üzembe ezzel a mechanizmussal, de a rövid felfüggesztési időszak miatt a frissítések ilyen telepítése nagymértékben csökkenti a virtuális gépekre gyakorolt hatást.

Többpéldányos (virtuális gépek rendelkezésre állási csoportba) frissítései alkalmazott frissítési tartományok egyszerre.

Egyes alkalmazások negatív hatással lehet több, mint a többire ezeket a frissítéseket. Valós idejű esemény feldolgozása, médiaadatfolyam vagy átkódolás vagy hálózati forgatókönyvek, magas teljesítmény végző alkalmazások például előfordulhat, hogy nem kell megtervezni, hogy egy 30 másodperces szünet működését. A virtuális gépen futó alkalmazások jövőbeli frissítések többet is megtudhat meghívásával a [ütemezett események](../virtual-machines-scheduled-events.md) API-ja a [Azure metaadat-szolgáltatás](../virtual-machines-instancemetadataservice-overview.md).
