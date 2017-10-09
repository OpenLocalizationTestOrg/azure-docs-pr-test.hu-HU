---
title: "egy SQL Server virtuális gép IPython Notebook kiszolgálóként aaaSet |} Microsoft Docs"
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
ms.openlocfilehash: ee83d1d5de671d9817c1bc1abd6b4f9c256dde8f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-an-azure-sql-server-virtual-machine-as-an-ipython-notebook-server-for-advanced-analytics"></a>Azure SQL Server virtuális gép beállítása IPython Notebook-kiszolgálóként a speciális elemzésekhez
Ez a témakör bemutatja, hogyan tooprovision, és konfigurálja az SQL Server virtuális gép toobe a felhőalapú adatokat tudományos környezet részeként használatos. támogató eszközöket, például a IPython Notebook, Azure Tártallózó, és AzCopy, valamint más hasznos adatokat tudományos projektek a segédeszközök hello Windows rendszerű virtuális gép van beállítva. Az Azure Tártallózó és AzCopy, például módszereket biztosítanak az kényelmes tooupload adatok tooAzure blob storage-ának a helyi számítógépen vagy toodownload azt tooyour helyi számítógép blobtárolóból.

hello Azure virtuális gép számos olyan rendszerkép található, amely tartalmazza a Microsoft SQL Server rendelkezik. Az adattárolási igényeinek megfelelő SQL Server virtuális gép lemezképének kiválasztása. Ajánlott lemezképeket a következők:

* SQL Server 2012 SP2 Enterprise kis toomedium adatok mérete
* SQL Server 2012 SP2 Enterprise optimalizálva nagy toovery nagy adatmennyiség DataWarehousing munkaterhelések
  
  > [!NOTE]
  > SQL Server 2012 SP2 Enterprise kép **nem tartalmazza az adatlemezt**. Meg kell tooadd, illetve egy vagy több virtuális merevlemezek toostore csatolja az adatok. Egy Azure virtuális gép létrehozásakor rendelkezik, és egy ideiglenes leképezve toohello D meghajtó hello leképezett operációs rendszer toohello C meghajtó lemezt. Ne használjon hello D meghajtó toostore adatokat. Hello név azt jelenti, mert csak az átmeneti tárolási biztosít. Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.
  > 
  > 

## <a name="Provision"></a>Csatlakozás toohello klasszikus Azure portálon, és az SQL Server rendszerű virtuális gép
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com/) a fiókjával.
   Ha nem rendelkezik Azure-fiókkal, az [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/) biztosít.
