---
title: "a klasszikus Azure virtuális gépek aaaCreate MySQL futtató |} Microsoft Docs"
description: "Windows Server 2012 R2 rendszert futtató Azure virtuális gép létrehozása és hello hello klasszikus üzembe helyezési modellel MySQL-adatbázis."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 98fa06d2-9b92-4d05-ac16-3f8e9fd4feaa
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: cynthn
ms.openlocfilehash: 2c9564955c2bab197a8e494e939ce16c0b9bb605
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-mysql-on-a-virtual-machine-created-with-hello-classic-deployment-model-running-windows-server-2016"></a>Egy virtuális gépen futó Windows Server 2016 hello klasszikus telepítési modellel létrehozott MySQL telepítése
[MySQL](https://www.mysql.com) egy népszerű nyílt forráskódú, akkor az SQL-adatbázis. Az oktatóanyag bemutatja, hogyan tooinstall, és futtassa hello **MySQL 5.7.18 közösségi változata** egy futó virtuális géphez MySQL-kiszolgálóként **Windows Server 2016**. Lehet, hogy a felhasználói élmény némileg eltérő MySQL vagy Windows Server más verzióiban.

Linux MySQL telepítésével kapcsolatos útmutatásért tekintse meg: [hogyan tooinstall Azure-beli MySQL](../../linux/mysql-install.md).

> [!IMPORTANT]
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="create-a-virtual-machine-running-windows-server-2016"></a>Windows Server 2016 rendszerű virtuális gép létrehozása
Ha még nem rendelkezik a virtuális gép Windows Server 2016 rendszert futtató, ezzel [oktatóanyag](./tutorial.md) toocreate hello virtuális gépet.

## <a name="attach-a-data-disk"></a>Adatlemez csatolása
Hello virtuális gép létrehozása után hozzácsatolhat adatlemezt nem kötelező. Termelési számítási feladatokhoz és elegendő szabad terület az operációs rendszer meghajtó (C:) hello tooavoid javasolt adatlemezt ad hozzá, amely magában foglalja hello operációs rendszer.

Lásd: [hogyan tooattach az adatok lemezre tooa Windows rendszerű virtuális gép](../attach-managed-disk-portal.md) és az üres lemez csatolása hello utasítások. Állítsa be a hello állomás gyorsítótárazása túl**nincs** vagy **írásvédett**.

## <a name="log-on-toohello-virtual-machine"></a>Jelentkezzen be a virtuális gép toohello
A következő lesz [jelentkezzen be a virtuális gép toohello](./connect-logon.md) így MySQL telepítheti.

## <a name="install-and-run-mysql-community-server-on-hello-virtual-machine"></a>Telepítése és futtatása a MySQL közösségi Server hello virtuális gépen
Kövesse az alábbi lépéseket tooinstall, konfigurálása és a MySQL Server hello közösségi változata fusson:

> [!NOTE]
> Hello IE beállíthatja letöltése Internet Explorerrel elemek, **fokozott biztonsági beállítások** tooOff, és egyszerűbbé tehető a hello folyamat letöltése. Hello Start menüből, kattintson a felügyeleti eszközök/Server Manager vagy helyi kiszolgáló, majd kattintson az Internet Explorer **fokozott biztonsági beállítások** és hello konfigurációs tooOff beállítása).
>
>

1. Kattintson a távoli asztali kapcsolattal toohello virtuális gép csatlakoztatása után **Internet Explorer** hello kezdőképernyőről.
2. Jelölje be hello **eszközök** hello jobb felső sarokban (hello cogged kerék ikon) gombra, majd **Internetbeállítások**. Hello kattintson **biztonsági** lapra, majd hello **megbízható helyek** ikonra, majd kattintson a hello **helyek** gombra. Vegye fel a megbízható helyek http://*.mysql.com toohello listáján. Kattintson a **Bezárás**, és kattintson a **OK**.
3. A hello címet az Internet Explorer címsorában, írja be a https://dev.mysql.com/downloads/mysql/.
4. Hello MySQL hely toolocate használja, és töltse le a hello hello MySQL Installer Windows legújabb verzióját. Hello MySQL telepítő kiválasztásakor a letöltési hello verziót hello (például hello mysql-telepítő-Közösség-5.7.18.0.msi 352.8 MB-os fájlméretben) fájl beállításának befejezéséhez, és mentse hello telepítő.
5. Ha hello telepítő letöltése befejeződött, kattintson **futtatása** toolaunch beállítása.
6. A hello **licencszerződés** lapon fogadja el hello licencszerződést, majd kattintson **következő**.
7. A hello **kiválasztása a telepítés típusát** hello telepítési módot, és kattintson kattintson **következő**. hello következő lépések azt feltételezik hello hello kiválasztását **csak a kiszolgáló** telepítés típusa.
8. Ha hello **ellenőrizze követelmények** oldal megjelenéskor kattintson **Execute** toolet hello telepítő telepíti a hiányzó összetevőket. Kövesse a megjelenő, például a hello C++ újraterjeszthető csomag futásidejű utasításokat.
9. A hello **telepítési** kattintson **Execute**. Ha a telepítés befejeződött, kattintson **következő**.

10. A hello **termék konfigurációjának** kattintson **következő**.

11. A hello **típusa és a hálózatkezelés** adja meg azokat az szükségeskonfiguráció típusa és a kapcsolat beállításait, beleértve a hello TCP-portot, ha szükséges. Válassza ki **speciális beállítások megjelenítése**, és kattintson a **következő**.
    ![](./media/mysql-2008r2/MySQL_TypeNetworking.png)

