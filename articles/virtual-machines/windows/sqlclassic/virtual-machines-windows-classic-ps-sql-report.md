---
title: "aaaUse PowerShell tooCreate VM rendelkező egy natív mód jelentéskiszolgáló |} Microsoft Docs"
description: "Ez a témakör ismerteti, és bemutatja, hogyan hello központi telepítése és konfigurálása az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen. "
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: 553af55b-d02e-4e32-904c-682bfa20fa0f
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/11/2017
ms.author: asaxton
ms.openlocfilehash: e7791199c87dff106132f1535da12de40a8dbc9c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toocreate-an-azure-vm-with-a-native-mode-report-server"></a>Használjon PowerShell tooCreate egy Azure virtuális gép, egy natív mód jelentéskészítő kiszolgáló
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

Ez a témakör ismerteti, és bemutatja, hogyan hello központi telepítése és konfigurálása az SQL Server Reporting Services natív üzemmódú jelentéskészítő kiszolgáló egy Azure virtuális gépen. hello vonatkozó lépéseit a dokumentum használatban manuális toocreate hello virtuális gép és a Windows PowerShell-parancsfájl tooconfigure Reporting Services hello virtuális gép. hello konfigurációs parancsfájl tartalmazza a HTTP és HTTPs a tűzfal port megnyitása.

> [!NOTE]
> Ha nincs szüksége **HTTPS** hello jelentéskészítő kiszolgálón **a 2**.
> 
> Miután létrehozta a virtuális gép hello az 1. lépésben, lépjen a toohello szakasz használata parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP. Hello parancsfájl futtatása után a hello jelentéskészítő kiszolgáló készen áll a toouse.

