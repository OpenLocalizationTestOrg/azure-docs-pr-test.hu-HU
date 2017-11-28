
1. <span data-ttu-id="eef6f-101">Az ilyen jogosultsággal, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-101">To escalate privileges, type:</span></span>
   
        sudo -s
   
    <span data-ttu-id="eef6f-102">Írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="eef6f-102">Enter your password.</span></span>
2. <span data-ttu-id="eef6f-103">Telepítse a MySQL közösségi Server verziót, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-103">To install MySQL Community Server edition, type:</span></span>
   
        zypper install mysql-community-server
   
    <span data-ttu-id="eef6f-104">Várjon, amíg a MySQL tölt le és telepít.</span><span class="sxs-lookup"><span data-stu-id="eef6f-104">Wait while MySQL downloads and installs.</span></span>
3. <span data-ttu-id="eef6f-105">MySQL induljon indul el, a rendszer beállításához, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-105">To set MySQL to start when the system boots, type:</span></span>
   
        insserv mysql
4. <span data-ttu-id="eef6f-106">Indítsa el a MySQL-démon (mysqld) manuálisan ezzel a paranccsal:</span><span class="sxs-lookup"><span data-stu-id="eef6f-106">Start the MySQL daemon (mysqld) manually with this command:</span></span>
   
        rcmysql start
   
    <span data-ttu-id="eef6f-107">A MySQL démon állapotának ellenőrzéséhez írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-107">To check the status of the MySQL daemon, type:</span></span>
   
        rcmysql status
   
    <span data-ttu-id="eef6f-108">A MySQL démon leállításához írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-108">To stop the MySQL daemon, type:</span></span>
   
        rcmysql stop
   
   > [!IMPORTANT]
   > <span data-ttu-id="eef6f-109">A telepítés után a MySQL gyökér szintű jelszó alapértelmezés szerint üres.</span><span class="sxs-lookup"><span data-stu-id="eef6f-109">After installation, the MySQL root password is empty by default.</span></span> <span data-ttu-id="eef6f-110">Azt javasoljuk, hogy **mysql\_biztonságos\_telepítési**, egy parancsfájl, amelynek segítségével biztonságos MySQL.</span><span class="sxs-lookup"><span data-stu-id="eef6f-110">We recommended that you run **mysql\_secure\_installation**, a script that helps secure MySQL.</span></span> <span data-ttu-id="eef6f-111">A parancsprogram kéri a MySQL gyökér szintű jelszó módosítása, távolítsa el a névtelen felhasználói fiókok, tiltsa le a távoli legfelső szintű bejelentkezéseket, távolítsa el az adatbázisok vizsgálati és töltse be újra a jogosultságok tábla.</span><span class="sxs-lookup"><span data-stu-id="eef6f-111">The script prompts you to change the MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload the privileges table.</span></span> <span data-ttu-id="eef6f-112">Azt javasoljuk, hogy ezek közül az összes igennel válaszol, és a gyökér szintű jelszó módosítása.</span><span class="sxs-lookup"><span data-stu-id="eef6f-112">We recommended that you answer yes to all of these options and change the root password.</span></span>
   > 
   > 
5. <span data-ttu-id="eef6f-113">Írja be a parancsfájl MySQL telepítési parancsfájl futtatására:</span><span class="sxs-lookup"><span data-stu-id="eef6f-113">Type this to run the script MySQL installation script:</span></span>
   
        mysql_secure_installation
6. <span data-ttu-id="eef6f-114">Jelentkezzen be a MySQL:</span><span class="sxs-lookup"><span data-stu-id="eef6f-114">Log in to MySQL:</span></span>
   
        mysql -u root -p
   
    <span data-ttu-id="eef6f-115">Adja meg a MySQL gyökér szintű jelszavát (amely módosította az előző lépésben), és akkor lesz jelenik meg a kérdés ahol kiadhatja kommunikál az adatbázis SQL-utasítások.</span><span class="sxs-lookup"><span data-stu-id="eef6f-115">Enter the MySQL root password (which you changed in the previous step) and you'll be presented with a prompt where you can issue SQL statements to interact with the database.</span></span>
