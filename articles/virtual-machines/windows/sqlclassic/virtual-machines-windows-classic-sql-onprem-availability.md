---
title: "aaaExtend helyszíni Always On rendelkezésre állási csoportok tooAzure |} Microsoft Docs"
description: "Ez az oktatóanyag hello klasszikus telepítési modellel létrehozott erőforrást használ, és ismerteti, hogyan toouse hello replika hozzáadása varázsló az SQL Server Management Studio (SSMS) tooadd egy Always On rendelkezésre állási csoportnak a replika Azure-ban."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 7ca7c423-8342-4175-a70b-d5101dfb7f23
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/31/2017
ms.author: mikeray
ms.openlocfilehash: 51fe117b40d69706b35f30101538a7a36116e128
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="extend-on-premises-always-on-availability-groups-tooazure"></a>A helyszíni Always On rendelkezésre állási csoportok tooAzure kiterjesztése
Always On rendelkezésre állási csoportokat magas rendelkezésre állási adatbázis csoportjai számára másodlagos replikák hozzáadásával adja meg. Ezekre a replikákra engedélyezése feladatátvétele adatbázisok meghibásodása esetén. Ezenkívül el használt toooffload, olvassa el a munkaterhelések vagy a biztonsági mentési feladatokat.

A helyi rendelkezésre állási csoportok tooMicrosoft Azure kiterjesztésére egy vagy több Azure virtuális gépeken futó SQL Server-kiépítés, majd adja hozzá őket, a replikák tooyour a helyi rendelkezésre állási csoportok.

Ez az oktatóanyag feltételezi, hogy rendelkezik-e hello következő:

* Aktív Azure-előfizetés. Is [regisztráljon egy ingyenes próbaverzióra](https://azure.microsoft.com/pricing/free-trial/).
* Egy meglévő Always On rendelkezésre állási csoportnak a helyi. További információ a rendelkezésre állási csoportok: [Always On rendelkezésre állási csoportok](https://msdn.microsoft.com/library/hh510230.aspx).
* Hello közötti kapcsolatot a helyszíni hálózat és az Azure virtuális hálózat. Ez a virtuális hálózat létrehozásával kapcsolatos további információkért lásd: [(klasszikus) Azure-portálon hozzon létre egy pont-pont kapcsolat használatával hello](../../../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md).

> [!IMPORTANT] 
> Azure az erőforrások létrehozására és kezelésére két különböző üzembe helyezési modellel rendelkezik: [Resource Manager és klasszikus](../../../azure-resource-manager/resource-manager-deployment-model.md). Ez a cikk hello klasszikus telepítési modell használatát bemutatja. A Microsoft azt javasolja, hogy az új telepítések esetén hello Resource Manager modellt használja.

## <a name="add-azure-replica-wizard"></a>Adja hozzá a replika Azure varázsló
Ez a szakasz bemutatja, hogyan toouse hello **replika Azure hozzáadása varázsló** tooextend az Always On rendelkezésre állási csoportnak megoldás tooinclude Azure replikákat.

> [!IMPORTANT]
> Hello **replika Azure hozzáadása varázsló** hello klasszikus telepítési modellel létrehozott virtuális gépek csak támogatja. Új Virtuálisgép-telepítések hello újabb Resource Manager modellt kell használja. Ha a virtuális gépek a Resource Manager használatával, majd manuálisan kell hozzáadni hello másodlagos Azure replika Transact-SQL commmands (itt nem látható) használatával. Ez a varázsló hello Erőforrás-kezelői forgatókönyvek nem fognak működni.

1. Az SQL Server Management Studio, bontsa ki a **magas rendelkezésre állású mindig a** > **rendelkezésre állási csoportok** > **[a következő rendelkezésre állási csoport neve]**.
2. Kattintson a jobb gombbal **rendelkezésre állási másodpéldányok**, majd kattintson a **adja hozzá a replika**.
3. Alapértelmezés szerint hello **adja hozzá a replika tooAvailability csoport varázsló** jelenik meg. Kattintson a **Tovább** gombra.  Ha be van jelölve hello **ne jelenjen meg többé ez a lap** alján hello hello során lehetőséget egy előző indítsa el a varázsló, ez a képernyő nem jelenik meg.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)
4. Szükséges tooconnect tooall meglévő másodlagos replikák lesz. Rákattinthat a **Connect...** minden egyes replikának vagy mellett kattintson **összes csatlakozás...** hello a hello képernyő aljára. A hitelesítés után kattintson **következő** tooadvance toohello következő képernyőn.
5. A hello **replikák megadása** lapon több lapot hello tetején találhatók: **replikák**, **végpontok**, **biztonsági mentési beállítások**, és **Figyelő**. A hello **replikák** lapra, majd **Azure replika hozzáadása...** toolaunch hello Azure replika hozzáadása varázsló.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)
6. Válassza ki egy meglévő Azure felügyeleti tanúsítványt a helyi Windows tanúsítványtároló hello, ha a telepítése előtt egy. Válassza ki vagy adja meg Azure-előfizetés hello partnerazonosítóját, ha egy előtt használt. Kattintson a letöltés toodownload és telepítse az Azure felügyeleti tanúsítvánnyal, és töltse le az Azure-fiók használatával előfizetések hello listáját.
   
    ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)
