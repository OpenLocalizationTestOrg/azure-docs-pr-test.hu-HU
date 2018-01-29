---
title: "Hivatkozási adatokat és a keresési táblák használja a Stream Analytics |} Microsoft Docs"
description: "Referenciaadatok használja a Stream Analytics-lekérdezés"
keywords: "keresési tábla, referenciaadatok"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: samacha
ms.openlocfilehash: 438ec565f3c6e06ab7ec92cf1bbfbdde88f99b6d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/11/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>A Stream Analytics bemeneti adatfolyam hivatkozási adatokat vagy keresési táblák használata
Referenciaadatok (más néven keresési tábla), amely statikus véges adatkészlet vagy lelassulnak, ami módosítása ideiglenesek, keresés végrehajtásához, vagy az adatfolyam függ. Annak hivatkozás adatok az Azure Stream Analytics-feladat, általában fogja használni a [hivatkozás adatok csatlakozás](https://msdn.microsoft.com/library/azure/dn949258.aspx) a lekérdezésben. A Stream Analytics használ a tárolási réteg a Referenciaadatoknál Azure Blob Storage tárolót, és az Azure Data Factory hivatkozás adatokat át legyenek-e vagy átmásolva az Azure Blob storage referenciaadatok, használni [tetszőleges számú felhőalapú és a helyszíni adattárolókhoz](../data-factory/copy-activity-overview.md). Referenciaadatok növekvő sorrendben az a dátum/idő, a blob neve (a bemeneti konfigurációs meghatározott) blobok sorozataként van modellezve. Az **csak** támogatja a sorozat végére hozzáadása egy dátum/idő használatával **nagyobb** a sorban utolsó blob a megadottól.

A Stream Analytics rendelkezik egy **100 MB-os felső határ az egyes blob** feladatok használatával tud feldolgozni több hivatkozás blobokat, de a **elérési út mintája** tulajdonság.


## <a name="configuring-reference-data"></a>Referenciaadatok konfigurálása
A referenciaadatok konfigurálásához először szeretne létrehozni, amely típusú bemeneti **referenciaadatok**. Az alábbi táblázat azt ismerteti, hogy minden egyes tulajdonsága, amely meg kell adnia a leírás a referenciaadatok bemeneti létrehozása során:


<table>
<tbody>
<tr>
<td>Tulajdonság neve</td>
<td>Leírás</td>
</tr>
<tr>
<td>A bemeneti Alias</td>
<td>Egy rövid nevet használt a feladat lekérdezésben a bemeneti hivatkozást.</td>
</tr>
<tr>
<td>Tárfiók</td>
<td>Hol találhatók a BLOB storage-fiók neve. Ha ugyanazt az előfizetést, a Stream Analytics-feladat, válassza ki azt a a legördülő listán.</td>
</tr>
<tr>
<td>Tárfiók kulcsa</td>
<td>A storage-fiókjához tartozó titkos kulcsot. Ez automatikusan lekérdezi megadni, ha a tárfiók ugyanahhoz az előfizetéshez, mint a Stream Analytics-feladat van.</td>
</tr>
<tr>
<td>A tároló</td>
<td>Tárolók adja meg a Microsoft Azure Blob szolgáltatásban tárolt blobok logikai csoportosítását. Amikor egy blob feltöltése a Blob szolgáltatás, meg kell adnia, hogy a blob tárolója.</td>
</tr>
<tr>
<td>Elérési út mintája</td>
<td>A megadott tárolóban található blobok helyének azonosításához használt elérési utat. Az elérési útban kiválaszthatja a következő 2 változó egy vagy több példányát adhatja meg:<BR>a {date}, {time}<BR>1. példa: products/{date}/{time}/product-list.csv<BR>2. példa: products/{date}/product-list.csv
</tr>
<tr>
<td>[Választható] dátumformátum</td>
<td>Ha a megadott elérési út mintája belül használt {date}, majd választhat, amelyben a blobok vannak rendszerezve dátumformátum a támogatott formátumok legördülő.<BR>Példa: Éééé/hh/nn-éééé/hh/nn, stb.</td>
</tr>
<tr>
<td>[Választható] idő formátuma</td>
<td>Ha a megadott elérési út mintája belül használt {time}, majd választhat az időformátum, amelyben a blobok vannak rendszerezve a támogatott formátumok legördülő.<BR>Példa: HH, ÓÓ/pp vagy HH: mm-es</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>Győződjön meg arról, a lekérdezések használhatók a megfelelően, Stream Analyticsnek tudnia kell, melyik szerializációs formátumot használ, a bejövő adatfolyamokhoz. A Referenciaadatoknál, a támogatott formátumok a következők CSV és JSON-NÁ.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Az UTF-8 jelenleg az egyetlen támogatott kódolási formátum</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Referenciaadatok ütemezés létrehozása
A lassan változó adatkészletet a referenciaadatok esetén majd adatok engedélyezve van a bemeneti konfigurációját a {date} segítségével egy elérési út mintája megadásával hivatkozás frissítése és támogatása {time} helyettesítés jogkivonatokat. A Stream Analytics szerzi be a frissített adatokat definíciókat az elérési út mintája alapján. Például egy mintát `sample/{date}/{time}/products.csv` a dátumformátummal **"Éééé-hh-nn"** és idő formátuma **"HH-mm"** arra utasítja a Stream Analytics a frissített blob átvételéhez `sample/2015-04-16/17-30/products.csv` 5:30-kor. április 16: , 2015 UTC időzóna.

> [!NOTE]
> Jelenleg Stream Analytics-feladatok keresse meg a blob frissítés csak akkor, ha a számítógép ideje kiadásokban a blob nevének kódolású idejével. Például a feladat keresi `sample/2015-04-16/17-30/products.csv` , amint lehetséges, de nincs korábbi mint 5:30-kor 2015. áprilisi 16 UTC idő zóna. A következőket hajtja végre *soha nem* keresse meg a korábban felderített legutóbb kódolt idővel blob.
> 
> Például Ha a feladat megtalálja a blob `sample/2015-04-16/17-30/products.csv` , figyelmen kívül hagyja az kódolt dátum korábbi, mint 5:30 PM 2015. áprilisi 16 fájlokat, ha a késői érkező `sample/2015-04-16/17-25/products.csv` blob jön létre ugyanabban a tárolóban a feladat nem fogja használni az.
> 
> Hasonlóképpen ha `sample/2015-04-16/17-30/products.csv` du. 10:03 2015. áprilisi 16 csak keletkezik, de nincs korábbi dátummal blob megtalálható-e a tároló, a feladat a kezdő pozíció: 10:03 PM 2015. áprilisi 16 fájlt használja, és előző hivatkozási adatokat arra használni, amíg ez nem sikerül.
> 
> Kivétel ez alól, ha a feladat újbóli feldolgozása adatokat időben, vagy ha a feladat első elindult. A feladat kezdési időpontban a hozott létre, mielőtt a feladat kezdési időpont van megadva a legutóbbi blob keresi. Erre azért van szükség, hogy van egy **nem üres** adatkészlet hivatkozik, a feladat indításakor. Ha az egyik nem található, a feladat megjelenítése a következő diagnosztikai: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Az Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) levezényelni a Stream Analytics hivatkozás adatok definíciók frissítéséhez szükséges frissített blobok létrehozásához használható. A Data Factory egy felhőalapú adatintegrációs szolgáltatás, amellyel előkészíthető és automatizálható az adatok továbbítása és átalakítása. Támogatja az adat-előállító [csatlakozik nagyszámú felhő alapú, és a helyszíni adattárolókhoz](../data-factory/copy-activity-overview.md) és egy rendszeres ütemterv alapján a könnyen megköveteli az adatok. További információk és részletes útmutatás a referenciaadatok létrehozására az a Stream Analytics, amelyek egy előre definiált ütemezés szerint frissíti a Data Factory-folyamathoz beállításáról, tekintse meg a [GitHub minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tippek az hivatkozás adatok frissítése
1. Felülírja a hivatkozás a BLOB-adatobjektumok nem akadályozza meg a Stream Analytics töltse be újra a blob, és egyes esetekben okozhat a feladat sikertelen lesz. Az ajánlott módszer a referenciaadatok módosítása, hogy a feladat bemeneti definiált azonos tárolóhoz és annak elérési útja minta használatával új blob hozzáadhat és használhat egy dátum/idő **nagyobb** a sorban utolsó blob a megadottól.
2. A BLOB-adatobjektumok hivatkozás vannak **nem** a blob "Utolsó módosítás" idő szerint, de csak a blob megadott dátumát és időpontját segítségével a {date} és {time} helyettesítések szempontjából.
3. Néhány alkalommal egy feladatot kell visszamehetnek az időben, ezért hivatkozás a BLOB-adatobjektumok nem kell módosítani vagy törölni.

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Bemutattuk Önnek a Stream Analytics felügyelt szolgáltatást, amely streamelő elemzéseket biztosít az eszközök internetes hálózatáról (IoT) származó adatokon. További információk a szolgáltatásról:

* [Get started using Azure Stream Analytics](stream-analytics-real-time-fraud-detection.md) (Bevezetés az Azure Stream Analytics használatába)
* [Scale Azure Stream Analytics jobs](stream-analytics-scale-jobs.md) (Azure Stream Analytics-feladatok méretezése)
* [Azure Stream Analytics Query Language Reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) (Referencia az Azure Stream Analytics lekérdezési nyelvhez)
* [Az Azure Stream Analytics felügyeleti REST API referenciája](https://msdn.microsoft.com/library/azure/dn835031.aspx)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.get.started]: stream-analytics-get-started.md
[stream.analytics.query.language.reference]: http://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: http://go.microsoft.com/fwlink/?LinkId=517301
