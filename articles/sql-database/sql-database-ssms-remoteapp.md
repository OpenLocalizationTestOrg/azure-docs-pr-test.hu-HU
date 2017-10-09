---
title: "aaaConnect tooSQL adatbázist az SQL Server Management Studio használatával az Azure Remoteappban |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan használja a biztonsági és teljesítménynövelő tooSQL adatbázishoz kapcsolódáskor SQL Server Management Studio az Azure Remoteappban toouse"
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
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a>SQL Server Management Studio használata az Azure RemoteApp tooconnect tooSQL adatbázis

> [!IMPORTANT]
> Azure RemoteApp hamarosan megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
>

## <a name="introduction"></a>Bevezetés
Az oktatóanyag bemutatja, hogyan toouse SQL Server Management Studio (SSMS) az Azure RemoteApp tooconnect tooSQL adatbázis. Az SQL Server Management Studio az Azure Remoteappban beállításának folyamatán hello bemutatja, hogyan, hello előnyeit ismerteti, és jeleníti meg a biztonsági funkciók használható az Azure Active Directoryban.

**Becsült idő toocomplete:** 45 percben

## <a name="ssms-in-azure-remoteapp"></a>Az Azure Remoteappban SSMS
Az Azure RemoteApp egy olyan távoli asztali szolgáltatás letölti a alkalmazások az Azure-ban. További információk itt: [Mi az RemoteApp?](../remoteapp/remoteapp-whatis.md)

Azure RemoteApp által biztosított futó SSMS hello felhasználói élmény ugyanaz, mint SSMS helyileg futó.

![Az Azure Remoteappban futtató SSMS ábrázoló képernyőfelvétel][1]

## <a name="benefits"></a>Előnyök
Nincsenek számos előnyt toousing SSMS az Azure Remoteappban, beleértve:

* Azure SQL server a 1433-as port nem rendelkezik a külsőleg (Azure-on kívüli) elérhetővé tett toobe.
* Nincs szükség tookeep hozzáadása és eltávolítása az IP-címek hello Azure SQL server tűzfalon.
* Az összes Azure RemoteApp kapcsolat fordulhat elő, HTTPS-KAPCSOLATON keresztül a 443-as port használatával titkosítja a távoli asztal protokoll
* Többfelhasználós és méretezhető.
* Nincs jobb a teljesítménye az SSMS rendelkező hello hello SQL-adatbázis megegyező régióban.
* Az Azure RemoteApp használata az Azure Active Directoryban, amely rendelkezik a felhasználói tevékenység jelentések hello prémium kiadásával naplózhatók.
* Többtényezős hitelesítés (MFA) is engedélyezheti.
* Hozzáférés bárhonnan esetén hello SSMS támogatott Azure RemoteApp-ügyfelek, mely az iOS, Android, Mac, Windows Phone és Windows-Számítógépet tartalmazza.

## <a name="create-hello-azure-remoteapp-collection"></a>Hello Azure RemoteApp-gyűjtemény létrehozása
Az alábbiakban hello lépéseket toocreate az Azure RemoteApp-gyűjteménnyel ssms alkalmazásával:

### <a name="1-create-a-new-windows-vm-from-image"></a>1. Hozzon létre egy új Windows virtuális Gépet lemezképből
Hello hello gyűjtemény toomake "Windows Server távoli asztali munkamenet gazdagépen Windows Server 2012 R2" lemezképet az új virtuális gép használja.

### <a name="2-install-ssms-from-sql-express"></a>2. Az SQL Express SSMS telepítése
Nyissa meg az alakzatot hello új virtuális gép, és keresse meg a letöltési oldal toothis: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)

A beállítás tooonly letöltését SSMS van. Letöltés után nyissa meg a hello telepítési könyvtár, és futtassa a telepítő tooinstall SSMS.

SQL Server 2014 SP1 tooinstall is kell. Innen tölthető le: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)

SQL Server 2014 Service Pack 1 az Azure SQL Database használatához alapvető funkcióit tartalmazza.

### <a name="3-run-validate-script-and-sysprep"></a>3. Parancsfájl-futtatási ellenőrzése és a Sysprep
Hello hello virtuális gép asztalát az érvényesítés nevű PowerShell parancsfájl. Ehhez kattintson duplán a futtatásához. Azt ellenőrzi, hogy adott hello virtuális gép készen áll az alkalmazások távoli üzemeltetéséhez használt toobe. Ha ellenőrzés befejeződött, azt fogja kérni toorun sysprep - toorun válassza ki azt.

