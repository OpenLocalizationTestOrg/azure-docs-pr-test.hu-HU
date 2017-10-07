---
title: "aaaAzure tároló példányok áttekintése |} Az Azure Docs"
description: "Az Azure Container Instances ismertetése"
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
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: c0662ede1260b15d9841bfc2c3c4cec4c30338d6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-container-instances"></a>Azure Container Instances

Tárolók gyorsan egyre hello előnyben részesített módja toopackage, telepítéséhez és felügyeletéhez a felhőalapú alkalmazásokhoz. Azure tároló példányok hello leggyorsabb és legegyszerűbb módja toorun Azure, a tároló, anélkül, hogy tooprovision bármely virtuális gépek és anélkül, hogy tooadopt egy magasabb szintű szolgáltatást kínál. 

Az Azure Container Instances ideális megoldás minden olyan forgatókönyv esetében, amely elkülönített tárolókban valósulhat meg, beleértve az egyszerű alkalmazásokat, a tevékenységek automatizálását és a buildfeladatokat. Forgatókönyvek, ahol teljes van szükség a tároló vezénylési, beleértve a szolgáltatásészlelés több tárolók, automatikus skálázás és koordinált alkalmazásfrissítések, azt javasoljuk, hello [Azure Tárolószolgáltatás](https://docs.microsoft.com/azure/container-service/).

## <a name="fast-startup-times"></a>Rövid indítási idők

A tárolók jelentős előnyöket nyújtanak a virtuális gépekkel szemben az indítás terén. Azure-tároló osztályt indítsa el a tároló nélkül hello kell tooprovision másodpercben az Azure-ban, és a virtuális gépek kezeléséhez.

## <a name="hypervisor-level-security"></a>Hipervizorszintű biztonság

Korábban a tárolók biztosítottak ugyan alkalmazásfüggőség-elkülönítési és erőforrás-szabályozási funkciót, azonban nem voltak eléggé megerősítve a rosszindulatú több-bérlős alkalmazásokhoz. Az Azure Container Instances használatakor az alkalmazások ugyanannyira el vannak különítve a tárolóban, mintha egy virtuális gépen futnának.

## <a name="custom-sizes"></a>Egyéni méretek

Tárolók csak egyetlen alkalmazás, de ezeket az alkalmazásokat hello pontos igényeinek nagy mértékben eltérhetnek általában optimalizált toorun. Az Azure Container Instanceszel pontosan annyi processzormagot és memóriát igényelhet, amennyire szüksége van. Után kell fizetnie függően mi kér, hello által kiszámlázott második, így finom optimalizálhatja kiadásait igényei szerint.

## <a name="public-ip-connectivity"></a>Nyilvános IP-kapcsolat

Azure tároló példányaival is elérhetővé teheti a tárolók közvetlenül toohello internet nyilvános IP-címmel. Jövőbeli hello azt fogja bontsa ki a hálózati képességekkel tooinclude integráció a virtuális hálózatok, terheléselosztók, és más alapvető hello Azure hálózati infrastruktúra betölteni.

## <a name="persistent-storage"></a>Állandó tárolók

tooretrieve és Azure tároló osztályt állapotban maradnak, kínálunk közvetlen csatlakoztatása az Azure-megosztások fájlokat.

## <a name="linux-and-windows-containers"></a>Linux- és Windows-tárolók

Azure tároló példányaival ütemezéssel megadhatja, hogy mindkét Windows és Linux tárolók hello azonos API. Egyszerűen jelezheti hello alap operációs rendszer típusa, és minden más azonos.

## <a name="co-scheduled-groups"></a>Együttesen ütemezett csoportok

Az Azure Container Instances támogatja az olyan több tárolóból álló csoportok ütemezését, amelyek osztoznak a gazdagépen, a helyi hálózaton és a tárolón, valamint azonos az életciklusuk. Ez lehetővé teszi, hogy Ön toocombine másokkal a fő alkalmazás támogató szerepkörben, például naplózásra működött.

## <a name="next-steps"></a>Következő lépések

Próbálja üzembe helyezni a tároló tooAzure egy egyetlen parancs használatával a [gyors üzembe helyezési útmutató](container-instances-quickstart.md).