## <a name="prerequisites-and-assumptions"></a>Előfeltételek és Előfeltételek
* **Azure-előfizetés**: Ellenőrizze az Azure-előfizetésben elérhető magok hello száma. Ha hoz létre a Virtuálisgép-méretet ajánlott hello **A3**, kell **4** rendelkezésre álló magot. Ha egy Virtuálisgép-méretet **A2**, kell **2** rendelkezésre álló magot.
  
  * tooverify hello core korlátot az előfizetéséhez, a klasszikus Azure-portál hello-beállítások gombra a hello bal oldali ablaktáblán, majd kattintson a használati hello felső menüjében.
  * tooincrease hello core kvóta, kapcsolattartási [Azure támogatási](https://azure.microsoft.com/support/options/). Virtuálisgép-méret információkért lásd: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* **A Windows PowerShell-Parancsprogramokról**: hello a témakör feltételezi, hogy rendelkezik-e a Windows PowerShell alapszintű ismeretét. A Windows PowerShell használatával kapcsolatos további információkért tekintse meg a hello következőt:
  
  * [A Windows PowerShell indítása a Windows Server](https://technet.microsoft.com/library/hh847814.aspx)
  * [Bevezetés a Windows PowerShell használatával](https://technet.microsoft.com/library/hh857337.aspx)

## <a name="step-1-provision-an-azure-virtual-machine"></a>1. lépés: Az Azure virtuális gép kiépítése
1. Keresse meg a klasszikus Azure portálon toohello.
2. Kattintson a **virtuális gépek** hello bal oldali ablaktáblán.
   
    ![a Microsoft azure virtuális gépek](./media/virtual-machines-windows-classic-ps-sql-report/IC660124.gif)
3. Kattintson az **Új** lehetőségre.
   
    ![Új gomb](./media/virtual-machines-windows-classic-ps-sql-report/IC692019.gif)
4. Kattintson a **gyűjteményből**.
   
    ![új virtuális gép gyűjteményből](./media/virtual-machines-windows-classic-ps-sql-report/IC692020.gif)
5. Kattintson a **SQL Server 2014 RTM Standard – Windows Server 2012 R2** és kattintson a nyílra toocontinue hello.
   
    ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
   
    Ha hello Reporting Services adatvezérelt előfizetések funkció van szüksége, válassza a **SQL Server 2014 RTM vállalati – Windows Server 2012 R2**. SQL Server kiadása és által nyújtott szolgáltatások támogatásáról további információkért lásd: [hello kiadás az SQL Server 2012 által támogatott funkciók](https://msdn.microsoft.com/library/cc645993.aspx#Reporting).
6. A hello **virtuálisgép-konfiguráció** lapon, a következő mezők hello szerkesztése:
   
   * Ha egynél több **VERZIÓJÚ kiadási dátum**, válassza ki a legújabb verziót hello.
   * **Virtuális gép neve**: hello számítógép nevét is használatos hello tovább konfigurálása lapon hello alapértelmezett felhőalapú szolgáltatás DNS-névvel. hello DNS nevének egyedinek kell lennie a hello Azure szolgáltatás között. Fontolja meg, hogy hello VM nevű számítógép, amely leírja, milyen hello VM szolgál. Például ssrsnativecloud.
   * **Réteg**: Standard
   * **Méret: A3** hello javasolt az SQL Server számítási feladatait a Virtuálisgép-méretet. Ha egy virtuális gép csak egy jelentéskészítő kiszolgálón, a virtuális gép méretét A2 esetén elegendő hello jelentéskészítő kiszolgáló észlel a nagy munkaterhelések. A virtuális gép a díjszabásról, lásd: [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).
   * **Új felhasználónevet**: hello-nevet a virtuális gép hello rendszergazdaként jön létre.
   * **Új jelszó** és **megerősítése**. Hello új rendszergazdai fiókhoz használja ezt a jelszót, és erős jelszó használatát javasoljuk.
   * Kattintson a **Tovább** gombra. ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
7. Hello következő lapon szerkessze a következő mezők hello:
   
   * **A felhőalapú szolgáltatás**: válasszon **hozzon létre egy új felhőalapú szolgáltatás**.
   * **A felhőalapú szolgáltatás DNS-név**: Ez az hello Felhőszolgáltatás, amely kapcsolódik a virtuális gép hello hello nyilvános DNS-nevét. hello alapértelmezett értéke hello virtuális gép nevét a megadott hello névhez. Ha hello témakört a későbbi lépésekben hoz létre egy megbízható az SSL-tanúsítványt, és majd hello DNS-név használható hello hello értéke "**kiadott**" hello tanúsítvány.
   * **Régió/affinitás csoport/virtuális hálózati**: válassza ki a hello régió legközelebbi tooyour végfelhasználók számára.
   * **A Tárfiók**: egy automatikusan létrehozott tárfiókot használja.
   * **A rendelkezésre állási csoport**: nincs.
   * **VÉGPONTOK** megtartása hello **távoli asztal** és **PowerShell** végpontok majd adja hozzá a HTTP vagy HTTPS-végpont a környezettől függően.
     
     * **HTTP**: nyilvános és titkos portok alapértelmezett hello **80**. Vegye figyelembe, hogy ha egy magánhálózati port a 80-as, nem módosíthatja **$HTTPport = 80** hello http parancsfájlban.
     * **HTTPS**: nyilvános és titkos portok alapértelmezett hello **443-as**. Biztonsági szempontból ajánlott toochange hello magánhálózati port, és konfigurálja a tűzfal és hello jelentéskészítő kiszolgáló toouse hello magánhálózati port. A végpontok további információkért lásd: [hogyan tooSet be egy virtuális gép kommunikációs](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json). Vegye figyelembe, hogy egy 443,-astól eltérő port használata esetén módosítsa a hello paraméter **$HTTPsport = 443-as** a hello HTTPS parancsfájl.
   * Kattintson a Tovább gombra. ![következő](./media/virtual-machines-windows-classic-ps-sql-report/IC692021.gif)
8. Hello hello varázsló utolsó lapján, tartsa hello alapértelmezett **hello Virtuálisgép-ügynök telepítéséhez** kijelölt. hello témakörben leírt lépések hasznosíthatja hello Virtuálisgép-ügynök, de ha tookeep a virtuális gép, Virtuálisgép-ügynök hello és bővítmények lehetővé teszi a tooenhance helykiszolgálójához CM.  A Virtuálisgép-ügynök hello további információkért lásd: [ügynök és Virtuálisgép-bővítmények – 1. rész](https://azure.microsoft.com/blog/2014/04/11/vm-agent-and-extensions-part-1/). Hello alapértelmezett telepített kiterjesztéseket ad futó egyik hello "BGINFO" bővítményt, amely hello virtuális gép asztalán jeleníti meg, a rendszer-információkat, például a belső IP- és szabad terület meghajtó.
9. Kattintson a Kész gombra. ![oké](./media/virtual-machines-windows-classic-ps-sql-report/IC660122.gif)
10. Hello **állapota** a virtuális gép részére hello **indítása (kiépítés)** során hello kiépítési folyamat és értékként jelenik majd meg **futtató** hello virtuális gép esetén kiosztott és készen áll toouse.

## <a name="step-2-create-a-server-certificate"></a>2. lépés: A kiszolgálói tanúsítvány létrehozása
> [!NOTE]
> Ha nincs szüksége HTTPS hello jelentéskészítő kiszolgálón, akkor **a 2** toohello szakaszt, és **parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP használata**. Használjon hello HTTP parancsfájl tooquickly hello jelentéskészítő kiszolgáló és a kiszolgáló készen áll a toouse lesz hello jelentés konfigurálása.

Rendelés toouse HTTPS hello VM kell egy megbízható az SSL-tanúsítványt. A forgatókönyvtől függően hello a következő két módszer egyikét használhatja:

* Érvényes SSL-tanúsítvány kiadott által hitelesítésszolgáltató (CA), és a Microsoft megbízhatónak. hello hitelesítésszolgáltató legfelső szintű tanúsítványok hello Microsoft Root Certificate programon keresztül terjesztett szükséges toobe. A programra vonatkozó további információkért lásd: [Windows és Windows Phone 8 SSL Root Certificate Program (tag CAs)](http://social.technet.microsoft.com/wiki/contents/articles/14215.windows-and-windows-phone-8-ssl-root-certificate-program-member-cas.aspx) és [bemutatása toohello Microsoft Root Certificate Program](http://social.technet.microsoft.com/wiki/contents/articles/3281.introduction-to-the-microsoft-root-certificate-program.aspx).
* Egy önaláírt tanúsítványt. Önaláírt tanúsítványok éles környezetekben nem ajánlott.

### <a name="toouse-a-certificate-created-by-a-trusted-certificate-authority-ca"></a>toouse létrehozni egy tanúsítványt egy megbízható tanúsítvány hitelesítésszolgáltatói (CA)
1. **Hello webhely kiszolgálói tanúsítványt kérhet egy hitelesítésszolgáltatótól**. 
   
    Használhat kiszolgálói tanúsítvány varázsló hello vagy toogenerate kérelem Tanúsítványfájl (Certreq.txt), hogy küldjön tooa hitelesítésszolgáltató vagy toogenerate egy online hitelesítésszolgáltatót kérelmet. Például a Microsoft tanúsítványszolgáltatások újdonságai a Windows Server 2012-ben. Attól függően, hogy a kiszolgálói tanúsítvány által nyújtott azonosítás biztonsági szint hello hello certification authority tooapprove néhány napon tooseveral hónapig a kérést, és küldjön egy tanúsítványfájlt. 
   
    A kiszolgálói tanúsítványok kérésével kapcsolatos további információkért tekintse meg a hello következőt: 
   
   * Használjon [Certreq](https://technet.microsoft.com/library/cc725793.aspx), [Certreq](https://technet.microsoft.com/library/cc725793.aspx).
   * Biztonsági eszközök tooAdminister Windows Server 2012-ben.
     
     [Biztonsági eszközök tooAdminister Windows Server 2012-ben](https://technet.microsoft.com/library/jj730960.aspx)
     
     > [!NOTE]
     > Hello **kiadott** hello mezője megbízható az SSL-tanúsítványt kell kell hello ugyanaz, mint hello **felhőalapú szolgáltatás DNS-név** meg használt hello új virtuális gép.

2. **Hello kiszolgálótanúsítvány telepítésének hello webkiszolgálón**. hello webkiszolgáló a jelen esetben ez hello VM állomások hello jelentéskészítő kiszolgáló és a hello webhely a későbbi lépésekben jön létre, amikor a Reporting Services konfigurálásához. Hello kiszolgálói tanúsítvány telepítésével hello webkiszolgálón hello tanúsítvány MMC beépülő modul használatával kapcsolatos további információkért lásd: [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).
   
    Ha azt szeretné, hogy toouse hello parancsprogramja ebben a témakörben tooconfigure hello jelentéskészítő kiszolgáló hello hello tanúsítványok értékének **ujjlenyomat** szükséges hello parancsfájl paraméterként. Hello részletei a következő szakaszban látható hogyan tooobtain hello hello tanúsítvány ujjlenyomata.
3. Rendelje hozzá a hello kiszolgálói tanúsítvány toohello jelentéskészítő kiszolgáló. hello hozzárendelés hello jelentéskészítő kiszolgáló konfigurálásakor hello a következő szakaszban befejeződött.

### <a name="toouse-hello-virtual-machines-self-signed-certificate"></a>Virtuális gépek önaláírt tanúsítvány toouse hello
Egy önaláírt tanúsítványt a virtuális gép hello jött létre, amikor a virtuális gép hello lett kiépítve. a tanúsítványnak hello hello azonos nevet, amint hello VM DNS-nevét. Az order tooavoid tanúsítvány hibákat kell hello tanúsítvány megbízható-e a virtuális gépért hello és minden felhasználó hello hely.

1. tootrust hello legfelső szintű hitelesítésszolgáltató hello tanúsítvány a helyi virtuális gép, hello hozzáadása hello tanúsítvány toohello **megbízható legfelső szintű hitelesítésszolgáltatók**. hello hello szükséges lépések összefoglalása látható. Hogyan tootrust hello hitelesítésszolgáltató részletes lépéseiért lásd: [a kiszolgálótanúsítvány telepítésének](https://technet.microsoft.com/library/cc740068).
   
   1. Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra. A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.
      
       ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja. 
      
       Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.
      
       ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
   2. Futtassa a mmc.exe. További információkért lásd: [hogyan: nézet tanúsítványok beépülő MMC-modulban hello](https://msdn.microsoft.com/library/ms788967.aspx).
   3. Hello Konzolalkalmazás **fájl** menü hello hozzáadása **tanúsítványok** beépülő modulban válassza **számítógépfiók** kéri, majd **tovább**.
   4. Válassza ki **helyi számítógép** toomanage majd **Befejezés**.
   5. Kattintson a **Ok** majd bontsa ki a hello **tanúsítványok - személyes** csomópont, és kattintson **tanúsítványok**. hello tanúsítvány neve után hello VM hello DNS-nevét és végződik **cloudapp.net**. Kattintson a jobb gombbal a hello tanúsítványnevet, és kattintson a **másolási**.
   6. Bontsa ki a hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, és kattintson rá jobb gombbal **tanúsítványok** majd **Beillesztés**.
   7. dupla toovalidate kattintson hello tanúsítvány neve alatt a **megbízható legfelső szintű hitelesítésszolgáltatók** , és ellenőrizze, hogy nincsenek hibák, és a tanúsítvány látható. Ha azt szeretné, hogy toouse hello HTTPS-parancsprogramja ebben a témakörben tooconfigure hello jelentéskészítő kiszolgáló hello hello tanúsítványok értékének **ujjlenyomat** szükséges hello parancsfájl paraméterként. **tooget hello ujjlenyomat értékének**, végezze el a következő hello. Van még egy PowerShell minta tooretrieve hello ujjlenyomatot szakaszban [parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTPS használatára](#use-script-to-configure-the-report-server-and-HTTPS).
      
      1. Kattintson duplán a hello hello tanúsítvány nevével, például ssrsnativecloud.cloudapp.net.
      2. Kattintson a hello **részletek** fülre.
      3. Kattintson a **ujjlenyomat**. hello ujjlenyomat hello érték jelenik meg hello részleteket tartalmazó mező, például a6 08 3 c. df f9 0b f7 e3 7c 25 kell adnia végrehajtási adatokat a4 kell adnia végrehajtási adatokat 7e ac 91 9c 2c fb 2f.
      4. Hello ujjlenyomat másolása és mentése hello értéket későbbi használatra, vagy most hello parancsfájl szerkesztése.
      5. (*) Hello parancsfájl futtatása előtt távolítsa el a hello szóközöket Between hello értékpárok. Például előtt feljegyzett hello ujjlenyomat most lenne a6083cdff90bf7e37c25eda4ed7eac919c2cfb2f.
      6. Rendelje hozzá a hello kiszolgálói tanúsítvány toohello jelentéskészítő kiszolgáló. hello hozzárendelés hello jelentéskészítő kiszolgáló konfigurálásakor hello a következő szakaszban befejeződött.

Ha egy önaláírt SSL-tanúsítványt használ, a hello nevét hello tanúsítvány már megegyezik hello VM hello állomásnevével. Ezért hello DNS hello gép már regisztrálva van globálisan, és elérhető az összes ügyfél.

## <a name="step-3-configure-hello-report-server"></a>3. lépés: Hello jelentéskészítő kiszolgáló konfigurálása
Ez a szakasz bemutatja, hogyan hello VM konfigurálása natív üzemmódú Reporting Services jelentéskészítő kiszolgálón. A következő módszerek tooconfigure hello jelentéskészítő kiszolgáló hello egyikét használhatja:

* Hello parancsfájl tooconfigure hello jelentéskészítő kiszolgáló használatához
* Használja a Configuration Manager tooConfigure hello jelentéskészítő kiszolgálón.

További részletes lépéseket, tekintse meg a hello szakasz [Connect toohello virtuális gép és a Start hello Reporting Services Configuration Manager](virtual-machines-windows-classic-ps-sql-bi.md#connect-to-the-virtual-machine-and-start-the-reporting-services-configuration-manager).

**Hitelesítési Megjegyzés:** Windows-hitelesítés hello ajánlott hitelesítési módszert, valamint hello alapértelmezett Reporting Services hitelesítés. Csak a virtuális gép hello konfigurált felhasználók férhetnek a Reporting Services és tooReporting szolgáltatások szerepkört.

### <a name="use-script-tooconfigure-hello-report-server-and-http"></a>Parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTP használata
toouse hello Windows PowerShell parancsfájl tooconfigure hello jelentéskészítő kiszolgálón, a következő lépéseket teljes hello. hello konfiguráció HTTP, HTTPS-nem tartalmazza:

1. Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra. A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja. 
   
    Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Nyissa meg a virtuális gép hello, **Windows PowerShell ISE** rendszergazdai jogosultságokkal. a Windows server 2012 rendszerben alapértelmezés szerint telepítve van a PowerShell ISE hello. Ajánlott használnia hello ISE helyett a szabványos Windows PowerShell-ablakot, hogy hello parancsfájl beillesztése hello ISE, módosítsa hello parancsfájlt, és futtassa a hello parancsfájl.
3. A Windows PowerShell ISE, kattintson a hello **nézet** menüre, majd majd **parancsfájl ablaktábla megjelenítése**.
4. A következő parancsfájl hello másolással illessze be a hello parancsfájl hello Windows PowerShell ISE parancsfájl ablaktáblára.
   
        ## This script configures a Native mode report server without HTTPS
        $ErrorActionPreference = "Stop"
   
        $server = $env:COMPUTERNAME
        $HTTPport = 80 # change hello value if you used a different port for hello private HTTP endpoint when hello VM was created.
   
        ## Set PowerShell execution policy toobe able toorun scripts
        Set-ExecutionPolicy RemoteSigned -Force
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Report Server Configuration Steps
   
        ## Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port $HTTPport (for local usage)
            write-host "Calling ReserveURL port $HTTPport"
            $r = $RSObject.ReserveURL('ReportServerWebService',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportServer port $HTTPport" 
   
        ## Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS              ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port $HTTPport
            write-host "Calling ReserveURL for ReportManager, port $HTTPport"
            $r = $RSObject.ReserveURL('ReportManager',"http://+:$HTTPport",1033)
            CheckResult $r "ReserveURL for ReportManager port $HTTPport"
   
        write-host -foregroundcolor green "Open Firewall port for $HTTPport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $HTTPport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $HTTPport)” -Direction Inbound –Protocol TCP –LocalPort $HTTPport
            write-host "Added rule Report Server (TCP on port $HTTPport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
5. Egy HTTP-port a 80-as eltérő hello VM hozta létre, ha módosítani hello paraméter $HTTPport = 80-as.
6. Reporting Services jelenleg konfigurált hello parancsfájl. Ha a Reporting Services toorun hello parancsfájl, hello verzió részét módosíthatja hello elérési toohello névtér túl "v11", a Get-WmiObject hello utasítást.
7. Hello parancsprogrammal.

**Érvényesítési**: tooverify, amely hello alapvető jelentéskészítő kiszolgáló működőképes, lásd: hello [ellenőrizze hello konfigurációs](#verify-the-configuration) későbbi szakaszában talál.

### <a name="use-script-tooconfigure-hello-report-server-and-https"></a>Parancsfájl tooconfigure hello jelentéskészítő kiszolgáló és a HTTPS használatára
toouse Windows PowerShell tooconfigure hello jelentéskészítő kiszolgálón, a következő lépéseket teljes hello. hello konfiguráció tartalmazza a HTTPS-en nem HTTP.

1. Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra. A böngésző konfigurációtól függően előfordulhat felszólító toosave csatlakozni a virtuális gép toohello .rdp fájlt.
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif) Hello VM felhasználónév, a felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használja. 
   
    Például a következő kép hello, hello virtuális gép neve: **ssrsnativecloud** és hello felhasználónév **tesztfelhasználó néven**.
   
    ![bejelentkezési tartalmaz virtuális gép neve](./media/virtual-machines-windows-classic-ps-sql-report/IC764111.png)
2. Nyissa meg a virtuális gép hello, **Windows PowerShell ISE** rendszergazdai jogosultságokkal. a Windows server 2012 rendszerben alapértelmezés szerint telepítve van a PowerShell ISE hello. Ajánlott használnia hello ISE helyett a szabványos Windows PowerShell-ablakot, hogy hello parancsfájl beillesztése hello ISE, módosítsa hello parancsfájlt, és futtassa a hello parancsfájl.
3. parancsfájlok futtatása, futtassa a következő Windows PowerShell-paranccsal hello tooenable:
   
        Set-ExecutionPolicy RemoteSigned
   
    Ezt követően futtathatja a következő tooverify hello házirend hello:
   
        Get-ExecutionPolicy
4. A **Windows PowerShell ISE**, kattintson a hello **nézet** menüre, és kattintson **parancsfájl ablaktábla megjelenítése**.
5. Másolja az alábbi parancsfájlt, és illessze be a Windows PowerShell ISE panelbe hello hello.
   
        ## This script configures hello report server, including HTTPS
        $ErrorActionPreference = "Stop"
        $httpsport=443 # modify if you used a different port number when hello HTTPS endpoint was created.
   
        # You can run hello following command tooget (.cloudapp.net certificates) so you can copy hello thumbprint / certificate hash
        #dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
        #
        # hello certifacte hash is a REQUIRED parameter
        $certificatehash="" 
        # hello certificate hash should not contain spaces
   
        if ($certificatehash.Length -lt 1) 
        {
            write-error "certificatehash is a required parameter"
        } 
        # Certificates should be all lower case
        $certificatehash=$certificatehash.ToLower()
        $server = $env:COMPUTERNAME
        # If hello certificate is not a wildcard certificate, comment out hello following line, and enable hello full $DNSNAme reference.
        $DNSName="+"
        #$DNSName="$server.cloudapp.net"
        $DNSNameAndPort = $DNSName + ":$httpsport"
   
        ## Utility method for verifying an operation's result
        function CheckResult
        {
            param($wmi_result, $actionname)
            if ($wmi_result.HRESULT -ne 0) {
                write-error "$actionname failed. Error from WMI: $($wmi_result.Error)"
            }
        }
   
        $starttime=Get-Date
        write-host -foregroundcolor DarkGray $starttime StartTime
   
        ## ReportServer Database name - this can be changed if needed
        $dbName='ReportServer'
   
        write-host "hello script will use $DNSNameAndPort as hello DNS name and port" 
   
        ## Register for MSReportServer_ConfigurationSetting
        ## Change hello version portion of hello path too"v11" toouse hello script for SQL Server 2012
        $RSObject = Get-WmiObject -class "MSReportServer_ConfigurationSetting" -namespace "root\Microsoft\SqlServer\ReportServer\RS_MSSQLSERVER\v12\Admin"
   
        ## Reporting Services Report Server Configuration Steps
   
        ## 1. Setting hello web service URL ##
        write-host -foregroundcolor green "Setting hello web service URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for ReportServer site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportServerWebService','ReportServer',1033)
            CheckResult $r "SetVirtualDirectory for ReportServer"
   
        ## ReserveURL for ReportServerWebService - port 80 (for local usage)
            write-host 'Calling ReserveURL port 80'
            $r = $RSObject.ReserveURL('ReportServerWebService','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportServer port 80" 
   
        ## ReserveURL for ReportServerWebService - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportServerWebService',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportServer port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportServerWebService port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport, with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportServerWebService',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportServer port $httpsport" 
   
        ## 2. Setting hello Database ##
        write-host -foregroundcolor green "Setting hello Database"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## GenerateDatabaseScript - for creating hello database
            write-host "Calling GenerateDatabaseCreationScript for database $dbName"
            $r = $RSObject.GenerateDatabaseCreationScript($dbName,1033,$false)
            CheckResult $r "GenerateDatabaseCreationScript"
            $script = $r.Script
   
        ## Execute sql script toocreate hello database
            write-host 'Executing Database Creation Script'
            $savedcvd = Get-Location
            Import-Module SQLPS                    ## this automatically changes toosqlserver provider
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## GenerateGrantRightsScript 
            $DBUser = "NT Service\ReportServer"
            write-host "Calling GenerateDatabaseRightsScript with user $DBUser"
            $r = $RSObject.GenerateDatabaseRightsScript($DBUser,$dbName,$false,$true)
            CheckResult $r "GenerateDatabaseRightsScript"
            $script = $r.Script
   
        ## Execute grant rights script
            write-host 'Executing Database Rights Script'
            $savedcvd = Get-Location
            cd sqlserver:\
            Invoke-SqlCmd -Query $script
            Set-Location $savedcvd
   
        ## SetDBConnection - uses Windows Service (type 2), username is ignored
            write-host "Calling SetDatabaseConnection server $server, DB $dbName"
            $r = $RSObject.SetDatabaseConnection($server,$dbName,2,'','')
            CheckResult $r "SetDatabaseConnection"  
   
        ## 3. Setting hello Report Manager URL ##
   
        write-host -foregroundcolor green "Setting hello Report Manager URL"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## SetVirtualDirectory for Reports (Report Manager) site
            write-host 'Calling SetVirtualDirectory'
            $r = $RSObject.SetVirtualDirectory('ReportManager','Reports',1033)
            CheckResult $r "SetVirtualDirectory"
   
        ## ReserveURL for ReportManager  - port 80
            write-host 'Calling ReserveURL for ReportManager, port 80'
            $r = $RSObject.ReserveURL('ReportManager','http://+:80',1033)
            CheckResult $r "ReserveURL for ReportManager port 80"
   
        ## ReserveURL for ReportManager - port $httpsport
            write-host "Calling ReserveURL port $httpsport, for URL: https://$DNSNameAndPort"
            $r = $RSObject.ReserveURL('ReportManager',"https://$DNSNameAndPort",1033)
            CheckResult $r "ReserveURL for ReportManager port $httpsport" 
   
        ## CreateSSLCertificateBinding for ReportManager port $httpsport
            write-host "Calling CreateSSLCertificateBinding port $httpsport with certificate hash: $certificatehash"
            $r = $RSObject.CreateSSLCertificateBinding('ReportManager',$certificatehash,'0.0.0.0',$httpsport,1033)
            CheckResult $r "CreateSSLCertificateBinding for ReportManager port $httpsport" 
   
        write-host -foregroundcolor green "Open Firewall port for $httpsport"
        write-host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
   
        ## Open Firewall port for $httpsport
            New-NetFirewallRule -DisplayName “Report Server (TCP on port $httpsport)” -Direction Inbound –Protocol TCP –LocalPort $httpsport
            write-host "Added rule Report Server (TCP on port $httpsport) in Windows Firewall"
   
        write-host 'Operations completed, Report Server is ready'
        write-host -foregroundcolor DarkGray $starttime StartTime
        $time=Get-Date
        write-host -foregroundcolor DarkGray $time
6. Módosítsa a hello **$certificatehash** paraméter hello parancsfájlban:
   
   * Ez egy **szükséges** paraméter. Ha hello tanúsítvány érték nem mentette hello előző lépéseiből, valamelyikével módszerek toocopy hello tanúsítvány kivonatoló függvénnyel követően – hello tanúsítványok ujjlenyomat hello.:
     
       A virtuális gép hello nyissa meg a Windows PowerShell ISE és hello a következő parancsot:
     
           dir cert:\LocalMachine -rec | Select-Object * | where {$_.issuer -like "*cloudapp*" -and $_.pspath -like "*root*"} | select dnsnamelist, thumbprint, issuer
     
       hello kimeneti hasonló toohello következő fog kinézni. Hello parancsfájl egy üres sort ad vissza, ha hello virtuális gép nem rendelkezik konfigurált például tanúsítvány, hello című [toouse hello virtuális gépek önaláírt tanúsítvány](#to-use-the-virtual-machines-self-signed-certificate).
     
     VAGY
   * A virtuális gép futtatásához mmc.exe hello, és adja meg az hello **tanúsítványok** beépülő modult.
   * A hello **megbízható legfelső szintű hitelesítésszolgáltatók** csomópontot, kattintson duplán a tanúsítvány neve. Hello VM hello önaláírt tanúsítványát használja, ha hello tanúsítvány van elnevezve hello VM hello DNS-nevét, és végződik **cloudapp.net**.
   * Kattintson a hello **részletek** fülre.
   * Kattintson a **ujjlenyomat**. hello ujjlenyomat hello érték jelenik meg hello részleteket tartalmazó mező, például af 11 60 b6 4b 28 8 d 89 0a 82 12 ff 6b a9 c3 66 4f 31 90 48
   * **Hello parancsfájl futtatása előtt**, távolítsa el Between hello értékpárok hello szóközöket. Például af1160b64b288d890a8212ff6ba9c3664f319048
7. Módosítsa a hello **$httpsport** paraméter: 
   
   * Ha a HTTPS-végpont hello 443-as portot használja, majd nem kell tooupdate hello parancsfájlban ezt a paramétert. Ellenkező esetben használja hello HTTPS titkos végpont a virtuális gép hello konfigurálása során kiválasztott hello portértéket.
8. Módosítsa a hello **$DNSName** paraméter: 
   
   * hello parancsfájl úgy van konfigurálva, a helyettesítő tanúsítvány $DNSName = "+". Ha nem akarja tooconfigure helyettesítő tanúsítvány kötés, megjegyzéssé $DNSName = "+"és a következő sorban, hello teljes $DNSNAme hivatkozás, ## $DNSName="$server.cloudapp.net hello engedélyezése".
     
       Módosítsa a hello $DNSName értéket, ha nem szeretné a Reporting Services toouse hello virtuális gép DNS-nevét. Hello paraméter használatakor hello tanúsítványt kell használnia az ezt a nevet, és regisztrál egy DNS kiszolgálón lévő globálisan hello nevét.
9. Reporting Services jelenleg konfigurált hello parancsfájl. Ha a Reporting Services toorun hello parancsfájl, hello verzió részét módosíthatja hello elérési toohello névtér túl "v11", a Get-WmiObject hello utasítást.
10. Hello parancsprogrammal.

**Érvényesítési**: tooverify, amely hello alapvető jelentéskészítő kiszolgáló működőképes, lásd: hello [ellenőrizze hello konfigurációs](#verify-the-connection) későbbi szakaszában talál. tooverify hello tanúsítványkötés nyisson meg egy parancssort rendszergazdai jogosultságokkal, és futtassa a parancsot a következő hello:

    netsh http show sslcert

hello eredmény hello következőket tartalmazza:

    IP:port                      : 0.0.0.0:443

    Certificate Hash             : f98adf786994c1e4a153f53fe20f94210267d0e7

### <a name="use-configuration-manager-tooconfigure-hello-report-server"></a>Használja a Configuration Manager tooConfigure hello jelentéskészítő kiszolgáló
Ha nem szeretné, hogy toorun hello PowerShell parancsfájl tooconfigure hello jelentéskészítő kiszolgálón, lépésekkel hello Ez a szakasz toouse hello Reporting Services natív üzemmódú configuration manager tooconfigure hello jelentéskészítő kiszolgáló.

1. Hello a klasszikus Azure portálon, válassza ki hello virtuális gép, és kattintson a Csatlakozás gombra. Hello felhasználónév és jelszó hello virtuális gép létrehozásakor beállított használata.
   
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-report/IC650112.gif)
2. Futtassa a Windows update és a frissítések toohello VM telepítése. Hello virtuális gép újraindítására szükség, ha hello virtuális gép újraindítása, és csatlakozzon újra a virtuális gép toohello a klasszikus Azure portálon hello.
3. A virtuális gép hello hello Start menüben írja be a **Reporting Services** , és nyissa meg **Reporting Services Configuration Manager**.
4. Hagyja bejelölve az alapértelmezett értékeit hello **kiszolgálónév** és **jelentéskészítő kiszolgáló példánya**. Kattintson a **Connect** (Csatlakozás) gombra.
5. Hello bal oldali ablaktáblában kattintson **webes szolgáltatás URL-címe**.
6. Alapértelmezés szerint RS "Az összes hozzárendelt" IP-80-as HTTP-port van konfigurálva. tooadd HTTPS:
   
   1. A **SSL-tanúsítvány**: select hello tanúsítványt toouse, [a virtuális gép neve] például. cloudapp.net. Ha nincsenek felsorolva tanúsítványok, című hello **2. lépés: hozzon létre egy kiszolgálói tanúsítványt** olvashat, hogyan tooinstall és megbízhatósági hello hello VM-tanúsítványt.
   2. A **SSL-Port**: válassza ki a 443-as. Ha más magánhálózati port VM hello konfigurált hello HTTPS titkos végpont, használni ezt az értéket.
   3. Kattintson a **alkalmaz** és hello művelet toocomplete várja.
7. Hello bal oldali ablaktáblában kattintson **adatbázis**.
   
   1. Kattintson a **Databas módosítása**e.
   2. Kattintson a **hozzon létre egy új jelentéskészítő kiszolgáló adatbázisában** majd **következő**.
   3. Hagyja hello alapértelmezett **kiszolgálónév**: neve hello VM legyen, és hagyja hello alapértelmezett **hitelesítési típus** , **aktuális felhasználó** – **integrált biztonsági**. Kattintson a **Tovább** gombra.
   4. Hagyja hello alapértelmezett **adatbázisnév** , **ReportServer** kattintson **következő**.
   5. Hagyja hello alapértelmezett **hitelesítési típus** , **szolgáltatás hitelesítő adatai** kattintson **következő**.
   6. Kattintson a **következő** a hello **összegzés** lap.
   7. Hello konfiguráció befejeztével kattintson **Befejezés**.
8. Hello bal oldali ablaktáblában kattintson **Jelentéskezelő URL-címe**. Hagyja hello alapértelmezett **virtuális könyvtár** , **jelentések** kattintson **alkalmaz**.
9. Kattintson a **kilépési** tooclose hello Reporting Services Configuration Manager.

## <a name="step-4-open-windows-firewall-port"></a>4. lépés: A Windows tűzfal Port megnyitása
> [!NOTE]
> Ha egy hello parancsfájlok tooconfigure hello jelentéskészítő kiszolgálón, ez a szakasz kihagyhatja. hello parancsfájl lépés tooopen hello tűzfalportot tartalmazza. hello alapértelmezett port a 80-as HTTP és HTTPS – 443-as port volt.
> 
> 

tooconnect távolról tooReport kezelő vagy hello jelentést Server hello virtuális gépen, a TCP-végpont hello virtuális gép szükséges. Ugyanaz a hello VM tűzfal port szükséges tooopen hello. hello végpont jött létre, amikor a virtuális gép hello lett kiépítve.

Ez a szakasz hogyan tooopen hello tűzfalportot alapvető információkat nyújt. További információkért lásd: [beállítani a tűzfalat a jelentéskészítő kiszolgáló elérése](https://technet.microsoft.com/library/bb934283.aspx)

> [!NOTE]
> Ha hello parancsfájl tooconfigure hello jelentéskészítő kiszolgáló használja, kihagyhatja ebben a szakaszban. hello parancsfájl lépés tooopen hello tűzfalportot tartalmazza.
> 
> 

A magánhálózati port a HTTPS használatára konfigurált 443-astól eltérő, ha módosítsa megfelelően a következő parancsfájl hello. tooopen port **443-as** a Windows tűzfal hello, végezze el a következő hello:

1. Nyissa meg a Windows PowerShell ablakot rendszergazdai jogosultságokkal.
2. Hello a HTTPS-végpont a virtuális gép hello konfigurálása során a 443-astól eltérő port használata esetén a következő parancs hello hello port frissítése, és futtassa hello parancsot:
   
        New-NetFirewallRule -DisplayName “Report Server (TCP on port 443)” -Direction Inbound –Protocol TCP –LocalPort 443
3. Hello parancs befejeződésekor **Ok** hello parancssor jelenik meg.

tooverify, hogy hello port meg van nyitva, nyisson meg egy Windows PowerShell ablakot, és futtassa a következő parancs hello:

    get-netfirewallrule | where {$_.displayname -like "*report*"} | select displayname,enabled,action

## <a name="verify-hello-configuration"></a>Hello konfigurációjának ellenőrzése
rendszergazdai jogosultságokkal rendelkező tooverify hello alapvető jelentéskészítő kiszolgálói funkciók most működik, nyissa meg a böngészőt, és Tallózás toohello következő jelentést készít a kiszolgáló ad Jelentéskezelő URL-CÍMEK:

* Keresse meg a virtuális gép hello, a toohello jelentéskészítő kiszolgáló URL-címe:
  
        http://localhost/reportserver
* Keresse meg a virtuális gép hello, a toohello jelentés Manager URL-címe:
  
        http://localhost/Reports
* A helyi számítógépről, keresse meg a toohello **távoli** hello VM Manager jelentést. Hello DNS-név a következő példa megfelelő hello a frissítése. Amikor a rendszer kéri a jelszót, használja a hello rendszergazdai hitelesítő adatokat, akkor jön létre, amikor a virtuális gép hello lett kiépítve. hello felhasználói név van hello [tartomány]\[felhasználónév] formátumban, ahol a hello tartománya hello VM számítógép nevét, például ssrsnativecloud\testuser. Ha nem használja a HTTP**S**, távolítsa el a hello **s** hello URL-címben. Című rész hello Tovább információt a további felhasználók létrehozása a virtuális Gépen.
  
        https://ssrsnativecloud.cloudapp.net/Reports
* A helyi számítógépről keresse meg a toohello távoli jelentéskészítő kiszolgáló URL-címe. Hello DNS-név a következő példa megfelelő hello a frissítése. Ha HTTPS nem használ, távolítsa el a hello s hello URL-címben.
  
        https://ssrsnativecloud.cloudapp.net/ReportServer

## <a name="create-users-and-assign-roles"></a>Felhasználók létrehozása és szerepkörök hozzárendelése
Konfigurálásának és ellenőrzésének hello jelentés kiszolgáló, miután egy közös felügyeleti feladat toocreate egy vagy több felhasználó, és felhasználók tooReporting szolgáltatások szerepkörök hozzárendelése. További információkért lásd: hello következő:

* [Helyi felhasználói fiók létrehozása](https://technet.microsoft.com/library/cc770642.aspx)
* [Felhasználói hozzáférés tooa jelentéskészítő kiszolgáló (Jelentéskezelő)](https://msdn.microsoft.com/library/ms156034.aspx))
* [Létrehozása és kezelése a szerepkör-hozzárendelések](https://msdn.microsoft.com/library/ms155843.aspx)

## <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate és a jelentések közzététele toohello Azure virtuális gépen
hello következő táblázat összefoglalja hello beállítások elérhető toopublish meglévő kiszolgálóról származó jelentések egy helyszíni számítógép toohello jelentés hello Microsoft Azure rendszerű virtuális gépeken üzemeltet egy részénél:

* **RS.exe parancsfájl**: használata RS.exe parancsfájl toocopy jelentés elemeit és meglévő jelentés server tooyour Microsoft Azure virtuális gépen. További információkért lásd: hello szakasz "natív üzemmódú tooNative módban – Ez a Microsoft Azure virtuális gép" a [minta Reporting Services rs.exe parancsfájl tooMigrate jelentés kiszolgálók közötti tartalom](https://msdn.microsoft.com/library/dn531017.aspx).
* **Jelentéskészítő**: hello virtuális gépet tartalmaz hello kattintson-Microsoft SQL Server jelentéskészítő egyszer verzióját. toostart jelentés jelentéskészítő hello először hello virtuális gépen:
  
  1. A böngészőben rendszergazdai jogosultságokkal.
  2. Keresse meg a tooreport manager hello virtuális gépen, és kattintson a **jelentéskészítő** hello szalagon.
     
     További információkért lásd: [telepítése, eltávolítása és a jelentéskészítő támogató](https://technet.microsoft.com/library/dd207038.aspx).
* **SQL Server Data Tools: Virtuális gép**: Ha SQL Server 2012-ben létrehozott hello VM, akkor az SQL Server Data Tools hello virtuális gépre van telepítve, és lehet használt toocreate **jelentés Server projektek** és virtuális hello szóló jelentések gép. SQL Server Data Tools közzétehet hello jelentések toohello jelentéskészítő kiszolgáló hello virtuális gépen.
  
    Ha SQL server 2014 hello VM hozta létre, telepítheti az SQL Server Data Tools - BI visual Studio. További információkért lásd: hello következő:
  
  * [Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2013-hoz](https://www.microsoft.com/download/details.aspx?id=42313)
  * [Microsoft SQL Server Data Tools összetevővel - üzleti intelligencia, a Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=36843)
  * [SQL Server Data Tools összetevővel és az SQL Server Business Intelligence (SSDT-bA)](http://curah.microsoft.com/30004/sql-server-data-tools-ssdt-and-sql-server-business-intelligence)
* **SQL Server Data Tools: Távoli**: A helyi számítógépen, a Reporting Services-projekt létrehozása az SQL Server Data Tools összetevővel, amely tartalmazza a Reporting Services-jelentéseket. Hello projekt tooconnect toohello webes szolgáltatás URL-CÍMEK konfigurálása.
  
    ![SSRS-projekt SSDT projektjének tulajdonságai](./media/virtual-machines-windows-classic-ps-sql-report/IC650114.gif)
* **Parancsfájl használata**: használja a parancsfájl toocopy jelentés tartalmához. További információkért lásd: [minta Reporting Services rs.exe parancsfájl tooMigrate jelentés kiszolgálók közötti tartalom](https://msdn.microsoft.com/library/dn531017.aspx).

## <a name="minimize-cost-if-you-are-not-using-hello-vm"></a>Ha nem használja a virtuális gép hello minimalizálása érdekében a költség
> [!NOTE]
> toominimize költségek az az Azure virtuális gépek Ha nincsenek használatban, állítsa le a klasszikus Azure portálon hello VM hello. Hello Windows energiagazdálkodási beállításait hello VM le a virtuális gép tooshut belül használja, ha van is szó, ugyanaz a virtuális gép hello összeg hello. tooreduce költség, a klasszikus Azure portálon hello VM hello le tooshut van szüksége. Ha már nincs szüksége a virtuális gép hello, ne feledje toodelete hello virtuális gép, és hello hozzárendelt .vhd fájlok tooavoid tárolási költségek. További információkért lásd: gyakran ismételt kérdések hello szakasz [virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/).

## <a name="more-information"></a>További információ
### <a name="resources"></a>Erőforrások
* A hasonló tartalomhoz kapcsolódó tooa egyetlen kiszolgáló telepítéséhez SQL Server Business Intelligence és a SharePoint 2013, lásd: [a Windows PowerShell tooCreate egy Azure virtuális gép az SQL Server BI és a SharePoint 2013](https://msdn.microsoft.com/library/azure/dn385843.aspx).
* Többkiszolgálós telepítésben, SQL Server Business Intelligence és a SharePoint 2013-at, lásd: hasonló tartalom kapcsolódó tooa [központi telepítése az SQL Server Business Intelligence Azure virtuális gépek](https://msdn.microsoft.com/library/dn321998.aspx).
* Általános információk az SQL Server Business Intelligence Azure virtuális gépek, kapcsolódó toodeployments: [SQL Server Business Intelligence Azure virtuális gépek](virtual-machines-windows-classic-ps-sql-bi.md).
* További információ az Azure számítási díjakat hello költség: hello virtuális gépek lapján [Azure árképzési Számológép](https://azure.microsoft.com/pricing/calculator/?scenario=virtual-machines).

### <a name="community-content"></a>Közösségi tartalom
* Hogyan toocreate jelentéskészítési szolgáltatások natív mód jelentést parancsfájl használata nélkül server részletes ismertetését lásd: [üzemeltető SQL jelentési szolgáltatás az Azure virtuális gép](http://adititechnologiesblog.blogspot.in/2012/07/hosting-sql-reporting-service-on-azure.html).

### <a name="links-tooother-resources-for-sql-server-in-azure-vms"></a>Hivatkozások tooother erőforrások az SQL Server Azure virtuális gépeken
[SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md)

