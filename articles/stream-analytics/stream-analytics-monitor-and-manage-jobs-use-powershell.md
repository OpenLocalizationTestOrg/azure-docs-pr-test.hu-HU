---
title: "aaaMonitor és a PowerShell-lel Stream Analytics-feladatok kezelése |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Azure PowerShell és a parancsmagok toomonitor és a Stream Analytics-feladatok kezelése."
keywords: az Azure powershell, az azure powershell-parancsmagok, a powershell-paranccsal, powershell-parancsprogramok
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 514f454e-d18c-4081-8304-ab48577e15e8
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 44abc82f1c44a5ebc1701badd6547b84dac239b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-stream-analytics-jobs-with-azure-powershell-cmdlets"></a>Figyelheti és kezelheti a Stream Analytics-feladatok Azure PowerShell-parancsmagokkal
Megtudhatja, hogyan toomonitor és felügyelhetők a Stream Analytics erőforrások az Azure PowerShell-parancsmagok és a powershell-parancsprogramok, amelyek alapvető Stream Analytics-feladatok végrehajtása.

## <a name="prerequisites-for-running-azure-powershell-cmdlets-for-stream-analytics"></a>Azure PowerShell-parancsmagok futtatását a Stream Analytics előfeltételei
* Azure-erőforráscsoport létrehozása az előfizetésben. hello az alábbiakban látható egy minta Azure PowerShell-parancsfájlt. Azure PowerShell információkért lásd: [telepítse és konfigurálja az Azure Powershellt](/powershell/azure/overview);  

Az Azure PowerShell 0.9.8-as:  

         # Log in tooyour Azure account
        Add-AzureAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group if you have more than one subscription on your account.
        Select-AzureSubscription -SubscriptionName <subscription name>

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>

Az Azure PowerShell 1.0:  

         # Log in tooyour Azure account
        Login-AzureRmAccount

        # Select hello Azure subscription you want toouse toocreate hello resource group.
        Get-AzureRmSubscription –SubscriptionName “your sub” | Select-AzureRmSubscription

        # If Stream Analytics has not been registered toohello subscription, remove remark symbol below (#) toorun hello Register-AzureProvider cmdlet tooregister hello provider namespace.
        #Register-AzureRmResourceProvider -Force -ProviderNamespace 'Microsoft.StreamAnalytics'

        # Create an Azure resource group
        New-AzureRMResourceGroup -Name <YOUR RESOURCE GROUP NAME> -Location <LOCATION>



> [!NOTE]
> Hozta létre a Stream Analytics-feladatok nincs figyelés alapértelmezés szerint engedélyezett.  Manuálisan engedélyezheti hello Azure Portal toohello feladat figyelő lapon lépjen a megfigyelést és vagy az Ön hello engedélyezése gombra kattintva teheti meg programozott módon helyen hello lépéseket követve [Azure Stream Analytics - figyelő adatfolyam Elemzés feladatok programozottan](stream-analytics-monitor-jobs.md).
> 
> 

## <a name="azure-powershell-cmdlets-for-stream-analytics"></a>A Stream Analytics az Azure PowerShell-parancsmagjai
a következő Azure PowerShell-parancsmagok hello használt toomonitor lehetnek és Azure Stream Analytics-feladatok kezelése. Vegye figyelembe, hogy az Azure PowerShell különböző verziói. 
**A felsorolt hello első parancs az Azure PowerShell 0.9.8-as hello példákban hello második parancs az Azure PowerShell 1.0 van.** hello Azure PowerShell 1.0 parancsok mindig lesz "AzureRM" hello parancsban.

### <a name="get-azurestreamanalyticsjob--get-azurermstreamanalyticsjob"></a>Get-AzureStreamAnalyticsJob |} Get-AzureRMStreamAnalyticsJob
Hello Azure-előfizetés vagy a megadott erőforráscsoport összes Stream Analytics-feladatok listája, vagy egy adott feladat erőforráscsoporton belül feladat információ lekérése.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsJob

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob

A PowerShell-parancs minden hello Stream Analytics-feladatok információ hello Azure-előfizetés adja vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US 

A PowerShell-parancs minden hello Stream Analytics-feladatok információ hello erőforráscsoport StreamAnalytics-alapértelmezett-közép-amerikai adja vissza.

**3. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob

A PowerShell-parancs hello Stream Analytics-feladat StreamingJob információ hello erőforráscsoport StreamAnalytics-alapértelmezett-közép-amerikai adja vissza.

