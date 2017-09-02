
<br>

> [!NOTE]
> Egy rendszergazda felhasználónevet és jelszót kapott, nincs-e esély arra, hogy már rendelkezik munkahelyi vagy iskolai azonosítója (néha is egy *szervezeti Azonosítóval*). Ha igen, azonnal megkezdheti az Azure-fiókjával jelszó használata kötelezővé tehető Azure-erőforrások elérésére használhat. Ha talál meg, hogy nem használja ezeket az erőforrásokat, szükség lehet a cikk segítséget való visszatéréshez. További információkért lásd: [fiókokat is használhatja a bejelentkezés](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) és [hogyan egy Azure-előfizetéshez az Azure AD kapcsolódó](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).
> 
> 

Az egyszerű lépésekre. Meg kell meghatározni az aláírt identitás a klasszikus Azure portálon Fedezze fel az alapértelmezett Azure Active Directory-tartomány, és új felhasználók hozzáadása az Azure közös rendszergazdaként.

## <a name="locate-your-default-directory-in-the-azure-classic-portal"></a>Keresse meg az alapértelmezett címtárat a klasszikus Azure portálon
Először jelentkezik be a [a klasszikus Azure portálon](https://manage.windowsazure.com) a személyes Microsoft-fiók identitással. Miután bejelentkezett, görgessen lefelé a kék panel bal oldalán, majd kattintson a **ACTIVE DIRECTORY**.

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

Először néhány a felhasználó adatainak keresése az Azure-ban. Meg kell jelennie a következőhöz a fő ablaktáblán látható, hogy rendelkezik-e egy alapértelmezett címtár.

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

Keressük meg néhány további információt. Kattintson a alapértelmezett directory sorra, amely azt az alapértelmezett címtár tulajdonságait.  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

Az alapértelmezett tartomány nevét megtekintéséhez kattintson **TARTOMÁNYOK**.

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

Itt kell tudni, hogy az Azure-fiók létrehozása után Azure Active Directoryban, amely a kivonat értéke (karakterlánc alapján generált szám) személyes alapértelmezett tartomány létrehozása a személyes azonosító onmicrosoft.com altartománya használja. Ez a tartomány, amelyhez most hozzáadjuk egy új felhasználót.

## <a name="creating-a-new-user-in-the-default-domain"></a>Új felhasználó létrehozása az alapértelmezett tartományban
Kattintson a **felhasználók** , és tekintse meg a egy személyes fiók. A kell megjelennie a **forrás** oszlop, hogy ez egy **Microsoft-fiók**. Azt szeretnénk, hogy a felhasználó létrehozása az alapértelmezett. onmicrosoft.com Azure Active Directory-tartományhoz.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

Kövesse az oktatóanyagban módosítjuk [ezeket az utasításokat](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) a következő néhány lépésben leírtak, de használja az adott példát.

Kattintson a lap alján **+ felhasználó hozzáadása**. Az oldal jelenik meg, írja be az új felhasználónevet, és ellenőrizze a **felhasználó típusa** egy **a szervezet új felhasználó**. Ebben a példában az új felhasználó neve: `ahmet`. Válassza ki az alapértelmezett tartomány, akkor a felderített korábban a tartományként ahmet tartozó e-mail cím. Kattintson a Tovább nyílra, amikor befejeződött.

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

Adja hozzá a további részleteket a Ahmet, de győződjön meg arról, hogy válassza ki a megfelelő **SZEREPKÖR** érték. Könnyen használható legyen **globális rendszergazda** annak sure dolgot dolgozik, de ha egy kisebb szerepkör használatához, érdemes. Ez a példa a **felhasználói** szerepkör. (További: [rendszergazdai jogosultságokkal szerepkör](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Ne engedélyezze a multi-factor authentication, kivéve, ha a többtényezős hitelesítés használjanak minden egyes bejelentkezés műveletet szeretne. Amikor végzett, kattintson a Tovább nyílra.

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

Kattintson a **létrehozása** készítése és ideiglenes jelszót Ahmet megjelenítése gombra.

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

Másolja a felhasználói e-mail-címet, vagy használjon **jelszó része az E-mail KÜLDÉSE**. Az információk hamarosan jelentkezzen be lesz szüksége.

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

Most látnia kell az új felhasználó **Ahmet a fejlesztői**, termékekre, az Azure Active Directoryból. Létrehozta az új munkahelyi vagy iskolai azonosító az Azure Active Directoryban. Azonban ez az identitás még nincs jogosultsága Azure-erőforrások használatára.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

Ha **jelszó része az E-mail KÜLDÉSE**, a következő típusú e-mailt küld.

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a>Az előfizetések Azure közös rendszergazdai jogosultságokkal hozzáadása
A továbbiakban létre kell az új felhasználó hozzáadása az előfizetés társadminisztrátoraként, így az új felhasználó bejelentkezve is a kezelési portálon. Ennek elvégzéséhez a bal alsó panelen kattintson a **beállítások**.

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

A fő beállítások területen kattintson a **RENDSZERGAZDÁK** felső és azt kell látnia csak a személyes Microsoft fiók azonosítóját. Kattintson a lap alján **+ Hozzáadás** társadminisztrátorának megadásához. Itt adja meg a felhasználó e-mail címe az az új hozna létre, beleértve az alapértelmezett tartomány. Képernyőfelvételen látható módon a következő, egy zöld pipa jelenik meg a felhasználó alapértelmezett könyvtár mellett. Ne felejtse el, válassza ki az összes olyan előfizetést, amely azt szeretné, hogy a felhasználó felügyelhető legyen.

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

Amikor elkészült, most látnia kell két felhasználó, beleértve az új társadminisztrátoraként személyazonosságát. Jelentkezzen ki a portálon.

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-the-new-users-password"></a>A bejelentkezés és az új jelszó módosítása
Jelentkezzen be az új felhasználó hozott létre.

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

Azonnal kérni fogja az új jelszót létrehozni.

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

Meg kell megkapja az, hogy a következőképpen néznek.

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a>Következő lépések
Most már használhatja az új Azure Active Directory-identitás használatára [Azure-erőforrás csoport sablonok](../articles/xplat-cli-azure-resource-manager.md).

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how to set them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
