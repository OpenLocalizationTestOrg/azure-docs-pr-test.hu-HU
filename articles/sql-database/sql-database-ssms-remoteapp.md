---
title: "Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio használatával az Azure Remoteappban |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével megtudhatja, hogyan kell használnia az SQL Server Management Studio eszközt az Azure Remoteappban a biztonsági és teljesítménynövelő SQL-adatbázishoz szeretne csatlakozni"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: ae1f2fa38d38fe6c10bc7960fddb07ae330d1eeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>Azure RemoteApp SQL Server Management Studio segítségével csatlakozzon az SQL Database

> [!IMPORTANT]
> Azure RemoteApp hamarosan megszűnik. A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).
>

## <a name="introduction"></a>Bevezetés
Az oktatóanyag bemutatja, hogyan használható az SQL Server Management Studio (SSMS) az Azure Remoteappban SQL adatbázishoz való kapcsolódáshoz. Azt a végigvezeti Önt az SQL Server Management Studio az Azure Remoteappban beállításának folyamatán, a következő előnyöket ismerteti, és jeleníti meg a biztonsági funkciók használható az Azure Active Directoryban.

**Az oktatóanyag áttekintésének becsült ideje:** 45 perc

## <a name="ssms-in-azure-remoteapp"></a>Az Azure Remoteappban SSMS
Az Azure RemoteApp egy olyan távoli asztali szolgáltatás letölti a alkalmazások az Azure-ban. További információk itt: [Mi az RemoteApp?](../remoteapp/remoteapp-whatis.md)

Futó Azure RemoteApp szolgáltatáshoz az SSMS helyben fut az SSMS ugyanazt a felhasználói élményt biztosít.

![Az Azure Remoteappban futtató SSMS ábrázoló képernyőfelvétel][1]

## <a name="benefits"></a>Előnyök
Sok előnyökkel is jár SSMS használatával az Azure Remoteappban, beleértve:

* Azure SQL server a 1433-as port nem rendelkezik ki vannak téve a külsőleg (Azure-on kívüli).
* Nincs szükség hozzáadása és eltávolítása az Azure SQL-kiszolgáló tűzfal IP-címek.
* Az összes Azure RemoteApp kapcsolat fordulhat elő, HTTPS-KAPCSOLATON keresztül a 443-as port használatával titkosítja a távoli asztal protokoll
* Többfelhasználós és méretezhető.
* Jobb a teljesítménye SSMS rendelkezik az SQL-adatbázis ugyanabban a régióban van.
* Az Azure RemoteApp használata az Azure Active Directoryban, amely rendelkezik a felhasználói tevékenység jelentések prémium kiadásával naplózhatók.
* Többtényezős hitelesítés (MFA) is engedélyezheti.
* Hozzáférés bárhonnan esetén a támogatott Azure RemoteApp-ügyfelek iOS, Android, Mac, Windows Phone és Windows-Számítógépet tartalmazó SSMS.

## <a name="create-the-azure-remoteapp-collection"></a>Az Azure RemoteApp-gyűjtemény létrehozása
Az Azure RemoteApp-gyűjtemény létrehozása az SSMS lépései a következők:

### <a name="1-create-a-new-windows-vm-from-image"></a>1. Hozzon létre egy új Windows virtuális Gépet lemezképből
A gyűjteményből a "Windows Server távoli asztali munkamenet gazdagépen Windows Server 2012 R2" lemezkép segítségével ellenőrizze az új virtuális gép.

### <a name="2-install-ssms-from-sql-express"></a>2. Az SQL Express SSMS telepítése
Nyissa meg az új virtuális Gépet, és keresse meg a letöltési lapra: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)

A rendszer felajánlja a csak az SSMS letöltéséhez. Letöltés után nyissa meg a telepítés könyvtárba, és telepítőjének futtatásával telepítse az SSMS.

Is szeretne telepíteni az SQL Server 2014 SP1. Innen tölthető le: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 az Azure SQL Database használatához alapvető funkcióit tartalmazza.

### <a name="3-run-validate-script-and-sysprep"></a>3. Parancsfájl-futtatási ellenőrzése és a Sysprep
A virtuális gép asztalán a PowerShell parancsfájl neve ellenőrzése. Ehhez kattintson duplán a futtatásához. Ellenőrzi, hogy a távoli gazda az alkalmazások készen áll-e a virtuális Gépet. Ha az ellenőrzés befejeződött, a program kéri futtatni a Sysprep programot – válassza ki a futtatni.

