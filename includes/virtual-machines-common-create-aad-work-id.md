
<br>

> [!NOTE]
> Egy rendszergazda felhasználónevet és jelszót kapott, nincs-e esély arra, hogy már rendelkezik munkahelyi vagy iskolai azonosítója (néha is egy *szervezeti Azonosítóval*). Ha igen, azonnal megkezdheti toouse az Azure-fiók tooaccess Azure-erőforrások jelszó használata kötelezővé tehető. Ha talál meg, hogy nem használja ezeket az erőforrásokat, szükség lehet tooreturn toothis cikk segítségét. További információkért lásd: [fiókokat is használhatja a bejelentkezés](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) és [hogyan Azure-előfizetés, kapcsolódó tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).
> 
> 

hello lépésekre egyszerű. Az aláírt kell toolocate-identitás az hello a klasszikus Azure portálon, a Fedezze fel az alapértelmezett Azure Active Directory-tartományhoz, majd adja meg egy új felhasználó tooit Azure közös rendszergazdaként.

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a>Keresse meg az alapértelmezett címtárban hello a klasszikus Azure portálon
Első lépésként toohello naplózás [a klasszikus Azure portálon](https://manage.windowsazure.com) a személyes Microsoft-fiók identitással. Miután bejelentkezett, görgessen le a bal oldali hello hello kék panel, majd kattintson a **ACTIVE DIRECTORY**.

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

Először néhány a felhasználó adatainak keresése az Azure-ban. Meg kell jelennie hello hello fő panelen a következő, megjeleníti, hogy rendelkezik-e egy alapértelmezett címtár hasonlót.

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

Keressük meg néhány további információt. Kattintson a során akkor kerülnek hello alapértelmezett címtár tulajdonságait hello alapértelmezett címtár sor.  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

tooview hello alapértelmezett tartomány nevét, kattintson a **TARTOMÁNYOK**.

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

Itt meg kell tudni toosee, hogy hello Azure-fiók létrehozása után Azure Active Directoryban, amely a kivonat értéke (karakterlánc alapján generált szám) személyes alapértelmezett tartomány létrehozása a személyes azonosító onmicrosoft.com altartománya használja. Ez az új felhasználó most hozzáadjuk hello tartomány toowhich.

## <a name="creating-a-new-user-in-hello-default-domain"></a>Új felhasználó létrehozása hello alapértelmezett tartományban
Kattintson a **felhasználók** , és tekintse meg a egy személyes fiók. Megjelenik a hello **forrás** oszlop, hogy ez egy **Microsoft-fiók**. Azt szeretnénk, ha a felhasználó az alapértelmezett toocreate. onmicrosoft.com Azure Active Directory-tartományhoz.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

Fogjuk toofollow [ezeket az utasításokat](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) a következő néhány lépésben leírtak hello, de egy adott példát.

Hello a hello lap alján, kattintson **+ felhasználó hozzáadása**. A hello oldal jelenik meg, írja be a hello új felhasználónevet, és tegye hello **felhasználó típusa** egy **a szervezet új felhasználó**. Ebben a példában a hello új felhasználónevet: `ahmet`. Válassza ki azt a felderített hello alapértelmezett tartományt korábban hello tartományként ahmet tartozó e-mail cím. Kattintson a Tovább nyílra hello befejezésekor.

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

Adja hozzá a további részleteket a Ahmet, de győződjön meg arról, hogy tooselect hello megfelelő **SZEREPKÖR** érték. Egyszerű toouse **globális rendszergazda** toomake sure dolgot dolgozik, de ha egy kisebb szerepkör használatához, érdemes. Ez a példa hello **felhasználói** szerepkör. (További: [rendszergazdai jogosultságokkal szerepkör](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Ne engedélyezze a multi-factor authentication, kivéve, ha azt szeretné, hogy az egyes naplókon műveletben toouse a többtényezős hitelesítést. Amikor végzett, kattintson a Tovább nyílra hello.

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

Kattintson a hello **létrehozása** toogenerate gombra, és ideiglenes jelszavakat Ahmet megjelenítése.

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

Másolja a hello felhasználói e-mail-címet, vagy használjon **jelszó része az E-mail KÜLDÉSE**. Szüksége lesz hello információk toolog a hamarosan.

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

Most már megtekintheti az új felhasználóhoz hello **Ahmet hello fejlesztői**, termékekre, az Azure Active Directoryból. Létrehozott hello új munkahelyi vagy iskolai azonosítása az Azure Active Directoryban. Azonban ez az identitás még nem rendelkezik engedélyekkel toouse Azure erőforrásokat.

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

Ha **jelszó része az E-mail KÜLDÉSE**, a következő típusú e-mailek hello zajlik.

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a>Az előfizetések Azure közös rendszergazdai jogosultságokkal hozzáadása
A továbbiakban létre kell tooadd hello új felhasználó az előfizetés társadminisztrátoraként, hello új felhasználó bejelentkezhessen a kezelési portál toohello. Ez, hello bal alsó panelen kattintson a toodo **beállítások**.

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

Hello fő beállítások területen, kattintson a **RENDSZERGAZDÁK** : hello felső, és csak a személyes Microsoft fiók azonosítóját megjelennie. Hello a hello lap alján, kattintson **+ Hozzáadás** toospecify társadminisztrátorának. Itt adja meg a hello új felhasználó hozna létre, beleértve az alapértelmezett tartomány üdvözlő e-mail címét. Képernyőfelvételen látható módon hello tovább, egy zöld pipa hello alapértelmezett könyvtár a következő toohello felhasználó jelenik meg. Tooselect ne feledje, hogy szeretné-e a felhasználó toobe képes tooadminister hello előfizetések mindegyikét.

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

Amikor elkészült, most látnia kell két felhasználó, beleértve az új társadminisztrátoraként személyazonosságát. Hello portál kijelentkezés.

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a>A bejelentkezés és hello új jelszó módosítása
Jelentkezzen be hello új felhasználó hozott létre.

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

Egy új jelszót kért toocreate azonnal lesz.

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

Meg kell megkapja, következőhöz hasonló hello sikeres.

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a>Következő lépések
Ezután már használhatja az új Azure Active Directory-identitás toouse [Azure-erőforrás csoport sablonok](../articles/xplat-cli-azure-resource-manager.md).

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how tooset them up, please read http://aka.ms/Dhf67j.
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
