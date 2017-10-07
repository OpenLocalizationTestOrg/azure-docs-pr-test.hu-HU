---
title: "annak a közös automatikus skálázás aaaOverview |} Microsoft Docs"
description: "Ismerje meg, hogy az erőforrás néhány hello közös minták tooauto skálázása az Azure-ban."
author: anirudhcavale
manager: orenr
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: ancav
ms.openlocfilehash: fc5bd97852e0af01aa32940c99721ab8e21033ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-common-autoscale-patterns"></a>Közös automatikus skálázás minták áttekintése
Ez a cikk ismerteti, néhány hello közös minták tooscale az erőforrás az Azure-ban.

Az Azure a figyelő automatikus méretezési csak tooVirtual gép méretezési készletek (VMSS), felhőszolgáltatások, app service-csomagokról és app service Environment-környezetek vonatkozik. 

# <a name="lets-get-started"></a>Lehetővé teszi, hogy első lépései

Ez a cikk feltételezi, hogy Ön ismeri a automatikus méretezési. Is [elindított ide tooscale lekérni az erőforrás][1]. hello közé tartoznak a következők hello közös méretezési minták.

## <a name="scale-based-on-cpu"></a>A skála CPU alapján

A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és 

- Azt szeretné, hogy tooscale alapján CPU out/skála.
- Továbbá azt szeretné, hogy tooensure példányok minimális száma. 
- Emellett kívánt méretezheti a példányok maximális száma toohello számos beállított tooensure.

![A skála CPU alapján][2]

## <a name="scale-differently-on-weekdays-vs-weekends"></a>Hétköznapokon vs hétvégén másképp méretezése

A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és

- (A hétköznapokon) alapértelmezés szerint a 3 példányokat szeretne.
- Nem várt forgalom hétvégén, és ezért szeretne tooscale too1 példány le hétvégén.

![Hétköznapokon vs hétvégén másképp méretezése][3]

## <a name="scale-differently-during-holidays"></a>Méretezhető másképp ünnepek során

A webes alkalmazás (/ VMSS/felhő szerepkör-szolgáltatás) és 

- Azt szeretné, hogy tooscale fel/le alapértelmezés szerint a CPU-használat alapján.
- Azonban ünnepi (vagy adott nap fontos a vállalati) során szeretné, hogy toooverride hello alapértelmezett beállításokat, és további kapacitással rendelkeznek a rendelkezésére.

![Skála eltérően a munkaszüneti napokat is][4]

## <a name="scale-based-on-custom-metric"></a>Egyéni metrika alapuló méretezési

Rendelkezik egy előtér-webkiszolgáló és egy API-réteghez, amelynek hello háttérrendszerrel kommunikál. 

- Azt szeretné, hogy a hello előtér egyéni események alapján tooscale hello API réteg (Példa: azt szeretné, hogy a kivételt folyamat alapján hello elemszáma bevásárlókocsiból vásárlás hello tooscale)

![Egyéni metrika alapuló méretezési][5]

<!--Reference-->
[1]: ./monitoring-autoscale-get-started.md
[2]: ./media/monitoring-autoscale-common-scale-patterns/scale-based-on-cpu.png
[3]: ./media/monitoring-autoscale-common-scale-patterns/weekday-weekend-scale.png
[4]: ./media/monitoring-autoscale-common-scale-patterns/holidays-scale.png
[5]: ./media/monitoring-autoscale-common-scale-patterns/custom-metric-scale.png