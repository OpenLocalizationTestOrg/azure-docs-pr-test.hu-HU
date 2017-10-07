---
title: "aaaUse hivatkozási adatokat és a keresési a Stream Analytics táblák |} Microsoft Docs"
description: "Referenciaadatok használja a Stream Analytics-lekérdezés"
keywords: "keresési tábla, referenciaadatok"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 06103be5-553a-4da1-8a8d-3be9ca2aff54
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 03/28/2017
ms.author: jeffstok
ms.openlocfilehash: fb1d18fba920db5e097d0c95d333e8e8390d1589
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-reference-data-or-lookup-tables-in-a-stream-analytics-input-stream"></a>A Stream Analytics bemeneti adatfolyam hivatkozási adatokat vagy keresési táblák használata
Referenciaadatok (más néven keresési tábla) olyan véges adat, amely statikus vagy lelassulnak, ami módosítása ideiglenesek, használni tooperform egy lookup vagy toocorrelate az adatfolyamban. toomake használatának az Azure Stream Analytics-feladat, általában segítségével egy [hivatkozás adatok csatlakozás](https://msdn.microsoft.com/library/azure/dn949258.aspx) a lekérdezésben. Stream Analytics használ az Azure Blob Storage tárolót a referenciaadatok hello tárolási rétege, és az Azure Data Factory hivatkozás lehet az adatokat átalakított és/vagy másolt tooAzure Blob-tároló, hivatkozás adatként használható a [tetszőleges számú felhőalapú és a helyszíni adattárolókhoz](../data-factory/data-factory-data-movement-activities.md). A referenciaadatok BLOB (hello bemeneti konfigurációs meghatározott) hello dátum/idő hello blob neve megadott sorrendben sorozatát van modellezve. Az **csak** hello feladatütemezési toohello végét hozzáadása egy dátum/idő használatával támogatja **nagyobb** mint hello meghatározott hello utolsó blob hello sorrendben.

A Stream Analytics rendelkezik egy **100 MB-os felső határ az egyes blob** , de a feladatok is feldolgozhat több hivatkozás blobok hello segítségével **elérési út mintája** tulajdonság.


## <a name="configuring-reference-data"></a>Referenciaadatok konfigurálása
tooconfigure a referenciaadatok először van szüksége, amely típusú bemeneti toocreate **referenciaadatok**. hello az alábbi táblázat azt ismerteti, hogy mindegyik tulajdonsághoz, akkor kell tooprovide hello referenciaadat-bemenetek létrehozásakor, a Leírás:


<table>
<tbody>
<tr>
<td>Tulajdonság neve</td>
<td>Leírás</td>
</tr>
<tr>
<td>A bemeneti Alias</td>
<td>Egy rövid nevet a hello feladat lekérdezés tooreference ebben a bemeneti használt.</td>
</tr>
<tr>
<td>Tárfiók</td>
<td>hol találhatók a BLOB storage-fiók hello hello neve. Amennyiben az hello a Stream Analytics-feladat tárolóként ugyanazt az előfizetést, kiválaszthatja a hello legördülő.</td>
</tr>
<tr>
<td>Tárfiók kulcsa</td>
<td>hello tartozó titkos kulcsot hello tárfiók. Ez lekérdezi automatikusan kitölti a rendszer a hello hello storage-fiók esetén a Stream Analytics-feladat tárolóként ugyanazt az előfizetést.</td>
</tr>
<tr>
<td>A tároló</td>
<td>Tárolók adja meg a Microsoft Azure Blob szolgáltatás hello tárolt blobok logikai csoportosítása. A blob toohello Blob szolgáltatás feltöltésekor meg kell adnia, hogy a blob tárolója.</td>
</tr>
<tr>
<td>Elérési út mintája</td>
<td>hello elérési utat használja toolocate hello megadott tárolóban található blobok. Hello elérési útban úgy is dönthet, toospecify hello a következő 2 változó egy vagy több példányát:<BR>a {date}, {time}<BR>1. példa: products/{date}/{time}/product-list.csv<BR>2. példa: products/{date}/product-list.csv
</tr>
<tr>
<td>[Választható] dátumformátum</td>
<td>Ha a megadott elérési út mintája hello belül használt {date}, majd választhat, amelyben a blobok vannak rendszerezve hello dátumformátum hello legördülő lista a támogatott formátumok.<BR>Példa: Éééé/hh/nn-éééé/hh/nn, stb.</td>
</tr>
<tr>
<td>[Választható] idő formátuma</td>
<td>Ha a megadott elérési út mintája hello belül használt {time}, majd választhat, amelyben a blobok vannak rendszerezve hello időformátum hello legördülő lista a támogatott formátumok.<BR>Példa: HH, ÓÓ/pp vagy HH: mm-es</td>
</tr>
<tr>
<td>Esemény szerializálási formátum</td>
<td>a lekérdezések működni hello megfelelően, a Stream Analytics toomake kell melyik szerializációs formátumot használja a bejövő adatfolyamokhoz tooknow. A Referenciaadatoknál, hello támogatott formátumok a következők CSV és JSON-NÁ.</td>
</tr>
<tr>
<td>Encoding</td>
<td>Az UTF-8 hello csak akkor támogatja a kódolási formátum jelenleg</td>
</tr>
</tbody>
</table>

## <a name="generating-reference-data-on-a-schedule"></a>Referenciaadatok ütemezés létrehozása
Esetén a referenciaadatok a lassan változó adatkészletet hivatkozás Adatfrissítés támogatása engedélyezve van egy elérési út mintája megadásával hello bemeneti konfigurációban hello {date} és {time} helyettesítés jogkivonatok használatával. A Stream Analytics szerzi be az elérési út mintája alapján frissítése hello hivatkozás adatmeghatározások. Például egy mintát `sample/{date}/{time}/products.csv` a dátumformátummal **"Éééé-hh-nn"** és idő formátuma **"HH-mm"** arra utasítja a Stream Analytics toopick fel a frissített hello blob `sample/2015-04-16/17-30/products.csv` , 5:30-kor. április 16, 2015 UTC időzóna.

> [!NOTE]
> Jelenleg Stream Analytics-feladatok keressen hello blob frissítése csak akkor, ha hello gépek idejét kiadásokban hello blob neve kódolású toohello idő. Például hello feladat keresi `sample/2015-04-16/17-30/products.csv` , amint lehetséges, de nincs korábbi mint 5:30-kor 2015. áprilisi 16 UTC idő zóna. A következőket hajtja végre *soha nem* keresi a korábbi, mint a legutóbb felderített hello kódolt idővel blob.
> 
> Például Miután hello feladat megkeresi hello blob `sample/2015-04-16/17-30/products.csv` , figyelmen kívül hagyja az kódolt dátum korábbi, mint 5:30 PM 2015. áprilisi 16 fájlokat, ha a késői érkező `sample/2015-04-16/17-25/products.csv` blob jön létre a hello azonos tároló hello feladat nem fogja használni az.
> 
> Hasonlóképpen ha `sample/2015-04-16/17-30/products.csv` du. 10:03 2015. áprilisi 16 csak keletkezik, de nincs korábbi dátummal blob megtalálható-e hello tároló, hello feladat kezdő pozíció: 10:03 PM 2015. áprilisi 16 ezt a fájlt használja, és addig hello előző referenciaadatok használja.
> 
> Egy kivétel toothis, de ha hello feladat toore adatfeldolgozásra időben hello feladat első indításakor. A kezdési idő hello feladat keres hello legutóbbi blob előállított előtt hello feladat kezdési időpont van megadva. Ez történik, hogy tooensure egy **nem üres** hivatkozik adatkészlet hello feladat indításakor. Ha az egyik nem található, hello feladat jeleníti meg a következő diagnosztikai hello: `Initializing input without a valid reference data blob for UTC time <start time>`.
> 
> 

[Az Azure Data Factory](https://azure.microsoft.com/documentation/services/data-factory/) használt tooorchestrate hello feladat létrehozása a Stream Analytics tooupdate hivatkozás adatmeghatározások szükséges frissített hello blobok lehet. Adat-előállító koordinálja és automatizálja hello mozgás és adatok átalakítása felhőalapú adatok integrációs szolgáltatás. Támogatja az adat-előállító [csatlakozás felhőalapú és helyszíni adattárolókhoz nagy számú tooa](../data-factory/data-factory-data-movement-activities.md) és egy rendszeres ütemterv alapján a könnyen megköveteli az adatok. További információt és lépésenkénti útmutatást hogyan tooset mentése a Data Factory-feldolgozási folyamat toogenerate referenciaadatok a Stream Analytics, amelyek egy előre definiált ütemezés szerint frissíti, tekintse meg a [GitHub minta](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/ReferenceDataRefreshForASAJobs).

## <a name="tips-on-refreshing-your-reference-data"></a>Tippek az hivatkozás adatok frissítése
1. Felülírja a hivatkozás a BLOB-adatobjektumok nem akadályozza meg a Stream Analytics tooreload hello blob, és egyes esetekben hello feladat toofail okozhat. hello ajánlott módja toochange referenciaadatok tooadd azonos tároló- és elérési út mintája definiált hello feladat bemeneti hello segítségével új blob és használni egy dátum/idő **nagyobb** mint hello meghatározott hello utolsó blob hello sorrendben.
2. A BLOB-adatobjektumok hivatkozás vannak **nem** rendezett hello blob "Utolsó módosítás" idő, hanem csak hello időpontja és dátuma a hello blob neve hello {date} és {time} helyettesítések továbbá használatával.
3. Néhány alkalommal egy feladatot kell visszamehetnek az időben, ezért hivatkozás a BLOB-adatobjektumok nem kell módosítani vagy törölni.

## <a name="get-help"></a>Segítségkérés
További támogatásért keresse fel az [Azure Stream Analytics-fórumot](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Következő lépések
Már bevezetett tooStream Analytics, a felügyelt szolgáltatás streaming analytics hello az eszközök internetes hálózatát származó adatokon. További információ a szolgáltatás toolearn lásd:

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
