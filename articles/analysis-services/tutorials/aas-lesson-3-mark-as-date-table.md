---
cím: aaa "Azure Analysis Services-oktatóanyag lecke 3: be van jelölve dátumtáblázatként |} Microsoft Docs"Leírás: ismerteti, hogyan toomark dátum tábla hello Azure Analysis Services-oktatóanyag projektben. szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-3-mark-as-date-table"></a>3. lecke: Megjelölés dátumtáblaként

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

A 2. leckében (Az adatok beszerzése) importálta a DimDate nevű táblát. Ha a modellben ennek a táblának DimDate a neve, *Dátumtábla* néven is szokás nevezni, mivel ez tartalmazza a dátum- és időadatokat.  
  
A DAX időintelligenciára vonatkozó funkcióinak használatakor, például ha a mértékegységeket később hozza létre, meg kell határoznia egy *Dátumtáblát* és egy egyedi azonosító *Dátumoszlopot* a táblában.
  
Ez a lecke megjelölni a hello DimDate táblázatban hello *dátumtáblázat* és hello dátumoszlopban (a hello dátumtáblázat) hello, *dátumoszlopban* (egyedi azonosítója).  

Csak azt követően megjelölni hello dátum tábla és a dátum oszlop, akkor egy időben toodo kissé háztartási toomake a modell könnyebb toounderstand. Figyelje meg hello DimDate tábla nevű oszlop **FullDateAlternateKey**. Ez az oszlop egy sor minden nap hello táblázatban szereplő naptári évenként tartalmazza. Ezt az oszlopot sokat fogja használni a mértékegységképletekben és jelentésekben. A FullDateAlternateKey elnevezés azonban nem túl jól azonosítja az oszlop funkcióját. Az Átnevezés túl**dátum**, így könnyebb tooidentify, és tartalmazzák a képletekben. Amikor csak lehetséges –-e egy jó ötlet toorename objektumok például a táblákat és oszlopokat toomake azokat könnyebben tooidentify SSDT és az alkalmazások, például a Power bi-ban és az Excel jelentés ügyfélen. 
  
Becsült idő toocomplete Ez a lecke: **három perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni. Ez a lecke hello feladatok elvégzése előtt kell befejeződött hello előző lecke: [lecke 2: adatok beolvasása](../tutorials/aas-lesson-2-get-data.md). 

### <a name="toorename-hello-fulldatealternatekey-column"></a>toorename hello FullDateAlternateKey oszlop

1.  A hello modellek tervezőjében, kattintson a hello **DimDate** tábla.

2.  Kattintson duplán a hello hello fejléc **FullDateAlternateKey** oszlop, és nevezze át túl**dátum**.

  
### <a name="tooset-mark-as-date-table"></a>be van jelölve dátumtáblázatként tooset  
  
1.  SELECT hello **dátum** oszlop, majd a hello **tulajdonságok** ablakban, a **adattípus**, győződjön meg arról, hogy **dátum** van kiválasztva.  
  
2.  Hello kattintson **tábla** menüben, majd kattintson a **dátum**, és kattintson a **be van jelölve dátumtáblázatként**.  
  
3.  A hello **be van jelölve dátumtáblázatként** párbeszédpanel hello **dátum** listbox, jelölje be hello **dátum** oszlop szerint hello egyedi azonosítója. Alapértelmezés szerint általában ez van kiválasztva. Kattintson az **OK** gombra. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>A következő lépések
[4. lecke: Kapcsolatok létrehozása](../tutorials/aas-lesson-4-create-relationships.md)
  
