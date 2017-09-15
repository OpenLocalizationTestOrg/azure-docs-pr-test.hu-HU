
<br>

> [!NOTE]
> <span data-ttu-id="ea338-101">Egy rendszergazda felhasználónevet és jelszót kapott, nincs-e esély arra, hogy már rendelkezik munkahelyi vagy iskolai azonosítója (néha is egy *szervezeti Azonosítóval*).</span><span class="sxs-lookup"><span data-stu-id="ea338-101">If you were given a user name and password by an administrator, there's a good chance that you already have a work or school ID (also sometimes called an *organizational ID*).</span></span> <span data-ttu-id="ea338-102">Ha igen, azonnal megkezdheti az Azure-fiókjával jelszó használata kötelezővé tehető Azure-erőforrások elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="ea338-102">If so, you can immediately begin to use your Azure account to access Azure resources that require one.</span></span> <span data-ttu-id="ea338-103">Ha talál meg, hogy nem használja ezeket az erőforrásokat, szükség lehet a cikk segítséget való visszatéréshez.</span><span class="sxs-lookup"><span data-stu-id="ea338-103">If you find that you cannot use those resources, you may need to return to this article for help.</span></span> <span data-ttu-id="ea338-104">További információkért lásd: [fiókokat is használhatja a bejelentkezés](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) és [hogyan egy Azure-előfizetéshez az Azure AD kapcsolódó](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span><span class="sxs-lookup"><span data-stu-id="ea338-104">For more information, see [Accounts that you can use for sign in](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) and [How an Azure subscription is related to Azure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span></span>
> 
> 

<span data-ttu-id="ea338-105">Az egyszerű lépésekre.</span><span class="sxs-lookup"><span data-stu-id="ea338-105">The steps are simple.</span></span> <span data-ttu-id="ea338-106">Meg kell meghatározni az aláírt identitás a klasszikus Azure portálon Fedezze fel az alapértelmezett Azure Active Directory-tartomány, és új felhasználók hozzáadása az Azure közös rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="ea338-106">You need to locate your signed on identity in the Azure classic portal, discover your default Azure Active Directory domain, and add a new user to it as an Azure co-administrator.</span></span>

