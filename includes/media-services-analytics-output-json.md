hello feladat észlelt és a nyomon követett lapok metaadatokat tartalmazó JSON kimeneti fájlt hoz létre. hello metaadatok jelzi a lapok hello helyét, valamint egy arcfelismerési azonosító szám jelző hello nyomon, hogy egyes koordinátát tartalmaz. Arcfelismerési azonosítószámát esetén körülmények nagyon eséllyel fordulnak elő tooreset hello elülső arcfelismerési elvesztése vagy átfedésben hello keretében, néhány első hozzárendelt több azonosítók egyének eredményez.

hello kimeneti JSON tartalmazza a következő attribútumok hello:

| Elem | Leírás |
| --- | --- |
| Verzió |Hello videó API verziója toohello utal. |
| Index | (Csak Media Redactor tooAzure vonatkozik) hello aktuális esemény hello keret indexét határozza meg. |
| időskálára |"Ticks" hello videó másodpercenként. |
| Az offset |Ez a tárolt időbélyegek hello idő eltolódását. Videó API-k 1.0-s verziójában az mindig 0 lesz. A jövőben támogatott forgatókönyveket, ezt az értéket módosíthatja. |
| képkockasebességhez |Videó hello képkockasebessége. |
| töredék |hello metaadatok töredék nevű különböző részekre van darabolásos fel. Minden töredéke a start, a időtartama, a időszakának száma és a esemény (eke) tartalmaz. |
| start |hello hello első esemény a "ticks" elindításához. |
| Időtartam |hello töredék, a "ticks" Hello hossza. |
| interval |mindegyik esemény bejegyzése hello töredék, a "ticks" Hello időközt. |
| események |Minden esemény észlelt, és adott időtartamig belül követhetők hello lapokat tartalmaz. Az események tömbje. hello külső tömb egy időköz jelöli. hello belső tömb 0 vagy több olyan eseményeket, amelyek ezen a ponton az időben történtek áll. Egy üres zárójel [] azt jelenti, hogy nincs lapok észlelt. |
| id |hello oldallal, amely a követett hello azonosítója. Ez a szám lehet, hogy akaratlanul is módosíthatja, ha egy lap nem válik. Egy adott személy azonos azonosító teljes teljes videó hello hello kell rendelkeznie, de ez nem garantálható esedékes toolimitations hello észlelési algoritmus (hangelnyelés, stb.) |
| x, y |hello bal felső sarokban X és Y koordinátáját hello szembesülhetnek határolókeret 0.0 too1.0 normalizált skála. <br/>-X és Y koordináták százalékosan relatív toolandscape mindig, így ha egy álló video (vagy feje-tetejére, az iOS hello esetben), konfigurálnia kell tootranspose hello koordináták ennek megfelelően. |
| szélesség, magassága |hello szélessége és magassága hello szembesülhetnek határolókeret 0.0 too1.0 normalizált skála. |
| facesDetected |Ez hello JSON eredmények hello végén található, továbbá összefoglalja hello lapok száma, az adott videó hello során észlelt hello algoritmust. Mivel hello azonosítók állítható alaphelyzetbe akaratlanul is, ha egy lap nem válik (pl. arcfelismerési kerül ki képernyőt, azonnal keres), ez a szám nem lehet, hogy mindig azonos hello igaz hello videó lapok száma. |

