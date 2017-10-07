---
title: "a Stream Analytics és a gépi tanulás integrációs aaaAzure |} Microsoft Docs"
description: "Hogyan toouse egy felhasználó által definiált függvény és a gépi tanulás a Stream Analytics-feladatok"
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: cfced01f-ccaa-4bc6-81e2-c03d1470a7a2
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 07/06/2017
ms.author: jeffstok
ms.openlocfilehash: e1ba7ab51ece80719839793e1320a7666cfc4181
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="performing-sentiment-analysis-by-using-azure-stream-analytics-and-azure-machine-learning"></a>Azure Stream Analytics és az Azure Machine Learning segítségével véleményeket elemzések végrehajtását
Ez a cikk ismerteti, hogyan lehet a tooquickly egy egyszerű Azure Stream Analytics-feladat, amely az Azure Machine Learning beállítani. Hello Cortana Intelligence Gallery tooanalyze szöveg streamadatok a Machine Learning véleményeket analytics modellt használnak, és határozza meg a hello véleményeket pontszám valós időben. A Cortana Intelligence Suite hello használata lehetővé teszi ennek a feladatnak anélkül, hogy a céggel kapcsolatos véleményeket elemzési modell kialakításának menő hello bemutatása.

Ez a cikk tooscenarios ehhez hasonló helyzeteknek megismert alkalmazhatja:

* Valós idejű véleményeket folyamatos Twitter-adatok elemzése.
* A felhasználói rekordok elemzése csevegés a támogató személyzete számára.
* Megjegyzések a videók, fórumok és blogok kiértékelése. 
* Sok más valós idejű, a prediktív pontozási forgatókönyvek.

Egy valós forgatókönyv esetén közvetlenül a Twitter adatfolyam hello adatok visszajelzést kap. toosimplify hello oktatóanyagban azt már megírta azt, hogy hello Streaming Analytics-feladat beolvasása Twitter-üzeneteket az Azure Blob storage CSV-fájlból. Létrehozhat saját CSV-fájl, vagy egy CSV-mintafájlt, ahogy az a következő kép hello használhatja:

![a CSV-fájlban szereplő minta Twitter-üzenetek](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-2.png)  

az Ön által létrehozott hello Streaming Analytics-feladat lesz hello véleményeket elemzési modell egy felhasználói függvény (UDF) hello minta szöveges adatokon hello blob tárolóból. hello kimeneti (hello véleményeket elemzés eredménye hello) írása toohello ugyanarra a blob tároló egy másik CSV-fájlban. 

