### <a name="configure-a-dns-label-for-hello-public-ip-address"></a><span data-ttu-id="948c6-101">A DNS-címke hello nyilvános IP-cím konfigurálása</span><span class="sxs-lookup"><span data-stu-id="948c6-101">Configure a DNS Label for hello public IP address</span></span>

<span data-ttu-id="948c6-102">tooconnect toohello SQL Server adatbázis-kezelő a hello Internet, fontolja meg egy DNS-címke a nyilvános IP-cím létrehozása.</span><span class="sxs-lookup"><span data-stu-id="948c6-102">tooconnect toohello SQL Server Database Engine from hello Internet, consider creating a DNS Label for your public IP address.</span></span> <span data-ttu-id="948c6-103">IP-címe is elérheti, de hello DNS-címke létrehoz egy A rekordot, amely könnyebben tooidentify és kivonatok hello alapul szolgáló nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="948c6-103">You can connect by IP address, but hello DNS Label creates an A Record that is easier tooidentify and abstracts hello underlying public IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="948c6-104">DNS-címkére esetén nincs szükség, ha terv, tooonly csatlakozás toohello SQL Server-példány hello belül azonos virtuális hálózaton, vagy csak a helyi számítógépre.</span><span class="sxs-lookup"><span data-stu-id="948c6-104">DNS Labels are not required if you plan tooonly connect toohello SQL Server instance within hello same Virtual Network or only locally.</span></span>

<span data-ttu-id="948c6-105">toocreate DNS-címke, először válassza **virtuális gépek** hello portálon.</span><span class="sxs-lookup"><span data-stu-id="948c6-105">toocreate a DNS Label, first select **Virtual machines** in hello portal.</span></span> <span data-ttu-id="948c6-106">Válassza ki az SQL Server virtuális gép toobring a tulajdonságai megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="948c6-106">Select your SQL Server VM toobring up its properties.</span></span>

1. <span data-ttu-id="948c6-107">Hello virtuális gépek – áttekintés, válassza ki a **nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="948c6-107">In hello virtual machine overview, select your **Public IP address**.</span></span>

    ![nyilvános IP-cím](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. <span data-ttu-id="948c6-109">Bontsa ki a nyilvános IP-cím hello tulajdonságait, **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="948c6-109">In hello properties for your Public IP address, expand **Configuration**.</span></span>

1. <span data-ttu-id="948c6-110">Adjon meg egy DNS-címkenevet.</span><span class="sxs-lookup"><span data-stu-id="948c6-110">Enter a DNS Label name.</span></span> <span data-ttu-id="948c6-111">Ez a név egy A rekordot, amely közvetlenül lehet használt tooconnect tooyour SQL Server virtuális gép neve helyett az IP-cím szerint.</span><span class="sxs-lookup"><span data-stu-id="948c6-111">This name is an A Record that can be used tooconnect tooyour SQL Server VM by name instead of by IP Address directly.</span></span>

1. <span data-ttu-id="948c6-112">Kattintson a hello **mentése** gombra.</span><span class="sxs-lookup"><span data-stu-id="948c6-112">Click hello **Save** button.</span></span>

    ![dns-címke](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-toohello-database-engine-from-another-computer"></a><span data-ttu-id="948c6-114">Adatbázis-kezelő toohello Csatlakozás másik számítógépről</span><span class="sxs-lookup"><span data-stu-id="948c6-114">Connect toohello Database Engine from another computer</span></span>

1. <span data-ttu-id="948c6-115">Egy számítógép csatlakozik a toohello internet, nyissa meg az SQL Server Management Studio (SSMS).</span><span class="sxs-lookup"><span data-stu-id="948c6-115">On a computer connected toohello internet, open SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="948c6-116">Ha még nem rendelkezik az SQL Server Management Studio alkalmazással, [innen](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) letöltheti.</span><span class="sxs-lookup"><span data-stu-id="948c6-116">If you do not have SQL Server Management Studio, you can download it [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>

1. <span data-ttu-id="948c6-117">A hello **tooServer csatlakozás** vagy **tooDatabase motor csatlakozás** hello szerkesztése párbeszédpanelen **kiszolgálónév** érték.</span><span class="sxs-lookup"><span data-stu-id="948c6-117">In hello **Connect tooServer** or **Connect tooDatabase Engine** dialog box, edit hello **Server name** value.</span></span> <span data-ttu-id="948c6-118">Adja meg a hello IP-cím vagy hello virtuális gép (hello előző feladatban meghatározott) teljes DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="948c6-118">Enter hello IP address or full DNS name of hello virtual machine (determined in hello previous task).</span></span> <span data-ttu-id="948c6-119">Egy vessző hozzáadásával az SQL Server TCP-portját is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="948c6-119">You can also add a comma and provide SQL Server's TCP port.</span></span> <span data-ttu-id="948c6-120">Például: `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span><span class="sxs-lookup"><span data-stu-id="948c6-120">For example, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span></span>

1. <span data-ttu-id="948c6-121">A hello **hitelesítési** mezőben válassza **SQL Server-hitelesítés**.</span><span class="sxs-lookup"><span data-stu-id="948c6-121">In hello **Authentication** box, select **SQL Server Authentication**.</span></span>

1. <span data-ttu-id="948c6-122">A hello **bejelentkezési** mezőben, egy érvényes SQL-bejelentkezési hello típusnév.</span><span class="sxs-lookup"><span data-stu-id="948c6-122">In hello **Login** box, type hello name of a valid SQL login.</span></span>

1. <span data-ttu-id="948c6-123">A hello **jelszó** mezőbe, írja be a jelszót hello hello bejelentkezési azonosító.</span><span class="sxs-lookup"><span data-stu-id="948c6-123">In hello **Password** box, type hello password of hello login.</span></span>

1. <span data-ttu-id="948c6-124">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="948c6-124">Click **Connect**.</span></span>

    ![ssms connect](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)