---
title: "SSL-kapcsolatot aaaConfigure Azure adatbázisban a következő PostgreSQL |} Microsoft Docs"
description: "Utasításokat és az információk tooconfigure PostgreSQL és a társított alkalmazás tooproperly Azure-adatbázis használata az SSL-kapcsolatok."
services: postgresql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: postgresql
ms.custom: 
ms.topic: article
ms.date: 05/15/2017
ms.openlocfilehash: 96a68088acd924196701e8d618d9d5edf44cb548
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql"></a>SSL-kapcsolatot PostgreSQL Azure-adatbázis konfigurálása
Azure-adatbázis PostgreSQL inkább csatlakozás a ügyfél alkalmazások toohello Secure Sockets Layer (SSL) PostgreSQL-szolgáltatást. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.

Alapértelmezés szerint a hello PostgreSQL-adatbázis szolgáltatás konfigurált toorequire SSL-kapcsolat. Másik lehetőségként letilthatja SSL megkövetelése tooyour adatbázis-szolgáltatás Kapcsolódás, ha az ügyfélalkalmazást nem támogatja az SSL-kapcsolatot. 

## <a name="enforcing-ssl-connections"></a>SSL-kapcsolatok kényszerítése
Az összes Azure Database hello Azure-portál és a parancssori felületen keresztül szerezhetők PostgreSQL-kiszolgálók esetén az SSL-kapcsolatok kényszerítési alapértelmezés szerint engedélyezve van. 

Hasonlóképpen hello "Kapcsolati karakterláncok" beállításokat a kiszolgálón a hello Azure-portálon előre definiált kapcsolati karakterláncok tartalmaznak hello szükséges paraméterek közös nyelvek tooconnect tooyour adatbázis-kiszolgáló SSL-t használ. hello SSL paraméter függ hello összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.

## <a name="configure-enforcement-of-ssl"></a>Az SSL kényszerítésének
Igény szerint letilthatja végrehajtó SSL-kapcsolatot. A Microsoft Azure azt javasolja, hogy tooalways engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.

### <a name="using-hello-azure-portal"></a>Hello Azure-portál használatával
Keresse fel az Azure-adatbázis PostgreSQL-kiszolgáló, és kattintson a **kapcsolatbiztonsági**. Hello váltása gomb tooenable használja, vagy tiltsa le a hello **kényszerítése SSL-kapcsolat** beállítást. Ezután kattintson a **Save** (Mentés) gombra. 

![Kapcsolat biztonsági - Disable SSL kényszerítése](./media/concepts-ssl-connection-security/1-disable-ssl.png)

Hello megtekintésével ellenőrizheti a hello beállítás **áttekintése** lap toosee hello **SSL kényszerítése állapot** mutató.

### <a name="using-azure-cli"></a>Az Azure parancssori felület használata
Engedélyezheti vagy letilthatja a hello **ssl-kényszerítési** paraméter használatával `Enabled` vagy `Disabled` értékeket az Azure parancssori felület.

