---
title: "az Azure Stream Analytics & AzureML functions méretezése aaaJob |} Microsoft Docs"
description: "Ismerje meg, hogyan tooproperly méretezni a Stream Analytics-feladatok (particionálás, SU mennyiség és egyebek) használata az Azure Machine Learning funkciók."
keywords: 
documentationcenter: 
services: stream-analytics
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 47ce7c5e-1de1-41ca-9a26-b5ecce814743
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: 3fbdfaf7e8e86896c56f1d18bbde3a10bd3dca04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="scale-your-stream-analytics-job-with-azure-machine-learning-functions"></a>A Stream Analytics-feladat az Azure Machine Learning functions méretezése
Gyakran egyszerűen tooset fel egy Stream Analytics-feladat, és néhány adatot végigfuttatása. Mit kell végezzük amikor toorun hello azonos feladat magasabb adatkötettel fut? Hogyan tooconfigure hello Stream Analytics feladat, hogy azt lehessen méretezni toounderstand igényel velünk. Ebben a dokumentumban tárgyaljuk hello különleges szempontjait méretezési Stream Analytics a Machine Learning funkciók feladatok. Hogyan tooscale Stream Analytics-feladatok általában: hello cikk tájékoztatást [feladatok skálázás](stream-analytics-scale-jobs.md).

