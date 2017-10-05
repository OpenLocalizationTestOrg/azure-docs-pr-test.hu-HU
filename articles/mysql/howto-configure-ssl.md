---
title: "Konfigurálja az SSL-kapcsolatot való biztonságos kapcsolódás Azure-adatbázis a MySQL |} Microsoft Docs"
description: "Útmutatást megfelelően konfigurálni az Azure-adatbázis MySQL és a társított alkalmazások megfelelően az SSL-kapcsolat használata"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 77e1b6266a2cf47949fa06358ec003f6b6b38065
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="configure-ssl-connectivity-in-your-application-to-securely-connect-to-azure-database-for-mysql"></a>Az alkalmazásokhoz való biztonságos kapcsolódás Azure-adatbázis a MySQL SSL-kapcsolat konfigurálása
Azure MySQL-adatbázis támogatja az Azure-adatbázis MySQL-kiszolgáló csatlakozik a Secure Sockets Layer (SSL) használó ügyfélalkalmazások. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése segít lánctámadások elleni védelem érdekében "man a középső" az adatfolyamot a kiszolgáló és az alkalmazás közötti titkosításával.

## <a name="step-1-obtain-ssl-certificate"></a>1. lépés: SSL-tanúsítvány beszerzése
Töltse le a tanúsítványt, az Azure-adatbázis a MySQL-kiszolgáló az SSL protokollt használó kommunikációra szükséges [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) és mentette a tanúsítványfájlt (a helyi meghajtójára Ebben az oktatóanyagban használtuk c:\ssl).
**A Microsoft Internet Explorer és a Microsoft Edge:** a letöltés befejezése után nevezze át a tanúsítvány BaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>2. lépés: A kötés SSL
### <a name="connecting-to-server-using-the-mysql-workbench-over-ssl"></a>Kapcsolódás a kiszolgálóhoz a MySQL munkaterület használata az SSL-en keresztül
Konfigurálja a MySQL munkaterület biztonságos SSL Csatornán keresztül csatlakozni. Keresse meg a **SSL** a MySQL munkaterület az új kapcsolat beállítása párbeszéd fülre. Adja meg a fájl helyét a **BaltimoreCyberTrustRoot.crt.pem** a a **SSL-hitelesítésszolgáltató fájl:** mező.
![testre szabott csempe mentése](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-to-server-using-the-mysql-cli-over-ssl"></a>Kapcsolódás a kiszolgálóhoz SSL-en keresztül a MySQL parancssori felület használatával
A MySQL parancssori felületén, hajtsa végre a következő parancsot:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>3. lépés: SSL-kapcsolatok az Azure-ban kényszerítése 
### <a name="using-azure-portal"></a>Az Azure Portal használata
Az Azure-portált használja, látogasson el a MySQL-kiszolgálót, majd kattintson az Azure Database **kapcsolatbiztonsági**. A váltógomb segítségével engedélyezheti vagy tilthatja le a **kényszerítése SSL-kapcsolat** beállítást. Ezután kattintson a **Save** (Mentés) gombra. A Microsoft azt javasolja, hogy mindig engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.
![ssl engedélyezése](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Az Azure parancssori felület használata
Engedélyezheti vagy letilthatja a **ssl-kényszerítési** paraméter, illetve Azure CLI engedélyezve vagy letiltva értékei alapján.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>4. lépés: SSL-kapcsolat ellenőrzése
A mysql végrehajtása **állapot** parancs futtatásával ellenőrizze, hogy rendelkezik-e csatlakozik a MySQL-kiszolgálóval SSL használatával:
```dos
mysql> status
```
Ellenőrizze, hogy a kapcsolat a kimeneti megtekintésével titkosított. Kell megjelennie: **SSL: titkosító használatban AES256-SHA** 

## <a name="sample-code"></a>Mintakód
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed to connect to MySQL: '.mysqli_connect_error());
}
```
### <a name="python"></a>Python
```
try:
conn = mysql.connector.connect(user='myadmin@myserver4demo',password='yourpassword',database='quickstartdb',host='myserver4demo.mysql.database.azure.com',ssl_ca='/var/www/html/BaltimoreCyberTrustRoot.crt.pem')
except mysql.connector.Error as err:
 print(err)
```

## <a name="next-steps"></a>Következő lépések
Tekintse át a következő különböző alkalmazás kapcsolati lehetőségek [adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)
