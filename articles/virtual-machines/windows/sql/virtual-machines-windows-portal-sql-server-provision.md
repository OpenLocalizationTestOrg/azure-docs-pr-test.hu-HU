---
title: "egy SQL Server virtuális gép aaaProvision |} Microsoft Docs"
description: "Hozzon létre, és csatlakoztassa tooa SQL Server virtuális gépet az Azure-ban hello portálon. Ez az oktatóanyag hello Resource Manager módot használja."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.assetid: 1aff691f-a40a-4de2-b6a0-def1384e086e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 08/14/2017
ms.author: jroth
ms.openlocfilehash: acb52b180103d83715b51b46e2519211c8f0e362
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="provision-a-sql-server-virtual-machine-in-hello-azure-portal"></a>Egy SQL Server rendszerű virtuális gép a hello Azure-portálon
> [!div class="op_single_selector"]
> * [Portál](virtual-machines-windows-portal-sql-server-provision.md)
> * [PowerShell](virtual-machines-windows-ps-sql-create.md)
> 
> 

A végpont oktatóanyag bemutatja, hogyan toouse hello Azure portál tooprovision egy SQL Server rendszerű virtuális gép.

hello Azure virtuális gép (VM) számos olyan rendszerkép található, amely tartalmazza a Microsoft SQL Server rendelkezik. Mindössze néhány kattintással jelöljön ki egy hello SQL virtuális gép lemezképek hello gyűjteményből, és építse ki azt az Azure környezetben.

Az oktatóanyag során az alábbi lépéseket fogja végrehajtani:

