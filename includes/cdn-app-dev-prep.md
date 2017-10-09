## <a name="prerequisites"></a>Előfeltételek
Azt is létrehozható, CDN felügyeleti, igazolnia kell toodo néhány előkészítő tooenable hello Azure Resource Manager és a kód toointeract.  toodo, ez lesz szüksége:

* Egy erőforrás csoport toocontain hello nem ez az oktatóanyag létrehozni a CDN-profil létrehozása
* Az alkalmazás Azure Active Directory-tooprovide hitelesítés konfigurálása
* Engedélyek toohello erőforráscsoport, hogy csak engedélyezett az Azure AD-bérlő felhasználók használhatják a CDN-profil alkalmazása

### <a name="creating-hello-resource-group"></a>Hello erőforráscsoport létrehozása
1. Jelentkezzen be a hello [Azure Portal](https://portal.azure.com).
2. Kattintson a hello **új** hello a bal felső, gombjára, majd **felügyeleti**, és **erőforráscsoport**.

    ![Új erőforráscsoport létrehozása](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)
3. Az erőforráscsoport hívás *CdnConsoleTutorial*.  Jelölje ki az előfizetését, és válassza ki azt a helyet környéken.  Ha kívánja, kattintson a hello **PIN-kód toodashboard** jelölőnégyzet toopin hello erőforrás csoport toohello irányítópult hello portálon.  Ez teszi egyszerűbbé toofind később.  Beállítás megadása után kattintson **létrehozása**.

    ![Elnevezési hello erőforráscsoport](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)
4. Miután hello erőforráscsoportban jön létre, ha tooyour irányítópult nem rögzíti, megtalálhatja kattintva **Tallózás**, majd **erőforráscsoportok**.  Kattintson a hello erőforrás csoport tooopen azt.  Jegyezze fel a **előfizetés-azonosító**.  Szükség lesz az később.

    ![Elnevezési hello erőforráscsoport](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-hello-azure-ad-application-and-applying-permissions"></a>Hello Azure AD-alkalmazás létrehozása és alkalmazása engedélyek
Kétféleképpen tooapp hitelesítés az Azure Active Directoryval: egyéni felhasználók számára, vagy egy egyszerű szolgáltatást. Egy egyszerű Windows hasonló tooa szolgáltatásfiók.  Ahelyett, hogy egy adott felhasználói engedélyek toointeract hello CDN profilokkal biztosítása, azt helyette engedélyeket hello toohello egyszerű szolgáltatásnév.  Automatikus, nem interaktív folyamatok szolgáltatásnevekről általában használják.  Annak ellenére, hogy ez az oktatóanyag egy interaktív Konzolalkalmazás ír, a lesz hello szolgáltatás egyszerű módszer összpontosítanak.

Egy egyszerű szolgáltatás létrehozása áll több lépésből áll, többek között az Azure Active Directory-alkalmazás létrehozása.  toodo, ez túl fogjuk[Ez az oktatóanyag](../articles/resource-group-create-service-principal-portal.md).

> [!IMPORTANT]
> Lehet, hogy toofollow hello lépéseket a hello [csatolt oktatóanyag](../articles/resource-group-create-service-principal-portal.md).  Az *rendkívül fontos* végezze el az pontosan leírtak szerint.  Győződjön meg arról, hogy toonote a **bérlői azonosító**, **bérlő tartománynevét** (általában egy *. onmicrosoft.com* tartomány kivéve, ha a megadott egyéni tartományt), **ügyfél-azonosító** , és **ügyfél-hitelesítési kulcs**, mert ezekre később fel kell.  Nagyon óvatos tooguard kell a **ügyfél-azonosító** és **ügyfél-hitelesítési kulcs**, mivel ezek a hitelesítő adatok bárki használt tooexecute műveletek hello szolgáltatás egyszerű.
>
> Amikor több-bérlős alkalmazás konfigurálása nevű toohello lépés, válassza ki a **nem**.
>
> Amikor elérhetővé toohello lépés [rendelje hozzá az alkalmazás toorole](../articles/azure-resource-manager/resource-group-create-service-principal-portal.md#assign-application-to-role), használjon hello korábban létrehozott erőforráscsoportot *CdnConsoleTutorial*, de hello helyett **olvasó** szerepkör, hozzárendelése Hello **CDN-profil közreműködői** szerepkör.  Hello alkalmazás hello hozzárendelése után **CDN-profil közreműködői** az erőforráscsoportot, a visszatérési toothis oktatóanyag szerepkört. 
>
>

Ha létrehozta a szolgáltatás egyszerű és hozzárendelt hello **CDN-profil közreműködői** szerepkör, hello **felhasználók** az erőforráscsoport panel alábbihoz hasonló toothis.

![Felhasználók panel](./media/cdn-app-dev-prep/cdn-service-principal-include.png)

### <a name="interactive-user-authentication"></a>Interaktív felhasználói hitelesítéssel
Ha helyett egy egyszerű, ahelyett, hogy egyes interaktív felhasználói hitelesítéssel kellene lennie, hello folyamat nagyon hasonló toothat a szolgáltatásnevet.  Valójában kell toofollow hello ugyanezt az eljárást, de néhány kisebb módosításokat.

> [!IMPORTANT]
> Csak kövesse a következő lépések, ha toouse egyes felhasználói hitelesítés helyett egy egyszerű szolgáltatás kiválasztása.
>
>

1. Ahelyett, hogy az alkalmazás létrehozásakor **webalkalmazás**, válassza a **natív alkalmazás**.

    ![Natív alkalmazás](./media/cdn-app-dev-prep/cdn-native-application-include.png)
2. A következő lapon hello kérni fogja a egy **átirányítási URI**.  hello URI azonosítója nem érvényesíthető, de ne feledje, hogy a megadott.  Erre később még szüksége lesz.
3. Nincs szükség toocreate van egy **ügyfél-hitelesítési kulcs**.
4. A szolgáltatás egyszerű toohello ne **CDN-profil közreműködői** szerepkör, fogjuk tooassign felhasználóknak vagy csoportoknak.  Ebben a példában láthatja, hogy korábban kiosztott *CDN bemutató felhasználó* toohello **CDN-profil közreműködői** szerepkör.  

    ![Egyes felhasználók hozzáférését](./media/cdn-app-dev-prep/cdn-aad-user-include.png)
