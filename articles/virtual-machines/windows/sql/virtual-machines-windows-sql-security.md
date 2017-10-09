---
title: az Azure SQL Server szempontjai aaaSecurity |} Microsoft Docs
description: "Ez a témakör egy Azure virtuális gépen futó SQL Server biztonságához általános útmutatást biztosít."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2017
ms.author: jroth
ms.openlocfilehash: 14c3d828fa87446da67beea6d28886de254afe15
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Az SQL Server Azure-beli virtuális gépeken történő futtatásának biztonsági szempontjai

Ez a témakör általános biztonsági irányelvek, amelyek biztonságos hozzáférést tooSQL kiszolgálópéldányok felderíthető egy Azure virtuális gépen (VM) tartalmaz.

Azure több iparági szabályozások és szabványok gyűjteménye, amelyek lehetővé teszik a toobuild megfelelő megoldást egy virtuális gépen futó SQL Server megfelel. Az előírásoknak való megfelelés az Azure-ral kapcsolatos információkért lásd: [Azure biztonsági és adatkezelési központ](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-toohello-sql-vm"></a>Vezérlő hozzáférés toohello SQL virtuális gép

Az SQL Server virtuális gép létrehozásakor fontolja meg, hogyan toocarefully szabályozza a hozzáférést toohello gép és a kiszolgáló tooSQL. Általában akkor tegye a következőket hello:

- Korlátozza a hozzáférést tooSQL kiszolgáló tooonly hello alkalmazások és ügyfelek számára, amelyek azt.
- Kövesse az ajánlott eljárások a felhasználói fiókok és jelszavak kezelése.

a következő szakaszok hello ad javaslatokat a ezen a ponton keresztül számbavétele.

## <a name="secure-connections"></a>Biztonságos kapcsolaton

Egy SQL Server virtuális gép létrehozásakor gyűjtemény képének hello **SQL Server-kapcsolat** beállítást ad meg a választott hello **(VM) belül helyi**, **titkos (virtuális hálózaton belül)** , vagy **nyilvános (Internet)**.

![SQL Server-kapcsolat](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

Hello nagyobb biztonság érdekében válassza ki a forgatókönyvnek a hello szigorúbb beállítást. Például ha egy alkalmazás, amely hozzáfér az SQL Server a hello azonos virtuális gép, majd **helyi** hello legbiztonságosabb választás. Ha az Azure alkalmazás hozzáférés toohello SQL Server, majd futtatja **titkos** titkosítja a kommunikációs tooSQL Server csak a megadott hello belül [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Ha szüksége **nyilvános** (internest) hozzáférési toohello SQL Server virtuális gép, majd győződjön meg arról, hogy toofollow Ez a témakör tooreduce a támadási felület más ajánlott eljárásokat.

hello hello portálon kijelölt beállítások bejövő biztonsági szabályok használata a virtuális gépek hello [hálózati biztonsági csoport](../../../virtual-network/virtual-networks-nsg.md) (NSG) tooallow vagy megtagadják a hálózati forgalom tooyour virtuális gépet. Módosíthatja, vagy hozzon létre új bejövő NSG-szabályok tooallow forgalom toohello SQL Server portja (alapértelmezés szerint 1433). Ezen a porton keresztül toocommunicate engedélyezett IP-címek is megadható.

![Hálózati biztonsági csoportszabályok](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Ezenkívül tooNSG szabályok toorestrict hálózati forgalmat, használhatja a Windows tűzfal hello hello virtuális gépen.

Ha végpontok hello klasszikus üzembe helyezési modellt használ, távolítsa el a végpontok hello virtuális gépen, ha nem használja. Az ACL-ekkel végpontokon útmutatásért lásd: [kezelése hello ACL a végpont](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint). Erre nincs szükség virtuális gépek erőforrás-kezelő hello használó.

Végül fontolja meg az Azure virtuális gép az SQL Server adatbázismotor hello hello példányának titkosított kapcsolatainak engedélyezésére. SQL server-példány konfigurálása egy aláírt tanúsítvánnyal. További információkért lásd: [titkosított kapcsolatok engedélyezése toohello adatbázismotor](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) és [kapcsolati karakterlánc szintaxisát](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Egy nem alapértelmezett portot használja.

Alapértelmezés szerint az SQL Server porton figyel egy jól ismert, 1433. A biztonság fokozása érdekében konfigurálja a SQL Server toolisten egy nem alapértelmezett portot, például a 1401. Ha egy SQL Server gyűjtemény lemezképet hello Azure-portálon, megadhatja ezt a portot hello **SQL Server-beállítások** panelen.

tooconfigure a kiépítés, után két lehetőség közül választhat:

- Erőforrás-kezelő virtuális gépekhez, kiválaszthatja a **SQL Server-konfigurációs** hello VM – áttekintés paneljén. Ez lehetővé teszi toochange hello port.

  ![TCP-port a portál módosítása](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Klasszikus virtuális gépek vagy SQL Server virtuális gépek nem kiépített hello portálon távolról csatlakozzon a virtuális gép toohello manuálisan konfigurálhatja a hello port. Hello konfigurálás lépéseinek végrehajtásához tekintse meg a [egy kiszolgáló tooListen konfigurálása egy adott TCP-port](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). Ha ez a manuális módszer használata esetén szükség tooadd a Windows tűzfal szabály tooallow bejövő forgalmat a TCP-porton.

> [!IMPORTANT]
> Adja meg egy nem alapértelmezett portot nem árt, ha az SQL Server port nyitva toopublic internetes kapcsolatok.

Ha SQL Server egy nem alapértelmezett portot figyel, meg kell adnia hello port csatlakoztatásakor. Példaként vegyünk egy forgatókönyvet, ahol hello kiszolgáló IP-címe 13.55.255.255, SQL Server 1401 portot figyel. tooconnect tooSQL kiszolgáló, kellene megadnia `13.55.255.255,1401` hello kapcsolati karakterláncban.

## <a name="manage-accounts"></a>Fiókok kezelése

Nem szeretné, hogy a támadók tooeasily becslés fiók nevét és jelszavát. A következő tippek toohelp hello használata:

- Hozzon létre egy egyedi helyi rendszergazdai fiók nem nevű **rendszergazda**.

- A fiókok összetett erős jelszavakat használjon. További információ toocreate egy erős jelszót, lásd: [hozzon létre egy erős jelszót](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) cikk.

- Alapértelmezés szerint Azure kiválasztja azokat a Windows-hitelesítés SQL Server virtuális gép telepítése során. Ezért hello **SA** bejelentkezés le van tiltva, és egy jelszó telepítője által hozzárendelt. Azt javasoljuk, hogy hello **SA** bejelentkezési nem szabad használni vagy engedélyezve. Ha egy SQL-bejelentkezési névvel kell rendelkeznie, használja a következő stratégiák hello egyikét:

  - Egy SQL-fiók létrehozása egyedi névvel rendelkező **sysadmin** tagságát. Ehhez a hello portálról engedélyezésével **SQL-hitelesítés** kiépítése során.

    > [!TIP] 
    > Ha nem az SQL-hitelesítés engedélyezi a kiépítés során, manuálisan módosítania kell hello hitelesítési mód túl**SQL Server és a Windows-hitelesítési mód**. További információkért lásd: [Server hitelesítési mód váltása](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Hello használata **SA** bejelentkezés engedélyezése hello bejelentkezés után üzembe helyezési és hozzárendelése egy új erős jelszót.

## <a name="follow-on-premises-best-practices"></a>Kövesse a helyszíni gyakorlati tanácsok

Továbbá ebben a témakörben leírt eljárások toohello ajánlott áttekinteni és megvalósításához hello hagyományos helyszínen biztonsági eljárásokkal, szükség szerint. További információkért lásd: [biztonsági szempontok az SQL Server telepítéséhez](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Következő lépések

Ha Ön is ajánlott eljárások teljesítményének körül iránt érdeklődik, lásd: [teljesítmény ajánlott eljárások az SQL Server Azure virtuális gépek](virtual-machines-windows-sql-performance.md).

Egyéb témakörök kapcsolódó toorunning SQL Server Azure virtuális gépeken, a következő témakörben: [SQL Server Azure virtuális gépek – áttekintés](virtual-machines-windows-sql-server-iaas-overview.md).

