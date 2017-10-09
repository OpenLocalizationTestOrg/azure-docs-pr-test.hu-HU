---
title: "SSL-kapcsolat toosecurely aaaConfigure tooAzure adatbázis kapcsolati MySQL |} Microsoft Docs"
description: "Hogyan tooproperly konfigurálása az Azure Database MySQL és társított alkalmazásokat toocorrectly utasításokat SSL-kapcsolat használata"
services: mysql
author: seanli1988
ms.author: seanli
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 07/28/2017
ms.openlocfilehash: 8c37c19d4c101abfb730f429a19441e94e52fc85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-ssl-connectivity-in-your-application-toosecurely-connect-tooazure-database-for-mysql"></a>Az SSL konfigurálása az alkalmazás toosecurely való csatlakozással tooAzure adatbázis kapcsolati MySQL
MySQL az Azure-adatbázishoz való csatlakozást az Azure-adatbázis MySQL tooclient olyan kiszolgálóalkalmazások esetén használja a Secure Sockets Layer (SSL) támogatja. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.

## <a name="step-1-obtain-ssl-certificate"></a>1. lépés: SSL-tanúsítvány beszerzése
Töltse le a hello tanúsítvány szükséges toocommunicate az Azure-adatbázissal a MySQL-kiszolgáló SSL-en keresztül [https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem](https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem) és hello tanúsítvány fájl tooyour helyi mentése meghajtó (ebben az oktatóanyagban a használtuk c:\ssl).
**A Microsoft Internet Explorer és a Microsoft Edge:** hello letöltés befejezése után nevezze át a hello tanúsítvány tooBaltimoreCyberTrustRoot.crt.pem.

## <a name="step-2-bind-ssl"></a>2. lépés: A kötés SSL
### <a name="connecting-tooserver-using-hello-mysql-workbench-over-ssl"></a>Hello MySQL munkaterület használata az SSL-en keresztül csatlakozó tooserver
Konfigurálja a MySQL munkaterület tooconnect biztonságos SSL-en keresztül. Keresse meg a toohello **SSL** hello MySQL munkaterület az új kapcsolat beállítása párbeszéd hello lapján. Adja meg a fájl helye hello hello **BaltimoreCyberTrustRoot.crt.pem** a hello **SSL-hitelesítésszolgáltató fájl:** mező.
![testre szabott csempe mentése](./media/howto-configure-ssl/mysql-workbench-ssl.png)

### <a name="connecting-tooserver-using-hello-mysql-cli-over-ssl"></a>Hello MySQL CLI használata SSL-en keresztül csatlakozó tooserver
A következő parancs hello hello MySQL parancssori felület segítségével hajtható végre:
```dos
mysql.exe -h mysqlserver4demo.mysql.database.azure.com -u Username@mysqlserver4demo -p --ssl-ca=c:\ssl\BaltimoreCyberTrustRoot.crt.pem
```

## <a name="step-3--enforcing-ssl-connections-in-azure"></a>3. lépés: SSL-kapcsolatok az Azure-ban kényszerítése 
### <a name="using-azure-portal"></a>Az Azure Portal használata
Hello Azure-portál használatával keresse fel a MySQL-kiszolgálót, majd kattintson az Azure Database **kapcsolatbiztonsági**. Hello váltása gomb tooenable használja, vagy tiltsa le a hello **kényszerítése SSL-kapcsolat** beállítást. Ezután kattintson a **Save** (Mentés) gombra. A Microsoft azt javasolja, tooalways engedélyezése **kényszerítése SSL-kapcsolat** vonatkozó fokozott biztonsági beállításait.
![ssl engedélyezése](./media/howto-configure-ssl/enable-ssl.png)

### <a name="using-azure-cli"></a>Az Azure parancssori felület használata
Engedélyezheti vagy letilthatja a hello **ssl-kényszerítési** paraméter, illetve Azure CLI engedélyezve vagy letiltva értékei alapján.
```azurecli-interactive
az mysql server update --resource-group myresource --name mysqlserver4demo --ssl-enforcement Enabled
```

## <a name="step-4-verify-ssl-connection"></a>4. lépés: SSL-kapcsolat ellenőrzése
Hello mysql végrehajtása **állapot** parancs tooverify, hogy csatlakozott-e tooyour MySQL kiszolgálóval SSL használatával:
```dos
mysql> status
```
Győződjön meg róla, hello kapcsolat titkosított hello kimeneti megtekintésével. Kell megjelennie: **SSL: titkosító használatban AES256-SHA** 

## <a name="sample-code"></a>Mintakód
### <a name="php"></a>PHP
```
$conn = mysqli_init();
mysqli_ssl_set($conn,NULL,NULL, "/var/www/html/BaltimoreCyberTrustRoot.crt.pem", NULL, NULL) ; 
mysqli_real_connect($conn, 'myserver4demo.mysql.database.azure.com', 'myadmin@myserver4demo', 'yourpassword', 'quickstartdb', 3306);
if (mysqli_connect_errno($conn)) {
die('Failed tooconnect tooMySQL: '.mysqli_connect_error());
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