A sysprep befejezése után azt a virtuális gép le fog állni.

A Azure RemoteApp-lemezképek létrehozásával kapcsolatos további tudnivalókért lásd: [egy RemoteApp-sablonlemezkép létrehozása az Azure-ban](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)

### <a name="4-capture-image"></a>4. Lemezkép rögzítése
Ha a virtuális gép leállt, az aktuális portálon található, és rögzítse.

Egy lemezkép rögzítésével kapcsolatos további információkért lásd: [egy Azure Windows virtuális gép létrehozása a klasszikus üzembe helyezési modellel lemezképének rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Azure RemoteApp sablon rendszerképei hozzáadása
Az aktuális portálon az Azure RemoteApp szakaszában nyissa meg a sablon lemezképek lapra, és kattintson a Hozzáadás gombra. A legördülő mezőben válassza a "Képek importálása a virtuális gépek könyvtárból", és válassza ki az imént létrehozott képet.

### <a name="6-create-cloud-collection"></a>6. Felhőalapú gyűjtemény létrehozása
Az aktuális portálon hozzon létre egy új Azure RemoteApp felhőalapú gyűjtemény. Válassza ki a sablon lemezképe most importált ssms alkalmazásával telepítve van-e.

![Új felhőalapú gyűjtemény létrehozása][2]

### <a name="7-publish-ssms"></a>7. SSMS közzététele
A közzétételi lap az új felhőalapú gyűjtemény, válassza ki a közzétenni egy alkalmazást a Start menüben, és SSMS listájából válasszon ki.

![Alkalmazás közzététele][5]

### <a name="8-add-users"></a>8. Felhasználók hozzáadása
A felhasználói hozzáférés lapon válassza ki a felhasználók, hozzáférhet a szolgáltatáshoz az SSMS tartalmazó Azure RemoteApp-gyűjteményhez.

![Felhasználó hozzáadása][6]

### <a name="9-install-the-azure-remoteapp-client-application"></a>9. Az Azure RemoteApp ügyfélalkalmazás telepítése
Töltse le, és telepítse az Azure RemoteApp-ügyfélen itt: [letöltése |} Az Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)

## <a name="configure-azure-sql-server"></a>Azure SQL-kiszolgáló konfigurálása
Győződjön meg arról, hogy az Azure szolgáltatás engedélyezve van-e tűzfal szükséges egyetlen konfiguráció is. Ha ez a megoldás használ, akkor nem szüksége megnyitásához a tűzfal IP-címek hozzáadása. A hálózati forgalmat, hogy az SQL Server más Azure-szolgáltatásokkal származik.

![Azure engedélyezése][4]

## <a name="multi-factor-authentication-mfa"></a>Többtényezős hitelesítés (MFA)
Többtényezős hitelesítés is engedélyezhető az alkalmazás kifejezetten. Ugrás az Azure Active Directory-alkalmazások fülre. A Microsoft Azure RemoteApp található bejegyzés. Kattintson az adott alkalmazáshoz, és adja meg, ha látni fogja az oldalon az alábbi ahol engedélyezheti az MFA ehhez az alkalmazáshoz.

![Többtényezős hitelesítés engedélyezése][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Az Azure Active Directory Premium felhasználói műveletek naplózása
Ha még nem rendelkezik Azure AD Premium, majd akkor kapcsolja be a könyvtár licencek részében. Prémium szintű engedélyezve van, a felhasználókat rendelhet a prémium szintű.

Amikor egy felhasználó számára az Azure Active Directoryban, majd lépjen az Azure Remoteapphoz bejelentkezési adatok megjelenítéséhez tevékenység lapját.

## <a name="next-steps"></a>Következő lépések
A fenti lépések végrehajtását követően fogja tudni futtatni az Azure RemoteApp-ügyfelet és a bejelentkezést egy hozzárendelt felhasználóval. Meg fog jelenni SSMS az alkalmazások rendelkezésre álló, és ha volt telepítve a számítógépen az Azure SQL-kiszolgálóhoz hozzáféréssel rendelkező ugyanúgy futtathatja.

Hogyan végezheti el a kapcsolatot az SQL Database további információkért lásd: [Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio eszközt, és végezze el a T-SQL-mintalekérdezés](sql-database-connect-query-ssms.md).

Ez minden most. Jó munkát!

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png