tooenable profil szerkesztése az alkalmazásra, szüksége lesz toocreate egy Profilszerkesztési házirend. Ezzel a házirend-profil szerkesztése és hello tartalmát hello alkalmazás sikeres befejezését követően kap jogkivonatok során végighaladnia fogyasztók hello szolgáltatásokat írja le.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Hello házirendek beállításainak szakaszában, válassza ki a **Profilszerkesztési házirendek** kattintson **+ Hozzáadás**.

![Válassza ki a Profilszerkesztési házirendek és hello Hozzáadás gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-policy.png)

Adja meg a házirend **neve** a az alkalmazás tooreference. Adja meg például a következőt: `SiPe`.

Válassza az **Identitásszolgáltatók** lehetőséget, és jelölje be a **Bejelentkezés helyi fiókba** jelölőnégyzetet. Azt is megteheti, hogy közösségi identitásszolgáltatókat választ ki, ha ezek már be vannak állítva. Kattintson az **OK** gombra.

![Válassza ki a helyi fiókkal bejelentkezik identitás-szolgáltatóként, és hello OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-identity-providers.png)

Válassza a **Profilattribútumok** elemet. Válassza ki az attribútumok hello fogyasztói megtekintéséhez és szerkesztéséhez a profilban. Például jelölje be az **Ország/régió**, a **Megjelenítendő név** és az **Irányítószám** attribútumokat. Kattintson az **OK** gombra.

![Egyes attribútumok kiválasztása és hello OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-attributes.png)

Válassza az **Alkalmazásjogcímek** lehetőséget. Válassza ki a kívánt visszaadott hello engedélyezési jogkivonatokba jogcímek küldött vissza tooyour alkalmazás sikeres Profilszerkesztési élmény után. Válassza például a **Megjelenítendő név** és az **Irányítószám** lehetőséget.

![Válasszon ki néhány alkalmazásjogcímet, majd kattintson az OK gombra.](media/active-directory-b2c-create-profile-editing-policy/add-b2c-editing-application-claims.png)

Kattintson a **létrehozása** tooadd hello házirend. hello házirend van megadva, **B2C_1_SiPe**. Hello **B2C_1_** előtag hozzáfűzött toohello nevét.

Nyissa meg a hello házirend kiválasztásával **B2C_1_SiPe**. Ellenőrizze a megadott hello hello beállításait, majd kattintson az **futtatása most**.

![Szabályzat kiválasztása és futtatása](media/active-directory-b2c-create-profile-editing-policy/run-b2c-editing-policy.png)

| Beállítás      | Érték  |
| ------------ | ------ |
| **Alkalmazások** | Contoso B2C-alkalmazás |
| **Válasz URL-cím kiválasztása** | `https://localhost:44316/` |

Egy új böngészőlapon nyílik meg, és ellenőrizheti, hogy hello Profilszerkesztési végfelhasználói élmény konfigurált módon.

> [!NOTE]
> Foglalja el tooa perc, a házirend létrehozásához, és frissíti a tootake hatása.
>