
1. <span data-ttu-id="f06a7-101">tooescalate jogosultságok, típus:</span><span class="sxs-lookup"><span data-stu-id="f06a7-101">tooescalate privileges, type:</span></span>
   
        sudo -s
   
    <span data-ttu-id="f06a7-102">Írja be a jelszót.</span><span class="sxs-lookup"><span data-stu-id="f06a7-102">Enter your password.</span></span>
2. <span data-ttu-id="f06a7-103">tooinstall MySQL közösségi Server edition, írja be:</span><span class="sxs-lookup"><span data-stu-id="f06a7-103">tooinstall MySQL Community Server edition, type:</span></span>
   
        zypper install mysql-community-server
   
    <span data-ttu-id="f06a7-104">Várjon, amíg a MySQL tölt le és telepít.</span><span class="sxs-lookup"><span data-stu-id="f06a7-104">Wait while MySQL downloads and installs.</span></span>
3. <span data-ttu-id="f06a7-105">tooset MySQL toostart hello rendszer indításakor, írja be:</span><span class="sxs-lookup"><span data-stu-id="f06a7-105">tooset MySQL toostart when hello system boots, type:</span></span>
   
        insserv mysql
4. <span data-ttu-id="f06a7-106">Ezzel a paranccsal manuálisan elindítani a hello MySQL démon (mysqld):</span><span class="sxs-lookup"><span data-stu-id="f06a7-106">Start hello MySQL daemon (mysqld) manually with this command:</span></span>
   
        rcmysql start
   
    <span data-ttu-id="f06a7-107">toocheck hello állapotának hello MySQL démon, írja be:</span><span class="sxs-lookup"><span data-stu-id="f06a7-107">toocheck hello status of hello MySQL daemon, type:</span></span>
   
        rcmysql status
   
    <span data-ttu-id="f06a7-108">toostop hello MySQL démon, írja be:</span><span class="sxs-lookup"><span data-stu-id="f06a7-108">toostop hello MySQL daemon, type:</span></span>
   
        rcmysql stop
   
   > [!IMPORTANT]
   > <span data-ttu-id="f06a7-109">A telepítés után hello MySQL gyökér szintű jelszó alapértelmezés szerint üres.</span><span class="sxs-lookup"><span data-stu-id="f06a7-109">After installation, hello MySQL root password is empty by default.</span></span> <span data-ttu-id="f06a7-110">Azt javasoljuk, hogy **mysql\_biztonságos\_telepítési**, egy parancsfájl, amelynek segítségével biztonságos MySQL.</span><span class="sxs-lookup"><span data-stu-id="f06a7-110">We recommended that you run **mysql\_secure\_installation**, a script that helps secure MySQL.</span></span> <span data-ttu-id="f06a7-111">hello parancsfájl toochange hello MySQL gyökér szintű jelszavát kéri, távolítsa el a névtelen felhasználói fiókok, tiltsa le a távoli legfelső szintű bejelentkezéseket, távolítsa el az adatbázisok vizsgálati és töltse be újra a hello jogosultságokkal tábla.</span><span class="sxs-lookup"><span data-stu-id="f06a7-111">hello script prompts you toochange hello MySQL root password, remove anonymous user accounts, disable remote root logins, remove test databases, and reload hello privileges table.</span></span> <span data-ttu-id="f06a7-112">Azt javasoljuk, hogy fogadja a hívást Igen tooall ezek közül, és hello gyökér szintű jelszó módosítása.</span><span class="sxs-lookup"><span data-stu-id="f06a7-112">We recommended that you answer yes tooall of these options and change hello root password.</span></span>
   > 
   > 
5. <span data-ttu-id="f06a7-113">Írja be a toorun hello parancsfájl MySQL telepítési parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="f06a7-113">Type this toorun hello script MySQL installation script:</span></span>
   
        mysql_secure_installation
6. <span data-ttu-id="f06a7-114">Jelentkezzen be tooMySQL:</span><span class="sxs-lookup"><span data-stu-id="f06a7-114">Log in tooMySQL:</span></span>
   
        mysql -u root -p
   
    <span data-ttu-id="f06a7-115">Adja meg a hello MySQL gyökér szintű jelszavát (amely hello előző lépésben módosította), és meg fogja jelenik meg a kérdés ahol kiadhatja SQL utasítás toointeract hello adatbázissal.</span><span class="sxs-lookup"><span data-stu-id="f06a7-115">Enter hello MySQL root password (which you changed in hello previous step) and you'll be presented with a prompt where you can issue SQL statements toointeract with hello database.</span></span>
