Címkékkel tooyour Azure-erőforrások toologically kategóriák szerint rendszerezze őket. Minden címke egy névből és egy értékből áll. Alkalmazhat például hello neve "Környezet" és a hello érték "Éles" tooall hello erőforrások éles környezetben. E nélkül a címke nélkül nehézségekbe ütközhet, ha meg szeretné állapítani, hogy az adott erőforrás fejlesztési, tesztelési vagy éles üzemi célokat szolgál. A „Környezet” és az „Éles” azonban csak például szolgál. Hello és jelentéssel bírnak hello legtöbb rendszerezéséhez az előfizetés értékeket adhat meg.

Címkék alkalmazása után minden hello erőforrások kérheti le az előfizetéshez, hogy a címke neve és értéke. Címkék engedélyezése, tooretrieve kapcsolódó erőforrásokat, amelyek a különböző erőforráscsoportokra találhatók. Ez a módszer akkor hasznos, ha tooorganize erőforrások számlázási vagy felügyeleti szüksége.

a következő korlátozások hello tootags vonatkoznak:

* Minden egyes erőforrás vagy erőforráscsoport legfeljebb 15 címkenév/érték párral rendelkezhet. Ez a korlátozás vonatkozik csak közvetlenül alkalmazható tootags toohello erőforráscsoport erőforrás. Az erőforráscsoportok sok olyan erőforrást tartalmazhatnak, amelyek mindegyike 15 címkenév/érték párral rendelkezik. 
* hello címkenév korlátozott too512 karakterek, hello címke értéke pedig korlátozott too256 karaktereket. Storage-fiókok hello címkenév korlátozott too128 karakterek, hello címke értéke pedig korlátozott too256 karaktereket.
* Az erőforráscsoport hello erőforrások nem örökli alkalmazott címkék toohello erőforráscsoportot. 

Ha több mint 15 értékeket, hogy kell-e tooassociate erőforrással, használja a JSON karakterláncnak hello címke értéke. hello JSON-karakterláncban alkalmazott tooa egyetlen címke neve sok értékeket tartalmazhat. Ez a cikk egy JSON-karakterlánc toohello címke hozzárendelése példáját mutatja be.
