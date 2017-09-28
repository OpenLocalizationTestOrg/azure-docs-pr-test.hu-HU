[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

Webalkalmazása regisztrálásához használja a táblázatban megadott beállításokat.

![Példák az új webalkalmazások regisztrációs beállításaira](./media/active-directory-b2c-register-web-app/b2c-new-app-settings.png)

| Beállítás      | Mintaérték  | Leírás                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name (Név)** | Contoso B2C-alkalmazás | Adjon meg egy olyan **nevet**, amely ismerteti az alkalmazást a felhasználók számára. | 
| **Webalkalmazás vagy webes API szerepeltetése** | Igen | Válassza az **Igen** lehetőséget webalkalmazások esetén. |
| **Implicit folyamat engedélyezése** | Igen | Válassza az **Igen** lehetőséget, ha az alkalmazása [OpenID Connect bejelentkezést](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) használ. |
| **Válasz URL-cím** | `https://localhost:44316` | A válasz URL-címek olyan végpontok, amelyeken keresztül az Azure AD B2C visszaadja az alkalmazás által kért jogkivonatokat. Adjon meg [egy megfelelő](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **válasz URL-címet**. Ebben a példában a webalkalmazás helyi, és a 44316-os porton figyel. |

Kattintson a **Create** (Létrehozás) gombra az alkalmazás regisztrálásához.

Az újonnan regisztrált alkalmazás a B2C-bérlő alkalmazásainak listájában jelenik meg. Válassza ki a webalkalmazását a listáról. Ekkor megjelenik a webalkalmazás tulajdonságpanelje.

![Webalkalmazás – Tulajdonságok](./media/active-directory-b2c-register-web-app/b2c-web-app-properties.png)

Jegyezze fel az alkalmazás globálisan egyedi **ügyfél-azonosítóját**. Ezt az azonosítót használja az alkalmazás kódjában.