### <a name="get-azurestreamanalyticsinput--get-azurermstreamanalyticsinput"></a>Get-AzureStreamAnalyticsInput |} Get-AzureRMStreamAnalyticsInput
Felsorolja az összes megadott Stream Analytics-feladatban definiált hello bemenet, vagy egy adott bevitel információ lekérése.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancs hello feladat StreamingJob definiált összes hello bemenet információt ad vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name EntryStream

A PowerShell-parancs hello feladat StreamingJob definiált EntryStream nevű hello bemeneti információt ad vissza.

### <a name="get-azurestreamanalyticsoutput--get-azurermstreamanalyticsoutput"></a>Get-AzureStreamAnalyticsOutput |} Get-AzureRMStreamAnalyticsOutput
Felsorolja az összes megadott Stream Analytics-feladatban definiált hello kimenetek, vagy egy adott kimeneti információ lekérése.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob

A PowerShell-parancs hello feladat StreamingJob definiált hello kimenetek információt ad vissza.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name Output

A PowerShell-parancs hello feladat StreamingJob meghatározott kimeneti nevű hello kimeneti információt ad vissza.

### <a name="get-azurestreamanalyticsquota--get-azurermstreamanalyticsquota"></a>Get-AzureStreamAnalyticsQuota |} Get-AzureRMStreamAnalyticsQuota
Az adatfolyam-egységek meghatározott hello kvóta információ lekérése.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsQuota –Location "Central US" 

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsQuota –Location "Central US" 

A PowerShell-parancs hello kvóta, valamint a folyamatos átviteli egységeket információ hello központi US régió adja vissza.

### <a name="get-azurestreamanalyticstransformation--getazurermstreamanalyticstransformation"></a>Get-AzureStreamAnalyticsTransformation |} GetAzureRMStreamAnalyticsTransformation
Egy Stream Analytics-feladatban definiált átalakítási információ lekérése.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Get-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

Az Azure PowerShell 1.0:  

    Get-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –Name StreamingJob

A PowerShell-parancs StreamingJob meghívta hello feladat StreamingJob hello átalakítása információt ad vissza.

### <a name="new-azurestreamanalyticsinput--new-azurermstreamanalyticsinput"></a>Új AzureStreamAnalyticsInput |} Új AzureRMStreamAnalyticsInput
Létrehoz egy új bemeneti belül a Stream Analytics-feladat, vagy egy meglévő megadott bemeneti adatok frissítése.

hello hello bemeneti név adható meg hello .JSON kiterjesztésű fájlt vagy hello parancssorban. Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.

Ha nem ad meg és adjon meg egy már létező bemeneti hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő bemeneti.

Ha megadja hello – Force paramétert, és adja meg egy létező adjon meg nevet, a hello bemeneti váltja megerősítés nélküli megadására.

