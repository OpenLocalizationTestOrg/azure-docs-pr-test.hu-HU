---
title: "aaaExcel bővítmény a Machine Learning webszolgáltatások |} Microsoft Docs"
description: "Azure Machine Learning Web toouse hogyan közvetlenül az Excel services-programozás nélkül."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 9618079d-502f-4974-a3e2-8f924042a23f
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/14/2017
ms.author: tedway;garye
ms.openlocfilehash: c52f40d33c9907f284e4750afe47181dc3365fe5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="excel-add-in-for-azure-machine-learning-web-services"></a>Excel-bővítmény az Azure Machine Learning webszolgáltatásokhoz
Excel segítségével könnyen toocall webszolgáltatások közvetlenül hello kell toowrite kódok nélkül.

## <a name="steps-toouse-an-existing-web-service-in-hello-workbook"></a>Egy meglévő webszolgáltatás hello munkafüzetben szereplő lépéseket tooUse

1. Nyissa meg hello [minta Excel-fájl](http://aka.ms/amlexcel-sample-2), tartalmazó hello Excel-bővítmény, és az utasok adatait hello Titanic.
2. Kattintással válassza ki a hello webszolgáltatás-"Titanic túlélő Előrejelzőjének (Excel-bővítmény minta) [Pontszám]" Ebben a példában.
   
    ![Válassza ki a webszolgáltatás][01]
3. Ezzel megnyitná toohello **Predict** szakasz.  Ez a munkafüzet már tartalmaz adatot, de üres a munkafüzet az Excel programban a cella kijelöléséhez és kattintson a **mintaadatokkal**.
4. Válassza ki a fejlécek hello adatok és hello bemeneti adatok tartomány ikonra.  Győződjön meg arról, hogy hello "adataimat fejlécekkel rendelkezik" jelölőnégyzet be van jelölve.
5. A **kimeneti**, adja meg hello cella kívánt kimeneti toobe, például "H1" hello itt.
6. Kattintson a **előrejelzése**.
   
    ![A szakasz előrejelzése][02]

A webes szolgáltatás, vagy használja a meglévő webszolgáltatás. Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).

A webszolgáltatáshoz hello API-kulcs beszerzése. Ahol elvégezhető ez a művelet függ a klasszikus Machine Learning webszolgáltatás egy új Machine Learning-webszolgáltatás közzététele e.

**A klasszikus webszolgáltatás** 

1. A Machine Learning Studióban, kattintson a hello **WEBSZOLGÁLTATÁSOK** szakasz hello bal oldali ablaktáblán, majd válassza ki a hello webszolgáltatáshoz.
   
    ![Studio válasszon egy webszolgáltatás-bővítmény][04]
2. Másolja az API-kulcs hello hello webszolgáltatáshoz.
   
    ![Studio API-kulcs][05]
3. A hello **IRÁNYÍTÓPULT** hello webszolgáltatáshoz lapra, majd hello **kérelem/válasz** hivatkozásra.
4. Keresse meg hello **kérelem URI-azonosítója** szakasz.  Másolja ki és mentse a hello URL-CÍMÉT.

> [!NOTE]
> Már lehetséges toosign történő hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portál tooobtain hello API-kulcs egy klasszikus Machine Learning webszolgáltatáshoz.
> 
> 

**Új webszolgáltatás**

1. A hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon kattintson **webszolgáltatások**, majd válassza a webes szolgáltatás. 
2. Kattintson a **felhasználásához**.
3. Keresse meg hello **alapvető fogyasztási adatai** szakasz. Másolja ki és mentse a hello **elsődleges kulcs** és hello **kérés-válasz** URL-CÍMÉT.

## <a name="steps-tooadd-a-new-web-service"></a>Lépéseket tooAdd egy új webszolgáltatás-bővítmény

1. A webes szolgáltatás, vagy használja a meglévő webszolgáltatás. Egy webszolgáltatás-bővítmény telepítésével kapcsolatos további információkért lásd: [útmutató 5. lépés: hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md).
2. Kattintson a **felhasználásához**.
3. Keresse meg hello **alapvető fogyasztási adatai** szakasz. Másolja ki és mentse a hello **elsődleges kulcs** és hello **kérés-válasz** URL-CÍMÉT.
4. Az Excel programban, lépjen a toohello **webszolgáltatások** szakasz (Ha Ön a hello **Predict** hello vissza toogo toohello webes szolgáltatások listájában kattintson).
   
    ![Nyissa meg tooWeb szolgáltatás kiválasztása][03]
5. Kattintson a **webszolgáltatás hozzáadása**.
6. Hello URL-cím illessze be az Excel-bővítmény szövegmező feliratú hello **URL-cím**.
7. Beillesztés hello API/elsődleges kulcs feliratú hello szövegmezőbe **API-kulcs**.
8. Kattintson az **Add** (Hozzáadás) parancsra.
   
    ![URL-CÍMÉT és API-kulcsát egy klasszikus webszolgáltatáshoz.][06]
9. toouse hello webszolgáltatás, kövesse az utasításokat megelőző hello, "Lépések tooUse egy meglévő webes szolgáltatás."

## <a name="sharing-your-workbook"></a>A munkafüzet megosztása
Ha a munkafüzet mentéséhez hello API/elsődleges kulcs hello web Services hozzáadott is menti. Ez azt jelenti, hogy csak ossza meg hello munkafüzet megbízható személlyel.

Bármely kérdései vannak a következő megjegyzés szakasz vagy a hello a [fórum](http://go.microsoft.com/fwlink/?LinkID=403669&clcid=0x409).

[01]: ./media/machine-learning-excel-add-in-for-web-services/image1.png
[02]: ./media/machine-learning-excel-add-in-for-web-services/image2.png
[03]: ./media/machine-learning-excel-add-in-for-web-services/image3.png
[04]: ./media/machine-learning-excel-add-in-for-web-services/image4.png
[05]: ./media/machine-learning-excel-add-in-for-web-services/image5.png
[06]: ./media/machine-learning-excel-add-in-for-web-services/image6.png
