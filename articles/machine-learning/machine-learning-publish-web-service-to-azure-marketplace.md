---
title: "aaa(deprecated) közzététel machine learning web service tooAzure piactér |} Microsoft Docs"
description: "(elavult) Hogyan toopublish az Azure Machine Learning webszolgáltatás toohello Azure piactéren"
services: machine-learning
documentationcenter: 
author: BharathS
manager: jhubbard
editor: cgronlun
ms.assetid: 68e908be-3a99-4cd7-9517-e2b5f2f341b8
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: bharaths
ROBOTS: NOINDEX
redirect_url: machine-learning-gallery-experiments
redirect_document_id: True
ms.openlocfilehash: 149abc3df9b79c1b37d233d5e85e803592ff1020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deprecated-publish-azure-machine-learning-web-service-toohello-azure-marketplace"></a>(elavult) Azure Machine Learning webszolgáltatás toohello Azure piactér közzététele

> [!NOTE]
> DataMarket és Data Services rendszer hatókörről, és a meglévő előfizetések rendszerből, és elindítása megszakítva 3/31/2017. Ennek köszönhetően ez a cikk is elavult. 
> 
> Másik megoldásként, közzéteheti a Machine Learning kísérleteket toohello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/) hello adatok tudományos Közösség hello juttatásra. További információkért lásd: [megosztás és a Cortana Intelligence Gallery hello erőforrások felderítéséhez](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).

hello Azure piactér hello alábbi képességekkel rendelkezik toopublish Azure Machine Learning webszolgáltatások fizet, vagy a külső felhasználók által megjeleníthető szolgáltatások szabad. Ez a cikk áttekintést hivatkozások tooguidelines tooget használatba az adott folyamatot. Ez a folyamat használatával biztosíthatja a webszolgáltatások más fejlesztők tooconsume érhető el az alkalmazásokban.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="overview-of-hello-publishing-process"></a>Hello közzétételi folyamat áttekintése
Az alábbiakban hello hello lépései az Azure Machine Learning web service tooAzure piactér közzététele:

