
<br>

> [!NOTE]
> <span data-ttu-id="58fef-101">Egy rendszergazda felhasználónevet és jelszót kapott, nincs-e esély arra, hogy már rendelkezik munkahelyi vagy iskolai azonosítója (néha is egy *szervezeti Azonosítóval*).</span><span class="sxs-lookup"><span data-stu-id="58fef-101">If you were given a user name and password by an administrator, there's a good chance that you already have a work or school ID (also sometimes called an *organizational ID*).</span></span> <span data-ttu-id="58fef-102">Ha igen, azonnal megkezdheti toouse az Azure-fiók tooaccess Azure-erőforrások jelszó használata kötelezővé tehető.</span><span class="sxs-lookup"><span data-stu-id="58fef-102">If so, you can immediately begin toouse your Azure account tooaccess Azure resources that require one.</span></span> <span data-ttu-id="58fef-103">Ha talál meg, hogy nem használja ezeket az erőforrásokat, szükség lehet tooreturn toothis cikk segítségét.</span><span class="sxs-lookup"><span data-stu-id="58fef-103">If you find that you cannot use those resources, you may need tooreturn toothis article for help.</span></span> <span data-ttu-id="58fef-104">További információkért lásd: [fiókokat is használhatja a bejelentkezés](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) és [hogyan Azure-előfizetés, kapcsolódó tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span><span class="sxs-lookup"><span data-stu-id="58fef-104">For more information, see [Accounts that you can use for sign in](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) and [How an Azure subscription is related tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span></span>
> 
> 

