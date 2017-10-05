---
title: "Állítson be egy SQL Server virtuális gépet IPython Notebook kiszolgálóként |} Microsoft Docs"
description: "Állítsa be adatok tudományos rendelkező virtuális gép az SQL Server és IPython kiszolgáló."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 1fd6014a-d180-4558-b4eb-d9b5a331a99f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: xibingao;bradsev
ms.openlocfilehash: 8a151a6a15d4d000a774e3ec4e38bfa0e58ca33b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Azure SQL Server virtuális gép beállítása IPython Notebook-kiszolgálóként a speciális elemzésekhez
Ez a témakör azt ismerteti, hogyan felkészítse és konfigurálja az SQL Server virtuális gép tudományos adatok felhőalapú környezetben üzemel, használható. A Windows rendszerű virtuális gép támogatási eszközöket, például a IPython Notebook, Azure Tártallózó, és AzCopy, valamint más hasznos adatokat tudományos projektek a segédeszközök van konfigurálva. Az Azure Tártallózó és AzCopy, például módszereket biztosítanak az kényelmes feltölteni az adatokat az Azure blob storage a helyi számítógépről, vagy a blob storage-ból tölthetik le a helyi számítógépen.

A virtuális gép az Azure számos olyan rendszerkép található, amely tartalmazza a Microsoft SQL Server rendelkezik. Az adattárolási igényeinek megfelelő SQL Server virtuális gép lemezképének kiválasztása. Ajánlott lemezképeket a következők:

* SQL Server 2012 SP2 Enterprise a kis-és közepes méretű adatok mérete
* SQL Server 2012 SP2 Enterprise optimalizálva DataWarehousing munkaterhelések nagy a nagyon nagy méretű adatok mérete
  
  > [!NOTE]
  > SQL Server 2012 SP2 Enterprise kép **nem tartalmazza az adatlemezt**. Akkor adja hozzá és/vagy csatlakoztassa egy vagy több virtuális merevlemezeket az adatok tárolásához. Egy Azure virtuális gép létrehozásakor rendelkezik egy a C meghajtó leképezve az operációs rendszerhez és lemezt ideiglenes leképezve a D meghajtóra. Ne használja a D meghajtó adatainak tárolásához. Foglalja, csak az átmeneti tárolási biztosít. Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.
  > 
  > 

