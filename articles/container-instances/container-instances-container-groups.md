---
title: "aaaAzure tároló példányok tárolócsoportok"
description: "Tárolócsoportok működése az Azure-tároló példányok ismertetése"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: 2b0e784609979045c8f77d7b6d0987ec3fee714c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="container-groups-in-azure-container-instances"></a>Azure-tárolót példányát tárolócsoportok

legfelső szintű erőforrás hello Azure tároló példányát tároló olyan. Ez a cikk ismerteti a tárolócsoportok és forgatókönyvek milyen típusú engedélyezése.

## <a name="how-a-container-group-works"></a>Egy tároló csoport működése

Egy tároló csoport gyűjteménye, amelyek a hello ütemezhetők tároló azonos gép működtetéséhez, és egy életciklussal, helyi hálózati és tárolási köteteket. Hasonló toohello fogalma egy *pod* a [Kubernetes](https://kubernetes.io/docs/concepts/workloads/pods/pod/) és [DC/OS](https://dcos.io/docs/1.9/deploying-services/pods/).

hello alábbi ábrán egy példa látható egy tároló csoport, amely több tároló tartalmazza.

![Tároló csoportok – példa][container-groups-example]

Vegye figyelembe:

- hello csoport, egy önálló gazdagépen van ütemezve.
- hello csoport közzétesz egy egyetlen nyilvános IP-cím, egy kitett port.
- hello csoport két tárolók tevődik össze. Egy tároló figyeli 80,-as porton, miközben hello más porton figyel 5000.
- hello csoport tartalmazza a kötet csatlakoztatások, két Azure fájlmegosztásokat, és minden tárolót csatlakoztatja egy helyileg hello megosztások.

### <a name="networking"></a>Hálózat

Tárolócsoportok ossza meg IP-cím és port névtér az IP-címet. tooenable külső ügyfelek tooreach hello csoporton belüli tárolója, fel kell fednie a hello IP-címe és hello tárolóból hello port. Mert hello csoport tárolókra port névtér port hozzárendelése nem támogatott. Tárolók egy csoporton belüli képes elérni egymást keresztül hello a localhost portok, amely ki van téve, akkor is, ha azokat a portokat nem érhetők el külsőleg hello csoport IP-címe.

### <a name="storage"></a>Storage

Megadhatja, hogy külső kötetek toomount tároló csoporton belül. A megadott elérési utak belül hello egyes tárolók egy csoport a köteteket is leképezheti.

## <a name="common-scenarios"></a>Gyakori forgatókönyvek

Több tároló csoportok olyan hasznos olyan esetekben, ahol a tároló lemezképekre különböző csapatok által kézbesítése, és külön erőforrás követelményekkel rendelkezik néhány másolatot egyetlen működési feladat toodivide kívánt. Példa használati lehetnek:

- Egy alkalmazás-tárolót és a naplózási tároló. hello naplózási tároló hello fő alkalmazás hello naplók és a metrikák kimeneti gyűjt, és írja őket toolong távú tárolási.
- Az alkalmazások és a figyelési tároló. rendszeres időközönként figyelési tároló hello lehetővé teszi egy kérelem toohello alkalmazás tooensure, hogy fut, és helytelenül válaszol, és riasztást küld, ha nem.
- Egy tároló kiszolgáló webalkalmazás és húzza a legújabb tartalom hello verziókövetésből tárolója.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan túl[a több tároló csoport telepítése](container-instances-multi-container-group.md) az Azure Resource Manager sablonnal.

<!-- IMAGES -->

[container-groups-example]: ./media/container-instances-container-groups/container-groups-example.png