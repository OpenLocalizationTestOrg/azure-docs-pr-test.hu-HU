---
title: aaaPowerShell modul a Machine Learning |} Microsoft Docs
description: "az Azure Machine Learning hello PowerShell-moduljának nyilvános előzetes üzemmódban érhető el. PowerShell toocreate használatát, és munkaterületek, kísérleteket, webszolgáltatások és további kezelését."
keywords: "kísérlet,lineáris regresszió,machine learning-algoritmusok,machine learning-oktatóanyag,prediktív modellezési technikák,adatelemzési kísérlet"
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: a9001cc2-3aa0-47e1-b175-1f76408ba1d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/15/2017
ms.author: garye;haining
ms.openlocfilehash: 59362027356b86bf286b7c07380db677ae1d71c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="powershell-module-for-microsoft-azure-machine-learning"></a>A Microsoft Azure Machine Learning PowerShell-modulja
Azure Machine Learning hello PowerShell-modul, amely lehetővé teszi a toouse Windows PowerShell toomanage munkaterületek, kísérleteket, adatkészletek, klasszikus webszolgáltatások és további hatékony eszköz.

Hello dokumentációjában tekintheti meg és töltse le a hello modul hello teljes forráskód, valamint a [https://aka.ms/amlps](https://aka.ms/amlps). 

> [!NOTE]
> hello Azure Machine Learning PowerShell modul jelenleg csak előzetes módban működik. hello modul folytatja a toobe továbbfejlesztett és a próbaidőszak alatt kibontva. Nyomon követheti a hello [Cortana Intelligence és a Machine Learning Blog](https://blogs.technet.microsoft.com/machinelearning/) hírek és információ.

## <a name="what-is-hello-machine-learning-powershell-module"></a>Mi az a Machine Learning PowerShell-modul hello?
hello a Machine Learning PowerShell modul egy. NET-alapú DLL-modult, amely lehetővé teszi toofully Azure Machine Learning munkaterületek, kísérleteket, adatkészleteket, klasszikus webszolgáltatások és klasszikus webszolgáltatás-végpontok kezelése Windows PowerShell. 

Hello modul, valamint letöltheti a köztük szabályszerűen elkülönített hello teljes forráskód [C# API réteg](https://github.com/hning86/azuremlps/blob/master/code/AzureMLSDK.cs). A dll-Fájlnak a saját .NET projekt hivatkozik, és kezelése az Azure Machine Learning keresztül .NET-kódot. Ezenkívül hello dll-fájl, amely közvetlenül a kedvenc ügyfélről is használhat, alapul szolgáló REST API-k függ.

## <a name="what-can-i-do-with-hello-powershell-module"></a>Mi a teendő hello PowerShell modullal?
Az alábbiakban hello feladatokat hajthat végre egy a PowerShell-modult. Tekintse meg a hello [teljes dokumentáció](https://aka.ms/amlps) az ezekhez és számos további funkciót.

* Új munkaterület kiépítése felügyeleti tanúsítvány használatával ([New-AmlWorkspace](https://github.com/hning86/azuremlps#new-amlworkspace))
* Kísérleti diagramot jelölő JSON-fájlok exportálása és importálása ([Export-AmlExperimentGraph](https://github.com/hning86/azuremlps#export-amlexperimentgraph) és [Import-AmlExperimentGraph](https://github.com/hning86/azuremlps#import-amlexperimentgraph))
* Kísérlet futtatása ([Start-AmlExperiment](https://github.com/hning86/azuremlps#start-amlexperiment))
* Webszolgáltatás létrehozása prediktív kísérletből ([New-AmlWebService](https://github.com/hning86/azuremlps#new-amlwebservice))
* Hozzon létre egy végpontot a közzétett webes szolgáltatás ([Add-AmlWebServiceEndpoint](https://github.com/hning86/azuremlps#add-amlwebserviceendpoint))
* RRS és/vagy BES webszolgáltatás-végpont meghívása ([Invoke-AmlWebServiceRRSEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicerrsendpoint) és [Invoke-AmlWebServicBESEndpoint](https://github.com/hning86/azuremlps#invoke-amlwebservicebesendpoint))

Íme egy PowerShell toorun egy meglévő kísérlet használatának gyors példája:

        #Find hello first Experiment named “xyz”
        $exp = (Get-AmlExperiment | where Description -eq ‘xyz’)[0]
        #Run hello Experiment
        Start-AmlExperiment -ExperimentId $exp.ExperimentId 

Részletesebb használati eset, lásd: Ez a cikk hello PowerShell modul tooautomate általában a kért feladat használatával: [létrehozása számos Machine Learning modellek és webes Szolgáltatásvégpontok PowerShell-lel egy kísérlet](machine-learning-create-models-and-endpoints-with-powershell.md).

## <a name="how-do-i-get-started"></a>Hogyan kezdhetek hozzá?
a Machine Learning PowerShell használatába tooget, töltse le a hello [kiadási csomag](https://github.com/hning86/azuremlps/releases) a Githubról, majd hajtsa végre hello [telepítési utasításokat](https://github.com/hning86/azuremlps/blob/master/README.md). hello utasítások azt ismertetik, hogyan toounblock hello letöltött/unzipped dll-fájl, és importálja azt a PowerShell környezetben. Hello parancsmagok hello munkaterület azonosítója, hello munkaterület engedélyezési jogkivonat, adja meg, és az Azure-régió, hogy a munkaterület hello hello igényelnek a legtöbb van. hello legegyszerűbb módja tooprovide hello értékek alapértelmezett config.json fájl keresztül történik. hello utasításokat is bemutatják, hogyan tooconfigure ezt a fájlt. 

És ha azt szeretné, hello git fa klónozza, hello kód módosítása, hogy helyileg a Visual Studio használatával.

## <a name="next-steps"></a>Következő lépések
Hello teljes hello PowerShell modul dokumentációjában található [https://aka.ms/amlps](https://aka.ms/amlps). 

Például egy kiterjesztett hogyan toouse hello modul valós forgatókönyv esetében, részletes hello kivétel-és nagybetűhasználattal, [létrehozása számos Machine Learning modellek és webes Szolgáltatásvégpontok PowerShell-lel egy kísérlet](machine-learning-create-models-and-endpoints-with-powershell.md).