7. Egyes mezők és értékeket, amelyeket lesz használt toocreate hello Azure virtuális gép (VM), amely helyt hello replika hello oldalon fogja majd feltölteni.
   
   | Beállítás | Leírás |
   | --- | --- |
   | **Kép** |Válassza ki az operációs rendszer és az SQL Server szükséges hello kombinációját |
   | **Virtuálisgép-mérettel** |Válassza ki a virtuális Gépet, amely az üzleti igényeknek leginkább megfelelő hello mérete |
   | **Virtuális gép neve** |Adjon meg egy egyedi nevet hello új virtuális Gépet. hello neve kell csak 3 és 15 karakter közötti lehet, is tartalmazhat csak betűket, számokat és kötőjeleket tartalmazhat, és kell betűvel kezdődhet, és betűvel vagy számmal végződhet. |
   | **Virtuális gép felhasználónév** |Adjon meg egy felhasználónevet, a virtuális gép hello hello rendszergazdai fiók lesz |
   | **Virtuális gép rendszergazdai jelszó** |Adjon meg egy jelszót hello új fiók |
   | **Jelszó megerősítése** |Új fiók hello hello jelszó megerősítése |
   | **Virtuális hálózat** |Adja meg a hello Azure virtuális hálózat adott hello új virtuális Gépet kell használnia. További információ a virtuális hálózatok: [virtuális hálózat áttekintése](../../../virtual-network/virtual-networks-overview.md). |
   | **Virtuális hálózati alhálózat** |Adja meg a virtuális hálózati alhálózat hello adott hello használandó új virtuális gép |
   | **Tartomány** |Ellenőrizze a hello előre megadott érték hello tartomány |
   | **Tartományi felhasználónév** |Adjon meg egy fiókot, amely hello helyi Rendszergazdák csoport tagja a helyi fürtcsomópont hello |
   | **Jelszó** |Hello jelszót hello tartományi felhasználó nevét |
8. Kattintson a **OK** toovalidate hello központi telepítési beállítások.
9. Jogi feltételek mellett jelenik meg. Olvassa el, és kattintson a **OK** Ha beleegyezik toothese feltételeit.
10. Hello **meg replikák** oldal akkor jelenik meg újra. Ellenőrizze az új Azure replikát hello hello hello beállításokat **replikák**, **végpontok**, és **biztonsági mentési beállítások** lapokon. Módosítsa a beállításokat toomeet üzleti igényei.  Hello paraméterek ezeken a lapokon található további információ: [replikák lapon adja meg (új rendelkezésre állási csoport varázsló/Add replika varázsló)](https://msdn.microsoft.com/library/hh213088.aspx). Vegye figyelembe, hogy figyelői nem hozható létre hello figyelő lapján, amelyek tartalmazzák az Azure-replikák rendelkezésre állási csoportokat. Emellett ha egy figyelő már létrejött korábbi toolaunching hello varázsló, kapni fog egy üzenet, amely azt jelzi, hogy nem támogatott az Azure-ban. Hogyan követően áttekintjük a hello toocreate figyelői **hozzon létre egy rendelkezésre állási csoport figyelőjének** szakasz.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)
11. Kattintson a **Tovább** gombra.
12. Azt szeretné, hogy a hello toouse hello szinkronizálási módszer kiválasztása **kezdeti adatszinkronizálás kiválasztása** lapot, és kattintson **következő**. A legtöbb esetben válassza **teljes adatszinkronizálás**. Adatok szinkronizálási módszerekkel kapcsolatos további információkért lásd: [válasszon kezdeti szinkronizálás lap (mindig a rendelkezésre állási csoport varázslók)](https://msdn.microsoft.com/library/hh231021.aspx).
13. Tekintse át a hello hello eredményeket **érvényesítési** lap. Javítsa ki a fennálló problémákat, és futtassa újból hello érvényesítési szükség esetén. Kattintson a **Tovább** gombra.
    
     ![SQL](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)
14. Tekintse át a hello hello beállításokat **összegzés** lapon, majd kattintson az **Befejezés**.
15. megkezdődik a hello létesítésének folyamatát kell használnia. Ha hello varázsló sikeresen befejeződött, kattintson **Bezárás** tooexit hello varázsló kívül.

> [!NOTE]
> hello Azure replika hozzáadása varázsló létrehoz egy naplófájlt az Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Ez a naplófájl nem sikerült használt tootroubleshoot Azure replika központi telepítések lehet. Hello varázsló nem sikerül, a művelet végrehajtása, ha minden korábbi műveletek visszavonásra kerül, beleértve a hello törlése kiépítve a virtuális gép.
> 
> 

## <a name="create-an-availability-group-listener"></a>Hozzon létre egy rendelkezésre állási csoport figyelőjének
Hello rendelkezésre állási csoport létrehozása után készítsen egy figyelőt a következő ügyfelek tooconnect toohello replikákat. Figyelők bejövő kapcsolatok tooeither hello elsődleges vagy egy írásvédett másodlagos replikára irányítja. A figyelők további információkért lásd: [egy ILB figyelőt az Always On rendelkezésre állási csoportok konfigurálása az Azure-](../classic/ps-sql-int-listener.md).

## <a name="next-steps"></a>Következő lépések
Az összeadás toousing hello **replika Azure hozzáadása varázsló** tooextend az Always On rendelkezésre állási csoportnak tooAzure is helyezheti előfordulhat, hogy néhány SQL Server számítási feladatait teljesen tooAzure. tooget indult el, lásd: [Azure SQL Server virtuális gépek kiépítése](../sql/virtual-machines-windows-portal-sql-server-provision.md).

Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépeken](../sql/virtual-machines-windows-sql-server-iaas-overview.md).

