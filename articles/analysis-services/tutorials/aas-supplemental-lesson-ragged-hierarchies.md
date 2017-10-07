---
cím: aaa "Azure Analysis Services útmutató kiegészítő lecke: hierarchiák rendezetlen |} Microsoft Docs"Leírás: ismerteti, hogyan toofix rendezetlen hello Azure Analysis Services-oktatóanyag hierarchiákat.
szolgáltatások: analysis-szolgáltatások documentationcenter: "Szerző: minewiskan manager: erikre szerkesztőben:" címkék: "

MS.AssetId: ms.service: analysis-szolgáltatások ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 05/26/2017 ms.author: owend
---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Kiegészítő lecke – Hézagos hierarchiák

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Ebben a kiegészítő leckében a különböző szinteken üres értékéket (tagokat) tartalmazó hierarchiák kimutatásokba való felvételének gyakori problémáját oldhatja meg. Például egy szervezetnél, ahol egy magas szintű vezető közvetlen beosztottjai közé részlegvezetők és nem vezető beosztású alkalmazottak is tartoznak. Vagy azok az ország-régió-város földrajzi hierarchiák, ahol egyes városoknak nincs szülőállama vagy megyéje, mint például Washington D.C. vagy Vatikánváros esetében. Ha a hierarchiában az üres tagok, gyakran toodifferent aljának, illetve rendezetlen, szintek.

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

Hello 1 400 kompatibilitási szinttel rendelkező táblázatos modellek rendelkezik egy további **elrejtése tagok** hierarchiákhoz tulajdonság. Hello **alapértelmezett** beállítás feltételezi, hogy üres tagok bármely szinten. Hello **üres tagok elrejtése** beállítás nem tartalmazza az üres tagok felvételekor hello hierarchiából tooa kimutatás vagy a jelentés.  
  
Becsült idő toocomplete Ez a lecke: **20 perc**  
  
## <a name="prerequisites"></a>Előfeltételek  
Ez a kiegészítőlecke-témakör a táblázatos modellezésről szóló oktatóanyag része. Ez a kiegészítő lecke hello feladatok elvégzése előtt kell befejeződött az összes korábbi során tapasztalatokat, vagy egy befejezett Adventure Works Internet értékesítési minta jelentésmodell-projekt. 

Hello AW Internet értékesítési projekt hello oktatóanyag részeként létrehozott, ha a modell nem még tartalmaz adatokat vagy, amelyek az egyenetlen szélű hierarchiákat. toocomplete Ez a kiegészítő lecke, először azt kell toocreate probléma hello néhány további táblákon való hozzáadásával, kapcsolatok, számított oszlopokban, egy mérték és egy új szervezeti hierarchia létrehozása. Ez a rész nagyjából 15 percet vesz igénybe. Ezt követően toosolve kap, néhány perc múlva.  

## <a name="add-tables-and-objects"></a>Táblázatok és objektumok hozzáadása
  
### <a name="tooadd-new-tables-tooyour-model"></a>tooadd új táblák tooyour modell
  
1.  A Tabular Model Explorerben bontsa ki az **Adatforrások** szakaszt, és kattintson a jobb gombbal a kapcsolatra, majd pedig az **Új táblázatok importálása** gombra.
  
2.  A Kezelőben válassza a **DimEmployee** és a **FactResellerSales** lehetőséget, majd kattintson az **OK** gombra.

3.  A Lekérdezésszerkesztőben kattintson az **Importálás** lehetőségre

4.  Hozza létre a következőket hello [kapcsolatok](../tutorials/aas-lesson-4-create-relationships.md):

    | 1. táblázat           | Oszlop       | Szűrés iránya   | 2. táblázat     | Oszlop      | Aktív |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Alapértelmezett            | DimDate     | Dátum        | Igen    |
    | FactResellerSales | DueDate      | Alapértelmezett            | DimDate     | Dátum        | Nem     |
    | FactResellerSales | ShipDateKey  | Alapértelmezett            | DimDate     | Dátum        | Nem     |
    | FactResellerSales | ProductKey   | Alapértelmezett            | DimProduct  | ProductKey  | Igen    |
    | FactResellerSales | EmployeeKey  | tooBoth táblák | DimEmployee | EmployeeKey | Igen    |

5. A hello **DimEmployee** table, hozza létre a következőket hello [számított oszlopok](../tutorials/aas-lesson-5-create-calculated-columns.md): 

    **Elérési út** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **FullName** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Level1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  A hello **DimEmployee** tábla, hozzon létre egy [hierarchia](../tutorials/aas-lesson-9-create-hierarchies.md) nevű **szervezet**. Adja hozzá a következő oszlopok sorrendben hello: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.

7.  A hello **FactResellerSales** table, hozza létre a következőket hello [mérték](../tutorials/aas-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Használjon [elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel, majd automatikusan létrehoz egy kimutatást.

9.  A **kimutatástábla mezői**, adja hozzá a hello **szervezet** hello hierarchia **DimEmployee** túl tábla**sorok**, és hello **ResellerTotalSales** hello mértéket **FactResellerSales** túl tábla**értékek**.

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    Ahogy látja, a hello kimutatás, hello hierarchia sorokat, amelyek egyenetlen jeleníti meg. Több sor is látható, amely üres tagokat tartalmaz.

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a>toofix hello hierarchia rendezetlen tulajdonság hello elrejtése tagok beállításával.

1.  A **Táblázatos modelltallózóban** bontsa ki a következőt: **Táblák** > **DimEmployee** > **Hierarchiák** > **Szervezet**.

2.  A **Tulajdonságok** > **Tagok elrejtése** részen válassza az **Üres tagok elrejtése** lehetőséget. 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Vissza az Excel programban frissítse a kimutatást hello. 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Így már sokkal jobban néz ki!

## <a name="see-also"></a>Lásd még:   
[9. lecke: Hierarchiák létrehozása](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Kiegészítő lecke – Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Kiegészítő lecke – Részletsorok](../tutorials/aas-supplemental-lesson-detail-rows.md)  