7. <span data-ttu-id="f06a7-116">új MySQL-felhasználó toocreate hello következő futtatásra hello **mysql >** parancssorba:</span><span class="sxs-lookup"><span data-stu-id="f06a7-116">toocreate a new MySQL user, run hello following at hello **mysql>** prompt:</span></span>
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="f06a7-117">Vegye figyelembe, hello pontosvesszővel (;) hello hello végén sorok fontosságúak a befejezési hello parancsok.</span><span class="sxs-lookup"><span data-stu-id="f06a7-117">Note, hello semi-colons (;) at hello end of hello lines are crucial for ending hello commands.</span></span>
8. <span data-ttu-id="f06a7-118">egy adatbázis és a támogatás hello toocreate `mysqluser` felhasználó engedélyeit, probléma hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="f06a7-118">toocreate a database and grant hello `mysqluser` user permissions on it, issue hello following commands:</span></span>
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    <span data-ttu-id="f06a7-119">Vegye figyelembe, hogy adatbázis felhasználónevek és jelszavak csak által használt parancsfájlok csatlakozó toohello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="f06a7-119">Note that database user names and passwords are only used by scripts connecting toohello database.</span></span>  <span data-ttu-id="f06a7-120">Adatbázis-felhasználói fiók neve nem feltétlenül jelenti azt, tényleges felhasználói fiókok hello rendszeren.</span><span class="sxs-lookup"><span data-stu-id="f06a7-120">Database user account names do not necessarily represent actual user accounts on hello system.</span></span>
9. <span data-ttu-id="f06a7-121">az egy másik számítógépre, írja be a toolog:</span><span class="sxs-lookup"><span data-stu-id="f06a7-121">toolog in from another computer, type:</span></span>
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    <span data-ttu-id="f06a7-122">Ha `ip-address` hello IP-cím hello számítógép, amelyen tooMySQL fog csatlakozni.</span><span class="sxs-lookup"><span data-stu-id="f06a7-122">where `ip-address` is hello IP address of hello computer from which you will connect tooMySQL.</span></span>
10. <span data-ttu-id="f06a7-123">tooexit hello MySQL adatbázis felügyeleti segédprogram, írja be:</span><span class="sxs-lookup"><span data-stu-id="f06a7-123">tooexit hello MySQL database administration utility, type:</span></span>
    
        quit

## <a name="add-an-endpoint"></a><span data-ttu-id="f06a7-124">Végpont hozzáadása</span><span class="sxs-lookup"><span data-stu-id="f06a7-124">Add an endpoint</span></span>
1. <span data-ttu-id="f06a7-125">MySQL telepítése után egy végpont tooaccess MySQL tooconfigure távolról lesz szüksége.</span><span class="sxs-lookup"><span data-stu-id="f06a7-125">After MySQL is installed, you'll need tooconfigure an endpoint tooaccess MySQL remotely.</span></span> <span data-ttu-id="f06a7-126">Jelentkezzen be toohello [a klasszikus Azure portálon][AzurePortal].</span><span class="sxs-lookup"><span data-stu-id="f06a7-126">Log in toohello [Azure  classic portal][AzurePortal].</span></span> <span data-ttu-id="f06a7-127">Kattintson a **virtuális gépek**, kattintson az új virtuális gép hello neve, majd **végpontok**.</span><span class="sxs-lookup"><span data-stu-id="f06a7-127">Click **Virtual Machines**, click hello name of your new virtual machine, and then click **Endpoints**.</span></span>
2. <span data-ttu-id="f06a7-128">Kattintson a **Hozzáadás** hello lap hello alján.</span><span class="sxs-lookup"><span data-stu-id="f06a7-128">Click **Add** at hello bottom of hello page.</span></span>
3. <span data-ttu-id="f06a7-129">Protokoll "MySQL" nevű végpont hozzáadása **TCP**, és **nyilvános** és **titkos** túl portok beállítása "3306".</span><span class="sxs-lookup"><span data-stu-id="f06a7-129">Add an endpoint named "MySQL" with protocol **TCP**, and **Public** and **Private** ports set too"3306".</span></span>
4. <span data-ttu-id="f06a7-130">tooremotely csatlakoztassa toohello virtuális gépet a számítógépről, típus:</span><span class="sxs-lookup"><span data-stu-id="f06a7-130">tooremotely connect toohello virtual machine from your computer, type:</span></span>
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    <span data-ttu-id="f06a7-131">Például ebben az oktatóanyagban létrehozott hello csúcsszintű gép használja, ezt a parancsot:</span><span class="sxs-lookup"><span data-stu-id="f06a7-131">For example, using hello virual machine we created in this tutorial, type this command:</span></span>
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