```azurecli
az postgres server update --resource-group myresourcegroup --name mypgserver-20170401 --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Ellenőrizze az alkalmazás vagy a keretrendszer támogatja SSL-kapcsolatok
Sok általános alkalmazás-keretrendszert használó PostgreSQL az adatbázis-szolgáltatásokhoz, például a Drupal és a Django, ne engedélyezze az SSL alapértelmezés szerint telepítése során. Engedélyezi az SSL-kapcsolatot kell elvégezni, a telepítés után, vagy a parancssori felület parancsai adott toohello alkalmazáson keresztül. Ha a PostgreSQL-kiszolgáló SSL-kapcsolatok kényszerít, és a kapcsolódó hello alkalmazás nincs megfelelően konfigurálva, a hello alkalmazás tooconnect tooyour adatbázis-kiszolgáló sikertelen lehet. További részleteket az alkalmazás dokumentációjának toolearn hogyan tooenable SSL-kapcsolatokat.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>A tanúsítványellenőrzés az SSL-kapcsolatot igénylő alkalmazások
Bizonyos esetekben az alkalmazások által létrehozott egy megbízható tanúsítvány hitelesítésszolgáltatói (CA) tanúsítvány (.cer) fájl tooconnect biztonságosan helyi tanúsítványfájl igényelnek. Tekintse meg a következő lépéseket tooobtain hello .cer fájl hello, hello tanúsítvány dekódolása, majd kösse tooyour alkalmazás.

### <a name="download-hello-certificate-file-from-hello-certificate-authority-ca"></a>Hello tanúsítványfájl letöltését hello tanúsítvány hitelesítésszolgáltatói (CA) 
hello tanúsítvány szükséges toocommunicate az Azure-adatbázissal SSL-en keresztül PostgreSQL-kiszolgálót nem találnak [Itt](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt). Töltse le a helyi hello tanúsítványfájlt.

### <a name="download-and-install-openssl-on-your-machine"></a>Töltse le és telepítse a OpenSSL a számítógépen 
az alkalmazás tooconnect biztonságosan szükséges toodecode hello tanúsítványfájl tooyour adatbázis-kiszolgáló, tooinstall OpenSSL a helyi számítógépre van szüksége.

#### <a name="for-linux-os-x-or-unix"></a>A Linux, OS X or Unix
hello OpenSSL-függvénytárak szerepelnek közvetlenül hello forráskódjának [OpenSSL szoftver Foundation](http://www.openssl.org). hello következő utasítások végigvezetik hello lépéseket szükséges tooinstall OpenSSL a Linux rendszerű számítógépen. Ebben a cikkben az Ubuntu 12.04 és magasabb toowork ismert parancsok.

Nyisson meg egy terminál-munkamenetet, és OpenSSL telepítése
```bash
wget http://www.openssl.org/source/openssl-1.1.0e.tar.gz
``` 
Csomagolja ki hello fájlokat hello letöltőcsomag
```bash
tar -xvzf openssl-1.1.0e.tar.gz
```
Adja meg hello könyvtárat, amelyben hello fájlok könyvtárban találhatók. Alapértelmezés szerint az alábbinak kell lennie.

```bash
cd openssl-1.1.0e
```
OpenSSL konfigurálása a következő hello a következő parancs futtatásával. Ha egy mappa fájljait hello /usr/local/openssl eltér, győződjön meg arról, hogy toochange hello követő szükség szerint.

```bash
./config --prefix=/usr/local/openssl --openssldir=/usr/local/openssl
```
Most, hogy OpenSSL megfelelően van konfigurálva, toocompile kell azt tooconvert a tanúsítványt. Futtassa a következő parancs hello toocompile:

```bash
make
```
Ha fordítása befejeződött, most készen áll a tooinstall OpenSSL végrehajtható fájlként hello a következő parancs futtatásával:
```bash
make install
```
tooconfirm, hogy sikeresen telepítette OpenSSL a rendszeren, futtatási hello következő parancsot, és arra, hogy hello toomake ellenőrizze azonos kimenethez.

```bash
/usr/local/openssl/bin/openssl version
```
Ha sikeres hello a következő üzenetet kell látnia.
```bash
OpenSSL 1.1.0e 7 Apr 2014
```

#### <a name="for-windows"></a>Windows rendszerhez
A következő módokon hello OpenSSL telepítése Windows rendszerű elvégezhető:
1. **(Ajánlott)**  Hello beépített Bash a Windows-szolgáltatást használ a Windows 10-es és újabb verziók, OpenSSL alapértelmezés szerint telepítve van-e. Hogyan tooenable Bash a Windows-funkciókat a Windows 10-es található utasításokat [Itt](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).
2. A letöltés a Win32/64 alkalmazás hello közösségi által biztosított. Amíg nem adja meg a OpenSSL szoftver Foundation hello, vagy hagyja jóvá a megadott Windows Installer telepítők, rendelkezésre álló telepítők listáját tartalmazzák [Itt](https://wiki.openssl.org/index.php/Binaries)

### <a name="decode-your-certificate-file"></a>A tanúsítványfájl dekódolása
hello letöltött fájl nem titkosított formátumban legfelső szintű hitelesítésszolgáltató. Használja a OpenSSL toodecode hello tanúsítványfájlt. toodo Igen, a parancsot, OpenSSL:

```dos
OpenSSL>x509 -inform DER -in BaltimoreCyberTrustRoot.cer -text -out root.crt
```

### <a name="connecting-tooazure-database-for-postgresql-with-ssl-certificate-authentication"></a>Csatlakozás tooAzure adatbázis PostgreSQL SSL tanúsítványalapú hitelesítéssel ellátott
Most, hogy sikeres rendelkeznek dekódolni, a tanúsítványt, csatlakoztathatja tooyour adatbázis-kiszolgáló biztonságos SSL-en keresztül. kiszolgáló-tanúsítvány ellenőrzés tooallow, hello fájl ~/.postgresql/root.crt a hello felhasználó kezdőkönyvtárának hello tanúsítványt kell helyezni. (A Microsoft Windows hello-fájl neve % APPDATA%\postgresql\root.crt.). hello következő tooAzure adatbázis csatlakozás PostgreSQL vonatkozó utasításokat tartalmazza.

> [!NOTE]
> Jelenleg egy ismert probléma használatakor "sslmode ellenőrizze teljes =" a kapcsolat toohello szolgáltatásban a hello kapcsolat nem jön létre a következő hiba hello: _kiszolgálótanúsítvány "&lt;régió&gt;. Control.Database.Windows.NET"(és más neveket 7) nem egyezik meg a gazdagép neve"&lt;kiszolgálónév&gt;. postgres.database.azure.com "._
> Ha "sslmode ellenőrizze teljes =" van szükség esetén használjon hello server elnevezési  **&lt;kiszolgálónév&gt;. database.windows.net** a kapcsolati karakterlánc állomás neve. Ez a korlátozás a jövőbeli hello tervezzük a tooremove. Egyéb használó kapcsolatok [SSL módok](https://www.postgresql.org/docs/9.6/static/libpq-ssl.html#LIBPQ-SSL-SSLMODE-STATEMENTS) toouse előnyben részesített hello állomás elnevezési konvenciót kell folytatódnia  **&lt;kiszolgálónév&gt;. postgres.database.azure.com**.

#### <a name="using-psql-command-line-utility"></a>Psql parancssori segédprogrammal
hello a következő példa bemutatja, hogyan kapcsolódnak az toosuccessfully a tooyour PostgreSQL server hello psql parancssori segédprogram segítségével. Használjon hello `root.crt` fájl létrehozása és hello `sslmode=verify-ca` vagy `sslmode=verify-full` lehetőséget.

A következő parancs hello hello PostgreSQL parancssori felület segítségével hajtható végre:
```bash
psql "sslmode=verify-ca sslrootcert=root.crt host=mypgserver-20170401.postgres.database.azure.com dbname=postgres user=mylogin@mypgserver-20170401"
```
Ha sikeres, a következő kimeneti hello jelenik meg:
```bash
Password for user mylogin@mypgserver-20170401:
psql (9.6.2)
WARNING: Console code page (437) differs from Windows code page (1252)
     8-bit characters might not work correctly. See psql reference
     page "Notes for Windows users" for details.
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-SHA384, bits: 256, compression: off)
Type "help" for help.

postgres=>
```

#### <a name="using-pgadmin-gui-tool"></a>Grafikus felhasználói Felülettel pgAdmin eszközzel
PgAdmin 4 tooconnect biztonságosan konfigurálása SSL-en keresztül igényel tooset hello `SSL mode = Verify-CA` vagy `SSL mode = Verify-Full` az alábbiak szerint:

![Képernyőkép a pgAdmin - kapcsolat – SSL-mód megkövetelése](./media/concepts-ssl-connection-security/2-pgadmin-ssl.png)

## <a name="next-steps"></a>Következő lépések
Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak PostgreSQL Azure-adatbázis](concepts-connection-libraries.md)
