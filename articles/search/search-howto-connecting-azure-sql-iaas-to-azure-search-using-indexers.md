---
title: "aaaSQL VM kapcsolat tooAzure keresési |} Microsoft Docs"
description: "Engedélyezze a titkosított kapcsolatokat, és hello tűzfal tooallow kapcsolatok tooSQL kiszolgáló konfigurálása az Azure Search indexelőt egy Azure virtuális gép (VM)."
services: search
documentationcenter: 
author: HeidiSteen
manager: pablocas
editor: 
ms.assetid: 46e42e0e-c8de-4fec-b11a-ed132db7e7bc
ms.service: search
ms.devlang: rest-api
ms.workload: search
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 01/23/2017
ms.author: heidist
ms.openlocfilehash: 1f0db8a2812b0a7d012e58bb873c4b2b29fa1338
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-a-connection-from-an-azure-search-indexer-toosql-server-on-an-azure-vm"></a>Egy egy Azure Search-indexelőt tooSQL kiszolgáló közötti kapcsolat kezelése egy Azure virtuális gépen
Leírtaknak megfelelően [csatlakozás az Azure SQL Database tooAzure keresés indexelők használatával](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md#faq), elleni indexelők létrehozása **SQL Server Azure virtuális gépeken** (vagy **SQL Azure virtuális gépek** röviden) van Azure Search által támogatott, de néhány biztonsági Előfeltételek tootake első kiszolgálásához. 

**Tevékenység időtartama:** körülbelül 30 percet, feltéve, hogy már telepített tanúsítványt hello virtuális gép.

## <a name="enable-encrypted-connections"></a>Engedélyezze a titkosított kapcsolatokat
Az Azure Search van szükség, egy nyilvános internetkapcsolaton keresztül egy titkosított csatornán indexelő minden olyan kérelem esetében. Ez a rész felsorolja hello lépéseket toomake ezt a munkát.

1. Ellenőrizze, hello tooverify hello tanúsítványtulajdonos nevének hello tulajdonságainak hello teljesen minősített tartományneve (FQDN) hello Azure virtuális Gépen. Egy eszköz, például CertUtils használhatja, vagy hello a tanúsítványok beépülő modul tooview hello tulajdonságait. Hello FQDN letölthető hello VM szolgáltatás panelre Essentials szakaszban, a hello **nyilvános IP-cím/DNS-névcímke** mezőjét, hello [Azure-portálon](https://portal.azure.com/).
   
   * Hello újabb használatával létrehozott virtuális gépek **erőforrás-kezelő** sablon, hello FQDN fájlrendszere `<your-VM-name>.<region>.cloudapp.azure.com`. 
   * Régebbi virtuális gépek jönnek létre például egy **klasszikus** VM, FQDN fájlrendszere hello `<your-cloud-service-name.cloudapp.net>`. 
2. Állítsa be az SQL Server toouse hello tanúsítványt hello beállításjegyzék-szerkesztővel (regedit) használatával. 
   
    Bár ez a feladat gyakran használják az SQL Server Configuration Manager, a forgatókönyv nem használható. Nem találja, hello tanúsítvány importálása sikertelen, mert hello hello Azure virtuális gép teljes Tartományneve nem egyezik meg a hello FQDN hello (azonosított hello tartományi vagy helyi számítógép hello, vagy hello hálózati tartomány toowhich tartományhoz) virtuális gép alapján. Ha nevei nem egyeznek meg, a regedit toospecify hello tanúsítvány használatára.
   
   * A regedit, keresse meg a toothis beállításkulcs: `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Microsoft SQL Server\[MSSQL13.MSSQLSERVER]\MSSQLServer\SuperSocketNetLib\Certificate`.
     
     Hello `[MSSQL13.MSSQLSERVER]` rész verzióján és példányán neve alapján változik. 
   * Állítsa be hello hello **tanúsítvány** kulcs toohello **ujjlenyomat** hello SSL-tanúsítványának toohello VM importált.
     
     Nincsenek néhány jobban számos módon tooget hello ujjlenyomata. A hello másoláskor **tanúsítványok** beépülő MMC-ben, akkor valószínűleg felveszi egy láthatatlan kezdő karakter [támogatási cikkben leírtak szerint](https://support.microsoft.com/kb/2023869/), amely hibát eredményez tett kísérlet során egy a kapcsolat. Néhány lehetséges megoldások a probléma kijavítása létezik. hello legegyszerűbb toobackspace keresztül, és írja be újra az első karakterének hello hello ujjlenyomat tooremove hello kezdő karakter hello regedit mezőjében kulcs értékét. Másik megoldásként használhatja egy másik eszköz toocopy hello ujjlenyomatot.
3. Engedélyek toohello biztosítása a szolgáltatásfiók számára. 
   
    Ellenőrizze, hogy az SQL Server-szolgáltatásfióknak hello hello SSL-tanúsítványának hello titkos kulcsot a megfelelő engedélyt kap. Ha eltéveszthetők ezt a lépést, az SQL Server nem fog elindulni. Használhatja a hello **tanúsítványok** beépülő modul vagy **CertUtils** ehhez a feladathoz.
4. Hello SQL Server-szolgáltatás újraindításához.

## <a name="configure-sql-server-connectivity-in-hello-vm"></a>SQL Server-kapcsolat konfigurálása a virtuális gép hello
Miután beállította az Azure Search által igényelt hello titkosított kapcsolati, nincsenek további konfigurációs lépéseket belső tooSQL Server Azure virtuális gépeken. Ha még nem tette meg, a hello következő lépésre toofinish a konfigurációval vagy az egyik ezek a cikkek:

* Az egy **erőforrás-kezelő** virtuális gép, lásd: [csatlakozzon az SQL Server virtuális gépet az Azure Resource Manager használatával tooa](../virtual-machines/windows/sql/virtual-machines-windows-sql-connect.md). 
* Az egy **klasszikus** virtuális gép, lásd: [csatlakozzon az SQL Server virtuális gépet az Azure klasszikus tooa](../virtual-machines/windows/classic/sql-connect.md).

Különösen, tekintse át az egyes cikkekhez tartozó hello részt "kapcsolaton keresztül csatlakozó internet hello szövegrészt".

## <a name="configure-hello-network-security-group-nsg"></a>Hálózati biztonsági csoport (NSG) hello konfigurálása
Nincs szokatlan tooconfigure hello NSG-t és a megfelelő Azure-végpont vagy a hozzáférés-vezérlési lista (ACL) toomake az Azure virtuális gép elérhető tooother felek. Valószínűleg régebben már kötöttek ez előtt tooallow saját alkalmazás logika tooconnect tooyour SQL Azure virtuális Gépen. Nem különbözik, egy Azure Search kapcsolat tooyour SQL Azure virtuális gép számára. 

hello a lenti hivatkozásokra kattintva utasításokkal láthatja el a Virtuálisgép-telepítések NSG-konfigurációban. Ezek az utasítások tooACL Azure SEarch végpont alapján az IP-címét használja.

> [!NOTE]
> Háttér, lásd: [Mi az a hálózati biztonsági csoport?](../virtual-network/virtual-networks-nsg.md)
> 
> 

* Az egy **erőforrás-kezelő** virtuális gép, lásd: [hogyan toocreate NSG-ket ARM telepítések](../virtual-network/virtual-networks-create-nsg-arm-pportal.md). 
* Az egy **klasszikus** virtuális gép, lásd: [hogyan toocreate NSG-ket klasszikus telepítések](../virtual-network/virtual-networks-create-nsg-classic-ps.md).

IP-címzést, amelyek könnyen vannak megoldásához, ha tisztában lehet a hello probléma és a lehetséges megoldások néhány kihívást jelenthet. fennmaradó hello szakaszokban az ajánlások hello ACL problémák kapcsolódó tooIP címek kezelésére.

#### <a name="restrict-access-toohello-search-service-ip-address"></a>Korlátozza a hozzáférést toohello keresési szolgáltatás IP-címe
Erősen ajánlott hello hozzáférés toohello IP-címet a keresési szolgáltatás a hozzáférés-vezérlési lista hello korlátozhatja az SQL Azure virtuális gépeken, nyílt tooany kapcsolatkérelmeknek elvégzése helyett. Könnyedén megtalálhatja a kimenő hello IP cím történő megpingelésével hello teljesen minősített Tartományneve (például `<your-search-service-name>.search.windows.net`) a search szolgáltatás.

#### <a name="managing-ip-address-fluctuations"></a>IP-cím ingadozását kezelése
Ha a keresési szolgáltatást (Ez azt jelenti, hogy egy másodpéldány és egy partíció) csak egy keresési egység, hello IP-cím rutin szolgáltatás újraindul, a keresési szolgáltatás IP-címmel meglévő ACL érvénytelenítése során fogja módosítani.

Egyirányú tooavoid hello ezt követő csatlakozási hiba: a toouse több mint egy replika és az Azure Search egy partíciót. Ez növeli a hello költség, de is megoldja a hello IP-cím probléma. Az Azure Search IP-címek ne módosítsa, ha egynél több keresési egység.

A második megközelítés tooallow hello kapcsolat toofail, és konfigurálja újra a hello NSG ACL-ek hello. Átlagosan számíthat IP-címek toochange minden néhány hétig. Azok számára, aki elvégzi a alkalomszerű alapon ellenőrzött indexelő Ez a megközelítés életképes lehet.

Egy harmadik életképes (de nem a különösen biztonságos) megoldás, toospecify hello IP-címtartománya a hello Azure-régió, ahol a keresési szolgáltatás ki van építve. hello IP-címtartománylista, amelyből nyilvános IP-címek tooAzure erőforrásokat foglal le a közzétett [Azure Datacenter IP-címtartományok](https://www.microsoft.com/download/details.aspx?id=41653). 

#### <a name="include-hello-azure-search-portal-ip-addresses"></a>Hello Azure Search portál IP-címeket tartalmaz
Hello Azure portál toocreate indexelőt használ, ha Azure Search portál logikát is kell hozzáférést tooyour SQL Azure virtuális gép létrehozás közbeni. Az Azure search portál IP-címek található történő megpingelésével `stamp2.search.ext.azure.com`.

## <a name="next-steps"></a>Következő lépések
Hello útból konfigurációjával megadhat egy SQL Server Azure virtuális gépen az Azure Search-indexelőt hello adatforrásként. Lásd: [csatlakozás az Azure SQL Database tooAzure keresés indexelők használatával](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md) további információt.