* [Válassza ki az SQL Virtuálisgép-rendszerkép hello gyűjteményből](#select-a-sql-vm-image-from-the-gallery)
* [Konfigurálja és hello virtuális gép létrehozása](#configure-the-vm)
* [Nyissa meg a virtuális gép hello a távoli asztal](#open-the-vm-with-remote-desktop)
* [Távoli csatlakozás tooSQL kiszolgáló](#connect-to-sql-server-remotely)

## <a name="select-a-sql-vm-image-from-hello-gallery"></a>Válassza ki az SQL Virtuálisgép-rendszerkép hello gyűjteményből

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) a fiókjával.

   > [!NOTE]
   > Ha nem rendelkezik Azure-fiókkal, az [Azure ingyenes próbát](https://azure.microsoft.com/pricing/free-trial/) biztosít.

2. A hello Azure-portálon, kattintson **új**. hello portál megnyitja hello **új** ablak.

3. A hello **új** ablak, kattintson a **számítási** majd **láthatja az összes**.

   ![Új számítás ablak](./media/virtual-machines-windows-portal-sql-server-provision/azure-new-compute-blade.png)

4. Hello keresési mezőbe, írja be a **SQL Server**, és nyomja le az ENTER BILLENTYŰT.

5. Kattintson a hello **szűrő** ikonra, és válassza ki **Microsoft** hello közzétevő. Kattintson a **végzett** hello szűrő ablak toofilter hello eredmények tooMicrosoft közzé az SQL Server-lemezképekben.

   ![Azure-beli virtuális gépek ablak](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-blade2.png)

5. Tekintse át a hello elérhető SQL Server-lemezképekben. Minden rendszerkép egy SQL Server-verziót és egy operációs rendszert azonosít.

6. Jelölje be hello kép nevű **szabad licenc: SQL Server 2016 SP1 fejlesztői a Windows Server 2016**.

   > [!TIP]
   > mivel már egy SQL Server fejlesztési, tesztelési célokra számára teljes körű kiadása, ebben az oktatóanyagban használt hello fejlesztői verzióját. Csak a futó virtuális gép hello hello költség fizetnie. Azonban áll szabad toochoose hello képek toouse ebben az oktatóanyagban bármelyikét.

   > [!TIP]
   > SQL Virtuálisgép-rendszerképek amit fel lehetne venni hello licencelési költségeket az SQL Server hello hello (kivéve a hello fejlesztői és az Express kiadás) hoz létre virtuális gép árképzése perc. Az SQL Server Developer ingyenesen használható fejlesztési/tesztelési célokra (üzemi környezetben nem), az SQL Express pedig ingyenesen használható kisebb számítási feladatokhoz (1 GB-nál kisebb memória, 10 GB-nál kevesebb tárhely). Egy másik beállítás toobring-a-saját-licenc (használata BYOL), és csak a virtuális gép hello fizetési van. Az ilyen rendszerképek nevei {BYOL} előtagot kapnak. 
   >
   > A lehetőségekkel kapcsolatos további információkért tekintse meg [az SQL Server Azure virtuális gépek díjszabási útmutatóját](virtual-machines-windows-sql-server-pricing-guidance.md).

7. Ellenőrizze, hogy a **Telepítési modell kiválasztása** alatt a **Resource Manager** van-e kiválasztva. Erőforrás-kezelő rendszer hello ajánlott az új virtuális gépek telepítési modelljét. 

8. Kattintson a **Create** (Létrehozás) gombra.

    ![SQL virtuális gép létrehozása a Resource Manager használatával](./media/virtual-machines-windows-portal-sql-server-provision/azure-compute-sql-deployment-model.png)

## <a name="configure-hello-vm"></a>Virtuális gép hello konfigurálása
Az SQL Server rendszerű virtuális gépek konfigurálására öt ablak szolgál.

| Lépés | Leírás |
| --- | --- |
| **Alapvető beállítások** |[Az alapvető beállítások konfigurálása](#1-configure-basic-settings) |
| **Méret** |[A virtuális gép méretének kiválasztása](#2-choose-virtual-machine-size) |
| **Beállítások** |[Választható funkciók konfigurálása](#3-configure-optional-features) |
| **Az SQL Server beállításai** |[Az SQL Server beállításainak konfigurálása](#4-configure-sql-server-settings) |
| **Összefoglalás** |[Felülvizsgálati hello összefoglaló](#5-review-the-summary) |

## <a name="1-configure-basic-settings"></a>1. Az alapvető beállítások konfigurálása

A hello **alapjai** ablakban adja meg a következő információ hello:

* Adjon meg egy egyedi **nevet** a virtuális gép számára.

* A virtuális gép lemeztípusaként válassza az **SSD** beállítást az optimális teljesítmény érdekében.

* Adjon meg egy **felhasználónév** hello VM hello helyi rendszergazdai fiókot. Ez a fiók adott SQL Server toohello **sysadmin** rögzített kiszolgálói szerepkör.

* Adjon meg egy erős **jelszót**.

* Ha több előfizetéssel, győződjön meg arról, hogy hello előfizetés a megfelelő hello az új virtuális Gépet.

* A hello **erőforráscsoport** mezőbe írja be az új erőforráscsoport nevét. Másik lehetőségként toouse egy meglévő erőforráscsoportot kattintson **meglévő**. Az erőforráscsoportok az Azure-ban egymáshoz kapcsolódó erőforrásokból (virtuális gépekből, tárfiókokból, virtuális hálózatokból stb.) álló gyűjtemények.

  > [!NOTE]
  > Az Azure szolgáltatásban futó SQL Server üzemelő példányok tesztelésekor vagy a velük való megismerkedéskor érdemes egy új erőforráscsoportot használni. A tesztelés befejezése után törölje a hello erőforrás csoport tooautomatically delete hello virtuális gép és az erőforráscsoporthoz társított összes erőforrás. További információ az erőforráscsoportokkal kapcsolatban: [Azure Resource Manager Overview](../../../azure-resource-manager/resource-group-overview.md) (Az Azure Resource Manager áttekintése).

* Válassza ki a **hely** a hello Azure-régió, amely üzemelteti a központi telepítés.

* Kattintson a **OK** toosave hello beállításait.

    ![Alapvető SQL-beállítások ablak](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-basic.png)

## <a name="2-choose-virtual-machine-size"></a>2. A virtuális gép méretének kiválasztása

A hello **mérete** . lépés:, válassza ki a virtuális gép méretét a hello **méret kiválasztása** ablak. hello ablak először ajánlott méreteket a kiválasztott hello lemezkép alapján jeleníti meg.

> [!IMPORTANT]
> hello becsült havi költségét hello megjelenő **méret kiválasztása** ablak nem tartalmazza az SQL Server licencelési költségeit. Ez az hello önmagában VM hello költségét. Hello Express és Developer kiadásában SQL Server ez pedig hello teljes becsült költség. Más kiadásain, lásd: hello [Windows virtuális gépek díjszabása](https://azure.microsoft.com/pricing/details/virtual-machines/windows/) válassza ki a cél az SQL Server kiadása. Lásd még: hello [útmutatást az SQL Server Azure virtuális gépek díjszabása](virtual-machines-windows-sql-server-pricing-guidance.md).

![SQL virtuális gépek méretbeállításai](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-vm-choose-a-size.png)

A termelési számítási feladatokhoz, tekintse meg a hello ajánlott méreteket és konfigurációjának [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md). Ha egy olyan méretű géphez, amely nem szerepel a listában, kattintson a hello **összes** gombra.

> [!NOTE]
> További információ a virtuális gépek méretével kapcsolatban: [Virtuális gépek méretei](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Válassza ki a virtuális gép méretét, majd kattintson a **Kijelölés** elemre.

## <a name="3-configure-optional-features"></a>3. Választható funkciók konfigurálása

A hello **beállítások** ablakban konfigurálása az Azure tárolási, hálózatkezelési és hello virtuális gép figyelését.

* A **Tároló** területen válassza az **Igen** lehetőséget a **Felügyelt lemezek használata** beállítás alatt.

   > [!NOTE]
   > A Microsoft a Managed Disks használatát javasolja az SQL Server esetében. A felügyelt lemezek leírók tárolási hello színfalak mögött. Emellett, ha felügyelt lemezzel rendelkező virtuális gépek szerepelnek hello azonos rendelkezésre állási csoportot, Azure hello tárolási erőforrások tooprovide megfelelő redundancia osztja el. További információkért lásd az [Azure Managed Disks áttekintését](../../../storage/storage-managed-disks-overview.md). A felügyelt lemezek a rendelkezésre állási csoportokban való használatával kapcsolatos részletekért lásd: [Felügyelt lemezek használata rendelkezésre állási csoporthoz tartozó virtuális gépekkel](../manage-availability.md).

* A **hálózati**, elfogadhatja az automatikusan kitölti a rendszer hello értékeket. Egyes funkciókra kattintva is toomanually hello konfigurálása **virtuális hálózati**, **alhálózati**, **nyilvános IP-cím**, és **hálózati biztonsági csoport**. Ez az oktatóanyag hello céljából tartsa hello alapértelmezett értékeket.

* Az Azure lehetővé teszi, hogy **figyelés** alapértelmezés szerint a hello hello VM kijelölt ugyanazt a tárfiókot. Ezeket a beállításokat itt módosíthatja.

* A **rendelkezésre állási csoport**, meghagyhatja az alapértelmezett hello **nincs** ehhez az oktatóanyaghoz. Ha azt tervezi, hogy az SQL AlwaysOn rendelkezésre állási csoportok mentése tooset, hello rendelkezésre állási tooavoid ismételt létrehozás hello virtuális gépeket konfigurálhatja.  További információkért lásd: [kezelése hello virtuális gépek rendelkezésre állási](../manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Ha végzett a beállítások konfigurálásával, kattintson az **OK** gombra.

## <a name="4-configure-sql-server-settings"></a>4. Az SQL Server beállításainak konfigurálása
A hello **SQL Server-beállítások** ablakban adott beállításait és optimalizálási lehetőségeit, az SQL Server konfigurálása. hello-beállítások, az SQL Server konfigurálható hello következőket tartalmazzák.

| Beállítás |
| --- |
| [Kapcsolatok](#connectivity) |
| [Hitelesítés](#authentication) |
| [Tároló konfigurálása](#storage-configuration) |
| [Automatikus javítás](#automated-patching) |
| [Automatikus biztonsági mentés](#automated-backup) |
| [Azure Key Vault-integráció](#azure-key-vault-integration) |
| [R-szolgáltatások](#r-services) |

### <a name="connectivity"></a>Kapcsolatok

A **SQL-kapcsolat**, hello adja meg a hozzáférés toohello SQL Server-példány a virtuális Gépet szeretne. Ez az oktatóanyag hello céljából, jelölje ki **nyilvános (internet)** tooallow kapcsolatok tooSQL Server gépek és szolgáltatások a hello internet. Ezt a lehetőséget választja, az Azure automatikusan konfigurálja hello tűzfal és hello hálózati biztonsági csoport tooallow forgalom 1433-as portot.

![SQL kapcsolati lehetőségek](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-connectivity-alt.png)

> [!TIP]
> Alapértelmezés szerint az SQL Server a jól ismert **1433-as** portot figyeli. A nagyobb biztonság érdekében a hello előző párbeszédpanel toolisten egy nem alapértelmezett portot, például a 1401 hello portjának módosítása. Ha így tesz, ehhez a porthoz kell csatlakoznia az összes ügyféleszközzel, például az SSMS-sel.

tooconnect tooSQL kiszolgálón keresztül hello internet, engedélyeznie kell az SQL Server hitelesítést, amely hello a következő szakaszban ismertetett.

Ha inkább toonot engedélyezése kapcsolatok toohello adatbázismotor keresztül hello internet, válasszon egyet az alábbi beállítások hello:

* **Helyi (belül csak virtuális gép)** tooallow kapcsolatok tooSQL kiszolgálót csak a virtuális gép hello belül.
* **Magán (virtuális hálózaton belül)** tooallow kapcsolatok tooSQL Server gépek és szolgáltatások hello az azonos virtuális hálózatban.

Általában a biztonság fokozása érdekében hello legszigorúbb kapcsolódási, amely lehetővé teszi, hogy a forgatókönyv kiválasztása. Azonban minden hello-beállítások biztonságos hálózati biztonsági csoportszabályok és SQL/Windows-hitelesítés használatával. Hálózati biztonsági csoport hello virtuális gép létrehozása után szerkesztheti. További információkért lásd [az SQL Server Azure-beli virtuális gépeken történő futtatásának biztonsági szempontjait](virtual-machines-windows-sql-security.md).

> [!NOTE]
> az SQL Server Express edition hello virtuálisgép-lemezkép nem automatikusan hello TCP/IP protokoll engedélyezéséhez. Ez érvényét veszti, még a hello nyilvános és Magánfelhő kapcsolat beállítások. A Express kiadását kell használnia az SQL Server Configuration Manager túl[manuálisan engedélyezi a hello TCP/IP-protokoll](#configure-sql-server-to-listen-on-the-tcp-protocol) hello létrehozása után a virtuális gép.

### <a name="authentication"></a>Authentication

Ha SQL Server-hitelesítésre van szüksége, kattintson az **Engedélyezés** lehetőségre az **SQL-hitelesítés** alatt.

![SQL Server-hitelesítés](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-authentication.png)

> [!NOTE]
> Ha azt tervezi, SQL Server tooaccess keresztül hello internet (azaz hello szövegrészt nyilvános kapcsolódási beállítást választja), engedélyeznie kell az SQL-hitelesítés itt. Nyilvános hozzáférés toohello SQL Server SQL-hitelesítés hello használatát igényli.
> 
> 

Ha engedélyezi az SQL Server-hitelesítést, adjon meg egy **bejelentkezési nevet** és egy **jelszót**. Ez a felhasználónév beállítás egy SQL Server-hitelesítési bejelentkezési és hello tagja, és **sysadmin** rögzített kiszolgálói szerepkör. További információk a hitelesítési módokról: [Choose an Authentication Mode](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode) (Hitelesítési mód kiválasztása).

Ha nem engedélyezi az SQL Server-hitelesítést, majd használhatja hello helyi rendszergazdai fiók hello VM tooconnect toohello SQL Server-példányon.

### <a name="storage-configuration"></a>Tároló konfigurálása

Kattintson a **tárkonfiguráció** toospecify hello tárolási követelmények érvényesek.

![SQL-tároló konfigurálása](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-storage.png)

> [!NOTE]
> Ha manuálisan konfigurálja a virtuális gép toouse standard szintű tárolót, ez a beállítás nem érhető el. Az automatikus tárolóoptimalizálás csak a prémium szintű Storage esetében érhető el.

> [!TIP]
> leállítja hello száma és a csúszkák felső határértéke hello hello kijelölt virtuális gép méretétől függ. A nagyobb és nagyobb teljesítményű virtuális gép több másolatot tud tooscale.

Megadhat különböző követelményeket, például a kimeneti/bemeneti műveletek másodpercenkénti számát (IOPS), a MB/másodpercben megadott átviteli sebességet, illetve a tárterület teljes méretét. Hello csúszó méretezik konfigurálja ezeket az értékeket. Ezek a tárolóbeállítások a számítási feladatok mennyiségéhez igazodva módosíthatók. hello portál automatikusan kiszámítja a lemezek tooattach száma hello és konfigurálása ezen követelmények alapján.

A **tárolási optimalizálva**, válasszon egyet az alábbi beállítások hello:

* **Általános** hello alapértelmezett beállítás, és a legtöbb számítási feladatot támogatja.
* **Tranzakciós** feldolgozási hello tárhely az adatbázisok hagyományos OLTP számítási optimalizálja.
* **Az adatraktározás terén** hello tárhely az elemzési és jelentéskészítési számítási optimalizálja.

### <a name="automated-patching"></a>Automatikus javítás

Az **Automatikus javítás** alapértelmezés szerint engedélyezve van. Automatizált javítás lehetővé teszi, hogy az Azure tooautomatically javítás az SQL Server és hello operációs rendszer. Adjon meg egy napot hello hét, ideje és időtartama a karbantartási időszak. Az Azure ebben a karbantartási időszakban végzi el a javításokat. hello karbantartási időszak ütemezése hello virtuális gép területi idő használ. Ha nem szeretné, hogy Azure tooautomatically javítás az SQL Server és hello operációs rendszert, kattintson a **letiltása**.  

![SQL automatikus javítás](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-patching.png)

További információk: [Automated Patching for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md) (Az SQL Server automatikus javítása Azure virtuális gépeken).

### <a name="automated-backup"></a>Automatikus biztonsági mentés

Az **Automatikus biztonsági mentés** területen engedélyezheti az összes adatbázis automatikus mentését. Az automatikus biztonsági mentés alapértelmezés szerint le van tiltva.

Ha engedélyezi az SQL automatikus biztonsági mentés, hello következőket konfigurálhatja:

* Biztonsági mentések megőrzési ideje (napokban)
* Biztonsági másolatok tárolási fiók toouse
* Titkosítási beállítás és a biztonsági mentések jelszavas védelme
* Rendszeradatbázisok biztonsági mentése
* Biztonsági mentések ütemezésének konfigurálása

biztonsági másolat, kattintson a tooencrypt hello **engedélyezése**. Majd adja meg a hello **jelszó**. Az Azure létrehoz egy tanúsítványt tooencrypt hello biztonsági mentések, és használja hello megadott jelszó tooprotect tanúsítvány.

![SQL automatikus biztonsági mentés](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-autobackup2.png)

 További információk: [Automated Backup for SQL Server in Azure Virtual Machines](virtual-machines-windows-sql-automated-backup.md) (Az SQL Server automatikus biztonsági mentése Azure virtuális gépeken).

### <a name="azure-key-vault-integration"></a>Azure Key Vault-integráció

toostore biztonsági titkokat az Azure-ban a titkosításhoz, kattintson a **Azure key vault-integráció** kattintson **engedélyezése**.

![SQL Azure Key Vault-integráció](./media/virtual-machines-windows-portal-sql-server-provision/azure-sql-arm-akv.png)

hello következő táblázatban hello paraméterek szükséges tooconfigure Azure Key Vault-integráció.

| PARAMÉTER | LEÍRÁS | PÉLDA |
| --- | --- | --- |
| **Key Vault URL** |hello hello kulcstároló helye. |https://contosokeyvault.vault.azure.net/ |
| **Egyszerű név** |Az Azure Active Directory szolgáltatás egyszerű neve. Ez a név egyben a hivatkozott tooas hello ügyfél-azonosítót. |fde2b411-33d5-4e11-af04eb07b669ccf2 |
| **Egyszerű titok** |Az Azure Active Directory szolgáltatás egyszerű titka. Ezt a titkos kulcsot is hivatkozott tooas hello Ügyfélkulcs. |9VTJSQwzlFepD8XODnzy8n2V01Jd8dAjwm/azF1XDKM= |
| **Hitelesítő adat neve** |**Hitelesítő adat neve**: AKV-integráció hitelesítő adatot hoz létre az SQL Serverben, így hello VM toohave hozzáférés toohello kulcstároló. Válasszon egy nevet ennek a hitelesítő adatnak. |mycred1 |

További információkért lásd: [Configure Azure Key Vault Integration for SQL Server on Azure VMs](virtual-machines-windows-ps-sql-keyvault.md) Az Azure Key Vault-integráció konfigurálása az SQL Serverhez Azure virtuális gépeken.

### <a name="r-services"></a>R-szolgáltatások

Hello beállítás tooenable rendelkezik [SQL Server R Services](https://msdn.microsoft.com/library/mt604845.aspx). Ez lehetővé teszi az SQL Server 2016 toouse advanced analytics. Kattintson a **engedélyezése** a hello **SQL Server beállításai** ablak.

> [!NOTE]
> Hello portál helytelenül letiltja a SQL Server 2016 Developer Edition, ezt a lehetőséget. A Developer Edition esetében az R Servicest manuálisan kell engedélyeznie a virtuális gép létrehozása után.

![Az SQL Server R Services engedélyezése](./media/virtual-machines-windows-portal-sql-server-provision/azure-vm-sql-server-r-services.png)

Amikor végzett az SQL Server beállításainak konfigurálásával, kattintson az **OK** gombra.

## <a name="5-review-hello-summary"></a>5. Felülvizsgálati hello összefoglaló

A hello **összefoglaló** ablakot, tekintse át hello összefoglaló, és kattintson a **beszerzési** toocreate SQL Server, erőforráscsoport és erőforrások virtuális Géphez megadott.

Hello telepítési hello Azure-portálon a figyelheti. Hello **értesítések** üdvözlő képernyőt hello tetején látható alapszintű hello központi telepítésének állapotát tartalmazza.

> [!NOTE]
> tooprovide az üzemelő példányon követelnek meg időkorlátja, szemléltetéséhez üzembe egy SQL virtuális gép toohello USA keleti régiójában az alapértelmezett beállításokkal. Ez az üzembe helyezési teszt összesen 26 percet toocomplete vett igénybe. A régiótól és a kiválasztott beállításoktól függően Ön ennél rövidebb vagy hosszabb üzembe helyezési időt is tapasztalhat.

## <a name="open-hello-vm-with-remote-desktop"></a>Nyissa meg a virtuális gép hello a távoli asztal

A következő lépéseket tooconnect toohello SQL Server virtuális géphez a távoli asztal hello használata:

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-remote-desktop-connect.md)]

Toohello SQL Server virtuális gépen a kapcsolódás után indítsa el az SQL Server Management Studio eszközt, és a helyi rendszergazdai hitelesítő adatokkal Windows-hitelesítéssel csatlakozzon. SQL Server-hitelesítés engedélyezve van, ha elérhetők, SQL-hitelesítéssel hello SQL-bejelentkezési és a kiépítés során megadott jelszót.

Hozzáférés toohello gép lehetővé teszi toodirectly módosítás számítógép és SQL Server-beállítások a követelmények alapján. Például nem hello tűzfal beállításainak konfigurálását, vagy módosítsa az SQL Server-konfigurációs beállítások.

## <a name="enable-tcpip-for-developer-and-express-editions"></a>TCP/IP engedélyezése a Developer és Express kiadásokhoz

SQL Server új virtuális gép létesítésekor Azure nem automatikusan hello TCP/IP protokoll engedélyezéséhez az SQL Server Developer és Express kiadásait. az alábbi hello részben megtudhatja, hogyan toomanually engedélyezze a TCP/IP protokollt, hogy az IP-cím szerint távolról kapcsolódhatnak.

a következő lépéseket használata hello **SQL Server Configuration Manager** tooenable hello TCP/IP-protokoll az SQL Server Developer és Express kiadásait.

> [!INCLUDE [Connect tooSQL Server VM with remote desktop](../../../../includes/virtual-machines-sql-server-connection-tcp-protocol.md)]

## <a name="connect-toosql-server-remotely"></a>Távoli csatlakozás tooSQL kiszolgáló

Ebben az oktatóanyagban a jelenleg választott **nyilvános** hozzáférés hello virtuális gép és **SQL Server-hitelesítés**. E beállítások automatikusan konfigurált hello virtuális gép tooallow SQL Serverhez való csatlakozást bármely ügyfél keresztül hello internet (feltéve, hogy hello helyes SQL-bejelentkezési névvel rendelkeznek).

> [!NOTE]
> Ha nem nyilvános a kiépítés során, majd módosíthatja az SQL-kapcsolat beállításainak hello portálon keresztül kiépítése után. További információ: [Az SQL csatlakozási beállításainak módosítása](virtual-machines-windows-sql-connect.md#change).

hello a következő szakaszok bemutatják, hogyan tooconnect tooyour SQL Server-példány a virtuális gépen keresztül egy másik számítógépről hello internet.

> [!INCLUDE [Connect tooSQL Server in a VM Resource Manager](../../../../includes/virtual-machines-sql-server-connection-steps-resource-manager.md)]

## <a name="next-steps"></a>Következő lépések

Egyéb Azure-ban az SQL Server használatával kapcsolatos információkért lásd: [SQL Server Azure virtuális gépeken](virtual-machines-windows-sql-server-iaas-overview.md) és hello [gyakran ismételt kérdések](virtual-machines-windows-sql-server-iaas-faq.md).