A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [bemeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-input] hello szakasza [Stream Analytics felügyeleti REST API-t Referenciatárában][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" 

A PowerShell-parancs létrehoz egy új bemeneti Input.json hello fájlból. Ha a meglévő bemeneti hello nevet hello bemeneti szolgáltatásdefiníciós fájlban megadott névvel már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream

A PowerShell-parancs egy új bemeneti EntryStream nevű hello feladatot hoz létre. Ha egy meglévő bemeneti ezen a néven már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.

**3. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US -JobName StreamingJob –File "C:\Input.json" –Name EntryStream -Force

A PowerShell-parancs a felváltja hello meglévő bemeneti forrás EntryStream meghívásra hello definíciós fájlból hello hello meghatározását.

### <a name="new-azurestreamanalyticsjob--new-azurermstreamanalyticsjob"></a>Új AzureStreamAnalyticsJob |} Új AzureRMStreamAnalyticsJob
Létrehoz egy új Stream Analytics-feladat a Microsoft Azure-ban, vagy frissíti a létező megadott feladatnak hello meghatározása.

hello feladat neve hello hello .JSON kiterjesztésű fájlt vagy a parancssori hello adható meg. Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.

Ha nem ad meg és adja meg a feladat nevét, amely már létezik hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő feladat.

Ha megadja hello – Force paramétert, és adjon meg egy létező feladat, hello feladatdefiníció váltja megerősítés nélküli megadására.

A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [Stream Analytics-feladat létrehozása] [ msdn-rest-api-create-stream-analytics-job] hello szakasza [Stream Analytics felügyeleti REST API-referencia Szalagtár][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" 

A PowerShell-parancs létrehoz egy új feladatot a JobDefinition.json hello definícióból. Ha egy meglévő feladat hello nevet hello feladat szolgáltatásdefiníciós fájlban megadott névvel már van definiálva, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\JobDefinition.json" –Name StreamingJob -Force

A PowerShell-parancs hello feladatdefiníció StreamingJob a felváltja.

### <a name="new-azurestreamanalyticsoutput--new-azurermstreamanalyticsoutput"></a>Új AzureStreamAnalyticsOutput |} Új AzureRMStreamAnalyticsOutput
Létrehoz egy új kimeneti belül a Stream Analytics-feladat, vagy frissíti a meglévő kimenettel.  

hello .JSON kiterjesztésű fájlt vagy a parancssori hello hello kimenet hello neve adható meg. Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.

Ha egy már létező kimeneti adja meg, és ne adjon meg hello – Force paramétert hello parancsmag ekkor megkérdezi, hogy tooreplace hello meglévő kimeneti.

Ha megadja hello – Force paramétert, és adja meg a név egy meglévő kimeneti hello kimeneti váltja megerősítés nélküli megadására.

A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [kimeneti létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-output] hello szakasza [Stream Analytics felügyeleti REST API-n Referenciatárában][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output

A PowerShell-parancs egy "kimeneti" nevű hello feladat StreamingJob új kimenetet hoz létre. Ha egy meglévő kimeneti ezen a néven már definiálva van, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Output.json" –JobName StreamingJob –Name output -Force

A PowerShell-parancs a "kimeneti" hello feladat StreamingJob hello definíciója váltja fel.

### <a name="new-azurestreamanalyticstransformation--new-azurermstreamanalyticstransformation"></a>Új AzureStreamAnalyticsTransformation |} Új AzureRMStreamAnalyticsTransformation
Létrehoz egy új átalakítása belül a Stream Analytics-feladat, vagy frissíti a meglévő átalakítása hello.

hello .JSON kiterjesztésű fájlt vagy a parancssori hello hello átalakítása hello neve adható meg. Ha mindkettő meg van adva, kell lennie hello ugyanaz, mint a hello hello fájlban hello parancssorban hello neve.

Ha nem adja meg és adja meg, hogy már létezik az átalakítás hello – Force paramétert hello parancsmag felhasználói jóváhagyást kér-e tooreplace hello meglévő átalakítása.

Ha megadja hello – Force paramétert, és adjon meg egy meglévő átalakítása, hello átalakítása váltja megerősítés nélküli megadására.

A JSON-fájlstruktúra hello és tartalma részletes információkért tekintse meg a toohello [átalakítása létrehozása (Azure Stream Analytics)] [ msdn-rest-api-create-stream-analytics-transformation] hello szakasza [Stream Analytics felügyeleti REST API Referenciatárában][stream.analytics.rest.api.reference].

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform

A PowerShell-parancs hello feladat StreamingJob StreamingJobTransform nevű új átalakítás hoz létre. Ha egy meglévő átalakítást már definiálva van ilyen nevű, hello parancsmag felhasználói jóváhagyást kér-e tooreplace azt.

**2. példa**

Az Azure PowerShell 0.9.8-as:  

    New-AzureStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

Az Azure PowerShell 1.0:  

    New-AzureRMStreamAnalyticsTransformation -ResourceGroupName StreamAnalytics-Default-Central-US –File "C:\Transformation.json" –JobName StreamingJob –Name StreamingJobTransform -Force

 A PowerShell-parancs a felváltja hello feladat StreamingJob hello StreamingJobTransform meghatározása.

### <a name="remove-azurestreamanalyticsinput--remove-azurermstreamanalyticsinput"></a>Remove-AzureStreamAnalyticsInput |} Remove-AzureRMStreamAnalyticsInput
Aszinkron módon törli egy adott bevitel a Stream Analytics-feladat, a Microsoft Azure-ban.  
Ha megad hello – Force paramétert, bemeneti hello törli megerősítés nélküli megadására.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Remove-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

Az Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EventStream

A PowerShell-parancs eltávolítja a bemeneti EventStream hello feladat StreamingJob hello.  

### <a name="remove-azurestreamanalyticsjob--remove-azurermstreamanalyticsjob"></a>Remove-AzureStreamAnalyticsJob |} Remove-AzureRMStreamAnalyticsJob
Aszinkron módon törli egy adott, a Microsoft Azure Stream Analytics-feladat.  
Ha megad hello – Force paramétert hello feladat lesz törölve megerősítés nélküli megadására.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Remove-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Az Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

A PowerShell-parancs hello feladat StreamingJob eltávolítja.  

### <a name="remove-azurestreamanalyticsoutput--remove-azurermstreamanalyticsoutput"></a>Remove-AzureStreamAnalyticsOutput |} Remove-AzureRMStreamAnalyticsOutput
A megadott kimeneti aszinkron módon töröl egy Microsoft Azure Stream Analytics-feladat.  
Ha megad hello – Force paramétert hello kimenet lesz törölve megerősítés nélküli megadására.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Remove-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Az Azure PowerShell 1.0:  

    Remove-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell-parancs eltávolítja hello a kimeneti hello feladat StreamingJob kimenetet.  

### <a name="start-azurestreamanalyticsjob--start-azurermstreamanalyticsjob"></a>Start-AzureStreamAnalyticsJob |} Start-AzureRMStreamAnalyticsJob
Aszinkron módon telepíti, és a Microsoft Azure Stream Analytics-feladat elindul.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Start-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

Az Azure PowerShell 1.0:  

    Start-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US -Name StreamingJob -OutputStartMode CustomTime -OutputStartTime 2012-12-12T12:12:12Z

A PowerShell-parancs indításakor hello StreamingJob egyéni kimeneti kezdési időpont beállítása tooDecember 12, 2012, 12:12:12 feladat UTC.

### <a name="stop-azurestreamanalyticsjob--stop-azurermstreamanalyticsjob"></a>STOP-AzureStreamAnalyticsJob |} STOP-AzureRMStreamAnalyticsJob
Aszinkron módon történik a Stream Analytics-feladat, a Microsoft Azure-beli leáll, és felszabadítása az erőforrásokhoz, melyeket volt használatban. hello feladatdefiníció és metaadatok elérhető marad hello Azure-portál és a felügyeleti API-k, az előfizetésen belül, hogy hello feladat szerkeszthető és újraindul. Nem kell fizetnie a feladat leállt hello állapotban.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Stop-AzureStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

Az Azure PowerShell 1.0:  

    Stop-AzureRMStreamAnalyticsJob -ResourceGroupName StreamAnalytics-Default-Central-US –Name StreamingJob 

A PowerShell-parancs hello feladat StreamingJob leállítása.  

### <a name="test-azurestreamanalyticsinput--test-azurermstreamanalyticsinput"></a>Teszt-AzureStreamAnalyticsInput |} Teszt-AzureRMStreamAnalyticsInput
Tesztek hello képességét Stream Analytics tooconnect tooa megadott bemenetet.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Test-AzureStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

Az Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsInput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name EntryStream

A PowerShell parancsot tesztek hello kapcsolati állapotához hello bemeneti EntryStream a StreamingJob.  

### <a name="test-azurestreamanalyticsoutput--test-azurermstreamanalyticsoutput"></a>Teszt-AzureStreamAnalyticsOutput |} Teszt-AzureRMStreamAnalyticsOutput
A megadott kimeneti tesztek hello képességét Stream Analytics tooconnect tooa.

**1. példa**

Az Azure PowerShell 0.9.8-as:  

    Test-AzureStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

Az Azure PowerShell 1.0:  

    Test-AzureRMStreamAnalyticsOutput -ResourceGroupName StreamAnalytics-Default-Central-US –JobName StreamingJob –Name Output

A PowerShell parancsot tesztek hello kapcsolati állapotához hello kimeneti StreamingJob-kimenetet.  

## <a name="get-support"></a>Támogatás kérése
Ha további segítségre van szüksége, próbálkozzon a [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics). 

## <a name="next-steps"></a>Következő lépések
* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

[msdn-switch-azuremode]: http://msdn.microsoft.com/library/dn722470.aspx
[powershell-install]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
[msdn-rest-api-create-stream-analytics-job]: https://msdn.microsoft.com/library/dn834994.aspx
[msdn-rest-api-create-stream-analytics-input]: https://msdn.microsoft.com/library/dn835010.aspx
[msdn-rest-api-create-stream-analytics-output]: https://msdn.microsoft.com/library/dn835015.aspx
[msdn-rest-api-create-stream-analytics-transformation]: https://msdn.microsoft.com/library/dn835007.aspx

[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301