1. Hozzon létre, és tegye közzé a Machine Learning kérés-válasz szolgáltatás (RR-EKET)
2. Hello szolgáltatás tooproduction telepíteni, és szerezze be a hello API-kulcs és OData-végpont adatai.
3. Túl a közzétett webes szolgáltatás toopublish hello használata hello URL-[Azure piactér (adatok piaci)](https://publish.windowsazure.com/workspace/) 
4. Ha küldött, az ajánlatot áttekintette és kell toobe, mielőtt az ügyfelek jóváhagyott elindíthatja az beszerzése. hello közzétételi folyamat eltarthat néhány nap. 

## <a name="walk-through"></a>Útmutató
### <a name="step-1-create-and-publish-a-machine-learning-request-response-service-rrs"></a>1. lépés: Létrehozása és közzététele a Machine Learning kérés-válasz szolgáltatás (RR-EKET)
 Ha ezt nem ez már, kérjük, szánjon egy pillantást, ez [bízná](machine-learning-walkthrough-5-publish-web-service.md).

### <a name="step-2-deploy-hello-service-tooproduction-and-obtain-hello-api-key-and-odata-endpoint-information"></a>2. lépés: Hello szolgáltatás tooproduction telepíteni, és szerezze be a hello API-kulcs és OData-végpont adatai
1. A hello [klasszikus Azure portál](http://manage.windowsazure.com), jelölje be hello **MACHINE LEARNING** hello bal oldali navigációs sávon a lehetőséget, majd válassza ki a munkaterületen. 
2. Kattintson a hello **WEBSZOLGÁLTATÁSOK** fülre, és azt szeretné, hogy toopublish toohello piactér válassza hello webszolgáltatás.
   
    ![Azure Piactér][workspace]
3. Válassza ki kellene például toohave hello piactér felhasznált hello végpontot. Ha nem hozott létre további végpontok, kiválaszthatja a hello **alapértelmezett** végpont.
4. Való kattintást követően hello végponton, fogja tudni toosee hello **API-kulcs**. Szüksége lesz az adott adatok később a 3. lépésben, ezért győződjön egy példányát.
   
    ![Azure Piactér][apikey]
5. Kattintson a hello **kérelem/válasz** metódus ezen a ponton nem támogatott közzétételi kötegelt végrehajtási szolgáltatás toohello piactéren. Amely a billentyűparanccsal léphet toohello API súgólapján hello kérelem/válasz metódust.
6. Másolás hello **OData-végpont címe**, akkor ezt az információt később a 3. lépés.
   
    ![Azure Piactér][odata]

hello szolgáltatás telepítése éles környezetben.

### <a name="step-3-use-hello-url-of-hello-published-web-service-toopublish-tooazure-marketplace-datamarket"></a>3. lépés: A közzétett webes szolgáltatás toopublish tooAzure piactér (DataMarket) hello használata hello URL-
1. Keresse meg a túl[Azure piactér (adatok piaci)](http://datamarket.azure.com/home) 
2. Kattintson a hello **közzététel** hivatkozás hello oldal hello tetején. A rendszer ekkor toohello [Microsoft Azure közzétételi Portáljára](https://publish.windowsazure.com)
3. Kattintson a hello **közzétevők** szakasz tooregister közzétevőként.
4. Egy új ajánlat létrehozásakor válassza ki a **adatszolgáltatások**, majd kattintson a **hozzon létre egy új szolgáltatás**. 
   
   ![Azure Piactér][image1]
   
   <br />
5. A **tervek** információt nyújtanak az ajánlat, beleértve a tarifacsomagot. Döntse el, ha egy ingyenes és fizetős szolgáltatás tartalmazni fogja. a fizetős, tooget fizetési információk, például a bank és az adót információk adja meg.
6. A **Marketing** ismertetik az ajánlatot, például a hello címet és a leírást az előfizetéshez.
7. A **árazás** hello árlista beállítása az adott országok terveket, illetve engedélyezheti, hogy a "autoprice" hello rendszer ajánlatát.
8. A hello **adatszolgáltatás** lapra, majd **webszolgáltatás** , hello **adatforrás**.
   
    ![Azure Piactér][image2]
9. Hello webes szolgáltatás URL-CÍMÉT és API-kulcsát beszerzése a klasszikus Azure portálon hello a fenti 2. lépésben leírtak szerint.
10. Hello piactér adatszolgáltatás beállítása párbeszédpanelen illessze be az OData-végpont címe hello hello **szolgáltatás URL-címe** szövegmezőben.
11. A **hitelesítési**, válassza a **fejléc** , hello **hitelesítési séma**.
    
    * Adja meg "Engedélyezés" hello **fejlécnév**.
    * A hello **Fejlécérték**, írja be a "Tulajdonos" (hello idézőjelek nélkül), kattintson a hello **terület** sáv megnyitásához, és illessze be hello API-kulcs.
    * Jelölje be hello **a szolgáltatás nem OData** jelölőnégyzetet.
    * Kattintson a **kapcsolat tesztelése** tootest hello kapcsolat.
12. A **kategóriák**, győződjön meg arról **Machine Learning** van kiválasztva.
13. Ha befejezte az összes beírása hello metaadatok kapcsolatos ajánlatát, kattintson a **közzététel**, majd **tooStaging leküldéses**. Ezen a ponton, értesítést kap a fennmaradó probléma merül fel, hogy kell-e toofix.
14. Miután meggyőződött a létrehozása után minden hello fennálló problémákat, kattintson a **toopush tooProduction jóváhagyási kérelmek**. hello közzétételi folyamat eltarthat néhány nap. 

[image1]:./media/machine-learning-publish-web-service-to-azure-marketplace/image1.png
[image2]:./media/machine-learning-publish-web-service-to-azure-marketplace/image2.png
[workspace]:./media/machine-learning-publish-web-service-to-azure-marketplace/selectworkspace.png
[apikey]:./media/machine-learning-publish-web-service-to-azure-marketplace/apikey.png
[odata]:./media/machine-learning-publish-web-service-to-azure-marketplace/odata.png

