Most, hogy egy eseményindítót, az idő toodo valami érdekes hello eseményindító által létrehozott hello adatokkal hozzáadását. Kövesse az alábbi lépéseket tooadd hello **SharePoint Online - fájl létrehozása** művelet. Ez a művelet létrehoz egy fájlt a SharePoint Online minden alkalommal hello új cikk eseményindító következik be. 

tooconfigure hello Ez a művelet, szüksége lesz a következő információ tooprovide hello. Mivel ezek az információk láthatja, hogy a rendszer az új fájl hello hello tulajdonságok bemeneteként hello eseményindító által létrehozott egyszerű toouse adatokat:

| Fájl tulajdonság létrehozása | Leírás |
| --- | --- |
| Webhely URL-címe |Ez a hello hello SharePoint Online helyet, ahol toocreate hello új fájl URL-CÍMÉT. Válassza ki a hello helyet hello-listáról. |
| Mappa elérési útja |Ez az hello mappát a (hello hello előző lépésben kiválasztott webhely URL-címe) ahol hello új fájl kerül. Tallózással keresse meg és válassza ki hello mappát. |
| Fájlnév |Ez az hello név hello fájl létrehozása folyamatban. |
| A fájl |a rendszer úgy tünteti toohello fájl hello tartalom. |

1. Válassza ki **+ új lépés** tooadd hello művelet.  
   ![SharePoint online művelet kép 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Jelölje be hello **művelet hozzáadása** hivatkozásra. A megnyíló hello keresőmezőbe ahol kereshet bármely művelet, tootake szeretné. Ebben a példában a SharePoint-műveletek végezhetők, iránt.    
   ![SharePoint online művelet kép 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Adja meg *sharepoint* toosearch kapcsolódó tooSharePoint műveletek számára.
4. Válassza ki **SharePoint Online - fájl létrehozása** művelet tootake hello szerint.   **Megjegyzés:**: akkor lesz felszólító tooauthorize a logic app tooaccess a SharePoint fiókot, ha nem hozott létre a kapcsolat tooSharePoint Online korábban.    
   ![SharePoint online művelet kép 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. Hello **létrehozás fájl** megnyílik szabályozzák.   
   ![SharePoint online művelet kép 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Válassza ki **webhely URL-címe** és Tallózás toofind hello hely toocreate hello fájl helyének.     
   ![SharePoint online művelet kép 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Válassza ki **mappa elérési útja** és Tallózás toofind hello mappa ahol hello új fájl kerül.  
   ![SharePoint online művelet kép 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Jelölje be hello **Fájlnév** szabályozza, és írja be a hello neve hello fájl toocreate kívánt. Itt adhatja meg hello fájlnevet közvetlenül, vagy használhatja a korábban létrehozott hello eseményindító hello tulajdonságok valamelyikét. Ehhez a Tulajdonságok parancsra kattintva hello listája **kimeneteinek új elem létrehozásakor**. A lista létrehozási csak megjelenítési, miután kiválasztotta a hello **Fájlnév** vezérlő. A walkthough a kiválasztott ID (hello hello új listaelem azonosítója) hello által létrehozott hello fájl hello névként **SharePoint Online - fájl létrehozása** művelet.    
   ![SharePoint online művelet kép 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Jelölje be hello **tartalom fájl** vezérlik, és adja meg a létrehozandó toohello fájl írt hello tartalmat. Hello fájl tartalom figyelje meg, használhatja a korábban létrehozott hello eseményindító hello tulajdonságok valamelyikét. Egyszerűen válassza hello tulajdonságok hello-listáról. Másik lehetőségként megadhat hello **tartalom fájl** szöveg közvetlenül hello vezérlőbe. Ebben a példában I kijelölt egyes tulajdonságok és területek és az egyes tulajdonságokhoz kötőjelet hozzá.        
   ![SharePoint online művelet kép 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Hello módosítások tooyour munkafolyamat mentése  
11. Gratulálunk, most már rendelkezik egy teljesen működőképes logikai alkalmazás, amely lesz kiváltva, ha egy új listaelem tooa SharePoint Online listája. hello alkalmazás majd létrehoz egy fájlt, az új listaelem hello hello tulajdonságok használatával.  Most tesztelheti, hozzon létre egy új elem hello SharePoint-listán. 

