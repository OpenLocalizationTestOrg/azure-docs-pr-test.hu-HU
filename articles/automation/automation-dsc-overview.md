---
title: "aaaAzure Automation DSC – áttekintés |} Microsoft Docs"
description: "Egy áttekintés az Azure Automation szükséges konfiguráló (DSC), a feltételek és ismert problémák"
services: automation
documentationcenter: dev-center-name
author: eslesar
manager: carmonm
keywords: "PowerShell dsc, a kívánt állapot konfigurációs, a powershell dsc azure"
ms.assetid: fd40cb68-c1a6-48c3-bba2-710b607d1555
ms.service: automation
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: powershell
ms.workload: TBD
ms.date: 06/15/2017
ms.author: eslesar
ms.openlocfilehash: 5b8e5104c7b5bed848c015ac26a8b7d1f5b24de9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-automation-dsc-overview"></a>Azure Automation DSC – áttekintés

Az Azure Automation DSC az Azure-szolgáltatások, amely lehetővé teszi toowrite, kezelése és PowerShell kívánt állapot konfigurációs szolgáltatása (DSC) fordítási [konfigurációk](https://msdn.microsoft.com/powershell/dsc/configurations), importálja [DSC erőforrások](https://msdn.microsoft.com/powershell/dsc/resources), és rendelje hozzá konfigurációk tootarget csomópontok, minden hello felhőben.

## <a name="why-use-azure-automation-dsc"></a>Miért érdemes használni az Azure Automation DSC

Az Azure Automation DSC számos előnye van, mint az Azure-on kívüli DSC.

### <a name="built-in-pull-server"></a>Beépített lekérési kiszolgálójával

Azure Automation szolgáltatásbeli biztosít egy [DSC lekérési kiszolgálójával](https://msdn.microsoft.com/en-us/powershell/dsc/pullserver) , hogy a célcsomópontokat automatikusan megjelenik a konfigurációk, felel meg a szükséges toohello állapotát, és megfelelőségi jelentéseket küldhetnek vissza.
hello beépített lekérési kiszolgálójával, az Azure Automationben hello kell tooset kiküszöböli be, és karbantartása a saját lekérési kiszolgálójával.
Azure Automation szolgáltatásbeli virtuális vagy fizikai Windows vagy Linux rendszerű gépek, hello felhőalapú vagy helyszíni célba.

### <a name="management-of-all-your-dsc-artifacts"></a>A DSC-összetevők felügyelete

Az Azure Automation DSC számos lehetőséget kínál azonos felügyeleti réteg túl hello[PowerShell célállapot-konfiguráció](https://msdn.microsoft.com/powershell/dsc/overview) , az Azure Automation kínál a PowerShell-parancsprogramok.

Hello Azure-portálon, vagy a PowerShell kezelheti az összes a DSC konfigurációk, erőforrások és célcsomópontokat.

![Képernyőfelvétel a hello Azure Automation panel](./media/automation-dsc-overview/azure-automation-blade.png)

### <a name="import-reporting-data-into-log-analytics"></a>Jelentési adatok importálása a Naplóelemzési

Az Azure Automation DSC Szolgáltatásban felügyelt csomópontok részletes jelentéskészítési állapot adatok toohello beépített lekérési kiszolgálójával küldése.
Azure Automation DSC toosend konfigurálhatja az adatok tooyour a Microsoft Operations Management Suite (OMS) Naplóelemzési munkaterület.
Hogyan toosend DSC állapot adatok tooyour Naplóelemzési munkaterület: toolearn [előre Azure Automation DSC jelentési adatok tooOMS Naplóelemzési](automation-dsc-diagnostics.md).

## <a name="introduction-video"></a>Bevezető videó

Tanul tooreading? Tekintse meg a következő videó 2015. május, amikor bejelentette először Azure Automation DSC hello rendelkezik.

>[!NOTE]
>Hello fogalmak és a videó tárgyalt életciklusa helyességét, amíg a Azure Automation DSC sokkal fejlődött, mivel ez a videó lett felvéve.
>Az általánosan elérhető van jóval szélesebb körű felhasználói Felületet a hello Azure-portálon, és számos további képességeket biztosít.

> [!VIDEO https://channel9.msdn.com/Events/Ignite/2015/BRK3467/player]

## <a name="next-steps"></a>Következő lépések

* toolearn hogyan tooonboard csomópontok toobe felügyelete az Azure Automation DSC Szolgáltatásban, lásd: [bevezetési gépeket Azure Automation DSC általi kezelésre](automation-dsc-onboarding.md)
* Azure Automation DSC használatának tooget lásd [Ismerkedés az Azure Automation DSC](automation-dsc-getting-started.md)
* információ fordítása a DSC-konfigurációk, így hozzárendelheti azokat tootarget csomópontok toolearn lásd [fordítása Azure Automation DSC-konfigurációja](automation-dsc-compile.md)
* PowerShell parancsmag-referencia az Azure Automation DSC Szolgáltatásban, lásd: [Azure Automation DSC-parancsmagok](/powershell/module/azurerm.automation/#automation)
* Díjszabási információkért lásd: [Azure Automation DSC díjszabása](https://azure.microsoft.com/pricing/details/automation/)
* Azure Automation DSC használata a folyamatos üzembe helyezés sorban, például toosee lásd: [folyamatos üzembe helyezés tooIaaS virtuális gépek használata Azure Automation DSC és Chocolatey](automation-dsc-cd-chocolatey.md)
