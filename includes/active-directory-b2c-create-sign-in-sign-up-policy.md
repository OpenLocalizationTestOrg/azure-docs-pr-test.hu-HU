tooenable bejelentkezhet az alkalmazásra, szüksége lesz egy bejelentkezés toocreate házirend. Ez a házirend sikeres bejelentkezések a fogyasztók halad bejelentkezés során, és alkalmazás hello jogkivonatok hello tartalmát kap hello szolgáltatásokat ismerteti.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Hello házirendek beállításainak szakaszában, válassza ki a **regisztráció vagy bejelentkezés házirendek** kattintson **+ Hozzáadás**.

![Válassza ki a regisztrálási vagy a bejelentkezési szabályzatokat, és kattintson a Hozzáadás gombra.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-policy.png)

Adja meg a házirend **neve** a az alkalmazás tooreference. Adja meg például a következőt: `SiUpIn`.

Válassza az **Identitásszolgáltatók** lehetőséget, és jelölje be a **Regisztráció e-mail-címmel** jelölőnégyzetet. Azt is megteheti, hogy közösségi identitásszolgáltatókat választ ki, ha ezek már be vannak állítva. Kattintson az **OK** gombra.

![Válassza ki az e-mailek előfizetési identitás-szolgáltatóként, és hello OK gombra.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-identity-providers.png)

Válassza a **Regisztrálási attribútumok** lehetőséget. Válassza ki az attribútumok kívánt toocollect hello fogyasztói a regisztráció során. Például jelölje be az **Ország/régió**, a **Megjelenítendő név** és az **Irányítószám** attribútumokat. Kattintson az **OK** gombra.

![Egyes attribútumok kiválasztása és hello OK gombra.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-attributes.png)

Válassza az **Alkalmazásjogcímek** lehetőséget. Válassza ki a kívánt visszaadott hello engedélyezési jogkivonatokba jogcímek küldött vissza tooyour alkalmazás sikeres regisztráció vagy bejelentkezés számítógép után. Válassza például a **Megjelenítendő név**, az **Identitásszolgáltató**, az **Irányítószám**, az **Új felhasználó** és a **Felhasználó objektumazonosítója** lehetőséget.

![Válasszon ki néhány alkalmazásjogcímet, majd kattintson az OK gombra.](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-application-claims.png)

Kattintson a **létrehozása** tooadd hello házirend. hello házirend van megadva, **B2C_1_SiUpIn**. Hello **B2C_1_** előtag hozzáfűzött toohello nevét.

Nyissa meg a hello házirend kiválasztásával **B2C_1_SiUpIn**. Ellenőrizze a megadott hello hello beállításait, majd kattintson az **futtatása most**.

![Szabályzat kiválasztása és futtatása](media/active-directory-b2c-create-sign-in-sign-up-policy/run-b2c-signup-signin-policy.png)

| Beállítás      | Érték  |
| ------------ | ------ |
| **Alkalmazások** | Contoso B2C-alkalmazás |
| **Válasz URL-cím kiválasztása** | `https://localhost:44316/` |

Egy új böngészőlapon nyílik meg, és ellenőrizheti hello regisztráció vagy bejelentkezés felhasználói élmény konfigurálva.

> [!NOTE]
> Foglalja el tooa perc, a házirend létrehozásához, és frissíti a tootake hatása.
>