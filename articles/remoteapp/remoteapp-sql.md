---
title: "aaaSQL Azure és az Azure RemoteApp |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse SQL Azure és az Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
editor: 
ms.assetid: 35f81d75-bfd7-4980-807e-00339f2cb2a4
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: fec4cb1f1ab3cde03b6ff613650e01bae3552824
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sql-azure-with-azure-remoteapp"></a>Az SQL Azure és az Azure RemoteApp
> [!IMPORTANT]
> Az Azure RemoteApp 2017. augusztus 31-ét követően megszűnik. Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.
> 
> 

Gyakran, ha a felhasználók választhatják toohost a Windows-alkalmazások az Azure RemoteApp hello felhőben azok is szeretné, hogy az adatok, például az SQL Server-kiszolgálók hello be a felhő toomigrate egy teljes körű felhőalapú telepítés. Ez lehetővé teszi egy teljesen a felhőben üzemeltetett megoldás megvalósítását, amely bármikor, bármilyen eszköz használatával elérhető az Azure RemoteApp használatával. Az alábbiakban hivatkozásokat, és hivatkozik, valamint útmutatást toohelp meg ezt a folyamatot.  

## <a name="migrate-your-sql-data"></a>SQL-adatok áttelepítése
Kezdje [egy SQL Server adatbázis tooAzure SQL-adatbázis áttelepítése](../sql-database/sql-database-cloud-migrate.md). 

## <a name="configure-azure-remoteapp"></a>Az Azure RemoteApp konfigurálása
Windows-alkalmazásait az Azure RemoteAppban üzemeltetheti. Az alábbiakban egy magas szintű részletes útmutatót talál:

1. Hozzon létre hello [Azure RemoteApp sablon virtuális Gépet](remoteapp-imageoptions.md). 
2. A virtuális gép hello szükséges hello alkalmazás telepítéséhez.
3. Hello alkalmazások konfigurálása, hogy csatlakozzon az SQL DB toohello, és ellenőrizze, hogy működik.
4. A Sysprep és a Leállítás hello virtuális gép. Rögzítse a virtuális gépet rendszerképként az Azure-ral való használathoz. **Megjegyzés:** tooensure, hogy a hello alkalmazás képes tooretain hello DB kapcsolati információ hello sysprep folyamat szüksége lesz. Ha hello alkalmazás nem tooretain hello DB kapcsolati adatokat, érdemes lehet a tooengage hello gyártójához hello alkalmazás toocheck hogyan hello kapcsolati karakterlánc adható meg.
5. Hello egyéni lemezképet importálja az Azure RemoteApp-címtárba hello megfelelő földrajzi hely, amely az SQL Azure üzemelő található kiválasztása. 
6. Telepítsen egy hello ugyanazokat az adatokat, az SQL Azure üzemelő hello fenti sablon használatával középpontba állításához és hello alkalmazás közzététele a RemoteApp-gyűjtemény. Azure RemoteApp üzembe helyezés hello azonos adatközpontba, ahol az SQL Azure üzemelő segít annak biztosítása érdekében hello leggyorsabb kapcsolat sebessége és késés csökkentésére. 

## <a name="app-and-sql-configuration-considerations"></a>Az alkalmazás és az SQL konfigurálására vonatkozó megfontolások:
Van néhány pontok tooconsider Azure SQL RemoteApp használata esetén:

Ismerje meg, [hogyan tooconfigure egy Azure SQL adatbázis-tűzfal](../sql-database/sql-database-firewall-configure.md). Egy olyan hello cikk államokból "kezdetben minden hozzáférési tooyour Azure SQL Database-kiszolgálóhoz van hello tűzfal blokkolja. Rendelés toobegin használata az Azure SQL Database-kiszolgálóhoz kell nyissa meg a klasszikus portálon toohello és adjon meg egy vagy több kiszolgálószintű tűzfalszabályt, amely engedélyezi hozzáférés tooyour Azure SQL Database-kiszolgálóhoz. Hello tűzfal szabályok toospecify mely IP-címtartományt, az Internet engedélyezettek hello, és használjon-e az Azure-alkalmazások megpróbálhatnak tooconnect tooyour Azure SQL Database-kiszolgálóhoz."

Azt is, amikor egy számítógép megpróbál tooconnect tooyour adatbázis-kiszolgálójának hello Internet, hello tűzfal ellenőrzi származó IP-címét a kiszolgálói szintű hello teljes készletének hello kérelmet hello és (ha szükséges) adatbázis-adatbázisszintű tűzfalszabályok. "Ha hello IP-cím hello kérelem hello kiszolgálószintű tűzfal szabályokban megadott hello tartományok egyikébe esik, hello kapcsolat kap tooyour Azure SQL Database-kiszolgálóhoz." Így nemcsak egyes forrás IP-címek, hanem IP-címtartományok is használhatók.

Hello részletes utasításokat követve [hogyan: hello Azure portál használatával SQL Database tűzfalbeállításainak konfigurálása](../sql-database/sql-database-configure-firewall-settings.md) toospecify hello IP-címtartományt. Hello SQL-tűzfalszabályok konfigurálásakor adja meg hello IP-címtartomány a megadott alhálózati hello hello Azure RemoteApp-gyűjteményt. Ez lehetővé teszi hello ARA-kiszolgálók tooconnect toohello SQL DB annak ellenére, hogy azok rendszer dinamikusan-hozzárendelt IP-címeket.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha hello használatával, amely a tooa SQL Azure Remoteappban üzemeltetett ügyfélalkalmazás database Azure-platformon futó, vagy a helyszíni lassú néhány oka lehet miért.  

* Az eszköz tooAzure közötti hálózati késés túl magas. Helyezze át a toohello legjobb és leggyorsabb hálózati kapcsolatra a legjobb teljesítmény érdekében. Használjon [azurespeed.com](http://azurespeed.com/) egy általános eszköz tootest, az eszközök késés tooAzure adatközpont.  
* Az Azure RemoteAppban üzemeltetett ügyfélalkalmazás magas terhelés alatt áll. Egy másik számlázási csomag, például a prémium tarifacsomag javítja a teljesítményt. Egy másik trükk az alkalmazás által felhasznált toomonitor hello erőforrások: egy aktív munkamenet közben a ctrl-alt-end billentyűkombinációt, amely elindítja a biztonsági Társítások képernyőn válassza ki a Feladatkezelőt, megfigyelheti, az alkalmazás erőforrás-használat hello végrehajtani.
* Az SQL Server magas terhelés alatt áll, vagy nincs optimalizálva. A hibaelhárításhoz kövesse az SQL útmutatásait. 

