## <a name="prerequisites"></a>Előfeltételek
Azt írhat CDN felügyeleti kódját, igazolnia kell ahhoz, hogy az Azure Resource Manager interaktív kód bizonyos előkészítés tegye.  Ehhez meg kell:

* Hozzon létre egy erőforráscsoportot tartalmazó nem ez az oktatóanyag létrehozni a CDN-profil
* A hitelesítés az alkalmazás Azure Active Directory konfigurálása
* Engedélyek alkalmazása az erőforráscsoportot, hogy csak az Azure AD-bérlő engedéllyel rendelkező felhasználók használhatják a CDN-profil

### <a name="creating-the-resource-group"></a>Az erőforráscsoport létrehozása
1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com).
2. Kattintson a **új** bal felső sarokban, gombra, majd **felügyeleti**, és **erőforráscsoport**.

    ![Új erőforráscsoport létrehozása](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Az erőforráscsoport hívás *CdnConsoleTutorial*.  Jelölje ki az előfizetését, és válassza ki azt a helyet környéken.  Ha kívánja, kattintson a **rögzítés az irányítópulton** jelölőnégyzetet, hogy az erőforráscsoport az irányítópulton rögzítheti a portálon.  Ez megkönnyíti a később található.  Beállítás megadása után kattintson **létrehozása**.

    ![Az erőforráscsoport elnevezése](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Az erőforráscsoport létrehozása, ha az irányítópulton nem rögzíti, után megtalálja kattintva **Tallózás**, majd **erőforráscsoportok**.  Kattintson az erőforráscsoport való megnyitásához.  Jegyezze fel a **előfizetés-azonosító**.  Szükség lesz az később.

    ![Az erőforráscsoport elnevezése](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>Az Azure AD-alkalmazás létrehozása és alkalmazása engedélyek
Kétféleképpen app hitelesítés az Azure Active Directoryval: egyéni felhasználók számára, vagy egy egyszerű szolgáltatást. Egy egyszerű szolgáltatásnév hasonlít a Windows szolgáltatás-fiók.  Helyett a jogosultságokat egy adott felhasználó kommunikál a CDN-profilra, azt helyette adja meg az egyszerű szolgáltatásnév a engedélyeket.  Automatikus, nem interaktív folyamatok szolgáltatásnevekről általában használják.  Annak ellenére, hogy ez az oktatóanyag egy interaktív Konzolalkalmazás ír, a lesz a szolgáltatás egyszerű módszer összpontosítanak.

Egy egyszerű szolgáltatás létrehozása áll több lépésből áll, többek között az Azure Active Directory-alkalmazás létrehozása.  Ehhez fogjuk [Ez az oktatóanyag](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Kövesse a lépéseket a a [csatolt oktatóanyag](../articles/resource-group-create-service-principal-portal.md).  Az *rendkívül fontos* végezze el az pontosan leírtak szerint.  Ügyeljen arra, hogy vegye figyelembe a **bérlői azonosító**, **bérlő tartománynevét** (általában egy *. onmicrosoft.com* tartomány kivéve, ha a megadott egyéni tartományt), **ügyfél-azonosító** , és **ügyfél-hitelesítési kulcs**, mert ezekre később fel kell.  Legyen körültekintő, amelyek védelmet a **ügyfél-azonosító** és **ügyfél-hitelesítési kulcs**, mivel ezeket a hitelesítő adatokat a műveletek végrehajtása a szolgáltatás egyszerű, bárki által használható.
>
> Amikor az alkalmazás konfigurálása több-bérlős nevű lépést, válassza ki a **nem**.
>
> Amikor elér a lépés [szerepkör alkalmazást](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), használja a korábban létrehozott erőforráscsoport *CdnConsoleTutorial*, de ahelyett, hogy a **olvasó** szerepkör, rendelje hozzá a  **CDN-profil közreműködői** szerepkör.  Az alkalmazás hozzárendelése után a **CDN-profil közreműködői** az erőforráscsoport ebben az oktatóanyagban lépjen vissza a szerepkört. 
>
>

Miután elkészítette az egyszerű szolgáltatásnév és hozzárendelve a **CDN-profil közreműködői** szerepkör, a **felhasználók** az erőforráscsoport panel ez hasonlóan kell kinéznie.

![Felhasználók panel](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktív felhasználói hitelesítéssel
Ha, helyett egy egyszerű szolgáltatást, akkor ahelyett, hogy egyes interaktív felhasználói hitelesítéssel, a folyamat nagyon hasonlít, amely a szolgáltatásnevet.  Valójában akkor kövesse ugyanazt az eljárást, de néhány kisebb módosításokat.

> [!IMPORTANT]
> Csak kövesse a következő lépések, ha egyéni felhasználói hitelesítés használata helyett egy egyszerű szolgáltatás kiválasztása.
>
>

1. Ahelyett, hogy az alkalmazás létrehozásakor **webalkalmazás**, válassza a **natív alkalmazás**.

    ![Natív alkalmazás](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. A következő oldalon kérni fogja a egy **átirányítási URI**.  Az URI-azonosítója nem érvényesíthető, de ne feledje, hogy a megadott.  Erre később még szüksége lesz.
3. Nincs szükség létrehozásához egy **ügyfél-hitelesítési kulcs**.
4. Ne az egyszerű szolgáltatás a **CDN-profil közreműködői** szerepkör, az oktatóanyagban módosítjuk rendelhető hozzá a felhasználókhoz és csoportokhoz.  Ebben a példában láthatja, hogy korábban kiosztott *CDN bemutató felhasználó* számára a **CDN-profil közreműködői** szerepkör.  

    ![Egyes felhasználók hozzáférését](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
