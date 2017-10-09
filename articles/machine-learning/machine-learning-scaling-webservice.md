---
title: "aaaHow tooincrease egyidejűségi beállítása pedig az Azure Machine Learning webszolgáltatás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooincrease egyidejűségi, az Azure Machine Learning webszolgáltatás további végpontok hozzáadása."
services: machine-learning
documentationcenter: 
author: neerajkh
manager: srikants
editor: cgronlun
keywords: "az Azure gépi tanulási, web services operationalization, a méretezés, a végponthoz, egyidejűségi beállítása"
ms.assetid: c2c51d7f-fd2d-4f03-bc51-bf47e6969296
ms.service: machine-learning
ms.devlang: NA
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 01/23/2017
ms.author: neerajkh
ms.openlocfilehash: e2ad16ec766820a64f36c31232f6a33a79196af4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scaling-an-azure-machine-learning-web-service-by-adding-additional-endpoints"></a>Az Azure Machine Learning webszolgáltatás méretezhetővé további végpontok hozzáadása
> [!NOTE]
> Ez a témakör ismerteti a technikák alkalmazható tooa **klasszikus** Machine Learning webszolgáltatáshoz. 
> 
> 

Alapértelmezés szerint minden egyes közzétett webszolgáltatás konfigurált toosupport 20 egyidejű kérelmek és lehet magas, mint 200 egyidejű kérelemre. Hello a klasszikus Azure portálon tartalmaz egy módja tooset ezt az értéket, amíg Azure Machine Learning automatikusan optimalizálja hello beállítás tooprovide hello legjobb teljesítmény érdekében a webszolgáltatáshoz és hello portál értéket a rendszer figyelmen kívül hagyja. 

Ha azt tervezi, toocall hello API-t a nagyobb terheléshez, mint 200 egyidejű hívások maximális értéket fogja támogatni, hello kell hozzon létre több végpontot azonos webszolgáltatáshoz. Majd véletlenszerűen terjesztheti a terhelés összes őket.

a gyakori feladatok hello skálázás egy webszolgáltatás. Valamilyen okból tooscale toosupport több mint 200 egyidejű kérelemre, magasabb rendelkezésre állását több végpontot keresztül, vagy adjon meg külön végpontok hello webszolgáltatáshoz. Hello méretezési hello további végpontokat hozzáadásával növelhető keresztül azonos webszolgáltatás [a klasszikus Azure portálon](https://manage.windowsazure.com/) vagy hello [Azure Machine Learning webszolgáltatás](https://services.azureml.net/) portálon.

Új végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md).

Ne feledje, hogy a nagy feldolgozási száma használatával hátrányos, ha nem hívás hello API ennek megfelelően nagy sebességet lehet. Szórványos időtúllépések és/vagy a teljesítményt a hello késés Ha viszonylag kis terhelésű elhelyezése egy API-t nagy terhelés konfigurált jelenhet meg.

hello szinkron API-k általában hol van szükség egy alacsony késési igényű helyzetekben használják. Itt késés lévő hello idő hello API toocomplete egy kérelmek, és nem derül ki minden hálózati késésekből származik. Tegyük fel, hogy rendelkezik-e az API-k és egy 50-ms késleltetés. toofully felhasználhatják az hello rendelkezésre álló kapacitásból nagy adatátviteli szintű és az egyidejű hívások maximális számának = 20, ez az API 20 * 1000 kell toocall / 50 = 400 másodpercenként alkalommal fordult elő. Ez további kiterjesztése, egyidejű hívások maximális 200 lehetővé teszi, hogy Ön toocall hello API 4000 hányszor másodpercenként, feltéve, hogy egy 50-ms késleltetés.

<!--Image references-->
[1]: ./media/machine-learning-scaling-webservice/machlearn-1.png
[2]: ./media/machine-learning-scaling-webservice/machlearn-2.png