## <a name="locate-your-default-directory-in-the-azure-classic-portal"></a><span data-ttu-id="ea338-107">Keresse meg az alapértelmezett címtárat a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="ea338-107">Locate your default directory in the Azure classic portal</span></span>
<span data-ttu-id="ea338-108">Először jelentkezik be a [a klasszikus Azure portálon](https://manage.windowsazure.com) a személyes Microsoft-fiók identitással.</span><span class="sxs-lookup"><span data-stu-id="ea338-108">Start by logging in to the [Azure classic portal](https://manage.windowsazure.com) with your personal Microsoft account identity.</span></span> <span data-ttu-id="ea338-109">Miután bejelentkezett, görgessen lefelé a kék panel bal oldalán, majd kattintson a **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="ea338-109">After you are logged in, scroll down the blue panel on the left side and click **ACTIVE DIRECTORY**.</span></span>

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

<span data-ttu-id="ea338-111">Először néhány a felhasználó adatainak keresése az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ea338-111">Let's start by finding some information about your identity in Azure.</span></span> <span data-ttu-id="ea338-112">Meg kell jelennie a következőhöz a fő ablaktáblán látható, hogy rendelkezik-e egy alapértelmezett címtár.</span><span class="sxs-lookup"><span data-stu-id="ea338-112">You should see something like the following in the main pane, showing that you have one default directory.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

<span data-ttu-id="ea338-113">Keressük meg néhány további információt.</span><span class="sxs-lookup"><span data-stu-id="ea338-113">Let's find out some more information about it.</span></span> <span data-ttu-id="ea338-114">Kattintson a alapértelmezett directory sorra, amely azt az alapértelmezett címtár tulajdonságait.</span><span class="sxs-lookup"><span data-stu-id="ea338-114">Click the default directory row, which brings you into the default directory properties.</span></span>  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

<span data-ttu-id="ea338-115">Az alapértelmezett tartomány nevét megtekintéséhez kattintson **TARTOMÁNYOK**.</span><span class="sxs-lookup"><span data-stu-id="ea338-115">To view the default domain name, click **DOMAINS**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

<span data-ttu-id="ea338-116">Itt kell tudni, hogy az Azure-fiók létrehozása után Azure Active Directoryban, amely a kivonat értéke (karakterlánc alapján generált szám) személyes alapértelmezett tartomány létrehozása a személyes azonosító onmicrosoft.com altartománya használja.</span><span class="sxs-lookup"><span data-stu-id="ea338-116">Here you should be able to see that when the Azure account was created, Azure Active Directory created a personal default domain that is a hash value (a number generated from a string of text) of your personal ID used as a subdomain of onmicrosoft.com.</span></span> <span data-ttu-id="ea338-117">Ez a tartomány, amelyhez most hozzáadjuk egy új felhasználót.</span><span class="sxs-lookup"><span data-stu-id="ea338-117">That's the domain to which you will now add a new user.</span></span>

## <a name="creating-a-new-user-in-the-default-domain"></a><span data-ttu-id="ea338-118">Új felhasználó létrehozása az alapértelmezett tartományban</span><span class="sxs-lookup"><span data-stu-id="ea338-118">Creating a new user in the default domain</span></span>
<span data-ttu-id="ea338-119">Kattintson a **felhasználók** , és tekintse meg a egy személyes fiók.</span><span class="sxs-lookup"><span data-stu-id="ea338-119">Click **USERS** and look for your single personal account.</span></span> <span data-ttu-id="ea338-120">A kell megjelennie a **forrás** oszlop, hogy ez egy **Microsoft-fiók**.</span><span class="sxs-lookup"><span data-stu-id="ea338-120">You should see in the **SOURCED FROM** column that it is a **Microsoft account**.</span></span> <span data-ttu-id="ea338-121">Azt szeretnénk, hogy a felhasználó létrehozása az alapértelmezett. onmicrosoft.com Azure Active Directory-tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="ea338-121">We want to create a user in your default .onmicrosoft.com Azure Active Directory domain.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

<span data-ttu-id="ea338-122">Kövesse az oktatóanyagban módosítjuk [ezeket az utasításokat](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) a következő néhány lépésben leírtak, de használja az adott példát.</span><span class="sxs-lookup"><span data-stu-id="ea338-122">We're going to follow [these instructions](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) in the next few steps, but use a specific example.</span></span>

<span data-ttu-id="ea338-123">Kattintson a lap alján **+ felhasználó hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="ea338-123">At the bottom of the page, click **+ADD USER**.</span></span> <span data-ttu-id="ea338-124">Az oldal jelenik meg, írja be az új felhasználónevet, és ellenőrizze a **felhasználó típusa** egy **a szervezet új felhasználó**.</span><span class="sxs-lookup"><span data-stu-id="ea338-124">In the page that appears, type the new user name, and make the **Type of User** a **New user in your organization**.</span></span> <span data-ttu-id="ea338-125">Ebben a példában az új felhasználó neve: `ahmet`.</span><span class="sxs-lookup"><span data-stu-id="ea338-125">In this example, the new user name is `ahmet`.</span></span> <span data-ttu-id="ea338-126">Válassza ki az alapértelmezett tartomány, akkor a felderített korábban a tartományként ahmet tartozó e-mail cím.</span><span class="sxs-lookup"><span data-stu-id="ea338-126">Select the default domain that you discovered previously as the domain for ahmet's email address.</span></span> <span data-ttu-id="ea338-127">Kattintson a Tovább nyílra, amikor befejeződött.</span><span class="sxs-lookup"><span data-stu-id="ea338-127">Click the next arrow when finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

<span data-ttu-id="ea338-128">Adja hozzá a további részleteket a Ahmet, de győződjön meg arról, hogy válassza ki a megfelelő **SZEREPKÖR** érték.</span><span class="sxs-lookup"><span data-stu-id="ea338-128">Add more details for Ahmet, but make sure to select the appropriate **ROLE** value.</span></span> <span data-ttu-id="ea338-129">Könnyen használható legyen **globális rendszergazda** annak sure dolgot dolgozik, de ha egy kisebb szerepkör használatához, érdemes.</span><span class="sxs-lookup"><span data-stu-id="ea338-129">It's easy to use **Global Admin** to make sure things are working, but if you can use a lesser role, that's a good idea.</span></span> <span data-ttu-id="ea338-130">Ez a példa a **felhasználói** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="ea338-130">This example uses the **User** role.</span></span> <span data-ttu-id="ea338-131">(További: [rendszergazdai jogosultságokkal szerepkör](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Ne engedélyezze a multi-factor authentication, kivéve, ha a többtényezős hitelesítés használjanak minden egyes bejelentkezés műveletet szeretne.</span><span class="sxs-lookup"><span data-stu-id="ea338-131">(Find out more at [Administrator permissions by role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Do not enable multi-factor authentication unless you want to use multifactor authentication for each log in operation.</span></span> <span data-ttu-id="ea338-132">Amikor végzett, kattintson a Tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="ea338-132">Click the next arrow when you're finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

<span data-ttu-id="ea338-133">Kattintson a **létrehozása** készítése és ideiglenes jelszót Ahmet megjelenítése gombra.</span><span class="sxs-lookup"><span data-stu-id="ea338-133">Click the **create** button to generate and display a temporary password for Ahmet.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

<span data-ttu-id="ea338-134">Másolja a felhasználói e-mail-címet, vagy használjon **jelszó része az E-mail KÜLDÉSE**.</span><span class="sxs-lookup"><span data-stu-id="ea338-134">Copy the user name email address, or use **SEND PASSWORD IN EMAIL**.</span></span> <span data-ttu-id="ea338-135">Az információk hamarosan jelentkezzen be lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="ea338-135">You'll need the information to log on shortly.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

<span data-ttu-id="ea338-136">Most látnia kell az új felhasználó **Ahmet a fejlesztői**, termékekre, az Azure Active Directoryból.</span><span class="sxs-lookup"><span data-stu-id="ea338-136">Now you should see the new user, **Ahmet the Developer**, sourced from Azure Active Directory.</span></span> <span data-ttu-id="ea338-137">Létrehozta az új munkahelyi vagy iskolai azonosító az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="ea338-137">You've created the new work or school identity with Azure Active Directory.</span></span> <span data-ttu-id="ea338-138">Azonban ez az identitás még nincs jogosultsága Azure-erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="ea338-138">However, this identity does not yet have permissions to use Azure resources.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

<span data-ttu-id="ea338-139">Ha **jelszó része az E-mail KÜLDÉSE**, a következő típusú e-mailt küld.</span><span class="sxs-lookup"><span data-stu-id="ea338-139">If you use **SEND PASSWORD IN EMAIL**, the following kind of email is sent.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a><span data-ttu-id="ea338-140">Az előfizetések Azure közös rendszergazdai jogosultságokkal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="ea338-140">Adding Azure co-administrator rights for subscriptions</span></span>
<span data-ttu-id="ea338-141">A továbbiakban létre kell az új felhasználó hozzáadása az előfizetés társadminisztrátoraként, így az új felhasználó bejelentkezve is a kezelési portálon.</span><span class="sxs-lookup"><span data-stu-id="ea338-141">Now you need to add the new user as a co-administrator of your subscription so the new user can sign in to the Management Portal.</span></span> <span data-ttu-id="ea338-142">Ennek elvégzéséhez a bal alsó panelen kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="ea338-142">To do this, in the lower-left panel click **Settings**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

<span data-ttu-id="ea338-143">A fő beállítások területen kattintson a **RENDSZERGAZDÁK** felső és azt kell látnia csak a személyes Microsoft fiók azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="ea338-143">In the main settings area, click **ADMINISTRATORS** at the top and you should see only your personal Microsoft account identity.</span></span> <span data-ttu-id="ea338-144">Kattintson a lap alján **+ Hozzáadás** társadminisztrátorának megadásához.</span><span class="sxs-lookup"><span data-stu-id="ea338-144">At the bottom of the page, click **+ADD** to specify a co-administrator.</span></span> <span data-ttu-id="ea338-145">Itt adja meg a felhasználó e-mail címe az az új hozna létre, beleértve az alapértelmezett tartomány.</span><span class="sxs-lookup"><span data-stu-id="ea338-145">Here, enter the email address of the new user you had created, including your default domain.</span></span> <span data-ttu-id="ea338-146">Képernyőfelvételen látható módon a következő, egy zöld pipa jelenik meg a felhasználó alapértelmezett könyvtár mellett.</span><span class="sxs-lookup"><span data-stu-id="ea338-146">As shown in the next screenshot, a green check mark appears next to the user for the default directory.</span></span> <span data-ttu-id="ea338-147">Ne felejtse el, válassza ki az összes olyan előfizetést, amely azt szeretné, hogy a felhasználó felügyelhető legyen.</span><span class="sxs-lookup"><span data-stu-id="ea338-147">Remember to select all of the subscriptions that you would like this user to be able to administer.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

<span data-ttu-id="ea338-148">Amikor elkészült, most látnia kell két felhasználó, beleértve az új társadminisztrátoraként személyazonosságát.</span><span class="sxs-lookup"><span data-stu-id="ea338-148">When you are done, you should now see two users, including your new co-administrator identity.</span></span> <span data-ttu-id="ea338-149">Jelentkezzen ki a portálon.</span><span class="sxs-lookup"><span data-stu-id="ea338-149">Log out of the portal.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-the-new-users-password"></a><span data-ttu-id="ea338-150">A bejelentkezés és az új jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="ea338-150">Logging in and changing the new user's password</span></span>
<span data-ttu-id="ea338-151">Jelentkezzen be az új felhasználó hozott létre.</span><span class="sxs-lookup"><span data-stu-id="ea338-151">Log in as the new user you created.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

<span data-ttu-id="ea338-152">Azonnal kérni fogja az új jelszót létrehozni.</span><span class="sxs-lookup"><span data-stu-id="ea338-152">You will immediately be prompted to create a new password.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

<span data-ttu-id="ea338-153">Meg kell megkapja az, hogy a következőképpen néznek.</span><span class="sxs-lookup"><span data-stu-id="ea338-153">You should be rewarded with success that looks like the following.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a><span data-ttu-id="ea338-154">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ea338-154">Next steps</span></span>
<span data-ttu-id="ea338-155">Most már használhatja az új Azure Active Directory-identitás használatára [Azure-erőforrás csoport sablonok](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="ea338-155">You can now use your new Azure Active Directory identity to use [Azure resource group templates](../articles/xplat-cli-azure-resource-manager.md).</span></span>

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
