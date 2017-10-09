---
title: "aaaVertically méretezési Azure virtuálisgép-méretezési csoportok |} Microsoft Docs"
description: "Hogyan toovertically méretezése válasz toomonitoring értesítések az Azure Automation szolgáltatásban, a virtuális gép"
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: madhana
editor: 
tags: azure-resource-manager
ms.assetid: 16b17421-6b8f-483e-8a84-26327c44e9d3
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/03/2016
ms.author: guybo
ms.openlocfilehash: 1cc35a805b6a5742252a89c21588ca451ff547a3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="vertical-autoscale-with-virtual-machine-scale-sets"></a>Virtuálisgép-méretezési csoportok függőleges skálázva
Ez a cikk ismerteti, hogyan toovertically méretezése Azure [virtuálisgép-méretezési csoportok](https://azure.microsoft.com/services/virtual-machine-scale-sets/) vagy reprovisioning nélkül. Virtuális gépek, amelyek nem szerepelnek a méretezési készlet függőleges skálázás, tekintse meg túl[függőleges skálázása az Azure virtuális gép az Azure Automation szolgáltatásban](../virtual-machines/windows/vertical-scaling-automation.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Függőleges skálázás, más néven *vertikális felskálázás* és *csökkentheti*, azt jelenti, hogy növelésével vagy csökkentésével válasz tooa terheléseknél engedélyezett virtuális gépek (VM) méretét. Hasonlítsa össze a [horizontális skálázás](virtual-machine-scale-sets-autoscale-overview.md), más néven tooas *horizontális felskálázás* és *méretezése*, ahol a virtuális gépek száma hello van-e módosítva az hello munkaterhelés függően.

Reprovisioning azt jelenti, hogy egy meglévő virtuális gép eltávolításakor, és cserélje le a egy újat. Vagy a Virtuálisgép-méretezési csoportban lévő virtuális gépek hello méretének csökkentése, egyes esetekben azt szeretné, hogy a meglévő virtuális gépek tooresize és megőrizni az adatokat, amíg más esetekben a toodeploy kell új virtuális gépeinek hello új méretét. Ez a dokumentum ismerteti a mindkét esetben.

Függőleges skálázás esetekben lehet hasznos:

* Egy szolgáltatás, amely a virtuális gépek szükség szerint (például a hétvégéket). Virtuálisgép-méretet hello csökkentése csökkentheti a havi költségeket.
* Virtuális gép mérete toocope a nagyobb igény szerinti növelésével további virtuális gépek létrehozása nélkül.

Függőleges állíthat be a metrika alapján riasztások a Virtuálisgép-méretezési csoportban indított toobe skálázás alapján. Hello riasztás aktiválásakor akkor következik be egy webhook, hogy a skála méretezhető, amely runbook beállítása felfelé vagy lefelé eseményindítók. Függőleges skálázás konfigurálhatja ezeket a lépéseket követve:

1. Hozzon létre egy Azure Automation-fiók futtató funkció.
2. Azure Automation függőleges méretezési runbookok a Virtuálisgép-méretezési készlet importálja az előfizetéshez.
3. Adjon hozzá egy webhook tooyour runbookot.
4. Adjon hozzá egy riasztási tooyour Virtuálisgép-méretezési csoportban webhook értesítést használ.

> [!NOTE]
> Függőleges automatikus skálázás csak akkor kerül sor a VM-méretek egyes tartományokon belül. Hasonlítsa össze a hello specifikációk minden méretű előtt tooscale egy tooanother a legmegfelelőbb (nagyobb szám nem mindig utalhat, hogy nagyobb virtuális gép mérete). Választhat, hogy tooscale hello párok méretű a következő között:
> 
> | Virtuálisgép-méretek pár skálázás |  |
> | --- | --- |
> | Standard_A0 |Standard_A11 |
> | Standard_D1 |Standard_D14 |
> | Standard_DS1 |Standard_DS14 |
> | Standard_D1v2 |Standard_D15v2 |
> | Standard_G1 |Standard szintű, G5 |
> | Standard_GS1 |Standard_GS5 |
> 
> 

## <a name="create-an-azure-automation-account-with-run-as-capability"></a>Hozzon létre egy Azure Automation-fiók futtató funkció
hello elsőként toodo szüksége, hozzon létre egy Azure Automation-fiók hello használt runbookok tooscale hello Virtuálisgép-méretezési csoport példányait futtató. Nemrég [Azure Automation](https://azure.microsoft.com/services/automation/) hello "Futtatás mint fiók" funkció automatikusan hello runbookok futtatásáért egy felhasználó nevében nagyon egyszerű szolgáltatásnevet hello beállítása így vezetett be. További a hello a cikkben az alábbi:

* [Runbookok hitelesítése Azure-beli futtató fiókkal](../automation/automation-sec-configure-azure-runas-account.md)

## <a name="import-azure-automation-vertical-scale-runbooks-into-your-subscription"></a>Azure Automation függőleges méretezési runbookok importálnia kell az előfizetéshez
runbookokat hello szükséges toovertically méretezési hello Azure Automation-Runbook gyűjteménye már közzé a Virtuálisgép-méretezési készlet. tooimport őket az előfizetés hello kövesse ebben a cikkben ismertetett visszaállítási lépésekkel:

* [Az Azure Automation forgatókönyv- és minták](../automation/automation-runbook-gallery.md)

Válassza ki a hello Tallózás gyűjtemény beállítást Runbookokat hello menüből:

![Importált Runbookok toobe][runbooks]

importált toobe igénylő runbookokat hello jelennek meg. Válassza ki azt kívánja-e függőleges vagy anélkül reprovisioning skálázás alapján hello runbookot:

![Runbookok gyűjteménye][gallery]

## <a name="add-a-webhook-tooyour-runbook"></a>A webhook tooyour runbook hozzáadása
Miután importált runbookokat hello szüksége lesz tooadd webhook toohello runbook úgy is elindítható a Virtuálisgép-méretezési csoportban a riasztások alapján. a webhook létrehozása a runbook hello részleteit ebben a cikkben ismertetett:

* [Azure Automation-webhook](../automation/automation-webhooks.md)

> [!NOTE]
> Ellenőrizze, hogy hello webhook URI hello webhook párbeszédpanel bezárása, szüksége lesz a következő szakaszban hello előtt másolja át.
> 
> 

## <a name="add-an-alert-tooyour-vm-scale-set"></a>Egy Virtuálisgép-méretezési csoportban riasztási tooyour hozzáadása
Az alábbiakban a PowerShell parancsfájl mely bemutatja, hogyan tooadd egy riasztási tooa Virtuálisgép-méretezési csoportban. Tekintse meg a cikk tooget hello hello metrika toofire hello riasztás nevét következő toohello: [Azure figyelő automatikus skálázás közös metrikák](../monitoring-and-diagnostics/insights-autoscale-common-metrics.md).

```
$actionEmail = New-AzureRmAlertRuleEmail -CustomEmail user@contoso.com
$actionWebhook = New-AzureRmAlertRuleWebhook -ServiceUri <uri-of-the-webhook>
$threshold = <value-of-the-threshold>
$rg = <resource-group-name>
$id = <resource-id-to-add-the-alert-to>
$location = <location-of-the-resource>
$alertName = <name-of-the-resource>
$metricName = <metric-to-fire-the-alert-on>
$timeWindow = <time-window-in-hh:mm:ss-format>
$condition = <condition-for-the-threshold> # Other valid values are LessThanOrEqual, GreaterThan, GreaterThanOrEqual
$description = <description-for-the-alert>

Add-AzureRmMetricAlertRule  -Name  $alertName `
                            -Location  $location `
                            -ResourceGroup $rg `
                            -TargetResourceId $id `
                            -MetricName $metricName `
                            -Operator  $condition `
                            -Threshold $threshold `
                            -WindowSize  $timeWindow `
                            -TimeAggregationOperator Average `
                            -Actions $actionEmail, $actionWebhook `
                            -Description $description
```

> [!NOTE]
> Az ajánlott tooconfigure egy ésszerű időkerete hello riasztással rendelés tooavoid kiváltó függőleges méretezés és szolgáltatáskiesést, túl gyakran az esetleg hozzá tartozó. Fontolja meg egy legalább 20-30 perc vagy több ablakot. Vegye figyelembe a horizontális skálázás, ha tooavoid megszakításának van szüksége.
> 
> 

További információ a hogyan toocreate riasztások tekintse meg a következő cikkek toohello:

* [A figyelő PowerShell Azure gyors üzembe helyezési-minták](../monitoring-and-diagnostics/insights-powershell-samples.md)
* [Figyelő platformfüggetlen parancssori felület Azure gyors üzembe helyezési-minták](../monitoring-and-diagnostics/insights-cli-samples.md)

## <a name="summary"></a>Összefoglalás
Ez a cikk bemutatta egyszerű függőleges méretezési példákat. Ezeket az építőelemeket - Automation-fiók, a runbookok, webhookokkal, riasztások - események gazdag számos műveletek testreszabott számú csatlakoztathatja.

[runbooks]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks.png
[gallery]: ./media/virtual-machine-scale-sets-vertical-scale-reprovision/runbooks-gallery.png
