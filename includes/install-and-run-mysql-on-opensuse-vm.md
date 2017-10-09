
1. tooescalate jogosultságok, típus:
   
        sudo -s
   
    Írja be a jelszót.
2. tooinstall MySQL közösségi Server edition, írja be:
   
        zypper install mysql-community-server
   
    Várjon, amíg a MySQL tölt le és telepít.
3. tooset MySQL toostart hello rendszer indításakor, írja be:
   
        insserv mysql
4. Ezzel a paranccsal manuálisan elindítani a hello MySQL démon (mysqld):
   
        rcmysql start
   
    toocheck hello állapotának hello MySQL démon, írja be:
   
        rcmysql status
   
    toostop hello MySQL démon, írja be:
   
        rcmysql stop
   
   > [!IMPORTANT]
   > A telepítés után hello MySQL gyökér szintű jelszó alapértelmezés szerint üres. Azt javasoljuk, hogy **mysql\_biztonságos\_telepítési**, egy parancsfájl, amelynek segítségével biztonságos MySQL. hello parancsfájl toochange hello MySQL gyökér szintű jelszavát kéri, távolítsa el a névtelen felhasználói fiókok, tiltsa le a távoli legfelső szintű bejelentkezéseket, távolítsa el az adatbázisok vizsgálati és töltse be újra a hello jogosultságokkal tábla. Azt javasoljuk, hogy fogadja a hívást Igen tooall ezek közül, és hello gyökér szintű jelszó módosítása.
   > 
   > 
5. Írja be a toorun hello parancsfájl MySQL telepítési parancsfájlt:
   
        mysql_secure_installation
6. Jelentkezzen be tooMySQL:
   
        mysql -u root -p
   
    Adja meg a hello MySQL gyökér szintű jelszavát (amely hello előző lépésben módosította), és meg fogja jelenik meg a kérdés ahol kiadhatja SQL utasítás toointeract hello adatbázissal.
7. új MySQL-felhasználó toocreate hello következő futtatásra hello **mysql >** parancssorba:
   
        CREATE USER 'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Vegye figyelembe, hello pontosvesszővel (;) hello hello végén sorok fontosságúak a befejezési hello parancsok.
8. egy adatbázis és a támogatás hello toocreate `mysqluser` felhasználó engedélyeit, probléma hello a következő parancsokat:
   
        CREATE DATABASE testdatabase;
        GRANT ALL ON testdatabase.* too'mysqluser'@'localhost' IDENTIFIED BY 'password';
   
    Vegye figyelembe, hogy adatbázis felhasználónevek és jelszavak csak által használt parancsfájlok csatlakozó toohello adatbázis.  Adatbázis-felhasználói fiók neve nem feltétlenül jelenti azt, tényleges felhasználói fiókok hello rendszeren.
9. az egy másik számítógépre, írja be a toolog:
   
        GRANT ALL ON testdatabase.* too'mysqluser'@'<ip-address>' IDENTIFIED BY 'password';
   
    Ha `ip-address` hello IP-cím hello számítógép, amelyen tooMySQL fog csatlakozni.
10. tooexit hello MySQL adatbázis felügyeleti segédprogram, írja be:
    
        quit

## <a name="add-an-endpoint"></a>Végpont hozzáadása
1. MySQL telepítése után egy végpont tooaccess MySQL tooconfigure távolról lesz szüksége. Jelentkezzen be toohello [a klasszikus Azure portálon][AzurePortal]. Kattintson a **virtuális gépek**, kattintson az új virtuális gép hello neve, majd **végpontok**.
2. Kattintson a **Hozzáadás** hello lap hello alján.
3. Protokoll "MySQL" nevű végpont hozzáadása **TCP**, és **nyilvános** és **titkos** túl portok beállítása "3306".
4. tooremotely csatlakoztassa toohello virtuális gépet a számítógépről, típus:
   
        mysql -u mysqluser -p -h <yourservicename>.cloudapp.net
   
    Például ebben az oktatóanyagban létrehozott hello csúcsszintű gép használja, ezt a parancsot:
   
        mysql -u mysqluser -p -h testlinuxvm.cloudapp.net

[MySQLDocs]: http://dev.mysql.com/doc/
[AzurePortal]: http://manage.windowsazure.com

[Image9]: ./media/install-and-run-mysql-on-opensuse-vm/LinuxVmAddEndpointMySQL.png
