tooenable minden részletre kiterjedő jelszó-változtatási az alkalmazásra, szüksége lesz egy jelszó-visszaállítási házirend toocreate. Vegye figyelembe, hogy hello bérlői kiterjedő jelszó-átállítási beállítás van megadva [Itt](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Ezzel a házirend-jelszó alaphelyzetbe állítása során hello fogyasztók halad, és alkalmazás hello jogkivonatok hello tartalmát kap hello szolgáltatásokat sikeres befejezését követően ismerteti.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

Hello házirendek beállításainak szakaszában, válassza ki a **jelszó-átállítási házirendek** kattintson **+ Hozzáadás**.

![Jelölje ki a regisztráció vagy bejelentkezés házirendek és hello Hozzáadás gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-policy.png)

Adja meg a házirend **neve** a az alkalmazás tooreference. Adja meg például a következőt: `SSPR`.

Válassza az **Identitásszolgáltatók** lehetőséget, és jelölje be a **Jelszó visszaállítása e-mail-címmel** jelölőnégyzetet. Kattintson az **OK** gombra.

![Válassza ki a jelszó-átállítási identitás-szolgáltatóként e-mail címet használja, és hello OK gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

Válassza az **Alkalmazásjogcímek** lehetőséget. Válassza ki a kívánt visszaadott hello engedélyezési jogkivonatokba jogcímek hátsó tooyour alkalmazás küld, miután a sikeres jelszó-létrehozási élmény. Válassza például a **Felhasználó objektumazonosítója** lehetőséget.

![Válasszon ki néhány alkalmazásjogcímet, majd kattintson az OK gombra.](media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Kattintson a **létrehozása** tooadd hello házirend. hello házirend van megadva, **B2C_1_SSPR**. Hello **B2C_1_** előtag hozzáfűzött toohello nevét.

Nyissa meg a hello házirend kiválasztásával **B2C_1_SSPR**. Ellenőrizze a megadott hello hello beállításait, majd kattintson az **futtatása most**.

![Szabályzat kiválasztása és futtatása](media/active-directory-b2c-create-password-reset-policy/run-b2c-password-reset-policy.png)

| Beállítás      | Érték  |
| ------------ | ------ |
| **Alkalmazások** | Contoso B2C-alkalmazás |
| **Válasz URL-cím kiválasztása** | `https://localhost:44316/` |

Egy új böngészőlapon nyílik meg, és ellenőrizheti, hogy hello jelszó alaphelyzetbe állítása az alkalmazás felhasználói élményt.

> [!NOTE]
> Foglalja el tooa perc, a házirend létrehozásához, és frissíti a tootake hatása.
>
