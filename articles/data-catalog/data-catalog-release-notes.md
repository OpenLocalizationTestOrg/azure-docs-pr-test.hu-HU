---
title: "aaaAzure Data Catalog kibocsátási megjegyzései |} Microsoft Docs"
description: "Kibocsátási megjegyzések az Azure Data Catalog."
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 3aca9c49-45a4-4352-92e6-bd25ee3eacf7
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: 661826f66020ba72a885c6b14522b53c8b469d20
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-catalog-release-notes"></a>Az Azure Data Catalog kibocsátási megjegyzései
## <a name="notes-for-hello-november-20-2015-release-of-azure-data-catalog"></a>Az Azure Data Catalog hello 2015. November 20 kibocsátási megjegyzései
### <a name="opening-data-sources-in-power-bi-desktop"></a>A Power BI Desktop nyitó adatforrások
Ha hello "Nyissa meg a Power BI Desktop" lehetőséget választja, a hello **Azure Data Catalog** portálon felhasználók szembesülhetnek a Power BI Desktop alkalmazás hello két problémák egyikét:

* A "Nem tooOpen dokumentum" hello címmel párbeszédpanel
* a Power BI Desktop alkalmazás hello nyílik meg, de hello fájl üres toobe jelenik meg.

Az egyes esetekben hello probléma megoldásához érdemes letöltése és telepítése a Power BI Desktop legújabb verziójának hello [powerbi.com webhelyen](https://powerbi.com).

## <a name="notes-for-hello-november-13-2015-release-of-azure-data-catalog"></a>Az Azure Data Catalog hello 2015. November 13 kibocsátási megjegyzései
### <a name="registering-and-connecting-tooteradata"></a>Regisztráció és tooTeradata csatlakozás
TooTeradata adatforrások felhasználók hello megfelelő Teradata ODBC-illesztőprogram van telepítve, amely egyezik hello (32 bites vagy 64 bites) a használt hello szoftver kapcsolódáskor.

Ez LÉPETT frissítésétől kiadás dátuma, hello legutóbbi [Teradata ODBC-illesztőprogram (verzió: 15.10) Windows](http://downloads.teradata.com/download/connectivity/odbc-driver/windows) kompatibilis Office 2013, de nem Office 2016.

## <a name="notes-for-hello-july-13-2015-release-of-azure-data-catalog"></a>Az Azure Data Catalog hello 2015. július 13 kibocsátási megjegyzései
### <a name="registering-and-connecting-toooracle-database"></a>Regisztráció és tooOracle adatbázis csatlakoztatása
Ha tooOracle adatbázis adatok források felhasználók csatlakozás telepíthető hello megfelelő Oracle illesztőprogramokat, amely egyezik hello (32 bites vagy 64 bites) a használt hello szoftver.

* 32 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor hello 32 bites Oracle illesztőprogramok fogja használni
* 64 bites Windows rendszert futtató kiszolgálón tárolt olyan adatforrások Oracle regisztrálásakor hello 64 bites Oracle illesztőprogramok fogja használni
* Ha tooOracle adatforrások Excel használatával a Microsoft Office hello 32 bites verzióját futtató számítógépen, beleértve a 64 bites Windows rendszeren hello 32 bites Oracle illesztőprogramok fogja használni
* Excel használatával a Microsoft Office hello 64 bites verzióját futtató számítógépen tooOracle adatforrások kapcsolódáskor használandó hello 64 bites Oracle illesztőprogramok

### <a name="registering-and-connecting-toosql-server-reporting-services"></a>Regisztráció és tooSQL Server Reporting Services csatlakoztatása
A rendszer jelenleg korlátozott tooNative mód csak kiszolgálók az SQL Server Reporting Services (SSRS) adatforrásokat támogatja. A SharePoint módban SSRS támogatása egy későbbi kiadásban lesz hozzáadva.

### <a name="opening-data-assets-in-excel"></a>Az Excel megnyitásakor adategységeket
Amikor megnyitja a Microsoft Excel adategységeket hello **Azure Data Catalog** portálon kéri a egy **Microsoft Excel biztonsági figyelmeztetés** párbeszédpanel megnyitásához. Standard szintű, ami normális működés, és a felhasználók választható **engedélyezése** toocontinue.

További információkért lásd: [engedélyezheti vagy tilthatja le a biztonsági riasztások hivatkozások és a gyanús webhelyekről származó fájlokat](https://support.office.com/article/Enable-or-disable-security-alerts-about-links-and-files-from-suspicious-websites-A1AC6AE9-5C4A-4EB3-B3F8-143336039BBE).

### <a name="proxy-and-policy-configuration-and-data-source-registration"></a>Proxy- és házirend-konfiguráció és adatok adatforrás regisztrálása
Felhasználók is talál, ahol toohello Azure Data Catalog-portál, de amikor azok megkísérlik az toohello adatforrás-regisztráló eszköz szembesülnek olyan hibaüzenetet, amely megakadályozza, hogy bejelentkezzen a toolog jelentkezhetnek.

A probléma megoldásához két lehetséges oka is van:

**1. ok: Az Active Directory összevonási szolgáltatások konfigurációs** hello adatforrás-regisztráló eszköz használja az űrlapos hitelesítés toovalidate felhasználói bejelentkezések Active Directoryban. Sikeres bejelentkezés az űrlapos hitelesítés engedélyezni kell a globális hitelesítési házirend hello egy Active Directory-rendszergazda.

Bizonyos esetekben a hiba oka lehet csak ha hello felhasználó hello vállalati hálózaton található, vagy csak akkor, ha hello felhasználó külső hello vállalati hálózathoz csatlakozik. hello globális hitelesítési házirend lehetővé teszi, hogy a hitelesítési módszerek toobe az intranetes és extranetes kapcsolatok külön engedélyezve. Bejelentkezési hiba akkor fordulhat elő, ha nincs engedélyezve az űrlapos hitelesítés hello hálózati mely hello a csatlakozó felhasználó.

További információkért lásd: [hitelesítési házirendek konfigurálása](https://technet.microsoft.com/library/dn486781.aspx).

**2. ok: Hálózati proxykonfigurációt** hello vállalati hálózat proxykiszolgálót használ, ha hello regisztrációs eszköz valószínűleg nem fogja tudni tooconnect tooAzure Active Directory hello proxyn keresztül. Felhasználók bizonyosodjon meg arról, hogy hello frissítésregisztráló eszköz hello eszköz konfigurációs fájlt, ez a szakasz a toohello fájl hozzáadása:

      <system.net>
        <defaultProxy useDefaultCredentials="true" enabled="true">
          <proxy usesystemdefault="True"
                          proxyaddress="http://<your corporate network proxy url>"
                          bypassonlocal="True"/>
        </defaultProxy>
      </system.net>


toolocate hello RegistrationTool.exe.config fájlt, indítsa el a hello regisztráló eszközzel, és nyissa meg a Windows Feladatkezelő segédprogram hello. A Feladatkezelő hello Részletek lapján kattintson a jobb gombbal a RegistrationTool.exe és hello legördülő menüből válassza ki a fájl helyének megnyitása.