A sysprep befejezését követően az hello virtuális gép le fog állni.

További információ az Azure RemoteApp lemezkép, létrehozása toolearn lásd: [hogyan toocreate RemoteApp sablon rendszerképet az Azure-ban](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)

### <a name="4-capture-image"></a>4. Lemezkép rögzítése
Hello virtuális gép leállt, ha az aktuális hello portál található, és rögzítse.

További információ az lemezképek rögzítését toolearn lásd [egy hello klasszikus telepítési modellel létrehozott Azure Windows virtuális gép lemezképének rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)

### <a name="5-add-tooazure-remoteapp-template-images"></a>5. Adja hozzá a tooAzure RemoteApp sablon rendszerképek
Az Azure RemoteApp szakasz hello aktuális portál hello nyissa meg toohello sablon lemezképek lapra, és kattintson a Hozzáadás gombra. A hello előugró párbeszédpanelen válassza a "Képek importálása a virtuális gépek könyvtárból", és válassza a hello újonnan létrehozott lemezképet.

### <a name="6-create-cloud-collection"></a>6. Felhőalapú gyűjtemény létrehozása
Hello aktuális portálon hozzon létre egy új Azure RemoteApp felhőalapú gyűjtemény. Válassza ki a hello sablon rendszerképet, amely az imént importált ssms alkalmazásával telepítve van-e.

![Új felhőalapú gyűjtemény létrehozása][2]

### <a name="7-publish-ssms"></a>7. SSMS közzététele
A lap az új felhőalapú gyűjtemény, jelölje be a közzététel az alkalmazás közzététele hello hello a Start menüben, és válassza a SSMS hello listából.

![Alkalmazás közzététele][5]

### <a name="8-add-users"></a>8. Felhasználók hozzáadása
Hello felhasználói hozzáférés lapon kiválaszthatja, hogy hozzáférési toothis Azure RemoteApp-gyűjtemény csak tartalmazza az SSMS hello felhasználók.

![Felhasználó hozzáadása][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a>9. Hello Azure RemoteApp ügyfélalkalmazás telepítése
Töltse le, és telepítse az Azure RemoteApp-ügyfélen itt: [letöltése |} Az Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)

## <a name="configure-azure-sql-server"></a>Azure SQL-kiszolgáló konfigurálása
hello csak szükséges konfiguráció Azure Services tooensure engedélyezve van a hello tűzfal. Ha ez a megoldás használja, majd nem kell tooadd bármely IP-címek tooopen hello tűzfal. SQL Server toohello engedélyezett hello hálózati forgalom származik, más Azure-szolgáltatásokkal.

![Azure engedélyezése][4]

## <a name="multi-factor-authentication-mfa"></a>Többtényezős hitelesítés (MFA)
Többtényezős hitelesítés is engedélyezhető az alkalmazás kifejezetten. Nyissa meg az Azure Active Directory toohello alkalmazások lapján. A Microsoft Azure RemoteApp található bejegyzés. Kattintson az adott alkalmazáshoz, és adja meg, ha megjelenik a hello lap az alábbi ahol engedélyezheti az MFA ehhez az alkalmazáshoz.

![Többtényezős hitelesítés engedélyezése][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Az Azure Active Directory Premium felhasználói műveletek naplózása
Ha nincs Azure AD Premium számára, akkor el kell tooturn legyen a licencek szakasz, Directory-hello. Prémium szintű engedélyezve van, a felhasználók toohello Premium szintet rendelhet.

Amikor tooa felhasználói az Azure Active Directoryban, majd lépjen toohello tevékenység lapon toosee bejelentkezési adatokat tooAzure RemoteApp.

## <a name="next-steps"></a>Következő lépések
Miután befejezte a fenti lépéseket minden hello, akkor lesz kell tudni toorun hello Azure RemoteApp-ügyfelet és a bejelentkezést egy hozzárendelt felhasználó. Meg fog jelenni SSMS az alkalmazások rendelkezésre álló, és ha volt telepítve a számítógépen, az access tooAzure SQL server ugyanúgy futtathatja.

Hogyan toomake hello kapcsolat tooSQL adatbázis további információkért lásd: [tooSQL adatbázis csatlakozzon az SQL Server Management Studio eszközt, és minta T-SQL-lekérdezést végrehajtani a](sql-database-connect-query-ssms.md).

Ez minden most. Jó munkát!

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png