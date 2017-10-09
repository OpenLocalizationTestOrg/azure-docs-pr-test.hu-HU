
<!--
includes/sql-database-include-ip-address-22-v12portal.md

Latest Freshness check:  2016-03-21 , daleche.

As of circa 2015-09-04, hello following topics might include this include:
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## Server-level firewall rules

### Add a server-level firewall rule through hello new Azure portal
-->


1. <span data-ttu-id="a86e5-101">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/) , http://portal.azure.com/.</span><span class="sxs-lookup"><span data-stu-id="a86e5-101">Log in toohello [Azure portal](https://portal.azure.com/) at http://portal.azure.com/.</span></span>
2. <span data-ttu-id="a86e5-102">Kattintson a bal oldali szalagcím hello, **összes TALLÓZÁSA**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-102">In hello left banner, click **BROWSE ALL**.</span></span> <span data-ttu-id="a86e5-103">Hello **Tallózás** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-103">hello **Browse** blade is displayed.</span></span>
3. <span data-ttu-id="a86e5-104">Görgetéssel **SQL Server-kiszolgálók**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-104">Scroll and click **SQL servers**.</span></span> <span data-ttu-id="a86e5-105">Hello **SQL Server-kiszolgálók** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-105">hello **SQL servers** blade is displayed.</span></span>
   
    ![Az Azure SQL Database-kiszolgálóhoz az hello portálon található][b21-FindServerInPortal]
4. <span data-ttu-id="a86e5-107">Kényelmi okokból kattintson hello minimalizálása érdekében a korábbi hello levő **Tallózás** panelen.</span><span class="sxs-lookup"><span data-stu-id="a86e5-107">For convenience, click hello minimize control on hello earlier **Browse** blade.</span></span>
5. <span data-ttu-id="a86e5-108">A hello szűrő szövegmezőbe írja be a kiszolgáló nevének hello elindításához.</span><span class="sxs-lookup"><span data-stu-id="a86e5-108">In hello filter text box, start typing hello name of your server.</span></span> <span data-ttu-id="a86e5-109">A sor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-109">Your row is displayed.</span></span>
6. <span data-ttu-id="a86e5-110">Kattintson a kiszolgáló hello sor.</span><span class="sxs-lookup"><span data-stu-id="a86e5-110">Click hello row for your server.</span></span> <span data-ttu-id="a86e5-111">A kiszolgáló egy panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-111">A blade for your server is displayed.</span></span>
7. <span data-ttu-id="a86e5-112">A kiszolgáló paneljén kattintson **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-112">On your server blade, click **Settings**.</span></span> <span data-ttu-id="a86e5-113">Hello **beállítások** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-113">hello **Settings** blade is displayed.</span></span>
8. <span data-ttu-id="a86e5-114">Kattintson a **tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-114">Click **Firewall**.</span></span> <span data-ttu-id="a86e5-115">Hello **tűzfalbeállítások** panel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a86e5-115">hello **Firewall Settings** blade is displayed.</span></span>
   
    ![Kattintson a beállítások > tűzfal][b31-SettingsFirewallNavig]
9. <span data-ttu-id="a86e5-117">Kattintson a **adja hozzá ügyfél IP**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-117">Click **Add Client IP**.</span></span> <span data-ttu-id="a86e5-118">Adjon meg egy nevet az új szabály hello első szövegmezőbe írja be.</span><span class="sxs-lookup"><span data-stu-id="a86e5-118">Type in a name for your new rule into hello first text box.</span></span>
10. <span data-ttu-id="a86e5-119">Hello alacsony és magas írja be az IP-cím kívánt hello tartomány értékeinek tooenable.</span><span class="sxs-lookup"><span data-stu-id="a86e5-119">Type in hello low and high IP address values for hello range you want tooenable.</span></span>
    
    * <span data-ttu-id="a86e5-120">Hasznos toohave hello alacsony értékre end azok **.0** és magas hello **.255**.</span><span class="sxs-lookup"><span data-stu-id="a86e5-120">It can be handy toohave hello low value end with **.0** and hello high with **.255**.</span></span>
    
    ![Az IP-cím tartomány tooallow hozzáadása][b41-AddRange]
11. <span data-ttu-id="a86e5-122">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="a86e5-122">Click **Save**.</span></span>

<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->
