---
title: "a gépi tanulási modellek aaaRetrain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooretrain modell és a frissítés hello Web service toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: d1cb6088-4f7c-4c32-94f2-f7523dad9059
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 342bb9954105339b4b634ff20968a64f4f9f750e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-machine-learning-model"></a>A gépi tanulási modell újratanítása
A machine learning modellek Azure Machine Learning operationalization hello folyamat részeként a modell betanítása és mentve. Majd használatba vétel toocreate predicative webszolgáltatás. a webhelyek, az irányítópultok és a mobilalkalmazások hello webszolgáltatás majd használni képes. 

Használata a Machine Learning modellek jellemzően nem statikus. Amint az elérhetővé válik az új adatok, vagy ha a hello API hello fogyasztói rendelkezik a saját adatok hello modellnek támogatnia kell a toobe retrained. 

Átképezési fordulhat elő, gyakran. Hello programozott Átképezési API szolgáltatással hello Átképezési API-kat és a frissítés hello webszolgáltatás segítségével hello újonnan betanított modell hello modell programozott módon működik. 

Ez a dokumentum folyamat átképezési hello ismerteti és bemutatja, hogyan toouse hello Átképezési API-kat.

## <a name="why-retrain-defining-hello-problem"></a>Miért újratanítása: hello probléma meghatározása
Hello gépi tanulás betanítása folyamat részeként a modell betanítása adatok használatával. Használata a Machine Learning modellek jellemzően nem statikus. Amint az elérhetővé válik az új adatok, vagy ha a hello API hello fogyasztói rendelkezik a saját adatok hello modellnek támogatnia kell a toobe retrained.

Ezekben az esetekben a programozott API egy kényelmes módszert arra tooallow biztosít, akkor vagy hello felhasználóinak azon az API-k toocreate ügyfél, amely egy egyszeri vagy rendszeres időközönként újratanítása is, hello modell használatával saját adataikat. Ezek majd értékelje ki a átképezési hello eredményeit, és hello webes API toouse hello újonnan betanított modell frissítése.

> [!NOTE]
> Ha egy meglévő tanítási kísérletet, és az új webszolgáltatás-bővítmény, érdemes lehet toocheck kimenő teljesített kapcsolat-újraépítési egy meglévő prediktív webes szolgáltatás szerepel a következő szakasz hello következő hello forgatókönyv helyett.
> 
> 

## <a name="end-to-end-workflow"></a>Teljes körű munkafolyamat
hello magában foglalja a következő összetevők hello: A tanítási kísérletet, és egy prediktív Kísérletté közzétett webszolgáltatásként. a modell betanítását, tanítási kísérletet hello átképezési tooenable hello kimenet betanított modellek webszolgáltatásként tehetők közzé. Ez lehetővé teszi az átképezési API access toohello modelljét. 

a lépéseket követve hello tooboth új és a klasszikus Web services vonatkoznak:

Hozzon létre hello kezdeti prediktív webszolgáltatás:

* Hozzon létre egy tanítási kísérletet
* A prediktív webes kísérlet létrehozása
* Egy prediktív webszolgáltatás-bővítmény telepítése

Hello webszolgáltatás működik:

* A frissítési kísérlet tooallow az átképezési betanítása
* Hello átképezési webszolgáltatás telepítése
* Hello kötegelt végrehajtási szolgáltatás tooretrain hello kódmodell használata

Az előző lépésekben hello útmutatást lásd: [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

Ha telepítette a klasszikus webszolgáltatás:

* Új végpont létrehozásához a hello prediktív webszolgáltatás
* Hello javítás URL-cím és a kód
* Használjon hello javítás URL-cím toopoint hello hello helyen új végpont retrained modell 

Az előző lépésekben hello útmutatást lásd: [klasszikus webszolgáltatás újratanítása](machine-learning-retrain-a-classic-web-service.md).

Ha a klasszikus webszolgáltatás átképezési nehézségek futtatja, lásd: [hibaelhárítása az Azure Machine Learning klasszikus webszolgáltatás az átképezési hello](machine-learning-troubleshooting-retraining-models.md).

Ha egy új webszolgáltatás-bővítmény:

* Jelentkezzen be Azure Resource Manager fiók tooyour
* Hello webszolgáltatás-definíciójának beolvasása
* Webszolgáltatás-definíciójának hello JSON exportálása
* Hello hivatkozás toohello frissítése `ilearner` hello JSON a blob
* A JSON-kódot egy webszolgáltatás-definíciójának importálása hello
* Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése

Az előző lépésekben hello útmutatást lásd: [egy új webszolgáltatás-bővítmény hello Machine Learning Management PowerShell-parancsmagok használatával újratanítása](machine-learning-retrain-new-web-service-using-powershell.md).

hello állíthatja be a klasszikus webszolgáltatáshoz átképezési eljárásban hello a következő lépéseket:

![Megőrzési folyamat áttekintése][1]

hello állíthatja be az új webszolgáltatás átképezési eljárásban hello a következő lépéseket:

![Megőrzési folyamat áttekintése][7]

## <a name="other-resources"></a>Egyéb források
* [Azure Data Factory átképezési és frissítési Azure Machine Learning modellek](https://azure.microsoft.com/blog/retraining-and-updating-azure-machine-learning-models-with-azure-data-factory/)
* [Hozzon létre az számos Machine Learning modellek és webes szolgáltatás végpontok egy kísérlet PowerShell használatával](machine-learning-create-models-and-endpoints-with-powershell.md)
* Hello [AML Átképezési modellek használata API-k](https://www.youtube.com/watch?v=wwjglA8xllg) a videó bemutatja, hogyan tooretrain gépi tanulási modellek létrehozása az Azure Machine Learning segítségével hello Átképezési API-kat és a PowerShell használatával.

<!--image links-->
[1]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE01.png
[7]: ./media/machine-learning-retrain-machine-learning-model/machine-learning-retrain-models-programmatically-IMAGE07.png

