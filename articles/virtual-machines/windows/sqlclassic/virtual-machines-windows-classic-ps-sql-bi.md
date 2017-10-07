---
title: aaaSQL Server Business Intelligence |} Microsoft Docs
description: "Ez a témakör hello klasszikus telepítési modellel létrehozott erőforrást használ, és az SQL Server rendszert futtató Azure virtuális gépek (VM) hello üzleti Üzletiintelligencia-funkcióit ismerteti."
services: virtual-machines-windows
documentationcenter: na
author: guyinacube
manager: erikre
editor: monicar
tags: azure-service-management
ms.assetid: c681e7a7-eeda-48aa-bc35-6277f4828244
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/30/2017
ms.author: asaxton
ms.openlocfilehash: e3288f0835d6c4a19baeeea5f6b65fec16cd751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-server-business-intelligence-in-azure-virtual-machines"></a>Az SQL Server Business Intelligence használata Azure-beli virtuális gépeken
> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

hello Microsoft Azure virtuális gép az SQL Server telepített tartalmazó lemezképek rendelkezik. hello SQL Server kiadásait támogatja a hello gyűjtemény lemezképei hello ugyanazokat a telepítési fájlokat tooon helyszíni számítógépek és a virtuális gépek telepítéséhez. Ez a témakör összefoglalja az SQL Server Business Intelligence (BI) hello hello képek telepített szolgáltatások és a virtuális gép kiépítése után szükséges konfigurációs lépéseket. Ez a témakör ismerteti a támogatott üzembe helyezési topológiák BI-funkciókat és ajánlott eljárások is.

## <a name="license-considerations"></a>Licenc kapcsolatos szempontok
Két módon toolicense SQL Server a Microsoft Azure virtuális gépek van:

1. Licenc mobilitási előnyeit, amelyek részei Software Assurance. További információkért lásd: [Azure frissítési garancián keresztüli Licenchordozhatósági](https://azure.microsoft.com/pricing/license-mobility/).
2. Az Azure virtuális gépek, a telepített SQL Server feldolgozási sebessége nagy. Lásd: "SQL Server" című szakaszában hello [Virtual Machines díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/#Sql).

A licencelési és a jelenlegi díjszabás további információkért lásd: [virtuális gépek Licensing FAQ](https://azure.microsoft.com/pricing/licensing-faq/%20/).

## <a name="sql-server-images-available-in-azure-virtual-machine-gallery"></a>SQL Server-rendszerképeit elérhető Azure virtuálisgép-katalógusban
hello Microsoft Azure virtuális gép számos olyan rendszerkép található, amely tartalmazza a Microsoft SQL Server rendelkezik. hello virtuálisgép-rendszerképek telepített hello szoftver hello hello operációs rendszer verziója és az SQL Server verziója hello függ. hello Azure virtuálisgép-katalógus elérhető lemezképeihez hello listája gyakran változik.

<!--![SQL image in azure VM gallery](./media/virtual-machines-windows-classic-ps-sql-bi/IC741367.png)-->
![SQL-lemezképet Azure virtuális gép gyűjteménye](./media/virtual-machines-windows-classic-ps-sql-bi/vm-sql-images.png)

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) hello következő PowerShell-parancsfájl listáját adja vissza hello Azure-rendszerképek hello ImageName "SQL-kiszolgáló" tartalmazó:

    # assumes you have already uploaded a management certificate tooyour Microsoft Azure Subscription. View hello thumbprint value from hello "Subscriptions" menu in Azure portal.

    $subscriptionID = ""    # REQUIRED: Provide your subscription ID.
    $subscriptionName = "" # REQUIRED: Provide your subscription name.
    $thumbPrint = "" # REQUIRED: Provide your certificate thumbprint.
    $certificate = Get-Item cert:\currentuser\my\$thumbPrint # REQUIRED: If your certificate is in a different store, provide it here.-Ser  store is hello one specified with hello -ss parameter on MakeCert

    Set-AzureSubscription -SubscriptionName $subscriptionName -Certificate $certificate -SubscriptionID $subscriptionID

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2016"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2016*"} | select imagename,category, location, label, description

    Write-Host -foregroundcolor green "List of available gallery images where imagename contains 2014"
    Write-Host -foregroundcolor green ">>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>>"
    get-azurevmimage | where {$_.ImageName -Like "*SQL-Server-2014*"} | select imagename,category, location, label, description

Kiadásainak és az SQL Server által támogatott szolgáltatások további információkért lásd: hello következő:

* [SQL Server kiadásai](https://www.microsoft.com/server-cloud/products/sql-server-editions/#fbid=Zae0-E6r5oh)
* [Szolgáltatások támogatott által hello kiadás az SQL Server 2016](https://msdn.microsoft.com/library/cc645993.aspx)

### <a name="bi-features-installed-on-hello-sql-server-virtual-machine-gallery-images"></a>BI szolgáltatások telepítése az SQL Server virtuális gépen lévő képek hello
hello következő táblázat összefoglalja az SQL Server hello közös Microsoft Azure virtuális gépen lévő képek telepített hello üzleti intelligencia szolgáltatások:

* SQL Server 2016 SP1 Enterprise
* Az SQL Server 2016 SP1 Standard
* SQL Server 2014 SP2 Enterprise
* Az SQL Server 2014 SP2 Standard
* SQL Server 2012 SP3 Enterprise
* Az SQL Server 2012 SP3 Standard

| SQL Server BI szolgáltatás | Hello gyűjtemény kép telepítve | Megjegyzések |
| --- | --- | --- |
| **Jelentéskészítési szolgáltatások natív mód** |Igen |Telepített, de a konfiguráció – beleértve az hello jelentés manager URL-címet igényel. Című rész hello [Reporting Services konfigurálása](#configure-reporting-services). |
| **Reporting Services – SharePoint módban** |Nem |hello Microsoft Azure virtuális gép gyűjteménye a rendszerkép nem tartalmaz a SharePoint vagy a SharePoint telepítési fájlok. <sup>1</sup> |
| **Analysis Services többdimenziós és az adatok adatbányászati (OLAP)** |Igen |Telepített és konfigurált, hello alapértelmezett Analysis Services-példány |
| **Analysis Services táblázatos** |Nem |Támogatott az SQL Server 2012, 2014 és 2016 képek, de az nincs telepítve alapértelmezés szerint. Egy másik Analysis Services-példány telepítését. Című rész hello más SQL Server-szolgáltatások és szolgáltatások telepítése ebben a témakörben. |
| **Analysis Services PowerPivot sharepointhoz** |Nem |hello Microsoft Azure virtuális gép gyűjteménye a rendszerkép nem tartalmaz a SharePoint vagy a SharePoint telepítési fájlok. <sup>1</sup> |

<sup>1</sup> kapcsolatban további tájékoztatást a SharePoint és az Azure virtuális gépek [a SharePoint 2013-hoz a Microsoft Azure architektúrák](https://technet.microsoft.com/library/dn635309.aspx) és [SharePoint telepítési Microsoft Azure virtuális gépeken](https://www.microsoft.com/download/details.aspx?id=34598).

![PowerShell](./media/virtual-machines-windows-classic-ps-sql-bi/IC660119.gif) Futtassa a következő PowerShell-parancs tooget hello telepített szolgáltatások, amelyek tartalmazzák az "SQL" hello szolgáltatás neve.

    get-service | Where-Object{ $_.DisplayName -like '*SQL*' } | Select DisplayName, status, servicetype, dependentservices | format-Table -AutoSize

## <a name="general-recommendations-and-best-practices"></a>Általános javaslatok és ajánlott eljárások
* ajánlott méret, a virtuális gép minimális hello **A3** SQL Server Enterprise Edition használatakor. Hello **A4** virtuálisgép-méret ajánlott az SQL Server BI Analysis Services és a Reporting Services telepítéséhez.
  
    Az aktuális Virtuálisgép-méretek hello információkért lásd: [Azure virtuálisgép-méretek](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* A Lemezkezelés ajánlott toostore adatokat, a napló és a meghajtók biztonságimásolat-fájlok eltérő **C**: és **D**:. Hozzon létre például egy adatlemezt **E**: és **F**:.
  
  * hello meghajtó gyorsítótárazási házirend hello alapértelmezett meghajtó **C**: nincs optimalizálva az adatai.
  * Hello **D**: elsősorban a lapozófájlt hello használt ideiglenes meghajtó. Hello **D**: meghajtó nem őrzi meg, és nem menti a blob Storage tárolóban. Felügyeleti feladatok, például a virtuálisgép-méret módosítása toohello alaphelyzetbe hello **D**: meghajtó. Túl ajánlott**nem** hello használata **D**: adatbázis-fájlok, beleértve a tempdb meghajtó.
    
    A lemezek létrehozása és további információkért lásd: [hogyan tooAttach egy virtuális gép adatlemez tooa](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* Állítsa le, vagy nem tervezi toouse szolgáltatások eltávolítása. Például ha hello virtuális gép csak a Reporting Serviceshez, állítsa le vagy távolítsa el az Analysis Services és az SQL Server Integration Services. hello példánycsoportokat példája hello szolgáltatás, amely alapértelmezés szerint elindul.
  
    ![SQL Server-szolgáltatások](./media/virtual-machines-windows-classic-ps-sql-bi/IC650107.gif)
  
  > [!NOTE]
  > SQL Server adatbázismotor hello hello támogatott BI esetekben szükséges. A virtuális gép egyetlen kiszolgáló topológiában hello adatbázismotor kell futó toobe hello azonos virtuális gép.
  
    További információkért tekintse meg a hello következőt: [Reporting Services eltávolítása](https://msdn.microsoft.com/library/hh479745.aspx) és [távolítsa el az Analysis Services példány](https://msdn.microsoft.com/library/ms143687.aspx).
* Ellenőrizze **Windows Update** az új "fontos frissítések". a Microsoft Azure virtuálisgép-rendszerképek hello gyakran frissülnek; azonban a fontos frissítések válhat elérhető **Windows Update** után utolsó frissítésének hello Virtuálisgép-lemezkép.

## <a name="example-deployment-topologies"></a>Példa üzembe helyezési topológiák
hello az alábbiakban például a Microsoft Azure virtuális gépek használó központi telepítések. az alábbi ábrákon hello topológiák hello lehetséges topológiákat használhatja az SQL Server BI szolgáltatás és a Microsoft Azure virtuális gépek csak közé tartoznak.

### <a name="single-virtual-machine"></a>Egyetlen virtuális gép
Analysis Services, Reporting Services, az SQL Server adatbázismotor és az adatforrások egy virtuális gépen.

![üzleti intelligencia számvitel a forgatókönyvet, és 1 virtuális gép](./media/virtual-machines-windows-classic-ps-sql-bi/IC650108.gif)

### <a name="two-virtual-machines"></a>Két virtuális gép
* Analysis Services, a Reporting Services és az SQL Server adatbázismotor hello egy virtuális gépen. A központi telepítés hello jelentés server-adatbázisokat tartalmazza.
* A második virtuális gép adatforrások. hello második virtuális gép tartalmazza az SQL Server adatbázismotor adatforrásként.

![üzleti intelligencia iaas forgatókönyv 2 virtuális gépekkel](./media/virtual-machines-windows-classic-ps-sql-bi/IC650109.gif)

### <a name="mixed-azure--data-on-azure-sql-database"></a>Vegyes Azure – adatok az Azure SQL-adatbázis
* Analysis Services, a Reporting Services és az SQL Server adatbázismotor hello egy virtuális gépen. A központi telepítés hello jelentés server-adatbázisokat tartalmazza.
* Az adatforrás az Azure SQL database.

![üzleti intelligencia iaas forgatókönyvek vm és adatforrásként AzureSQL](./media/virtual-machines-windows-classic-ps-sql-bi/IC650110.gif)

### <a name="hybrid-data-on-premises"></a>Hibrid – helyszíni adatok
* Telepítési példában szereplő Analysis Services, a Reporting Services és az SQL Server adatbázismotor hello futtatni, egy virtuális gépen. Virtuálisgép-gazdagépek hello hello jelentés server-adatbázisokat. hello virtuális gép olyan Azure virtuális hálózat használatával a helyi tartomány, vagy néhány más VPN-megoldás tunneling illesztett tooan.
* Az adatforrás a helyszíni.

![üzleti intelligencia iaas-forgatókönyvek virtuális gép és a helyszíni adatforrások](./media/virtual-machines-windows-classic-ps-sql-bi/IC654384.gif)

## <a name="reporting-services-native-mode-configuration"></a>A Reporting Services natív mód konfigurálása
hello virtuális gép gyűjtemény lemezkép az SQL Server jelentéskészítési szolgáltatások natív mód telepítve, tartalmazza, azonban hello jelentéskészítő kiszolgálón nincs konfigurálva. Ebben a szakaszban található lépéseket hello hello Reporting Services jelentéskészítő kiszolgáló konfigurálása. További információk a jelentéskészítési szolgáltatások natív mód beállítása, a következő témakörben: [telepítése Reporting Services natív mód jelentéskészítő kiszolgáló (SSRS)](https://msdn.microsoft.com/library/ms143711.aspx).

> [!NOTE]
> A Windows PowerShell parancsfájlok tooconfigure hello jelentéskészítő kiszolgáló használó hasonló tartalom, lásd: [használja a Powershellt tooCreate egy Azure virtuális gép, egy natív mód jelentéskészítő kiszolgáló](../classic/ps-sql-report.md).

### <a name="connect-toohello-virtual-machine-and-start-hello-reporting-services-configuration-manager"></a>Csatlakozás a virtuális gép toohello, és indítsa el a Reporting Services Configuration Manager hello
Kapcsolódás Azure virtuális gép tooan két gyakori munkafolyamat van:

* hello, a tooconnect kattintson hello hello virtuális gép nevét, majd **Connect**. A távoli asztali kapcsolat megnyitása és hello számítógépnév automatikusan feltöltődik értékkel.
  
    ![Csatlakoztassa tooazure virtuális gépet](./media/virtual-machines-windows-classic-ps-sql-bi/IC650112.gif)
* Csatlakoztassa a toohello virtuális gépet a Windows távoli asztali kapcsolat. A távoli asztal hello hello a felhasználói felületen:
  
  1. Típus hello **felhőszolgáltatás neve** hello számítógép neve.
  2. Írja be a kettőspont (:), és hello hello TCP távoli asztali végpont konfigurált nyilvános port számát.
     
      Myservice.cloudapp.NET:63133
     
      További információkért lásd: [Mi az a cloud service?](https://azure.microsoft.com/manage/services/cloud-services/what-is-a-cloud-service/).


**Indítsa el a Reporting Services Konfigurációkezelő**

A **Windows Server 2012/2016**:

1. A hello **Start** írja be **Reporting Services** toosee azokat az alkalmazásokat.
2. Kattintson a jobb gombbal **Reporting Services Configuration Manager** kattintson **Futtatás rendszergazdaként**.

A **Windows Server 2008 R2**:

1. Kattintson a **Start**, és kattintson a **minden program**.
2. Kattintson a **Microsoft SQL Server 2016**.
3. Kattintson a **konfigurációs eszközök**.
4. Kattintson a jobb gombbal **Reporting Services Configuration Manager** kattintson **Futtatás rendszergazdaként**.

Vagy:

1. Kattintson a **Start**.
2. A hello **Keresés programokban és fájlokban** párbeszédpanelen írja be a **a reporting services**. Ha Windows Server 2012 hello virtuális gép fut, írja be a **a reporting services** hello Windows Server 2012 kezdőképernyőjén.
3. Kattintson a jobb gombbal **Reporting Services Configuration Manager** kattintson **Futtatás rendszergazdaként**.
   
    ![Keresse meg az ssrs configuration Managerben](./media/virtual-machines-windows-classic-ps-sql-bi/IC650113.gif)

### <a name="configure-reporting-services"></a>Reporting Services konfigurálásához
**Szolgáltatásfiók és a webszolgáltatás URL-címe:**

1. Ellenőrizze a hello **kiszolgálónév** hello helyi kiszolgáló nevét, és kattintson **Connect**.
2. Üres megjegyzés hello **jelentéskészítő kiszolgáló adatbázisának neve**. hello adatbázis létrehozása hello konfigurációs befejezéséről.
3. Ellenőrizze a hello **jelentéskészítő kiszolgáló állapota** van **elindítva**. Ha azt szeretné, hogy tooverify hello szolgáltatást a Windows Kiszolgálókezelő, hello szolgáltatást-e a hello **SQL Server Reporting Services** Windows-szolgáltatás.
4. Kattintson a **szolgáltatásfiók** és szükség szerint módosítsa a hello fiók. Hello virtuális gépet egy tartományhoz nem csatlakoztatott környezetben használata esetén hello beépített **ReportServer** fiókja is elegendő. A szolgáltatásfiókról hello további információkért lásd: [szolgáltatásfiók](https://msdn.microsoft.com/library/ms189964.aspx).
5. Kattintson a **webes szolgáltatás URL-címe** hello bal oldali ablaktáblán.
6. Kattintson a **alkalmaz** tooconfigure hello alapértelmezett értékeket.
7. Megjegyzés: hello **jelentéskészítő kiszolgáló Web Service URL-címei**. Vegye figyelembe, hello alapértelmezett TCP-port 80 hello URL-cím része. Egy későbbi lépésben hozzon létre egy Microsoft Azure virtuális gép végpontjának hello port.
8. A hello **eredmények** ablaktáblában ellenőrizze hello műveletek sikeresen befejeződtek.

**Adatbázis:**

1. Kattintson a **adatbázis** hello bal oldali ablaktáblán.
2. Kattintson a **adatbázis**.
3. Győződjön meg arról **hozzon létre egy új jelentéskészítő kiszolgáló adatbázisában** van kijelölve, majd kattintson a Tovább gombra.
4. Győződjön meg arról **kiszolgálónév** kattintson **kapcsolat tesztelése**.
5. Ha hello eredmény **kapcsolat tesztelése sikerült**, kattintson a **OK** majd **következő**.
6. Megjegyzés: hello adatbázisnév **ReportServer** és hello **jelentéskészítő kiszolgáló mód** van **natív** kattintson **következő**.
7. Kattintson a **következő** a hello **hitelesítő adatok** lap.
8. Kattintson a **következő** a hello **összegzés** lap.
9. Kattintson a **következő** a hello **előrehaladás és befejezési** lap.

**Webes portál URL-címe vagy a Jelentéskezelő URL-címe 2012 és a 2014:**

1. Kattintson a **webes portál URL-CÍMÉT**, vagy **Jelentéskezelő URL-címe** 2014 és 2012 hello bal oldali ablaktáblán.
2. Kattintson az **Alkalmaz** gombra.
3. A hello **eredmények** ablaktáblában ellenőrizze hello műveletek sikeresen befejeződtek.
4. Kattintson a **kilépési**.

A jelentéskészítő kiszolgáló engedélyek információkért lásd: [engedélyek megadása natív mód jelentéskészítő kiszolgálón](https://msdn.microsoft.com/library/ms156014.aspx).

### <a name="browse-toohello-local-report-manager"></a>Keresse meg a toohello helyi Jelentéskezelő
tooverify hello, Tallózás tooreport manager konfigurálása a virtuális gép hello.

1. Hello VM indítsa el az Internet Explorer rendszergazdai jogosultságokkal.
2. Keresse meg a virtuális gép hello toohttp://localhost/reports.

### <a name="tooconnect-tooremote-web-portal-or-report-manager-for-2014-and-2012"></a>tooConnect tooRemote webes portál vagy a Jelentéskezelő 2014 és 2012
Ha azt szeretné, tooconnect toohello webes portál vagy a Jelentéskezelő 2014 és 2012 hello virtuális gép távoli számítógépről, hozzon létre egy új virtuális gép TCP-végponton. Alapértelmezés szerint hello jelentéskészítő kiszolgáló figyel a HTTP-kérelmek **80-as port**. Ha hello jelentéskészítő kiszolgáló URL-címek toouse egy másik portot adja meg, az utasításoknak hello eltérő portszámot kell megadnia.

1. Hozzon létre egy virtuális gép a TCP 80-as Port hello végpontra. További információ: hello [virtuális gép végpontok és tűzfalportok](#virtual-machine-endpoints-and-firewall-ports) szakasz ebben a dokumentumban.
2. Nyissa meg a 80-as portot a hello virtuális gép tűzfal.
3. Keresse meg a toohello webes portál vagy a Jelentéskezelő, Azure virtuális gépek használatának **DNS-név** hello URL-címben hello kiszolgáló neveként. Példa:
   
    **Jelentéskészítő kiszolgáló**: http://uebi.cloudapp.net/reportserver **webes portál**: http://uebi.cloudapp.net/reports
   
    [Beállítani a tűzfalat a jelentéskészítő kiszolgáló elérése](https://msdn.microsoft.com/library/bb934283.aspx)

### <a name="toocreate-and-publish-reports-toohello-azure-virtual-machine"></a>tooCreate és a jelentések közzététele toohello Azure virtuális gépen
hello következő táblázat összefoglalja hello beállítások elérhető toopublish meglévő kiszolgálóról származó jelentések egy helyszíni számítógép toohello jelentés hello Microsoft Azure rendszerű virtuális gépeken üzemeltet egy részénél:

* **Jelentéskészítő**: hello virtuális gépet tartalmaz hello kattintson-Microsoft SQL Server jelentéskészítő az SQL 2014 és az 2012 egyszer verzióját. toostart jelentés jelentéskészítő hello először az SQL 2016 hello virtuális gépen:
  
  1. A böngészőben rendszergazdai jogosultságokkal.
  2. Keresse a toohello webportál hello virtuális gépre, és válassza ki a hello **letöltése** hello jobb felső ikonra.
  3. Válassza ki **jelentéskészítő**.
     
     További információkért lásd: [Start jelentéskészítő](https://msdn.microsoft.com/library/ms159221.aspx).
* **SQL Server Data Tools**: virtuális gép: SQL Server Data Tools hello virtuális gépre van telepítve, és lehet használt toocreate **jelentés Server projektek** és jelentések hello virtuális gépen. SQL Server Data Tools közzétehet hello jelentések toohello jelentéskészítő kiszolgáló hello virtuális gépen.
* **SQL Server Data Tools: Távoli**: A helyi számítógépen, a Reporting Services-projekt létrehozása az SQL Server Data Tools összetevővel, amely tartalmazza a Reporting Services-jelentéseket. Hello projekt tooconnect toohello webes szolgáltatás URL-CÍMEK konfigurálása.
  
    ![SSRS-projekt SSDT projektjének tulajdonságai](./media/virtual-machines-windows-classic-ps-sql-bi/IC650114.gif)
* Hozzon létre egy. Amely kapcsolatos jelentéseket tartalmazza, majd töltse fel és hello meghajtó csatlakoztatása virtuális merevlemezen.
  
  1. Hozzon létre egy. Virtuális merevlemez-meghajtóról a helyi számítógépen, amely tartalmazza a jelentéseket.
  2. Hozzon létre és telepítsen egy felügyeleti tanúsítványt.
  3. Hello VHD-fájl tooAzure hello Add-AzureVHD parancsmaggal töltse fel [létrehozása és feltöltése a Windows Server VHD tooAzure](../classic/createupload-vhd.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
  4. Hello lemez toohello virtuális géphez csatolása.

## <a name="install-other-sql-server-services-and-features"></a>Más SQL Server-szolgáltatások és szolgáltatások telepítése
tooinstall további SQL Server szolgáltatásokat, például az Analysis Services táblázatos módban hello SQL server telepítése varázsló futtatása. hello telepítőfájljainak hello virtuális gép helyi lemezen vannak.

1. Kattintson a **Start** majd **minden program**.
2. Kattintson a **Microsoft SQL Server 2016**, **Microsoft SQL Server 2014** vagy **Microsoft SQL Server 2012** majd **konfigurációs eszközök**.
3. Kattintson a **SQL Server telepítési központjának**.

Vagy C:\SQLServer_13.0_full\setup.exe, C:\SQLServer_12.0_full\setup.exe vagy C:\SQLServer_11.0_full\setup.exe futtatása

> [!NOTE]
> hello első alkalommal futtatja az SQL Server telepítése beállítása további fájlok letölthetők és hello virtuális gép újraindítását és újra kell indítania az SQL Server telepítése szükséges.
> 
> Ha toorepeatedly kell ki a Microsoft Azure virtuális gép hello hello lemezkép testreszabása, érdemes lehet létrehozni a saját SQL Server-rendszerképet. Analysis Services SysPrep funkciót az SQL Server 2012 SP1 CU2 volt engedélyezve. További információkért lásd: [telepítése az SQL Server SysPrep használata szempontjai](https://msdn.microsoft.com/library/ee210754.aspx) és [Sysprep támogatási kiszolgálói szerepköre tekintetében](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).
> 
> 

### <a name="tooinstall-analysis-services-tabular-mode"></a>tooInstall Analysis Services táblázatos módban
Ebben a szakaszban lévő lépéseket hello **összefoglalója** hello Analysis Services táblázatos módban telepítését. További információkért lásd: hello következő:

* [Telepítse az Analysis Services táblázatos módban](https://msdn.microsoft.com/library/hh231722.aspx)
* [A táblázatos modellezési (Adventure Works oktatóanyag)](https://msdn.microsoft.com/library/140d0b43-9455-4907-9827-16564a904268)

**tooInstall Analysis Services táblázatos módban:**

1. Hello SQL Server telepítése varázslóban kattintson **telepítési** a hello bal oldali ablaktáblán, és kattintson a **új SQL server önálló telepítése vagy szolgáltatások tooan meglévő telepítési hozzáadása**.
   
   * Ha megjelenik a hello **Tallózás a mappák**, keresse meg a tooc:\SQLServer_13.0_full, c:\SQLServer_12.0_full vagy c:\SQLServer_11.0_full, és kattintson **Ok**.
2. Kattintson a **következő** on hello termék frissítések lap.
3. A hello **telepítési típus** lapon jelölje be **az SQL Server új telepítés** kattintson **következő**.
4. A hello **telepítési szerepkör** kattintson **SQL Server-szolgáltatások telepítése**.
5. A hello **szolgáltatásválasztást** kattintson **Analysis Services**.
6. A hello **példány konfigurációja** írja be egy nevet, például a **táblázatos** történő **nevű példány** és **példányazonosító** szöveg jelölőnégyzetéből.
7. A hello **Analysis Services konfigurációs** lapon jelölje be **táblázatos módban**. Hello aktuális felhasználó toohello rendszergazdai engedélyek listát vesznek fel.
8. Végezze el, és zárja be az SQL Server telepítővarázsló hello.

## <a name="analysis-services-configuration"></a>Az Analysis Services beállítása
### <a name="remote-access-tooanalysis-services-server"></a>Távoli hozzáférés tooAnalysis Services-kiszolgáló
Analysis Services-kiszolgáló csak a windows-hitelesítést támogatja. Analysis Services tooaccess távolról a ügyfélalkalmazásokat, például az SQL Server Management Studio vagy SQL Server Data Tools összetevővel, hello virtuális gép a beavatkozását toobe illesztett tooyour helyi tartomány, Azure virtuális hálózat használatával. További információ: [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md).

A **alapértelmezett példány** TCP porton figyel az Analysis Services **2383**. Nyissa meg a hello port hello virtuális gépek tűzfalon. Egy fürtözött nevű Analysis Services-példány is figyeli a porton **2383**.

Az egy **megnevezett példány** az Analysis Services hello SQL Server tallózó szolgáltatásának kell toomanage port hozzáférés. hello SQL Server Browser alapértelmezett beállítások mellett akkor port **2382**.

A virtuális gépek tűzfal hello, nyissa meg a portot **2382** , és hozzon létre egy statikus Analysis Services példány port neve.

1. már meglévő tooverify portok hello virtuális gép, és melyik folyamat használja az hello portokat használja, futtassa a következő parancs rendszergazda jogosultságokkal rendelkező hello:
   
        netstat /ao
2. Használja az SQL Server Management Studio toocreate egy statikus Analysis Services nevű példány port "Port" frissítésével táblázatos AS értéket példány általános tulajdonságok. További információkért lásd: a "Rögzített port használatára egy alapértelmezett vagy megnevezett példány" hello [hello Windows tűzfal tooAllow Analysis Services használatához konfigurálása](https://msdn.microsoft.com/library/ms174937.aspx#bkmk_fixed).
3. Indítsa újra az Analysis Services szolgáltatás hello hello táblázatos példányát.

További információ: hello **virtuális gép végpontok és tűzfalportok** szakasz ebben a dokumentumban.

## <a name="virtual-machine-endpoints-and-firewall-ports"></a>Virtuális gép végpontokat és a tűzfal portok
Ez a szakasz a Microsoft Azure virtuális gép végpontok toocreate és portok tooopen hello virtuális gép tűzfalak foglalja össze. hello következő táblázat összefoglalja hello **TCP** toocreate végpontjainak és hello portok tooopen hello virtuális gépek tűzfal portjai.

* Ha egy virtuális használ, és hello következő két elem teljesül, toocreate VM végpontok nem szükséges, és nem kell tooopen hello portok a tűzfalon hello hello VM.
  
  * Nem távoli kapcsolódás a virtuális gép hello toohello az SQL Server szolgáltatásai. A távoli asztali kapcsolat toohello VM létrehozó és hello SQL Server funkcióinak helyileg a virtuális gép hello nem tekinthető a távoli kapcsolat toohello SQL Server szolgáltatásokat.
  * Ne csatlakozzon hello VM tooan helyszíni tartományban Azure virtuális hálózat vagy egy másik VPN-bújtatás megoldás keresztül.
* Ha hello virtuális gép nem csatlakoztatott tooa tartományhoz, de azt szeretné, tooremotely csatlakozás toohello SQL Server szolgáltatásokat a virtuális Gépen:
  
  * Hello portok megnyitása hello a virtuális gép hello.
  * Hozzon létre virtuális gép végpontjai hello feljegyzett portok (*).
* Ha hello virtuális gép csatlakoztatott tooa tartományban egy VPN-alagúton, például az Azure virtuális hálózat használatával, majd hello végpontok esetén nincs szükség. Azonban hello portok megnyitása hello a virtuális gép hello.
  
  | Port | Típus | Leírás |
  | --- | --- | --- |
  | **80** |TCP |Jelentéskészítő kiszolgáló távelérés (*). |
  | **1433** |TCP |SQL Server Management Studio (*). |
  | **1434** |UDP |SQL Server Browser bővítményt. Ez szükséges, ha a virtuális gép csatlakoztatott tooa tartomány hello. |
  | **2382** |TCP |SQL Server Browser bővítményt. |
  | **2383** |TCP |SQL Server Analysis Services alapértelmezett példány, fürtözött elnevezett példányok. |
  | **Felhasználó által definiált** |TCP |Hozzon létre egy statikus Analysis Services példány port nevű úgy dönt, a portszámot, és majd feloldása hello tűzfal hello portszámát. |

Végpontok létrehozásával kapcsolatos további információkért tekintse meg a hello következőt:

* Hozza létre a végpontok:[hogyan tooSet be végpontok tooa virtuális gép](../classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).
* SQL Server: A hello "Konfigurációs lépéseket tooconnect toohello virtuális gép használata az SQL Server Management Studio" című szakaszában talál [Azure SQL Server virtuális gépek kiépítése](../sql/virtual-machines-windows-portal-sql-server-provision.md).

hello alábbi ábrán látható a virtuális gép tűzfal tooallow távelérési toofeatures hello hello portok tooopen és hello VM-összetevőket.

![portok tooopen üzleti intelligencia alkalmazásokhoz az Azure virtuális gépeken](./media/virtual-machines-windows-classic-ps-sql-bi/IC654385.gif)

## <a name="resources"></a>Erőforrások
* Hello terméktámogatási irányelveiből hello Azure virtuális gép környezetben használt Microsoft server szoftver. a következő témakör hello például a BitLocker, a Feladatátvételi fürtszolgáltatás és a hálózati terheléselosztás szolgáltatás támogatását összegzi. [Microsoft server szoftver támogatása az Azure virtuális gépek](http://support.microsoft.com/kb/2721672).
* [SQL Server Azure virtuális gépek – áttekintés](../sql/virtual-machines-windows-sql-server-iaas-overview.md)
* [Virtuális gépek](https://azure.microsoft.com/documentation/services/virtual-machines/)
* [Az Azure SQL Server virtuális gépek kiépítése](../sql/virtual-machines-windows-portal-sql-server-provision.md)
* [Hogyan tooAttach egy adatlemez tooa virtuális gép](../classic/attach-disk.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
* [Egy adatbázis tooSQL kiszolgálót egy Azure virtuális gép áttelepítése](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json)
* [Kiszolgáló üzemmód egy Analysis Services-példány hello meghatározása](https://msdn.microsoft.com/library/gg471594.aspx)
* [Többdimenziós modellezési (Adventure Works oktatóanyag)](https://technet.microsoft.com/library/ms170208.aspx)
* [Azure dokumentációs központban](https://azure.microsoft.com/documentation/)
* [Hibrid környezetben a Power BI használatával](https://msdn.microsoft.com/library/dn798994.aspx)

> [!NOTE]
> [Küldje el visszajelzését és kapcsolattartási adatokat a Microsoft SQL Server-csatlakozás](https://connect.microsoft.com/SQLServer/Feedback)

### <a name="community-content"></a>Közösségi tartalom
* [Az Azure SQL-adatbázisok felügyelete-a PowerShell használatával](http://blogs.msdn.com/b/windowsazure/archive/2013/02/07/windows-azure-sql-database-management-with-powershell.aspx)