<span data-ttu-id="58fef-105">hello lépésekre egyszerű.</span><span class="sxs-lookup"><span data-stu-id="58fef-105">hello steps are simple.</span></span> <span data-ttu-id="58fef-106">Az aláírt kell toolocate-identitás az hello a klasszikus Azure portálon, a Fedezze fel az alapértelmezett Azure Active Directory-tartományhoz, majd adja meg egy új felhasználó tooit Azure közös rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="58fef-106">You need toolocate your signed on identity in hello Azure classic portal, discover your default Azure Active Directory domain, and add a new user tooit as an Azure co-administrator.</span></span>

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a><span data-ttu-id="58fef-107">Keresse meg az alapértelmezett címtárban hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="58fef-107">Locate your default directory in hello Azure classic portal</span></span>
<span data-ttu-id="58fef-108">Első lépésként toohello naplózás [a klasszikus Azure portálon](https://manage.windowsazure.com) a személyes Microsoft-fiók identitással.</span><span class="sxs-lookup"><span data-stu-id="58fef-108">Start by logging in toohello [Azure classic portal](https://manage.windowsazure.com) with your personal Microsoft account identity.</span></span> <span data-ttu-id="58fef-109">Miután bejelentkezett, görgessen le a bal oldali hello hello kék panel, majd kattintson a **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="58fef-109">After you are logged in, scroll down hello blue panel on hello left side and click **ACTIVE DIRECTORY**.</span></span>

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

<span data-ttu-id="58fef-111">Először néhány a felhasználó adatainak keresése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="58fef-111">Let's start by finding some information about your identity in Azure.</span></span> <span data-ttu-id="58fef-112">Meg kell jelennie hello hello fő panelen a következő, megjeleníti, hogy rendelkezik-e egy alapértelmezett címtár hasonlót.</span><span class="sxs-lookup"><span data-stu-id="58fef-112">You should see something like hello following in hello main pane, showing that you have one default directory.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

<span data-ttu-id="58fef-113">Keressük meg néhány további információt.</span><span class="sxs-lookup"><span data-stu-id="58fef-113">Let's find out some more information about it.</span></span> <span data-ttu-id="58fef-114">Kattintson a során akkor kerülnek hello alapértelmezett címtár tulajdonságait hello alapértelmezett címtár sor.</span><span class="sxs-lookup"><span data-stu-id="58fef-114">Click hello default directory row, which brings you into hello default directory properties.</span></span>  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

<span data-ttu-id="58fef-115">tooview hello alapértelmezett tartomány nevét, kattintson a **TARTOMÁNYOK**.</span><span class="sxs-lookup"><span data-stu-id="58fef-115">tooview hello default domain name, click **DOMAINS**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

<span data-ttu-id="58fef-116">Itt meg kell tudni toosee, hogy hello Azure-fiók létrehozása után Azure Active Directoryban, amely a kivonat értéke (karakterlánc alapján generált szám) személyes alapértelmezett tartomány létrehozása a személyes azonosító onmicrosoft.com altartománya használja. Ez az új felhasználó most hozzáadjuk hello tartomány toowhich.</span><span class="sxs-lookup"><span data-stu-id="58fef-116">Here you should be able toosee that when hello Azure account was created, Azure Active Directory created a personal default domain that is a hash value (a number generated from a string of text) of your personal ID used as a subdomain of onmicrosoft.com. That's hello domain toowhich you will now add a new user.</span></span>

## <a name="creating-a-new-user-in-hello-default-domain"></a><span data-ttu-id="58fef-117">Új felhasználó létrehozása hello alapértelmezett tartományban</span><span class="sxs-lookup"><span data-stu-id="58fef-117">Creating a new user in hello default domain</span></span>
<span data-ttu-id="58fef-118">Kattintson a **felhasználók** , és tekintse meg a egy személyes fiók.</span><span class="sxs-lookup"><span data-stu-id="58fef-118">Click **USERS** and look for your single personal account.</span></span> <span data-ttu-id="58fef-119">Megjelenik a hello **forrás** oszlop, hogy ez egy **Microsoft-fiók**.</span><span class="sxs-lookup"><span data-stu-id="58fef-119">You should see in hello **SOURCED FROM** column that it is a **Microsoft account**.</span></span> <span data-ttu-id="58fef-120">Azt szeretnénk, ha a felhasználó az alapértelmezett toocreate. onmicrosoft.com Azure Active Directory-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="58fef-120">We want toocreate a user in your default .onmicrosoft.com Azure Active Directory domain.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

<span data-ttu-id="58fef-121">Fogjuk toofollow [ezeket az utasításokat](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) a következő néhány lépésben leírtak hello, de egy adott példát.</span><span class="sxs-lookup"><span data-stu-id="58fef-121">We're going toofollow [these instructions](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) in hello next few steps, but use a specific example.</span></span>

<span data-ttu-id="58fef-122">Hello a hello lap alján, kattintson **+ felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="58fef-122">At hello bottom of hello page, click **+ADD USER**.</span></span> <span data-ttu-id="58fef-123">A hello oldal jelenik meg, írja be a hello új felhasználónevet, és tegye hello **felhasználó típusa** egy **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="58fef-123">In hello page that appears, type hello new user name, and make hello **Type of User** a **New user in your organization**.</span></span> <span data-ttu-id="58fef-124">Ebben a példában a hello új felhasználónevet: `ahmet`.</span><span class="sxs-lookup"><span data-stu-id="58fef-124">In this example, hello new user name is `ahmet`.</span></span> <span data-ttu-id="58fef-125">Válassza ki azt a felderített hello alapértelmezett tartományt korábban hello tartományként ahmet tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="58fef-125">Select hello default domain that you discovered previously as hello domain for ahmet's email address.</span></span> <span data-ttu-id="58fef-126">Kattintson a Tovább nyílra hello befejezésekor.</span><span class="sxs-lookup"><span data-stu-id="58fef-126">Click hello next arrow when finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

<span data-ttu-id="58fef-127">Adja hozzá a további részleteket a Ahmet, de győződjön meg arról, hogy tooselect hello megfelelő **SZEREPKÖR** érték.</span><span class="sxs-lookup"><span data-stu-id="58fef-127">Add more details for Ahmet, but make sure tooselect hello appropriate **ROLE** value.</span></span> <span data-ttu-id="58fef-128">Egyszerű toouse **globális rendszergazda** toomake sure dolgot dolgozik, de ha egy kisebb szerepkör használatához, érdemes.</span><span class="sxs-lookup"><span data-stu-id="58fef-128">It's easy toouse **Global Admin** toomake sure things are working, but if you can use a lesser role, that's a good idea.</span></span> <span data-ttu-id="58fef-129">Ez a példa hello **felhasználói** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="58fef-129">This example uses hello **User** role.</span></span> <span data-ttu-id="58fef-130">(További: [rendszergazdai jogosultságokkal szerepkör](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Ne engedélyezze a multi-factor authentication, kivéve, ha azt szeretné, hogy az egyes naplókon műveletben toouse a többtényezős hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="58fef-130">(Find out more at [Administrator permissions by role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Do not enable multi-factor authentication unless you want toouse multifactor authentication for each log in operation.</span></span> <span data-ttu-id="58fef-131">Amikor végzett, kattintson a Tovább nyílra hello.</span><span class="sxs-lookup"><span data-stu-id="58fef-131">Click hello next arrow when you're finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

<span data-ttu-id="58fef-132">Kattintson a hello **létrehozása** toogenerate gombra, és ideiglenes jelszavakat Ahmet megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="58fef-132">Click hello **create** button toogenerate and display a temporary password for Ahmet.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

<span data-ttu-id="58fef-133">Másolja a hello felhasználói e-mail-címet, vagy használjon **jelszó része az E-mail KÜLDÉSE**.</span><span class="sxs-lookup"><span data-stu-id="58fef-133">Copy hello user name email address, or use **SEND PASSWORD IN EMAIL**.</span></span> <span data-ttu-id="58fef-134">Szüksége lesz hello információk toolog a hamarosan.</span><span class="sxs-lookup"><span data-stu-id="58fef-134">You'll need hello information toolog on shortly.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

<span data-ttu-id="58fef-135">Most már megtekintheti az új felhasználóhoz hello **Ahmet hello fejlesztői**, termékekre, az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="58fef-135">Now you should see hello new user, **Ahmet hello Developer**, sourced from Azure Active Directory.</span></span> <span data-ttu-id="58fef-136">Létrehozott hello új munkahelyi vagy iskolai azonosítása az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="58fef-136">You've created hello new work or school identity with Azure Active Directory.</span></span> <span data-ttu-id="58fef-137">Azonban ez az identitás még nem rendelkezik engedélyekkel toouse Azure erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="58fef-137">However, this identity does not yet have permissions toouse Azure resources.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

<span data-ttu-id="58fef-138">Ha **jelszó része az E-mail KÜLDÉSE**, a következő típusú e-mailek hello zajlik.</span><span class="sxs-lookup"><span data-stu-id="58fef-138">If you use **SEND PASSWORD IN EMAIL**, hello following kind of email is sent.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a><span data-ttu-id="58fef-139">Az előfizetések Azure közös rendszergazdai jogosultságokkal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="58fef-139">Adding Azure co-administrator rights for subscriptions</span></span>
<span data-ttu-id="58fef-140">A továbbiakban létre kell tooadd hello új felhasználó az előfizetés társadminisztrátoraként, hello új felhasználó bejelentkezhessen a kezelési portál toohello.</span><span class="sxs-lookup"><span data-stu-id="58fef-140">Now you need tooadd hello new user as a co-administrator of your subscription so hello new user can sign in toohello Management Portal.</span></span> <span data-ttu-id="58fef-141">Ez, hello bal alsó panelen kattintson a toodo **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="58fef-141">toodo this, in hello lower-left panel click **Settings**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

<span data-ttu-id="58fef-142">Hello fő beállítások területen, kattintson a **RENDSZERGAZDÁK** : hello felső, és csak a személyes Microsoft fiók azonosítóját megjelennie.</span><span class="sxs-lookup"><span data-stu-id="58fef-142">In hello main settings area, click **ADMINISTRATORS** at hello top and you should see only your personal Microsoft account identity.</span></span> <span data-ttu-id="58fef-143">Hello a hello lap alján, kattintson **+ Hozzáadás** toospecify társadminisztrátorának.</span><span class="sxs-lookup"><span data-stu-id="58fef-143">At hello bottom of hello page, click **+ADD** toospecify a co-administrator.</span></span> <span data-ttu-id="58fef-144">Itt adja meg a hello új felhasználó hozna létre, beleértve az alapértelmezett tartomány üdvözlő e-mail címét.</span><span class="sxs-lookup"><span data-stu-id="58fef-144">Here, enter hello email address of hello new user you had created, including your default domain.</span></span> <span data-ttu-id="58fef-145">Képernyőfelvételen látható módon hello tovább, egy zöld pipa hello alapértelmezett könyvtár a következő toohello felhasználó jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="58fef-145">As shown in hello next screenshot, a green check mark appears next toohello user for hello default directory.</span></span> <span data-ttu-id="58fef-146">Tooselect ne feledje, hogy szeretné-e a felhasználó toobe képes tooadminister hello előfizetések mindegyikét.</span><span class="sxs-lookup"><span data-stu-id="58fef-146">Remember tooselect all of hello subscriptions that you would like this user toobe able tooadminister.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

<span data-ttu-id="58fef-147">Amikor elkészült, most látnia kell két felhasználó, beleértve az új társadminisztrátoraként személyazonosságát.</span><span class="sxs-lookup"><span data-stu-id="58fef-147">When you are done, you should now see two users, including your new co-administrator identity.</span></span> <span data-ttu-id="58fef-148">Hello portál kijelentkezés.</span><span class="sxs-lookup"><span data-stu-id="58fef-148">Log out of hello portal.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a><span data-ttu-id="58fef-149">A bejelentkezés és hello új jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="58fef-149">Logging in and changing hello new user's password</span></span>
<span data-ttu-id="58fef-150">Jelentkezzen be hello új felhasználó hozott létre.</span><span class="sxs-lookup"><span data-stu-id="58fef-150">Log in as hello new user you created.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

<span data-ttu-id="58fef-151">Egy új jelszót kért toocreate azonnal lesz.</span><span class="sxs-lookup"><span data-stu-id="58fef-151">You will immediately be prompted toocreate a new password.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

<span data-ttu-id="58fef-152">Meg kell megkapja, következőhöz hasonló hello sikeres.</span><span class="sxs-lookup"><span data-stu-id="58fef-152">You should be rewarded with success that looks like hello following.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a><span data-ttu-id="58fef-153">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="58fef-153">Next steps</span></span>
<span data-ttu-id="58fef-154">Ezután már használhatja az új Azure Active Directory-identitás toouse [Azure-erőforrás csoport sablonok](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="58fef-154">You can now use your new Azure Active Directory identity toouse [Azure resource group templates](../articles/xplat-cli-azure-resource-manager.md).</span></span>

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
