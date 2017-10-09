
Alapértelmezés szerint a API-k a Mobile Apps háttérből névtelenül meghívható. A következő lépésben toorestrict tooonly hitelesített ügyfelek.  

* **NODE.js vissza (az Azure-portálon hello) keresztül end** :  

    A Mobile Apps beállításaiban kattintson **könnyen táblák** és ki kell jelölnie a táblázatot. Kattintson a **engedélyeinek módosítása**, jelölje be **hitelesített hozzáférés csak** összes engedélyeket, és kattintson a **mentése**.
* **Háttér .NET (C#)**:  

    Hello kiszolgálóprojektet, lépjen a túl**tartományvezérlők** > **TodoItemController.cs**. Adja hozzá a hello `[Authorize]` toohello attribútum **TodoItemController** osztály, az alábbiak szerint. toorestrict hozzáférés csak toospecific módszerek is alkalmazhat az attribútum csak toothose módszerek hello osztály helyett. Hello kiszolgálóprojektet közzé.

        [Authorize]
        public class TodoItemController : TableController<TodoItem>

* **NODE.js-háttéralkalmazáshoz (Node.js-kódot) keresztül** :  

    toorequire hitelesítési a tábla eléréséhez, adja hozzá a következő sor toohello Node.js kiszolgáló parancsfájl hello:

        table.access = 'authenticated';

    További részletekért lásd: [Útmutató: hitelesítés megkövetelése a hozzáférési tootables](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-auth). Hogyan toodownload hello gyors üzembe helyezés kódú projektben a helyről: toolearn [hogyan: letöltési hello Node.js háttérrendszer gyors üzembe helyezés kód projekt Git használatával](../articles/app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart).