hello. a következő ábra azt mutatja be ezt a konfigurációt. Amint több reális forgatókönyvek esetében a blob storage lecserélheti Twitter-adatok az Azure Event Hubs bemeneti adatfolyam. Emellett sikerült készít egy [Microsoft Power BI](https://powerbi.microsoft.com/) valós idejű megjelenítésével kapcsolatos hello összesített céggel kapcsolatos véleményeket.    

![Stream Analytics a Machine Learning integrációjának áttekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-figure-1.png)  

## <a name="prerequisites"></a>Előfeltételek
Megkezdése előtt győződjön meg arról, hogy a következő hello:

* Aktív Azure-előfizetés.
* Néhány adatot a CSV-fájlból. Letöltheti a korábban bemutatott hello fájl [GitHub](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/sampleinput.csv), vagy létrehozhat saját fájlt. Ez a cikk feltételezzük, hogy a Githubról hello fájlt használ.

Magas szinten ebben a cikkben bemutatott toocomplete hello feladatok meg hello a következő:

1. Hozzon létre egy Azure storage-fiókot és egy blob storage tárolót, és töltse fel a CSV-formátumú bemeneti fájl toohello tároló.
3. Adja hozzá a céggel kapcsolatos véleményeket elemzési modell hello Cortana Intelligence Gallery tooyour Azure Machine Learning munkaterülettel, és ez a modell rendszerbe állítása a Machine Learning-munkaterület hello webszolgáltatásként.
5. Hozzon létre egy Stream Analytics-feladat, amely ennek a webszolgáltatásnak a rendelés toodetermine véleményeket függvényében hello szöveges bevitel hívásokat.
6. Hello Stream Analytics-feladat indítása és hello kimeneti ellenőrzése.

## <a name="create-a-storage-container-and-upload-hello-csv-input-file"></a>A tároló létrehozása és hello CSV bemeneti fájl feltöltése
Ebben a lépésben a CSV-fájl, például egy a Githubról elérhető hello is használhatja.

1. Hello Azure-portálon, kattintson **új** &gt; **tárolási** &gt; **tárfiók**.

   ![új tárfiók létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-create-storage-account.png)

2. Adjon meg egy nevet (`samldemo` hello példában). hello neve csak kisbetűket és számokat használhatja, és az Azure között egyedinek kell lennie. 

3. Adjon meg egy létező erőforráscsoportot, és adjon meg egy helyet. Helyét, javasoljuk, hogy az oktatóanyag használatban létrehozott összes hello erőforrások hello azonos helyen.

    ![Adja meg a tárfiókadatok](./media/stream-analytics-machine-learning-integration-tutorial/create-sa1.png)

4. Hello Azure-portálon válassza ki a tárfiók hello. Hello storage-fiók panelen kattintson **tárolók** majd  **+ &nbsp;tároló** toocreate blob Storage tárolóban.

    ![A blob-tároló létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-sa2.png)

5. Adjon meg egy nevet hello tároló (`azuresamldemoblob` hello példában), és ellenőrizze, hogy **hozzáférési típus** értéke túl**Blob**. Ha végzett, kattintson az **OK** gombra.

    ![Adja meg a blob-tároló adatait](./media/stream-analytics-machine-learning-integration-tutorial/create-sa3.png)

6. A hello **tárolók** panelen, jelölje be hello új tároló, tároló hello paneljének megnyitása, amelyen.

7. Kattintson a **Feltöltés** gombra.

    ![Egy tároló "Feltöltés" gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-sa-upload-button.png)

8. A hello **feltöltése a blob** panelen adja meg a hello CSV-fájl, amelyet az toouse ehhez az oktatóanyaghoz. A **Blob-típusú**, jelölje be **blokkblob** és set hello blokk too4 MB, amely elegendő-e ez az oktatóanyag méretezés.

    ![a blob-fájl feltöltése](./media/stream-analytics-machine-learning-integration-tutorial/create-sa4.png)

9. Kattintson a hello **feltöltése** hello hello panel alsó részén gombra.

## <a name="add-hello-sentiment-analytics-model-from-hello-cortana-intelligence-gallery"></a>A Cortana Intelligence Gallery hello hello véleményeket elemzési modell hozzáadása

Most, hogy hello mintaadatokat egy blobba, engedélyezheti a Cortana Intelligence Gallery hello véleményeket elemzési modellt.

1. Nyissa meg toohello [véleményeket prediktív elemzési modell](https://gallery.cortanaintelligence.com/Experiment/Predictive-Mini-Twitter-sentiment-analysis-Experiment-1) oldal a Cortana Intelligence Gallery hello.  

2. Kattintson a **Megnyitás a Studióban**.  
   
   ![A Stream Analytics Machine Learning, nyissa meg a Machine Learning Studio](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-open-ml-studio.png)  

3. Jelentkezzen be toogo toohello munkaterületen. Válasszon ki egy helyet.

4. Kattintson a **futtatása** hello lap hello alján. hello folyamat fut, amely egy percet vesz igénybe.

   ![Futtassa a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-run-experiment.png)  

5. Miután hello folyamat végrehajtása sikeresen befejeződött, válassza ki a **webes szolgáltatás telepítése** alján hello hello.

   ![egy webszolgáltatás telepítése a kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-deploy-web-service.png)  

6. amely a céggel kapcsolatos véleményeket elemzési modell kész toouse hello toovalidate kattintson hello **teszt** gombra. Adja meg például a "Tetszik Microsoft" bemeneti szöveget. 

   ![teszt kísérletben a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test.png)  

    Hello teszt működik, a következő példa egy eredmény hasonló toohello jelenik meg:

   ![a vizsgálati eredmények a Machine Learning Studióban](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-test-results.png)  

7. A hello **alkalmazások** oszlopban kattintson hello **Excel 2010 vagy korábbi munkafüzet** hivatkozás toodownload egy Excel-munkafüzet. hello munkafüzet hello egy API-kulcs és, hogy kell-e újabb tooset hello Stream Analytics-feladat mentése hello URL-címet tartalmazza.

    ![Stream Analytics-Machine Learning, gyors áttekintő](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-quick-glance.png)  


## <a name="create-a-stream-analytics-job-that-uses-hello-machine-learning-model"></a>A Stream Analytics-feladat által használt hello gépi tanulási modell létrehozása

Mostantól létrehozhat egy Stream Analytics-feladat hello minta Twitter-üzeneteket olvasó hello CSV-fájlból a blob Storage tárolóban. 

### <a name="create-hello-job"></a>Hello feladat létrehozása

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com).  

2. Kattintson a **új** > **az eszközök internetes hálózatát** > **Stream Analytics-feladat**. 

   ![Az Azure portál elérési tooa új Stream Analytics-feladat beolvasása](./media/stream-analytics-machine-learning-integration-tutorial/azure-portal-new-iot-sa-job.png)
   
3. Név hello feladat `azure-sa-ml-demo`, adja meg az előfizetés, adjon meg egy meglévő erőforráscsoportot, vagy hozzon létre egy újat és válasszon hello helyet hello feladat.

   ![új Stream Analytics-feladat beállításainak megadása](./media/stream-analytics-machine-learning-integration-tutorial/create-job-1.png)
   

### <a name="configure-hello-job-input"></a>Hello feladat bemeneti konfigurálása
hello feladat lekérdezi a bemeneti hello CSV-fájlból, hogy a korábbi tooblob tárolási feltöltve.

1. Hello feladat létrehozása után, a **feladat topológia** hello feladat panelen, kattintson a hello **bemenetek** mezőbe.  
   
   !["Bemenetek" Stream Analytics-feladat panelen párbeszédpanel](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input.png)  

2. A hello **bemenetek** panelen kattintson a **+ Hozzáadás**.

   !["Hozzáadás" bemeneti toohello Stream Analytics-feladat hozzáadására szolgáló gomb](./media/stream-analytics-machine-learning-integration-tutorial/create-job-add-input-button.png)  

3. Töltse ki a hello **új bemeneti** panel ezekkel az értékekkel:

    * **A bemeneti alias**: hello név használata `datainput`.
    * **Adatforrás típusa**: válasszon **adatfolyam**.
    * **Forrás**: válasszon **Blob-tároló**.
    * **Beállítás importálása**: válasszon **használja a jelenlegi előfizetés blob-tároló**. 
    * **A tárfiók**. Válassza ki a korábban létrehozott hello tárfiókot.
    * **Tároló**. A korábban létrehozott válassza hello tároló (`azuresamldemoblob`).
    * **Esemény szerializálási formátum**. Válassza ki **CSV**.

    ![Új feladat bemeneti beállításai](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-create-sa-input-new-portal.png)

4. Kattintson a **Create** (Létrehozás) gombra.

### <a name="configure-hello-job-output"></a>Hello feladatkiemenetét konfigurálása
hello feladat küld eredmények toohello azonos blob-tároló, amikor lekérdezi a bemeneti. 

1. A **feladat topológia** hello feladat panelen, kattintson a hello **kimenetek** mezőbe.  
  
   ![Új kimeneti Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-output.png)  

2. A hello **kimenetek** panelen kattintson a **+ Hozzáadás**, majd adja hozzá egy hello kimenet `datamloutput`. 

3. A **gyűjtése**, jelölje be **Blob-tároló**. Majd adja meg a többi hello hello kimeneti a beállításokat a hello ugyanazokat az értékeket, amelyet használt beviteli hello blob-tároló:

    * **A tárfiók**. Válassza ki a korábban létrehozott hello tárfiókot.
    * **Tároló**. A korábban létrehozott válassza hello tároló (`azuresamldemoblob`).
    * **Esemény szerializálási formátum**. Válassza ki **CSV**.

   ![Új feladat kimenet beállításai](./media/stream-analytics-machine-learning-integration-tutorial/create-output2.png) 

4. Kattintson a **Create** (Létrehozás) gombra.   


### <a name="add-hello-machine-learning-function"></a>Hello Machine Learning-függvény hozzáadása 
Korábban közzétett egy gépi tanulási modell tooa webszolgáltatás-bővítmény. A mi esetünkben hello adatfolyam állapotelemzési feladat futtatásakor küldené minden egyes minta tweetet webszolgáltatásból hello bemeneti toohello véleményeket elemzés céljából. Gépi tanulás webszolgáltatás hello adja vissza a céggel kapcsolatos véleményeket (`positive`, `neutral`, vagy `negative`) és egy pozitív hello tweetet valószínűségét. 

Hello oktatóanyag ezen részében adja meg egy hello adatfolyam állapotelemzési feladat függvényt. hello függvény meghívott toosend lehetnek egy tweetet toohello webszolgáltatás és hello válasz visszaszerzésében. 

1. Győződjön meg arról, hogy hello webes szolgáltatás URL-CÍMÉT és API-kulcsát korábban letöltött hello Excel-munkafüzet.

2. Visszatérési toohello feladat áttekintése panelen.

3. A **beállítások**, jelölje be **funkciók** majd **+ Hozzáadás**.

   ![Egy függvény toohello Stream Analytics-feladat hozzáadása](./media/stream-analytics-machine-learning-integration-tutorial/create-function1.png) 

4. Adja meg `sentiment` hello másként funkciót alias, és adja meg ezeket az értékeket használó hello panel hello többi:

    * **Típus működéséhez**: válasszon **Azure ML**.
    * **Beállítás importálása**: válasszon **egy másik előfizetésben található Importálás**. Ez lehetővé teszi egy alkalommal tooenter hello URL-cím és a kulcsot.
    * **URL-cím**: illessze be hello webes szolgáltatás URL-CÍMÉT.
    * **Kulcs**: hello API-kulcs a beillesztés.
  
    ![A Machine Learning-függvény toohello Stream Analytics-feladat hozzáadására szolgáló beállítások](./media/stream-analytics-machine-learning-integration-tutorial/add-function.png)  
    
5. Kattintson a **Create** (Létrehozás) gombra.

### <a name="create-a-query-tootransform-hello-data"></a>Hozzon létre egy tootransform hello adatait kérdezi le.

A Stream Analytics egy deklaratív, az SQL-alapú lekérdezés tooexamine hello bemeneti használ, és dolgozza fel. Ebben a szakaszban egy lekérdezést, amely a bemeneti olvassa be az egyes tweetet, majd meghívja az hello gépi tanulás funkció tooperform véleményeket elemzés hoz létre. hello lekérdezés ezután elküldi a hello eredmény toohello kimeneti (blob-tároló) definiálását.

1. Visszatérési toohello feladat áttekintése panelen.

2.  A **feladat topológia**, kattintson a hello **lekérdezés** mezőben.

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/create-query.png)  