## <a name="what-is-an-azure-machine-learning-function-in-stream-analytics"></a>Mi az az Azure Machine Learning függvény a Stream Analytics?
A Stream Analytics a Machine Learning függvény használható például a Stream Analytics lekérdezési nyelv hello rendszeres függvényhívás. Mögött hello helyszín, azonban a hello függvényhívások nem ténylegesen az Azure Machine Learning webszolgáltatás-kérelmeket. Machine Learning web szolgáltatások támogatási "kötegelés" több sort minimális kötegelt nevezzük, a hello azonos a webes API-hívás, tooimprove teljes teljesítményt. Tekintse meg a következő cikkekben olvashat; hello [Azure Machine Learning funkciók a Stream Analytics](https://blogs.technet.microsoft.com/machinelearning/2015/12/10/azure-ml-now-available-as-a-function-in-azure-stream-analytics/) és [Azure Machine Learning webszolgáltatások](../machine-learning/machine-learning-consume-web-services.md).

## <a name="configure-a-stream-analytics-job-with-machine-learning-functions"></a>A Stream Analytics-feladat Machine Learning funkciók konfigurálása
A gépi tanulás funkció Stream Analytics-feladat konfigurálásakor nincsenek két paraméterek tooconsider, hello Machine Learning függvényhívások és adatfolyam-egységek (SUs) hello Stream Analytics-feladathoz kiosztott hello hello kötegmérete. toodetermine hello megfelelő értékeket ezeket, először egy kell döntést közötti késleltetés és a teljesítményt, ez azt jelenti, hogy hello Stream Analytics-feladat késését, és minden SU átviteli sebességgel. SUS-t mindig vehetők tooa feladat tooincrease átviteli jól particionált Stream Analytics-lekérdezés, noha a további SUs növeli a futó hello feladat hello költségét.

Ezért ez azért fontos toodetermine hello *tolerancia* késés a Stream Analytics-feladat futtatása. Az Azure Machine Learning szolgáltatáskérések futtatásával további késés természetes növeli a kötegelt méretet, amelyet a rendszer összetett hello Stream Analytics-feladat hello késését. Hello a köteg méretének növelését, ugyanakkor a Stream Analytics-feladat tooprocess hello lehetővé teszi, hogy * hello további események *ugyanannyi* a Machine Learning webszolgáltatás a szolgáltatáskéréseket. Gyakran hello Machine Learning web service késés növekedése a köteg méretének növekedése részterv lineáris toohello, ezért fontos tooconsider hello leginkább költséghatékony kötegméret bármely adott helyzetben a Machine Learning webszolgáltatáshoz. hello alapértelmezett kötegméret hello webszolgáltatás kérelmek 1000, előfordulhat, hogy módosítható hello használva [Stream Analytics REST API](https://msdn.microsoft.com/library/mt653706.aspx "Stream Analytics REST API") vagy hello [PowerShell ügyfél a Stream Analytics](stream-analytics-monitor-and-manage-jobs-use-powershell.md "Stream Analytics-ügyfél PowerShell").

A Köteg mérete meghatározása után hello mennyisége adatfolyam-egységek (SUs) meghatározható, hello száma alapján események, hogy a hello függvény másodpercenként tooprocess kell. További információ az adatfolyam-egységek: [Stream Analytics feladatok méretezése](stream-analytics-scale-jobs.md).

Általánosságban nincsenek 20 egyidejű kapcsolatok toohello Machine Learning webszolgáltatáshoz tartozó minden 6 SUS-t, azzal a különbséggel, hogy 1 SU feladatok és a 3 SU feladat kap 20 egyidejű kapcsolatok is.  Például ha hello bemeneti adatok sebessége 200 000 események másodpercenkénti és hello kötegméret marad a 1000 hello eredményül kapott web service késés 1000 események minimális kötegelt toohello alapértelmezett érték 200 MS-os. Ez azt jelenti, hogy minden kapcsolat tehet 5 kérelmek toohello Machine Learning webszolgáltatás egy másik. 20 kapcsolatok hello Stream Analytics-feladat feldolgozhatja a 20 000 a 200 MS-OS és ezért 100 000 események másodpercenként. Ezért tooprocess 200 000 események másodpercenkénti, hello Stream Analytics-feladatban kell 40 egyidejű kapcsolatok, too12 SUS-t, amely. az alábbi hello diagram azt ábrázolja, hello kérelmeinek hello Stream Analytics feladat toohello Machine Learning webszolgáltatás végpontja – 6 SUS-t minden maximális 20 egyidejű kapcsolatok tooMachine tanulás webszolgáltatás rendelkezik.

![A Stream Analytics méretezése a Machine Learning funkciók 2 feladat példa](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-00.png "méretezési Stream Analytics a Machine Learning funkciók 2 feladat példa")

Általában ***B*** a Köteg mérete ***L*** hello webes szolgáltatás várakozási kötegméret B ezredmásodpercben, a hello egy Stream Analytics-feladat az átviteli ***N*** SUs van:

![A Stream Analytics a Machine Learning funkciók képlet méretezni](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-02.png "Stream Analytics a Machine Learning funkciók képlet méretezni")

Lehet, hogy egy további figyelembe hello "maximális egyidejű hívások" hello Machine Learning web service ügyféloldali, javasoljuk tooset a toohello maximális értéket (jelenleg 200).

Ez a beállítás további tájékoztatást lásd hello [méretezés a cikk a Machine Learning webszolgáltatások](../machine-learning/machine-learning-scaling-webservice.md).

## <a name="example--sentiment-analysis"></a>Példa – véleményeket elemzés
hello alábbi példa tartalmazza a Stream Analytics-feladat hello véleményeket elemzés gépi tanulás funkció, a hello leírtak [Stream Analytics Machine Learning integrációs oktatóanyag](stream-analytics-machine-learning-integration-tutorial.md).

hello Ez egy egyszerű teljesen particionált lekérdezés hello követ **véleményeket** működik, a lent látható módon:

    WITH subquery AS (
        SELECT text, sentiment(text) as result from input
    )

    Select text, result.[Score]
    Into output
    From subquery

Vegye figyelembe a következő forgatókönyv; hello 10 000 Twitter-üzenetek / s átviteli sebességgel a Stream Analytics-feladatban kell létrehozni tooperform véleményeket elemzése hello Twitter-üzeneteket (esemény). 1 SU használ, a Stream Analytics-feladat lehet képes toohandle hello forgalom? Hello alapértelmezett kötegmérete 1000 hello feladat meg kell tudni tookeep mentése hello bevitellel. További hello hozzáadni a Machine Learning-függvény legfeljebb egy második, a késés, amely hello általános alapértelmezett késés hello véleményeket elemzési Machine Learning webszolgáltatásba (az alapértelmezett kötegméret 1000) kell létrehozni. hello Stream Analytics feladat **általános** vagy végpontok közötti késés állna néhány másodpercig. További részletes tekintse meg azokat a Stream Analytics-feladat *különösen* Machine Learning függvényhívások hello. Hello kötegméret 1000, amelynek, 10 000 események átviteli lépnek körülbelül 10 kérelmek tooweb szolgáltatás. Még a 1 SU esetén elegendő létesített egyidejű kapcsolatok tooaccommodate ebben a bemeneti forgalmat.

De mi történik, ha hello bemeneti események gyakorisága növekszik 100 x, és most hello Stream Analytics-feladatban kell tooprocess 1 000 000 Twitter-üzenetek / másodperc? Két lehetőség áll rendelkezésre:

1. Növelje a köteg méretének hello, vagy
2. Partíció hello bemeneti adatfolyam tooprocess hello események párhuzamosan

Hello hello első beállítást, a feladat **késés** növeli.

Hello második lehetőség több SUs ehhez kiépített toobe kell, és ezért a Machine Learning web service több egyidejű kérelmet létrehozni. Ez azt jelenti, hogy a hello feladat **költség** növeli.

Feltételezik hello késés hello véleményeket elemzési Machine Learning webszolgáltatásba 200 MS-os 1000-esemény kötegek, vagy az alábbi 250ms 5000-esemény kötegek, 10 000-esemény kötegek 300ms vagy 500ms 25 000-esemény kötegek.

1. Hello első lehetőséggel, (**nem** több SUs kiépítése), hello köteg mérete túl növekedhet**25 000**. Ez pedig lehetővé teszi hello feladat tooprocess 20 egyidejű kapcsolatok toohello Machine Learning webszolgáltatásba 1 000 000 eseményeket (hívás / 500ms késéssel). Ezért hello hello Stream Analytics-feladat toohello véleményeket függvény kérelmek elleni hello a webszolgáltatási kérelmeket kell emelni a Machine Learning miatt további késését **200 MS-os** túl**500ms**. Azonban ügyeljen arra, hogy a köteg méretének **nem** végtelenül növelhető, mivel hello Machine Learning webszolgáltatások hello terhelés méretének egy kérelem 4 MB vagy kisebb webes szolgáltatás kérelmek időtúllépési művelet 100 másodperc múlva.
2. Hello második beállításának használata esetén hello kötegméret marad, 1000, és 200 MS-os web service késésű, minden 20 egyidejű kapcsolatok toohello webszolgáltatás lenne képes tooprocess 1000 * 20 * 5 események másodpercenkénti 100 000 =. Ezért egy második, hello feladat tooprocess 1 000 000 események 60 SUs lenne szükség. Összehasonlított toohello első lehetőség, a Stream Analytics-feladat lenne több webalkalmazás-szolgáltatás kötegelt kérelmekben, viszont egy magasabb költsége előállítása érdekében.

Az alábbiakban a táblát, hello átviteli sebességgel hello Stream Analytics-feladat van különböző SUS-t és a Köteg mérete (az események másodpercenkénti számát).

| Köteg mérete (a gépi tanulás összetevő várakozási ideje) | 500 (200 MS-OS) | 1000 (200 MS-OS) | 5000 (250ms) | 10 000-re (300ms) | 25 000 (500ms) |
| --- | --- | --- | --- | --- | --- |
| **1 SU** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **3 SUS-t** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **6 SUS-t** |2,500 |5,000 |20,000 |30,000 |50,000 |
| **12 SUS-t** |5,000 |10,000 |40,000 |60,000 |100,000 |
| **18 SUS-t** |7,500 |15,000 |60,000 |90,000 |150,000 |
| **24 SUS-t** |10,000 |20,000 |80,000 |120,000 |200,000 |
| **…** |… |… |… |… |… |
| **60 SUS-t** |25,000 |50,000 |200,000 |300,000 |500,000 |

Mostanra akkor már rendelkezik Stream Analytics a Machine Learning funkciók működése beható ismerete. Valószínűleg is tudomásul veszi, hogy az adatforrások és az egyes "pull" "pull" adatait adja vissza egy kötegelt események Stream Analytics-feladatok hello Stream Analytics-feladat tooprocess. Milyen hatással van a ez lekéréses modell hello Machine Learning webszolgáltatás-kérelmek?

A Machine Learning funkciók hivatott hello kötegméret általában pontosan nem osztható hello minden Stream Analytics-feladat "pull" által visszaadott események száma. Ha ez történik, hello Machine Learning webszolgáltatáshoz "részleges" kötegek hívja. Ebben az esetben toonot fel Önnek további feldolgozás várakozási lekéréses toopull összevonási eseményeit a terhelés.

## <a name="new-function-related-monitoring-metrics"></a>Új, a függvény kapcsolatos figyelési metrikákat
Hello figyelő területét egy Stream Analytics-feladat három további függvény kapcsolatos metrikákat lettek hozzáadva. Ahogy az alábbi ábra hello FÜGGVÉNY, FÜGGVÉNY események és sikertelen FÜGGVÉNY KÉRELMEKET.

![A Stream Analytics a Machine Learning funkciók metrikák méretezése](./media/stream-analytics-scale-with-ml-functions/stream-analytics-scale-with-ml-functions-01.png "Stream Analytics a Machine Learning funkciók metrikák méretezése")

hello a következők:

**FÜGGVÉNY kérelmek**: hello függvény kérelmek számát jelenti.

**FÜGGVÉNY események**: hello számú események hello kérelmek működik.

**SIKERTELEN FÜGGVÉNY kérelmek**: hello sikertelen függvény kérelmek számát jelenti.

## <a name="key-takeaways"></a>Kulcs Takeaways
toosummarize hello főbb pontjai, a rendelés tooscale egy Stream Analytics-feladat a Machine Learning funkciók hello következő elemek figyelembe kell venni:

1. hello bemeneti események száma
2. hello megengedett Stream Analytics-feladat futtatása hello késése (és így hello kötegméret hello Machine Learning webszolgáltatás kérelmek)
3. hello kiépítve a Stream Analytics SUS-t és a gépi tanulás webszolgáltatás-kérelmek (hello további függvény kapcsolatos költségek) hello száma

Egy teljesen particionált Stream Analytics lekérdezési példaként használt. Egy összetettebb lekérdezés van szüksége a hello [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics) kiváló forrást biztosítanak a további segítség hello Stream Analytics csapat van.

## <a name="next-steps"></a>Következő lépések
toolearn Stream Analytics kapcsolatos további információkért lásd:

* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)
