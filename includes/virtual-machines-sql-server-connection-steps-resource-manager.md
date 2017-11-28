### <a name="configure-a-dns-label-for-the-public-ip-address"></a><span data-ttu-id="4cbd7-101">DNS-címke konfigurálása a nyilvános IP-címhez</span><span class="sxs-lookup"><span data-stu-id="4cbd7-101">Configure a DNS Label for the public IP address</span></span>

<span data-ttu-id="4cbd7-102">Ha az internetről szeretne csatlakozni az SQL Server-adatbázismotorhoz, érdemes létrehoznia egy DNS-címkét a nyilvános IP-címhez.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-102">To connect to the SQL Server Database Engine from the Internet, consider creating a DNS Label for your public IP address.</span></span> <span data-ttu-id="4cbd7-103">Csatlakozhat IP-cím alapján, de a DNS-címke létrehoz egy könnyebben azonosítható A rekordot, és kivonatolja az alapul szolgáló nyilvános IP-címet.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-103">You can connect by IP address, but the DNS Label creates an A Record that is easier to identify and abstracts the underlying public IP address.</span></span>

> [!NOTE]
> <span data-ttu-id="4cbd7-104">Nincs szükség DNS-címkére, ha csak az adott virtuális hálózaton belül vagy csak helyben szeretne csatlakozni az SQL Server-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-104">DNS Labels are not required if you plan to only connect to the SQL Server instance within the same Virtual Network or only locally.</span></span>

<span data-ttu-id="4cbd7-105">DNS-címke létrehozásához először válassza a **Virtuális gépek** elemet a portálon.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-105">To create a DNS Label, first select **Virtual machines** in the portal.</span></span> <span data-ttu-id="4cbd7-106">Jelölje ki az SQL Server rendszerű virtuális gépet a tulajdonságai megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-106">Select your SQL Server VM to bring up its properties.</span></span>

1. <span data-ttu-id="4cbd7-107">A virtuális gép áttekintésében válassza ki a **nyilvános IP-címét**.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-107">In the virtual machine overview, select your **Public IP address**.</span></span>

    ![nyilvános IP-cím](./media/virtual-machines-sql-server-connection-steps/rm-public-ip-address.png)

1. <span data-ttu-id="4cbd7-109">A Nyilvános IP-cím tulajdonságai között bontsa ki a **Konfiguráció** elemet.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-109">In the properties for your Public IP address, expand **Configuration**.</span></span>

1. <span data-ttu-id="4cbd7-110">Adjon meg egy DNS-címkenevet.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-110">Enter a DNS Label name.</span></span> <span data-ttu-id="4cbd7-111">Ez a név egy A rekord, amelynek használatával név szerint csatlakozhat az SQL Server rendszerű virtuális géphez az IP-cím megadásával való közvetlen csatlakozás helyett.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-111">This name is an A Record that can be used to connect to your SQL Server VM by name instead of by IP Address directly.</span></span>

1. <span data-ttu-id="4cbd7-112">Kattintson a **Mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-112">Click the **Save** button.</span></span>

    ![dns-címke](./media/virtual-machines-sql-server-connection-steps/rm-dns-label.png)

### <a name="connect-to-the-database-engine-from-another-computer"></a><span data-ttu-id="4cbd7-114">Csatlakozás az adatbázismotorhoz egy másik számítógépről</span><span class="sxs-lookup"><span data-stu-id="4cbd7-114">Connect to the Database Engine from another computer</span></span>

1. <span data-ttu-id="4cbd7-115">Nyissa meg az SQL Server Management Studio (SSMS) alkalmazást egy internethez csatlakozó számítógépen.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-115">On a computer connected to the internet, open SQL Server Management Studio (SSMS).</span></span> <span data-ttu-id="4cbd7-116">Ha még nem rendelkezik az SQL Server Management Studio alkalmazással, [innen](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) letöltheti.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-116">If you do not have SQL Server Management Studio, you can download it [here](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>

1. <span data-ttu-id="4cbd7-117">A **Kapcsolódás a kiszolgálóhoz** vagy a **Kapcsolódás az adatbázismotorhoz** párbeszédpanelen szerkessze a **Kiszolgáló neve** értéket.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-117">In the **Connect to Server** or **Connect to Database Engine** dialog box, edit the **Server name** value.</span></span> <span data-ttu-id="4cbd7-118">Adja meg a virtuális gép (az előző feladat során meghatározott) IP-címét vagy teljes DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-118">Enter the IP address or full DNS name of the virtual machine (determined in the previous task).</span></span> <span data-ttu-id="4cbd7-119">Egy vessző hozzáadásával az SQL Server TCP-portját is megadhatja.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-119">You can also add a comma and provide SQL Server's TCP port.</span></span> <span data-ttu-id="4cbd7-120">Például: `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-120">For example, `mysqlvmlabel.eastus.cloudapp.azure.com,1433`.</span></span>

1. <span data-ttu-id="4cbd7-121">A **Hitelesítés** mezőben válassza az **SQL Server-hitelesítés** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-121">In the **Authentication** box, select **SQL Server Authentication**.</span></span>

1. <span data-ttu-id="4cbd7-122">A **Bejelentkezés** szövegmezőbe írjon be egy érvényes SQL-bejelentkezési nevet.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-122">In the **Login** box, type the name of a valid SQL login.</span></span>

1. <span data-ttu-id="4cbd7-123">A **Jelszó** szövegmezőbe írja be a bejelentkezési jelszót.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-123">In the **Password** box, type the password of the login.</span></span>

1. <span data-ttu-id="4cbd7-124">Kattintson a **Connect** (Csatlakozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="4cbd7-124">Click **Connect**.</span></span>

    ![ssms connect](./media/virtual-machines-sql-server-connection-steps/rm-ssms-connect.png)