3. Adja meg a következő lekérdezés hello:

    ```
    WITH sentiment AS (  
    SELECT text, sentiment(text) as result from datainput  
    )  

    Select text, result.[Score]  
    Into datamloutput
    From sentiment  
    ```    

    hello lekérdezés meghívja a korábban létrehozott hello függvényt (`sentiment`) sorrendben tooperform véleményeket elemzés a minden egyes tweetet hello bemeneti adatok. 

4. Kattintson a **mentése** toosave hello lekérdezés.


## <a name="start-hello-stream-analytics-job-and-check-hello-output"></a>Hello Stream Analytics-feladat indítása és hello kimeneti ellenőrzése

Most elindíthatja hello Stream Analytics-feladat.

### <a name="start-hello-job"></a>Hello feladat indítása
1. Visszatérési toohello feladat áttekintése panelen.

2. Kattintson a **Start** hello panel hello tetején.

    ![A lekérdezés Streaming Analytics-feladat létrehozása](./media/stream-analytics-machine-learning-integration-tutorial/start-job.png)  

3. A hello **indítási feladat**, jelölje be **egyéni**, majd válassza ki egy nap előzetes toowhen hello CSV fájltároló tooblob feltöltött. Amikor elkészült, kattintson a **Start**.  


### <a name="check-hello-output"></a>Hello kimeneti ellenőrzése
1. Néhány percig, amíg megjelenik a tevékenység hello futtatása lehetővé hello feladat **figyelés** mezőbe. 

