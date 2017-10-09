### <a name="create-a-new-logical-sql-server-in-hello-azure-portal"></a><span data-ttu-id="945a5-101">Hozzon létre egy új logikai SQL server hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="945a5-101">Create a new logical SQL server in hello Azure portal</span></span>

1. <span data-ttu-id="945a5-102">Kattintson az **Új** gombra, keresse meg a **logikai kiszolgáló** lehetőséget, majd nyomja le az **ENTER** billentyűt.</span><span class="sxs-lookup"><span data-stu-id="945a5-102">Click **New**, search **logical server**, and then hit **ENTER**.</span></span>

    ![logikai kiszolgáló keresése](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. <span data-ttu-id="945a5-104">Válassza az **SQL Server (logikai kiszolgáló)** elemet.</span><span class="sxs-lookup"><span data-stu-id="945a5-104">Select **SQL server (logical server)**</span></span> 

    ![logikai kiszolgáló kiválasztása](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. <span data-ttu-id="945a5-106">Kattintson a **létrehozása** tooopen hello új SQL-kiszolgáló (logikai kiszolgáló) panelen.</span><span class="sxs-lookup"><span data-stu-id="945a5-106">Click **Create** tooopen hello new SQL Server (logical server) blade.</span></span>

   <span data-ttu-id="945a5-107"><kbd>![nyissa meg a logikai kiszolgáló panelt](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logikai kiszolgálójuk paneljéről](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd></span><span class="sxs-lookup"><span data-stu-id="945a5-107"><kbd> ![open logical server blade](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![logical server blade](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd></span></span>
  
3. <span data-ttu-id="945a5-108">Hello SQL Server (a logikai kiszolgáló) panelen kiszolgáló neve mezőbe adjon meg egy érvényes nevet a hello új logikai kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="945a5-108">In hello SQL Server (logical server) blade's server name text box, provide a valid name for hello new logical server.</span></span> <span data-ttu-id="945a5-109">Zöld pipa jelzi, hogy érvényes nevet adott meg.</span><span class="sxs-lookup"><span data-stu-id="945a5-109">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![új kiszolgáló neve](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > <span data-ttu-id="945a5-111">hello az új kiszolgáló teljesen minősített név lesz < your_server_name >. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="945a5-111">hello fully qualified name for your new server will be <your_server_name>.database.windows.net.</span></span>
    >
    
4. <span data-ttu-id="945a5-112">Hello kiszolgáló rendszergazdai bejelentkezés szövegmezőben adjon meg egy SQL-hitelesítési bejelentkezési hello az ehhez a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="945a5-112">In hello Server admin login text box, provide a user name for hello SQL authentication login for this server.</span></span> <span data-ttu-id="945a5-113">Ehhez a bejelentkezéshez hello kiszolgáló fő bejelentkezéssel néven ismert.</span><span class="sxs-lookup"><span data-stu-id="945a5-113">This login is known as hello server principal login.</span></span> <span data-ttu-id="945a5-114">Zöld pipa jelzi, hogy érvényes nevet adott meg.</span><span class="sxs-lookup"><span data-stu-id="945a5-114">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![SQL-rendszergazda felhasználóneve](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. <span data-ttu-id="945a5-116">A hello **jelszó** és **jelszó megerősítése** szövegmezőkben hello server egyszerű bejelentkezési fiók adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="945a5-116">In hello **Password** and **Confirm password** text boxes, provide a password for hello server principal login account.</span></span> <span data-ttu-id="945a5-117">Zöld pipa jelzi, hogy érvényes jelszót adott meg.</span><span class="sxs-lookup"><span data-stu-id="945a5-117">A green check mark indicates that you have provided a valid password.</span></span>
    
    ![SQL-rendszergazda jelszava](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. <span data-ttu-id="945a5-119">Válasszon egy előfizetést, amelyben engedély toocreate objektummal rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="945a5-119">Select a subscription in which you have permission toocreate objects.</span></span>

    ![előfizetést](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. <span data-ttu-id="945a5-121">Hello erőforrás csoport szövegmezőben, válassza ki a **hozzon létre új** , hello erőforrás csoport szövegmezőben adja meg egy érvényes nevet az hello új erőforráscsoport (is használhat egy meglévő erőforráscsoportot, ha már létrehozott egy szolgáltatást).</span><span class="sxs-lookup"><span data-stu-id="945a5-121">In hello Resource group text box, select **Create new** and then, in hello resource group text box, provide a valid name for hello new resource group (you can also use an existing resource group if you have already created one for yourself).</span></span> <span data-ttu-id="945a5-122">Zöld pipa jelzi, hogy érvényes nevet adott meg.</span><span class="sxs-lookup"><span data-stu-id="945a5-122">A green check mark indicates that you have provided a valid name.</span></span>

    ![új erőforráscsoport](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. <span data-ttu-id="945a5-124">A hello **hely** szövegmezőben, válasszon ki egy adattípust center megfelelő tooyour helyre – például "Kelet-Ausztrália".</span><span class="sxs-lookup"><span data-stu-id="945a5-124">In hello **Location** text box, select a data center appropriate tooyour location - such as "Australia East".</span></span>
    
    ![kiszolgáló helye](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > <span data-ttu-id="945a5-126">hello jelölőnégyzetét **engedélyezése az azure szolgáltatások tooaccess server** nem módosítható ezen a panelen.</span><span class="sxs-lookup"><span data-stu-id="945a5-126">hello checkbox for **Allow azure services tooaccess server** cannot be changed on this blade.</span></span> <span data-ttu-id="945a5-127">Ez a beállítás a hello kiszolgáló tűzfal panelen módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="945a5-127">You can change this setting on hello server firewall blade.</span></span> <span data-ttu-id="945a5-128">További információkért lásd a [biztonságos használat első lépéseit](../articles/sql-database/sql-database-manage-servers-portal.md) ismertető témakört.</span><span class="sxs-lookup"><span data-stu-id="945a5-128">For more information, see [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md).</span></span>
    >
    
9. <span data-ttu-id="945a5-129">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="945a5-129">Click **Create**.</span></span>

    ![létrehozás gomb](./media/sql-data-warehouse-create-logical-server/create.png)