12. A hello **fiókok és szerepkörök** lapján adjon meg egy erős jelszót a MySQL legfelső szintű. További MySQL felhasználói fiókok hozzáadása, igény szerint, és kattintson a **következő**.

    ![](./media/mysql-2008r2/MySQL_AccountsRoles_Filled.png)
13. A hello **Windows-szolgáltatás** lapon toohello alapértelmezett beállításainak módosítása hello MySQL-kiszolgálót futtató Windows-szolgáltatás, igény szerint adja meg, és kattintson a **következő**.

    ![](./media/mysql-2008r2/MySQL_WindowsService.png)
14. választási lehetőségek a hello hello **beépülő modulok és bővítmények** lap opcionálisak. Kattintson a **következő** toocontinue.
15. A hello **speciális beállítások** lapon adja meg a módosítások toologging beállításokat, szükség szerint, és kattintson a **következő**.

    ![](./media/mysql-2008r2/MySQL_AdvOptions.png)
16. A hello **kiszolgáló beállításainak alkalmazása** kattintson **Execute**. Hello konfigurációs lépések befejezését követően kattintson **Befejezés**.
17. A hello **termék konfigurációjának** kattintson **következő**.
18. A hello **telepítési teljes** lapján kattintson **másolási napló tooClipboard** Ha azt szeretné, tooexamine később, és kattintson **Befejezés**.
19. Írja be a hello kezdőképernyőről **mysql**, és kattintson a **MySQL 5.7 parancssori ügyfél**.
20. Adja meg a 12. lépésben megadott hello gyökér szintű jelszavát, és lehetősége lesz a kérdés ahol parancsok tooconfigure MySQL adhat ki. A parancsokat és szintaxist hello részletekért lásd: [MySQL dokumentációjának](https://dev.mysql.com/doc/refman/5.7/en/server-configuration.html).

    ![](./media/mysql-2008r2/MySQL_CommandPrompt.png)
21. Kiszolgáló alapértelmezett beállításait, például alapszintű hello és adatkönyvtárak és meghajtókat is konfigurálhatja. További információkért lásd: [6.1.2 kiszolgáló konfigurációs alapértelmezés szerint](https://dev.mysql.com/doc/refman/5.7/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Végpontok konfigurálása

Hello MySQL szolgáltatás toobe elérhető tooclient azon számítógépek hello Internet kell konfigurálnia a végpont hello TCP-portot, és egy Windows tűzfalszabály létrehozásához. hello alapértelmezett port meg, melyik hello MySQL-kiszolgáló szolgáltatás figyeli, MySQL-ügyfelek értéke 3306. Megadhat egy másik portra, mindaddig, amíg hello port hello értéket kapott a hello konzisztens **típusa és a hálózatkezelés** (11. lépést az előző eljárásban hello) lap.

> [!NOTE]
> Éles környezetben való használathoz fontolja meg a hello hajtsanak végre hello MySQL-kiszolgáló szolgáltatás elérhető tooall számítógépek a hello internetes biztonsági következményei. Forrás IP-címek, amelyek számára engedélyezett toouse hello végpont a egy hozzáférés-vezérlési lista (ACL) hello készletének adhat meg. További információkért lásd: [hogyan tooSet be végpontok tooa virtuális gép](setup-endpoints.md).
>
>

MySQL-kiszolgáló szolgáltatás hello végpont tooconfigure:

1. Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)**, kattintson a MySQL-virtuális gép hello nevét, majd **végpontok**.
2. Hello parancssávon kattintson **Hozzáadás**.
3. A hello **végpont hozzáadása** írja be egy egyedi nevet az **neve**.
4. Válassza ki **TCP** hello protokollként.
5. Írjon be egy hello portszámot, például **3306**, mindkét **nyilvános Port** és **magánhálózati Port**, és kattintson a **OK**.

## <a name="add-a-windows-firewall-rule-tooallow-mysql-traffic"></a>Adja hozzá a Windows tűzfal szabály tooallow MySQL-forgalom
egy Windows tűzfalszabály, amely lehetővé teszi, hogy a MySQL-forgalom hello Internet, futtassa a következő parancsot a következő hello tooadd egy _emelt szintű Windows PowerShell-parancssorában történő_ hello MySQL server virtuális gépen.

    New-NetFirewallRule -DisplayName "MySQL57" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public

## <a name="test-your-remote-connection"></a>A távoli kapcsolat tesztelése
meg kell adnia a hello DNS-nevét tartalmazó hello VN hello felhőszolgáltatás, tootest a távoli kapcsolat toohello Azure virtuális Gépen futó hello MySQL Server szolgáltatásból.

1. Hello Azure-portálon, kattintson **virtuális gépek (klasszikus)**, kattintson a MySQL-kiszolgáló virtuális gép hello nevét, majd **áttekintése**.
2. A virtuális gép irányítópult hello, vegye figyelembe a hello **DNS-név** érték. Például:

   ![](media/mysql-2008r2/MySQL_DNSName.png)
3. A MySQL vagy hello MySQL-ügyfelet futtató helyi számítógép futtassa a következő parancs toolog a MySQL felhasználóként hello.

     MySQL -u <yourMysqlUsername> - p -h<yourDNSname>

   Például használatával hello MySQL felhasználónév _dbadmin3_ és hello _testmysql.cloudapp.net_ DNS-beli név hello virtuális géphez, MySQL hello a következő parancs használatával tudott elindulni:

     MySQL -u dbadmin3 -p -h testmysql.cloudapp.net

## <a name="next-steps"></a>Következő lépések
toolearn futtató MySQL, bővebben lásd: hello [MySQL dokumentáció](http://dev.mysql.com/doc/).
