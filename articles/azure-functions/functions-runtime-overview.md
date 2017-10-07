---
title: "aaaAzure funkciók futásidejű áttekintése |} Microsoft Docs"
description: "Hello Azure Functions futásidejű Preview áttekintése"
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.openlocfilehash: 8ce3e5037731d499c330b395c89c90109d18d65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-runtime-overview"></a>Az Azure Functions futásidejű áttekintése

hello Azure Functions Futtatókörnyezettel lehetőséget biztosít az új, tootake kihasználják az hello egyszerűség és rugalmasság hello az Azure Functions programozási modell a helyszínen. Hello épülő azonos nyissa meg a forrás gyökerek, az Azure Functions, az Azure Functions Futtatókörnyezettel majdnem azonos fejlesztői élmény hello felhőalapú szolgáltatásként telepített helyszíni tooprovide.

![Az Azure Functions futásidejű a betekintő portálon][1]

hello Azure Functions Futtatókörnyezettel lehetőséget biztosít az Ön tooexperience Azure Functions előtt véglegesítése toohello felhő. Ezzel a módszerrel hello kód eszközök készít majd átvihető toohello felhőhöz való áttelepítésekor.  hello futásidejű új beállítások, például éjszaka használatával a helyszíni számítógépek toorun kötegelt folyamatok hello tartalék számítási teljesítményt is megnyílik. A szervezet tooconditionally send data tooother rendszerek, mind a helyszíni és felhőben hello eszközök is használható.

hello Azure Functions Futtatókörnyezettel két részből áll:
* Az Azure Functions futásidejű kezelés szerepköre
* Az Azure Functions futásidejű feldolgozói szerepkör

## <a name="azure-functions-management-role"></a>Az Azure Functions kezelés szerepköre

hello Azure Functions szerepkör állomás biztosít a funkciók a helyszíni hello kezelését. Ez a szerepkör hello a következő feladatokat hajtja végre:

* Hello Azure funkciók felügyeleti portálján, amely hello tárolása hello megegyezik a hello látni [Azure-portálon](https://portal.azure.com). Ez lehetővé teszi a funkciók fejleszt hello azonos módon, mint az hello Azure-portálon.
* Funkciók osztja több funkciók dolgozó között.
* A közzétételi végpont biztosítása, így közzéteheti a függvényeket közvetlenül a Microsoft Visual Studio eszközből.

## <a name="azure-functions-worker-role"></a>Az Azure Functions feldolgozói szerepkör

hello Azure Functions feldolgozói szerepkörök telepítve van a Windows-tárolókban, és azt, ahol a funkciókódot hajt végre.  Több feldolgozói szerepköröket telepítheti a szervezetben, és ahol tehetik kulcs úgy használjon tartalék számítási teljesítményt.

## <a name="minimum-requirements"></a>Minimumkövetelmények

tooget használatába hello Azure Functions Futtatókörnyezettel kell rendelkeznie a virtuális gép **Windows Server 2016 vagy a Windows 10 Creators Update** való hozzáférés tooa **SQL Server** példány.

## <a name="next-steps"></a>Következő lépések

Telepítse a hello [Azure Functions Futtatókörnyezettel előzetes verzió](https://aka.ms/azafr)

<!--Image references-->
[1]: ./media/functions-runtime-overview/AzureFunctionsRuntime_Portal.png
