[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister a webes API-beállításokkal hello hello táblában.

![Példák az új webes API regisztrációs beállításaira](./media/active-directory-b2c-register-web-api/b2c-new-web-api-settings.png)

| Beállítás      | Mintaérték  | Leírás                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name (Név)** | Contoso B2C API | Adjon meg egy **neve** hello alkalmazás, amely leírja a API tooconsumers. | 
| **Webalkalmazás vagy webes API szerepeltetése** | Igen | Válassza az **Igen** lehetőséget a webes API-k esetén. |
| **Implicit folyamat engedélyezése** | Igen | Válassza az **Igen** lehetőséget, ha az alkalmazása [OpenID Connect bejelentkezést](../articles/active-directory-b2c/active-directory-b2c-reference-oidc.md) használ. |
| **Válasz URL-cím** | `https://localhost:44316/` | A válasz URL-címek olyan végpontok, amelyeken keresztül az Azure AD B2C visszaadja az alkalmazás által kért jogkivonatokat. Adjon meg [egy megfelelő](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-web-app-or-api-reply-url) **válasz URL-címet**. Ebben a példában a webes API helyi, és a 44316-os porton figyel. |
| **Alkalmazásazonosító URI** | api-t | hello App ID URI az hello azonosítója a webes API-t használja. hello hello tartománnyal URI jön létre az Ön teljes azonosítóját. |

Kattintson a **létrehozása** tooregister az alkalmazást.

Az újonnan regisztrált alkalmazás hello B2C-bérlő hello alkalmazások listája jelenik meg. Válassza ki a webes API hello listából. hello API tulajdonság ablaktáblán jelenik meg.

![Webes API – Tulajdonságok](./media/active-directory-b2c-register-web-api/b2c-web-api-properties.png)

Jegyezze fel a hello globálisan egyedi **alkalmazás ügyfél-azonosító**. Az alkalmazás kódjában hello Azonosítóját használnia.