2. Ha egy eszköz általában a blob storage tooexamine hello tartalmát használja, használja az adott eszköz tooexamine hello `azuresamldemoblob` tároló. Másik lehetőségként hello lépései hello Azure-portálon:

    1. Hello portálon található hello `samldemo` tárolási fiókot, és hello fiókon belül található hello `azuresamldemoblob` tároló. Két fájlt hello tárolóban látja: hello hello minta Twitter-üzeneteket tartalmazó fájlt, és hello Stream Analytics-feladat által létrehozott CSV-fájlból.
    2. Kattintson a jobb gombbal a létrehozott hello fájlt, és válassza ki **letöltése**. 

   ![A Blob-tároló CSV-feladat kimeneti letöltése](./media/stream-analytics-machine-learning-integration-tutorial/download-output-csv-file.png)  

3. Nyissa meg hello CSV-fájl jön létre. A következő példa hello hasonlót lásd:  
   
   ![Stream Analytics Machine Learning, CSV megtekintése](./media/stream-analytics-machine-learning-integration-tutorial/stream-analytics-machine-learning-integration-tutorial-csv-view.png)  


### <a name="view-metrics"></a>Nézet metrikák
Azure Machine Learning-függvény vonatkozó metrikáinak is megtekintheti. hello jelennek meg a következő függvény vonatkozó metrikáinak hello **figyelés** hello feladat panelen mezőben:

* **Kérelmek működéséhez** küldött kérelmek tooa Machine Learning webszolgáltatásba hello számát jelzi.  
* **Események működéséhez** hello kérelem események hello számát jelzi. Alapértelmezés szerint minden kérelem tooa Machine Learning webszolgáltatásba too1, 000 események másolatot tartalmazza.  


## <a name="next-steps"></a>Következő lépések

* [A Stream Analytics bemutatása tooAzure](stream-analytics-introduction.md)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Integrálható a REST API-t és a gépi tanulás](stream-analytics-how-to-configure-azure-machine-learning-endpoints-in-stream-analytics.md)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)