2. Hello klasszikus Azure portálon, a hello weblapon hello jobb alsó sarkában kattintson **+ új**, kattintson a **számítási**, kattintson a **virtuális gép**, és kattintson a **FROM GYŰJTEMÉNYELEM**.
3. A hello **hozzon létre egy virtuális gépet** lapon válasszon ki egy virtuális gép lemezképet tartalmazó SQL Server-adatok igényei szerint, és kattintson a jobb alsó sarkában hello lap hello tovább nyílra. Hello naprakész információkat hello támogatott Azure SQL Server-lemezképek, a következő témakörben: [Ismerkedés az SQL Server Azure virtuális gépek](http://go.microsoft.com/fwlink/p/?LinkId=294720) hello témakörében [SQL Server Azure virtuális gépek](http://go.microsoft.com/fwlink/p/?LinkId=294719) dokumentációkészlet.
   
   ![Válassza ki az SQL Server rendszerű virtuális Géphez][1]
4. A hello első **virtuálisgép-konfiguráció** lapján adja meg a következő információkat:
   
   * Adjon meg egy **virtuális gép neve**.
   * A hello **új FELHASZNÁLÓNEVET** mezőben, egyedi felhasználónév típus hello virtuális gép helyi rendszergazdai fiók.
   * A hello **új jelszó** mezőbe írjon be egy erős jelszót. További információkért lásd az [erős jelszavak](http://msdn.microsoft.com/library/ms161962.aspx) létrehozását ismertető cikket.
   * A hello **jelszó megerősítése** mezőbe írja be ismét hello jelszót.
   * Jelölje be hello megfelelő **mérete** hello a legördülő listából.
     
     > [!NOTE]
     > hello mérete hello virtuális gép kiépítése során van megadva: A2 mérete hello legkisebb üzemi terhelés esetén. A virtuális gép ajánlott minimális mérete A3 használata esetén az SQL Server Enterprise Edition. Válassza ki a A3 vagy újabb SQL Server Enterprise Edition használatakor. Válassza ki a A4 tranzakciós munkaterhelések lemezképek SQL Server 2012 vagy 2014 Enterprise optimalizált használata esetén.
     > Válassza ki a A7 Data Warehousing munkaterhelések lemezképek SQL Server 2012 vagy 2014 Enterprise optimalizált használata esetén. kiválasztott hello méret konfigurálhatja az adatlemezek számának korlátozása. Hogy csatolhat a tooa virtuális gép elérhető virtuálisgép-méretek és adatlemezek száma hello legfrissebb információkért lásd: [Azure virtuálisgép-méretek](http://msdn.microsoft.com/library/azure/dn197896.aspx). Díjszabási információkért lásd: [virtuális Macines árképzési](https://azure.microsoft.com/pricing/details/virtual-machines/).
     > 
     > 
   
   Kattintson a Tovább nyílra hello hello alsó jobb toocontinue.
   
   ![Virtuálisgép-konfiguráció][2]
5. A hello második **virtuálisgép-konfiguráció** lapon, hálózati, tárolási és rendelkezésre állás erőforrásait:
   
   * A hello **Felhőszolgáltatás** válassza **hozzon létre egy új felhőalapú szolgáltatás**.
   * A hello **felhőalapú szolgáltatás DNS-név** , hogy ő maga befejeződne formátumú nevét adja meg a hello első része egy DNS-nevet az Ön által választott **TESTNAME.cloudapp.net**
   * A hello **régió/AFFINITÁS csoport/virtuális hálózati** mezőben válasszon ki egy régiót, ahol a virtuális lemezképet fog üzemelni.
   * A hello **Tárfiók**, válasszon egy meglévő tárfiókot, vagy válasszon ki egy automatikusan létrehozott másikat.
   * A hello **rendelkezésre ÁLLÁSI csoport** mezőben válassza **(nincs)**.
   * Olvassa el, és fogadja el a hello díjszabási információkat.
6. A hello **VÉGPONTOK** a hello üres legördülő lista alatt kattintson **neve**, és válassza ki **MSSQL** írja be az adatbázis-kezelő (hellopéldányánakhelloportszám**1433** hello alapértelmezett példány).
7. Az SQL Server virtuális gép is lehetnek egy IPython Notebook kiszolgálóra, amely egy későbbi lépésben lesz konfigurálva.
   Adjon hozzá egy új végpont toospecify hello port toouse a IPython Notebook kiszolgáló. Adjon meg egy nevet a hello **neve** oszlop, válassza ki a portszámot az Ön által választott hello nyilvános port és 9999 hello magánhálózati port.
   
   Kattintson a Tovább nyílra hello hello alsó jobb toocontinue.
   
   ![Válassza ki az MSSQL és IPython portok][3]
8. Fogadja el az alapértelmezett hello **telepítése Virtuálisgép-ügynök** beállítás be van jelölve, majd kattintson a hello hello be hello jobb alsó sarkában hello varázsló toocomplete hello virtuális gép üzembe helyezési folyamat.
   
   `![Virtuális gép végső beállítások][4]
9. Várjon, amíg a Azure előkészíti a virtuális gép. Várt hello virtuális gép állapota tooproceed keresztül:
   
   * (Kiépítése) indítása
   * Leállítva
   * (Kiépítése) indítása
   * Futó (kiépítése)
   * Fut

## <a name="RemoteDesktop"></a>Nyissa meg a hello virtuális gép használata a távoli asztal és a telepítés befejezéséhez
1. Kiépítés befejeztével kattintson a virtuális gép toogo toohello IRÁNYÍTÓPULT-oldalon hello név. Hello a hello lap alján, kattintson **Connect**.
2. Válassza a tooopen hello rpd fájl hello Windows távoli asztal programmal (`%windir%\system32\mstsc.exe`).
3. A hello **Windows biztonsági** párbeszédpanelen adja meg a helyi rendszergazdai fiók, amelyet a korábbi lépésben hello jelszó.
   (Megadását hello virtuális gép tooverify hello hitelesítő adatokkal.)
4. hello első bejelentkezéskor toothis virtuális gépen több folyamatok esetleg toocomplete, beleértve a asztali, Windows-frissítések és hello Windows kezdeti konfigurációs feladatok (sysprep) megvalósításának. A Windows sysprep befejezése után az SQL Server telepítése konfigurációs feladatokat hajtja végre. Ezek a feladatok okozhat néhány percet várnia, amíg a művelet befejeződik. `SELECT @@SERVERNAME`nem adhat vissza hello helyes nevét, csak az SQL Server telepítése befejeződik, és SQL Server Management Studio nem lehet visable hello start lap.

Ha a Windows távoli asztal csatlakoztatott toohello virtuális gép, hello virtuális gép működik sokkal, mint bármely más számítógépre. Csatlakozás az SQL Server Management Studio (hello virtuális gépen futó) SQL Server alapértelmezett példánya toohello hello a szokásos módon.

## <a name="InstallIPython"></a>IPython jegyzetfüzet és egyéb támogató eszközök telepítése
tooconfigure az új SQL Server virtuális gép tooserve egy IPython Notebook-kiszolgálóval, és telepítése további támogató eszközök ilyen AzCopy, Azure Tártallózó, hasznos adatok tudományos Python-csomagokat, és mások számára, egy speciális testreszabás parancsfájl tooyou biztosítja. tooinstall:

1. Kattintson a jobb gombbal hello **Windows Start** ikonra, majd **parancssor (rendszergazda)**
2. A következő parancsok hello másolása és beillesztése hello parancssorban.
  
        set script='https://raw.githubusercontent.com/Azure/Azure-MachineLearning-DataScience/master/Misc/MachineSetup/Azure_VM_Setup_Windows.ps1'
        @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString(%script%))"
3. Amikor a rendszer kéri, adja meg egy tetszőleges jelszót hello IPython Notebook kiszolgáló.
4. hello testreszabási parancsfájl automatizálja a telepítés utáni számos eljárást, többek között:
    * Telepítési és beállítási IPython Notebook kiszolgáló
    * TCP-portok megnyitása a korábban létrehozott hello végpontok hello Windows tűzfalon:
    * A távoli SQL Server-kapcsolathoz
    * A kiszolgáló távoli kapcsolatot IPython Notebook
    * A minta IPython jegyzetfüzet és SQL-parancsfájlok beolvasása
    * Letölti és telepíti a hasznos adatok tudományos Python-csomagok
    * Letöltése és telepítése az Azure-eszközök például az AzCopy és Azure Tártallózó  
    <br>
5. Előfordulhat, hogy hozzáférési és IPython Notebook futtatása minden böngészőből helyi vagy távoli hello URL-cím használatával `https://<virtual_machine_DNS_name>:<port>`, ahol hello IPython nyilvános portot hello virtuális gép kiépítése során kiválasztott.
6. IPython Notebook server a háttérben szolgáltatásként fut, és automatikusan újraindul hello virtuális gép újraindításakor.

## <a name="Optional"></a>Adatlemez csatolása igény szerint
Ha a Virtuálisgép-lemezkép nem tartalmazza az adatlemezek, azaz, a C meghajtó (operációsrendszer-lemez) és a D meghajtó (ideiglenes lemez), lemezek egy tooadd van szüksége, vagy további adatok lemezek toostore adatait. hello Virtuálisgép-lemezkép az SQL Server 2012 SP2 Enterprise optimalizálva DataWarehousing munkaterhelések előre konfigurált SQL Server adatainak és naplókönyvtárainak fájlok további lemezzel rendelkezik.

> [!NOTE]
> Ne használjon hello D meghajtó toostore adatokat. Hello név azt jelenti, mert csak az átmeneti tárolási biztosít. Kínál nincs redundancia vagy a biztonsági mentést, mert az nem található az Azure storage.
> 
> 

tooattach adatlemeznek, kövesse az ismertetett lépések hello [hogyan tooAttach egy adatlemez tooa Windows rendszerű virtuális gép](../virtual-machines/windows/classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json), amely végigvezeti Önt keresztül:

1. A korábbi lépések kiépített üres lemez(ek) toohello virtuális gép csatlakoztatása
2. Új lemez(ek) hello hello virtuális gépen inicializálása

## <a name="SSMS"></a>Csatlakozás tooSQL Server Management Studio eszközt, és kevert módú hitelesítés engedélyezése
SQL Server adatbázismotor hello nem használható Windows-hitelesítés nélkül tartományi környezetben. tooconnect toohello adatbázismotor egy másik számítógépről, SQL Server konfigurálása a vegyes módú hitelesítést. A vegyes módú hitelesítés az SQL Server-hitelesítést és a Windows-hitelesítést is lehetővé teszi. SQL-hitelesítési mód által előírt sorrendben tooingest adatoknak közvetlenül az SQL Server virtuális gép adatbázisok a [Azure Machine Learning Studio](https://studio.azureml.net) hello adatokat importálhat modullal.

1. Távoli asztal használatával csatlakoztatott toohello a virtuális gépet, miközben használja a hello Windows **keresési** ablaktábla és a típus **SQL Server Management Studio** (SMSS). Kattintson a toostart hello SQL Server Management Studio (SSMS). Érdemes lehet egy helyi tooSSMS tooadd az asztalon későbbi használatra.
   
   ![Indítsa el a szolgáltatáshoz az SSMS][5]
   
   hello azt hello felhasználók Management Studio környezet létre kell hoznia a Management Studio első megnyitásakor. Ez eltarthat néhány pillanatig.
2. Megnyitásakor, a Management Studio hello megadja **tooServer csatlakozás** párbeszédpanel megnyitásához. A hello **kiszolgálónév** mezőjébe hello virtuális gép tooconnect toohello adatbázismotor hello Object Explorer hello nevét.
   (Hello virtuális gép neve helyett használhatja **(helyi)** vagy egy egyetlen időszak hello **kiszolgálónév**. Válassza ki **Windows-hitelesítés**, és hagyja  ***a\_VM\_neve*\\a\_helyi\_rendszergazda**  a hello **felhasználónév** mezőbe. Kattintson a **Connect** (Csatlakozás) gombra.
   
   ![Csatlakozás tooServer][6]
   
   <br>
   
   > [!TIP]
   > Hello SQL Server hitelesítési mód a Windows beállításjegyzék-kulcs módosítás vagy hello SQL Server Management Studio használatával módosíthatja. toochange hitelesítési mód használata hello beállításjegyzék kulcsváltozás, kezdő egy **új lekérdezés** , majd hajtsa végre a következő parancsfájl hello:
   > 
   > 
   
       USE master
       go
   
       EXEC xp_instance_regwrite N'HKEY_LOCAL_MACHINE', N'Software\Microsoft\MSSQLServer\MSSQLServer', N'LoginMode', REG_DWORD, 2
       go

    toochange hello hitelesítési mód az SQL Server management Studio használatával:

1. A **SQL Server Management Studio Object Explorerben**, kattintson a jobb gombbal az SQL Server (hello virtuális gép neve) hello-példány nevét, és kattintson a **tulajdonságok**.
   
   ![Kiszolgáló tulajdonságai][7]
2. A hello **biztonsági** lap **kiszolgálóhitelesítés**, jelölje be **SQL Server és a Windows-hitelesítés üzemmód**, és kattintson a **OK** .
   
   ![A hitelesítési mód kiválasztása][8]
3. A hello **SQL Server Management Studio** párbeszédpanel, kattintson a **OK** lehet visszaigazolni hello követelmény toorestart SQL Server.
4. A **Object Explorer**, kattintson a jobb gombbal a kiszolgáló, és kattintson a **indítsa újra a**. (Ha az SQL Server Agent fut, azt is újra kell indítani.)
   
   ![Újraindítás][9]
5. A hello **SQL Server Management Studio** párbeszédpanel, kattintson a **Igen** gombra kattintva elfogadja, amelyet az SQL Server toorestart.

## <a name="Logins"></a>SQL Server-hitelesítési bejelentkezési létrehozása
tooconnect toohello adatbázismotor egy másik számítógépről, létre kell hoznia legalább egy SQL Server-hitelesítési bejelentkezési névként.  

Létrehozhat új SQL Server bejelentkezési programozott módon, vagy SQL Server Management Studio használatával hello. programozott módon, indítsa el az SQL-hitelesítéshez új rendszergazda felhasználó toocreate egy **új lekérdezés** , majd hajtsa végre a következő parancsfájl hello. Cserélje le < új felhasználónevet\> és < Új jelszó\> a választható *felhasználónév* és *jelszó*. 

    USE master
    go

    CREATE LOGIN <new user name> WITH PASSWORD = N'<new password>',
        CHECK_POLICY = OFF,
        CHECK_EXPIRATION = OFF;

    EXEC sp_addsrvrolemember @loginame = N'<new user name>', @rolename = N'sysadmin';


Szükség szerint állítsa be a hello jelszóházirend (házirend ellenőrzése és a jelszó lejárati kikapcsolja hello mintakód). További információ az SQL Server-bejelentkezésekről: [Bejelentkezés létrehozása](http://msdn.microsoft.com/library/aa337562.aspx).  

toocreate új SQL Server bejelentkezési hello SQL Server Management Studio használatával:

1. A **SQL Server Management Studio Object Explorerben**, bontsa ki a kívánt toocreate hello új bejelentkezés hello server-példány hello mappa.
2. Kattintson a jobb gombbal hello **biztonsági** mappára, mutasson túl**új**, és válassza ki **bejelentkezési...** .
   
   ![Új bejelentkezés][10]
3. A hello **- új bejelentkezés** párbeszédpanel hello **általános** lapján adja meg hello hello új felhasználó hello **bejelentkezési név** mezőbe.
4. Kattintson az **SQL Server-hitelesítés** lehetőségre.
5. A hello **jelszó** mezőbe írja be a jelszót hello új felhasználó. Adjon meg, hogy a jelszó újra hello **jelszó megerősítése** mezőbe.
6. házirend jelszóbeállításokat tooenforce összetettségét és kényszerítési, válassza ki **jelszó házirend** (ajánlott). Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
7. házirend jelszóbeállításokat tooenforce lejáratának, válassza ki **kényszerítése a jelszó lejárati** (ajánlott). Jelszó házirend kell kijelölt tooenable ezt a jelölőnégyzetet. Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
8. tooforce hello felhasználói toocreate hello először a bejelentkezés után új jelszót használja, válasszon **kell változtatni a jelszót a következő bejelentkezésekor** (ajánlott, ha a bejelentkezési adatai valaki másnak toouse. Ha hello bejelentkezési saját használatra, ne válassza ezt a lehetőséget.) Jelszó lejárata kényszerítése kell kijelölt tooenable ezt a jelölőnégyzetet. Ez az alapértelmezett lehetőség, ha SQL Server-hitelesítés van kiválasztva.
9. A hello **alapértelmezett adatbázis** listára, válassza ki az alapértelmezett adatbázis hello bejelentkezési azonosítóhoz. **fő** hello alapértelmezett ezt a beállítást. Ha még nem hozott létre a felhasználói adatbázis, hagyja üresen ezt a túl beállítása**fő**.
10. A hello **alapértelmezett nyelv** listában, hagyja **alapértelmezett** hello értékként.
    
    ![Bejelentkezési tulajdonságok][11]
11. Ha ez hello első bejelentkezés hoz létre, érdemes lehet kijelölni ehhez a bejelentkezéshez egy SQL Server rendszergazdájához. Ebben az esetben a **Kiszolgálói szerepkörök** lapon jelölje be a **sysadmin** jelölőnégyzetet.
    
    > [!IMPORTANT]
    > Hello SysAdmin (rendszergazda) rögzített kiszolgálói szerepkör tagjai rendelkeznek a teljes körű vezérlést biztosítanak hello adatbázismotor. Biztonsági okokból ügyeljen korlátozására, ez a szerepkör tagjának kell lennie.
    > 
    > 
    
    ![sysadmin][12]
12. Kattintson az OK gombra.

## <a name="DNS"></a>Határozza meg a virtuális gép hello hello DNS-neve
tooconnect toohello SQL Server adatbázismotorhoz egy másik számítógépről, ismernie kell a tartománynévrendszer (DNS) hello hello virtuális gép nevét.

(Ez a hello neve hello internet által használt tooidentify hello virtuális gépet. Hello IP-címet is használhat, de hello IP-cím változhat, amikor Azure redundancia vagy karbantartás erőforrásokat helyez át. hello DNS-neve lesz stabil, mert azok tooa új IP-cím átirányítva.)

1. A klasszikus Azure portál hello (vagy hello előző lépésben), válassza ki a **virtuális gépek**.
2. A hello **VIRTUÁLISGÉP-példányok** lap hello **DNS-név** oszlop, a Keresés és a példány hello DNS-név hello virtuális géphez, amely akkor jelenik meg utasításnak **http://**. (hello felhasználói felülete nem jelenik meg hello teljes nevet, de a kattintson a jobb gombbal, és válassza a másolás.)

## <a name="cde"></a>Adatbázis-kezelő toohello Csatlakozás másik számítógépről
1. Egy számítógép csatlakozik a toohello internet, nyissa meg az SQL Server Management Studio eszközt.
2. A hello **tooServer csatlakozás** vagy **tooDatabase motor csatlakozás** párbeszédpanel hello **kiszolgálónév** mezőbe írja be a virtuális gép (meghatározott hello hello DNS-neve előző tevékenység) és egy nyilvános végpontot portszámot hello formátumban *DNSName, portszám* például **tutorialtestVM.cloudapp.net,57500**.
3. A hello **hitelesítési** mezőben válassza **SQL Server-hitelesítés**.
4. A hello **bejelentkezési** be, olyan bejelentkezési azonosítót, egy korábbi feladatban létrehozott hello nevét.
5. A hello **jelszó** mezőbe, írja be a jelszót hello az Ön által létrehozott korábbi feladat hello bejelentkezési adatokat.
6. Kattintson a **Connect** (Csatlakozás) gombra.

## <a name="amlconnect"></a>Adatbázis-kezelő toohello csatlakozzon az Azure Machine Learning
Hello Team adatok tudományos folyamat későbbi szakaszában, hello segítségével [Azure Machine Learning Studióban](https://studio.azureml.net) toobuild és központi telepítése a machine learning modellek. az SQL Server virtuális gép adatbázisok közvetlenül az Azure Machine Learning képzési vagy pontozási, tooingest adatait használja hello **és adatokat importálhat** modul az új [Azure Machine Learning Studio](https://studio.azureml.net) kipróbálásához. Ez a témakör hello Team adatok tudományos folyamat útmutató hivatkozások segítségével további részleteket is tartalmazza. Bevezetésért lásd: [Mi az Azure Machine Learning Studio?](machine-learning-what-is-ml-studio.md).

1. A hello **tulajdonságok** hello panelén [és adatokat importálhat modul](https://msdn.microsoft.com/library/azure/dn905997.aspx), jelölje be **Azure SQL Database** a hello **adatforrás** legördülő listából.
2. A hello **adatbázis-kiszolgáló neve** szöveget adja meg`tcp:<DNS name of your virtual machine>,1433`
3. Adja meg hello SQL-felhasználó nevét a hello **kiszolgáló felhasználói fiók nevét** szövegmezőben.
4. Adja meg a hello sql jelszó hello **kiszolgáló felhasználói fiók jelszavát** szövegmezőben.
   
   ![Az Azure Machine Learning-adatok beolvasása][13]

## <a name="shutdown"></a>Leállítás és felszabadítani a virtuális gép, ha nincsenek használatban
Azure virtuális gépeken árú **csak a valóban használt funkciókért kell fizetnie, amennyit**. nem éppen tooensure számlázva, amikor nem használja a virtuális gépet, rendelkezik toobe hello **leállítva (Deallocated)** állapotát.

> [!NOTE]
> Hello virtuális gép leállítása belül (a Windows energiagazdálkodási lehetőségek használatával), hello virtuális gép le van állítva, de továbbra is lefoglalt. Ön éppen nem kiszámlázott, tooensure minden esetben állítsa le a virtuális gépek hello [klasszikus Azure portál](http://manage.windowsazure.com/). Hello Powershellen keresztül VM meghívásával ShutdownRoleOperation amelyeknél "PostShutdownAction" értéke túl is leállíthatja "StoppedDeallocated".
> 
> 

tooshutdown és hello virtuális gép felszabadítása:

1. Jelentkezzen be toohello [klasszikus Azure portál](http://manage.windowsazure.com/) a fiókjával.  
2. Válassza ki **virtuális gépek** hello bal oldali navigációs sávon a.
3. A virtuális gépek hello listájában kattintson a virtuális gépet, majd lépjen toohello hello név **IRÁNYÍTÓPULT** lap.
4. Hello a hello lap alján, kattintson **LEÁLLÍTÁSI**.

![Virtuális gép leállítása][15]

hello virtuális gép lesz felszabadítása. lehetséges, de nem törlődnek. A klasszikus Azure portál hello bármikor előfordulhat, hogy indítsa újra a virtuális gép.

## <a name="your-azure-sql-server-vm-is-ready-toouse-whats-next"></a>Az Azure SQL Server virtuális gép készen áll a toouse: következő?
A virtuális gép már készen áll a toouse az adatok tudományos gyakorlatokban. hello virtuális gép is az Azure Machine Learning és hello Team adatok tudományos folyamat (TDSP) egy IPython Notebook kiszolgálóként hello feltárása és az adatok feldolgozására és más feladatok együtt használatra kész.

hello hello tudományos folyamata a következő lépések vannak leképezve a hello [Team adatok tudományos folyamat](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) , HDInsight, a folyamat az adatok áthelyezése, illetve az Azure Machine learning hello adatokból előkészítésekor minta ott lépéseket is lehetnek. Learning.

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

