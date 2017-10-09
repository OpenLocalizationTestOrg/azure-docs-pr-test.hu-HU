[!INCLUDE [active-directory-b2c-portal-add-application](active-directory-b2c-portal-add-application.md)]

tooregister a mobil, vagy a natív alkalmazás, a megadott hello hello-beállítások használata.

![Példák az új mobil vagy natív alkalmazások regisztrációs beállításaira](./media/active-directory-b2c-register-mobile-native-app/b2c-new-mobile-native-app-settings.png)

| Beállítás      | Mintaérték  | Leírás                                        |
| ------------ | ------- | -------------------------------------------------- |
| **Name (Név)** | Contoso B2C-alkalmazás | Adjon meg egy **neve** hello alkalmazáshoz, amely az alkalmazás tooconsumers ismerteti. |
| **Natív ügyfél** | Igen | Válassza az **Igen** lehetőséget mobil vagy natív alkalmazás esetén. |
| **Egyéni átirányítási URI** | `com.onmicrosoft.contoso.appname://redirect/path` | Adjon meg egy egyéni sémával rendelkező átirányítási URI-t. Ügyeljen arra, hogy [megfelelő átirányítási URI-t](../articles/active-directory-b2c/active-directory-b2c-app-registration.md#choosing-a-native-application-redirect-uri) válasszon, és különleges karakterek, például aláhúzásjelek ne szerepeljenek benne. |

Kattintson a **létrehozása** tooregister az alkalmazást.

Az újonnan regisztrált alkalmazás hello B2C-bérlő hello alkalmazások listája jelenik meg. Válassza ki a mobil, vagy a natív alkalmazás hello listából. hello alkalmazás tulajdonság ablaktáblán jelenik meg.

![Az alkalmazás tulajdonságai](./media/active-directory-b2c-register-mobile-native-app/b2c-mobile-native-app-properties.png)

Jegyezze fel a hello globálisan egyedi **alkalmazás ügyfél-azonosító**. Az alkalmazás kódjában hello Azonosítóját használnia.

Ha natív alkalmazása az Azure AD B2C által védett webes API-t hív meg, kövesse a következő lépéseket:
   1. Hozzon létre egy alkalmazás titkos kulcs is toohello **kulcsok** panelen, majd kattintson a hello **készítése kulcs** gombra. Jegyezze fel a hello **Alkalmazáskulcs** érték. Az alkalmazás kódjában hello alkalmazás titkos kulcsként hello érték használata.
   2. Kattintson az **API-hozzáférés**, majd a **Hozzáadás**, lehetőségre, és válassza ki webes API-ját és a hatóköröket (engedélyeket).

> [!NOTE]
> Az **Application Secret** (Alkalmazástitok) fontos biztonsági hitelesítő adat, amelynek megfelelő biztonságáról gondoskodni kell.
> 