## <a name="Provision"></a>A klasszikus Azure portál csatlakozni, és az SQL Server rendszerű virtuális gép
1. Jelentkezzen be a [a klasszikus Azure portálon](http://manage.windowsazure.com/) a fiókjával.
   Ha nem rendelkezik Azure-fiókkal, az [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/) biztosít.
2. Kattintson a klasszikus Azure portálon, a weblap bal alsó **+ új**, kattintson a **számítási**, kattintson a **virtuális gép**, és kattintson a **FROM GYŰJTEMÉNY**.
3. Az a **hozzon létre egy virtuális gépet** lapon válasszon ki egy virtuális gép lemezképet tartalmazó SQL Server-adatok igényei szerint, és kattintson a jobb alsó sarkában az oldal tovább nyílra. A támogatott SQL Server-rendszerképeit Azure a legfrissebb információkért lásd: [Ismerkedés az SQL Server Azure virtuális gépek](http://go.microsoft.com/fwlink/p/?LinkId=294720) a témakör a [SQL Server Azure virtuális gépek](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentációkészlet.
   
   ![Válassza ki az SQL Server rendszerű virtuális Géphez][1]
4. Az első **virtuálisgép-konfiguráció** lapján adja meg a következő információkat:
   
   * Adjon meg egy **virtuális gép neve**.
   * Az a **új FELHASZNÁLÓNEVET** mezőbe, a virtuális gép helyi rendszergazdai fiók egyedi felhasználói típusnév.
   * Az a **új jelszó** mezőbe írjon be egy erős jelszót. További információkért lásd az [erős jelszavak](http://msdn.microsoft.com/library/ms161962.aspx) létrehozását ismertető cikket.
   * Az a **jelszó megerősítése** mezőbe írja be ismét a jelszót.
   * Válassza ki a megfelelő **mérete** a legördülő listából.
     
     > [!NOTE]
     > A virtuális gép méretét a kiépítés során van megadva: A2 érték a termelési számítási feladatokhoz ajánlott legkisebb méretét. A virtuális gép ajánlott minimális mérete A3 használata esetén az SQL Server Enterprise Edition. Válassza ki a A3 vagy újabb SQL Server Enterprise Edition használatakor. Válassza ki a A4 tranzakciós munkaterhelések lemezképek SQL Server 2012 vagy 2014 Enterprise optimalizált használata esetén.
     > Válassza ki a A7 Data Warehousing munkaterhelések lemezképek SQL Server 2012 vagy 2014 Enterprise optimalizált használata esetén. A kiválasztott méret konfigurálhatja az adatlemezek számának korlátozása. A rendelkezésre álló virtuálisgép-méretek és a virtuális gépekhez csatolható adatlemezek száma legfrissebb információkért lásd: [Azure virtuálisgép-méretek](http://msdn.microsoft.com/library/azure/dn197896.aspx). Díjszabási információkért lásd: [virtuális Macines árképzési](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Kattintson a Tovább nyílra alsó folytatja.
   
   ![Virtuálisgép-konfiguráció][2]
5. A második **virtuálisgép-konfiguráció** lapon, hálózati, tárolási és rendelkezésre állás erőforrásait:
   
   * Az a **Felhőszolgáltatás** válassza **hozzon létre egy új felhőalapú szolgáltatás**.
   * Az a **felhőalapú szolgáltatás DNS-név** , hogy ő maga befejeződne formátumban nevét, adja meg egy DNS-nevet az Ön által választott első része **TESTNAME.cloudapp.net**
   * Az a **régió/AFFINITÁS csoport/virtuális hálózati** mezőben válasszon ki egy régiót, ahol a virtuális lemezképet fog üzemelni.
   * Az a **Tárfiók**, válasszon egy meglévő tárfiókot, vagy válasszon ki egy automatikusan létrehozott másikat.
   * Az a **rendelkezésre ÁLLÁSI csoport** mezőben válassza **(nincs)**.
   * Olvassa el és fogadja el a díjszabási információkat.
6. A a **VÉGPONTOK** területen kattintson az üres legördülő **neve**, és válassza ki **MSSQL** írja be az adatbázismotor példánya portjának száma (**1433-as** az alapértelmezett példány).
7. Az SQL Server virtuális gép is lehetnek egy IPython Notebook kiszolgálóra, amely egy későbbi lépésben lesz konfigurálva.
   Adjon hozzá egy új végpontot adja meg a portot használ a IPython Notebook kiszolgálóhoz. Írjon be egy nevet a **neve** oszlop, válassza ki a portszámot az Ön által választott, a nyilvános port és 9999 a magánhálózati port.
   
   Kattintson a Tovább nyílra alsó folytatja.
   
   ![Válassza ki az MSSQL és IPython portok][3]
8. Fogadja el az alapértelmezett **telepítése Virtuálisgép-ügynök** beállítás be van jelölve, majd kattintson a a jelet a jobb alsó sarkában a varázsló befejezi a virtuális gép létesítésének folyamatát kell használnia.
   
   `![Virtuális gép végső beállítások][4]
9. Várjon, amíg a Azure előkészíti a virtuális gép. Várt végezze el a virtuális gép állapota:
   
   * (Kiépítése) indítása
   * Leállítva
   * (Kiépítése) indítása
   * Futó (kiépítése)
   * Fut

## <a name="RemoteDesktop"></a>Nyissa meg a virtuális gép használata a távoli asztal és a telepítés befejezéséhez
1. Ha kiépítése befejeződött, kattintson az IRÁNYÍTÓPULT-oldalon Ugrás a virtuális gép nevére. Kattintson a lap alján **Connect**.
2. Válassza ki a Windows távoli asztal programmal rpd fájl megnyitásához (`%windir%\system32\mstsc.exe`).
3. : A **Windows biztonsági** párbeszédpanelen adja meg a jelszót a helyi rendszergazdai fiók, amelyet a korábbi lépésben.
   (Előfordulhat, hogy kérni ellenőrizze a hitelesítő adatokat a virtuális gép.)
4. Jelentkezzen be a virtuális gépre, először néhány folyamat befejeződik, beleértve a telepítés asztali, Windows-frissítések, és a Windows kezdeti konfigurációs feladatok (sysprep) megvalósításának esetleg. A Windows sysprep befejezése után az SQL Server telepítése konfigurációs feladatokat hajtja végre. Ezek a feladatok okozhat néhány percet várnia, amíg a művelet befejeződik. `SELECT @@SERVERNAME`nem adhat vissza a megfelelő nevet, mindaddig, amíg az SQL Server telepítése befejeződik, és az SQL Server Management Studio nem lehet a start lap visable.

A virtuális géphez a Windows távoli asztal csatlakozás után a virtuális gép bármely más számítógépre hasonlóan működik. Csatlakozás az SQL Server Management Studio (a virtuális gépen futó) az SQL Server alapértelmezett példányát a szokásos módon.

## <a name="InstallIPython"></a>IPython jegyzetfüzet és egyéb támogató eszközök telepítése
Az új SQL Server virtuális gép IPython Notebook kiszolgálóként, és további támogató eszközök telepítése, ilyen AzCopy, Azure Tártallózó, hasznos adatok tudományos Python-csomagokat, és mások konfigurálásához Önnek egy különös testreszabási parancsfájl valósul meg. Telepítése:

1. Kattintson a jobb gombbal a **Windows Start** ikonra, majd **parancssor (rendszergazda)**
2. Másolja a következő parancsokat, és illessze be a parancssorba.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. Amikor a rendszer kéri, adja meg egy tetszőleges jelszót a IPython Notebook számára.
4. A testreszabási parancsfájl automatizálja a telepítés utáni számos eljárást, többek között:
    * Telepítési és beállítási IPython Notebook kiszolgáló
    * TCP-portok megnyitása a Windows tűzfal a korábban létrehozott végpontok:
    * A távoli SQL Server-kapcsolathoz
    * A kiszolgáló távoli kapcsolatot IPython Notebook
    * A minta IPython jegyzetfüzet és SQL-parancsfájlok beolvasása
    * Letölti és telepíti a hasznos adatok tudományos Python-csomagok
    * Letöltése és telepítése az Azure-eszközök például az AzCopy és Azure Tártallózó  
    <br>
5. Előfordulhat, hogy hozzáférési és IPython Notebook használatával URL-cím a helyi vagy távoli böngészők-ről futtatva `https://<virtual_machine_DNS_name>:<port>`, ahol a virtuális gép kiépítése során kiválasztott IPython nyilvános portot.
6. IPython Notebook server a háttérben szolgáltatásként fut, és automatikusan újraindul a virtuális gép újraindításakor.

## <a name="Optional"></a>Adatlemez csatolása igény szerint
Ha a Virtuálisgép-lemezkép nem tartalmazza az adatlemezek, azaz, a C meghajtó (operációsrendszer-lemez) és a D meghajtó (ideiglenes lemez), lemezek kell hozzáadnia az adatokat egy vagy több lemezt az adatok tárolásához. A Virtuálisgép-lemezkép az SQL Server 2012 SP2 Enterprise optimalizálva DataWarehousing munkaterhelések előre konfigurált SQL Server adatainak és naplókönyvtárainak fájlok további lemezzel rendelkezik.

> [!NOTE]
> Ne használja a D meghajtó adatainak tárolásához. Foglalja, csak az átmeneti tárolási biztosít. Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.
> 
> 

További adatlemezt csatolni, kövesse a témakörben ismertetett [hogyan lehet adatlemezt csatolni egy Windows rendszerű virtuális gép](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), amely végigvezeti Önt keresztül:

1. Üres lemez(ek) csatolása a virtuális gép kiépítése a korábbi lépések
2. A virtuális gép új lemez(ek) inicializálása

## <a name="SSMS"></a>SQL Server Management Studio csatlakozhat, és a vegyes módú hitelesítés engedélyezése
Az SQL Server Adatbázismotor nem használhatja a Windows-hitelesítést egy tartománykörnyezet nélkül. Ha az Adatbázismotorhoz egy másik számítógépről szeretne csatlakozni, konfigurálja az SQL Servert vegyes módú hitelesítéshez. A vegyes módú hitelesítés az SQL Server-hitelesítést és a Windows-hitelesítést is lehetővé teszi. SQL-hitelesítési mód van szükség ahhoz, hogy közvetlenül az SQL Server virtuális gép adatbázisból származó adatok a [Azure Machine Learning Studio](https://studio.azureml.net) az adatok importálása modullal.

1. A virtuális géphez csatlakozik a távoli asztal használatával, használja a Windows **keresési** ablaktábla és a típus **SQL Server Management Studio** (SMSS). Ide kattintva elindíthatja az SQL Server Management Studio (SSMS). Érdemes lehet mutató parancsikont az asztalon későbbi használatra SSMS.
   
   ![Indítsa el a szolgáltatáshoz az SSMS][5]
   
   A Management Studio első megnyitásakor annak létre kell hoznia a felhasználó Management Studio-környezetét. Ez eltarthat néhány pillanatig.
2. Megnyitásakor, a Management Studio megadja a **kapcsolódás a kiszolgálóhoz** párbeszédpanel megnyitásához. Az a **kiszolgálónév** mezőbe írja be az Object Explorer az adatbázismotorhoz való csatlakozáshoz a virtuális gép nevét.
   (A virtuális gép neve helyett használhatja **(helyi)** vagy egy egyetlen időszak, mint a **kiszolgálónév**. Válassza ki **Windows-hitelesítés**, és hagyja  ***a\_VM\_neve*\\a\_helyi\_rendszergazda** a a **felhasználónév** mezőbe. Kattintson a **Connect** (Csatlakozás) gombra.
   
   ![Csatlakozás kiszolgálóhoz][6]
   
   <br>
   
   > [!TIP]
   > Az SQL Server hitelesítési mód a Windows beállításjegyzék-kulcs módosítás vagy az SQL Server Management Studio használatával módosíthatja. Használja a beállításjegyzék-módosítás-hitelesítés üzemmód módosítása, indítsa el a **új lekérdezés** , és hajtsa végre a következő parancsfájlt:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    Az SQL Server management Studio használatával-hitelesítés üzemmód módosítása:

1. A **SQL Server Management Studio Object Explorerben**, kattintson a jobb gombbal a (a virtuális gép neve) SQL Server-példány nevét, és kattintson a **tulajdonságok**.
   
   ![Kiszolgáló tulajdonságai][7]
2. A **Biztonság** lap **Kiszolgálóhitelesítés** területén válassza az **SQL Server és Windows-hitelesítési mód** lehetőséget, majd kattintson az **OK** gombra.
   
   ![A hitelesítési mód kiválasztása][8]
3. Az a **SQL Server Management Studio** párbeszédpanel, kattintson a **OK** lehet visszaigazolni az a követelmény, indítsa újra az SQL Server.
4. A **Object Explorer**, kattintson a jobb gombbal a kiszolgáló, és kattintson a **indítsa újra a**. (Ha az SQL Server Agent fut, azt is újra kell indítani.)
   
   ![Újraindítás][9]
5. Az a **SQL Server Management Studio** párbeszédpanel, kattintson a **Igen** gombra kattintva elfogadja, hogy szeretné-e indítania az SQL Server.

## <a name="Logins"></a>SQL Server-hitelesítési bejelentkezési létrehozása
Ha az Adatbázismotorhoz egy másik számítógépről szeretne csatlakozni, létre kell hoznia legalább egy SQL Server hitelesítési bejelentkezést.  

Új SQL Server bejelentkezési programozott módon hozható létre vagy az SQL Server Management Studio használatával. Hozzon létre új rendszergazda felhasználó SQL-hitelesítéshez a programozott módon, indítsa el a **új lekérdezés** , és hajtsa végre a következő parancsfájlt. Cserélje le < új felhasználónevet\> és < Új jelszó\> a választható *felhasználónév* és *jelszó*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


(A minta kód kikapcsolja házirend ellenőrzése és a jelszó lejárati) szükség szerint, állítsa be a jelszóházirend. További információ az SQL Server-bejelentkezésekről: [Bejelentkezés létrehozása](http://msdn.microsoft.com/library/aa337562.aspx).  

Az SQL Server Management Studio használatával új SQL Server bejelentkezési azonosítók létrehozása:

1. A **SQL Server Management Studio Object Explorerben**, bontsa ki a mappát, amelyben szeretné létrehozni az új bejelentkezési adatait a server-példány.
2. Kattintson a jobb gombbal a **biztonsági** mappára, mutasson a **új**, és válassza ki **bejelentkezési...** .
   
   ![Új bejelentkezés][10]
3. A **Bejelentkezés – Új** párbeszédpanel **Általános** lapján adja meg az új felhasználó nevét a **Bejelentkezési név** mezőben.
4. Kattintson az **SQL Server-hitelesítés** lehetőségre.
5. A **Jelszó** mezőbe írja be az új felhasználó jelszavát. Adja meg újra a jelszót a **Jelszó megerősítése** mezőben.
6. Házirend-beállításokban jelszó összetettségét és kényszerítési megköveteléséhez **jelszó házirend** (ajánlott). Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
7. Házirend-beállításokban jelszó lejáratának megköveteléséhez **kényszerítése a jelszó lejárati** (ajánlott). Kényszerítése jelszóházirend meg kell adni ahhoz, hogy ezt a jelölőnégyzetet. Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
8. Válassza ki, hogy a felhasználó számára hozzon létre egy új jelszót az első alkalommal való használatakor a bejelentkezés után, **kell változtatni a jelszót a következő bejelentkezésekor** (ajánlott, ha ehhez a bejelentkezéshez van, hogy valaki más használja. Ha a bejelentkezés saját használatra, ne válassza ezt a lehetőséget.) Kényszerítése jelszólejárati meg kell adni ahhoz, hogy ezt a jelölőnégyzetet. Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
9. Az **Alapértelmezett adatbázis** listából válasszon ki egy alapértelmezett adatbázist a bejelentkezéshez. Az alapértelmezett beállítás a **master**. Ha még nem hozott létre egy felhasználói adatbázist, hagyja meg a **master** beállítást.
10. Az a **alapértelmezett nyelv** listában, hagyja **alapértelmezett** értékeként.
    
    ![Bejelentkezési tulajdonságok][11]
11. Ha ez az első létrehozott bejelentkezése, célszerű lehet SQL Server-rendszergazdaként megadnia a bejelentkezést. Ebben az esetben a **Kiszolgálói szerepkörök** lapon jelölje be a **sysadmin** jelölőnégyzetet.
    
    > [!IMPORTANT]
    > A sysadmin rögzített kiszolgálói szerepkör tagjai teljes vezérlést kapnak az Adatbázismotor felett. Biztonsági okokból ügyeljen korlátozására, ez a szerepkör tagjának kell lennie.
    > 
    > 
    
    ![sysadmin][12]
12. Kattintson az OK gombra.

## <a name="DNS"></a>Határozza meg a virtuális gép DNS-neve
Ha egy másik számítógépről szeretne az SQL Server Adatbázismotorhoz csatlakozni, ismernie kell a virtuális gép Tartománynév-szolgáltatójának (DNS) nevét.

(Az internet ezzel a névvel azonosítja a virtuális gépet. Az IP-címet is használhatja, azonban az megváltozhat, ha az Azure erőforrásokat helyez át redundancia vagy karbantartás miatt. A DNS-név mindig stabil, mivel átirányítható egy új IP-címre.)

1. A klasszikus Azure-portálon (vagy az előző lépésben), válassza ki a **virtuális gépek**.
2. Az a **VIRTUÁLISGÉP-példányok** lap a **DNS-név** oszlop, a Keresés és a másolása a virtuális gépet, amely akkor jelenik meg a DNS-nevét utasításnak **http://**. (A felhasználói felülete nem jelenik meg a teljes nevet, de a kattintson a jobb gombbal, és válassza a másolás.)

## <a name="cde"></a>Csatlakozás az adatbázismotorhoz való csatlakozáshoz egy másik számítógépről
1. Nyissa meg az SQL Server Management Studio alkalmazást egy internethez csatlakozó számítógépen.
2. A a **kapcsolódás a kiszolgálóhoz** vagy **kapcsolódás az adatbázismotorhoz** párbeszédpanel a **kiszolgálónév** formátumban adja meg a DNS-neve, a virtuális gép (az előző feladatban meghatározott) és egy nyilvános végpontot portszámot *DNSName, portszám* például **tutorialtestVM.cloudapp.net,57500**.
3. A **Hitelesítés** mezőben válassza az **SQL Server-hitelesítés** lehetőséget.
4. A **Bejelentkezés** mezőbe írja be egy korábbi feladatban létrehozott bejelentkezés nevét.
5. A **Jelszó** mezőbe írja be a korábbi feladatban létrehozott bejelentkezés jelszavát.
6. Kattintson a **Connect** (Csatlakozás) gombra.

## <a name="amlconnect"></a>Csatlakozzon az adatbázismotorhoz való csatlakozáshoz az Azure gépi tanulás
Az Team tudományos folyamat későbbi szakaszában fogja használni a [Azure Machine Learning Studióban](https://studio.azureml.net) létrehozásához és telepítéséhez a machine learning modellek. Az SQL Server virtuális gép adatbázis betöltési közvetlenül az Azure Machine Learning képzési vagy pontozási, használja a **és adatokat importálhat** az új modul [Azure Machine Learning Studio](https://studio.azureml.net) kipróbálásához. Ez a témakör további részleteket a csapat az tudományos folyamata útmutatóban található hivatkozásokon keresztül is tartalmazza. Bevezetésért lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. Az a **tulajdonságok** ablaktábláján a [és adatokat importálhat modul](https://msdn.microsoft.com/library/azure/dn905997.aspx), jelölje be **Azure SQL Database** a a **adatforrás** legördülő listából.
2. Az a **adatbázis-kiszolgáló neve** szöveget adja meg`tcp:<DNS name of your virtual machine>,1433`
3. Adja meg az SQL felhasználónevet a **kiszolgáló felhasználói fiók nevét** szövegmezőben.
4. Adja meg az sql-felhasználó jelszót a **kiszolgáló felhasználói fiók jelszavát** szövegmezőben.
   
   ![Az Azure Machine Learning-adatok beolvasása][13]

## <a name="shutdown"></a>Leállítás és felszabadítani a virtuális gép, ha nincsenek használatban
Azure virtuális gépeken árú **csak a valóban használt funkciókért kell fizetnie, amennyit**. Győződjön meg arról, hogy meg vannak nem kiszámlázott amikor nem használja a virtuális gépet, hogy rendelkezik kell lennie a **leállítva (Deallocated)** állapotát.

> [!NOTE]
> A virtuális gép leállítása belül (a Windows energiagazdálkodási lehetőségek használatával), a virtuális gép le van állítva, de továbbra is lefoglalt. Győződjön meg arról, hogy még nem kiszámlázott, hogy minden esetben állítsa le a virtuális gépek a [klasszikus Azure portál](http://manage.windowsazure.com/). vagy a PowerShell parancsfelület ShutdownRoleOperation parancsával, amelynek a PostShutdownAction argumentumában a StoppedDeallocated értéket kell ehhez megadni.
> 
> 

A Leállítás utáni állapotot és a virtuális gép felszabadítása:

1. Jelentkezzen be a [klasszikus Azure portál](http://manage.windowsazure.com/) a fiókjával.  
2. Válassza ki **virtuális gépek** bal oldali navigációs sávján.
3. A virtuális gépek listájának megtekintéséhez kattintson a virtuális gép nevére, majd nyissa meg a **IRÁNYÍTÓPULT** lap.
4. Kattintson a lap alján **LEÁLLÍTÁSI**.

![Virtuális gép leállítása][15]

A virtuális gép lesz felszabadítása. lehetséges, de nem törlődnek. A klasszikus Azure-portálon bármikor előfordulhat, hogy indítsa újra a virtuális gép.

## <a name="your-azure-sql-server-vm-is-ready-to-use-whats-next"></a>Az Azure SQL Server virtuális gép készen áll a használatra: következő?
A virtuális gép már a adatok tudományos gyakorlatokban használatra kész. A virtuális gép is az Azure Machine Learning és a Team adatok tudományos folyamat (TDSP) feltárása és feldolgozására, az adatok és más feladatok együtt IPython Notebook kiszolgálóként használatra kész.

A következő lépéseket az adatok tudományos folyamatban vannak leképezve a a [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , HDInsight, a folyamat az adatok áthelyezése, illetve mintát, hogy az adatokat az Azure Machine Learning tanulva előkészítésekor lépéseket is lehetnek.

[1]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/selectsqlvmimg.png
[2]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/4vm-config.png
[3]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/sqlvmports.png
[4]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmpostopts.png
[5]:./media/machine-learning-data-science-setup-sql-server-virtual-machine/searchssms.png
[6]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/19connect-to-server.png
[7]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/20server-properties.png
[8]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/21mixed-mode.png
[9]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/22restart2.png
[10]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/23new-login.png
[11]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/24test-login.png
[12]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/25sysadmin.png
[13]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/amlreader.png
[15]: ./media/machine-learning-data-science-setup-sql-server-virtual-machine/vmshutdown.png