7. <span data-ttu-id="eef6f-116">Hozzon létre egy új MySQL-felhasználó, futtassa a következő parancsot a **mysql >** parancssorba:</span><span class="sxs-lookup"><span data-stu-id="eef6f-116">To create a new MySQL user, run the following at the **mysql>** prompt:</span></span>
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="eef6f-117">Megjegyzés: a pontosvesszővel (;) a sorok végén fontosságúak a befejezési a parancsok.</span><span class="sxs-lookup"><span data-stu-id="eef6f-117">Note, the semi-colons (;) at the end of the lines are crucial for ending the commands.</span></span>
8. <span data-ttu-id="eef6f-118">Hozzon létre egy adatbázist, és biztosítson számára a `mysqluser` felhasználó engedélyeit adja ki a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="eef6f-118">To create a database and grant the `mysqluser` user permissions on it, issue the following commands:</span></span>
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="eef6f-119">Vegye figyelembe, hogy adatbázis felhasználónevek és jelszavak csak által használt parancsfájlok az adatbázishoz való kapcsolódás.</span><span class="sxs-lookup"><span data-stu-id="eef6f-119">Note that database user names and passwords are only used by scripts connecting to the database.</span></span>  <span data-ttu-id="eef6f-120">Adatbázis-felhasználói fiók neve nem feltétlenül jelenti azt, a rendszer a tényleges felhasználói fiókokat.</span><span class="sxs-lookup"><span data-stu-id="eef6f-120">Database user account names do not necessarily represent actual user accounts on the system.</span></span>
9. <span data-ttu-id="eef6f-121">Jelentkezzen be egy másik számítógépről, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-121">To log in from another computer, type:</span></span>
   
        GRANT ALL ON testdatabase.* TO 'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    <span data-ttu-id="eef6f-122">Ha `ip-address` az IP-cím, a számítógép, amelyen MySQL fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="eef6f-122">where `ip-address` is the IP address of the computer from which you will connect to MySQL.</span></span>
10. <span data-ttu-id="eef6f-123">Lépjen ki a MySQL-adatbázis felügyeleti segédprogram, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-123">To exit the MySQL database administration utility, type:</span></span>
    
        quit

## <a name="add-an-endpoint"></a><span data-ttu-id="eef6f-124">Végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eef6f-124">Add an endpoint</span></span>
1. <span data-ttu-id="eef6f-125">MySQL telepítése után meg kell MySQL távoli eléréséhez a végpont konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="eef6f-125">After MySQL is installed, you'll need to configure an endpoint to access MySQL remotely.</span></span> <span data-ttu-id="eef6f-126">Jelentkezzen be a [a klasszikus Azure portálon][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="eef6f-126">Log in to the [Azure  classic portal][AzurePortal].</span></span> <span data-ttu-id="eef6f-127">Kattintson a **virtuális gépek**, kattintson az új virtuális gép nevét, majd **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="eef6f-127">Click **Virtual Machines**, click the name of your new virtual machine, and then click **Endpoints**.</span></span>
2. <span data-ttu-id="eef6f-128">Kattintson a **Hozzáadás** az oldal alján.</span><span class="sxs-lookup"><span data-stu-id="eef6f-128">Click **Add** at the bottom of the page.</span></span>
3. <span data-ttu-id="eef6f-129">Protokoll "MySQL" nevű végpont hozzáadása **TCP**, és **nyilvános** és **titkos** portok "3306" értékre.</span><span class="sxs-lookup"><span data-stu-id="eef6f-129">Add an endpoint named "MySQL" with protocol **TCP**, and **Public** and **Private** ports set to "3306".</span></span>
4. <span data-ttu-id="eef6f-130">Távoli kapcsolódás a virtuális géphez a számítógépről, írja be:</span><span class="sxs-lookup"><span data-stu-id="eef6f-130">To remotely connect to the virtual machine from your computer, type:</span></span>
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    <span data-ttu-id="eef6f-131">Például a létrehozott ebben az oktatóanyagban csúcsszintű gép használja, ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="eef6f-131">For example, using the virual machine we created in this tutorial, type this command:</span></span>
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
